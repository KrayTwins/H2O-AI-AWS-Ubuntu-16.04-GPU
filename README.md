# H2O Driverless AI on AWS
H2O Driverless AI is a high-performance, GPU-enabled, client-server application for the rapid \
development and deployment of state-of-the-art predictive analytics models. It reads tabular \
data from plain text sources and automates data visualization, feature engineering, model training, \
and model explanation.

H2O has a Community AMI for the h2oai-driverless-ai-1.0.24 - ami-222aea5f \
This is the easiest method of getting \
H2O Driverless AI up and running quickly with a license to run on AWS. \
http://docs.h2o.ai/driverless-ai/latest-stable/docs/userguide/install/aws.html

# Installing H2O Driverless AI on AWS GPU Compute EC2 p2.exlarge with Ubuntu-16.04

If you want to try the H2O Diverless AI you need to contact H2O sales sales@h2o.ai or request a trial license \
https://www.h2o.ai/try-driverless-ai/

For this example lets choose an use an AWS p2.xlarge instance (It is the lowest spec GPU Compute AMI available and has a \
on demand price $0.90 /hr, however you can usually get p2.xlarge at a Spot Price of less than half that in us-west-1 and us-east-1)

Choose an Amazon Machine Image (AMI) \
https://console.aws.amazon.com/ec2/v2/home?region=us-east-1#LaunchInstanceWizard: \
Ubuntu Server 16.04 LTS (HVM), SSD Volume Type - ami-43a15f3e \
Choose an Instance Type \
To take advantage of the enormous computing capability of H2O AI choose a GPU Compute Family Instance. \
The p2.exlarge, is a powerful instance with 4 vCPUs, 61 Gb or memory and by default 1 Nvidia Tesla K80 GPU. \
Choose at least 40Gb for EBS Storage

[AWS GPU Compute p2 intstances](https://aws.amazon.com/ec2/instance-types/p2/) \
[Install H2O Diverless AI on Ubuntu with GPUs](http://docs.h2o.ai/driverless-ai/latest-stable/docs/userguide/install/ubuntu.html# )

```
## login into Ubuntu 16.04 EC2 instance
$ ssh -i <key_pair>.pem http://ec2-xxx-xxx-xxx-xxx.compute-1.amazonaws.com
$ sudo su
## Prepare for installation of Nvidia Tesla K80 GPU Ubuntu 16.04 drivers
$ apt update
$ apt upgrade -y linux-aws
$ reboot
```

```
## login back into Ubuntu 16.04 EC2 instance
$ ssh -i <key_pair>.pem http://ec2-xxx-xxx-xxx-xxx.compute-1.amazonaws.com
$ sudo su
## install gcc and make
$ apt install -y gcc make linux-headers-$(uname -r)

## Install drivers for Nvidia Tesla K80 GPU Ubuntu 16.04
## Download Nvidia for Tesla 80 Ubuntu 16.04
$ wget http://us.download.nvidia.com/XFree86/Linux-x86_64/367.106/NVIDIA-Linux-x86_64-367.106.run
$ /bin/bash ./NVIDIA-Linux-x86_64-367.106.run
$ reboot
```

```
## login back into Ubuntu 16.04 EC2 instance
$ ssh -i <key_pair>.pem http://ec2-xxx-xxx-xxx-xxx.compute-1.amazonaws.com
$ sudo su
## Check Nvidia divers are installed for Nvidia Telsla K80 GPU
$ nvidia-smi -q | head
```

If installed correctly will show \
==============NVSMI LOG==============

Timestamp                           : Mon Apr  9 12:25:59 2018 \
Driver Version                      : 367.106

Attached GPUs                       : 1 \
GPU 0000:00:1E.0 \
    Product Name                    : Tesla K80 \
    Product Brand                   : Tesla 

```

```
## Install docker-ce to run the H2O Driverless AI container (note use the 'docker run' commend for CPU and 'nvidea-docker run' GPU Compute)  
$ mkdir dai_rel_1.0.30
$ cd dai_rel_1.0.30/
$ wget https://s3-us-west-2.amazonaws.com/h2o-internal-release/docker/driverless-ai-docker-runtime-latest-release.gz
$ mkdir data log license tmp
$ apt install apt-transport-https ca-certificates curl software-properties-common
$ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
$ add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu artful stable"
$ apt update
$ apt install docker-ce
```

```
## Install Nvidia docker package: nvidia-docker  
$ wget -P /tmp https://github.com/NVIDIA/nvidia-docker/releases/download/v1.0.1/nvidia-docker_1.0.1-1_amd64.deb
$ dpkg -i /tmp/nvidia-docker*.deb
$ rm /tmp/nvidia-docker*.deb
```
```
$ docker load < driverless-ai-docker-runtime-latest-release.gz

```

```
## nvidia-docker conatiner opening ports 12345 and 54321
# http://ec2-xxx-xxx-xxx-xxx.compute-1.amazonaws.com:12345
nvidia-docker run \
    --rm \
    -u `id -u`:`id -g` \
    -p 12345:12345 \
    -p 54321:54321 \
    -p 8888:8888 \
    -v `pwd`/data:/data \
    -v `pwd`/log:/log \
    -v `pwd`/license:/license \
    -v `pwd`/tmp:/tmp \
    opsh2oai/h2oai-runtime
```







