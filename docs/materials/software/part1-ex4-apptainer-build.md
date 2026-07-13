---
status: testing
---

<style type="text/css">
  pre em { font-style: normal; background-color: yellow; }
  pre strong { font-style: normal; font-weight: bold; color: \#008; }
</style>

Software Exercise 1.4: Build and Use an Apptainer Container
====================================

**Objective**: Practice the steps required to build your own custom apptainer container. 

**Why learn this?**: You may need to go through this process if you 
want to use a container for your jobs and can't find one that has 
what you need. 

Process overview
----------------

In order to build a container, you need to create a specification file, called a 
"definition file", that incorporates all the files, commands and variables needed 
to install your software. There are many exercises today showing various definition files. 

You will invoke a build command to actually create the container - reading in the 
definition file, and creating a container file with the extension `.sif`. 

Once you have a `.sif` container file, you can use it in jobs by adding a single line 
to your HTCondor submit file. 

What does our code need? 
-----------------

1. Create a script called `hello-cow.py`:

		:::file
		#!/usr/bin/env python3

		import cowsay
		cowsay.cow('Hello OSG User School')

	Notice that it imports and uses a library called `cowsay`. In order for the script 
	to run, the `cowsay` python package needs to be installed. 

1. Give it executable permissions: 

		:::console
		$ chmod +x hello-cow.py

1. Try running the script:

		:::console
		$ ./hello-cow.py

	It will likely fail, because the `cowsay` library isn't installed. This is a 
	scenario where we will build our own container that includes a base 
	Python installation and the `cowsay` Python library. 

Preparing a Definition File
---------------------------

We can describe our desired Apptainer image in a special format called a 
**definition file**. This has special keywords that will direct Apptainer 
when it builds the container image. 

1. Create a file called `py-cowsay.def` with these contents: 

		:::file
		Bootstrap: docker
		From: hub.opensciencegrid.org/htc/ubuntu:24.04

		%post
			apt-get update -y
			apt-get install -y \
					python3-pip \
					python3-numpy
			python3 -m pip install cowsay

	Note that we are starting with the same `ubuntu` base we used in previous 
	exercises. The `%post` statement includes our installation commands, including 
	updating the `pip` and `numpy` packages, and then using `pip` to install `cowsay`.

To learn more about definition files, see [Exercise 2.4](part2-ex4-apptainer-definition.md)

Build the Container
-------------------

Once the definition file is complete, we can build the container. 

1. Run the following command to build the container: 

		:::console
		$ apptainer build --ignore-proot py-cowsay.sif py-cowsay.def

As with the Docker image in the [previous exercise](part1-ex3-docker-jobs.md), 
the first argument is the name to give to the newly create image file and the 
second argument is how to build the container image - in this case, the definition file. 

Testing the Image Locally
-------------------

1. Do you remember how to interactively test an image? Look back 
at [Exercise 1.1](part1-ex1-run-apptainer.md) and guess what command would 
allow us to test our new container. 

1. Try running: 

		:::console
		$ apptainer shell py-cowsay.sif

1. Then try running the `hello-cow.py` script: 

		:::console
		Apptainer> ./hello-cow.py

1. If it produces an output, our container works! We can now exit (by typing `exit`)
and submit a job. 

Submit a Job
--------------

1. Make a copy of a submit file from a previous exercise in this section. Can you 
guess what options need to be used or modified? 

1. Make sure you have the following (in addition to `log`, `error`, `output` and 
CPU and memory requests): 

		:::file
		container_image = py-cowsay.sif
		
		executable = hello-cow.py

1. Submit the job and verify the output when it completes. 

		:::file
		  ______________________
		| Hello OSG User School! |
		  ======================
							  \
							   \
								 ^__^
								 (oo)\_______
								 (__)\       )\/\
									 ||----w |
									 ||     ||

!!! warning "Proper location for container `.sif` files"
	The above example is storing the `py-cowsay.sif` file in the home directory on the Access Point,
	and in turn that means the job will transfer the file via the Access Point. Container image files,
	however, are typically large and if you are submitting many jobs, the Access Point can be overwhelmed
	trying to transfer so many large files!

	In practice, container image files like this one should be placed in your `$DATA` directory, and the
	submit file should use the `osdf:///` protocol to declare the transfer. For more information, see the
	[Data Exercises](/school-2025/materials/data/part2-ex1-osdf-inputs/) or the 
	[OSPool guide on using the OSDF](https://portal.osg-htc.org/documentation/htc_workloads/managing_data/osdf/).

Apply to Your Work
------------------

In the next set of exercises, you will explore strategies for finding or creating 
apptainer containers with certain software and identify an approach that works for *your* software. 
