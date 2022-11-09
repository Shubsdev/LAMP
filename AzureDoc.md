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

After confirming The Service is running, We would try to connect to the server Using the Public Ip from a web browser.

<img width="519" alt="image" src="https://user-images.githubusercontent.com/102925329/200819330-0708d972-9ed3-4fe0-bed4-4280830a38eb.png">

We ran into an error of not being able to access from the Public Ip just like in AWS
This occurs because we have not set needed Inbound rules to allow connection via Port 80
This can be done from the security groups but the steps are a little different in Azure

We go to the Networking Blade of the VM 

<img width="956" alt="image" src="https://user-images.githubusercontent.com/102925329/200819832-8555c309-f09a-4726-86ba-ec2b7902eb72.png">

After adding Port 80 Inbound rule :

<img width="947" alt="image" src="https://user-images.githubusercontent.com/102925329/200821149-212b322d-ce83-4af6-ad6e-325054e7dee0.png">

After Adding needed inbound rules, we Reload the page

<img width="949" alt="image" src="https://user-images.githubusercontent.com/102925329/200821327-53bfbeb2-8215-4cf5-bc00-66e2da864820.png">

**Connected Successfully**


**INSTALLING MYSQL**

Now that you have a web server up and running, you need to install a Database Management System (DBMS) to be able to store and manage data for your site in a relational database. MySQL is a popular relational database management system used within PHP environments, so we will use it in our project.

We use the command : 
     
    sudo apt install mysql-server
    
Log Into the MYSQL console with the command : 

    sudo mysql
    
    
<img width="491" alt="image" src="https://user-images.githubusercontent.com/102925329/200822197-0568a64c-7140-4290-bcc8-eb5c8d684545.png">

Run this script to lock down database and set password: 

    ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'PassWord.1';

Secure Installation

    sudo mysql_secure_installation
    
Test to login into MYSQL console : 

    sudo mysql -p
    
<img width="493" alt="image" src="https://user-images.githubusercontent.com/102925329/200823085-0d47aea1-063a-4eb1-8f77-52e3ba9b1b94.png">

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
    
<img width="493" alt="image" src="https://user-images.githubusercontent.com/102925329/200824930-1eaeb979-f101-4325-86a6-beed1e937908.png">

Your new website is now active, but the web root /var/www/lampproject is still empty. Create an index.html file in that location so that we can test that the virtual host works as expected:

     sudo echo 'This is for AZURE GITHUB and the current hostname is:' $(curl -s http://169.254.169.254/latest/meta-data/public-hostname) 'with public IP' $(curl -s http://169.254.169.254/latest/meta-data/public-ipv4) > /var/www/lampproject/index.html
     
Now load the Public IP: 

<img width="943" alt="image" src="https://user-images.githubusercontent.com/102925329/200825743-97f348f8-194e-4794-9bad-4c0a87be424b.png">

We still have previous page, this is because I forgot to run the 
   
    sudo systemctl reload apache2
    
So as to reload the service and apply changes 

   <img width="396" alt="image" src="https://user-images.githubusercontent.com/102925329/200826268-3afac3d1-3a43-48c7-a79c-9b5b6b98ad35.png">
