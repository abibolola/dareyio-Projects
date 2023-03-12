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