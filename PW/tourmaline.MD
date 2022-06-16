# Tutorial: Running Tourmaline on an AWS instance

* Start up an AWS SLURM instance for running Tourmaline

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
