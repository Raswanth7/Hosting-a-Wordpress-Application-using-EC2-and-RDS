# Hosting-a-Wordpress-Application-using-EC2-and-RDS 

## Pre-requisites 
You will have to launch an EC2 instance and also create a RDS database. Make sure our EC2 instance is able to access our RDS database by configuring the security group of our RDS database. 

### Here we have our RDS Database 

![Screenshot 2024-07-18 222415](https://github.com/user-attachments/assets/2305f58b-02b3-4e5a-8dd8-053bc65b8af5)

### Here we have our EC2 Instance

![Screenshot 2024-07-18 222559](https://github.com/user-attachments/assets/783ba1f2-b8fc-4fcf-a071-89c2c1dc3ec9)

## Let us connect to our EC2 
We will be connecting to our EC2 instance using PuTTy by logging in using our public IP and private key pair 

![Screenshot 2024-07-18 222626](https://github.com/user-attachments/assets/6fc0676f-dacd-4def-a7e5-dcb20f7971da) 

![Screenshot 2024-07-18 222657](https://github.com/user-attachments/assets/db28455b-03a5-481e-984f-482c7b014980)

## Now let us install MySQL and Configure MySQL  
Write the following commands to install mysql and connect to RDS: 
```bash
sudo yum install mysql -y
```
Enter the following command to set an environment variable for your MySQL host: 
```bash
export MYSQL_HOST=<your-endpoint>
```
 ![Screenshot 2024-07-18 222849](https://github.com/user-attachments/assets/d6ee2d50-bd1a-4178-bdbe-0b5b34035be0)  

 Connect to RDS by entering the following command: 
```bash
mysql -h <endpoint-of-RDS> -P <port-number> -u <username> -p
Enter password : <password>
```
Create a database user for your WordPress application and give the user permission to access the wordpress database. Run the following commands: 
```bash
CREATE DATABASE <database-name>;
CREATE USER '<database-username>' IDENTIFIED BY '<password>';
GRANT ALL PRIVILEGES ON wordpress.* TO <database-username>;
FLUSH PRIVILEGES;
Exit
```

## Install Wordpress 
First let us install our Apache Web Server and enable it. Run the following commands: 
```bash
sudo yum install httpd -y
sudo service httpd start
```

Now let us paste our public IP in our browser to check if our webserver is working 

![Screenshot 2024-07-18 223404](https://github.com/user-attachments/assets/3c3ed312-5b08-480b-9afa-ea6be13002ac) 

Go to browser and getlink for wordpress installation 

![Screenshot 2024-07-18 223532](https://github.com/user-attachments/assets/91e185ea-ae51-4c67-9f44-e14efd32b8ae) 

Now run the following command to install wordpress and unzip it: 
```bash
wget https://wordpress.org/latest.zip
unzip latest.zip
```
Then if you run ls to view the contents of your directory, you will see a latest file and a directory called wordpress with the uncompressed contents. 
```bash
ls
latest.zip  wordpress
```
Now move into wordpress directory, list the contents and create a copy of the default config file using the following commands:
```bash
cd wordpress/
ls
cp wp-config-sample.php wp-config.php
```
Now you will see the file wp-config.php 

![Screenshot 2024-07-18 224439](https://github.com/user-attachments/assets/90c13887-4295-45d9-877d-95f1630907ba) 

Run cat command to print the content: 
```bash
cat wp-config.php
```
Copy the content and paste it in your notepad: 

![Screenshot 2024-07-18 223630](https://github.com/user-attachments/assets/b40fc1fe-5f1d-4c10-8e58-30c28bd08681)

![Screenshot 2024-07-18 223708](https://github.com/user-attachments/assets/b445708a-1b38-478a-96f8-67c2e99aab0a) 

Edit the database configuration by changing the following lines: 

![Screenshot 2024-07-18 223814](https://github.com/user-attachments/assets/c5ae5a15-b3b9-4324-b7ca-de5c9fe17ef6)

Go to this link to generate values for this configuration section 

![Screenshot 2024-07-18 223910](https://github.com/user-attachments/assets/a92d9fc0-3413-4439-b014-2e4c36d3b8bf) 

![Screenshot 2024-07-18 223947](https://github.com/user-attachments/assets/6da842b2-d15c-4913-bb0f-786eccc63f29)

Replace the entire content in that section with the content from the link. 

![Screenshot 2024-07-18 224018](https://github.com/user-attachments/assets/807ed749-9d1b-48ad-8be7-4b56f46e3dd1) 

Now copy the entire content such that we will be using it insometime 

![Screenshot 2024-07-18 224145](https://github.com/user-attachments/assets/64f182cc-0f36-4159-ae16-5e35ba60a853) 

Now run vim command to edit the configuration. First delete the lines and replace/paste it by the content we have copied 
```bash
vim wp-config.php
```
![Screenshot 2024-07-18 224317](https://github.com/user-attachments/assets/a994b890-82d9-4bcb-9633-327afadae71f) 

Now run the cat command to print the content and check if the configuration is updated: 
```bash
cat wp-config.php
```
![Screenshot 2024-07-18 224524](https://github.com/user-attachments/assets/1be800a2-3e7c-4f0d-a0c8-0a57c549e938) 

![Screenshot 2024-07-18 224551](https://github.com/user-attachments/assets/85b26289-a990-4b6a-a501-b8b1f615e70c)  

## Install php and deploy Wordpress
Run the following command to install php:
```bash
sudo amazon-linux-extras install -y lamp-mariadb10.2-php7.2 php7.2
```
Now move to /home/ec2-user directory and list contents
```bash
cd ..
ls
```
Move all wordpress files into /var/www/html/ directory using command:
```bash
cp -r wordpress/* /var/www/html/
```
Get into /var/www/html/ and list the contents and you will see all the wordpress files there: 
```bash
cd /var/www/html/
ls
```
Now restart our webserver using command: 
```bash
service httpd restart
```
Now if you refresh our browser we get

![Screenshot 2024-07-18 224851](https://github.com/user-attachments/assets/3d1d21da-828e-42e5-8eb2-78c4b96c1a9d) 

Fill the details and click on install wordpress

![Screenshot 2024-07-18 224824](https://github.com/user-attachments/assets/d0e2b043-a6c3-42d0-aead-7356c21ba465) 

You will land into this login page

![Screenshot 2024-07-18 224919](https://github.com/user-attachments/assets/f1c1bd5d-0377-4d11-92fb-8a4cb62360ea) 

Now if you paste your public IP on the browser you get

![Screenshot 2024-07-18 224949](https://github.com/user-attachments/assets/c1248342-23b2-4dda-8dda-f539bb276053) 

We have successfully hosted our wordpress application 

![Screenshot 2024-07-18 225053](https://github.com/user-attachments/assets/7f229058-eef9-46df-96ff-89fb88682e85)
































 



