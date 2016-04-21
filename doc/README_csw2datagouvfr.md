# La passerelle INSPIRE -> data.gouv.fr

La passerelle INSPIRE est un outil mis à disposition des administrateur de données géographiques _"Inspire"_ pour leur permettre d'automatiser la publication de leurs données dans data.gouv.fr.

L'objectif de ce document est de la présenter et d'en expliquer le fonctionnement.

## Qu'est-ce que la passerelle Inspire ?

Il s'agit d'un ensemble d'outils permettant de publier des jeux de données géographiques _"Inspire"_  sur la plateforme des données publiques françaises, [data.gouv.fr](https://www.data.gouv.fr).

Elle répond à plusieurs objectifs :

* Mieux valoriser l’information géographique via l’Open Data
* S’appuyer sur les infrastructures existantes et sur Inspire
* Disposer d’un outil de suivi des données performant

Elle s'appuie sur plusieurs standards mis en œuvre dans le cadre de la directive Inspire ainsi que sur quelques formats additionnels :

* Standard de service web de catalogage : CSW 2.0.2
* Standard de service web pour la publication des données : WFS (toutes versions)
* Formats de métadonnées :  ISO 19115 et 19139, Dublin-Core

D'un point de vue opérationnel, la passerelle Inspire moissonne un ensemble de catalogues CSW considérés comme pertinents et analyse les métadonnées collectées. L'analyse porte essentiellement sur la __détection des conditions d'utilisation__ et de réutilisation des données et sur la __détection de l'accessibilité en ligne effective__ des données.

Lorsqu'un jeu de données est compatible avec les critères de la plateforme [data.gouv.fr](https://www.data.gouv.fr), il peut être publié par l'un des organismes point de contact, à l'aide d'un outil dédié qui réalise les opérations suivantes :

* Création et mise à jour de la fiche descriptive
* Création à la volée de versions GeoJSON, CSV et Shapefile du jeu de données, avec reprojection le cas échéant
* Collecte d'informations sur l'utilisation des données

La passerelle est développée en mode agile au sein de l'[Incubateur de Services Numériques](https://beta.gouv.fr) du [Secrétariat Général pour la Modernisation de l'Action Publique](http://modernisation.gouv.fr).

Elle s'appuie exclusivement sur des briques Open-Source, et notamment :

* [Node.js](https://nodejs.org/en/) : moteur d'exécution JavaScript côté serveur
* [MongoDB](https://www.mongodb.org/) : base de données _orientée document_
* [Redis](http://redis.io/) : base de données _orientée clé/valeur_
* [GDAL](http://www.gdal.org/) : couche d'accès aux données géographiques
* [uData](https://github.com/etalab/udata) : plateforme sociale de gestion de données

## Pré-requis applicables aux données

Afin que vos données puissent être intégrées à data.gouv.fr via la passerelle, il faut qu'elles disposent de métadonnées et que celles-ci remplissent tous les critères suivants :

* avoir le mot-clé `données ouvertes` (voir [étiqueter](/fr/features/documentation/md_classify.html)) ;
* avoir une licence ouverte et indiquer qu'il n'y a aucune limitation au sens INSPIRE (voir [gérer les CGUs](/fr/features/documentation/md_cgu.html#conditions)) ;
* être présentes dans un catalogue librement accessible via CSW (voir [serveur CSW](/fr/features/publish/csw_server.html)) ;
* contenir au moins un lien de téléchargement opérationnel.

Les liens de téléchargement reconnus par la passerelle sont :

* lien vers un service WFS (voir [affecter un service WFS](/fr/features/publish/webservices.html#associer-un-flux-wfs)) ;
* lien vers des fichiers de données vecteur aux formats GeoJSON, Shapefile, MapInfo MIF/MID, MapInfo TAB et GML ;
* lien vers des fichiers de données raster aux formats ECW, JPEG2000 et GeoTIFF.

Les liens vers des fichiers PDF ne sont pas reconnus comme des liens vers des données.

## Utilisation pas à pas

### Compte et organisation sur data.gouv.fr
Pour publier des données via la passerelle, il est nécessaire de disposer d'un compte compte individuel sur data.gouv.fr et de l'associer à une organisation.

1. Créer un compte sur data.gouv.fr

    Pour créer un compte ou se connecter : https://www.data.gouv.fr/login. Il est recommandé de créer un compte directement sans l'interface d'un réseau social.

    ![DataGouv - Inscription/connexion](/img/annex_bridge_INSPIRE_DataGouv_00a.png "Se connecter ou créer un compte sur DataGouv")

2. Créer / rejoindre une organisation sur data.gouv.fr

    Pour cela, il faut passer par l'administration de son profil : https://www.data.gouv.fr/fr/admin/organization/new/. Si elle existe déjà, faites une demande pour la rejoindre.

    ![DataGouv - Organisation](/img/annex_bridge_INSPIRE_DataGouv_00b_NewOrganization.png "Créer son organisation sur DataGouv")

### Référencement et moissonnage d'un flux CSW

1. Demander à ce que votre flux CSW soit référencé

    Pour référencer votre flux CSW, écrivez à [inspire@data.gouv.fr](mailto:inspire@data.gouv.fr?subject=Ajout d'un service CSW pour diffusion synchronisée sur DataGouv&cc=projets@isogeo.fr) en indiquant votre compte data.gouv.fr, votre / vos organisation(s) et bien sûr le(s) flux concerné(s).

2. Lancer le moissonnage de son catalogue

    Une fois votre flux CSW référencé par l'équipe de data.gouv.fr,  lancez le moissonnage. Pour cela, [se rendre sur la page des flux](https://inspire.data.gouv.fr/services/by-protocol/csw) et cliquer sur `Synchroniser` en regard de votre service.

    ![Passerelle INSPIRE - Open Data (1)](/img/annex_bridge_INSPIRE_DataGouv_1a_syncCSW.png "Page d'accueil de la passerelle")

3. Vérifier le moissonnage

    Une fois la synchronisation terminée (actualiser la page au bout de quelques minutes selon le nombre de métadonnées à moissonner), ouvrir la page détaillée du service.
    
    Plusieurs filtres facilitent la consultation des métadonnées moissonnées :
    
* `Disponibilité = Oui` : limite l'affichage aux métadonnées dont les données sont accessibles (cf. [prérequis](/fr/appendices/bridge_csw2datagouvfr.html#pr-requis))
* `Type de résultat = Jeu de données ou Jeu de données (non géographiques)` : en choisissant 'Jeu de données', seules les métadonnées publiées à l'origine en ISO 19139 sont affichées ; en choisissant 'Jeu de données (non géographiques)', seules les métadonnées publiées à l'origine en Dublin Core sont affichées
* `Donnée ouverte = Oui` : limite l'affichage aux données ouvertes dont la licence est reconnue par data.gouv.fr. Exemples de licences non reconnues par data.gouv.fr : la licence engagée et la licence associée du Grand-Lyon
* `Publié sur data.gouv.fr = Oui` : identifie les métadonnées moissonnées par la passerelle et déjà publiées sur data.gouv.fr

    ![Passerelle INSPIRE - Open Data (1)](/img/annex_bridge_INSPIRE_DataGouv_1b_serviceDetails.png "Page d'accueil de la passerelle")

    Si une donnée semble ne pas être disponible, revérifier les [prérequis](/fr/appendices/bridge_csw2datagouvfr.html#pr-requis) puis [contacter l'équipe data.gouv.fr](mailto:inspire@data.gouv.fr?subject=Problème de moissonnage d'un CSW (Isogeo)&cc=projets@isogeo.fr).

### Association et publication

1. Aller sur https://inspire.data.gouv.fr/

    ![Passerelle INSPIRE - Open Data (1)](/img/annex_bridge_INSPIRE_DataGouv_1.png "Page d'accueil de la passerelle")

2. Autoriser la passerelle à utiliser le compte data.gouv.fr

    ![Passerelle INSPIRE - Open Data (2)](/img/annex_bridge_INSPIRE_DataGouv_2_oauth.png "Lier son compte DataGouv")

3. Choisir l'organisation à configurer

    ![Passerelle INSPIRE - Open Data (3)](/img/annex_bridge_INSPIRE_DataGouv_3_LinkOrga.png "Choisir parmi ses organisations")

4. Associer le catalogue moissonné

    Dans la liste, choisir le catalogue correspondant au flux que vous avez référencé précédemment.

    ![Passerelle INSPIRE - Open Data (4)](/img/annex_bridge_INSPIRE_DataGouv_4_PickCatalog.png "Choisir parmi les catalogues sources référencés")

5. Choisir les producteurs à associer à ce catalogue

    Il s'agit de faire correspondre les contacts renseignés dans la métadonnée et le producteur identifié de la donnée. Par exemple, l'administrateur d'une IDG peut indiquer à quels ayant-droits correspondent quelles données.

    ![Passerelle INSPIRE - Open Data (4)](/img/annex_bridge_INSPIRE_DataGouv_6_producerMatched.png "Choisir parmi les producteurs à associer")

6. Synchroniser le catalogue pour obtenir les données prêtes à être publiées

    ![Passerelle INSPIRE - Open Data (4)](/img/annex_bridge_INSPIRE_DataGouv_7b_syncRunning.png "Choisir parmi les producteurs à associer")

7. Gérer la publication des données sur data.gouv.fr

    3 statuts sont possibles :
    * `Données attente de publication`, les nouvelles données recensées qui attendent une action de votre part ;
    * `Données en mode privé`, visibles uniquement par les membres de votre organisation ;
    * `Données publiées`, visibles publiquement sur data.gouv.fr.

    ![Passerelle INSPIRE - Open Data (7)](/img/annex_bridge_INSPIRE_DataGouv_9_dataPublishedBack.png "Régler le niveau de publication des données sur le portail DataGouv")
