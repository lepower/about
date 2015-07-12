Goals
=====

.. note::
  
  To ease the comprehension between current team members and because of probable
  multiple changes, this part will be written in French_.


Objectifs initiaux
~~~~~~~~~~~~~~~~~~

Nous voulons réaliser un **appareil de mesure** de:

- courant (intensité en Ampères)
- tension (en Volts)
- phase (en Hertz)

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
dont les plans hardware sont **ouverts**, communiquant avec un protocole **open source**
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

- Augmentation légère de la phase: le réseau est en surproduction par rapport à sa consommation,
  la plateforme pourrait ordonner aux appareils de stockage d'énergie (batteries, chauffe-eau,
  pompes à eau) de se mettre en route;
- Diminution légère de la phase: sous-production par rapport à la demande, il faut couper des
  appareils non critiques (radiateurs électriques, cumulus, ...) et déclencher la mise en
  contact d'autres sources de production. Cela peut prévenir de larges pannes, notamment
  dans les PVD où le réseau ne suit pas forcément le développement économiques des ménages.



Premier Prototype
~~~~~~~~~~~~~~~~~

- Réaliser a minima une simulation de l'échange de données entre une future prise et une gateway BLE.
- Identifier ce qui peut être mesuré, avec quelle précision, avec quelle fréquence.
- Réaliser une implémentation simple sur carte nRF51/52 qui réalise là encore l'écange de données.
- Identifier les éventuels soucis lors de l'utilisation d'un chip de mesure externe connecté au CPU BLE.
  (occupation mémoire ? pins GPIO accessibles ? Fréquence de polling ? Interruptions possibles ?)
- Réaliser une gateway s'appuyant sur une bibliothèque C capable de:

    * Faire fonctionner la découverte et/ou l'appairage
    * Afficher les données remontées par chaque capteur
    * Envoyer des ordres éventuels.

Dans un second temps, il faudra réaliser un prototype avec chip de mesure que l'on peut brancher
(même de manière infâme) à au moins un appareil. De là, étudier l'utilisation de l'appareil et
ce que l'on peut améliorer sans perdre en vision.


A résoudre
~~~~~~~~~~

Appairage
---------

Doit-on:

1. Mettre notre gateway en mode écoute et demander à l'utilisateur d'appuyer sur un bouton
   d'appairage dans un temps donné ?
2. Mettre nos appareils en mode écoute et lancer une détection *broadcast* BLE ?

Le premier cas semble préférable pour améliorer l'expérience d'appairage lorsque plusieurs
capteurs sont présents. Dépend de ce qui est prévu par la norme BLE.


Trouver des noms
----------------

- A l'appareil (PowerPlug ? LePlug? ...)
- Au protocole applicatif au-dessus de BLE entre le plug et la gateway.

Concernant l'appareil
---------------------

Etant donné que l'objectif 1 est de répondre à des questions simples entre nous, type:

- Quelle est la puissance consommée par ma TV sur une semaine ?
- Comment comparer cela avec la consommation de ma chambre ?

... une des premiers choix hardware concernera l'intervalle de puissance auquel on s'intéresse
pour le premier prototype. Nous mesurerons probablement des puissantes de 10W (lampe) à 3000W
(aspirateur). Quelle méthode de mesure/chip adopter pour rester précis sur cet intervalle étendu ?

Acteurs et écosystème
---------------------

Mieux connaître les acteurs, protocoles, nonprofit... aujourd'hui existants. Points forts,
points faibles ? Où se place-t-on ? Notre produit a-t-il toujours un intérêt ? Quelle est sa
différence ? Si des projets équivalent existent, les rejoindre ? Identifier la différence ?

On veut savoir si:

- le produit a une chance de servir étant donnés les centaines de protocoles & hardware déjà
  existants dans le secteur (cf. Legrand Smart Building)
- nous ne devenions pas une énième startup qui "révolutionne le smart grid" mais quelque chose
  de vraiment utile.


.. _French: https://en.wikipedia.org/wiki/French_language
