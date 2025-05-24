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
If using the preexisting account, select the preexisting key pair and security group "launch-wizard-1". Otherwise, make a new RSA keypair and hold onto it, and create a new security group with both HTTP (80) and HTTPS (443) ports open, also increase the storage to about 25 to remain under the free limit:
> [!CAUTION]
> DO NOT LOSE THE KEY PAIR, or you will have to restart the entire process and take down the server.

![image](https://github.com/user-attachments/assets/da1ee96a-5d13-494f-adf7-6d20de2b6e07)    

<br/><br/>
Once the instance is created, allocate an elastic IP that'll act as a static IP for use with the domain name later on.
![image](https://github.com/user-attachments/assets/17f90614-29bc-41d9-817e-e92902783458)  

<br/><br/>
The next step is to select the IP address (or click on Allocate Elastic IP address to create one beforehand) and then select the associate option from the Actions menu. Also, record the IP address for later use (inside the orange box):
![image](https://github.com/user-attachments/assets/9b5aa2e0-562c-45ec-97c0-8fa12aa87e01)

This is the end of setting up the AWS server.
<br/><br/>  

## Domain Name:
The next steps take place on https://www.namecheap.com/ after logging in (these steps are only required if the Elastic IP address changed):  
Once logged in, go to the Domain List and click on manage the domain name:
![image](https://github.com/user-attachments/assets/a2910103-2e55-4120-8835-1e364ed80d42)
<br/><br/>  

In the manage menu, edit both records and enter the IP address from the Elastic IP that we saved from earlier, then keep the TTL (orange box) as automatic/30 mins, which works fine, but I like using 60 mins. Depending on the time you put in, is the time you want to wait before proceeding. 
![image](https://github.com/user-attachments/assets/91cb0ca8-01d1-466c-89b6-fda3198e96e1)
<br/><br/>  <br/><br/>  
 
## Setting up a web server:  
### Basic (SSH) Linux setup
Boot up your Linux instance if it's not up already and bring up the terminal.

> [!NOTE]
> If you are using SSH from the same Linux machine to the same IP address/Domain name but using a new AWS instance, you'll get a warning and be told to run a command like `ssh-keygen -f '<KeyAddress>/.ssh/known_hosts' -R '<IPaddress/Domain Name'`. The terminal will provide a full version of it that you will just need to copy and paste.

1. Use the ssh command `ssh -i <address/file> ubuntu@<IPaddress>`, replacing the "address/file" with the location of the key pair on the Linux machine and replacing the "IPaddress" with the Elastic IP address or the domain name. Both options work.  
2. Run `sudo apt update`.  

And now the Linux installation is ready for use.  

### Install/run Apache:
Run `sudo apt install apache2` and Apache is all set up.

### Install/run Certbot (SSL certificate):
Time to get HTTPS available for the webpage. <br/><br/>
Run the following commands:  
```
1. sudo snap install --classic certbot
2. sudo ln -s /snap/bin/certbot /usr/bin/certbot
3. sudo certbot --apache  
(certbot, 2025)
```
 
## WordPress:
AWS
AWS
### Install/run WordPress:
AWS
AWS
### Import Template:
AWS
AWS

## Script



## Reference List:
certbot. (2025). certbot instructions. Electronic Frontier Foundation. Retrieved 24/05/2025 from https://certbot.eff.org/instructions?ws=apache&os=snap


