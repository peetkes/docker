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
1. Have available a docker image for the desired MarkLogic version or be connected to MarkLogic's internal docker registry(Need VPN)
1. Optionally download the MarkLogicConverters.rpm  for the specific MarkLogic version into the root folder of this project

# Installation steps (once off)
Execute (this will download all required docker dependencies to build marklogic image)

    docker-compose build   

Make sure you have a .env file in the single-node or multi-node folder of this project with the following content:

single-node
```
mlVersion=10.0-4.2
mlHost=sccss.dhf.1042
mlAdmin=admin
mlPassword=admin
mlPortMapping=7997-8025:7997-8025
```

multi-node
```
mlVersion=10.0-5.2
mlHost=marklogic
mlAdmin=admin
mlPassword=admin
mlPorts=7997-8025
mlPortMappings=7997-8025

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

Make sure you have a gradle.properties file with below content:
```
mlHost=[containername]
mlPassword=admin
```
where mlPassword are optional

## Server Setup
-------------
    ./gradlew dockerSetupMarkLogicNode [-PmlPassword=yourPassword]

## Server Start
-------------
    ./gradlew dockerStart

## Server Stop
-------------
    ./gradlew dockerStop

## Server TearDown
-------------
    ./gradlew dockerTeardown
