# Steeltoe Dynamic Logging

## Goal

Assuming you have already pushed an app based on the Steeltoe Cloud Foundry templates, this lab will review what additions were made in the project to enable dynamic logging and the automatic benefits in App Manager.

## Prerequisites

- Visual Studio (min 2015)
- Internet Access
- Web Browser (Chrome, Firefox, Edge, Safari)(Not Internet Explorer)

## Enable Steeltoe dynamic logging

1. There are additional features that came pre-configured in the Steeltoe template app. Open the `Program.cs` file by double clicking.

1. Notice the `ConfigureLogging` section within `IWebHostBuilder` has an added Steeltoe DynamicLogger package to the Microsoft Console logging provider.
  ```cs
  .ConfigureLogging((builderContext, loggingBuilder) => {
    // Add Steeltoe Dynamic Logging provider
    loggingBuilder.AddDynamicConsole();
  })
  ```

1. Open the `ValuesController.cs` file in the "Controllers" folder, by double clicking. Note the custom log entries being written.
  ```cs
  _logger.LogInformation("Hi There");
  ```

  You can write log entries with no special packages, using the C# `Microsoft.Extensions.Logging.ILogger` package. There are options like logging Critical, Error, Warning, Info entries. Because Steeltoe DynamicLogger has been enabled to monitor these custom app logs, everything is written within App Manager and Metrics for review - automatically!

## Discover and use dynamic logging in App Manager

1. Open the App Manager website in a browser.

1. Navigate to your app's home page.

1. Now click the `Logs` tab at the top of the panel.

1. Adjust the logging level by clicking the "Configure Logging Levels" button. This window gives you the ability to turn logging verbosity up or down, depending on the package.
  ![App Logging Levels](a_logging_levels.PNG)

  In the above screen shot, logging was enabled for the "Microsoft.AspNetCore.Mvc" package. Now as new requests are made, you can see deeper log detail with every request about how the route template was calculated - see the log entry `Route matched with {action = "Get", controller = "Values"}.` in the below image.
  ![App Log](a_mvc_log.PNG)

## Complete

Dynamic logging is a very powerful tool while in development and even more powerful for production. Now you can turn things up when there's a issue and consume those messages not only in App Manager but also in whatever logging platform the foundation has been provided (splunk, etc). Then turn it back down when everything is ok.