# WEB STACK IMPLEMENTATION (LEMP STACK)

This project is similar to the web stack in project 1. The only difference is the alternative web server - NGINX, which is also very popular and widely used by many websites in the Internet.

## _STEP 1 – INSTALLING THE NGINX WEB SERVER_
---
- Using these commands; NGINX - a high performance web server is installed
```
sudo apt update
sudo apt install nginx
```
- To verify that nginx was successfully installed and is running as a service in Ubuntu, run: ```sudo systemctl status nginx```

Before we can receive any traffic by our Web Server, we need to open TCP port 80 which is default port that web brousers use to access web pages in the Internet.

As we know, we have TCP port 22 open by default on our EC2 machine to access it via SSH, so we need to add a rule to EC2 configuration to open inbound connection through port 80

- To test how if the Nginx server can respond to requests from the Internet.
Open a web browser of your choice and try to access following url ```http://<Public-IP-Address>:80```

- The public IP of an EC2 instance can be retrieved using ```curl -s http://169.254.169.254/latest/meta-data/public-ipv4```

- The public IP on the browser shows the default page of the nginx web server

![NGINX default page](https://github.com/abibolola/dareyio-Projects/blob/main/Screenshots/Project2/nginx%20homepage%20port%2080.JPG)

## _STEP 2 — INSTALLING MYSQL_
---
-  MySQL is a popular relational database management system used within PHP environments, and it can be installed with: ```sudo apt install mysql-server```

- When the installation is finished, log in to the MySQL console by typing: ```sudo mysql```
![mysql console](https://github.com/abibolola/dareyio-Projects/blob/main/Screenshots/Project2/login%20to%20mysql%20console.JPG)

- To enable security measures on the mysql requires an inital secuirty configuration, which is ```sudo mysql_secure_installation```

## _STEP 3 – INSTALLING PHP_
---
- In order to install the php to serve dynamic web contents on the nginx server and depndence for php to work with mysql, use this: ```sudo apt install php-fpm php-mysql```
## _STEP 4 — CONFIGURING NGINX TO USE PHP PROCESSOR_
---
- Create a root web direectory for my projectLEMP, which will host the contents of the webiste ```sudo mkdir /var/www/projectLEMP```
- Assign ownership of the directory with the [$USER]() environment variable, which will reference your current system user: ```sudo chown -R $USER:$USER /var/www/projectLEMP```
- Create a new configuration file in Nginx’s sites-available directory
```
sudo nano /etc/nginx/sites-available/projectLEMP
```
- Then input the configuration on the file, to craete a sites for the projectLEMP.
- Activate your configuration by linking to the config file from Nginx’s sites-enabled directory:
```
sudo ln -s /etc/nginx/sites-available/projectLEMP /etc/nginx/sites-enabled/
```
- You can test your configuration for syntax errors by typing: ``` sudo nginx -t```
- We also need to disable default Nginx host that is currently configured to listen on port 80, for this run:
```
sudo unlink /etc/nginx/sites-enabled/default
```
- Nginx will use the configuration next time it is reloaded. Reload Nginx with: ```sudo systemctl reload nginx```
- The new website is now active, but the web root /var/www/projectLEMP is still empty. Create an index.html file in that location so that we can test that the new server block works as expected:
```
sudo echo 'Hello LEMP from hostname' $(curl -s http://169.254.169.254/latest/meta-data/public-hostname) 'with public IP' $(curl -s http://169.254.169.254/latest/meta-data/public-ipv4) > /var/www/projectLEMP/index.html
```
- Now open the website URL using IP address:

![new nginx webpage](https://github.com/abibolola/dareyio-Projects/blob/main/Screenshots/Project2/new%20nginx%20webpage.JPG)

## _STEP 5 – TESTING PHP WITH NGINX_
---

Create a test PHP file called info.php in the document root using nano editor
```
sudo nano /var/www/projectLEMP/info.php
```
- Type or paste the following lines into the new file
```
<?php
phpinfo();
```
- When you open domain_name or public_IP in your browser, you will see a web page containing detailed information about your server:

![php info](https://github.com/abibolola/dareyio-Projects/blob/main/Screenshots/Project2/php%20webpage.JPG)

## _STEP 6 – RETRIEVING DATA FROM MYSQL DATABASE WITH PHP_
---
- In this step you will create a test database (DB) with simple "To do list" and configure access to it, so the Nginx website would be able to query data from the DB and display it

![show database](https://github.com/abibolola/dareyio-Projects/blob/main/Screenshots/Project2/show%20table%20for%20example_user.JPG)

- Next, we’ll create a test table named todo_list. From the MySQL console, run the following statement:
```
CREATE TABLE example_database.todo_list (
mysql>     item_id INT AUTO_INCREMENT,
mysql>     content VARCHAR(255),
mysql>     PRIMARY KEY(item_id)
mysql> );
```
- Insert a few rows of content in the test table. This command is repeated 4 times:

```mysql> INSERT INTO example_database.todo_list (content) VALUES ("My first important item");```

- To confirm that the data was successfully saved to your table, run:

```mysql>  SELECT * FROM example_database.todo_list;```

![database rows](https://github.com/abibolola/dareyio-Projects/blob/main/Screenshots/Project2/insert%20more%20rows%20in%20the%20table.JPG)

- The PHP script has been connected to the MySQL database. Hence, when the web page is loaded on a browser in a path todo_list.php, it displays the database table:

![php database webpage](https://github.com/abibolola/dareyio-Projects/blob/main/Screenshots/Project2/php%20page%20querying%20mysql%20database.JPG)