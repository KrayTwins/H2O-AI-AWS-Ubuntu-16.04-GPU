# H2O Driverless AI on AWS
H2O Driverless AI is a high-performance, GPU-enabled, client-server application for the rapid \
development and deployment of state-of-the-art predictive analytics models. It reads tabular \
data from plain text sources and automates data visualization, feature engineering, model training, \
and model explanation.

H2O has a Community AMI for the h2oai-driverless-ai-1.0.24 - ami-222aea5f
This is the easiest method of getting \
H2O Driverless AI up and running quickly with a license to run on AWS. \
http://docs.h2o.ai/driverless-ai/latest-stable/docs/userguide/install/aws.html

# H2O Driverless AI GPU on AWS p2.exlarge with Ubuntu-16.04

If you 

Choose an Amazon Machine Image (AMI) \
https://console.aws.amazon.com/ec2/v2/home?region=us-east-1#LaunchInstanceWizard: \
Ubuntu Server 16.04 LTS (HVM), SSD Volume Type - ami-43a15f3e \
Choose an Instance Type \
To take advantage of the enormous computing capability of H2O AI choose a GPU Compute Family Instance. \
The p2.exlarge, is a powerful instance with 4 vCPUs, 61 Gb or memory and by default 1 Nvidia Tesla K80 GPU.



https://aws.amazon.com/ec2/instance-types/p2/ 

http://docs.h2o.ai/driverless-ai/latest-stable/docs/userguide/install/ubuntu.html# 

For this experiment lets choose use an AWS p2.xlarge AMI (It is the lowest spec GPU Compute AMI available and has a \
on demand price $0.90 /hr, however you can usually get p2.xlarge at a Spot Price of less than half that in us-west-1 and us-east-1)


