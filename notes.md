Docker formation

Docker :

################################# => Théo
SLIDE 1

#### Pourquoi utiliser Docker & les conteneurs

* Qu'est ce que les conteneurs ?
* Y'a t-il que Docker ?
* Pourquoi pas utiliser des machines virtuelles ?
* Pourquoi c'est plus léger ?
* Viable en production & en développement ?
* Qu'est ce qu'on y gagne ?

##### Qu'est ce que les conteneurs ?

* Environnement de système d'exploitation
* Fourni une portabilité du logiciel
* Facilité de partager et construire une application
* Installe QUE le nécessaire => Léger, + efficiente (moins de resources)
* Il existe un conteneur pour TOUT

##### Pourquoi les utilise-t-on ?

* Normalisation entre dev/prod
* "Ca marche sur mon pc"
* Scalable
* Isolement
* Plus facile à manager que des VM

##### Ce qu'il y a d'autres

* VMs
* RKT => concurrent de Docker

##### Installation Linux

* Installation du démon Docker (processus en background)
* Paquet à part entière de Ubuntu
$ sudo apt-get update

$ sudo apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg-agent \
    software-properties-common
$ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
$ sudo apt-get update
$ sudo apt-get install docker-ce docker-ce-cli containerd.io
$ sudo groupadd docker
$ sudo usermod -aG docker $USER

* Se mettre dans le groupe docker (afin de pas faire du root)
* https://docs.docker.com/engine/install/ubuntu/#install-docker-engine

##### Installation MacOS

* Soft a installé directement
* https://docs.docker.com/docker-for-mac/install/


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

#### DockerFile / Principe

* Création de l'image au moment du docker build
* .dockerignore du dossier passé dans le docker build
* Principe des layers (si identique utilise celui en cache)
* Important de bien ordonnnancer les commandes du DOCKERFILE
* Commandes utilisées dans nos DockerFile

FROM => Image utilisée comme base (comme ubuntu, debian)
FROM [--platform=<platform>] <image>[:<tag>] [AS <name>]

ENV => Ajout d'une variable d'environnement
ENV <key> <value>

RUN => Commande exécuté dans le conteneur (à l'instant t)
RUN /bin/bash -c 'source $HOME/.bashrc; echo $HOME'
RUN /bin/bash -c 'source $HOME/.bashrc; \
echo $HOME'

WORKDIR => Les prochaines commandes RUN/CMD/ENTRYPOINT/COPY/ADD seront exécuté dans ce dossier après
WORKDIR /path/to/workdir

USER Setter le username/UID qui utilise les commandes RUN/CMD/ENTRYPOINT
USER <user>[:<group>]

COPY => copie ou télécharge des fichiers dans l'image
COPY [--chown=<user>:<group>] <src>... <dest>
COPY [--chown=<user>:<group>] ["<src>",... "<dest>"]

ADD=> copie ou télécharge des fichiers dans l'image mais peut ajouter une commande (comme décompresser un tar par ex)
ADD [--chown=<user>:<group>] <src>... <dest>
ADD [--chown=<user>:<group>] ["<src>",... "<dest>"]

EXPOSE Expose un port du conteneur & écoute spécifiquement ce port (pour publier le port docker run -p)
EXPOSE <port> [<port>/<protocol>...]

VOLUME (docker run -v) Crée un point de montage dans le conteneur & dans le file system (persistence de la donnée)
VOLUME ["/data"]

ENTRYPOINT spécifie un binaire ou un script qui sera exécuté au runtime du conteneur. (un exécutable ex: nginx)
ENTRYPOINT Utilisé si on veut des images à usage unique (genre serveur web)
ENTRYPOINT ["executable", "param1", "param2"]

CMD Pour spécifier une commande à exécuter seulement lors du lancement du conteneur
CMD Si on surcharge la commande de run, cela va directement écraser la commande (en laissant l'entrypoint), peut servir pour passer des arguments
CMD ["executable","param1","param2"]
CMD command param1 param2

Microbadger --> layer --> paralelle avec git, suite chainé de hash (présentation)
* https://microbadger.com/
* Analyse des hashs de layer des images
* Photo de l'analyse

Après avoir build les images, pour les partager on peut les pousser dans conteneur registry

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
Le rebuild ne répare pas grand chose
Différence avec le makefile

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
