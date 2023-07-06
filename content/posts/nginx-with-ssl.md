---
title: "Setting up a Local Nginx with a Self-Signed Certificate"
description: "Create a safe local nginx for test usage"
date: 2023-07-06T02:10:18.062Z
draft: false
toc: true
images:
author: "Justin Hung"
type: ["posts", "post"]
tags:
  - Docker
  - docker-compose
  - Nginx
categories:
  - POC
series:
  - Docker
---

In this blog post, we will discuss how to create a local Nginx server with 443 port and redirect 80 to 443 with Docker Compose. We will also include the command for generating a self-certificate.

## Setting up the Docker Compose File

Firstly, we need to create a `docker-compose.yml` file in the root directory of our project. In this file, we will define the service for the Nginx server.

```yaml
version: "3.8"
services:
  nginx:
    image: nginx:stable-alpine
    container_name: nginx
    ports:
      - "80:80"
      - "443:443"
    restart: unless-stopped
    volumes:
      - ./config/conf.d/:/etc/nginx/conf.d/:ro
      - ./config/nginx.conf:/etc/nginx/nginx.conf:ro
      - ./config/certs/:/etc/nginx/certs/:ro
```

In the above code snippet, we define the Nginx service and specify the image to use. We also define the container name and specify the ports to be used. Furthermore, we mount the configuration file and certificate files from the local machine to the container. Be careful with the path of the folder, in this example, you should create a `config` folder under the folder where `docker-compose.yml` is.

## Creating the Configuration File

Next, we need to create the Nginx configuration file on our local machine. We will create a file named `nginx.conf`and save it in the `config` directory of our project.

> `config/nginx.conf`
> 

```bash
user  nginx;
worker_processes  auto;

error_log  /var/log/nginx/error.log notice;
pid        /var/run/nginx.pid;

events {
    worker_connections  1024;
}

http {
    include       /etc/nginx/mime.types;
    default_type  application/octet-stream;

    log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';

    access_log  /var/log/nginx/access.log  main;
    error_log /var/log/nginx/error.log;

    sendfile        on;
    #tcp_nopush     on;

    keepalive_timeout  65;

    #gzip  on;

    include /etc/nginx/conf.d/*.conf;
}
```

Then, we will create a file named `default.conf` and save it in the `config/conf.d` directory of our project.

> `config/conf.d/nginx.conf`
> 

```bash
user  nginx;
worker_processes  auto;

error_log  /var/log/nginx/error.log notice;
pid        /var/run/nginx.pid;

events {
    worker_connections  1024;
}

http {
    include       /etc/nginx/mime.types;
    default_type  application/octet-stream;

    log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';

    access_log  /var/log/nginx/access.log  main;
    error_log /var/log/nginx/error.log;

    sendfile        on;
    #tcp_nopush     on;

    keepalive_timeout  65;

    #gzip  on;

    include /etc/nginx/conf.d/*.conf;
}
```

In the above code snippet, we define the Nginx configuration file. We set up the server to listen on port 80 and redirect to HTTPS. We also configure the server to listen on port 443 with SSL enabled. Furthermore, we configure the certificate file path in the configuration file.

## Generating a Self-Certificate

Lastly, we need to generate a self-certificate for our Nginx server. We can generate a self-certificate using the OpenSSL command.

```
openssl genrsa -out justin.local.key 2048
openssl req -new -x509 -key justinlocal.key \
    -out justin.local.crt \
    -subj "/C=TW/ST=Taiwan/L=MyCity/O=MyCompany/CN=*.justin.local" \
    -days 3650
```

In the above code snippet, we generate a self-certificate using two OpenSSL commands. The first command generates a private key and certificate signing request, while the second command generates a self-signed certificate.

## Mounting the Configuration and Certificate Files

After creating the Docker Compose file and the Nginx configuration file, we need to mount these files to the container. In the docker-compose.yml file, we specified the volumes to be mounted.

```yaml
volumes:
  - ./config/conf.d/:/etc/nginx/conf.d/:ro
  - ./config/nginx.conf:/etc/nginx/nginx.conf:ro
  - ./config/certs/:/etc/nginx/certs/:ro
```

We mount the nginx.conf file to `/etc/nginx/nginx.conf` in the container and the certs folder to `/etc/nginx/certs` in the container. The certs folder contains the self-signed certificate files that we generated earlier.

## Running the Nginx Server

To run the Nginx server, we need to run the following command in the terminal in the root directory of our project.

```
docker-compose up
```

This command starts the container and runs the Nginx server. We can access the Nginx server by visiting [https://localhost](https://localhost/) in our web browser.

## Conclusion

In this blog post, we create a local Nginx server with 443 port and redirect 80 to 443 with Docker Compose. We also included the command for generating a self-certificate. By following the steps in this blog post, you can easily set up your own Nginx server for local development. Additionally, we went through how to mount the configuration and certificate files and how to run the Nginx server using Docker Compose.