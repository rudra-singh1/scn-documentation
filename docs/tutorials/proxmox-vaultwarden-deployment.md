---
title: Proxmox Deployment Guide - Vaultwarden
---

### Deployed by: Esther Jang, Paul Phillion, Rudra Singh

---

## Section 1: What is Proxmox?

- **Proxmox VE (Virtual Environment)** is an open-source server management platform designed to deploy and manage virtual machines (VMs) and containers.
- It integrates KVM hypervisor and LXC containers, enabling users to manage virtual infrastructure through a web-based interface.

## Section 2: Access Requirements for Proxmox VE at Seattle Community Network

- To submit a request for a SCN self-hosted Proxmox VM on our private cloud, please fill out [this form](https://docs.google.com/forms/d/e/1FAIpQLSf6NYZHTfxi_hcQM1DWJ8mhgJDl-iiqRL4yTQG_x1az6XEfEQ/viewform?usp=sf_link).
- If you are working on a project for SCN and need access to the Proxmox VE, please continue reading.
- Next, you will need access to the OpenVPN. Proxmox VE can only be accessed on the VPN. Specific details can be obtained from the SCN discord. Once connected to the VPN, a specific IP will be provided for you to access the Proxmox VE, where you can input your credentials.

## Section 3: Setting up your VM

- ### Install SSH
- ### Install Keys
- ### Test SSH CLI

## Section 4: SSH Troubleshooting

- Please let the SCN discord know if you have trouble SSH'ing in. A useful command during setup was `sudo ufw status`. If it is active, use `ufw disable`. The expected status should be inactive. If that still doesn’t work, try restarting the VM from the Proxmox VE.

## Section 5: Beginning Deployment: Vaultwarden

```bash
docker pull vaultwarden/server:latest
docker run -d --name vaultwarden -v /vw-data/:/data/ --restart unless-stopped -p 80:80 vaultwarden/server:latest
```

## Section 6: Docker Compose

- Create a Docker Compose file:

```yaml
version: "3"

services:
  vaultwarden:
    container_name: vaultwarden
    hostname: vaultwarden9
    ports:
      - "127.0.0.1:8080:80"
    environment:
      - LOG_FILE=/log/access.log
      - LOG_LEVEL=info
      - EXTENDED_LOGGING=true
    image: vaultwarden/server:latest
    restart: unless-stopped
    volumes:
      - /opt/vw-data:/data
      - /var/log/vw:/log
```

- To start and run your container application in detached mode, use:
  
```bash
docker-compose up -d
docker-compose start
docker-compose stop
docker-compose restart
```

## Section 7: Azure DNS

- For this deployment, we're utilizing a public IP address. This address must be configured within Azure DNS to point to our chosen domain name.
- Navigate to Home - Microsoft Azure with your account.
- Open “DNS zones”. If you were using a private IP, you would use “Private DNS zones”.
- We will be creating a subdomain under seattlecommunitynetwork.org.
- Click on that, and under DNS management, click recordsets, and click add.
- Insert the beginning of the subdomain name, which in our case is “vaultwarden”.
- Below, under IP address, insert your IP, and create your new subdomain.

## Section 8: Enabling Nginx

```bash
sudo apt update
sudo apt install nginx
sudo systemctl enable nginx
sudo systemctl start nginx
```

- Next, go to `/etc/nginx/sites-enabled/default`. Delete everything inside `default` and paste this:

```nginx
# The `upstream` directives ensure that you have a http/1.1 connection
# This enables the keepalive option and better performance
#
# Define the server IP and ports here.
upstream vaultwarden-default {
  zone vaultwarden-default 64k;
  server 127.0.0.1:8080;
  keepalive 2;
}

# Needed to support websocket connections
# See: https://nginx.org/en/docs/http/websocket.html
# Instead of "close" as stated in the above link we send an empty value.
# Else all keepalive connections will not work.
map $http_upgrade $connection_upgrade {
    default upgrade;
    ''      "";
}

# Redirect HTTP to HTTPS
server {
    listen 80;
    listen [::]:80;
    server_name vaultwarden.seattlecommunitynetwork.org;

    if ($host = vaultwarden.seattlecommunitynetwork.org) {
        return 301 https://$host$request_uri;
    }
    return 404;
}

server {
    # For older versions of nginx appened http2 to the listen line after ssl and remove `http2 on`
    listen 443 ssl;
    listen [::]:443 ssl;
   # http2 on;
    server_name vaultwarden.seattlecommunitynetwork.org;

    # Specify SSL Config when needed
    ssl_certificate /etc/path...;
    ssl_certificate_key /etc/path...;
    ssl_trusted_certificate /etc/path...;

    client_max_body_size 525M;

    location / {
      proxy_http_version 1.1;
      proxy_set_header Upgrade $http_upgrade;
      proxy_set_header Connection $connection_upgrade;

      proxy_set_header Host $host;
      proxy_set_header X-Real-IP $remote_addr;
      proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
      proxy_set_header X-Forwarded-Proto $scheme;

      proxy_pass http://vaultwarden-default;
    }

    # Optionally add extra authentication besides the ADMIN_TOKEN
    # Remove the comments below `#` and create the htpasswd_file to have it active
    #
    #location /admin {
    # See: https://docs.nginx.com/nginx/admin-guide/security-controls/configuring-http-basic-authentication/
      #auth_basic "Administrator's Area";
      #3auth_basic_user_file /path/to/htpasswd_file;

      #proxy_http_version 1.1;
      #proxy_set_header Upgrade $http_upgrade;
      #proxy_set_header Connection $connection_upgrade;

      #proxy_set_header Host $host;
      #proxy_set_header X-Real-IP $remote_addr;
      #proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;

      #proxy_set_header X-Forwarded-Proto $scheme;
      #proxy_pass http://vaultwarden-default;
    }
}
```

## Section 9: Obtain SSL Certificates

```bash
sudo apt install certbot python3-certbot-nginx
sudo certbot --nginx -d vaultwarden.seattlecommunitynetwork.org
```

- Certbot will modify your Nginx configuration to handle HTTPS and redirect from HTTP to HTTPS.
- Ensure that you copy the file paths provided at the conclusion of the certbot process into the default nginx configuration file, replacing the corresponding comments with these paths.
## Section 10: Ensure Everything is Running

```bash
sudo systemctl status nginx
```

## Section 11: Additional Features

- Enabling admin panel:

Go back to `/etc/nginx/sites-enabled/default` and uncomment the admin section at the bottom. Follow directions at [Nginx Admin Guide](https://docs.nginx.com/nginx/admin-guide/security-controls/configuring-http-basic-authentication/) to encrypt your admin password as a `.env` file (preferably using argon CLI).

Once done, make sure you create a `.env` file in the directory where the compose file is with `VAULTWARDEN_ADMIN_TOKEN=[insert your hashed admin token]`.

Then in your compose, add these two lines under environment:

```yaml
- ADMIN_TOKEN=${VAULTWARDEN_ADMIN_TOKEN}
- DOMAIN=https://vaultwarden.seattlecommunitynetwork.org
```

Restart the container and try logging into <https://vaultwarden.seattlecommunitynetwork.org/admin>.

Once logged in, SMTP and 2FA enabling settings can be configured on the home page.

## Section 12: Data Backup

- Enabling backups for Vaultwarden is a simple process. Please review the attached backup bash script, which facilitates the transfer of Vaultwarden data to another virtual machine. Additionally, there is a cleanup bash script designed to retain only the most recent file in the other VM, deleting all others. Feel free to modify the scripts as necessary to suit your specific requirements.






Backup script:
```bash
#!/bin/bash

docker-compose down
datestamp=$(date +%m-%d-%Y)
zip -9 -r /home/scn/backups/${datestamp}.zip /opt/vw-data*
scp -i ~/.ssh-comm/id_rsa /home/scn/backups/${datestamp}.zip azureuser@[IP address]:~/backups/
docker-compose up -d
```

Cleanup Script:
```bash
#!/bin/bash

# Define the directory containing backup files
backup_dir=~/backups

# Go to the backup directory
cd "$backup_dir" || exit

# Find and delete older backup files (excluding the latest day)
find . -type f -name '*.zip' ! -mtime -1 -exec rm {} +

# Exit
exit 0
```
## Section 13: Using the Backup
To restore a data backup to the original virtual machine, simply unzip the file and delete the existing contents of `/opt/vw-data`. Then, transfer the contents of your zip file into this directory. Perform a quick restart of the container, and you will have successfully restored the version of the backup you selected.
