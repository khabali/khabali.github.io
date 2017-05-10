---
layout: post
title:  "Roulette Russe pour les Devs/Ops et les sysadmin"
date:   2017-05-10 19:15:00 +0200
author: Anas KHABALI
categories: linux
tags: linux
comments: true
---
La roulette russe, ce jeu très répandu dans le monde entier, mais qui ne fait rire personne. Pour ceux qui ne connaissaient pas l'existence de ce "jeu", le but est simple. Les acteurs du jeu chargent le revolver d'une seule balle, puis le pointe sur leur front et tire tour à tour.
Si le joueur est chanceux, la balle n'est pas partie dans le cas inverse, on peut :boom:

![alt text][roulette-russe]{:class="img-responsive"}

Et si c'était possible de jouer le même jeu avec la même règle sur votre serveur Linux ou Unix **Ne jouez pas sur votre serveur de production même si la tentation peut être forte !**

Vous êtes un devs/ops/sysadmin, ce jeu est pour vous :

`[ $[ $RANDOM % 6 ] == 0 ] && rm -rf / || echo *Click*`

Ou

`[ $[ $RANDOM % 6 ] == 0 ] && rm -rf --no-preserve-root / || echo *Click*`

Les deux commandes si dessus ont une chance sur six de supprimer tous vos fichiers. Donc ne les essayez pas sur vos serveurs de production sauf si vous savez ce que vous faites ou que vous n' aimez pas votre employeur :smirk:

Mais pour le fun voici une version sans danger du jeu :

`[ $[ $RANDOM % 6 ] == 0 ] && echo '*Oh nooo*' || echo '*Click*'`

Amusez vous bien.

[roulette-russe]: {{site.baseurl}}/assets/images/2017/05/10/roulette-russe-linux.jpg "roulette russe"   
