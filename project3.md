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