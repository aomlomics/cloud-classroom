# Working with Amazon Linux-aarch64 (ARM64) architecture 

##  Install development tools on Amazon Linux AMI

For your system to be able to compile software, you need to install several development tools, such as `make`, `gcc`, and `autoconf`. Because software compilation is not a task that every Amazon EC2 instance requires, these tools are not installed by default, but they are available in a package group called **Development Tools** that is easily added to an instance with the `yum groupinstall` command.

`sudo yum groupinstall "Development Tools"`

## Install Anaconda

Instructions from https://docs.anaconda.com/anaconda/install/linux-aarch64/ 

- Download the 64-Bit (AWS Graviton2 / ARM64) Installer

`wget https://repo.anaconda.com/archive/Anaconda3-2021.05-Linux-aarch64.sh`

- Install Anaconda

`bash ~/Downloads/Anaconda3-2021.04-Linux-aarch64.sh`

- To control whether or not each shell session has the base environment activated or not, run  `conda config --set auto_activate_base False` or `True`

- Close and open your terminal window for the installation to take effect, or you can enter the command `source ~/.bashrc`

## Bowtie2
