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

