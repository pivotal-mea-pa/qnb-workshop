# Interacting With Kubernetes Clusters

## Goal

Connect to a Kubernetes cluster and view the Kubernetes Dashboard.

## Install the Kubernetes CLI - kubectl

1. In order to run containers (pods) on Kubernetes we need the Kubernetes CLI -- `kubectl`
  - Mac OS X
  
    Download the release with the command: 
    ```
    curl -LO https://storage.googleapis.com/kubernetes-release/release/`curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt`/bin/darwin/amd64/kubectl
    ```
    Make the kubectl binary executable 
    ```
    chmod +x ./kubectl
    ```
    Move the binary in to your PATH. 
    ```
    sudo mv ./kubectl /usr/local/bin/kubectl
    ```
  - Windows
  
    Download the release from this [link](https://storage.googleapis.com/kubernetes-release/release/v1.11.0/bin/windows/amd64/kubectl.exe)
    
    Or if you have `curl` installed, use this command: 
    ```
    curl -LO https://storage.googleapis.com/kubernetes-release/release/v1.11.0/bin/windows/amd64/kubectl.exe
    ```
	Add the binary to your PATH.
	- Right click Computer, Select Properties
	- Select Advanced Tab
	- Select Environment Variables
	- select `PATH`
	- Select edit
	- add `;<PATH-TO-KUBECTL-BINARY>`
  
  - Linux
    
    Download the release with the command: 
    ```
    curl -LO https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl
    ```
    Make the kubectl binary executable 
    ```
    chmod +x ./kubectl
    ```
    Move the binary in to your PATH. 
    ```
    sudo mv ./kubectl /usr/local/bin/kubectl
    ```

	  
## Login to Kubernetes as a developer 

  - Now that we have the CLI we must authenticate to our cluster. **Note: Only users with roles pks.clusters.admin or pks.clusters.manage can login to PKS**
    
    Using the PKS CLI, login to the PKS API by typing `pks login -a <PKS-API-HOSTNAME> -u <LDAP-USERNAME> -p <LDAP-PASSWORD> -k`
    - `PKS-API-HOSTNAME` is where the PKS server is deployed
    - `LDAP-USERNAME` is your AD userid
    - `LDAP-PASSWORD` is your regular AD password

  ```
  $ pks login -a api.pks.<domain-name> -u einstein -p password -k
    API Endpoint: api.pks.<domain-name>
    User: einstein
  ```
    
  - Then, list the kubernetes clusters deployed by PKS.

  ```
  $ pks clusters
  Name     Plan Name  UUID                                  Status     Action
  pks-wmt  small      91c80e8c-7a9c-4bb8-bfb4-f4a54e2a4f72  succeeded  CREATE
  ```

  - Once we know the kubernetes cluster we want to communicate with, we need to setup a local `~/.kube/config` for the `kubectl` CLI using:
  
  ```
  pks get-credentials <CLUSTER-NAME>
  ```
  
_Note: If OIDC/LDAP is enabled, you must use the following flow_

  - Download [this](https://github.com/Pivotal-Field-Engineering/pks-workshop/blob/master/get-pks-k8s-config.sh) script to authenticate as a user against PKS UAA (assuming you are on a Linux/Mac machine)
  - Execute the script as `./get-pks-k8s-config.sh --API=api.pks.<domain-name> --CLUSTER=<cluster-master-ip> --USER=<ldap-user>`
    - <cluster-master-ip> is the IP address or hostname of PKS Cluster deployed
    - <ldap-user> is the AD userid 

  ```
  ./get-pks-k8s-config.sh --API=api.pks.<domain-name> --CLUSTER=10.195.1.128 --USER=euler                                                                                                                                                                     ⏎
  Password:Cluster "10.195.1.128" set.
  Context "10.195.1.128" created.
  Switched to context "10.195.1.128".
  User "euler" set.
  ``` 

## Use Kubernetes as a Developer!

  1. Attempt to access resources in the `default` and `kube-system` namespaces. Notice we are allowed access to the `default` namespace (or any other namespace that the cluster admin gave the permissions for) but not the `kube-system` namespace as expected!
  
  ```
   $ kubectl get all --all-namespaces 
     Error from server (Forbidden): pods is forbidden: User "tesla" cannot list pods at the cluster scope
     Error from server (Forbidden): replicationcontrollers is forbidden: User "tesla" cannot list replicationcontrollers at the cluster scope
     Error from server (Forbidden): services is forbidden: User "tesla" cannot list services at the cluster scope
     Error from server (Forbidden): daemonsets.apps is forbidden: User "tesla" cannot list daemonsets.apps at the cluster scope
     Error from server (Forbidden): deployments.apps is forbidden: User "tesla" cannot list deployments.apps at the cluster scope
     Error from server (Forbidden): replicasets.apps is forbidden: User "tesla" cannot list replicasets.apps at the cluster scope
     Error from server (Forbidden): statefulsets.apps is forbidden: User "tesla" cannot list statefulsets.apps at the cluster scope
     Error from server (Forbidden): horizontalpodautoscalers.autoscaling is forbidden: User "tesla" cannot list horizontalpodautoscalers.autoscaling at the cluster scope
     Error from server (Forbidden): jobs.batch is forbidden: User "tesla" cannot list jobs.batch at the cluster scope
     Error from server (Forbidden): cronjobs.batch is forbidden: User "tesla" cannot list cronjobs.batch at the cluster scope

   $ kubectl get all -n default                                                                                                                                                                                                                                           ⏎
     NAME                 TYPE        CLUSTER-IP     EXTERNAL-IP   PORT(S)   AGE
     service/kubernetes   ClusterIP   10.100.200.1   <none>        443/TCP   1d
  ```
   
   
	
	