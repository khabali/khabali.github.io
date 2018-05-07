---
layout: post
title:  "Comment convertir un InputStream en String en Java 8"
date:   2018-05-07 10:24:00 +0100
author: Anas KHABALI
tags: java java-8
comments: false
---
Voici, pour moi, la façon la plus simple pour convertir un InputStream en String en Java 8.

```
final InputStream is = votre InputStream ...
final String string = new BufferedReader(new InputStreamReader(is))
                          .lines().collect(joining("\n"));
```

Penser bien sûr à fermer votre stream après :wink:
