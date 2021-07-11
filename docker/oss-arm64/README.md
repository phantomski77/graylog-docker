# Graylog Docker Setup fork for arm64v8 (Raspberry Pi4) architecture

## What is this ##
The official [Docker2/graylog-docker GitHub repo](https://github.com/Graylog2/graylog-docker) unfortunately doesn't have *arm64v8* compatible `Dockerfile` yet, so I have added a new directory `oss-arm64`, modified the official Dockerfile and few supporting files with latest available and compatible arm64v8 dependencies and removed some deprecated JVM tags.

Latest stable version of official Graylog is *4.1.1* - this arm64v8 customised version is available with the tags `4.1.1-arm64`, `arm64` and `latest` on [Docker hub](https://hub.docker.com/r/phantomski/graylog).


## What is changed in this fork?

### Additional oss-arm64 folder

This additional folder contains all the necessary modified files to build an arm64v8 compatible Graylog image for running on Raspberry Pi 4.

### Modified files in oss-arm64 folder

**Dockerfile** - to build an arm64v8 compatible image, using latest available arm64v8 architecture dependencies:
* Debian Buster Slim
    * v10.10 Slim (`arm64v8/debian:buster-slim` or `debian:buster-slim`)
* Java JDK JRE
    * Java SE 11 (LTS) JDK JRE 11.0.11+9 (`arm64v8/openjdk:11-jre-slim-buster`)
    * 11 LTS is the latest useable as Java SE 16 doesn’t have small JRE, only full JDK. Also, in 16, the old CMS Garbage Collector is deprecated.

**hooks/build** - main script to build an image using Dockerfile
Small change to select correct folder so copy doesn't produce errors during build and hard set versioning/tags

**README.md**
This file.