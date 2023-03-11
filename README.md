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

![Poll Mockup](./launching-of-an-instance.png)

---

>Connecting into your EC2 instance
```
cd Downloads
 chmod 400 project1.pem
 ssh -i "project1.pem" ubuntu@ec2-54-160-136-189.compute-1.amazonaws.com
 ```

![Poll Mockup](./ssh-and-pemkey.png)

---
---

# INSTALLING APACHE AND UPDATING THE FIREWALL 

>Update a list of packages in package manager and to Run apache2 package installation use the command:
```
sudo apt update
sudo apt install apache2 
```
 file:///home/aoa-clothings/Documents/DEVOPS/PBL%20Project/install.png
---

>Checking the status of appache 2
```
sudo systemctl status apache2
```
![Poll Mockup](./status.png)

---

>Verification of Apache 2 installation

![Poll Mockup](./welcome.png)

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

![Poll Mockup](./sudo-mysql.png)

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

![Poll Mockup](./mysql-secure.png)

---
>Test to see if you’re able to login into the MySQL console by typing: 
```
$ sudo mysql -p 
```
![Poll Mockup](./mysql-p.png)

mysql> exit 

---
---

# INSTALLING PHP

>You have Apache installed to serve your content and MySQL installed to store and manage your data. PHP is the component of our setup that will process code to display dynamic content to the end user. In addition to the php package, you’ll need php-mysql, a PHP module that allows PHP to communicate with MySQL-based databases. You’ll also need libapache2-mod-php to enable Apache to handle PHP files. Core PHP packages will automatically be installed as dependencies. 
```
sudo apt install php libapache2-mod-php php-mysql
```

![Poll Mockup](./installing-php.png)

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

![poll mockup](./sudo-vi.png)

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

![poll mockup](./Screenshot%20from%202023-03-11%2011-40-52.png)

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

![poll mockup](./sudo-vm.png)

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

![poll mockup](./vm-var.png)

>When you are finished, save and close the file, refresh the page and you will see a page similar to this:

![poll mockup](./php.jpeg)

>After checking the relevant information about your PHP server through that page, it’s best to remove the file you created as it contains sensitive information about your PHP environment -and your Ubuntu server. You can use rm to do so: 
```
sudo rm /var/www/projectlamp/index.php
```
>You can always recreate this page if you need to access the information again later. # My-PBL-Project
