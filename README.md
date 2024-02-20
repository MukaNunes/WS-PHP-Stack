WS-PHP-Stack Docker Environment
===============

### PHP development environment
- php5.6
- php7.2
- composer
- xdebug
- redis
- phpmyadmin
- mysql 5.6
- Pecl (Certificate fix 02/2024)

### Running
> Put the PHP-Stack project folder in your projects folder
> Then run:

```bash
docker compose up -d --build
```

### Services:
- **localhost**: Nginx WebServer
- **172.8.0.13:33063**: MySQL
- **172.8.0.14:6379**: Redis
- **localhost:8080**: phpmyadmin
