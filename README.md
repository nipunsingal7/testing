# Shell-Script
</br>

## Table of Contents
- [Description](#Description)
- [Features](#Features)
- [Code Explanation](#Code-Explanation)
- [Usage](#Usage)
</br></br>

## Description
We are setting up the shell script for installation of drupal/wordpress, php,apache2 and mysql. We will be using packers and LAMP stack for the smooth and easy installation. 
</br></br>
## Features
- Fast installation
- Version variables
- Choice to install drupal/wordpress
- LAMP server and packers for installation of packages
</br></br>

## Code Explanation

We have to set the debconf frontend to non-interactive as it is recommended. Debconf is a configuration system for debian packages that provides interface for packages configuration. Dialog is the default frontend for apt package manager in debian

```bash
echo 'debconf debconf/frontend select Noninteractive' | sudo debconf-set-selections
sudo apt-get install -y -q
```
</br>
We used sleep statement to hold the further execution of the script as apt-get update command sometimes takes more time than usual to completely execute.

```bash
sleep 30
```
</br>
We are installing tasksel, as it reduces the problem of specifying versions for php, apache and mysql.

```bash
sudo apt-get install tasksel -y
```
</br>
We are using the lamp-server to install all the required packages, i.e mysql,php and apache. In the second command we installed the extensions needed for setting up drupal.

```bash
sudo tasksel install lamp-server
sudo apt install php-dom php-gd php-xml -y
```
</br>
We used the if statement, which is taking the input environment variable as a decision parameter to whether install drupal or wordpress. In this statement it will install drupal and if control goes to else statement then wordpress will install.

```bash
if [ $input == "drupal" ]
```
</br>
We are using nested if else, This if statement is to check whether mysql/database is already configured in the instance or not, if mysql is already installed then it will not execute the database creation commands othewise it will. These commands are for drupal, wordpress commands are also given in the script.

```bash
if [ -f /etc/init.d/mysql* ]
        then
                echo "Already installed"
        else
               
                sudo mysql -u root -padmin -e "CREATE DATABASE drupal CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci;"
                sudo mysql -u root -padmin -e "GRANT SELECT, INSERT, UPDATE, DELETE, CREATE, DROP, INDEX, ALTER, CREATE TEMPORARY TABLES ON drupal.* TO ‘drupaluser’@’localhost’ IDENTIFIED BY 'root';"
                
        fi 
```
</br>
This command is creating a database(line1) and a user granting all the permission it will have to access the database (line2). This is for drupal database creation, wordpress database creation also given in the script.

```bash
 sudo mysql -u root -padmin -e "CREATE DATABASE drupal CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci;"
                sudo mysql -u root -padmin -e "GRANT SELECT, INSERT, UPDATE, DELETE, CREATE, DROP, INDEX, ALTER, CREATE TEMPORARY TABLES ON drupal.* TO ‘drupaluser’@’localhost’ IDENTIFIED BY 'root';"
```
</br>
In this command we are specifying the drupal version using the variable D9. Then we are downloading tar file of that particular version of drupal and untar it.
 
```bash
D9="drupal-9.1.6"
wget https://ftp.drupal.org/files/projects/$D9.tar.gz
tar -zxf $D9.tar.gz
```
</br>
Note- The below explanation is for drupal, same is for the wordpress.
 
Creating a Root directory for drupal

```bash
mkdir /var/www/html/drupal
```
</br>
Moving Drupal files to localhost

```bash
cp -R drupal-8.6.2/* drupal-8.6.2/.htaccess /var/www/html/drupal
```
</br>
Creating directory for Drupal

```bash
mkdir /var/www/html/drupal/sites/default/files
```
</br>
Changing permissions of the new directories, So that Drupal can access them

```bash
sudo chown www-data:www-data /var/www/html/drupal/sites/default/files
```
</br>
Creating a Drupal configuration file

```bash
sudo cp /var/www/html/drupal/sites/default/default.settings.php /var/www/html/drupal/sites/default/settings.php
```
</br>
Changing access authorization of the Drupal configuration file

```bash
sudo chown www-data:www-data /var/www/html/drupal/sites/default/settings.php
```
</br>
Restarting the Apache service

```bash
service apache2 restart
```
</br>
Enabling Clean URLs on Apache: To avoid warnings when installing Drupal, it is preferable to enable these before.
	
```bash
sudo sed -i '172s/.*/        AllowOverride All/' /etc/apache2/apache2.conf
```
</br></br>

## Usage

```bash
./shellscript [drupal/wordpress] [version]
```
