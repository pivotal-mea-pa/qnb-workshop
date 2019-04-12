## Pushing apps
 
### Goals
1. Show how pushing .NET Framework 4.x apps and .NET Core apps work the same.
1. Target both Linux and Windows stacks.

### Steps
1. Push the FunnyQuotesBasicLinux .NET Core frontend.

    ```
    > cd FunnyQuotesBasicLinux
    > cf push
    ```
    
    * Note the default stack is `cflinuxfs2` when omitted, and the `dotnet_core_buildpack` in the manifest.yml file when pushing .NET Core apps to Linux.

1. Push the FunnyQuotesBasicWindows .NET Framework 4.x frontend.

    ```
    > cd FunnyQuotesBasicWindows
    > cf push
    ```
    
    * Note the `windows2016` stack and the `hwc_buildpack` in the manifest.yml file when pushing .NET Framework 4.x apps to Windows.
    * The Hosted Web Core buildpack is essentially the kernel of IIS.

1. Access the app URL, get a few funny quotes, get some laughs. :)
1. View log output

    ```
    > cf logs FunnyQuotesBasicLinux --recent
    ```

1. Show log tailing while pushing / starting up.

    ```
    > cf logs FunnyQuotesBasicLinux
    ```
  
1. Optional: Press Kill button and show recovery
1. Optional: Scale the app
