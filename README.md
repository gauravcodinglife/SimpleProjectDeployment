Automated CI/CD Pipeline for Static Web Deployment

<img width="82" height="20" alt="image" src="https://github.com/user-attachments/assets/52b1cd73-f511-4b64-aacd-ef5c47b9f944" />

License: MIT
AWS
Jenkins

📖 Overview
This project implements a fully automated Continuous Integration and Continuous Deployment (CI/CD) pipeline for a static payment website. The infrastructure leverages AWS EC2 for hosting, Apache2 as the web server, and Jenkins for orchestration, ensuring zero-downtime deployments and eliminating manual intervention.

🏗️ Architecture Diagram
mermaid

graph TD
    A[Developer] -->|Git Push| B(GitHub Repository)
    B -->|Webhook/Poll| C[Jenkins Server]
    C -->|SSH Connection| D[AWS EC2 Instance]
    D -->|Deploy Artifacts| E[/var/www/html]
    E -->|Serve| F[Apache2 Web Server]
    F -->|HTTP Port 80| G[End User Browser]
️ Tech Stack
Category	Technology
Cloud Provider	AWS EC2 (Ubuntu 22.04)
CI/CD Tool	Jenkins LTS
Web Server	Apache2
Version Control	Git & GitHub
Protocol	SSH (Key-Based Authentication)
OS	Linux (Ubuntu)
️ Pipeline Workflow
Code Commit: Developer pushes HTML/CSS/JS changes to the main branch.
Trigger: Jenkins detects changes via SCM polling or webhook.
Checkout: Jenkins pulls the latest source code into the workspace.
Transfer: Artifacts are securely transferred via SSH to the EC2 instance.
Deployment: Files are placed in /var/www/html.
Restart: Apache service is restarted to reflect changes.
Live: Website is updated instantly at the Public IP.
📋 Prerequisites
AWS Account with EC2 access
Jenkins Server (Installed & Configured)
GitHub Account
SSH Key Pair (.pem file)
Basic knowledge of Linux commands
🚀 Installation & Setup
1. Infrastructure Provisioning (AWS EC2)
Launch an Ubuntu EC2 instance and configure Security Groups:

Type	Port	Source
SSH	22	Your IP
HTTP	80	0.0.0.0/0
Jenkins	8080	Your IP
2. Web Server Configuration
Connect to EC2 and install Apache:

Bash

sudo apt update
sudo apt install apache2 -y
sudo systemctl start apache2
sudo systemctl enable apache2
3. Jenkins Configuration
Install Plugin: Publish Over SSH
Configure SSH Server:
Name: local-apache
Hostname: localhost (If Jenkins is on EC2) or EC2_Public_IP
Username: ubuntu
Remote Directory: /var/www/html
Key: Paste private key content
Sudo Permissions:
Allow Jenkins to restart Apache without a password:
Bash

sudo visudo
# Add: jenkins ALL=(ALL) NOPASSWD: ALL
📂 Jenkins Job Configuration
Parameter	Value
Source Files	web/**
Remove Prefix	web
Exec Command	sudo systemctl restart apache2
🌐 Accessing the Application
Once the build is successful, access the deployed website via:

text

http://<YOUR-EC2-PUBLIC-IP>
🔒 Security Best Practices
SSH Keys: Private keys are stored in Jenkins Credentials Store, never in code.
Security Groups: Restricted access to port 22 and 8080.
Least Privilege: Jenkins user granted only necessary sudo permissions.
No Root Login: SSH access configured for ubuntu user only.
📸 Screenshots
✅ Jenkins Build Success
(Upload screenshot of your Jenkins Console Output showing SUCCESS)

✅ Live Website
(Upload screenshot of your website running on EC2 IP)

✅ SSH Configuration
(Upload screenshot of your Publish Over SSH settings)

🧩 Troubleshooting
Issue	Solution
Permission Denied	Ensure jenkins user has sudo access (visudo)
SSH Connection Failed	Verify Security Group allows Port 22
403 Forbidden	Check /var/www/html ownership (www-data)
Jenkins 403 Crumb Error	Logout/Login or check CSRF protection settings
🎯 Learning Outcomes
Implemented end-to-end CI/CD automation.
Configured secure SSH key-based authentication.
Managed Linux services (Apache) via Jenkins.
Deployed static web assets to cloud infrastructure.


👨‍💻 Author
Gaurav
DevOps Engineer | Cloud Enthusiast



