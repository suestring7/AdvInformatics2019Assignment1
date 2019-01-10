# Questions

## HPC good citizenship

1. On the UCI cluster, the resource request "-pe openmp 64" refers to the number of processors requested.  Does that
   request mean that your commands will use multiple processors?

* No. It depends on the command or software you use. Although there are some software that can run in parallel by default. Most software would require you to mannually set them to run in parallel and tell them the number of processors. You need to read the documentations carefully.

2. In general, how do you know how many processors, how much RAM, how many files would be required/needed/written by the
   jobs you are running on the cluster?

* Read documentations carefully and if it is the case of poorly documented, run some small test jobs to help figure the answer.

3. In order to be a "good citizen", you need to have some idea of much RAM your job requires.  In particular, you need
   to know the "peak" (i.e., maximum) RAM required at any point during execution.  Show an example of the shell command
   that you would use on a Linux machine to measure run time and peak ram usage of an arbitrary command, where the time/peak RAM values are written to a file.

* Given a Linux machine, the shell command I would use to measure run time and peak ram usage would be `env time -v -o [OUTPUT] [COMMAND]`
An example is shown below with the time/peak RAM values **bolded**.
```
env time -v -o test.txt qstat
cat test.txt
        Command being timed: "qstat"
        User time (seconds): 0.04
        **System time (seconds): 0.01**
        Percent of CPU this job got: 48%
        Elapsed (wall clock) time (h:mm:ss or m:ss): 0:00.11
        Average shared text size (kbytes): 0
        Average unshared data size (kbytes): 0
        Average stack size (kbytes): 0
        Average total size (kbytes): 0
        **Maximum resident set size (kbytes): 15268**
        Average resident set size (kbytes): 0
        Major (requiring I/O) page faults: 0
        Minor (reclaiming a frame) page faults: 2820
        Voluntary context switches: 94
        Involuntary context switches: 2
        Swaps: 0
        File system inputs: 8
```

4. What are the units of your answer for number 3?

* The unit for time is seconds and the unit for peak RAM is kbytes. All the format are written clearly by the default output.

5. What are the bash commands for the following operations:

    * Checking that a file exists
        * `[ -e $filename ]`
    * Checking that a file exists and is not empty
        * `[ -s $filename ]`

6. How would you use the commands from your answer to 5 to write a work flow for HPC that only runs a job if the
   expected output file is **not** present.

* `[ -e $filename ] || RunCommand`

## Trickier questions

7. Would your answer to number 3 work on Apple's OS X operating system?  If no, do you have any idea why not? 

* No. Since OS X and Linux are different branches of Unix, some of the shell command could remain the same function or name but most of them differ in a lot of things. In my OS X, I could still use `env time [COMMAND]`, but the output is different and the options are different as well. Not sure if it was the built-in shell command or something I installed later.

8. Most of the HPC nodes have 512Gb (gigabytes) of RAM. Let's say you have a job that will require **no more** than 24Gb
   of RAM.  How would you request resources so that you can run more than one job on a node **and** not cause nodes to
   crash?  Show an example of a skeleton HPC script as part of your answer.  **Knowing how to do this is super important
   and will save you loads of frustration and prevent you from taking out your colleagues' jobs on the cluster,
   preventing you from getting nasty emails from Harry!!!!!!!!!!!**

```
#!/bin/bash
#$ -q pub*,free*
#$ -l mem_size=24G
```

* The script fully relies on the scheduler's ability to schedule the job properly. In real world, I usually require more mem_size and even more cores for the job to prevent crashing and conflicts. For example:

```
#!/bin/bash
#$ -q pub64,free64
#$ -l mem_size=25G
#$ -pe openmp 4
```

