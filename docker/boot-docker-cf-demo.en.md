# Deploy Docker container to Cloud Foundry
In this lab we will learn to run a Spring MVC app using a embedded H2 database on Docker

## Requirements
1. Install Docker for Desktop for your platform from <https://www.docker.com/products/docker-desktop>
1. Git Clone or download reservation-demo repo at <https://github.com/Pivotal-Field-Engineering/reservation-demo>
1. Install [Cloud Foundry CLI](https://docs.cloudfoundry.org/cf-cli/install-go-cli.html) on your desktop.
1. Create a free account on [PWS](https://pws.pivotal.io) OR your organization's PAS service.
1. Configure cf-cli to login to your PAS service.
1. Create an account on Docker hub and a repository.
1. Authenticate with docker hub using ```docker login```

## Build the reservation-demo project  

1. Open a terminal in the `reservation-service` directory of the project and create the deployment artifact  
```
./mvnw package
```
1. Create a file a text file called Dockerfile in the same directory and copy the following text to it and save it:  
```
FROM openjdk:8-jdk-alpine
COPY target/reservation-service-0.0.1-SNAPSHOT.jar app.jar
ENTRYPOINT ["/usr/bin/java","-jar","/app.jar"]
```
1. Run the command below to create docker image using the build artifact.
```
docker build --rm -t foo/myfirst-boot-on-docker:latest .
```
1. Confirm the image is in your local docker repository
```
docker images
```
1. Tag the local image created to match your docker repo name so it gets uploaded there
```
docker tag foo/myfirst-boot-on-docker:latest your-docker-username/your-docker-repository:latest
```
1. Deploy docker image to Docker hub
```
docker push your-docker-username/your-docker-repository:latest
```
1. Deploy docker image to Cloud Foundry (make sure Docker support is enabled in CF install).
```
cf push uniquename-service --docker-image your-docker-username/your-docker-repository:latest
```
1. Cloud Foundry will pull your docker image from your public repository.  

1. Confirm it is running
```
cf apps
```
1. Let's save some data to the server using curl commands
```
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
```
curl --request GET \
  --url http://uniquename-service.your-cf-domain.com/reservations
```
