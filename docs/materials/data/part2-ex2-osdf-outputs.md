---
status: reviewed
---

Data Exercise 2.2: Using OSDF for outputs
=========================================================
This exercise uses a [Minimap2](https://github.com/lh3/minimap2) workflow to
demonstrate how OSDF can transfer job *outputs* directly into an OSDF-enabled Object Store.

As covered in the Data [lecture](files/osgus25-data.pdf), OSDF federated data network that is accessible to
the OSPool with built-in caching. For large files that many jobs read or write, OSDF reduces
load on the OSPool network and keeps copies at caching sites across the U.S., which speeds up
transfers to and from the Execution Points.

However, OSDF is not just for input files. It can also be used to store outputs from jobs, which is especially useful for large files that many jobs will read later. In this exercise, you will demonstrate how to use OSDF to store outputs from a job, so that later jobs can read them from them. You will build an indexed reference genome and, instead of returning it to the
Access Point, redirect it straight into OSDF so that later jobs can pull it from the nearest cache.

Setup
-----
- Make sure you are logged in to `ap40.uw.osg-htc.org`.
- Create a folder named `minimap2-read` and `cd` into it.
- Download the required files for this lesson:

        osdf object get /osg-public/school/2026/minimap2.sif .
        osdf object get /osg-public/school/2026/Celegans_ref.fa .

!!! note "File Sizes"
    - What are the sizes of the container and the reference genome file?


Prepare files for the job
--------------------------------
Minimap2 compares and aligns large pieces of genetic information, like DNA sequences. This is
useful for tasks like identifying similarities between genomes, verifying sequences, or assembling
genetic data from scratch. For this exercise, we will index the reference genome so that later
mapping jobs can align FASTQ reads against it quickly.


Place the container in OSDF
--------------------------------

The `minimap2.sif` container is used by every job and rarely changes, so it is a good candidate for
OSDF. Caching it near the Execution Points improves throughput across a large set of jobs.

>[!WARNING]
> OSDF caches files aggressively based on their name. Reusing a filename from a previous version can
> cause a job to download a stale copy. Use unique, version-controlled names such as
> `data_file_04JAN2025_version4.txt`, with the date of last update and a version identifier, so that
> HTCondor always pulls the file you expect from OSDF.

### Copy your data to the OSDF space

OSDF provides a per-user directory that is served through the caching servers. On
`ap40.uw.osg-htc.org`, that directory is `/ospool/ap40/data/[USERNAME]/`. Copy your `minimap2.sif`
image there:

        cp minimap2.sif /ospool/ap40/data/[USERNAME]/

Files placed in `/ospool/ap40/data/[USERNAME]/` are accessible only to your own jobs.

Note that we do **not** need to copy the indexed genome (`Celegans_ref.mmi`) into OSDF by hand — the
job produces it, and `transfer_output_remaps` will write it to OSDF for us.

Considerations
--------------
- Why is `minimap2.sif` placed in the OSDF directory?

Create a wrapper script and submit the job
---------------------------------------

We need a wrapper script and a submit file that use OSDF for both the container and the output.

1. Copy the following contents and save them as `minimap2_index.sh`:

        #!/bin/bash
        minimap2 -x map-ont -d Celegans_ref.mmi Celegans_ref.fa

2. Create `minimap2_index.sub` using either `vim` or `nano`:

```
        container_image        = osdf:///ospool/ap40/data/[USERNAME]/minimap2.sif

        executable             = ./minimap2_index.sh

        transfer_input_files   = Celegans_ref.fa

        transfer_output_files  = Celegans_ref.mmi
        transfer_output_remaps = "Celegans_ref.mmi = osdf:///ospool/ap40/data/[USERNAME]/Celegans_ref.mmi"
        output                 = indexing_step1_$(Cluster)_$(Process).out
        error                  = indexing_step1_$(Cluster)_$(Process).err
        log                    = indexing_step1_$(Cluster)_$(Process).log

        request_cpus           = 4
        request_disk           = 5 GB
        request_memory         = 5 GB

        queue 1
```


!!! info "Redirecting outputs to OSDF with transfer_output_remaps"

    By default, HTCondor returns every file listed in `transfer_output_files` to the directory you
    submitted the job from on the Access Point. `transfer_output_remaps` lets you send individual
    outputs somewhere else instead. Its syntax is a semicolon-separated list of `source = destination`
    pairs:

    ```
    transfer_output_remaps = "<file_on_execution_point> = <destination> ; <another_file> = <destination>"
    ```

    The destination can be a new local path/filename *or* a URL. When the destination is an
    `osdf://` URL, HTCondor uploads the file straight from the Execution Point into the OSDF-enabled
    Object Store (the OSDF origin) — it never lands on the Access Point at all. As with input URLs,
    there is no server name (three slashes in `:///`); the path is just the OSDF namespace
    (`/ospool/ap40/data/[USERNAME]/` here).

    In this job that means the freshly built `Celegans_ref.mmi` is written directly to your OSDF
    directory. Because it now lives in OSDF, the ScalingUp mapping exercises can read it back through
    the nearest cache with a plain `osdf:///ospool/ap40/data/[USERNAME]/Celegans_ref.mmi` in
    `transfer_input_files`, keeping the large index off the Access Point and out of the OSPool network
    path. (Remember the naming warning above: to overwrite the index later, give it a new versioned
    name so caches don't serve a stale copy.)

3. Submit your `minimap2_index.sub` job to the OSPool:

```
   condor_submit minimap2_index.sub
```

