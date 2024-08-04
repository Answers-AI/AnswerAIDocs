---
description: Learn how to deploy AnswerAI on Digital Ocean
---

# Deploying AnswerAI on Digital Ocean

## Overview

This guide will walk you through the process of deploying AnswerAI on a Digital Ocean Droplet, including setting up a reverse proxy with Nginx and configuring SSL for secure connections.

## Create Droplet

1. From the Digital Ocean dashboard, click **Droplets** from the dropdown menu.

<!-- TODO: Add screenshot of Digital Ocean dashboard dropdown -->

2. Select your preferred data region and choose a Basic $6/mo Droplet type.

<!-- TODO: Add screenshot of Droplet creation options -->

3. For authentication, select Password (or SSH key if you prefer).

<!-- TODO: Add screenshot of authentication method selection -->

4. Complete the creation process. After a few moments, your Droplet should be ready.

<!-- TODO: Add screenshot of successful Droplet creation -->

## Connect to your Droplet

- For Windows users: Follow the [PuTTY connection guide](https://docs.digitalocean.com/products/droplets/how-to/connect-with-ssh/putty/).
- For Mac/Linux users: Use the [OpenSSH connection guide](https://docs.digitalocean.com/products/droplets/how-to/connect-with-ssh/openssh/).

## Install Docker

1. Download the Docker installation script:

   ```bash
   curl -fsSL https://get.docker.com -o get-docker.sh
   ```

2. Run the installation script:

   ```bash
   sudo sh get-docker.sh
   ```

3. Install Docker Compose:

   ```bash
   sudo curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
   ```

4. Set the correct permissions:

   ```bash
   sudo chmod +x /usr/local/bin/docker-compose
   ```

## Set Up AnswerAI

1. Clone the AnswerAI repository:

   ```bash
   git clone https://github.com/AnswerAI/AnswerAI.git
   ```

2. Navigate to the docker folder:

   ```bash
   cd AnswerAI && cd docker
   ```

3. Create and edit a `.env` file:

   ```bash
   nano .env
   ```

4. Add the following environment variables:

   ```
   PORT=3000
   DATABASE_PATH=/root/.answerai
   APIKEY_PATH=/root/.answerai
   SECRETKEY_PATH=/root/.answerai
   LOG_PATH=/root/.answerai/logs
   BLOB_STORAGE_PATH=/root/.answerai/storage
   ```

   Optionally, add `ANSWERAI_USERNAME` and `ANSWERAI_PASSWORD` for app-level authorization.

5. Save and exit the editor (Ctrl + X, then Y).

6. Start the Docker container:

   ```bash
   docker compose up -d
   ```

7. Access the app at `http://YOUR_DROPLET_IP:3000`.

## Set Up Nginx Reverse Proxy

1. Install Nginx:

   ```bash
   sudo apt update
   sudo apt install nginx
   ```

2. Allow Nginx through the firewall:

   ```bash
   sudo ufw allow 'Nginx HTTP'
   ```

3. Create a new Nginx configuration file:

   ```bash
   sudo nano /etc/nginx/sites-available/your_domain
   ```

4. Add the following configuration (replace `your_domain` with your actual domain):

   ```nginx
   server {
       listen 80;
       listen [::]:80;
       server_name your_domain;
       location / {
           proxy_pass http://localhost:3000;
           proxy_http_version 1.1;
           proxy_set_header Host $host;
           proxy_set_header Upgrade $http_upgrade;
           proxy_set_header Connection 'upgrade';
           proxy_cache_bypass $http_upgrade;
       }
   }
   ```

5. Enable the configuration:

   ```bash
   sudo ln -s /etc/nginx/sites-available/your_domain /etc/nginx/sites-enabled/
   ```

6. Test and restart Nginx:

   ```bash
   sudo nginx -t
   sudo systemctl restart nginx
   ```

7. Update your domain's DNS A record to point to your Droplet's IP address.

## Set Up SSL with Certbot

1. Install Certbot dependencies:

   ```bash
   apt install python3.10-venv
   sudo python3 -m venv /opt/certbot/
   sudo /opt/certbot/bin/pip install --upgrade pip
   ```

2. Install Certbot:

   ```bash
   sudo /opt/certbot/bin/pip install certbot certbot-nginx
   ```

3. Create a symlink for Certbot:

   ```bash
   sudo ln -s /opt/certbot/bin/certbot /usr/bin/certbot
   ```

4. Obtain and install SSL certificate:

   ```bash
   sudo certbot --nginx
   ```

5. Set up automatic renewal:

   ```bash
   echo "0 0,12 * * * root /opt/certbot/bin/python -c 'import random; import time; time.sleep(random.random() * 3600)' && sudo certbot renew -q" | sudo tee -a /etc/crontab > /dev/null
   ```

## Updating AnswerAI

To update AnswerAI to the latest version:

1. Navigate to the AnswerAI directory:

   ```bash
   cd AnswerAI/docker
   ```

2. Stop and remove the current Docker container:

   ```bash
   sudo docker compose stop
   sudo docker compose rm
   ```

3. Pull the latest AnswerAI image:

   ```bash
   docker pull answerai/answerai
   ```

4. Start the updated container:

   ```bash
   docker compose up -d
   ```

## Troubleshooting

- If you can't access the app, check that the Docker container is running:

  ```bash
  docker ps
  ```

- For Nginx issues, check the Nginx error logs:

  ```bash
  sudo tail -f /var/log/nginx/error.log
  ```

- If SSL is not working, ensure your domain is correctly pointing to your Droplet's IP and that Certbot completed successfully.

Remember to keep your system and AnswerAI updated regularly for the best performance and security.

<!-- TODO: Add a video tutorial on deploying to Digital Ocean -->
