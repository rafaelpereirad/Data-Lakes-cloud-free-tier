
# Ambari

Access Public_IP_Master:8080

Login as:

```
user: admin
password: admin
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



