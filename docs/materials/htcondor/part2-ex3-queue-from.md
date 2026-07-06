# HTC Exercise 2.3: Submit with “queue from”

## Exercise Goals

While using `queue *N*`, `$(Cluster)`, and `$(Process)` can be useful for some jobs, it's not great for all jobs. What if you want to submit the same task for differently named files? What if you want to iterate over non-numerical parameters?

In the next two exercises, you will explore more ways to use a single
submit file to submit many jobs. **The goal of this exercise is to submit many
jobs from a single submit file by using the `queue ... from` syntax to read
variable values from a file.**

## Background

When submitting many jobs from a single submit file, always consider:

-   What makes each job unique? For example, there is one job per data file or parameter?
-   How should you tell HTCondor to distinguish each job?

For `queue *N*`, jobs are distinguished by the built-in `$(Process)` variable. But what if you need something more customized or something that's not a number? HTCondor can do that! HTCondor can distinguish and submit multiple jobs using *custom* variables.

## Example: Counting Words in Files

Imagine you have a collection of books, and you want to analyze how word usage varies from book to book or author to author. You could create separate submit files for each book and submit all of the files manually, but you'd have to edit the file each time.

Below is an example submit file for this task. If we created a submit file for each text, we'd have to change the last five lines above the `queue` statement for each text we want to analyze.

``` file
shell                   = ./freq.py Alice_in_Wonderland.txt
request_memory          = 20MB
request_disk            = 20MB

transfer_input_files = freq.py, Alice_in_Wonderland.txt
output               = Alice_in_Wonderland.out
error                = Alice_in_Wonderland.err
log                  = Alice_in_Wonderland.log

queue
```

This would be overly verbose and tedious! Let's see a better way to automate this.

## Queue Jobs From a List of Values

Suppose we want to modify our word-frequency analysis from a previous exercise so that it outputs **only the top *N* most common words of a document**. However, we want to experiment with different values of *N*. 

For this analysis, we will have a new version of the word-frequency counting
script. First, we need a new version of the word counting program so that it
accepts an extra number as a command line argument and prints only that many
of the most common words. For example, if we run:

```
./wordcount-top-n.py Alice_in_Wonderland.txt 25
```

it should print out the top 25 frequently used words.

Here is the new code, `wordcount-top-n.py` (it's not necessary to understand this code):

``` python
#!/usr/bin/env python3

import os
import sys
import operator

if len(sys.argv) != 3:
    print(f'Usage: {os.path.basename(sys.argv[0])} DATA NUM_WORDS')
    sys.exit(1)
input_filename = sys.argv[1]
num_words = int(sys.argv[2])

words = {}

with open(input_filename, 'r') as my_file:
    for line in my_file:
        line_words = line.split()
        for word in line_words:
            if word in words:
                words[word] += 1
            else:
                words[word] = 1

sorted_words = sorted(words.items(), key=operator.itemgetter(1))
for word in sorted_words[-num_words:]:
    print(f'{word[0]} {word[1]:8d}')
```

To submit this program with a collection of two variable values for each run, one for the number of top words and one for the filename:

1.  Save the script as an executable file `wordcount-top-n.py`.
1.  Download and unpack some books from Project Gutenberg:

        :::console
        [username@ap40]$ wget https://osg-htc.org/school-2026/docs/materials/htcondor/files/books.tar.gz
        [username@ap40]$ tar -xzvf books.tar.gz

1.  Create a new submit file (or base it off a previous one!) named `wordcount-top.sub` with **memory and disk requests of 20 MB.**
1.  Update other statements to work with two variables, `book` and `n`:

        :::file
        shell = ./wordcount-top-n.py $(book) $(n)
        transfer_input_files = wordcount-top-n.py, $(book)

        log = wordcount-top-n.log
        output = $(book)_top_$(n).out 
        error = $(book)_top_$(n).err 
        
        queue book, n from books_n.txt

    Note especially the changes to the `queue` statement; it now tells HTCondor to read a separate text file of ***pairs*** of values, which will be assigned to `book` and `n` respectively.

1.  Create the separate text file of job variable values and save it as `books_n.txt`:

        :::file
        Alice_in_Wonderland.txt, 10 
        Alice_in_Wonderland.txt, 25 
        Alice_in_Wonderland.txt, 50 
        Pride_and_Prejudice.txt, 10 
        Pride_and_Prejudice.txt, 25 
        Pride_and_Prejudice.txt, 50
        Dracula.txt, 10
        Dracula.txt, 25
        Dracula.txt, 50
        Huckleberry_Finn.txt, 10
        Huckleberry_Finn.txt, 25
        Huckleberry_Finn.txt, 50

    Note that we used 3 different values for *n* for each book.

1.  Submit the file.
1.  Do a quick sanity check: How many jobs were submitted? How many log, output, and error files were created?

Extra Challenge
-----------------

You may have noticed that the output of these jobs has a messy naming convention. Because our macros resolve to the filenames, including their extension (e.g., `Alice_in_Wonderland.txt.txt`), the output filenames contain with multiple extensions (e.g., `Alice_in_Wonderland.txt.txt.err`). Although the extra extension is acceptable, it makes the filenames harder to read and possibly organize. *Change your submit file and variable file for this exercise so that the output filenames do not include the `.txt` extension.*

