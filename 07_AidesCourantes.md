Chapitre : AIDES COURANTES

A-  Aides

    Jusqu'à présent, nous avons examiné certaines des fonctionnalités de base connues sous le nom d'opérations CRUD (Créer, Lire, Mettre à jour, Supprimer), mais Mongoose offre également la possibilité de configurer plusieurs types de méthodes et de propriétés d'assistance. Celles-ci peuvent être utilisées pour simplifier davantage le travail avec les données.

    Créons un schéma utilisateur ./src/models/user.jsavec les champs firstNameet lastName:

        let mongoose = require('mongoose')

        let userSchema = new mongoose.Schema({

            firstName: String,
            
            lastName: String

        })

        module.exports = mongoose.model('User', userSchema)

    -   Propriété virtuelle
    
    Une propriété virtuelle n'est pas limitée à la base de données. Nous pouvons l'ajouter à notre schéma en tant qu'aide pour obtenir et définir des valeurs.
    Créons une propriété virtuelle appelée fullNamequi peut être utilisée pour définir des valeurs sur firstNameet lastNameles récupérer sous forme de valeur combinée lors de la lecture :

        userSchema.virtual('fullName').get(function() {
            
            return this.firstName + ' ' + this.lastName
        
        })
        
        userSchema.virtual('fullName').set(function(name) {
        
            let str = name.split(' ')
        
            this.firstName = str[0]
        
            this.lastName = str[1]
        })

    Les rappels pour get et set doivent utiliser le mot-clé function car nous devons accéder au modèle via le mot-clé this . L'utilisation de fonctions à flèches épaisses modifiera ce à quoi this fait référence.

    Maintenant, nous pouvons définir firstNameet lastNameen attribuant une valeur à fullName:

        let model = new UserModel()

        model.fullName = 'Thomas Anderson'

        console.log(model.toJSON())  // Output model fields as JSON

        console.log()

        console.log(model.fullName)  // Output the full name

    Le code ci-dessus affichera le résultat suivant :

        { _id: 5a7a4248550ebb9fafd898cf,
            
            firstName: 'Thomas',
        
            lastName: 'Anderson' 
        }

        Thomas Anderson

    -   Méthodes d'instance

    Nous pouvons créer des méthodes d'assistance personnalisées sur le schéma et y accéder via l'instance du modèle. Ces méthodes auront accès à l'objet modèle et peuvent être utilisées de manière assez créative. Par exemple, nous pourrions créer une méthode pour trouver toutes les personnes qui ont le même prénom que l'instance actuelle.
    Dans cet exemple, créons une fonction pour renvoyer les initiales de l'utilisateur actuel. Ajoutons une méthode d'assistance personnalisée appelée getInitialsau schéma :  

        userSchema.methods.getInitials = function() {

            return this.firstName[0] + this.lastName[0]

        }

    Cette méthode sera accessible via une instance de modèle :

        let model = new UserModel({

            firstName: 'Thomas',

            lastName: 'Anderson'

        })
        let initials = model.getInitials()
        console.log(initials) // This will output: TA

    -   Méthodes statiques

    Similaires aux méthodes d'instance, nous pouvons créer des méthodes statiques sur le schéma. Créons une méthode pour récupérer tous les utilisateurs de la base de données :

        userSchema.statics.getUsers = function() {

            return new Promise((resolve, reject) => {

                this.find((err, docs) => {

                if(err) {

                    console.error(err)

                    return reject(err)

                }

                    resolve(docs)

                })

            })

        }

    L'appel getUsers à la classe Model renverra tous les utilisateurs de la base de données :

        UserModel.getUsers()
            .then(docs => {

                console.log(docs)

            })
            .catch(err => {

                console.error(err)

            })

    L'ajout d'instances et de méthodes statiques est une bonne approche pour implémenter une interface pour les interactions de base de données sur les collections et les enregistrements.

    -   Middleware (Intergiciel)

    Le middleware est un ensemble de fonctions qui s'exécutent à des étapes spécifiques d'un pipeline. Mongoose prend en charge le middleware pour les opérations suivantes :

        *   Agrégat

        *   Document

        *   Modèle
 
        *   Requête

    Par exemple, les modèles ont des fonctions pre et post qui prennent deux paramètres :

        1-  Type d'événement ('init', 'validate', 'save', 'remove')

        2-  Un rappel qui est exécuté avec cette référence à l'instance du modèle

        Exemple de middleware (aka hooks pré et post) :

            https://i.imgur.com/mdas1cw.png

    Démontrons cela, à titre d’exemple, en ajoutant deux champs appelés createdAt et updatedAt à notre schéma :

        let mongoose = require('mongoose')

        let userSchema = new mongoose.Schema({

            firstName: String,

            lastName: String,

            createdAt: Date,

            updatedAt: Date

        })

        module.exports = mongoose.model('User', userSchema)

    Lorsque model.save()est appelé, un pre(‘save’, …)événement post(‘save’, …)est déclenché. Pour le deuxième paramètre, vous pouvez passer une fonction qui est appelée lorsque l'événement est déclenché. Ces fonctions prennent un paramètre pour la fonction suivante dans la chaîne middleware.

    Ajoutons un hook de pré-enregistrement et définissons des valeurs pour createdAtetupdatedAt :

        userSchema.pre('save', function (next) {

            let now = Date.now()

            
            this.updatedAt = now

            // Set a value for createdAt only if it is null
            if (!this.createdAt) {

                this.createdAt = now

            }

            // Call the next function in the pre-save chain

            next()    

        })

    Créons et sauvegardons notre modèle :

        let UserModel = require('./user')
        let model = new UserModel({
            fullName: 'Thomas Anderson'
        })

        msg.save()
            .then(doc => {
                console.log(doc)
            })
            .catch(err => {
                console.error(err)
            })

    Vous devriez voir les valeurs pour createdAtet updatedAtquand l'enregistrement créé est imprimé :

        { _id: 5a7bbbeebc3b49cb919da675,
            firstName: 'Thomas',
            lastName: 'Anderson',
            updatedAt: 2018-02-08T02:54:38.888Z,
            createdAt: 2018-02-08T02:54:38.888Z,
            __v: 0
        }

    -   Plugins

    Supposons que nous souhaitons savoir quand un enregistrement a été créé et mis à jour pour la dernière fois dans chaque collection de notre base de données. Au lieu de répéter le processus ci-dessus, nous pouvons créer un plugin et l'appliquer à chaque schéma.

    Créons un fichier ./src/model/plugins/timestamp.jset reproduisons la fonctionnalité ci-dessus en tant que module réutilisable :

        module.exports = function timestamp(schema) {

        // Add the two fields to the schema

            schema.add({ 

                createdAt: Date,

                updatedAt: Date

            })

            // Create a pre-save hook

            schema.pre('save', function (next) {

                let now = Date.now()
            
                this.updatedAt = now

                // Set a value for createdAt only if it is null

                if (!this.createdAt) {

                    this.createdAt = now

                }

                // Call the next function in the pre-save chain

                next() 

            })
        }
    
    Pour utiliser ce plugin, il suffit de le transmettre aux schémas qui doivent recevoir cette fonctionnalité :

        let timestampPlugin = require('./plugins/timestamp')

        emailSchema.plugin(timestampPlugin)
        
        userSchema.plugin(timestampPlugin)



