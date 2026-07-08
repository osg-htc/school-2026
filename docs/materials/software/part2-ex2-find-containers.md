---
status: testing
---

<style type="text/css"> pre em { font-style: normal; background-color: yellow; } pre strong { font-style: normal; font-weight: bold; color: \#008; } </style>

Software Exercise 2.2: Find Existing Containers
============================================================

**Objective**: Identify a container that will work for your jobs. 

**Why learn this?**: If you can find a container with your software already installed, 
submitting jobs is as simple as adding one line to your submit file. 

Overview
-----------

Go through some of the suggestions below to search for containers. Make sure 
to read the warnings and tips. 

Once you find an existing container, you can go through the steps shown in 
[Exercise 1.2](part1-ex2-apptainer-jobs.md) and [Exercise 1.3](part1-ex3-docker-jobs.md) 
to test it in a job. 

Find containers 
-----------

Here are some places to go looking for existing containers with your software: 

* Your software's documentation page
* [OSG-Supported Containers](https://portal.osg-htc.org/documentation/htc_workloads/using_software/available-containers-list/)
* [DockerHub](https://hub.docker.com/), for example: 
	* [miniconda](https://hub.docker.com/u/continuumio)
	* [rocker](https://hub.docker.com/u/rocker)
	* [jupyter](https://hub.docker.com/u/jupyter)
	* [nvidia](https://hub.docker.com/u/nvidia)
	* (and many more!)
* [NVIDIA Catalog](https://catalog.ngc.nvidia.com/containers)

!!! warning "Reviewing containers"
	There are a lot of Docker containers on Docker Hub, but they are not all 
	created equal. Anyone can create an account on Docker Hub and share container images there, so it’s important to exercise caution when choosing a container image on Docker Hub. These are some indicators that a container image on Docker Hub is consistently maintained, functional and secure:
	
	- The container image is updated regularly.
	- The container image is associated with a well established company, community, or other group that is well-known.
	- There is a Dockerfile or other listing of what has been installed to the container image.
	- The container image page has documentation on how to use the container image. [^1]

!!! question "Knowledge Check"
	Given these indicators:
	
	1. Can you find a container on [Docker Hub](https://hub.docker.com/) that would be 
	useful for running Jupyter notebooks that use tensorflow? 
	
	1. Does your chosen image meet at least 2 of the criteria above? 


[^1]: This list and previous text taken from [Introduction to Docker](https://carpentries-incubator.github.io/docker-introduction/)
