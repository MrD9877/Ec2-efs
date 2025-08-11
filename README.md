# Overview

This is a simple Node.js server setup using an AWS EC2 instance. In this project, we will go through how to set up an EC2 instance, secure it, access it, and finally test our Node.js server.

# Creating a EC2 Instance

1. Go to AWS Management Console.
2. Navigate to the EC2 Dashboard.
3. Click on "Launch Instance".
4. Choose an Amazon Machine Image (AMI) (e.g., Amazon Linux 2).
5. Select an Instance Type (e.g., t2.micro for free tier).
6. Configure Instance Details (default settings are usually fine).
7. Add Storage (default settings are usually fine).
8. Add Tags (optional).
9. Configure Security Group:
   - Create a new security group or select an existing one.
   - Add rules to allow SSH (port 22) and HTTP (port 80) access.
10. Review and Launch the Instance.
11. Select or create a key pair for SSH access.
12. Click "Launch Instances".

# Accessing the EC2 Instance
