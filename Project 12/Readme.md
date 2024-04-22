# Ansible refactoring and static assignments. #

## Code Refactoring ##

Refactoring is a general term in computer programming. It means making changes to the source code without changing expected behaviour of the software. The main idea of refactoring is to enhance code readability, increase maintainability and extensibility, reduce complexity, add proper comments without affecting the logic.

In my case,I will move things around a little bit in the code but the overall state of the infrastructure will remain the same.

## Jenkins job enhancement ##

To enhance my jenkins job, I started by creating a new directory on my Jenkins-Ansible server and named it *ansible-config-artifact* and changed permissions so that Jenkins can save files in the folder.

![alt text](<images/1-created dir and changed permissions.png>)


I  than went ahead and installed *copy artifact* plugin from the jenkins web console.

![alt text](<images/2 - installed copy artifact.png>)

I created a new freestyle project named *save_artifacts* and configured it accordingly to be triggered by the completion of my existing *ansible* project.

![alt text](<images/3 -created new free style project.png>)


I than went ahead and tested by configuration.


![alt text](<images/4 -tested save -artifacts.png>)


## Refactor Ansible code by importing other playbooks into site.yml ##

In order to make my *playbook* more organised,I would need to break tasks up into different files hence organising complex sets of tasks and being able to reuse them.

In order to do this I created a new file in *playbooks* and named it **site.yml**. **site.yml** would become the a parent to all my other *playbooks*.

I also created a newfolder in the root of my repo and named it **static-assignments** where all other children playbooks would be stored.

![alt text](<images/4 -created site.yml and static-assi and moved common.yml.png>)


To test my new dev enviroment,I created another playbook under *static-assignments* and named it **common-del.yml** which was to configure deletion of the wireshark utility. 

![alt text](<images/5-updated site.yml.png>)

I also edited my *ansible.cfg* file to point to my inventory to *ansible-config-artifacts*

![alt text](<images/6-edited ansible.cfg file.png>)

I than ran my playbook and deleted wireshank.

![alt text](<images/7 -deleted wireshark.png>)

## Configure UAT Webservers with a role 'Webserver' ##

For this part, I provisioned two fresh EC2 instances using RHEL 8 image to use as *uat* servers.

I also created a webserver role manually.

![alt text](<images/9-created webserver roles maneully.png>)


I also updated my *uat-webserver* fille with the private IP addresses of my servers.

![alt text](<images/10 - updated uta.yml.png>)

I also edited the */etc/ansible/ansible.cfg* by uncommenting roles_path and provided the full path to my roles directory.

![alt text](<images/11-updated ansible cfg.png>)

I than added some configuration tasks in *main.yml* 

![alt text](<images/12- updated main.yml.png>)

To do the following,

- Install and configure Apache (httpd service)
- Clone Tooling website from GitHub https://github.com//tooling.git.
- Ensure the tooling website code is deployed to /var/www/html on each of 2 UAT Web servers.
- Make sure httpd service is started.

Within the *static-assignments* folder, I created a new assignment for *uat-webservers* named **uat-webservers.yml** and also updated my *site.yml* to refer my *uat-webservers.yml* role inside *site.yml*

![alt text](<images/13 -referenced webserver in static-assigment.png>)

![alt text](<images/14 -updated site.yml.png>)

Finally I ran my playbook.

![alt text](<images/15 -run playbook.png>)

Success!!!!!


![alt text](<images/16 - success.png>)



![alt text](<images/17 -sucess.png>)