---
layout: post
title:  "Git trucs et astuces - Des jolis logs"
date:   2017-04-29 16:33:00 +0200
author: Anas KHABALI
categories: git
tags: git-trucs-et-astuces
comments: true
---
__Voici un aperçu du git log classique__\\
Ce format classique prend énormément de place dans une console pour afficher quelques informations.
Et en plus, si les commentaires des commits sont bien détaillés et bien verbeux. Les logs peuvent devenir inexploitable via cette commande.
Souvent, nous aimerions accéder uniquement à la première ligne des commits dans un log puis aller voir le détail après.\\
![alt text][git_log_capture]{:class="img-responsive"}

__Et voila un aperçu de notre joli git log__\\
Ici, nous avons uniquement la première ligne de chaque commit avec le nom de l'auteur et la date en plus de la version courte du hash du commit.
Ce format rend les logs plus lisible et plus exploitable en affichant plus de commit et moins de bla bla des messages des commit.
![alt text][git_joli_log_capture]{:class="img-responsive"}

__Alors, comment obtenir ce joli aperçu des git log ?__\\
C'est très simple, il suffit de lancer cette petite commande.\\
```
git log --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit
```

:rage2: *Je vous l'accorde c'est trop long à retenir, mais merci au git alias*

Donc il suffit d'ajouter un alias pour cette commande comme suite :\\
```
git config --global alias.lg "log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit"
```

et puis pour avoir vos joli log, il vous suffit d'utiliser `git lg` maintenant

**Bonus**\\
Si vous voullez avoir les lignes changées vous pouvez utiliser `git lg -p`

Voila :bowtie:

[git_log_capture]: {{site.baseurl}}/assets/images/git_log.jpg "git log"
[git_joli_log_capture]: {{site.baseurl}}/assets/images/git_joli_log.jpg "git lg (joli log)"
