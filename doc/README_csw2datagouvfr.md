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

* avoir le mot-clé `données ouvertes` ;
* avoir une licence ouverte et indiquer qu'il n'y a aucune limitation au sens INSPIRE ;
* être présentes dans un catalogue librement accessible via CSW ;
* contenir au moins un lien de téléchargement opérationnel.

Les liens de téléchargement reconnus par la passerelle sont :

* lien vers un service WFS ;
* lien vers des fichiers de données vecteur aux formats GeoJSON, Shapefile, MapInfo MIF/MID, MapInfo TAB et GML ;
* lien vers des fichiers de données raster aux formats ECW, JPEG2000 et GeoTIFF.

Les liens vers des fichiers PDF ne sont pas reconnus comme des liens vers des données.

## Utilisation pas à pas

Pour publier des métadonnées via la passerelle Inspire vous devez suivre les étapes suivantes :

1. Vérifiez que vous disposez d'un compte sur data.gouv.fr et qu'il est associé à une organisation référencée ;
2. Référencez votre flux CSW et vérifiez que le moissonnage est opérationnel ;
3. Associez des producteurs référencés dans les métadonnées de votre flux CSW à votre organisation ;
4. Publier les métadonnées pertinentes sur data.gouv.fr

Ces étapes sont détaillées dans les chapitres suivants.


### Créer un compte et rejoindre une organisation sur data.gouv.fr
Pour publier des données via la passerelle, vous devez disposer d'un compte compte individuel sur data.gouv.fr et de l'associer à une organisation.

#### Créer un compte sur data.gouv.fr

Pour créer un compte ou se connecter : https://www.data.gouv.fr/login. Il est recommandé de créer un compte directement sans l'interface d'un réseau social.

![DataGouv - Inscription/connexion](/img/annex_bridge_INSPIRE_DataGouv_00a.png "Se connecter ou créer un compte sur DataGouv")

#### Créer / rejoindre une organisation sur data.gouv.fr

Pour cela, il faut passer par l'administration de votre profil : https://www.data.gouv.fr/fr/admin/organization/new/. Si elle existe déjà, faites une demande pour la rejoindre.

![DataGouv - Organisation](/img/annex_bridge_INSPIRE_DataGouv_00b_NewOrganization.png "Créer son organisation sur DataGouv")


### Référencer et moissonner un flux CSW

#### Demander à référencer votre flux CSW

Pour référencer le flux CSW de votre catalogue, écrivez à [inspire@data.gouv.fr](mailto:inspire@data.gouv.fr?subject=Ajout d'un service CSW pour diffusion synchronisée sur DataGouv&cc=projets@isogeo.fr) en indiquant votre compte data.gouv.fr, votre / vos organisation(s) et bien sûr le(s) flux concerné(s).

#### Lancer le moissonnage de votre catalogue

Une fois votre flux CSW référencé par l'équipe de data.gouv.fr,  lancez le moissonnage. Pour cela :
    
- rendez vous sur la [page d'accueil de la passerelle Inspire](https://inspire.data.gouv.fr/) ;
- cliquez sur le bouton [Consulter les catalogues](https://inspire.data.gouv.fr/services/by-protocol/csw) ;
- puis cliquez sur `Synchroniser` en regard de votre service.

![Passerelle INSPIRE - Open Data (1)](/img/annex_bridge_INSPIRE_DataGouv_1a_syncCSW.png "Page d'accueil de la passerelle")

#### Vérifier le moissonnage

Une fois la synchronisation terminée (actualisez la page au bout de quelques minutes selon le nombre de métadonnées à moissonner), ouvrez la page détaillée du service.
    
Plusieurs filtres facilitent la consultation des métadonnées moissonnées :
    
* `Disponibilité = Oui` : limite l'affichage aux métadonnées dont les données sont accessibles (cf. [prérequis](#pré-requis-applicables-aux-données))
* `Type de résultat = Jeu de données ou Jeu de données (non géographiques)` : en choisissant 'Jeu de données', seules les métadonnées publiées à l'origine en ISO 19139 sont affichées ; en choisissant 'Jeu de données (non géographiques)', seules les métadonnées publiées à l'origine en Dublin Core sont affichées
* `Donnée ouverte = Oui` : limite l'affichage aux données ouvertes dont la licence est reconnue par data.gouv.fr. Exemples de licences non reconnues par data.gouv.fr : la licence engagée et la licence associée du Grand-Lyon
* `Publié sur data.gouv.fr = Oui` : identifie les métadonnées moissonnées par la passerelle et déjà publiées sur data.gouv.fr

![Passerelle INSPIRE - Open Data (1)](/img/annex_bridge_INSPIRE_DataGouv_1b_serviceDetails.png "Page d'accueil de la passerelle")

Si une donnée semble ne pas être disponible, revérifiez les [prérequis](#pré-requis-applicables-aux-données) puis [contactez l'équipe data.gouv.fr](mailto:inspire@data.gouv.fr?subject=Problème de moissonnage d'un CSW (Isogeo)&cc=projets@isogeo.fr).

### Associer des producteurs à votre organisation

#### S'authentifier sur la passerelle

![Passerelle INSPIRE - Open Data (1)](/img/annex_bridge_INSPIRE_DataGouv_1.png "Page d'accueil de la passerelle")
        
- rendez vous sur la [page d'accueil de la passerelle Inspire](https://inspire.data.gouv.fr/) ;
- cliquez sur le bouton [C'est par ici !](https://inspire.data.gouv.fr/services/by-protocol/csw) ;
- autorisez la passerelle à utiliser votre compte data.gouv.fr

![Passerelle INSPIRE - Open Data (2)](/img/annex_bridge_INSPIRE_DataGouv_2_oauth.png "Lier son compte DataGouv")

#### Associer des producteurs à votre organisation

![Passerelle INSPIRE - Open Data (3)](/img/annex_bridge_INSPIRE_DataGouv_3_LinkOrga.png "Choisir parmi ses organisations")

- dans la liste des organisations associées à votre compte, cliquez sur le nom de l'organisation à configurer ;
- vérifiez que votre organisation est associée au bon catalogue. Si ce n'est pas le cas, cliquez sur le bouton "Modifier" et sélectionner le catalogue que vous avez demandé de référencer précédemment ;
- cliquez sur le bouton "Associer des producteurs" et sélectionner les producteurs pour lesquels vous assumerez la publication des métadonnées. 

![Passerelle INSPIRE - Open Data (4)](/img/annex_bridge_INSPIRE_DataGouv_6_producerMatched.png "Choisir parmi les producteurs à associer")
    
Lors de cette dernière étape, vous ne devez pas sélectionner des producteurs dont vous n'avez pas la responsabilité. En effet, une fois que vous aurez associé un producteur à votre organisation aucune autre organisation ne pourra l'associer à son propre compte. Vous ne devez donc pas associer à votre organisation des producteurs dont la politique de publication doit être assurer indépendamment de la vôtre. Typiquement, n'associer pas l'IGN, le BRGM, l'INSEE ou d'autres producteurs de données de ce type si vous ne faites pas partie de ces organismes. Par contre, il peut être très pertinent qu'un EPCI prenne en charge la publication des données pour le compte de ses communes.


### Publier des métadonnées sur data.gouv.fr

#### Accéder à la page de votre organisation

- rendez vous sur la page de votre organisation dans la passerelle : [Sélection de l'organisation](https://inspire.data.gouv.fr/account/organizations) ;
- dans la liste des organisations associées à votre compte, cliquez sur le nom de l'organisation associée au catalogue dont vous voulez publier des données.

#### Vérifier l'état de publication de vos données

Le premier cadre au haut de la page dresse un état des lieux des données publiables au sens de data.gouv.fr :

- les données déjà publiées et accessibles sur data.gouv.fr ;
- les données en attente de publication : les données vérifiant les [prérequis](#pré-requis-applicables-aux-données), issues de producteurs associés à votre organisme et qui n'ont pas encore été publiées (elles sont en attente d'une action de votre part).

Les données qui ne vérifient pas les prérequis et qui ne sont pas issues de producteurs associés à votre organisme n'arrapaissent pas dans cette page.

#### Publier des données en attente de publication

- depuis la page précédente, cliquez sur le bouton "Publier des données" ;
- le premier cadre en haut de la page identifie l'ensemble des données en attente d'être publiées. Cliquez sur le lien "Publier" d'un jeu de données particulier ou sur le bouton "Publier toutes les données" pour publier ces données sur data.gouv.fr

![Passerelle INSPIRE - Open Data (7)](/img/annex_bridge_INSPIRE_DataGouv_9_dataPublishedBack.png "Publier des données sur le portail DataGouv")

## Problèmes fréquemments rencontrés

### Vos métadonnées sont dans votre catalogue CSW mais ne sont pas visibles dans data.gouv.fr

Plusieurs raisons peuvent expliquer qu'une fiche de métadonnées présente dans un catalogue CSW ne soit pas visible dans data.gouv.fr :

- votre catalogue CSW n'est peut-être pas référencé par inspire.data.gouv.fr
- il est possible qu'aucune organisation référencée dans data.gouv.fr ne soit associée à votre catalogue
- le producteur de vos données n'est peut-être associé à aucune organisation référencée dans data.gouv.fr
- vos métadonnées sont peut-être en attente de publication dans inspire.data.gouv.fr
- vos métadonnées sont peut-être moissonnées mais les données ne sont peut-être pas considérées comme ouverte ni disponibles par la passerelle
- le moissonnage de votre catalogue nécessite peut-être d'être relancé manuellement si vos métadonnées ont été mises à jour très récemment dans votre catalogue
- vos métadonnées ne sont peut-être pas accessibles via votre service CSW (elles nécessitent peut-être d'être publiées dans votre catalogue)


### Vos métadonnées sont moissonnées mais ne sont pas visibles dans la liste des données en attente de publication

Pour que vous puissiez les publier, vos métadonnées doivent :

- ne pas avoir déjà été publiées précédemment
- vérifier des [prérequis](#pré-requis-applicables-aux-données)
- être issues de producteurs associés à votre organisation

Vérifiez donc :

- que vos métadonnées remplissent bien les prérequis ;
- que le producteur référencé dans vos métadonnées est bien associé à votre organisation déclarée dans inspire.data.gouv.fr. Si ce n'est pas le cas :
	- s'il est légitime que toutes les métadonnées soitent publiées par vous pour le compte du producteur, ajoutez-le à la liste des producteurs associés à votre organisation,
	- sinon, ne publiez pas ses métadonnées.


### Vous ne pouvez pas ajouter un producteur à votre organisation

Un producteur ne peut être associé qu'à une seule organisation déclarée dans data.gouv.fr. Si un producteur n'apparait pas dans la liste des producteurs associable à votre organisation cela peut provenir de plusieurs causes :

- le producteur en question a déjà été associé à une organisation (peut-être de manière erronée ou abusive) ;
- le producteur n'apparaît pas dans les fiches de métadonnées publiables sur data.gouv.fr. Vérifiez alors l'orthographe du producteur dans vos fiches de métadonnées et vérifiez les prérequis applicables aux métadonnées. Le producteur peut ne pas apparaître si, par exemple, aucune de ses données n'est considérée comme ouverte et accessible en ligne.