# Principe de l'appli : webservice
Vous devez réaliser un Web Service en langage PHP qui permet de récupérer les informations d’un « adhérent » à partir de son « identifiant ». Les données (liste des adhérents) sont stockées dans un fichier CSV.

# choix  du framework et des composants
Symfony 4.1 : j'aime symfony et je n'avais pas testé la V4
choix d'une API REST : plus lisible que SOAP
MVC : 
* model : src/Entity/Userlabels.php
* vue : une homepage index.html.twig et gestion custom des 404 : templates\bundles\TwigBundle\Exception\error404.html.twig
* Controller : src/Controller/CsvController.php

pré-requis : 
* apache2
* php 7.1

installation : 
* copier le fichier AJE.zip fourni à la racine www de votre serveur apache
* décompresser ce fichier et supprimer l'archive AJE.zip

autres possibilités d'installation : 
dans votre répertoire www :
* git clone https://github.com/joulie/demo1.git .
* php composer.phar install
* (et eventuellement si suppression du .htaccess : php -S 127.0.0.1:8000 -t public)

améliorations non réalisées faute de temps :
passer les routes en annotation
gérer une mise en page html pour les éléments non Json
une icone 

éléments réalisés : 
* une homepage qui liste les actions possibles via l'URL /
* vous trouverez les résultats du test1 via l'URL /test1 : test1.1 via /test1/{id existant} - test1.2 via /test1/{id inexistant} 
* vous trouverez les résultats du test2 via l'URL /test2 : test 2.1 via /test2 - test 2.2 avec un fichier .csv vide (hébergé ici dans hardis\public\donnees.csv - test 2.3 en supprimant le fichier.csv