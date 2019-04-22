## Circuit Breaker - Hystrix
1. Access Hystrix dashboard.
	Apps Manager > Space > Services > Hystrix > Manage.
	
1. Access FunnyQuotesUICore GUI and request a few quotes.
1. Note dashboard and successful counts.
1. Stop backend FunnyQuotesServiceOwin.
1. Request quotes from GUI.
	1. Observe low latency fallback to default message.
	1. Note dashboard failed counts (red).
1. Keep requesting quotes until circuit is triggered.
	1. Note fallback requests in open circuit (blue).

## Highlights
1. Open FunnyQuotesUICore.Clients.RestFunnyQuotesClient.
	1. Note how commands are created.
	1. Thread isolation based on circuit group name to avoid thread starvation scenario.
	1. Because execution happens on different set of threads, access to `HttpContext` must be properly marshalled. See use of `app.UseHystrixRequestContext()` in Startup.cs to do this.
1. See `services.AddHystrixCommand<RestFunnyQuotesClient.GetQuoteCommand>("Core.RandomQuote", "Core.RandomQuote", Configuration);` to register commands into container.
1. See `services.AddHystrixMetricsStream(Configuration);` and `app.UseHystrixMetricsStream();` to publish metrics to dashboard.
