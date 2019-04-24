# Spring Boot Azure Starter Storage

## Spring Initializr

Use [Spring Boot Initializr](https://start.spring.io/) to create a skeleton spring boot application with Web dependency.

Choose whether you want a maven or gradle project.  I am using Java, Spring Boot 2.14.

Set your Project Metadata

Group: io.pivotal (use your organization)

Artifact: azure-storage-demo

Description: Demo project for using Azure Storage from Spring Boot

We will use a Jar for packaging and Java version 8

![alt text][initializr-storage-demo]

Make sure you include the Web dependency

![alt text][initializr-web-dependency]

[initializr-storage-demo]: images/initializr-storage-demo.png "Storage demo"

[initializr-web-dependency]: images/initializr-web-dependency.png "Web dependency"

Generate the project and use your favorite IDE or code editor for the remainder of the steps.
