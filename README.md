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
- connect to your machine:  
`ssh -i "<your_keypair.pem>" ubuntu@<PublicIPadress_of_frontend`  

- update and install a web server: Apache2 for example    
`apt-get update`  
`sudo apt install apache2`  
`sudo ufw allow 'Apache'`  
`sudo ufw status`  
`sudo systemctl status apache2`  

- transfer the needed front_end files to the proper directories in your front_end machine.  
use the following command line (example for index.js file):  

`scp -i "<your_keypair.pem>" "./<wherethefileis>/index.js" ubuntu@<PublicIPadress_of_frontend>:/home/ubuntu/index.js`

- you may need to create a static directory in your front end machine at adress: /var/www/html/  

- then relaunch the Apache2 server to check everything is properly installed:  
`sudo systemctl status apache2`

- to check, please connect through your webbrowser to the proper http://<public_IP_adress_of_frontend machine>  


- on chrome, you may need to allow the site to get access to your webcam by going to the following option link:  
(chrome://flags/#unsafely-treat-insecure-origin-as-secure)  
![alt text](https://github.com/JeanMILPIED/Face_Rekognition_AWS/blob/master/webcam-google.JPG) 


### Machine2: Back end EC2 instance  
- Ubuntu free Tier: Ubuntu-bionic-18.04-amd64-server + a 20Go memory request  
- in a Public Subnet of your VPC  
- Security group: allow TCP 22 from your IP, Custom TCP 5000  
- connect to your machine:  
`ssh -i "<your_keypair.pem>" ubuntu@<PublicIPadress_of_backend>`  

- update and install Anaconda and create a virtual flask environment  
`sudo apt-get update`  
`wget https://repo.anaconda.com/archive/Anaconda3-2019.03-Linux-x86_64.sh`  
`exit`   
`conda create -n my_flask_env python=3.6`   
`conda activate my_flask_env`   
`pip install Flask`   
`pip install flask_cors`   
`pip install boto3`   
`pip install flask_cor`   

      note: you may need to disconnect and reconnect to your EC2 in order to get it properly work  

- then you should strore your AWS credentials on the machine at the proper location:  
the nano command allows you to modify the files  
` mkdir ~/.aws  
  nano ~/.aws/credentials`  

  - your credentials should look like this:  
        ```
        [default]   
        aws_access_key_id= ...   
        aws_secret_access_key= ...   
        aws_session_token= ...   
        ```

- then you should modify the configuration of your aws for the proper region  
`nano ~/.aws/config`  

  - your region should look like this  
```
[default]    
region = us-east-1   
```           

- you can then transfer the .py file named my_rek_app.py to the home directory in the backend machine  
```
scp -i "<your_key.pem>" "<where the file is>/my_rek_app.py" ubuntu@<backend IP adress>:/home/ubuntu/my_rek_app.py  
```

- then lauch the .py program  
```
python3 my_rek_api.py  
```

### database: S3 bucket instance
to store the image files created by the webcam, we will use the S3 database format  
- go to S3 service in AWS  
- create a bucket named 'image-for-reko'  
- keep its security open for this time  

### Then make the whole project work!
#### some changes first
- in the index.js file, change the IP adress to the Public IP of your backend machine  
- don't forget to update your credentials in the ~/.aws/ folder  

#### and let the magic happen
- connect to your frontend public IP adress  
- have fun!  

![alt text](https://github.com/JeanMILPIED/Face_Rekognition_AWS/blob/master/2020-01-23%20(2).png)


