---
layout: post
title:  "Concevoir un HashMap en Java"
date:   2019-12-31 10:10:00 +0200
author: Anas KHABALI
tags: datastructure hashmap java
comments: true
---
On va voir comment concevoir un HashMap sans utilisé aucune librarie. Le but, est de comprendre comment ça fonctionne. 
Mais d'abord, voyons ce que s'est un HashMap.

HashMap est une structure de données permetant de stocker des pairs de (clé, valeur) et surtout d'acceder plus tard à une valeur en ayant sa clé associée et cela de façon plutot efficace.

Pour rester simple on va implementer les 3 fonctionnalités principale du HashMap:

- put(cle, valeur) : ajoute _valeur_ au HashMap en la mappant a _cle_. ou met à jour la valeur si la clé existe déjà.  
- get(cle) : retourne la valeur mappé a cette clé ou _null_ si la clé n'existe pas dans la map.
- remove(cle) : supprime la cle et sa valeur du HashMap si la clé existe.

# Example

````
MyHashMap hashMap = new MyHashMap();
hashMap.put(1, 1);          
hashMap.put(2, 2);         
hashMap.get(1);             // renvoie 1
hashMap.get(3);             // renvoie null (car la clé n'existe pas)
hashMap.put(2, 1);          // met a jour la valeur existante
hashMap.get(2);             // retourne 1 
hashMap.remove(2);          // supprime la cle 2 et sa valeur
hashMap.get(2);             // renvoie null (car la clé n'existe pas) 
````

_Vous pouvez vous arretez ici et essayer de reflechir par vous même à une implementation avant de passer directement à la solution. ça reste un exircice interessant_

# Implementation
Hashmap est une structure de données commune qui est implemanté dans plusieurs langagues de programation. 
Par example, en Python vous avez le *dict* et en Java la class *HashMap*. La carachtéristique qui distinct le plus un HashMap c'est son capacité à offrir un accés très rapide à une valeur associé à une clé.

>Pour implémenter cette structure de données correctement, il y a deux principales problèmatiques que l'on doit traiter pour avoir une implémentation efficace. 1) la conception de la fonction de hashage. 2) la gestion des collisions des hash.

1). *la fonction de hashage*: le but de cette fonction, dans le cadre d'un HashMap, est d'attribuer à une clé une address dans l'espace de stockage interne du HashMap. On peut voir ça comme un system qui associe un code postale à une adresse postale donnée. Comme vous pouvez le deviner, une bonne fonction de hash doit bien distribuer les clés dans l'espace de stockage interne pour ne pas se retrouver avec la majorité des clés concentrer dans seulement quelques cases.

2). la gestion des collision: la fonction de hashage transforme le un grand nombre de clés en un ensemble d'addresse bien limité et cela peut engendrer la generation d'adresse similaire pour des clés différentes. C'est ce qu'on appel une collision. Comme les collision sont inevitable, c'est important qu'on est une stratégie pour les gerer.

Selon nos choix pour gérer les 2 problématiques ci-dessus, on peut avoir différentes implementaion pour notre structure de données HashMap.

## Solution 1 - Modulo avec un Tableau

### Analyse
Une des implémentations les plus intuituves, c'est d'avoir un tableau interne d'une certain *capacité* et d'utiliser le *modulo* comme fonction de hashage. Supposant que la clé est de type entier. On peux calculer l'addresse *(l'index dans le tableau)* pour stocker la valeur dans notre tableau en calculant *index* = *clé* % *capacité* 

*PS: au cas ou la clé est un Objet, on peut utiliser son *hashCode* qui est de type entier egalement.*

*PS : Il est conseillé d'utilisé un entier premier (par example *2069*) pour definir la capacité du tableau pour réduire les collisions au maximum. Et bien evidament si la taille des données est connue en avance. L'ideal, est d'avoir un tableau de cette taille là.*

Maintenant quand 2 différentes clées sont associées au meme index dans le tableau. comment cela peut être géré ?

Et bien la solution la plus simple est d'avoir une liste dans chaque index du tableau de la map. Dedans on ajoutera touts les elements associés à un hash d'une clé données. Et là ça devient evident que l'acces au données deviendra moins efficace s'il y a trop de collision car pour trouver une valeur associé à une clé on devra parcourir touts les elements dans la liste contenant les elements qui se chauvauche. d`ou l'interet d'avoir une bonne fonction de hashage.

La *list* peut aussi être une *liste chainée*. ça ne change rien au problème de collision mais ça limitera la memoire utilisé pour stocker les elements qui peuvent se chevaucher.


### Algorithme
Pour chacune des methodes dans la stucture de données HahMap, à savoir *get()*, *put()*, *remove()*. Le plus gros du travails se résume à trouver la valeur associé à une clé et stocker dans le HashMap. 

Cette recherche peut se résumer en 2 étapes:

Pour une clé donnée, d'abord on calcule son hash en utilisant la methode de hashage. le hash correspond à l'index *(adresse)* dans notre espace de stockage interne *(simple tableau)*. Avec ce hash on va être capable d'identifier la sous liste où la valeur va être stockée.

Maintenant qu'on a trouvé la liste devant contenir la valeur, on devra simplement la parcourir pour verifier que notre pair (clé, valeur) existe.

![alt text][hashmap]{:class="img-responsive"}


### Java Code

<script src="https://gist.github.com/khabali/ed8d1b7fbbd1b5512d17cde7cb160e49.js"></script>



[hashmap]: {{site.baseurl}}/assets/images/2019/12/31/hashmap_fr.png "HashMap fonctionnement interne" 






