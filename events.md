# Les événements

L'écosystème Meaoo est hautement réactif et repose entièrement sur l'échange d'événements en temps réel.

Cette section liste les événenemts auxquels vous pouvez souscrire.

### <a name="agent"></a> Agent situation

Route : `project-65/prod/user/situation`</br>
Description : changement de situation de l'agent.

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

topic : `project-65/prod/context/change/weather`</br>
Description : changement de météo.

Payload :
```json
{"condition":"snow"}
```

|champ|description|
|---|---|
|`condition`|Indique les nouvelles conditions météo. Ce champ peut prendre n'importe quelle valeur spécifiée dans la colonne *code technique* des [conditions météo](context.md#weather) |

### <a name="air"></a> Qualité de l'air

Route : `project-65/prod/context/change/air`</br>
Description : changement de la qualité de l'air.

Payload :
```json
{"condition":"normal"}
```

|champ|description|
|---|---|
|`condition`|Indique la nouvelle qualité de l'air. Ce champ peut prendre n'importe quelle valeur spécifiée dans la colonne *code technique* de la [qualité de l'air](context.md#air) |