## Start AWS cluster

Once you have [configured your cluster](https://github.com/shenjean/cloud-classroom/blob/main/PW/configure.MD), you can turn it on and off by clicking on the <b>round power button</b> corresponding to your instance name. It may take up to 15 minutes for an AWS cluster to be ready. Once the cluster is up, the "Node Status" bar will turn green up to the first tick indicating that the login node is ready for use. 

<img src="media/Gear.png" style="width:3.77778in;height:2.13889in" alt="Resource configuration on Parallel Work's Compute page" />


### Connecting to the master node via SSH
To get the IP address of the master node, click on the "i" button next to the Resource Configuration button. This opens the "Status" window that displays the IP address to be used to connect to the Master node. Click on the hyperlink next to the "Master node" to copy the IP address.

<img src="media/Info.png" style="width:3.77778in;height:2.13889in" alt="Status window" />

If you created the cloud instance with your [public key](https://github.com/shenjean/cloud-classroom/blob/main/PW/keygen.MD), you will be able to connect and transfer files to/from the master node without a password. For example:

`ssh username@1.232.141.222`

`rsync file_to_transfer username@1.232.141.222:/contrib/username/`
`scp username@1.232.141.222:/contrib/username/file_to_transfer localfilename`

### Connnecting to the master node via Parallel Works

### Transient vs permanent storage

Since this is an on-demand cluster, all files in the `/home` directory are transient and will be deleted after the cluster is shut down. Files in the `/contrib` volume, however, persists after a cluster is shutdown and can be used from any cloud instance. It is recommended to install common software such as conda and conda environments in the `/contrib/username/home` folder. A conda environment in the `/contrib` folder can be started up from a job script, for example:

```sh
#!/bin/bash
#SBATCH --job-name=CAMISIM      # Job name 
#SBATCH --output=CAMISIM.log     # Standard output and error log    

conda activate /contrib/username/home/miniconda3/envs/CAMISIM
cd /contrib/username/CAMISIM
python metagenomesimulation.py defaults/config.ini
```