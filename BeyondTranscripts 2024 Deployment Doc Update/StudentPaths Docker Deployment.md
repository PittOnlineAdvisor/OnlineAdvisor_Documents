# StudentPaths Docker Deployment

## Requirements

- Git
- Java 1.8
- Docker

## Docker Image Generation

Note that the image name generated need to match the name in the docker-compose.yml file.

```bash
git clone https://github.com/PittOnlineAdvisor/OnlineAdvisor_FrontEnd.git
cd OnlineAdvisor_FrontEnd
docker build -t FRONTEND_IMAGE_NAME .
```

```bash
git clone https://github.com/PittOnlineAdvisor/OnlineAdvisor.git
cd OnlineAdvisor
docker build -t BACKEND_IMAGE_NAME .
```

```bash
git clone https://github.com/PittOnlineAdvisor/OnlineAdvisor_ML.git
cd OnlineAdvisor_ML
docker build -t MACHINE_LEARNING_IMAGE_NAME .
```

## Docker Compose

- To access the file, navigate to your local OnlineAdvisor_ML folder.
- Below is an example `docker-compose.yml` file, replace the `image` field under `advisor-frontend`, `advisor-backend`, and `advisor-ml` accordingly depending on the image generated in the previous step
- Please note that the application requires MySQL version 8

```yml
version: "3.8"
services:
  advisor-frontend:
    image: "online-advisor-frontend"
    depends_on:
      - advisor-redis
      - advisor-mysql
      - advisor-backend
      - redis-persist
      - advisor-ml
    ports:
      - 3000:80
    volumes:
      - ./nginx.conf:/etc/nginx/nginx.conf
  advisor-ml:
    image: "online-advisor-ml"
    depends_on:
      - redis-persist
    volumes:
      - ./models:/app/models
      - ./config.ini:/app/config.ini
  advisor-redis:
    image: "redis:alpine"
  redis-persist:
    build: ./CustomedRedis
    volumes:
      - ./redis.conf:/usr/local/etc/redis/redis.conf
  advisor-backend:
    image: "online-advisor-backend"
    depends_on:
      - advisor-mysql
      - advisor-redis
    ports:
      - 8080:8080
  advisor-mysql:
    image: mysql:8.0
    command: --default-authentication-plugin=mysql_native_password --init-file /data/init.sql
    environment:
      MYSQL_ROOT_PASSWORD: advisor@2021
    # ports:
    #   - 3306:3306
    volumes:
      - ./mysql:/var/lib/mysql
      - ./init.sql:/data/init.sql
```

## Launch App

- After modifying the `docker-compose.yml` file, run the following command in the terminal

```bash
cd OnlineAdvisor_ML
docker compose up
```
