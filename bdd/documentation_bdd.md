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

---

## Classes géographiques

### `r_patbat`.`geo_patbat_site`
Classe géographique regroupant les informations de toutes les entités de niveau « Site ».
| Colonne | Description | Type | Valeur par défaut |
| :--- | :--- | :--- | :--- |
| id_entite | Identifiant de l'entité | integer | |
| supsite | Superficie du site, champ calculé automatiquement à l'insert | integer | |
| supsol_tot_bati | Total calculé de la superficie au sol des bâtiments compris dans le site | integer | |
| supdvp_tot_bati | Total calculé de la superficie développée des bâtiments compris dans le site | integer | |
| supdvp_tot_mesuree | Indique si la superficie développée totale des bâtiments est mesurée pour tous | boolean | FALSE |
| nb_bati | Compte du nombre de bâtiments ayant existé dans le site, utilisé pour le code de référence des bâtiments | smallint | 0 |
| nb_ssite | Compte du nombre de sous-sites ayant existé dans le site, utilisé pour le code de référence des sous-sites | smallint | 0 |
| geom | Géométrie du site | geometry(MultiPolygon,2154) | |

Particularités à noter :
- Clé primaire sur `id_entite`.
- 1 clé étrangère : `id_entite` → `an_patbat_entite(id_entite)`.
- 3 triggers :
  - `t_t3_insee_commune` (`AFTER INSERT OR UPDATE OF geom`) : récupère le code INSEE et le nom de la commune intersectant la géométrie et met à jour `an_patbat_entite`.
  - `t_t3_parcelles` (`AFTER INSERT OR UPDATE OF geom`) : recherche les parcelles cadastrales intersectant un buffer négatif de 3 m de la géométrie et les insère dans `an_patbat_parcelle`.
  - `t_t100_log` (`AFTER INSERT OR UPDATE OR DELETE`) : journalisation.
- Cette table n'est en pratique jamais manipulée directement : toutes les opérations transitent par la vue de gestion `geo_v_patbat_site`.

### `r_patbat`.`geo_patbat_ssite`
Classe géographique regroupant les informations de toutes les entités de niveau « Sous-site ».
| Colonne | Description | Type | Valeur par défaut |
| :--- | :--- | :--- | :--- |
| id_entite | Identifiant de l'entité | integer | |
| id_site | Identifiant du site dans lequel est compris le sous-site | integer | |
| supsite | Superficie du sous-site, champ calculé automatiquement à l'insert | integer | |
| supsol_tot_bati | Total calculé de la superficie au sol des bâtiments compris dans le sous-site | integer | |
| geom | Géométrie du sous-site | geometry(MultiPolygon,2154) | |

Particularités à noter :
- Clé primaire sur `id_entite`.
- 2 clés étrangères :
  - `id_entite` → `an_patbat_entite(id_entite)`
  - `id_site` → `geo_patbat_site(id_entite)`
- 3 triggers : `t_t3_insee_commune`, `t_t3_parcelles`, `t_t100_log` (mêmes fonctions que pour `geo_patbat_site`).

### `r_patbat`.`geo_patbat_bati`
Classe géographique regroupant les informations de toutes les entités de niveau « Bâtiment ».
| Colonne | Description | Type | Valeur par défaut |
| :--- | :--- | :--- | :--- |
| id_entite | Identifiant de l'entité | integer | |
| id_site | Identifiant du site dans lequel est compris le bâtiment | integer | |
| etat | État du bâtiment | character varying(2) | |
| supsol | Superficie au sol du bâtiment, champ calculé automatiquement à l'insert | integer | |
| supdvp | Superficie développée du bâtiment | integer | |
| supdvp_mesuree | Indique si la superficie développée est mesurée | boolean | FALSE |
| nb_etage | Nombre d'étages du bâtiment | smallint | |
| nb_uf | Compte du nombre d'unités fonctionnelles ayant existé dans le bâtiment, utilisé pour le code de référence des UF | smallint | 0 |
| geom | Géométrie du bâtiment | geometry(MultiPolygon,2154) | |

Particularités à noter :
- Clé primaire sur `id_entite`.
- 3 clés étrangères :
  - `id_entite` → `an_patbat_entite(id_entite)`
  - `id_site` → `geo_patbat_site(id_entite)`
  - `etat` → `lt_patbat_etat(code)`
- 3 triggers : `t_t3_insee_commune`, `t_t3_parcelles`, `t_t100_log`.

### `r_patbat`.`geo_patbat_pdl`
Table contenant les informations des Points De Livraison de fluides (compteurs).
| Colonne | Description | Type | Valeur par défaut |
| :--- | :--- | :--- | :--- |
| id_pdl | Identifiant unique du PDL | serial | nextval() |
| code_ref | Code de référence du PDL | character varying(100) | |
| type_fluide | Code du type de fluide du PDL | character varying(2) | |
| puissance | Puissance du PDL | smallint | |
| type_compteur | Type du compteur (principal ou sous-compteur) | character varying(2) | |
| compteur_parent | Code de référence du compteur parent | character varying(100) | |
| observ | Observations diverses | character varying(200) | |
| op_sai | Login de la personne ayant saisi les données | character varying(50) | |
| op_maj | Login de la personne ayant modifié les données | character varying(50) | |
| dbinsert | Date d'insertion du PDL | timestamp with time zone | |
| dbupdate | Date de modification du PDL | timestamp with time zone | |
| geom | Géométrie du PDL (point) | geometry(Point,2154) | |

Particularités à noter :
- Clé primaire sur `id_pdl`.
- Contrainte `UNIQUE` sur `code_ref`.
- 3 clés étrangères :
  - `type_fluide` → `lt_patbat_fluide(code)`
  - `compteur_parent` → `geo_patbat_pdl(code_ref)` (auto-référence, `ON DELETE SET NULL ON UPDATE CASCADE` : si le `code_ref` du parent change, les sous-compteurs sont mis à jour automatiquement)
  - `type_compteur` → `lt_patbat_type_compteur(code)`
- 1 trigger : `t_t_verif_type_pdl` (`BEFORE INSERT OR UPDATE`) : vérifie la cohérence de la hiérarchie compteur principal / sous-compteur (existence d'un compteur principal avant de créer un sous-compteur, interdiction qu'un compteur ait lui-même son parent, interdiction de repasser un compteur principal en sous-compteur s'il a encore des sous-compteurs rattachés, etc.).

---

## Classes attributaires

### `r_patbat`.`an_patbat_entite`
Classe attributaire pivot, regroupant les informations communes à toutes les entités du référentiel (site, sous-site, bâtiment, unité fonctionnelle, local).
| Colonne | Description | Type | Valeur par défaut |
| :--- | :--- | :--- | :--- |
| id_entite | Identifiant unique de l'entité | serial | nextval() |
| niveau | Nom du niveau de l'entité : Site, Sous-site, Bâtiment, Unité, Local | character varying(20) | |
| ref_code | Code de référence de l'entité | character varying(50) | |
| nom_usage | Nom d'usage de l'entité | character varying(100) | |
| nom_officiel | Nom officiel de l'entité | character varying(100) | |
| categorie | Catégorie principale de l'entité | character varying(50) | |
| sous_categories | Codes de sous-catégories, séparés par `;` | text | |
| type_etablissement | Types d'établissement de l'entité (`ERP`, `ERT` ou `ERP;ERT`) | character varying(50) | |
| insee | Code INSEE de la commune dans laquelle se trouve l'entité | character varying(5) | |
| commune | Nom de la commune dans laquelle se trouve l'entité | character varying(80) | |
| observ | Commentaires divers | text | |
| op_sai | Login de la personne ayant inséré les données | character varying(50) | |
| op_maj | Login de la personne ayant modifié les données | character varying(50) | |
| dbinsert | Date d'insertion de l'entité | timestamp with time zone | |
| dbupdate | Date de mise à jour de l'entité | timestamp with time zone | |
| dbstatut | Statut de l'entité, gestion corbeille | character varying(2) | '10' |

Particularités à noter :
- Clé primaire sur `id_entite`.
- 2 clés étrangères :
  - `categorie` → `lt_patbat_categorie(code)`
  - `niveau` → `lt_patbat_niveau(valeur)`
- Contrainte `CHECK` : `dbstatut IN ('10', '11')` (`10` = actif, `11` = désactivé/corbeille).
- 3 triggers :
  - `t_t_nettoyage_qgis_multiselect` (`BEFORE INSERT OR UPDATE`) : nettoie le champ `type_etablissement` envoyé par QGIS lors d'une sélection multiple (transforme `{"ERP","ERT"}` en `ERP;ERT`).
  - `t_t_nettoyage_sous_categories` (`BEFORE INSERT OR UPDATE`) : si la `categorie` change, filtre `sous_categories` pour ne garder que les codes appartenant à la nouvelle catégorie.
  - `t_t100_log` (`AFTER INSERT OR UPDATE OR DELETE`) : insère une trace de l'opération dans `an_patbat_log`.

### `r_patbat`.`an_patbat_unite_fonc`
Classe attributaire regroupant les informations des entités de niveau « Unité fonctionnelle ».
| Colonne | Description | Type | Valeur par défaut |
| :--- | :--- | :--- | :--- |
| id_entite | Identifiant de l'entité | integer | |
| id_bati | Identifiant du bâtiment dans lequel se trouve l'unité fonctionnelle | integer | |
| supdvp | Superficie développée de l'unité fonctionnelle | integer | |
| supdvp_mesuree | L'unité fonctionnelle a-t-elle une superficie développée mesurée (fiable) | boolean | FALSE |
| nb_local | Compte du nombre de locaux qui ont été présents dans l'UF, utilisé pour générer le code de référence des locaux (compteur auto-incrémenté) | smallint | 0 |

Particularités à noter :
- Clé primaire sur `id_entite`.
- 2 clés étrangères :
  - `id_entite` → `an_patbat_entite(id_entite)`
  - `id_bati` → `an_patbat_entite(id_entite)`
- 1 trigger : `t_t100_log` (`AFTER INSERT OR UPDATE OR DELETE`).

### `r_patbat`.`an_patbat_local`
Classe attributaire regroupant les informations de tous les locaux.
| Colonne | Description | Type | Valeur par défaut |
| :--- | :--- | :--- | :--- |
| id_entite | Identifiant de l'entité | integer | |
| id_uf | Identifiant de l'unité fonctionnelle dans laquelle se trouve le local | integer | |
| supdvp | Superficie développée du local | integer | |
| supdvp_mesuree | Le local a-t-il une superficie développée mesurée | boolean | FALSE |

Particularités à noter :
- Clé primaire sur `id_entite`.
- 2 clés étrangères :
  - `id_entite` → `an_patbat_entite(id_entite)`
  - `id_uf` → `an_patbat_unite_fonc(id_entite)`
- 1 trigger : `t_t100_log` (`AFTER INSERT OR UPDATE OR DELETE`).

### `r_patbat`.`an_patbat_orga`
Classe attributaire décrivant les organisations (EPCI, communes, bailleurs, prestataires...).
| Colonne | Description | Type | Valeur par défaut |
| :--- | :--- | :--- | :--- |
| id_orga | Identifiant unique de l'organisation | serial | nextval() |
| nom | Nom de l'organisation | character varying(100) | |
| observ | Commentaires divers | text | |
| op_sai | Login de la personne ayant saisi l'organisation | character varying(50) | |
| op_maj | Login de la personne ayant modifié l'organisation | character varying(50) | |
| dbinsert | Date d'insertion de l'organisation | timestamp with time zone | |
| dbupdate | Date de mise à jour de l'organisation | timestamp with time zone | |
| dbstatut | Statut de l'organisation, gestion corbeille | character varying(2) | '10' |

Particularités à noter :
- Clé primaire sur `id_orga`.
- Contrainte `CHECK` : `dbstatut IN ('10', '11')`.

### `r_patbat`.`an_patbat_contact`
Classe stockant les informations des contacts (personnes physiques rattachées aux entités).
| Colonne | Description | Type | Valeur par défaut |
| :--- | :--- | :--- | :--- |
| id_contact | Identifiant unique du contact | serial | nextval() |
| denomination | Nom du contact | character varying(100) | |
| tel | Numéro de téléphone du contact | character varying(11) | |
| mobile | Numéro de mobile du contact | character varying(11) | |
| email | Email du contact | character varying(150) | |
| organisme | Identifiant de l'organisme du contact | smallint | |
| observ | Commentaires divers | character varying(200) | |
| op_sai | Login de la personne ayant saisi le contact | character varying(50) | |
| op_maj | Login de la personne ayant modifié le contact | character varying(50) | |
| dbinsert | Date d'insertion du contact | timestamp with time zone | |
| dbupdate | Date de mise à jour du contact | timestamp with time zone | |
| dbstatut | Statut du contact, gestion corbeille | character varying(2) | '10' |

Particularités à noter :
- Clé primaire sur `id_contact`.
- 1 clé étrangère : `organisme` → `an_patbat_orga(id_orga)`.
- Contrainte `CHECK` : `dbstatut IN ('10', '11')`.

### `r_patbat`.`an_patbat_parcelle`
Table de lien entre les entités géographiques et les parcelles cadastrales sur lesquelles elles se situent (alimentée automatiquement, cf. triggers `t_t3_parcelles`).
| Colonne | Description | Type | Valeur par défaut |
| :--- | :--- | :--- | :--- |
| gid | Identifiant unique du lien entre la parcelle et l'entité | serial | nextval() |
| id_entite | Identifiant de l'entité | integer | |
| insee | Code INSEE de la parcelle | character varying(5) | |
| commune | Commune de la parcelle | character varying(80) | |
| geo_parcelle | Identifiant de la parcelle | text | |
| section | Section de la commune dans laquelle se trouve la parcelle | text | |
| numero | Numéro de la parcelle | text | |

Particularités à noter :
- Clé primaire sur `gid`.
- Table alimentée automatiquement par les triggers `t_t3_parcelles` posés sur `geo_patbat_site`, `geo_patbat_ssite` et `geo_patbat_bati` (voir section Fonctions transverses).

### `r_patbat`.`an_patbat_log`
Table contenant les logs des opérations réalisées sur les tables des entités.
| Colonne | Description | Type | Valeur par défaut |
| :--- | :--- | :--- | :--- |
| idlog | Identifiant de la ligne de log | integer | valeur issue de `patbat_log_seq` |
| tablename | Nom de la table sur laquelle porte le log | character varying(80) | |
| type_ope | Type d'opération du log (`INSERT`/`UPDATE`/`DELETE`) | text | |
| dataold | Données datant d'avant l'opération (ligne complète concaténée) | text | |
| datanew | Données après l'opération (ligne complète concaténée) | text | |
| dbupdate | Date de l'opération | timestamp | now() |

Particularités à noter :
- Clé primaire sur `idlog`.
- Alimentée par le trigger `ft_m_patbat_log()` posé sur `an_patbat_entite`, `geo_patbat_site`, `geo_patbat_ssite`, `geo_patbat_bati`, `an_patbat_unite_fonc` et `an_patbat_local` (trigger `t_t100_log`, `AFTER INSERT OR UPDATE OR DELETE`).

### `r_patbat`.`an_patbat_media`
Structure de table « modèle » utilisée par le module média de GEO pour la saisie de documents joints par les utilisateurs (photos, plans, notices, factures...).
| Colonne | Description | Type | Valeur par défaut |
| :--- | :--- | :--- | :--- |
| gid | Identifiant interne | serial | nextval() |
| id | Identifiant de la table liée | integer | |
| type_media | Code du type de média | character varying(2) | |
| media | Champ Média de GEO | text | |
| miniature | Champ miniature de GEO | bytea | |
| n_fichier | Nom du fichier | text | |
| t_fichier | Type de média dans GEO | text | |
| observ | Commentaires divers | text | |
| op_sai | Login de la personne ayant saisi le média | character varying(50) | |
| dbinsert | Date d'insertion du média | timestamp with time zone | |

Particularités à noter :
- 1 clé étrangère : `type_media` → `lt_type_media(code)`.

