# Bonus HTC Exercise 2.4: Submit With “queue matching”

## Exercise Goal

Now that you've mastered many `queue` statements, it's time to learn another powerful `queue` statement!

**The goal of this exercise is to submit many jobs from a single submit file by using the `queue ... matching` syntax.** With this, you can submit jobs with variables that match files with a specified pattern!

## Counting Words in Files

Returning to our book word-counting example, let's pretend that instead of
three books, we have an entire library. While we could list all of the text
files in a `books.txt` file and use `queue book from books.txt`, it could be a
tedious process, especially for tens of thousands of files. Luckily HTCondor
provides a mechanism for submitting jobs based on pattern-matched files.

## Queue Jobs By Matching Filenames

This is an example of a common scenario: We want to run one job per file, where the filenames match a certain consistent pattern. The `queue ... matching` statement is made for this scenario.

Let’s see this in action. First, here is a new version of the script (note, we removed the 'top n words' restriction):

``` python
#!/usr/bin/env python3

import os
import sys
import operator

if len(sys.argv) != 2:
    print(f'Usage: {os.path.basename(sys.argv[0])} DATA')
    sys.exit(1)
input_filename = sys.argv[1]

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
for word in sorted_words:
    print(f'{word[0]} {word[1]:8d}')
```

To use the script:

1.  Create and save this script as `wordcount.py`.
1.  Verify the script by running it on one book manually.
1.  Create a new submit file to submit one job (pick a book file and model your submit file off of the one above).
1.  Modify the following submit file statements to work for all books:

        :::text
        transfer_input_files = $(book) 
        arguments = $(book) 
        output = $(book).out 
        error = $(book).err 
        queue book matching *.txt

    !!!note
        As always, the order of statements in a submit file does not matter, except that the `queue` statement should be last. Also note that any submit file variable name (here, `book`, but true for `process` and all others) may be used in any mixture of upper- and lowercase letters.

1.  Submit the jobs.

HTCondor uses the `queue ... matching` statement to look for files in the submit directory that match the given pattern, then queues one job per match. For each job, the given variable (e.g., `book` here) is assigned the name of the matching file, so that it can be used in `output`, `error`, and other statements.

!!! question "Questions"
    How many jobs were created? Is this what you expected?

## Using "queue matching" with files in subdirectories

!!! danger "Beware the dangers of matching more files than intended!"
    
    If you ran this in the same directory as Exercise 2.3, you may have noticed that a job was submitted for the `books_n.txt` file that holds the variable values in the `queue from` statement.

One solution to avoid the above situation may be to put all of the books into an `books` directory and `queue book matching books/*.txt`. However, note that when doing this, the `$(book)` variable *includes the slash*.

For example, `$(book)` takes on the value `books/Alice_in_Wonderland.txt`.

How would this affect each of the the following lines in your submit file?

- `shell = ./wordcount.py $(book)`
- `transfer_input_files = $(book)`
- `output = wordcount_$(book).out`

### Using `$BASENAME` and `$Fn`

HTCondor has a few convenient macros. We'll look at two, `$BASENAME` and `$Fn`.

1. `$BASENAME` refers to a filename at the end of any path, with the extension.
1. `$Fn` refers to a filename at the end of any path, without the extension.[^1]
[^1]: If your extension has more than one period (i.e., `.tar.gz`, you will need to use a variation on the `$BASENAME` macro. See [HTCondor docs](https://htcondor.readthedocs.io/en/latest/admin-manual/introduction-to-configuration.html#function-macros-in-configuration) for more information.)

For example, if the value of `$(book)` is `books/Alice_in_Wonderland.txt`, our macros have these values:

| Macro | Value |
| --- | --- |
| `$BASENAME(book)` | `Alice_in_Wonderland.txt` |
| `$Fn(book)` | `Alice_in_Wonderland` |

1. Move all of your book files into a subdirectory called `books`.
1. Rewrite your submit file to use the `$BASENAME` and/or `$Fn` macros to submit multiple jobs.
1. Monitor your jobs. Did you get the expected outputs?

Read more about the `$BASENAME` macro in the [HTCondor manual](https://htcondor.readthedocs.io/en/latest/admin-manual/introduction-to-configuration.html#function-macros-in-configuration) for more extensive options.

## Extra Challenge 1

In the example above, you used a single log file for all three jobs. HTCondor handles this situation with no problem; each job writes its events into the log file without getting in the way of other events and other jobs. But as you may have seen, it may be difficult for a person to understand the events for any particular job in the combined log file.

Create a new submit file that works just like the one above, except that each job writes its own log file.

## Extra Challenge 2

Between this exercise and the previous one, you have explored two of the three primary `queue` statements. How would you use the `queue in ... list` statement to accomplish the same thing(s) as one or both of the exercises?

