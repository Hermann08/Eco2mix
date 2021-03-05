

# Rapport Technique 

`  

## Eco2mix Analysis Py  



Promotion DA Bootcamp novembre 2020  

Participants : Stéphanie Krzyzelewski  

Hermann Agbota  

Guillaume Ostyn 

### A. Contexte 

  Ce projet a vu le jour, d’un point de vue technique, dans le cadre de la validation des  connaissances acquises durant la formation BootCamp Novembre 2020 de DataScientest.  

  Le but étant de montrer notre capacité à raisonner en tant que Data Analyst, nous devions  choisir un thème afin de mettre en œuvre les techniques d’analyse de données apprises  durant 9 semaines sur des données concrètes, choisies par nos soins.  


### B. Objectifs du projet  

##### 1. Objectifs à atteindre  

` `L’objectif principal de ce projet est de construire un modèle de prédiction de la  consommation d’énergie électrique en France à partir des données disponibles. Notre  ambition est d’approcher au plus près les consommations constatées, en testant différents  modèles de régression.  

` `Un des objectifs secondaires est de comprendre, , les principales caractéristiques du mix énergétique en France à travers différentes granularités :  

\- Géographique : nationale/régionale/extra-nationale  

\- Temporelle : annuelle / mensuelle / journalière  

\- Structurelle : énergies fossiles (gaz/charbon/pétrole) / renouvelables (éolien /  hydraulique / solaire) / fissibles (nucléaire)  


### C. Data  

##### 1. Cadre  

  Dans ce projet, nous avons utilisé un dataset créé par RTE, disponible librement sur la  plateforme (lien: Données éCO2mix régionales consolidées et définitives (janvier 2013 à  septembre 2020)).  

  Ce jeu de données (définitives car vérifiées et approuvées par l'ensemble des acteurs de la  filière), rafraîchi chaque demi-heure, présente des données "temps réel" issues de  l'application éCO2mix de 2013 à 2020. Au total, cela représente plus de 1,6 millions de  relevés.  

On y trouve au pas demi-heure :  

\- Les noms de chaque région  

\- Le code INSEE région  

\- La date et l’heure de chaque enregistrement  

\- Le mois de chaque enregistrement  

\- L’année de chaque enregistrement  

\- La consommation réalisée.  

\- La production selon les différentes filières composant le mix énergétique.  - La consommation des pompes dans les Stations de Transfert d'Energie par Pompage  (STEP).  

\- Les échanges physiques aux frontières.  

\- Le découpage en filière et technologie du mix de production.  

\- Les échanges commerciaux aux frontières.  

##### 2. Pertinence  

  Le dataset initial présente beaucoup de disparités c’est à dire de données non enregistrées.  Nous les appellerons des valeurs manquantes.  

  Les valeurs manquantes peuvent être dues à plusieurs scénarios en fonction de la variable.  S’agissant de la variable ‘Nucléaire’ par exemple, la valeur manquante pourrait traduire un  défaut de production d’énergie nucléaire à un instant T, ou la non présence de centrale  nucléaire dans une région donnée. Il est donc primordial de procéder à un nettoyage de la  base.  

Afin d’avoir un jeu de données cohérent et dépourvu de valeur manquante, nous avons :  - Supprimer les colonnes ayant plus de 70% de valeurs manquantes  - Remplacer les valeurs manquantes des autres colonnes par la valeur 0 (pour les  différentes familles de productions d’énergies)  

\- Supprimer les colonnes redondantes (région/code INSEE région par exemple)  - Supprimer les années avec des données non fiables ou incomplètes (2013 et 2020)  

  Au regard des différentes variables, nous avons la possibilité de prédire la consommation de  l’énergie ou la production par type d’énergie en fonction des données temporelles (Date  heure) et de la Région. 

  Pour ce projet nous avons décidé de prédire la consommation d’énergie. La variable cible est  donc la variable ‘Consommation’.  
Une fois la variable choisie, il est donc important de visualiser la distribution de l’ensemble  des valeurs.  



![GitHub Logo](images/logo.png)

  La figure ci-dessus présente la distribution de la variable consommation, qui suit une loi de  Poisson.  

  Nous avons ensuite vérifié les relations entre les différentes variables.  L’énergie nucléaire fournit la plus grosse quantité d’énergie ; cette source d'énergie est moins  agile que la source hydraulique. L'énergie nucléaire fournit donc la base pour la  consommation ; l'hydraulique et les bioénergies (dans une moindre mesure) fournissent le  complément pour atteindre la consommation. Ces différentes variables dépendent également  des données temporelles. Sur cette hypothèse nous avons mis en place une matrice de  corrélation, présentée ci-dessous. 

image

  Nous observons une corrélation entre la consommation et les bioénergies (60%) ainsi que  l'énergie hydraulique (40%). La corrélation entre la consommation et l'énergie nucléaire est  faible (20%).  


### D. Projet  

##### 1. Classification du problème  

  Ce projet fait référence à une problématique de régression linéaire, de time séries, et plus  particulièrement de l’apprentissage automatique supervisé. Les algorithmes de machine  learning utilisant les méthodes de régression et des times series est la meilleure approche  pour prédire la consommation d’énergie en fonction des données temporelles. Plusieurs  paramètres existent pour mesurer la performance des modèles de Machine Learning. Parmi  celles-ci nous utiliserons La RMSE (Racine carrée de l’écart quadratique moyen) pour  comparer et mesurer la performance obtenue pour les modèles retenus dans ce projet.  


##### 2. Choix du modèle 
  Nous disposons des données sur la consommation et  la production d'énergies des différents secteurs de production en France, de 2013 à 2020 sur  des tranches horaires de 30 minutes. Ces données représentent au total 1.6 millions de lignes 
5  
et 66 colonnes. La consommation de l’énergie dépend de plusieurs facteurs comme la  quantité produite, la population, et des saisons. Ainsi pour prédire la consommation d’énergie  en fonction des différentes variables, nous avons choisi les algorithmes classiques de  régression tels que SGD Regressor et Lasso et des timeseries comme Holt Winters, fbprophet


##### 3. Optimisation du modèle  

  Les meilleurs paramètres du modèle sont recherchés grâce à la méthode GridSearchCV ou  la méthode de validation croisée.  
Concernant SGD : le GridSearchCV n’a pas abouti pleinement car il n’a pu être fait que pour  l’hyperparamètre alpha, alpha étant réduit à 4 choix, étant donné le volume du dataset. Les  délais de traitement étaient très longs (>4H pour la recherche du paramètre  loss=squared_loss/huber ou epsilon insensitive). Par rapport au modèle par défaut  (alpha=0.0001), les performances n’ont pas été améliorées.  
Concernant LASSO: les performances se sont améliorées nettement passant de 70%  (modèle par défaut) à 92% lors du choix de l’alpha par validation croisée (test pour toutes les  régions incluses dans le modèle).  
Afin de réduire les temps de traitement, notre projet s’est focalisé sur la prédiction de la  consommation d’une région (Auvergne-Rhône-Alpes). 
  
  Pour les modèles SGD et Lasso, il convient cependant de préciser que les résultats  s’améliorent nettement lorsque les tests sont effectués avec toutes les régions et pas  seulement une seule. Il semblerait que les régions permettent d’effectuer un meilleur  apprentissage et soient des variables explicatives importantes pour l’optimisation.  



### E. Description des travaux réalisés

Répartition de l’effort sur la durée et dans l’équipe  

Le projet s’est structuré en 4 parties bien distinctes:  
- l’exploration des données du dataset  
- la visualisation des données  
- la prédiction avec le Machine Learning  
- la production du rapport avec Streamlit. 
 
 
##### 1. Exploration des données du dataset  
Durée: 7 jours  

  Après avoir téléchargé le jeu de données sur le site de RTE, nous avons chargé le dataset  puis avons ensuite réalisé une analyse des données qui nous a permis, grâce aux  connaissances acquises, de nettoyer le data set. Le nettoyage a résidé en la suppression de  nombreuses colonnes vides et en le remplacement des valeurs manquantes pour quelques  variables de production.  
Difficulté(s) rencontrée(s) / solution(s) adoptée(s)  
La volumétrie conséquente du dataset utilisé nécessite un temps de chargement long et une  machine puissante; l’exploration, l’analyse et la modélisation requérant trop de temps pour  être en ligne avec la durée du projet.  
Face à ce problème, nous sommes passés sous Google Colab qui nous alloue une machine  virtuelle bien plus puissante. Aussi, le travail collaboratif en a été grandement facilité.  
Connaissance(s) acquise(s) lors de cette partie:  
L’exploration des données nous a permis de nous familiariser avec l’environnement Google  Colab, très pratique et à l’interface intuitive. 


##### 2. Data visualization  
Durée: 8 jours  
  Pour cette partie importante du projet, nous avons séparé les tâches en 4 parties: 
7  
- Graphiques d’évolution de la consommation et de la production  
- Graphiques de la consommation et de la production par région  
- Graphiques de la consommation et de la production par type d’énergie  
- Rédaction de la fiche d’exploration des données  

###### a. Encodage des graphiques  

  Durée: 3 jours (+ 4 jours de modification)  
Pour cette partie, les connaissances acquises lors de la formation ont été mobilisées. Nous  nous sommes réparti les tâches (1 partie par personne) afin d’optimiser le planning.  Nous avons réalisé en nombre (plus de 20 graphiques différents sortis) des diagrammes  plus élaborés que ceux vus lors de la formation. Le choix du type de graphiques utilisé a été  crucial, afin de maximiser l’impact et la compréhension du message à passer.  
Difficulté(s) rencontrée(s) / solution(s) adoptée(s)  
Lors de notre revue de projet avec notre tuteur, nous avons dû revoir en profondeur les  graphes fournis afin de ne plus en garder qu’un nombre réduit.  
La connaissance de la contrainte sur le nombre de graphes à produire nous aurait permis de  réduire le temps de cette partie.  
Connaissance(s) acquise(s) lors de cette partie  
Nous avons approfondi nos connaissances des librairies de data viz de Python (matplotlib et  seaborn). Aussi, nous avons vu l’importance du choix des graphiques afin de rendre  l’analyse plus “évidente”. 


###### b. Rédaction de la fiche d’exploration des données  
  Durée: 1 jour  
La rédaction de la fiche d’exploration des données a été rapide. Nous avons, afin de  diminuer la charge, regroupé les variables supprimées. 

##### 3. Machine Learning  
Durée: 20 jours  
 
 
  Le choix des timeseries n’est pas venu immédiatement. En effet, c’est après une recherche  bibliographique que nous avons trouvé ces modèles qui semblaient tout à fait adaptés à  notre projet. Nous avons décidé d’en faire une partie à part entière (et nous avons bien fait!).  
Difficulté(s) rencontrée(s) / solution(s) adoptée(s)  
Volumétrie / temps de calcul: 
 
  Les temps de calcul des algorithmes sélectionnés varient grandement. En effet, le modèle  Lasso est relativement rapide alors que le SGD est long, tout comme le TBATS. Afin de  contourner ce problème tout en ayant des résultats comparables, il a été décidé de réduire  la taille du dataset en ne choisissant qu’une seule région (la région Auvergne) et de réduire  l’étendue temporelle.  
Compétences techniques / théoriques:  
Pour l’utilisation des modèles de timeseries, la principale difficulté rencontrée est venue de  la triple saisonnalité des données (annuelle, hebdomadaire, horaire), alors que dans la  littérature disponible les modèles traitent des données saisonnières simples.  Aussi, les modèles ciblés au départ (SARIMA, Holt-Winters) ont nécessité un temps  important de d’appropriation des concepts mathématiques sous-jacents (saisonnalité, tests  de stationnarité, ACF/PACF…). Pour finir, des modèles plus simples (d’un point de vue  pratique) ont été envisagés. L’existence du module de formation sur les timeseries nous a  été dévoilée le 08 janvier.  
Pertinence:  
  Comme expliqué précédemment, notre première approche était erronée dans l’interprétation  des données à disposition.  
Connaissance(s) acquise(s) lors de cette partie  
Lors de cette partie, nous avons pu appliquer par nous mêmes, en autonomie, les notions  apprises lors de la formation (manipulation de dataset, apprentissage, gridsearch…). Aussi,  des notions relatives aux timeseries, plus complexes d’un point de vue mathématique ont  été utilisées, ainsi que des algorithmes fort intéressants permettant de prendre en compte  les saisonnalités (tels que TBATS ou fbprophet).  


##### 4. Production du rapport Streamlit  
Durée: 5 jours  
  Finalement, les modèles ayant été testés et les performances ayant été mesurées, nous  nous sommes attaqués à la production du rapport Streamlit.  
Difficulté(s) rencontrée(s) / solution(s) adoptée(s)  
Volumétrie / temps de calcul:  
Encore une fois, la taille de notre dataset (même fortement réduit) ne nous permettait pas  d’afficher quoi que ce soit depuis un environnement anaconda (même l’affichage d’un  df.head() n’était pas possible). Et encore une fois, notre salut est venu de Google Colab. En  effet, nous avons dû rechercher un moyen sur les forums afin de lancer Streamlit depuis  Colab, moyen que nous avons trouvé et qui s’avère très bien fonctionner. 

Compétences techniques / théoriques: 

  L’installation de Streamlit et son utilisation a nécessité un temps non négligeable à notre  équipe. Peut-être qu'une masterclass plus focalisée sur ce sujet (ou avoir installé et essayé  streamlit sur un exemple donné avant la masterclass) nous aurait aidés à passer ce temps  sur le développement de la présentation.  
Connaissance(s) acquise(s) lors de cette partie 
  
Cette partie nous a permis de nous familiariser avec la librairie Streamlit. Une fois  l’environnement Colab mis en place, son appropriation a été rapide, aisée, presque ludique.  Streamlit tombe assez bien sous les doigts, pour un résultat qui n’est pas comparable à ce  qui peut être fait, notamment en terme d’interactivité, avec la bonne vieille présentation  Powerpoint.  

#### 2. Bibliographie  
- https://train.datascientest.com/dashboard 
- https://www.analyticsvidhya.com/blog/2018/02/time-series-forecasting-methods/ - https://otexts.com/fpp2/complexseasonality.html 
- https://www.youtube.com/watch?v=DeORzP0go5I 
- https://www.youtube.com/watch?v=oY-j2Wof51c 
- https://www.youtube.com/watch?v=WjeGUs6mzXg 
- https://pythonawesome.com/pythons-best-automated-time-series-models/ - Forecasting Time Series with Multiple Seasonalities using TBATS in Python | by  Grzegorz Skorupa | intive Developers | Medium 
- https://towardsdatascience.com/how-to-predict-a-time-series-part-1-6d7eb182b540 - https://facebook.github.io/prophet/docs/quick_start.html 
- https://facebook.github.io/prophet/docs/diagnostics.html 
- https://www.rte-france.com/ 
- https://www.enedis.fr/ 
- https://machinelearnia.com/descente-de-gradient


### F. Résultats, bilan et suite(s) du projet 

##### 1. Résultats  
Dans cette partie, nous allons synthétiser les résultats obtenus des différents modèles que  nous avons utilisés et allons les analyser.  
Modèle utilisé Train/Test sets RMSE Temps de calcul  1-9 
Lasso random 723 1 SGD random 724 7 SGD time 720 7 Holt-Winters*** time 980 (927*) 5 
TBATS** time 782 9 
fbprophet 
time 
877 
5



Remarques:  
* rmse avec correction de biais basée sur le train set 
10  
** rmse calculé avec un train set tronqué (taille divisée par 4)  
*** comportement divergent si dataset commencé après 2014...

Analyse du tableau récapitulatif:

  Les meilleurs résultats (en termes de rmse) sont atteints par les algorithmes de régression  “standards” alors même que les algorithmes timeseries sont destinés… aux timeseries et  donc designés pour l’application qui nous intéresse ici. Ce point est un point d’étonnement et  mériterait une investigation plus profonde.  
Les résultats des algorithmes de timeseries étonnent aussi. Les performances du Holt Winters sont assez clairement en retrait, ce qui semble logique (une seule saisonnalité,  algorithme plus “basique”). Par contre, l’algorithme TBATS est plus performant que le  fbprophet, alors même que ce dernier est décrit dans la littérature comme plus performant  que des algorithmes plus évolués que le TBATS (type SARIMAX).  
Un point important à souligner dans le cas d’une industrialisation d’un de ces modèles est le  temps de calcul nécessaire. Cette contrainte nous amènerait dans le cas d’un modèle de  régression classique à choisir le modèle Lasso et favoriserait le fbprophet dans le cas d’un  modèle timeseries, le TBATS étant de loin l’algorithme le plus long (alors qu’il est plus  performant). Aussi, il faut préciser que le modèle fbprophet a une API très simple et permet  de faire des prédictions sans toutefois réaliser de test statistique préalable (comme celui de  la stationnarité du signal) ou de paramétrer des arguments qui ont un sens mathématique  complexe (à l’instar du modèle SARIMAX par exemple).  
  En conclusion, nous dirons que le choix d’un algorithme ne doit pas être fait sur la base de  sa seule performance intrinsèque (quelle que soit la métrique choisie), mais doit résulter  d’un compromis performance/vitesse/facilité_de_mise_en_oeuvre afin de pouvoir le mettre  en production facilement et assurer une compréhension et une maintenabilité du code dans 


##### 2. Conclusion sur les objectifs  

  A l’instar du tableau récapitulatif fait ci-dessus, nous pouvons en déduire que l’objectif de  prédiction de la Consommation est atteint, avec plus ou moins de performance selon les  modèles.  
Nous avons de plus été à même de dégager les grandes caractéristiques du mix  énergétique de la France:  
- en terme de variation de la consommation:  
- elle suit une triple saisonnalité : année(selon saison) /semaine  
(consommation moindre le weekend lorsque les entreprises fonctionnent  moins) /journée ( consommation moindre la nuit à contrario plus forte aux  heures de pointe midi et soir 18-20h)  
- la consommation a une tendance baissière depuis 2016.  
- en terme de solde énergétique:  
- les régions excédentaires sont celles ayant des centrales nucléaires  implantées, et souvent avec un tissu industriels dense  

