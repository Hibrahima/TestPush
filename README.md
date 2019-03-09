## Choix de design et Implementation 
### Architecture
Notre architecture est composée de 3 couches que sont : la couche métier, la couche DAO (Data Access Object) et la couche service.
-   La couche métier représente l’ensemble de toute les entités(tables) qui existent dans la base de données. Elle définit le mapping relationnel entre les entités.
-   La couche DAO (couche d’accès aux données) nous permet de faire tous les traitements (crud et extra) sur nos entités Jpa.
- La couche service permet de faire le lien entre notre couche dao et le web. Le service web a été développé avec Jersey. Toutes les réponses des requites http sont en format JSON.   
Grâce à ces trois couches, la séparation des responsabilités est appliquée et cela facilite la maintenance du code.
### Implementation
Pour éviter les effets souvent drastiques du cascade, nous avons préféré utiliser pour la plupart de nos mappings **CascadeType.PERSIST** et **CascadeType.MEGE** et le fetch type par défaut n'a pas non plus été changé car nous pensons que cela correspond a ce que nous voulons. 

La plupart de nos requêtes sont faites avec les requêtes nommées dans le soucis de faciliter la maintenance du code.

Pour maintenir l’intégrité de données entre les relations bidirectionnelles, nous avons implémenté des méthodes utilitaires qui vont binder les deux bouts de la relation et éviter ainsi un problème de mapping.
#### Mapping Relationnel
1. Participation (réponses aux sondages)  
	- Nous avons fait le choix de construire une participation en fonction d'un utilisateur et d'un sondage. 
	- Elle a don une clé primaire composée.
	- Elle est constituee d'un ensemble de plages horaires
	- La date a laquelle et le moment auquel elle a ete cree sont sauvees respectivement par LocalDate.now() et LocalTime.now()
2. FoodPreference (preference alimentaire) 
	-  Clé primaire composée (utilisateur et reunion).
	- Un mail est envoyé aux participants afin de renseigner leur preference si besoin 
	- Une description peut être renseignée si besoin
3. Allergy (allergie)
	- Ne devrait pas changer en fonction de la reunion ou du sondage
	- Donc pas de clé primaire composée, enfin :smiley:  
4. Survey (sondage)
	- Sa creation entraîne automatiquement la creation de la reunion qui lui est associée
	- Il a **4 états**:
		- **Default** : quand le sondage est créé
		- **Ready** : lorsqu'au moins une collection de plages 						    horaires a été ajoutée au sondage - a ce stade le sondage est ouvert aux participations
		- **Pending**: lorsqu'au moins un participant a été ajoute a ce sondage
		- **Validated**: lorsqu'une date finale a été choisie pour ce sondage
	- Ne peut pas être modifié quand il est ready
	- Est constitue d'un ensemble de dates (ensemble de plages horaires)
5. Role
	- Peut être participant, creator,...
	- Est **unique**
6. User (utilisateur)
	- A un ensemble de sondages (sondages créés)
	- Définit un mapping **ManyToMany** avec Role :
	         - Plusieurs utilisateurs peuvent avoir plusieurs mêmes roles
	 - A une collection de reunions auxquelles il a participé ou manqué
	 - A une collection d'allergies
7. TimeSlot (plage horaire)
-	A un score qui est incrémenté lorsqu'elle a été choisie comme faisant partie d'une réponse sondage
-	Est actif ou inactif 
        - Actif: lorsqu'elle est créée
        - Inactif: lorsqu'elle a été ajoutée a une custom date( ensemble de plages horaires disponibles pour la meme date)
- Ne peut pas être ajoutée a une custom date lorsque la plage horaire est inactive -  cela permet de garder l’intégrité de donnée car elle aurait potentiellement un score et ne peut donc pas être ajoutée a une autre date avec ce même score 
- A une heure de debut et fin
#### Couche de service
- Le projet propose une api assez riche ( à vérifier avec les fichiers Postman :smirk:)
- Toutes les requêtes sont développées afin de retourner une réponse en JSON meme en cas d'erreur - au moins si ça secoue on le saura :smiley: 
- Peut ramener toutes les plages horaires contenues dans les réponses d'un sondage X dans l'ordre décroissant de score ( cool :open_mouth:) - facile pour choisir une date alors (ouais bof :neutral_face:) mais cool quand même :sweat_smile:  
- Peut ramener les absents et les présents d'une reunion
- Peut ramener les reunions que l'utilisateur Y a manqué ou assisté
- Peut évidemment valider une date choisie - d'ailleurs vous qui n'avez pas choisi cette date là, vous êtes tous absents hein :open_mouth:
- Peut faire un tas d'autres choses - découvrez le avec nos collections Postman :relaxed:

Le diagramme de classe se trouve dans le dossier documentation à la racine du projet.
