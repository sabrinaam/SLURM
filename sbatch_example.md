## IMPORTANT

- All SBATCH options must be together at the top of the submission script.
- Once SBATCH reads a line that is not an SBATCH option it will ignore any subsequent options.
- This ordering of options is recommended to keep a consistency across all user scripts for easier troubleshooting.


See learning section below for simple explanation of commands.
- –mem-per-cpu must come after -c because it cannot allocate memory if it does not know the number of cpus.


### Example
```
#!/bin/bash
#SBATCH -M cluster
#SBATCH -A account
#SBATCH -p partition
#SBATCH --mail-type=end
#SBATCH --mail-user=emailaddress
#SBATCH -J JOBNAME
#SBATCH -e SLURM-JBID-%j.err
#SBATCH -o SLURM-JBID-%j.out
#SBATCH -n 0
#SBATCH -c 0
#SBATCH --mem-per-cpu=0
#SBATCH --time=00:00:00
export OMP_NUM_THREADS=$SLURM_CPUS_PER_TASK
module add shared
```

## Explanation

```
## required header for all bash scripts
#!/bin/bash
 
## required line start for all SBATCH options
#SBATCH
   -M, --clusters=<string>            ## (opt) define the cluster(s) for job submission
   -A, --account=<account>            ## (req) define account in which to run
   -p, --partition=<partition_names>  ## (req) define partition(s) in which to run (comma separated)
   --mail-type=<type>                 ## (opt) send emails at NONE, BEGIN, END, FAIL, REQUEUE, ALL
   --mail-user=<email address>        ## (opt) addresses to receive emails (comma separated)
   -J, --job-name=<jobname>           ## (req) job name           
   -e SLURM-JBID-%j.err               ## (opt) standard error log
   -o SLURM-JBID-%j.out               ## (opt) standard output log
   -n, --ntasks=<number>              ## (req) number of task required. Most likely 1.
   -c, --cpus-per-task=<ncpus>        ## (req) the number of CPUs required. Be careful to only request
                                        ## the optimal number based on your application and data size
                                        ## Use TITAN to determine the optimal.
   --mem=<size[units]>                ## (req) Use either M or G suffix
                                        ## --mem-per-cpu is being deprecated
   ---time=00:05:00time>              ## (req) the max run-time of your job
                                        ## Acceptable  time  formats  include:
                                        ## minutes
                                        ## minutes:seconds
                                        ## hours:minutes:seconds
                                        ## days-hours
                                        ## days-hours:minutes
                                        ## days-hours:minutes:seconds
 
export OMP_NUM_THREADS=$SLURM_CPUS_PER_TASK    ## required for OMP jobs only
module add shared                              ## Loading required modules
```
## Interactive Sessions

```
salloc                           # slurm job allocation
--cluster=clustername            # (optional) list of clusternames allowed to answer
--account=account                # account to run under
--qos=normal                     # qos to run with
--cpus-per-task=1                # number of CPUs reserved for this allocation 
--nodes=1                        # limits this allocation to a single node
--time=12:00:00                  # max time for this allocation
--mem=1024                       # amount of memory for allocation
-p partition                     # partition in which to reserve allocation
--gres=gpu:k80:2                 # (GPU ONLY) requesting 2 GPUs
--gres-flags=enforce-binding     # (GPU ONLY) forcing the binding between GPU and CPU sockets
srun --pty bash                  # command to execute in this allocation, in this case a bash prompt

# Interactive session for non-GPU
$ salloc --account=account --qos=normal --cpus-per-task=1 --nodes=1 --time=12:00:00 --mem=1024 -p interactive srun --pty bash
# Interctive session for GPU
$ salloc --account=account --qos=normal --cpus-per-task=1 --nodes=1 --time=12:00:00 --gres=gpu:k80:2 --gres-flags=enforce-binding --mem=1024 -p interactive srun --pty bash
```

### Note

Once inside an interactive session, you can make use of the screen or tmux commands, allowing access to multiple shells.
- Check how to use screen command [here](https://www.tecmint.com/screen-command-examples-to-manage-linux-terminals/).
