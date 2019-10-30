# Votre terrain de jeu

Cette section détailles les concepts de base à comprendre pour passer un bon moment dans ce hackathon.

## <a name="ville"></a> La ville

Votre objectif est de fournir à un agent secret une application qui va l'aider à se déplacer dans une ville en déjouant les aléas de circulation qui peuvent survenir à tout moment.

![en orange, la zone de l'aéroport](assets/meaoocity.png "en orange, la zone de l'aéroport")

Pour se déplacer dans la ville, votre agent pourra se mouvoir à pieds ou emprunter un des autres moyens de transport mis à disposition par la municipalité :
* Un métro
* Un parc de vélos en libre service
* Des taxis autonomes (robotaxi)

### <a name="vehicle_type"></a> Moyens de transport

L'agent a à sa disposition 4 moyens de transport.

| Moyen | code technique | [MeaooTime](concepts.md#meaootime) par unité de longueur | Description |
|---|---|---|---|
| A pieds | `walk` | 00:07:00 [MeaooTime](concepts.md#meaootime) | A pieds, l'agent peut emprunter toutes les routes à l'exception de la zone de l'aéroport et à condition qu'il ne soit pas dans le métro.</br>Il peut circuler dans les deux sens sur une route.</br>Il peut également traverser les parcs et jardins.</br>Il peut changer de moyen de transport à tout moment. |
| En métro | `subway` | 00:04:12 [MeaooTime](concepts.md#meaootime) | A métro, l'agent peut emprunter les lignes sous-terraines à condition d'être proche d'une station de métro.</br>Il peut circuler dans les deux sens.</br>Il peut changer de moyen de transport uniquement lorsqu'il est à une station de métro.|
| A vélo | `bike` | 00:02:00 [MeaooTime](concepts.md#meaootime) | A vélo, l'agent peut emprunter toutes les routes à l'exception de la zone de l'aéroport et à condition qu'il ne soit pas dans le métro et qu'il soit proche d'un vélo en free floating.</br>Il peut circuler dans les deux sens sur une route.</br>Il peut changer de moyen de transport à tout moment. |
| En voiture | `car` | 00:01:00 [MeaooTime](concepts.md#meaootime) | En voiture, l'agent peut se déplacer sur toutes les routes à condition qu'une voiture soit proche de lui. A défaut, il peut en appeler une pour venir le chercher.</br>Les routes sont à sens unique.</br>Il peut changer de moyen de transport à tout moment.|

Pour piloter ces moyens de transport nous avons mis en place une [commande de déplacement](command.md#move).

> **ATTENTION** : l'objectif de ce hackathon est de fournir une application qui conseille l'utilisateur pendant le parcours (et lancer les commandes de déplacement en fonction du choix de l'utilisateur). L'objectif n'est pas de développer un algorithme de déplacement optimal d'un pion sur une carte. L'expérience utilisateur est la clé.

### <a name="coord"></a> Système de coordonnées

Les coordonnées dans la ville sont définies par un plan orthonormé exprimé en coordonnées cartésiennes `x` et `y`, l'origine (0,0) se trouve dans le coin en bas à gauche de la ville:

![Un plan orthonormé](https://upload.wikimedia.org/wikipedia/commons/thumb/0/05/2D_Cartesian_Coordinates_Fr.svg/400px-2D_Cartesian_Coordinates_Fr.svg.png "Un plan orthonormé")

Les unités utilisées sont exprimées en mètres et correspondent à la localisation exacte dans la MeaooCity visible dans l'espace de [démo](demo.md).

## <a name="map"></a> La MeaooMap

La MeaooMap est l'application qui vous permet de visualiser le terrain de jeu dans lequel votre agent va évoluer. Vous pouvez y accéder sur `http://control-tower.[NAMESPACE].xp65.renault-digital.com` (remplacez `[NAMESPACE]` par votre identifiant de namespace).

La MeaooMap affiche non seulement la ville mais également le [MeaooTime](concepts.md#meaootime) consommé par votre agent ainsi que les aléas de circulation et le [contexte extérieur](context.md).

## <a name="city"></a> La MeaooCity

La MeaooCity est la matérialisation physique de la ville, elle mesure 22m x 6m. 
Les routes sont matérialisées par des rubans de led qui réagissent aux événements de l'écosystème. 

La matérialisation physique permet de faire le lien entre le monde digital, et notre monde réel.

La taille de la ville permet quant à elle de faire circuler la [MeaooCar](car.md).

## <a name="meaootime"></a> Le MeaooTime

Les déplacements dans la ville sont chronométrés à l'aide d'une unité de mesure appelée **MeaooTime**. Il s'agit d'une unité de temps virtuelle destinée à représenter le temps que l'agent passe dans les transports. L'unité de mesure est la minute.

Afin de ne pas passer des heures devant l'écran (surtout si l'agent se déplace à pieds), ce temps s'écoule de manière plus ou moins rapide.

Exemple : un piéton qui avance d'une certaine distance prends plus de temps que si cette même distance était parcourure en voiture. Le temps va donc augmenter de manière significative pour le piéton, et moindre pour la voiture.


