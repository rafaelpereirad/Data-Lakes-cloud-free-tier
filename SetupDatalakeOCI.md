# Terraform




# Ambari

Access Public_IP_Master:8080

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

Click Next

In Select Version page click "Add version", choose the ODP-VDF.xml file in this repo and click "Read Version Info":

<img width="307" alt="image" src="https://github.com/user-attachments/assets/0bc1919a-4a4c-449d-aa22-dabf8763abaa">

<img width="360" alt="image" src="https://github.com/user-attachments/assets/26e07117-ec85-4b5c-bcf4-a72ec86bc442">

Click Next

In page Install Options, select "Perform manual registration on hosts and do not use SSH" and copy in target hosts:

```
master
worker1
worker2
worker3
```

<img width="560" alt="image" src="https://github.com/user-attachments/assets/1265efee-37d0-4c15-9b33-a795108d5d1c">


ssh to instance and run script:

```
./hdfscorrections.sh
```


After setting up all services:

<img width="1400" alt="image" src="https://github.com/user-attachments/assets/048ebbcd-98b4-454a-9f33-392813753d69">

Interfaces:

HDFS:

MasterIP:50070

<img width="1290" alt="image" src="https://github.com/user-attachments/assets/83b93976-9e75-4582-9e62-42899d5748b9">

YARN:

MasterIP:8088

<img width="1360" alt="image" src="https://github.com/user-attachments/assets/16dd9d29-ccc9-49a3-a941-596f0cf14de4">

MapReduce:

MasterIP:19888

<img width="1387" alt="image" src="https://github.com/user-attachments/assets/799ea02c-73e3-4350-be98-6ada62524a9a">

NiFi: (HTTP - not encrypted)

http://MasterIP:9090/nifi/

<img width="1392" alt="image" src="https://github.com/user-attachments/assets/2f9bd457-07f1-4b5b-845f-c2500bf26a9c">

