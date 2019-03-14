# Consume the Microsoft Azure Service Broker in your application

## Goal

With the Azure SQL service available for binding, create the necessary DBContext and list the newly created tables' content.

## Prerequisites

- Visual Studio (min 2015)
- Internet Access

## Bind the SQL service to your application

1. Back in Visual Studio, open the `manifest.yml` file by double clicking and add the following to the services section (create that section if it's not there).
  ```yml
  
    ...

    services:
      - my-azure-db
  ```

## Add SQL DBContext

1. Open `Program.cs` and confirm the Cloud Foundry provider has been added. This provider gives your app the ability to locate the VCAP environment variables that are created by Cloud Foundry, and parse them.
  ```cs
  WebHost.CreateDefaultBuilder(args)
    ...
    .UseCloudFoundryHosting()
    .AddCloudFoundry()
    .UseStartup<Startup>()
    ...
  ```

1. You may need to add the dependency
  ```cs
  using Steeltoe.Extensions.Configuration.CloudFoundry;
  ```

1. Create a new class file named `TestContext.cs` and replace the default class with the following code.
  ```cs
  using Microsoft.EntityFrameworkCore;
  using System.ComponentModel.DataAnnotations;
  using System.ComponentModel.DataAnnotations.Schema;
  ```
  ```cs
  public class TestContext : DbContext {
    public TestContext(DbContextOptions options) : base(options){ }

    public DbSet<TestData> TestData { get; set; }
  }

  public class TestData {
    [Key]
    [DatabaseGenerated(DatabaseGeneratedOption.Identity)]
    public int Id { get; set; }

    public string Data { get; set; }
  }
  ```

1. Create a new class file named `SampleData.cs` and replace the default class with the following code. This will initialize the a database table using the TestData schema and fill it with 2 rows of data, if none exists.
  ```cs
  using Microsoft.EntityFrameworkCore;
  using Microsoft.Extensions.DependencyInjection;
  using System;
  using System.Linq;
  ```
  ```cs
  public class SampleData {
    internal static void InitializeMyContexts(IServiceProvider serviceProvider) {
      if (serviceProvider == null) {
        throw new ArgumentNullException("serviceProvider");
      }
      using (var serviceScope = serviceProvider.GetRequiredService<IServiceScopeFactory>().CreateScope()) {
        var db = serviceScope.ServiceProvider.GetService<TestContext>();
        db.Database.EnsureCreated();
      }
      InitializeContext(serviceProvider);
    }

    private static void InitializeContext(IServiceProvider serviceProvider) {
      using (var serviceScope = serviceProvider.GetRequiredService<IServiceScopeFactory>().CreateScope()) {
        var db = serviceScope.ServiceProvider.GetService<TestContext>();
        if (db.TestData.Any()) {
            return;
        }

        AddData<TestData>(db, new TestData() { Data = "Test Data 1 - TestContext " });
        AddData<TestData>(db, new TestData() { Data = "Test Data 2 - TestContext " });
        db.SaveChanges();
      }
    }

    private static void AddData<TData>(DbContext db, object item) where TData: class {
      db.Entry(item).State = EntityState.Added;
    }
  }
  ```

1. Locate the `Controllers\ValuesController.cs` file and double click it, to open. Find the `[HttpGet]` endpoint and Replace the method logic with the following.
  ```cs
  [HttpGet]
  public ActionResult<IEnumerable<TestData>> Get([FromServices] TestContext context) {
    var connection = context.Database.GetDbConnection();
    return context.TestData.ToList();
  }
  ```

1. You may need to add the dependency
  ```cs
  using System.Collections.Generic;
  ```

1. Open `Startup.cs` and locate the `ConfigureServices` method. Add our newly created database context.
  ```cs
  public void ConfigureServices(IServiceCollection services) {
    ...

    services.AddDbContext<TestContext>(options => options.UseSqlServer(Configuration));
    services.AddMvc();
  }
  ```

1. You may need to add the dependency
  ```cs
  using Steeltoe.CloudFoundry.Connector.SqlServer.EFCore;
  ```

## Complete

The app is ready for prime time. You have retrieved the serivce information (with connection info) from the provided environment variables, create a database context, initialized the data store, and made querying available via RESTFul endpoint.