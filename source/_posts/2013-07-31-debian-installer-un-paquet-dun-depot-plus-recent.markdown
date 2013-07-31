---
layout: post
title: "Debian : Installer un paquet d’un dépôt plus récent"
date: 2013-07-31 11:57
comments: false
sharing: false
categories: ["Debian", "APT"]
---

Supposons que nous disposons d'un serveur tournant avec Debian stable. Les paquets de la stable ont pour avantage d'être moins sujets aux bugs. Cependant, dans certains cas, il est intéressant d’avoir la dernière version d’un logiciel, en particulier pour des applications jeunes ou à la mode, mais néanmoins intéressantes ! Certains paquets comme **Node.js** ou **ownCloud** ne sont même pas présent dans les dépôts **Wheezy** !

Nous allons donc spécifier à Debian qu'on souhaite que certains paquets soient installés à partir des dépôts de **Jessie**<!-- more -->

Commençons donc par ajouter les dépôts dans _/etc/apt/sources.list_

	## Jessie
	deb http://ftp.be.debian.org/debian/ jessie main non-free contrib
	deb-src http://ftp.be.debian.org/debian/ jessie main non-free contrib

Ensuite, on va utiliser une option intéressante dans APT, permettant de lui indiquer notre distribution. Ainsi, on rajoute dans _/etc/apt/apt.conf_ la ligne suivante

	APT::Default-Release "wheezy";

permettant de spécifier notre choix de version.

Enfin, il suffit d'installer notre logiciel en spécifiant le dépôt testing de cette manière :

	# aptitude install -t jessie <nom_paquet>

Avec cette configuration, la mise à jour du système s'occupera de mettre à niveau les paquets qui avaient été installés depuis le dépôt stable en utilisant le dépôt stable actuel et les paquets qui avaient été installés depuis le dépôt testing en utilisant le dépôt testing actuel.

On peut s’assurer de la bonne priorité des dépôts avec la commande :

	$ apt-cache policy