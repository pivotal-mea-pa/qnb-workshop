#  Day Two Operations with BOSH

Pivotal Cloud Foundy maintains four levels of HA:

* Container
* Process
* VM
* AZ

Container availability is managed by Pivotal Container Service (PKS) and Pivotal Application Service (PAS).  The other three levels are managed by BOSH.

## Process Availability

Demonstrate how BOSH monitors and restarts the processes belonging to a deployment.

1. Log into opsman and get the *Bosh Commandline Credentials*
Connect to ops manager then create an alias with the credentials

2. ssh into the opsmanager vm and create an alias with the credentials.

```
$ alias b="BOSH_CLIENT=ops_manager BOSH_CLIENT_SECRET=XPSY1s3tFFAmZsVibaiyORY_E6xpiz8p BOSH_CA_CERT=/var/tempest/workspaces/default/root_ca_certificate BOSH_ENVIRONMENT=10.172.1.5 bosh"
```

2. List the deployments

```
$ b deployments --column=Name
Using environment '10.172.1.5' as user 'director' (bosh.*.read, openid, bosh.*.admin, bosh.read, bosh.admin)

Name  
apm-f14ee571031574e218e0  
cf-403bbbe79799feeffb3d  
harbor-container-registry-f63961d450a4107f7e71  
p-healthwatch-b8eec1d18ff71bd6f352  
p-rabbitmq-d72f3228f8a3fd6e50ea  
p-redis-d3b124ef6cea14eb8c2d  
p-spring-cloud-services-3d36320869fe2c6f2c64  
pivotal-container-service-e4e28a54b391536a5877  
pivotal-mysql-c795b4cd425e0f651efb  
service-instance_2f64748c-eefb-49db-bdc3-f0ad61a9a974  
service-instance_a6b0c687-ee39-43db-a531-fb066fbe4c91  

11 deployments

Succeeded
```

3. List the virtual machines for a deployment

```
b --deployment cf-403bbbe79799feeffb3d  vms
Using environment '10.172.1.5' as user 'director' (bosh.*.read, openid, bosh.*.admin, bosh.read, bosh.admin)

Task 25350. Done

Deployment 'cf-403bbbe79799feeffb3d'

Instance                                             Process State  AZ   IPs          VM CID                                   VM Type      Active  
backup-prepare/f7b79945-e86d-4dd5-9a66-f86757c7e7ff  running        az1  192.168.2.8  vm-e790a385-327a-41a4-8ac7-ca30b89a947e  micro        true  
blobstore/244c843b-9805-4054-986c-d5b665fb2f38       running        az1  192.168.2.5  vm-515f7930-c58e-4c05-98ca-096300b38fa3  medium.mem   true  
compute/29933554-eeef-4eca-956a-b7b6479be806         running        az1  192.168.2.7  vm-766ce8ef-4e60-4aa6-a096-dc62ba4bf67a  xlarge.disk  true  
control/d99dc57f-dfdf-44d9-883c-c85f15ed9287         running        az1  192.168.2.6  vm-f42b120c-fd42-4270-b11f-8d2154f51c6b  xlarge.disk  true  
database/fbf0e5fd-2a4f-4e0e-aafa-4d470bc19fbf        running        az1  192.168.2.4  vm-7249049e-80bc-4a9e-81e4-abd4717ac34a  large.disk   true  
router/70e747b3-3d84-415a-8145-035540eae105          running        az1  192.168.2.9  vm-f9e07616-6285-4763-81fe-db424e2d08f9  micro        true  

6 vms

Succeeded
```

4. Connect to a VM and see the processes

```
 b --deployment cf-403bbbe79799feeffb3d ssh router/70e747b3-3d84-415a-8145-035540eae105
Using environment '10.172.1.5' as user 'director' (bosh.*.read, openid, bosh.*.admin, bosh.read, bosh.admin)

Using deployment 'cf-403bbbe79799feeffb3d'

Task 25356. Done
Unauthorized use is strictly prohibited. All access and activity
is subject to logging and monitoring.
Welcome to Ubuntu 14.04.5 LTS (GNU/Linux 4.4.0-135-generic x86_64)

 * Documentation:  https://help.ubuntu.com/

The programs included with the Ubuntu system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Ubuntu comes with ABSOLUTELY NO WARRANTY, to the extent permitted by
applicable law.

Last login: Sat Oct 20 21:27:01 2018 from 10.172.1.4
To run a command as administrator (user "root"), use "sudo <command>".
See "man sudo_root" for details.

router/70e747b3-3d84-415a-8145-035540eae105:~$ sudo -i
router/70e747b3-3d84-415a-8145-035540eae105:~# monit summary
The Monit daemon 5.2.5 uptime: 22d 8h 11m 

Process 'consul_agent'              running
Process 'gorouter'                  running
Process 'metron_agent'              running
Process 'bosh-dns'                  running
Process 'bosh-dns-resolvconf'       running
Process 'bosh-dns-healthcheck'      running
System 'system_localhost'           running
router/70e747b3-3d84-415a-8145-035540eae105:~# ps -ef | grep gorouter
vcap      6082     1  0 Sep28 ?        01:35:33 /var/vcap/packages/gorouter/bin/gorouter -c /var/vcap/jobs/gorouter/config/gorouter.yml
vcap      6089  6082  0 Sep28 ?        00:00:00 /bin/bash -e /var/vcap/jobs/gorouter/bin/run_gorouter
vcap      6090  6082  0 Sep28 ?        00:00:00 /bin/bash -e /var/vcap/jobs/gorouter/bin/run_gorouter
vcap      6096  6090  0 Sep28 ?        00:00:00 /bin/bash -e /var/vcap/jobs/gorouter/bin/run_gorouter
vcap      6101  6089  0 Sep28 ?        00:00:00 /bin/bash -e /var/vcap/jobs/gorouter/bin/run_gorouter
vcap      6102  6095  0 Sep28 ?        00:00:00 /bin/bash -e /var/vcap/jobs/gorouter/bin/run_gorouter
vcap      6103  6100  0 Sep28 ?        00:00:00 /bin/bash -e /var/vcap/jobs/gorouter/bin/run_gorouter
vcap      6104  6102  0 Sep28 ?        00:00:00 logger -p user.error -t vcap.gorouter.stderr
vcap      6106  6103  0 Sep28 ?        00:00:10 logger -p user.info -t vcap.gorouter.stdout
root     21915 21897  0 21:28 pts/0    00:00:00 grep --color=auto gorouter
router/70e747b3-3d84-415a-8145-035540eae105:~# kill -9 6082
router/70e747b3-3d84-415a-8145-035540eae105:~# monit summary
The Monit daemon 5.2.5 uptime: 22d 8h 12m 

Process 'consul_agent'              running
Process 'gorouter'                  not monitored
Process 'metron_agent'              running
Process 'bosh-dns'                  running
Process 'bosh-dns-resolvconf'       running
Process 'bosh-dns-healthcheck'      running
System 'system_localhost'           running
router/70e747b3-3d84-415a-8145-035540eae105:~# 
router/70e747b3-3d84-415a-8145-035540eae105:~# ps -ef | grep gorouter
vcap     21931     1  2 21:29 ?        00:00:00 /var/vcap/packages/gorouter/bin/gorouter -c /var/vcap/jobs/gorouter/config/gorouter.yml
vcap     21937 21931  0 21:29 ?        00:00:00 /bin/bash -e /var/vcap/jobs/gorouter/bin/run_gorouter
vcap     21938 21931  0 21:29 ?        00:00:00 /bin/bash -e /var/vcap/jobs/gorouter/bin/run_gorouter
vcap     21944 21938  0 21:29 ?        00:00:00 /bin/bash -e /var/vcap/jobs/gorouter/bin/run_gorouter
vcap     21948 21943  0 21:29 ?        00:00:00 /bin/bash -e /var/vcap/jobs/gorouter/bin/run_gorouter
vcap     21950 21937  0 21:29 ?        00:00:00 /bin/bash -e /var/vcap/jobs/gorouter/bin/run_gorouter
vcap     21952 21949  0 21:29 ?        00:00:00 /bin/bash -e /var/vcap/jobs/gorouter/bin/run_gorouter
vcap     21953 21948  0 21:29 ?        00:00:00 logger -p user.error -t vcap.gorouter.stderr
vcap     21954 21952  0 21:29 ?        00:00:00 logger -p user.info -t vcap.gorouter.stdout
root     21985 21897  0 21:30 pts/0    00:00:00 grep --color=auto gorouter
router/70e747b3-3d84-415a-8145-035540eae105:~# monit summary
The Monit daemon 5.2.5 uptime: 22d 8h 13m 

Process 'consul_agent'              running
Process 'gorouter'                  running
Process 'metron_agent'              running
Process 'bosh-dns'                  running
Process 'bosh-dns-resolvconf'       running
Process 'bosh-dns-healthcheck'      running
System 'system_localhost'           running
```

Monit restarted the failed process.


## VM Availability

## AZ Availability
