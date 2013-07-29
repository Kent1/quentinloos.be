---
layout: post
title: "Hole Punching – P2P à travers un NAT"
date: 2012-10-25 18:05
comments: false
categories: [Réseaux]
---

# Rappel

NAT, _Network Address Translation_, permet de faire la correspondance entre des adresses IP publiques et des adresses IP privées. Les machines connectées derrière un NAT ont donc une adresse IP privée au choix :

*   Classe A : 10.0.0.0/8
*   Classe B : 172.16.0.0/12
*   Classe C : 192.168.0.0/16

Le NAT s'occupe donc de remplacer les adresses privées par celles publiques et inversement.<!-- more -->

## Avantages

Un adresse IP suffit pour connecter plusieurs machines ! Les adresses IPv4 se font rares, et coûtent cher. De plus, les machines derrière un NAT ne sont pas directement accessibles, ce qui peut être un plus pour la sécurité.

## Fonctionnement

#### Datagrammes sortants

* Remplacement de l’adresse IP privée par une publique.
* Remplacement du port avec un nouveau port.

#### Table de traduction

* Stockage de la correspondance (adresse IP local, port local) et (adresse IP du NAT, nouveau port assigné).

#### Datagrammes entrants

* Remplacement de l’adresse IP du NAT par l’adresse IP privée correspondante.
* Remplacement du port par le port correspondant.

## Problématique

Deux clients souhaitent communiquer. Au minimum l’un des deux se trouve derrière un NAT. Ce dernier a donc une adresse IP locale, et donc inaccessible en dehors du réseau privé.

<span style="color: #3366ff;">**Comment contacter une machine connectée derrière un NAT ?**</span>

Outre quelques solutions telles que configurer le NAT statiquement, ou encore utiliser le protocole UPnP IGD, je vais vous présenter une méthode assez robuste et dynamique !

# Hole Punching

Signifie perforation, en effet on va essayer de faire des trous dans le NAT. Pour cela, on a besoin d'un serveur intermédiaire, qu'on va appeler S. Supposons donc deux clients A et B, connecté avec un serveur S.  Supposons que A souhaite établir une connexion avec B.

À la connexion avec S, les clients lui transmettent leur adresse IP et le port par lequel ils émettent les paquets (leur adresse IP et port privé). Le serveur S stocke cette information, ainsi que l’adresse IP et le port inscrit dans les paquets (l’adresse IP et port du NAT). Notez que si le client n’est pas derrière un NAT, alors les couples sont identiques.

Voila comment la technique du Hole Punching procède :

*   A ne sait pas directement joindre B, il demande donc de l’aide à S.
*   S répond à A avec l’adresse publique et privée de B.

    En même temps, S envoie une demande de connexion à B, avec l’adresse publique et privée de A.
*   Quand A et B reçoivent les IP venant de S, ils commencent à s’envoyer des paquets, aux adresses publiques et privées. Quand ils reçoivent une réponse valide d’une des deux adresses, les clients n’envoient plus de paquets à l’autre adresse.

Connaissant le comportement du Hole Punching, nous allons considérer plusieurs scénarios :

*   Les clients sont derrière le même NAT.
*   Les clients sont derrière deux NAT différents.
*   Les clients sont derrière deux niveaux de NAT différents.

Nous supposons d’abord que le protocole de transport est UDP

### Les clients sont derrière le même NAT.

Supposons donc que deux clients A et B, se trouvant dans le même réseau privé, veulent communiquer.

Supposons aussi que les deux clients ont une connexion UDP avec le serveur S. Le NAT a donc réservé le port public 62000 pour la connexion avec A, et le port public 62005 pour la connexion de S à B

Le client A envoie un message à S, demandant une connexion avec B. S répond à A avec l’adresse publique et privée de B, et envoie en même temps l’adresse publique et privée de A à B. Les deux clients, A et B, tentent une connexion avec les différentes

adresses IP.

Les paquets adressés aux adresses privées atteindront normalement leur destination.

Les paquets adressés aux adresses publiques peuvent atteindre leur destination, en fonction du comportement du NAT.

Parmis ces deux liens, les clients sélectionneront plus probablement le lien direct, c’est à dire à destination des adresses privées.

![](http://www.brynosaurus.com/pub/net/p2pnat/img8.png "common NAT")

### Les clients sont derrière des NAT différents

Si les clients sont derrière deux NAT différents, la connexion aux adresses IP privées reçues par le serveur S échouera; Soit la connexion n'aboutira pas, soit elle aboutira vers un mauvais client.

Par contre les connexions engendrées vers les adresses IP publiques, eux atteindront normalement leur cible !

Supposons que c'est A qui envoie un message en premier. Le paquet envoyé, traverse le NAT de A et donc le NAT de A enregistre une nouvelle session sortante. La source de ce paquet est la même que la session existante entre A et S, mais la destination est différente. À l'avenir, le NAT transmettra donc les paquets entrants de B vers A. Ça crée donc un **trou** dans le NAT. Le premier message de B fera de même pour son NAT.

Si le paquet de A arrive avant que le NAT de B soit troué, alors le paquet sera jeté. Mais sinon, les paquets de A seront transmis à B.

![](http://www.brynosaurus.com/pub/net/p2pnat/img9.png "differentNAT")

### Les clients sont derrière plusieurs niveaux de NAT différents

Supposons cette fois-ci un NAT C, déployé par un ISP pour démultiplier ses clients sur seulement quelques adresses IP. Les NAT de A et de B sont à l'intérieur du réseau crée par ce NAT C. Seulement le NAT C et le serveur S ont donc une adresse IP globalement joignable.

![](http://www.brynosaurus.com/pub/net/p2pnat/img10.png "levelNAT")

Avec la technique vue précédemment, le meilleur comportement serait que A envoie un message jusqu’au NAT de B, et que B envoie un message jusqu’au NAT de A sans passer par le NAT C. Cependant les adresses « publiques » des NAT A et B sont inconnues, puisque S ne connaît que l’adresse publique du NAT C. Les paquets doivent donc être envoyé à l'adresse publique du NAT C, la seule connue.

### Commentaires

En somme, l'UDP Hole Punching marche généralement bien. Cependant, dans le cas de plusieurs niveaux de NAT, les NAT n’implémentent pas toujours l’« haiprin translation », c’est à dire l’effet de loopback des paquets destinés à des machines locales.

De plus, comme il n’y a pas de connexion UDP, on ne peut savoir quand une communication est finie. Les NAT implémente donc le plus souvent un timer, fermant le « trou »quand il n’y a plus eu d’échange depuis un certain temps.

## TCP Hole Punching

Le fonctionnement du TCP Hole Punching reste relativement le même, mais la communication est plus complexe dans ce cas là.

### Problème de TCP

Lors de la création d'un socket, soit on se connecte à un serveur, soit on écoute dans l'attente d'une connexion entrante. Mais pas les deux. De plus, la création d'un socket sur un certain port, entraîne la réservation de ce port. Un autre socket ne peut être créé avec le même numéro de port.

Pour que la technique du Hole Punching fonctionne pour TCP, nous devons pouvoir utiliser un même port pour écouter et envoyer des paquets. Une option spéciale des sockets permet la réutilisation de port. Elle doit être active sur les sockets des clients.

### Différence UDP et TCP au niveau des sockets

Avec UDP, la technique du Hole Punching demande l’utilisation d’un seul socket

Avec TCP par contre, il faut utiliser plusieurs sockets.

*   Un socket pour la connexion avec S
*   Un socket pour écouter les connexions entrantes
*   Deux sockets pour l’envoie de messages à B (l’un pour l’adresse publque, l’autre pour l’adresse privée)

### Cas de connexion TCP

Il reste un deuxième problème : en fonction de l’ordre d’arrivée des paquets SYN plusieurs cas de figure sont possibles.

1. A envoie un SYN à B qui est dropé par le NAT de B, et B envoie après un SYN à A, qui passe à travers le NAT de A, avant que A retransmette un SYN. En fonction de l’implémentation de TCP :

*   Soit A se rappelle que le SYN reçu provient d’un hôte qu’on à essayer de contacter. Alors TCP va réutiliser le socket utilisé pour la connexion avec B.
*   Soit A ne se rappelle pas, alors le paquet va entraîner la création d’un nouveau socket.

2.A et B envoie une demande de connexion en même temps, ces demandes créent un trou dans les NAT respectifs, et traverse le NAT de leur destination.

Dans ce cas, chaque client reçoit un SYN, mais attendais un SYN-ACK. Ils répondent avec un SYN-ACK.

## Caractéristique des NAT acceptant le Hole Punching

Pour que la technique du Hole Punching soit efficace, les NAT doivent respecter quelques caracteristiques. Beaucoup de NAT satisfont ces propriétés, mais pas tous.

### NAT en cône

Le NAT doit à chaque source (adresse + port) différente du réseau privé, attribuer un port différent. C’est à dire que toute session venant de la même source correspond à une seule entrée dans le NAT.

Ça n’est pas le cas de tous les NAT, qui sont souvent pensé dans le modèle client/serveur.

Par exemple, dans le cas du UDP Hole Punching, si le NAT de A n’utilise pas le même port pour la connexion avec S et avec B, la technique échoue.

Cependant, la nouvelle entrée d’un NAT pour une source déjà présente

dans sa table, est parfois prévisible.

### Gestion des connexions TCP entrantes inconnues

Quand le NAT reçoit un SYN venant d’un inconnu, il est important que le NAT le jette silencieusement.

En effet, certains NAT réponde par un paquet RST, d’autre même par une erreur ICMP, ça n’est pas toujours fatal à la technique du Hole Punching, mais ça peut interférer.

### Scan de paquets

Certains NAT sont connus pour rechercher dans les paquets les adresses IP, et les transformer en l’adresse publique de NAT.

Cependant c’est facilement contournable, on peut par exemple utiliser le complément à 1 de l’adresse IP, ou crypter le message.

### Boucles locales du NAT

Dans le cas de plusieurs niveaux de NAT, les NAT doivent pouvoir distinguer les paquets qui doivent aller vers l’adresse publique qui est la leur, et donc renvoyer le paquet à l’interieur du réseau.

## Conclusion

Pour autant que les NAT impliqués respectent certaines propriétés, la

technique du Hole Punching, aussi bien pour l’UDP que le TCP, se révèle

stable et robuste.

#### Bibliographie

*   Peer to peer communication accross Network Adress Translator, _Bryan Ford, Pyda Srisuresh, Dan Kegel_