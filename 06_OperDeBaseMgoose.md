Chapitre :  OPERATION DE BASE DANS MONGOOSE

A-  Opérations de base

    Mongoose dispose d'une API flexible et propose de nombreuses façons d'accomplir une tâche. Nous ne nous concentrerons pas sur les variantes, car cela dépasse le cadre de ce cours. Cependant, essayez de vous rappeler que la plupart des opérations peuvent être effectuées de plusieurs manières, soit syntaxiquement, soit via l'architecture de l'application.

    -   Créer un enregistrement

        Créons une instance du modèle de courrier électronique et enregistrons-la dans la base de données :

        let EmailModel = require('./email')

        let msg = new EmailModel({

            email: 'ADA.LOVELACE@GMAIL.COM'

        })

        msg.save()
            .then(doc => {
                console.log(doc)
            })
            .catch(err => {
                console.error(err)
            })

    Le résultat est un document qui est renvoyé après une sauvegarde réussie :

        { 
        _id: 5a78fe3e2f44ba8f85a2409a,
        email: 'ada.lovelace@gmail.com',
        __v: 0 
        }

    Les champs suivants sont renvoyés (les champs internes sont préfixés par un trait de soulignement) :

        1-  Le _idchamp est généré automatiquement par Mongo et constitue une clé primaire de la collection. Sa valeur est un identifiant unique pour le document.
        
        2-  La valeur du emailchamp est renvoyée. Notez qu'elle est en minuscules car nous avons spécifié l' lowercase:trueattribut dans le schéma.
        
        3-  __vest la propriété versionKey définie sur chaque document lors de sa première création par Mongoose. Sa valeur contient la révision interne du document.
        Si  vous essayez de répéter l'opération d'enregistrement ci-dessus, vous obtiendrez une erreur car nous avons spécifié que le champ e-mail doit être unique.
        311790:0

    -   Récupérer l'enregistrement

        Essayons de récupérer l'enregistrement que nous avons enregistré dans la base de données plus tôt. La classe modèle expose plusieurs méthodes statiques et d'instance pour effectuer des opérations sur la base de données. Nous allons maintenant essayer de trouver l'enregistrement que nous avons créé précédemment à l'aide de la méthode find et de transmettre l'e-mail comme terme de recherche.

            EmailModel
                .find({

                    email: 'ada.lovelace@gmail.com'   // search query

                })
                .then(doc => {

                    console.log(doc)

                })
                .catch(err => {

                    console.error(err)

                })

        Le document renvoyé sera similaire à celui affiché lors de la création de l'enregistrement :

            { 
                _id: 5a78fe3e2f44ba8f85a2409a,
                email: 'ada.lovelace@gmail.com',
                __v: 0 
            }

    -   Mettre à jour l'enregistrement

        Modifions l'enregistrement ci-dessus en changeant l'adresse e-mail et en y ajoutant un autre champ, le tout en une seule opération. Pour des raisons de performances, Mongoose ne renverra pas le document mis à jour, nous devons donc passer un paramètre supplémentaire pour le demander :

            EmailModel
                .findOneAndUpdate(

                    {
                        email: 'ada.lovelace@gmail.com' // search query
                    },

                    {
                        email: 'theoutlander@live.com'  // field:values to update
                    },

                    {
                        new: true,       // return updated doc
                        runValidators: true  // validate before update
                    }

                )
                .then(doc => {
                    console.log(doc)
                })
                .catch(err => {
                    console.error(err)
                })

        Le document renvoyé contiendra l'email mis à jour :

                { 
                _id: 5a78fe3e2f44ba8f85a2409a,
                email: 'theoutlander@live.com',
                __v: 0 
                }

    -   Supprimer l'enregistrement

        Nous allons utiliser l' findOneAndRemove appel pour supprimer un enregistrement. Il renvoie le document d'origine qui a été supprimé :

            EmailModel
                .findOneAndRemove({
                    
                    email: 'theoutlander@live.com'
                    
                })
                .then(response => {
                    
                    console.log(response)
                    
                })
                .catch(err => {
                    
                    console.error(err)
                    
                })
