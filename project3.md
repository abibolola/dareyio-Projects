# SIMPLE TO-DO APPLICATION ON MERN WEB STACK

In this project, to implement a web solution based on MERN stack in AWS Cloud.

MERN Web stack consists of following components:

- MongoDB: A document-based, No-SQL database used to store application data in a form of documents.
- ExpressJS: A server side Web Application framework for Node.js.
- ReactJS: A frontend framework developed by Facebook. It is based on JavaScript, used to build User Interface (UI) components.
- Node.js: A JavaScript runtime environment. It is used to run JavaScript on a machine rather than in a browser.

## _STEP 1 – BACKEND CONFIGURATION_

- Lets get the location of Node.js software from Ubuntu repositories.

```curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -```

- Install Node.js with the command below ```sudo apt-get install -y nodejs```

- Create a new directory for your To-Do project and enter the dir: ``` mkdir Todo && cd Todo```

- Use the command ```npm init``` to initialise your project, so that a new file named package.json will be created.

![npm init](https://github.com/abibolola/dareyio-Projects/blob/main/Screenshots/Project3/npm%20init.JPG)

- Install Express using npm: ```npm install express```
- Install the dotenv module: ```npm install dotenv```
- Now create a file index.js with the command: ```touch index.js```
- Put a bare code in the index.js file and start the server with this command: ```node index.js```

![index.js on port 5000](https://github.com/abibolola/dareyio-Projects/blob/main/Screenshots/Project3/server%20running%20on%20port%205000.JPG)

### MODELS

Since the app is going to make use of Mongodb which is a NoSQL database, we need to create a model.
A model is at the heart of JavaScript based applications, and it is what makes it interactive.

To create a Schema and a model, install mongoose which is a Node.js package that makes working with mongodb easier: ```npm install mongoose```

### MONGODB DATABASE

- We need a database where we will store our data. For this we will make use of mLab. mLab provides MongoDB database as a service solution (DBaaS), so to make life easy, you will need to sign up for a shared clusters free account, which is ideal for our use case
- Create a file in your Todo directory and name it .env.
```
touch .env
vi .env
```
- Add the connection string to access the database in it, just as below:
```
DB = 'mongodb+srv://<username>:<password>@<network-address>/<dbname>?retryWrites=true&w=majority'
```

- Start our code using ```node index.js``` and test our backend code with RESTFul API using POSTMAN:

![test DB with postman](https://github.com/abibolola/dareyio-Projects/blob/main/Screenshots/Project3/postman_test_DB.JPG)

## _STEP 2 – FRONTEND CREATION_
---

- To start out with the frontend of the To-do app, we will use the create-react-app command to scaffold our app: ``` npx create-react-app client```

- This will create a new folder in your Todo directory called client, where you will add all the react code.
```
npm install concurrently --save-dev
npm install nodemon --save-dev
```

- When you are in the Todo directory run: ```npm run dev```

![npm run dev](https://github.com/abibolola/dareyio-Projects/blob/main/Screenshots/Project3/npm%20ru%20dev%20in%20todo.JPG)

- The frontend application using React.js that communicates with a backend application written using Expressjs.

![final webpage](https://github.com/abibolola/dareyio-Projects/blob/main/Screenshots/Project3/Final%20webPage.JPG)