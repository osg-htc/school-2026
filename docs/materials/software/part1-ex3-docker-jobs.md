---
status: testing
---

<style type="text/css"> pre em { font-style: normal; background-color: yellow; } pre strong { font-style: normal; font-weight: bold; color: #008; } </style>

Software Exercise 1.3: Use Docker Containers in OSPool Jobs
====================================

**Objective**: Convert between container formats and use them. 

**Why learn this?**: The OSPool works best with apptainer containers, but many existing 
containers are created as Docker containers; if you can find an existing Docker container,
you'll need to do this step to use it on the OSPool. 

Convert Docker container to an apptainer format
-------------------

While it is technically possible to use a Docker container directly in a job, 
there are some good reasons for converting it to a local Apptainer container first. 

1. We are going to be using a [Python image from Docker Hub](https://hub.docker.com/_/python). 
Click on the "Tags" tab to see all the different versions of this container that exists. 

1. Let's use version `3.11-alpine`. To convert the Docker container to a local Apptainer container, run: 
	
		:::console
		$ apptainer build --ignore-proot local-py311.sif docker://python:3.11-alpine

	The first argument after `build` is the name of the new Apptainer container file, the 
second argument is what we're building from (in this case, Docker). 

	!!! warning "--ignore-proot"
		Due to security settings on the OSPool APs, `--ignore-proot` is a required arguement. This might change in the future.
	
	!!! warning "Ensure environment is ready for Apptainer commands"
		If your login has been interrupted or you've changed terminals since
		[Software Exercise 1.1](/school-2025/materials/software/part1-ex1-run-apptainer), 
		then make sure to run the commands in the
		[Setup section](/school-2025/materials/software/part1-ex1-run-apptainer/#setup)
		of that exercise before proceeding!

1. (Optional): you can explore the container using the same commands as the [first exercise](part1-ex1-run-apptainer.md). What can you learn about this container and what is installed? 

Use the converted container in a job
-------------------

1.  Make a copy of your submit file from the [previous container exercise](part1-ex2-apptainer-jobs.md) or build from an existing submit file. 

1.  Add the following lines to the submit file or modify existing lines to match the lines below: 

		:::file
		container_image = local-py311.sif

1.  Use the same executable as the [previous exercise](part1-ex2-apptainer-jobs.md). 

1. Once these steps are done, submit the job. You might get a warning about using OSDF for container transfers - ignore this warning for now.

!!! warning "Proper location for container `.sif` files"
	The above example is storing the `local-py310.sif` file in the home directory on the Access Point,
	and in turn that means the job will transfer the file via the Access Point. Container image files,
	however, are typically large and if you are submitting many jobs, the Access Point can be overwhelmed
	trying to transfer so many large files!

	In practice, container image files like this one should be placed in your `$DATA` directory, and the
	submit file should use the `osdf:///` protocol to declare the transfer. For more information, see the
	[Data Exercises](/school-2025/materials/data/part2-ex1-osdf-inputs/) or the 
	[OSPool guide on using the OSDF](https://portal.osg-htc.org/documentation/htc_workloads/managing_data/osdf/).


Apply to Your Work
------------------

Do you or your colleagues use Docker containers for running calculations? Is the software you want to use already available via a container registry 
   such as [DockerHub](https://hub.docker.com/), [NVIDIA Catalog](https://catalog.ngc.nvidia.com/containers), or elsewhere?
