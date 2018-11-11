# PKS Tile Install

## Goal
Configure and deploy the PKS Tile within PCF Ops Manager.

## Prerequisites

- Deployed Pivotal Ops Manager (2.0+) 
- Deployed Pivotal Director Tile
    - Post-deploy scripts enabled
- VMware vCenter 
    - Admin Username/Password
    - Datacenter Name
    - Cluster Name
    - Datastore Name

## Download PKS Tile

1. Navigate to [Pivotal Network](https://network.pivotal.io/)

1. Register and login.

1. Download the [Pivotal Container Service (PKS) Tile](https://network.pivotal.io/products/pivotal-container-service)

1. Accept the EULA

## Upload PKS Tile to Pivotal Ops Manager

1. Navigate to the Pivotal Ops Manager FQDN (hostname). 

1. Login.

1. Select `Import a Product`.

1. Browse to the product downloaded earlier.

1. Wait while the product is uploaded, this may take time depending on your connection to the Pivotal Ops Manager. *When completed notice the new product in the side panel of Pivotal Ops Manager.*

## Configure the PKS Tile

1. Click the green `+` on the product tile.

1. Click the `Pivotal Container Service` tile. PKS deploys kubernetes clusters on demand, where those clusters will live, and what features are enabled in those clusters will be defined in this tile.

1. Configure the `Assign AZs and Networks` page.
    - Select an `AZ` for both the singleton jobs, and all other jobs to run in.

    - Select `PKS-Management` for `network` if having followed earlier demos. This network is where the PKS Management API itself will live.

    - Select `PKS-Clusters` for `service network` if having followed earlier demos. This network is where PKS will provision Kubernetes Clusters on demand.

    - Click Save.

1. Configure the `PKS API` page.
    
    - Either generate a self signed certificate for our PKS API endpoint or copy/paste in the certificate PEM and private key PEM. This certificate will be used when connecting to the PKS management API endpoint. We will use this endpoint for deploying PKS clusters.
        - The certificate must be valid for `*.pks.<INSERT DOMAIN HERE>`. 
    
    - Click Save.

1. Configure the Plan 1 - Plan 3 pages. These plans will define the type of Kubernetes clusters available for on demand deployment.
    
    For each plan: 
    
    - Select `active` or `inactive`. At least one is required to be `active`.
    
    - Fill in the `Name` and `Description` fields for each `active` plan.

    - Select the AZ that the cluster will be deployed to.

    - Select `RBAC` for Authorization Mode.

    - Adjust the `ETCD/Master VM Type` depending on what you require for the plan. 
        - This will apply to **ALL** kubernetes clusters deployed with PKS and this plan

    - Adjust the `Master Disk Type` depending on what you require for the plan. 
        - This will apply to **ALL** kubernetes clusters deployed with PKS and this plan

    - Adjust the `Worker VM Type` depending on what you require for the plan. 
        - This will apply to **ALL** kubernetes clusters deployed with PKS and this plan

    - Adjust the `Worker Persistent Disk Type` depending on what you require for the plan. 
        - This will apply to **ALL** kubernetes clusters deployed with PKS and this plan

    - Adjust the `Worker Node Instances` depending on what you require for the plan. 
        - This can be changed on the fly when deploying kubernetes clusters with the PKS CLI

    - Use the `Add-Ons` box to define what a cluster includes out of the box. This will add any additional deployments to the deployed Kubernetes cluster in the system namespace. For example, this could be a pod that gathers metrics or does logging for your cluster. This should be in a `yaml` format. You can specify multiple files using `---` as a separator.

    - If you want users to be able to create pods with privileged containers, select the `Enable Privileged Containers - Use with caution` option.
    
    - Click Save

1. Configure the `Kubernetes Cloud Provider` page. This section will configure where the deployed Kubernetes clusters will store any persistent disks.
    
    - Choose `vSphere`.

    - Fill in the corresponding values:
        - `vCenter Credentials`
        - `vCenter Host`
        - `Datacenter Name`
        - `Datastore Name`
    
    - For the `VM Folder` we must tell PKS where our Kubernetes Clusters are currently deployed. To retrieve the name of the folder, navigate to your `Pivotal Ops Manager Director` tile, click vCenter Config, and locate the value for VM Folder. The default folder name is `pcf_vms`.

    - Click Save.

1. Configure the `Networking` page. This section will configure how routing in Kubernetes is performed.
    
    - Select `Flannel`.

    - Click Save.

1. Configure the `UAA` page. 

    - Enter `api.pks.<INSERT DOMAIN HERE>` for the UAA URL. The UAA (User Authentication & Authorization) service runs on the same VM that the PKS API will run on. In order to authenticate with it we must give it a valid hostname. *UAA will run on port 8443*

    - Click Save.

1. Configure the `Errands` page. Errands are scripts that run at designated points during an installation.

    - Enable the `Upgrade all clusters errand`. 
    
        - Because PKS uses floating stemcells, updating the PKS tile with a new stemcell triggers the rolling of every VM in all Kubernetes clusters. Also, updating other product tiles in your deployment with a new stemcell causes the PKS tile to roll VMs. This rolling is enabled by the Upgrade all clusters errand. Pivotal recommends that you keep this errand turned on because automatic rolling of VMs ensures that all deployed cluster VMs are patched. However, automatic rolling can cause downtime in your deployment.



## Complete the PKS Installation

1. Click the Installation Dashboard link to return to the Installation Dashboard.
    - *Notice the tile has changed form orange to green signifying it it ready to be applied!*

1. Click Apply Changes on the right navigation.
