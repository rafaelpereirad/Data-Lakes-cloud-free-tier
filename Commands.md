
# Ambari

Start/Stop/Restart Ambari server:

```sh
sudo ambari-server start
sudo ambari-server stop
sudo ambari-server restart
```

Start/Stop/Restart Ambari agent:

```sh
sudo ambari-agent start
sudo ambari-agent stop
sudo ambari-agent restart
```

Logs to debug:

```sh
cat /var/log/ambari-agent/ambari-agent.log
cat /var/log/ambari-server/ambari-server.log
```

```sh
sudo ambari-agent start
```
# Zookeeper

export ZOOKEEPER_HOME="/usr/odp/1.2.2.0-128/zookeeper"
export PATH=$PATH:$ZOOKEEPER_HOME/bin
export PATH=$ZOOKEEPER_HOME/bin:$PATH

Start/Stop/Status Zookeeper server:

```sh
zhServer.sh start 
zhServer.sh stop
zkServer.sh status
```

Connect to Zookeeper CLI:

zkCli.sh -server 
zkServer.sh status
