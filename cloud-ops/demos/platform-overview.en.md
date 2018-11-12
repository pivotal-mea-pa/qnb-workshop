#  Platform Overview


Create a simple demo of the Fortune Teller sample application.  This will give us an applicatino to view in later modules.

1. Download Fortune Teller

```
git clone https://github.com/Pivotal-Field-Engineering/fortune-teller-demo.git
```

2. Create a manifest for cloud foundry

```
$ cp manifest-pcf.yml manifest.yml
```


3. Cloud Foundry Org and Space

```
$ cf login
API endpoint: https://api.system.labs.cbmasters.com

Email> admin

Password> 
Authenticating...
OK

$ cf create-org workshop
$ cf create-space fortune -o workshop
$ cf target -o workshop -s fortune

```

4. Build the app

```
$ ./mvnw package
```

5. Create the services

First, create a fork of the config github and change the script to point to the new repo.


```
$ ./scripts/create_services_pcf.sh
Creating service instance fortunes-db in org workshop / space fortune as admin...
OK

Create in progress. Use 'cf services' or 'cf service fortunes-db' to check operation status.
Creating service instance config-service in org workshop / space fortune as admin...
OK

Create in progress. Use 'cf services' or 'cf service config-service' to check operation status.
Creating service instance service-registry in org workshop / space fortune as admin...
OK

Create in progress. Use 'cf services' or 'cf service service-registry' to check operation status.
Creating service instance circuit-breaker in org workshop / space fortune as admin...
OK

Create in progress. Use 'cf services' or 'cf service circuit-breaker' to check operation status.
```

Wait until all services have been created.


```

$ cf services
Getting services in org workshop / space fortune as admin...

name               service                       plan       bound apps   last operation
circuit-breaker    p-circuit-breaker-dashboard   standard                create succeeded
config-service     p-config-server               standard                create succeeded
fortunes-db        p.mysql                       db-small                create succeeded
service-registry   p-service-registry            standard                create succeeded

```


```
$ cf push
```

