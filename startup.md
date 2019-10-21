# Votre accueil

Les équipes de *BeMyApp* et les équipes de *Renault-Digital* sont là pour vous accueillir et vous accompagner au long de ce Week-End.

Plus particulièrement, la core-team de MeaooCar est à votre disposition pour résoudre les problèmes techniques liés aux environnements mis à votre disposition, ou répondre à vos questions concernant le fonctionnement de l'écosystème et des APIs qui vous sont founies.

## Vos données utiles

A votre arrivée, vous aurez besoin de récupérer la salle qui est assignée à votre équipe, ainsi que les informations sur votre environnement de développement.

Les informations de votre environnement sont composées de :
1. Un identifiant de namespace
2. L'adresse de votre broker de message
3. Les credentials de votre broker de message (login et mot de passe)
4. Les credentials du réseau wifi qui est mis à votre disposition si vous en avez besoin (privilégiez vos connexion 4g)

## Votre namespace

Votre identifiant de namespace vous permettra de vous connecter aux différentes APIs de récupération d'état qui sont mises à votre disposition. Une description complète de leur fonctionnement et des signatures de méthodes est détaillée ailleurs, mais nous vous listons ici les adresses de base de ces APIs (remplacez `[NAMESPACE]` par votre identifiant de namespace) :

| name     | base url |
|----------|------|
| Graph    | `http://graph.[NAMESPACE].xp65.renault-digital.com` |
| Context  | `http://context-controller.[NAMESPACE].xp65.renault-digital.com` |
| Agent    | `http://agent-controller.[NAMESPACE].xp65.renault-digital.com` |


[< retour](README.md)