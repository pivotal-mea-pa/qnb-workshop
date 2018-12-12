# Environment Setup

In this exercise, we will be setting up the environment and all the tools required.

It assumes you are running Linux or MacOS, although it is possible to configure on Windows too.

If you are in a Pivotal workshop and don't want to install and configure these tools in your laptop, ask for the VM which has all required tools already installed. However, you'll still have to provide credentials/accounts for git and Pivotal Web Services.

### 1. Java

You'll need Java 8.

### 2. git install

You'll need the git CLI in order to clone and use the code in this repository.

Follow the instructions [here](https://help.github.com/articles/set-up-git/#platform-mac) to install and configure the git CLI on your host.


### 3. Cloud Foundry

We will be using Fiserv's own installation of PCF. If you have your login credentials we will use them in a minute, if you need them we will get some.

### 4. Cloud Foundry CLI

The **Cloud Foundry** command line interface is an easy way to interact with instances of **Cloud Foundry**.

You can obtain the CLI for multiple OS [here](https://github.com/cloudfoundry/cli)

### 5. Cloning the repository

Clone the GIT repository to your local machine. On the command line issue the following command:
```
git clone https://github.com/vrusso-pivotal/fiserv-workshop.git
```

This command will copy the code in the repository to your local machine, creating a directory named `fiserv-workshop`. All actions will now be done inside this directory.

Once you have cloned the repository, it is important to build it to create the application artifacts.

The projects use [gradle](http://gradle.org) as the build tool with gradle wrapper. Thus, all it is required to build all the microservices is:
```
./gradlew assemble
```


### 6. Login to Cloud Foundry

Using your PCF credentials run the following command on the command line
```
cf login
```

When prompted enter your email (login) and password. There should be a known org and space to be used for this workshop, go ahead and select those when prompted.

### 7. Creating the services

Now that you hav logged into Cloud Foundry, we want to begin to install a few services that we will be using later. Use the marketplace command to see the services that you can install
```
cf marketplace
```

The first column contains the names of the services, we want `Service Registry`, `Circuit Breaker Dashboard`, and `MySQL`. The second column is the plan, generally speaking each service will have a free plan, we will be using that.

We have the mareketplace name of the service from the first column and the plan from the second column. Now we will give our services instances names, this is what our applications will use to connect to them. For the purpooses of this lab we have already picked out names for you, however you may use whatever names you'd like the future.
```
cf create-service p-mysql 100mb traderdb
cf create-service p-service-registry standard discovery-service
cf create-service p-circuit-breaker-dashboard standard circuit-breaker-dashboard
```

## Summary

At the end of this lab, you should have an environment setup to enable you to deploy applications to **Cloud Foundry**. You will also have the code required for the rest of the labs.
