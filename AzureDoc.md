## LAMP STACK IMPLEMENTATION ON AZURE

This is a Walkthrough on steps to create a LAMP(Linux, Apache, MySQL, PHP or Python, or Perl)

 First Step would be to create the virtual machine on the azure portal 
 
 <img width="953" alt="image" src="https://user-images.githubusercontent.com/102925329/200814543-c783f61d-54be-4143-b7fa-a623e1bb602b.png">

Confirm Instance is Running

<img width="946" alt="image" src="https://user-images.githubusercontent.com/102925329/200816082-2611385a-8d74-428c-b207-0bef8fd1f2e7.png">

Now we connect to instance using the Public IP

**Successfully Connected**

<img width="564" alt="image" src="https://user-images.githubusercontent.com/102925329/200817358-68948075-de4b-4a2a-8e56-38cb0e0f43dc.png">

**INSTALLING APACHE AND UPDATING THE FIREWALL**

First of all we run the :
         
    sudo apt update    --to update the packages
    
Then we run : 

    sudo apt install apache2    #to install the apache package

To verify that apache2 is running as a Service in our OS, use following command :

    sudo systemctl status apache2

<img width="842" alt="image" src="https://user-images.githubusercontent.com/102925329/200818744-2e8d3814-7e8f-4dd5-85a5-31aaa74afa98.png">

