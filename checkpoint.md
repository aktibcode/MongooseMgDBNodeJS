A-  Ce que vous visez

    Dans ce point de contrôle, vous exécuterez une série d'instructions guidées pour manipuler et gérer votre base de données tout en utilisant Mongoose

        PS : n'oubliez pas de commenter votre code avant de le soumettre.

B-  Instructions

    1-  Installation et configuration de Mongoose :
    
        Ajoutez MongoDB et Mongoose au package.json du projet. Stockez l'URI de votre base de données MongoDB Atlas dans le fichier .env privé sous le nom MONGO_URI. Entourez l'URI de guillemets simples ou doubles et assurez-vous qu'il n'y a pas d'espace entre la variable et le `=` et la valeur et le `=`. Connectez-vous à la base de données à l'aide de la syntaxe suivante :

        `mongoose.connect(<Votre URI>, { useNewUrlParser: true, useUnifiedTopology: true });

    2-  Créez une personne avec ce prototype :

        - Prototype de personne -

        --------------------

        nom : chaîne [obligatoire]

        âge : nombre

        favoriteFoods : tableau de chaînes (*)

        Utilisez les types de schéma de base de Mongoose. Si vous le souhaitez, vous pouvez également ajouter d'autres champs, utiliser des validateurs simples tels que required ou unique et définir des valeurs par défaut. Consultez la  documentation de Mongoose (http://mongoosejs.com/docs/guide.html).

    3-  Créer et sauvegarder un enregistrement d'un modèle :

        Créez une instance de document à l'aide du constructeur Person que vous avez créé précédemment. Transmettez au constructeur un objet contenant les champs name, age et favoriteFoods. Leurs types doivent être conformes à ceux du schéma Person. Appelez ensuite la méthode document.save() sur l'instance de document renvoyée. Transmettez-lui un rappel à l'aide de la convention Node. 
        
            /* Exemple */
            // ...

        personne.save(fonction(err, données) {

            // ...fais ton truc ici...

        });

    4-  Créer de nombreux enregistrements avec model.create()

    Parfois, vous devez créer plusieurs instances de vos modèles, par exemple lorsque vous alimentez une base de données avec des données initiales. Model.create() prend un tableau d'objets comme [{name: 'John', ...}, {...}, ...] comme premier argument et les enregistre tous dans la base de données.

    Créez plusieurs personnes avec  Model.create() , en utilisant l'argument de fonction arrayOfPeople.

    5-  Utilisez model.find() pour rechercher dans votre base de données

    Trouver toutes les personnes portant un prénom donné, en utilisant Model.find() -> [Personne]

    6-  Utilisez model.findOne() pour renvoyer un seul document correspondant à partir de votre base de données

    Recherchez une seule personne qui possède un certain aliment dans ses favoris, en utilisant Model.findOne() -> Person. Utilisez l'argument de fonction food comme clé de recherche.

    7-  Utilisez model.findById() pour rechercher votre base de données par _id

    Recherchez la (seule !!) personne ayant un _id donné, en utilisant Model.findById() -> Person. Utilisez l'argument de fonction personId comme clé de recherche.

    8-  Effectuez des mises à jour classiques en exécutant Rechercher, Modifier, puis Enregistrer

    Recherchez une personne par _id (utilisez l'une des méthodes ci-dessus) avec le paramètre personId comme clé de recherche. Ajoutez « hamburger » à la liste des aliments préférés de la personne (vous pouvez utiliser Array.push()). Ensuite, à l'intérieur du rappel de recherche, enregistrez la personne mise à jour.

    Remarque :  cela peut paraître compliqué si, dans votre schéma, vous avez déclaré favoriteFoods comme un tableau, sans spécifier le type (c'est-à-dire [String]). Dans ce cas, favoriteFoods est par défaut de type mixte et vous devez le marquer manuellement comme modifié à l'aide de document.markModified('edited-field'). Voir  la documentation de Mongoose

 

    9-  Effectuer de nouvelles mises à jour sur un document à l'aide de model.findOneAndUpdate()

    Recherchez une personne par nom et définissez l'âge de la personne sur 20 ans. Utilisez le paramètre de fonction personName comme clé de recherche.

    Remarque :  vous devez renvoyer le document mis à jour. Pour ce faire, vous devez transmettre les options document { new: true } comme 3e argument à findOneAndUpdate(). Par défaut, ces méthodes renvoient l'objet non modifié.

 

    10- Supprimer un document à l'aide de model.findByIdAndRemove

    Supprimez une personne en fonction de son identifiant. Vous devez utiliser l'une des méthodes findByIdAndRemove() ou findOneAndRemove(). Elles sont similaires aux méthodes de mise à jour précédentes. Elles transmettent le document supprimé à la base de données. Comme d'habitude, utilisez l'argument de fonction personId comme clé de recherche.


    11- MongoDB et Mongoose - Supprimer de nombreux documents avec model.remove()

    Supprimez toutes les personnes dont le nom est « Mary » à l'aide de Model.remove(). Transmettez-le à un document de requête avec le champ de nom défini et, bien sûr, effectuez un rappel.

    Remarque :  Model.remove() ne renvoie pas le document supprimé, mais un objet JSON contenant le résultat de l'opération et le nombre d'éléments affectés. N'oubliez pas de le transmettre au rappel done(), car nous l'utilisons dans les tests.


    12- Aides à la recherche de chaîne pour affiner les résultats de recherche

    Recherchez des personnes qui aiment les burritos. Triez-les par nom, limitez les résultats à deux documents et masquez leur âge. Enchaînez .find(), .sort(), .limit(), .select(), puis .exec(). Transmettez le rappel done(err, data) à exec().