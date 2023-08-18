# Assignment2

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
