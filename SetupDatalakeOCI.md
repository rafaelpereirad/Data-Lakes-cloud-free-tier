# Terraform

In OCI menu, go to **Developer Services > Resource Manager > Stacks** and click "Create Stack":

<img width="1451" alt="image" src="https://github.com/user-attachments/assets/9beb449a-2f7e-45d6-96d7-0555c28e263f">

Paste the Terraform folder in this repo and click "Next":

<img width="859" alt="image" src="https://github.com/user-attachments/assets/f6c33612-46db-4a44-89cd-e7973affbbbb">

Paste the public key in order to be able to access the machines through ssh, choose the Public IP in which you will access the cluster and leave "Install Ambari" option to install Ambari using Open Data Plataform (ODP) stack:

<img width="855" alt="image" src="https://github.com/user-attachments/assets/c4b14a31-e824-422b-bd98-f56d97d29ca8">

Click "Create":

<img width="875" alt="image" src="https://github.com/user-attachments/assets/ea613f6e-9961-403e-a366-5ec9115c5ef0">

In the created stack, there are the options to plan(check resources that will be deployed) and apply(create the resources):

<img width="1447" alt="image" src="https://github.com/user-attachments/assets/b60a9888-5e7b-4399-9b3e-357424d664ac">

Selecting "Apply", the resources will be created:

<img width="375" alt="image" src="https://github.com/user-attachments/assets/5631b0c4-2a70-45e7-b991-2924b3e02984">

Stack apply will be successfully deployed and the resources are available:

<img width="900" alt="image" src="https://github.com/user-attachments/assets/a7d854a1-31cf-4614-93ee-799b69ff2711">

Check created instances in **Compute > Instances**:

<img width="1000" alt="image" src="https://github.com/user-attachments/assets/cd96fd5b-9f21-412c-9148-fa49c9ea1d08">

# Ambari

Access in the browser

Public_IP_Master:8080

<img width="548" alt="image" src="https://github.com/user-attachments/assets/fb63589a-5254-4400-b2d5-3a1d528eb138">

Login as:

```
Userame: admin
Password: admin
```

Click launch install Wizard:

<img width="330" alt="image" src="https://github.com/user-attachments/assets/e53bdbaf-0486-48d0-9953-ed80c6c1e926">

Give a name to the cluster:

<img width="760" alt="image" src="https://github.com/user-attachments/assets/3b34199b-85e8-4b0a-adad-d0effeb3e9e0">

Click "Next"

In Select Version page click "Add version", choose the ODP-VDF.xml file in this repo and click "Read Version Info":

<img width="307" alt="image" src="https://github.com/user-attachments/assets/0bc1919a-4a4c-449d-aa22-dabf8763abaa">

<img width="360" alt="image" src="https://github.com/user-attachments/assets/26e07117-ec85-4b5c-bcf4-a72ec86bc442">

Click "Next"

In page Install Options, select "Perform manual registration on hosts and do not use SSH" and copy in target hosts:

```
master
worker1
worker2
worker3
```

<img width="560" alt="image" src="https://github.com/user-attachments/assets/1265efee-37d0-4c15-9b33-a795108d5d1c">

Click "Next"

<img width="1229" alt="image" src="https://github.com/user-attachments/assets/5bd0e102-a69d-4ce1-8deb-0e779088ba07">

Click "Next"

Choose minimal installation components (HDFS, YARN + MapReduce2, Tez and ZooKeeper):

<img width="1206" alt="image" src="https://github.com/user-attachments/assets/4c78ea0e-9e82-49a8-991a-0e4a84bbe52c">

Click "Next"

Select Master components:

<img width="1013" alt="image" src="https://github.com/user-attachments/assets/58a63cba-8f01-4fbb-9013-fe8643e83eda">

Assign clients:

<img width="1093" alt="image" src="https://github.com/user-attachments/assets/d9a384f0-8283-4fb9-8fe9-26163655a438">

Click "Next"

Leave default in Customize Services:

<img width="1297" alt="image" src="https://github.com/user-attachments/assets/b5615b4f-1cf3-427e-81a1-ef2b90a9214b">

Click "Next"

Click in "Deploy":

<img width="1021" alt="image" src="https://github.com/user-attachments/assets/9289276c-40da-438a-8e93-8a2107d849f3">

Wait for services to be installed:

<img width="1029" alt="image" src="https://github.com/user-attachments/assets/3f092132-979e-4022-8dd8-cc9196bf74f6">

<img width="1033" alt="image" src="https://github.com/user-attachments/assets/c2a296f2-47df-47d4-9e09-37347c6cfb5a">

Click "Next" and "Complete":

<img width="1034" alt="image" src="https://github.com/user-attachments/assets/248bf563-8cf4-4dbb-b5ed-b87e6feaf437">

ssh to instance and run script:

```
./hdfscorrections.sh
```

Then, restart YARN:

<img width="1400" alt="image" src="https://github.com/user-attachments/assets/92fe2508-12e7-4482-bdf8-e6bf00e4c66c">

To install Hive, ssh to instance and run script:

```
./hivesetup.sh
```

Click Add service:

<img width="1404" alt="image" src="https://github.com/user-attachments/assets/5b727103-7deb-418a-944a-2ee7f1e1ed13">

Choose Hive:

<img width="1327" alt="image" src="https://github.com/user-attachments/assets/40882118-5057-4bf0-b30d-0ad26b6093d1">

Select Master:

<img width="1345" alt="image" src="https://github.com/user-attachments/assets/1b24a95d-0ae8-418b-8a4f-0b8004c3d7d1">

Assign Clients:

<img width="1350" alt="image" src="https://github.com/user-attachments/assets/0c9c4aed-27fd-487b-81ab-6cbdb03dd5e1">

In customizing services, choose "Existing PostgreSQL", type "hive" in "Database Password" and to confirm click "Test Connection":

<img width="861" alt="image" src="https://github.com/user-attachments/assets/619710a2-8836-48c5-a78e-390a2b414e47">

Then, click deploy and wait for Hive to be installed:

<img width="1377" alt="image" src="https://github.com/user-attachments/assets/a78b9c6a-c04e-48a9-ba39-71d99075e853">

After setting up all services:

<img width="1400" alt="image" src="https://github.com/user-attachments/assets/048ebbcd-98b4-454a-9f33-392813753d69">

# Interfaces

HDFS:

```
MasterIP:50070
```

<img width="1090" alt="image" src="https://github.com/user-attachments/assets/83b93976-9e75-4582-9e62-42899d5748b9">

YARN:

```
MasterIP:8088
```

<img width="960" alt="image" src="https://github.com/user-attachments/assets/16dd9d29-ccc9-49a3-a941-596f0cf14de4">

MapReduce:

```
MasterIP:19888
```

<img width="1187" alt="image" src="https://github.com/user-attachments/assets/799ea02c-73e3-4350-be98-6ada62524a9a">

Spark3 History server:

```
MasterIP:18082
```

<img width="1005" alt="image" src="https://github.com/user-attachments/assets/f91a3dfe-73fb-4e04-92e4-30a112f75c1a">

NiFi: (HTTP - not encrypted)

http://MasterIP:9090/nifi/

<img width="1192" alt="image" src="https://github.com/user-attachments/assets/2f9bd457-07f1-4b5b-845f-c2500bf26a9c">

# Commands

### Ambari

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

### HDFS

Switch user to hdfs:

```sh
sudo su hdfs
```

### Hive 

Switch user to hive:

```sh
sudo su hive
```

Connect to Hive:

```
beeline -u "jdbc:hive2://master:10000/default" -n hive -p hive
```

<img width="483" alt="image" src="https://github.com/user-attachments/assets/5f08f489-96e7-46e7-b59c-caa304f5f293">


### Spark 

Switch user to spark:

```sh
sudo su spark
```

Launch spark-shell:

```sh
spark-shell
```

<img width="641" alt="image" src="https://github.com/user-attachments/assets/a74a03f1-144b-4e96-8fb8-bc8620fbf44b">


### Zookeeper

Start/Stop/Status Zookeeper server:

```sh
zhServer.sh start 
zhServer.sh stop
zkServer.sh status
```

Connect to Zookeeper CLI:

```sh
zkCli.sh -server 
```

