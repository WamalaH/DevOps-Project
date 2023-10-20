# Implementing Wordpress website with LVM storage management #

In this project I will attempt to prepare storage infastructure on two Linux servers and implement a basic web solution using *wordpress*.

Wordpress is a free and open-source content management system written in **PHP** and paired with **MySQL** or **MariaDB** as it's backend Relational Database Management System(RDBMS).

In this project I will be using *RedHat* as my OS.

## Requirements ##

- Two  Ec2 in instances running on RedHat OS.
- 6 Volumes inthe same AZ as my instances.

## Challenges ##

While implementing this project I encounted a challenge of finding the latest **Remirepo** as *RedHat* OS needs this to correctly install **Apache**. I also observed that the **Remirepo** has to correspond with your *RedHat* OS version.

## Step by Step
### Implementing LVM on Linux servers. ##

I started by provisioning a linux instance from **AWS** running on *RedHat* OS. I also went ahead and provisioned  3 storage Volumes each with 10GB of space that I attached to my instance which i named **Web_Server**

![Alt text](<Images/provisioned 3vols and attached them to wbserver.png>)

Using the *gdisk* utility I created a single partition on each of the 3 disks.


![Alt text](<Images/created single partition on all 3 vols.png>)


I run the *lsblk* command to confirm that the partitions had been created.

![sudo gdisk /dev/name of your disk][def]




[def]: <Images/confirmation of partitions.png>


Next, using *sudo yum install lvm2* i insatlled the LVM package. 

I maked the 3 disks as physical volumes(PVs) by using the *pvcreate* utility. **sudo pvcreate /dev/--**

![Alt text](<Images/PVs created.png>)


By using the *vgcreate* utility, I was able to add all the 3 newly created physical volumes to a volume group (VG) which I named **webdata-vg**

![Alt text](<Images/Vg-webdata created.png>)


I created two logical volumes, *app-lv* and *logs-lv* and allocated each of the 14GB of space by using the *lvcreate* utility.

![Alt text](<Images/logical volumes created.png>)

Using the *mkfs.ext4* I formatted the two logical volumes with **ext4** file system. Below are the commands that i issued

`sudo mkfs -t ext4 /dev/webdata-vg/apps-lv`

`sudo mkfs -t ext4 /dev/webdata-vg/logs-lv`

![Alt text](<Images/formated logical volumes.png>)





