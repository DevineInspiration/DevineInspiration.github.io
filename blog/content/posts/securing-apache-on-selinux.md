---
layout: post
title: Securing an Apache Server with SELinux
date: 2021-07-28 10:38:00 +0000

---

In this report, the importance of the security of a webserver is discussed on modern deployments, as part of this, implementations demonstrating ways of securing a Apache webserver hosted on CentOS are shown with suitable explanation

*Keywords—  Apache, SELinux, Web Hosting, Webserver, Server Security, CentOS; *



## 1. Introduction
Apache is one of the most popular web servers currently being used across the internet, with 32.2% of all outward-facing web servers being hosted using Apache [[1](#reference-1)]. As this server is so popular there is a large pressure to find new security exploits which may be used maliciously. 

Making sure there are appropriate security measures is important when hosting websites, especially sites that will be handling user/company data, by tightening the security of the webserver you are reducing the risk of data breaches, where data is either stolen, modified, or deleted. The server could also be hijacked, and be used for other malicious purposes, for example modifying the pages served by the server to contain viruses or log user inputs. All these outcomes could end up being a breach of GDPR regulations which could then cause monetary losses for the entity responsible for hosting the server if legal action is taken. 

Some industry standard ways of adding security to a server running Apache may include;

Limiting the server’s functionality to the minimum feature set, if the sole purpose of the server is to serve HTML files, then Apache’s extra functionality - for example, support for PHP, ASP.NET and other technologies are not required, disabling these will reduce the number of potential vulnerabilities that could be exploited in these unused technologies.

It is also good practice to start the Apache server as an unprivileged user, this will allow the server administrator to have more control over the scope of access that the server has from the start, reducing the permissions to the minimum necessary 

Disabling support for Server-side Includes (SSIs) if server side includes are not required for the functionality of the server, SSIs are used to allow web applications to have a way of running local scripts and commands on the server, however, if this functionality is manipulated and proper safeguarding has not been put in place it could allow attackers to make modifications to the server. SSIs also cause a high load on the server which could compromise the performance of the server. Very often these days the functionality brought about by SSIs can be emulated or closely replicated with other web technologies such as PHP, which should be more secure and less intensive on the server.

Chroot - chroot can be used to change the apparent root directory of a process, changing the root directory from the default user root can help stop attackers from modifying files unrelated to the Apache server, for example, system files for the Linux operating system Apache is running on. a process which has been chrooted cannot modify files or directories outside of its new environment, this can be described as a form of sandboxing and this isolated environment the Apache server runs in is called a jail, a chroot jail.

SELinux - Security-Enhanced Linux - Linux by default uses discretionary access control methods such as limited file permission metadata or access control lists, using this method users and programs are allowed to give access to other users/applications to files that they otherwise wouldn’t have, SELinux works using exception policies, where all actions are denied unless access is specifically permitted, whether that be an application requesting access to a file, or a user wanting to enter a directory, all must be specifically allowed through the exceptions list provided through SELinux’s configuration [[2](#heading=h.30j0zll)].



## 2. Server Configuration
In this report, I will be investigating potential server configurations with security concerns taken into consideration with 2 different virtual hosts, one with root in /var/www/html, and ./public_html. The first configuration discussed is virtual host in the default root location for Apache defined in SELinux[[3](#heading=h.1fob9te)].

First step when setting up an Apache Server is starting the httpd service, this is the name for apache server daemon[[4](#heading=h.3znysh7)], which will allow the server able to handle HTTP requests

*systemctl start httpd.service*

*systemctl enable httpd.service*

Next is to add an exception into the firewalld (dynamic firewall found in CentOS[[5](#heading=h.tyjcwt)]) to allow http traffic to reach the server from outside the local network using this command:


    firewall-cmd --permenant --zone=public --add-service=http

If required, https traffic can also be allowed, using this command:


    firewall-cmd --permenant --zone=public --add-service=https

Then the firewall must be restarted to allow the new excepted traffic through the firewall

    firewall-cmd --reload
    
     \


Next is configuring the Apache server configuration files, these can be accessed in a text editor with the filepath:

    /etc/httpd/conf/httpd.conf

	

As we will be using a system directory for this virtual host, we should disable access to users directories, this will help protect any users personal information

    UserDir disable

To prevent Apache from running in a users personal directory, a dedicated user should be used for running the server, this will protect the user’s personal files and allows better control over the permissions of the apache server

Use these options to set the user details that Apache runs as:


    user apache

    group apache

	

To prevent targeted attacks on outdated or buggy versions of Apache we can hide the version from the footer that is served along side and documents that the server sends out on request, this can be done using the commands:


    ServerSignature Off 

    ServerTokens Prod 

The Apache server should not be the owner of its configuration files. This can be changed with the commands below, this will help prevent attackers from modifying the servers config maliciously if access to the “apache” user is gained.


    chown -R root:apache /etc/httpd

    chmod -R u=rwx,g=r,o-rwx /etc/httpd

To make sure SSIs cannot be exploited if they are not being used, make sure that the following line cannot be found in the config file:

    Options +Includes

Disable directory listing to help prevent attackers from discovering every file used in the web application, this could be used for dumping the web application with the intent of reverse engineering, disabling this can be accomplished by adding:

    Options -Indexes

For the second virtual host, the root will be within a users directory, in the folder “public_html”

To check if SELinux allows Apache to read from a users directory use the command:

    getsebool httpd_read_user_content

If this returns 0, access to user files is disabled, to enable use:

	setsebool -P httpd_read_user_content 1

To make sure that SELinux allows Apache access to the public html folder, the correct label should be set for that directory, this can be done using the command[[6](#heading=h.2et92p0)]: 


    semanage fcontext -a -t httpd_sys_content_t “./public_html(/.*)?”

This new exception in SELinux can applied using the command: 

    restorecon -R -v ./public_html

Apache server configuration files, can be accessed in a text editor with the filepath:

    /etc/httpd/conf/httpd.conf

As Apache needs access to a users directory, allow it access by commenting out the following line:

    #UserDir disable

To tell Apache specifically what directory it has access to within the users directory use:

    UserDir public_html

To prevent Apache from running in a users personal directory, a dedicated user should be used for running the server, this will protect the user’s personal files and allows better control over the permissions of the apache server


    user apache

    group apache

	

To prevent potential attackers from scanning the webserver for its specific apache version in order to target unpatched version, this command can be used to mask the version 


    ServerSignature Off 

    ServerTokens Prod 

To prevent Apache from accessing system files, disallow access to system root using the following directive:


    <Directory />
    Require all denied
    </Directory>

To tell Apache which directory it is to access for user files and allow access, add the following directive to the config:


    <Directory “./public_html>
    	AllowOverride All 
		Require all granted
    </Directory> 



## 3. Conclusion
To conclude, the configuration discussed, has plenty of safeguard to make sure that both Apache’s built-in settings and SELinux are used to tighten the security of the webserver, A large number of the security risks discussed in the introduction are handled by the implementations, making this a much more secure server for use hosting a public web page or application. The implementations are successful in their goal of securing against some of the security concerns discussed earlier making this report a positive success.


## References

1. W3techs.com. 2021. Usage Statistics and Market Share of Apache, July 2021. [online] Available at: [https://w3techs.com/technologies/details/ws-apache](https://w3techs.com/technologies/details/ws-apache)

2. Wiki.centos.org. 2021. HowTos/SELinux - CentOS Wiki. [online] Available at: &lt;[https://wiki.centos.org/HowTos/SELinux](https://wiki.centos.org/HowTos/SELinux)>

3. Wiki.centos.org. 2021. HowTos/SELinux - CentOS Wiki. [online] Available at: &lt;[https://wiki.centos.org/HowTos/SELinux#Relabeling_Files](https://wiki.centos.org/HowTos/SELinux%23Relabeling_Files)> 

4. Httpd.apache.org. 2021. httpd - Apache Hypertext Transfer Protocol Server - Apache HTTP Server Version 2.4. [online] Available at: &lt;[https://httpd.apache.org/docs/2.4/programs/httpd.html](https://httpd.apache.org/docs/2.4/programs/httpd.html)  >

5. DigitalOcean. 2021. How To Set Up a Firewall Using FirewallD on CentOS 7 | DigitalOcean. [online] Available at: &lt;[https://www.digitalocean.com/community/tutorials/how-to-set-up-a-firewall-using-firewalld-on-centos-7](https://www.digitalocean.com/community/tutorials/how-to-set-up-a-firewall-using-firewalld-on-centos-7)> 

6. Lists.fedoraproject.org. 2021. How to set SELinux to allow apache-httpd only to access a particular user's content. [online] Available at: &lt;[https://lists.fedoraproject.org/pipermail/users/2011-April/395403.html](https://lists.fedoraproject.org/pipermail/users/2011-April/395403.html)>