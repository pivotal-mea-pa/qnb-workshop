# Connect to a DB on CF

**Previous:** [CF Deployment](../cf-deployment)

In this section, we'll configure our app to talk to a database on our Cloud Foundry instance.

First, let's take a look at the services available. Type:
```bash
cf marketplace
```
and you should see something like:
```bash
Getting services from marketplace in org cfdev-org / space cfdev-space as admin...
OK

service   plans        description
p-mysql   10mb, 20mb   MySQL databases on demand

TIP:  Use 'cf marketplace -s SERVICE' to view descriptions of individual plans of a given service.
```
We'll be utilizing the `p-mysql` service under the `20mb` plan with our app. Let's create it:
```bash
cf create-service p-mysql 20mb notes-db
```
should return something like:
```bash
Creating service instance notes-db in org cfdev-org / space cfdev-space as admin...
OK
```

***

Now that we've created our service, we should bind it to our app. You can manually bind it using
```bash
cf bind-service notes notes-db
```
or make use of service bindings in our `ci/manifest.yml`:
```yaml
applications:
  - name: notes
    buildpack: <BUILDPACK_NAME>
    services:       # <--- Bind services
      - notes-db    # <--- The service that was just created
```
This method is preferred because it's portable between different instances of CF.

Push your changes now, and once the deploy task has run, take a look at the bindings that have been setup using
```bash
cf env notes
```
which returns something like
```bash
Getting env variables for app notes in org cfdev-org / space cfdev-space as admin...
OK

System-Provided:
{
 "VCAP_SERVICES": {
  "p-mysql": [
   {
    "binding_name": null,
    "credentials": {
     "hostname": "10.144.0.141",
     "jdbcUrl": "jdbc:mysql://10.144.0.141:3306/cf_6d0f940b_ee3a_4914_9cf6_63ef02d95a69?user=jWmOCZ7e1CY3Yvxd\u0026password=Y3QIm3fJldNpnoTd",
     "name": "cf_6d0f940b_ee3a_4914_9cf6_63ef02d95a69",
     "password": "Y3QIm3fJldNpnoTd",
     "port": 3306,
     "uri": "mysql://jWmOCZ7e1CY3Yvxd:Y3QIm3fJldNpnoTd@10.144.0.141:3306/cf_6d0f940b_ee3a_4914_9cf6_63ef02d95a69?reconnect=true",
     "username": "jWmOCZ7e1CY3Yvxd"
    },
    "instance_name": "notes-db",
    "label": "p-mysql",
    "name": "notes-db",
    "plan": "20mb",
    "provider": null,
    "syslog_drain_url": null,
    "tags": [
     "mysql"
    ],
    "volume_mounts": []
   }
  ]
 }
}

{
 "VCAP_APPLICATION": {
  "application_id": "06d04a78-ee64-4acf-a4fe-e457e99e7fbd",
  "application_name": "notes",
  "application_uris": [
   "notes.dev.cfdev.sh"
  ],
  "application_version": "91dc4285-ab16-4344-8f8c-fa1df2d4d683",
  "cf_api": "https://api.dev.cfdev.sh",
  "limits": {
   "disk": 1024,
   "fds": 16384,
   "mem": 256
  },
  "name": "notes",
  "space_id": "6b0c42ad-6d64-4a8c-8c48-cae23ae4b2d6",
  "space_name": "cfdev-space",
  "uris": [
   "notes.v3.pcfdev.io"
  ],
  "users": null,
  "version": "91dc4285-ab16-4344-8f8c-fa1df2d4d683"
 }
}

No user-defined env variables have been set

No running env variables have been set

No staging env variables have been set
```

The section of interest to us is in the `VCAP_SERVICES` object. It contains a single binding of `p-mysql` named `notes-db`. The shows that the binding worked correctly. The Steeltoe connector automatically parses the environment for the application and determines the credentials for the bound `p-mysql` instance. 

That's it! Run a `GET` on `https://notes.dev.cfdev.sh/api/notes`. You should see:
```json
[]
```

**Git Tag:** [deploy-and-connect-to-db-on-cf](https://github.com/xtreme-steve-elliott/NotesApp/tree/deploy-and-connect-to-db-on-cf)

**Up Next:** [Smoke Tests](../smoke-tests)
