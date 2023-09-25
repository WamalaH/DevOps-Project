# Implementing Loadblancers with Nginx #

Load balancing is the distrubution of work loads of tasks among several computers or servers so that no one computer or server gets overloaded with too much work.

This helps everything run smoothly and ensures websites and apps work quickly and don't get too slow.

In this project am going to **Nginx** as my load balancer. **Nginx** is a versatile software that acts like a webserver,reverse proxy and a load balancer.

## Prerequisite ##
- Two aws EC2 instances running on ubuntu and installed with *apache* webserver on both of them .

- One aws EC2 instance running on ubuntu and installed with **Nginx** and configure it to act as a load balancer distributing traffic across the two apach webservers.


## Apache Webservers ##

I provisioned two EC2 webservers, updated them and installed apache on them using the below command

 > sudo apt update -y &&  sudo apt install apache2 -y 



 ![confirmation of apach
 ](<Images/installed and confirmed apache2 .png>)


## Configuring Apache to serve a page showing it,s public IP ##
Using my *vi* text editor, I added a new listen directive for port 8000.

![Alt text](<Images/configured apache to serve content on port 8000.png>)

Next I edited  /etc/apache2/sites-available/000-default.conf file and changed port 80 to port 8000.
![Alt text](<Images/changed virutaul host port to 8000.png>)


I restarted apache in order to load the new configuration using the command below:

> sudo systemctl restart apache2

I created a new index.html file,changed ownship of the file and also replaced/overrod the default html file of apache.

![Alt text](<Images/changed ownership and also overrod.png>)

I restarted apache and I was able to connect to my website using the server  public IP address.

![Alt text](<Images/able to connect to my website using my public ip address.png>)

This was done on both of my EC2 instances that were provisioned as webservers with *Apache* installed on both of them.



## Configuring Nginx as a Load Balancer ##

I provisioned a 3rd EC2 instance,updated it and installed *Nginx* using the command below:

> sudo apt update -y && sudo apt install nginx -y


![Alt text](<Images/installed nginx.png>)


I edited the */etc/nginx/conf.d/loadbalancer.conf* with new information about the Public IP addresses of my two webservers including the repective listening ports (port 8000) and also included the public IP address of my Load Balancer.

![Alt text](<Images/configured nginx loadbalaner.png>)


I tested my configuration with the below command:

> sudo nginx -t

![Alt text](<Images/tested configuration of my nginx.png>)


Finally I tested my Load balacer set up by using my Nginx Load Balancer Public IP address to get to my website pages.

![Alt text](Images/sucesses.png)

