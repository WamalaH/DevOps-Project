# Devops Tooling Website Solution. #

In this project,I will be implementing a tooling website solution which makes access toDevOps tools within the corporate infrastructure easily accessible.

Some of the *tools* will be 

- **Jenkins** - free and open source automation server used to build *CI/CD* pipelines.

- **Kubernetes** - an open source container-orchestration system for automating computer application deployment,scaling and management.

- **Jfrog Artifactory** - Universal Repository Manager supporting all major packaging formats, build tools and CI
servers. Artifactory

- **Rancher** - an open source software platform that enables organizations to run and manage Docker and
Kubernetes in production.

- **Grafana** - a multi-platform open source analytics and interactive visualization web application.

- **Prometheus** - An open-source monitoring system with a dimensional data model, flexible query language,
efficient time series database and modern alerting approach.

## Prerequisites ##

1. Infrastructure: AWS
2. Webserver Linux: Red Hat Enterprise Linux 8
3. Database Server: Ubuntu 20.04 + MySQL
4. Storage Server: Red Hat Enterprise Linux 8 + NFS Server
5. Programming Language: PHP
6. Code Repository: GitHub


## Implementing a business website using NFS for the backend file storage. ##

I provisioned a **RedHat** instance from AWS and attached 3 volumes to it.

![Alt text](<Images/volumes attached.png>)

Using `sudo gdisk` I created partions on the attached volumes.

![Alt text](<Images/partionions created.png>)

I confirmed that the partitions were created.

![Alt text](<Images/partions confirmed.png>)

Using `sudo yum install lvm2 -y` I installed lvm

![Alt text](<Images/LVM installed.png>)

`sudo pvcreate /dev/...`, I marked each of the 3 disks as physical volumes.

![Alt text](<Images/pv created.png>)

![Alt text](<Images/pvs confirmed.png>)

Next I added all my 3 PVs to a volume group (VG) that I named **vg-webdata**

![Alt text](<Images/VG created.png>)


I created 3 *logical volumes* namely **lv-apps**, **lv-logs** and **lv-opt**.

![Alt text](<Images/lvs created.png>)


I than went ahead and formatted as **xfs** by using the command `sudo mkfs -t xfs /dev/webdata-vg/..` 

![Alt text](<Images/xfs file system formatted.png>)


Next I created 3 mount points and mounted my *Logical volumes* to the respectful mount points.

![Alt text](<Images/mounting done.png>)


I than installed **NFS** server and configured it to start on reboot and made sure it was up and runnning by issueing the below commands.

`sudo yum -y update`

`sudo yum install nfs-utils -y`

`sudo systemctl start nfs-server.service`

`sudo systemctl enable nfs-server.service`

`sudo systemctl status nfs-server.service`

![Alt text](<Images/nfs installed.png>)

I than set up permissions that would allow my websevers to read, write and execute files on the**NFS**

![Alt text](<Images/permission set to allow webservers.png>)


By editing the `sudo vi /etc/exports`,
I gave access toNFS for clients within the same subnet.

![Alt text](<Images/access for clients in the same subnet.png>)


By issueing the following command `rpcinfo -p | grep nfs` I was able to see what port was being used by **NFS** and opened the ports by adding anew inbound rule on my instance.

![Alt text](<Images/opened ports.png>)


## Configuring backend database. ##

I provisioned a **ubuntu** instance from *AWS* and named it as Db_server.

I installed **MySQL** using `sudo apt install mysql`

![Alt text](<Images/mysql installed on db_server.png>)

I went ahead to create a database and named it *tooling*. I also created a user namely, webaccess and granted permissission to *webaccess* user on *tooling* database to do anything only from the webservers **subnet cidr**

![Alt text](<Images/database and users.png>)


## Preparing the Web Servers ##

I provisioned 3 *RedHat* instances from *AWS* and did the following to all 3 of them.

I installed **NFS** client on them by issuing the following command `sudo yum install nfs-utils nfs4-acl-tools -y`

I mounted */var/www/* and targeted the NFS server's export for apps.

I installed installed *remi's repository, *apache* and *PHP* 

I also mounted the log folder for *Apache* to NFS server's export for logs.

![Alt text](<Images/mounted and targeted nfs.png>)


I edited `/etc/fstab` so that the mountings would persist on the web server after reboot.

![Alt text](<Images/nfs ws persist.png>)

I forked the tooling source code from Darey.io Github Account to my git hub account.

Opened port 80 on all the webservers.

# Results #

![](Images/success.png)


