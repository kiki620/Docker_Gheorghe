# Docker_Gheorghe
trabajo de Docker 2025

version: '3.9'

services:
  db:
    image: mariadb:10.6
    restart: always
    environment:
      MYSQL_ROOT_PASSWORD: rootpassword
      MYSQL_DATABASE: nextcloud_db
      MYSQL_USER: nextclouduser
      MYSQL_PASSWORD: nextcloudpassword
    volumes:
      - db_data:/var/lib/mysql
      - ./base_de_datos.sql:/docker-entrypoint-initdb.d/base_de_datos.sql

  app:
    image: nextcloud
    restart: always
    depends_on:
      - db
    ports:
      - 8080:80
    environment:
      MYSQL_HOST: db
      MYSQL_DATABASE: nextcloud_db
      MYSQL_USER: nextclouduser
      MYSQL_PASSWORD: nextcloudpassword
    volumes:
      - ./cloudNew:/var/www/html

  cron:
    image: nextcloud
    restart: always
    depends_on:
      - db
    entrypoint: /cron.sh
    volumes:
      - ./cloudNew:/var/www/html
    environment:
      MYSQL_HOST: db
      MYSQL_DATABASE: nextcloud_db
      MYSQL_USER: nextclouduser
      MYSQL_PASSWORD: nextcloudpassword

volumes:
  db_data:
