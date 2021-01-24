## CPL Evaluation Portal (Frontend)
* Live: http://118.27.10.116:5000/
* Backend: http://0.0.0.0:8000/
* Frontend: http://0.0.0.0:5000/
* DockerHub: https://hub.docker.com/u/siyam04

### NPM Commands
* Run Server
```
npm run serve
```
* Run For Production
```
npm run build
```

* Docker-Compose Issues
```
rm -rf node_modules package-lock.json && npm install
```
```
close and re-open terminal
```
```
docker-compose up
```

### Docker-Compose Commands

* BUILD
```
docker-compose build --no-cache --force-rm
```
* PUSH
```
docker-compose push
```
* PULL
```
docker-compose pull
```
* UP
```
docker-compose up
```
* DOWN
```
docker-compose down
```
* UP, DOWN (run in background)
```
docker-compose up -d
```
```
docker-compose down -d
```

### Docker Commands

* IMAGE LIST
```
docker images
```
* CONTAINER LIST
```
docker container ls -a
```
* STOP CONTAINER
```
docker container stop container_id
```
* PROCESS LIST
```
docker ps -a
```
* REMOVE IMAGES
```
docker image rm image_id_1 image_id_2 image_id_3
```
* REMOVE CONTAINER
```
docker container rm container_id
```
### Project Related (from project directory)

* SHOW IMAGES
``` 
docker-compose images
```
* SHOW COMMANDS
```
docker-compose
```
* PULL, MIGRATE, FAKER, UP
```
docker-compose pull
```
```
docker-compose run my_service_name python manage.py migrate
```
```
docker-compose run my_service_name python fake_data.py (if available)
```
```
docker-compose up
```   
* SHOW HOST IP
```
docker-compose exec my_service_name bash
```
```
hostname -i
```
```
docker-compose exec my_db_service_name bash
```
```
hostname -i   
```
