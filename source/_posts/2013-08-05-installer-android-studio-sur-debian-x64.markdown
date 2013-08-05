---
layout: post
title: "Installer Android Studio sur Debian (x64)"
date: 2013-08-05 12:18
comments: false
sharing: false
categories: [Debian, Android]
---

Voulant essayer, et pourquoi pas refaire un peu de développement Android, j'ai décidé de tester leur nouvel IDE pour Android, à savoir Android Studio. <!-- more -->
{% img right http://1.bp.blogspot.com/-u5dfSsMOMC0/UZO_5DC_W9I/AAAAAAAACM8/YCMn15HPzpE/s320/Studio_table.png 141 140%}

## Installation

Pour ce faire, j'ai téléchargé l'archive sur <a href="http://developer.android.com/sdk/installing/studio.html">ce site</a>. Une fois décompressée, on navigue dans l'arborescence pour exécuter android-studio/bin/studio.sh.

Voila le tout en commande donc :

    $ wget http://dl.google.com/android/studio/android-studio-bundle-130.737825-linux.tgz -o android-studio.tgz
    $ tar -xvf android-studio.tgz
    $ cd android-studio/bin/
    $ ./studio.sh

## Dépendances 32-bit

Une fois le tour du proprio fait, je crée un petit projet et j'essaie de le lancer sur mon smartphone en vain. En fait il manque quelques dépendances 32-bit, et petit à petit je progresse .. Voici donc la liste des paquets à installer pour pouvoir lancer votre projet :

    # aptitude install lib32ncurses5 lib32z1 lib32stdc++6

