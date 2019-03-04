# Steeltoe Dynamic Logging

## Goal

xxxxx

## Prerequisites

- Internet Access
- Web Browser (Chrome, Firefox, Edge, Safari)(Not Internet Explorer)

## What is needed in the app, to enable dynaic logging?

1. There are additional features that came pre-configured in the Steeltoe template app. Open the `Program.cs` file by double clicking.

1. Notice the `ConfigureLogging` section within `IWebHostBuilder`. This added the Steeltoe DynamicLogger package to the Microsoft Console logging provider.

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