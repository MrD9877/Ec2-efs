# EC2

## Overview

This is a simple Node.js server setup using an AWS EC2 instance. In this project, we will go through how to set up an EC2 instance, secure it, access it, and finally test our Node.js server.
Example port http://ec2-51-20-72-204.eu-north-1.compute.amazonaws.com:3000

## Creating a EC2 Instance

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

## Accessing the EC2 Instance

### Browser Access

1. Go to EC2=-->Instance-->(your instance)
2. Click Connect
3. use EC2 Instance connect
4. Enter the username (default is usually `ec2-user` for Amazon Linux).
5. Click Connect.
6. You should now be connected to your EC2 instance via the browser.

### SSH Access (vs code)

1. [Add Security Group Which allow "Inbound Rules" type:ssh port:22 source:<Your IP>](#creating-a-security-group)
2. [In Same group add type:"Custom TCP" port:"<Port you will use in express server(eg: 3000)> source:"0.0.0.0/0"](#creating-a-security-group)
3. [Edit ssh config file in vs code](#edit-ssh-config-file)
4. Now you can connect to your EC2 instance using the command:

   ```bash
   ssh my-ec2-instance
   ```

> 💡 **TIP:**  
> If normal command not working like `npm i -g pm2` use `sudo npm i -g pm2`

### Running Express Server

1. Create a Express Server

```js
import express from "express";
const app = express();

app.get("/", (req, res) => {
  res.send("Hello from EC2!");
});
//use port you allowed in security group
app.listen(3000, "0.0.0.0", () => {
  console.log("Server running on port 3000");
});
```

2. SSH into your EC2 instance using the method described above.(Amazon Linux)
3. Update the package manager:

```bash
  sudo yum update -y
```

3. Install node js

```bash
sudo yum install -y nodejs
```

4. Install git

```bash
sudo yum install git -y
```

5. Clone and Run you repo.

> 💡 **TIP:**  
> Use pm2 to make production server run even when you exit ssh connection and for auto restart
>
> ```bash
> sudo npm i -g pm2
> npm run build
> pm2 start dist/src/index.js
> pn2 log
> ```

6. Use URl: http://ec2-51-20-72-204.eu-north-1.compute.amazonaws.com:3000

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

# EFS

## Overview

This is a simple guide to set up and use Amazon Elastic File System (EFS) with an EC2 instance. EFS provides scalable file storage that can be shared across multiple EC2 instances.

## Creating an EFS File System

1. Go to AWS Management Console.
2. Navigate to the EFS Dashboard.
3. Click on "Create file system".
4. Choose the VPC where your EC2 instance is located.
5. Configure the file system settings (default settings are usually fine).
6. Click "Create".

## Mounting EFS to EC2 Instance

> **⚠️ Important:** Add security rule to same Availability zone as Ec2\
> Add an Inbound Rule:\
> Type: NFS\
> Protocol: TCP\
> Port: 2049\
> Source: Select the Security Group of your EC2 instance.

> **⚠️ Important:** use this command to give acces to modify EFS in EC2
>
> ```bash
> sudo chown -R ec2-user:ec2-user /home/ec2-user/fs/efs
> ```

1. SSH into your EC2 instance using the method described above.
2. Install the NFS client:

```bash
sudo yum install -y nfs-utils
```

3. Create a directory to mount the EFS file system:

```bash
sudo mkdir /mnt/efs
```

4. Mount the EFS file system:

```bash
sudo mount -t nfs4 -o nfsvers=4.1 <file-system-id>.efs.<region>.amazonaws.com:/ /mnt/efs
```

Replace `<file-system-id>` with your EFS file system ID and `<region>` with the AWS region where your EFS is located. 5. Verify the mount:

```bash
df -h
```

You should see the EFS file system listed.

6. To make the mount persistent across reboots, add the following line to your `/etc/fstab` file:

```plaintext
<file-system-id>.efs.<region>.amazonaws.com:/ /mnt/efs nfs4 defaults,_netdev 0 0
```
