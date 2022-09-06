---
layout: post
comments: true
author: islimani
title: Connecting and transfering files between machines using SSH (French)
date: 2020-11-14 13:32:20 +0300
words: 0
pdf: true
summary:  # Add post description (optional)
thumbnail: /assets/img/ssh.png
category:
        - ssh
---

## Introduction

Dans ce TP on a utilisé deux virtual machines Linux Ubuntu 14 et Ubuntu 16 pour le serveur et le client respectivement dans un environment vmware. 

* L'addresse du serveur est: `192.168.134.131` et du client `192.168.134.129`
* L'utlisateur par default dans la machine serveur est: `dev`
* Le hostname de la machine serveur est: `genux`

## Le service SSH: Machine serveur
### Installation de sshd

`sudo apt-get install openssh-server `

### Vérification que les clés ne sont pas générés:

```sh
cd /etc/ssh
ls
```

Les fichiers `ssh_host_rsa_key` et `ssh_host_rsa_key.pub` ne se trouvent pas

### Démarage de ssd et vérification de la génération des clés:

`Sudo service ssh start`

Désormais, on trouve dans `/etc/ssh` les fichiers suivants:
```sh
moduli            ssh_host_dsa_key.pub    ssh_host_ed25519_key.pub
ssh_config        ssh_host_ecdsa_key      ssh_host_rsa_key
sshd_config       ssh_host_ecdsa_key.pub  ssh_host_rsa_key.pub
ssh_host_dsa_key  ssh_host_ed25519_key    ssh_import_id

```

## Le service SSH: Machine cliente

`ssh-keygen -t rsa`

![Figure 1 Création des clés]({{site.baseurl}}/assets/img/generate-key.png)

Notre clé publique est générée sous le dossier: `~/.ssh/id_rsa`

On va procéder a le copier au serveur en utilisant la commande suivante: 

`ssh-copy-id serveur@123.45.56.78`

![Figure 2 Copie de la clé publique]({{site.baseurl}}/assets/img/ssh-copy.png)

Maintenant, on va essayer de se connecter à la machine serveur:

`ssh 'dev@192.168.134.131'`

![Figure 3 Se connecter au remote serveur]({{site.baseurl}}/assets/img/connect.png)

On va par la suite essayer d'exploiter une faille dans cette par ouvrir une session root:

`sudo su`

On va créer un utilisateur `ensa` avec un password `ensa`

![Figure 4 Se connecter comme étant root]({{site.baseurl}}/assets/img/su.png)

Pour remédier à ce probleme, on bloque l'accée au utilisateur root en ajoutant la ligne suivante

`PermitRootLogin no`  à  `/etc/ssh/sshd_config`

`sudo service ssh restart`

![Figure 5 Blocker l'accée au Root Utilisateur]({{site.baseurl}}/assets/img/noroot.png)

Pour authoriser ou bloquer seulement quelques utilisateurs, on va modifier une autre fois le fichier `/etc/ssh/sshd_config` et ajouter les lignes suivantes:

```
AllowUsers user1 user2
AllowGroups group1 group2
DenyUsers user1 user2
DenyGroups group1 group2
```


Pour éviter de retaper les paraphrases chaque fois qu'on veut se connecter on va utiliser les commandes suivantes:

```
ssh-agent 
ssh-add ~/.ssh/id_rsa
```

## Transfert de fichier par `scp`

Je vais utiliser `scp` pour copier mon dossier de Music au serveur

`scp -r ./Music/ dev@192.168.134.131:nouveau_music` 


## Épilogue

Dans ce TP, on a appris plusieurs choses sur la communication ssh dans une architecture client-serveur et le transfert des données entre plusieurs machines.

