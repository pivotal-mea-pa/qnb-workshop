# Docker deploy to Cloud Foundry   
In this lab we will learn to deploy the docker image with the reservation-service created [earlier](../boot-docker-demo) on to Cloud Foundry.

## Requirements  
1. Java 8+ JDK Installed
1. Install Docker for Desktop for your platform from <https://www.docker.com/products/docker-desktop>  
1. Install [Cloud Foundry CLI](https://docs.cloudfoundry.org/cf-cli/install-go-cli.html) on your desktop.
1. Create a free account on [PWS](https://pws.pivotal.io) OR your organization's PAS installation.  
1. Configure cf-cli to login to your PAS service by running `cf login`   
1. Create an account on Docker hub and a repository.  
1. Authenticate with docker hub using `docker login`  


## Upload the docker file to docker hub

1. Confirm the image is in your local docker repository
```bash
docker images
```

1. Tag the local image created to match your docker repo name so it gets uploaded there
```bash
docker tag foo/myfirst-boot-on-docker:latest your-docker-username/your-docker-repository:latest
```

1. Deploy docker image to Docker hub
```bash
docker push your-docker-username/your-docker-repository:latest
```

1. Remove the `manifest.yml` file so it does not conflict with the docker deploy.  
```bash
rm manifest.yml
```   

## Deploy docker image to Cloud Foundry  

1. Deploy docker image to Cloud Foundry (make sure Docker support is enabled in CF install).
```bash
cf push uniquename-service --docker-image your-docker-username/your-docker-repository:latest
```

1. Cloud Foundry will pull your docker image from your public repository.  

1. Confirm it is running
```bash
cf apps
```
Copy the fully qualified domain name of the deployed application.

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
