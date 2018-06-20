---
layout: post
title:  "Mocker une API REST via un proxy HTTP"
date:   2018-06-20 14:00:00 +0200
author: Anas KHABALI
tags: test mock
comments: false
---
### C'est quoi un mock et pourquoi nous en avons besoin ?
Un mock est un système ou logiciel ou tout simplement un Objet capable de simuler et d'imiter le comportement d'un vrai système.
Les mock sont souvent utilisés pour développer des tests unitaires qui couvre la logique de notre code d'une manière simple et rapide.
Le but des tests unitaires est souvent de garantir que le code se comporte comme prévu.
Ils permettent aussi de s'assurer qu'il n'y a pas de régression lorsque nous corrigeons
un bug ou ajoutons des fonctionnalités ou retravaillons le code.

Le développement des tests unitaires n'est pas souvent une tache facile, car souvent, notre code doit interagir
avec des système complexe et lourds, des API externes qui sont des fois payantes.
Une multitude d'outils et de Framework existe pour créer des mock, mais ça nécessite souvent plus de développement
(plus de travail et de code à maintenir) pour créer des mocks.

Imaginons si on avait un système capable de capturer et de reproduire le comportement d'un autre système.
Cela facilitera la création des mockes et ainsi le développement des tests,
car nous allons pouvoir exécuter les mêmes tests en interagissant avec le vrai système ou avec le système de mockes
sans absolument rien changer dans notre code de tests.

Ainsi nous pourrions par exemple lancer nos tests en dev en utilisant le système mocké ou en intégration en utilisant le vrai système.   

### Mocker une API REST avec un Proxy HTTP
Cela peut bien sûr aussi s'appliquer à n'importe quel système basé sur le protocole HTTP.

En regardant de près la définition d'un proxy :

> un composant qui joue le rôle d'intermédiaire en se plaçant entre deux hôtes
pour faciliter ou surveiller leurs échanges. - Wikipedia

![Diagramme montrant un proxy](https://upload.wikimedia.org/wikipedia/commons/b/bb/Proxy_concept_en.svg "Simple Proxy")

En se basant sur cette définition, il est clair que nous pouvons utiliser un proxy
HTTP pour capturer les requêtes sortantes de notre code de tests ainsi que les réponses de l'API que nous consommons.
Le même proxy peut être configurable pour fonctionner comme un passe plat en capturant les requêtes/réponses
ou en fonctionnant dans un mode simulation en essayant de construire les réponses aux requêtes qu'il reçoit à partir
des requêtes/réponses préalablement capturées.

Cela ressemble à un *Men in the middle*, mais cette fois, pour un objectif noble :wink:

Je vais faire une suite à ce post avec une simple implémentation montrant le concept en Java.
