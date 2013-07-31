---
layout: post
title: "Phoronix Test Suite"
date: 2012-09-19 17:47
comments: false
categories: [GNU/Linux, Benchmark]
---

{% img right http://openbenchmarking.org/css/pts-200.png "Phoronix Test Suite" %}

Pour un projet, je dois comparer les solutions de virtualisation existantes. Entre autre, un point de comparaison sont les performances. J'ai donc recours à des _benchmarks_, des stress tests, qui ont pour but de donner un score de performance de la machine.

Pour réaliser ces benchmarks, j'ai tout de suite pensé au site connu, [Phoronix](http://www.phoronix.com "Phoronix"), qui, en quelques années, c'est imposé comme la référence des tests logiciels et matériels sous GNU/Linux. J'espérais y trouver des logiciels de benchmarks, et ... j'ai été comblé ! Il se trouve que le site à lancé son propre logiciel de benchmarks, couvrant tous les aspects possibles à tester, _Phoronix Test Suite_.

Phoronix Test Suite permet donc d'effectuer des benchmarks. Il comprend des _tests_ et des _suites_ de tests, permettant ainsi de tester le cpu par des tests de compilation, la lecture/écriture sur le disque, mais aussi des tests de logiciels, comme Apache ou Nginx.<!-- more -->

### Installation

Pour installer Phoronix sur une Debian :

	# aptitude install phoronix-test-suite

ou

	# aptitude install php5-cli php5-gd
	$ wget http://phoronix-test-suite.com/releases/repo/pts.debian/files/phoronix-test-suite_4.6.1_all.deb
	# dpkg -i phoronix-test-suite_4.0.1_all.deb

Pour utiliser Phoronix Test Suite, il faut installer des tests. Pour tous les installer, par exemple, on exécute :

	$ phoronix-test-suite install universe

Phoronix Test Suite gère les dépendances lui même. Cette commande installera la suite _universe_ qui contient tous les tests.

Pour lister les suites qui existent

	$ phoronix-test-suite list-available-suites

Ensuite on peut exécuter n'importe quels tests/suites avec la commande run

	$ phoronix-test-suite run universe

Toutes les commandes peuvent être listées en tapant simplement

	$ phoronix-test-suite

Pour le reste, je vous laisse lire la doc, disponible [ici](http://phoronix-test-suite.com/?k=documentation)

[![](http://openbenchmarking.org/css/openbenchmarking.png "OpenBenchmarking.org")](http://openbenchmarking.org/ "OpenBenchmarking")

Ce site permet d'accueillir vos benchmarks. En effet, au lancement de tests avec Phoronix Test Suite, le logiciel vous demandera s'il doit uploader les résultats.

Ensuite on peut facilement comparer avec d'autres configurations matérielles par exemple.