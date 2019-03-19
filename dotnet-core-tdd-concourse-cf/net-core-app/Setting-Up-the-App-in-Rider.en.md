**Previous:** [.NET Core App](/#net-core-app)

In this section, we'll describe how to start your first app using [[JetBrains Rider]]. We'll be building a Notes RESTful API where multiple users can add and retrieve notes.

<details>
    <summary>Run <strong>Rider</strong>.</summary>
    <a href="intro-rider-welcome.png" target="_blank">
        ![intro-rider-welcome.png](intro-rider-welcome.png)
    </a>
</details>

***

<details>
    <summary>Click <strong>New Solution</strong> and choose <strong>Empty Solution</strong>. Name it <strong>NotesApp</strong>.</summary>
    <a href="intro-rider-new-solution.png" target="_blank">
        ![intro-rider-new-solution.png](intro-rider-new-solution.png)
    </a>
</details>

***

<details>
    <summary>Right-click on the solution and add a new project.</summary>
    <a href="intro-rider-new-project-menu.png" target="_blank">
        ![intro-rider-new-project-menu.png](intro-rider-new-project-menu.png)
    </a>
</details>

***

<details>
    <summary>Select <strong>ASP.NET Core Web Application</strong> from the <strong>.NET Core</strong> section. Name it <strong>NotesApp</strong>.</summary>
    <a href="intro-rider-new-project.png" target="_blank">
        ![intro-rider-new-project.png](intro-rider-new-project.png)
    </a>
</details>

***

<details>
  <summary>Let's take a look at the generated project in the <strong>Solution Explorer</strong>.</summary>
  <a href="intro-rider-solution-explorer.png" target="_blank">
    ![intro-rider-solution-explorer.png](intro-rider-solution-explorer.png)
  </a>
</details>

***

First is `NotesApp • 1 project`. This is the top-level *solution* that contains one *project*. The solution file is not one that is usually modified manually, but it can be accessed by right-clicking the solution, and going to `Edit > Edit NotesApp.sln` in the **Solution Explorer**.

Next is the `NotesApp` project. A *project* is part of a *solution*. This file may be accessed in a similar fashion to how `NotesApp.sln` is accessed.

In the **Dependencies** section, you'll find **.NETCoreApp 2.2** indicating that we are using .NET Core 2.2 for our project.

The **Packages** section provides a deep-dive into what dependencies we have included in our project. You can also view these by looking at `NotesApp.csproj`.

The `Controllers` directory contains an auto-generated `ValuesController.cs` which we'll be updating/replacing in this tutorial.

The `Properties` directory and the `launchSettings.json` file within are not needed for this guide, and should be deleted.

The `wwwroot` directory is for publishing a static website. It will not be used in this guide, and should be deleted.

The `appsettings.json` file is the default configuration file for the project. `appsettings.Development.json` contains configurations used only in the `Development` environment. You can specify additional configuration files with `appsettings.{environment}.json`.

`Program.cs` contains the Main program, which runs a web server built with our `Startup` class. `Startup.cs` is where we'll define a lot of the setup for our database, our injectable classes, and various Cloud Foundry integrations.

**Git Tag:** [setting-up-the-app](https://github.com/xtreme-steve-elliott/NotesApp/tree/setting-up-the-app)

**Up Next:** [Setting Up xUnit](../Setting-Up-xUnit)
