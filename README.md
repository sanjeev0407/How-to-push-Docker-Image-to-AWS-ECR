## How-to-push-Docker-Image-to-AWS-ECR
#Prerequisites
Ubuntu 16/18/20.04 LTS
SSH access with sudo privileges

## step -1: Creating Node.js Application
### Lets create the directory named nodejsdocker to add node js files to test.
<em> ~$ sudo su - 

~# cd /opt

~# mkdir nodejsdocker

~# cd nodejsdocker
</em> 
### Create the package.json file where you will specify all dependencies of your Node JS application
~# sudo nano package.json
### paste the below lines into it
<em>
{
  "name": "Docker_NodeJS_App",
  
  "version": "0.1",
  
  "description": "Node.js Application with Docker",
  
  "main": "server.js",
 
 "scripts": {
   
   "start": "node server.js"
  
  },
  
  "dependencies": {
    
    "express": "^4.17.1"
 
 }

}
</em>
### Next create the server.js page to test Node JS application with express framework

<em>~# sudo nano server.js </em>

### Paste the below lines in it
<em>
'use strict';

const express = require('express');

// Constants
const PORT = 3000;
const HOST = '0.0.0.0';


const app = express();
app.get('/', (req, res) => {
res.send('Testing Node JS Application');
});

app.listen(PORT, HOST);
console.log(`Running on http://${HOST}:${PORT}`);/<em>

### create a .dockerignore file and paste the below lines

<em>~# sudo nano .dockerignore<em>

<em>node_modules

npm-debug.log<em>

# Step-2: Install Docker and Create Docker Image
### Install below prerequisite packages

<em>~# sudo apt-get install apt-transport-https ca-certificates curl gnupg-agent software-properties-common<em>
#### Add below Docker’s official GPG key:

<em>~# curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -<em>
### Add the Docker APT repository to your system

<em>~# sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"<em>
### Update the Docker Repository Information

<em>~# sudo apt-get update<em>
### Install Docker on Ubuntu

<em>~# sudo apt-get install docker-ce<em>
### Once installed verify Docker Service status

~# sudo systemctl status docker
## Output:

<em> docker.service - Docker Application Container Engine

Loaded: loaded (/lib/systemd/system/docker.service; enabled; vendor preset: enabled)

Active: active (running) since Sun 2020-03-22 07:43:44 UTC; 32s ago

Docs: https://docs.docker.com

Main PID: 118265 (dockerd)

Tasks: 10

CGroup: /system.slice/docker.service

└─118265 /usr/bin/dockerd -H fd:// --containerd=/run/containerd/containerd.sock <em>
### Running Docker commands without sudo

* By default, Docker requires administrator privileges, Docker group is created when during the installation of Docker packages.

### Add your system user to docker group.

<em>~# sudo usermod -aG docker $USER<em>
### Change the docker.sock permission

<em>~# sudo chmod 666 /var/run/docker.sock<em>
### Next create the Dockerfile with below command in Project root directory

<em>~# sudo nano Dockerfile<em>

### Paste the below Dockerfile instructions in it

 <br>FROM node:12 </br>
 <br># To Create nodejsapp directory /<br>
 <br>WORKDIR /nodejsapp </br>
 <br># To Install All dependencies </br>
 <br>COPY package*.json ./ </br>
 <br>RUN npm install </br>
 <br># To copy all application packages </br>
 <br>COPY . . </br>
 <br># Expose port 3000 and Run the server.js file to start node js application </br>
 <br>EXPOSE 3000 </br>
 <br>CMD [ "node", "server.js" ]<em> </br>
 
 ### Now build the Docker Image using below command

<em>~# sudo docker build -t nodejsdocker .<em>

# Step-3: Install AWS CLI on Ubuntu

#### Download the aws cli bundle using below command

<em>~$ sudo curl https://s3.amazonaws.com/aws-cli/awscli-bundle.zip -o awscli-bundle.zip <em>

#### Install the unzip and python on Ubuntu if not installed

<em> ~# sudo apt install unzip python<em>

#### Extract the aws cli bundle setup

 <em>~# sudo unzip awscli-bundle.zip <em>

#### Configure the AWS CLI on Ubuntu

<em>~# sudo ./awscli-bundle/install -i /usr/local/aws -b /usr/local/bin/aws<em>

#### Verify the AWS CLI version

<em>aws --version<em>

### Output:

aws-cli/1.18.97 Python/2.7.18rc1 Linux/5.4.0-1015-aws botocore/1.17.20

#### Configure AWS CLI with your Access Key ID,  Secret Access  key and region

<em>~# aws configure<em>

# Step-4: Creating ECR Repository in AWS

1. Login to AWS console
2. Find the AWS Elastic Container Registry Service as shown below and Click on Elastic Container Registry
3. Click on Create repository
4. Enter the name of your ECR Name and click on Create repository.
eg: nodjsdocker
5. once created, you will see below message and click on View push commands.
6. you will see below push commands
7. follow the in that commands
Make sure that you have the latest version of the AWS CLI and Docker installed. For more information, see Getting Started with Amazon ECR .
8. Use the following steps to authenticate and push an image to your repository. For additional registry authentication methods, including the Amazon ECR credential helper, see Registry Authentication .
   <br>i) Retrieve an authentication token and authenticate your Docker client to your registry.</br>
 <br>Use the AWS CLI: </br>

9. <br><em>~$ aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin AccountID.dkr.ecr.us-east-1.amazonaws.com</em> </br>

 <br>*Note*: If you receive an error using the AWS CLI, make sure that you have the latest version of the AWS CLI and Docker installed.
Build your Docker image using the following command. For information on building a Docker file from scratch see the instructions here . You can skip this step if your image is already built:

10. <em>~$ docker build -t nodejsdocker .</em> <br>

 <br>After the build completes, tag your image so you can push the image to this repository: <br>

11.<br><em>~$ docker tag nodejsdocker:latest AccountID.dkr.ecr.us-east-1.amazonaws.com/nodejsdocker:latest</em> <br>

 <br>Run the following command to push this image to your newly created AWS repository: <br>

12. <br><em>~$ docker push AccountID.dkr.ecr.us-east-1.amazonaws.com/nodejsdocker:latest</em> <br>

# Step-5: How to push Docker-image to DockerHub

1. docker tag [imageID] [dockerhub username]/Imagename: version

it's created one image with [dockerhub username]/Imagename

2. docker push [dockerhub username]/Imagename

The image went to dockerhhub
