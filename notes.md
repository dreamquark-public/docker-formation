Docker formation

Docker :

Notion/concept de docker : OS/pourquoi c'est plus léger/pourquoi on l'utilise en développement et en prod
différence virtualisation/containerusiation
namespace linux
LXC
RKT

Flexible: Even the most complex applications can be containerized.
Lightweight: Containers leverage and share the host kernel.
Interchangeable: You can deploy updates and upgrades on-the-fly.
Portable: You can build locally, deploy to the cloud, and run anywhere.
Scalable: You can increase and automatically distribute container replicas.
Stackable: You can stack services vertically and on-the-fly.

(Il n'existe pas que docker quand on parle de "contenerisation")
Comprendre les commandes qu'on utilise
Différence entre image et container(run)
Installation de docker

Le Dockerfile
Build --> vers un container registry

Microbadger --> layer --> paralelle avec git, suite chainé de hash

Exercice : 
Ecrire un dockerfile (rick&morty like/node)
Repo Antoine
Changement sur des variables d'environnemnts

1er étape : COPY le code source endur
2eme étape : VOLUME dans le DockerFile
3eme étape : Volume au lancement de la commande docker

Push sur un docker registry (tag différent par personne) --> test registry local --> création
Différence entre push/pull/build
Problème ==> Comment "faire parler" des conteneurs entre eux.
Network, workdir, volume, user, entrypoint, command

Commandes de docker:
rm
stop
up
build
push
pull
kill
attach
exec
search
logs
run

différence entre image de dev et de prod
Docker save/export/restore
Docker cp

docker images:
rmi
docker rmi -f $(docker images)

commandes docker system:
docker system prune
docker volume prune
docker network prune

docker rm -f $(docker ps -aq)

Docker-compose :

Qu'est ce que c'est
Quel problème ça répond
Problème => network, volume, sert pour le dev (point d'appui pour la prod).

Savoir les différences entre le vendor, le build, le up. Ou rm ou stop. REPONDRE A LA QUESTION DES VAR DENV

Exercice :

Transformer la commande de docker run/network en docker-compose. 1 seul fichier à modifier.
Ecrire un docker-compose.yml
--> insister sur le network + volume. (un peu les en var) (override)

Un exemple de commande make ==> pratique pour utiliser des commandes qu'on ne connait pas forcément + enchainement.

commandes docker-compose:
rm
stop
up (-d)
build
pull
exec
run
down

présentation swarm/stack

Kubernetes --> pourquoi on l'utilise ?