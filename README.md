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

## Browser Access

1. Go to EC2=-->Instance-->(your instance)
2. Click Connect
3. use EC2 Instance connect
4. Enter the username (default is usually `ec2-user` for Amazon Linux).
5. Click Connect.
6. You should now be connected to your EC2 instance via the browser.

## SSH Access (vs code)

1. [Add Security Group Which allow "Inbound Rules" type:ssh port:22](#creating-a-security-group)
2. [Edit ssh config file in vs code](#edit-ssh-config-file)
3. Now you can connect to your EC2 instance using the command:

   ```bash
   ssh my-ec2-instance
   ```

### Creating a Security Group

1. Go to the EC2 Dashboard.
2. Click on "Security Groups" in the left sidebar.
3. Click "Create Security Group".
4. Name your security group (e.g., `MySecurityGroup`).
5. Add a description (optional).
6. Under "Inbound rules", click "Add rule".
7. Select "SSH" from the Type dropdown.
8. Ensure the Port is set to 22.
9. For "Source", you can select "My IP" to restrict access to your IP address or "Anywhere" for broader access.
10. Click "Create security group".
11. Go to EC2 Dashboard and select your instance.
12. Click on "Actions" -> "Networking" -> "Change Security Groups".
13. Select the security group you just created.
14. Click "Assign Security Groups".

### Edit SSH Config File

1. Open your terminal or VS Code.
2. Navigate to your SSH config file, usually located at `~/.ssh/config`.(ctrl+shift+p)
3. If the file doesn't exist, create it.
4. Add the following configuration:

   ```plaintext
   Host my-ec2-instance
       HostName <your-ec2-public-ip>
       User ec2-user
       IdentityFile ~/.ssh/<your-key-pair-file>.pem
   ```

5. Save the file.
