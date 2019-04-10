# Workshop highlighting how showcase Bindings, specifically MySQL binding to deployed .net application

### 1. Create new visual studio Project (MVC and API)

### 2. Add the following Nuget Packages:
Autofac
Steeltoe.CloudFoundry.ConnectorAutofac
Steeltoe.CloudFoundry.ConnectorBase

### 3. Add the following under app_start:
ServerConfig.cs



### 4. update Application_Start in global.asax and the below

		  /*add server configuration*/
            ServerConfig.RegisterConfig("development");
            var builder = new ContainerBuilder();

            // Register all the controllers with Autofac
            builder.RegisterControllers(typeof(WebApiApplication).Assembly);
            builder.RegisterApiControllers(typeof(WebApiApplication).Assembly);
            
            builder.RegisterMySqlConnection(ServerConfig.Configuration);
            
            builder.RegisterDbContext<MovieContext>(ServerConfig.Configuration);
            
          
            // Create the Autofac container
            var container = builder.Build();
            DependencyResolver.SetResolver(new AutofacDependencyResolver(container));
            
            GlobalConfiguration.Configuration.DependencyResolver = new AutofacWebApiDependencyResolver((IContainer)container); //Set the WebApi DependencyResolver


### 5. Update References in global.asax

using System.Web.Http;
using System.Web.Mvc;
using System.Web.Optimization;
using System.Web.Routing;

using Autofac;
using Autofac.Integration.Mvc;
using Autofac.Integration.WebApi;
using Steeltoe.CloudFoundry.ConnectorAutofac;
using Steeltoe.CloudFoundry.Connector.EF6Autofac;
using Lab03.Models;

### 6. Add new movies controller

### 7. add MySqlDbContextContainerBuilderExtensions

 
### 7. cf push

### 8. Login to app manager