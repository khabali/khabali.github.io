---
layout: post
title:  "Supprimer des fichiers d'un commit git déjà effectué"
date:   2018-06-29 17:07:00 +0200
author: Anas KHABALI
tags: git
comments: false
---
ça m'arrive d'effectuer un commit contenant des fichiers non désirés par erreur.
Dans git c'est facile de réparer ce genre d'erreur.

Commencer par annuler le commit tout en gardant vos modifications, en utilisant la commande *git reset*

```
git reset --soft HEAD^   #ou git reset --soft HEAD~1
```
Cette commande permet d'annuler le dernier commit et de remettre les modifications effectuées dedans dans l'état staging.

Maintenant, enlever les fichiers non désirés du staging avec la commande suivante

```
git reset HEAD path/vers/fichiers_non_desires
```

et refaire votre commit avec les bon fichiers restant dans staging.

## Note
Pour refaire votre commit vous pouvez même réutiliser le même commit avec la commande `git commit -c ORIG_HEAD  `

Si vous avez déjà effectuer un push dans une branche distante, il vous faudra refaire un `git push -f` pour mettre à jour votre branche distante.

Voila :wink:
