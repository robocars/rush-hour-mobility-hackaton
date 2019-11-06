# Les événements

L'écosystème Meaoo est hautement réactif et repose entièrement sur l'échange d'événements en temps réel.

Cette section liste les événements émis par la MeaooMap auxquels vous pouvez souscrire.

## L'agent
### <a name="agent"></a> Agent situation

> **Topic** : `[TOPIC_PREFIX]/prod/user/situation`  
> **Description** : changement de situation de l'agent.

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
|`vehicle_type`|Indique le moyen de transport utilisé par l'agent. Ce champ peut prendre n'importe quelle valeur spécifiée dans la colonne *code technique* des [moyens de transport](concepts.md#vehicle_type) |
|`position`|Position de l'agent dans le [système de coordonnées de la MeaooCity](concepts.md#coord)|
|`total_cost`|Correspond au [MeaooTime](concepts.md#meaootime), le temps virtuel passé par l'agent dans les transports. Cette valeur est exprimée en minutes.|

### <a name="objective"></a> Objective Reached

> **Topic** : `[TOPIC_PREFIX]/prod/user/objective-reached`  
> **Description** : L'agent a atteint un objectif de sa [mission](interactionschema.md#missionstart).

Réponse :
```json
{
    "x": 3.024000000000001,
    "y": 2
}
```

|champ|description|
|---|---|
|`x`|Les coordonnées en x de l'objectif atteint |
|`y`|Les coordonnées en y de l'objectif atteint|

## L'environnement
### <a name="weather"></a> Météo

> **Topic** : `[TOPIC_PREFIX]/prod/context/change/weather`  
> **Description** : changement de météo.

Payload :
```json
{"condition":"snow"}
```

|champ|description|
|---|---|
|`condition`|Indique les nouvelles conditions météo. Ce champ peut prendre n'importe quelle valeur spécifiée dans la colonne *code technique* des [conditions météo](context.md#weather) |

### <a name="air"></a> Qualité de l'air

> **Topic** : `[TOPIC_PREFIX]/prod/context/change/air`  
> **Description** : changement de la qualité de l'air.

Payload :
```json
{"condition":"normal"}
```

|champ|description|
|---|---|
|`condition`|Indique la nouvelle qualité de l'air. Ce champ peut prendre n'importe quelle valeur spécifiée dans la colonne *code technique* de la [qualité de l'air](context.md#air) |

## <a name="incident"></a> Les incidents de la ville

Les événements incidents peuvent indiquer plusieurs situations:
- fermeture/ouverture de voies de circulation
- fermeture/ouverture de ligne de métro
- ralentissements sur les voies de ciculation
- panne de robotaxi

Les événements incidents entraînent un "freeze" de la ville et du déplacement en cours. Une commande venant de l'application suite à une interaction utilisateur relancera l'écosystème  

### <a name="route"></a> Fermeture/ouverture de route

> **Topic** : `[TOPIC_PREFIX]/prod/environment/change/roads_status`  
> **Description** : événement concernant les voies dont l'accessibilité vient d'être modifiée pour les robotaxis(car), les vélos (bike) et à pieds (walk). 
RAPPEL: Le métro est géré à part

Payload :
```json
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
    },
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
    },
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
|`location`|positions dans le [système de coordonnées de la MeaooCity](concepts.md#coord) des extrémités du segment de ligne concerné|
|`state`|état de la voie de ciculation concernée (`open` ou `close`)|

### <a name="route"></a> Fermeture/ouverture de ligne de métro

> **Topic** : `[TOPIC_PREFIX]/prod/environment/change/lines_state`  
> **Description** : événement concernant l'accessibilité des lignes de métro

Payload :
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
|`location`|positions dans le [système de coordonnées de la MeaooCity](concepts.md#coord) des extrémités du segment de ligne concerné|
|`state`|état de la voie de ciculation concernée (`open` ou `close`)|

### <a name="route"></a> Ralentissement des voies de circulation

> **Topic** : `[TOPIC_PREFIX]/prod/environment/change/traffic_conditions`  
> **Description** : événement concernant les voies de ciculation subissant un changement de la fluidité du traffic

Payload :
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

### <a name="route"></a> Panne de robotaxi

> **Topic** : `[TOPIC_PREFIX]/prod/environment/change/breakdown`  
> **Description** : événement concernant les pannes de robotaxi

Payload :
```json
{"vehicle": "1234"}
```

|champ|description|
|---|---|
|`vehicle`|identifiant du robotaxi concerné par la panne, il n'est donc plus utilisable|

## Les Robotaxi

### <a name="vehicle"></a> Vehicle attitude

> **Topic** : `[TOPIC_PREFIX]/prod/{id}/status/attitude`  
> **Description** : changement d'attitude du robotaxi `{id}`

Réponse :
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


## <a name="missionstart"></a> Evénement initial de déclenchement de la mission

> **Topic** : `[TOPIC_PREFIX]/prod/user/mission`  
> **Description** : événement qui donne la mission de l'agent. Il s'agit de l'événement de départ du scénario de démo de votre application.

Réponse :
```json
{
    "mission": "Votre mission, si vous l'acceptez, consiste à passer par l'ensemble des points ci dessous.",
    "positions": [
        {
            "x": 0.0,
            "y": 0.0
        },
        ...
    ]
}
```

|champ|description|
|---|---|
|`mission`|Message décrivant sa mission à l'agent.|
|`positions`|Ensemble de positions (dans le [système de coordonnées de la MeaooCity](concepts.md#coord)) que l'agent doit atteindre dans l'ordre. Le dernier élément du tableau est sa destination finale. Tous les points précédents doivent être franchis par l'agent.|
