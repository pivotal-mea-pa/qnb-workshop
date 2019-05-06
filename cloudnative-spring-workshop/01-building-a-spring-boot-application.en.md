# Building a Spring Boot Application

In this lab we'll build and deploy a simple [Spring Boot](https://docs.spring.io/spring-boot/docs/current/reference/htmlsingle) application to Cloud Foundry whose sole purpose is to reply with
a standard greeting.

## Getting started

While we could visit
https://start.spring.io to create a new Spring Boot project, we will
start with a skeleton . Open a Terminal (e.g., `cmd` or `bash`
shell). Change the working directory to be
`devops-workshop/labs/my_work/cloud-native-spring`

```bash
cd devops-workshop/labs/my_work/cloud-native-spring
```

Open this project in your editor/IDE of choice.

<details>
<summary>_STS/Eclipse Import with Gradle Help_</summary>

<ol><li>Select _File
&gt; Import…_</li>
<li>In the subsequent dialog choose _Gradle &gt; Existing
Gradle Project_ then click the _Next_ button.</li>
<li>In the _Import Gradle
Project_ dialog browse to the _cloud-native-spring_ directory (e.g.
`cloud-native-spring/labs/my_work/cloud-native-spring`) then click the
_Open_ button, then click the _Finish_ button.</li>
</ol>
</details>

<details>
<summary>_STS/Eclipse Import with Maven Help_</summary>

<ol><li>Select _File &gt; Import…_</li>

<li>In the subsequent dialog choose
_Maven &gt; Existing Maven Project_ then click the _Next_ button.</li>

<li>In
the _Import Maven Project_ dialog browse to the
_cloud-native-spring_ directory (e.g.
`cloud-native-spring/labs/my_work/cloud-native-spring`) then click the
_Open_ button, then click the _Finish_ button.</li>
</details>

## Add an Endpoint
Within your editor/IDE complete the following steps:
* Create a new
package `io.pivotal.controller_ underneath _src/main/java`.
* Create
a new class named `GreetingController` in the aforementioned package.
* Add an `@RestController` annotation to the class
`io.pivotal.controller.GreetingController` (i.e.,
_/cloud-native-spring/src/main/java/io/pivotal/controller/GreetingController.java_).

```java
package io.pivotal.controller;

import org.springframework.web.bind.annotation.RestController;

@RestController
public class GreetingController {

}
```

Add the following request handler to the class
`io.pivotal.controller.GreetingController` (i.e.,
_/cloud-native-spring/src/main/java/io/pivotal/controller/GreetingController.java_).

```java
@GetMapping("/")
public String hello() {
  return "Hello World!";
}
```
Completed:

```java
package io.pivotal.controller;

import org.springframework.web.bind.annotation.RestController;
import org.springframework.web.bind.annotation.GetMapping;

@RestController
public class GreetingController {
  @GetMapping("/hello")
  public String hello() {
    return "Hello World!";
  }
}
```

## Build the _cloud-native-spring_ application

Return to the Terminal session you opened previously and make sure your working directory is
set to be `cloud-native-spring/labs/my_work/cloud-native-spring`

First
we'll run tests
```bash
mvn test
```
Next we'll package the application as an
executable jar
```bash
mvn package
```

## Run the _cloud-native-spring_ application

Now we're ready to run the application . Run the application
with:
```bash
mvn spring-boot:run
```
You should see the application start up an
embedded Apache Tomcat server on port 8080 (review terminal output):

```bash
2018-08-22 17:40:18.193 INFO 92704 --- \[ main\]
o.s.b.w.embedded.tomcat.TomcatWebServer : Tomcat started on port(s):
8080 (http) with context path '' 2018-08-22 17:40:18.199 INFO 92704 ---
\[ main\] i.p.CloudNativeSpringUiApplication : Started
CloudNativeSpringUiApplication in 7.014 seconds (JVM running for 7.814)
```
Browse to http://localhost:8080/hello

Stop the _cloud-native-spring_
application. In the terminal window type `Ctrl + C`

## Deploy _cloud-native-spring_ to Pivotal Cloud Foundry

We've built and run the
application locally. Now we'll deploy it to Cloud Foundry.

Create an
application manifest in the root folder
_devops-workshop/labs/my\_work/cloud-native-spring_

```bash
touch manifest.yml
```
Add application metadata, using a text editor (of choice)
```yaml
--- applications:
  - name: cloud-native-spring
  random-route: true
  instances: 1
  path: ./target/cloud-native-spring-0.0.1-SNAPSHOT.jar
  buildpacks:
  - java\_buildpack\_offline
  stack: cflinuxfs3
  env:
    JAVA_OPTS: -Djava.security.egd=file:///dev/urandom
```

The above manifest entries will work with Java Buildpack 4.x series and
JDK 8. If you built the app with JDK 11 and want to deploy it you will
need to make an additional entry in your manifest, just below `JAVA_OPTS`, add:
```yaml
---
  JBP\_CONFIG\_OPEN\_JDK\_JRE: '{ jre: { version: 11.+ } }'
```

Push application into Cloud Foundry
```bash
cf push
```
-&gt; To specify an
alternate manifest and buildpack, you could update the above to be:
```bash
cf push -f manifest.yml -b java_buildpack
```
Assuming the offline
buildpack was installed and available for use with your targeted
foundation. You can check for which buildpacks are available by
executing
```bash
cf buildpacks
```
Find the URL created for your app in the
health status report or issue the following command to see your running apps.

```bash
cf apps
```

Browse to your app's `/hello` endpoint

Check the
log output:

```bash
cf logs cloud-native-spring --recent
```
<b>Congratulations!</b>
You’ve just completed your first Spring Boot application.
