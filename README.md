Université du Québec à Montréal - UQAM

**TP2 - Intégration des données vectorielles et matricielles**

Travail présenté comme exigence partielle dans le cadre du cours

INTÉGRATION ET VISUALISATION DE DONNÉES GÉOGRAPHIQUES (GEO7630)

Responsable : Clément GLOGOWSKI

Par Charles CLÉMENT & Eliott LIBNER

Jeudi 7 Mars 2024

\-----

**QUALITÉ ENVIRONNEMENTALE DES FLUX DE VÉLOS À MONTRÉAL** 

 

**RÉSUMÉ DU PROJET :** 

L’objectif du projet est de mettre en avant la qualité environnementale des principaux flux de vélos au sein de la Ville de Montréal. Ainsi, en agrégeant diverses données (qualité de l’air, îlots de chaleur urbain, mesures acoustiques, collisions routières) sous forme d’un indice intégrée, il sera possible de visualiser les zones, tronçons de routes et/ou pistes cyclables où un trajet à vélo possède le plus faible impact théorique sur la santé et la sécurité de l’utilisateur. À l’aide de données temporelles, il sera également possible de constater si l’implantation du Réseau Express Vélo (REV) a eu un impact sur les flux, et s’il a permis de favoriser les itinéraires ayant une qualité environnementale satisfaisante pour les cyclistes. 

 

À noter :

- Toutes les données sont préalablement reprojetées en 3857. 

- Les interpolations (krigeage) et les jointures spatiales seront réalisées sur ArcGIS par la suite, et non sur FME. 

- Les données pour lesquelles aucun traitement n’a encore été réalisé ne sont pas visibles sur le schéma explicatif. Elles sont cependant décrites ci-dessous. 

 

**SCHÉMA EXPLICATIF :** 

![](https://lh7-us.googleusercontent.com/docsz/AD_4nXfJbPvz6ps8uvv5jG_Iz8uAGT5PsvzxIe_6vzl1hZzOgfH5KvH_u9Uy7TWaA6SgIMQ7uydYtNq4C9P9LMKVsc4ZIKKTevsf_zlVUkar62SzK4hQpo0-sZTxXEQId9yLFL7mfgCjXOkVmcqk57MXn3Y?key=mO37J39l4RNscQY75Ojvjg)


**PARTIE 1 : JEUX DE DONNÉES LIÉS AU TRANSPORT** 


**1.1. Comptage des vélos sur les pistes cyclables (vecteur / points)** 

Pour ce jeu de données, deux méthodes de traitements sont utilisées selon l’année de création : 

- De 2009 à 2018, nous avons d’abord calculer la somme du nombre de passages par station, puis il était nécessaire d’inverser le sens du tableau CSV. En effet, une colonne correspond ici à une station alors qu’il faudrait que chaque station corresponde à une ligne. Une fois inversé, il a fallu donner un ID à chacune de ces stations, en utilisant la liste des ID existants dans le jeu de données de localisation des compteurs. Une jointure attributaire est par la suite réalisée pour obtenir l’ensemble des informations pour chaque station (coordonnées XY notamment). Les données sont par la suite nettoyées. 

![](https://lh7-us.googleusercontent.com/docsz/AD_4nXe7Q81pPMDZOtqPCXEnEyjFtwxoEMxuaCiy8H0iIzPPFd0L9lCs_HVmz7WWU1386lU43WATIw44SWXknGaMGfaHEOFf0KY8tN1zc8udTluHiR0d4Ea0K9lHVvSpDYHdWoDnfWeUBl0LyTSPjhOROEc?key=mO37J39l4RNscQY75Ojvjg)

- De 2019 à 2024, c’est plus simple. Il suffit de faire la jointure avec le jeu de données de localisation des compteurs (champ ID), puis de nettoyer les données. 

 ![](https://lh7-us.googleusercontent.com/docsz/AD_4nXeCEz2T7mHtbha4jrqALOfkuxkimkx7bmPB9BX0Qnx_oj-oiFV4Rnwc1-H1WyWvdcYcHR0vQl3LdPM829lPkhDGQmuG-36z-F8HtWU36dKp14pNI7o-nAz3CdT-lzrkuDvTSPsKEeyl0tBVnlEKbPs?key=mO37J39l4RNscQY75Ojvjg)

Grâce à ce traitement, nous pourrons réaliser une interpolation permettant de mettre en avant la concentration de vélos sur l’ensemble de l’île de Montréal, et cela pour chaque année. L’influence (ou non) de la création du REV pourra aussi être mise en avant (\*_voir Compilation des données de comptage_).


**1.2.  Réseau cyclable et Réseau Express Vélo (REV) (vecteur / lignes)** 

Dans notre cas, aucun traitement n’est nécessaire pour ces jeux de données. Une jointure spatiale sera réalisée lorsque les différentes interpolations seront effectuées (comptage, indice) afin de mettre en avant la qualité environnementale des pistes cyclables.

 
**1.3.  Déplacement MTL Trajet (2017) (vecteur / points)** 

Nous gardons seulement les points dont le mode de transport est le vélo et qui sont contenus dans l’île de Montréal en réalisant un « Clip » avec les limites terrestres.

 ![](https://lh7-us.googleusercontent.com/docsz/AD_4nXfnxMrvBUbiZYZ-he-ipSX1M-0-YNEXvv2xrrSBaS0jwG3a1s5fM6Clyf5E3483D94iCwEOFP-sIjziTj8xqF0lDp6SotMsav3I66-iY0yrnrkF64fUWnppzHQIP17VlKE1YRSr0jDYIwhy44foi_s?key=mO37J39l4RNscQY75Ojvjg)

**1.4.  Historique des déplacements (publié par BIXI Montréal) (vecteur / points)** 

Ce jeu de données contient le détail des déplacements réalisés via le réseau de vélo en libre service de BIXI Montréal, avec pour chaque ligne la date et la station de départ, et la date et la station d’arrivée. Nous avons tout d’abord dissocié les départs et les arrivées, le but étant de mettre en avant les principales zones de récupération et de dépôt des cycles, et donc les principaux flux. Par la suite, le jeu de données des stations a permis d’assigner les coordonnées XY au déplacement grâce à l’ID des stations de départ et d’arrivée. Après un nettoyage où nous gardons seulement un champ ID, nous pouvons calculer la somme du nombre de passages par station, et cela pour chacun des 10 jeux de données annuels.

![](https://lh7-us.googleusercontent.com/docsz/AD_4nXc4A5W3ETx6X1m-79x8jPZ0B2IcYXqfA5pbcTMjISRa9Tgwnul-kFnoYz-jT3sGJR6RIgIlifdJTHzPq58FcEORtjpMpet8qz3HTB7Gr-yt-Qli0I7cDoTmMByrFo7TySjzpjkI8muHiaGqcJn8SQ?key=mO37J39l4RNscQY75Ojvjg)

**1.5.  Fréquentation des voies actives sécuritaires (VAS)** 

Ce jeu de données contient des stations de comptage sur la VAS. Une simple jointure nous a permis de créer les points. 

![](https://lh7-us.googleusercontent.com/docsz/AD_4nXfaKvtGXiEM1XvNMiHtGd0wJaHPPJckMnq9CKiG0iPrwoerOKqD6PNOwLUiqTq1vrXjj351t2bgkeo1ePVgkE6Q9ntkhwekYEnpnkVnM59T7Jp8ep3B2iyVXPAsJEh36HjyOgqmyJMg3AXDj8DVrpw?key=mO37J39l4RNscQY75Ojvjg)

**1.6.  Compilation des données de comptage** 

Pour finir, nous réalisons une compilation des données de comptage de vélos, c’est-à-dire : 1.1. **Comptage des vélos sur les pistes cyclables ;** 1.3. **Déplacement MTL Trajet (2017) ;** 1.4. **Historique des déplacements (publié par BIXI Montréal) et** 1.5. **Fréquentation des voies actives sécuritaires (VAS).** Au préalable, une vérification des données est effectuée afin de ne conserver que les coordonnées X/Y et le “score” par point sur l’ensemble de l’île de Montréal. La géométrie des points a été également réatribuée. Enfin, les résultats sont “writés” dans la base de données.

****![](https://lh7-us.googleusercontent.com/docsz/AD_4nXeTfJXrppdrePyIqO7LzNlDrDwJbO0tAzs8_N0Hr0Tt4SkauxlnUlF0KxoJsZrk5e_hloa5WV0NKNtgDplMgBeaqJtyIUEG1qYBciNufmPEpIahfZXtiMWkpqD99zxD3fSJtlDL96rGvOh_MiyJ1xg?key=mO37J39l4RNscQY75Ojvjg)****

Ce traitement nous permet d’obtenir une meilleure répartition des données ponctuelles dans l’espace, afin d’optimiser le résultat du krigeage par la suite. Cependant, certaines données (1 vélo) pourraient être comptabilisées plusieurs fois, par exemple dans le cas où un point du jeu de données “Déplacement MTL Trajet” a été pris proche des stations de BIXI ou d’un compteur de la ville de Montréal. En utilisant un krigeage (méthode statistique), nous supposons que les conséquences de cette surestimation potentielle seront minimes. 


**PARTIE 2 : JEUX DE DONNÉES LIÉS À L’INDICE INTÉGRÉ** 


**2.1. Collisions routières (vecteur / points) · CRITÈRE 1** 

Ce jeu de données liste les collisions survenues à Montréal depuis 2012. Dans notre cas, seules les collisions impliquant au minimum 1 vélo sont sélectionnées. Une interpolation permettra de mettre en avant les zones où la sécurité des cyclistes est la moins bonne. Nous supposons que ces zones peuvent être directement liées à celles concentrant de nombreux flux de vélos. Ainsi, si c’est le cas, nous calculerons la proportion d’accidents selon le nombre de vélos dans la zone, afin de connaître le % de chance d’accident pour 1 vélo.

**2.2. RSQA - indice de la qualité de l’air (historique) (vecteur / points) · CRITÈRE 2** 

La Ville de Montréal mesure la qualité de l’air sous la forme d’une valeur numérique appelée « indice de la qualité de l’air (IQA) ». Le présent ensemble de données permet d'accéder aux valeurs historiques de l'IQA, mises à jour quotidiennement. Une interpolation sera nécessaire pour établir une cartographie estimative de la qualité de l’air sur l’ensemble de l’île de Montréal. Selon les valeurs de qualité de l’air, des classes seront alors créées. 


**2.3. Îlots de chaleur (vecteur / polygones) · CRITÈRE 3** 

Polygones représentant les îlots de chaleur à la surface du sol. Aucun traitement est nécessaire pour ce jeu de données puisque les polygones sont déjà distinguées en 5 catégories, sur l’ensemble de la zone d’étude.
 

**2.4. Mesures de niveaux acoustiques (vecteur / points) · CRITÈRE 4** 

Ce jeu de données compile les niveaux de bruit collectés de façon ponctuelle ou pour une période prolongée à l'aide de sonomètres afin d'évaluer les niveaux de bruit de fond.  Une interpolation sera effectuée afin d’obtenir une répartition sur l’ensemble de l’île de Montréal. 

**2.5.** **Limites terrestres de l’île de Montréal (vecteur / polygone)** 

Aucun traitement n’est nécessaire pour ce jeu de données. Les limites terrestres sont seulement utilisées pour ne garder que les « Déplacements MTL Trajet » sur l’île de Montréal, mais aussi pour définir l'étendue du krigeage. 

\-----

**Erreurs et apprentissage du FME :**

Face à la très grande quantité de jeux de données, notamment due à la dimension temporelle utilisée, l’ergonomie et la clarté de l’ensemble du Workbench FME n’est pas optimal. À l’avenir, pour alléger le FME, il semble pertinent d’utiliser soit, moins de jeux de données différents, soit de réduire la plage temporelle traitée.
