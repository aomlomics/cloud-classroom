# Working with Amazon Linux-aarch64 (ARM64) architecture 

[Amazon instances](https://aws.amazon.com/ec2/instance-types/) like C6g, M6g, and R6g are powered by ARM-based AWS Graviton2 processors. They deliver better price performance over current generation instances. Additionally, local NVMe-based SSDs are physically connected to the host server and provide block-level storage that is coupled to the lifetime of the instance. 

Since ARM-based processors are relative new, many software developed and run on Intel's x86 and x84 processors are not yet compatible with the aarch64 (ARM64) architecture. Here are some Linux-aarch64 compatible software that have been tested on AWS:

## General software
###  Install development tools on Amazon Linux AMI

For your system to be able to compile software, you need to install several development tools, such as `make`, `gcc`, and `autoconf`. Because software compilation is not a task that every Amazon EC2 instance requires, these tools are not installed by default, but they are available in a package group called **Development Tools** that is easily added to an instance with the `yum groupinstall` command.

`sudo yum groupinstall "Development Tools"`

### Anaconda

Instructions from https://docs.anaconda.com/anaconda/install/linux-aarch64/ 

- Download the 64-Bit (AWS Graviton2 / ARM64) Installer

`wget https://repo.anaconda.com/archive/Anaconda3-2021.05-Linux-aarch64.sh`

- Install Anaconda

`bash ~/Downloads/Anaconda3-2021.04-Linux-aarch64.sh`

- To control whether or not each shell session has the base environment activated or not, run  `conda config --set auto_activate_base False` or `True`

- Close and open your terminal window for the installation to take effect, or you can enter the command `source ~/.bashrc`

### Jupyter notebook

Instructions modified from https://docs.aws.amazon.com/dlami/latest/devguide/setup-jupyter-config.html

`conda install jupyter`

Start Jupyter Notebook server (more secure key self-signed SSL certificate in the [guide](https://docs.aws.amazon.com/dlami/latest/devguide/setup-jupyter-config.html) above)

`jupyter notebook`

Jupyter provides a password utility. Run the following command and enter your preferred password at the prompt.

`jupyter notebook password`

Run the following command to forward all requests on local port 8888 to port 8888 on your remote Amazon EC2 instance. Update the command by replacing the location of your key to access the Amazon EC2 instance and the public DNS name of your Amazon EC2 instance. This command is for Linux/MacOS. For Windows, refer to the [guide](https://docs.aws.amazon.com/dlami/latest/devguide/setup-jupyter-config.html).

`ssh -i ~/mykeypair.pem -N -f -L 8888:localhost:8888 ubuntu@ec2-###-##-##-###.compute-1.amazonaws.com`

To login to Jupyter notebook server from your local browser, use this link: https://localhost:8888

#### Add conda environment to Jupyter notebook kernel

The conda environment below supports basic machine learning packages

`conda create -n ML python=3.7`

Install packages
```
conda install ipykernel
conda install tensorflow
conda install scikit-learn
conda install xgboost
conda install netcdf4
conda install xarray
conda install matplotlib
conda install seaborn
```

Install kernel based on current conda environment
`python -m ipykernel install --user --name ML`

On your local browser, connect to your Jupyter notebook server and click refresh. You should be able to see a "ML" under the "New" button on the right of the window.

### Boost (C++ library)

- Download and install from https://www.boost.org/doc/libs/1_77_0/more/getting_started/unix-variants.html
```
tar --bzip2 -xf /path/to/boost_1_77_0.tar.bz2
cd /path/to/boost_1_77_0
sudo ./bootstrap.sh
sudo ./b2 install
```
### cmake

Download binary distribution for Linux aarch64 from https://cmake.org/download/

### Docker

- Install Docker: `sudo yum install docker`
- Connect to the Docker daemon
```
sudo systemctl unmask docker
sudo systemctl start docker
```

## Bioinformatics software

### Bowtie2

Download the latest release (e.g. bowtie2-2.4.4-linux-aarch64.zip) from: http://bowtie-bio.sourceforge.net/bowtie2/index.shtml. The pre-compiled binaries are ready to use. 

### Samtools

- Install pre-requisites

```
sudo yum install autoconf automake make gcc perl-Data-Dumper zlib-devel bzip2 bzip2-devel xz-devel curl-devel openssl-devel ncurses-devel
```

- Download and install latest release (e.g. samtools-1.13.tar.bz2) from https://github.com/samtools/samtools

```
tar -xf samtools-1.13.tar.bz2
./configure
make
sudo make install
```


