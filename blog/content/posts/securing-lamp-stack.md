---
layout: post
title: Deployment and Securing of a LAMP Stack
date: 2021-07-28 10:38:00 +0000

---
<!-----

You have some errors, warnings, or alerts. If you are using reckless mode, turn it off to see inline alerts.
* ERRORs: 0
* WARNINGs: 0
* ALERTS: 9

Conversion time: 2.393 seconds.


Using this HTML file:

1. Paste this output into your source file.
2. See the notes and action items below regarding this conversion run.
3. Check the rendered output (headings, lists, code blocks, tables) for proper
   formatting and use a linkchecker before you publish this page.

Conversion notes:

* Docs to Markdown version 1.0β40
* Sun Nov 10 2024 17:03:19 GMT-0800 (PST)
* Source doc: CSN10109 - Cloud and Advanced Networks
* This document has images: check for >>>>>  gd2md-html alert:  inline image link in generated source and store images to your server. NOTE: Images in exported zip file from Google Docs may not appear in  the same order as they do in your doc. Please check the images!

----->


<p style="color: red; font-weight: bold">>>>>>  gd2md-html alert:  ERRORs: 0; WARNINGs: 0; ALERTS: 9.</p>
<ul style="color: red; font-weight: bold"><li>See top comment block for details on ERRORs and WARNINGs. <li>In the converted Markdown or HTML, search for inline alerts that start with >>>>>  gd2md-html alert:  for specific instances that need correction.</ul>

<p style="color: red; font-weight: bold">Links to alert messages:</p><a href="#gdcalert1">alert1</a>
<a href="#gdcalert2">alert2</a>
<a href="#gdcalert3">alert3</a>
<a href="#gdcalert4">alert4</a>
<a href="#gdcalert5">alert5</a>
<a href="#gdcalert6">alert6</a>
<a href="#gdcalert7">alert7</a>
<a href="#gdcalert8">alert8</a>
<a href="#gdcalert9">alert9</a>

<p style="color: red; font-weight: bold">>>>>> PLEASE check and correct alert issues and delete this message and the inline alerts.<hr></p>


<p>
Deployment and Securing of a LAMP Stack
</p>
<p>
<strong><em>Introduction gives some background and context. However, there is academic research or literature review undertaken. That should have been included in the coursework. Presentation is adequate but could be improved (commands in different font, figures are not clear). 15/40</em></strong>
</p>
<p>
<strong><em>Proof of experimentation is presented, but extensively with unnecessary details. At times it looks more a tutorial than an academic report. some technical details could better serve the report as annexes. There were some elements of discussion but they need more critical depth. 25/60</em></strong>
</p>
<p>
<strong><em>Abstract</em>— In this report, deployment of a LAMP stack for use with WordPress is discussed, each of the technologies used explained and steps to secure the deployment on both a local virtual machine and on a public cloud. A comparison will be made between both of these deployments</strong>
</p>
<p>
<strong><em>Keywords—  LAMP, Ubuntu, GCP, Public Cloud Web Development Platform, WordPress Server Security; </em></strong>
</p>
<ol>

<li>
Introduction
<p>
For many years now web developers have been looking for stable, secure and robust platforms for deploying their applications. Most modern web applications have four main parts in their deployment, An underlying operating system, a web server, a database system and a form of server side scripting functionality
</p>
<p>
One of the most popular deployment stacks is called LAMP. LAMP is an abbreviation of the names of the 4 efficient open source technologies used. In this case, Linux is used as the operating system, Apache as the web server, MariaDB as the database system, and PHP as the server-side scripting language.
</p>
<p>
LAMP has been a well known software set with mentions of the stack as a concept around the turn of the new millenia, and still today used by many large sites such as Wikipedia [1]. LAMP is credited as being a versatile, all in one solution, for hosting a web application, hence the continued popularity
</p>
<p>
Linux is an operating system, thought as one of the largest open source projects, means this operating system is one of the most versatile, with distributions building apon the code base, suited for many different types of deployments, wether it be a fully featured desktop operating system, a dedicated server or a low power embedded device, linux can be modified as needed 
</p>
<p>
Apache - Apache is an open source web server, designed to handle HTTP requests from a client’s web browser and serves the requested pages. Apache is one of the most popular web servers currently being used by 31.3% of known sites according to w3tech at the time of writing[2]. Due to the open source nature of apache, development is carried out by both the Apache Software Foundation and a community of developers who can implement new features and functions through the use of “modules”. This modularity is what makes Apache so powerful, with good performance on linux making these two pieces of software a good match.  
</p>
<p>
MariaDB - A community developed fork of the relational database system MySQL. MariaDB is designed to be a perfect replacement for MySQL, supporting the same features, using the same default configuration files, and the same ports. The development team of MariaDB had this as one of their main goals, that and providing speed improvement over MySQL
</p>
<p>
MySQL - A open source relational database system currently owned and developed by Oracle. The concept of the relational database was invented in 1970 by Edgar Frank Codd [3]. This concept allowed for large amounts of data to be collected in one entity, and with speed, insert, store and search the data through the use of complex relationships in the data defined at the time of entry
</p>
<p>
PHP -  PHP is a server-side scripting language. which allows ver-side scripting language, PHP is used to allow a website's clients to induce an operation on the server hosting the website without having to run anything on their own device, for example requesting data from a database system to display on the page served back to the client. PHP was designed to be work in a large number of different applications, and can be easily implemented along side HTML/HTML5 code to allow for very powerful web applications is described in [4] 
</p></li>

<li>Local Machine Deployment
<p>
For the first instance of LAMP stack deployments, we will be using an Ubuntu 20.04 install running in a virtual machine with VMWare being used as the hypervisor.
</p>
<p>
Initially, a new user is made on the operating system to help secure the machine, preventing the server from running with root privileges, which could be extremely harmful if the server is compromised. This user is created with the name “lamp” and password “serveruser” using the commands below:
</p>
<p>
sudo useradd -c "lamp" -m -s /bin/bash lamp
</p>
<p>
passwd lamp
</p>
<p>
serveruser
</p>
<h4>Figure 1: Setting password for new user</h4>


<p>


<p id="gdcalert1" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image1.png). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert2">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>


<img src="images/image1.png" width="" alt="alt_text" title="image_tooltip">

</p>
<p>
Next, this user must be given superuser privileges in order to make installations of new software and access necessary files/directories, after the user account is created, switch to the new user to begin the install process of the LAMP stack, this can be done with the commands below:
</p>
<p>
sudo usermod -aG sudo lamp
</p>
<p>
sudo su - lamp
</p>
<p>
The first software package to install is the Apache web server, we will use apt-get command to pull the latest version of the server, before installing Apache, apt-get will also be updated to get the latest package lists, both commands are below:
</p>
<p>
sudo apt-get update
</p>
<p>
sudo apt-get install apache2
</p>
<p>
To test if Apache was installed correctly, either open a web browser and navigate to localhost or make use of curl to request the page from inside the terminal. The test should return the sample page that comes with Apache
</p>
<p>
curl http://127.0.0.1
</p>
<p>
The next package to install is PHP, and a module for Apache to enable PHP functionality, these can both be installed using the command:
</p>
<p>
sudo apt-get install php libapache2-mod-php
</p>
<p>
To validate the install, a test page was created for PHP, which will display information about the PHP installation, as before either a web browser or curl can be used for testing as shown below:
</p>
<p>
sudo sh -c 'echo "&lt;?php phpinfo(); ?>" > /var/www/html/phpinfo.php'
</p>
<p>
curl http://127.0.0.1/phpinfo.php
</p>
<p>
In order to secure the Apache server, some configuration changes need to be made to prevent the server from providing sensitive information about itself to potential attackers, like the server’s version number, etc. Open the custom configuration file for editing using a text editor of your choice like so:
</p>
<p>
sudo nano /etc/apache2/conf-available/custom.conf
</p>
<p>
Add these following lines to the configuration file:
</p>
<p>
ServerTokens Prod
</p>
<p>
ServerSignature Off
</p>
<p>
TraceEnable Off
</p>
<p>
Options all -Indexes
</p>
<p>
After saving the file, the custom configuration can be enabled in Apache like so:
</p>
<p>
	
</p>
<p>
sudo a2enconf custom
</p>
<p>
Enabling the mod-security module for Apache continues to improve the security of the server, allowing for the filtering of HTTP requests and responses. The installation and enabling of this module is can be accomplished using these commands:
</p>
<p>
sudo apt-get install libapache2-mod-security2
</p>
<p>
sudo a2enmod security2
</p>
<p>
To use mod-security a configuration file must be loaded, to use the default config copy it using the following command:
</p>
<p>
sudo mv /etc/modsecurity/modsecurity.conf-recommended /etc/modsecurity/modsecurity.conf
</p>
<p>
Next, the configuration file must be modified to enable and enforce the filtering rules, like so:
</p>
<p>
 
</p>
<p>
sudo nano /etc/modsecurity/modsecurity.conf
</p>
<p>
Change “SecRuleEngine” to “On” as seen in Figure 2
</p>
<h4 style="text-align: center">Figure 2: Modified Mod-Security Config</h4>


<p>


<p id="gdcalert2" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image2.png). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert3">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>


<img src="images/image2.png" width="" alt="alt_text" title="image_tooltip">

</p>
<p>
Next Install the MariaDB server and the MySQL PHP plugin to allow for PHP accessing of the database
</p>
<p>
sudo apt-get install mariadb-server php-mysql
</p>
<p>
Once the install has finished, check the service status using the command below, it should state “running” as seen in Figure 3:
</p>
<p>
sudo systemctl status mariadb
</p>
<h4 style="text-align: center">Figure 3: Running MariaDB server</h4>


<p>


<p id="gdcalert3" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image3.png). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert4">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>


<img src="images/image3.png" width="" alt="alt_text" title="image_tooltip">

</p>
<p>
Run the secure installation wizard to setup passwords, removing anonymous users,  disabling remote root login, etc
</p>
<p>
sudo mysql_secure_installation
</p>
<h4 style="text-align: center">Figure 4: Completion of MySQL Secure Wizard</h4>


<p>


<p id="gdcalert4" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image4.png). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert5">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>


<img src="images/image4.png" width="" alt="alt_text" title="image_tooltip">

</p>
<p>
After this remote MySQL access was disabled by binding all connections to localhost preventing external connections to the database by doing the following:
</p>
<p>
sudo nano /etc/mysql/my.cnf
</p>
<p>
Add line:
</p>
<p>
bind-address = 127.0.0.1
</p>
<p>
Restart MariaDB to apply
</p>
<p>
sudo systemctl restart mysql.service
</p>
<p>
To make use of the LAMP stack, WordPress will also be installed on this server.
</p>
<p>
So next add a user and create a database in MariaDB for WordPress to use, this user has the username “dbadmin” and a password of “pass123”
</p>
<p>
sudo mysql
</p>
<p>
create database wordpress;
</p>
<p>
create user 'dbadmin'@'localhost' identified by 'pass123';
</p>
<p>
This user should have full privileges over the database, so use these commands to grant the required privilege and reload the user privileges:
</p>
<p>
grant all privileges to wordpress.* as ‘dbadmin’@’localhost’ identified by ‘pass123’;
</p>
<p>
flush privileges;
</p>
<p>
To install the latest version of WordPress use:
</p>
<p>
wget <a href="https://wordpress.org/latest.zip">https://wordpress.org/latest.zip</a>
</p>
<p>
unzip latest.zip
</p>
<p>
sudo cp -r wordpress /var/www/html
</p>
<p>
To point WordPress to the MariaDB database we must first copy and modify the sample config file like so:
</p>
<p>
 cd /var/www/html/wordpress
</p>
<p>
sudo cp wp-config-sample.php wp-config.php 
</p>
<p>
sudo nano wp-config.php
</p>
<p>
The details created earlier should be entered into this config file as seen in Figure 5
</p>
<h4 style="text-align: center">Figure 5: Modified WordPress Config</h4>


<p>


<p id="gdcalert5" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image5.png). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert6">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>


<img src="images/image5.png" width="" alt="alt_text" title="image_tooltip">

</p>
<p>
In order to get Apache to serve the WordPress pages as intended, the document root must be modified to the newly copied WordPress folder, open the default page config using:
</p>
<p>
sudo nano /etc/apache2/sites-enabled/000-default.conf
</p>
<p>
Change the DocumentRoot to  /var/www/html/wordpress
</p>
<p>
Finally, Uncomplicated Firewall is used to block traffic on ports that are not required for the functionality of the server. The commands used to enable and set the firewall rules are as follow:
</p>
<p>
sudo ufw enable
</p>
<p>
sudo ufw allow 22
</p>
<p>
sudo ufw allow 80
</p>
<p>
sudo ufw allow 443
</p>
<p>
Check the rules are correct using the following command:
</p>
<p>
sudo ufw status
</p>
<h4 style="text-align: center">Figure 6: Enabled Firewall rules</h4>


<p>


<p id="gdcalert6" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image6.png). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert7">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>


<img src="images/image6.png" width="" alt="alt_text" title="image_tooltip">

</p>
<p>
After this, the WordPress wizard will be accessible by navigating to the server’s IP address as seen below:
</p>
<h4 style="text-align: center">Figure 7: WordPress setup wizard</h4>


<p>


<p id="gdcalert7" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image7.png). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert8">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>


<img src="images/image7.png" width="" alt="alt_text" title="image_tooltip">

</p></li>

<li>Public Cloud Deployment
<p>
In this deployment, Google Cloud Platform is used as the public cloud. While using Google Cloud there are two methods of deploying a WordPress + LAMP software stack, The first being creating a blank VM and using the same setup as the local machine deployment, with the extra step of having to provision hardware for the Linux VM to run on, in testing I set up the VM manually using this command in the Google Cloud Shell:
</p>
<p>
gcloud compute instances create lamp-stack 
</p>
<p>
    --image-family=debian-10 
</p>
<p>
    --image-project=debian-cloud
</p>
<p>
    --machine-type=e2-micro
</p>
<p>
    --zone=us-central1-a
</p>
<h4 style="text-align: center">Figure 7: VM creation in Google Cloud Shell</h4>


<p>


<p id="gdcalert8" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image8.png). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert9">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>


<img src="images/image8.png" width="" alt="alt_text" title="image_tooltip">

</p>
<p>
To begin the setup, use SSH to remote into the virtual machine using the command:
</p>
<p>
gcloud compute ssh lamp-stack
</p>
<p>
There were no differences in the install processes during testing even across the different Linux distributions used.
</p>
<p>
Another method of deploying a WordPress + LAMP stack is making use of Google Cloud’s “Click to Deploy” images available on the marketplace
</p>
<p>
This method allows for a user of Google Cloud to extremely rapidly deploy a majority preconfigured WordPress including the same software stack as on the manual installation as seen below in the list of installed software in the image:
</p>
<h4>Figure 8: Click to Deploy Software list</h4>


<p>


<p id="gdcalert9" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image9.png). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert10">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>


<img src="images/image9.png" width="" alt="alt_text" title="image_tooltip">

</p>
<p>
However, after testing, other than the firewall settings, which are only accepting traffic on port 80 and 443, it seems that the rest of the security of the server is left at relatively out-the-box levels, looking at the apache modules
</p></li>

<li>Conclusion
<p>
To conclude, these methods are robust ways to setup and deploy a platform for a web application, with suitable security measures proposed in the process to make sure that the deployment is not only functional, but also safe. To make a comparison between the Public Cloud offerings and Local Deployments, If you do not have hardware to host the platform on, Public Cloud offerings such as Google Cloud Platform allow you to set up your own platform as you wish, or save time by using a preconfigured image, all without having to purchase hardware.  However if large amounts of traffic is expected, costs of hosting the public cloud can become very large.
</p></li>

<li>Evaluation
<p>
Overall this report covers a significant amount on the configuration of such web application platforms, however more research could have been done on the alternatives on how to fully make use of the public cloud for this kind of deployment.
</p>
<p>
As I continued to research it was also discovered that using the mod-security module for Apache allows for use of the “OWASP ModSecurity Core Rule Set'' which is a set of generic attack detection rules which mod-security can use to prevent more attacks, this rule set is based apon a largely contributed, open attack listing called the Open Web Application Security Project. This would prevent from even more advanced attacks
</p>
<h1>References</h1>

</li> 
<ol>
 
<li>Wikipedia Technical Information. (2021). Retrieved 30 September 2021, from <a href="https://en.wikipedia.org/wiki/Wikipedia:FAQ/Technical">https://en.wikipedia.org/wiki/Wikipedia:FAQ/Technical</a></li>
 
<li><em>Usage Statistics and Market Share of Apache, November 2021. W3techs.com. (2021). Retrieved 2 November 2021, from <a href="https://w3techs.com/technologies/details/ws-apache">https://w3techs.com/technologies/details/ws-apache</a>.</em></li>
 
<li><em>Karanjit, A. (2016). MEAN vs. LAMP Stack.</em></li>
 
<li>Emelianov, M. (2017). What makes LAMP stack important for your web projects. Open Source for You,</li> 
</ol></li> 
</ol>