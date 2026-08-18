## Schéma fonctionnel
```mermaid
erDiagram
    an_patbat_entite {
        int4 id_entite PK
        varchar niveau FK
        varchar ref_code
        varchar nom_usage
        varchar nom_officiel
        varchar categorie FK
        text sous_categories
        varchar type_etablissement
        varchar insee
        varchar commune
        varchar dbstatut
    }

    lt_patbat_niveau {
        varchar code PK
        varchar valeur UK
    }

    lt_patbat_categorie {
        varchar code PK
        varchar valeur
    }

    lt_patbat_sous_cat {
        varchar code PK
        varchar code_cate FK
        varchar valeur
    }

    lt_patbat_etat {
        varchar code PK
        varchar valeur
    }

    geo_patbat_site {
        int4 id_entite PK
        int4 supsite
        int4 supsol_tot_bati
        int4 supdvp_tot_bati
        bool supdvp_tot_mesuree
        int2 nb_bati
        int2 nb_ssite
        geometry geom
    }

    geo_patbat_ssite {
        int4 id_entite PK
        int4 id_site FK
        int4 supsite
        int4 supsol_tot_bati
        geometry geom
    }

    geo_patbat_bati {
        int4 id_entite PK
        int4 id_site FK
        varchar etat FK
        int4 supsol
        int4 supdvp
        bool supdvp_mesuree
        int2 nb_etage
        int2 nb_uf
        geometry geom
    }

    an_patbat_unite_fonc {
        int4 id_entite PK
        int4 id_bati FK
        int4 supdvp
        bool supdvp_mesuree
        int2 nb_local
    }

    an_patbat_local {
        int4 id_entite PK
        int4 id_uf FK
        int4 supdvp
        bool supdvp_mesuree
    }

    an_patbat_orga {
        int4 id_orga PK
        varchar nom
        text observ
        varchar dbstatut
    }

    lt_patbat_role {
        varchar code PK
        varchar valeur
    }

    lk_patbat_orga_role {
        int4 gid PK
        int4 id_entite FK
        int2 id_orga FK
        varchar code_role FK
    }

    an_patbat_contact {
        int4 id_contact PK
        varchar denomination
        varchar tel
        varchar email
        int2 organisme FK
        varchar dbstatut
    }

    lt_patbat_fonction {
        varchar code PK
        varchar valeur
    }

    lk_patbat_contact {
        int4 gid PK
        int4 id_entite FK
        int2 id_contact FK
        varchar code_fonction FK
    }

    lt_patbat_fluide {
        varchar code PK
        varchar valeur
    }

    lt_patbat_type_compteur {
        varchar code PK
        varchar valeur
    }

    geo_patbat_pdl {
        int4 id_pdl PK
        varchar code_ref UK
        varchar type_fluide FK
        int2 puissance
        varchar type_compteur FK
        varchar compteur_parent FK
        geometry geom
    }

    lk_patbat_pdl {
        int4 gid PK
        int4 id_entite
        int2 id_pdl
    }

    lt_type_media {
        varchar code PK
        varchar valeur
    }

    an_patbat_media {
        int4 gid PK
        int4 id
        varchar type_media FK
        text media
        text n_fichier
    }

    lk_patbat_adresse {
        int4 gid PK
        int4 id_entite FK
        int4 id_adresse
    }

    an_patbat_parcelle {
        int4 gid PK
        int4 id_entite
        varchar insee
        text geo_parcelle
    }

    lt_patbat_niveau ||--o{ an_patbat_entite : niveau
    lt_patbat_categorie ||--o{ an_patbat_entite : categorie
    lt_patbat_categorie ||--o{ lt_patbat_sous_cat : code_cate

    an_patbat_entite ||--|| geo_patbat_site : id_entite
    an_patbat_entite ||--|| geo_patbat_ssite : id_entite
    an_patbat_entite ||--|| geo_patbat_bati : id_entite
    an_patbat_entite ||--|| an_patbat_unite_fonc : id_entite
    an_patbat_entite ||--|| an_patbat_local : id_entite

    geo_patbat_site ||--o{ geo_patbat_ssite : id_site
    geo_patbat_site ||--o{ geo_patbat_bati : id_site
    geo_patbat_bati ||--o{ an_patbat_unite_fonc : id_bati
    an_patbat_unite_fonc ||--o{ an_patbat_local : id_uf
    lt_patbat_etat ||--o{ geo_patbat_bati : etat

    an_patbat_orga ||--o{ an_patbat_contact : organisme
    an_patbat_entite ||--o{ lk_patbat_orga_role : id_entite
    an_patbat_orga ||--o{ lk_patbat_orga_role : id_orga
    lt_patbat_role ||--o{ lk_patbat_orga_role : code_role
    an_patbat_entite ||--o{ lk_patbat_contact : id_entite
    an_patbat_contact ||--o{ lk_patbat_contact : id_contact
    lt_patbat_fonction ||--o{ lk_patbat_contact : code_fonction

    lt_patbat_fluide ||--o{ geo_patbat_pdl : type_fluide
    lt_patbat_type_compteur ||--o{ geo_patbat_pdl : type_compteur
    geo_patbat_pdl ||--o{ geo_patbat_pdl : compteur_parent
    an_patbat_entite ||--o{ lk_patbat_pdl : id_entite
    geo_patbat_pdl ||--o{ lk_patbat_pdl : id_pdl

    lt_type_media ||--o{ an_patbat_media : type_media
    an_patbat_entite ||--o{ lk_patbat_adresse : id_entite
    an_patbat_entite ||--o{ an_patbat_parcelle : id_entite
```



## Résumé fonctionnel

Le modèle de données est composé d'une table principale `an_patbat_entite` qui comporte les informations communes aux 5 niveaux du référentiel et crée l'identifiant unique `id_entite` pour chaque entité.

Chaque niveau a aussi une table spécifique qui réutilise l'identifiant `id_entite` dans laquelle est stocké les informations spécifique à ce niveau :
- Site : `geo_patbat_site`
- Sous-site : `geo_patbat_ssite`
- Bâtiment : `geo_patbat_bati`
- Unité fonctionnelle : `an_patbat_unite_fonc`
- Local : `an_patbat_local`

Les données extérieures aux niveaux (`médias`, `contacts`...) sont stockées dans des tables extérieures détaillées ci-dessous, elles réutilisent l'`id_entite`.

Les tables sont modifiées via des vues regroupant les informations de la table principale ainsi que des tables spécifique au niveau, ces vues sont accompagnées d'un trigger 'instead of' ainsi que d'une fonction qui modifie les données des tables.

## Dépendances

- Les fonctions `ft_r_parcelles_bati` et `ft_r_parcelles_site_ssite` utilisent la table `geo_parcelle` du schéma `r_cadastre` afin de récupérer les parcelles sur lesquelles se trouvent site, sous-site et bâtiment de façon automatique.

- Les fonctions `ft_r_insee_commune` et `ft_m_gestion_v_site` utilisent la table `geo_osm_commune` du schéma `r_osm` afin de récupérer les codes insee et commune sur lesquels se trouvent site, sous-site et bâtiment. Ainsi que pour vérifier que le site ne se trouve que dans une commune.



  
## Classes
