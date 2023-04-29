# Graylog Docker Setup fork for arm64v8 (Raspberry Pi4) architecture

## What is this

Latest stable version of official Graylog is *4.3.14* - this `arm64v8` customised version is available pre-built with the tags `4.3.14-arm64`, `arm64` and `latest` on [Docker hub](https://hub.docker.com/r/phantomski/graylog) with the detailed description. 

## Usage

If you don't want to use pre-built image from Docker Hub, you can build it yourself using `build` script with predefined parameters in `docker/oss-arm64/hooks`, or by using standard `Dockerfile` in `docker/oss-arm64` (please note some parameters are required)

## What is changed in this fork?

I have added a new directory `oss-arm64`, modified the official `Dockerfile` and few supporting files with latest available and compatible arm64v8 dependencies and removed some deprecated JVM tags.

### Additional oss-arm64 folder

This additional folder contains all the necessary modified files to build an `arm64v8` compatible Graylog image for running on Raspberry Pi 4 and possibly other arm64 architecture based systems (Odroid C2, Odroid C4, Odroid N2+, Apple M1/M2 Macs, Amazon EC2 on AWS Graviton, Microsoft Project Volterra, etc.)

### Modified files in oss-arm64 folder

**Dockerfile** - to build an `arm64v8` compatible image, using latest available dependencies:
* Ubuntu Jammy
    * v22.04.2 LTS Jammy Jellyfish (`ubuntu:jammy`)
* Eclipse Temurin Java JDK JRE
    * Java SE 11 (LTS) JDK JRE 11.0.19+7 (`eclipse-temurin:11-jre-jammy`)
    * Removed JRE options:
        * -XX:+UseCGroupMemoryLimitForHeap
        * -XX:+UseConcMarkSweepGC
        * -XX:+CMSConcurrentMTEnabled
        * -XX:+CMSClassUnloadingEnabled
        * -XX:+UseParNewGC

**hooks/build** - main script to build an image using Dockerfile with default parameters.
Small change to select correct folder so copy doesn't produce errors during build and hard set versioning/tags

**README.md**
This file.
