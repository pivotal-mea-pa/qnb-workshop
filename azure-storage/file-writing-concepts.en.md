# Cloud Native Principles and Files

## Review Cloud Native Principles

## File Integration Patterns

Writing Files to local file system is a Cloud Native anti-pattern, but sometimes you just got to for any number of reasons.

## Alternatives

Although 12-factor and Cloud Native principles discourage writing to the local file system, you can utilize cloud storage for apps that need to implement file integration patterns.

### Blob storage

Blob storage is the most obvious answer and this guide will demonstrate how to utilize Azure blob storage to attach and write a file.

### SMB

https://docs.microsoft.com/en-us/azure/storage/files/storage-how-to-use-files-windows

### Spring Integration

https://docs.spring.io/spring-integration/docs/4.3.11.RELEASE/reference/html/files.html#file-timestamps

## Summary

To utilize file storage for cloud applications you have several choices: Blob storage, SMB and Spring Integration.  In this demonstration, we show how to use Spring Boot, Azure Storage Service Broker and User-Provided Ser vice. 
