## Goal

In order for BOSH to communicate with specific infrastructures (GCP/AWS/vSphere/etc.) we abstract these details into a BOSH cloud-config. Lets create and upload our own to the BOSH director.

## Prerequisites

1. Obtain your workshop jumpbox public IP, Username, and Password 
1. Obtain your GCP Network Name 

## Part 1: Creating the Cloud Config


1. Utilize your favorite command-line text editor to create a `cloud-config.yml` text file. (We will use `nano`, but `vi` works as well!)

  - `nano cloud-config.yml`

1. Paste the following into the `cloud-config.yml`.

````yaml
azs:
- name: z1
- name: z2
- name: z3

vm_types:
- name: default
- name: large

disk_types:
- name: default
  disk_size: 3000

networks:
- name: default
  type: manual
  subnets:
  - range:   10.244.0.0/24
    gateway: 10.244.0.1
    dns:     [8.8.8.8, 8.8.4.4]
    reserved: [10.244.0.0-10.244.0.10]
    azs:     [z1, z2]
    cloud_properties:
      name: vboxnet0
- name: vip
  type: manual
  subnets:
  - range:   10.244.0.0/24
    gateway: 10.244.0.1
    dns:     [8.8.8.8, 8.8.4.4]
    reserved: [10.244.0.10-10.244.0.255]
    azs:     [z1, z2]
    cloud_properties:
      name: vboxnet0

compilation:
    workers: 3
    reuse_compilation_vms: true
    az: z1
    vm_type: default
    network: default
````


## Part 2: Understanding the Cloud Config

When deploying services we have to define the following:

  1. `azs` or availability zones allow us to provide high availability to our services deployed with BOSH. Here we create 3 zones, z1, z2, and z3.

  1. `vm_types` allow us to t-shirt size the vms that make up our services deployed with BOSH. In public clouds, we would list the machine type, see Reference section below.

  1. `disk_types` describe what type disks can be attached as a persistent disk to the vms that make up our services deployed with BOSH. In this case we create a 3GB disk.

  1. `networks` define what ips, gateways, dns, and specific cloud provider networking properties are needed. Here we will use the `10.244.0.0 - 10.244.0.255` subnet with google dns `8.8.8.8`, and the network that you replaced.

  1. `compilation` specifies which of all the above resources BOSH should use when compiling code.

## Part 3: Uploading the Cloud Config to the BOSH director

1. Upload the `cloud-config.yml` to the BOSH director. All services we deploy with BOSH will use this from now on.

  - `bosh -e my-bosh update-cloud-config cloud-config.yml`

## Reference

1. [vSphere CPI](https://bosh.io/docs/vsphere-cpi/#cloud-config)
2. [Google CPI](https://bosh.io/docs/google-cpi/#cloud-config)
3. [AWS CPI](https://bosh.io/docs/aws-cpi/#cloud-config)
4. [Azure CPI](https://bosh.io/docs/azure-cpi/#cloud-config)
5. 
