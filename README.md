Docker Tutorial For Beginners - How To Containerize Wordpress Applications
===


Create an empty project directory.
---

```bash
$ mkdir wordpress-docker
$ cd wordpress-docker/

```

Create a docker-compose.yml file that starts your WordPress blog and a separate MySQL instance with volume mounts for data persistence:
---

```bash
$ touch docker-compose.yml
```

Content docker-compose.yml
---

```yml
version: '3'

services:
  # Database
  db:
    image: mysql:5.7
    volumes:
      - db_data:/var/lib/mysql
    restart: always
    environment:
      MYSQL_ROOT_PASSWORD: password
      MYSQL_DATABASE: wordpress
      MYSQL_USER: wordpress
      MYSQL_PASSWORD: wordpress
    networks:
      - wpsite
  # phpmyadmin
  phpmyadmin:
    depends_on:
      - db
    image: phpmyadmin/phpmyadmin
    restart: always
    ports:
      - '8080:80'
    environment:
      PMA_HOST: db
      MYSQL_ROOT_PASSWORD: password 
    networks:
      - wpsite
  # Wordpress
  wordpress:
    depends_on:
      - db
    image: wordpress:latest
    ports:
      - '8000:80'
    restart: always
    volumes: ['./:/var/www/html']
    environment:
      WORDPRESS_DB_HOST: db:3306
      WORDPRESS_DB_USER: wordpress
      WORDPRESS_DB_PASSWORD: wordpress
    networks:
      - wpsite
networks:
  wpsite:
volumes:
  db_data:
```

Build the project
---

Now, run docker compose up -d from your project directory.


```bash
$ docker compose up -d
```


Bring up WordPress in a web browser
---

Open http://localhost:8000 in a web browser.
