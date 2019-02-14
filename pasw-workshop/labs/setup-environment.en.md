# Setup Desktop Environment

## Goal

Understand the tools needed to create and push a microservice app.

## Prerequisites

- Visual Studio (min 2015)
- Web Browser (Chrome, Firefox, Edge, Safari)(Not Internet Explorer)
- Powershell

## Download and Validate cf cli

1. Download [the cli](https://cli.run.pivotal.io/stable?release=windows64&source=github).

1. Unpack the zip file.

1. Double click `cf-installer.exe` to begin installation.

1. When prompted, click **Install**, then **Finish**

1. To verify your installation, open powershell and type cf. If your installation was successful, the cf CLI help listing appears. You may need to restart the powershell window to see the cf cli help listing appear.

#### If you don't have permission to install the cli, download the cli executable

1. Download the [cli exe](https://packages.cloudfoundry.org/stable?release=windows64-exe&source=github).

1. Unpack the zip file.

1. Copy the `cf.exe` executable to `c:\Windows\System32` folder. (this is to add `cf` to your path env variable)

1. To verify your installation, open powershell and type `cf`. If your installation was successful, the cf CLI help listing appears. You may need to restart the powershell window to see the cf cli help listing appear.

## Load your creds and API URL in environment variables

1. Open a powershell window and type in the following commands. Remember to replace the values below with your student creds assigned in the [Sheet](/demo/intro-creds).

  ```bash
  $env:cf_api = "<PAS API URL>"
  $env:cf_username = "<Student Username from Sheet>"
  $env:cf_password = "<Student Password from Sheet>"
  ```

## Install the Steeltoe Visual Studio project templates

1. Download the VSIX [templates installer](https://github.com/SteeltoeOSS/Tooling/releases/download/templates-0.0.1/App-Templates-VSIX.vsix).

1. With Visual Studio closed, double click the downloaded file to install the templates.

1. Once complete, open Visual Studio and choose `File > New > Project > C#` there should be additional Steeltoe and Cloud Foundry template listed.
  <img src="a_visual-studio-templates.PNG" alt="VS Steeltoe Templates" width="400"/>