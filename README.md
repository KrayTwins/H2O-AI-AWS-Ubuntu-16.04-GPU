# H2O Driverless AI GPU on AWS p2.exlarge with Ubuntu-16.04

# Choose an Amazon Machine Image (AMI)
https://console.aws.amazon.com/ec2/v2/home?region=us-east-1#LaunchInstanceWizard: \
Ubuntu Server 16.04 LTS (HVM), SSD Volume Type - ami-43a15f3e \
Choose an Instance Type \
To take advantage of the enormous computing capability of H2O AI choose a GPU Compute Family Instance. \
The p2.exlarge, is a powerful instance with 4 vCPUs, 61 Gb or memory and by default 1 Nvidia Tesla K80 GPU.



https://aws.amazon.com/ec2/instance-types/p2/ \
 \
http://docs.h2o.ai/driverless-ai/latest-stable/docs/userguide/install/ubuntu.html# \

For this experiment lets choose use an AWS p2.xlarge AMI (It is the lowest spec GPU Compute AMI available and has a \
on demand price $0.90 /hr, however you can usually get p2.xlarge at a Spot Price of less than half in us-west-1 and us-east-1)

# Configure the AWS p2.xlarge instance to use the Nvidia Tesla K80 GPU
https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/install-nvidia-driver.html
sudo apt-get update \
sudo apt-get upgrade -y linux-aws \
sudo reboot \
sudo apt-get install -y gcc make linux-headers-$(uname -r) \

A common error that occurs when installing the NVIDIA Telsa K80 is that the gcc & make are not in the path

gcc --version \
make --version \

Download the AWS Nvidia driver for Ubuntu 16.04
wget http://us.download.nvidia.com/XFree86/Linux-x86_64/367.106/NVIDIA-Linux-x86_64-367.106.run
sudo /bin/bash ./NVIDIA-Linux-x86_64-367.106.run
sudo reboot
sudo nvidia-smi -q | head

==============NVSMI LOG==============

Timestamp                           : Mon Apr  9 12:25:59 2018
Driver Version                      : 367.106

Attached GPUs                       : 1
GPU 0000:00:1E.0
    Product Name                    : Tesla K80
    Product Brand                   : Tesla

mkdir dai_rel_1.0.30
cd dai_rel_1.0.30/
wget https://s3-us-west-2.amazonaws.com/h2o-internal-release/docker/driverless-ai-docker-runtime-latest-release.gz
mkdir data log license tmp
sudo apt install apt-transport-https ca-certificates curl software-properties-common
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu artful stable"
sudo apt update
sudo apt install docker-ce
sudo docker info or sudo docker run hello-world

wget -P /tmp https://github.com/NVIDIA/nvidia-docker/releases/download/v1.0.1/nvidia-docker_1.0.1-1_amd64.deb
sudo dpkg -i /tmp/nvidia-docker*.deb
sudo rm /tmp/nvidia-docker*.deb

sudo docker load < driverless-ai-docker-runtime-latest-release.gz

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
