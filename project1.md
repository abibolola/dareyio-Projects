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