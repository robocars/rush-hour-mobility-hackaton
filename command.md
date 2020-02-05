# Les commandes

Les commandes sont des événements à publier sur le broker de messages. Ils sont écoutés par l'écosystème Moeaoo et interprétés pour réaliser les actions associées.

## Commandes disponibles sur votre environnement meaoo de dev

Ces commandes vous sont fournies afin de faciliter vos développements et vos tests. Elles ont un impact direct sur l'écosystème et permettent de manipuler [le contexte](context.md), l'agent, ou le graphe par exemple. 
Lors de la démo finale, ou pendant vos tests sur la MeaooCity réelle, ces commandes ne seront pas acceptées et vous retourneront au mieux un message d'erreur, au pire rien.

### <a name="reset"></a> Reset

> **Protocol** : MQTT  
> **Topic** : `[ENVIRONMENT]/prod/city/reset`  
> **QoS** : `0`  
> **Description** : permet de réinitialiser le MeaooTime de l'agent

Payload : vide

### <a name="teleport"></a> Téléporter l'agent

> **Protocol** : MQTT  
> **Topic** : `[ENVIRONMENT]/prod/user/path-to-target`  
> **QoS** : `0`  
> **Description** : téléporte l'agent à un endroit de la map. Permet de tester les cas que l'on souhaite

Payload :
```json
{
    "vehicle_type": "walk",
    "path": [
        [21, 5.6],
        [20.9, 5.6]
    ],
    "costs": [0.0, 0.0]
}
```

|champ|description|
|---|---|
|`vehicle_type`|Indique le moyen de transport avec lequel déplacer l'agent. Ce champ peut prendre n'importe quelle valeur spécifiée dans la colonne *code technique* des [moyens de transport](concepts.md#vehicle_type). Au final la valeur peut rester fixe car elle n'a pas d'incidence sur la téléportation |
|`path`|doit contenir impérativement un array de deux valeurs, chacune d'entre elle étant un array contenant une position x et y. Le second array contient l'endroit où téléporter l'agent (dans notre cas x=20.9 et Y=5.6. Le premier array est la même position décalée de +/- 0.1 sur x ou sur y |
|`cost`|doit impérativement contenir deux valeurs à 0 (donc à ne pas modifier) |

Note : cette commande peut également être utilisée pour téléporter un taxi. Il suffit de remplacer `walk` par `car` dans le payload json.

### <a name="circulation"></a> Changer les conditions de circulation

> **Protocol** : MQTT  
> **Topic** : `[ENVIRONMENT]/prod/city/morph/traffic_conditions`  
> **QoS** : `0`  
> **Description** : modifie les conditions de circulation dans la ville (n'affecte que le déplacement en voiture)

Payload :
```json
[
	{"road": "edge_67", "slowing_factor": 3},
	{"road": "edge_68", "slowing_factor": 3}
]
```

|champ|description|
|---|---|
|`road`|identifiant du segment de route sur lequel appliquer le niveau de fluidité souhaité du traffic. Ce champ correspond aux `id` des `edges` des [fichiers de graphe de la ville](api.md#fichiers_graph).|
|`slowing_factor`|facteur de fluidité de la voie de ciculation concernée. Plage de valeurs:[1,10], ce sont des entiers, 1 = traffic fluide, 10 = traffic 10x plus lent|

### <a name="fermerouvrirmetro"></a> Fermer/ouvrir une ligne de métro

> **Protocol** : MQTT  
> **Topic** : `[ENVIRONMENT]/prod/city/morph/lines_state`  
> **QoS** : `0`  
> **Description** : ferme ou ouvre une section de ligne de métro. Les lignes étant bi directionnelle, une statut est valable pour 1 sens de circulation. Il est donc possible de fermer seulement le sens de circulation A->B et pas B->A

Payload :
```json
[
  {"line": "edge_6", "state": "close"},
  {"line": "edge_12", "state": "close"}
]
```

|champ|description|
|---|---|
|`line`|identifiant du segment de route sur lequel appliquer l'état indiqué dans "state". Ce champ correspond aux `id` des `edges` des [fichiers de graphe de la ville](api.md#fichiers_graph).|
|`state`|`close` ou `open`|

### <a name="fermerouvrirmetro"></a> Fermer/ouvrir une route

> **Protocol** : MQTT  
> **Topic** : `[ENVIRONMENT]/prod/city/morph/roads_status`  
> **QoS** : `0`  
> **Description** : Description ; ferme ou ouvre une route pour un moyen de transport donné. Certains moyens de transport routiers étant bi directionnels, un statut = 1 sens de circulation. Il est donc possible de fermer seulement le sens de circulation A->B et pas B->A

Payload :
```json
[
  {
      "car": [
          {"road": "edge_54", "state": "close"}
      ]
  },
  {
      "bike": [
          {"road": "edge_12", "state": "close"}
      ]
  },
  {
      "walk": [
          {"road": "edge_32", "state": "close"}
      ]
  }
]
```

|champ|description|
|---|---|
|`car`|tableau regroupant les voies "open" et "close" pour les robotaxis|
|`bike`|tableau regroupant les voies "open" et "close" pour les vélos|
|`walk`|tableau regroupant les voies "open" et "close" pour les déplacements à pieds|
|`road`|identifiant de la voie de ciculation concernée telle que répertoriée dans l'api graph|
|`state`|état de la voie de ciculation concernée (`open` ou `close`)|

## Commandes disponibles sur l'environnement meaoo de démo

*Ces commandes sont les seules qui seront acceptées lors de la démo par l'écosystème*.

### <a name="move"></a> Déplacer l'agent

> **Protocol** : MQTT  
> **Topic** : `[ENVIRONMENT]/prod/user/path`  
> **QoS** : `0`  
> **Description** : déplace l'agent depuis sa position courante vers une cible, en utilisant le moyen de transport indiqué <u>et le chemin le plus rapide</u>.

Payload :
```json
{
  "vehicle_type": "subway",
  "target": {"x": 4.4, "y": 0.2}
}
```

|champ|description|
|---|---|
|`vehicle_type`|Indique le moyen de transport avec lequel déplacer l'agent. Ce champ peut prendre n'importe quelle valeur spécifiée dans la colonne *code technique* des [moyens de transport](concepts.md#vehicle_type) |
|`target`|Position cible vers laquelle déplacer l'agent (exprimée dans le [système de coordonnées de la MeaooCity](concepts.md#coord)). Si la cible n'est pas sur une route ou sur une ligne de métro, le système trouve de lui-même le carrefour ou la station de métro la plus proche et l'utilise comme cible.|

#### Cas particulier du RoboTaxi (vehicle_type=car)

Le robotaxi est le seul moyen de transport que vous pouvez déplacer lorsque vous n'êtes pas à son bord. 

Comment ça marche ?

* Si la position de votre agent correspond à la position d'un RoboTaxi, la commande de déplacement `vehicle_type=car` sera considérée comme une demande de déplacement de l'agent avec le robotaxi.
* Si la position de votre agent ne correspond pas à la position d'un RoboTaxi, la commande de déplacement `vehicle_type=car` déplacera un RoboTaxi (vide) vers `target`.

Cette mécanique permet d'appeler un RoboTaxi à un point de pickup défini et en même temps de déplacer votre agent vers ce point de pickup (pour gagner du temps par exemple, comme lorsque vous demandez à votre Uber de venir vous chercher au carrefour le plus proche plutôt qu'au pied de votre immeuble pour lui éviter les petites rues)

Exemple :

```java
// déplace l'agent vers une cible et déplace une voiture vers la même cible
publish '{"vehicle_type": "bike","target": {"x": 21.8, "y": 4.0}}' to '$ENVIRONMENT/prod/user/path'
publish '{"vehicle_type": "car","target": {"x": 21.8, "y": 4.0}}' to '$ENVIRONMENT/prod/user/path'

// attendre que l'agent et la voiture aient atteint la cible 
...

// déplacer l'agent (en voiture) vers une nouvelle cible
publish '{"vehicle_type": "car","target": {"x": 14., "y": 2.0}}' to '$ENVIRONMENT/prod/user/path'
```

#### Raisons pour lesquelles la commande pourrait ne pas fonctionner

* La commande n'est pas envoyée sur le bon topic
* Le payload n'est pas bien formatté.
* Les [règles de changement de transport](concepts.md#vehicle_type) ne sont pas respectées.
* La destination est égale à la position courante.
* Aucun [chemin](graph.md) ne permet d'aller à la cible.

Dans tous les cas, vous pouvez souscrire au topic `[ENVIRONMENT]/prod/user/status` qui vous renseigne sur le [statut des commandes de déplacement](events.md#status) de l'agent. 

### <a name="stop"></a> Arrêter l'agent en cours de déplacement

> **Protocol** : MQTT  
> **Topic** : `[ENVIRONMENT]/prod/user/stop`  
> **QoS** : `0`  
> **Description** : arrête l'agent pendant un déplacement.

Payload : aucun

#### Raisons pour lesquelles la commande pourrait ne pas fonctionner

* La commande n'est pas envoyée sur le bon topic
* L'agent est en pleine voie dans le métro (auquel cas il s'arrêtera à la prochaine station)