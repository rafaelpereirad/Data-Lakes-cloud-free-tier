
# 1.SSH to Instance

## 1.1 Initial update and necessary tools installation

In both Master and Worker nodes:

```sh
sudo yum update -y

sudo yum install -y curl unzip tar wget sed gcc* openssl-devel openssl krb5-workstation krb5-libs python-devel python java-1.8.0-openjdk-devel snappy openssh-server openssh-clients

wget -O- https://clemlabs.s3.eu-west-3.amazonaws.com/RPM-GPG-KEY-SHA256-Jenkins -O /tmp/RPM-GPG-KEY-SHA256-Jenkins

sudo rpm --import /tmp/RPM-GPG-KEY-SHA256-Jenkins

sudo wget -O /etc/yum.repos.d/odp.repo https://clemlabs.s3.eu-west-3.amazonaws.com/centos9-aarch64/odp-release/1.2.2.0-128/odp.repo

sudo wget -O /etc/yum.repos.d/ambari.repo https://clemlabs.s3.eu-west-3.amazonaws.com/centos9-aarch64/ambari-release/2.7.9.0.0-61/ambari.repo

sudo yum update -y

sudo yum install odp-select.aarch64 -y

sudo yum install ambari-agent.aarch64 -y
```

Only in Master node:

```sh
sudo yum install ambari-server.aarch64 -y

sudo yum install postgresql-server postgresql-contrib -y
```

## 1.2 Services settings to configure environment

```sh
sudo systemctl disable firewalld
sudo service firewalld stop

sudo sudo setenforce 0
enabled=0

sudo systemctl start chronyd
sudo systemctl enable chronyd
```

Set up a user console in case ssh permissions get broken in machines, you can access it in OCI console connection:

```sh
sudo useradd console
sudo usermod -aG wheel console
sudo passwd console
```

<img width="460" alt="image" src="https://github.com/user-attachments/assets/84ea17ee-0920-4853-ad92-03561af8e189">

```sh
sudo nano /etc/hosts
```

Copy on top:

```sh
10.0.0.2   master
10.0.0.3   worker1
10.0.0.4   worker2
10.0.0.5   worker3
```

Create ssh key pair in master:

```sh
ssh-keygen -t rsa -P '' -f ~/.ssh/id_rsa
cat .ssh/id_rsa.pub >> ~/.ssh/authorized_keys
cat ~/.ssh/authorized_keys
```

Copy the generated key in Worker nodes:

```
sudo nano /root/.ssh/authorized_keys
```

## 1.3 Setting up Ambari-server

```sh
sudo ambari-server setup -j /usr/lib/jvm/java-1.8.0-openjdk
sudo ambari-server start
sudo ambari-agent start
```

Logs to Debug:

```sh
cat /var/log/ambari-agent/ambari-agent.log
cat /var/log/ambari-server/ambari-server.log
```

# 2.Ambari Server

DataLake

RHEL9: https://clemlabs.s3.eu-west-3.amazonaws.com/centos9-aarch64/odp-release/1.2.2.0-128/rpms/

Hosts:

```sh
master
worker1
worker2
worker3
```

## 2.1 YARN Fail solution:

```sh
sudo -u hdfs hdfs dfs -chown root:root /odp/apps/1.2.2.0-128/hbase

sudo -u hdfs hdfs dfs -chmod 755 /odp/apps/1.2.2.0-128/hbase

sudo -u hdfs hdfs dfs -put /var/lib/ambari-agent/tmp/yarn-ats/1.2.2.0-128/hbase.tar.gz /odp/apps/1.2.2.0-128/hbase/

sudo -u hdfs hdfs dfs -chmod 555 /odp/apps/1.2.2.0-128/hbase

sudo ambari-server restart
```

## 2.2 Installing Hive:

SSH to Master Node:

```sh
sudo ambari-sever stop

sudo nano /var/lib/pgsql/data/postgresql.conf
```

Edit:

```sh
   Listen_addresses = ‘*’
   port = 5432
```

```sh
sudo nano /var/lib/pgsql/data/pg_hba.conf
```

Add at the bottom:

```sh
      host    all   hive              10.0.0.2/32         trust
```

```sh
sudo -u postgres psql -U postgres
```

Run commands to create hive database:

```sql
CREATE DATABASE hive;
CREATE USER hive WITH PASSWORD 'hive';
GRANT ALL PRIVILEGES ON DATABASE hive TO hive;
```

```sh
sudo service postgresql restart

sudo yum install postgresql-jdbc.noarch -y

sudo ambari-server setup -j /usr/lib/jvm/java-1.8.0-openjdk --jdbc-db=postgres --jdbc-driver=/usr/share/java/postgresql-jdbc.jar

sudo ambari-server start
```

In case of error starting Hive:

```sh
sudo mkdir -p /etc/hive/
sudo ambari-server restart
```
