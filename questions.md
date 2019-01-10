# Questions

## HPC good citizenship

1. On the UCI cluster, the resource request "-pe openmp 64" refers to the number of processors requested.  Does that
   request mean that your commands will use multiple processors?
   
   ***Not necessarily. In order to use multiple processors you have to run a multi-core program/command that is able to execute using multiple processors.***
   
2. In general, how do you know how many processors, how much RAM, how many files would be required/needed/written by the
   jobs you are running on the cluster?
   
   ***Use this command which will tell you all of the things in this question and more about your jobs:***
   
  ```
  $ /usr/bin/time -v <program> <args>
  ```   
   
3. In order to be a "good citizen", you need to have some idea of much RAM your job requires.  In particular, you need
   to know the "peak" (i.e., maximum) RAM required at any point during execution.  Show an example of the shell command
   that you would use on a Linux machine to measure run time and peak ram usage of an arbitrary command, where the time/peak RAM values are written to a file.
   
  ```
  $ /usr/bin/time -v ./test.sh | grep -e "User time (seconds):" -e "Maximum resident set size (kbytes):" > goodcitizen.txt
  ```
   
4. What are the units of your answer for number 3?

   ***seconds and kbytes*** 
   
5. What are the bash commands for the following operations:

   * Checking that a file exists
    ```
   FILE=$1     
   if [ -f $FILE ]; then
      echo "File $FILE exists."
   else
      echo "File $FILE does not exist."
   fi
   ```  
   * Checking that a file exists and is not empty
    
    ``` 
   FILE=$1
   if [[ -f $FILE && -s $FILE ]]; then 
    echo "exist and not empty"
   else 
    echo "not exist or empty"; 
   fi
    
    ```
    

6. How would you use the commands from your answer to 5 to write a work flow for HPC that only runs a job if the
   expected output file is **not** present.
   
   ***You could just replace the "echo" after the "else" with a command that will start your job running like qsub. This way it will only start the job if the output file is not present or is empty.
   
   ```
   FILE=$1
   if [[ -f $FILE && -s $FILE ]]; then 
    echo "exist and not empty"
   else 
    qsub ./test.sh; 
   fi 
   ```

## Trickier questions

7. Would your answer to number 3 work on Apple's OS X operating system?  If no, do you have any idea why not? 

   ***No, -v is not an option for /usr/bin/time in OSX. 

8. Most of the HPC nodes have 512Gb (gigabytes) of RAM. Let's say you have a job that will require **no more** than 24Gb
   of RAM.  How would you request resources so that you can run more than one job on a node **and** not cause nodes to
   crash?  Show an example of a skeleton HPC script as part of your answer.  **Knowing how to do this is super important
   and will save you loads of frustration and prevent you from taking out your colleagues' jobs on the cluster,
   preventing you from getting nasty emails from Harry!!!!!!!!!!!**
