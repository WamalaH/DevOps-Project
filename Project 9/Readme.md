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


I created a directory to store the website and a second one to store backup log data files by issueing the following commands

`sudo mkdir -p /var/www/html`

`sudo mkdir -p /home/recovery/logs`

![Alt text](<Images/created var_www_html.png>)


I than mounted /var/www/html to /dev/webdata-vg/apps-lv by issueing the below command.

`sudo mount /dev/webdata-vg/apps-lv /var/www/html/`

![Alt text](<Images/mounted html to apps.png>)


Before mounting the log directory, I had to first backup */var/log* into *home/recovery/logs*. This was achived by using the **rsync** command.

`sudo rsync -av /var/log/. /home/recovery/logs/`

I than mounted the log directory by issuing the below command.

`sudo mount /dev/webdata-vg/logs-lv /var/log`

![Alt text](<Images/backed up all files in log dir and mounted var log.png>)


I than updated the */etc/fstab* file with the **uuid** of the devices so that the mount configuration would persist after restarting the server.

![Alt text](<Images/etc_fstab edited.png>)

I than tested the configuration and reloaded the daemon with the following commands.

`sudo mount -a`

`sudo systemctl daemon-reload`

## Installing Wordpress ##

I installed Wordpress on my *Webserver* by using **sudo yum** because the instance was running *Redhat* destro. but before installing wordpress,I had to update my server and also install *wget*,*apache* and its dependencies.

`sudo yum -y update`

`sudo yum -y install wget httpd php php-mysqlnd php-fpm php-json`

Before installing *PHP*, I had to install the latest *php* remi repo on my server by issuing the below commands. The *remi* repo has to correspond to your version of **RedHat**.

`sudo yum install https://dl.fedoraproject.org/pub/epel/epel-release-latest-9.noarch.rpm`

`sudo yum install yum-utils http://rpms.remirepo.net/enterprise/remi-release-9.rpm`

`sudo yum module list php`

`sudo yum module reset php`

`sudo yum module enable php:remi-8.4`
`sudo yum install php php-opcache php-gd php-curl`

`php-mysqlnd`

`sudo systemctl start php-fpm`

`sudo systemctl enable php-fpm`

`sudo setsebool -P httpd_execmem 1`

The above also set up some default security features.

![Alt text](<Images/httpd-apache installed.png>)

I than restarted *Apache* and also confirmed that *php* was installed and running on my server.

`sudo systemctl restart httpd`

![Alt text](<Images/httpd-apache installed.png>)

Finally I installed **wordpress** on my *webserver* and copied it to **var/www/html** 

`mkdir wordpress`

`cd   wordpress`

`sudo wget http://wordpress.org/latest.tar.gz`

`sudo tar xzvf latest.tar.gz`

`sudo rm -rf latest.tar.gz`

`cp wordpress/wp-config-sample.php wordpress/wp-config.php`

`cp -R wordpress /var/www/html/`

I also changed file ownership/permission so that apache could work with wordpress.

![Alt text](<Images/changed file permissions.png>)

lastly I installed


## Preparing DataBase Server ##

I prepared the Database server by repeating the same steps as the webserver but instead of *apps-lv*, I created *db-lv* and mounted it to **/db** directory instead of */var/www/html/*

## Installing MySQL on DataBase Server 

Since the Database server also had a **RedHat** destro. I used *sudo yum* to update and  install MySql.

`sudo yum update`

`sudo yum install mysql-server`

![Alt text](<Images/mysql-server installed.png>)

I also configured the Database to work with *wordPress*
by creating a user with the private IP address of my webserver as the name of the user.

![Alt text](<Images/mysql access for webserver.png>)


I also opened port 3306 to allow MySQL to communicate.

![Alt text](<Images/opened mysql port.png>)

As a result of the above configuration I was able to access **WordPress** from my browser using the my webServer public IP address.

![Alt text](<Images/wordpress succes 1.png>)



![Alt text](<Images/wordpress success 2.png>)


![Alt text](<Images/super success.png>)















