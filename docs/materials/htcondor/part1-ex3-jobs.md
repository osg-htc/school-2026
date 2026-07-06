<style type="text/css"> pre em { font-style: normal; background-color: yellow; } pre strong { font-style: normal; font-weight: bold; color: \#008; } </style>

# HTC Exercise 1.3: Run Jobs!

## Exercise Goal

Now that we've seen what resources are available on the OSPool, we're going to start submitting our own jobs!

**The goal of this exercise is to transform a list of computational tasks into a format that HTCondor understands.** This is done by writing job submit files. You can give ("submit") these files to HTCondor, which then has the responsibility for running them on available capacity. Learning to write submit files is a huge step in learning to use an HTC system!

!!! note
    This exercise will take longer time than the first two, short ones. If you are having any problems getting the jobs to run, please ask the instructors! It is critical that you know how to run jobs.


## A List of Computational Tasks

Imagine that you're a researcher who wants to collect information about the slots in the OSPool. You decide to do this by writing a script that collects information about the current machine into a CSV file. Below is the script you write:

```
#!/bin/bash

echo "Date,Hostname,System,Directory,OSG Site Name"
echo "$(date),$(hostname),$(uname -spo),$(pwd),${OSG_SITE_NAME}"

```

You don't need to understand everything the script is doing to complete the exercise.

Since this script is not computationally heavy, we can test this script on the Access Point. If you cannot run your script from the command line, HTCondor probably cannot run it on another machine. It's important to test your code before you submit it as a job, but be aware that you should *never* run compute-heavy code directly on the Access Point.

### Create and test our executable script

Create a directory on `ap40` for this exercise using the `mkdir` command. Give it a name that makes sense to you.

In this new directory, save the script above as `slotinfo.sh` using your favorite command line text editor (i.e. `nano`, `vim`, `emacs`). If you type `ls -lh`, you'll see some detailed information about our script.

```
[user@ap40 htc_1.3_submitjobs]$ ls -lh
total 4.0K
-rw-r--r-- 1 user user 130 Jul  3 13:39 slotinfo.sh
```

The very left column (`-rw-r--r--`) shows the permissions of our files. The 2nd through 4th characters (`rw-`) tell us that we can read and write this file, but not execute it.

With the `chmod` command, change the permissions of the script so it's executable.

```
[username@ap40 htc1-3]$ chmod +x slotinfo.sh
```

!!! question
    What changes when you run `ls -lh` again?

Execute the script and observe the output printed to the screen.

```
[username@ap40 htc1-3]$ ./slotinfo.sh
```

!!! question
    What information is printed? How is this information organized?

Careful observers might note that we're missing the output of `${OSG_SITE_NAME}`. This is an environment variable unique to jobs in the OSPool, but is not available on the Access Point.

## Running Your First List of Jobs

Now that we've confirmed that our script works, we're ready to deploy a small-scale test on the OSPool! But to do so, we'll need to translate our task into a language that HTCondor understands - this "translation" is given through the job submit file.

When you want to run HTCondor jobs, you will first need to write an HTCondor submit file for them. In this exercise, you will run the same `slotinfo.sh` script as we did above, but this time, the command will run within jobs on *Execution Points* (aka a slot) in the OSPool, instead of the Access Point.

First, create a submit file called `slotinfo.sub` using your favorite text editor and then transfer the following information to that file:

``` file
shell = ./slotinfo.sh
transfer_input_files = slotinfo.sh

output = slotinfo_$(Process).out
error = slotinfo_$(Process).err
log = slotinfo_$(Cluster).log

request_cpus = 1
request_memory = 5 MB
request_disk = 2 MB

queue 50
```

Save your submit file using the name `slotinfo.sub`.

!!! note
    You can name the HTCondor submit file using any filename. 
    It's a good practice to always include the `.sub` extension, but it is not required. 
    This is because the submit file is a simple text file that we are using to pass information to HTCondor.

The lines of the submit file have the following meanings:

| Submit Command  | Explanation                                                                                                                                                                |
|:----------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `shell`    | The command we want to run. |
| `transfer_input_files` | The files we need as input for our job. |
| `output`        | The filename where HTCondor will write the standard output from your jobs (messages printed to the screen). |
| `error`         | The filename where HTCondor will write the standard error from your jobs (error messages printed to the screen). This particular jobs is not likely to have any, but it is best to include this line for every job. |
| `log`           | The filename where HTCondor will write information about your jobs. While not strictly required, we recommend having a log file for every submission. |
| `request_*`     | How many `cpus`, `memory`, and `disk` we want. In this case, we don't need much because the script is small and isn't computationally intensive. |
| `queue`         | Tells HTCondor to run your jobs with the settings above. In this example, we will run 50 instances of the job. |

!!! note "What are `$(Process)` and `$(Cluster)`?"
    Remember the job IDs we saw in the last exercises with `condor_status` and `condor_q`?
    
    The `$(Process)` and `$(Cluster)` variables are used by HTCondor that are related to your jobs' IDs. Once you submit jobs, HTCondor will automatically populate these variables with the actual cluster and process numbers.

Double-check your submit file, so that it matches the text above. Then, tell HTCondor to run your jobs with the `condor_submit` command:

``` console
[username@ap40 ~]$ condor_submit slotinfo.sub
Submitting job(s).
50 job(s) submitted to cluster NNNN.
```

The actual cluster number will be shown instead of `NNNN`. **If, instead of the text above, there are error messages, read them carefully and then try to correct your submit file or ask for help.**

Notice that `condor_submit` returns back to the shell prompt right away. It does **not** wait for your jobs to run. Instead, as soon as it has finished submitting your jobs into the queue, the submit command finishes.

### View your jobs in the queue

Try these commands to view your jobs in the queue:

```
[user@ap40 ~] condor_q
[user@ap40 ~] condor_q -nobatch
[user@ap40 ~] condor_watch_q
```

You may not even catch the jobs in the `R` running state, because the `slotinfo.sh` script runs very quickly. When the jobs are finished, it will 'leave' the queue and no longer be listed in the `condor_q` output.

After the jobs finish, take a look at your working directory with `ls -lh`. You'll see some newly generated files, including `.out`, `.err`, and `.log` files. Let's check the contents of the `.out` files, which is where job information printed to the terminal screen will be printed for the jobs.

``` console
[username@ap40 ~]$ cat slotinfo_0.out
Date,Hostname,System,Directory,OSG Site Name
Tue May 27 21:25:49 UTC 2025,osgvo-docker-pilot-7646558687-fqlp8,Linux x86_64 GNU/Linux,/srv,CHTC
```

Since looking at each `.out` file individually is not efficient, we can concatenate the results in a single `.csv` file with a few shell commands:

```
[username@ap40 htc1-3]$ head -n 1 slotinfo_0.out > slotinfo_combined.csv
[username@ap40 htc1-3]$ tail -q -n +2 slotinfo_*.out >> slotinfo_combined.csv
```

Take a look at `slotinfo_combined.csv`. What information did we collect? What details stand out? Did the jobs execute around the same time or at different times?

The `.err` file should be empty, unless there were issues running the `slotinfo.sh` executable after it was transferred to the slot. The `.log` file is more complex and will be the focus of a later exercise.

## Running Jobs With Arguments

Very often, when you run a command on the command line, it includes arguments (i.e. options) after the program name.

In an HTCondor submit file, the program (or 'executable') name goes in the `executable` statement and **all remaining arguments** go into an `arguments` statement. For example, if we have a script that multiplies all following numbers together:

``` console
[username@ap40 ~]$ ./multipy.sh 1 2 3
6
```

`multiply.sh` is the *executable* and `1 2 3` are the *arguments*.

In the submit file, we can specify this in two ways. View the two examples in the tabs.

=== "Option 1: Using `shell`"

    ``` file
    shell = ./multiply.sh 1 2 3
    transfer_input_files = multiply.sh
    ```

=== "Option 2: Using `executable` and `arguments`"

    ``` file
    executable = multiply.sh
    arguments = 1 2 3
    ```

* Note that the `shell` example is written *exactly* how you might execute the task on your own computer. You *must* transfer the executable script using `transfer_input_files`, if you use this option.

* If you use the `executable` and `arguments` option, the executable script (i.e., `multiply.sh`) is automatically transferred by HTCondor and does not need to be lincluded in the `transfer_input_files` line.

Let’s try a job submission with arguments. We will revisit our `slotinfo.sh` script. If you check the time zones of the outputs of each job, you may have a heterogenous mix of time zones—some may be in UTC, some in CDT, EDT, MDT, PDT, etc.

This isn't very helpful for comparison between jobs! Let's have our script take in a time zone as an argument so that all times are listed in that time zone.

Create a new script called `tz_slotinfo.sh` with the following content:

```
#!/bin/bash

if [ "$#" -ne 1 ]; then
    echo "Usage: $0 [time zone]"
    exit
fi

echo "Date,Hostname,System,Directory,OSG Site Name"
echo "$(TZ="${1}" date),$(hostname),$(uname -spo),$(pwd),${OSG_SITE_NAME}"

```

Make sure the script is executable, and test it using `America/Los_Angeles` as the argument.
```
[username@ap40 htc1-3]$ ./tz_slotinfo.sh America/Los_Angeles
```

Since we're in Wisconsin, we're going to use the `America/Chicago` time zone for our jobs. Create a new submit file and save the following text in it.

``` file
shell = ./tz_slotinfo.sh America/Chicago
transfer_input_files = tz_slotinfo.sh

output = tz_slotinfo_$(Process).out
error = tz_slotinfo_$(Process).err
log = tz_slotinfo_$(Cluster).log

request_cpus = 1
request_memory = 5 MB
request_disk = 3 MB

queue 50
```
You can save the file using any name, but as a reminder, we recommend it uses the `.sub` file extension. 

Except for changing a few filenames, this submit file is nearly identical to the last one, except for the addition of "America/Chicago" as an argument in the `shell` line.

Submit these new jobs to HTCondor. Again, watch for it to run using `condor_watch_q`. When the jobs finish, it will disappear from the queue and create `.out` and `.err` files.

!!! question
    After checking that there were no errors, concatenate the files into a new `.csv` file and view it. Did we get the time zone we expected for all jobs?

## Extra Challenge 1

!!! note
    There are Extra Challenges throughout the school curriculum. You may be better off coming back to these after you've completed all other exercises for your current working session.

Create and submit a second submit file for the `tz_slotinfo.sh` job, but use `executable` and `arguments` instead of `shell`.

## Extra Challenge 2

Below is a Python script that does something similar to the shell script above. Write a submit file and submit a job that runs this Python script.

```python
#!/usr/bin/env python3

"""Extra Challenge for OSG School
Written by Tim Cartwright
"""

import getpass
import os
import platform
import socket
import sys
import time

arguments = None
if len(sys.argv) > 1:
    arguments = '"' + ' '.join(sys.argv[1:]) + '"'

print(__doc__, file=sys.stderr)
print('Time    :', time.strftime('%Y-%m-%d (%a) %H:%M:%S %Z'))
print('Host    :', getpass.getuser(), '@', socket.gethostname())
uname = platform.uname()
print("System  :", uname[0], uname[2], uname[4])
print("Version :", platform.python_version())
print("Program :", sys.executable)
print('Script  :', os.path.abspath(__file__))
print('Args    :', arguments)
```
