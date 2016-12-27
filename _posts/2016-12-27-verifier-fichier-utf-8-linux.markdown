---
layout: post
title:  "Verifier si un fichier est en UTF-8 - Linux"
date:   2016-12-27 16:22:00 +0000
author: Anas KHABALI
categories: encoding linux
---
Dernierement, j'ai eu affaire à la commande `file` pour essayer de vérifier le type d'encoding d'un fichier.

Le fichier contenait 400 lignes de 1060 caractères par ligne.

Il était généré par un programme Java en UTF-8, mais la commande `file -i nom_fichier` sur une machine 'intégration  donnait comme résultat :

```
[#]$ file -i nom_fichier 

[#]$ text/plain; charset=us-ascii 
```

Après une petite heure de recherche, je suis tombé sur le bug [https://bugs.gw.com/view.php?id=533](https://bugs.gw.com/view.php?id=533)

Apparemment, la commande file ne scanne pas le fichier en entier pour determiner sont encoding mais il effectue uniquement une supposition en se basant sur une partie du fichier.
Ce qui la rend pas fiable pour ce genre de vérification.

Ce bug a été corrigé dans la version 5.28 en ajoutant l'option -P qui permet de passer le nombre de byte à scanner pour identifier l'encoding du fichier.
Mais comme la machine virtuelle que j'utilise a encore la version 5.04 de la commande file, j'étais obligé de trouver une solution de contournement.
Une des solutions pour vérifier si un fichier est en utf-8 est de faire une conversion avec la commande 'iconv' du fichier de l'UTF-8 vers l'UTF-8 ou UTF-16 et vérifier le code sortie de la commande `echo $?` qui doit être égale à zéro si le fichier est bien en UTF-8.  

```
[#]$ iconv –f utf-8 –t utf-8 nom_du_fichier &> /dev/nulll

[#]$ echo $?  # voir si c’est égale à 0
```

**Dans un script bash cela peut être utilisé comme ça :**

```
iconv -f UTF-8 -t UTF8 nom_fichier
if [ "$?" -ne 0 ];then
  echo "[FATAL] Le fichier n'est pas en UTF-8 : nom_fichier"
fi
```

**Pour trouver tous les fichiers non UTF-8 dans un dossier par exemple :**

```
find . -type f | xargs -I {} bash -c "iconv -f utf-8 -t utf-16 {} &>/dev/null || echo {}" > utf8_fail
```

**Attention cette commande remonte aussi les fichiers binaires.**


