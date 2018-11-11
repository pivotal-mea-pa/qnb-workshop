# Pivotal Ops Manager

## Goal
Bring up the PCF Ops Manager VM within vCenter. This will be the operational console we will use to deploy PKS.
    
## Prerequisites

- VMware vCenter
- Ability to upload `*.OVA` template to vCenter
- IP to assign to the PCF Ops Manager with DNS hostname.
    - Gateway, DNS, and Netmask corresponding to this IP.
- NTP Server

## Download the OVA Template:

1. Point your browser to `https://network.pivotal.io/`
1. Create an Account and Login
1. Scroll down and under the `Pivotal Cloud Foundry` section click `Pivotal Cloud Foundry Operations Manager`
1. Under `Release Download Files` click on the one that reads `Pivotal Cloud Foundry Ops Manager for vSphere`.
1. Accept the EULA
1. **Ensure** the release is `2.0` or higher!

## Upload the OVA Template to vSphere:

1. Login into your vSphere Web Client

1. Using the `hosts and clusters` view right click on your vCenter

1. Click `Deploy OVF Template`

1. Click `Browse...` and navigate to where the `OVA` downloaded earlier is located

1. Select a `Datacenter` where the VM will live

1. Select a `cluster` where the VM will live

1. Review the details and click `next`

1. Select a `datastore` where the VM will live

1. Select a `network` where the VM will live

1. Fill in the specific values for PCF Ops Manager as defined below and click `next` followed by `finish`!
    - **Admin Password:** This password is used to SSH into the Ops Manager VM we are deploying. The username is always `ubuntu`
    - **Custom Hostname:** If you would like to set a custom hostname for the PCF Ops Manager do so here, this hostname should resolve to the PCF Ops Manager IP.
    - **DNS:** The DNS server that can resolve hostnames within your datacenter.
    - **Default Gateway:** When we are attempting to communicate with an IP that is out of our local subnet we will go to a router for directions. Put that router IP here.
    - **IP Address:** The IP address for the Ops Manager VM.
    - **NTP Servers:** A valid NTP server to keep time for the Ops Manager VM.
    - **Netmask:** The corresponding netmask for our subnet.
    
1. Once fully uploaded to vSphere, power on the VM.

## Configuring PCF Ops Manager

1. Navigate to `https://<HOSTNAME-SPECIFIED-EARLIER>`. If you do not have DNS setup to resolve the hostname to the Ops Manager VM given earlier when deploying the OVA, directly utilize the IP specified earlier, to access Ops Manager.

1. You might need to accept the SSL cert if it is `self-signed`.

1. Depending on your needs select the correct type of Authentication System, we will choose `Internal Authentication` for the remander of this guide. Under the covers PCF Ops Manager deploys `PCF UAA` which manages access to the `PCF` ecosystem. The type of authentication system you choose defines how `UAA` will retrieve and store users that can access the PCF Ops Manager. 

    More info below:

    - **Use an Identity Provider:** Links together PCF Ops Manager with an external Identity Provider which maintains the user database to access the PCF Ops Manager. SAML is an example of an IdP type, and Okta is an example of a IdP itself.

    - **Internal Authentication:** PCF UAA will create and maintain a new user database for use with PCF Ops Manager.
  
1. Set `username` and `password`. These will be used to login to the PCF Ops Manager **GUI**. 
    - *Remember `ssh` credentials were specified earlier!*
1. Set the `Decryption passphrase`. This is used to encrypt the database backing our authentication service `PCF UAA`.
1. Set `Http Proxy`, `Https Proxy`, and `No proxy` depending on *if* you need to use a proxy to access the internet from your datacenter.
1. Agree to the terms and click `finish`!