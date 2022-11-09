## LAMP STACK IMPLEMENTATION ON AWS

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

First of all we run the :
         
    sudo apt update     

<img width="461" alt="image" src="https://user-images.githubusercontent.com/102925329/200561441-36f28143-1f69-49e1-aed3-a77a32d07102.png">

Then we run : 

    sudo apt install apache2    #to install the apache package

To verify that apache2 is running as a Service in our OS, use following command :

    sudo systemctl status apache2

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

**Connected Successfully**


**INSTALLING MYSQL**

Now that you have a web server up and running, you need to install a Database Management System (DBMS) to be able to store and manage data for your site in a relational database. MySQL is a popular relational database management system used within PHP environments, so we will use it in our project.

We use the command : 
     
    sudo apt install mysql-server
    
Log Into the MYSQL console with the command : 

    sudo mysql

<img width="419" alt="image" src="https://user-images.githubusercontent.com/102925329/200566941-22895ccb-efec-4a9c-8cdb-8fd98fee6927.png">

Run this script to lock down database and set password: 

    ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'PassWord.1';

Secure Installation

    sudo mysql_secure_installation
    
Test to login into MYSQL console : 

    sudo mysql -p

<img width="448" alt="image" src="https://user-images.githubusercontent.com/102925329/200570038-a7693fcd-00f0-4e94-a3b9-d7f02a064718.png">

**LOGIN SUCCESSFUL**

**INSTALLING PHP**

You have Apache installed to serve your content and MySQL installed to store and manage your data. PHP is the component of our setup that will process code to display dynamic content to the end user. In addition to the php package, you’ll need php-mysql, a PHP module that allows PHP to communicate with MySQL-based databases. You’ll also need libapache2-mod-php to enable Apache to handle PHP files. Core PHP packages will automatically be installed as dependencies.

To install all packages needed : 

    sudo apt install php libapache2-mod-php php-mysql
    
To confirm Php installed : 
     
     php -v

## CREATE A VIRTUAL HOST USING APACHE
Apache on Ubuntu 20.04 has one server block enabled by default that is configured to serve documents from the /var/www/html directory.
We will leave this configuration as is and will add our own directory next next to the default one.

To create the new directory : 

    sudo mkdir /var/www/lampproject

Assign ownership of the directory with your current system user: 

    sudo chown -R $USER:$USER /var/www/lampproject

Then, create and open a new configuration file in Apache’s sites-available directory using your preferred command-line editor: 
 
    sudo vi /etc/apache2/sites-available/lampproject.conf

This will create a new blank file. Paste in the following bare-bones configuration by hitting on i on the keyboard to enter the insert mode, and paste the text:

    <VirtualHost *:80>

    ServerName lampproject
    
    ServerAlias www.lampproject 
    
    ServerAdmin webmaster@localhost
    
    DocumentRoot /var/www/lampproject
    
    ErrorLog ${APACHE_LOG_DIR}/error.log
    
    CustomLog ${APACHE_LOG_DIR}/access.log combined

    </VirtualHost>


You can now use a2ensite command to enable the new virtual host: 

    sudo a2ensite lampproject
    
 You might want to disable the default website that comes installed with Apache. This is required if you’re not using a custom domain name, because in this case Apache’s default configuration would overwrite your virtual host. To disable Apache’s default website use a2dissite command , type:
 
    sudo a2dissite 000-default
    
 <img width="497" alt="Screenshot 2022-11-08 203815" src="https://user-images.githubusercontent.com/102925329/200659070-20898392-5cf9-4680-b2ed-c99f29ec30d2.png">

Your new website is now active, but the web root /var/www/lampproject is still empty. Create an index.html file in that location so that we can test that the virtual host works as expected:

     sudo echo 'This is for GITHUB and the current hostname is:' $(curl -s http://169.254.169.254/latest/meta-data/public-hostname) 'with public IP' $(curl -s http://169.254.169.254/latest/meta-data/public-ipv4) > /var/www/lampproject/index.html
     
Now load the Public IP: 

<img width="521" alt="image" src="https://user-images.githubusercontent.com/102925329/200661001-647fc775-0fcf-4da1-9413-1b59f18543c3.png">


 
