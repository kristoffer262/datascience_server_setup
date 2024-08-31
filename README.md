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
