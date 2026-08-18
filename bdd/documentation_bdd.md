## Schéma fonctionnel

## Résumé fonctionnel

Le modèle de données est composé d'une table principale `an_patbat_entite` qui comporte les informations communes aux 5 niveaux du référentiel et crée l'identifiant unique `id_entite` pour chaque entité.

Chaque niveau a aussi une table qui réutilise l'identifiant `id_entite` dans laquelle est stocké les informations spécifique à ce niveau :
- Site : `geo_patbat_site`
- Sous-site : `geo_patbat_ssite`
- Bâtiment : `geo_patbat_bati`
- Unité fonctionnelle : `an_patbat_bati`
- Local : `an_patbat_local`

## Dépendances

- Les fonctions `ft_r_parcelles_bati` et `ft_r_parcelles_site_ssite` utilisent la table `geo_parcelle` du schéma `r_cadastre` afin de récupérer les parcelles sur lesquelles se trouvent site, sous-site et bâtiment de façon automatique.

- La fonction `ft_r_insee_commune` utilise la table `geo_osm_commune` du schéma `r_osm` afin de récupérer les codes insee et commune sur lesquels se trouvent site, sous-site et bâtiment.



  
## Classes
