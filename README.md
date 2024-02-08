# Tuto - Serveur Samba sous ubuntu server 23

Pour crée un serveur samba simple il faudra simplement suivre les commandes suivantes

## Installer Samba sous Ubuntu Server LTS 23

Mise a jour du système :

```bash
apt-get update
```

Ensuite, installez samba :


```bash
apt install samba -y
```

activer le démarrage automatique de smbd (Samba) :

```bash
systemctl enable smbd
```

## Ajouter un utilisateur pour le partage samba

Ajouter un utilisateur avec la commande ``adduser``
```bash
adduser tgojkovic
```

Bien remplacer "*tgojkovic*" par le nom de notre utilisateur

Pour que l'utilisateur puisse se connecter au partage, il faut l'autoriser dans Samba, en plus de la création au sein du système Linux. Pour cela, il faut utiliser la commande "smbpasswd" pour déclarer l'utilisateur et lui créer un mot de passe Samba

```bash
smbpasswd -a tgojkovic
```

création du groupe @partage
```bash
groupadd partage
```

On va ajouter l'utilisateur "tgojkovic" dans le groupe partage :

```bash
gpasswd -a it-connect partage
```


## Créer un partage en utilisant Samba

```bash
nano /etc/samba/smb.conf
```

```bash
[partage]
   comment = Partage de données
   path = /srv/partage
   guest ok = no
   read only = no
   browseable = yes
   valid users = @partage
   guest account = nobody
```

- ``[partage]`` : sert à spécifier le nom du partage entre "[]", c'est le nom qui devra être utilisé pour accéder au partage
- ``comment`` : description du partage
- ``path`` : chemin vers le dossier à partager, sur le serveur
- ``guest ok`` : accès invité au partage (par défaut "no"). Si vous décidez d'activer cette option, vous devez configurer l'option "guest account" qui par défaut prend la valeur "nobody".
- ``read only`` : partage accessible uniquement en lecture seule (yes ou no)
- ``browseable`` : le partage doit-il être visible ou masqué si on liste les partages du serveur avec un hôte distant (découverte réseau). La valeur "yes" permet de le rendre visible.
- ``valid users`` : spécifier les utilisateurs ou les groupes qui ont les droits d'accès au partage (les droits sur le système de fichiers doivent être cohérents vis-à-vis de cette autorisation). On précise un utilisateur avec son identifiant et un groupe avec son identifiant précédé du caractère "@". Pour indiquer plusieurs valeurs, séparez-les par une virgule.

redemmarer le service samba :

```bash
systemctl restart smbd
```

créer le répertoire :

```bash
mkdir /srv/partage
```

donner des droits :
```bash
chgrp -R partage /srv/partage/ && chmod -R g+rw /srv/partage/
```

Vous avez terminé, a présent vous pouvez le tester.

## Auteur

[![Teo GOJKOVIC](https://img.shields.io/badge/Teo_GOJKOVIC-222e45?style=for-the-badge&logo=github&logoColor=white)](https://github.com/Teo-Gojkovic)
