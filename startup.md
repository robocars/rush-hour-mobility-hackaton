# Votre accueil

Les équipes de *Renault-Digital* et d'*Epita* sont là pour vous accueillir et vous accompagner au long de ce Hakathon.

Plus particulièrement, les mentors sont à votre disposition pour répondre à vos questions concernant le fonctionnement de l'écosystème et des APIs qui vous sont fournies. La core team MEAOO quant à elle pourra résoudre les problèmes techniques liés aux environnements mis à votre disposition.

## Vos données utiles

A votre arrivée, *une salle sera assignée à votre équipe, et vous aurez besoin de récupérer les informations de votre environnement de développement*.

Les informations de votre environnement sont composées de :
1. Un identifiant d'environnement (référencé dans cette documentation avec le mot *[ENVIRONMENT]*)
1. L'adresse de votre broker de message
1. Les credentials de votre broker de message (login et mot de passe)

### Votre environnement

Votre identifiant d'environnement vous permettra de vous connecter aux différentes APIs de récupération d'état qui sont mises à votre disposition ainsi qu'à la [MeaooMap](concepts.md#map).

### Votre broker de messages

Un broker de messages est associé à environnement. Vous avez besoin des informations de connexion pour vous abonner ou pour publier sur les différents topics (existants ou que vous aller créer), ainsi qu'un préfixe qui servira de root à vos topics.

Si vous souhaitez utiliser le broker de messages pour publier sur vos propres topics, vous devrez absolument les publier sous l'arborescence `[ENVIRONMENT]/myteam/...`

[< retour](README.md)