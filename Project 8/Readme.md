# Automating Loadbalancer configuration with Shell scripting #


In this project I will be automating the entire procecess of implementing a load balancer to distribute traffic across two backend webservers. As a DevOps engineer automation is at the heart of what we do.

Automation helps us speed the deployment of services and reduces the chance of making errors in day to day activities/tasks.

## Prerequisites:

- Provision three EC2 instances from *AWS* running on *ubuntu*

- Two of the three instances will be configured with **Apache**,which will be my webservers.

- The third EC2 instance will be configured with **Nginx** which is going to be used as my *loadbalancer*


## Deploying and Configuring the Webservers. ##

I provisioned two EC2 instances and opened port 8000 on both of them to allow traffic from anywhere using the security group.

![Alt text](<Images/provisioned instance and opened port 8000.png>)


Next I created a file **install.sh** and wrote my shell script .


![Alt text](<Images/created install.sh file.png>)

I than changed permissions on **install.sh** file in order to make it an excutable file.

![Alt text](<Images/changed file permission on install.sh.png>)


I than ran the command below:

>     ./install.sh PUBLIC_IP

I replaced *PUBLIC_IP* with the correct public ip addresses of my webservers.

![Alt text](<Images/installed apache.png>)


Basically what the script did was to update apt, installed apache2, created/opened a new listening port,8000. The script also changed a few file permissions inorder to change the virtualHost port from port 80 to port 8000. It also created a new **index.html** file .


The script was run on both of my webservers.

I was than able to get to my website via the public ip addresses of my webservers via port 8000

![Alt text](Images/webserver_1.png)


![Alt text](Images/webserver_2.png)