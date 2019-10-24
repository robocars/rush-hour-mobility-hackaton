# Les événements

L'écosystème Meaoo est hautement réactif et repose entièrement sur l'échange d'événements en temps réel.

Cette section liste les événements émis par la MeaooMap auxquels vous pouvez souscrire.

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
- ralentissements
- pannes
- *A compléter*

Les événements incidents entraînent un "freeze" de la ville et du déplacement en cours. Une commande venant de l'application suite à une interaction utilisateur relancera l'écosystème  

## <a name="missionstart"></a> Evénement initial de déclenchement de la mission

*A faire*

