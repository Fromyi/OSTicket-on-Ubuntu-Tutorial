# Installing OSTicket onto Ubuntu

## Objective

The osTicket installation project aimed to set up and configure an open-source ticketing system on an Ubuntu virtual machine. The primary focus was to deploy a functional help desk solution, ensuring proper installation, database configuration, and web server setup. This hands-on experience helped develop technical troubleshooting skills and a deeper understanding of IT support workflows.

### Skills Learned

- Installing and configuring web applications on Ubuntu.
- Setting up and managing LAMP stack (Linux, Apache, MySQL, PHP).
- Troubleshooting installation and configuration issues.
- Managing database operations and user permissions.
- Understanding help desk and ticketing system functionalities.

### Tools Used

- Ubuntu Virtual Machine (VM) as the operating system.
- osTicket for ticketing system deployment.
- Apache web server for hosting the osTicket platform.
- MySQL for database management and user authentication.
- PHP for processing backend functionality.
- phpMyAdmin for database administration and management.
- SSH for remote administration and troubleshooting.

## Steps
### Update system packages

Sudo apt install -y unzip curl git

<img width="476" alt="Screenshot 2025-02-26 at 11 30 01 AM" src="https://github.com/user-attachments/assets/ca5fc3f5-ca6b-4c13-b964-e332fd870af5" />

### Install apache2 web server
Sudo apt install -y apache2

sudo systemctl start apache2

Sudo systemctl enable apache2 

<img width="513" alt="Screenshot 2025-02-26 at 11 37 31 AM" src="https://github.com/user-attachments/assets/408f0e43-e8ad-4248-ba45-fe522caa0e28" />

### Install PHP & Required extensions & verify php installation
sudo apt install -y php libapache2-mod-php php-{mysql,imap,gd,xml,mbstring,intl,apcu,cli,zip,common,fpm,curl,bcmath} 

Php -v

<img width="921" alt="Screenshot 2025-02-26 at 11 43 04 AM" src="https://github.com/user-attachments/assets/932a9e31-d7f9-4933-9ab0-72bd0725c56e" />

<img width="427" alt="Screenshot 2025-02-26 at 11 43 39 AM" src="https://github.com/user-attachments/assets/94cd349f-bbf0-42af-a53e-906d5b47929f" />

### Install MariaDB (MySQL) databas server
sudo apt install -y mariadb-server

Sudo mysql_secure_installation

<img width="483" alt="Screenshot 2025-02-26 at 11 44 52 AM" src="https://github.com/user-attachments/assets/d829cae7-190e-465f-a618-994a8949a055" /><br>
<img width="505" alt="Screenshot 2025-02-26 at 11 46 17 AM" src="https://github.com/user-attachments/assets/808155c4-5819-4769-b7e1-698c100b6658" /><br>
<img width="525" alt="Screenshot 2025-02-26 at 11 46 59 AM" src="https://github.com/user-attachments/assets/2e26be42-e502-45bc-8c61-c79ec98cd7f9" /><br>
<img width="512" alt="Screenshot 2025-02-26 at 11 47 15 AM" src="https://github.com/user-attachments/assets/b1de9482-3a84-4638-9486-9dac8b8255e6" /><br>
<img width="496" alt="Screenshot 2025-02-26 at 11 47 23 AM" src="https://github.com/user-attachments/assets/9f9c0077-2142-4c95-84d5-f379db9e2bb8" /><br>
<img width="477" alt="Screenshot 2025-02-26 at 11 47 42 AM" src="https://github.com/user-attachments/assets/9d45ca07-0674-47ee-a8ce-353bee7e2c77" /><br>
<img width="495" alt="Screenshot 2025-02-26 at 11 47 54 AM" src="https://github.com/user-attachments/assets/d65b4351-2cad-4bad-ac9d-2b529f0ff3c1" />


### Log into mySQL as Root and Create database and user for OSTicket
sudo mysql -u root -p 

CREATE DATABASE osticket_db; 

CREATE USER 'osticket_user'@'localhost' IDENTIFIED BY ‘admin123’; 

GRANT ALL PRIVILEGES ON osticket_db.* TO 'osticket_user'@'localhost'; 

FLUSH PRIVILEGES; EXIT;

<img width="603" alt="Screenshot 2025-03-05 at 10 44 58 PM" src="https://github.com/user-attachments/assets/01ae020c-e198-49e4-9a94-2da8e43be135" />

### Download and Extract osTicket
cd /var/www/html

sudo wget https://github.com/osTicket/osTicket/releases/download/v1.18.2/osTicket-v1.18.2.zip (Check GitHub for latest release and replace that if outdated)

sudo unzip osTicket-v1.18.2.zip -d osticket

sudo chown -R www-data:www-data osticket

sudo chmod -R 755 osticket

<img width="381" alt="Screenshot 2025-03-05 at 9 47 07 PM" src="https://github.com/user-attachments/assets/4d6a3ebd-5a90-4b2d-acb6-a5250ce13250" /><br>
<img width="868" alt="Screenshot 2025-03-05 at 9 47 24 PM" src="https://github.com/user-attachments/assets/ef1a1644-e2dc-44c1-a1de-65ba6dfeb508" /><br>
<img width="547" alt="Screenshot 2025-03-05 at 9 47 40 PM" src="https://github.com/user-attachments/assets/423c73fb-32bf-46da-b27c-31e8dca48400" /><br>
<img width="531" alt="Screenshot 2025-03-05 at 9 44 03 PM" src="https://github.com/user-attachments/assets/d73b815d-20e2-40f0-9996-e955523851fb" /><br>
<img width="530" alt="Screenshot 2025-03-05 at 9 46 44 PM" src="https://github.com/user-attachments/assets/cf9b2af7-3e75-45c1-bcd5-085ae5414d36" />

### Navigate to the osTicket Directory & Rename the Sample Config File
cd /var/www/html/osticket/upload/include

#### *Check to see if out-sampleconfi.php is in this directory*
Ls -la

sudo cp ost-sampleconfig.php ost-config.php

sudo chmod 0666 ost-config.php

sudo chown www-data:www-data ost-config.php

<img width="705" alt="Screenshot 2025-03-05 at 10 28 39 PM" src="https://github.com/user-attachments/assets/f494c37e-555b-4c3f-be85-fdad6617edaf" />

#### *Once this is completed you can go to the next step of configuring apache2*

### Configuring Apache2 for osTicket

*Before anything find out your IPv4*

ip -a | grep net

<img width="651" alt="Screenshot 2025-03-05 at 9 51 59 PM" src="https://github.com/user-attachments/assets/e0a5fbf1-a13c-4d21-9d79-0c023a3a6781" />

### Create a new Virtual Host file
sudo nano /etc/apache2/sites-available/osticket.conf

Add Conf in textline: 
```apache
<VirtualHost *:80>  	
  ServerAdmin admin@example.com 
	DocumentRoot /var/www/html/osticket/upload 
	ServerName yourdomain.com 

 <Directory /var/www/html/osticket/upload> 
		Require all granted 
		AllowOverride All 
		Options FollowSymLinks MultiViews 
	</Directory> 

 ErrorLog ${APACHE_LOG_DIR}/error.log 
	CustomLog ${APACHE_LOG_DIR}/access.log combined
 </VirtualHost>
```
#### See below How I change my imformation from the code above. 

<img width="651" alt="Screenshot 2025-03-05 at 9 51 59 PM" src="https://github.com/user-attachments/assets/7ac10628-779a-40cf-abe8-32eb76fca944" /><br>
<img width="371" alt="Screenshot 2025-03-05 at 10 12 04 PM" src="https://github.com/user-attachments/assets/ac95cf1b-b89e-4994-8d34-b41589b4be0b" />

### Enable the new site and Apache rewrite module:
sudo a2ensite osticket.conf
sudo a2enmod rewrite
sudo systemctl restart apache2

<img width="460" alt="Screenshot 2025-03-05 at 10 21 29 PM" src="https://github.com/user-attachments/assets/bdd7bb54-690a-4f84-aa8b-ee5bea3a1a41" />

### Use information to setup Osticket

<img width="1203" alt="Screenshot 2025-03-05 at 10 21 45 PM" src="https://github.com/user-attachments/assets/fda24ca0-2e09-4d23-9b88-70d53ecf31a1" /><br>
<img width="570" alt="Screenshot 2025-03-05 at 10 29 50 PM" src="https://github.com/user-attachments/assets/3a2f3ba5-b3d2-4338-beb9-8d60e542de01" /><br>
<img width="827" alt="Screenshot 2025-03-05 at 10 31 04 PM" src="https://github.com/user-attachments/assets/31a93685-0a6f-4e19-8335-7939f736108e" /><br>
<img width="808" alt="Screenshot 2025-03-05 at 10 31 12 PM" src="https://github.com/user-attachments/assets/3f5b54dd-3947-48b3-b088-3b537e29dc3a" />

### Delete setup directory
sudo rm -rf /var/www/html/osticket/upload/setup

<img width="716" alt="Screenshot 2025-03-05 at 10 52 23 PM" src="https://github.com/user-attachments/assets/4893f401-b45e-4e21-8acd-af80c3083e5c" />

### Once this is completed you have successfully installed OSTicket on your VM
<img width="1359" alt="Screenshot 2025-03-05 at 10 49 28 PM" src="https://github.com/user-attachments/assets/25859666-dc36-460c-9e59-2965a9ecfadd" />




## *Final Notes:*
### I hope this tutorial on installing OSticket on VM helped you in your tech journey! 

## If you experience error message 500 try these steps below to help solve.

### *Run the following Apache2 access*

sudo chown -R www-data:www-data /var/www/html/osticket

sudo chmod -R 755 /var/www/html/osticket

sudo chmod 0666 /var/www/html/osticket/include/ost-config.php
