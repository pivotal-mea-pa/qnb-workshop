# Reactive Microservices - Spring Boot2 Performance Harness

This is a sample Spring Boot 2 app to demonstrate the raw performance difference between a Spring Boot 2 app vs a Spring Boot 1 app. 

The request/response flow of the demo is:

**ingress** = user/gatling-test > boot2-load-sample > sample-load-target

**egress** = sample-load-target > boot2-load-sample > user/gatling-test 

1. Open a terminal and clone workshop demo github repo: <https://github.com/Pivotal-Field-Engineering/pace-cnd-java>

1. Change to the "boot2-load-demo" folder

```
    cd ./boot2-load-demo
```

## Backing Service

```bash
./gradlew -p applications/sample-load-target clean bootRun
```

## Spring Boot 2 based app:

### Run the Spring Boot 2 based app:

```bash
./gradlew -p applications/boot2-load-sample clean bootRun
```

### Call target endpoint

Assuming that [httpie](https://httpie.org/) is installed

```bash
http POST 'http://localhost:8082/passthrough/messages' id="1" payload="one"   delay="1000"
```

OR with CURL

```bash
curl -X "POST" "http://localhost:8082/passthrough/messages" \
     -H "Accept: application/json" \
     -H "Content-Type: application/json" \
     -d $'{
  "id": "1",
  "payload": "one",
  "delay": "1000"
}'
```


## Spring Boot 1 based app:

### Run the Spring Boot 1 based app:

```bash
./gradlew -p applications/boot1-load-sample clean bootRun
```

### Call target endpoint

Assuming that [httpie](https://httpie.org/) is installed

```bash
http POST 'http://localhost:8081/passthrough/messages' id="1" payload="one"   delay="1000"
```

OR with CURL

```bash
curl -X "POST" "http://localhost:8081/passthrough/messages" \
     -H "Accept: application/json" \
     -H "Content-Type: application/json" \
     -d $'{
  "id": "1",
  "payload": "one",
  "delay": "1000"
}'
```

## Run Load tests

Run the following tests and each time increase SIM_USERS and compare Gatling Reports.  Observe at some point the Spring Boot 1 apps (Tomcat) performance lags behind Spring Boot 2 (Netty) significantly.

### Against Boot 2 version of the app

```bash
./gradlew -p applications/load-scripts  -DTARGET_URL=http://localhost:8082 -DSIM_USERS=300 gatlingRun
```

### Against Boot 1 version of the app

```bash
./gradlew -p applications/load-scripts  -DTARGET_URL=http://localhost:8081 -DSIM_USERS=300 gatlingRun
```
