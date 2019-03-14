# Azure Storage with Pivotal Application Services for Windows and Steeltoe

## Pre-requisites

The foundation to run the demo on must have PASW and the Microsoft Azure Service Broker deployed. This gives you Windows Diego Cells which the manifests provided in the Visual Studio Templates assume are present.

The Workshop assumes the Students have been given the following:
 - **API URL** of the Workshop foundation
 - URL of **App Manager**
 - **User name** and **password** to login to the foundation
 - **Org and Space** associated with the user account

## LABS
### Intro & Creds Lab

An initial step for the Student to gather needed PAS info
 - API URL
 - AppManager URL
 - Pre-loaded credentials from a gSheet for PAS

### Setup Environment

A step to verify the Student has the correct software and cli's loaded, provides some workarounds for Student's with limited desktop permissions, and set a few local environment variables with their creds (that are used in future labs).

### Bind the Microsoft Azure Service Broker

Using AppManager guide the workshop student through the Marketplace to find the Azure SQL Service Broker and bind to their space.

### Consume the Microsoft Azure Service Broker in your application

Using the .NET Core template application, add in Steeltoe config dependencies to retrieve VCAP, create the DBContext for data storage, and watch the magic happen.

### Next Steps

A "congrats" for completing the workshop and a reminder to continue learning through Pivotal Education's cloud native .NET developer course. There is also a reminder to experience all the examples in the Steeltoe public repo on Pivotal Web Services.

## Config.json with all slides and labs
	```json
	{
		"workshopSubject":"Azure Storage with Pivotal Application Services for Windows and Steeltoe",
		"workshopHomepage":"",
		"workshopHostname":"steeltoe-azure-storage",
		"modules": [
			{
				"type": "demos",
				"content": [
					{
						"name":"Introduction & Credentials",
						"filename":"pasw-workshop/labs/intro-creds"
					},
					{
						"name":"Setup Environment",
						"filename":"pasw-workshop/labs/setup-environment"
					},
					{
						"name":"Bind the Microsoft Azure Service Broker",
						"filename":"steeltoe-azure-storage/labs/bind-sb"
					},
					{
						"name":"Consume the Microsoft Azure Service Broker in your application",
						"filename":"steeltoe-azure-storage/labs/dbcontext"
					},
					{
						"name":"Push Microservice to PASW",
						"filename":"pasw-workshop/labs/push-microservice"
					},
					{
						"name":"Continuing Your .NET Cloud Native Journey",
						"filename":"steeltoe-azure-storage/labs/next-steps"
					}
				]
			}
		]
	}
	```