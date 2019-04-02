Intro
=====

This lab will guide you through building a **BASIC** continuous delivery
pipeline using Jenkins and Cloud Foundry.

Initial Setup
=============

1.  Fork the Spring-Music repository. If you do not have a github
    account, you can skip this step and use my repository.

        `https://github.com/cloudfoundry-samples/spring-music.git`

2.  3.  Login to your Jenkins instance at
    <https://jenkins-0.sys.yourcompany.com/> with the same username and
    password that you use for CloudFoundry.

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

2.  Select **Use Grade Wrapper**.

3.  Check both **Make gradlew executable** and **From Root Build Script
    Directory**.

4.  In switches, enter `-Pbuildversion=$BUILD_NUMBER`.

5.  In tasks, enter `clean assemble`.

6.  In build file, enter `build.gradle`.

7.  Check \*Force GRADLE\_USER\_HOME to use workspace.

8.  On "Build Triggers", select "Build when a change is pushed to
    GitHub"

Execute Shell
=============

    DEPLOYED_VERSION_CMD=$(CF_COLOR=false cf apps | grep 'mapUS.' | cut -d" " -f1)
    export BUILD_VERSION="1.2"
    export DEPLOYED_VERSION_CMD
    echo DEPLOYED_VERSION_CMD
    export ROUTE_VERSION="default"
    echo "Deployed Version: $DEPLOYED_VERSION"
    echo "Route Version: $ROUTE_VERSION"
    export API=https://api.sys.yourcompany.com

    wget -O cf.tgz https://cli.run.pivotal.io/stable?release=linux64-binary&version=6.13.0&source=github-rel
    sleep 5
    tar -zxvf ./cf.tgz

    ./cf api --skip-ssl-validation $API
    ./cf login -u <user> -p <password> -o <org> -s <space>

    ./cf push -f manifest.yml

Execute Build Job
=================

1.  Select **Build Now**.

2.  Select the build in **Build History**, then select **Console
    Output**.

3.  Jenkins should build and push the app, then you can click the link
    at the bottom to see the running app.
