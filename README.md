# Set up typesense with digital ocean ubuntu server

Before starting, now would be a good idea to set the A record for your domain to point to your digital ocean server. You can do this by logging into your domain provider and adding an A record for your domain. The IP address should be the IP address of your digital ocean server.

## Step 1: Download & Install Typesense

Once you have logged into your digital ocean ubuntu server, run the following commands to download, install and run typesense
```shell
# Download
curl -O https://dl.typesense.org/releases/0.24.0/typesense-server-0.24.0-amd64.deb

# Install
sudo apt install ./typesense-server-0.24.0-amd64.deb

# Run
sudo systemctl status typesense-server.service
```

press ```ctrl + c``` to exit.

Now we should have typesense installed and typesense should always run on the server, even after reboots.

We can run the below command to see if it is all working properly (optional)
```shell
# Test (optional)
curl http://localhost:8108/health
```

it should return ```{"ok":true}``` if working correctly.

## Step 2: Add SSL Certificate

Once you have typesense installed, we can add an SSL certificate to it.

```shell
# Ensure snapd is up to date
sudo snap install core; sudo snap refresh core

# Install Certbot
sudo snap install --classic certbot

# Prepare the certbot command
sudo ln -s /snap/bin/certbot /usr/bin/certbot

# Start Certificate Process
sudo certbot certonly --standalone
```

You will be asked to enter your email address and agree to the terms of service. Then you will be asked to enter your domain name. Once you have done that, your certificate will be generated.

Now we need to copy the path the certificate and key, so we can use it in the Typesense configuration file.

```shell
Certificate is saved at: /etc/letsencrypt/live/<yourdomain.com>/fullchain.pem
Key is saved at:         /etc/letsencrypt/live/<yourdomain.com>/privkey.pem
```

## Step 3: Configure Typesense

the following command will open the typesense configuration file in nano, so we can edit it.

```
nano /etc/typesense/typesense-server.ini
```

we will need to do the following things to this file:
1. Change the api-port to 443
2. Add the path to the ssl-certificate
3. Add the path to the ssl-certificate-key

the configuration file should look like this:

```
; Typesense Configuration

[server]

api-address = 0.0.0.0
api-port = 443
data-dir = /var/lib/typesense
api-key = <Your API Key>
log-dir = /var/log/typesense
ssl-certificate = /etc/letsencrypt/live/<yourdomain.com>/fullchain.pem
ssl-certificate-key = /etc/letsencrypt/live/<yourdomain.com>/privkey.pem
```

Congratulations! You have now set up typesense with a digital ocean ubuntu server and added an SSL certificate to it.

You can test to see if it is working by going to this codesandbox and entering the server details - https://codesandbox.io/s/typesense-server-test-r7lt9s?file=/src/Typesense/createCollection.js