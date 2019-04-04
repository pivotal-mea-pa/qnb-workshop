# .NET Core TDD/Concourse/CF Rampup

The intent of these materials is to bring one up to speed on writing basic Web APIs in .NET Core while following the practices of TDD. Concourse is used as a CI Pipeline and Cloud Foundry as cloud hosting. The latter two aspects come after the application and tests (except smoke tests) have been written.

## Pre-requisites

### .NET Core SDK / dotnet CLI

You'll either need the ability to install these tools, or already have them on your machine. The guide will walk through where to obtain them. More information can be found [here](development-tools/sdk-for-.net-core-and-dotnet-cli.en.md).

### NuGet Access

Your machine will either need access to public NuGet or your private NuGet server will need to have access to the dependencies outlined in [Libraries Used](#libraries-used).

### Cloud Foundry

You'll need either the ability to install Cloud Foundry Dev locally on your machine or the following from an existing Cloud Foundry foundation:

- **API URL** of the foundation
- **User name** and **password** to login to the foundation
- **Org** and **Space** associated with the user account

You can find more information [here](development-tools/cf-dev.en.md)

### Concourse

If you plan on following the Concourse portions of this guide, you'll either need an existing Concourse pipeline, or Docker for deploying one on your local machine. You can find out more [here](development-tools/deploy-concourse.en.md).

## Libraries Used

### Overall

- [Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App/) ([GitHub](https://github.com/aspnet/AspNetCore))
- [Pomelo.EntityFrameworkCore.MySql](https://www.nuget.org/packages/Pomelo.EntityFrameworkCore.MySql/) ([GitHub](https://github.com/PomeloFoundation/Pomelo.EntityFrameworkCore.MySql))
- [Steeltoe.CloudFoundry.ConnectorCore](https://www.nuget.org/packages/Steeltoe.CloudFoundry.ConnectorCore/) ([GitHub](https://github.com/SteeltoeOSS/connectors))
- [Steeltoe.CloudFoundry.Connector.EFCore](https://www.nuget.org/packages/Steeltoe.CloudFoundry.Connector.EFCore/)

### Testing / Documentation
- [Microsoft.NET.Test.Sdk](https://www.nuget.org/packages/Microsoft.NET.Test.Sdk/)
- [Microsoft.AspNetCore.TestHost](https://www.nuget.org/packages/Microsoft.AspNetCore.TestHost)
- [xunit](https://www.nuget.org/packages/xunit/) ([GitHub](https://github.com/xunit/xunit))
- [xunit.runner.visualstudio](https://www.nuget.org/packages/xunit.runner.visualstudio/)
- [FluentAssertions](https://www.nuget.org/packages/FluentAssertions/) ([GitHub](https://github.com/fluentassertions/fluentassertions))
- [Moq](https://www.nuget.org/packages/Moq/) ([GitHub](https://github.com/moq/moq4))
- [Swashbuckle.AspNetCore](https://www.nuget.org/packages/Swashbuckle.AspNetCore/) ([GitHub](https://github.com/domaindrivendev/Swashbuckle.AspNetCore))
