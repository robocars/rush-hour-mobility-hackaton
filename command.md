# Les commandes

Les commandes sont des événements à publier sur le broker de messages. Ils sont écoutés par l'écosystème Moeaoo et interprétés pour réaliser les actions associées.

## Utilisables lors de la démo

*Ces commandes sont les seules qui seront acceptées lors de la démo par l'écosystème*.

### <a name="move"></a> Déplacer l'agent

> **Protocol** : MQTT  
> **Topic** : `project-65/prod/user/path`  
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
|`vehicle_type`|Indique le moyen de transport avec lequel déplacer l'agent. Ce champ peut prendre n'importe quelle valeur spécifiée dans la colonne *code technique* des [moyens de transport](city.md#vehicle_type) |
|`target`|Position cible vers laquelle déplacer l'agent (exprimée dans le [système de coordonnées de la MeaooCity](city.md#coord)). Si la cible n'est pas sur une route ou sur une ligne de métro, le système trouve de lui-même le carrefour ou la station de métro la plus proche et l'utilise comme cible.|

#### Raisons pour lesquelles la commande pourrait ne pas fonctionner

* La commande n'est pas envoyée sur le bon topic
* Le payload n'est pas bien formatté.
* Les [règles de changement de transport](city.md#vehicle_type) ne sont pas respectées.
* La destination est égale à la position courante.
* Aucun [chemin](graph.md) ne permet d'aller à la cible.

## Utilisables sur votre environnement de développement

Ces commandes vous sont fournies afin de faciliter vos développements et vos tests. Elles ont un impact direct sur l'écosystème et permettent de manipuler [le contexte](context.md), l'agent, ou le graphe par exemple. 
Lors de la démo finale, ou pendant vos tests sur la MeaooCity réelle, ces commandes ne seront pas acceptées et vous retourneront au mieux un message d'erreur, au pire rien.

