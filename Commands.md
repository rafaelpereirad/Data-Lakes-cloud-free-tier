
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

# HDFS

Switch user to hdfs:

```sh
sudo su hdfs
```

# Zookeeper

Start/Stop/Status Zookeeper server:

```sh
zhServer.sh start 
zhServer.sh stop
zkServer.sh status
```

Connect to Zookeeper CLI:

zkCli.sh -server 
zkServer.sh status

