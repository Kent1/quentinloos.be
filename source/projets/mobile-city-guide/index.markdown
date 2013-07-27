---
layout: page
title: "Mobile City Guide"
date: 2013-07-27 14:23
comments: false
sharing: false
footer: true
---

Mobile City Guide à été fait dans le cadre de mon cours de _Génie Logiciel_ &amp; _Gestion de Projets Logiciels_ de 3ème BAC. Cette application tourne sur la plateforme _Android_ 2.2 et consiste à gérer des points d&rsquo;intérêts dans une ville, et des fonctionnalités autour de ceux-ci.

Imaginons que nous sommes pour la journée dans une ville que nous ne connaissons pas. Nous pouvons donc, grâce a notre smartphone et au logiciel, créer un itinéraire en fonction de paramètres (préférences, temps, argent, &#8230;).

Voila un rapide tour des fonctionnalité de l&rsquo;application.

### Exploration

Lors du lancement de l&rsquo;application, le mode exploration est activé. La position de l&rsquo;utilisateur est déterminé permettant ainsi que les points d&rsquo;intérêts (POIs) situés à proximité soient affichés sur la carte. Si ça n&rsquo;est pas le premier lancement de l&rsquo;application, les POIs environnants de la dernière position connu sont affichés.

Il est également possible d&rsquo;afficher ces points sous forme d&rsquo;une liste en choisissant l&rsquo;option &laquo;&nbsp;POI List&nbsp;&raquo; dans le menu.

Pour obtenir les informations d&rsquo;un point d&rsquo;intérêt, il suffit d&rsquo;appuyer dessus.

{% img /images/mobile-city-guide/map.png 300 243 Map %}

### Affichage d&rsquo;un point d&rsquo;intérêt

Cet écran présente les informations ainsi qu&rsquo;une brève description du point d&rsquo;intérêt considéré, ainsi que la durée et le prix. C&rsquo;est également sur cet écran qu&rsquo;est donné la possibilité de noter le point d&rsquo;intérêt, de l&rsquo;ajouter à l&rsquo;itinéraire courant ou encore de le marquer comme visité.

{% img /images/mobile-city-guide/info.png 182 300 Info %}

### Préférences

Accessible depuis le menu, cet écran dispose de deux catégories. D&rsquo;une part le _profil utilisateur,_ comme son nom, son âge et son sexe. Et aussi une catégorie _Carte_ permettant elle, d&rsquo;activer la vue satellite ou de modifier la distance d&rsquo;affichage des POIs environnants et le déplacement nécessaire à une mise à jour des POIs.

**Filtres**

Il est possible de n&rsquo;afficher que les POIs dont la note est supérieure à une certaine valeur ou dont le tag correspond à une sélection.

{% img /images/mobile-city-guide/filters.png 300 239 Filtres %}

### Itinéraire

L&rsquo;écran d&rsquo;itinéraire permet de consulter l&rsquo;itinéraire courant et d&rsquo;obtenir des informations dessus. Le menu propose de le supprimer ou d&rsquo;en créer un nouveau à partir de certains critères. On peut aussi simplement supprimer un POI en appuyer longtemps sur celui-ci. Pour afficher l&rsquo;itinéraire courant, il suffit d&rsquo;appuyer sur le bouton _Afficher sur la carte_ ou directement depuis la carte via le menu.

{% img /images/mobile-city-guide/itinerary.png 300 240 Itinéraire %}

### Création d&rsquo;itinéraire

L&rsquo;application peut proposer un itinéraire, il faut pour cela entrer une durée maximum ainsi qu&rsquo;un prix maximum et optionnellement une note minimum et une liste de tags