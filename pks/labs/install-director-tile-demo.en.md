# PCF Director

## Goal

Configure and deploy PCF Director Tile within PCF Ops Manager. This will be our base to build PKS on.

## Prerequisites

- VMware vCenter
  - Admin Username/Password
  - Datacenter Name
  - Cluster Name
  - Datastore Name
  - Network Name
- IP Range of (around) 30 IPs converted to CIDR to assign to PKS. [CIDR Calculator](http://www.subnet-calculator.com/cidr.php)
  - Gateway, DNS, and Netmask corresponding to this range.
- NTP Server
- Deployed Pivotal Ops Manager


## Enable PCF Director and (vSphere) Cloud Communication

1. Navigate to the Ops Manager FQDN (hostname).

2. Login with the Ops Manager credentials setup earlier.

3. Click on Director Tile (vSphere). PCF deploys and manages disks, networks, and vms. In order to do this it needs access to our IaaS. This is where we will configure how it manages the IaaS.

4. On the  `vCenter Config` page fill in the following information:

  - **vCenter Host**: The hostname of the vCenter that manages the ESXi/vSphere cluster.
  - **vCenter Username**: A vCenter username with create and delete privileges for virtual machines (VMs), disks, and folders.
  - **vCenter Password:** The password for the vCenter user specified above.
  - **Datacenter Name:** The name of the datacenter as it appears in vCenter.
  - **Virtual Disk Type:** The Virtual Disk Type to provision for all VMs.
  - **Ephemeral Datastore Names (comma delimited):** The names of the datastores that store ephemeral VM disks deployed by Ops Manager.
  - **Persistent Datastore Names (comma delimited):** The names of the datastores that store persistent VM disks deployed by Ops Manager.
  - **Networking Type:** We will use `Standard vCenter Networking` for this guide. NSX can also be used.
  - **VM Folder:** The vSphere datacenter folder (default: `pcf_vms`) where Ops Manager places VMs.
  - **Template Folder:** The vSphere datacenter folder (default: `pcf_templates`) where Ops Manager places VM templates.
  - Note: After your initial deployment, you will not be able to edit the VM Folder, Template Folder, and Disk path Folder names.

5. On the `Director Config` page fill in the following:

  - **NTP Servers:** A valid NTP server(s) to keep time for the PCF Director.
  - **Ensure Enable Post Deploy Scripts** is checked.

6. On the `Create Availability Zones` page we create multiple Availability Zones which allow us to provide high-availability and load balancing to our containers deployed to PKS. At least three availability zones are recommended for a highly available installation of PKS.

To add **Availability Zones** run the following:

  - Click Add.
  - Enter a unique Name for the Availability Zone.
  - Enter the name of an existing vCenter Cluster to use as an Availability Zone.
(Optional) Enter the name of a Resource Pool in the vCenter cluster that you specified above. The jobs running in this Availability Zone share the CPU and memory resources defined by the pool.

7. On the `Create Networks` page we create virtual networks which allows the PCF Director to deploy VMs in specific networks. We will create 2 networks, one for the management of PKS, and one for the kubernetes clusters themselves.

To add **Networks** run the following:

  - Select Enable ICMP checks to enable ICMP on your networks. Ops Manager uses ICMP checks to confirm that components within your network are reachable.
  - PKS deploys Kubernetes Clusters on demand via a CLI command. The PKS API Manager itself requires a single VM which will listen for CLI commands and then deploy the cluster. This VM will be deployed in the network we create below:

    - Click `Add Network`.
    - Enter `PKS-Management` for the network.
    - Click `Add Subnet` to create one or more subnets for the network.
    - Enter the full path and name of the vSphere Network Switch we will use to connect VMs to. For example `VM Network`. If your vSphere Network Name contains a forward slash character, replace the forward slash with the URL-encoded forward slash character %2F. For example `YOUR-DIRECTORY-NAME%2FYOUR-NETWORK-NAME`.
    - For CIDR, enter a valid CIDR block in which to deploy VMs. For example, enter 192.0.2.0/24. **This should be a range of IPs that can be used for the management plane of PKS. Only 10 IPs will be needed for this development env.**
    - For Reserved IP Ranges, enter any IP addresses from the CIDR that you want to blacklist from the installation. PKS will not deploy any VM to any address in this range.
    - Enter your DNS and Gateway IP addresses for this network.
    - Select which Availability Zones to use with the network.

  - PKS deploys Kubernetes Clusters on demand via a CLI command. Those clusters will be deployed in the network we create below:
    - Click Add Network.
    - Enter `PKS-Clusters` for the network.
    - In order to dynamically provision PKS clusters in this network, select the `Service Networks` checkbox.
    - Click Add Subnet to create one or more subnets for the network.
    - Enter the full path and name of the vSphere Network Switch we will use to connect VMs to. For example `VM Network`. If your vSphere Network Name contains a forward slash character, replace the forward slash with the URL-encoded forward slash character %2F. For example `YOUR-DIRECTORY-NAME%2FYOUR-NETWORK-NAME`.
    - For CIDR, enter a valid CIDR block in which to deploy VMs. For example, enter 192.0.2.0/24. **This should be a range of IPs that can be used for all of the PKS clusters. EACH Kubernetes cluster you want to deploy will use at least 2 IPs.**
    - For Reserved IP Ranges, enter any IP addresses from the CIDR that you want to blacklist from the installation. PKS will not deploy any VM to any address in this range.
    - Select which Availability Zones to use with the network.
    - Click Save.

8. On the `Assign AZs and Networks` page we decide to which `network` and `AZ` PCF Director should be deployed to as follows:

- Since the Director is a single VM, use the drop-down menu to select a Singleton Availability Zone for it.
- Use the drop-down menu to select a Network for your PCF Ops Manager Director. We will use `PKS-Management`.
- Click Save.

## Complete the PCF Director Installation

1. Click the Installation Dashboard link to return to the Installation Dashboard.

  - Notice the tile has changed form orange to green signifying it it ready to be applied!
2. Click "Apply Changes" on the right navigation to begin deploying!
