**Previous:** [Adding Integration Tests](../adding-integration-tests)

In this section we'll integrate MySQL with the app.

### Adding mySQL

**For Linux/Mac OS X:**

Install and start `mysql` onto your local machine:
```bash
brew install mysql
brew services start mysql
```
You may want to set a password for `mysql` via the following:
```bash
$(brew --prefix mysql)/bin/mysqladmin -u root password NEWPASS
```
where `NEWPASS` is the password you want it to be. For the following, I've just set it to `password`.

**For Windows:**

Please refer to [Installing MySQL on Microsoft Windows](https://dev.mysql.com/doc/refman/8.0/en/windows-installation.html)

***

For our project, we'll be using [Pomelo.EntityFrameworkCore.MySql](https://github.com/PomeloFoundation/Pomelo.EntityFrameworkCore.MySql) to integrate with a mysql db.

<details>
  <summary>Add the <b>Pomelo.EntityFrameworkCore.MySql</b> from NuGet to <b>NotesApp</b> project.</summary>
  <a href="../Adding-a-Persistence-Layer/pomelo-dependency.png" target="_blank">
    ![pomelo-dependency.png](../Adding-a-Persistence-Layer/pomelo-dependency.png)
  </a>
</details>

***

The current implementation of `Startup` makes use of an in-memory databse. We're going to replace that with the use of an actual database, and make the integration tests use the in-memory database. For the integration tests, simply override `ConfigureDbOptions` in `TestStartup.cs` and use the in-memory database.

To simplify connecting to databases, make use of [Steeltoe](http://steeltoe.io/)'s database connectors.

<details>
    <summary>Add dependencies for <b>Steeltoe.CloudFoundry.ConnectorCore</b> and <b>Steeltoe.CloudFoundry.Connector.EFCore</b></summary>
    <a href="../Adding-a-Persistence-Layer/steeltoe-mysql-connector.png" target="_blank">
        ![steeltoe-mysql-connector.png](../Adding-a-Persistence-Layer/steeltoe-mysql-connector.png)
    </a>
</details>

***

In `Program.cs`, add the following in place of `.UseStartup<Startup>();`:
```c#
// Tells it where to start looking from for files.
.UseContentRoot(Directory.GetCurrentDirectory())
.UseStartup<Startup>()
// Adding extra options
.ConfigureAppConfiguration((builderContext, config) =>
{
    config
        .SetBasePath(builderContext.HostingEnvironment.ContentRootPath)
        // Tells the app that it needs appsettings.json.
        .AddJsonFile("appsettings.json", false, true)
        // Tells the app that if available, also apply the settings from the matching environment based settings file.
        .AddJsonFile($"appsettings.{builderContext.HostingEnvironment.EnvironmentName}.json", true);
});
```

***

In `Startup.cs`, within `ConfigureDbOptions`, replace
```c#
options.UseInMemoryDatabase("notes_app");
```
with
```c#
options.UseMySql(Configuration);
```

**Note:** You may need to manually add
```c#
using Steeltoe.CloudFoundry.Connector.MySql.EFCore;
```
in case it does not suggest it to you.

The Steeltoe connector allows you to configure your local credentials in `appsettings.Development.json`.
```json
"mysql": {
    "client" : {
        "database": "notes_app",
        "username": "root",
        "password": "password"
    }
}
```
Replace the information with what your credentials. Run the app, and make an api call to `http://localhost:5000/api/notes`. You should see an empty array:
```json
[]
```

In a terminal, launch into MySQL and explore the table:
```
mysql> use notes_app;
Database changed

mysql> show tables;
+---------------------+
| Tables_in_notes_app |
+---------------------+
| Notes               |
+---------------------+
1 row in set (0.00 sec)

mysql> select * from Notes;
Empty set (0.00 sec)
```

As you can see, a database and table has automatically been added.

If we had not made the integration tests use an in-memory database, we would see our tests start failing after some time, as they would not be cleaning up after themselves and would end up not matching our expected values.

**Git Tag:** [adding-a-persistence-layer](https://github.com/xtreme-steve-elliott/NotesApp/tree/adding-a-persistence-layer)

**Up Next:** [Continuous Integration / Deployment](/workshop/#continuous-integration--deployment)
