---
status: testing
---

<style type="text/css">
  pre em { font-style: normal; background-color: yellow; }
  pre strong { font-style: normal; font-weight: bold; color: \#008; }
</style>

Software Exercise 2.1 - Choose Software Options
============================================================

**Objective**: Decide how you want to make your software portable

**Why learn this?**: This is the next step to getting your own 
research jobs running on the OSPool! 

Know your software
------------------

Pick at least one software you want to use on the OSPool as a test subject. 

!!! question "What if I don't have a specific software?"
	If you came to the OSG School without a specific project, never fear! Pick a 
	software from the list below for the next set of exercises: 
	
	* GROMACS
	* minimap2
	* Pytorch

Using either existing code (if you wrote it) or the download/installation page (if someone 
else wrote it), try to answer the following questions: 

1. What options exist for installation and/or use? 
	1. Is the software already available as a container? 
	1. Can you install the software via a tool like `conda` or R's `install.packages()`? 

1. What dependencies does the software have? 
	1. Does it depend on a certain version of (say) Python or R? 
	1. Are certain packages or add-ons needed? 

	> Example 1: an R package will require a base R installation
	> 
	> Example 2: some codes require that a library called the "Gnu Scientific
	Library (GSL) be already installed on your computer"

1. Once you've identified this information, copy the template in 
 [this Google document](https://docs.google.com/document/d/1WpV1mJ8uJQEd6asHUuvWjN28qTss281CPiL-8uG_eL0/edit?tab=t.p5fk07rmi8dk) and 
 fill out the first four rows of the table with as much information as you can. 

Picking an approach
---------------------

1. Read through the approaches listed below. 
1. Look at the exercises recommended for each approach and try to implement a job that runs the software! 
1. Add notes to your table in the [Google Doc](https://docs.google.com/document/d/1WpV1mJ8uJQEd6asHUuvWjN28qTss281CPiL-8uG_eL0/edit?tab=t.p5fk07rmi8dk) with what you're going to try. 

Side-quest: Create an executable
---------------------

Regardless of which approach you use, check out 
the [Build an HTC-Friendly Executable](part4-ex1-build-executable.md) exercise
for some tips on how to make your script more robust and easy to use with multiple jobs. 

Try this first: existing container
------------------------------

The first approach to try is using an existing container. 

<table>
 <tr>
  <td>
This approach may work if: 
  </td>
  <td>
<ul>
<li>you know that your software has an existing container implementation (Docker or Apptainer) OR</li>
<li>you are running a simple code with few dependencies that will likely run in a "stock" environment OR</li>
<li>you are using an extremely common software tool like pytorch or the standard tidyverse</li>
</ul>
  </td>
 </tr>
 <tr>
  <td>
This approach may not work if: 
  </td>
  <td>
<ul>
<li>your code needs a particular set of packages or libraries or</li>
<li>the software does not have an existing container deployment OR</li>
<li>the software is otherwise rare or poorly supported.</li>
</ul>
  </td>
 </tr>
</table>

If this is you, do the following: 

1. Go through [Exercise 2.2](part2-ex2-find-containers.md).
1. Add notes to the [Google Doc](https://docs.google.com/document/d/1WpV1mJ8uJQEd6asHUuvWjN28qTss281CPiL-8uG_eL0/edit?tab=t.p5fk07rmi8dk)

Try this next: adding packages to a container
------------------

If you can't find an existing container, you'll need to build one. The first level 
of building containers is adding specific packages to an existing base. 

<table>
 <tr>
  <td>
This approach may work if: 
  </td>
  <td>
<ul>
<li>Your software can be installed as a conda environment OR</li>
<li>Your software is in a common scripting language (e.g. R, Python, Julia) and uses packages that can be installed with the standard commands for that language</li>
</ul>
  </td>
 </tr>
 <tr>
  <td>
This approach may not work if: 
  </td>
  <td>
<ul>
<li>your code is using experimental packages or libraries</li>
<li>the software is not available as a single package or environment</li>
</ul>
  </td>
 </tr>
</table>

If this is you: 

1. Try the examples in [Exercise 2.2](part2-ex3-apptainer-examples.md) to customize 
existing templates for `conda`, `R` and `Python` based codes. 
1. Add notes to the [Google Doc](https://docs.google.com/document/d/1WpV1mJ8uJQEd6asHUuvWjN28qTss281CPiL-8uG_eL0/edit?tab=t.p5fk07rmi8dk)


!!! info "Docker vs Apptainer?"
	Most of our examples will show how to build apptainer containers, as we think 
	a) they are easier to build for beginners and b) they work on the OSPool. However, 
	if you want to be able to use a container on your own computer, or share with 
	collaborators, Docker containers may be a better option. See [Exercise 3.1](part3-ex1-docker-build.md)
	for more information. 

Try this third: Container builds with more components
------------------

Some software have more complex installation instructions. You will still be 
building a container in this case, but the basic definition file will look different. 

<table>
 <tr>
  <td>
This approach may work if: 
  </td>
  <td>
<ul>
<li>you are running any code that has installation instructions.</li>
</ul>
  </td>
 </tr>
 <tr>
  <td>
This approach may not work if: 
  </td>
  <td>
<ul>
<li>It's unlikely that this approach will fail!!</li>
</ul>
  </td>
 </tr>
</table>

If this is you, we recommend doing two things: 

1. Look at some of the examples from [Exercise 2.3](part2-ex3-apptainer-examples.md), to 
try to build your own understanding of the pieces of a definition file. 
1. Then go to [Exercise 2.4](part2-ex4-apptainer-definition.md) and do the exercises there. If 
you get stuck or have questions, flag down a staff member or talk to a neighbor! 
1. Add notes to the [Google Doc](https://docs.google.com/document/d/1WpV1mJ8uJQEd6asHUuvWjN28qTss281CPiL-8uG_eL0/edit?tab=t.p5fk07rmi8dk)


Apply to Your Work
------------------

This whole page is about applying the previous exercises to your work 🙂.
