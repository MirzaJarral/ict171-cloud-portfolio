# ICT171 Cloud Server Project - Personal Portfolio

**Student Name:** Mirza Jarral
**Student Number:** 35017797
**Domain:** https://mirzajarral.work
**Server IP:** 15.135.223.103

---

## Project Overview

A personal portfolio website hosted on AWS EC2 (t3.micro, Ubuntu 24.04) with Nginx web server and a custom domain from GoDaddy (mirzajarral.work). SSL/TLS is enabled for secure HTTPS access using Let's Encrypt. This project demonstrates Infrastructure as a Service (IaaS) implementation for ICT171.

---

## Server Specifications

| Item | Details |
|------|---------|
| Cloud Provider | Amazon Web Services (AWS) |
| Service | EC2 (IaaS) |
| Region | ap-southeast-2 (Sydney) |
| Instance Type | t3.micro |
| OS | Ubuntu 24.04.4 LTS (Noble) |
| Web Server | Nginx |
| SSL/TLS | Enabled (Let's Encrypt) |
| SSL Certificate Name | mirzajarral.work |
| SSL Expiry | July 22, 2026 (61 days valid as of May 2026) |

---

## Table of Contents

1. [AWS Account Setup](#1-aws-account-setup)
2. [EC2 Instance Launch](#2-ec2-instance-launch)
3. [Connect to Your Server (SSH)](#3-connect-to-your-server-ssh)
4. [Install Nginx Web Server](#4-install-nginx-web-server)
5. [Deploy Portfolio Files](#5-deploy-portfolio-files)
6. [GoDaddy Domain & DNS](#6-godaddy-domain--dns)
7. [Configure Nginx for Custom Domain](#7-configure-nginx-for-custom-domain)
8. [Install SSL/TLS (HTTPS)](#8-install-ssltls-https)
9. [Automation Script](#9-automation-script)
10. [Troubleshooting](#10-troubleshooting)
11. [References](#11-references)

---

## 1. AWS Account Setup

### Step 1: Create an AWS Account

1. Open your web browser and go to: **https://aws.amazon.com**
2. Click the orange **"Create an AWS Account"** button (top right)
3. Enter your email address and choose a password
4. Enter your account name (e.g., "ICT171-Mirza")
5. Select **"Personal"** account type
6. Enter your contact information (name, phone number, address)
7. Enter your credit/debit card information (AWS requires this for verification - you won't be charged for free tier)
8. Verify your identity by phone (you'll receive an automated call with a code)
9. Select the **Basic (Free)** support plan
10. Wait 5-10 minutes for your account to fully activate

### Step 2: Set Your Region

1. After logging into AWS Console, look at the top right corner
2. Click the region dropdown and select **"Asia Pacific (Sydney) ap-southeast-2"**

**Why Sydney?** I am based in Perth, Western Australia. Sydney is the closest full AWS region to Perth, providing the lowest latency for my website.

---

## 2. EC2 Instance Launch

### Step 1: Navigate to EC2 Dashboard

1. In the AWS Management Console, type **"EC2"** in the search bar
2. Click on **"EC2"** from the search results

### Step 2: Launch an Instance

1. Click the orange **"Launch instance"** button

### Step 3: Name Your Instance

1. Under **"Name and tags"**, enter: `mirza-portfolio-server`

### Step 4: Choose an Amazon Machine Image (AMI)

1. Under **"Application and OS Images"**, click **"Quick Start"**
2. Select **"Ubuntu Server 24.04 LTS (HVM), SSD Volume Type"**

### Step 5: Choose an Instance Type

1. Select **"t3.micro"** (Free tier eligible)

### Step 6: Create a Key Pair

1. Under **"Key pair (login)"**, click **"Create new key pair"**
2. **Key pair name:** Enter `mirza-key`
3. Click **"Create key pair"**
4. Save the downloaded `.pem` file somewhere safe

### Step 7: Configure Network Settings

1. Under **"Network settings"**, click **"Edit"**
2. Keep **"Allow SSH traffic from"** as **"My IP"**
3. Click **"Add security group rule"** and select **"HTTP"** (port 80)
4. Click **"Add security group rule"** again and select **"HTTPS"** (port 443)

### Step 8: Launch the Instance

1. Click the orange **"Launch instance"** button
2. Note down your **Public IP address** (this server uses `15.135.223.103`)

---

## 3. Connect to Your Server (SSH)

### For Mac or Linux Users:

### Step 1: Open Terminal

- On Mac: Press `Cmd + Space`, type "Terminal", press Enter
- On Linux: Press `Ctrl + Alt + T`

### Step 2: Navigate to Your Key File Location

```bash
cd ~/Downloads
````
### Step 3: Set Correct Permissions for Your Key File

```bash
chmod 400 mirza-key.pem
````
### Step 4: Connect via SSH

```bash
ssh -i mirza-key.pem ubuntu@15.135.223.103
````
### Step 5: Accept the Connection

When asked "Are you sure you want to continue connecting?", type yes and press Enter.

You should now see a command prompt like: ubuntu@ip-xxx:~$

## 4. Install Nginx Web Server
---
You are still in the SSH terminal on your server.

### Step 1: Update Your Server's Package List

```bash
sudo apt update
```
### Step 2: Upgrade Existing Packages

```bash
sudo apt upgrade -y
```
### Step 3: Install Nginx

```bash
sudo apt install nginx -y
```
### Step 4: Verify Nginx is Running

```bash
sudo systemctl status nginx
```
Expected output: You should see "active (running)" in green text. Press q to exit.

### Step 5: Test That Your Website is Working

Open a web browser on your local computer
Go to: http://15.135.223.103
You should see the "Welcome to nginx!" page

## 5. Deploy Portfolio Files
---
### Step 1: Locate the Web Root Directory

The default folder for website files is:

text
/var/www/html/
### Step 2: Set Correct Permissions

```bash
sudo chown -R www-data:www-data /var/www/html/
sudo chmod -R 755 /var/www/html/
```
### Step 3: Create HTML/CSS Files Manually

Create the main HTML file:

```bash
sudo nano /var/www/html/index.html
```
Paste your HTML code, then save with Ctrl+X, then Y, then Enter.

Create CSS file (if you have one):

```bash
sudo nano /var/www/html/style.css
```
### Step 4: Set Permissions Again

```bash
sudo chown -R www-data:www-data /var/www/html/
sudo chmod -R 755 /var/www/html/
```
### Step 5: Test Nginx Configuration

```bash
sudo nginx -t
```
### Step 6: Reload Nginx

```bash
sudo systemctl reload nginx
```
### Step 7: Verify Your Portfolio is Live

Open your web browser
Go to: http://15.135.223.103
You should now see YOUR portfolio
## 6. GoDaddy Domain & DNS
---
### Step 1: Purchase a Domain

For this project, I purchased mirzajarral.work from GoDaddy.

### Step 2: Access Your DNS Settings

Log into GoDaddy
Go to My Products → Domains → DNS
### Step 3: Create A Records

Add these two records:

Type	Name	Value	TTL
A	@	15.135.223.103	600
A	www	15.135.223.103	600
### Step 4: Verify DNS Resolution

```bash
nslookup mirzajarral.work
```
Expected output: 15.135.223.103

## 7. Configure Nginx for Custom Domain
---
### Step 1: Edit Nginx Configuration

```bash
sudo nano /etc/nginx/sites-available/default
```
### Step 2: Update the server_name Line

Find this line:

text
server_name _;
Change it to:

text
server_name mirzajarral.work www.mirzajarral.work;
### Step 3: Save and Exit

Press Ctrl+X, then Y, then Enter.

### Step 4: Test and Reload

```bash
sudo nginx -t
sudo systemctl reload nginx
```
### Step 5: Test Your Domain

Go to: http://mirzajarral.work in your browser.

## 8. Install SSL/TLS (HTTPS)

### Step 1: Install Certbot

```bash
sudo apt install certbot python3-certbot-nginx -y
```
### Step 2: Obtain SSL Certificate

```bash
sudo certbot --nginx -d mirzajarral.work -d www.mirzajarral.work
```
### Step 3: Follow the Prompts

Question	Answer
Enter email address	Your email
Agree to terms?	Press A then Enter
Redirect HTTP to HTTPS?	2
### Step 4: Verify SSL is Working

```bash
sudo certbot certificates
```
Output shows:

Certificate Name: mirzajarral.work
Expiry Date: 2026-07-22 (61 days valid)
### Step 5: Verify in Browser

Go to: https://mirzajarral.work - look for the padlock icon.

## 9. Automation Script

Script Name: ssl-monitor.sh

Purpose: Monitors SSL certificate expiry, creates backups, and tests renewal.

Full Script Code

```bash
#!/bin/bash
# SSL Certificate Monitor & Backup Script
# Author: Mirza Jarral
# Student: 35017797
# Domain: mirzajarral.work

DOMAIN="mirzajarral.work"
LOG_FILE="/var/log/ssl-monitor.log"
BACKUP_DIR="/home/ubuntu/ssl-backups"
CERT_PATH="/etc/letsencrypt/live/$DOMAIN"

mkdir -p $BACKUP_DIR

log_message() {
    echo "$(date '+%Y-%m-%d %H:%M:%S') - $1" >> $LOG_FILE
}

check_expiry() {
    expiry_date=$(sudo openssl x509 -in $CERT_PATH/fullchain.pem -noout -enddate | cut -d= -f2)
    expiry_epoch=$(date -d "$expiry_date" +%s)
    current_epoch=$(date +%s)
    days_left=$(( ($expiry_epoch - $current_epoch) / 86400 ))
    log_message "Certificate expires in $days_left days"
    echo "Certificate valid for $days_left days"
}

backup_certificates() {
    timestamp=$(date '+%Y%m%d_%H%M%S')
    backup_file="$BACKUP_DIR/ssl_${DOMAIN}_${timestamp}.tar.gz"
    sudo tar -czf $backup_file -C /etc/letsencrypt live/$DOMAIN
    echo "Backup created: $backup_file"
    log_message "Backup created: $backup_file"
}

test_renewal() {
    sudo certbot renew --dry-run >> $LOG_FILE 2>&1
    if [ $? -eq 0 ]; then
        echo "Renewal test passed"
    else
        echo "Renewal test failed"
    fi
}

show_info() {
    sudo openssl x509 -in $CERT_PATH/fullchain.pem -noout -issuer -subject -dates
}

case "${1:-}" in
    backup) backup_certificates ;;
    info) show_info ;;
    test) test_renewal ;;
    *)
        check_expiry
        backup_certificates
        test_renewal
        show_info
        ;;
esac

log_message "Script completed"

``` 
### How to Install the Script


#### Create the script
```bash
sudo nano /usr/local/bin/ssl-monitor.sh
```

#### Paste the script code above, then save (Ctrl+X, Y, Enter)

#### Make it executable
```bash
sudo chmod +x /usr/local/bin/ssl-monitor.sh
```

#### Test it
```bash
sudo /usr/local/bin/ssl-monitor.sh
```
#### Set Up Automatic Daily Run

```bash
sudo crontab -e
```

Add this line:
```bash
text
0 2 * * * /usr/local/bin/ssl-monitor.sh
```
## 10. Troubleshooting

Issue	Solution
Cannot SSH	Check security group allows port 22 from your IP
Website shows "Welcome to nginx"	Files not in /var/www/html/
Domain not working	DNS propagation takes time (up to 48 hours)
HTTPS not working	Run sudo certbot renew --dry-run

### Useful Commands

```bash
sudo systemctl status nginx
sudo tail -f /var/log/nginx/error.log
nslookup mirzajarral.work
sudo certbot certificates
```
## 11. References

AWS EC2 Documentation
Nginx Official Documentation
GoDaddy DNS Help
Certbot (Let's Encrypt)

## Video Explainer

[LINK TO YOUR VIDEO WILL GO HERE]

## Server Status

Check	Status
EC2 Instance	✅ Running
Nginx	✅ Active
Domain DNS	✅ Resolving
SSL Certificate	✅ Valid (expires July 22, 2026)
Automation Script	✅ Installed
Cron Job	✅ Scheduled (daily at 2am)
Live Server: https://mirzajarral.work

GitHub Repository: https://github.com/mirzajarral/ict171-cloud-portfolio

*This documentation is complete - another ICT171 student can rebuild this server from scratch using these steps.*
