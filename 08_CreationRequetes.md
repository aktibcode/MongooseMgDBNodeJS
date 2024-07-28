Chapitre : CREATION DE REQUETES

A-  Création de requetes

    Mongoose dispose d'une API très riche qui gère de nombreuses opérations complexes prises en charge par MongoDB. Prenons par exemple une requête dans laquelle nous pouvons créer progressivement des composants de requête.

    Dans cet exemple, nous allons :

        1-  Trouver tous les utilisateurs.
        
        2-  Ignorez les 100 premiers enregistrements.
        
        3-  Limitez les résultats à 10 enregistrements.
        
        4-  Trier les résultats par le champ prénom.
        
        5-  Sélectionnez le prénom.
        
        6-  Exécutez cette requête.

        UserModel.find()  // find all users

            .skip(100)       // skip the first 100 items

            .limit(10)       // limit to 10 items

            .sort({firstName: 1}) // sort ascending by firstName

            .select({firstName: true}) // select firstName only

            .exec()                   // execute the query

            .then(docs => {

                console.log(docs)

            })
            .catch(err => {

                console.error(err)

            })


    Nous avons couvert les bases de Mongoose et ce qu'il peut faire. Il s'agit d'une bibliothèque riche, pleine de fonctionnalités utiles et puissantes qui rendent le travail avec des modèles de données dans la couche applicative très agréable.

    Bien que vous puissiez interagir directement avec Mongo à l'aide de Mongo Driver, Mongoose simplifiera cette interaction en vous permettant de modéliser les relations entre les données et de les valider facilement.

    Fait amusant : Valeri Karpov, le créateur de Mongoose, est celui qui a inventé le terme The MEAN Stack .



    