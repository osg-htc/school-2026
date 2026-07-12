---
status: testing
---

<style type="text/css"> pre em { font-style: normal; background-color: yellow; } pre strong { font-style: normal; font-weight: bold; color: \#008; } </style>

Software Exercise 2.3: Apptainer examples
============================================================

**Objective**: Customize an existing apptainer definition file to create a conda, Python, R 
or Julia environment. 

**Why learn this?**: A large amount of research software is based on existing 
base languages (like `R` or `Python`), and these examples should be useful for 
getting started with building a custom container environment. 

Overview
------------

In this set of exercises, we have provided sample definition files as a starting 
point for building a container. In all these examples, each container starts with an existing container with the 
base language or package manager. 

After each example, we will include links to other, similar definition files, 
stored in CHTC's [Recipes Repository](https://github.com/CHTC/recipes). 

If you want to learn more about the components of a definition file, see 
the examples in [Example 2.4](part2-ex4-apptainer-definition.md). 

What to do
------------

1. Find the definition file that looks most similar to your software installation instructions. 
1. (Optional) Look at other examples in the CHTC `recipes` repository to see if there is another example that might be a better fit. 
1. Edit the definition file to install your needed packages or libraries. 
1. Try to build and test a container. 
	1. If you need to review the process for building and testing a container see [Exercise 1.4](part1-ex4-apptainer-build.md)

Sample definition files
------------

### Conda

The placeholder libraries in this example are `cowsay` and `fortunes`; replace 
the names of these libraries with the ones you want to install. 

```
Bootstrap: docker
From: continuumio/miniconda3:latest

%post
    conda install python=3.10
```

See other conda definition file examples here: [Conda definition files](https://github.com/CHTC/recipes/tree/main/software/Conda)

### R / tidyverse

The placeholder libraries in this example are `cowsay` and `fortunes`; replace 
the names of these libraries with the ones you want to install. 

Most R programs rely on libraries that are part of the tidyverse; if you are using 
any of the common tidyverse programs, you should use a definition file like this: 

```
Bootstrap: docker
From: rocker/tidyverse:4.3.1

%post
    R -e "install.packages(c('cowsay','fortunes'), dependencies=TRUE, repos='http://cran.rstudio.com/')"
```

If you aren't using any of the tidyverse packages, you can build 
on a base R package with this definition file: 

```
Bootstrap: docker
From: rocker/r-ver:4.3.1

%post
    R -e "install.packages(c('cowsay','fortunes'), dependencies=TRUE, repos='http://cran.rstudio.com/')"
```

See other R definition file examples here: [R definition files](https://github.com/CHTC/recipes/tree/main/software/R)

### Python / pip

The placeholder libraries in this example are `scipy` and `cowsay`; replace 
the names of these libraries with the ones you want to install. 

```
Bootstrap: docker
From: python:3.11

%post
    python3 -m pip install scipy cowsay
```

See other Python definition file examples here: [Python definition files](https://github.com/CHTC/recipes/tree/main/software/Python)

### Julia

The placeholder libraries in this example are `Cowsay` and `DataFrames`; replace 
the names of these libraries with the ones you want to install. 

```
Bootstrap: docker
From: julia:1.10

%post
        export JULIA_DEPOT_PATH="/opt/julia"

        # Install your packages using the following command:
        julia -e 'using Pkg; Pkg.add(["Cowsay", "DataFrames"]); Pkg.instantiate(); Pkg.precompile()'

%environment
        export JULIA_DEPOT_PATH=":/opt/julia"
```

See other Julia definition file examples here: [Julia definition files](https://github.com/CHTC/recipes/tree/main/software/Julia)
