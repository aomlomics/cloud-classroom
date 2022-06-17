## Submitting a job on your cloud instance

The Parallel Works system creates an on-demand clusters, so the master node will be up and running until you shut it down. The remaining compute nodes will be turned on when jobs are submitted, will keep running as long as there are jobs, and will automatically shut down when they have been idle for five minutes. In order to use the compute nodes, a job must be submitted using SLURM.

### Example SLURM job script

Save the commands to be run in your job into a file. This is the simplest example of a SLURM job script, `testjob.sh`

```sh
#!/bin/bash
#SBATCH --job-name=TEST      # Job name 
#SBATCH --output=TEST.log     # Standard output and error log     

echo "Hello world"
```

In your SLURM job script, you can specify what resources you need to run your job, such as number of nodes, amount of memory, and amount of time. There are lots of tutorials on SLURM online, and [here](https://wiki.rc.usf.edu/index.php/Guide_to_SLURM) is one such tutorial.

### Conda environments

* Once miniconda is installed in the `/contrib/user/home` folder, the next time a user starts up another instance, `conda init` can be run to reactivate conda. For example, `/contrib/user/home/miniconda3/bin/conda init`. Remember to restart the login session after running `conda init`. After restarting the session, a user can then run conda commands like `conda activate env_name`.

* It looks like the conda environment does not activate within the job script. To run a job within a conda environment, just activate the environment on the controller node before submitting the job, e.g. `conda activate biobakery3` then `sbatch job.sh`.

### Submit a job
`sbatch testjob.sh`

### Check status of a job
* To check status of a job: `squeue -u username`
The status of a job is indicated in the `ST` column of the `squeue output`: R= Running, CF= Compute node is being booted up.

* If a compute node is not starting up (stuck at CF), you can look at the log file to see if any problems are found:
`sudo tail -f /var/log/slurm/elastic.log`
This monitors system messages as the compute nodes are started up and can give clues on whether any computing node is stuck or if the node(s) are still working. Sometimes, it could be that the computing node does not have enough vCPUs to perform the job specified.

### Cancel a job
Use `squeue -u username` to first retrive the `JOBID`, then use `scancel` to cancel a job with the JOBID. For example, if the JOBID of a job is 27 then you would do: `scancel 27` to cancel that job.

### Interactive job
You can also start up an interactive job session with SLURM. This allows you to run scripts directly on the compute node. Here is the simplest example:

`srun --job-name "Interactive" --pty bash`

With `srun`, you can also specify what resources you need to run your job, such as number of nodes, amount of memory, and amount of time. 
