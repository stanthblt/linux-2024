# TP2 admins : Web stack

## Sommaire

- [TP2 admins : Web stack](#tp2-admins--web-stack)
  - [Sommaire](#sommaire)
- [I. Good practices](#i-good-practices)
- [II. Reverse proxy buddy](#ii-reverse-proxy-buddy)
  - [A. Simple HTTP setup](#a-simple-http-setup)
  - [B. HTTPS auto-signé](#b-https-auto-signé)
  - [C. HTTPS avec une CA maison](#c-https-avec-une-ca-maison)

# I. Good practices

🌞 **Limiter l'accès aux ressources**

```bash
docker run --memory=1g --cpus=1 IMAGE_NAME
```

🌞 **No `root`**

```DockerFile
FROM nginx:latest
RUN useradd -m -s /bin/bash nginxuser
USER nginxuser
```

🌞 **Gestion des droits du volume qui contient le code**

```bash
sudo chmod -R 755 /app
```

🌞 **Gestion des capabilities sur le conteneur NGINX**

```bash
services:
  nginx:
    image: custom-nginx
    cap_drop:
      - ALL
```

🌞 **Mode read-only**

```bash
services:
  nginx:
    image: custom-nginx
    read_only: true
```

## A. Simple HTTP setup

🌞 **Adaptez le `docker-compose.yml`**

```bash
version: '3.9'

services:
  app:
    build: ./app
    ports: 
      - "8080:8080"
    environment:
      - DB_HOST=db
      - PORT=8080

  db:
    image: mysql
    environment:
      - MYSQL_ROOT_PASSWORD=password
    ports:
      - "3306:3306"
    volumes:
      - ./seed.sql:/docker-entrypoint-initdb.d/seed.sql

  phpmyadmin:
    image: phpmyadmin
    ports:
      - "1337:80"
    environment:
      - PMA_HOST=db
      - MYSQL_ROOT_PASSWORD=password

  reverse-proxy:
    image: nginx
    ports:
      - "80:80"
      - "443:443"
    volumes:
      - ./reverse-proxy/nginx.conf:/etc/nginx/conf.d/default.conf
      - ./certs:/certs
```

```bash
server {
    listen       80;
    server_name  www.supersite.com;

    location / {
        proxy_pass   http://app:8080;
    }
}

server {
    listen       80;
    server_name  pma.supersite.com;

    location / {
        proxy_pass   http://phpmyadmin:80;
    }
}
```

## B. HTTPS auto-signé

🌞 **HTTPS** auto-signé

```bash
openssl req -new -newkey rsa:4096 -days 365 -nodes -x509 \
-keyout www.supersite.com.key -out www.supersite.com.crt
```

```bash
server {
    listen 443 ssl;
    server_name www.supersite.com;

    ssl_certificate /certs/www.supersite.com.crt;
    ssl_certificate_key /certs/www.supersite.com.key;

    location / {
        proxy_pass http://app:8080;
    }
}

server {
    listen 443 ssl;
    server_name pma.supersite.com;

    ssl_certificate /certs/www.supersite.com.crt;
    ssl_certificate_key /certs/www.supersite.com.key;

    location / {
        proxy_pass http://phpmyadmin:80;
    }
}
```

🌞 **Générer une clé et un certificat de CA**

```bash
openssl genrsa -des3 -out CA.key 4096
openssl req -x509 -new -nodes -key CA.key -sha256 -days 1024 -out CA.pem
```

🌞 **Générer une clé et une demande de signature de certificat pour notre serveur web**

```bash
openssl req -new -nodes -out www.supersite.com.csr -newkey rsa:4096 -keyout www.supersite.com.key
```

🌞 **Faire signer notre certificat par la clé de la CA**

```txt
authorityKeyIdentifier=keyid,issuer
basicConstraints=CA:FALSE
keyUsage = digitalSignature, nonRepudiation, keyEncipherment, dataEncipherment
subjectAltName = @alt_names

[alt_names]
DNS.1 = www.supersite.com
DNS.2 = www.tp2.admins
```

```bash
openssl x509 -req -in www.supersite.com.csr -CA CA.pem -CAkey CA.key \
-CAcreateserial -out www.supersite.com.crt -days 500 -sha256 -extfile v3.ext
```

🌞 **Ajustez la configuration NGINX**

```bash
server {
    listen 443 ssl;
    server_name www.supersite.com;

    ssl_certificate /certs/www.supersite.com.crt;
    ssl_certificate_key /certs/www.supersite.com.key;

    location / {
        proxy_pass http://app:8080;
    }
}

server {
    listen 443 ssl;
    server_name pma.supersite.com;

    ssl_certificate /certs/www.supersite.com.crt;
    ssl_certificate_key /certs/www.supersite.com.key;

    location / {
        proxy_pass http://phpmyadmin:80;
    }
}
```