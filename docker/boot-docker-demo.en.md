# Spring Boot and Docker
In this lab we will learn to run a Spring MVC app using a embedded H2 database on Docker

## Requirements  
1. Java 8+ JDK Installed  
1. Install Docker for Desktop for your platform from <https://www.docker.com/products/docker-desktop>  


## Download the reservation-demo project

1. Git Clone or download reservation-demo repo at <https://github.com/Pivotal-Field-Engineering/reservation-demo>

1. Open a terminal in the `reservation-service` directory of the project and create the deployment artifact
```bash
./mvnw package
```
1. Create a file a text file called Dockerfile in the same directory and copy the following text to it and save it:
```
FROM openjdk:8-jdk-alpine
COPY target/reservation-service-0.0.1-SNAPSHOT.jar app.jar
ENTRYPOINT ["/usr/bin/java","-jar","/app.jar"]
```
1. Run the command below to create docker image using the build artifact.
```bash
docker build --rm -t foo/myfirst-boot-on-docker:latest .
```
1. Confirm the image is in your local docker repository
```bash
docker images
```
1. Run the docker image in a container
```bash
docker run -d -p 8080:8080 foo/myfirst-boot-on-docker:latest
```
1. Confirm it is running
```bash
docker ps
```
1. Let's save some data to the server using curl commands
```bash
curl --request POST \
  --url http://localhost:8080/reservations \
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
  --url http://localhost:8080/reservations
```
1. Open a terminal in the `reservation-ui` project. Run the UI
```bash
spring-boot:run
```
1. Open a browser <http://localhost:8081> to see the UI.   
1. Do some more bookings and refresh the UI to see the bookings.

1. Stop the docker container so it frees up the port. Get the container id using `docker ps` above   
```bash
docker stop <container-id-from-docker-ps>
```
