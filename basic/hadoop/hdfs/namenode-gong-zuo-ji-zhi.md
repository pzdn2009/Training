* [NameNode的功能](# 1. NameNode的功能)
* [处理时机](# 2. 处理时机)
* [术语](# 3. 术语)
* [流程分析](# 4. 流程分析)
* [fsimage & edits](5. fsimage & edits)

# 1. NameNode的功能

* 负责客户端请求的响应
* 元数据的管理（查询，修改）

专门建立一种机制来保证元数据。

# 2. 处理时机

* 首次启动
* 非首次启动
* 故障恢复

# 3. 术语

* **NN**：NameNode
* **2NN**：Secondary NameNode
* **fsimage**: 元数据镜像文件（保存文件系统的目录树）
* **edits**: 元数据操作日志（针对目录树的修改操作）
* 内存
* **fsimage.checkpoint**: fsimage + edits 合并结构。
* **DN**：DataNode
* **元数据**：文件系统目录树

# 4. 流程分析

![](/assets/hadoop-namenode.png)

角色：
* client：客户端
* NN：负责管理元数据
* 2NN：辅助NN
* DN：同步Block信息

流程：
* NN 启动时，先滚动edits并生成一个空的edits.inprogress，然后加载edits和fsimage到内存中，此时namenode内存就持有最新的元数据信息。
* client开始对namenode发送元数据的增删改查的请求，这些请求的操作首先会被记录的edits.inprogress中（查询元数据的操作不会被记录在edits中，因为查询操作不会更改元数据信息），如果此时namenode挂掉，重启后会从edits中读取元数据的信息。然后，namenode会在内存中执行元数据的增删改查的操作。
* 由于edits中记录的操作会越来越多，edits文件会越来越大，导致namenode在启动**加载**edits时会很慢，所以需要对edits和fsimage进行合并（所谓合并，就是将edits和fsimage加载到内存中，照着edits中的操作一步步执行，最终形成新的fsimage）。secondarynamenode的作用就是帮助namenode进行edits和fsimage的合并工作。
* secondarynamenode首先会询问namenode是否需要checkpoint（触发checkpoint需要满足两个条件中的任意一个，定时时间到和edits中数据写满了）。直接带回namenode是否检查结果。
* secondarynamenode执行checkpoint操作，**首先会让namenode滚动edits并生成一个空的edits.inprogress，滚动edits的目的是给edits打个标记，以后所有新的操作都写入edits.inprogress**，其他未合并的edits和fsimage会拷贝到secondarynamenode的本地，然后将拷贝的edits和fsimage加载到内存中进行合并，生成fsimage.chkpoint，然后将fsimage.chkpoint拷贝给namenode，重命名为fsimage后替换掉原来的fsimage。
* namenode在启动时就只需要加载之前未合并的edits和fsimage即可，因为合并过的edits中的元数据信息已经被记录在fsimage中。

## 4.1 NameNode机制

1. 第一次启动NN格式化后，创建`fsimage`和`edits`文件。
   
   如果不是第一次启动，直接加载`fsimage`和`edits`到内存。
2. Client对元数据进行增删改的请求;
3. NN记录操作日志，更新滚动日志（先记录再操作，保证一致性），记录到`edits.inprogress`；
4. NN在内存中对数据进行增删改查。

## 4.2 Secondary NameNode机制

1. 2NN询问NN是否需要`checkpoint`。直接带回NN是否检查结果。
2. 2NN请求执行`checkpoint`。
3. NN滚动正在写的`edits`日志。
4. 将滚动前的编辑日志和镜像文件拷贝到2NN。
5. 2NN加载编辑日志和镜像文件到内存，并合并。
6. 生成新的镜像文件`fsimage.chkpoint`。
7. 拷贝`fsimage.chkpoint`到NN。
8. NN将`fsimage.chkpoint`重新命名成`fsimage`。

# 5. fsimage & edits

namenode被格式化之后，将在`/opt/module/hadoop-2.7.2/data/tmp/dfs/name/current`目录中产生如下文件：
```
fsimage_0000000000000000000
fsimage_0000000000000000000.md5
seen_txid
VERSION
```

文件解释：

* fsimage文件：HDFS文件系统元数据的一个**永久性的检查点**，其中包含HDFS文件系统的所有目录和文件inode的序列化信息。
* edits文件：存放HDFS文件系统的所有更新操作的路径，文件系统客户端执行的所有写操作首先会被记录到edits文件中。
* seen_txid文件保存的是一个数字，就是**最后一个edits_的数字**
* 每次NN启动的时候都会将fsimage文件读入内存，加载edits里面的更新操作，保证内存中的元数据信息是最新的、同步的，可以看成NameNode启动的时候就将fsimage和edits文件进行了合并。

```xml
# 以 XML格式 查看 fsimage，并生成xml文件
hdfs oiv -p XML -i fsimage_0000000000000000025 -o /opt/module/hadoop-2.7.2/fsimage.xml
# 查看内容
cat /opt/module/hadoop-2.7.2/fsimage.xml

<inode>
    <id>16386</id>
    <type>DIRECTORY</type>
    <name>user</name>
    <mtime>1512722284477</mtime>
    <permission>admin:supergroup:rwxr-xr-x</permission>
    <nsquota>-1</nsquota>
    <dsquota>-1</dsquota>
</inode>
<inode>
    <id>16387</id>
    <type>DIRECTORY</type>
    <name>admin</name>
    <mtime>1512790549080</mtime>
    <permission>admin:supergroup:rwxr-xr-x</permission>
    <nsquota>-1</nsquota>
    <dsquota>-1</dsquota>
</inode>
<inode>
    <id>16389</id>
    <type>FILE</type>
    <name>wc.input</name>
    <replication>3</replication>
    <mtime>1512722322219</mtime>
    <atime>1512722321610</atime>
    <perferredBlockSize>134217728</perferredBlockSize>
    <permission>admin:supergroup:rw-r--r--</permission>
    <blocks>
        <block>
            <id>1073741825</id>
            <genstamp>1001</genstamp>
            <numBytes>59</numBytes>
        </block>
    </blocks>
</inode >
```

```xml
# 以 XML格式查看edits，并生成xml文件
hdfs oev -p XML -i edits_0000000000000000012-0000000000000000013 -o /opt/module/hadoop-2.7.2/edits.xml
# 查看
cat /opt/module/hadoop-2.7.2/edits.xml

<?xml version="1.0" encoding="UTF-8"?>
<EDITS>
    <EDITS_VERSION>-63</EDITS_VERSION>
    <RECORD>
        <OPCODE>OP_START_LOG_SEGMENT</OPCODE>
        <DATA>
            <TXID>129</TXID>
        </DATA>
    </RECORD>
    <RECORD>
        <OPCODE>OP_ADD</OPCODE>
        <DATA>
            <TXID>130</TXID>
            <LENGTH>0</LENGTH>
            <INODEID>16407</INODEID>
            <PATH>/hello7.txt</PATH>
            <REPLICATION>2</REPLICATION>
            <MTIME>1512943607866</MTIME>
            <ATIME>1512943607866</ATIME>
            <BLOCKSIZE>134217728</BLOCKSIZE>
            <CLIENT_NAME>DFSClient_NONMAPREDUCE_-1544295051_1</CLIENT_NAME>
            <CLIENT_MACHINE>192.168.1.5</CLIENT_MACHINE>
            <OVERWRITE>true</OVERWRITE>
            <PERMISSION_STATUS>
                <USERNAME>admin</USERNAME>
                <GROUPNAME>supergroup</GROUPNAME>
                <MODE>420</MODE>
            </PERMISSION_STATUS>
            <RPC_CLIENTID>908eafd4-9aec-4288-96f1-e8011d181561</RPC_CLIENTID>
            <RPC_CALLID>0</RPC_CALLID>
        </DATA>
    </RECORD>
    <RECORD>
        <OPCODE>OP_ALLOCATE_BLOCK_ID</OPCODE>
        <DATA>
            <TXID>131</TXID>
            <BLOCK_ID>1073741839</BLOCK_ID>
        </DATA>
    </RECORD>
    <RECORD>
        <OPCODE>OP_SET_GENSTAMP_V2</OPCODE>
        <DATA>
            <TXID>132</TXID>
            <GENSTAMPV2>1016</GENSTAMPV2>
        </DATA>
    </RECORD>
    <RECORD>
        <OPCODE>OP_ADD_BLOCK</OPCODE>
        <DATA>
            <TXID>133</TXID>
            <PATH>/hello7.txt</PATH>
            <BLOCK>
                <BLOCK_ID>1073741839</BLOCK_ID>
                <NUM_BYTES>0</NUM_BYTES>
                <GENSTAMP>1016</GENSTAMP>
            </BLOCK>
            <RPC_CLIENTID></RPC_CLIENTID>
            <RPC_CALLID>-2</RPC_CALLID>
        </DATA>
    </RECORD>
    <RECORD>
        <OPCODE>OP_CLOSE</OPCODE>
        <DATA>
            <TXID>134</TXID>
            <LENGTH>0</LENGTH>
            <INODEID>0</INODEID>
            <PATH>/hello7.txt</PATH>
            <REPLICATION>2</REPLICATION>
            <MTIME>1512943608761</MTIME>
            <ATIME>1512943607866</ATIME>
            <BLOCKSIZE>134217728</BLOCKSIZE>
            <CLIENT_NAME></CLIENT_NAME>
            <CLIENT_MACHINE></CLIENT_MACHINE>
            <OVERWRITE>false</OVERWRITE>
            <BLOCK>
                <BLOCK_ID>1073741839</BLOCK_ID>
                <NUM_BYTES>25</NUM_BYTES>
                <GENSTAMP>1016</GENSTAMP>
            </BLOCK>
            <PERMISSION_STATUS>
                <USERNAME>admin</USERNAME>
                <GROUPNAME>supergroup</GROUPNAME>
                <MODE>420</MODE>
            </PERMISSION_STATUS>
        </DATA>
    </RECORD>
</EDITS>
```



