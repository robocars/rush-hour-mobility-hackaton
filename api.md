# Apis

Plusieurs API sont mises à votre disposition pour obtenir des informations sur l'écosystème:

## Agent

Base URL : `http://agent-controller.[NAMESPACE].xp65.renault-digital.com`

Description : Cette API permet de récupérer les informations relatives à votre agent.

### <a name="agent"></a> Agent situation

Route : `/api/user/situation/last`
Méthode : `GET`
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

Détail de la réponse :
|champ|description|
|---|---|
|`vehicle_type`|Indique le moyen de transport utilisé par l'agent (dernier ou actuel). Ce champ peut prendre n'importe quelle valeur spécifiée dans la colonne *code technique* des [moyens de transport](city.md#vehicle_type) |
|`position`|Position de l'agent dans le [système de coordonnées de la MeaooCity](city.md#coord)|
|`total_cost`|Correspond au *MeaooTime*, le temps virtuel passé par l'agent dans les transports. Cette valeur est exprimée en minutes.|


## <a name="context"></a> Context



## <a name="graph"></a> Graph



