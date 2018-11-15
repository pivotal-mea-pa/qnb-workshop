#  Middlware upgrade - Java Buildpack

### Prerequisites

1. Make sure that you complete "Prerequisites" in "Platform Overview" demo
2. Run "Platform Overview" demo

### Re-stage Spring Boot application to "development" 

1. In "development" console tab 

```
    paap upgrade-middleware
```

2. Open a new console tab and start a "development" environment smoke test

```
    paap run-smoketest
```

The output response of this smoketest end point is the JDK version:

```
    {"response":"1.8.0_121"}
    {"response":"1.8.0_121"}
    {"response":"1.8.0_121"}
    {"response":"1.8.0_121"}
```

3. Verify that the JDK version is **1.8.0_121**. After **#1** completes, verify that the JDK version
changes to **1.8.0_162** with no smoke test errors.

```
    {"response":"1.8.0_162"}
    {"response":"1.8.0_162"}
    {"response":"1.8.0_162"}
```

4. Show the console commands and output for **#1**. The "development" and "production"
spaces should have different versions of the java buildpack deployed.

```
    ----------------------------------------------------------------------
    COMMAND: cf buildpack-usage -b java_buildpack
    
    Checking which apps use buildpack java_buildpack ...
    
    OK
    
    org    space        application
    demo   production   springboot-app
    demo   production   springboot-app_
    
    ----------------------------------------------------------------------
    COMMAND: cf buildpack-usage -b java_buildpack-v48
    
    Checking which apps use buildpack java_buildpack-v48 ...
    
    OK
    
    org    space         application
    demo   development   springboot-app
    demo   development   springboot-app_
    
    ----------------------------------------------------------------------
```


5. Run "teardown" command to delete the org and undo middleware upgrade

```
    paap teardown
```

```
    COMMAND: cf delete-org -f $CF_ORG
    
    Deleting org demo as admin...
    OK
    
    ----------------------------------------------------------------------
    COMMAND: cf delete-buildpack -f java_buildpack-v48
    
    Deleting buildpack java_buildpack-v48...
    OK
    
    ----------------------------------------------------------------------
```