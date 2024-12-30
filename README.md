![picto](https://github.com/sigagglocompiegne/orga_gest_igeo/blob/master/doc/img/geocompiegnois_2020_reduit_v2.png)

# Référentiel patrimoine bâti

## Changelog
- 19/12/2024 : initialisation de la documentation

## Désignation
Le périmètre du référentiel porte sur le patrimoine des collectivités locales en situation de propriété ou d'exploitation de tout ou partie des locaux d'un batiment.

## Objectif
Les objectifs poursuivis sont de
- construire un référentiel partagé et limitatif des seuls besoins métiers (est exclu la gestion de chaque local ou la gestion d'une troisième dimension avec les étages des batiments)
- définir le cycle de vie du référentiel en identifiant les événements de nature à le faire vivre
- définir des modalités d'information en cas de modification du référentiel auprès des différents services concernés pour répercussion dans leurs progiciels métiers et les mettre en oeuvre
- idéalement, étudier l'hypothèse d'intégration automatisée du référentiel et sa mise à jour, dans les progiciels métiers et la mettre en oeuvre

## Classes
Le modèle de données decrivant le patrimoine bâti s'organise autour de 3 classes principales et 1 optionnelle.
_D'autre classes sont intégrées pour gérer des informations métiers attachées au référentiel_

### Définitions
#### Site
Lieu géographique cohérent comportant du patrimoine bâti et disposant d’une dénomination usuelle commune (il fait sens commun).
Il est autant que possible indépendant d’une approche métier, organisationnelle ou administrative afin d'assurer sa stabilité (délimitation, dénomination).

#### Sous-site (optionnel)
Classe optionnelle permettant si nécessaire de définir un sous ensemble spécifique d’un site.

#### Bâtiment
Construction souterraine et/ou au-dessus du sol, ayant pour objectif d'être permanente, pour abriter des humains ou des activités humaines.
Un bâtiment possède a minima un accès depuis l’extérieur.
Dans la mesure du possible, un bâtiment est distinct d’un autre dès lors qu’il est impossible de circuler entre eux.

_NB : La définition est héritée celle du CNIG à la suite dess travaux du projet de Référentiel National des Bâtiments (RNB)_

#### Unité fonctionnelle
Ensemble de locaux / cellules d'un bâtiment partageant une fonction commune.
Cette classe répond à la volonté de ne pas avoir à descendre le modèle à l'échelle de chaque local.

### Relations
- Un site doit être sur une et seule commune
- Un sous site doit être inclus dans un site
- Un bâtiment doit être inclus dans un site
- Une unité fonctionnelle doit être incluse dans un bâtiment
- Un bâtiment peut être inclus dans aucun à plusieurs sous-sites

### Cycle de vie
Sont identifiées ici les seuls événements ayant une incidence sur le référentiel.
Les événement relatifs aux informations métiers attachées au référentiel ne rentrent pas ce cadre.
- achat de patrimoine
- vente de patrimoine
- démolition batiment
- construction batiment
- division batiment
- changement usage de locaux
- ajout nouvel usage de locaux
- suppression usage de locaux
- tout chgangement engendrant un changement de M57
