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
Modèle de description du patrimoine bâti s'organise autour de 3 classes principales et 1 optionnelle

### Site
Lieu géographique cohérent comportant du patrimoine bâti et disposant d’une dénomination usuelle commune (de référence et donc indépendante d’une approche métier)
+ critere géo qui fait sens. garder approche spatiale stable et déliée des approches organisationnelles / administratives

### Sous-site (optionnel)
Sous ensemble spécifique d’un site 
cas d’usage : adresse postale différente, segmentation d’un site

### Bâtiment
Construction souterraine et/ou au-dessus du sol, ayant pour objectif d'être permanente, pour abriter des humains ou des activités humaines. Un bâtiment possède a minima un accès depuis l’extérieur. Dans la mesure du possible, un bâtiment est distinct d’un autre dès lors qu’il est impossible de circuler entre eux
Reprise de la définition du CNIG issue des travaux du projet Référentiel National des Bâtiments (RNB)

### unité fonctionnelle
Ensemble de locaux / cellules partageant une fonction commune 
UF classes est un ensemble de locaux d’un batiment ayant une fonction commune de lieu de classe

## Cycle de vie
- achat de patrimoine
- vente de patrimoine
- démolition batiment
- construction batiment
- division batiment
- changement usage de locaux
- ajout nouvel usage de locaux
- suppression usage de locaux
- tout chgangement engendrant un changement de M57
