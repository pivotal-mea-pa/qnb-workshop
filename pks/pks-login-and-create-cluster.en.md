# Manage Kubernetes Clusters

## Goal

Deploy a Kubernetes cluster, and setup our host to connect to it.

## Prerequisites

  - Deployed Pivotal Ops Manager (2.0+)
  - Deployed Pivotal Director Tile
  - Post-deploy scripts enabled
  - Deployed Pivotal Container Service Tile
  - Installed PKS CLI
  
## Determine PKS Management API IP

In order to reach the PKS Management API service we must ensure either our DNS or Load Balancer is sending us to the correct PKS Management API url. 
We cannot utilize IPs due to the default security enforcement built into the PKS Management API. 
**Skip this step if you deployed a load balancer and/or registered the PKS Management API hostname with your DNS.**

1. Navigate to the Ops Manager FQDN (hostname).
1. Login with the Ops Manager credentials setup earlier.
1. Click on Pivotal Container Service Tile.
1. Click on Status.
1. Note the IP for the Pivotal Container Service job down.

## Login to PKS CLI

In order to create a Kubernetes Cluster with PKS we utilize the PKS CLI. Since PKS is backed by UAA for authorization, we use a preconfigured `admin` username and password stored with PKS UAA to log into PKS.
While external identity resources are supported for backing UAA this demo is focused on using local users stored in UAA.

  1. Navigate to the Ops Manager FQDN (hostname).
  1. Login with the Ops Manager credentials setup earlier.
  1. Click on Pivotal Container Service Tile.
  1. Click on Credentials.
  1. Find the **Uaa Admin Password** entry and click on Link to Credential.
  1. Note down the secret.
  1. Using the PKS CLI, login to the PKS API by typing `pks login -a <PKS-API-HOSTNAME> -u <UAA-USERNAME> -p <UAA-USER-PASSWORD> -k`
    - `PKS-API-HOSTNAME` comes from Step `Determine PKS Management API IP` above
    - `UAA-USERNAME` is `admin`
    - `UAA-USER-PASSWORD` is the secret looked up previously

  ```
  $ pks login -a api.pks.<domain-name> -u admin -p <admin_secret> -k
  API Endpoint: api.pks.home.<domain-name>
  User: admin
  ```
  
## Create a Kubernetes Cluster

  1. Lookup the PKS plans configured in the PKS Tile using `pks plans`
 
  ```
  $ pks plans
  Name    ID                                    Description
  small   8A0E21A8-8072-4D80-B365-D1F502085560  Example: This plan will configure a lightweight kubernetes cluster. Not recommended for production workloads.
  medium  58375a45-17f7-4291-acf1-455bfdc8e371  Example: This plan will configure a medium sized kubernetes cluster, suitable for more pods.
  ```
  1. Provision a Kubernetes Cluster by typing `pks create-cluster <CLUSTER-NAME> --external-hostname <cluster-name.domain-name> --plan <plan-name>`
    - The `CLUSTER-NAME` can be replaced with any name you choose
    - Note: The `--external-hostname` flag determines which hostname PKS will add to the certificate to authenticate to the Kubernetes cluster with. The hostname you place here will have to be resolved and load-balanced to the Kubernetes Master IP.

  ```
  $ pks create-cluster pks-wmt -e pks-wmt.pks.<domain-name> -p small
  Name:                     pks-wmt
  Plan Name:                small
  UUID:                     91c80e8c-7a9c-4bb8-bfb4-f4a54e2a4f72
  Last Action:              CREATE
  Last Action State:        in progress
  Last Action Description:  Creating cluster
  Kubernetes Master Host:   pks-wmt.pks.<domain-name>
  Kubernetes Master Port:   8443
  Worker Nodes:             3
  Kubernetes Master IP(s):  In Progress
  Network Profile Name:     
  
  Use 'pks cluster pks-wmt' to monitor the state of your cluster
  ```
  1. Watch the status of our cluster. It can take up to 10 minutes to create the cluster.
 
  ```
  $ pks cluster pks-wmt
  Name:                     pks-wmt
  Plan Name:                small
  UUID:                     91c80e8c-7a9c-4bb8-bfb4-f4a54e2a4f72
  Last Action:              CREATE
  Last Action State:        succeeded
  Last Action Description:  Instance provisioning completed
  Kubernetes Master Host:   pks-wmt.pks.<domain-name>
  Kubernetes Master Port:   8443
  Worker Nodes:             3
  Kubernetes Master IP(s):  192.168.0.231
  Network Profile Name:  
  ```
  1. Once cluster if finished creating, you need to create a DNS entry for the Master IP shown in the status output to point to the `external-hostname` you indicated while creating the cluster
  1. Finally, you can issue `pks get-credentials <CLUSTER-NAME>`, which creates an entry in your local `~/.kube/config` file to be able to interact with Kubernetes cluster
    - Enter the `UAA-USER-PASSWORD` that we looked up previously when prompted for Password.

  ```
  $ pks get-credentials pks-wmt
  Fetching credentials for cluster pks-wmt.
  Password: ******
  Context set for cluster pks-wmt.
  
  You can now switch between clusters by using:
  $kubectl config use-context <cluster-name>
  ```
  1. Confirm `kubectl` is working by invoking a command such as `kubectl get nodes`
 
  ```
  $ kubectl get nodes -o wide                                                           
  NAME                                   STATUS   ROLES    AGE   VERSION   INTERNAL-IP     EXTERNAL-IP     OS-IMAGE             KERNEL-VERSION      CONTAINER-RUNTIME
  087b6975-43aa-4100-8fb3-5aa029a1461f   Ready    <none>   4d    v1.11.5   192.168.0.232   192.168.0.232   Ubuntu 16.04.5 LTS   4.15.0-39-generic   docker://17.12.1-ce
  1a1e955c-38bc-4773-95ae-c6546a179c7d   Ready    <none>   4d    v1.11.5   192.168.0.234   192.168.0.234   Ubuntu 16.04.5 LTS   4.15.0-39-generic   docker://17.12.1-ce
  6e39f1fa-0bdf-42df-95db-6e6fd90a80b3   Ready    <none>   4d    v1.11.5   192.168.0.233   192.168.0.233   Ubuntu 16.04.5 LTS   4.15.0-39-generic   docker://17.12.1-ce
  ```
  
  
  
  
  