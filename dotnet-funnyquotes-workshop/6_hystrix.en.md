## Hystrix
1. Create hystrix dashboard on PCF marketplace.
1. Bind to FunnyQuotesUICore & restart.
1. Access Hystrix dashboard (service > manage from App Manager).
1. Access FunnyQuotesUICore GUI and request a few quotes.
1. Show dashboard and successful counts.
1. Stop backend FunnyQuotesServiceOwin.
1. Request quotes from GUI.
	1. Highlight low latency fallback to default message.
	1. Show dashboard failed counts (red).
1. Keep requesting quotes until circuit is triggered.
	1. Highlight fallback requests in open circuit (blue).

## Show code
1. Open FunnyQuotesUICore.Clients.RestFunnyQuotesClient.
	1. Highlight how commands are created.
	1. Explain thread isolation based on circuit group name to avoid thread starvation scenario.
	1. Because execution happens on different set of threads, access to `HttpContext` must be properly marshalled. Show use of `app.UseHystrixRequestContext()` in Startup.cs to do this.
1. Show `services.AddHystrixCommand<RestFunnyQuotesClient.GetQuoteCommand>("Core.RandomQuote", "Core.RandomQuote", Configuration);` to register commands into container.
1. Show `services.AddHystrixMetricsStream(Configuration);` and `app.UseHystrixMetricsStream();` to publish metrics to dashboard.
