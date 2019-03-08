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

* Install [Docker](https://docs.docker.com/install) 18.09.0 on your local computer in order to use the docker-machine commands.

* On Windows and Mac docker-machine and docker-compose is automatically installed along with Docker.
If you are using Linux, install [docker-machine](https://docs.docker.com/machine/install-machine) and [docker-compose](https://docs.docker.com/compose/install) on your local computer.

* Extract the *backend.zip*, which can be found inside the same folder as this setup guide, into a new directory named *pastub2019backend*.
Run all following commands inside the *pastub2019backend* directory.

* Create a Ubuntu 18.04-LTS Virtual Machine with the size *t3.medium* using docker-machine.
Insert your AWS Access Key ID at *yourAWSAccessKeyID* and your AWS Secret Access Key at *yourAWSSecretAccessKey*.
On Linux and Mac you maybe need to execute the commands with sudo privileges.
```sh
docker-machine create --driver amazonec2 --amazonec2-access-key yourAccessKeyHere --amazonec2-secret-key yourSecretKeyHere --amazonec2-ami ami-0bdf93799014acdc4 --amazonec2-instance-type t3.medium --amazonec2-open-port 8080 --amazonec2-region eu-central-1 pastub2019
```

* Set your docker-machine environment variables to the AWS VM on Linux and Mac.
```sh
eval $(docker-machine env pastub2019)
```
* on Windows.
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

**Hint**: If the cURL request is not working, manually insert the IP of the backend that you obtained from the docker-machine ip pastub2019 into the cURL request.
If it's still not working, try to send the request with Postman.

In the next section we guide you through the setup of the mobile app.

## Mobile App
Author: D. Bosin, A. Schürer

### Abstract definition
Install Java, Node.js, npm, React Native, Android Studio and SDK.
Set the IP of the backend.
Build and execute the android app on your android phone or android virtual device (AVD).

The following steps describe how to install the dependencies for building and executing the android app.
For more information on how to setup the Android development environment on your system, see [React Native Getting Started Guide](https://facebook.github.io/react-native/docs/getting-started.html).

* Install [Java](https://www.java.com/de/download/help/download_options.xml) 8u201.

* Install [Node.js](https://nodejs.org/en/download) 10.15.3 LTS and [npm](https://www.npmjs.com/get-npm) 6.8.0.

* Install React Native 0.58.4.
On Linux and Mac you maybe need to execute the commands with sudo privileges.
```sh
npm install -g react-native-cli
```

* Install [Android Studio](https://developer.android.com/studio/install).

* Install [Android SDK](https://developer.android.com/studio/intro/update) 8.1 (Oreo) with the Android Studio SDK Manager.

* Extract the *mobileapp.zip*, which can be found inside the same folder as this setup guide, into a new directory named *pastub2019mobileapp*.
Run all following commands inside the *pastub2019mobileapp* directory.

* Open the file *pastub2019mobileapp/src/lib/config.json* and add the IP you obtained from docker-machine ip pastub2019.
Don't add a prefix like *http* in front of the IP and don't add a suffix like a port to the IP.
```json
export default {
apiEndpoint: 'x.x.x.x',
}
```
**Hint**: If you don't add your own IP, you are using the backend of the PAS team.

* Install the dependencies.
Run this and all following commands inside the *pastub2019mobileapp* directory.
```sh
npm install
```

* Created an Android Virtual Device with Android 8.1 (Oreo) with the [AVD Manager](https://developer.android.com/studio/run/managing-avds).

* Set [environment variables](https://developer.android.com/studio/command-line/variables) on Linux and Mac in the file .bash\_profile
```sh
export ANDROID_HOME=$HOME/Android/Sdk
export PATH=$PATH:$ANDROID_HOME/emulator
export PATH=$PATH:$ANDROID_HOME/tools
export PATH=$PATH:$ANDROID_HOME/tools/bin
export PATH=$PATH:$ANDROID_HOME/platform-tools
```

* on Windows.
```sh
set ANDROID_SDK_ROOT=C:\Android\sdk\
```

* Run the Android Virtual Device via the AVD Manager.

* Start the app.
```sh
react-native run-android
```
**Hint**: If the application doesn't start on the virtual device, make sure that you set the environment variables correctly.

* When you start the app for the first time, you need to log in.
Enter *admin* as username and *adminiscooler* as password. Also make sure to allow the app to use the emulators location and set the emulators location to a location in Berlin. (e.g. 52.5140, 13.3350)

### Alternative to AVD
The following steps describe how to run the mobile app on a real Android Device

* Enable [developer mode](https://developer.android.com/studio/debug/dev-options) on your device.

* Connect your Android Device with your computer via USB and accept the alert which is popping up onto your device.

* Run the commands inside the *pastub2019mobileapp* directory to install the app on your device.

```sh
npm install
react-native run-android
```

The mobile app is now installed and you can start using it.