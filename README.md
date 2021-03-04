# How to Deploy MFA on Ubuntu 20.04 LTS
## 1. Install Apache, MySQL, and PHP 7.4
Update packages and install required softwares:

    sudo apt update
    sudo apt install apache2 mysql-server php libapache2-mod-php
    sudo apt install graphviz aspell ghostscript clamav php7.4-pspell php7.4-curl php7.4-gd php7.4-intl php7.4-mysql php7.4-xml php7.4-xmlrpc php7.4-ldap php7.4-zip php7.4-soap php7.4-mbstring
    sudo service apache2 restart
## 2. Install MFA
Run the following commands to install MFA and set permissions:

    cd /var/www/html/
    sudo git clone -b prod https://github.com/Grows-IT/moodle.git mfa
    sudo mkdir /var/mfadata
    sudo chown -R www-data /var/mfadata/
    sudo chmod -R 777 /var/mfadata/
    sudo chmod -R 0755 /var/www/html/mfa/
## 3. Setup MySQL Server
### 3.1 Setup security

     sudo mysql_secure_installation

> Create a password. Answer 'y' to all other questions.

### 3.2 Add Configuration
Open an editor by running the following command:

    sudo nano /etc/mysql/mysql.conf.d/mysqld.cnf
> Add the following lines at the end of the file.

    default_storage_engine = innodb
    innodb_file_per_table = 1

>Press ‘ctrl + x’ to save, answer ‘y’ and enter. Then restart MySQL

      sudo service mysql restart

### 3.3 Create mfa database
Login:

    sudo mysql -u root -p

> Enter the password you created in step 3.1

  Create database:
  

    CREATE DATABASE mfa DEFAULT CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;

Create user:

    create user 'mfa'@'localhost' IDENTIFIED BY '[password]';
>  Replace [password] with your own password
    
Grant user permissions then quit

    GRANT SELECT,INSERT,UPDATE,DELETE,CREATE,CREATE TEMPORARY TABLES,DROP,INDEX,ALTER ON mfa.* TO 'mfa'@'localhost';
    quit;
    
## 4. Setup MFA
Execute the setup script:

    cd /var/www/html/mfa/admin/cli
    sudo chmod -R 777 /var/www/html/mfa
    sudo -u www-data /usr/bin/php install.php
    
> User default values by pressing ‘Enter’, except for the following questions:

== Web address ==
`http://[server ip]/mfa`
> Replace [server ip] with the external server IP

== Data directory ==
`/var/mfadata`

== Database name ==
`mfa`

== Database user ==
`mfa`

== Databse user password ==
[password]
> Replace [password] with the password created in step 3.3 when create user

== Full site name ==
`Modern Farm Academy`

== Short name for site (eg single word) ==
`MFA`

> Please create your own admin password when prompt.

**Note: Once you answered all the questions, the setup will run for about 10 minutes.** 
When done, visit **http://[server ip]/mfa** to see if the installation is successful.

## 5. Enable mobile web service
1. Visit **http://[server ip]/mfa** 
2. Login with admin user
3. On the left hand side menu, navigate to ‘Site Administration’ 
4. Scroll to the bottom and click ‘Mobile settings’
5. Check the box for ‘Enable web services for mobile devices’
6. Click save changes
