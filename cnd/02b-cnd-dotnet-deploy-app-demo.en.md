# Funny Quotes Demo - Part 1
This solution demonstrates the use of multiple microservices built using multiple .NET technologies and running on PCF. The primary goal is to show that PCF can handle legacy code that existed as early as .NET version 1.0, with common stacks introduced as part of .NET framework up to the newest .NET core application. The solution demonstrates a natural progression / evolution that a customer would take towards making their apps cloud native.


The following are the technical features demonstrated within the entire solution:


- Running WebForms application
- Use of ASMX services
- WCF service connectivity
- OWIN bootstrap into IIS
- .NET core MVC application on Linux stack
- Service discovery via Eureka (using ASMX, WCF, and REST)
- MySQL connectivity using EntityFramework
- Config server with GIT repo


# Instructions - Pushing a .NET Full Framework Application
This demo demonstrates pushing a .NET Framework application on to Pivotal Application Services. The quotes will be loaded from an in-memory database. This requires that the Windows stem cell be available on the target foundation.


## Demonstrating pushing a .NET framework application


Step 1: Clone the Funny Quotes repository located [here](https://github.com/Pivotal-Field-Engineering/funnyquotes).

Step 1a: Download compiled build artifacts from https://github.com/Pivotal-Field-Engineering/funnyquotes/releases


Step 2: Open the solution PCFDotNetLegacyToCloudNative.sln file located in the src folder.


Step 3: Navigate to the FunnyQuotesUIForms project in the solution, then build and publish the project. Alternatively use precompiled version from releases package.


Step 3: Login to your foundation.


Step 4: Push the application using the Hosted Web Core buildpack and the Windows 2016 stem cell.


	`cf push FunnyQuotesUIForms -b hwc_buildpack -s windows2016`


Step 5: Launch the application in the browser to view random returned quotes


Step 6: Demonstrate scaling and failover of the application
g and failover of the application
