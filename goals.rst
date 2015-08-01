Goals
=====

.. note::

  To ease the comprehension between current team members and because of probable
  multiple changes, this part will be written in French_.


Objectifs initiaux
~~~~~~~~~~~~~~~~~~

Nous voulons réaliser un **appareil de mesure** de:

- courant ou intensité (Ampères)
- tension (Volts)
- fréquence (Hertz)

Situé à la frontière entre la prise commandée, le watt-mètre et les objets connectés,
cet appareil permettrait entre autres:

- de mesurer la puissance instantanée consommée par certains appareils
- de détecter des variations de phase rapidement

  * éventuellement prévenir des pannes en cas de sous-production
  * stocker de l'énergie en cas de surproduction
  * revendre  de l'énergie en cas de sous-production

- d'allumer/éteindre certains appareils, de la manière la plus intelligente possible.

Nous pouvons alors imaginer connaître précisément notre consommation, que ce soit de
manière instantanée sur un appareil ou dans une pièce entière; sur un mois ou une semaine,
à certaines périodes de la journée, etc.


Vision à long terme
~~~~~~~~~~~~~~~~~~~

La connaissance de notre consommation précise, à l'aide d'appareils **très peu coûteux**,
dont les plans hardware sont **ouverts**, communiquant à l'aide d'un protocole **open source**
simple, ouvre la voie à beaucoup de possibilités.

Création d'un écosystème autour de la donnée énergétique
--------------------------------------------------------

De nombreux acteurs pourront créer leurs propres appareils de mesure compatibles avec d'autres
types de prises, plus efficaces pour certains cas particuliers, tout simplement plus beaux
et donc plu répandus, etc.

D'autres se lanceraient dans la création de plateformes d'échange de données, au niveau d'un
quartier, d'une ville, d'un réseau électrique indépendant ou entre particuliers, tout simplement
pour "noter" les appareils les plus consommateurs ou les publicités mensongères.

Des opérateurs énergétiques indépendants pourraient voir le jour, avec notamment des offres
très ciblées ou prépayées.

Connaître sa consommation exacte pourrait rentrer dans les moeurs, et motivera d'autant plus
l'achat de panneaux solaires/batteries/se lancer sur le marché de la production citoyenne
d'énergie alors que cela peut sembler encore très nébuleux aujourd'hui.

Permettre un réseau électrique plus intelligent
-----------------------------------------------

Ces prises connectées, via un partage de données à travers une gateway vers une plateforme
donnée (qui pourrait être une simple instance dans un village énergétiquement autosuffisant
par exemple) peuvent détecter des variations de phase.

- **Augmentation légère** de la phase: le réseau est en surproduction par rapport à sa consommation,
  la plateforme pourrait ordonner aux appareils de stockage d'énergie (batteries, chauffe-eau,
  pompes à eau) de se mettre en route;

- **Diminution légère** de la phase: sous-production par rapport à la demande, il faut couper des
  appareils non critiques (radiateurs électriques, cumulus, ...) et déclencher la mise en
  contact d'autres sources de production. Cela peut prévenir de larges pannes, notamment
  dans les PVD où le réseau ne suit pas forcément le développement économiques des ménages.



Premier Prototype
~~~~~~~~~~~~~~~~~

- Il nous faut une board avec laquelle commencer le développement.

  * Choix d'un système de prise de mesure
  * Choix d'un chip Wi-Fi à brancher sur STM32
  * Design d'une board minimale (sur Kicad + Upverter) avec un STM32 + chip Wi-Fi + chip de mesure

- Premiers développements sur cette board

  * Réaliser des mesures: attention certaines (fréquence notamment) semblent un peu touchy à réaliser !
  * Simuler l'appairage: un SSID privé émis, un serveur interne pour recevoir la configuration, un
    fonctionnement interne pour tenter l'appairage un certain nombre de fois...

- Premiers développements côté serveur

  * Recherche de plateformes & protocoles existants pour recevoir les données
  * Plateforme très simple de collecte de données, si cela n'existe pas déjà
  * Premières applications simples pour donner du sens aux données brutes, essayer la commande


A résoudre
~~~~~~~~~~

Couche réseau
~~~~~~~~~~~~~

Il faut garder à l'esprit que le Wi-Fi n'est actuellement qu'un des protocoles de transport
possibles; il rend plus complexe l'appairage et a une portée moyenne mais permet de ne pas
développer de gateway.

D'autres versions pourraient fonctionner sur chip radio, à la façon des `Open Energy Monitor`_
qui utilisent des adaptateurs pour différentes
`fréquences radio<http://openenergymonitor.org/emon/buildingblocks/which-radio-module>`_
(868MHz, 433MHz, ...)


Trouver des noms
----------------

- A l'appareil (PowerPlug? LePlug? MindPlug?...)
- A la ou les plateforme d'agrégation en ligne, si besoin;
- Au protocole applicatif entre la prise (ou plus tard, la gateway) et la plateforme d'agrégation.

Concernant l'appareil
---------------------

L'objectif primaire est d'avoir un premier prototype à tester dans des cas simples, tels que:

- Quelle est la puissance consommée par tel ou tel appareil ?

  * Machine à laver en mode économique
  * Réfrigérateur suivant la température de la pièce
  * TV allumée ou en veille

- Comment rendre simple un appairage Wi-Fi dans le cas de ces objets ?

... une des premiers choix hardware concernera l'intervalle de puissance auquel on s'intéresse
pour le premier prototype. Nous mesurerons probablement des puissantes de 10W (lampe) à 3000W
(aspirateur).

Quelle sera la précision attendue ou nécessaire ? Quelle est celle des produits du marché ?


Chip de mesure
--------------

- OpenEnergyMonitor utilise des clamps (SCT-013-000) pour la mesure de courant et une résistance
  de shunt pour le potentiel. La fréquence est mesurée en prenant 53 mesures par cycle, soit environ
  2000 mesures par seconde. Problème, cela nécessite une alimentation externe et est hors de prix.
  Par contre pas besoin de séparation des circuits, de redresseur, etc.

- Des tas d'autres prises connectées existent `dans le commerce<https://www.aruco.com/tag/prise-connectee/>`_.
  Il serait intéressant de passer commande et d'analyser les circuits d'alimentation.

- Alexis a des pistes de chip de mesure que nous pourrions réutiliser, cf `lepower/hardware`_.

- Il est proposé de poser la question à des connaissances (A.M) pour en savoir plus à ce sujet.

Acteurs et écosystème
---------------------

Mieux connaître les centaines d'acteurs, protocoles, nonprofit... aujourd'hui existants.
Quels sont leurs points forts, points faibles ? Où se place-t-on dans ce milieu ?
Notre produit a-t-il toujours un intérêt ? Quelle est sa différence ?
Si des projets équivalent existent, les rejoindre ? Identifier la différence ?


.. _French: https://en.wikipedia.org/wiki/French_language
.. _Open Energy Monitor: http://openenergymonitor.org/emon/
.. _lepower/hardware: https://github.com/lepower/hardware
