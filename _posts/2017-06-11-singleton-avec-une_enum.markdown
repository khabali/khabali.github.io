---
layout: post
title:  "Design pattern - Singleton implémenté avec une Enum"
date:   2017-06-11 17:45:00 +0200
author: Anas KHABALI
tags: Design-pattern Singleton
comments: false
---

Voici la plus simple et efficace implémentation d'un Singleton comme décrite par [Joshua Bloch](https://en.wikipedia.org/wiki/Joshua_Bloch "wiki page of Joshua Bloch") en utilisant une enum au lieu d'une classe avec un constructeur privé et une méthode statique pour crée l'instance.

```
public enum MonSingleton {

	INSTANCE;

	private MaClass mClass = new MaClass();

	public MaClass get() {
		return mClass;
	}

	/**
	 * la définition de la class
	 */
	private final class MaClass {
      // vos attributs et méthodes...
	}
}
```

Et à l'utilisation ça donne :

```
MonSingleton.INSTANCE.get();
```    

Voilà
