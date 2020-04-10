# hadoop-sh

```bash
#!/usr/bin/env bash

# This script runs the hadoop core commands.

# /usr/local/hadoop/bin/hadoop
bin=`which $0` 
# /usr/local/hadoop/bin
bin=`dirname ${bin}`
# bin目录下
bin=`cd "$bin"; pwd`

# /usr/local/hadoop/libexec
DEFAULT_LIBEXEC_DIR="$bin"/../libexec

HADOOP_LIBEXEC_DIR=${HADOOP_LIBEXEC_DIR:-$DEFAULT_LIBEXEC_DIR}
# 执行配置
. $HADOOP_LIBEXEC_DIR/hadoop-config.sh

# 使用
function print_usage(){
  echo "Usage: hadoop [--config confdir] [COMMAND | CLASSNAME]"
  echo "  CLASSNAME            run the class named CLASSNAME"
  echo " or"
  echo "  where COMMAND is one of:"
  echo "  fs                   run a generic filesystem user client"
  echo "  version              print the version"
  echo "  jar <jar>            run a jar file"
  echo "                       note: please use \"yarn jar\" to launch"
  echo "                             YARN applications, not this command."
  echo "  checknative [-a|-h]  check native hadoop and compression libraries availability"
  echo "  distcp <srcurl> <desturl> copy file or directories recursively"
  echo "  archive -archiveName NAME -p <parent path> <src>* <dest> create a hadoop archive"
  echo "  classpath            prints the class path needed to get the"
  echo "  credential           interact with credential providers"
  echo "                       Hadoop jar and the required libraries"
  echo "  daemonlog            get/set the log level for each daemon"
  echo "  trace                view and modify Hadoop tracing settings"
  echo ""
  echo "Most commands print help when invoked w/o parameters."
}

if [ $# = 0 ]; then
  print_usage
  exit
fi

COMMAND=$1 #hadoop fs
# 打印帮助命令
case $COMMAND in
  # usage flags
  --help|-help|-h)
    print_usage
    exit
    ;;

  #hdfs commands hdfs命令
  namenode|secondarynamenode|datanode|dfs|dfsadmin|fsck|balancer|fetchdt|oiv|dfsgroups|portmap|nfs3)
    echo "DEPRECATED: Use of this script to execute hdfs command is deprecated." 1>&2
    echo "Instead use the hdfs command for it." 1>&2
    echo "" 1>&2
    #try to locate hdfs and if present, delegate to it.
    shift
    if [ -f "${HADOOP_HDFS_HOME}"/bin/hdfs ]; then
      exec "${HADOOP_HDFS_HOME}"/bin/hdfs ${COMMAND/dfsgroups/groups}  "$@"
    elif [ -f "${HADOOP_PREFIX}"/bin/hdfs ]; then
      exec "${HADOOP_PREFIX}"/bin/hdfs ${COMMAND/dfsgroups/groups} "$@"
    else
      echo "HADOOP_HDFS_HOME not found!"
      exit 1
    fi
    ;;

  #mapred commands for backwards compatibility MR命令
  pipes|job|queue|mrgroups|mradmin|jobtracker|tasktracker)
    echo "DEPRECATED: Use of this script to execute mapred command is deprecated." 1>&2
    echo "Instead use the mapred command for it." 1>&2
    echo "" 1>&2
    #try to locate mapred and if present, delegate to it.
    shift
    if [ -f "${HADOOP_MAPRED_HOME}"/bin/mapred ]; then
      exec "${HADOOP_MAPRED_HOME}"/bin/mapred ${COMMAND/mrgroups/groups} "$@"
    elif [ -f "${HADOOP_PREFIX}"/bin/mapred ]; then
      exec "${HADOOP_PREFIX}"/bin/mapred ${COMMAND/mrgroups/groups} "$@"
    else
      echo "HADOOP_MAPRED_HOME not found!"
      exit 1
    fi
    ;;

  #core commands core命令
  *)
    # the core commands
    if [ "$COMMAND" = "fs" ] ; then
      CLASS=org.apache.hadoop.fs.FsShell
    elif [ "$COMMAND" = "version" ] ; then
      CLASS=org.apache.hadoop.util.VersionInfo
    elif [ "$COMMAND" = "jar" ] ; then
      CLASS=org.apache.hadoop.util.RunJar
      if [[ -n "${YARN_OPTS}" ]] || [[ -n "${YARN_CLIENT_OPTS}" ]]; then
        echo "WARNING: Use \"yarn jar\" to launch YARN applications." 1>&2
      fi
    elif [ "$COMMAND" = "key" ] ; then
      CLASS=org.apache.hadoop.crypto.key.KeyShell
    elif [ "$COMMAND" = "checknative" ] ; then
      CLASS=org.apache.hadoop.util.NativeLibraryChecker
    elif [ "$COMMAND" = "distcp" ] ; then
      CLASS=org.apache.hadoop.tools.DistCp
      CLASSPATH=${CLASSPATH}:${TOOL_PATH}
    elif [ "$COMMAND" = "daemonlog" ] ; then
      CLASS=org.apache.hadoop.log.LogLevel
    elif [ "$COMMAND" = "archive" ] ; then
      CLASS=org.apache.hadoop.tools.HadoopArchives
      CLASSPATH=${CLASSPATH}:${TOOL_PATH}
    elif [ "$COMMAND" = "credential" ] ; then
      CLASS=org.apache.hadoop.security.alias.CredentialShell
    elif [ "$COMMAND" = "trace" ] ; then
      CLASS=org.apache.hadoop.tracing.TraceAdmin
    elif [ "$COMMAND" = "classpath" ] ; then
      if [ "$#" -gt 1 ]; then
        CLASS=org.apache.hadoop.util.Classpath
      else
        # No need to bother starting up a JVM for this simple case.
        if $cygwin; then
          CLASSPATH=$(cygpath -p -w "$CLASSPATH" 2>/dev/null)
        fi
        echo $CLASSPATH
        exit
      fi
    elif [[ "$COMMAND" = -*  ]] ; then
        # class and package names cannot begin with a -
        echo "Error: No command named \`$COMMAND' was found. Perhaps you meant \`hadoop ${COMMAND#-}'"
        exit 1
    else
      CLASS=$COMMAND
    fi

    # cygwin path translation
    if $cygwin; then
      CLASSPATH=$(cygpath -p -w "$CLASSPATH" 2>/dev/null)
      HADOOP_LOG_DIR=$(cygpath -w "$HADOOP_LOG_DIR" 2>/dev/null)
      HADOOP_PREFIX=$(cygpath -w "$HADOOP_PREFIX" 2>/dev/null)
      HADOOP_CONF_DIR=$(cygpath -w "$HADOOP_CONF_DIR" 2>/dev/null)
      HADOOP_COMMON_HOME=$(cygpath -w "$HADOOP_COMMON_HOME" 2>/dev/null)
      HADOOP_HDFS_HOME=$(cygpath -w "$HADOOP_HDFS_HOME" 2>/dev/null)
      HADOOP_YARN_HOME=$(cygpath -w "$HADOOP_YARN_HOME" 2>/dev/null)
      HADOOP_MAPRED_HOME=$(cygpath -w "$HADOOP_MAPRED_HOME" 2>/dev/null)
    fi

    shift

    # Always respect HADOOP_OPTS and HADOOP_CLIENT_OPTS
    HADOOP_OPTS="$HADOOP_OPTS $HADOOP_CLIENT_OPTS"

    #make sure security appender is turned off
    HADOOP_OPTS="$HADOOP_OPTS -Dhadoop.security.logger=${HADOOP_SECURITY_LOGGER:-INFO,NullAppender}"

    export CLASSPATH=$CLASSPATH
    # 调用JAVA来执行
    exec "$JAVA" $JAVA_HEAP_MAX $HADOOP_OPTS $CLASS "$@"
    ;;

esac
```


