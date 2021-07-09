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

=========================
sudo vi /etc/rc.local

echo "never" > /sys/kernel/mm/transparent_hugepage/enabled
echo "never" > /sys/kernel/mm/transparent_hugepage/defrag
=============================

sudo yum install -y wget
sudo wget https://dev.mysql.com/get/Downloads/Connector-J/mysqlconnector-java-5.1.47.tar.gz ~/
tar zxvf mysql-connector-java-5.1.47.tar.gz
sudo mkdir -p /usr/share/java/
sudo cp ~/mysql-connector-java-5.1.47/mysql-connector-java-5.1.47-bin.jar 
/usr/share/java/mysql-connector-java.jar
