# Apis

Plusieurs API sont mises à votre disposition pour obtenir des informations sur l'écosystème:

## Agent

Base URL : `http://agent-controller.[NAMESPACE].xp65.renault-digital.com`

Description : Cette API permet de récupérer les informations relatives à votre agent.

### <a name="agent"></a> Agent situation

Route : `/api/user/situation/last`</br>
Méthode : `GET`</br>
Description : récupère la dernière situation connue de l'agent.

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

Base URL : `http://context-controller.[NAMESPACE].xp65.renault-digital.com`

Description : Cette API permet de récupérer les informations relatives au contexte extérieur à la MeaooCity.

### <a name="weather"></a> Météo

Route : `/api/context/weather/current`</br>
Méthode : `GET`</br>
Description : récupère la météo actuelle.

Réponse :
```json
{"condition":"snow"}
```

|champ|description|
|---|---|
|`condition`|Indique les conditions météo courantes. Ce champ peut prendre n'importe quelle valeur spécifiée dans la colonne *code technique* des [conditions météo](context.md#weather) |

### <a name="air"></a> Qualité de l'air

Route : `/api/context/air/current`</br>
Méthode : `GET`</br>
Description : récupère la qualité de l'air actuelle.

Réponse :
```json
{"condition":"normal"}
```

|champ|description|
|---|---|
|`condition`|Indique la qualité de l'air actuelle. Ce champ peut prendre n'importe quelle valeur spécifiée dans la colonne *code technique* de la [qualité de l'air](context.md#air) |

## <a name="graph"></a> Graph



