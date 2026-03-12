# Création du réseau personnalisé
docker network create test_net

# Lancement du serveur Nginx sur le nouveau réseau
docker run -d --name serveur --network test_net nginx

# Vérification par ping depuis un conteneur éphémère
docker run --rm --network test_net alpine ping -c 2 serveur
# Le ping fonctionne

# Vérification par requête HTTP
docker run --rm --network test_net alpine wget -qO- serveur
# La requete fonctionne

#Inspection du réseau pour identifier les conteneurs connectés
docker network inspect test_net

# Lancement sur le pont par défaut
docker run -d --name serveur-bridge nginx

# Tentative de ping 
docker run --rm alpine ping -c 2 serveur-bridge
#Pas de réponse

# Lancement d'un conteneur isolé
docker run -d --name passager alpine sleep 1000

# Connexion au réseau test_net en cours d'exécution
docker network connect test_net passager

# Vérification de la connectivité vers 'serveur'
docker exec passager ping -c 2 serveur
# Le ping fonctionne

# Déconnexion et nettoyage
docker network disconnect test_net passager

docker rm -f serveur serveur-bridge passager
docker network rm test_net

1) il simplifie la communication entre conteneurs et isole mieux les services.

2) il permet aux conteneurs de se contacter directement par leur nom.

3) il permet de voir quels conteneurs sont sur le réseau et leurs informations.