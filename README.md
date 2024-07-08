# Single-Endpoint-with-Nginx
This is an EC2 and S3 bucket architecture with a Reverse Proxy 

# SINGLE END POINT ARCHITECTURE FOR EC2 AND S3 SERVICES

## SUMMARY

This Project demonstrates the integration of EC2 and S3 facilitated by a reverse proxy setup.
The aim is to showcase how corporate firms provide a seamless experience for managing and accessing diverse resources under a unified access point.

In this project, we will launch an EC2 instance on AWS, create an S3 bucket and configure Nginx as the reverse proxy.

EC2: hosts the core application, enabling scalable computing capacity to meet customer demands.
S3: Offers secure, scalable object storage for vast amount of data to serve customers efficiently and reliably. 

We will configure (Nginx) as the Reverse Proxy on the EC2 instance, directing requests to the appropriate destination; either to the EC2-hosted application or the S3 bucket based on the request path.

We will create an object in the S3 bucket (an **_index.html_** file) which will be served through the reverse proxy.

The target is to have unified access output through Nginx which will look like this:



![alt text](<../Images/Image 10.PNG>)

