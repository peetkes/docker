# Overview

Project will create (in docker)

* Either a single MarkLogic node (folder 'single-node')
* Or a MarkLogic node with a maven enabled opensdk jdk 11 development node and a maven and gradle enabled development node(multi-node) 

Admin account for MarkLogic is
* Username: admin
* Password: admin

# Prerequisites

1. docker toolkit installed
1. Java 8 (or higher) installed
1. Following ports available
    * 8000-8020
1. Download the appropriate version of the MarkLogic Server installer into the single-node/src/marklogic folder
1. Optionally download the appropriate MarkLogicConverters.rpm for the specific MarkLogic version into the single-node/src/marklogic folder

# Create a docker image with MarkLogic installed and initialized 

Build the image
    cd single-node/src/marklogic
    docker build -t marklogic:[mlVersion]-preinitialized .

This will take a while, when this process has finished you can start an instance
    docker run -d --name initial-install --hostname [mlHost] -p 8001:8001 marklogic:[mlVersion]-preinitialized

the --hostname will actually set the hostname of the docker container as it will be used on the hostname command.

Now MarkLogic has to be initialized, this can be done via the admin UI on http://localhost:8001 or via the following command if you want to script the process
    docker exec initial-install init-marklogic

This will give something like below:
    Initializing marklogic...
    Initialization complete for marklogic...

Now we can create a new image on which MarkLogic is already initialized
    docker commit initial-install marklogic:[mlVersion]-installed

# Installation steps (once off)

Make sure you have a gradle.properties file in the single-node or multi-node folder of this project with the following content:

single-node
```
projectName=development-sn
mlVersion=10.0-5.2
mlHost=marklogic
mlAdmin=admin
mlPassword=admin
mlHealthPort=7997
mlForeignBindPort=7998
mlBindPort=7999
mlServicesPort=8000
mlAdminPort=8001
mlManagePort=8002
mlAppPorts=8010-8020

srcFolder=./Data
```

multi-node
```
projectName=development-sn
mlVersion=10.0-5.2
mlHost=marklogic
mlAdmin=admin
mlPassword=admin
mlHealthPort=7997
mlForeignBindPort=7998
mlBindPort=7999
mlServicesPort=8000
mlAdminPort=8001
mlManagePort=8002
mlAppPorts=8010-8020

srcFolder=./Data

mvnHost=maven
mvnApplicationFolder=/Users/[user]/[application-folder]
mvnApplicationPort=8080
mvnApplicationPortMapping=8080
mvnApplicationDebugPort=5005
mvnApplicationDebugPortMapping=5005

devHost=develop
devApplicationFolder=/Users/[user]/[application-folder]
devApplicationPort=9090
devApplicationPortMapping=8080
devApplicationDebugPort=6005
devApplicationDebugPortMapping=5005

mavenSettingsFolder=/Users/[user]/.m2
```

Execute (this will download all required docker dependencies to build marklogic image)
    gradle composeBuild -i   

## Server Up
-------------
    gradle composeUp -i

## Server Down
-------------
    gradle composeDown -i

## Server Restart
-------------
    gradle composeRestart

## Server TearDown
-------------
    gradle composeTeardown
