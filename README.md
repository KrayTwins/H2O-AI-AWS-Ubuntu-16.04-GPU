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

```
## login into Ubuntu 16.04 EC2 instance
$ ssh -i <key_pair>.pem http://ec2-xxx-xxx-xxx-xxx.compute-1.amazonaws.com
$ sudo su
## Prepare for installation of Nvidia Tesla K80 GPU Ubuntu 16.04 drivers
$ apt-get update
$ apt-get upgrade -y linux-aws
$ reboot
```

```
## A common issue when installing the Nvidia drivers, is that 'gcc' and 'make' are not in path, check if they are installed
# gcc (Ubuntu 5.4.0-6ubuntu1~16.04.9) 5.4.0 20160609
# Copyright (C) 2015 Free Software Foundation, Inc.
# This is free software; see the source for copying conditions.  There is NO
# warranty; not even for MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.
# GNU Make 4.1
# Built for x86_64-pc-linux-gnu
# Copyright (C) 1988-2014 Free Software Foundation, Inc.
# License GPLv3+: GNU GPL version 3 or later <http://gnu.org/licenses/gpl.html>
# This is free software: you are free to change and redistribute it.
# There is NO WARRANTY, to the extent permitted by law.

$ gcc --version
$ make --version

## Install drivers for Nvidia Tesla K80 GPU Ubuntu 16.04
## Download Nvidia for Tesla 80 Ubuntu 16.04
$ wget http://us.download.nvidia.com/XFree86/Linux-x86_64/367.106/NVIDIA-Linux-x86_64-367.106.run
$ /bin/bash ./NVIDIA-Linux-x86_64-367.106.run
$ reboot
```


[AWS GPU Compute p2 intstances](https://aws.amazon.com/ec2/instance-types/p2/) \
[Install H2O Diverless AI on Ubuntu with GPUs](http://docs.h2o.ai/driverless-ai/latest-stable/docs/userguide/install/ubuntu.html# )





