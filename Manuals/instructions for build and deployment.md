# instructions for build and deployment
## Environment requirement
* git

* Docker

* java 1.8 with maven

## Install Docker

Follow the instruction here [Get Docker | Docker Documentation](https://docs.docker.com/get-docker/)

## Build

### frontend

```bash
git clone https://github.com/PittOnlineAdvisor/OnlineAdvisor_FrontEnd.git
cd OnlineAdvisor_FrontEnd
docker build -t <FRONTEND_IMAGE_NAME> .
```

### backend

```bash
git clone https://github.com/PittOnlineAdvisor/OnlineAdvisor.git
cd OnlineAdvisor
docker build -t <BACKEND_IMAGE_NAME> .
```

### machine learning

```bash
git clone https://github.com/PittOnlineAdvisor/OnlineAdvisor_ML.git
cd OnlineAdvisor_ML
docker build -t <MACHINE_LEARNING_IMAGE_NAME> .
```

### modify the docker-compose.yml

since you are building the image by yourself, you may need to change the image name in `docker-compose.yml` to use the image you just built.

Here is the template, please feel free to modify it

```dockerfile
version: '3.8'
services:
  advisor-frontend:
    image: '<FRONTEND_IMAGE_NAME>'
    restart: unless-stopped
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
    image: '<MACHINE_LEARNING_IMAGE_NAME>'
    restart: unless-stopped
    depends_on:
      - redis-persist
    volumes:
      - ./models:/app/models
      - ./config.ini:/app/config.ini
  advisor-redis:
    image: 'redis:alpine'
    restart: unless-stopped
  redis-persist:
    build: ./CustomedRedis
    restart: unless-stopped
    volumes: 
      - ./redis.conf:/usr/local/etc/redis/redis.conf
  advisor-backend:
    image: '<BACKEND_IMAGE_NAME>'
    restart: unless-stopped
    depends_on:
      - advisor-mysql
      - advisor-redis
    ports:
      - 8080:8080
  advisor-mysql:
    image: mysql
    command: --default-authentication-plugin=mysql_native_password --init-file /data/init.sql
    restart: unless-stopped
    environment:
      MYSQL_ROOT_PASSWORD: <MYSQL_ROOT_PASSWORD>
    volumes:
      - ./mysql:/var/lib/mysql
      - ./init.sql:/data/init.sql
```





## Deployment

- [ ] download the installer from box according to your OS. 
- [ ] run the exe or  unzip the installer to extract all files to a folder
- [ ] make sure docker is already running
- [ ] run the start.bat or start.bash depending on your OS.

