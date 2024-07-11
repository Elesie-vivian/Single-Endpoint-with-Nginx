# Single-Endpoint-with-Nginx
This is an EC2 and S3 bucket architecture with a Reverse Proxy 

# SINGLE END POINT ARCHITECTURE FOR EC2 AND S3 SERVICES

## SUMMARY
This Project demonstrates the integration of EC2 and S3 facilitated by a reverse proxy setup. 
The aim is to showcase how corporate firms provide a seamless experience for managing and accessing diverse resources under a unified access point.
EC2: hosts the core application, enabling scalable computing capacity to meet customer demands.

S3: Offers secure, scalable object storage for vast amount of data to serve customers efficiently and reliably


### Achieving Unified Access: The Role of a Reverse Proxy

A reverse Proxy will be configured on the EC2 instance, directing requests to the appropriate destination; either to the EC2-hosted application or 
the S3 bucket based on the request path.
This setup exemplifies a best practice in cloud architecture, providing both operational simplicity and enhanced security.


**Project Setup**

1.	For this project, we will setup two separate chrome browser profiles.
2.	AWS management console access using John IAM account 
3.	EC2 Instance Setup: We will launch an EC2 that will host the main application and associate it with an Elastic IP.
4.	S3 Bucket Configuration: using Mary IAM account. Here we will create
and configure an S3 bucket for storing application data, ensuring proper permissions and web hosting settings are in place.
5.	Reverse Proxy Configuration: Install and configure a web server (e.g., Nginx) on the EC2 instance to act as a reverse proxy, routing requests to either to the application or the S3 bucket based on the URL path.

## STEP 1
## Setting up the EC2 Instance

•	Log on to the AWS console 
•	Launch an Ubuntu EC2 instance
•	Assign a Static IP (Elastic IP): Associate an Elastic IP address with 
the EC2 instance to ensure it retains the same public IP address across reboots.

![alt text](<Images/Image 1.PNG>)

![alt text](<Images/Image 2.PNG>)


 ## STEP 2
 ## Creating S3 Bucket

 1.	Log on to the console as Mary
 2.	Create a new bucket and give it a name of your choice, (we name this bucket maryjanes3123). 

 ![alt text](<Images/Image 3.PNG>)

3.	Create a new object inside the bucket.Here, you would upload an index.html file containing a simple content.
•	On your computer, create an index.html file with the content Welcome to Amazon S3
•	Upload the index.html file on S3 bucket as shown below.

![alt text](<Images/Image 4.PNG>)


## STEP 3
## Configuring S3 Bucket for Web Hosting

•	Enable Static Website Hosting. In the S3 bucket settings, we will enable static website hosting to make our bucket content accessible via HTTP

Note: This we achieve by making edits under properties configuration.

![alt text](<Images/Image 5.PNG>)

## STEP 4
## Configuring a Web Server as a Reverse Proxy

Now, we have successfully assigned an elastic IP to our instance, let’s install nginx on it.
Nginx is an open-source web server, proxy server, reverse proxy server, load balancer and HTTP cache. 
It has a high performance, stability and simple configuration and low resource consumption.

1.	 On your EC2 instance, Install Nginx web server

sudo apt update -y && sudo apt install nginx -y


   ![alt text](<Images/Image 6.PNG>) 



2. Configure the web server: Configure the web server to serve your S3 app directly and to forward requests to your S3 bucket

Create and edit a new file called mybucket.

   sudo nano /etc/nginx/sites-available/mybucket

   On the nano editor , paste the configuration code snippet below in the file and replace the highlighted part with your own generated links 


   server {
    listen 80;
    server_name 100.26.55.173;  # Replace with your domain name or server IP address

    location / {
        proxy_pass https://your-bucket-name.s3.amazonaws.com; # Replace 
        with the link you generated after you enabled  static web hosting for your bucket
        proxy_set_header Host your-bucket-name.s3.amazonaws.com;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}


Note: For Server name; we replace with EC2 domain name or server IP address, ie the elastic IP address.

Proxy_pass https; we replace with http://maryjanes3123.s3.amazonaws.com

Proxy_set_header Host maryjanes3123.s3.amazonaws.com;

Also, we edit inbound rules under security groups in our EC2 instance to http port 80.

3.	Make the index.html file public

_Follow the steps below to make the index.html file public_

Navigate to the *index.html* file, click on *Actions* and then click on *Make Public using ACL*

![alt text](<Images/Image 7.PNG>)


## STEP 5
## Testing and Validation

After setting up the EC2 instance, configuring the S3 bucket and establishing a reverse proxy, time to test and validate these configurations.
This step is essential to ensure that our setup works as intended and both the EC2-hosted application and the S3 bucket content are accessible through the unified access point we have created.

**Testing Steps:**

1.	Direct Application Access:

•	Access the application hosted on the EC2 instance directly through the Elastic IP to confirm its running as expected.
•	Verify that the application responds to requests and functions correctly without any reverse proxy interference

![alt text](<Images/Image 8.PNG>)


2.	S3 Bucket Access via reverse Proxy:
•	Attempt to access the S3 bucket content by navigating to `<IP-address>/bucket-name`
•	Check that the reverse proxy correctly routes the request to the S3 bucket and that the content is served without any issues.


**Further Validations:**

Consistency in Access: We check and confirm that the Elastic IP remains stable across reboots, eliminating the need to adjust access configurations frequently.

Unified Endpoint functionality: Ensure that the unified access point reliably routes traffic to both the EC2 application and the S3 bucket based on the request path.





