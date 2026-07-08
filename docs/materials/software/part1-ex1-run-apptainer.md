---
status: ready
---

<style type="text/css"> pre em { font-style: normal; background-color: yellow; } pre strong { font-style: normal; font-weight: bold; color: \#008; } </style>

Software Exercise 1.1: Run and Explore Containers
============================================================

**Objective**: Observe the differences between the base environment and 
the environment inside a container. 

**Why learn this?**: Being able to run a container directly allows you to confirm 
what is installed and whether any additional scripts or code will work in the context 
of the container. 

Setup
--------

Make sure you are logged into `ap40.uw.osg-htc.org` or `ap41.uw.osg-htc.org`.  For this exercise 
we will be using Apptainer containers maintained by OSG staff or existing 
containers on Docker Hub. 

There is some set-up that we should do to help lighten the load on the 
Access Point as we work with containers. 

First, download the `apptainer-setup.sh` script:

```
pelican object get osdf:///osg-public/school/apptainer-setup.sh ./
```

Then run the script using this command:

```
. apptainer-setup.sh
```

You should see a message that the setup has been completed.
(This is the only time you'll need to run this script.)

What is our default environment? 
-------------------

1. Type the following commands on your Access Point. 

		:::console
		$ grep "PRETTY_NAME" /etc/os-release 
		$ python3 --version
		$ R --version
		$ gcc --version

1. Each of these commands tells you something about the software and operating system 
that is available on the Access Point itself. 
	1. What is the version of Linux on the Access Point? 
	1. Which commands (`python`, `R`, `gcc`) don't run at all? Why might that be? 
	1. What are the versions of the programs that *did* run? 

	!!! note "Full Linux version information"
		In the above example, we used `grep` to only show the value of the `PRETTY_NAME`
		in the `/etc/os-release` file. There's a lot more information in that file
		about the Linux version, which you can see with 
		
			:::console
			$ cat /etc/os-release

Exploring a different environment with Apptainer
-------------------

Now, let's explore a container from the [OSG-Supported List](https://portal.osg-htc.org/documentation/htc_workloads/using_software/available-containers-list/). 

1. Find the full path for the `ubuntu 24.04` container image. 

1. To run it, use this command: 

		:::console
		$ apptainer shell /cvmfs/singularity.opensciencegrid.org/htc/ubuntu:24.04

	It may take a few minutes to start - don't worry if this happens. 

	!!! warning "About the `/cvmfs` path"
		In the above example, we used a path beginning with `/cvmfs` to launch a container.
        
        **This should be the only place that you use such a path!**

1. Once the container starts, the prompt will change to either `Singularity>` or 
  `Apptainer>`. Run `ls` and `pwd`. Where are you? Do you see your files? 

1. The `apptainer shell` command will automatically connect your home directory to 
the running container so you can use your files. 

1. Run the same test commands as before: 

		:::console
		$ grep "PRETTY_NAME" /etc/os-release 
		$ python3 --version
		$ R --version
		$ gcc --version

1. Compare and contrast with what happened when you ran the commands in the default 
Access Point environment. What changed? 

1. Exit out of the container by typing `exit`. 

<!--
Exploring a different environment with Docker
------------------

The process for interactively running a Docker container will be very 
similar to an apptainer container. The main difference is a `docker://` prefix 
before the container's identifying name. 

1. We are going to be using a [Python image from Docker Hub](https://hub.docker.com/_/python). 
Click on the "Tags" tab to see all the different versions of this container that exists. 

1. Let's use version `3.11-alpine`. To run it interactively, use this command: 

		:::console
		$ apptainer shell docker://python:3.11-alpine

1. Once the container starts and the prompt changes, try running similar commands 
as above. What version of Linux is used in this container? Does the version of Python 
match what you expect, based on the name of the container? 

1. Once done, type `exit` to leave the container. 
--> 

Apply to Your Work
------------------

Consider what it takes to install your software - has it been tested on specific versions 
of Linux? Does it need special libraries? Depend on a certain version of Python? Containers 
are a way to make sure all the correct elements are in place for your software 
to work successfully and consistently. 
