# Docker LEMP for Laravel

## Technologies
* [PHP 8.1]
* [PostgreSQL: latest]
* [Nginx: latest]
* [Redis: latest]

## Installation instructions:
* cd into your docker root path
* ``copy .env.example .env``
* ``mkdir config/nginx``
* ``copy site.conf.example config/nginx``
* ``mkdir www``
* copy your laravel file in www directory
* ``docker-compose up -d --build`` or ``docker compose up -d --build``