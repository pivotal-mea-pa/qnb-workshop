## Hystrix

1. Provision a hystrix dashboard, the standard plan, named `hystrix` from the marketplace.

    ```
        cf create-service p-circuit-breaker-dashboard standard hystrix
    ```
1. Uncomment the `hystrix` line in the `manifest.yml` file of both FunnyQuotesServicesOwin & FunnyQuotesUICore projects.

1. Push both applications.

    ```
        > cd FunnyQuotesServicesOwin
        > cf push FunnyQuotesServicesOwin -s windows2016 -b hwc_buildpack
    ```
    ```
        > cd FunnyQuotesUICore
        > cf push FunnyQuotesUICore
    ```

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

## Show code
1. Open FunnyQuotesUICore.Clients.RestFunnyQuotesClient.
	1. Note how commands are created.
	1. Thread isolation based on circuit group name to avoid thread starvation scenario.
	1. Because execution happens on different set of threads, access to `HttpContext` must be properly marshalled. See use of `app.UseHystrixRequestContext()` in Startup.cs to do this.
1. See `services.AddHystrixCommand<RestFunnyQuotesClient.GetQuoteCommand>("Core.RandomQuote", "Core.RandomQuote", Configuration);` to register commands into container.
1. See `services.AddHystrixMetricsStream(Configuration);` and `app.UseHystrixMetricsStream();` to publish metrics to dashboard.
