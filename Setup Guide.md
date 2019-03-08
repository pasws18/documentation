# Setup of Mobile App and Backend
This document guides you through the setup of our mobile app and the corresponding backend.

## Software Artifacts
This sections gives an overview of all software artifacts that are needed to setup our mobile app and the corresponding backend.
Each software artifact can be found inside the same folder as this setup guide.

### backend.zip
The backend.zip contains all software artifacts needed to deploy the backend for our mobile app.

### mobileapp.zip
The mobileapp.zip contains all software artifacts needed to build and execute our mobile app.

## Backend
Author:  A. Schürer

### Abstract definition
Create a Ubuntu Linux Virtual Machine on AWS EC2, install Docker and open port 8080 for TCP connections.

The backend is empowered by an Node.js Express.js web server in combination with a mongoDB.
Both the web server and the database are shipped in their own container.
The containers are based on [Docker](http://www.docker.com) and the deployment is managed by [Docker Compose](https://docs.docker.com/compose).
You can run these containers anywhere with docker installed.
We recommend you to use [Ubuntu](http://www.ubuntu.com) 18.04-LTS, Docker 18.09.0 and host the backend on [AWS EC2](https://aws.amazon.com/ec2) with *t3.medium* size.
The VM has to be accessible over TCP on port 8080.
The following steps describe how to create an Ubuntu VM with Docker using [docker-machine](https://docs.docker.com/machine).

* [Install Docker](https://docs.docker.com/install) 18.09.0 on your local computer in order to use the docker-machine commands.

* On Windows and Mac docker-machine and docker-compose is automatically installed along with Docker.
If you are using Linux, [install docker-machine](https://docs.docker.com/machine/install-machine) and [install docker-compose](https://docs.docker.com/compose/install) on your local computer.

* Extract the *backend.zip*, which can be found inside the same folder as this setup guide, into a new directory named *pastub2019backend*.
Run all following commands inside the *pastub2019backend* directory.

* Create a Ubuntu 18.04-LTS Virtual Machine with the size *t3.medium* using docker-machine.
Insert your AWS Access Key ID at *yourAWSAccessKeyID* and your AWS Secret Access Key at *yourAWSSecretAccessKey*.
On Linux and Mac you maybe need to execute the commands with sudo privileges.
```sh
docker-machine create --driver amazonec2 --amazonec2-access-key yourAccessKeyHere --amazonec2-secret-key yourSecretKeyHere --amazonec2-ami ami-0bdf93799014acdc4 --amazonec2-instance-type t3.medium --amazonec2-open-port 8080 --amazonec2-region eu-central-1 pastub2019
```

* Set your docker-machine environment variables to the AWS VM for Linux and Mac.
```sh
eval $(docker-machine env pastub2019)
```
* for Windows.
```sh
@FOR /f "tokens=*" %i IN ('docker-machine env pastub2019') DO @%i
```

* Start the application.
Run this and all following commands inside the *pastub2019backend* directory.
```sh
docker-compose build
docker-compose up -d
```

* Obtain the public IP of your AWS EC2 VM.
```sh
docker-machine ip pastub2019
```

* Send a request to your backend hosted on your AWS EC2 VM to check if the backend is running.
```sh
curl -X GET $(docker-machine ip pastub2019):8080
```
The backend is working when the cURL request returns a greeting message

**Hint**: If the cURL request is not working, manually insert the IP of the backend that you obtained from the docker-machine ip into the cURL request.
If it's still not working, try to send the request with Postman.

In the next section we guide you through the setup of the mobile app.