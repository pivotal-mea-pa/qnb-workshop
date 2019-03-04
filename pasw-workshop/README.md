# Pivotal Application Services for Windows Workshop 

Workshop to introduce the basics of PAS to a .NET savvy group. The workshop creates and pushes a .NET Core app to PASW. All labs are run on a Windows desktop and assume powershell is accessible (by default it is on Windows).

## Pre-requisites

The foundation to run the demo on must have PASW deployed. This gives you Windows Diego Cells which the manifests provided in the Visual Studio Templates assume are present.

The Workshop assumes the Students have been given the following:
 - **API URL** of the Workshop foundation
 - URL of **App Manager**
 - **User name** and **password** to login to the foundation
 - **Org and Space** associated with the user account

There is a sample worksheet in the [Field Collateral/PASW Workshop](https://drive.google.com/open?id=1ajWGLFQ2aE0Ta7iN3hF0jGwDp73VZt9P) folder to use.

### Intro & Creds Lab

An initial step for the Student to gather needed PAS info
 - API URL
 - AppManager URL
 - Per-loaded credentials from a gSheet for PAS

### Setup Environment

A step to verify the Student has the correct software and cli's loaded, provides some workarounds for Student's with limited desktop permissions, and set a few local environment variables with their creds (that are used in future labs).

### Create Microservice

The Student opens Visual Studio and creates a new project based on the public Steeltoe Cloud Foundry templates.

### Push Microservice

Using Visual Studio the Student publishes the new microservice app (which creates the artifact), reviews the manifest provided in the template, and executes `cf push`.

### Explore Applications Manager

A step by step to log in to App Manager and understand Org/Space navigation, get to the home page of a deployed app, and discover the marketplace.

### Attach an autoscaler

Using App Manager the Student is guided through the marketplace to attach an autoscaler to their app and set a scaling rule.

### Add a new route

Using App Manager the Student is guided through attaching a new route to their app.

### Next Steps

A "congrats" note to the student for completing the workshop, and a reminder to continue learning about cloud native .NET with the Steeltoe Workshop. There is also an offer to experience all the examples in the Steeltoe public repo on Pivotal Web Services

## Config.json with all labs

	````json
	{
		"workshopSubject":"Pivotal Application Services for Windows",
		"workshopHomepage":"",
		"workshopHostname":"pasw-workshop",
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
						"name":"Create .NET Microservice",
						"filename":"pasw-workshop/labs/create-microservice"
					},
					{
						"name":"Push Microservice to PASW",
						"filename":"pasw-workshop/labs/push-microservice"
					},
					{
						"name":"Explore Applications Manager",
						"filename":"pasw-workshop/labs/explore-appmanager"
					},
					{
						"name":"Attach an autoscaler",
						"filename":"pasw-workshop/labs/attach-autoscaler"
					},
					{
						"name":"Add a new route",
						"filename":"pasw-workshop/labs/add-route"
					},
					{
						"name":"Next Steps",
						"filename":"pasw-workshop/labs/next-steps"
					}
				]
			}
		]
	}
	````