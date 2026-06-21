```markdown
# Linux EC2 Server Setup

## Project Overview

This project demonstrates how to create and configure a Linux cloud server using AWS EC2. The goal was to launch an Ubuntu server, connect with SSH, create a new Linux user, grant sudo access, update the server, and document the setup process.

This project is part of my hands-on Junior DevOps portfolio.

---

## Technologies Used

- AWS EC2
- Ubuntu Linux
- SSH
- Linux users and groups
- Security Groups
- Git
- GitHub
- Markdown

---

## Architecture

```text
Local Machine
     |
     | SSH using private key
     v
AWS EC2 Ubuntu Server
     |
     | Linux user configuration
     v
New sudo user: devops
What I Built
Created an Ubuntu EC2 instance in AWS
Configured a security group for SSH access
Connected to the server using SSH
Updated Linux system packages
Installed basic command-line tools
Created a new Linux user
Added the new user to the sudo group
Configured SSH access for the new user
Verified sudo access
Documented commands and screenshots
Steps Performed
1. Created EC2 Instance
I launched an Ubuntu EC2 instance in AWS using a free-tier eligible instance type.
2. Configured Security Group
I configured the security group to allow SSH access on port 22 from my IP address.
3. Connected Using SSH
I connected to the instance using an SSH private key.
ssh -i ~/.ssh/devops-linux-key.pem ubuntu@EC2_PUBLIC_IP
4. Updated the Server
sudo apt update
sudo apt upgrade -y
5. Installed Basic Tools
sudo apt install -y curl wget git tree unzip htop
6. Created a New User
sudo adduser devops
7. Added User to Sudo Group
sudo usermod -aG sudo devops
8. Configured SSH Access for New User
sudo mkdir -p /home/devops/.ssh
sudo cp /home/ubuntu/.ssh/authorized_keys /home/devops/.ssh/authorized_keys
sudo chown -R devops:devops /home/devops/.ssh
sudo chmod 700 /home/devops/.ssh
sudo chmod 600 /home/devops/.ssh/authorized_keys
9. Verified New User Access
ssh -i ~/.ssh/devops-linux-key.pem devops@EC2_PUBLIC_IP
whoami
sudo whoami
Screenshots
EC2 Instance Created

Security Group SSH Rule

SSH Login Successful

System Updated

New User Created

Sudo Access Verified

Key Commands Used
sudo apt update
sudo apt upgrade -y
sudo adduser devops
sudo usermod -aG sudo devops
groups devops
sudo whoami
More commands are documented in:
notes/commands-used.md
Problems Faced
One important issue when configuring SSH is making sure the private key has the correct permissions.
The fix was:
chmod 400 ~/.ssh/devops-linux-key.pem
Another important step was making sure the new user's .ssh folder and authorized_keys file had the correct ownership and permissions.
What I Learned
Through this project, I learned how to:
Launch a Linux cloud server
Connect to a remote server using SSH
Configure AWS security group access
Update Ubuntu packages
Create and manage Linux users
Add users to the sudo group
Configure SSH access for a non-default user
Document a DevOps project for GitHub
Resume Bullet
Created and configured an Ubuntu EC2 cloud server with SSH access, Linux user management, package updates, sudo permissions, and basic security hardening as part of a hands-on DevOps portfolio project.
