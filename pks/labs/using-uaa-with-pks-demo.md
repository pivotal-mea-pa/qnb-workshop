# Using the UAA Client (UAAC) with PKS

In order to use the PKS CLI we need to authenticate ourselves with it.
Since PKS is backed by UAA for authorization, we can either integrate with an external identity management system (AD/LDAP supported now), or we can create a username and password in UAA which we then will utilize to log into PKS.

In the scenario where we do integrate with an AD/LDAP system, we will also need to use the UAA CLI to map LDAP groups with UAA scopes.

## Pre-Requisites

- Pivotal Ops Manager deployed
- Pivotal Director Tile deployed
  - Post-deploy scripts enabled
- Pivotal Container Service Tile deployed

## Goals
Create a new local user in UAA with the UAA Client (UAAC) and then login PKS with this user.

## Determine UAA Admin Credentials

1. Navigate to the Ops Manager FQDN (hostname) in your browser of choice.

1. Login with the Ops Manager credentials setup earlier.

1. Click on Pivotal Container Service Tile.

1. Click on Credentials.

1. Find the **Pks Uaa Management Admin Client** entry and click on Link to Credential.

1. Note down the secret.

1. Click on Status.

1. Note the IP for the `Pivotal Container Service` job down.

## Access the UAAC CLI

In order to create a new user in UAA we need to use the UAA Client (UAAC) CLI , which requires Ruby locally.

- If you do not wish to install Ruby and UAAC locally, skip the following 2 steps and utilize UAAC on the Ops Manger VM.
  - To access the Ops Manager, use SSH and the password set when deploying the Pivotal Ops Manager

        $ ssh ubuntu@<PIVOTAL-OPS-MANAGER-FQDN>

1. Install Ruby, help with this can be found [here](https://www.ruby-lang.org/en/documentation/installation/).

1. Install the `cf-uaac` RubyGem

       $ gem install cf-uaac

## Access the deployed UAA Service

In order to reach the UAA service we must ensure either our DNS or Load Balancer is sending us to the correct UAA url. We cannot utilize IPs due to the default security enforcement built into UAA. **Skip this step if you deployed a load balancer and/or registered the PKS Management API hostname with your DNS**

1. In order to have our ***Local*** DNS system resolve the PKS Management API hostname to the correct IP lets edit the local host resolution file. Replace the IP used below with your IP noted earlier. If using the PCF Ops Manager VM use `Linux` below.

    - Mac OS X

          $ sudo vim /etc/hosts

          ---
          IP Address Host
          192.0.2.1 api.pks.<INSERT DOMAIN HERE>

    - Windows

        Use Notepad and open `c:\Windows\System32\Drivers\etc\hosts` with Administrator rights.

          ---
          IP Address Host
          192.0.2.1 api.pks.<INSERT DOMAIN HERE>

    - Linux

          $ sudo vim /etc/hosts

          ---
          IP Address Host
          192.0.2.1 api.pks.<INSERT DOMAIN HERE>


## Grant Cluster Access to a User

1. Target your UAA API endpoint using `uaac target https://<PKS-API>:8443 --skip-ssl-validation`.

       $ uaac target https://api.pks.pivotal.io:8443 --skip-ssl-validation

  - Replace <PKS-API> with the URL to your PKS API server.You configured this URL in the PKS API section of Installing PKS for your IaaS.

1. Authenticate with UAA using the secret you retrieved in the previous section using `uaac token client get admin -s <INSERT GENERATED UAA ADMIN SECRET>`.

       $ uaac token client get admin -s generated-super-secret

1. Create a user by running `uaac user add <USERNAME> --emails <USER-EMAIL> -p <USER-PASSWORD>`.

       $ uaac user add pks_admin --emails pks_admin@pivotal.io -p password

1. Assign a scope to the user to allow them to access Kubernetes clusters. Use `uaac member add <UAA-SCOPE> <USERNAME>`.

       $ uaac member add pks.clusters.admin pks_admin

    - Replacing `UAA-SCOPE` with one of the following UAA scopes depending on the role of the user:
        - pks.clusters.admin: Users with this scope have full access to all clusters.
        - pks.clusters.manage: Users with this scope can only access clusters they create.

## Login to PKS With Newly Created User

1. Login to your PKS API Server using `pks login -a <PKS-API> -u <USERNAME> -p <USER-PASSWORD> -k`

       $ pks login -a api.pks.pivotal.io -u dev01 -p devpassword -k


## Grant PKS Access to an LDAP Group

1. Log in as the UAA admin using the same procedure as above, before you created the user.

1. To assign the `pks.clusters.manage` or `pks.clusters.admin` scopes to all users in an LDAP group, run the following command:
  ```
  uaac group map --name SCOPE GROUP-DISTINGUISHED-NAME
  ```
  Where GROUP-DISTINGUISHED-NAME is the LDAP Distinguished Name (DN) for the group. For example:
  ```
  uaac group map --name pks.clusters.manage cn=operators,ou=groups,dc=example,dc=com
  ```
