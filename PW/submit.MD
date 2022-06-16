## Submitting a job on your cloud instance

The Parallel Works system creates an on-demand clusters, so the master node will be up and running until you shut it down. The remaining compute nodes will be turned on when jobs are submitted, will keep running as long as there are jobs, and will automatically shut down when they have been idle for five minutes. In order to use the compute nodes, a batch job using the Slurm scheduler must be submitted.

### Example SLURM job script

Save the commands to be run in your job into a file, e.g. `testjob.sh`

```sh
#!/bin/bash
#SBATCH --job-name=TEST      # Job name 
#SBATCH --output=TEST.log     # Standard output and error log     

echo "Hello world"
```

### Submit a job
`sbatch testjob.sh`


### Check status of a job
* To check status of a job: `squeue -u username`
The status of a job is indicated in the `ST` column of the `squeue output`: R= Running, CF= Compute node is being booted up.

> If a compute node is not starting up, you can look at the log file to see if any problems are found:
> `sudo tail -f /var/log/slurm/elastic.log`
> This monitors system messages as the compute nodes are started up and can give clues on whether any computing node is stuck or if the node(s) are still working.

### Cancel a job
Use `squeue -u username` to first retrive the `JOBID`, then use `scancel` to cancel a job with the JOBID. For example, if the JOBID of a job is 27 then you would do: `scancel 27` to cancel that job.
