**Previous:** [JetBrains Rider](../JetBrains-Rider) or [Microsoft Visual Studio](../Microsoft-Visual-Studio)

In this section, you'll install Concourse, a Continuous Integration tool, on your local machine using Docker. We'll use this to build, test, and deploy the .NET app to Cloud Foundry.

First, find the particular flavour of [Docker](https://store.docker.com/search?type=edition&offering=community) suitable for your OS, and then install/start it using the instructions on the description page.

Once you've run
```bash
docker version
```
and have seen it succeed, you can proceed.

**Note:** On Windows, if Docker prompts you to logout to complete installation, agree. It may also prompt you upon starting it for the first time, asking if you want to enable **Hyper-V and Containers**. Say yes, and your machine will restart to enable them.

***

At this point, it is recommended that you create a dedicated folder that will the files/tools we'll need for working with pipelines and Cloud Foundry. For the purposes of this guide, the folder will be referred to as `tools-folder`. Once it is created, we need to add its location to our environment for [Linux](https://stackoverflow.com/a/26962251), [Mac](https://www.architectryan.com/2012/10/02/add-to-the-path-on-mac-os-x-mountain-lion) or [Windows](https://www.architectryan.com/2018/03/17/add-to-the-path-on-windows-10/).

***

Next, retrieve the Docker Compose file from [Concourse's site](https://concourse-ci.org/docker-compose.yml), placing it in `tools-folder`, and run
```bash
docker-compose up -d
```
in your terminal from that directory. Concourse should now be running on `127.0.0.1:8080`. Visiting it will present you with the web UI for Concourse.

**Note:** On Windows, you may be prompted to allow network access for something called `vpnkit`. You should only need to enable it for private networks. Public network access shouldn't be required.

***

Lastly, download `fly` CLI from the web UI and place the excutable in `tools-folder`. One Windows, you'll want to additionally right-click on `fly.exe` and, under `Properties`, unblock it. Once that is done, set your Concourse target:
```bash
fly -t targetname login -c http://127.0.0.1:8080 -u test -p test
```
where `targetname` is whatever you want your target name to be.

**Note:** The last two arguments (`-u` and `-p`) are the credentials of the default account installed with Concourse.

**Up Next:** [CF Dev](../CF-Dev)

**References**  
[Concourse Setup and Operations docs](https://concourse-ci.org/setup-and-operations.html)  
