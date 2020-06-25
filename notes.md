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
* Evite d'installer tout sur sa machine

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

#### Comment partager les images qu'on vient de créer ?

* Le conteneur registry => DQ utilise Docker Hub
* Il en existe d'autres: celui de GCP/Azure/AWS par exemple
* Outil centralisé afin de gérer les versions d'images, les droits d'accès, analyse des images etc
* Branché à la CI (CircleCI dans notre cas)
* Docker push => envoie le build de l'image avec le tag DOCKER_TAG seléctionné
* <container_regsitry>/<image_name>:<image_tag>
  - dreamquark/ => cela veut dire qu'on l'envoi dans le conteneur registry dreamquark
  - existe aussi dreamquarkprod
* Quand on build on crée une image unique avec un sha256

##### Différence entre image de dev et de prod

* Permet déjà d'avoir un environnement iso-prod
* Très peu de différence
* En developpement, on monte en volume le code source afin de faire les changements realtime
* On copie les fichiers/dossiers en prod

##### Docker save/export/restore/cp

* docker save ou export sensiblement identique 
- docker save [OPTIONS] IMAGE [IMAGE...]
- docker export [OPTIONS] CONTAINER
* Save pour une image, export pour un container
* docker load pour une image save => permet de l'ajouter à son registry interne comme un pull
* docker import à la suite d'un export => permet de l'ajouter à son registry interne comme un pull


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

##### Pourquoi docker-compose

* Comment "faire parler" les conteneurs entre eux (réseau)
* Comment configurer en runtime plusieurs conteneurs à la fois

##### Docker-compose

* Outil maintenu par docker : https://github.com/docker/compose
* En une seule commande on peut lancer plusieurs conteneurs (appelé service)
* Description simple du réseau, des volumes (pas un enchainement de commande docker) entre autre
* Tout ce qu'on peut faire avec docker-compose peut être fait en commande docker
* Décrit en format YAML
* Un seul fichier mais peut être surchargé

##### Docker-compose

* On peut décrire donc :
  - Image utilisé
  - Les variables d'environnement
  - Les volumes
  - Les ports à exposer du conteneur
  - Commande de démarrage (ou l'entrypoint)
  - Le réseau utilisé
  - etc..
* Peut avoir une notion de dépendance entre les conteneurs (DB démarre avant le serveur web)
* Point d'appui pour la production (différent fichier de configuration)
* Prémice de l'orchestration et pousse à réfléchir sur le déploiment de l'application

##### Docker-compose installation

sudo curl -L "https://github.com/docker/compose/releases/download/1.26.0/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose

sudo chmod +x /usr/local/bin/docker-compose


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

#### Démystification

* les Makefiles (dans brain-docker ou autre)
  - "Raccourci" pour utiliser les commandes présentés précédemment
  - Rien de plus
  - Commande make pour faciliter le travail de chacun
* Exemple avec le vendor:

vendor: ## install repositories and vendors
	$(ENV) $(DKC) $(DKC_CFG) run --rm engine-async make vendor-dev
	$(ENV) $(DKC) $(DKC_CFG) run --rm api make vendor
	$(ENV) $(DKC) $(DKC_CFG) run --rm brain-app make vendor
.PHONY: vendor

  - Simple exécution séquentiel de commande docker compose

* Si changement de variable d'environnement (nom de l'image ou autre) => make up
* Ajout d'un network ou d'un volume => make 
* Le docker-compose up ne recrée que les services où il y a des changements
* Make build => si on utilise des images locales (voué à disparaitre) => quand on change le dockerfile surtout
* Make build => sachant qu'on monte en volume le code source
* Make vendor => lorsqu'il y a un changement dans les paquets (node_modules ou poetry)

################################# => Antoine
SLIDE 11

Exercice :

Transformer la commande de docker run/network en docker-compose. 1 seul fichier à modifier.
Ecrire un docker-compose.yml
--> insister sur le network + volume. (un peu les en var) (override)

(Un exemple de commande make ==> pratique pour utiliser des commandes qu'on ne connait pas forcément + enchainement.)

################################# => Théo
SLIDE 12

##### Le passage en production

* Notre application tourne, on est très content
* Ca marche sur chacun des PC de la boite, cool
* Faut peut-être montrer ça au client maintenant !
  => Le passage en production est de mise

##### Différentes solutions

* Docker Swarm => gérer par Docker, permet la création d'un cluster de conteneur
* OpenShift => Surcouche de Docker & Kubernetes, gérer par RedHat
* Rancher => Surcouche de Docker & Kubernetes
* Nomad => Solution de HashiCorp

##### Pourquoi Kubernetes ?

* Porté par Google & open source (fondation CloudNative)
* Orchestrateur de contenu
* Technologie qui s'universalise
* Supporté par la majorité des cloud provider (et on prem)
* Facilement configurable
* Permet une scalabilité très réactive
* Désigné pour du micro-service
* Conteneur centric
* Fonctionne SUR des machines/noeuds

Screen

* Possible d'installer Minikube en local pour faire des tests de déploiement Kubernetes

##### Il manque un truc quand même...

* Mais beaucoup de configuration et de fichiers
* Difficilement versionnable
* Templating pas évident
* La gestion de nombreux environnement n'était pas forcément scalable

##### Helm

* Package manager de configuration Kubernetes
* Soutenu par Kubernetes directement
* Systeme de templating très simple
* Package l'application (notre petit déploiement Brainy avec ses moultes briques)
* En 1 fichier de configuration (YAML encore et toujours)
* Simplifie énormément la configuration 
* L'upgrade de configuration est plus simple (rollback possible)

 => si vous voulez faire un serveur minecraft local (avec Minikube) : 
 helm install --name my-release \
    --set minecraftServer.eula=true,persistence.dataDir.enabled=false \
    stable/minecraft

#################################

#### Liens intéressant, les sources quoi

https://kubernetes.io/docs/home/
https://helm.sh/
https://docs.docker.com/get-started/
=> bcp de bon tutos directement sur Docker si vous voulez approfondir
https://docs.docker.com/compose/
https://docs.docker.com/compose/compose-file/