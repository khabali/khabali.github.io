---
layout: post
title:  "Git trucs et astuces - effectuer un rebase"
date:   2017-03-12 13:00:00 +0100
author: Anas KHABALI
categories: git
tags: git-trucs-et-astuces
comments: true
---
>Vous trouverez içi quelques trucs et astuces pour git.\\
j'essaierais, dans une série de postes, de fournir une liste de commandes utiles pour le développeur dans certain sénario classique.

__C'est quoi un rebase de branche ?__\\
Un rebase consiste à synchroniser l'arbre des commits d'une branche avec l'arbre d'une autre branche, souvent la branche principale *master*.\\
Cette action permet de garder un historique propre de notre répositorie git pour retrouver les modifications effectuer et surtout les raison de ces modifications.

Dans un workflow minimaliste d'utilisation de git. nous créons une branche pour réaliser une nouvelle fonctionnalité ou pour corriger une anomalie. une fois notre développement terminé, revue, testé et jugé stable nous pouvons fusionner (*merge*) notre branche avec la branche principal *master*

 Avant de fusionner notre branche, des fois un **rebase** de cette branche nous permet de récuperer les autres commits effectuer sur *master* pour corriger les conflits ou juste pour rester à jours avec la branche master.

![alt text][branche]  **=> après le rebase =>** ![alt text][branche-rebase]

Pour cela, il faut retrouver le point de rebase, (*le commit*), entre les deux branche. grâce a cette commande
`git merge-base ma-branch master`

Dans l'image ci-dessus la base du merge entre les branches *my-branch* et *master* est le commit **b**

une fois la base du merge trouver *(le hash du commit retourné par la commande précédente)* on peut commencer le rebase grâce a cette commande
`git rebase -i ${HASH}`

**${HASH}** doit être remplacer le hash du commit qui sert de base au rebase, les 6 premiers charachtères sont suffisant pour lancer la commande *exemple : abc123*
`git rebase -i abc123`

>Si vous connaissez le nombre de commit que vous vouliez rebaser, vous pouviez utiliser la commande
`git rebase -i HEAD~n` où **n** est le nombre de commit.

Après avoir lancer la commande *git rebase -i ${HASH}* git ouvre la liste de commit de votre branche a rebaser dans votre éditeur de logiciel préféré

>Pour définir votre éditeur préféré utilisé par git
utiliser la commande `git config --global core.editor "chemin vers notepade++ par example"`

```
pick 1fc6c95 correction bug 2215
pick 6b2481b fonctionalité utile
pick c619268 mise a jour de la java doc
```

a ce stade du rebase vous pouvez fusionner des commits ou changer leurs messages

**Fusionner les commits de la branche my-branch**

Dans le fichier qui liste vos commits vous pouvez remplacer le mot clé *pick* avec *squash* pour fusionner les commits

```
pick 1fc6c95 correction bug 2215
squash 6b2481b fonctionalité utile
squash c619268 mise a jour de la java doc
```
Sauvegarder le fichier et fermer l'éditeur.

**Changer le message de certains commits**

Pour changer le messages d'un commit, utilisez le mot clé *reword*

```
reword 1fc6c95 j'ai fait un truc
pick 6b2481b fonctionnalité 1: une fonnctionnalité importante
```

sauvegarder et fermer le fichier. un fichier s'ouvre et vous permet de changer le message de votre commit.
changez le sauvegardez et fermez à nouveau le fichier

```
1fc6c95 fonctionnalité 2: description de la fonctionnalité
6b2481b fonctionnalité 1: une fonnctionnalité importante
```

Effectuer le rebase en lancer la commande :

`git rebase origin/master`

**Récapitulatif des commandes**

```
git merge-base my-branch master
git rebase -i ${HASH}
git rebase origin/master
```

[branche]: {{site.baseurl}}/assets/images/git-branch.png "structure du repo git"
[branche-rebase]: {{site.baseurl}}/assets/images/git-rebase.png "structure du repo git après le rebase"
