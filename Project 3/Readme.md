# LAMP Stack Implementation

LAMP stack is a comprehensive set of technologies that are used to build and deploy dynamic websites by combining  four main technologies mainly **Linux**, **Apach**,**MYSQL** and **PHP**(LAMP) 


# Requirements #

1. Install Apache and update the firewall
2. Install MySQL
3. Install PHP
4. Create a virtual host for your website



Inorder to complete this project, I had to provision a ubuntu server from AWS by registering for an account and than launched an instance of t3.micro with ubuntu 22.04LTS

I created 2 inbound rules one to allow ssh to be accessable from everywhere and 2nd one to open hht port to allow access from the web.

I than went ahead and connected to my instance using the windows powershell and the public keys from the E2c instance

## Installing PHP and updating the firewall ##

Apache server is an open source software that is available for free.It runs on more than half of all webservers in the world.It can be highly customized to meet the needs of many different environments by using extensions and modules.

 I run the ** sudo apt update **
  ![Alt text](<images/apt update.png>)
