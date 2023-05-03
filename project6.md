# WEB SOLUTION WITH WORDPRESS

In this project, it is tasked to prepare storage infrastructure on two Linux servers and implement a basic web solution using WordPress. WordPress is a free and open-source content management system written in PHP and paired with MySQL or MariaDB as its backend Relational Database Management System (RDBMS).

## Three-tier Architecture
---

Three-tier Architecture is a client-server software architecture pattern that comprise of 3 separate layers.

- Presentation Layer (PL): This is the user interface such as the client server or browser on your laptop.

- Business Layer (BL): This is the backend program that implements business logic. Application or Webserver

- Data Access or Management Layer (DAL): This is the layer for computer data storage and data access. Database Server or File System Server such as FTP server, or NFS Server

## LAUNCH AN EC2 INSTANCE THAT WILL SERVE AS “WEB SERVER”.
---
![new ebs volume](https://github.com/abibolola/dareyio-Projects/blob/main/Screenshots/project6/New%20ebs%20volumes.JPG)

![attach volume to ec2](https://github.com/abibolola/dareyio-Projects/blob/main/Screenshots/project6/Attach%20volume.JPG)

- Login to the web server and use lsblk command to inspect what block devices are attached to the server

![list of blocks](https://github.com/abibolola/dareyio-Projects/blob/main/Screenshots/project6/lsblk.JPG)

- Use gdisk utility to create a single partition on each of the 3 disks
```
sudo gdisk /dev/xvdf
sudo gdisk /dev/xvdg
sudo gdisk /dev/xvdh
```

![partition new volume1](https://github.com/abibolola/dareyio-Projects/blob/main/Screenshots/project6/partition%20new%20volumes1.JPG)

![partition new volume2](https://github.com/abibolola/dareyio-Projects/blob/main/Screenshots/project6/partition%20new%20volumes2.JPG)

![lsblk after partition](https://github.com/abibolola/dareyio-Projects/blob/main/Screenshots/project6/lsblk%20after%20partition.JPG)

- Install lvm2 package on the web server using ```sudo yum install lvm2```
- Use *pvcreate utility* to mark each of the 3 disks new partition as physical volumes (PVs) to be used by LVM

```sudo pvcreate /dev/xvdf1 /dev/xvdg1 /dev/xvdh1```

- Verify that the Physical volume has been created successfully by running  ```sudo pvs```

![pvs](https://github.com/abibolola/dareyio-Projects/blob/main/Screenshots/project6/pvs.JPG)

- Use *vgcreate utility* to add all 3 PVs to a volume group (VG). Name the VG webdata-vg and verify the sucessfully created volume group using ```sudo vgs```

```sudo vgcreate webdata-vg /dev/xvdh1 /dev/xvdg1 /dev/xvdf1```

![volume group](https://github.com/abibolola/dareyio-Projects/blob/main/Screenshots/project6/vgs.JPG)

- Use *lvcreate utility* to create 2 logical volumes. apps-lv (Use half of the PV size), and logs-lv Use the remaining space of the PV size. NOTE: apps-lv will be used to store data for the Website while, logs-lv will be used to store data for logs.
```
sudo lvcreate -n apps-lv -L 14G webdata-vg
sudo lvcreate -n logs-lv -L 14G webdata-vg
```

Verify that your Logical Volume has been created successfully by running ```sudo lvs```

![lvs](https://github.com/abibolola/dareyio-Projects/blob/main/Screenshots/project6/lvs.JPG)

![lsblk lvs](https://github.com/abibolola/dareyio-Projects/blob/main/Screenshots/project6/complete%20lsblk.JPG)

- Use mkfs.ext4 to format the logical volumes with ext4 filesystem
```
sudo mkfs -t ext4 /dev/webdata-vg/apps-lv
sudo mkfs -t ext4 /dev/webdata-vg/logs-lv
```

- Create /var/www/html directory to store website files  ```sudo mkdir -p /var/www/html```

- Create /home/recovery/logs to store backup of log data  ```sudo mkdir -p /home/recovery/logs```

- Mount /var/www/html on apps-lv logical volume  ```sudo mount /dev/webdata-vg/apps-lv /var/www/html/```

- Use *rsync utility* to backup all the files in the log directory /var/log into /home/recovery/logs (This is required before mounting the file system)  ```sudo rsync -av /var/log/. /home/recovery/logs/```

- Mount /var/log on logs-lv logical volume. (Note that all the existing data on /var/log will be deleted. That is why using *rsync utility* to back it up  is very important)  ```sudo mount /dev/webdata-vg/logs-lv /var/log```

- Restore log files back into /var/log directory  ```sudo rsync -av /home/recovery/logs/. /var/log```

![df -h after mount](https://github.com/abibolola/dareyio-Projects/blob/main/Screenshots/project6/df%20after%20mounting.JPG)

- Update /etc/fstab file so that the mount configuration will persist after restart of the server.

- The UUID of the device will be used to update the /etc/fstab file using ```sudo bldid```

![sudo blkid](https://github.com/abibolola/dareyio-Projects/blob/main/Screenshots/project6/sudo%20blkid.JPG)

```sudo vi /etc/fstab```

- Update /etc/fstab in this format using your own UUID and rememeber to remove the leading and ending quotes.

![vi /etc/fstab](https://github.com/abibolola/dareyio-Projects/blob/main/Screenshots/project6/vi%20etc-fstab.JPG)

- Test the configuration and reload the daemon
```
 sudo mount -a
 sudo systemctl daemon-reload
 ```

 ![df -h after fstab](https://github.com/abibolola/dareyio-Projects/blob/main/Screenshots/project6/df%20-h%20after%20fstab.JPG)

 ## Step 2 — Prepare the Database Server
 ---

 - Use the RedHat Linux for the database server. Repeat the same steps as for the Web Server, but instead of apps-lv create db-lv and mount it to /db directory instead of /var/www/html/.

 ```sudo vgcreate database-vg /dev/xvdf1 /dev/xvdg1 /dev/xvdh1```

```sudo lvcreate -n db-lv -L 20G database-vg```

```sudo mkfs.ext4 /dev/database-vg/db-lv```

![vgdisplay](https://github.com/abibolola/dareyio-Projects/blob/main/Screenshots/project6/vgdisplay%20for%20db%20volumes.JPG)

## Step 3 — Install WordPress on your Web Server EC2
---

- Update the repository ```sudo yum -y update```

- Install wget, Apache and it’s dependencies  ```sudo yum -y install wget httpd php php-mysqlnd php-fpm php-json```

- Start Apache
```
sudo systemctl enable httpd
sudo systemctl start httpd
```

- To install PHP and it’s dependencies

```
sudo yum install https://dl.fedoraproject.org/pub/epel/epel-release-latest-8.noarch.rpm
sudo yum install yum-utils http://rpms.remirepo.net/enterprise/remi-release-8.rpm
sudo yum module list php
sudo yum module reset php
sudo yum module enable php:remi-7.4
sudo yum install php php-opcache php-gd php-curl php-mysqlnd
sudo systemctl start php-fpm
sudo systemctl enable php-fpm
setsebool -P httpd_execmem 1
```

- Restart Apache ```sudo systemctl restart httpd```

- Download wordpress and copy wordpress to var/www/html
```
mkdir wordpress
  cd   wordpress
  sudo wget http://wordpress.org/latest.tar.gz
  sudo tar xzvf latest.tar.gz
  sudo rm -rf latest.tar.gz
  cp wordpress/wp-config-sample.php wordpress/wp-config.php
  cp -R wordpress /var/www/html/
```

- Configure SELinux Policies
```
sudo chown -R apache:apache /var/www/html/wordpress
sudo chcon -t httpd_sys_rw_content_t /var/www/html/wordpress -R
sudo setsebool -P httpd_can_network_connect=1
```

## Step 4 — Install MySQL on your DB Server EC2

```
sudo yum update
sudo yum install mysql-server
```

- Verify that the service is up and running by using ```sudo systemctl status mysqld``` , if it is not running, restart the service and enable it so it will be running even after reboot:
```
sudo systemctl restart mysqld
sudo systemctl enable mysqld
```

## Step 5 — Configure DB to work with WordPress
---
```
sudo mysql
CREATE DATABASE wordpress;
CREATE USER `myuser`@`172.31.90.74` IDENTIFIED BY 'mypass';
GRANT ALL ON wordpress.* TO `myuser`@`172.31.90.74`;
FLUSH PRIVILEGES;
SHOW DATABASES;
exit
```

## Step 6 — Configure WordPress to connect to remote database
---

- Open MySQL port 3306 on DB Server EC2. For extra security, allow access to the DB server ONLY from the Web Server’s IP address, so in the Inbound Rule configuration specify source as /32

![mysql inbound rule](https://github.com/abibolola/dareyio-Projects/blob/main/Screenshots/project6/DB%20inbound%20rule%20mysql.JPG)

- Install MySQL client and test that you can connect from the Web Server to the DB server by using mysql-client
```
sudo yum install mysql
sudo mysql -u admin -p -h 172.31.18.52
```

![web to db connect](https://github.com/abibolola/dareyio-Projects/blob/main/Screenshots/project6/mysql%20connect%20from%20WebServer.JPG)


### ENCOUNTER THIS ERROR
---

![ERROR](https://github.com/abibolola/dareyio-Projects/blob/main/Screenshots/project6/error%20opening%20wordpress.JPG)

- This requires debugging and troubleshooting
- The issue is from the WordPress database credentials that are stored in the wp-config.php file

![wp-config edit](https://github.com/abibolola/dareyio-Projects/blob/main/Screenshots/project6/wp-config%20edit.JPG)

![wordpress open](https://github.com/abibolola/dareyio-Projects/blob/main/Screenshots/project6/opening%20wordpress.JPG)

![final webpage](https://github.com/abibolola/dareyio-Projects/blob/main/Screenshots/project6/wordpress%20webpage.JPG)

