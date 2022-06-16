# Tutorial: Using Docker on an AWS instance

In this tutorial, we will try to run the Docker version of Tourmaline. [Tourmaline](https://github.com/aomlomics/tourmaline) is an amplicon sequence processing workflow for Illumina sequence data that uses QIIME 2 and the software packages it wraps. Tourmaline manages commands, inputs, and outputs using the Snakemake workflow management system.

* Start up an AWS SLURM instance just sufficient enough to run Docker and Tourmaline's test dataset

```
{
"controller_net_type":false,
"region":"us-east-1",
"availability_zone":"us-east-1c",
"architecture":"amd64",
"management_shape":"m6id.2xlarge",
"controller_image":"latest",
"export_fs_type":"xfs",
"image_disk_name":"snap-04f8963f5d94148b6",
"image_disk_count":"1",
"image_disk_size_gb":"5000",
"partition_config":[
	{
	"default":"YES",
	"enable_spot":false,
	"name":"compute",
	"architecture":"amd64",
	"instance_type":"d3en.8xlarge",
	"max_node_num":"1",
	"net_type":false,
	"elastic_image":"latest",
	"availability_zone":"us-east-1c"
	}
]
}
```
