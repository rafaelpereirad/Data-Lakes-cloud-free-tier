Hive se der o erro:

Error: Could not open client transport with JDBC Uri: jdbc:hive2://Master:10000/default: Failed to open new session: java.lang.RuntimeException: org.apache.hadoop.ipc.RemoteException(org.apache.hadoop.security.authorize.AuthorizationException): User: hive is not allowed to impersonate postgres (state=08S01,code=0)

Editar no Ambari com valor * e dar restart em tudo:

<img width="963" alt="image" src="https://github.com/user-attachments/assets/dba63de1-3083-4715-b9dc-f73248a709e1">

hadoop.proxyuser.hive.groups
hadoop.proxyuser.hive.hosts

E a propriedade hive.server2.enable.doAs colocar como true:

<img width="289" alt="image" src="https://github.com/user-attachments/assets/24c67011-9203-43b3-9117-dc86eb260814">

# Carregando o TPC-DS

scp -i /path/privatekey -r /path/DSGen-software-code-3.2.0rc1 opc@IP_Master:~

```
sudo yum install docker
docker pull gcc:9
```

<img width="552" alt="image" src="https://github.com/user-attachments/assets/1baabd4a-4e56-41a0-8f9e-1b6f88797bd6">

```
docker run -it -v /home/opc/DSGen-software-code-3.2.0rc1:/project gcc:9 /bin/bash
apt update && apt install -y flex
apt update && apt install -y bison
ln -s /usr/bin/flex /usr/bin/lex
cd /project/tools
make CC=gcc
exit
```

Para gerar 1GB de dados:
```
mkdir ~/dataTPCDS
cd DSGen-software-code-3.2.0rc1/tools/
./dsdgen -scale 1 -dir ~/dataTPCDS
```

dataTPCDS:

<img width="472" alt="image" src="https://github.com/user-attachments/assets/c7f2e4a7-42ee-4307-a7e4-42609f59405f">
