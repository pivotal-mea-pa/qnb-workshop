# The Other Services

In this exercise, we will be deploying all the applications in the project to the cloud and create all required services. For each of these services we will examine their manifests before we push.

### 1. Quote service
We have already deployed the quote service in the previous lab, so nothing to do here.

### 2. Accounts service
Navigate to the `accounts-service` directory, run the gradle wrapper assemble command, then **cf push**

### 3. Portfolio service

The portfolio service has a dependency on 3 services:

- A RDBMS.
- The quote service.
- The Account service.

The portfolio service also connects to the quote and account service. We are using a registry service to automatically and dynamically discover the other services. The other services are discovered automatically use the discovery service.

Do the same for portfolio that you did for account.

### 4. Web service
The Web service is the UI front-end and also acts as an API aggregator. As such, it uses all the other microservices in the project, i.e. The quote, account and portfolio services.

Similarly to above, we will be using the registry service to retrieve information about these microservices.

### 5. User service
The User service is the provides a user repository and authorization api.

### Exercises

 - Push each of the services to the platform.

    > How could you push all the services in one go?
    > The **Cloud Foundry** manifest file allows us to [define multiple applications in a single file](http://docs.pivotal.io/pivotalcf/devguide/deploy-apps/manifest.html#multi-apps)

Once completed, go to the URL of the Web service in your browser.

## Summary
Congratulations! You have now deployed a set of microservices to the cloud that interact with each other.
