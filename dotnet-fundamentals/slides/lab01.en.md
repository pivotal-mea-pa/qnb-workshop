# Push a .NET Applicatio to Pivotal Application Services Platform

## Goal

Using Visual Studio, compile the app to create an artifact. Then push the artifact to the Platform, for deployment.

## Prerequisites

- Visual Studio (min 2015)
- PAS API Access
- PAS App Manager Access

## Create the App Artifact

1. Create new Visual Studio Web Application ![Publish Menu Option](create-vs-solution.PNG)

2. Build the solution 

1. With the visual studio opened in Visual Studio, right click on the project name and choose `publish`. (No need to `build` or `restore`, it will happen automatically)
    

1. The "Publish" window will open where you can choose the `FolderProfile`. Let the other settings remain as default.
     ![Publish Menu Option](lab01-publish.PNG)

1. Click the `Publish` button to create a new app artifact. 

1. In the output window, note the location of the newly created artifact. Your location will be a little different, but in the above example it's `C:\Projects\Source\Repos\PCF-Workshop\.NET Framework\Lab01\Published`. This folder has the compiled DLL, runtime dependencies, and the PAS manifest. That is your artifact!

## Create manifest.yml

1. create a file called manifest.yml in the published folder with following contents
```
---
applications:
- name: Lab01
  instances: 2
  memory: 1G
  disk_quota: 512M
  buildpack: hwc_buildpack
  stack: windows2016
  health-check-type: process
```
## Log in to PAS

1. Open a powershell window.

2. Log in to PAS. You can copy and paste the following command into your powershell window to login. This command requires that you have setup the environment variables discussed earlier.

   ```
   cf login -a $env:cf_api -u $env:cf_username -p $env:cf_password
   ```
3. You may be prompted to choose an Org and Space. Choose the organization and space specified by your proctor. If you are not prompted, by default your Org and Space have already been set.

4. To confirm you have targeted the correct Org and Space, type `cf target`. The output of this command will be similar to the following: 

    ```powershell
    PS C:\> cf target
    api endpoint:   https://api.system.pcf.acme.io
    api version:    2.120.0
    user:           <USERName>
    org:            <org>
    space:          sandbox
    ```

## Push the Artifact

With the app artifact created, the appropriate Org and Space targeted in PAS, you are ready to push!

1. In the powershell window, `cd` into the directory holding the artifact. This is the directory you published into from Visual Studio. Example:

     ```powershell
     PS C:\> cd 'C:\Projects\Source\Repos\PCF-Workshop\.NET Framework\Lab01\Published'
     ```

1. In the powershell window, type the command `cf push`. Your Artifact will be staged in a temporary place on PAS, containerized, deployed, and a route bound to it. Easy!

     ```bash
     requested state: started
     instances: 1/1
     usage: 1G x 1 instances
     urls: lab-01.domain.com
     last uploaded: Mon Jan 7 01:01:10 UTC 2000
     stack: windows2016
     buildpack: binary

          state     since                    cpu      memory         disk           details
     #0   running   2000-01-01 01:01:01 PM   231.0%   498.1M of 1G   161.9M of 1G (10)
     ```

1. Validate that the microservice is running. The microservice URL is the URL output in the previous step plus the suffix of `/api/values/5`. For example, the above output had the URL as `my-app-host-name.domain.com` and the associated microservice URL would be `https://my-app-host-name.domain.com/api/values/5`. Open this URL in your browser to see the microservice in action.


## Complete

Congratulations! You've pushed your first app! 