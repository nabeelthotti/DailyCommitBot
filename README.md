# Daily Commit Bot

## Overview
The Daily Commit Bot is an automated system designed to perform daily commits on a specified GitHub repository. Running on an AWS EC2 instance, this bot commits to a GitHub repository five times a day, appending the current date and time to a text file (`daily_commit.txt`) and pushing the update. This ensures consistent activity across the repository, which is useful for demonstration purposes, continuous integration testing, or keeping a repository active.

## Purpose
The primary purpose of this project is to demonstrate:
- The integration of GitHub with automated scripts running in a cloud environment.
- The use of AWS EC2 for scheduling and executing routine tasks.
- Basic automation practices in software development that can be applied to larger CI/CD pipelines.

## Technology Stack
- AWS EC2
- GitHub
- Bash Scripting
- Cron (for scheduling tasks)

## Getting Started

### Prerequisites
- An AWS account
- A GitHub account
- Basic familiarity with Linux commands and SSH

### Setting Up Your EC2 Instance
1. **Log into AWS Management Console and access EC2 Dashboard**:
   Navigate to the EC2 section and launch a new instance.

2. **Configure and Launch the Instance**:
   - Select the "Amazon Linux 2 AMI" as it's free tier eligible.
   - Choose a `t2.micro` instance type.
   - Configure instance details, add storage if needed, and configure a security group allowing SSH access.

3. **SSH Key Pair**:
   - Create a new key pair during instance setup.
   - Download the private key to your local machine and keep it secure.

### Configuring Your Environment
1. **SSH into Your Instance**:
   - Use the following command to SSH into your instance, replacing `/path/to/your-key.pem` with the actual path to your SSH private key, and `your-ec2-public-dns` with the public DNS of your EC2 instance:
     ```bash
     ssh -i /path/to/your-key.pem ec2-user@your-ec2-public-dns
     ```

2. **Install Git and Setup Environment**:
   - Update your instance and install Git:
     ```bash
     sudo yum update -y
     sudo yum install git -y
     ```
   - Configure Git with your details:
     ```bash
     git config --global user.name "Your Name"
     git config --global user.email "your_email@example.com"
     ```

3. **Clone Your Repository**:
   - Clone your GitHub repository:
     ```bash
     git clone git@github.com:your-username/your-repository.git
     ```
   - Navigate to the repository directory:
     ```bash
     cd your-repository
     ```

### Creating the Script
1. **Script Creation**:
   - Create `commit_script.sh` in your repository directory with the following content:
     ```bash
     #!/bin/bash
     cd /path/to/your/repository
     git pull
     echo "Commit on $(date)" >> daily_commit.txt
     git add daily_commit.txt
     git commit -m "Auto-commit on $(date)"
     git push origin main
     ```
   - Make the script executable:
     ```bash
     chmod +x commit_script.sh
     ```

2. **Schedule with Cron**:
   - Edit your crontab to schedule the script:
     ```bash
     crontab -e
     0 8,11,14,17,20 * * * /path/to/your/repository/commit_script.sh
     ```

## Security Considerations
Ensure your AWS EC2 instance is secured by:
- Limiting SSH access (via security groups) to your IP address.
- Regularly updating your instance with the latest security patches.
- Monitoring access and usage regularly.

## Contributing
Contributions to this project are welcome! Please fork the repository, make your changes, and submit a pull request.

