# Steeltoe in .NET

Workshop highlighting how to use [steeltoe](https://steeltoe.io) in your .net application

### 1. Create new visual studio Project (MVC and API), this can be the same project used in Lab01

### 2. Add the following Nuget Packages:
Steeltoe.Extensions.Logging.DynamicLogger
Steeltoe.Management.EndpointWeb

### 3. Add the following files under app_start:
LoggingConfig.cs
ApplicationConfig.cs file



### 4. update Application_Start in global.asax and the below

		// Create applications configuration
            ApplicationConfig.Configure("development");


            // Create logging system using configuration
            LoggingConfig.Configure(ApplicationConfig.Configuration);
            
            // Add management endpoints to application
            ManagementConfig.ConfigureManagementActuators(
                
                ApplicationConfig.Configuration,
                LoggingConfig.LoggerProvider,
                GlobalConfiguration.Configuration.Services.GetApiExplorer(),
                LoggingConfig.LoggerFactory);

            // Start the management endpoints
            ManagementConfig.Start();

			 protected void Application_Stop()
        {
            ManagementConfig.Stop();
        }

### 5. Update References in global.asax
```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Web;
using System.Web.Http;
using System.Web.Mvc;
using System.Web.Optimization;
using System.Web.Routing;
```

### 6. in Home cotroller add the following:

```csharp
 private ILogger<HomeController> _logger;


 public HomeController()
        {
            _logger = LoggingConfig.LoggerFactory.CreateLogger<HomeController>();
        }

		public ActionResult Index()
        {
            var minlvl = GetMinLogLevel(_logger);
            Console.WriteLine($"Minimum level set on _logger: {minlvl}");
            Debug.WriteLine($"Minimum level set on _logger: {minlvl}");
            _logger.LogTrace("This is a {LogLevel} log", LogLevel.Trace.ToString());
            _logger.LogDebug("This is a {LogLevel} log", LogLevel.Debug.ToString());
            _logger.LogInformation("This is a {LogLevel} log", LogLevel.Information.ToString());
            _logger.LogWarning("This is a {LogLevel} log", LogLevel.Warning.ToString());
            _logger.LogError("This is a {LogLevel} log", LogLevel.Error.ToString());
            _logger.LogCritical("This is a {LogLevel} log", LogLevel.Critical.ToString());
            return View();
            
        }
```csharp

### 7. cf push as described in Lab01

### 8. Once you Login to app manager, you will notice the following
- a. Health Check-end point
![Publish Menu Option](lab02-healthcheck.PNG)

- b. Logging Levels: Ability to configure logging levels 
![Publish Menu Option](lab02-LoggingLevels.PNG)

- b. Tracing and Thread Information
![Publish Menu Option](lab02-TraceAndThread.PNG)

