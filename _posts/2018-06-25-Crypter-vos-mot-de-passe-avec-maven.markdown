---
layout: post
title:  "Crypter vos mots de passe avec Maven"
date:   2018-06-25 15:00:00 +0200
author: Anas KHABALI
tags: maven
comments: false
---
J'ai vu beaucoup de développeurs mettre leurs mots de passe en claire dans le fichier maven settings.
Crypter un mot de passe avec maven vous prend 2 secondes de votre précieux temps, donc faites le, surtout sur des serveurs d'intégration, QA ou autres.
Commencer par créer un mot de passe principal _(Master password)_ qui servira ensuite à crypter vos mots de passe.

```
mvn --encrypt-master-password <password>
```
Ajouter ce mot de passe crypté dans le fichier _settings-security.xml_ situé dans le dossier _${user.home}/.m2_.
(créer le fichier s'il n'existe pas).

Votre fichier doit ressembler à ça :

```
<settingsSecurity>
  <master>{pMLrZwfHeCAHRDC7FEJyow06f4/+sHtbM/rHj0oNS+0=}</master>
</settingsSecurity>
```
Maintenant, utiliser la commande suivante pour crypter les mots de passe des serveurs que vous rajoutez dans le fichier _settings.xml_

```
mvn --encrypt-password <password>
```

Dans la section *servers* du fichiers _Setting.xml_ vous pouvez ajouter votre mot de passe crypté

```
<server>
    <id>myserver</id>
    <username>myUser</username>
    <password>{i9EfFtdDZEwHZZj9KLGeV9d5PfW+Xuf/shfOIU1blio=}</password>
</server>
```
Voilà :wink:

## Pour encore plus de sécurité :
Le fichier contenant le mot de passe principal peut être créer sur un autre serveurs sécurisé ou sur une clé usb appartenant uniquement
aux personnes habilités.

Imaginons que la clé usb est monté sur un volume _/Volumes/mySecureUsb/maven/settings-security.xml_
Dans ce cas là, il faut indiquer dans le fichier _${user.home}/.m2/settings-security.xml_ ou se trouve le mot de passe master comme cela.

```
<settingsSecurity>
  <relocation>/Volumes/mySecureUsb/maven/settings-security.xml</relocation>
</settingsSecurity>
```

### Bonus
Vous pouvez aussi ajouter des commentaires ou des informations supplémentaires autour de votre mots de passe crypté.

```
<server>
    <id>myserver</id>
    <username>myUser</username>
    <password>Ce mot de passe expirera le 01/12/2058  {i9EfeV9d5PfW+Xuf/shfOIU1blio=}</password>
</server>
```
