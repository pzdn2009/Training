# API Client

## API Client

## 1. Java API

分为同步API和异步API，异步API有一个回调函数。

主要内容：会话session，节点类型（临时、永久），回调方法，Watcher，权限控制。

```java
import java.util.concurrent.CountDownLatch;
import java.util.List;
import org.apache.zookeeper.*;
import org.apache.zookeeper.Watcher.Event.KeeperState;
import org.apache.zookeeper.ZooDefs.Ids;

public class ZookeeperPractice implements Watcher {
 private static CountDownLatch countDownLatch = new CountDownLatch(1);
 public static void main(String[] args) throws Exception {

 ZooKeeper zooKeeper = new ZooKeeper("localhost:2181", 5000,

 new ZookeeperPractice());

 try {

    countDownLatch.await();
    System.out.println("Zookeeper session established");
    zooKeeper.create("/pzdn2", "pengzhen".getBytes(),

    Ids.OPEN_ACL_UNSAFE, CreateMode.PERSISTENT);
    System.out.println("Success create znode: /pzdn2");

    List<String> chiList = zooKeeper.getChildren("/", true);
    System.out.println(chiList);
    //zooKeeper.exists(path, watch);
    //zooKeeper.delete(path, version);
    //zooKeeper.setData(path, data, version);
    //zooKeeper.setACL(path, acl, version);
    //zooKeeper.addAuthInfo(scheme, auth);
    //zooKeeper.getData(path, watch, stat);
 } catch (Exception e) {
    // TODO: handle exception
 }
 }
 @Override
 public void process(WatchedEvent event) {
    // TODO Auto-generated method stub
    System.out.println("Receive watched event:" + event);
    if (event.getState() == KeeperState.SyncConnected) {
    countDownLatch.countDown();
 }
 }
}
```

## 2. zkClient

开源客户端，封装了原生的Java API接口。内部实现了同步连接管理、Session超时重连、Watcher反复注册等功能。

Watcher采用了Java通用的Listener模式。

## 3. Curator

Apache顶级项目，开源zk客户端，全世界范围内使用最广泛的ZK客户端之一。

* 自动重试
* FluentAPI

提供的一些典型的使用场景：

* LeaderSelector
* InterProcessMutex
* DistributedAtomicInterer
* CylicBarrier

提供了一些工具：

* ZKPaths
* EnsurePATH
* TestingServer
* TestingCluster

  n

