# Questions

## HPC good citizenship

1. On the UCI cluster, the resource request "-pe openmp 64" refers to the number of processors requested.  Does that
   request mean that your commands will use multiple processors?
   
   ***No it all depends on the program you are using. If the progam you are using was built to be greedy and uses all available resources it will. Some programs are not built to run on multiple cores at all. Those that are you usually need to specify the number of core to use. (there will be a flag to say how many cores to use)***
   
2. In general, how do you know how many processors, how much RAM, how many files would be required/needed/written by the
   jobs you are running on the cluster?
   
   ***I would use ```/usr/bin/time -v COMMAND``` This will tell you a the processor and ram used. Along with files read/written and output size.*** 

   
   
3. In order to be a "good citizen", you need to have some idea of much RAM your job requires.  In particular, you need
   to know the "peak" (i.e., maximum) RAM required at any point during execution.  Show an example of the shell command
   that you would use on a Linux machine to measure run time and peak ram usage of an arbitrary command, where the time/peak RAM values are written to a file.
   
   ***Something like this should work***
```{bash}
$ /usr/bin/time -v COMMAND 2>&1 | grep -e "Maximum resident set size (kbytes):" -e "User time (seconds):" > stats.txt
```
   
4. What are the units of your answer for number 3?

***bytes and seconds***

5. What are the bash commands for the following operations:

    * Checking that a file exists
    
    ``` [ -f "FILENAME" ] && echo "The file exists" || echo "The file does not exist" ```
    
    * Checking that a file exists and is not empty
    
    ``` [ -s "FILENAME" ] && echo "The file exists and is not empty" || echo "The file does not exist, or is empty" ```

6. How would you use the commands from your answer to 5 to write a work flow for HPC that only runs a job if the
   expected output file is **not** present.
   
   ```if [ -e "FILENAME" ]; then echo "you already did this"; else qsub script.sh;  fi```
   
   

## Trickier questions

7. Would your answer to number 3 work on Apple's OS X operating system?  If no, do you have any idea why not? 

***Didn't work for me. Not exactly sure why, but I'm sure I would need to install some things to make OS X behave more similar to a pure linux system***

8. Most of the HPC nodes have 512Gb (gigabytes) of RAM. Let's say you have a job that will require **no more** than 24Gb
   of RAM.  How would you request resources so that you can run more than one job on a node **and** not cause nodes to
   crash?  Show an example of a skeleton HPC script as part of your answer.  **Knowing how to do this is super important
   and will save you loads of frustration and prevent you from taking out your colleagues' jobs on the cluster,
   preventing you from getting nasty emails from Harry!!!!!!!!!!!**
   
   
   **You can limit your resources using the (#$ -l resource=). If you run multiple jobs on the HPC you could set it up as an array and only submit a few at a time (#$ -tc).***
   
   
```
#!/bin/bash
#$ -N TEST
#$ -q bio
#$ -pe openmp 4
#$ -l mem_size=24G
#$ -t 1-1000
#$ -tc Some_number
```
   
