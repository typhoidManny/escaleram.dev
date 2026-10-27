---
title: 🔒Private Home Surveillance w/ Frigate🔒
description: How to setup a local-first Home Surveillance System with AI detection.
slug: frigate
date: 2026-10-25 15:38:40+0000
image: frigateScreenshot.jpg
categories:
    - Post
tags:
    - Tutorial
weight: 1       # You can add weight to some posts to override the default sorting (date descending)

cover:
    hidden: false
    hiddenInList: false
    hiddenInSingle: false
    
toc: true
---
<div style="text-align: center">

> Cover photo blurred to protect my location.

</div>

## Intro

I've got some concerns with how Big Tech is handling sensitive data. I won't get into that topic today, but it made me think twice when I thought about getting a home surveillance system to spy on my cat (named Fish).

Luckily, after a bit of research I found [Frigate NVR](https://frigate.video/). It happened to check all of my boxes and even came with a lot more features to play around with.

Below I will provide some guidance and show you how I configured Frigate with object detection, face recognition, and more on afforadable hardware! 

## Hardware

### Cameras

<div style="text-align: center">

Prices below as of 2026:

</div>

    - 4x Tapo C101 (Used on Amazon - $50)
    - 2x Tapo C217 (Amazon - $60)

Im using the C101 camera model indoors and the C217 outdoors. 

Both of these models support RTSP; the C217 also supports pan/tilt features over ONVIF.   

### PC Hardware
You will also want a mid-range PC or a Raspberry Pi to run the software. If you dont have an Edge TPU and want to run object detection, you might want to consider a PC with a dedicated GPU.

I'm using the Google Coral TPU USB Accelerator (Amazon - ~$100) to run my object detection. This is only one option of many.

My GPU is an NVIDIA GeForce RTX 2060 purchased nearly 10 years ago. This can be used to run lightweight object detection models, but im only using it for processing the footage.

<div style="text-align: center">

Total cost for cameras and AI Coprocessor: **$210.00**

</div>

## Software

### PC Environment
Frigate runs on [Docker](https://docs.docker.com/get-started/docker-overview/) so experience with containerization is going to help a lot.

This works well in a Linux environment; I have yet to try Windows or MacOS. Personally, I'm running CachyOS which I wouldn't recommend for beginners.

### Cameras
The Tapo cameras require you to create a TP-Link account for setup. Upon initial setup, the cameras are configured to stream directly to TP-Links proprietary cloud platform. This is something I wanted to avoid in favor of using RTSP to receive streams locally and preventing the cameras from directly accessing the internet.

**NOTE: TP-Link does not make it easy to keep the camera streams isolated from external servers.** You must first create a TP-Link account to boot the cameras, create an RTSP stream account for each camera, then configure a static IP for each camera. In order to prevent camera streams from reaching Tapo servers, you must configure each camera with a bad gateway after setting up the streams in Frigate.



