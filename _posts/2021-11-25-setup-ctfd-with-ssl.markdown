---
title: How To Setup Your CTFd Platform With HTTPS And SSL
published: true
description: Setup your ctf server from the ground up and configure a ssl for having https.
tags: ctf, ctfd, https
cover_image: https://res.cloudinary.com/dmrgfufa4/image/upload/v1598668440/articles/ics%20from%20google%20calendar/Capture_d_e%CC%81cran_2020-08-28_a%CC%80_21.33.15.png

layout: post
date: 2021-11-25 18:34:51 -0500
categories: ctf, ctfd, https
---
If you want to organize and host a CTF event, one of the best and easiest options available for managing this is [CTFd](https://github.com/CTFd/CTFd).

This open-source platform lets you manage users, challenges, and their categories in a very easy way, so the only thing we need to do is to clone the spin up a server, clone the repo, run the docker-compose, and set up the TLS certificate.

General requirements:
- [ ] VPS/server/droplet (I'm using [digital ocean](https://www.digitalocean.com/) but any server works)
- [ ] A domain (I'm using a domain bought and configured in Google domains, but if you can modify the A registries, you're good to go)

## 1. Spin up your server

For this, I'm going to use [Digital Ocean](https://www.digitalocean.com/), but this can be done in practically any cloud provider.
I created a droplet (VPS) with the following features:

![Sample digital ocean server](https://res.cloudinary.com/dmrgfufa4/image/upload/v1637902940/articles/ctfd%20with%20https/1.1.png)
![Sample digital ocean server](https://res.cloudinary.com/dmrgfufa4/image/upload/v1637902940/articles/ctfd%20with%20https/1.2.png)
![Sample digital ocean server](https://res.cloudinary.com/dmrgfufa4/image/upload/v1637902942/articles/ctfd%20with%20https/1.3.png)
![Sample digital ocean server](https://res.cloudinary.com/dmrgfufa4/image/upload/v1637902941/articles/ctfd%20with%20https/1.4.png)

After connection to the new server (`ssh root@YOUR_IP`), the requirements are the following:

- [ ] git
- [ ] docker
- [ ] docker-compose
- [ ] some editor (I will use vim)

To install this, I first updated the packages with:

```shell
sudo apt update -y && sudo apt upgrade -y
```

### 1.1 [OPTIONAL] Installing tmux and nice terminal

I like to have tmux and some preconfigured vim, so I use [this script](https://github.com/roeeyn/dotfiles/blob/master/script/bootstrap_remote_server.sh) which configures this for me.

You can have it by executing:

```shell
# Install zsh
sudo apt install zsh

# Execute bootstrap script
curl -L https://raw.githubusercontent.com/roeeyn/dotfiles/master/script/bootstrap_remote_server.sh | sh
```

After this, you need to log out and log in again to start with zsh.


### 1.2 Install Docker

Then executed the official commands from [Docker documentation](https://docs.docker.com/engine/install/ubuntu/#install-using-the-repository):

```shell
# Docker requirements
sudo apt install ca-certificates curl gnupg lsb-release

# Docker GPG key
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg

# Docker setup of the stable repository
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/nul

# Docker installation
sudo apt update -y
sudo apt install docker-ce docker-ce-cli containerd.io
```

### 1.3 Install Docker Compose

Following the official docker-compose [documentation](https://docs.docker.com/compose/install/), we can install this with:

```shell
# Get the docker-compose binary
sudo curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose

# Make it executable
sudo chmod +x /usr/local/bin/docker-compose
```

## 2. Clone the CTFd repository

We need to clone the CTFd repository, which contains all the files that we need to set up our platform.

```shell
git clone https://github.com/CTFd/CTFd.git
```

## 3. Initialize the Platform

After cloning the repo, we can access the CTFd directory with `cd CTFd` and there we can see all the platform files.

To initialize the platform we can execute:

```shell
# If you would like to see the logs (recommended)
docker-compose up

# If you just trust that everything is going well (no logs)
docker-compose up -d
```

This will pull all the docker images and create all the needed containers. After finishing the startup process, the landing page of **the platform should be available in the public ip**.

![Proof of working IP](https://res.cloudinary.com/dmrgfufa4/image/upload/v1637902940/articles/ctfd%20with%20https/3.1.png)

Here you will need to set up the information about your CTF.

![Proof of working IP](https://res.cloudinary.com/dmrgfufa4/image/upload/v1637902942/articles/ctfd%20with%20https/3.2.png)

## 4. Add the IP to your DNS

When you already have your platform up and running, you need to register it to your domain. For this, I'm using [Google domains](https://domains.google.com/), and you need to add an A register to the domain. In my case, it looks like the following:

![Google Domains demo](https://res.cloudinary.com/dmrgfufa4/image/upload/v1637902941/articles/ctfd%20with%20https/4.1.png)

If everything went well, at this point you should already be having the platform at your domain (with NO certificate or https yet).

![Proof of working domain](https://res.cloudinary.com/dmrgfufa4/image/upload/v1637902941/articles/ctfd%20with%20https/4.2.png)

## 5. Add a Certificate with Certbot (Let's Encrypt)

For adding a certificate we need to execute some more steps.

### 5.1 Generate a certificate

Before creating the certbot certificate, make sure to **stop all the containers from the previous steps**.
If you ran your containers from previous steps in *normal* mode (no `-d` after `docker-compose`), you can only press Ctrl-C in the running process.
If you ran your containers from previous steps in detached moded (with the `-d` flag after `docker-compose`) you should execute `docker-compose down` inside the `CTFd` directory.

The easiest way to generate a certificate is with the certbot docker image. Following the [official documentation](https://eff-certbot.readthedocs.io/en/stable/install.html#running-with-docker), this can be done with the following:

```shell
docker run -it --rm --name certbot \
          -v "/etc/letsencrypt:/etc/letsencrypt" \
          -v "/var/lib/letsencrypt:/var/lib/letsencrypt" \
          certbot/certbot certonly
```

We should select the `Spin up a temporary server` option, enter your email, accept the agreements, and enter the domain we're planning to use.
This is similar to what you should see:

```
➜  ~ docker run -it --rm --name certbot \
            -v "/etc/letsencrypt:/etc/letsencrypt" \
            -v "/var/lib/letsencrypt:/var/lib/letsencrypt" \
            -p 80:80 -p 443:443 certbot/certbot certonly

Saving debug log to /var/log/letsencrypt/letsencrypt.log

How would you like to authenticate with the ACME CA?
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
1: Spin up a temporary webserver (standalone)
2: Place files in webroot directory (webroot)
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
Select the appropriate number [1-2] then [enter] (press 'c' to cancel): 1
Enter email address (used for urgent renewal and security notices)
 (Enter 'c' to cancel): rodrigo.medina.neri@gmail.com

- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
Please read the Terms of Service at
https://letsencrypt.org/documents/LE-SA-v1.2-November-15-2017.pdf. You must
agree in order to register with the ACME server. Do you agree?
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
(Y)es/(N)o: Y

- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
Would you be willing, once your first certificate is successfully issued, to
share your email address with the Electronic Frontier Foundation, a founding
partner of the Let's Encrypt project and the non-profit organization that
develops Certbot? We'd like to send you email about our work encrypting the web,
EFF news, campaigns, and ways to support digital freedom.
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
(Y)es/(N)o: Y
Account registered.
Please enter the domain name(s) you would like on your certificate (comma and/or
space separated) (Enter 'c' to cancel): examplectf.manguitoblue.io
Requesting a certificate for examplectf.manguitoblue.io

Successfully received certificate.
Certificate is saved at: /etc/letsencrypt/live/examplectf.manguitoblue.io/fullchain.pem
Key is saved at:         /etc/letsencrypt/live/examplectf.manguitoblue.io/privkey.pem
This certificate expires on 2022-02-24.
These files will be updated when the certificate renews.

NEXT STEPS:
- The certificate will need to be renewed before it expires. Certbot can automatically renew the certificate in the background, but you may need to take steps to enable that functionality. See https://certbot.org/renewal-setup for instructions.
We were unable to subscribe you the EFF mailing list because your e-mail address appears to be invalid. You can try again later by visiting https://act.eff.org.

- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
If you like Certbot, please consider supporting our work by:
 * Donating to ISRG / Let's Encrypt:   https://letsencrypt.org/donate
 * Donating to EFF:                    https://eff.org/donate-le
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
```
### 5.2 Copy Your Recently Created Certificates

Your certificates should now be in `/etc/letsencrypt/live/YOUR_DOMAIN/`. In my case, it is `/etc/letsencrypt/live/examplectf.manguitoblue.io/`.
Let's copy `fullchain.pem` and `privkey.pem` into the CTFd folder, so we can use this inside our nginx container. For this, you can execute:

```shell
# Inside your CTFd repo directory
cp /etc/letsencrypt/live/YOUR_DOMAIN/fullchain.pem ./conf/nginx/fullchain.pem
cp /etc/letsencrypt/live/YOUR_DOMAIN/privkey.pem ./conf/nginx/privkey.pem
```

### 5.3 Modify Your docker-compose Configuration

After copying the certificates to the CTFd files, you need to add them to the nginx container volume, so they can be used inside the container. For this, you need to update the volumes section of the nginx service inside the `docker-compose.yml` with the following:

```yaml
...
volumes:
      - ./conf/nginx/http.conf:/etc/nginx/nginx.conf
      - ./conf/nginx/fullchain.pem:/certificates/fullchain.pem:ro
      - ./conf/nginx/privkey.pem:/certificates/privkey.pem:ro
...
```

Also, you need to add the https port to the mapping, this is done by adding the following line in the file:

```yaml
...
ports:
  - 80:80
  - 443:443
...
```

At the end the complete `docker-compose.yml` should look something like this:

```yaml
version: '2'

services:
  ctfd:
    build: .
    user: root
    restart: always
    ports:
      - "8000:8000"
    environment:
      - UPLOAD_FOLDER=/var/uploads
      - DATABASE_URL=mysql+pymysql://ctfd:ctfd@db/ctfd
      - REDIS_URL=redis://cache:6379
      - WORKERS=1
      - LOG_FOLDER=/var/log/CTFd
      - ACCESS_LOG=-
      - ERROR_LOG=-
      - REVERSE_PROXY=true
    volumes:
      - .data/CTFd/logs:/var/log/CTFd
      - .data/CTFd/uploads:/var/uploads
      - .:/opt/CTFd:ro
    depends_on:
      - db
    networks:
        default:
        internal:

  nginx:
    image: nginx:1.17
    restart: always
    volumes:
      - ./conf/nginx/http.conf:/etc/nginx/nginx.conf
      - ./conf/nginx/fullchain.pem:/certificates/fullchain.pem:ro
      - ./conf/nginx/privkey.pem:/certificates/privkey.pem:ro
    ports:
      - 80:80
      - 443:443
    depends_on:
      - ctfd

  db:
    image: mariadb:10.4.12
    restart: always
    environment:
      - MYSQL_ROOT_PASSWORD=ctfd
      - MYSQL_USER=ctfd
      - MYSQL_PASSWORD=ctfd
      - MYSQL_DATABASE=ctfd
    volumes:
      - .data/mysql:/var/lib/mysql
    networks:
        internal:
    # This command is required to set important mariadb defaults
    command: [mysqld, --character-set-server=utf8mb4, --collation-server=utf8mb4_unicode_ci, --wait_timeout=28800, --log-warnings=0]

  cache:
    image: redis:4
    restart: always
    volumes:
    - .data/redis:/data
    networks:
        internal:

networks:
    default:
    internal:
        internal: true
```

### 5.4 Modify Your Nginx Configuration

Finally, we need to specify in the `conf/nginx/http.conf` that we want to use the certificates and the SSL, and also, redirect any traffic from http to https. For this, we need to do the following:

#### 5.4.1 Add the http redirect

```nginx
server {
  listen 80;
  return 301 https://$host$request_uri;
}
```

#### 5.4.2 Define the SSL certificates

```nginx
server {
  listen 443 ssl;

  ssl_certificate /certificates/fullchain.pem;
  ssl_certificate_key /certificates/privkey.pem;
  ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
  ssl_ciphers HIGH:!aNULL:!MD5;
  ...
```

In the end, the complete `conf/nginx/http.conf` file should look something like this:

```nginx
worker_processes 4;

events {

  worker_connections 1024;
}

http {

  # Configuration containing list of application servers
  upstream app_servers {

    server ctfd:8000;
  }

  server {
    listen 80;
    return 301 https://$host$request_uri;
  }

  server {

    listen 443 ssl;

    ssl_certificate /certificates/fullchain.pem;
    ssl_certificate_key /certificates/privkey.pem;
    ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
    ssl_ciphers HIGH:!aNULL:!MD5;

    client_max_body_size 4G;

    # Handle Server Sent Events for Notifications
    location /events {

      proxy_pass http://app_servers;
      proxy_set_header Connection '';
      proxy_http_version 1.1;
      chunked_transfer_encoding off;
      proxy_buffering off;
      proxy_cache off;
      proxy_redirect off;
      proxy_set_header Host $host;
      proxy_set_header X-Real-IP $remote_addr;
      proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
      proxy_set_header X-Forwarded-Host $server_name;
    }

    # Proxy connections to the application servers
    location / {

      proxy_pass http://app_servers;
      proxy_redirect off;
      proxy_set_header Host $host;
      proxy_set_header X-Real-IP $remote_addr;
      proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
      proxy_set_header X-Forwarded-Host $server_name;
    }
  }
}
```

## 6. Restart docker-compose containers

As we needed to stop all the containers to create the certificate, we need to start them again. This time (and if the previous steps worked well) we may run it in detached mode, so they can be living there even when we log out from the remote server. For this, we can execute:

```shell
# Inside the CTFd repo directory
docker-compose up -d

# If you want to be extra cautious you can run:
# docker-compose up --force-recreate -d
```

If everything went well, you may now see that the page has a valid certificate.

![Proof of working certificate](https://res.cloudinary.com/dmrgfufa4/image/upload/v1637902941/articles/ctfd%20with%20https/5.1.png)

## Wrap up

Even if they seem to be lots of steps, they aren't as hard as they seem in the beginning. CTFd and certbot are great projects very well maintained and you shouldn't have any issues while using them.

If this guide helped you, please consider giving it a like.
Good luck in your CTF, and happy hacking!
