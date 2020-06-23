Docker formation

Docker :

################################# => Théo
SLIDE 1

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
Installation de docker(Mac & Linux) / le user docker & ajout du user dans le group (pas utiliser sudo, pourquoi?)

################################# => Antoine
SLIDE 2

Comprendre les commandes qu'on utilise

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

################################# => Antoine
SLIDE 3

Docker => Différence entre image et container(run) => répondre à la question : Quand on utilise une variable d'environnement, est-ce qu'il faut rebuild ou pas ?

################################# => Théo
SLIDE 4

Le Dockerfile
Build --> vers un container registry

Microbadger --> layer --> paralelle avec git, suite chainé de hash (présentation)

################################# => Antoine
SLIDE 5

Exercice : 
Ecrire un dockerfile (rick&morty like/node)
Repo Antoine
Changement sur des variables d'environnemnts

1er étape : COPY le code source endur
2eme étape : VOLUME dans le DockerFile
3eme étape : Volume au lancement de la commande docker

################################# => Théo
SLIDE 6

Push sur un docker registry (tag différent par personne) --> test registry local --> création (création du docker registry)
Différence entre push/pull/build

différence entre image de dev et de prod
Docker save/export/restore
Docker cp

################################# => Antoine
SLIDE 7

docker images:
docker rmi -f $(docker images)

docker rm -f $(docker ps -aq)

commandes docker system:
docker system prune
docker volume prune
docker network prune

Lazy Docker

################################# => Théo
SLIDE 8

Problème ==> Comment "faire parler" des conteneurs entre eux.
Network, workdir, volume, user, entrypoint, command

Docker-compose :

Qu'est ce que c'est
Quel problème ça répond
Problème => network, volume, sert pour le dev (point d'appui pour la prod).

################################# => Antoine
SLIDE 9

commandes docker-compose:
rm
stop
up (-d)
build
pull
exec
run
down

################################# => Théo
SLIDE 10

DREAMQUARK PART:

Savoir les différences entre le vendor, le build, le up. Ou rm ou stop. REPONDRE A LA QUESTION DES VAR DENV

################################# => Antoine
SLIDE 11

Exercice :

Transformer la commande de docker run/network en docker-compose. 1 seul fichier à modifier.
Ecrire un docker-compose.yml
--> insister sur le network + volume. (un peu les en var) (override)

(Un exemple de commande make ==> pratique pour utiliser des commandes qu'on ne connait pas forcément + enchainement.)

################################# => Théo
SLIDE 12

présentation swarm/stack

Kubernetes --> pourquoi on l'utilise ?
Helm -> présentation et pourquoi on l'utilise

#################################