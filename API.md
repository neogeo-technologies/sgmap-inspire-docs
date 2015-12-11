# API Inspire

## Contexte

Initiée en 2014 puis lancée en mars 2015, la [passerelle Inspire](https://inspire.data.gouv.fr) a fait l'objet de nombreux développements techniques.

Après plus de 6 mois de fonctionnement régulier, il est temps de transformer la passerelle Inspire en une véritable plateforme, utilisable par le plus grand nombre.

## Que fait la passerelle ?

### Situation actuelle

* Référencement de catalogues CSW
* Moissonnage de catalogues CSW (format ISO 19139 uniquement)
* Conversion ISO 19139 vers JSON interne
* Nettoyage et validation des métadonnées
* Moissonnage de services WFS
* Identification des liens données / services
* Vérification des ressources distantes (check des URLs)
* Identification des jeux de données Shapefile et MapInfo (archives Zip)
* Normalisation partielle des noms des organisations
* Qualification de l'ouverture et la disponibilité des données
* Recherche dans les catalogues référencés
* Recherche globale (unification et dédoublonnage)
* Conversion et téléchargement de données à la volée
* Publication des données sur data.gouv.fr

### Développements en cours

* Prise en charge de nouveaux champs ISO 19139 (emprise, aspects légaux)

### Développements planifiés

* Prise en charge de la licence ODbL (demande de Rennes Métropole)
* Prise en charge des métadonnées Dublin Core (demande Grand Lyon)
* Scan des dossiers FTP (demande du [CRAIG](http://www.craig.fr/))
* Prise en charge des formats d'imagerie (en particulier l'orthophotographie)
* Indicateur de disponibilité des services WFS

### Développements futurs

* Archivage des métadonnées XML
* Moissonnage des services WMS

## Projets de standardisation

* Création d'un schéma JSON pour les métadonnées, compatible ISO 19139
