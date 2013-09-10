---
layout: post
title: "Utiliser les PPA dans Debian"
date: 2013-08-03 16:52
comments: false
sharing: false
categories: Debian
---

Cette article fait suite à l'article <a href="{{ root_url }}/blog/2013/07/31/debian-installer-un-paquet-dun-depot-plus-recent/">Debian : Installer un paquet d'un dépôt plus récent</a>.

Les PPA d'ubuntu sont des dépôts d'utilisateurs qui créent des paquets de certains logiciels. Prenons le cas de [Sublime Text](http://sublimetext.com). Ce logiciel ne se trouve pas dans les dépôt Debian, n'étant pas un logiciel libre. Et bien il existe un PPA pour Sublime Text, que nous allons importer dans notre Debian ! <!-- more -->

Pour cela, commençons à installer deux petits programmes qui contiennent une commande d'ajout de dépôt :
	# aptitude install python-software-properties software-properties-common

Ainsi, pour ajouter le PPA pour Sublime Text, nous exécutons simplement :

    # apt-add-repository ppa:webupd8team/sublime-text-2

Ensuite il suffit d'aller modifier la source APT nouvellement créée, et y adapter la version, par exemple Ubuntu Raring en place de Debian sid.

    # cat /etc/apt/sources.list.d/webupd8team-sublime-text-2-sid.list
    deb http://ppa.launchpad.net/webupd8team/sublime-text-2/ubuntu raring main
    deb-src http://ppa.launchpad.net/webupd8team/sublime-text-2/ubuntu raring main
