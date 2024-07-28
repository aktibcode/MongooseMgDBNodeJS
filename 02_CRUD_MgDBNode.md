Chapitre : OPERATION CRUD MONGODB DANS NODEJS

A-  Travailler avec des documents MongoDB dans Node.js

        https://youtu.be/ztW9_W7REPY

B- Insérer un document dans les applications Node.js

        https://youtu.be/NLl1pyt68T8

    Examinez le code suivant, qui montre comment insérer un seul document et plusieurs documents dans une collection.

    -   Insérer un document

        Pour insérer un seul document dans une collection, ajoutez insertOne() à la variable collection. La méthode insertOne() accepte un document comme argument et renvoie une promesse. Dans cet exemple, le document en cours d'insertion est stocké dans une variable appelée sampleAccount, qui est déclarée juste au-dessus de la fonction main().

            const dbname = "bank"
            const collection_name = "accounts"
            
            const accountsCollection = client.db(dbname).collection(collection_name)

            const sampleAccount = {
                account_holder: "Linus Torvalds",
                account_id: "MDB829001337",
                account_type: "checking",
                balance: 50352434,
            }

            const main = async () => {
                try {
                    await connectToDatabase()
                    // insertOne method is used here to insert the sampleAccount document
                    let result = await accountsCollection.insertOne(sampleAccount)
                    console.log(`Inserted document: ${result.insertedId}`)
                } catch (err) {
                    console.error(`Error inserting document: ${err}`)
                } finally {
                    await client.close()
                }
            }
            
            main()

    -   Insérer plusieurs documents

        Pour insérer plusieurs documents, ajoutez la méthode insertMany() à l'objet collection, puis transmettez un tableau de documents à la méthode insertMany(). La méthode insertMany() renvoie une promesse. Nous attendons la promesse pour obtenir le résultat de l'opération, que nous utilisons ensuite pour enregistrer le nombre de documents insérés dans la console. Dans cet exemple, les comptes à insérer sont stockés dans une variable de tableau appelée sampleAccounts. Cette variable est définie juste au-dessus de la fonction main().

            const dbname = "bank"
            const collection_name = "accounts"
            
            const accountsCollection = client.db(dbname).collection(collection_name)

            const sampleAccounts = [
                {
                    account_id: "MDB011235813",
                    account_holder: "Ada Lovelace",
                    account_type: "checking",
                    balance: 60218,
                },
                {
                    account_id: "MDB829000001",
                    account_holder: "Muhammad ibn Musa al-Khwarizmi",
                    account_type: "savings",
                    balance: 267914296,
                },
            ]

            const main = async () => {
                try {
                    await connectToDatabase()
                    let result = await accountsCollection.insertMany(sampleAccounts)
                    console.log(`Inserted ${result.insertedCount} documents`)
                    console.log(result)
                } catch (err) {
                    console.error(`Error inserting documents: ${err}`)
                } finally {
                    await client.close()
                }
            }

            main()

C-  Interrogation d'une collection MongoDB dans les applications Node.js

        https://youtu.be/sUr3YCq8Tqo

    Examinez le code suivant, qui montre comment interroger des documents dans MongoDB avec Node.js.

    -   Utilisation de find()

    La méthode find() est une opération de lecture qui renvoie un curseur vers les documents correspondant à la requête. La méthode find() prend un document de requête ou de filtre comme argument. Si vous ne spécifiez pas de document de requête, la méthode find() renvoie tous les documents de la collection.

    Dans cet exemple, nous recherchons tous les comptes dont le solde est supérieur ou égal à 4700. La méthode find() accepte un filtre de requête, que nous affectons à une variable appelée documentsToFind. Nous traitons chaque document renvoyé par la méthode find() en itérant le curseur, qui est affecté à la variable result.

        const dbname = "bank"
        const collection_name = "accounts"
        
        const accountsCollection = client.db(dbname).collection(collection_name)

        // Document used as a filter for the find() method
        const documentsToFind = { balance: { $gt: 4700 } }

        const main = async () => {
            try {
                await connectToDatabase()
                // find() method is used here to find documents that match the filter
                let result = accountsCollection.find(documentsToFind)
                let docCount = accountsCollection.countDocuments(documentsToFind)
                await result.forEach((doc) => console.log(doc))
                console.log(`Found ${await docCount} documents`)
            } catch (err) {
                console.error(`Error finding documents: ${err}`)
            } finally {
                await client.close()
            }
        }

        main()

    -   Utilisation de findOne()

    Dans cet exemple, nous renvoyons un seul document à partir d'une requête, qui est affecté à une variable appelée documentToFind. Nous utilisons la méthode findOne() sur l'objet de collection pour renvoyer le premier document qui correspond aux critères de filtre, qui sont définis dans la variable documentToFind.

        const dbname = "bank"
        const collection_name = "accounts"
        
        const accountsCollection = client.db(dbname).collection(collection_name)

        // Document used as a filter for the findOne() method
        const documentToFind = { _id: ObjectId("62a3638521a9ad028fdf77a3") }

        const main = async () => {
            try {
                await connectToDatabase()
                // findOne() method is used here to find a the first document that matches the filter
                let result = await accountsCollection.findOne(documentToFind)
                console.log(`Found one document`)
                console.log(result)
            } catch (err) {
                console.error(`Error finding document: ${err}`)
            } finally {
                await client.close()
            }
        }

        main()

D-  Mise à jour des documents dans les applications Node.js

        https://youtu.be/Ay-JBzbOxWo

    Examinez le code suivant, qui montre comment mettre à jour des documents dans MongoDB avec Node.js.

    -   Utilisation de updateOne()

    Dans cet exemple, nous utilisons updateOne() pour mettre à jour la valeur d'un champ existant dans un document. Ajoutez la méthode updateOne() à l'objet de collection pour mettre à jour un seul document qui correspond aux critères de filtre, qui sont stockés dans la variable documentToUpdate. Le document de mise à jour contient les modifications à apporter et est stocké dans la variable update.

            const dbname = "bank"
            const collection_name = "accounts"

            const accountsCollection = client.db(dbname).collection(collection_name)

            const documentToUpdate = { _id: ObjectId("62d6e04ecab6d8e130497482") }

            const update = { $inc: { balance: 100 } }

            const main = async () => {
                try {
                    await connectToDatabase()
                    let result = await accountsCollection.updateOne(documentToUpdate, update)
                    result.modifiedCount === 1
                        ? console.log("Updated one document")
                        : console.log("No documents updated")
                } catch (err) {
                    console.error(`Error updating document: ${err}`)
                } finally {
                    await client.close()
                }
            }

            main()

    -   Utilisation de updateMany()

    Dans cet exemple, nous mettons à jour de nombreux documents en ajoutant une valeur au tableau transfer_complete de tous les documents de compte courant. La méthode updateMany() est ajoutée à l'objet de collection. La méthode accepte un filtre qui correspond au(x) document(s) que nous voulons mettre à jour et une instruction de mise à jour qui indique au conducteur comment modifier le document correspondant. Le filtre et les documents de mise à jour sont stockés dans des variables. La méthode updateMany() met à jour tous les documents de la collection qui correspondent au filtre.

        const database = client.db(dbname);
        const bank = database.collection(collection_name);

        const documentsToUpdate = { account_type: "checking" };

        const update = { $push: { transfers_complete: "TR413308000" } }

        const main = async () => {
            try {
                await connectToDatabase()
                let result = await accountsCollection.updateMany(documentsToUpdate, update)
                result.modifiedCount > 0
                ? console.log(`Updated ${result.modifiedCount} documents`)
                : console.log("No documents updated")
            } catch (err) {
                console.error(`Error updating documents: ${err}`)
            } finally {
                await client.close()
            }
        }

        main()

E-  Suppression de documents dans les applications Node.js

        https://youtu.be/5QmmAFxEiu0

    Consultez le code suivant, qui montre comment supprimer des documents dans MongoDB avec Node.js.

    -   Utilisation de deleteOne()

    Pour supprimer un seul document d'une collection, utilisez la méthode deleteOne() sur un objet de collection. Cette méthode accepte un filtre de requête qui correspond au document que vous souhaitez supprimer. Si vous ne spécifiez pas de filtre, MongoDB recherche et supprime le premier document de la collection. Voici un exemple :

        const dbname = "bank"
        const collection_name = "accounts"

        const accountsCollection = client.db(dbname).collection(collection_name)

        const documentToDelete = { _id: ObjectId("62d6e04ecab6d8e13049749c") }

        const main = async () => {
            try {
                await connectToDatabase()
                let result = await accountsCollection.deleteOne(documentToDelete)
                result.deletedCount === 1
                ? console.log("Deleted one document")
                : console.log("No documents deleted")
            } catch (err) {
                console.error(`Error deleting documents: ${err}`)
            } finally {
                await client.close()
            }
        }

        main()

    -   Utilisation de deleteMany()
    
    Vous pouvez supprimer plusieurs documents d'une collection en une seule opération en appelant la méthode deleteMany() sur un objet de collection. Pour spécifier les documents à supprimer, transmettez un filtre de requête qui correspond aux documents que vous souhaitez supprimer. Si vous fournissez un document vide, MongoDB fait correspondre tous les documents de la collection et les supprime. Dans l'exemple suivant, nous supprimons tous les comptes avec un solde inférieur à 500 à l'aide d'un filtre de requête. Ensuite, nous imprimons le nombre total de documents supprimés.

        const dbname = "bank"
        const collection_name = "accounts"

        const accountsCollection = client.db(dbname).collection(collection_name)

        const documentsToDelete = { balance: { $lt: 500 } }

        const main = async () => {
            try {
                await connectToDatabase()
                let result = await accountsCollection.deleteMany(documentsToDelete)
                result.deletedCount > 0
                    ? console.log(`Deleted ${result.deletedCount} documents`)
                    : console.log("No documents deleted")
            } catch (err) {
                console.error(`Error deleting documents: ${err}`)
            } finally {
                await client.close()
            }
        }
            
        main()

F-  Création de transactions MongoDB dans des applications Node.js

        https://youtu.be/V6bBU1Im5bk

    Examinez le code suivant, qui montre comment créer des transactions multi-documents dans MongoDB avec Node.js.

    -   Créer une transaction

    Dans cette section, nous allons parcourir le code pour créer une transaction étape par étape. Nous démarrons la transaction en utilisant la withTransaction()méthode de la session. Nous définissons ensuite la séquence d'opérations à effectuer à l'intérieur des transactions, en passant l' sessionobjet à chaque opération dans les transactions.

    1-  Créer des variables utilisées dans la transaction.

        // Collections
        const accounts = client.db("bank").collection("accounts")
        const transfers = client.db("bank").collection("transfers")

        // Account information
        let account_id_sender = "MDB574189300"
        let account_id_receiver = "MDB343652528"
        let transaction_amount = 100

    2-  Démarrer une nouvelle session.

        const session = client.startSession()

    3-  Commencez une transaction avec la WithTransaction()méthode sur la session.

        const transactionResults = await session.withTransaction(async () => {
            // Operations will go here
        })

    4-  Mettre à jour le balancechamp du compte de l'expéditeur en décrémentant transaction_amountle champ solde.

        const senderUpdate = await accounts.updateOne(
            { account_id: account_id_sender },
            { $inc: { balance: -transaction_amount } },
            { session }
        )

    5-  Mettre à jour le balancechamp du compte du destinataire en incrémentant transaction_amountle champ solde.

        const receiverUpdate = await accounts.updateOne(
            { account_id: account_id_receiver },
            { $inc: { balance: transaction_amount } },
            { session }
        )

    6-  Créez un document de transfert et insérez-le dans la collection de transferts.

        const transfer = {
            transfer_id: "TR21872187",
            amount: 100,
            from_account: account_id_sender,
            to_account: account_id_receiver,
        }

        const insertTransferResults = await transfers.insertOne(transfer, { session })

    7-  Mettez à jour le transfers_completetableau du compte de l'expéditeur en ajoutant le transfer_idau tableau.

        const updateSenderTransferResults = await accounts.updateOne(
            { account_id: account_id_sender },
            { $push: { transfers_complete: transfer.transfer_id } },
            { session }
        )

    8-  Mettez à jour le transfers_completetableau du compte du récepteur en ajoutant le transfer_idau tableau.

        const updateReceiverTransferResults = await accounts.updateOne(
            { account_id: account_id_receiver },
            { $push: { transfers_complete: transfer.transfer_id } },
            { session }
        )

    9-  Enregistrez un message concernant le succès ou l’échec de la transaction.

        if (transactionResults) {
            console.log("Transaction completed successfully.")
        } else {
            console.log("Transaction failed.")
        }

    10- Détectez les erreurs et fermez la session.

        {} catch (err) {
            console.error(`Transaction aborted: ${err}`)
            process.exit(1)
        } finally {
            await session.endSession()
        await client.close()
        }

G-  Conclusion

        https://youtu.be/qT17N4TVmcY

    Dans cette compétence, vous avez appris à :

        *   Documents express en Node.js
        *   Insérer des documents en utilisant insertOne()etinsertMany().
        *   Interrogez les documents en utilisant findOne()et find().
        *   Supprimez les documents en utilisant deleteOne()et deleteMany().
        *   Créer une transaction multi-documents.

