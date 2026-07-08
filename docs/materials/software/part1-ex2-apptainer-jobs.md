---
status: testing
---

<style type="text/css"> pre em { font-style: normal; background-color: yellow; } pre strong { font-style: normal; font-weight: bold; color: \#008; } </style>

Software Exercise 1.2: Use Apptainer Containers in OSPool Jobs
============================================================

**Objective**: Submit a job that uses an existing apptainer container; compare default 
job environment with a specific container job environment. 

**Why learn this?**: By comparing a non-container and container job, you'll better 
understand what a container can do on the OSPool. 

Default environment for jobs
-------------------

First, let's run a job without a container to see what the typical job environment is. 

1. Create a bash script named `job-env.sh` with the following lines: 

		:::bash
		#!/bin/bash
	
		grep "PRETTY_NAME" /etc/os-release 
		python3 --version
		R --version
		gcc --version
	
	This will print out the version of Linux on the computer, the versions of Python and R, 
	and the version of `gcc`, a common software compiler. 

1. Make the script executable:

		:::console
		$ chmod +x job-env.sh

1. Run the script on the Access Point.

		:::console
		$ ./job-env.sh

	What results did you get? 

1. Copy a submit file from a previous OSPool job and edit it so that the 
script you just wrote is the executable. 

1. Submit the job and read the standard output file when it completes. What version 
of Linux was used for the job? What is the version of `gcc` or Python? 

	!!! note "Exploring the pool"
		If you are curious about what is available by default on the OSPool, you 
		can try submitting many jobs like the one above, and see what they report 
		back. Refer back to [yesterday's material](../htcondor/part2-ex2-queue-n.md) for different ways to submit 
		a list of jobs. 

Container environment for jobs
---------------------

Now, let's try running that same script inside a container. 

1. For this job, we will use the OSG-provided Ubuntu "Focal" image, as we did in the previous exercise. The `container_image` submit file option will tell HTCondor to use this container for the job: 

		:::file
		universe = container
        OSDF_URL = osdf:///TBD
		container_image = $(OSDF_URL)/TBD
   
1. If the submit file you copied has something like `requirements = (OSGVO_OS_STRING == "RHEL 9")`, remove that. When you use containers, you should not specify an OS in the requirements as that will unnecessarily limit the number of resources you can run on.

1. Based on our previous test of this container, what do you expect the job to output? 

1. Submit the job and read the standard output file when it completes. 

Experimenting with other containers
-------------

1. Look at the list of OSG-Supported containers: [OSG Supported Containers](https://portal.osg-htc.org/documentation/htc_workloads/using_software/available-containers-list/)

1. Try submitting a job that uses one of these containers. Change the executable 
script to explore different aspects of that container. 

Apply to Your Work
------------------

How important is it for your jobs to have a consistent software environment? How portable are scripts and software that you use? 
