# TP2 Commun : Stack Web

🌞 **`docker-compose.yml`**

```bash
[et0@TP2 ~]$ cd dockerTp2/ && cat docker-compose.yml
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
```