# Exemple de mission simple sans incidents

Je me suis abonné au topic `[TOPIC_PREFIX]/prod/user/mission` et j'ai reçu:

```json
{
    "mission": "Votre mission, si vous l'acceptez, consiste à aller à cet endroit le plus rapidement possible.",
    "positions": [
        {
            "x": 0.2,
            "y": 0.2
        }
    ]
}
```

J'interroge alors les APIs à ma disposition pour récupérer la position actuelle de mon agent :

`GET http://agent-controller.[NAMESPACE].xp65.renault-digital.com/api/user/situation/last`  

```json
{
    "vehicle_type": "walk",
    "position": {
        "x": 0.2,
        "y": 1.8
    },
    "total_cost": 0.0
}
```

J'interroge le graphe pour aller à destination avec plusieurs moyens de transport différents:

`POST http://graph.[NAMESPACE].xp65.renault-digital.com/road_graph/shortest_path/bike '{"departure": { "x":0.2, "y":1.8 }, "arrival": { "x":0.2, "y":0.2 }}'`

```json
{
    "cars": 
    [
        {
            "id": "bike", 
            "paths": [ [0.2, 1.8], [0.2, 1.8], [0.2, 0.2] ], 
            "costs": [ 0.0, 3.599999999999998 ], 
            "path_length": 3.599999999999998
        }
    ]
}
```

`POST http://graph.[NAMESPACE].xp65.renault-digital.com/road_graph/shortest_path/walk '{"departure": { "x":0.2, "y":1.8 }, "arrival": { "x":0.2, "y":0.2 }}'`

```json
{
    "cars": 
    [
        {
            "id": "walk", 
            "paths": [ [0.2, 1.8], [0.2, 1.8], [0.2, 0.2] ], 
            "costs": [ 0.0, 9.5 ], 
            "path_length": 9.5
        }
    ]
}
```

J'affiche les informations obtenues à mon agent, à savoir que faire le déplacement à vélo lui prendra 3,6 minutes et à pieds 9,5 minutes.

Mon agent choisi de faire le déplacement à pieds. J'informe l'écosystème du choix de mon agent pour faire un déplacement à l'aide de la commande move.

Je publie `{"vehicle_type": "walk", "target": {"x": 0.2, "y": 0.2}}` sur le topic `[TOPIC_PREFIX]/prod/user/path`.

Je peux constater le déplacement de mon agent en m'abonnant au topic `[TOPIC_PREFIX]/prod/user/situation` et constater qu'il a atteint son objectif en écoutant le topic `[TOPIC_PREFIX]/prod/user/mission`
