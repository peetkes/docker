# Overview

Project will create (in docker)

* A single MarkLogic node

Admin account is
* Username: admin
* Password: admin

# Prerequisites

1. docker toolkit installed
1. Java 8 installed
1. Following ports available
    * 8000-8020
1. Have available a docker image for the desired MarkLogic version or be connected to MarkLogic's internal docker registry(Need VPN)
1. Optionally download the MarkLogicConverters.rpm  for the specific MarkLogic version into the root folder of this project

# Installation steps (once off)
This docker-compose makes use of marklogic precreated images so you need to be onthe MarkLogic VPN and you need to login:

    docker login mlregistry.marklogic.com

Execute (this will download all required docker dependencies to build marklogic image)

    docker-compose build   

Make sure you have a .env file in the root folder of this project with the follwoing content:
```
mlVersion=[MarkLogic version number]
mlHost=[container name]
```

Make sure you have a gradle.properties file with below content:
```
mlHost=[containername]
mlConverterInstaller=MarkLogicConverters-10.0-4.2.x86_64.rpm
mlPassword=admin
```
where mlConverterInstaller and mlPassword are optional

## Server Setup
-------------
    ./gradlew mlDockerSetupNode [-PmlPassword=yourPassword]

## Server Start
-------------
    ./gradlew mlDockerStart

## Server Stop
-------------
    ./gradlew mlDockerStop

## Server TearDown
-------------
    ./gradlew mlDockerTeardown
