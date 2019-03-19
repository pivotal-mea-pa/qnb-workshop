**Previous:** [[Adding Other Endpoints]]

### Setup

To start making integration tests for our `NotesController`, I'd suggest making a new **xUnit Unit Test** project called `NotesApp.IntegrationTests`. Use the same versions of **Microsoft.NET.Test.Sdk**, **xunit**, **xunit.runner.visualstudio**, and **FluentAssertions** as the `NotesApp.Tests` project.

For integration tests, we're going to need a test server to to run our endpoints on. Add **Microsoft.AspNetCore.TestHost** as a NuGet dependency to `NotesApp.IntegrationTests`.

Because setup of a test server is going to be very similar across controllers, let's add a utility class for setting everything up. Create a new directory, `Utils`, and make a new class, `HttpClientUtils`, with the following:
```c#
public static class HttpClientUtils
{
    public static HttpClient CreateClient()
    {
        var server = new TestServer(
            new WebHostBuilder()
                .UseStartup<TestStartup>()
        );
        return server.CreateClient();
    }
}
```

`TestStartup` is a class that simply extends `Startup` from `NotesApp`, and allows us to override specific features during testing.

The `HttpClient` return value of `CreateClient` is what will be used for making calls to our endpoints.

Within `Startup` of `NotesApp`, add
```c#
services.AddRouting(opt => opt.LowercaseUrls = true);
```
just above
```c#
services.AddMvc()
```
within `ConfigureServices`.

This gives us the flexibility of not having to use title case URLs when hitting our endpoints.

***

### Writing the Tests

Within `NotesApp.IntegrationTests`, create a new directory, `Controllers`, and add a new class, `NotesControllerTests`.

Add the following to `NotesControllerTests`:
```c#
private const string NotesEndpoint = "/api/notes";
private readonly HttpClient _client;

public NotesControllerTests()
{
    // Staticly imported from HttpClientUtils
    _client = CreateClient();
}
```

Let's begin with a simple example. Add a test that will make sure that calling `PostAsync(Note note)` with an invalid note will return a `BadRequest` status code. The `HttpStatusCode` enum can be quite useful here. You can call to the endpoint using:
```c#
await _client.PostAsJsonAsync<Note>(NotesEndpoint, null);
```

We've tested for the `CreatedAtRoute` in our unit tests, so let's write our integration test now. When the controller returns a `CreatedAtRouteResult`, it puts the location of the `Note` into `response.Headers.Location` in the form of a `URI`. Verify that the path is correct. To retrieve the model from the response, you can use the following:
```c#
var actual = await response.Content.ReadAsAsync<Note>();
```

Add tests for the remaining endpoints and test cases that we covered with our unit tests.

**Git Tag:** [adding-integration-tests](https://github.com/xtreme-steve-elliott/NotesApp/tree/adding-integration-tests)

**Up Next:** [[Adding a Persistence Layer]]
