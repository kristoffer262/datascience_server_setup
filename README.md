# Data Science server setup
This repository can be used as a help for setting up a data science server, with tips, guidelines etc. It describes the process I have used for my projects. The trickyness comes from using GPU support, introducing version handling of CUDA drivers/CUDA toolkit.

# Target for server installation
* Containerized platform using Docker
* Should be accessible from internet in projects
* Should be used for web services
* Should be used for machine learning tasks incl GPU support, with focus at PyTorch as deep learning framework
* Should publish containers with Jupyter notebooks
* The server should not have a GUI och screen connected, connection is done through SSH
* The server should be a recent Ubuntu installation

# TODO
* Set up script updating DNS record
* Set up a SSH key authentication for SSH connections
* Set up HTTPS for web services

# Hardware setup
* GPU: nVidia GeForce GTX 1660 SUPER

# Resulting setup
* Ubuntu version: 24.04 LTS
* nVidia driver version: 555.58.02
* CUDA Toolkit version: 12.4

# Installation process
## Install Ubuntu server
First step is to install Ubuntu 24.04 server. This is easily done from Ubuntu's official website. Download the current .iso-file. To "burn" .iso-file to a USB-stick, install Rufus at rufus.ie. Follow instructions to "burn" iso-file to a USB-stick (all data from the USB-stick will be lost at this step).
After this step, boot server on USB-step and install Ubuntu, do not add Docker as an application in the server setup, this will be installed later.

## Basic Ubuntu update
Update Ubuntu packages, add nvidia repository, and install optional pacakges with:
```
sudo add-apt-repository ppa:graphics-drivers/ppa
sudo apt-get update
sudo apt-get upgrade
sudo apt-get install net-tools
```
## Install nVidia drivers
Here comes the trick with getting the right versions of packages. To use pytorch at a current version, CUDA Toolkit 12.4 will be used in the docker containers (https://pytorch.org/get-started/locally/). For this to be compatible, I use nVidia driver 555. The following tools may give guidandce in finding optimal set of versions:
```
sudo ubuntu-drivers list --gpgpu
sudo ubuntu-drivers devices
```

I install nVidia driver 555 with:
```
sudo apt install nvidia-driver-555
sudo reboot
```

## Install Docker
Follow the official Docker installation guidelines from https://docs.docker.com/engine/install/ubuntu/. Optionally give the Ubuntu user privileges to run docker commands in cli with:
```
sudo usermod -aG docker <USERNAME>
``` 

After this, test the installation with the given hello-world container:
```
sudo docker run hello-world
```

Now, install the Docker nVidia container toolkit, that is neccesary in order to run containers with GPU support:
```
sudo apt-get install -y nvidia-container-toolkit
```

## Testing the setup
To test the setup, run the following command, and verify that a container with pytorch can be run:
```
docker run --gpus all -it --rm pytorch/pytorch:2.4.0-cuda12.4-cudnn9-runtime
```

In the container, start python and run the following python-command:
```
import torch
torch.cuda.is_available()
```
This should return True when the GPU is available in the Docker container. You could also run the ubuntu command "nvidia-smi", and verify the configuration in this output.

# Setting up a image and container for data science tasks
One Docker-solution that works for me is the following. Create a folder named "docker" in the user folder (~), and another subfolder, e.g., "datascience1". In this folder, add the Dockerfile file and requirements.txt file from this repository, "samples" folder.

## Create an image
Inside the docker/datascience1-folder, run this command:
```
docker build -t datascience1 . -f Dockerfile
```
This create a Docker image with the configuration from Dockerfile.

## Create a container
To run a container, and expose jupyter port as 18888, and adding permanent storage for the container in /project, run the following command:
```
docker run -dt --mount source=data_science_1,target=/project --name datascience1 -p 18888:8888 --gpus all datascience1:latest
```

Voilá, done.