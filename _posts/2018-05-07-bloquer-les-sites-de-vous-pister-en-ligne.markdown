---
layout: post
title:  "Bloquer un site web !"
date:   2018-05-07 17:44:00 +0200
author: Anas KHABALI
tags: privacy
comments: false
---
### Pourquoi ?
Aujourd'hui, quasiment tous les sites web ont une intégration avec les outils des géants du web,
comme Google ou Facebook ..., en utilisant leurs boutons de connexion, de partage de contenu
ou d'autres outils d'analytique ou de publicité. Ce qu'il fait que vous êtes pister partout ou vous
êtes sur internet. Vous pouvez aussi avoir vos propres raisons pour bloquer des sites web sur votre machine :wink:

### Comment faire ?
Si vous souhaitez préserver vos données pour qu'elles ne soient pas récupérées par des tiers
que vous ne connaissez pas ou pour d'autres raisons personnelles. Il existe une solution assez simple.
Pour cela, vous ajoutez ces adresses à votre fichier hosts.

* Sous Windows le fichier se trouve dans `C:\Windows\System32\drivers\etc\hosts`
* Sous linux c'est le fichier `/etc/hosts`

Dans le fichier `hosts` vous redirigez les sites à bloquer vers votre adresse local `127.0.0.1`

```
127.0.0.1     connect.facebook.net
127.0.0.1     google-analytics.com www.google-analytics.com
127.0.0.1     platform.twitter.com
127.0.0.1     googleadservices.com www.googleadservices.com
```
Et voilà, maintenant toutes les requêtes vers ces sites web ne dépasseront pas votre ordinateur et son réseau local.

### Trouver les sites qui vous pistent quand vous visitez vos sites favoris
Une fois vous êtes sur votre site favori, vous ouvrez la console de dev (souvent avec le raccourcis F12)
Dans l'onglet Réseau (Network), vous repérez les adresses des sites qui vous pistent ou simplement les adresses
que vous désirez bloquer. Puis vous les ajoutez à votre fichier `hosts`.

Comme ça vos données ne voyageront plus plus loin que vous :airplane:
