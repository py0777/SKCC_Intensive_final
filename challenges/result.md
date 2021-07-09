# 호스트 접속
ssh -i SKCC_Intensive.pem centos@15.165.142.171
ssh -i SKCC_Intensive.pem centos@15.165.182.88
ssh -i SKCC_Intensive.pem centos@15.165.188.198
ssh -i SKCC_Intensive.pem centos@15.165.189.35
ssh -i SKCC_Intensive.pem centos@15.165.26.238

#Linux setup
sudo passwd centos   pw: centos
sudo vi /etc/ssh/sshd_config
--> PasswordAuthentication yes 으로변경??
sudo systemctl restart sshd.service

#i. Add the following linux accounts to all nodes
```
sudo useradd training
sudo echo 'training' | sudo passwd --stdin training
sudo usermod -u 8800 training
```
```
sudo groupadd skcc
sudo usermod -g skcc training
```

```

sudo chmod +w /etc/sudoers
sudo vi /etc/sudoers
trainin ALL=(ALL)  NOPASSWD: ALL

sudo vi /etc/sysctl.conf
vm.swappiness=1
````

sudo vi /etc/hosts

10.0.0.21  cm
10.0.0.8  mn
10.0.0.9  dn1
10.0.0.10  dn2
10.0.0.14  dn3

10.0.0.21 cm.bdai.com cm
10.0.0.8 m1.bdai.com m1
10.0.0.9 d1.bdai.com d1
10.0.0.10 d2.bdai.com d2
10.0.0.14 d3.bdai.com d3

* Change the hostname (각각의 호스트에서 수행)
sudo hostnamectl set-hostname cm.bdai.com
hostname -f
sudo hostnamectl set-hostname m1.bdai.com
hostname -f
sudo hostnamectl set-hostname d1.bdai.com
hostname -f
sudo hostnamectl set-hostname d2.bdai.com
hostname -f
sudo hostnamectl set-hostname d3.bdai.com
hostname -f
```
sudo  yum install nscd ntp -y
sudo systemctl start ntpd
sudo systemctl start nscd
```
status로 확인
sudo systemctl status ntpd
sudo systemctl status nscd
재접속하면 
![image](https://user-images.githubusercontent.com/7609848/125024734-576a7480-e0bc-11eb-8002-564c8be8c17e.png)

=========================
sudo vi /etc/rc.local

echo "never" > /sys/kernel/mm/transparent_hugepage/enabled
echo "never" > /sys/kernel/mm/transparent_hugepage/defrag
=============================

iii. List the Linux release you are using
> df -h
![image](https://user-images.githubusercontent.com/7609848/125025018-da8bca80-e0bc-11eb-8c72-eb79f239602d.png)

v. List the command and output for yum repolist enabled
> sudo vi /etc/yum/pluginconf.d/fastestmirror.conf
enabled=0
> repolist
![image](https://user-images.githubusercontent.com/7609848/125025367-8503ed80-e0bd-11eb-900c-ff5d93e5b8fc.png)

vi. List the /etc/passwd entries for training (only in master name node
> sudo cat /etc/passwd | grep training
vii. List the /etc/group entries for skcc (only in master name node)
> sudo cat /etc/group | grep skcc
![image](https://user-images.githubusercontent.com/7609848/125025711-20955e00-e0be-11eb-90ae-3abd9e6e2528.png)
viii. List output of the flowing commands:
getent group skcc
getent passwd training

![image](https://user-images.githubusercontent.com/7609848/125025826-54708380-e0be-11eb-9547-695127844330.png)

# jdbc설치
sudo wget https://dev.mysql.com/get/Downloads/Connector-J/mysql-connector-java-5.1.47.tar.gz

tar zxvf mysql-connector-java-5.1.47.tar.gz
sudo mkdir -p /usr/share/java/
sudo cp ~/mysql-connector-java-5.1.47/mysql-connector-java-5.1.47-bin.jar /usr/share/java/mysql-connector-java.jar

# mariadb 설치
sudo yum install mariadb-server
sudo systemctl stop mariadb
sudo systemctl enable mariadb
sudo systemctl start mariadb
sudo /usr/bin/mysql_secure_installation

mysql -u root -p
비밀번호 1
```sql
CREATE DATABASE scm DEFAULT CHARACTER SET utf8 DEFAULT COLLATE utf8_general_ci;
GRANT ALL ON scm.* TO 'scm'@'%' IDENTIFIED BY 'scm';
CREATE DATABASE amon DEFAULT CHARACTER SET utf8 DEFAULT COLLATE utf8_general_ci;
GRANT ALL ON amon.* TO 'amon'@'%' IDENTIFIED BY 'amon';
CREATE DATABASE rman DEFAULT CHARACTER SET utf8 DEFAULT COLLATE utf8_general_ci;
GRANT ALL ON rman.* TO 'rman'@'%' IDENTIFIED BY 'rman';
CREATE DATABASE hue DEFAULT CHARACTER SET utf8 DEFAULT COLLATE utf8_general_ci;
GRANT ALL ON hue.* TO 'hue'@'%' IDENTIFIED BY 'hue';
CREATE DATABASE metastore DEFAULT CHARACTER SET utf8 DEFAULT COLLATE utf8_general_ci;
GRANT ALL ON metastore.* TO 'hive'@'%' IDENTIFIED BY 'hive';
CREATE DATABASE sentry DEFAULT CHARACTER SET utf8 DEFAULT COLLATE utf8_general_ci;
GRANT ALL ON sentry.* TO 'sentry'@'%' IDENTIFIED BY 'sentry';
CREATE DATABASE navms DEFAULT CHARACTER SET utf8 DEFAULT COLLATE utf8_general_ci;
GRANT ALL ON navms.* TO 'navms'@'%' IDENTIFIED BY 'navms';
CREATE DATABASE nav DEFAULT CHARACTER SET utf8 DEFAULT COLLATE utf8_general_ci;
GRANT ALL ON nav.* TO 'nav'@'%' IDENTIFIED BY 'nav';
CREATE DATABASE oozie DEFAULT CHARACTER SET utf8 DEFAULT COLLATE utf8_general_ci;
GRANT ALL ON oozie.* TO 'oozie'@'%' IDENTIFIED BY 'oozie';
GRANT ALL ON test.* TO 'eduuser'@'%' IDENTIFIED BY 'letmein';FLUSH PRIVILEGES;
```
![image](https://user-images.githubusercontent.com/7609848/125029380-7f5dd600-e0c4-11eb-9a4a-b489273e7bdf.png)


1. A command and output that shows the hostname of your 
database server
2. A command and output that reports the database server version
3. A command and output that lists all the databases in the server
![image](https://user-images.githubusercontent.com/7609848/125030023-6e619480-e0c5-11eb-96fd-df76b18da018.png)


sudo vi /etc/yum.repos.d/cloudera-nanager.repo

[cloudera-manager]
```
# Packages for Cloudera Manager, Version 5, on RedHat or CentOS 7 x86_64
name=Cloudera Manager
baseurl=http://ec2-3-34-114-205.ap-northeast-2.compute.amazonaws.com/cloudera-repos/cdh5/5.16.2/
gpgkey =http://ec2-3-34-114-205.ap-northeast-2.compute.amazonaws.com/cloudera-repos/cdh5/5.16.2/RPM-GPG-KEY-cloudera
gpgcheck = 1
```
sudo yum install cloudera-manager-daemons cloudera-manager-agent cloudera-manager-server
sudo rpm --import http://ec2-3-34-114-205.ap-northeast-2.compute.amazonaws.com/cloudera-repos/cdh5/5.16.2/RPM-GPG-KEY-cloudera

cloudera manager server
sudo yum install cloudera-manager-daemons cloudera-manager-agent cloudera-manager-server
sudo /usr/share/cmf/schema/scm_prepare_database.sh mysql scm scm scm

sudo systemctl start cloudera-scm-server
http://15.165.142.171:7180/cmf/login
![image](https://user-images.githubusercontent.com/7609848/125032389-dbc2f480-e0c8-11eb-9e8d-cce70ff4eba7.png)

hdfs
yarn
sqoop
hive
impala

http://ec2-3-34-114-205.ap-northeast-2.compute.amazonaws.com/cloudera-parcels/cdh5/5.16.2/
![image](https://user-images.githubusercontent.com/7609848/125033877-debee480-e0ca-11eb-9a0f-c59d11c68fec.png)
![image](https://user-images.githubusercontent.com/7609848/125035659-0e6eec00-e0cd-11eb-8006-23a6f8fe3015.png)
![image](https://user-images.githubusercontent.com/7609848/125035767-2e061480-e0cd-11eb-91f5-59bdad5f83e3.png)



