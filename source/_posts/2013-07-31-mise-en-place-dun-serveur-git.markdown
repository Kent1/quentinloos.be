---
layout: post
title: "Mise en place d'un serveur git"
date: 2013-07-31 22:51
comments: false
sharing: false
categories: [git]
---

Git est un logiciel de gestion de versions décentralisé. C'est un logiciel libre créé par Linus Torvalds, le créateur du noyau Linux.

Je ne vais pas m'attarder sur comment utiliser git, [d'autres](http://www.miximum.fr/tutos/1546-enfin-comprendre-git) le font mieux que moi. Cette article a pour objectif de créer rapidement et simplement un dépôt git sur un serveur, accessible depuis SSH.
<!-- more -->

## Installation de git

Commençons donc par la mise en place du serveur à proprement parler. Ici pas d'interface web, on va juste créer un serveur permettant de créer des dépôts, et les actions tel que commit, clone, push etc..

Comme git à été conçu sur un modèle décentralisé (et ça n'est pas un petit atout !), il n'y a pas vraiment de serveur. Il suffit donc d'installer git ...

    # aptitude install git git-core

... pour avoir un serveur git !

## Création d'un dépôt

Maintenant que git est installé, nous pouvons créer des dépôts, et ce très facilement !

    $ mkdir depot.git
    $ cd depot.git
    $ git init --bare

C'est tout ce qu'il y a à faire ! L'option <code>--bare</code> permet de créer un dépôt git sans répertoire de travail. À partir de la, le serveur git est fonctionnel, et peut être cloner comme ceci :

    $ git clone /path/to/depot.git

Voyons maintenant comment y accèder à travers SSH.

## Connexion au serveur par SSH

### Création de l'utilisateur git

La création d'un utilisateur git est utile pour pouvoir se connecter par SSH et disposer d'un dossier pour les dépôts git. Personnellement, j'utilise l'accès SSH mais d'autres moyens existent, comme la connexion avec HTTP ou par démon, dont je ne parlerai pas. Cette méthode est aussi intéressante pour l'installation de Gitolite, détaillé peut-être plus tard.

Donc nous allons créer un utilisateur git, **sans mot de passe**.

    # adduser --system --shell /bin/bash --group --disabled-password --home /home/git/ git

Les utilisateurs **systèmes** n'ont pas accès au shell et appartiennent au groupe _nogroup_, c'est pour cette raison que nous passons les paramètres **&#8211;shell** et** &#8211;group**. C'est dans_ /home/git/_ que seront placés nos dépôts. Beaucoup préfèrent, et je pense que c'est mieux d'un point de vue conceptuel, mettre les dépôt dans_ /var/git/_. À vous de voir donc.

### Connexion par clés

Ensuite on va configurer les clés pour SSH. Il vous faut donc une paire de clés. Pour ça, taper sur la machine avec laquelle vous vous connectez :

    $ ssh-keygen

Cela aura pour effet de créer une paire de clés, une **publique** et une **privée**. Ces clés sont créées par défaut dans _~/.ssh/_.

Ensuite on va copier notre clé publique sur le serveur, plus précisement dans_ /home/git/.ssh/authorized_keys_. On peut avoir un accès à l'utilisateur git grâce à <code>su</code>.

    # su git
    $ cd
    $ mkdir .ssh
    $ nano .ssh/authorized_keys

Et on copie notre clé publique à cette endroit.
Et voila, notre serveur refuse l'accès SSH par mots de passe, mais accepte par clés. Vous pouvez essayer de vous connecter en SSH. (Sauf si comme moi, vous oubliez d'autoriser l'accès dans _/etc/ssh/sshd_config_ et vous cherchez pendant bien 2 heures.)

Pour cloner un dépôt créé auparavant, il suffit donc de faire ceci :

    $ git clone git@quentinloos.be:depot.git

Vous aurez normalement un message d'avertissement disant que vous clonez un dépôt vide. Ça n'est pas très grave. Pour commiter la première fois, vous devez cependant faire comme ceci :

    $ touch README
    $ git add REAME
    $ git commit -m "Premier commit"
    $ git push origin master

Après vous pouvez simpement utiliser la commande **git push** sans paramètres.
