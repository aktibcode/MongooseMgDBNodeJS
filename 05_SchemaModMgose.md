Chapitre :  SCHEMA ET MODELE DE MONGOOSE

A-  Debut

    -   Installation de Mongoose

    Allons dans le dossier du projet et initialisons notre projet :

        npm init -y

    Installons Mongoose et une bibliothèque de validation avec la commande suivante :

        npm install mongoose validator

    La commande d'installation ci-dessus installera la dernière version des bibliothèques. La syntaxe Mongoose de ce cours est spécifique à Mongoose v5 et au-delà .

B-  Connexion à la base de données

    Créez un fichier ./src/database.js sous la racine du projet.
    Ensuite, nous ajouterons une classe simple avec une méthode qui se connecte à la base de données.
    Votre chaîne de connexion variera en fonction de votre installation.

        let mongoose = require('mongoose');
        const server = '127.0.0.1:27017'; // REPLACE WITH YOUR DB SERVER
        const database = 'myDB';      // REPLACE WITH YOUR DB NAME

        class Database {
            constructor() {
                this._connect()
            }
            _connect() {
                mongoose.connect(`mongodb://${server}/${database}`)
                .then(() => {
                    console.log('Database connection successful')
                })
                .catch(err => {
                    console.error('Database connection error')
                })
            }
            }
        module.exports = new Database()

    L' require(‘mongoose’) appel ci-dessus renvoie un objet Singleton. Cela signifie que lorsque vous appelez require(‘mongoose’) pour la première fois, il crée une instance de la classe Mongoose et la renvoie. Lors des appels suivants, il renverra la même instance qui a été créée et qui vous a été renvoyée la première fois en raison du fonctionnement de l'importation/exportation de modules dans ES6.

C-  Note complémentaire

    À propos du module d'importation/requis du flux de travail :

        https://i.imgur.com/YTJSOrF.png

    Dans le code précédent, nous avons également transformé notre classe de base de données en singleton en renvoyant une instance de la classe dans l' module.exportsinstruction, car nous n'avons besoin que d'une seule connexion à la base de données.
    ES6 nous permet de créer très facilement un modèle singleton (instance unique) en raison de la façon dont le chargeur de module fonctionne en mettant en cache la réponse d'un fichier précédemment importé.

D-  Schéma Mongoose vs. Modèle

    Un modèle Mongoose est un wrapper sur le schéma Mongoose . Un schéma Mongoose définit la structure du document, les valeurs par défaut, les validateurs, etc... tandis qu'un modèle Mongoose fournit une interface à la base de données pour créer, interroger, mettre à jour, supprimer des enregistrements, etc.

    La création d'un modèle Mongoose comprend principalement trois parties :

    1-  Référencement de Mongoose

        let mongoose = require('mongoose')

        Cette référence sera la même que celle qui a été renvoyée lors de notre connexion à la base de données, ce qui signifie que les définitions de schéma et de modèle n'auront pas besoin de se connecter explicitement à la base de données.

    2-  Définition du schéma

        Un schéma définit les propriétés du document via un objet où le nom de la clé correspond au nom de la propriété dans la collection.

        let emailSchema = new mongoose.Schema({

            email: String

        })

        Nous définissons ici une propriété appelée email avec un type de schéma String qui correspond à un validateur interne qui sera déclenché lorsque le modèle sera enregistré dans la base de données. Il échouera si le type de données de la valeur n'est pas un type de chaîne.
            
            Les types de schéma suivants sont autorisés :

                *   Tableau
                *   Booléen
                *   Tampon
                *   Date
                *   Mixte (un type de données générique/flexible)
                *   Nombre
                *   ID d'objet
                *   Chaîne

        Mixed et ObjectId sont définis sous require(‘mongoose’).Schema.Types.

    3-  Exporter un modèle

        Nous devons appeler le constructeur du modèle sur l'instance Mongoose et lui transmettre le nom de la collection et une référence à la définition du schéma.

            module.exports = mongoose.model('Email', emailSchema)

        Combinons le code ci-dessus ./src/models/email.jspour définir le contenu d’un modèle de courrier électronique de base :

            let mongoose = require('mongoose')
            
            let emailSchema = new mongoose.Schema({

                email: String

            })

            module.exports = mongoose.model('Email', emailSchema)

        Une définition de schéma doit être simple, mais sa complexité dépend généralement des exigences de l'application. Les schémas peuvent être réutilisés et peuvent également contenir plusieurs schémas enfants. Dans l'exemple ci-dessus, la valeur de la propriété email est un type de valeur simple. Cependant, il peut également s'agir d'un type d'objet avec des propriétés supplémentaires.

        Nous pouvons également créer une instance du modèle que nous avons défini ci-dessus et le remplir à l'aide de la syntaxe suivante :

            let EmailModel = require('./email')

            let msg = new EmailModel({

                email: 'ada.lovelace@gmail.com'

            })

        Améliorons le schéma de messagerie pour faire de la propriété email un champ unique et obligatoire. Nous pouvons ensuite convertir la valeur en minuscules avant de l'enregistrer. Nous pouvons également ajouter une fonction de validation qui garantira que la valeur est une adresse email valide. Nous référencerons et utiliserons la bibliothèque de validation installée précédemment.

            let mongoose = require('mongoose')

            let validator = require('validator')

            let emailSchema = new mongoose.Schema({

                email: {
                    type: String,
                    required: true,
                    unique: true,
                    lowercase: true,
                    validate: (value) => {
                    return validator.isEmail(value)
                    }
                }

            })
            
            module.exports = mongoose.model('Email', emailSchema)