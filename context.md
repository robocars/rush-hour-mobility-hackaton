# Le contexte extérieur

Le contexte extérieur est une information importante dans l'expérience utilisateur d'un service à la mobilité. En effet, on pourrait se dire qu'il n'est pas très judicieux de proposer un déplacement à vélo lorsque les routes sont verglacées.

Dans notre écosystème, le contexte extérieur est composé de différents éléments.

## <a name="weather"></a> Météo

La météo est caractérisée par les conditions suivantes :

| code technique | description |
| --- | --- |
| `snow` | Il neige. |
| `rain` | Il pleut. |
| `normal` | Il fait beau. |
| `heat wave` | Période de canicule. |

Vous pouvez obtenir la météo via l'[API dédiée](api.md#weather) ou vous abonner aux [événements de mise à jour](events.md#weather).

## <a name="air"></a> Qualité de l'air

La qualité de l'air est caractérisée par les conditions suivantes :

| code technique | description |
| --- | --- |
| `normal` | Qualité normale. |
| `pollution peak` | Pic de pollution. |

Vous pouvez obtenir la quatlié de l'air via l'[API dédiée](api.md#air) ou vous abonner aux [événements de mise à jour](events.md#air).
