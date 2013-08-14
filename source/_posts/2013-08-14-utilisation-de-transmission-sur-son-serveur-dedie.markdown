---
layout: post
title: "Utilisation de Transmission sur son serveur dédié"
date: 2013-08-14 11:20
comments: false
sharing: false
categories: [Torrent, Transmission, Auto-hébergement]
---

Transmission est un client BitTorrent connu. Je l'utilsais dejà sur mon laptop avec son interface gtk, elle est simple, intuitive et cependant complète.

Dans cette article, nous allons profiter de notre serveur dédié pour s'occuper du téléchargement et du partage des torrents. Transmission dispose d'une **interface en ligne de commande** et d'**une interface web**. Nous verrons ici comment installer transmission avec son interface web.
<!-- more -->

# Transmission-daemon

## Installation

Première chose à faire, installer transmission-daemon. Je crée en même temps un dossier dans mon home pour les téléchargements.

	# aptitude install transmission-daemon
	$ mkdir ~/torrents

Debian crée un utilisateur, `debian-tranmission`, qui s'occupe de lancer le daemon.

## Configuration

Le fichier `/etc/transmission-daemon/settings.json` s'occupe de la configuration de transmission. Pour pouvoir modifier la configuration du démon, il faut impérativement qu'il soit arrêté :

	# /etc/init.d/transmission-daemon stop

### Interface web

Pour nous permettre d'accéder à l'interface web en dehors du localhost, par exemple dans tout le réseau `192.168.0.0/16`, on le renseigne dans le fichier de configuration :

	"rpc-whitelist": "127.0.0.1, 192.168.*.*"

L'interface web dispose aussi d'une authentification.

	"rpc-username": "transmission"
	"rpc-password": "coucou"

Le mot de passe sera crypté au redémarrage.

Après avoir redémarrer le démon, l'interface web est accessible à l'adresse http://ip_serveur:9091/. N'oubliez pas d'ajouter une règle à votre firewall/routeur si nécessaire !

### Dossier de torrents

Vous pouvez aussi spécifier un autre dossier de téléchargement :

	"download-dir": "/home/user/torrents",

Il faut cependant changer les permissions de `torrents` pour permettre à l'utilisateur `debian-transmission` d'y écrire

	# chgrp debian-transmission ~/torrents/
	# chmod g+rw ~/torrents/

Voilà finalement le résultat :

{% img center /images/Transmission/transmission.png 1000 450 %}
