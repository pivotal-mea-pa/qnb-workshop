Intro
=====

This lab will guide you through building a **BASIC** continuous delivery
pipeline using Jenkins and Cloud Foundry.

Initial Setup
=============

1.  Fork the Spring-Music repository. If you do not have a github
    account, you can skip this step and use my repository.

        `https://github.com/cloudfoundry-samples/spring-music.git`

2.  (Optional) Download/Install locally your Jenkins server https://jenkins.io/download/

3.  Login to your Jenkins instance (local or remote) at
    <https://jenkins-0.sys.yourcompany.com/> with your username and password.

Create the Build Job
====================

1.  Click **New Item**, give it the name **spring-music** and select
    “Build a free-style software project.” Then click `OK`.

Configure Git
=============

1.  Under **Source Code Management**, select **Git**, and supply your
    repository URL (e.g.
    `https://github.com/<YOUR_GIT_USERNAME>/spring-music`). Leave
    credentials as **none**.

Configure Gradle Build
======================

1.  Select **Add Build Step** and then **Invoke Gradle Script**.

2.  Select **Use Gradle Wrapper**.

3.  Check **Make gradlew executable**

4.  In switches, enter `-Pbuildversion=$BUILD_NUMBER`.

5.  In tasks, enter `clean assemble`.

6.  In build file, enter `build.gradle`.

7.  Check \*Force GRADLE\_USER\_HOME to use workspace.

8.  On "Build Triggers", select "GitHub hook trigger for GITScm polling" and Poll SCM. 

8.  In Schedule enter the polling interval (Ex. every 2 min => H/2 * * * * ).

9.  Select **Add Build Step** and then **Execute Shell**.

10.  Populate/update the script below to match your envoronment and save.

11.  Jenkins host operating system (e.g. `CLI_HOST_OS="macosx64-binary"`). Other options for the binary could be: `linux32-binary`, `linux64-binary`, `macosx64-binary`, `windows32-exe`, `windows64-exe`).

12.  PCF Installation (e.g. `CLI_API=https://api.run.pivotal.io`)

13.  Populate your-username your-password your-org your-dev in the sample shell script


Execute Shell
=============
    export SCRIPT_BUILD_VERSION="1.3"
    export CLI_HOST_OS="macosx64-binary"
    export CLI_API=https://api.run.pivotal.io
    wget -O cf.tgz https://cli.run.pivotal.io/stable?release=$CLI_HOST_OS&source=github-rel
    sleep 15
    tar -zxvf ./cf.tgz
    ./cf api --skip-ssl-validation $CLI_API
    ./cf login -u <your-username> -p <your-password> -o <your-org> -s <your-dev>
    ./cf push -f manifest.yml
    
Execute Build Job
=================

1.  Select **Build Now**.

2.  Select the build in **Build History**, then select **Console
    Output**.

3.  Jenkins should build and push the app, then you can click the link
    at the bottom to see the running app.
