---
theme: post
title: "Enable HTTPS on Azure Web App with Let's Encrypt's free SSL certificate"
categories: [azure]
tags: [azure, https, lets encrypt, ssl]
---

# [](#intro)Background
By default, Azure Web Apps have HTTPS enabled on their default address. However, when you use custom domain, you are only able to connect through HTTP, which is not secure. 
You can use Let's Encrypt's free SSL certificate to enable HTTPS for your custom domain. The process I present is fully manual, as for now I only need cert for a limited period of time. Certificates expire after 3 months and must be renewed.

# [](#prerequisites)Prerequisites
You will need:
* Azure Web App with Basic plan or higher (Custom domains + SSL Support)
* Connected custom domain
* Ability to add DNS TXT record to your domain
* System with Linux and sudo access (you can use live USB key or something similar)

# [](#instructions)Instructions
First, install all necessary software. You may have some of it preinstalled with your system.
```bash
sudo apt update
sudo apt-get install letsencrypt
sudo apt-get install python-pip
sudo pip install --upgrade pip
sudo pip install certbot
sudo apt-get install openssl
```

Generate certificate. During this step you will be required to add a text record to your domain's DNS configuration. Take into consideration the change has to propagate, so this step may take up to 15 minutes. As I understand, the record can be later removed.
```bash
sudo certbot certonly --manual --preferred-challenge dns -d <your-domain>
```

Package files for Azure's liking (Azure requires `.pfx` files). Use securely generated password and save it for later. (You can for example use your password manager for both.)
```bash
sudo openssl pkcs12 -export -out path/to/<cert-name>.pfx -inkey <path from previous step>/privkey.pem -in <path from previous step>/fullchain.pem
```
_Note: Paths to the `.pem` files are in the output of previous command. They should be similar to `/etc/letsencrypt/live/<your-domain>`_

Upload `path/to/<cert-name>.pfx` file to Azure's SSL panel. Use password you entered in the previous step. Add a binding to your domain. Wait a moment and connect to your site using `https://` protocol.
