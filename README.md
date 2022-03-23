# MariaDb-10.7.3 or fewer installation for Linux (Debian based) OS
How to install safely mariadb (mysql) dbms without any errors ?

# Prerequisies

This tutorial concerns only Linux users precisely Kali Linux 2021 even 2022 or Debian 11 Bullseye 
users because this has been successful on these systems.
This tutorial is though shared with no guarantee. Any responsability is so declined !
Any basic knowledge on linux is recommanded, needed to continue.

# GUIDE
These following steps will show you how to install MariaDb database management tool easily with no errors.
Even the "Can't connect to local MySQL server through socket '/var/run/mysqld/mysqld.sock (2)" error will be fixed.

## Removing pre-installed version and its dependencies (completely)
Remove all MariaDb/MySQL packages from the system.
Warning: this will delete all databases from your server. Make sure you have backed everything up before doing this.

Run the following command. This will use apt-get to remove all packages with the name beginning with “mysql”. The –purge switch tells it to purge all configs and related files from the system.

```
sudo apt-get --purge remove "mysql*"
```
## Removing the configs files
Now MariaDB has been completely uninstalled, the next step is to ensure all configs are gone. 
the –purge option tells the system to remove all configs, however it will only remove the default configs that came with the install. Any custom configs that were added after the install will still be there.
Use these commands to get the location of the config files.

```
locate my.cnf
locate mysqld.sock
locate mysql.sock
```
And remove them manually with:

```
rm -rf path/to/config/files
```
Usually, the files are located at /etc/mysql/. Or at:

/etc/my.cnf  
/etc/mysql/my.cnf  
$MYSQL_HOME/my.cnf  
~/.my.cnf  

It is better to move them to a different directory such as the /tmp/ directory.

```
sudo mv /etc/mysql/ /tmp/mysql_configs/
```
Warning: Some servers are configured to delete the contents of the /tmp/ directory on reboot. 
If you want to keep the configs safe it’s best to move them to your home directory.

## Updating the operating system for taking account the new changes on it

```
sudo apt update
sudo apt upgrade -y
```

## Downloading the tarball source
The best way to get Mariadb installed without any errors is to use its tarball source.
Any other way that I've tried has been ended with errors.
The source is at : [https://mariadb.com/downloads/](https://mariadb.com/downloads/)

[Debian 11 users or Kali Linux 2021/2022]

## Checking the status of the MySQL/MariaDb service 

```
sudo systemctl status mysql
```
## Stopping the service

```
sudo systemctl stop mysql
```
or 
```
sudo service mysql stop  
```
## Moving into "/opt/" directory and execute any commands below one by one.

```
cd /opt

wget https://dlm.mariadb.com/2146324/MariaDB/mariadb-10.7.3/repo/debian/mariadb-10.7.3-debian-bullseye-amd64-debs.tar

tar -xf mariadb-10.7.3-debian-bullseye-amd64-debs.tar

rm -rf mariadb-10.7.3-debian-bullseye-amd64-debs.tar

cd mariadb-10.7.3-debian-bullseye-amd64-debs/

sudo ./setup_repository

sudo apt update

sudo apt upgrade -y

```
## Fixing broken packages or dependencies after having executed "sudo apt upgrade"
if broken packages or dependencies appear, they need to be fixed before continuing.
Read carefully the message which will appear after executing "sudo apt upgrade" command.
The fix command is in the message. Execute it with "sudo" !

## Checking MySQL/MariaDb status again

```
sudo systemctl status mysql
```
##  If the service is not running, restart it 

```
sudo systemctl restart mysql
```
##  To prevent this issue from happening, set the MySQL service to automatically start at boot:
This will avoid us to type "sudo systemcl start mysql" when still needed.

```
sudo systemctl enable mysql
```
## Creating if not exists a new config file

```
sudo nano /etc/mysql/my.cnf
```
## Paste the foolowing lines to the content

```
[mysqld]
socket=/var/run/mysqld/mysqld.sock
[client]
socket=/var/run/mysqld/mysqld.sock

```
## Restarting the service

```
sudo systemctl restart mysql
```

## Checking the MySQL Folder Permission

```
sudo chmod -R 755 /var/run/mysqld

sudo systemctl restart mysql
```

## Checking for Multiple MySQL Instances

```
ps -A|grep mysqld
```
 If there are multiple MySQL instances running, terminate them with:

```
sudo pkill mysqld
```

## Restarting the MySQL service to start a single instance of MySQL:

```
sudo systemctl restart mysql
```

## Connecting the databases with default or root logins

```
mysql -u root -p
```
## Connecting the databases with root logins having deleted default ones

```
mysql_install_db [options]
```



[REFERENCES]

https://phoenixnap.com/kb/mysql-server-through-socket-var-run-mysqld-mysqld-sock-2
https://kerneltalks.com/tools/how-to-start-stop-restart-mariadb-server-in-linux/
https://mariadb.com/kb/en/mysql_install_db/
https://mariadb.com/kb/en/installing-system-tables-mysql_install_db/


[How to cite]
ADI Junior, 2022. MariaDb-10.7.3 or fewer installation for Linux (Debian based) OS. Site: https://github.com/rootoor-dev/MariaDb-10.7.3-or-fewer-installation/edit/main/README.md

THANK YOU !!!

