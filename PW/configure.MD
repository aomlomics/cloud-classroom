## Configure an AWS Cluster

Click on the "Gear" icon on the "Compute" tab to open the configuration window. 

* Project - From the drop down list box select the project to be charged for this work.
* Access Public Key - to SSH to the Master node in your cluster from another host such as your laptop, you can put in your public key from that machine.
* Max Workers - The maximum number of nodes
* <b>Availability zone</b> must be set as `us-east-1c`
* <b>efa</b> must be disabled 
* The <b>Lustre</b> filesystem is for the reading and writing of huge files (terrabytes) and is expensive. Do not use unless absolutely accessary 
  
Here is a configuration that works

```
{
"architecture":"amd64",
"availability_zone":"us-east-1c",
"controller_image":"latest",
"controller_net_type":false,
"export_fs_type":"xfs",
"image_disk_count":"1",
"image_disk_name":"snap-04f8963f5d94148b6",
"image_disk_size_gb":"5000",
"management_shape":"c5n.2xlarge",
"partition_config":[
        {
        "architecture":"amd64",
        "availability_zone":"us-east-1c",
        "default":"YES",
        "elastic_image":"latest",
        "enable_spot":false,
        "instance_type":"g3.16xlarge",
        "max_node_num":"1",
        "name":"Compute",
        "net_type":false
        }
],
"region":"us-east-1"
}
```

### Troubleshooting

After starting the cluster, you can look at the log file to see if any problems are found:

`sudo tail -f /var/log/slurm/elastic.log`

This monitors system messages as the compute nodes are started up and can give clues on whether any computing node is stuck or if the node(s) are still working.
