categories:
  - jenkins
  - shell
tags:
  - jenkins
  - 持续集成
  - 脚本
  - ''
title: jenkins节点重启脚本
date: 2015-12-29 13:58:00
---


## 远程重启jenkins节点脚本
``` shell
NODE_CONFIG=nodes.ini
JENKINS_CLIENT_COMMAND=/opt/work/local/jenkins/jenkins-client/bin/client.sh


start_nodes() {
        while read node
        do
                if [ -z "$node" ]; then
                        continue;
                fi

                echo "current node: $node";
                start_remote_jenkins $node;
        done < $NODE_CONFIG
}

start_remote_jenkins() {
        remote_ip=$1
        ssh $remote_ip 2>&1 << remote_command
        sh $JENKINS_CLIENT_COMMAND;
        exit;
remote_command
}


start_nodes
```

<!-- more -->

## jenkins slave脚本
jenkins slave重启脚本

``` shell


function file_source() {
  if [ -f $1 ]; then
    source $1;
  fi
}


file_source /etc/profile
file_source ~/.profile
file_source ~/bash_profile


if test -z "${JAVA_HOME}"; then
  echo "please config JAVA_HOME~"
  exit 1;
fi


BIN_PARAM='start'
if [ $# -ge 1 ]; then
  BIN_PARAM=$1
fi


JAVA_EXE=$JAVA_HOME/bin/java

BIN_PATH=$( cd "$( dirname "${BASH_SOURCE[0]}" )" && pwd )
SLAVE_CONFIG=$BIN_PATH/slave.conf
source $SLAVE_CONFIG 

SLAVE_HOME=$(dirname $BIN_PATH)
LOG_FILE=$SLAVE_HOME/logs/jenkins_slave.log

SLAVE_PID_COMMAND="ps -ef | grep $SECRET_ID | grep -v grep | awk '{print \$2}'"
SLAVE_PID=$(eval $SLAVE_PID_COMMAND)

START_COMMAND="nohup $JAVA_EXE -jar ${BIN_PATH}/slave.jar -jnlpUrl http://${SERVER_HOST}:${SERVER_PORT}/computer/${SLAVE_NAME}/slave-agent.jnlp -secret $SECRET_ID 1>>$LOG_FILE 2>&1 &"


echo "========================================"
echo "java bin path: $JAVA_EXE"
echo "slave client home path: $SLAVE_HOME"
echo "slave client bin path: $BIN_PATH"
echo "load slave client conf: $SLAVE_CONFIG"
echo "========================================"
echo "jenkins connection params:"
echo "  - server host: $SERVER_HOST"
echo "  - server port: $SERVER_PORT"
echo "  - slave name: $SLAVE_NAME"
echo "  - secret id: $SECRET_ID"
echo "========================================"



function startup() {
  echo "execute get process command: $SLAVE_PID_COMMAND"
  if [ -n "$SLAVE_PID" ]; then
    echo "process with $SECRET_ID has existed."
    echo "jenkins slave client pid: $SLAVE_PID"
    exit 0;
  fi

  echo "execute script shell: $START_COMMAND"
  eval $START_COMMAND
  
  if [ $? -ne 0 ]; then
    echo "fail to startup the jenkins slave client."
    exit 1;
  fi

  SLAVE_PID=$(eval $SLAVE_PID_COMMAND)
  echo "the jenkins slave client process id: $SLAVE_PID"
  return;
}


function shutdown() {
  echo "execute get process command: $SLAVE_PID_COMMAND"
  if [ -z "$SLAVE_PID" ]; then
    echo "no process with $SECRET_ID"
    exit 0;
  fi
  
  echo "killing process with pid: $SLAVE_PID"
  kill -9 $SLAVE_PID

  if [ $? -ne 0 ]; then
    echo "fail to shutdown the jenkins slave client with pid: $SLAVE_PID"
    exit 1;
  fi

  echo "the jenkins slave client has stoped."
  return;
}


function bootstrap() {
  if [ $BIN_PARAM == "start" ] || [ $BIN_PARAM == "startup" ]; then
    echo "startup jenkins slave client..."
    startup
    exit 0;
  elif [ $BIN_PARAM == "stop" ] || [ $BIN_PARAM == "shutdown" ]; then
    echo "shutdown jenkins slave client..."
    shutdown
    exit 0;
  else
    echo "not support param: $1"
    exit 1;
  fi
}


bootstrap

```


## node节点
``` javascript
var links = document.getElementsByClassName("model-link");
for (var i=0;i<links.length;i++)
{
	console.log(links[i].innerText);
}
```
``` ini
10.10.1.11
10.10.1.12
10.10.1.13
10.13.2.14
10.13.2.15
10.13.2.16
```