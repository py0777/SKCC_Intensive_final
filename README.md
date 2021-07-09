os
sudo vi /etc/ssh/sshd_config
change ->
PasswordAuthentication yes
#PasswordAuthenticaltion no
으로 변경
sudo systemctl restart sshd.service
** Update /etc/host
sudo vi /etc/hosts
add ->
******* THIS IS PRIVATE IP (Add this to each of the hosts)
10.0.0.246 cm.bdai.com cm
10.0.0.247 m1.bdai.com m1
10.0.0.111 d1.bdai.com d1
10.0.0.42 d2.bdai.com d2
10.0.0.190 d3.bdai.com d3
** Change the hostname (각각의 호스트에서 수행)
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
** REBOOT THE SERVER AND CHECK** Install Java 8 or above on all nodes
sudo yum install -y java-1.8.0-openjdk-devel
******************* ON ALL NODES *************** 
Install a supported JDBC connector on all nodes 
sudo yum install -y wget
sudo wget https://dev.mysql.com/get/Downloads/Connector-J/mysqlconnector-java-5.1.47.tar.gz ~/
tar zxvf mysql-connector-java-5.1.47.tar.gz
sudo mkdir -p /usr/share/java/
sudo cp ~/mysql-connector-java-5.1.47/mysql-connector-java-5.1.47-bin.jar 
/usr/share/java/mysql-connector-java.jar
** Import Cloudera manager repository on cm host only
sudo wget http://ec2-3-34-114-205.ap-northeast2.compute.amazonaws.com/cloudera-repos/cdh5/5.16.2/clouderamanager.repo -P /etc/yum.repos.d/
sudo rpm --import http://ec2-3-34-114-205.ap-northeast2.compute.amazonaws.com/cloudera-repos/cdh5/5.16.2/RPM-GPG-KEYcloudera
** Modify cloudera.manager.repo to point the baseurl to the repository server 
and the parcel directory
******SAMPLE:
[cloudera-manager]
# Packages for Cloudera Manager, Version 5, on RedHat or CentOS 7 x86_64
name=Cloudera Manager
baseurl=http://ec2-3-34-114-205.ap-northeast2.compute.amazonaws.com/cloudera-repos/cdh5/5.16.2/
gpgkey =http://ec2-3-34-114-205.ap-northeast2.compute.amazonaws.com/cloudera-repos/cdh5/5.16.2/RPM-GPG-KEYcloudera
gpgcheck = 1
*******SAMPLE:
** Install Cloudera Manager on the cm host
sudo yum install -y cloudera-manager-daemons cloudera-manager-serversudo yum install -y cloudera-manager-daemons cloudera-manager-agent
** Install MariaDB on the cm host and set security
sudo yum install -y mariadb-server
sudo systemctl enable mariadb
sudo systemctl start mariadb
sudo /usr/bin/mysql_secure_installation
** Create the databases and users in MariaDB
mysql -u root -p (암호 admin)
CREATE DATABASE scm DEFAULT CHARACTER SET utf8 DEFAULT COLLATE 
utf8_general_ci;
GRANT ALL ON scm.* TO 'scm-user'@'%' IDENTIFIED BY 'somepassword';
CREATE DATABASE amon DEFAULT CHARACTER SET utf8 DEFAULT COLLATE 
utf8_general_ci;
GRANT ALL ON amon.* TO 'amon-user'@'%' IDENTIFIED BY 'somepassword';
CREATE DATABASE rman DEFAULT CHARACTER SET utf8 DEFAULT COLLATE 
utf8_general_ci;
GRANT ALL ON rman.* TO 'rman-user'@'%' IDENTIFIED BY 'somepassword';
CREATE DATABASE hue DEFAULT CHARACTER SET utf8 DEFAULT COLLATE 
utf8_general_ci;
GRANT ALL ON hue.* TO 'hue-user'@'%' IDENTIFIED BY 'somepassword';
CREATE DATABASE metastore DEFAULT CHARACTER SET utf8 DEFAULT COLLATE 
utf8_general_ci;
GRANT ALL ON metastore.* TO 'metastore-user'@'%' IDENTIFIED BY 
'somepassword';
CREATE DATABASE sentry DEFAULT CHARACTER SET utf8 DEFAULT COLLATE 
utf8_general_ci;
GRANT ALL ON sentry.* TO 'sentry-user'@'%' IDENTIFIED BY 'somepassword';
CREATE DATABASE oozie DEFAULT CHARACTER SET utf8 DEFAULT COLLATE 
utf8_general_ci;
GRANT ALL ON oozie.* TO 'oozie-user'@'%' IDENTIFIED BY 'somepassword';
GRANT ALL ON test.* TO 'eduuser'@'%' IDENTIFIED BY 'letmein';FLUSH PRIVILEGES;
SHOW DATABASES;
EXIT;
** Setup the CM database and start Cloudera Manager Server
sudo /usr/share/cmf/schema/scm_prepare_database.sh mysql scm scm-user 
somepassword
sudo rm /etc/cloudera-scm-server/db.mgmt.properties
sudo systemctl start cloudera-scm-server
******************* ONLY ON THE CM NODE *******************
