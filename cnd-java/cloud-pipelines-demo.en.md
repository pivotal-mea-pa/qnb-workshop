# Cloud-Pipelines-Concourse

Instructions to demo CI/CD pipelines with Concourse

## Step 1: Initial set up

### Github
Create a github account and install github client on your laptop. To validate if git is available, run below command.

```bash
$ git -version
```

Follow instructions to set up [SSH]((https://help.github.com/en/articles/connecting-to-github-with-ssh)) for github account. Validate SSH access by following instructions from [here](https://help.github.com/en/articles/testing-your-ssh-connection) 


### Docker
Follow instructions available at [Docker](https://docs.docker.com/docker-for-mac/). If you already have docker and docker-compose, you may skip this step. Test the setup by running the following.

```bash
$ docker --version
Docker version 18.09.2, build 6247962
$ docker-compose --version
docker-compose version 1.23.2, build 1110ad01
```
### Working directory
Create a working directory to bring all the needed dependencies together. Let us call this directory CP_HOME for demo purposes.

```bash
$ mkdir cloudpipelines
$ export CP_HOME="absolute_path_of_cloudpipelines"
$ cd cloudpipelines
```
### Concourse

Clone github [concourse](https://github.com/concourse/concourse-docker)
```bash
$ clone https://github.com/concourse/concourse-docker.git
$ cd concourse-docker
```
Generate ssh keys needed to run the concourse, concourse-web and concourse-worker.
```bash
$ cd concourse-docker
$ ./keys/generate
wrote private key to /keys/session_signing_key
wrote private key to /keys/tsa_host_key
wrote ssh public key to /keys/tsa_host_key.pub
wrote private key to /keys/worker_key
wrote ssh public key to /keys/worker_key.pub
```
Next, run `docker-compose up -d` to start Concourse in the background:

```bash
$ docker-compose up -d
Starting concourse-docker_db_1 ... done
Starting concourse-docker_web_1 ... done
Starting concourse-docker_worker_1 ... done
```

Check logs of running docker container if concourse does not come properly.
```bash
$ docker logs -f concourse-docker_worker_1
```

#### Concourse CLI -- Fly

Open http://127.0.0.1:8080/ in your browser:

[![initial](https://github.com/starkandwayne/concourse-tutorial/blob/master/docs/images/dashboard-no-pipelines.png)](http://127.0.0.1:8080/)

Click on your operating system to download the `fly` CLI.

Once downloaded, copy the `fly` binary into your path (`$PATH`), such as `/usr/local/bin` or `~/bin`. Don't forget to also make it executable. For example,

```bash
$ sudo mkdir -p /usr/local/bin
$ sudo mv ~/Downloads/fly /usr/local/bin
$ sudo chmod 0755 /usr/local/bin/fly
```

#### Concourse Login -- Fly

```bash
$ fly --target test login --concourse-url http://127.0.0.1:8080 -u test -p test
$ fly --target test sync
```

### PWS account

Ask workshop coordinator for a Pivotal Web Services account. Sign up for a trial account at [PWS](https://run.pivotal.io/)


### Sample Applications

#### Fork

[app-config-1](https://github.com/Pivotal-Field-Engineering/pace-cnd-app-config)

[stub-runner-boot](https://github.com/Pivotal-Field-Engineering/pace-cnd-java-stub-runner-boot)

[greeting-ui](https://github.com/Pivotal-Field-Engineering/pace-cnd-java-greeting-ui)

[fortune-service](https://github.com/Pivotal-Field-Engineering/pace-cnd-java-fortune-service)

#### Clone sample applications to working directory

```bash
$ cd $CP_HOME
$ git clone https://github.com/<your_github_id>/pace-cnd-app-config.git
$ git clone https://github.com/<your_github_id>/pace-cnd-java-stub-runner-boot.git
$ git clone https://github.com/<your_github_id>/pace-cnd-java-greeting-ui.git
$ git clone https://github.com/<your_github_id>/pace-cnd-java-fortune-service.git
$ git clone https://github.com/CloudPipelines/concourse.git
```

### Bintray account

Create a bintray account for your own maven repository. Visit [bintray](https://bintray.com/signup) to signup.

Once account is set up, create a maven repository.

Once maven repository is created, create packages for pace-cnd-java-greeting-ui, pace-cnd-java-fortune-service and pace-cnd-java-stub-runner-boot. During package creation choose license as "Apache 2.0" and provide github link of the package.

Refer "Set Me Up" of Bintray account for setting up .m2/settings.xml and distribution management

```plain
<?xml version="1.0" encoding="UTF-8" ?>
<settings xsi:schemaLocation='http://maven.apache.org/SETTINGS/1.0.0 http://maven.apache.org/xsd/settings-1.0.0.xsd'
          xmlns='http://maven.apache.org/SETTINGS/1.0.0' xmlns:xsi='http://www.w3.org/2001/XMLSchema-instance'>

 <servers>
    <server>
      <id>bintray-<your_bintray_id>-maven-repo</id>
      <username>your_bintray_id</username>
      <password>your_bintray_key</password>
   </server>
 </servers>

    <profiles>
        <profile>
            <repositories>
                <repository>
                    <snapshots>
                        <enabled>false</enabled>
                    </snapshots>
                    <id>bintray-<your_id>-maven-repo</id>
                    <name>bintray</name>
                    <url>maven_repo_download</url>
                </repository>
            </repositories>
            <pluginRepositories>
                <pluginRepository>
                    <snapshots>
                        <enabled>false</enabled>
                    </snapshots>
                    <id>bintray-<your_id>-maven-repo</id>
                    <name>bintray-plugins</name>
                    <url>maven_repo_download</url>
                </pluginRepository>
            </pluginRepositories>
            <id>bintray</id>
        </profile>
    </profiles>
    <activeProfiles>
        <activeProfile>bintray</activeProfile>
    </activeProfiles>
</settings>

```

Take note of download and upload URL's for the maven packages. For Bintray, they are different.

## Step 2: Understand Pipeline

Review the following

(a) Base pipeline.yml present under $CP_HOME/concourse/src/pipeline/pipeline.yml

(b) Tasks present under $CP_HOME/concourse/src/tasks folder

(c) Show an example of running pipeline and relate the pipeline jobs to tasks found above.


## Step 3: Application Configuration

### pace-cnd-java-stub-runner-boot

Discuss about stub runner, show how stub runner runs stubs by downloading from bintray.

Go to pace-cnd-java-stub-runner-boot project, perform build and upload artifact to bintray. This artifact is downloaded from
bintray and used for running tests.

```bash
$ cd $CP_HOME
$ cd pace-cnd=java-stub-runner-boot
$ export MAVEN_REPO_URL=https://api.bintray.com/maven/<your_bintray_id>/maven-repo/pace-cnd-java-stub-runner-boot
$ export MAVEN_REPO_ID=bintray-<your_bintray_id>-maven-repo
$ ./mvnw clean install
$ ./mvnw clean deploy -Ddistribution.management.release.url="${MAVEN_REPO_URL}" -Ddistribution.management.release.id="${MAVEN_REPO_ID}"
```

Bintray does not automatically allow artifacts to be published for any one to download, so please publish artifacts for pipeline access.

### pace-cnd-java-fortune-service

Discuss about fortune service, review code and show fortune service contract with greeting-ui. Explain about version branch.

```bash
$ cd $CP_HOME
$ cd pace-cnd-java-fortune-service
$ git branch version
```

Open pom.xml and update below maven properties. These properties are used in uploading artifact to bintray. 

```plain
<distribution.management.release.id>bintray-<your_bintray_id>-maven-repo</distribution.management.release.id>
<distribution.management.release.url>https://api.bintray.com/maven/<your_bintray_id>/maven-repo/pace-cnd-java-fortune-service</distribution.management.release.url>
```

Release goes through test and stage before releasing to production. Each environment maps to a space in PWS. Create 3 spaces in PWS under your org.

Space names could be <workshop_id>-fortune-service for test, <workshop_id>-stage for stage, <workshop_id> for prod.

```bash

$ cf login -a api.run.pivotal.io

$ cf create-space td-fortune-service
$ cf create-space td-greeting-ui
$ cf create-space td-stage
$ cf create-space td-prod

```


Fortune Service depends on various marketplace services. Refer to cloud-pipelines.yml in this project and update config server github location.

Pipeline does not create services automatically in production environment. It is recommended to create services manually, refer below.

```bash

$ cf login -a api.run.pivotal.io

# choose org and space for production, run the below commands.

$ cf create-service p-service-registry trial service-registry
$ cf create-service p-circuit-breaker-dashboard trial circuit-breaker-dashboard
$ cf create-service cleardb spark fortune-db
$ cf create-service cloudamqp lemur cloud-bus
$ cf create-service  -c '{"git": { "uri": "https://github.com/Pivotal-Field-Engineering/pace-cnd-app-config" }, "count": 1 }' p-config-server trial config-server 


```

### pace-cnd-java-greeting-ui

Discuss about greeting ui, review code and show how greeting-ui calls fortune-service. Explain about version branch.


```bash
$ cd $CP_HOME
$ cd pace-cnd-java-greeting-ui
$ git branch version
```

Open pom.xml and update below maven properties. These properties are used in uploading artifact to bintray.

```plain
<distribution.management.release.id>bintray-<your_bintray_id>-maven-repo</distribution.management.release.id>
<distribution.management.release.url>https://api.bintray.com/maven/<your_bintray_id>/maven-repo/pace-cnd-java-greeting-ui</distribution.management.release.url>
```

Greeting UI depends on various marketplace services. Refer to cloud-pipelines.yml in this project and update config server github location.

Pipeline does not create services automatically in production environment. It is recommended to create services manually. 

Greeting UI is deployed in same production space as Fortune Service, so no need to run below commands. 

```bash

$ cf login -a api.run.pivotal.io

# choose org and space for production, run the below commands.

$ cf create-service p-service-registry trial service-registry
$ cf create-service p-circuit-breaker-dashboard trial circuit-breaker-dashboard
$ cf create-service cloudamqp lemur cloud-bus
$ cf create-service  -c '{"git": { "uri": "https://github.com/Pivotal-Field-Engineering/pace-cnd-app-config" }, "count": 1 }' p-config-server trial config-server 


```

### Application Specific Pipeline Credentials

Each project needs its own credentials file to use in pipeline. Let us start with base credential file and update as needed. Explain various sections of the credentials file.

```bash
$ mkdir credentials
$ cd credentials
$ cp $CP_HOME/concourse/src/pipeline/credentials-sample-cf.yml credentials-fortune-service.yml
$ cp $CP_HOME/concourse/src/pipeline/credentials-sample-cf.yml credentials-greeting-ui.yml
```

Set your PAS environment, PAS credentials, PAS spaces, bintray credentials, bintray artifact upload endpoints, git SSH private key. Space names could be <workshop_id>-fortune-service for test, <workshop_id>-stage for stage, <workshop_id> for prod.


## Step 4: Run Pipeline

### Run fortune-service pipeline

Create fortune-service pipeline by providing pipeline.yml and credentials-fortune-service.yml

```bash

$ fly --target test login --concourse-url http://127.0.0.1:8080 -u test -p test
$ fly -t test set-pipeline -p fortune-service -c "${CP_HOME}/concourse/src/pipeline/pipeline.yml" -l "${CP_HOME}/credentials/credentials-fortune-service.yml" -n

```

When pipeline is created it is paused, to unpause the run the following command OR log in to concourse admin UI console to unpause the pipeline

```bash

$fly -t test unpause-pipeline -p fortune-service

```

### Run greeting-ui pipeline

Greeting-ui has dependency on fortune-service stubs, wait until fortune-service pipeline completes build-and-upload job before running this pipeline. 


```bash

$ fly --target test login --concourse-url http://127.0.0.1:8080 -u test -p test
$ fly -t test set-pipeline -p greeting-ui -c "${CP_HOME}/concourse/src/pipeline/pipeline.yml" -l "${CP_HOME}/credentials/credentials-greeting-ui.yml" -n

```

When pipeline is created it is paused, to unpause the run the following command OR log in to concourse admin UI console to unpause the pipeline

```bash

$fly -t test unpause-pipeline -p greeting-ui

```

## Step 5: Cloud Pipelines Documentation

For additional information and recipies like DB migration refer [here](https://cloudpipelines.github.io/concourse/)

