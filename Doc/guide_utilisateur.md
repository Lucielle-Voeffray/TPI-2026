# Guide utilisateur

## Concept et buts de ce document

### Public cible

### 

## Administration

### Mettre à jour le système

Pour le moment il n'existe pas de manière unifiée de mettre à jour tous les
noeuds. Il faut se connecter, soit sur l'interface graphique et entrer en
shell dans chaque noeud et effectuer les commandes suivantes:

```bash

sudo apt update
sudo apt upgrade # Ajouter -y pour le mode "sans interraction"

```

### Ajouter un noeud

Voici les différentes étapes quant à l'ajout de noeuds au cluster.

#### Installer Proxmox

La première étape est d'installer l'OS Proxmox sur la machine en question.

Les informations suivantes devront être entrées durant la mise en place:

| Hostname                   | Root Password | IP de management                         | interface |
|----------------------------|---------------|------------------------------------------|-----------|
| a41-srvXX.tpi-lucielle.lan | Selon keepass | Entre 10.19.59.243/24 et 10.19.59.254/24 | nic0      |

Avec ces informations créées, il ne reste plus qu'à suivre les demandes de
l'installateur de l'OS dont voici des captures d'écran.

1) Choisir l'installation graphique est la version la plus user-friendly, c'est
celle-ci qui va être couverte ici, les autres installations sont à vous de
gérer.

![Choix de l'installateur](./Captures/installation/1.png)

2) Lire et accepter les EULA

![EULA](./Captures/installation/2.png)

#### Configurer le réseau

#### Rejoindre le cluster

### Utilisateurs et accès

## Utilisation

### Concepts

### 

### Mise en place
