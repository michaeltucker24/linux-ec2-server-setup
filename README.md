# AWS EC2 Linux Server Setup and Access Control Lab

## Overview

This repository documents the setup and basic access configuration of an Ubuntu Linux server running on AWS EC2.

The purpose of this project is to practice common junior DevOps and cloud support tasks, including launching a cloud server, connecting with SSH, configuring security group access, creating a non-default Linux user, assigning sudo privileges, and verifying server access.

This project is part of my Junior DevOps portfolio and focuses on Linux, AWS, SSH, user management, and basic cloud server administration.

---

## Scenario

A new Ubuntu Linux server has been provisioned in AWS EC2 for a small web application or internal project.

Before the server can be used, it needs to be configured for secure administrative access. The default cloud user is used only for initial setup, and a separate Linux user is created for ongoing administration.

The task is to configure the server, verify SSH access, assign sudo permissions, and document the setup process.

---

## Objectives

* Launch an Ubuntu EC2 instance
* Configure an AWS security group for SSH access
* Connect to the server using SSH
* Update system packages
* Install basic administration tools
* Create a new Linux user
* Add the new user to the sudo group
* Configure SSH access for the new user
* Verify sudo access
* Review basic server information
* Document commands, findings, and verification steps

---

## Environment

| Component        | Details                       |
| ---------------- | ----------------------------- |
| Cloud Provider   | AWS                           |
| Service          | EC2                           |
| Operating System | Ubuntu Linux                  |
| Shell            | Bash                          |
| Access Method    | SSH                           |
| Documentation    | GitHub README and screenshots |

---

## Tools and Commands Used

| Tool / Command        | Purpose                            |
| --------------------- | ---------------------------------- |
| `ssh`                 | Connect to the EC2 instance        |
| `apt`                 | Update packages and install tools  |
| `adduser`             | Create a new Linux user            |
| `usermod`             | Add user to the sudo group         |
| `groups`              | Verify group membership            |
| `id`                  | Verify user account details        |
| `chmod`               | Set file and directory permissions |
| `chown`               | Set file and directory ownership   |
| `systemctl`           | Review system services             |
| `cat /etc/os-release` | Verify operating system details    |

---

## Skills Demonstrated

* AWS EC2 instance setup
* Security group configuration
* SSH key-based authentication
* Linux package management
* Linux user administration
* sudo privilege management
* SSH access configuration
* Basic server validation
* Technical documentation
* Junior DevOps troubleshooting workflow

---

## Server Setup Workflow

### 1. Launch EC2 Instance

Created an Ubuntu EC2 instance in AWS.

Recommended instance type:

```bash
t2.micro
```

or:

```bash
t3.micro
```

---

### 2. Configure Security Group

Configured inbound SSH access.

```text
Type: SSH
Protocol: TCP
Port: 22
Source: My IP
```

This allows SSH access while avoiding unnecessary exposure to the public internet.

---

### 3. Connect to Server with SSH

Connected to the server using the downloaded private key.

```bash
ssh -i ~/.ssh/devops-linux-key.pem ubuntu@EC2_PUBLIC_IP
```

---

### 4. Confirm Server Access

Verified the logged-in user, hostname, and operating system.

```bash
whoami
hostname
cat /etc/os-release
```

Expected user:

```bash
ubuntu
```

---

### 5. Update System Packages

Updated the Ubuntu package index and upgraded installed packages.

```bash
sudo apt update
sudo apt upgrade -y
```

---

### 6. Install Basic Administration Tools

Installed common tools used during Linux administration.

```bash
sudo apt install -y curl wget git tree unzip htop
```

---

### 7. Create a New Linux User

Created a new non-default Linux user.

```bash
sudo adduser devops
```

Verified the user:

```bash
id devops
```

Example output:

```text
uid=1001(devops) gid=1001(devops) groups=1001(devops)
```

---

### 8. Add User to Sudo Group

Granted administrative privileges by adding the user to the sudo group.

```bash
sudo usermod -aG sudo devops
```

Verified group membership:

```bash
groups devops
```

Expected output should include:

```text
sudo
```

---

### 9. Configure SSH Access for New User

Created an SSH directory for the new user.

```bash
sudo mkdir -p /home/devops/.ssh
```

Copied the existing authorized key.

```bash
sudo cp /home/ubuntu/.ssh/authorized_keys /home/devops/.ssh/authorized_keys
```

Updated ownership and permissions.

```bash
sudo chown -R devops:devops /home/devops/.ssh
sudo chmod 700 /home/devops/.ssh
sudo chmod 600 /home/devops/.ssh/authorized_keys
```

---

### 10. Verify New User SSH Access

Logged in as the new user.

```bash
ssh -i ~/.ssh/devops-linux-key.pem devops@EC2_PUBLIC_IP
```

Verified identity:

```bash
whoami
```

Expected result:

```bash
devops
```

Verified sudo access:

```bash
sudo whoami
```

Expected result:

```bash
root
```

---

### 11. Review Basic Server Information

Ran basic Linux commands to review server state.

```bash
uptime
df -h
free -h
ip addr
lsb_release -a
```

Reviewed running services:

```bash
systemctl list-units --type=service --state=running
```

---

## Common Issues and Fixes

### Issue 1: Permission denied when connecting with SSH

Possible causes:

* Incorrect private key path
* Private key permissions are too open
* Wrong username
* Security group does not allow SSH from your IP

Fix:

```bash
chmod 400 ~/.ssh/devops-linux-key.pem
```

Then reconnect:

```bash
ssh -i ~/.ssh/devops-linux-key.pem ubuntu@EC2_PUBLIC_IP
```

---

### Issue 2: New user cannot use sudo

Possible cause:

The user was not added to the sudo group.

Fix:

```bash
sudo usermod -aG sudo devops
groups devops
```

---

### Issue 3: New user cannot SSH into server

Possible causes:

* Missing `.ssh` directory
* Missing `authorized_keys` file
* Incorrect ownership
* Incorrect permissions

Fix:

```bash
sudo mkdir -p /home/devops/.ssh
sudo cp /home/ubuntu/.ssh/authorized_keys /home/devops/.ssh/authorized_keys
sudo chown -R devops:devops /home/devops/.ssh
sudo chmod 700 /home/devops/.ssh
sudo chmod 600 /home/devops/.ssh/authorized_keys
```

---

### Issue 4: SSH works, but only for the default Ubuntu user

Possible cause:

The SSH public key was only configured for the default `ubuntu` user.

Fix:

Copy the existing authorized key to the new user's `.ssh` directory and correct the ownership and permissions.

```bash
sudo cp /home/ubuntu/.ssh/authorized_keys /home/devops/.ssh/authorized_keys
sudo chown -R devops:devops /home/devops/.ssh
sudo chmod 700 /home/devops/.ssh
sudo chmod 600 /home/devops/.ssh/authorized_keys
```

---

## Verification Checklist

Before considering the server setup complete, verify:

* [ ] EC2 instance is running
* [ ] SSH security group rule allows access from your IP
* [ ] SSH login works with the default Ubuntu user
* [ ] System packages are updated
* [ ] Basic tools are installed
* [ ] New Linux user exists
* [ ] New user is part of the sudo group
* [ ] New user can SSH into the server
* [ ] New user can run sudo commands
* [ ] Basic server information was reviewed
* [ ] Commands are documented
* [ ] Screenshots are included
* [ ] EC2 instance is stopped or terminated when no longer needed

---

## Customer-Facing Update Example

The Ubuntu EC2 server was configured successfully. SSH access was verified, system packages were updated, a new administrative Linux user was created, and sudo access was confirmed. The server is ready for additional application deployment or configuration work.

---

## Lessons Learned

This project reinforced the importance of validating cloud server access at multiple layers:

* AWS security groups
* SSH key permissions
* Linux user configuration
* Group membership
* sudo access

A working cloud server is not just about launching an EC2 instance. It also requires secure access, proper user setup, and verification that the server can be administered safely.

---

## Portfolio Note

This project is part of my Junior DevOps portfolio. It demonstrates foundational cloud and Linux skills required for junior DevOps, cloud support, and Linux support roles.
