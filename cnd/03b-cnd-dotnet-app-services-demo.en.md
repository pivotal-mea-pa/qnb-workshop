# Funny Quotes Demo - Part 2


# Instructions - Using Services with Legacy .NET Applications
The second step of the Funny Quotes demo is to demonstrate pushing a .NET Framework legacy service on to Pivotal Application Services. The .NET services will demonstrate the use of a MySQL database and a service registry. The demo will include an Entity Frameworks migration to initially populate a MySQL database with quotes to be used by the web service as well as binding the database to the application. This requires that the Windows stem cell be available.


## Pre-demo Configuration
Step 1: Because it can take some time to provision, it is recommended that services be pre-created and configured for a Service Registry instance and a Config Server instance ahead of the demo.


Step 2: Clone the config server repo located [here](https://github.com/macsux/fortunesconfig).


Step 3: Configure the config server instance to point to the files in the config server repo directory of the solution.  Modify the FunnyQuotesUIForms.yaml and edit the client type to 'asmx'. Valid values are: 'rest', 'asmx', 'local', 'wcf'.


Note: The demo application has a timer that will reload the config data so that the application does not have to be restarted or the configuration data reloaded.


## Demonstrating using a MySQL database and pushing a .NET Framework legacy service


Step 1: Navigate to the services marketplace on your foundation and create a new MySQL database instance.


Step 2: Open the solution file from Part 1 and navigate to the FunnyQuotesLegacyService project, then build and publish the project.


Step 3: Login to your foundation.


Step 4: Push the service using the Hosted Web Core buildpack and the Windows 2016 stem cell.


	`cf push FunnyQuotesLegacyService -b hwc_buildpack -s windows2016 --health-check-type http`


Step 5: Bind the service to the MySQL database you created in Step 1.


Step 6: Bind the UI application to the config server instance that was previously created.


Step 7: Bind the UI application to the service registry instance that was previously created.


Step 8: Bind the service to the service registry instance that was previously created and restart the service.


Step 9: Restart and launch the FunnyQuotesUIForms application in the browser to view random returned quotes.
