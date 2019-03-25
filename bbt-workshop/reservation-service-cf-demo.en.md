# Spring Boot on Cloud Foundry
In this lab we will learn how to deploy to Cloud Foundry

## Requirements    
1. Java 8+ JDK Installed  
1. Install [Cloud Foundry CLI](https://docs.cloudfoundry.org/cf-cli/install-go-cli.html) on your desktop.
1. Create a free account on [PWS](https://pws.pivotal.io) OR your organization's PAS installation.  
1. Configure cf-cli to login to your PAS service by running `cf login`  

## Build the reservation-service  

1. Git Clone or download reservation-demo repo at <https://github.com/Pivotal-Field-Engineering/reservation-demo>  

1. Open a terminal in the `reservation-service` directory of the project

1. Build the `reservation-service`
```bash
./mvnw package
```

1. Deploy the application to Cloud Foundry
```bash
cf push
```
This will read the `mainfest.yml` located in the directory and use that metadata for the application.

1. Confirm it is running
```bash
cf apps
```
Copy the fully qaualified domain name of the deployed application.   

1. Let's save some data to the server using curl commands
```bash
curl --request POST \
  --url http://uniquename-service.your-cf-domain.com/reservations \
  --header 'content-type: application/json' \
  --data '[{
	"id": null,
	"name": "Milan",
	"status": "booked"
}]
'
```

1. Confirm data is saved
```bash
curl --request GET \
  --url http://uniquename-service.your-cf-domain.com/reservations
```
## Build the reservation-ui  

1. Open the code in your favorite IDE.   
1. We need to modify the URL that this project uses to connect to the reservation-service since it is using http://localhost:8080. Find the place in the code to change it to use the reservation-service we deployed earlier.    

1. After making the change, build and deploy the application to Cloud Foundry
```bash
./mvnw package
cf push
```  
This will read the `mainfest.yml` located in the directory and use that metadata for the application.

1. Confirm it is running using `cf apps` command. Open a browser where the reservation-ui is running.  
