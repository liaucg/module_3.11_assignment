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

## Step 6: Add AWS access key ID and AWS secret access key to GitHub secrets
In order to access AWS ECR registry the AWS access key ID and AWS secret access key will be added to GitHub repository secrets.

![image](https://github.com/liaucg/module_3.11_assignment/assets/22501900/94969256-0d18-4573-b55d-23dd8277c32c)

## Step 7: Create a GitHub Actions workflow file
This GitHub workflow file contains the instructions that the workflow will execute. There are 4 steps to be executed:

1. Create a Ubuntu remote *environment/Runner* where the workflow can run and build the image.
2. Login to the remote machine via SSH using the pre-written workflow by Official GitHub Actions, i.e. **checkout@v3**. It simply check out our GitHub repository for [Dockerfile](Dockerfile) to build the docker image.
3. Configure AWS credentials with the secrets created in Step 6.
4. Login to AWS ECR
5. Build the docker image by using the [Dockerfile](Dockerfile), tag the image with a version and push it to AWS ECR repository created in Step 5. The commands to do these tasks are written in the **run** which will be executed in bash shell of remote machine.