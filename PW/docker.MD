# Running Docker on an AWS cluster instance

* Docker is installed by default on each AWS instance. You will need to use `sudo` for most Docker-related commands.

* In this tutorial, we will try to run the Docker version of Tourmaline. [Tourmaline](https://github.com/aomlomics/tourmaline) is an amplicon sequence processing workflow for Illumina sequence data that uses QIIME 2 and the software packages it wraps. Tourmaline manages commands, inputs, and outputs using the Snakemake workflow management system.

## Create an AWS SLURM instance

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

## Start an interactive session on the computing node
`srun --job-name "Tourmaline" --pty bash`

## Start Docker engine on computing node
Docker is installed by default on each AWS instance. You will need to use `sudo` for most of Docker-related commands. First, let's start the Docker engine: `sudo systemctl start docker`

## Download Docker image

`sudo docker pull aomlomics/tourmaline`

## Run Docker container
`sudo docker run -v $HOME:/data -it aomlomics/tourmaline`

Follow the instructions on the [Tourmaline README](https://github.com/aomlomics/tourmaline) to run the setup and analysis for the test data.
