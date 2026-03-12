# Création du réseau et du volume
docker network create app_network
docker volume create db_volume

# lancement du conteneur mysql
docker run -d --name mysql_db --network app_network -v db_volume:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=secret -e MYSQL_DATABASE=app_db mysql:8.0

# lancement du conteneur webapp
docker run -d --name webapp --network app_network -p 8080:80 nginx

#inspection du réseau
docker network inspect app_network
#Les containers apparaissent bien dans le réseau

# Insertion d'une donnée
docker exec -i mysql_db mysql -u root -psecret app_db -e "CREATE TABLE IF NOT EXISTS inventory (id INT AUTO_INCREMENT PRIMARY KEY, item VARCHAR(50)); INSERT INTO inventory (item) VALUES ('Docker Volume Test');"

# Vérification de la présence de cette donnée
docker exec -i mysql_db mysql -u root -psecret app_db -e "SELECT * FROM inventory;"

# Supression du conteneur
docker rm -f mysql_db

docker run -d --name mysql_db --network app_network -v db_volume:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=secret -e MYSQL_DATABASE=app_db mysql:8.0

# Vérification de la présence de la donnée
docker exec -i mysql_db mysql -u root -psecret app_db -e "SELECT * FROM inventory;"
#La donnée est toujours présente

#Nettoyage 
docker rm -f webapp mysql_db
docker network rm app_network
docker volume rm db_volume