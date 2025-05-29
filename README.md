# ICT171 Cloud Project
Alessandro Alvarenga Sasso | 35573786

## Setting up AWS:
First, go to AWS at https://aws.amazon.com/ and log in
![image](https://github.com/user-attachments/assets/dfe22857-3b4f-4fb0-be1d-0dfe13301f62)  

<br/><br/>
Afterwards, access the Amazon EC2 dashboard but make sure that the correct region (Asia Pacific, Sydney) is selected.  
![image](https://github.com/user-attachments/assets/006d8ecd-0403-448e-b5c6-c5fc8ffa86dc)  

<br/><br/>
And now click the orange launch instance button.
![image](https://github.com/user-attachments/assets/14a696ed-4b48-433e-8d5f-2788e10cfa8b)    

<br/><br/>
Enter a name, the specifics aren't too important, and then select the Ubuntu operating system:
![image](https://github.com/user-attachments/assets/516a5a0c-93e7-43a9-9d09-fb465d021d80)    

<br/><br/>
If using the preexisting account, select the preexisting key pair and security group "launch-wizard-1". Otherwise, make a new RSA keypair and hold onto it, and create a new security group with both HTTP (80) and HTTPS (443) ports open, also increase the storage to 30 if you want to use all of the free storage available in the AWS free trial (I like going a bit under just in case:
> [!CAUTION]
> DO NOT LOSE THE KEY PAIR, or you will have to restart the entire process and take down the server.

(How to set up new key pair/security rules as mentioned above)
![image](https://github.com/user-attachments/assets/6b743bdf-156e-4437-a810-5c917341f941) 
![image](https://github.com/user-attachments/assets/a0fd06ee-df19-418d-9cc1-64aad4f29f88)
(How to use pre-existing ones)
![image](https://github.com/user-attachments/assets/da1ee96a-5d13-494f-adf7-6d20de2b6e07)    

<br/><br/>
Once the instance is created, go to the Elastic IP menu to allocate an Elastic IP that'll act as a static IP for use with the domain name later on.
![image](https://github.com/user-attachments/assets/17f90614-29bc-41d9-817e-e92902783458)  

<br/><br/>
Once in the menu, click on the orange Allocate IP address button:
![image](https://github.com/user-attachments/assets/b73ecfc7-f7a3-4233-a504-3dfa77fdc836)

<br/><br/>
Now click on the orange button again, as all the settings should be fine by default (you may want to double-check that the region is the same region as your server):
![image](https://github.com/user-attachments/assets/0f29dfcd-d63a-4953-b26c-6cd304ff3a7a)

<br/><br/>
The next step is to select the IP address and then select the associate option from the Actions menu. Also, record the IP address for later use (inside the orange box):
![image](https://github.com/user-attachments/assets/9b5aa2e0-562c-45ec-97c0-8fa12aa87e01)

Once in the associate menu, click on every field, and the instance you just created should show up in the drop-down box, which you can click on, and then click on Allow Reallocation and then the orange Associate button:
![image](https://github.com/user-attachments/assets/9a18d2dd-f63a-47eb-8d8e-834d29942419)


This is the end of setting up the AWS server.
<br/><br/>  

## Domain Name:
The next steps take place on https://www.namecheap.com/ after logging in (these steps are only required if the Elastic IP address changed):  
Once logged in, go to the Domain List and click on manage the domain name:
![image](https://github.com/user-attachments/assets/a2910103-2e55-4120-8835-1e364ed80d42)
<br/><br/>  

In the manage menu, edit both records and enter the IP address from the Elastic IP that we saved from earlier, then keep the TTL (orange box) as automatic/30 mins, which works fine, but I like using 60 mins. Depending on the time you put in, is the time you want to wait before proceeding. 
![image](https://github.com/user-attachments/assets/91cb0ca8-01d1-466c-89b6-fda3198e96e1)
<br/><br/>  
 
## Setting up a web server:  
### Basic (SSH) Linux setup
Boot up your Linux instance if it's not up already and bring up the terminal.

> [!NOTE]
> If you are using SSH from the same Linux machine to the same IP address/Domain name but using a new AWS instance, you'll get a warning and be told to run a command like `ssh-keygen -f '<KeyAddress>/.ssh/known_hosts' -R '<IPaddress/Domain Name'`. The terminal will provide a full version of it that you will just need to copy and paste.

1. Use the ssh command `ssh -i <address/file> ubuntu@<IPaddress>`, replacing the "address/file" with the location of the key pair on the Linux machine and replacing the "IPaddress" with the Elastic IP address or the domain name. Both options work.  
2. Run `sudo apt update`.  

And now the Linux installation is ready for use.  
<br/><br/>

### Install/run Apache:
Run `sudo apt install apache2` and Apache is all set up.
<br/><br/>

### Install/run Certbot (SSL certificate):
Time to get HTTPS available for the webpage. <br/><br/>
Run the following commands:  
```
1. sudo snap install --classic certbot
2. sudo ln -s /snap/bin/certbot /usr/bin/certbot
3. sudo certbot --apache
```
> (certbot, 2025)


## WordPress:
### Install/run WordPress:  
1. Copy and paste the below into the terminal and run it to install the dependencies required for WordPress:  
```
sudo apt install ghostscript \
                 libapache2-mod-php \
                 mysql-server \
                 php \
                 php-bcmath \
                 php-curl \
                 php-imagick \
                 php-intl \
                 php-json \
                 php-mbstring \
                 php-mysql \
                 php-xml \
                 php-zip
```
> (Mikołajczyk & Llewellyn, 2025)

<br/></br>
2. The installation of WordPress itself:  

```
sudo mkdir -p /srv/www
sudo chown www-data: /srv/www
curl https://wordpress.org/latest.tar.gz | sudo -u www-data tar zx -C /srv/www
```
> (Mikołajczyk & Llewellyn, 2025)

<br/></br>
3. Configuring Apache:  

Create a config file: `sudo touch /etc/apache2/sites-available/wordpress.conf` <br/>Edit file: `sudo nano /etc/apache2/sites-available/wordpress.`  

Copy and paste the following into the file opened with the previous 'nano' statement: <br/>
```
<VirtualHost *:80>
    DocumentRoot /srv/www/wordpress
    <Directory /srv/www/wordpress>
        Options FollowSymLinks
        AllowOverride Limit Options FileInfo
        DirectoryIndex index.php
        Require all granted
    </Directory>
    <Directory /srv/www/wordpress/wp-content>
        Options FollowSymLinks
        Require all granted
    </Directory>
</VirtualHost>
```
<br/>
Run: 

```
sudo a2ensite wordpress
sudo a2enmod rewrite
sudo a2dissite 000-default
sudo service apache2 reload
```
> (Mikołajczyk & Llewellyn, 2025)

<br/></br>
4. Database configuration:  

Run: `sudo mysql -u root`

Once running mysql:  

Run: `CREATE DATABASE wordpress;` and, make sure to replace <password> `CREATE USER wordpress@localhost IDENTIFIED BY '<password>';`

> [!WARNING]
> Instructions for the below. Copy and paste each line one at a time, after pasting the first line Shift+Enter to move onto the next line before pasting or you will run the line before you're done putting it together.

```
GRANT SELECT,INSERT,UPDATE,DELETE,CREATE,DROP,ALTER
    -> ON wordpress.*
    -> TO wordpress@localhost;
```
> (Mikołajczyk & Llewellyn, 2025)

Run: `FLUSH PRIVILEGES;` and `quit` and `sudo service mysql start` and with that done, the database is operational.   <br/></br>

5. Configure WordPress to connect to the database:  

Run: `sudo -u www-data cp /srv/www/wordpress/wp-config-sample.php /srv/www/wordpress/wp-config.php`

Replace <password> below with the same password created earlier:  
```
sudo -u www-data sed -i 's/database_name_here/wordpress/' /srv/www/wordpress/wp-config.php
sudo -u www-data sed -i 's/username_here/wordpress/' /srv/www/wordpress/wp-config.php
sudo -u www-data sed -i 's/password_here/<password>/' /srv/www/wordpress/wp-config.php
```

Go to: https://api.wordpress.org/secret-key/1.1/salt/ and save the contents of the page, then find:
```
define( 'AUTH_KEY',         'put your unique phrase here' );
define( 'SECURE_AUTH_KEY',  'put your unique phrase here' );
define( 'LOGGED_IN_KEY',    'put your unique phrase here' );
define( 'NONCE_KEY',        'put your unique phrase here' );
define( 'AUTH_SALT',        'put your unique phrase here' );
define( 'SECURE_AUTH_SALT', 'put your unique phrase here' );
define( 'LOGGED_IN_SALT',   'put your unique phrase here' );
define( 'NONCE_SALT',       'put your unique phrase here' );
```
and replace it using nano: `sudo -u www-data nano /srv/www/wordpress/wp-config.php` with the contents you saved from the webpage earlier.  </br></br>
> (Mikołajczyk & Llewellyn, 2025)

6. Setting up WordPress:

### Import Template:
AWS
AWS

## Script



## Reference List:
certbot. (2025). certbot instructions. Electronic Frontier Foundation. Retrieved 24/05/2025 from https://certbot.eff.org/instructions?ws=apache&os=snap
Mikołajczyk, N., & Llewellyn, L. (2025). Install and configure WordPress. Canonical Ltd. Retrieved 24/05/2025 from https://ubuntu.com/tutorials/install-and-configure-wordpress


