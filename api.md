# Apis

Plusieurs API sont mises à votre disposition pour obtenir des informations sur l'écosystème:

## Agent

> **Base URL** : `http://agent-controller.[NAMESPACE].xp65.renault-digital.com`  

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
|`vehicle_type`|Indique le moyen de transport utilisé par l'agent (dernier ou actuel). Ce champ peut prendre n'importe quelle valeur spécifiée dans la colonne *code technique* des [moyens de transport](city.md#vehicle_type) |
|`position`|Position de l'agent dans le [système de coordonnées de la MeaooCity](city.md#coord)|
|`total_cost`|Correspond au *MeaooTime*, le temps virtuel passé par l'agent dans les transports. Cette valeur est exprimée en minutes.|

## <a name="context"></a> Context

> **Base URL** : `http://context-controller.[NAMESPACE].xp65.renault-digital.com`  
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

L'api graph est disponible sur l'url de base suivante http://graph.[NAMESPACE].xp65.renault-digital.com/

### <a name="fichiers_voies"></a> Fichiers de description des voies de circulation

3 fichiers json décrivent les voies de ciculation de la ville:
- /processed/v2/neighbours.json contient des informations permettant de dessiner les routes (pour les voitures) (modifié) 
- /processed/v2/subway_neighbours.json contient des informations permettant de dessiner les lignes de métro
- /processed/v2/walk_neighbours.json contient des informations permettant de dessiner les chemins piétonniers

Ces 3 fichiers ont la même structure:
```json
{
    "start": {
        "x": 12.8,
        "y": 5.6
    },
    "neighbors": [
        {
            "x": 14.6,
            "y": 5.6
        }
    ]
}
```

|champ|description|
|---|---|
|`start`|point sur la carte représentant l'extrémité A d'un segment|
|`neighbors`| tableau de points représentant la ou les extrémités d'un ou plusieurs segment ayant pour origine le point A|

Exemple:
A------B
 \
  \
   \
    B

### <a name="cout_velo"></a> Calcul de coût pour un trajet à vélo

> **Endpoint** : /road_graph/shortest_path/bike

> **Méthode** : POST

> **Content-Type** : application/json

> **Description** : permet de calculer le trajet le plus optimal du point *departure* vers le point *arrival* et retourner celui ci ainsi que le MeaooTime correspondant à ce déplacement

Payload :
```json
{
    "departure":
    {
        "x":15.79,
        "y":2.0
    },
    "arrival":
    {
        "x":11.0,
        "y":2.0
    }
}
```

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

Remarque: le membre *cars* de la réponse est un héritage, nous devrions retrouver un memebre plus parlant à la place

|champ|description|
|---|---|
|`paths`|coordonnées des points composant le chemin identifié par l'api comme étant le plus optimal|
|`costs`|MeaooTime associé aux segments du paths|
|`path_length`|MeaooTime complet du déplacement sur le paths (donc somme des costs|

### <a name="cout_metro"></a> Calcul de coût pour un trajet en métro

> **Endpoint** : /road_graph/shortest_path/subway

> **Méthode** : POST

> **Content-Type** : application/json

> **Description** : permet de calculer le trajet le plus optimal du point *departure* vers le point *arrival* et retourner celui ci ainsi que le MeaooTime correspondant à ce déplacement

Payload :
```json
{
    "departure":
    {
        "x":15.79,
        "y":2.0
    },
    "arrival":
    {
        "x":11.0,
        "y":2.0
    }
}
```

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

Remarque: le membre *cars* de la réponse est un héritage, nous devrions retrouver un memebre plus parlant à la place

|champ|description|
|---|---|
|`paths`|coordonnées des points composant le chemin identifié par l'api comme étant le plus optimal|
|`costs`|MeaooTime associé aux segments du paths|
|`path_length`|MeaooTime complet du déplacement sur le paths (donc somme des costs|

### <a name="cout_metro"></a> Calcul de coût pour un trajet à pieds

> **Endpoint** : /road_graph/shortest_path/walk

> **Méthode** : POST

> **Content-Type** : application/json

> **Description** : permet de calculer le trajet le plus optimal du point *departure* vers le point *arrival* et retourner celui ci ainsi que le MeaooTime correspondant à ce déplacement

Payload :
```json
{
    "departure":
    {
        "x":15.79,
        "y":2.0
    },
    "arrival":
    {
        "x":11.0,
        "y":2.0
    }
}
```

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

Remarque: le membre *cars* de la réponse est un héritage, nous devrions retrouver un memebre plus parlant à la place

|champ|description|
|---|---|
|`paths`|coordonnées des points composant le chemin identifié par l'api comme étant le plus optimal|
|`costs`|MeaooTime associé aux segments du paths|
|`path_length`|MeaooTime complet du déplacement sur le paths (donc somme des costs|

### <a name="cout_metro"></a> Calcul de coût pour un trajet en voiture

> **Endpoint** : /road_graph/shortest_path/car

> **Méthode** : POST

> **Content-Type** : application/json

> **Description** : 
- permet de calculer le trajet le plus optimal du point *departure* vers le point *arrival* et retourner celui ci ainsi que le MeaooTime correspondant à ce déplacement

- la différence se fait sur vehicles qui n'existe pas pour les autres endpoints
- cette différence s'explique par le fait que le taxi n'est pas nécessairement au point departure
- il faut donc spécifier en plus du point de départ et du point d'arrivée la position du taxi
- l'api va alors calculer le chemin le plus optimal pour que le taxi vienne chercher l'agent au point de départ et l'emmène au point d'arrivée
- Il est possible de demander à l'api de faire plusieurs calculs pour plusieurs taxi en un seul appel
- c'est la raison pour laquelle c'est un array
- le champ id est un id de corrélation permettant à l'appelant d'identifier les vehicules dans la réponse
- En effet la réponse contiendra autant de chemins que de de vehicules passés dans le tableau
- Ces chemins seront taggés avec l'id de corrélation
- l'api calculera donc pour chaque id de corrélation le chemin optimal
la réponse est identique en structure aux précédents appels à ceci prêt que le champ id contiendra  l id de corrélation

Payload :
```json
{
    "departure":
    {
        "x":21.8,
        "y":5.6
    },
    "arrival":
    {
        "x":12.8,
        "y":2.0
    },
    "vehicles":
    [
        {
            "id":"00000000be83b208",
            "x":21.8,
            "y":5.6
        }
    ]
}
```
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

Remarque: le membre *cars* de la réponse est un héritage, nous devrions retrouver un memebre plus parlant à la place

|champ|description|
|---|---|
|`paths`|coordonnées des points composant le chemin identifié par l'api comme étant le plus optimal|
|`costs`|MeaooTime associé aux segments du paths|
|`path_length`|MeaooTime complet du déplacement sur le paths (donc somme des costs|

### <a name="liste_metro"></a> obtenir la liste des stations de métro

> **Endpoint** : /subway/stations

> **Méthode** : GET

> **Content-Type** : application/json

> **Description** : retourne une liste de positions correspondant aux stations de métro

Réponse:
```json
[
    {
        "x": 8.6,
        "y": 3.8
    },
    {
        "x": 11.9,
        "y": 5.6
    },
    ...
]
```

### <a name="conditions_circulation"></a> obtenir les conditions de circulation actuelles

> **Endpoint** : /road_graph/traffic_conditions

> **Méthode** : GET

> **Content-Type** : application/json

> **Description** : 
- permet de récupérer les voies de circulation avec des conditions de traffic inhabituelles
- réponse au même format que l'event traffic_conditions

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
|`location`|positions dans le [système de coordonnées de la MeaooCity](city.md#coord) des extrémités du segment de ligne concerné|
|`slowing_factor`|facteur de fluidité de la voie de ciculation concernée. Plage de valeurs:[1,10], ce sont des entiers, 1 = traffic fluide, 10 = traffic 10x plus lent|


### <a name="voies_fermees"></a> obtenir les voies de circulation actuellement fermées
Pour les robotaxis:
> **Endpoint** : /road_graph/roads_status/car 

Pour les vélos:
> **Endpoint** : /road_graph/roads_status/bike 

Pour les déplacement à pieds:
> **Endpoint** : /road_graph/roads_status/walk

> **Méthode** : GET

> **Content-Type** : application/json

> **Description** : 
- permet de récupérer les voies de circulation actuellement fermées

réponse au même format que l'event roads_status mais de manière unitaire cette fois par moyen de transport

Réponse:
```json
Pour les robotaxis:
[
    {
        "car": [
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
                "state":"open"
            }
        ]
    }
]
Pour les vélos:
[
    {
        "bike": [
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
                "state":"open"
            }
        ]
    }
]
Pour les déplacement à pieds:
[
    {
        "walk": [
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
                "state":"open"
            }
        ]
    }
]
```
|champ|description|
|---|---|
|`car`|tableau regroupant les voies "open" et "close" pour les robotaxis|
|`bike`|tableau regroupant les voies "open" et "close" pour les vélos|
|`walk`|tableau regroupant les voies "open" et "close" pour les déplacements à pieds|
|`id`|identifiant de la voie de ciculation concernée telle que répertoriée dans l'api graph|
|`location`|positions dans le [système de coordonnées de la MeaooCity](city.md#coord) des extrémités du segment de ligne concerné|
|`state`|état de la voie de ciculation concernée (`open` ou `close`)|

### <a name="lignes_de_metro_fermees"></a> obtenir les lignes de métro actuellement fermées 

> **Endpoint** : /road_graph/line_state

> **Méthode** : GET

> **Content-Type** : application/json

> **Description** : 

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
        "state":"open"
    }
]
```

|champ|description|
|---|---|
|`id`|identifiant du segment de ligne de métro concerné telle que répertoriée dans l'api graph|
|`location`|positions dans le [système de coordonnées de la MeaooCity](city.md#coord) des extrémités du segment de ligne concerné|
|`state`|état de la voie de ciculation concernée (`open` ou `close`)|

## <a name="dev_apis"></a> Dev environment specific apis

## <a name="reset_circulation"></a> Reset des voies de circulation

> **Endpoints** : /road_graph/reset_graph/car
/road_graph/reset_graph/bike
/road_graph/reset_graph/walk
/road_graph/reset_graph/subway

> **Méthode** : POST

> **Content-Type** : application/json

> **Description** : 
permet de supprimer tous les ralentissements et réouvrir toutes les voies de circulation / lignes fermées