## Actuators
1. Launch FunnyQuotesUICore locally
1. Access http://localhost:60421/cloudfoundryapplication and show different actuator endpoints and data within them
1. Hit the same endpoint on PCF to highlight that it's secured when running on the platform
1. Open up apps manager. (Ensure that app has been opened at least once over https if using self signed certs).
	1. Highlight special Steeltoe icon (actuators were detected in the app)
	1. Highlight health check for each instance
	1. Show dynamic log levels
	1. Show request tracing

## Metrics
1. View the Metrics link of the app. Highlight the following:
	1. Dashboarding & ability to add charts for metrics
	1. Events chart
	1. Pulling up logs over timeline
	1. Different sources of logs (RTR/APP)
1. Find a log for a router logs for quote requests  (search for `GET /Home/GetQuote` keyword). Access distributed tracing. Highlight the Gantt chart. 
1. Note: Automatic log correlation only works in .NET core apps, but you can get Gantt charts out of the box. Will be added in future version of Steeltoe. Manual correlation can be done right now by prefixing all logs with traceid/spanid in incoming headers

## Highlight Code
1. Adding actuators and enabling them in code. See startup.cs or global.asmx.cs and search for `Actuator`. It has to be registered and activated.
1. When using legacy stack with autofac, adding actuators automatically registers dynamic console logger which is needed to change log levels at runtime
	1. For .NET core app, open up Program.cs and highlight explicit registration of dynamic console
1. Show  `services.AddDistributedTracing(Configuration);` and how it automatically appends headers to any `HttpClient` invocations (.NET core only)
