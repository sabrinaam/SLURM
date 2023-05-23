
## Introduction

The SLURM Workload Manager (formally known as Simple Linux Utility for Resource Management or SLURM), or SLURM, is a free and open-source job scheduler for Linux and Unix-like kernels, used by many of the world's supercomputers and computer clusters.
It provides three key functions.
- First, it allocates exclusive and/or non-exclusive access to resources (computer nodes) to users for some duration of time so they can perform work.
- Second, it provides a framework for starting, executing, and monitoring work (typically a parallel job such as MPI) on a set of allocated nodes.
- Finally, it arbitrates contention for resources by managing a queue of pending jobs.

## User Commands

The command option –help also provides a brief summary of options. Note that the command options are all case sensitive.

- **sacct** is used to report job or job step accounting information about active or completed jobs.
- **salloc** is used to allocate resources for a job in real time. Typically this is used to allocate resources and spawn a shell. The shell is then used to execute srun commands to launch parallel tasks.
- **sbatch** is used to submit a job script for later execution. The script will typically contain one or more srun commands to launch parallel tasks.
- **scancel** is used to cancel a pending or running job or job step. It can also be used to send an arbitrary signal to all processes associated with a running job or job step.
- **squeue** reports the state of jobs or job steps. It has a wide variety of filtering, sorting, and formatting options. By default, it reports the running jobs in priority order and then the pending jobs in priority order.
- **srun** is used to submit a job for execution or initiate job steps in real time. srun has a wide variety of options to specify resource requirements, including: minimum and maximum node count, processor count, specific nodes to use or not use, and specific node characteristics (so much memory, disk space, certain required features, etc.). A job can contain multiple job steps executing sequentially or in parallel on independent or shared resources within the job's node allocation.

## Example Command Usage

### List all current jobs for a user:
```
squeue -u <username>

```

### List all running jobs for a user:
```
squeue -u <username> -t RUNNING
 ```
### List all pending jobs for a user:
```
squeue -u <username> -t PENDING
```
### List all current jobs in the general partition for a user:
```
squeue -u <username> -p general
 ```
 
### List detailed information for a job (useful for troubleshooting):
```
scontrol show jobid -dd <jobid>
```
### List status info for a currently running job:
```
sstat --format=AveCPU,AvePages,AveRSS,AveVMSize,JobID -j <jobid> --allsteps
 ```
 
### Once your job has completed, you can get additional information that was not available during the run. This includes run time, memory used, etc. To get statistics on completed jobs by jobID:
```
sacct -j <jobid> --format=JobID,JobName,MaxRSS,Elapsed
```
 
### To view the same information for all jobs of a user:
```
sacct -u <username> --format=JobID,JobName,MaxRSS,Elapsed
```

### Add this to your .bash_profile for easy lookup of job info (lookup multiple jobids with a comma separated list)
```
alias sacctjob='sacct -o Cluster%15,JobID%20,JobName%30,MaxVMSize%15,ReqTRES%30,TimeLimit%15,Elapsed%15,State%15,ExitCode -M server1,server2,server3 -j $1'
```
