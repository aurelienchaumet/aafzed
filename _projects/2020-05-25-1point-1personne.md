Après avoir lu récemment cet [article](https://www.maartenlambrechts.com/2018/02/13/one-person-one-dot-maps-and-how-to-make-them.html) de Maarten Lambrechts, où il explique qu'il n'avait jamais réalisé de cartes de densités de point (et surtout où il donne la méthodologie qu'il a suivie), je me suis dit que moi non plus je n'avais pas jamais réalisé de cartes de ce genre...

Sans doute jamais eu besoin tout simplement 😁 mais en même temps pourquoi ne pas essayer !

## 1. De quoi parle-t-on ?

Afin d'avoir les bonnes base, une recherche sur le grand internet s'impose.

Un des premiers résultats de recherche qui arrive est un article rédigé par Sylvain Genevois.

[Les cartes par densité de points deviennent de plus en plus courantes et accessibles](http://cartonumerique.blogspot.com/2019/03/cartes-par-densite-de-points.html?m=1)

En résumé (très rapide, mais je vous conseille d'aller lire son article qui permet d'appréhender plus finement la question 👌), sur une carte de densité de points, l'information est portée par un point. Géographiquement parlant, la position n'est pas forcément indiquée de manière exacte, elle peut même être volontairement placée aléatoirement, notamment pour des questions de secret statistique. Au niveau attributaire (les informations derrière chaque point), chaque point peut correspondre à un "objet" réel (1 point = 1 personne par exemple) ou à un regroupement d'objets similaires (1 point=10 personnes).

Ce qu'on peut donc retenir sur l'objectif d'une carte de densité de points, c'est de xxxx

## 2. Quelques exemples

Pour avoir une idée plus visuelle de la chose, une multitude d'exemples ont été regroupés par Boris Mericskay ici

[https://medium.com/@BorisMericskay/visualiser-le-monde-avec-des-points-5da00f442c39](https://medium.com/@BorisMericskay/visualiser-le-monde-avec-des-points-5da00f442c39)

*2 cartes exemples*

Et encore M. Mericskay

[https://twitter.com/BorisMericskay/status/1258744572496281605?s=19](https://twitter.com/BorisMericskay/status/1258744572496281605?s=19) 

## 3. La méthode en discussion

Plutôt que de reprendre ce qui existe déjà par ailleurs et reexpliquer en quoi cela consiste, j'ai préféré me (vous) poser quelques questions de fond et de forme. Avec mon humble geo-expérience évidemment !

La question que je me suis posée à l'origine est "peut-on représenter plus finement les données de l'insee carroyage ?" 

Peut-être en dispatchant le nombre d'individus de chaque carreau à l'intérieur. Pourquoi pas, il faut tester (c'est ce que je me suis dit sur le moment) et puis des questions arrivent (c'est la que ça devient intéressant !) :

- Dans quels polygones dispatcher le nombre d'individus ?
- Jusqu'à quelle échelle cela peut rester visible ?
- Est-ce que la symbologie sera fixée en fonction de l'échelle ?

Je vais essayer de détailler chacune de ces questions tel que j'y ai réfléchi jusqu'aujourdhui. Le but de cet article est de donner un avis personnel, pour pouvoir par la suite échanger avec d'autres avis, n'hésitez pas à critiquer / questionner / interroger / valider en commentant l'article ou en m'envoyant un message 📩

Ce n'est pas réellement un tutoriel pour réaliser une carte de densité de points, mais vous trouverez tout de même des éléments pour la création de ce type de carte par la suite 😉 à travers l'exemple de la population.

### Parce qu'il faut bien commencer quelque part, commençons par une maille large, la commune

En partant du niveau le moins fin, on peut commencer par repartir nos points dans chaque commune.

De prime abord, ce niveau n'a pas l'air très intéressant pour notre cas, car trop grossier, mais il va permettre d'introduire quelques notions de base pour la suite. Donc allons y ! 

Téléchargeons donc une couche de limites communales

[Index of /data/etalab-cadastre/2020-01-01/geojson/departements/17/](https://cadastre.data.gouv.fr/data/etalab-cadastre/2020-01-01/geojson/departements/17/)

On prendra pour cet exemple le département de la Charente-Maritime et non la France entière, cela évitera d'avoir des temps de traitement trop longs.

Donc maintenant que nous avons un niveau de répartition (la commune), nous allons utiliser le nombre d'habitants par commune et dire à un logiciel de créer des points aléatoirement à l'intérieur des limites de chaque commune. 

On utilisera ici Qgis, outil libre et gratuit de cartographie.

*Installer qgis lien*

*Ressources pour utiliser qgis lien*

Le principe est simple : on affiche la couche communale des contours administratifs (par exemple la couche communes du pvi vecteur), on affiche le fichier du nombre dhabitants par commune et on fait une jointure entre les 2.

Allons-y, étant donné que c'est simple :D.

Pour trouver le nombre d'habitants à la commune, on peut aller les exporter sur l'observatoire statistique de l'INSEE.

[Insee - Statistiques locales](https://statistiques-locales.insee.fr/)

*Détailler la procédure via Loom ?*

Comme des images valent mieux que des mots, vous trouverez la procédure détaillée dans la vidéo ci-dessous

Une fois cela fait, il suffit d'utiliser la fonction **Vecteur→Recherche→Créer des points aléatoires dans les polygones.**

Dans le champs Expression de la boîte de dialogue, penser à indiquer le champ contenant l'information du nombre d'habitants *aka nombre de points par commune qu'on souhaite afficher.*

Il faut ensuite diminuer la taille des points, afin d'avoir un affichage "correct" (taille 0,1 dans cet exemple).

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/5c15e9f9-6b1a-420e-ade9-f548e3e113b8/Capture_decran_2020-05-11_a_10.21.28.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/5c15e9f9-6b1a-420e-ade9-f548e3e113b8/Capture_decran_2020-05-11_a_10.21.28.png)

Bon OK, on a un nuage de points plus ou moins dense en fonction du nombre d'habitants de chaque commune et de sa surface. 

Haha ! En fait on s'approche graaaaandement de la notion de densité d'habitants. Peut-être même que c'est ça...

*Image de la densité de population* 

 OK pourquoi pas, mais on peut sans doute aller plus loin.

### La donnée la plus fine existante en termes de recensement de population en France

L'INSEE délivre le recensement de la population sous forme de carreaux d'1km et de 200m, qu'on appelle "Population carroyée". Le principe est qu'à l'intérieur de chaque carreau, on sait qu'il y a XX habitants.

Hm hmm, intéressant... Peut-on réaliser la même méthode que précédemment avec les contours des communes, me direz-vous?

Mais ouiiiii!! Non seulement, nous pouvons, mais nous allons le faire !

Pour cela, il faut tout d'abord récupérer le fichier de carreaux INSEE ici 

*Lien* 

Le fichier est disponible à l'échelle de la France entière, donc les temps de traitement vont être plus importants. 

Même process que précédemment :

- Afficher la couche de la population carroyée sur Qgis
- Utiliser la fonction "Points aléatoires à l'intérieur des polygones"
- Indiquer dans Expression le champ "Ind" (qui contient le nombre de personnes de chaque carreau)
- Aller prendre un café / Faire une balade / Revenir demain (Rayer la ou les mention(s) inutile(s) en fonction de la puissance de son ordinateur) 2h de mon côté - Macbook blablabla

Si on revient à l'échelle du département de la Charente-Maritime, on voit que la distribution des points est plus fine qu'avec la première méthode (logique !).

### Essai de croisement d'une donnée géographique plus fine : population carroyée à l'échelle des bâtiments

Afin d'aller un peu plus loin dans la spatialisation des données du recensement de population, on peut essayer de rebasculer les données carroyées par bâtiment.

A priori, aucune idée du résultat... La question que je me pose avant d'essayer c'est de savoir si cela va apporter une plus-value par rapport au résultat précédent. 

Premières limites identifiées :

- La répartition du nombre de points dans les bâtiments de chaque carreau est arbitraire.
Dans un même carreau, un bâtiment de 1 000m2 pourrait recevoir 3 points et un autre bâtiment pourrait en recevoir 50

## Sources

- [https://docs.mapbox.com/mapbox-gl-js/example/restrict-bounds/](https://docs.mapbox.com/mapbox-gl-js/example/restrict-bounds/)
-
