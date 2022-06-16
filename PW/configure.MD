## Configure an AWS Cluster

Before starting a cloud instance, you will have to configure the computing resources needed for that cloud instance. To do that, click on the <b>Resources</b> tab on [Parallel Works](https://noaa.parallel.works/u/resources) then click <b>Add Resource</b> to add a new resource. If you have an existing resource, click <b>Edit</b> to modify the configuration for a specified resource.

<img src="media/Resources.png" style="width:3.77778in;height:2.13889in" alt="Parallel Work's Resources page" />

Click on the "Gear" icon on the "Compute" tab to open the configuration window. 

| Field  | Description | 
|------------|--------|
| Resource Type | Select the type of resource (AWS, Google, Azure etc) that you would like to use, and whether you require an elastic instance or a cluster with job submission and scheduling (e.g. SLURM) |
| Resource Account | Select the NOAA account to use for this work |
| Multi Factor | No |
| Jump Node | No |
| Resource Project | Select the project to be charged for this work |
| Multi User | Select Yes if you require multiple NOAA users to access the cluster. Otherwise, No |
| Access Public Key | To access the cloud resource via SSH, you have to copy and paste your public key from the machine used for SSH access. See [here](https://github.com/shenjean/cloud-classroom/blob/main/PW/keygen.MD) for instructions on how to generate a public key |
| Max Workers | The maximum number of nodes in the cloud instance |
| Availability zone | Must be `us-east-1c` |
| EFA | Must be disabled |
| Lustre | The Lustre filesystem is for the reading and writing of huge files (terrabytes) and is expensive. Do not use unless the regular EBS file systems do not meet your needs |
  
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
