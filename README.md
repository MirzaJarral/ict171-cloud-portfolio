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

**IMPORTANT:** Make sure you're in the correct region before launching any services.

1. After logging into AWS Console, look at the top right corner
2. Next to your account name, you'll see a region name (default is "US East (N. Virginia)")
3. Click the region dropdown and select **"Asia Pacific (Sydney) ap-southeast-2"**

**Why Sydney?** I am based in Perth, Western Australia. Sydney is the closest full AWS region to Perth, providing the lowest latency for my website.

---

## 2. EC2 Instance Launch

### Step 1: Navigate to EC2 Dashboard

1. In the AWS Management Console, type **"EC2"** in the search bar at the top
2. Click on **"EC2"** from the search results
3. You are now on the EC2 Dashboard

### Step 2: Launch an Instance

1. Click the orange **"Launch instance"** button

### Step 3: Name Your Instance

1. Under **"Name and tags"**, enter: `mirza-portfolio-server`

### Step 4: Choose an Amazon Machine Image (AMI)

1. Under **"Application and OS Images"**, click **"Quick Start"**
2. Search for **"Ubuntu"** in the search bar
3. Select **"Ubuntu Server 24.04 LTS (HVM), SSD Volume Type"**
4. Ensure the architecture is **64-bit (x86)**

### Step 5: Choose an Instance Type

1. Under **"Instance type"**, click the dropdown
2. Select **"t3.micro"** (this is eligible for free tier)
3. Make sure it says "Free tier eligible" next to it

### Step 6: Create a Key Pair (IMPORTANT - Save This!)

1. Under **"Key pair (login)"**, click **"Create new key pair"**
2. **Key pair name:** Enter `mirza-key` (or any name you'll remember)
3. **Key pair type:** Select **"RSA"**
4. **Private key file format:** Select **".pem"** (for Mac/Linux) OR **".ppk"** (for Windows with PuTTY)
5. Click **"Create key pair"**
6. **YOUR KEY WILL DOWNLOAD AUTOMATICALLY** - Save this file somewhere safe (like your Desktop folder)
7. **DO NOT LOSE THIS FILE** - You cannot connect to your server without it

### Step 7: Configure Network Settings

1. Under **"Network settings"**, click **"Edit"**
2. Keep **"Allow SSH traffic from"** as **"My IP"** (this automatically adds your current IP address)
3. Click **"Add security group rule"** under the existing rules
4. For the new rule:
   - **Type:** Select **"HTTP"**
   - **Source type:** Select **"Anywhere-IPv4"** (0.0.0.0/0)
5. Click **"Add security group rule"** again
6. For the second new rule:
   - **Type:** Select **"HTTPS"**
   - **Source type:** Select **"Anywhere-IPv4"** (0.0.0.0/0)

Your security group should now have THREE rules:
| Type | Port | Source |
|------|------|--------|
| SSH | 22 | Your IP address |
| HTTP | 80 | 0.0.0.0/0 |
| HTTPS | 443 | 0.0.0.0/0 |

### Step 8: Configure Storage

1. Under **"Configure storage"**, keep the default: **20 GiB gp3**
2. This is within free tier limits

### Step 9: Launch the Instance

1. Review all your settings
2. Click the orange **"Launch instance"** button at the bottom right
3. You will see a success message: "Successfully initiated launch of instance"
4. Click the instance ID (looks like `i-xxxxxxxxxxxxx`) to go to your instances list

### Step 10: Wait for Instance to Start

1. Your instance will show **"Pending"** for about 1-2 minutes
2. Then it will change to **"Running"** 
3. Once "Running", note down your **Public IP address** (this server uses `15.135.223.103`)

---

## 3. Connect to Your Server (SSH)

### For Mac or Linux Users:

#### Step 1: Open Terminal

- On Mac: Press `Cmd + Space`, type "Terminal", press Enter
- On Linux: Press `Ctrl + Alt + T`

#### Step 2: Navigate to Your Key File Location

```bash
cd ~/Downloads
Step 3: Set Correct Permissions for Your Key File

bash
chmod 400 mirza-key.pem
Step 4: Connect via SSH

bash
ssh -i mirza-key.pem ubuntu@15.135.223.103
Step 5: Accept the Connection

When asked "Are you sure you want to continue connecting?", type yes and press Enter.

You should now see a welcome message and a command prompt like: ubuntu@ip-xxx:~$

4. Install Nginx Web Server

You are still in the SSH terminal on your server.

Step 1: Update Your Server's Package List

bash
sudo apt update
Step 2: Upgrade Existing Packages

bash
sudo apt upgrade -y
Step 3: Install Nginx

bash
sudo apt install nginx -y
Step 4: Verify Nginx is Running

bash
sudo systemctl status nginx
Expected output: You should see "active (running)" in green text. Press q to exit.

Step 5: Test That Your Website is Working

Open a web browser on your local computer
In the address bar, type: http://15.135.223.103
Press Enter
Expected result: You should see the "Welcome to nginx!" page.

5. Deploy Portfolio Files

Step 1: Locate the Web Root Directory

On Ubuntu with Nginx, the default folder for website files is:

text
/var/www/html/
Step 2: Set Correct Permissions

bash
sudo chown -R www-data:www-data /var/www/html/
sudo chmod -R 755 /var/www/html/
Step 3: Create HTML/CSS Files Manually (Method Used)

For this project, I created my portfolio files directly on the server using the nano text editor:

bash
# Create the main HTML file
sudo nano /var/www/html/index.html
Then paste your HTML code, save with Ctrl+X, then Y, then Enter.

bash
# Create CSS file (if you have one)
sudo nano /var/www/html/style.css
Step 4: Set Permissions Again After Creating Files

bash
sudo chown -R www-data:www-data /var/www/html/
sudo chmod -R 755 /var/www/html/
Step 5: Test Nginx Configuration

bash
sudo nginx -t
Expected output:

text
nginx: configuration file /etc/nginx/nginx.conf test is successful
Step 6: Reload Nginx to Apply Changes

bash
sudo systemctl reload nginx
Step 7: Verify Your Portfolio is Live

Open your web browser
Go to: http://15.135.223.103
You should now see YOUR portfolio, not the default Nginx page
6. GoDaddy Domain & DNS

Step 1: Purchase a Domain

For this project, I purchased the domain mirzajarral.work from GoDaddy.

Step 2: Access Your DNS Settings

Log into your GoDaddy account
Click on your name/avatar in the top right
Select "My Products" from the dropdown
Under "Domains", find your domain (mirzajarral.work) and click "DNS"
Step 3: Delete Existing A Records

Scroll down to the "Records" section
Look for any existing "A" records (Type = A)
Click the 3 dots (...) next to each one and select "Delete"
Step 4: Create New A Records

Click "Add new record" and fill in:

First record (root domain):

Field	Value
Type	A
Name	@
Value	15.135.223.103
TTL	600
Click "Save"

Second record (www subdomain):

Click "Add new record" again:

Field	Value
Type	A
Name	www
Value	15.135.223.103
TTL	600
Click "Save"

Step 5: Verify DNS Resolution

bash
nslookup mirzajarral.work
Expected output: 15.135.223.103

7. Configure Nginx for Custom Domain

Step 1: Edit the Nginx Configuration File

bash
sudo nano /etc/nginx/sites-available/default
Step 2: Find the server_name Line

Look for a line that says:

text
server_name _;
Step 3: Replace with Your Domain

Change that line to:

text
server_name mirzajarral.work www.mirzajarral.work;
Step 4: Save and Exit

Press Ctrl + X to exit
Press Y to confirm save
Press Enter to keep the same filename
Step 5: Test Configuration

bash
sudo nginx -t
Step 6: Reload Nginx

bash
sudo systemctl reload nginx
Step 7: Test Your Domain

Open your web browser
Go to: http://mirzajarral.work
You should see your portfolio
8. Install SSL/TLS (HTTPS)

Step 1: Install Certbot

bash
sudo apt install certbot python3-certbot-nginx -y
Step 2: Obtain and Install SSL Certificate

bash
sudo certbot --nginx -d mirzajarral.work -d www.mirzajarral.work
Step 3: Follow the Prompts

Question	My Answer
Enter email address	[my email address]
Agree to terms?	Press A then Enter
Share email with EFF?	N
Redirect HTTP to HTTPS?	2 (for "Redirect")
Step 4: Verify SSL is Working

bash
sudo certbot certificates
Output from my server:

text
Found the following certs:
  Certificate Name: mirzajarral.work
    Serial Number: 615e72a95858d1110fd49a2b5edbb5f766c
    Key Type: ECDSA
    Domains: mirzajarral.work www.mirzajarral.work
    Expiry Date: 2026-07-22 02:36:36+00:00 (VALID: 61 days)
    Certificate Path: /etc/letsencrypt/live/mirzajarral.work/fullchain.pem
    Private Key Path: /etc/letsencrypt/live/mirzajarral.work/privkey.pem
Step 5: Test Auto-Renewal

bash
sudo certbot renew --dry-run
Step 6: Verify HTTPS in Browser

Open your web browser
Go to: https://mirzajarral.work
Look for the padlock icon next to the URL
9. Automation Script

Script Name: ssl-monitor.sh

Purpose: Automatically monitors SSL certificate expiry, creates backups, and tests renewal functionality.

Location: /usr/local/bin/ssl-monitor.sh

What the Script Does

Function	Description
Check Expiry	Calculates days until certificate expires
Alert	Warnings at 30 days, critical at 7 days
Backup	Creates timestamped .tar.gz backups of certificates
Rotation	Keeps only last 10 backups
Test Renewal	Runs certbot renew --dry-run
Logging	All activity logged to /var/log/ssl-monitor.log
Full Script Code

bash
#!/bin/bash
# ============================================
# SSL Certificate Monitor & Backup Script
# Author: Mirza Jarral
# Student: 35017797
# Purpose: Monitor SSL expiry, backup certificates, and log status
# For: ICT171 Cloud Server Project
# Domain: mirzajarral.work
# ============================================

# Configuration
DOMAIN="mirzajarral.work"
LOG_FILE="/var/log/ssl-monitor.log"
BACKUP_DIR="/home/ubuntu/ssl-backups"
CERT_PATH="/etc/letsencrypt/live/$DOMAIN"

# Colors for output
RED='\033[0;31m'
GREEN='\033[0;32m'
YELLOW='\033[1;33m'
NC='\033[0m'

# Create backup directory
mkdir -p $BACKUP_DIR

# Function to log messages
log_message() {
    echo "$(date '+%Y-%m-%d %H:%M:%S') - $1" >> $LOG_FILE
}

# Function to check certificate expiry
check_expiry() {
    if [ ! -f "$CERT_PATH/fullchain.pem" ]; then
        log_message "ERROR: Certificate not found at $CERT_PATH"
        echo -e "${RED}ERROR: Certificate not found${NC}"
        exit 1
    fi
    
    expiry_date=$(sudo openssl x509 -in $CERT_PATH/fullchain.pem -noout -enddate | cut -d= -f2)
    expiry_epoch=$(date -d "$expiry_date" +%s)
    current_epoch=$(date +%s)
    days_left=$(( ($expiry_epoch - $current_epoch) / 86400 ))
    
    log_message "Certificate for $DOMAIN expires in $days_left days"
    
    if [ $days_left -lt 30 ] && [ $days_left -ge 7 ]; then
        log_message "WARNING: Certificate expires in $days_left days!"
        echo -e "${YELLOW}WARNING: Certificate expires in $days_left days${NC}"
    elif [ $days_left -lt 7 ]; then
        log_message "CRITICAL: Certificate expires in $days_left days!"
        echo -e "${RED}CRITICAL: Certificate expires in $days_left days!${NC}"
    else
        echo -e "${GREEN}Certificate valid for $days_left days${NC}"
    fi
}

# Function to backup certificates
backup_certificates() {
    timestamp=$(date '+%Y%m%d_%H%M%S')
    backup_file="$BACKUP_DIR/ssl_${DOMAIN}_${timestamp}.tar.gz"
    
    sudo tar -czf $backup_file -C /etc/letsencrypt live/$DOMAIN
    
    if [ -f $backup_file ]; then
        log_message "Backup created: $backup_file"
        echo -e "${GREEN}Backup created: $backup_file${NC}"
        
        cd $BACKUP_DIR
        ls -t ssl_*.tar.gz 2>/dev/null | tail -n +11 | xargs -r rm
        log_message "Cleaned old backups (kept last 10)"
    else
        log_message "ERROR: Backup failed"
        echo -e "${RED}Backup failed!${NC}"
    fi
}

# Function to test renewal
test_renewal() {
    log_message "Testing certificate renewal (dry run)"
    sudo certbot renew --dry-run >> $LOG_FILE 2>&1
    
    if [ $? -eq 0 ]; then
        log_message "Renewal test successful"
        echo -e "${GREEN}Renewal test passed${NC}"
    else
        log_message "Renewal test FAILED"
        echo -e "${RED}Renewal test failed - check logs${NC}"
    fi
}

# Function to show certificate info
show_info() {
    echo "========================================="
    echo "SSL Certificate Information for $DOMAIN"
    echo "========================================="
    sudo openssl x509 -in $CERT_PATH/fullchain.pem -noout -issuer -subject -dates
    echo "========================================="
}

# Main execution
case "${1:-}" in
    backup)
        backup_certificates
        ;;
    info)
        show_info
        ;;
    test)
        test_renewal
        ;;
    *)
        echo "=== SSL Monitor Script ==="
        echo "Running on: $(date)"
        echo ""
        check_expiry
        echo ""
        backup_certificates
        echo ""
        test_renewal
        echo ""
        show_info
        ;;
esac

log_message "Script completed successfully"
How to Install the Script

bash
# Create the script file
sudo nano /usr/local/bin/ssl-monitor.sh

# Paste the script code above
# Save and exit (Ctrl+X, then Y, then Enter)

# Make the script executable
sudo chmod +x /usr/local/bin/ssl-monitor.sh

# Create log file
sudo touch /var/log/ssl-monitor.log
sudo chmod 666 /var/log/ssl-monitor.log
How to Run the Script

bash
# Run full check
sudo /usr/local/bin/ssl-monitor.sh

# Run only backup
sudo /usr/local/bin/ssl-monitor.sh backup

# Show certificate info only
sudo /usr/local/bin/ssl-monitor.sh info

# Test renewal only
sudo /usr/local/bin/ssl-monitor.sh test
Set Up Automatic Daily Run (Cron)

bash
sudo crontab -e
Add this line:

text
0 2 * * * /usr/local/bin/ssl-monitor.sh
View Logs

bash
# Check script logs
cat /var/log/ssl-monitor.log

# Follow logs in real-time
sudo tail -f /var/log/ssl-monitor.log
10. Troubleshooting

Common Issues and Solutions

Issue	Solution
Cannot SSH	Check security group allows port 22 from your IP
Website shows "Welcome to nginx"	Files not in /var/www/html/ or wrong permissions
Domain not working	DNS propagation takes time (up to 48 hours)
HTTPS not working	Run sudo certbot renew --dry-run
403 Forbidden	Run sudo chmod -R 755 /var/www/html/
Useful Diagnostic Commands

bash
# Check Nginx status
sudo systemctl status nginx

# Check Nginx error logs
sudo tail -f /var/log/nginx/error.log

# Check DNS resolution
nslookup mirzajarral.work

# Check SSL certificate
sudo certbot certificates

# Test Nginx config
sudo nginx -t

# Reload Nginx
sudo systemctl reload nginx
11. References

Documentation References

AWS EC2 Documentation
Nginx Official Documentation
GoDaddy DNS Help
Certbot (Let's Encrypt)
Ubuntu 24.04 Server Guide


Video Explainer

[LINK TO YOUR VIDEO WILL GO HERE]

Video Contents

Timestamp	Section
0:00 - 0:30	Introduction (name, student number, domain)
0:30 - 1:30	EC2 Setup in AWS Console
1:30 - 2:30	SSH connection and Nginx installation
2:30 - 3:30	Portfolio deployment (HTML/CSS files)
3:30 - 4:30	GoDaddy DNS configuration
4:30 - 5:30	SSL/TLS installation (Let's Encrypt)
5:30 - 6:30	Automation script demonstration
6:30 - 7:00	Conclusion and GitHub repo walkthrough
Project Timeline

Date	Task
May 15, 2026	EC2 instance launched
May 15, 2026	Nginx installed
May 16, 2026	Portfolio HTML/CSS created
May 17, 2026	Domain (mirzajarral.work) purchased on GoDaddy
May 18, 2026	DNS records configured
May 19, 2026	SSL/TLS installed via Let's Encrypt
May 20, 2026	Automation script created
May 21, 2026	Documentation completed
Server Status

Check	Status
EC2 Instance	✅ Running (t3.micro, Sydney region)
Nginx	✅ Active
Domain DNS	✅ Resolving (mirzajarral.work → 15.135.223.103)
SSL Certificate	✅ Valid (expires July 22, 2026)
Automation Script	✅ Installed
Cron Job	✅ Scheduled (daily at 2am)
Live Server: https://mirzajarral.work

GitHub Repository: https://github.com/mirzajarral/ict171-cloud-portfolio

*This documentation is complete - another ICT171 student can rebuild this exact server configuration from scratch using these steps.*
