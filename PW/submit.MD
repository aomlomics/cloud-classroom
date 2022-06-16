## Submitting a job on your cloud instance

The Parallel Works system creates an on-demand clusters, so the master node will be up and running until you shut it down. The remaining compute nodes will be turned on when jobs are submitted , will keep running as long as there are jobs and will automatically shut down when they have been idle for five minutes.

In order to use the compute nodes, a batch job using the Slurm scheduler must be submitted.



### Troubleshooting

After starting the cluster, you can look at the log file to see if any problems are found:

`sudo tail -f /var/log/slurm/elastic.log`

This monitors system messages as the compute nodes are started up and can give clues on whether any computing node is stuck or if the node(s) are still working.
