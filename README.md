Projet Doodle - SIR  
## But du projet
Le but de ce projet est de développer une application web java style doodle-like. Il s'agit de permettre aux utilisateurs de proposer un/des créneau(x) horaire(s) afin de choisir la date et le créneau adéquats pour une reunion. 
## Fonctionnalités 
### Fonctionnalités supportées par le projet
Dans le but de proposer les fonctionnalités principales du projet, une liste exhaustive des services déjà développés et testés est présentée ci-dessous 
- Créer un compte utilisateur
- Créer un sondage
- Créer des plages horaires
- Ajouter une collection de plages horaires aux sondages crées
- Récupérer les réponses des sondages
- Valider une date et un créneau pour une reunion 
- Envoyer des mails pour renseigner les preferences lorsque le créneau choisi contient une pause
- Envoyer des mails pour participer aux sondages créés 
- Hasher le mot de passe 
### Fonctionnalités encore a développer
- Lien de participation au sondage : cette fonctionnalité a été laissée à plus tard pour le développement du front-end. En effet, il est prévu que ce lien redirige vers un composant du front-end et cela explique notre choix.
- Sécuriser les services backend (en fonction des roles - authorization and authentication) : Un plus que nous envisageons d'implementer
- Tests unitaires
## Technologies
Le projet a été mis en oeuvre avec le langage de programmation **Java**. La couche d’accès aux données (DAO = Data Access Object) et de la persistence de données est gérée par **JPA** et **Hibernate** et la couche de service par **Jersey**. La base de données utilisée est **MySQL**. Le hashage est réalisé par la librairie **Bcrypt** et l'envoi de mail par **Apache Commons Email**.
- Hibernate : version 4.3.10
- MySQL : version 5.1.47
- Jersey :  version 1.19.4
- Bcrypt: version 0.3m
- Commons Email : version 1.5

Le plugin maven **Tomcat 7** de Apache est utilisé comme web container pour compiler et exécuter l’application.

## Lancer l'application
Il est nécessaire d'effectuer certaines tâches afin de pouvoir tester aisément le projet.
### Base de données
Ces operations peuvent être faites au travers d'une application graphique comme **phpMyAdmin** mais cependant nous fournissons ici les commandes sql à exécuter en ligne de commande.
- Se connecter au serveur mysql installe sur votre machine
- Créer une base de données **MySQL** nommée **db_sir**     
**create database db_sir;**              
- Créer un utilisateur db_sir_user avec le mot de passe db_sir_pass     
**create user 'db_sir_user'@'localhost'  identified by  'db_sir_pass';**
- Octroyer tous les droits sur la base   
 **grant all privileges on db_sir.\*  TO  'db_sir_user'@'localhost';**
- Importer le backup (db_sir.sql a la racine du projet):    
**mysql -u \<user> -p  db_sir < db_sir.sql** - remplacer user par un utilisateur valide - renseigner le mot de passe. Attention toute autre base de donnée avec le meme nom sera écrasée. 
### Lancer le projet avec Maven
Se déplacer dans un dossier ou le projet sera téléchargé.
- Cloner le projet ou télécharger le     
**git clone https://gitlab.istic.univ-rennes1.fr/ihaidara/project_sir.git**
- Se déplacer dans le dossier **backend** du projet
- Taper mvn tomcat7:run
- Enjoy it :) !

## Remarque
- Le dossier **api** contient  des fichiers collections **Postman**
-  Ces fichiers pourront être utilisés pour tester l'application en les important dans Postman
- Toutes les requêtes http devront normalement produire un resultant même en cas d'erreur (grâce a notre super api - couche de service)  
- Le dossier **documentation** contient  le diagramme
- N'oublier pas les point virgule a la fin des requêtes sql :) 
