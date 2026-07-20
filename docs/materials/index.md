# OSG School Materials

## School Overview and Intro

!!! abstract "Welcome Slides"
	View the slides: [pdf](welcome/files/osgs26-day1-part1-welcome.pdf)

!!! abstract "Introduction to High Throughput Computing"
	View the slides: [pdf](welcome/files/osgs26-htc-intro.pdf)

## Intro to HTCondor Job Execution

!!! abstract "HTCondor: Introductory Slides"
	View the slides: [pdf](htcondor/files/osgus26-htc-htcondor.pdf)

!!! note "Exercises: Intro to HTCondor"
	- [Exercise 1.1: Log in to the local submit machine and look around](htcondor/part1-ex1-login.md)
	- [Exercise 1.2: Experiment with HTCondor commands](htcondor/part1-ex2-commands.md)
	- [Exercise 1.3: Run jobs!](htcondor/part1-ex3-jobs.md)
	- [Exercise 1.4: Read and interpret log files](htcondor/part1-ex4-logs.md)
	- [Exercise 1.5: Determining Resource Needs](htcondor/part1-ex5-request.md)
	- [Exercise 1.6: Remove jobs from the queue](htcondor/part1-ex6-remove.md)
	- [Bonus Exercise 1.7: Explore `condor_q`](htcondor/part1-ex7-queue.md)
	- [Bonus Exercise 1.8: Explore `condor_status`](htcondor/part1-ex8-status.md)

!!! abstract "HTCondor Multiple Job Slides"
	View the Slides: [pdf](htcondor/files/osgus26-htc-htcondor-multiple-jobs.pdf)

!!! note "Exercises: Running Many HTC Jobs" 
	- [Exercise 2.1: Work with input and output files](htcondor/part2-ex1-files.md)
	- [Exercise 2.2: Use `queue N`, `$(Cluster)`, and `$(Process)`](htcondor/part2-ex2-queue-n.md)
	- [Exercise 2.3: Use `queue from` with custom variables](htcondor/part2-ex3-queue-from.md)
	- [Bonus Exercise 2.4: Use `queue matching` with a custom variable](htcondor/part2-ex4-queue-matching.md)
 
!!! example "HTC worksheet"
	- Intro slides: [pdf](htcondor/files/osgs26-intro-to-worksheet.pdf)
	- Worksheet: [pdf](htcondor/files/HTC-Workflow-Planning.pdf) or [Google Drive](https://docs.google.com/presentation/d/1USA6-qNur1Aa41pdvugJ7GqRKwHwnabTQ3dHVWtrlVY/)

## Software

!!! abstract "Software Slides"
	View the Slides: [pdf](software/files/osgs26-software.pdf) [pptx](software/files/osgs26-software.pptx)


!!! note "Software Exercises"
	- [Exercise 1.1 - Run and Explore Apptainer Containers](software/part1-ex1-run-apptainer.md)
	- [Exercise 1.2 - Use Apptainer Containers in OSPool Jobs](software/part1-ex2-apptainer-jobs.md)
	- [Exercise 1.3 - Use Docker Containers in OSPool Jobs](software/part1-ex3-docker-jobs.md)
	- [Exercise 1.4 - Build and Use an Apptainer Container](software/part1-ex4-apptainer-build.md)
	- [Exercise 2.1 - Choose Software Options](software/part2-ex1-software-strategies.md)
	- [Exercise 2.2 - Finding Containers](software/part2-ex2-find-containers.md)
	- [Exercise 2.3 - Apptainer Examples](software/part2-ex3-apptainer-examples.md)
	- [Exercise 2.4 - Apptainer Definition Files](software/part2-ex4-apptainer-definition.md)
	- [Exercise 2.5 - Example of Manual Installation](software/part2-ex5-manual-install.md)
	- [Exercise 3.1 - Build Your Own Docker Container](software/part3-ex1-docker-build.md)
	- [Exercise 4.1 - Build an HTC-Friendly Executable](software/part4-ex1-build-executable.md)

## The Open Science Pool (OSPool)

!!! abstract "OSPool Slides"
	View the slides: [pdf](ospool/files/osgs26-day2-part3-ospool-timc.pdf)

!!! note "Exercises: Researching the OSPool"
	- [Exercise 1.1: Where Do Jobs Run?](ospool/part1-ex1-where-run.md)
	- [Exercise 1.2: How Much Can I Get?](ospool/part1-ex2-capacity.md)
	- [Exercise 1.3: How Does Capacity Change?](ospool/part1-ex3-dynamic-capacity.md)
	- [Exercise 1.4: What Is In an Execution Point?](ospool/part1-ex4-ep-sandbox.md)
	- [Bonus Exercise 1.5: Viewing OSPool Information](ospool/part1-ex5-ospool-views.md)

## Data

!!! abstract "Data Slides"
	View the Slides: [pptx](data/files/osgus26-data.pptx)

!!! note "Data Exercises"
	- [Exercise 1.1: Understanding a job's data needs](data/part1-ex1-data-needs.md)
	- [Exercise 1.2: transfer\_input\_files, transfer\_output\_files, and remaps](data/part1-ex2-file-transfer.md)
	- [Exercise 1.3: Splitting input](data/part1-ex3-blast-split.md)
	- [Exercise 2.1: OSDF for inputs](data/part2-ex1-osdf-inputs.md)
	- [Exercise 2.2: OSDF for outputs](data/part2-ex2-osdf-outputs.md)

## Troubleshooting

!!! abstract "Troubleshooting Slides"
	View the Slides: [pdf](troubleshooting/files/osgs26-troubleshooting.pdf) [ppt](troubleshooting/files/osgs26-troubleshooting.pptx)

!!! note "Troubleshooting Exercises"
	- [Exercise 1.1: Troubleshooting Jobs](troubleshooting/part1-ex1-troubleshooting.md)
	- [Exercise 1.2: Job Retry](troubleshooting/part1-ex2-job-retry.md)

## Workflows with DAGMan

!!! abstract "DAGMAn Slides"
	View the slides: [pptx](workflows/files/osgs26-dagman.pptx), [pdf](workflows/files/osgs26-dagman.pdf)

!!! note "DAGMan Exercises"
	- [Exercise 1.1: Coordinating set of jobs: A simple DAG](workflows/part1-ex1-simple-dag.md)
	- [Exercise 1.2: A brief detour through the Mandelbrot set](workflows/part1-ex2-mandelbrot.md)
	- [Exercise 1.3: A more complex DAG](workflows/part1-ex3-complex-dag.md)
	- [Exercise 1.4: Handling jobs that fail with DAGMan](workflows/part1-ex4-failed-dag.md)
	- [Exercise 1.5: Workflow Challenges](workflows/part1-ex5-challenges.md)

## Biology Resources


## Machine Learning
!!! note "Machine Learning"
	View the slides: [pptx](ml/files/throughput_ml.pptx), [pdf](ml/files/throughput_ml.pdf)


## Research Computing Facilitation
!!! note "Research Computing Facilitation"
	View the slides: [pptx](facilitation/files/osgs26-facilitation.pptx), [pdf](facilitation/files/osgs26-facilitation.pdf)

## Final Talks

!!! abstract "Slides"	
	*   Philosophy: [Slides coming soon]
	*   Forward (Tim’s final talk): [pdf](final/files/osgs26-day5-part9-forward-timc.pdf)
