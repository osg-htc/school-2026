---
status: testing
---

<style type="text/css">
  pre em { font-style: normal; background-color: yellow; }
  pre strong { font-style: normal; font-weight: bold; color: \#008; }
</style>

# Bonus Workflows Exercise 1.5: YOUR Jobs and More on Workflows

Below are some optional challenges to improve your understanding of DAGMan and how to work with it to deploy workflows with HTCondor.

Challenge 1
-----------

Think about the calculations that you run for your research. 
You may be working on those calculations during the School this week.
Could DAGMan help you run those calculations?

In particular, do you have a calculation or set of calculations that you have to run **first** before you can run some other type of calculation?
Do you have multiple such sets of calculations?
What would the parent-child relationships look like for a DAG workflow applied to your situation?

If you have any "manual" checks for the success of your calculations, could you use DAGMan features like PRE or POST scripts to do those checks for you?

Challenge 2
-----------

Try to generate other Mandelbrot images. Some possible locations to look at with goatbroat:

``` console
goatbrot -i 1000 -o ex1.ppm -c 0.0016437219722,-0.8224676332988 -w 2e-11 -s 1000,1000
goatbrot -i 1000 -o ex2.ppm -c 0.3958608398437499,-0.13431445312500012 -w 0.0002197265625 -s 1000,1000
goatbrot -i 1000 -o ex3.ppm -c 0.3965859374999999,-0.13378125000000013 -w 0.003515625  -s 1000,1000
```

You can convert ppm files with `convert`, like so:

``` console
convert ex1.ppm ex1.jpg
```

Now make a movie! Make a series of images where you zoom into a point in the Mandelbrot set gradually. (Those points above may work well.) Assemble these images with the "convert" tool which will let you convert a set of JPEG files into an MPEG movie.


Challenge 3
-----------

Try out Pegasus. Pegasus is a workflow manager that uses DAGMan and can work in a grid environment and/or run across different types of clusters (with other queueing software). It will create the DAGs from abstract DAG descriptions and ensure they are appropriate for the location of the data and computation.

Links to more information:

-   [Pegasus Website](https://pegasus.isi.edu)
-   [Pegasus Documentation](https://pegasus.isi.edu/documentation)
-   [Pegasus on OSG](https://portal.osg-htc.org/documentation/htc_workloads/automated_workflows/tutorial-pegasus/)

If you have any questions or problems, please feel free to contact the Pegasus team by emailing <pegasus-support@isi.edu>


Challenge 4
-----------

In the exercise, individual but highly-similar submit files were used for every "goatbrot" job.
If there were 400 goatbrot jobs instead of 4, this would quickly become unwieldy and unnecessarily repetitive.

There is an option for use in the `.dag` file called `VARS` that allows you to reuse submit files across nodes while still passing unique per-job information. 
Can you find the information in the HTCondor manual about the DAGMan `VARS` feature? 
Then, can you update your DAG description so that there is only a single `goatbrot.sub` file used (instead of `goatbrot1.sub`, `goatbrot2.sub`, etc.)?


Challenge 5
-----------

You've done a 2x2 pattern as a proof of concept, maybe even a 3x3 pattern as part of an earlier challenge.
What if your advisor came back and said they wanted a 10x10 tiling, or to be able to change the center to an arbitrary value?

While you could make the changes to your DAG workflow by hand, it quickly becomes tedious and repetitive manual labor.
That's usually a sign that you should write a script to do it for you!

For this challenge, write a script that - in turn - writes the `.dag` and related submit files **for you** based on the parameters you think are interesting to change, but that you don't want to have to change by hand.

A good start is to make a second DAG workflow with a small version of the desired change incorporated, so that you can compare it to original 2x2 pattern.
What patterns do you notice when you write the second DAG?
What are the "defining values" that dictate the final result?

Next, write a `generate-DAG` script in your favorite programming language, which encodes the logic you identify and outputs the `.dag` and `.sub` files you need.

Confirm that the script will generate the same workflows you've already written (the original 2x2 and your exploratory example), then try it out for something bigger!
Do some spot checking to make sure it looks reasonable, then give it a shot.

