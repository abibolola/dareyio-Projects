# MEAN STACK DEPLOYMENT TO UBUNTU IN AWS

MEAN Stack is a combination of following components:

- MongoDB (Document database) – Stores and allows to retrieve data.
- Express (Back-end application framework) – Makes requests to Database for Reads and Writes.
- Angular (Front-end application framework) – Handles Client and Server Requests
- Node.js (JavaScript runtime environment) – Accepts requests and displays results to end user

## _Step 1: Install NodeJs_
---

- Node.js is a JavaScript runtime built on Chrome’s V8 JavaScript engine. Node.js is used to set up the Express routes and AngularJS controllers.
- Update and upgrade Ubuntu

```sudo apt update``` 

```sudo apt upgrade```

- Download the latest and stable version of the nodejs to avoid compatibility errors

```
sudo apt -y install curl dirmngr apt-transport-https lsb-release ca-certificates

curl -sL https://deb.nodesource.com/setup_16.x | sudo -E bash -
```
```sudo apt install -y nodejs```

![node -v](https://github.com/abibolola/dareyio-Projects/blob/main/Screenshots/project4/nodejs%20-v.JPG)

## _Step 2: Install MongoDB_
---
- MongoDB stores data in flexible, JSON-like documents. Fields in a database can vary from document to document and data structure can be changed over time

```
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 0C49F3730359A14518585931BC711F9BA15703C6
```

```
echo "deb [ arch=amd64 ] https://repo.mongodb.org/apt/ubuntu trusty/mongodb-org/3.4 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-3.4.list
```

- Installing the mongoDB using  ```sudo apt install -y mongodb```
- Starting the server  ```sudo service mongodb start``` and checking status  ```sudo systemctl status mongodb```

![mongodb status](https://github.com/abibolola/dareyio-Projects/blob/main/Screenshots/project4/mongodb-status.JPG)

- Install body-parser package

- We need ‘body-parser’ package to help us process JSON files passed in requests to the server.

```sudo npm install body-parser```

- Create a folder named ‘Books’

```mkdir Books && cd Books```

- In the Books directory, Initialize npm project   ```npm init```

## _STEP 3: INSTALL EXPRESS AND SET UP ROUTES TO THE SERVER_
---

- We also will use Mongoose package which provides a straight-forward, schema-based solution to model your application data. We will use Mongoose to establish a schema for the database to store data of our book register.

```sudo npm install express mongoose```

