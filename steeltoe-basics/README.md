# Steeltoe Cloud Native .NET Microservices Workshop 

Workshop to introduce cloud native .NET with the Steeltoe Framework. The workshop creates and pushes a .NET Core app to PASW. All labs are run on a Windows desktop and assume powershell is accessible (by default it is on Windows).

## Pre-requisites

The foundation to run the demo on must have PASW deployed. This gives you Windows Diego Cells which the manifests provided in the Visual Studio Templates assume are present.

The Workshop assumes the Students have been given the following:
 - **API URL** of the Workshop foundation
 - URL of **App Manager**
 - **User name** and **password** to login to the foundation
 - **Org and Space** associated with the user account

There is a sample worksheet in the [Field Collateral/Steeltoe Workshop](https://drive.google.com/open?id=1ajWGLFQ2aE0Ta7iN3hF0jGwDp73VZt9P) folder to use.

## SLIDES
### About the Workshop

This deck is an overview of what the labs will cover. The intention is for the Workshop Proctor to review these slides with the entire class, prior to letting everyone begin the hands-on labs individually.

### Advanced Steeltoe

This deck covers advanced topics that will be relavant to a student once they have completed the labs. It is meant to be presented prior to the labs being completed.
- Custom health contributors

## LABS
### Intro & Creds Lab

An initial step for the Student to gather needed PAS info
 - API URL
 - AppManager URL
 - Pre-loaded credentials from a gSheet for PAS

### Setup Environment

A step to verify the Student has the correct software and cli's loaded, provides some workarounds for Student's with limited desktop permissions, and set a few local environment variables with their creds (that are used in future labs).

### Environment Variables

It is typical to find .NET developers that aren't too familar with using environment variables as a means for app settings. Tradaitionally this was done with web.*.config files. This lab aims to introduce them to the Steeltoe Cloud Foundry Provider, where they can consume env var values that have been set in an app manifest.

### User Provided Services with Steeltoe Cloud Foundry Provider

This lab extends the environment variable lab, to take those same environment variables and put their values in a User Provided Service. Their app manifest is altered to bind the service and consume the value through the app.

### Include Steeltoe Actuators and Create a Custom Health Check

Adding in some simple Steeltoe packages and pushing to PASW will enable lots of cloud native things in App Manager! This lab will show a student how.

### App Manager with Management Actuators

Continuing from the previous lab now that the app is pushed, review the new features found in App Manager.

### Steeltoe Dynamic Logging

Examples of logging within a .NET Core application and how to retrieve those logs in App Manager.

### Next Steps

A "congrats" for completing the workshop and a reminder to continue learning through Pivotal Education's cloud native .NET developer course. There is also a reminder to experience all the examples in the Steeltoe public repo on Pivotal Web Services.

## Config.json with all slides and labs
```json
{
	"workshopSubject":"Steeltoe Cloud Native .NET Microservices Workshop ",
	"workshopHomepage":"",
	"workshopHostname":"steeltoe-basics",
	"modules": [
		{
			"type": "concepts",
			"content": [
				{
					"name":"Advanced Features of Steeltoe",
					"filename":"steeltoe-basics/slides/advanced-steeltoe"
				}
			]
		},
		{
			"type": "demos",
			"content": [
				{
					"name":"Introduction & Credentials",
					"filename":"steeltoe-basics/labs/intro-creds"
				},
				{
					"name":"Setup Environment",
					"filename":"pasw-workshop/labs/setup-environment"
				},
				{
					"name":"Environment Variables",
					"filename":"steeltoe-basics/labs/env-vars"
				},
				{
					"name":"Push Microservice to PASW",
					"filename":"pasw-workshop/labs/push-microservice"
				},
				{
					"name":"User Provided Services with Steeltoe",
					"filename":"steeltoe-basics/labs/cups"
				},
				{
					"name":"Include Steeltoe Actuators and Create a Custom Health Check",
					"filename":"steeltoe-basics/labs/actuators"
				},
				{
					"name":"Push Microservice to PASW",
					"filename":"pasw-workshop/labs/push-microservice"
				},
				{
					"name":"App Manager with Management Actuators",
					"filename":"steeltoe-basics/labs/using-actuators"
				},
				{
					"name":"Steeltoe Dynamic Logging",
					"filename":"steeltoe-basics/labs/logging"
				},
				{
					"name":"Continuing Your .NET Cloud Native Journey",
					"filename":"steeltoe-basics/labs/next-steps"
				}
			]
		}
	]
}
```