# La MeaooCity

La MeaooCity est l'environnement dans lequel votre agent va évoluer. Elle est représentée physiquement dans l'espace de démo.

Vous y aurez accès toutes les 5 heures pour une durée maximale de 30 minutes afin de tester à l'échelle vos développements.

La [démo finale](demo.md) aura également lieu dans la MeaooCity.

## <a name="vehicle_type"></a> Moyens de transport

Pour effectuer des déplacements, l'agent a à sa disposition 4 possibilités.

| Moyen | code technique | MeaooTime par unitè de longueur | Description |
|---|---|---|---|
| A pieds | `walk` | 00:07:00 MeaooTime | A pieds, l'agent peut emprunter toutes les routes à l'exception de la zone de l'aéroport et à condition qu'il ne soit pas dans le métro.</br>Il peut circuler dans les deux sens sur une route.</br>Il peut également traverser les parcs et jardins.</br>Il peut changer de moyen de transport à tout moment. |
| En métro | `subway` | 00:04:12 MeaooTime | A métro, l'agent peut emprunter les lignes sous-terraines à condition d'être proche d'une station de métro.</br>Il peut circuler dans les deux sens.</br>Il peut changer de moyen de transport uniquement lorsqu'il est à une station de métro.|
| A vélo | `bike` | 00:02:00 MeaooTime | A vélo, l'agent peut emprunter toutes les routes à l'exception de la zone de l'aéroport et à condition qu'il ne soit pas dans le métro et qu'il soit proche d'un vélo en free floating.</br>Il peut circuler dans les deux sens sur une route.</br>Il peut changer de moyen de transport à tout moment. |
| En voiture | `car` | 00:01:00 MeaooTime | En voiture, l'agent peut se déplacer sur toutes les routes à condition qu'une voiture soit proche de lui. A défaut, il peut en appeler une pour venir le chercher.</br>Les routes sont à sens unique.</br>Il peut changer de moyen de transport à tout moment.|


## <a name="coord"></a> Système de coordonnées

Les coordonnées dans la MeaooCity sont définies par un plan orthonormé exprimé en coordonnées cartésiennes `x` et `y`

![ ](https://upload.wikimedia.org/wikipedia/commons/thumb/0/05/2D_Cartesian_Coordinates_Fr.svg/1024px-2D_Cartesian_Coordinates_Fr.svg.png)

Les unités utilisées sont exprimées en mètres et correspondent à la localisation exacte dans la MeaooCity visible dans l'espace de démo.

## L'environnement technique

L'environnement technique de la MeaooCity est une réplique de l'environnement de développement qui vous sera mis à disposition.

Prévoyez de pouvoir changer les informations de connexion à l'écosystème lors de vos développements, à savoir les URLs de connexion aux APIs et l'adresse et les credentials du broker de message.