# Module 3.11 Assignment
3.11 Continuous Deployment - Container

## Objective
The objective of this assignment is to set up a continuous integration and continuous deployment (CI/CD) pipeline for a Node.js application that is deployed on AWS using Docker containers.

## Step 1: Create a code repository in GitHub
<img width="462" alt="github repo" src="https://github.com/liaucg/module_3.11_assignment/assets/22501900/02e85296-eba4-4a42-a75a-e60a02a43755">

## Step 2: Clone the code repository into local machine
```
$ git clone git@github.com:liaucg/module_3.11_assignment.git
```
## Step 3: Create a Node.js application using the Express framework
Here is a simple "Hello World from Node.js" application
```js
'use strict';

const express = require('express');

// Constants
const PORT = 8080;
const HOST = '0.0.0.0';
const OS = require('os');
const ENV = 'DEV';


// App
const app = express();
app.get('/', (req, res) => {
  res.statusCode = 200;
  const msg = 'Hello World from Node.js!';
  res.send(msg);
});

app.listen(PORT, HOST);
console.log(`Running on http://${HOST}:${PORT}`);
```
## Step 4: Create a Dockerfile for the application that includes all necessary dependencies
This Dockerfile contains directives to copy the package*.json file and install dependencies with npm command. Here port **8080** is being exposed because the Nodejs application is listening on port **8080**.
```docker
FROM node:16-alpine

WORKDIR /my-app

COPY package*.json ./

RUN npm install

COPY . .

EXPOSE 8080

CMD ["node", "index.js"]
```
## Step 5: Create AWS ECR repository
Here a ECR repository is created to the house the pushed container images where later it can be sed by ECS service to get deployed on Amazon platform.
![image](https://github.com/liaucg/module_3.11_assignment/assets/22501900/db26051c-c649-4b83-bbfe-80e8d99e3015)
