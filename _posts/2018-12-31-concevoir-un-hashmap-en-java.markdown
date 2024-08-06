---
layout: post
title:  "Concevoir une HashMap en Java"
subtitle: "Comment concevoir une HashMap sans utiliser aucune librarie ?"
image: /assets/images/2019/12/31/hashmap_fr.png
date:   2019-12-31 15:00:00 +0200
author: Anas KHABALI
tags: datastructure hashmap java
reading_time: 15
comments: false
---
**Comment concevoir une HashMap sans utiliser aucune librarie ?** 

Le but, est de comprendre comment fonctionne cette structure de données. 

Une HashMap est une structure de données permettant le stockage de pairs (clé, valeur) et surtout d'accéder plus tard à une valeur en ayant sa clé associée de façon efficace.

Pour rester simple, voici les 3 principales fonctionnalités d'une HashMap :
- `put(cle, valeur)` : ajouter **valeur** à la HashMap en l'associant à **cle** ou mettre à jour la valeur si la clé existe déjà.  
- `get(cle)` : retourner la valeur associée à cette clé ou _null_ si la clé n'existe pas dans la HashMap.
- `remove(cle)` : supprimer la clé et sa valeur de la HashMap si la clé existe.

## Exemple

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

_Vous pouvez vous arrêter ici et essayer de réfléchir par vous-même à une implémentation avant de passer directement à la solution. Ça reste un exercice intéressant_

## Implémentation
Hashmap est une structure de données commune, implémenté dans plusieurs langages de programmation. 
Par exemple, en Python, il y a `dict` et en Java la class `HashMap`. La caractéristique qui distinct le plus une HashMap c'est sa capacité à offrir un accès très rapide à une valeur associée à une clé.

>Pour implémenter cette structure de données correctement, il y a deux principales problématiques que l'on doit traiter pour avoir une implémentation efficace. 1) La conception de la fonction de hashage. 2) La gestion des collisions des hash.

1). **la fonction de hashage**: Le but de cette fonction, dans le cadre d'une HashMap, est d'attribuer à une clé une adresse dans l'espace de stockage interne de la HashMap. C'est similaire  un system qui associe un code postal à une adresse postal donnée. Ça devient évident, qu'une bonne fonction de hash doit bien distribuer les clés dans l'espace de stockage interne pour ne pas se retrouver avec la majorité des clés concentrer dans seulement quelques cases.

2). **la gestion des collision**: La fonction de hashage transforme un grand nombre de clés en un ensemble d'adresse bien limité et cela peut engendrer la génération d'adresse similaire pour des clés différentes. C'est ce qui est appelé une collision. Comme les collisions sont inévitable, c'est important d'avoir une stratégie pour les gérer.

Selon les choix effectués pour la gestion des 2 problématiques ci-dessus, on peut avoir différentes implémentations pour notre structure de données HashMap.

### Solution 1 - Modulo avec un Tableau
#### Analyse
Une des implémentations les plus intuitives, c'est d'avoir un tableau interne d'une certain *capacité* et d'utiliser le *modulo* comme fonction de hashage. Supposant que la clé est de type entier. On peut calculer l'addresse *(l'index dans le tableau)* pour stocker la valeur dans notre tableau en calculant *index* = *clé* % *capacité* 

*PS : au cas où la clé est un Objet, on peut utiliser son `hashCode` qui est de type entier egalement.*

*PS : Il est conseillé d'utiliser un entier premier (par example *2069*) pour definir la capacité du tableau pour réduire les collisions au maximum. Et bien évidemment si la taille des données est connue en avance. L'idéal, est d'avoir un tableau de cette taille là.*

**Maintenant quand 2 différentes clés sont associées au même index dans le tableau. Comment cela peut être géré ?**

Et bien la solution la plus simple est d'avoir une liste dans chaque index du tableau de la HashMap. Dedans, on ajoutera tous les éléments associés à un hash d'une clé données. Et là ça devient évident que l'accès aux données deviendra moins efficace s'il y a trop de collision, car pour trouver une valeur associé à une clé, on devra parcourir tous les éléments dans la liste contenant les éléments qui se chevauche. D'où l'intérêt d'avoir une bonne fonction de hashage.

La *list* peut aussi être une *liste chainée*. Ça ne change rien au problème de collision, mais ça limitera la mémoire utilisée pour stocker les éléments qui peuvent se chevaucher.


#### Algorithme
Pour chacune des méthodes dans la structure de données HahMap, à savoir `get()`, `put()`, `remove()`. Le plus gros du travail se résume à trouver la valeur associée à une clé et stocker dans le HashMap. 

Cette recherche peut se résumer en 2 étapes :

Pour une clé donnée, d'abord, on calcule son hash en utilisant la méthode de hashage. Le hash correspond à l'index *(adresse)* dans notre espace de stockage interne *(simple tableau)*. Avec ce hash on va être capable d'identifier la sous-liste où la valeur va être stockée.

Maintenant qu'on a trouvé la liste devant contenir la valeur, on devra simplement la parcourir pour vérifier que notre pair (clé, valeur) existe.

![alt text][hashmap]{:class="img-responsive"}


#### Java Code
Voici, une implémentation de ce qui a été décrit ci-dessus. Une lecture du code et de ses commentaires embarqué facilitera encore plus la compréhension.

{% gist ed8d1b7fbbd1b5512d17cde7cb160e49 %}

[hashmap]: {{site.baseurl}}/assets/images/2019/12/31/hashmap_fr.png "HashMap fonctionnement interne" 