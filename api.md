# Apis

Plusieurs API sont mises à votre disposition pour obtenir des informations sur l'écosystème:

## Agent

> **Base URL** : `http://[ENVIRONMENT].xp65.renault-digital.com/api/agent`  

> **Description** : Cette API permet de récupérer les informations relatives à votre agent.

### <a name="agent"></a> Agent situation

> **Route** : `/api/user/situation/last`  
> **Méthode** : `GET`  

> **Description** : récupère la dernière situation connue de l'agent.

Réponse :
```json
{
    "vehicle_type": "bike",
    "position": {
        "x": 3.024000000000001,
        "y": 2
    },
    "total_cost": 95.83786949752295
}
```

|champ|description|
|---|---|
|`vehicle_type`|Indique le moyen de transport utilisé par l'agent (dernier ou actuel). Ce champ peut prendre n'importe quelle valeur spécifiée dans la colonne *code technique* des [moyens de transport](concepts.md#vehicle_type) |
|`position`|Position de l'agent dans le [système de coordonnées de la MeaooCity](concepts.md#coord)|
|`total_cost`|Correspond au [MeaooTime](concepts.md#meaootime), le temps virtuel passé par l'agent dans les transports. Cette valeur est exprimée en minutes.|

## <a name="context"></a> Context

> **Base URL** : `http://[ENVIRONMENT].xp65.renault-digital.com/api/context`  
> **Description** : Cette API permet de récupérer les informations relatives au contexte extérieur à la MeaooCity.

### <a name="weather"></a> Météo

> **Route** : `/api/context/weather/current`  
> **Méthode** : `GET`  

> **Description** : récupère la météo actuelle.

Réponse :
```json
{"condition":"snow"}
```

|champ|description|
|---|---|
|`condition`|Indique les conditions météo courantes. Ce champ peut prendre n'importe quelle valeur spécifiée dans la colonne *code technique* des [conditions météo](context.md#weather) |

### <a name="air"></a> Qualité de l'air

> **Route** : `/api/context/air/current`  
> **Méthode** : `GET`  

> **Description** : récupère la qualité de l'air actuelle.

Réponse :
```json
{"condition":"normal"}
```

|champ|description|
|---|---|
|`condition`|Indique la qualité de l'air actuelle. Ce champ peut prendre n'importe quelle valeur spécifiée dans la colonne *code technique* de la [qualité de l'air](context.md#air) |

## <a name="graph"></a> Graph

> **Base URL** : `http://[ENVIRONMENT].xp65.renault-digital.com/api/graph`  

### <a name="fichiers_graph"></a> Fichiers de graphe de la ville

4 fichiers json décrivent le graphe de circulation de la ville:
- `/processed/bike.json ` contient les informations de circulation des vélos
- `/processed/vehicule.json ` contient les informations de circulation des voitures
- `/processed/walk.json ` contient les informations de circulation des piétons
- `/processed/subway.json ` contient les informations de circulation des métros

Ces fichiers sont au format JSON Cytoscape

```json
{
    "elements":{
        "nodes":[],
        "edges":[]
    }
}
```

|champ|description|
|---|---|
|`nodes`|intersections dans la ville. Dans le cas particulier du métro, les nodes sont également des stations de métro. Attention dans ces fichiers, les positions sont exprimées en milimètres, alors que l'écosystème Meaoo a pour unité de mesure le mètre.|
|`edges`|routes reliant les intersections. Ces routes sont dirigées, c'est à dire qu'elles correspondent à un sens de circulation seulement. Il est donc possible d'avoir un ou deux edges entre deux nodes A et B s'il s'agit ou non d'un sens unique. Le champ `id` des objets de ce tableau peut vous intéresser pour fermer ou ouvrir des routes avec nos APIs de dev.|

> **ASTUCE** : vous pouvez avoir une représentation graphique de ces fichiers en utilisant [Graphy](https://lukaszkostrzewa.github.io/#)


### <a name="fichiers_voies"></a> Fichiers de description des voies de circulation

3 fichiers json décrivent les voies de ciculation de la ville:
- `/processed/neighbours.json ` contient des informations permettant de dessiner les routes (pour les voitures)
- `/processed/subway_neighbours.json` contient des informations permettant de dessiner les lignes de métro
- `/processed/walk_neighbours.json` contient des informations permettant de dessiner les chemins piétonniers

Ces 3 fichiers ont la même structure:
```json
{
    "start": { "x": 12.8, "y": 5.6 },
    "neighbors": [
        { "x": 14.6, "y": 5.6 }
    ]
}
```

|champ|description|
|---|---|
|`start`|point sur la carte représentant l'extrémité A d'un segment dans le [système de coordonnées de la MeaooCity](concepts.md#coord)|
|`neighbors`| tableau de points  dans le [système de coordonnées de la MeaooCity](concepts.md#coord) représentant la ou les extrémités d'un ou plusieurs segment ayant pour origine le point A|

Exemple:
```json
A------B
 \
  \
   \
    C

{
    "start": { A },
    "neighbors": [
        { B },
        { C }
    ]
}
```

### <a name="cout_velo"></a> Calcul de coût pour un trajet à vélo

> **Endpoint** : `/road_graph/shortest_path/bike`  
> **Méthode** : `POST`  
> **Content-Type** : `application/json`

> **Description** : permet de calculer le trajet le plus optimal du point `departure` vers le point `arrival`. 

Payload :
```json
{
    "departure": { "x":15.79, "y":2.0 },
    "arrival": { "x":11.0, "y":2.0 }
}
```

|champ|description|
|---|---|
|`departure`|Position de départ (dans le [système de coordonnées de la MeaooCity](concepts.md#coord)) du trajet à calculer.|
|`arrival`|Position d'arrivée (dans le [système de coordonnées de la MeaooCity](concepts.md#coord)) du trajet à calculer.|

Réponse:
```json
{
    "cars": 
    [
        {
            "id": "bike", 
            "paths": 
            [
                [15.79, 2.0], 
                [14.6, 2.0], 
                [12.8, 2.0], 
                [11.9, 2.0], 
                [11.0, 2.0]
            ], 
            "costs": 
            [
                0.0,
                3.599999999999998, 
                3.599999999999998, 
                1.8000000000000007, 
                1.8000000000000007
            ], 
            "path_length": 10.799999999999997
        }
    ]
}
```

Remarque: le membre `cars` de la réponse est un héritage, nous devrions retrouver un membre plus parlant à la place. Il peut être iignoré.

|champ|description|
|---|---|
|`id`|Moyen de transport qui a été calculé.|
|`paths`|coordonnées (dans le [système de coordonnées de la MeaooCity](concepts.md#coord)) des points composant le chemin identifié par l'api comme étant le plus optimal. Ces coordonnées sont exprimées dans un tableau dont l'indice `0` est la composante `x` et l'indice `1` est la composante  `y`.|
|`costs`|[MeaooTime](concepts.md#meaootime) associé aux segments du `paths`|
|`path_length`|[MeaooTime](concepts.md#meaootime) complet du déplacement sur le paths (égal à la somme des `costs`)|

### <a name="cout_metro"></a> Calcul de coût pour un trajet en métro

> **Endpoint** : `/road_graph/shortest_path/subway`  
> **Méthode** : `POST`  
> **Content-Type** : `application/json`

> **Description** : permet de calculer le trajet le plus optimal du point `departure` vers le point `arrival`. 

Payload :
```json
{
    "departure": { "x":15.79, "y":2.0 },
    "arrival": { "x":11.0, "y":2.0 }
}
```

|champ|description|
|---|---|
|`departure`|Position de départ (dans le [système de coordonnées de la MeaooCity](concepts.md#coord)) du trajet à calculer.|
|`arrival`|Position d'arrivée (dans le [système de coordonnées de la MeaooCity](concepts.md#coord)) du trajet à calculer.|

Réponse:
```json
{
    "cars": 
    [
        {
            "id": "subway", 
            "paths": 
            [
                [15.79, 2.0], 
                [14.6, 2.0], 
                [12.8, 2.0], 
                [11.9, 2.0], 
                [11.0, 2.0]
            ], 
            "costs": 
            [
                0.0,
                3.599999999999998, 
                3.599999999999998, 
                1.8000000000000007, 
                1.8000000000000007
            ], 
            "path_length": 10.799999999999997
        }
    ]
}
```

Remarque: le membre `cars` de la réponse est un héritage, nous devrions retrouver un membre plus parlant à la place. Il peut être iignoré.

|champ|description|
|---|---|
|`id`|Moyen de transport qui a été calculé.|
|`paths`|coordonnées (dans le [système de coordonnées de la MeaooCity](concepts.md#coord)) des points composant le chemin identifié par l'api comme étant le plus optimal. Ces coordonnées sont exprimées dans un tableau dont l'indice `0` est la composante `x` et l'indice `1` est la composante  `y`.|
|`costs`|[MeaooTime](concepts.md#meaootime) associé aux segments du `paths`|
|`path_length`|[MeaooTime](concepts.md#meaootime) complet du déplacement sur le paths (égal à la somme des `costs`)|

### <a name="cout_metro"></a> Calcul de coût pour un trajet à pieds

> **Endpoint** : `/road_graph/shortest_path/walk`  
> **Méthode** : `POST`  
> **Content-Type** : `application/json`

> **Description** : permet de calculer le trajet le plus optimal du point `departure` vers le point `arrival`. 

Payload :
```json
{
    "departure": { "x":15.79, "y":2.0 },
    "arrival": { "x":11.0, "y":2.0 }
}
```

|champ|description|
|---|---|
|`departure`|Position de départ (dans le [système de coordonnées de la MeaooCity](concepts.md#coord)) du trajet à calculer.|
|`arrival`|Position d'arrivée (dans le [système de coordonnées de la MeaooCity](concepts.md#coord)) du trajet à calculer.|

Réponse:
```json
{
    "cars": 
    [
        {
            "id": "walk", 
            "paths": 
            [
                [15.79, 2.0], 
                [14.6, 2.0], 
                [12.8, 2.0], 
                [11.9, 2.0], 
                [11.0, 2.0]
            ], 
            "costs": 
            [
                0.0,
                3.599999999999998, 
                3.599999999999998, 
                1.8000000000000007, 
                1.8000000000000007
            ], 
            "path_length": 10.799999999999997
        }
    ]
}
```

Remarque: le membre `cars` de la réponse est un héritage, nous devrions retrouver un membre plus parlant à la place. Il peut être iignoré.

|champ|description|
|---|---|
|`id`|Moyen de transport qui a été calculé.|
|`paths`|coordonnées (dans le [système de coordonnées de la MeaooCity](concepts.md#coord)) des points composant le chemin identifié par l'api comme étant le plus optimal. Ces coordonnées sont exprimées dans un tableau dont l'indice `0` est la composante `x` et l'indice `1` est la composante  `y`.|
|`costs`|[MeaooTime](concepts.md#meaootime) associé aux segments du `paths`|
|`path_length`|[MeaooTime](concepts.md#meaootime) complet du déplacement sur le paths (égal à la somme des `costs`)|

### <a name="cout_metro"></a> Calcul de coût pour un trajet en voiture

> **Endpoint** : `/road_graph/shortest_path/car`  
> **Méthode** : `POST`  
> **Content-Type** : `application/json`

> **Description** : permet de calculer le trajet le plus optimal du point `departure` vers le point `arrival`. Le robotaxi étant potentiellement à un autre endroit de la ville, il faut également prendre en compte sa position pour intégrer dans le calul le trajet de cette position vers `departure`.

```
vehicle -----> departure -----> arrival
```

Payload :
```json
{
    "departure": { "x":15.79, "y":2.0 },
    "arrival": { "x":11.0, "y":2.0 },
    "vehicles":
    [
        { "id":"correl_1", "x":21.8, "y":5.6 },
        { "id":"correl_2", "x":11.2, "y":0.2 }
    ]
}
```

|champ|description|
|---|---|
|`departure`|Position de départ (dans le [système de coordonnées de la MeaooCity](concepts.md#coord)) du trajet à calculer.|
|`arrival`|Position d'arrivée (dans le [système de coordonnées de la MeaooCity](concepts.md#coord)) du trajet à calculer.|
|`vehicles`|liste de positions (dans le [système de coordonnées de la MeaooCity](concepts.md#coord)) permettant de simuler le trajet d'un robotaxi en approche de `departure`.|
|`vehicles.id`|identifiant de corrélation permettant d'associer les éléments de la réponse aux données passées dans l'appel.|


Réponse:
```json
{
    "cars": 
    [
        {
            "id": "correl_1", 
            "paths": 
            [
                [21.8, 5.6], 
                [21.8, 2.0], 
                [15.79, 2.0], 
                [14.6, 2.0], 
                [12.8, 2.0], 
                [11.9, 2.0], 
                [11.0, 2.0]
            ], 
            "costs": 
            [
                0.0,
                3.0,
                6.0,
                3.599999999999998, 
                3.599999999999998, 
                1.8000000000000007, 
                1.8000000000000007
            ], 
            "path_length": 19.799999999999997
        },
        {
            "id": "correl_2", 
            "paths": 
            [
                [11.2, 0.2], 
                [11.2, 2.0], 
                [15.79, 2.0], 
                [14.6, 2.0], 
                [12.8, 2.0], 
                [11.9, 2.0], 
                [11.0, 2.0]
            ], 
            "costs": 
            [
                0.0,
                1.0,
                3.0,
                3.599999999999998, 
                3.599999999999998, 
                1.8000000000000007, 
                1.8000000000000007
            ], 
            "path_length": 14.799999999999997
        }
    ]
}
```

Remarque: le membre *cars* de la réponse est un héritage, nous devrions retrouver un memebre plus parlant à la place

|champ|description|
|---|---|
|`id`|Id de corrélation correspondant à la position qui a été donnée dans le payload de l'appel.|
|`paths`|coordonnées (dans le [système de coordonnées de la MeaooCity](concepts.md#coord)) des points composant le chemin identifié par l'api comme étant le plus optimal. Ces coordonnées sont exprimées dans un tableau dont l'indice `0` est la composante `x` et l'indice `1` est la composante  `y`.|
|`costs`|[MeaooTime](concepts.md#meaootime) associé aux segments du `paths`|
|`path_length`|[MeaooTime](concepts.md#meaootime) complet du déplacement sur le paths (égal à la somme des `costs`)|

### <a name="liste_metro"></a> obtenir la liste des stations de métro

> **Endpoint** : `/subway/stations`  
> **Méthode** : `GET`  

> **Description** : retourne une liste de positions correspondant aux stations de métro

Réponse:
```json
[
    { "x": 8.6, "y": 3.8 },
    { "x": 11.9, "y": 5.6 },
    ...
]
```

### <a name="conditions_circulation"></a> obtenir les conditions de circulation actuelles

> **Endpoint** : `/road_graph/traffic_conditions`  
> **Méthode** : `GET`  

> **Description** : permet de récupérer les conditions de traffic actuelles (une route qui ne serait pas listée dans cette réponse est considérée comme ayant un traffic normal). 

Réponse:
```json
[
    { 
        "id":"edge_10",
        "locations": {
            "from":{"x":0.2,"y":0.8},
            "to":{"x":0.2,"y":1.8}
        },
        "slowing_factor":3
    },
    { 
        "id":"edge_50",
        "locations": {
            "from":{"x":1.2,"y":0.8},
            "to":{"x":1.2,"y":1.8}
        },
        "slowing_factor":1
    }
]
```

|champ|description|
|---|---|
|`id`|identifiant de la voie de ciculation concernée telle que répertoriée dans l'api graph|
|`location`|positions dans le [système de coordonnées de la MeaooCity](concepts.md#coord) des extrémités du segment de ligne concerné|
|`slowing_factor`|facteur de fluidité de la voie de ciculation concernée. Plage de valeurs:[1,10], ce sont des entiers, 1 = traffic fluide, 10 = traffic 10x plus lent|


### <a name="voies_fermees"></a> obtenir les voies de circulation actuellement fermées
Pour les robotaxis:
> **Endpoint** : `/road_graph/roads_status/car`  
> **Méthode** : `GET`  

Pour les vélos:
> **Endpoint** : `/road_graph/roads_status/bike`  
> **Méthode** : `GET`  

Pour les déplacement à pieds:
> **Endpoint** : `/road_graph/roads_status/walk`  
> **Méthode** : `GET`  

> **Description** :  permet de récupérer les voies de circulation actuellement fermées.

Réponse:
```json
[
    { 
        "id":"edge_10",
        "locations": {
            "from":{"x":0.2,"y":0.8},
            "to":{"x":0.2,"y":1.8}
        },
        "state":"close"
    },
    { 
        "id":"edge_50",
        "locations": {
            "from":{"x":1.2,"y":0.8},
            "to":{"x":1.2,"y":1.8}
        },
        "state":"close"
    }
]
```

|champ|description|
|---|---|
|`id`|identifiant de la voie de ciculation concernée telle que répertoriée dans l'api graph|
|`location`|positions dans le [système de coordonnées de la MeaooCity](concepts.md#coord) des extrémités du segment de ligne concerné|
|`state`|état de la voie de ciculation concernée.|

### <a name="lignes_de_metro_fermees"></a> obtenir les lignes de métro actuellement fermées 

> **Endpoint** : `/road_graph/line_state`  
> **Méthode** : `GET`  

> **Description** : permet de récupérer les ligne de métro actuellement fermées.

Réponse:
```json
[
    { 
        "id":"edge_10",
        "locations": {
            "from":{"x":0.2,"y":0.8},
            "to":{"x":0.2,"y":1.8}
        },
        "state":"close"
    },
    { 
        "id":"edge_50",
        "locations": {
            "from":{"x":1.2,"y":0.8},
            "to":{"x":1.2,"y":1.8}
        },
        "state":"close"
    }
]
```

|champ|description|
|---|---|
|`id`|identifiant du segment de ligne de métro concerné telle que répertoriée dans l'api graph|
|`location`|positions dans le [système de coordonnées de la MeaooCity](concepts.md#coord) des extrémités du segment de ligne concerné|
|`state`|état de la voie de ciculation concernée.|

## <a name="vehicles"></a> Vehicules

> **Base URL** : `http://[ENVIRONMENT].xp65.renault-digital.com/api/vehicle`  

> **Description** : Cette API permet de récupérer les informations courantes concernant les robotaxi présents dans la ville.

### Obtention des informations de tous les véhicules

> **Endpoint** : `/api/v1/vehicles`  
> **Méthode** : `GET`  

> **Description** :  permet de récupérer tous les véhicules présents dans la ville.

Réponse:
```json
[
    {
        "id": "0000000000000000",
        "attitude": {
            "id": null,
            "position": {
                "x": 0.0,
                "y": 0.0
            },
            "orientation": 0.0,
            "speed": 0.0
        },
        "battery": 64.0,
        "available": true
    },
    { ... }
]
```

|champ|description|
|---|---|
|`id`|identifiant du robotaxi.|
|`battery`|pourcentage de batterie restant dans le vehicule.|
|`available`|état de disponibilité du taxi.|
|`attitude.id`|non utilisé.|
|`attitude.position`|positions du véhicule dans le [système de coordonnées de la MeaooCity](concepts.md#coord).|
|`attitude.orientation`|orientation du véhicule sur un cercle de 360°. 0 étant au Nord et 90 étant l'Est.|
|`attitude.speed`|Vitesse du véhicule (exprimée en m/s).|

### Obtention des informations d'un véhicule

> **Endpoint** : `/api/v1/vehicles/{id}`  
> **Méthode** : `GET`  

> **Description** :  permet de récupérer les informations d'un vehicule d'id `{id}`.

Réponse:
```json
{
    "id": "0000000000000000",
    "attitude": {
        "id": null,
        "position": {
            "x": 0.0,
            "y": 0.0
        },
        "orientation": 0.0,
        "speed": 0.0
    },
    "battery": 64.0,
    "available": true
}
```

|champ|description|
|---|---|
|`id`|identifiant du robotaxi.|
|`battery`|pourcentage de batterie restant dans le vehicule.|
|`available`|état de disponibilité du taxi.|
|`attitude.id`|non utilisé.|
|`attitude.position`|positions du véhicule dans le [système de coordonnées de la MeaooCity](concepts.md#coord).|
|`attitude.orientation`|orientation du véhicule sur un cercle de 360°. 0 étant au Nord et 90 étant l'Est.|
|`attitude.speed`|Vitesse du véhicule (exprimée en m/s).|

## <a name="dev_apis"></a> Dev environment specific apis

Ces APIs ne sont disponibles que sur votre environnement de développement. Elle ne sont pas utilisable lors de la démo.

### <a name="reset_circulation"></a> Reset des voies de circulation

Pour les robotaxis:
> **Endpoint** : `/road_graph/reset_graph/car`  
> **Méthode** : `POST`  

Pour les vélos:
> **Endpoint** : `/road_graph/reset_graph/bike`  
> **Méthode** : `POST`  

Pour les déplacement à pieds:
> **Endpoint** : `/road_graph/reset_graph/walk`  
> **Méthode** : `POST`  

Pour le métro:
> **Endpoint** : `/road_graph/reset_graph/subway`  
> **Méthode** : `POST`  

> **Description** : permet de supprimer tous les ralentissements et réouvrir toutes les voies de circulation / lignes fermées

**NOTE** : cet appel ne génère pas d'événements de réouverture ou d'événements de retour à la normale des conditions de circulation. 

