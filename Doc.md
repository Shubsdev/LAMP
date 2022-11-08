## LAMP STACK IMPLEMENTATION

This is a Walkthrough on steps to create a LAMP(Linux, Apache, MySQL, PHP or Python, or Perl)

 First Step would be to create the EC2 instance on the AWS console.
 
![image](https://user-images.githubusercontent.com/102925329/200542741-6ec6aeb7-eefa-4430-9b70-d1959c69ec98.png)

Confirm Instance is Running

<img width="954" alt="image" src="https://user-images.githubusercontent.com/102925329/200546009-e39745fa-b772-42b8-b5c6-c0442af1045a.png">

Now we connect to instance using the Public IP

<img width="445" alt="image" src="https://user-images.githubusercontent.com/102925329/200559830-e1de09c5-3574-4525-8c0c-486476a98a6a.png">


We would be connecting to the Instance Using The SSH Client Putty 

<img width="224" alt="image" src="https://user-images.githubusercontent.com/102925329/200559693-73c7bc2c-86a0-4d73-a580-0f11c9c58d09.png">

**Successfully Connected**

<img width="351" alt="image" src="https://user-images.githubusercontent.com/102925329/200560362-09362eda-6916-4f3d-a851-f1b1dfdda586.png">


**INSTALLING APACHE AND UPDATING THE FIREWALL**

First of all we run the : 'sudo apt update' to update the packages 

<img width="461" alt="image" src="https://user-images.githubusercontent.com/102925329/200561441-36f28143-1f69-49e1-aed3-a77a32d07102.png">

Then we run : 'sudo apt install apache2' to install the apache package

To verify that apache2 is running as a Service in our OS, use following command : sudo systemctl status apache2

<img width="433" alt="image" src="https://user-images.githubusercontent.com/102925329/200562559-ef327240-8381-46b7-8a27-5dcaade09cff.png">


After confirming The Service is running, We would try to connect to the server Using the Public Ip from a web browser.

<img width="874" alt="image" src="https://user-images.githubusercontent.com/102925329/200564314-45fa5318-c2d6-460c-a877-51b364eea023.png">

We ran into an error of not being able to access from the Public Ip 
This occurs because we have not set needed Inbound rules to allow connection via Port 80
This can be done from the security groups

<img width="950" alt="image" src="https://user-images.githubusercontent.com/102925329/200564799-1d69089a-8a13-4cd4-bec2-3e8128574a47.png">

<img width="949" alt="image" src="https://user-images.githubusercontent.com/102925329/200564991-c3121609-e595-4618-9fcf-4870b59149b4.png">


After Adding needed inbound rules, we Reload the page 

<img width="898" alt="image" src="https://user-images.githubusercontent.com/102925329/200565255-66a550d3-892c-4998-a529-245154c3f535.png">

