# Using BOSH to show PKS Clusters

## Goal
To explore the PKS Cluster VMs/IPs/Logs we must dive deeper into the PCF Director, also known as BOSH.

## Prerequisites
- Deployed Pivotal Ops Manager (2.0+)
- Deployed Pivotal Director Tile
  - Post-deploy scripts enabled
- Deployed Pivotal Container Service Tile

## Access the BOSH CLI
  1. Navigate to the Pivotal Ops Manager FQDN.

  2. Login.

  3. Click on the Pivotal Director Tile (vSphere).

  4. Click the tab labeled `status`. Here is the list of VMs deployed by the platform and the current status.

  5. Note the IP of the `Ops Manager Director` job down, this is the Director. The director has the knowledge of all kubernetes clusters deployed.

  6. Click the tab labeled `credentials`. Here is the Pivotal Director credentials that the platform auto generates when deploying tiles.

  7. Click `Link to Credential` under Director Credentials.

  8. Note the `identity`, and `password` down. This is the username and password we will use to connect to the Director.

  9. Using SSH and the password set when deploying Pivotal Ops Manager, SSH to the Ops Manager VM.
      ```
      $ ssh ubuntu@<PIVOTAL-OPS-MANAGER-FQDN>
      ```
  10. Use the `bosh` cli to connect to the Director using the IP found earlier.
      ```
      $ bosh alias-env pcf -e <DIRECTOR-IP> --ca-cert /var/tempest/workspaces/default/root_ca_certificate
      ```
  11. Login to the `bosh` cli using the username, and password found earlier.
      ```
      $ bosh -e pcf login
      ```

## View BOSH Deployments

  1. BOSH deploys our PKS clusters. Lets list those deployments.
      ```
      $ bosh -e pcf deployments
      Using environment '10.0.2.1' as user 'director' (bosh.*.read, openid, bosh.*.admin, bosh.read, bosh.admin)

      Name                                                   Release(s)                          Stemcell(s)                                       Team(s)                                         Cloud Config
      pivotal-container-service-fc2740c309c815ffadf5         bosh-dns/0.0.11                     bosh-vsphere-esxi-ubuntu-trusty-go_agent/3468.13  -                                               latest
                                                          docker/30.1.4
                                                          kubo/0.10.0
                                                          kubo-etcd/6
                                                          kubo-service-adapter/0.5.0-dev.191
                                                          on-demand-service-broker/0.19.0
                                                          pks-api/0.0.0-dev.219
                                                          pks-helpers/15.0.0
                                                          pks-nsx-t/0.1.0-dev.75
                                                          pks-nsx-t-precheck/0.1.0-dev.72
                                                          postgres/23
      service-instance_2c4ca713-f66e-4837-b5e5-d91c324f8cb0  bosh-dns/0.0.11                     bosh-vsphere-esxi-ubuntu-trusty-go_agent/3468.13  pivotal-container-service-fc2740c309c815ffadf5  latest
                                                          docker/30.1.4
                                                          kubo/0.10.0
                                                          kubo-etcd/6
                                                          pks-helpers/15.0.0
                                                          pks-nsx-t/0.1.0-dev.75                                                    
      ```

      - Notice the `Pivotal-container-service deployment` which was deployed from the PKS tile. Also notice the `service-instance_*` which is our deployed PKS cluster, note it down.

    2. With the deployment name we can list the vms part of it. Notice the master IP of the PKS cluster.
        ```
        $ bosh -e pcf -d service-instance_2c4ca713-f66e-4837-b5e5-d91c324f8cb0 instances
        Using environment '10.0.2.1' as user 'director' (bosh.*.read, openid, bosh.*.admin, bosh.read, bosh.admin)

        Task 57. Done

        Deployment 'service-instance_2c4ca713-f66e-4837-b5e5-d91c324f8cb0'

        Instance                                     Process State  AZ   IPs
        master/40eea419-3f84-43f0-b6b4-a3c1e99b7f4f  running        AZ1  10.0.2.51
        worker/ae7edd19-2356-4333-8b70-41d8cc7ffc9c  running        AZ1  10.0.2.52

        2 instances

        Succeeded
        ```

  3. Exit out of the SSH Client
      ```
      $ exit
      ```
