---
layout: post
title: Deployment and Securing of a LAMP Stack
date: 2021-12-02 01:35:00 +0000

---

In this report/tutorial, deployment of a LAMP stack for use with WordPress is discussed, each of the technologies used explained and steps to secure the deployment on both a local virtual machine and on a public cloud. A comparison will be made between both of these deployments

*Keywords—  LAMP, Ubuntu, GCP, Public Cloud Web Development Platform, WordPress Server Security;* 



## 1. Introduction

For many years now web developers have been looking for stable, secure and robust platforms for deploying their applications. Most modern web applications have four main parts in their deployment, An underlying operating system, a web server, a database system and a form of server side scripting functionality

One of the most popular deployment stacks is called LAMP. LAMP is an abbreviation of the names of the 4 efficient open source technologies used. In this case, Linux is used as the operating system, Apache as the web server, MariaDB as the database system, and PHP as the server-side scripting language.

*Key Terms-*

LAMP has been a well known software set with mentions of the stack as a concept around the turn of the new millennia, and still today used by many large sites such as Wikipedia [1]. LAMP is credited as being a versatile, all in one solution, for hosting a web application, hence the continued popularity

Linux is an operating system, thought as one of the largest open source projects, means this operating system is one of the most versatile, with distributions building apon the code base, suited for many different types of deployments, whether it be a fully featured desktop operating system, a dedicated server or a low power embedded device, linux can be modified as needed 

Apache - Apache is an open source web server, designed to handle HTTP requests from a client’s web browser and serves the requested pages. Apache is one of the most popular web servers currently being used by 31.3% of known sites according to w3tech at the time of writing[2]. Due to the open source nature of apache, development is carried out by both the Apache Software Foundation and a community of developers who can implement new features and functions through the use of “modules”. This modularity is what makes Apache so powerful, with good performance on linux making these two pieces of software a good match.  

MariaDB - A community developed fork of the relational database system MySQL. MariaDB is designed to be a perfect replacement for MySQL, supporting the same features, using the same default configuration files, and the same ports. The development team of MariaDB had this as one of their main goals, that and providing speed improvement over MySQL

MySQL - A open source relational database system currently owned and developed by Oracle. The concept of the relational database was invented in 1970 by Edgar Frank Codd [3]. This concept allowed for large amounts of data to be collected in one entity, and with speed, insert, store and search the data through the use of complex relationships in the data defined at the time of entry

PHP -  PHP is a server-side scripting language. PHP is used to allow a website's clients to induce an operation on the server hosting the website without having to run anything on their own device, for example requesting data from a database system to display on the page served back to the client. PHP was designed to be work in a large number of different applications, and can be easily implemented along side HTML/HTML5 code to allow for very powerful web applications is described in [4] 



## 2. Local Machine Deployment

For the first example of a LAMP stack deployment, we will be using a Ubuntu 20.04 install running in a virtual machine with VMWare being used as the hypervisor.

Initially, a new user is made on the operating system to help secure the machine, preventing the server from running with root privileges, which could be extremely harmful if the server is compromised. This user is created with the name “lamp” and password “serveruser” using the commands below:

	sudo useradd -c "lamp" -m -s /bin/bash lamp

	passwd lamp

	serveruser


##### *Figure 1: Setting password for new user*

![Figure 1](./image1.png "Figure 1: Setting password for new user")


Next, this user must be given superuser privileges in order to make installations of new software and access necessary files/directories, after the user account is created, switch to the new user to begin the install process of the LAMP stack, this can be done with the commands below:

	sudo usermod -aG sudo lamp

	sudo su - lamp

The first software package to install is the Apache web server, we will use apt-get command to pull the latest version of the server, before installing Apache, apt-get will also be updated to get the latest package lists, both commands are below:

	sudo apt-get update

	sudo apt-get install apache2

To test if Apache was installed correctly, either open a web browser and navigate to localhost or make use of curl to request the page from inside the terminal. The test should return the sample page that comes with Apache

	curl http://127.0.0.1

The next package to install is PHP, and a module for Apache to enable PHP functionality, these can both be installed using the command:

	sudo apt-get install php libapache2-mod-php

To validate the install, a test page was created for PHP, which will display information about the PHP installation, as before either a web browser or curl can be used for testing as shown below:

	sudo sh -c 'echo "&lt;?php phpinfo(); ?>" > /var/www/html/phpinfo.php'
	
	curl http://127.0.0.1/phpinfo.php

In order to secure the Apache server, some configuration changes need to be made to prevent the server from providing sensitive information about itself to potential attackers, like the server’s version number, etc. Open the custom configuration file for editing using a text editor of your choice like so:

	sudo nano /etc/apache2/conf-available/custom.conf

Add these following lines to the configuration file:

	ServerTokens Prod

	ServerSignature Off

	TraceEnable Off

	Options all -Indexes

After saving the file, the custom configuration can be enabled in Apache like so:

	

	sudo a2enconf custom

Enabling the mod-security module for Apache continues to improve the security of the server, allowing for the filtering of HTTP requests and responses. The installation and enabling of this module is can be accomplished using these commands:

	sudo apt-get install libapache2-mod-security2

	sudo a2enmod security2

To use mod-security a configuration file must be loaded, to use the default config copy it using the following command:

	sudo mv /etc/modsecurity/modsecurity.conf-recommended /etc/modsecurity/modsecurity.conf

Next, the configuration file must be modified to enable and enforce the filtering rules, like so:

 

	sudo nano /etc/modsecurity/modsecurity.conf

Change “SecRuleEngine” to “On” as seen in Figure 2


##### *Figure 2: Modified Mod-Security Config*


![Figure 2](./image2.png "Figure 2: Modified Mod-Security Config")


Next Install the MariaDB server and the MySQL PHP plugin to allow for PHP accessing of the database

	sudo apt-get install mariadb-server php-mysql

Once the install has finished, check the service status using the command below, it should state “running” as seen in Figure 3:

	sudo systemctl status mariadb


##### *Figure 3: Running MariaDB server*




![Figure 3](./image3.png "Figure 3: Running MariaDB server")


Run the secure installation wizard to setup passwords, removing anonymous users,  disabling remote root login, etc

	sudo mysql_secure_installation


##### *Figure 4: Completion of MySQL Secure Wizard*


![Figure 4](./image4.png "Completion of MySQL Secure Wizard")


After this remote MySQL access was disabled by binding all connections to localhost preventing external connections to the database by doing the following:

	sudo nano /etc/mysql/my.cnf

Add line:

	bind-address = 127.0.0.1

Restart MariaDB to apply

	sudo systemctl restart mysql.service

To make use of the LAMP stack, WordPress will also be installed on this server.

So next add a user and create a database in MariaDB for WordPress to use, this user has the username “dbadmin” and a password of “pass123”

	sudo mysql

	create database wordpress;

	create user 'dbadmin'@'localhost' identified by 'pass123';

This user should have full privileges over the database, so use these commands to grant the required privilege and reload the user privileges:

	grant all privileges to wordpress.* as ‘dbadmin’@’localhost’ identified by ‘pass123’;

	flush privileges;

To install the latest version of WordPress use:

	wget [https://wordpress.org/latest.zip](https://wordpress.org/latest.zip)

	unzip latest.zip

	sudo cp -r wordpress /var/www/html

To point WordPress to the MariaDB database we must first copy and modify the sample config file like so:

	 cd /var/www/html/wordpress

	sudo cp wp-config-sample.php wp-config.php 

	sudo nano wp-config.php

The details created earlier should be entered into this config file as seen in Figure 5


##### *Figure 5: Modified WordPress Config*

![Figure 5](./image5.png "Modified WordPress Config")


In order to get Apache to serve the WordPress pages as intended, the document root must be modified to the newly copied WordPress folder, open the default page config using:

	sudo nano /etc/apache2/sites-enabled/000-default.conf

Change the DocumentRoot to  /var/www/html/wordpress

Finally, Uncomplicated Firewall is used to block traffic on ports that are not required for the functionality of the server. The commands used to enable and set the firewall rules are as follow:

	sudo ufw enable

	sudo ufw allow 22

	sudo ufw allow 80

	sudo ufw allow 443

Check the rules are correct using the following command:

	sudo ufw status


##### *Figure 6: Enabled Firewall rules*


![Figure 6](images/image6.png "Figure 6: Enabled Firewall rules")


After this, the WordPress wizard will be accessible by navigating to the server’s IP address as seen below:


##### *Figure 7: WordPress setup wizard*




![Figure 7](./image7.png " Figure 7: WordPress setup wizard")




## 3. Public Cloud Deployment

In this deployment, Google Cloud Platform is used as the public cloud. While using Google Cloud there are two methods of deploying a WordPress + LAMP software stack, The first being creating a blank VM and using the same setup as the local machine deployment, with the extra step of having to provision hardware for the Linux VM to run on, in testing I set up the VM manually using this command in the Google Cloud Shell:

	gcloud compute instances create lamp-stack 
	    --image-family=debian-10 
	    --image-project=debian-cloud
	    --machine-type=e2-micro
	    --zone=us-central1-a


##### *Figure 8: VM creation in Google Cloud Shell*


![Figure 8](./image8.png "Figure 8: VM creation in Google Cloud Shell")


To begin the setup, use SSH to remote into the virtual machine using the command:

	gcloud compute ssh lamp-stack

There were no differences in the install processes during testing even across the different Linux distributions used.

Another method of deploying a WordPress + LAMP stack is making use of Google Cloud’s “Click to Deploy” images available on the marketplace

This method allows for a user of Google Cloud to extremely rapidly deploy a majority preconfigured WordPress including the same software stack as on the manual installation as seen below in the list of installed software in the image:


##### *Figure 9: Click to Deploy Software list*



![Figure 9](./image9.png "Figure 9: Click to Deploy Software list")


However, after testing, other than the firewall settings, which are only accepting traffic on port 80 and 443, it seems that the rest of the security of the server is left at relatively out-the-box levels, looking at the apache modules



## 4. Conclusion
To conclude, these methods are robust ways to setup and deploy a platform for a web application, with suitable security measures proposed in the process to make sure that the deployment is not only functional, but also safe. To make a comparison between the Public Cloud offerings and Local Deployments, If you do not have hardware to host the platform on, Public Cloud offerings such as Google Cloud Platform allow you to set up your own platform as you wish, or save time by using a preconfigured image, all without having to purchase hardware.  However if large amounts of traffic is expected, costs of hosting the public cloud can become very large.



## 5. Evaluation

Overall this report covers a significant amount on the configuration of such web application platforms, however more research could have been done on the alternatives on how to fully make use of the public cloud for this kind of deployment.

As I continued to research, it was also discovered that using the mod-security module for Apache allows for use of the “OWASP ModSecurity Core Rule Set'' which is a set of generic attack detection rules which mod-security can use to prevent more attacks, this rule set is based upon a largely contributed, open attack listing called the Open Web Application Security Project. This would prevent from even more advanced attacks from infiltrating the webserver.


## References



1. Wikipedia Technical Information. (2021). Retrieved 30 September 2021, from [https://en.wikipedia.org/wiki/Wikipedia:FAQ/Technical](https://en.wikipedia.org/wiki/Wikipedia:FAQ/Technical)
   
2. Usage Statistics and Market Share of Apache, November 2021. W3techs.com. (2021). Retrieved 2 November 2021, from [https://w3techs.com/technologies/details/ws-apache](https://w3techs.com/technologies/details/ws-apache).
   
3. Karanjit, A. (2016). MEAN vs. LAMP Stack.
    
4. Emelianov, M. (2017). What makes LAMP stack important for your web projects. Open Source for You,