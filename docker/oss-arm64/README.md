# Graylog Docker Setup fork for arm64 ARMv8.0-A (Raspberry Pi4) architecture

## What is this

Latest stable version of official Graylog that works on arm64 (ARMv8.0-A) instruction set chipsets, like those used on RaspberryPi 4 / 400 / CM4, with all required dependencies, is **4.3.15**. This version is no longer maintained with the latest fixes and dependencies, especially for Java.

This customised version is available pre-built with the tags `4.3.15-arm64-java23`, `arm64` and `latest` on [Docker hub](https://hub.docker.com/r/phantomski/graylog) with the detailed description.
You can also build it yourself with the `build` script and `Dockerfile` provided here.

## Rationale

The reason for this custom build is that Graylog versions 5.0+ requires **MongoDB** minimum version 5.0.7.

ARM Cortex-A76 chips like Broadcom BCM2712 in Raspberry Pi 5 / 500 / CM5 or Rockchip RK3588 in Radxa ROCK Pi 5B / Turing RK1 are **ARMv8.2-A** architecture. They can run those higher versions of MongoDB and thus Graylog without problem.

ARM Cortex-A72 chips like Broadcom BCM2711 in Raspberry Pi 4 / 400 / CM4 are  **ARMv8.0-A** architecture.

Unfortunately MongoDB v4.4.18 is the last one that supports ARMv8.0-A, anything above (including v4.4.19) needs at least ARMv8.2-A or better. This limits Graylog to version 4.3.15.

As the Graylog 4.x versions are not regularly maintained anymore, to support Raspberry Pi 4 users, I'm trying to keep the 4.3.15 version as much up to date as possible myself. This includes latest Java versions and patches, upstream Graylog changes applicable for 4.x and the underlying Linux (Ubuntu) distribution used for the images.

## Latest update (19/03/2025)

I have tried to build the Graylog image using the latest Java SE 23 JDK JRE 23.0.2+7 with latest Ubuntu 24.04.2 LTS both for Java and Graylog. Despite Graylog 4.x being oficially only provided with Java major versions 9 and later 11, version 23 runs without any issues, I might say even better, at least in terms of resources utilisation. It also provides more recent CVE and bug fixes, making the image more secure.

## Future

I am gradually moving to more modern arm64 systems (Apple Silicon, Raspberry Pi 5), so this might very likely be the last compliant version I'll be attempting to build. If you want to keep these updates coming, please kindly generate an Issue on GitHub and I'll do my best to respond.

## Usage

If you don't want to use pre-built image from Docker Hub, you can build it yourself using `build` script with predefined parameters in `docker/oss-arm64/hooks`, or by using standard `Dockerfile` in `docker/oss-arm64` (please note some parameters are required).

## What is changed in this fork?

I have added a new directory `oss-arm64`, modified the official `Dockerfile` and few supporting files with latest available and compatible arm64v8 dependencies and removed some deprecated JVM tags.

### Additional oss-arm64 folder

This additional folder contains all the necessary modified files to build an `arm64v8` compatible Graylog image and deploy the complete Graylog stack for running on Raspberry Pi 4 and possibly other arm64 architecture based systems (Odroid C2, Odroid C4, Odroid N2+, Apple M1/M2 Macs, Amazon EC2 on AWS Graviton, Microsoft Project Volterra, etc.)

### Modified files in oss-arm64 folder

**Dockerfile** - to build an `arm64v8` compatible image, using latest available dependencies:
* Ubuntu Noble
    * v24.04.2 LTS Noble Numbat (`ubuntu:noble`)
* Eclipse Temurin Java JDK JRE
    * Java SE 23 JDK JRE 23.0.2+7 (`eclipse-temurin:23-jre-noble`)
    * Removed JRE options:
        * -XX:+UseCGroupMemoryLimitForHeap
        * -XX:+UseConcMarkSweepGC
        * -XX:+CMSConcurrentMTEnabled
        * -XX:+CMSClassUnloadingEnabled
        * -XX:+UseParNewGC
        * -XX:NewRatio=1
        * -XX:MaxMetaspaceSize=256m
        * -XX:+ResizeTLAB

**hooks/build** - main script to build an image using Dockerfile with default parameters.
Small change to select correct folder so copy doesn't produce errors during build and hard set versioning/tags

**compose.yaml** - example Docker Compose to deploy the whole Graylog / OpenSearch / mongoDB stack

**README.md**
This file.
