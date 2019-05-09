# Deploy an application to PKS

## Goal
To deploy an application to PKS

## Prerequisites
    + Deployed Pivotal Ops Manager
    + Deployed Pivotal Director tile
    + Deployed Pivotal Container Service tile
    + Docker image of the application to be deployed.
    + Installed kubectl
    + Installed PKS cli
    + Created cluster

## Deploying the application

  1. Login to pks using

          $ pks login -k -a {pks URL} -u ssinghal

  2. CLI will prompt for the password. Enter the password.

  3. To see all the created clusters by command:

          $ pks clusters

  4. Choose the cluster name where you want to deploy the application and add that cluster name to the below command.

          $ kubectl config use-context {cluster name}

          $ kubectl config use-context test-cluster1

  5. Create pod.yml and service.yml for your application and then use it for deployment.

          $ kubectl create -f pod.yml -f service.yml

  6. To check the created pods:

          $ kubectl get pods

  7. To check the created services:

          $ kubectl get services

Now using the external IP of the service that you deployed you can access the application.
