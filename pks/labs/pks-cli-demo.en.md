# Setting up the PKS CLI

## Goal

Install the PKS CLI and understand the options available to us via the CLI.

## Install the PKS CLI

1. Navigate to `https://network.pivotal.io/products/pivotal-container-service`
2. Select PKS CLI
3. Download the correct CLI version for your operating system.
4. Add the binary to your `PATH` using the following instructions depending on your Operating System.
  - Mac OS X, Linux
  
  Make the pks binary executable:
  ```
  $ chmod +x ~/Downloads/pks-darwin*
  ```
  
  Move the binary in to your `PATH`.
  ```
  $ sudo mv ~/Downloads/pks-darwin /usr/local/bin/pks
  ```

  - Windows
  
    - Rename the binary to `pks.exe`
	- Add the binary in to your `PATH`.
	  - Right click Computer, Select Properties
	  - Select Advanced Tab
	  - Select Environment Variables
	  - select `PATH`
	  - Select edit
	  - add `;<PATH-TO-PKS-BINARY>`
	  
## Show the PKS CLI options available

Use the CLI to show you the options available to us!

```
$ pks
```

This should return something similar to the following:

```
The Pivotal Container Service (PKS) CLI is used to create, manage, and delete Kubernetes clusters. To deploy workloads to a Kubernetes cluster created using the PKS CLI, use the Kubernetes CLI, kubectl.

Version: 1.2.1-build.22

Usage:
  pks [command]

Available Commands:
  cluster                View the details of the cluster
  clusters               Show all clusters created with PKS
  create-cluster         Creates a kubernetes cluster, requires cluster name, an external host name, and plan
  create-network-profile Create a network profile
  delete-cluster         Deletes a kubernetes cluster, requires cluster name
  delete-network-profile Delete a network profile
  get-credentials        Allows you to connect to a cluster and use kubectl
  help                   Help about any command
  login                  Log in to PKS
  logout                 Log out of PKS
  network-profiles       Show all network profiles created with PKS
  plans                  View the preconfigured plans available
  resize                 Increases the number of worker nodes for a cluster

Flags:
  -h, --help      help for pks
      --version   version for pks

Use "pks [command] --help" for more information about a command.
```