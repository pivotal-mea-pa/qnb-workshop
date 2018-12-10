# Quotes Service

For this lab we have provided most of the code that makes up the Quote service, while leaving a skeleton of the **getQuotes** method in the controller to be completed.

Go ahead and open the quotes-service project in your favorite IDE and navitgate down to the directory `src/main/java/io/pivotal/quotes`. Here you can see the pieces that make up the Quotes Service we are going to edit the **QuoteV1Controller.java**.


### Quotes Controller

As you can see here most of the code for the **getQuotes** method is missing. Our task for this section is to use the pieces we already have, and some knowledge about Spring annotations to complete the method.
 - If at any time you want to skip ahead, you can find the completed code [here](https://github.com/Pivotal-Field-Engineering/pivotal-bank-demo/blob/master/quotes-service/src/main/java/io/pivotal/quotes/controller/QuoteV1Controller.java)

First off, let's look at our Quote Service class found in the **quotes/service** folder. We have a few Strings that are hardcoded pointing to the IEX Trading's API URL, we have three main methods **getQuote**, **getQuotes**, and **getCompanyInfo**. There are also three fallback methods used by Hystrix to provide failover if our three main methods should ever fail.

Based on the principles of domain driven design, we can assume that our controller will be leveraging some or all of these main methods. (NOTE: The fallback methods are internal use only.) Looking at our QuoteV1Controller you will find the getCompanies method is already completed (1 down two to go). Let's use that method as a template for filling the missing information to our getQuotes method.
 - Another hint: We will be calling both getQuote and getQuotes from the service class inside the getQuotes controller method)

We will want to do several things:
 - Add the RequestMapping annotation
 - Change the return value from null to more resemble our neighbor **getCompanies**
 - Use the Request Parameter **query** captured in the method argument to build our calls to the service methods, again referencing **getCompanies** and the above null case.
 - Remember that our one controller is going to handle either a single quote, or multiple quotes where the quotes will be comma separated.

To test your quote service, use the provided gradle wrapper and assemble it, then run it locally
```
./gradlew assemble
java -jar build/libs/quotes.jar
```
If we have completed the **getQuotes** correctly, we can navigate to `http://localhost:8080/v1/quotes?q=FISV` and we should receive a plan txt JSON response with Fiserv's stock trading data.

### Pushing the controller

In **Cloud Foundry** vocabulary, deploying an application is referred to as *pushing* the application since we are uploading the application artifact to the cloud.

More information on the application deployment process can be found [here](http://docs.pivotal.io/pivotalcf/devguide/deploy-apps/deploy-app.html)

The `cf push` command is used for this. In fact, running this command with no parameters, the CLI will look for a file *manifest.yml* in the current directory. Alternatively, you can specify an application name as the parameter and the CLI will upload all files it finds in the current directory.

### Application manifest files.
Application manifests tell `cf push` what to do with applications. This includes everything from how many instances to create and how much memory to allocate to what services applications should use.

A manifest can help you automate deployment, especially of multiple applications at once.

### Modifying the manifest files.
Since each application requires a route to be bounded to the application, and potentially there may be many instances of these services deployed, we want to ensure we have unique routes bounded to the services we deploy.

Luckily, Pivotal Cloud Foundry allows us to assign a [random-route](http://docs.pivotal.io/pivotalcf/devguide/deploy-apps/manifest.html#random-route) to applications. The manifest files in the project include this option.

### Exercise

1. Navigate to the top level quotes-service directory, you will find the **manifest.yml** there.
1. *Push* the **quote service** to the cloud by [specifying the specific manifest file](http://docs.pivotal.io/pivotalcf/devguide/deploy-apps/manifest.html#find-manifest) to the `cf push` command.
1. When the app has completed pushing, make sure to note down the URL from the output. We wll be using that to test our application below.

# Summary

Ensure you have a working quote service application by sending HTTP requests to it, put the below into your browser:

`http://<ROUTE_TO_QUOTE_SERVICE>/v1/quotes?q=PVTL`

Ensure the service registers itself with the registry server by looking at the discovery service dashboard. (Services - Service Registry - Manage)

Congratulations! you have now deployed an application to the cloud that registers itself with the registry service and handles HTTP requests.
