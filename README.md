# Deployment Instruction
This is a clean install instruction on Ubuntu 18 LTS.
## 1. Install Apache, MySQL, and PHP
```
sudo apt update
sudo apt install apache2 mysql-server php libapache2-mod-php
sudo apt install graphviz aspell ghostscript clamav php7.2-pspell php7.2-curl php7.2-gd php7.2-intl php7.2-mysql php7.2-xml php7.2-xmlrpc php7.2-ldap php7.2-zip php7.2-soap php7.2-mbstring
```
Estimated running time: 7 minutes.
## 2. Install Moodle
```
cd /var/www/html/
git clone https://github.com/waritsan/moodle.git moodle
cd moodle/
sudo git checkout prod
sudo chmod -R 0755 /var/www/html/moodle/
sudo mkdir /var/moodledata
sudo chown -R www-data /var/moodledata/
sudo chmod -R 777 /var/moodledata/
```
Estimated time taken: 5 minutes.
## 3. Setup MySQL Server
### 3.1 Setup DB root password
```
sudo mysql_secure_installation
```
Enter '0' for password strength. Create a password. Answer 'y' to all other questions.

```
sudo nano /etc/mysql/mysql.conf.d/mysqld.cnf
```
Add the following lines under [mysqld] section

```
default_storage_engine = innodb
innodb_file_per_table = 1
innodb_file_format = Barracuda
```
Save with ctrl + x and enter. Then restart MySQL

```
sudo service mysql restart
```
### 3.2 Create Database
```
sudo mysql -u root -p
```
Enter the password you created in 3.1

```
CREATE DATABASE moodle DEFAULT CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
create user 'moodle'@'localhost' IDENTIFIED BY '<YOUR_PASSWORD>';
GRANT SELECT,INSERT,UPDATE,DELETE,CREATE,CREATE TEMPORARY TABLES,DROP,INDEX,ALTER ON moodle.* TO moodle@localhost IDENTIFIED BY '<YOUR_PASSWORD>';
quit;
```
## 4. Setup Moodle
```
cd /var/www/html/moodle/admin/cli
sudo chmod -R 777 /var/www/html/moodle
sudo -u www-data /usr/bin/php install.php
```
Answer the questions below. Otherwise 'enter' the default values.
== Web address ==

```
http://<server ip>/moodle
```
== Data directory ==

```
/var/moodledata
```
== Database name ==

```
moodle
```
== Database user ==

```
moodle
```
== Databse user password ==

```
<YOUR_PASSWORD>
```
The setup will run for about 10 minutes.
When done revert the permission.

```
sudo chmod -R 0755 /var/www/html/moodle
```
---

Moodle - the world's open source learning platform

Moodle <https://moodle.org> is a learning platform designed to provide
educators, administrators and learners with a single robust, secure and
integrated system to create personalised learning environments.

You can download Moodle <https://download.moodle.org> and run it on your own
web server, ask one of our Moodle Partners <https://moodle.com/partners/> to
assist you, or have a MoodleCloud site <https://moodle.com/cloud/> set up for
you.

Moodle is widely used around the world by universities, schools, companies and
all manner of organisations and individuals.

Moodle is provided freely as open source software, under the GNU General Public
License <https://docs.moodle.org/dev/License>.

Moodle is written in PHP and JavaScript and uses an SQL database for storing
the data.

See <https://docs.moodle.org> for details of Moodle's many features.
