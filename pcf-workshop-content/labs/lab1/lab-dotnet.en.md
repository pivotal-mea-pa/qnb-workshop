Target the Environment
======================

1.  Follow the instructions to [Get Environment Access](../README.adoc)

Push It!
========

1.  Change to the *DotNet-cf-sample-app* sample application directory:

        $ cd $BOOTCAMP_HOME/dotnet-cf-sample-app

2.  Push the application!

        $ cf push

Add a Database service
======================

1.  Bind the mysql service you previously added to the Java application
    using the apps manager interface

Notice that the data is the same as the java app

1.  Make a change to the data by deleting from the java app — notice the
    data is updated in the .NET app.

View the .NET environment
=========================

1.  Go to the secret endpoint in your app: /?all=

You can see the Windows container and app environment variables

On to the next Lab!
===================

[Lab2 - Binding to Cloud Foundry Services](/demos/binding-cf-services)
