---
layout: post
title: "Contenu mixte HTTP/HTTPS"
date: 2013-07-31 15:53
comments: false
sharing: false
categories: [SSL, HTML]
---

Le support SSL est activé sur ce site, mais lorsque je m'y rendais, Firefox me signalait qu'il avait bloqué du **contenu non sécurisé**. Le problème venait du chargement des fonts et de JQuery depuis les serveurs de Google, grâce aux lignes suivantes :

	<script src='http://ajax.googleapis.com/ajax/libs/jquery/1.7.2/jquery.min.js'></script>
	<link href='http://fonts.googleapis.com/css?family=Open+Sans:400italic,400,700' rel='stylesheet' type='text/css'>

<!-- more -->
En effet, les requêtes pour ces fichiers étant en HTTP, le contenu de la page n'était pas entièrement sécurisé.

Pour résoudre ce problème, il suffit d'utiliser des adresses relatives de ce type

	<script src="//ajax.googleapis.com/ajax/libs/jquery/1.7.2/jquery.min.js"></script>
	<link href='//fonts.googleapis.com/css?family=Open+Sans:400italic,400,700' rel='stylesheet' type='text/css'>

Ainsi, lors du chargement en HTTP de la page, les requêtes vers les serveurs de Google se feront en HTTP, mais lors du chargement de la page en HTTPS, les requêtes vers les serveurs de Google seront elles aussi en HTTPS.

Attention de bien vérifier tout de même que le contenu distant est accessible en HTTP et en HTTPS !