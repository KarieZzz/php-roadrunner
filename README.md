# php-roadrunner

Symfony project template based on docker with <a href="https://github.com/spiral/roadrunner">RoadRunner</a> as server.


## Installation
```bash
composer install
docker-compose up -d
```

Server is reachable via http://localhost:8080/


## Composer Scripts
- **reset-workers**: run this script manually or automatically with file watchers to reload changed configurations and files
- **show-workers**: display running workers
