# LAMP_RockyLinux

-------------------------------------------------------
Install Apache web server on Rocky Linux or AlmaLinux 8
-------------------------------------------------------

Perform a system update:
$ sudo dnf update -y

Install Apache HTTPd
$ sudo dnf install httpd

Start and enable HTTPd service
$ sudo systemctl start httpd
$ sudo systemctl enable httpd
$ systemctl status httpd

Configure Firewall Rules

Open port 80 or HTTP:
$ sudo firewall-cmd --permanent --zone=public --add-service=http

Open port 443 or HTTPS:
$ sudo firewall-cmd --permanent --zone=public --add-service=https

Reload firewall to make changes into effect:
sudo firewall-cmd --reload

Alternatively, it is ideal to set SELinux permissions globally for your Apache server.
sudo setsebool -P httpd_unified 1

The command will update SELinux Boolean values and the -P flag to update the boot-time value, making the change persistent with system reboots. 
Overall, the httpd_unified is the Boolean value that will instruct SELinux to treat all Apache (HTTPD) processes as the same type.

Find Server IP Address
curl -4 icanhazip.com

sudo dnf install curl -y
Alternatively, you could use the following command to find the server’s internal IP address.
hostname -I



Install PHP on Rocky Linux 8 | AlmaLinux 8
------------------------------------------
Check list php repos:
$ sudo dnf module list php

Reset default module for PHP: and Install:
$ sudo dnf -y module install php:7.4

Install basic PHP extensions:
sudo dnf -y install php php-fpm php-mysqlnd php-opcache php-gd php-xml php-mbstring php-zip

RHEL 8 based systems use php-fpm instead of mod_php. Start php-fpm service.
$ sudo systemctl enable --now php-fpm

Verify that php-fpm is running by checking on its status:
$ sudo systemctl status php-fpm

Restart httpd service:
$ sudo systemctl restart httpd

Create a test PHP file called info.php under the directory /var/www/html:
$ sudo vim /var/www/html/info.php

Paste content below into the file:
	<?php
	phpinfo();
	?>
	
	
---------------------------------	
Installing MariaDB on Rocky Linux
---------------------------------

Step 1:Install MariaDB package 
------------------------------

As a best practice, you should update your operating system before installing the package.
$ sudo dnf update

$ sudo dnf module list mariadb

****************************
To install desired version: this can be skipped
$ sudo vi /etc/yum.repos.d/mariadb.repo

Paste script below:
    [mariadb]
    name = MariaDB
    baseurl = http://yum.mariadb.org/10.6/rhel8-amd64
    module_hotfixes=1
    gpgkey=https://yum.mariadb.org/RPM-GPG-KEY-MariaDB
    gpgcheck=1 
	
$ sudo dnf update
**************************

$ sudo dnf install mariadb-server mariadb
$ sudo systemctl start mariadb
$ sudo systemctl enable mariadb
$ sudo systemctl status mariadb

Step 2: Securing MariaDB Server
-------------------------------
$ sudo mysql_secure_installation
Cleaning up...
[root@localhost ~]# mysql_secure_installation

NOTE: RUNNING ALL PARTS OF THIS SCRIPT IS RECOMMENDED FOR ALL MariaDB
      SERVERS IN PRODUCTION USE!  PLEASE READ EACH STEP CAREFULLY!

In order to log into MariaDB to secure it, we'll need the current
password for the root user.  If you've just installed MariaDB, and
you haven't set the root password yet, the password will be blank,
so you should just press enter here.

Enter current password for root (enter for none): ****** step1 press enter
OK, successfully used password, moving on...

Setting the root password ensures that nobody can log into the MariaDB
root user without the proper authorisation.

Set root password? [Y/n] y ****** step2 press Y
New password:
Re-enter new password:
Password updated successfully!
Reloading privilege tables..
 ... Success!


By default, a MariaDB installation has an anonymous user, allowing anyone
to log into MariaDB without having to have a user account created for
them.  This is intended only for testing, and to make the installation
go a bit smoother.  You should remove them before moving into a
production environment.

Remove anonymous users? [Y/n] y ****** step3 press Y
 ... Success!

Normally, root should only be allowed to connect from 'localhost'.  This
ensures that someone cannot guess at the root password from the network.

Disallow root login remotely? [Y/n] y ****** step3 press Y
 ... Success!

By default, MariaDB comes with a database named 'test' that anyone can
access.  This is also intended only for testing, and should be removed
before moving into a production environment.

Remove test database and access to it? [Y/n] y ****** step4 press Y
 - Dropping test database...
 ... Success!
 - Removing privileges on test database...
 ... Success!

Reloading the privilege tables will ensure that all changes made so far
will take effect immediately.

Reload privilege tables now? [Y/n] y ****** step5 press Y
 ... Success!

Cleaning up...

All done!  If you've completed all of the above steps, your MariaDB
installation should now be secure.

Thanks for using MariaDB!
[root@localhost ~]#
