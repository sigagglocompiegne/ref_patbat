![picto](https://github.com/sigagglocompiegne/orga_gest_igeo/blob/master/doc/img/geocompiegnois_2020_reduit_v2.png)

# Référentiel patrimoine bâti

## Changelog
- 19/12/2024 : initialisation de la documentation
- 03/04/2025 : Ajout volet classes, définitions, relations, cycle de vie
- 
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
D'autres classes sont intégrées pour gérer des informations métiers attachées au référentiel (ex : classe pour gérer les contacts)

### Définitions

Classe | Définition | Remarque
--------- | --------- |--------- 
Site | Lieu géographique cohérent, généralement clos, comportant du patrimoine bâti et disposant d’une dénomination usuelle commune (il fait sens commun)  | Il est autant que possible indépendant d’une approche métier, organisationnelle ou administrative afin d'assurer sa stabilité (délimitation, dénomination). 
Sous-site | Classe optionnelle décrivant un sous-lieu géographique cohérent du patrimoine bâti dans un site | Cette classe permet si nécessaire de segmenter des fractions d'un site
Bâtiment | Construction souterraine et/ou au-dessus du sol, ayant pour objectif d'être permanente, pour abriter des humains ou des activités humaines. Un bâtiment possède a minima un accès depuis l’extérieur. Dans la mesure du possible, un bâtiment est distinct d’un autre dès lors qu’il est impossible de circuler entre eux. | La définition est héritée de celle du CNIG à la suite dess travaux du projet de Référentiel National des Bâtiments (RNB)
Unité fonctionnelle | Ensemble de locaux / cellules d'un bâtiment partageant une fonction commune. | Cette classe répond à la volonté de ne pas avoir à descendre le modèle à l'échelle de chaque local.

### Relations
- Un site doit être sur une et seule commune
- Un sous site doit être inclus dans un site
- Un bâtiment doit être inclus dans un site
- Une unité fonctionnelle doit être incluse dans un bâtiment
- Un bâtiment peut être inclus dans aucun à plusieurs sous-sites

### Cycle de vie
Seuls les événements ayant une incidence sur le référentiel et un besoin d'information transverse à l'ensemble des SI métiers sont identifiés.
Les événement relatifs aux informations métiers attachées au référentiel ne rentrent pas ce cadre.

Ce qui relève du cycle de vie strict du patrimoine bâti :
- patrimoine : achat | vente |
- batiment : construction | démolition | division
- usage des locaux : changement d'usage | nouvel usage | suppression d'un usage

_(- changement de catégorie de fonction induisant un changement de M57 (à détailler))_

Les changements hors référentiel :
- information de contact / “référent”
- information générales héritées d'autres référentiels
  - changement d'adresse
  - changement de parcelles
  - identifiant batiment du RNB 
