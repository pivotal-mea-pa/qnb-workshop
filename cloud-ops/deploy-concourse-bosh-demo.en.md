# BOSH

## Prerequisites

1. Permission to install two CLI commands
2. BASH shell


## Concourse Deployment

We're using BOSH to deploy concourse as a way to learn more about BOSH.


### Download local clients

The installation script uses two CLI tools--*bosh* client and the client for Concourse called *fly*.

* [bosh cli](https://bosh.io/docs/cli-v2-install/)
* [fly](https://www.concourse-ci.org/download.html)
* [*Additional Dependencies*](https://bosh.io/docs/cli-v2-install/#additional-dependencies)

Download and install these tools using the instructions according to your operation system.  BOSH is bootstraping and compiling BOSH using the *Additional Dependencies*.



### Download our Concourse Bundle

To make the installation easier in environment without Internet access, the deployment is configured as an offline bundle.

The bundle may be downloaded from <https://storage.googleapis.com/cbmasterspivotal/concourse-deployment.tgz>

or source
https://github.com/Pivotal-Field-Engineering/concourse-deployment

If downloading from github, you'll need to run *offline.sh* to download the files.


### Edit the *variables*

```
  cp variables.sample variables
```

Edit the *variables* file.


### Create a resource pool in vCenter

This configuration uses a vSphere resource pool.  Please create a resource pool called *concourse*.


### Install BOSH Director

This step creates the BOSH director.  

```
  ./install_bosh.sh
```

After compilation we can see the running virtual machine in our IaaS cloud console.


### Create the Cloud config

This step loads the cloud config.  The cloud config defines IaaS specific configuration like vm types, disk sizes, and network address range used by the Director during a deployments. It is seperate to keep IaaS specific configuration into its own file and keep deployment manifests IaaS independent.

```
  ./install_cloudconfig.sh
```

Answer yes to continue.


### Upload the Stemcell

This stemcell will be used for the concourse deployment.

```
  ./install_stemcell.sh
```


### Install Concourse

This step asks BOSH to deploy Concourse.  

```
  ./install_concourse.sh
```

Concourse will be running on the URL set in the *external_host* variable.  The username and password are *concourse*.



