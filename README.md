<details>
<summary>Table of Contents:</summary>

0. [Launching an instance in AWS](#launching-an-instance-in-aws)
0. [Installing Apache and Udating the Firewall](#installing-apache-and-updating-the-firewall)
0. [Installing MYSQL](#installing-mysql)
0. [Installing PHP](#installing-php)
0. [Creating a Virtual host for your Website Using Apache](#creating-a-virtual-host-for-your-website-using-apache)
0. [Enable PHP on the Website](#enable-php-on-the-website)
</details>


# LAUNCHING AN INSTANCE IN AWS
---
>EC2 instance

![launching-of-an-instance](https://user-images.githubusercontent.com/125014455/224656215-40be0f65-c932-4834-af1d-96824ff4ca4f.jpg)

---

>Connecting into your EC2 instance
```
cd Downloads
 chmod 400 project1.pem
 ssh -i "project1.pem" ubuntu@ec2-54-160-136-189.compute-1.amazonaws.com
 ```

![ssh-and-pemkey](https://user-images.githubusercontent.com/125014455/224656307-3f4daf90-dc86-42ce-9dd5-937034cf4412.jpg)

---
---

# INSTALLING APACHE AND UPDATING THE FIREWALL 

>Update a list of packages in package manager and to Run apache2 package installation use the command:
```
sudo apt update
sudo apt install apache2 
```

![install](https://user-images.githubusercontent.com/125014455/224656596-8c301875-6d5d-40bc-aed3-d65b740e74b0.jpg)
 

---

>Checking the status of appache 2
```
sudo systemctl status apache2
```

![status](https://user-images.githubusercontent.com/125014455/224656775-13d2b1e9-1429-40b4-b71a-a0d23bdcd747.jpg)

---

>Verification of Apache 2 installation

![welcome](https://user-images.githubusercontent.com/125014455/224656826-5f14e135-0a54-4f23-90d6-28ca212280c8.jpg)

---
---

# INSTALLING MYSQL

```
sudo apt install mysql-server
```

>Log into the MySQL console by typing:

```
sudo mysql
```

![sudo-mysql](https://user-images.githubusercontent.com/125014455/224656989-2893cb5d-0ab1-4296-9ba5-65b277595636.jpg)

---

>set your password with:
```
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'Password of your choice';
```
>Exit the MYSQ shell with "exit" then click the enter button

--- 

>start the interactive script by running:
```
sudo mysql_secure_installation 
```

![mysql-install](https://user-images.githubusercontent.com/125014455/224657118-ca49efcf-bd48-459b-9aca-c6e5b325bbfc.jpg)

---
>Test to see if you’re able to login into the MySQL console by typing: 
```
$ sudo mysql -p 
```

![mysql-p](https://user-images.githubusercontent.com/125014455/224657245-4fab3852-5c74-434f-bb17-55333b2039ab.jpg)

mysql> exit 

---
---

# INSTALLING PHP

>You have Apache installed to serve your content and MySQL installed to store and manage your data. PHP is the component of our setup that will process code to display dynamic content to the end user. In addition to the php package, you’ll need php-mysql, a PHP module that allows PHP to communicate with MySQL-based databases. You’ll also need libapache2-mod-php to enable Apache to handle PHP files. Core PHP packages will automatically be installed as dependencies. 
```
sudo apt install php libapache2-mod-php php-mysql
```

![installing-php](https://user-images.githubusercontent.com/125014455/224657373-f5eaf530-6ff4-4d94-ba54-6f8c0b42a3f8.jpg)

---

# CREATING A VIRTUAL HOST FOR YOUR WEBSITE USING APACHE

>In this project, we will set up a domain called **projectlamp**

>Create the directory for projectlamp using ‘mkdir’ command as follows: 
```
sudo mkdir /var/www/projectlamp
sudo chown -R $USER:$USER /var/www/projectlamp
```

>create and open a new configuration file in Apache’s sites-available directory using your preferred command-line editor. 
```
sudo vi /etc/apache2/sites-available/projectlamp.conf
```

![sudo-vi](https://user-images.githubusercontent.com/125014455/224657541-59275db0-0103-4a4c-a403-c73f2b354469.jpg)


>This will create a new blank file. Paste in the following bare-bones configuration by hitting on i on the keyboard to enter the insert mode, and paste the text:
```
<VirtualHost *:80>
ServerName projectlamp
ServerAlias www.projectlamp
ServerAdmin webmaster@localhost
DocumentRoot /var/www/projectlamp
ErrorLog ${APACHE_LOG_DIR}/error.log
CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```
>To save and close the file, simple hit esc on your keyboard and type **:wq** : for command w for write q for quit

![Screenshot from 2023-03-11 11-40-52](https://user-images.githubusercontent.com/125014455/224511912-64b059cd-ddfd-47dc-8314-295f07e126e1.png)

>You can use the ls command to show the new file in the sites-available directory 
```
sudo ls /etc/apache2/sites-available
```

>You will see something like this; 
```
000-default.conf default-ssl.conf  projectlamp.conf
```

>You can now use a2ensite command to enable the new virtual host:
```
sudo a2ensite projectlamp 
```

>To disable the default website that comes installed with Apache,
to make sure your configuration file doesn’t contain syntax errors
and to reload Apache so these changes take effect use:
```
sudo a2dissite 000-default
sudo apache2ctl configtest
sudo systemctl reload apache2
```

>Your new website is now active, but the web root /var/www/projectlamp is empty.

>Now go to your browser and try to open your website URL using IP address:
```
http://<Public-IP-Address>:80
```
---
---

# ENABLE PHP ON THE WEBSITE
>With the default DirectoryIndex settings on Apache, a file named index.html will always take precedence over an index.php file. This is useful for setting up maintenance pages in PHP applications, by creating a temporary index.html file containing an informative message to visitors. Because this page will take precedence over the index.php page, it will then become the landing page for the application. Once maintenance is over, the index.html is renamed or removed from the document root, bringing back the regular application page.

>In case you want to change this behavior, you’ll need to edit the /etc/apache2/mods-enabled/dir.conf file and change the order in which the index.php file is listed within the DirectoryIndex directive:
```
sudo vim /etc/apache2/mods-enabled/dir.conf
```

>Copy and paste the below:
```
<IfModule mod_dir.c>
#Change this:
#DirectoryIndex index.html index.cgi index.pl index.php index.xhtml index.htm
#To this:
DirectoryIndex index.php index.html index.cgi index.pl index.xhtml index.htm
</IfModule>
```

![sudo-vm](https://user-images.githubusercontent.com/125014455/224511938-d525ddfd-7b2b-4937-8faa-d8f1ce7f8722.png)


>After saving and closing the file, you will need to reload Apache so the changes take effect: 
```
sudo systemctl reload apache2
```

>Finally, we will create a PHP script to test that PHP is correctly installed and configured on your server. 

>Now that you have a custom location to host your website’s files and folders, we’ll create a PHP test script to confirm that Apache is able to handle and process requests for PHP files. 

>Create a new file named index.php inside your custom web root folder: 
```
vim /var/www/projectlamp/index.php
```

>Copy and paste the below:
```
<?php
phpinfo();
```

![vm-var](https://user-images.githubusercontent.com/125014455/224658045-9084a117-9712-4bc6-b9a9-d1b814d74a91.jpg)

 
>When you are finished, save and close the file, refresh the page and you will see a page similar to this:

![php](https://user-images.githubusercontent.com/125014455/224511977-5570caaf-2914-4bd2-a9c4-bf8387a8c4f2.jpeg)


>After checking the relevant information about your PHP server through that page, it’s best to remove the file you created as it contains sensitive information about your PHP environment -and your Ubuntu server. You can use rm to do so: 
```
sudo rm /var/www/projectlamp/index.php
```
>You can always recreate this page if you need to access the information again later. # My-PBL-Project
