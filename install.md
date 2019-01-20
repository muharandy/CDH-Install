# Personal Notes

## Preinstall Check

Check swappiness
```
cat /proc/sys/vm/swappiness
```

Change swappiness to 1
```
sysctl vm.swappiness=1
```

change swappingess to 1 permanently
REF: https://scottlinux.com/2010/06/23/adjust-your-swappiness/
```
vi /etc/sysctl.conf
```

Disable transparent huge page support
REF: https://blacksaildivision.com/how-to-disable-transparent-huge-pages-on-centos
```
echo 'never' > /sys/kernel/mm/transparent_hugepage/enabled
echo 'never' > /sys/kernel/mm/transparent_hugepage/defrag
```


## Create Database
```
create database amon DEFAULT CHARACTER SET UTF8;
grant all on amon.* to 'amon'@'%' IDENTIFIED BY 'amon';

create database rman DEFAULT CHARACTER SET UTF8;
grant all on rman.* to 'rman'@'%' IDENTIFIED BY 'rman';

create database metastore DEFAULT CHARACTER SET UTF8;
grant all on metastore.* to 'hive'@'%' IDENTIFIED BY 'hive';

create database sentry DEFAULT CHARACTER SET UTF8;
grant all on sentry.* to 'sentry'@'%' IDENTIFIED BY 'sentry';

create database nav DEFAULT CHARACTER SET UTF8;
grant all on nav.* to 'nav'@'%' IDENTIFIED BY 'nav';

create database navms DEFAULT CHARACTER SET UTF8;
grant all on navms.* to 'navms'@'%' IDENTIFIED BY 'navms';

create database hue DEFAULT CHARACTER SET UTF8;
grant all on hue.* to 'hue'@'%' IDENTIFIED BY 'hue';

create database oozie default character set utf8;
grant all privileges on oozie.* to 'oozie'@'localhost' identified by 'oozie';
grant all privileges on oozie.* to 'oozie'@'%' identified by 'oozie';
```

## force to install old version of cloudera manager
- wget the repo file
- change the repo file to point to the old version
- dont forget to run yum clean all


## mysql install
```
wget https://dev.mysql.com/get/mysql57-community-release-el7-11.noarch.rpm
rpm -i mysql57-community-release-el7-11.noarch.rpm
```

## mysql old version install (in this case 5.5)
- modify mysql-community.repo so that the enabled one is just the 5.5 (change the enabled=1 to enable it, enabled=0 to disabled it)
- remove the previous installation (if any)
- yum install mysql-community-server
- check to make sure that it install the desired version, not the latest one

## Start mysql
```
systemctl start mysqld
```

## Setup mysql secure installation
```
/usr/bin/mysql_secure_installation
```

## install mysql JDBC connector
```
wget https://dev.mysql.com/get/Downloads/Connector-J/mysql-connector-java-5.1.45.tar.gz
tar xzvf mysql-connector-java-5.1.45.tar.gz
mkdir /usr/share/java
cp mysql-connector-java-5.1.45/mysql-connector-java-5.1.45-bin.jar /usr/share/java/mysql-connector-java.jar
```

## Setup Mysql Replication

https://dev.mysql.com/doc/refman/5.5/en/replication-howto.html
```
CREATE USER 'repl'@'%.gce.cloudera.com' IDENTIFIED BY 'repl';
GRANT REPLICATION SLAVE ON *.* TO 'repl'@'%.gce.cloudera.com';

CHANGE MASTER TO MASTER_HOST='fajar-bc-1.gce.cloudera.com', MASTER_USER='repl', MASTER_PASSWORD='repl', MASTER_LOG_FILE='', MASTER_LOG_POS=4;
```



## Setup CM Repo
```
wget https://archive.cloudera.com/cm5/redhat/7/x86_64/cm/cloudera-manager.repo
mv cloudera-manager.repo /etc/yum.repos.d/
```

Install CM
```
yum install oracle-j2sdk1.7
yum install cloudera-manager-daemons cloudera-manager-server
```


## Prepare SCM Database
```
create database scm;
grant all on scm.* to 'scm'@'%' IDENTIFIED BY 'scm';
```

run the script
```
/usr/share/cmf/schema/scm_prepare_database.sh mysql scm scm
```

## CM Setup
Open CM at host-ip:7180
follow the setup wizard
