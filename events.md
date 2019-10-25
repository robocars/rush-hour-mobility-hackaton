# Les événements

L'écosystème Meaoo est hautement réactif et repose entièrement sur l'échange d'événements en temps réel.

Cette section liste les événements émis par la MeaooMap auxquels vous pouvez souscrire.

## L'agent
### <a name="agent"></a> Agent situation

> **Topic** : `project-65/prod/user/situation`  
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
|`vehicle_type`|Indique le moyen de transport utilisé par l'agent. Ce champ peut prendre n'importe quelle valeur spécifiée dans la colonne *code technique* des [moyens de transport](city.md#vehicle_type) |
|`position`|Position de l'agent dans le [système de coordonnées de la MeaooCity](city.md#coord)|
|`total_cost`|Correspond au *MeaooTime*, le temps virtuel passé par l'agent dans les transports. Cette valeur est exprimée en minutes.|

## L'environnement
### <a name="weather"></a> Météo

> **Topic** : `project-65/prod/context/change/weather`  
> **Description** : changement de météo.

Payload :
```json
{"condition":"snow"}
```

|champ|description|
|---|---|
|`condition`|Indique les nouvelles conditions météo. Ce champ peut prendre n'importe quelle valeur spécifiée dans la colonne *code technique* des [conditions météo](context.md#weather) |

### <a name="air"></a> Qualité de l'air

> **Topic** : `project-65/prod/context/change/air`  
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

> **Topic** : `project-65/prod/environment/change/roads_status`  
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
|`location`|positions dans le [système de coordonnées de la MeaooCity](city.md#coord) des extrémités du segment de ligne concerné|
|`state`|état de la voie de ciculation concernée (`open` ou `close`)|

### <a name="route"></a> Fermeture/ouverture de ligne de métro

> **Topic** : `project-65/prod/environment/change/lines_state`  
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
|`location`|positions dans le [système de coordonnées de la MeaooCity](city.md#coord) des extrémités du segment de ligne concerné|
|`state`|état de la voie de ciculation concernée (`open` ou `close`)|

### <a name="route"></a> Ralentissement des voies de circulation

> **Topic** : `project-65/prod/environment/change/traffic_conditions`  
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
|`location`|positions dans le [système de coordonnées de la MeaooCity](city.md#coord) des extrémités du segment de ligne concerné|
|`slowing_factor`|facteur de fluidité de la voie de ciculation concernée. Plage de valeurs:[1,10], ce sont des entiers, 1 = traffic fluide, 10 = traffic 10x plus lent|

### <a name="route"></a> Panne de robotaxi

> **Topic** : `project-65/prod/environment/change/breakdown`  
> **Description** : événement concernant les pannes de robotaxi

Payload :
```json
{"vehicle": "1234"}
```

|champ|description|
|---|---|
|`vehicle`|identifiant du robotaxi concerné par la panne, il n'est donc plus utilisable|


## <a name="missionstart"></a> Evénement initial de déclenchement de la mission

*A faire*

