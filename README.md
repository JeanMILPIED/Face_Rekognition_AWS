# Face_Rekognition_AWS
set up an API interface for AWS face rekognition service usage

## Objective
You can follow this tutorial to set-up a simple API for face recognition (using your laptop webcam images) and the AWS 'rekognition service'. 
Nothing complicated. All files are shared in this github.

## Pre-requisites
What you must have:
- AWS credentials for your account
- some knowledge of AWS cloud services like EC2 and S3
- some basic knowledge of python environments

## the project structure
In this project we are going to set-up 2 machines + 1 database in the cloud:
1: a front end machine (in a public VPC Subnet): where the API with webcam interface runs
2: a back-end machine (also in a public VPC Subnet with a private restricted access): where the python code runs (using flask package)
3: a database : where the images are saved and exchanged

## Machines Set-up
### Machine1: Front end EC2 instance
- Ubuntu free Tier: Ubuntu-bionic-18.04-amd64-server
- in a Public Subnet of your VPC
- Security group: allow TCP 22 from your IP, HTTP 80, Custom TCP 5000
- connect to your machine

`ssh -i "<your_keypair.pem>" ubuntu@<Public_IP_adress_of_front_end`

- update and install a web server: Appache2 for example
`
apt-get update
sudo apt install apache2
sudo ufw allow 'Apache'
sudo ufw status
sudo systemctl status apache2
`
- transfer the needed front_end files to the proper directories in your front_end machine.
use the following command line:

`scp -i "<your_keypair.pem>" "./<where the file is>/index.js" ubuntu@<Public_IP_adress_of_front_end>:/home/ubuntu/index.js`

- you may need to create a static directory in your front end machine at adress: /var/www/html/




### Machine2: Back end EC2 instance
### database: S3 bucket instance
