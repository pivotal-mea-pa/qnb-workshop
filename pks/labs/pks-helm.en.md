# Deploy an application using Kubernetes-Helm

## Goal
To deploy an application to PKS using Helm

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

  3. See all the created clusters by command:

          $ pks clusters

  4. Choose the cluster name where you want to deploy the application and add that cluster name to the below command.

          $ kubectl config use-context {cluster name}

  5. Check if the helm is installed using the command:

          $ which helm

  6. In case helm is not installed:

      MAC users can install it using : brew install kubernetes-helm

      From script:

            $ curl -LO https://git.io/get_helm.sh
            $ chmod 700 get_helm.sh
            $ ./get_helm.sh

  7. Create rbac-config.yml file to create the service account.

          $ kuberctl create -f rbac-config.yml

  8. Install tiller server on kubernetes cluster

  9. Initiate the tiller configuration into the cluster using command. This will install the tiller into the pods.

          $ helm init --service-account tiller

  10. To verify whether tiller configuration was successful run the below commands which will list the pods:

          $ kubectl -n kube-system get pods

  11. To create helm chart configuration:

          $ helm create geosearch

  This command will create a directory named geosearch which will have the helm chart configuration files.
  If you goto the directory there will be 2 files and 2 folders created.
  You can do some changes related to port of application in those files as required.
  For geosearch application, we will change the repository to point to the docker image that we created and update the tag with the required one.
  Because we are using containers, we can define the container port as well.
  We need the service of type load balancer. So update the service type to LoadBalancer.

  12. Now goto the deployment.yml and update the container port. You can hard code the value(not recommended) or can read it using parameters like  {{ .Values.containerPort }}


  13. To test if the changes made to the helm chart configuration were error free, you can dry run the helm installation using:

          $ helm install --dry-run --name ((name of the folder where helm chart was created)) .

  14. On successful dry run, you can install the helm using command:

          $ helm install --name geosearch .

  15. To check the created pods:

          $ kubectl get pods

  16. To check the created services:

      $ kubectl get services

Now using the external IP of the service that you deployed you can access the application.
