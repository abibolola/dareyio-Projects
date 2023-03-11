# WEB STACK IMPLEMENTATION (LAMP STACK) IN AWS
Solution stacks are sets of individual components that create a complete environment for application development. The components are usually independently developed, but their frequent combined usage and compatibility qualify them to become a stack.

The LAMP stack is a popular open-source solution stack used primarily in web development (Linux, Apache, MySQL, PHP or Python, or Perl).
## _STEP 1 — INSTALLING APACHE AND UPDATING THE FIREWALL_
---
- Create an [AWS](aws.amazon.com) account and spin up an EC2 instance (Ubuntu Server 20.04 LTS (HVM) - t2.micro)

![LAMP EC2](https://github.com/abibolola/dareyio-Projects/blob/main/Screenshots/Project1/running%20ec2%20instance.JPG)

- Remote access into the linux server using SSH and the Public IP of the instance
- For my remote server login, I use VSCODE terminal

![EC2 remote access](https://github.com/abibolola/dareyio-Projects/blob/main/Screenshots/Project1/login%20to%20the%20ec2%20instance.JPG)

- Install Apache using Ubuntu’s package manager ‘apt’:
```
#update a list of packages in package manager
sudo apt update

#run apache2 package installation
sudo apt install apache2
```
- To verify that apache2 is running as a Service in our OS, use following command ``` sudo systemctl status apache2```
- To retrive the public-ip from the instance. ```curl -s http://169.254.169.254/latest/meta-data/public-ipv4```

## _STEP 2 — INSTALLING MYSQL AND PHP_
---
- Install MYSQL for database and PHP for backend development

- Again, use ‘apt’ to acquire and install MYSQL: ``` sudo apt install mysql-server```
- Run a security script that comes pre-installed with MySQL using: ``` sudo mysql_secure_installation```
-  In addition to the php package, you’ll need php-mysql, a PHP module that allows PHP to communicate with MySQL-based databases. You’ll also need [libapache2-mod-php]() to enable Apache to handle PHP files. Core PHP packages will automatically be installed as dependencies.
- To install these 3 packages at once, run: ``` sudo apt install php libapache2-mod-php php-mysql```
- Once the installation is finished, you can run the following command to confirm your PHP version: ```php -v```

## _STEP 3 — CREATING A VIRTUAL HOST FOR YOUR WEBSITE USING APACHE_
---
- Set up a domain called [projectlamp]()
- Create the directory for projectlamp using mkdir command as follows ```sudo mkdir /var/www/projectlamp```

- Assign ownership of the directory with your current system user ```sudo chown -R $USER:$USER /var/www/projectlamp```

- Create and open a new configuration file in Apache’s sites-available directory ```sudo vi /etc/apache2/sites-available/projectlamp.conf```
- Paste in the following bare-bones configuration by hitting on **i** on the keyboard to enter the insert mode, and paste the text
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

- To check the new file in the sites-available directory ```sudo ls /etc/apache2/sites-available```

- Something like this will come up;

```000-default.conf  default-ssl.conf  projectlamp.conf```

- Use a2ensite command to enable the new virtual host: ```sudo a2ensite projectlamp```

- To disable Apache’s default website use a2dissite command: ```sudo a2dissite 000-default```
- To make sure your configuration file doesn’t contain syntax errors ```sudo apache2ctl configtest```
 and reload Apache so these changes take effect: ```sudo systemctl reload apache2```
- Create an index.html file in that location so that we can test that the virtual host works
```
sudo echo 'Hello LAMP from hostname' $(curl -s http://169.254.169.254/latest/meta-data/public-hostname) 'with public IP' $(curl -s http://169.254.169.254/latest/meta-data/public-ipv4) > /var/www/projectlamp/index.html
```

- Open the website URL using IP address ```http://<Public-IP-Address>:80```  OR  ```http://<Public-DNS-Name>:80```

![Virtual host Web page](https://github.com/abibolola/dareyio-Projects/blob/main/Screenshots/Project1/virtual%20host%20web-page.JPG)

## _STEP 4 — ENABLE PHP ON THE WEBSITE_
----
