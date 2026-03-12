# Commandes

docker run -d --name web-port -p 8080:80 nginx

http://localhost:8080

curl http://localhost:8080

docker ps

docker port web-port

docker stop web-port
docker rm web-port

docker run -d --name web-port -p 127.0.0.1:8080:80 nginx

curl http://localhost:8080

docker port web-port

docker stop web-port
docker rm web-port


## Exemple `EXPOSE` seul

dockerfile
FROM nginx
EXPOSE 80

docker build -t nginx-expose .

docker run -d --name web-expose nginx-expose

docker ps

docker stop web-expose
docker rm web-expose


EXPOSE ne publie pas vers la machine hote


1) port du conteneur : celui sur la quelle l'appli écoute
port hote : celui sur la machine pour accéder à l'appli

2) Il n'y a pas d'ouverture reseau

3) evironnement de dev / test local / service perso ...