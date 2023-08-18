# Assignment2 
###Task-1
#### Networking

sudo docker network create --driver bridge my_network
sudo docker network ls
{
219a39006a13   my_network   bridge    local
}
sudo docker run -it -d --network my_network -p 8080:80 --name nginix_container nginx
sudo docker run -it -d --network my_network -p 8081:80 --name httpd_container httpd
http://50.19.39.100:8080/
http://50.19.39.100:8081/
{
CONTAINER ID   IMAGE     COMMAND                  CREATED              STATUS              PORTS                                   NAMES
6b2398b97c41   httpd     "httpd-foreground"       20 seconds ago        Up 5 seconds        0.0.0.0:8081->80/tcp, :::8081->80/tcp   httpd_container
8cc9b7319094   nginx     "/docker-entrypoint.…"   About a minute ago   Up About a minute   0.0.0.0:8080->80/tcp, :::8080->80/tcp   nginix_container
}
sudo docker stop 8cc9b7319094
sudo docker rm   8cc9b7319094
sudo docker run -it -d --network my_network -p 8082:80 --name nginix_container nginx
{
182a0aff4f5d   nginx     "/docker-entrypoint.…"      5 seconds ago   0.0.0.0:8082->80/tcp, :::8082->80/tcp   nginix_container
http://50.19.39.100:8082/
}
Remove the container using command
sudo docker stop id
sudo docker rm id
Remove the network using command
sudo docker network rm networkid


##############################################################################################################################################

Assignment-2 Task-2

#install docker compose

sudo curl -L "https://github.com/docker/compose/releases/download/1.26.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
sudo chmod =x /usr/local/bin/docker-compose

###ocker compose file
nano docker-compose.yml

version: '3.3'
networks:
    batman:
      driver: bridge

services:
  db:
    image: mysql:5.7
    networks:
      - batman
    volumes:
      - db_data:/var/lib/mysql
    restart: always
    environment:
      MYSQL_ROOT_PASSWORD: root
      MYSQL_DATABASE: wordpress
      MYSQL_USER: ibrar
      MYSQL_PASSWORD: ibrar

  wordpress:
    depends_on:
      - db
    image: "wordpress:latest"
    ports:
      - "8000:80"
    restart: always
    environment:
      WORDPRESS_DB_HOST: db:3306
      WORDPRESS_DB_USER: ibrar
      WORDPRESS_DB_PASSWORD: ibrar
      WORDPRESS_DB_NAME: nginx

volumes:
    db_data: {}

sudo docker-compose up
sudo docker ps
sudo docker service ls
##machine ip:8000 
sudo docker-compose down



############Grafana installation
sudo apt-get update
sudo apt-get install -y software-properties-common
sudo add-apt-repository "deb https://packages.grafana.com/oss/deb stable main"
wget -q -O - https://packages.grafana.com/gpg.key | sudo apt-key add -
sudo apt-get update
sudo apt-get install grafana

#######Install Loki and Promtail using Docker
#####Download Loki Config
wget https://raw.githubusercontent.com/grafana/loki/v2.8.0/cmd/loki/loki-local-config.yaml -O loki-config.yaml
Run Loki Docker container
docker run -d --name loki -v $(pwd):/mnt/config -p 3100:3100 grafana/loki:2.8.0 --config.file=/mnt/config/loki-config.yaml

####Download Promtail Config
wget https://raw.githubusercontent.com/grafana/loki/v2.8.0/clients/cmd/promtail/promtail-docker-config.yaml -O promtail-config.yaml

####Run Promtail Docker container
docker run -d --name promtail -v $(pwd):/mnt/config -v /var/log:/var/log --link loki grafana/promtail:2.8.0 --config.file=/mnt/config/promtail-config.yaml


####NOTE
Grafana used for visualization monitoring
Loki stores the logs and gives logs to Grafana for display
Promtail collect the logs from machine and send to Loki.
