**Previous:** [Deploy Concourse](../Deploy-Concourse)

We will install CF Dev locally to simulate the Cloud Foundry environment. Prior to this, you must have the [CF CLI](https://github.com/cloudfoundry/cli#downloads) installed. Follow the instructions [here](https://github.com/cloudfoundry-incubator/cfdev) to install and run CF Dev.

Once complete, run:
```bash
cf login -a https://api.dev.cfdev.sh --skip-ssl-validation
```
with the provided admin user and password (`admin`/`admin`). When prompted for an `org`, choose `1. cfdev-org`:
```bash
API endpoint: https://api.dev.cfdev.sh

Email> admin

Password>
Authenticating...
OK

Select an org (or press enter to skip):
1. cfdev-org
2. system

Org> 1
Targeted org cfdev-org

Targeted space cfdev-space



API endpoint:   https://api.dev.cfdev.sh (API version: 2.120.0)
User:           admin
Org:            cfdev-org
Space:          cfdev-space

```

**Up Next:** [.NET Core App](/#net-core-app)
