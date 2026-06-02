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

3) Choisir le disque d'installation

Si le noeud est un Nutanix, il devrait y avoir un disque plus petit que les
autres

![Choix disque](./Captures/installation/3.png)

4) Choisir la locale et le clavier

![Locale](./Captures/installation/5.png)

5) Définir le réseau

Remplir selon la tableau fourni plus haut.
L'interface réseau devrait être la nic0, sur laquel le trunk des VLans 280-282
et le VLan 283 non tagué.

![Réseau](./Captures/installation/5.png)

6) Résumé d'installation

Contrôller les installations et lancer l'installation

![installation](./Captures/installation/6.png)

#### Configurer le réseau

Pour les configuration réseau, vous trouverez les commandes à lancer dans la
documentation de réalisation du projet.

#### Rejoindre le cluster

Afin de rejoindre la node au cluster il faut se connecter à un noeud qui est
déjà dans le cluster, sous datacenter --> cluster, il devrait y avoir un bouton
"join information". Copier ces information, puis les coller dans le même menu
mais le nouveau noeud.

### Utilisateurs et accès

A l'heure actuelle il y a deux utilisateurs:
root --> Accès à tout via l'interface graphique
ansible --> Accès pour la codebase Ansible, ne peux que créer et détruire des
VMs dans le pool `ansible-vms`. Pas d'accès graphique

## Utilisation

### Mise en place

#### SSH - LXC

La méthode la plus simple pout l'utilisation de cette codebase est de se
connecter au LXC qui se trouve dans le cluster. La codebase y est déjà pull, il
ne reste qu'à créer le fichier de configuration et lancer le venv avant de
lancer les playbooks.

#### Linux distro

Si vous avez un VM Linux ou un ordinateur sous Linux, vous pouvez installer
Python3:

```bash
yay -S python # Pour Arch Linux
```

Clone le repository:

```bash
git clone https://github.com/Lucielle-Voeffray/TPI-2026.git tpi-2026/
```

De se déplacer dans le dossier:

```bash
cd tpi-2026/ansible
```

Créer le venv:

```bash
python3 -m venv .venv
```

Activer le venv:

```bash
.venv/bin/activate
```

Installer Ansible et les dépendences du projet:

```bash
pip install proxmoxer requests ansible
ansible-galaxy collection install -r requirements.yml

```

Une fois la codebase en place, il faut créer le fichier de configuration

### Fichier de configuration

Le fichier de configuration se trouve dans ./config/enseignant.yml

Les commentaires dans le fichier indique comment le remplir.

### Lancer un playbook

Une fois la configuration prête, lancer les playbooks avec les commandes
suivantes

#### Créer les VMs

Créer les VMs est on ne peut plus simple:

```bash
ansible-playbook -i inventory.yml playbooks/deploy_vm.yml --ask-vault-pass
```

Le vault-pass est stocké dans la base de données keepass, veuillez vous référer
à l'administrateur système pour y avoir accès.

#### Supprimer les VMs

Les détruire est autant plus simple:

Dry-run:

```bash
ansible-playbook -i inventory.yml playbooks/deploy_vm.yml --ask-vault-pass
```

Run:

```bash
ansible-playbook -i inventory.yml playbooks/deploy_vm.yml --ask-vault-pass -e confirm_destroy=true
```

### Garder des logs

Si vous souhaitez garder des logs de ce qui est créé, vous pouvez ajouter `>
logs/<nom_du_fichier_de_log>.log` à la fin des commandes entionnées au dessus.
