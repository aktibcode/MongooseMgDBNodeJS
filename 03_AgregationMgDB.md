Chapitre : AGREGATION MONGODB AVEC NODEJS

A-  Création d'un pipeline d'agrégation MongoDB dans les applications Node.js

        https://youtu.be/bZ4bvm56yqg

B-  Utilisation des étapes d'agrégation MongoDB avec Node.js : $match et $group

        https://youtu.be/0JLhGj-yXds

    Consultez le code suivant, qui montre comment créer les étapes $matchet $groupd’un pipeline d’agrégation dans MongoDB avec Node.js.

    -   Utilisation de $match

    L'agrégation vous permet de transformer les données de votre collection en faisant passer les documents d'une étape à une autre. Ces étapes peuvent être constituées d'opérateurs qui transforment ou organisent vos données d'une manière spécifique. Dans cette leçon, nous avons utilisé $matchet $group.

    L' $match étape filtre les documents en utilisant une correspondance d'égalité simple, comme $match: { author: "Dave"}, ou des expressions d'agrégation utilisant des opérateurs de comparaison, comme $match: { likes: { $gt: 100 } }. Cet opérateur accepte un document de requête et transmet les documents résultants à l'étape suivante. $match doit être placé au début de votre pipeline pour réduire le nombre de documents à traiter.

    -   Utilisation de $group

    L' $group étape sépare les documents selon une clé de groupe et renvoie un document pour chaque clé de groupe unique. La clé de groupe est généralement un champ du document, mais elle peut également être une expression qui se résout en un champ. L' $groupétape peut être utilisée avec des expressions d'agrégation pour effectuer des calculs sur les documents groupés. Un exemple de ceci est l'addition du nombre total de billets de cinéma vendus en utilisant l' $sum opérateur :

    $group: { _id: "$movie", totalTickets: { $sum: "$tickets" } }

    Le "$movie" est la clé du groupe et le totalTickets champ est le résultat de l' $sum opérateur.

    Dans le code suivant, nous attribuons le nom de notre collection à une variable pour plus de commodité. Tout d'abord, nous déclarons certaines variables pour contenir la connexion à la base de données et la collection que nous utiliserons :

        const client = new MongoClient(uri)
        const dbname = "bank";
        const collection_name = "accounts";
        const accountsCollection = client.db(dbname).collection(collection_name);

    Ensuite, nous créons un pipeline d'agrégation qui utilise $matchet $groupet qui recherchera les comptes avec un solde inférieur à 1 000 $. Ensuite, nous regroupons les résultats par account_type, et calculons le total_balanceet avg_balancepour chaque type.

        const pipeline = [
            // Stage 1: match the accounts with a balance less than $1,000
            { $match: { balance: { $lt: 1000 } } },
            // Stage 2: Calculate average balance and total balance
            {
                $group: {
                _id: "$account_type",
                total_balance: { $sum: "$balance" },
                avg_balance: { $avg: "$balance" },
                },
            },
        ]

    Pour exécuter un pipeline d'agrégation, nous ajoutons la méthode aggregate à la collection. La méthode aggregate prend un tableau d'étapes comme argument, qui est stocké dans une variable. La méthode aggregate renvoie un curseur sur lequel nous pouvons effectuer une itération pour obtenir les résultats.

        const main = async () => {
            try {
                await client.connect()
                console.log(`Connected to the database 🌍. \nFull connection string: ${safeURI}`)
                let result = await accountsCollection.aggregate(pipeline)
                for await (const doc of result) {
                console.log(doc)
                }
            } catch (err) {
                console.error(`Error connecting to the database: ${err}`)
            } finally {
                await client.close()
            }
        }

        main()

C-  Utilisation des étapes d'agrégation MongoDB avec Node.js : $sort et $project

        https://youtu.be/wdwb-MKQsOU

        Examinez le code suivant, qui montre comment créer les étapes $sort et $project d’un pipeline d’agrégation dans MongoDB avec Node.js.

    -   $sort

        L'agrégation est un outil puissant qui nous donne la capacité de calculer et de transformer nos données. Dans cette compétence, nous nous sommes concentrés sur les étapes $sort et $project.

        L' $sort étape prend tous les documents d'entrée et les trie dans un ordre spécifique. Les documents peuvent être triés par ordre numérique, alphabétique, croissant ou décroissant.

        L' $sort étape accepte une clé de tri qui spécifie le champ sur lequel effectuer le tri. La clé de tri peut être 1 pour l'ordre croissant ou -1 pour l'ordre décroissant. Par exemple :

        { $sort: { balance: 1 } } trie les documents par ordre croissant selon le balance champ.

        { $sort: { balance: -1 } } trie les documents par ordre décroissant selon le balancechamp.

    -   $project

        L' $project étape prend tous les documents d'entrée et transmet uniquement un sous-ensemble des champs de ces documents en spécifiant les champs à inclure ou à exclure.

        Par exemple, si nous voulons que nos documents résultants incluent uniquement le account_id, nous écrivons { $project: { _id: 0, account_id: 1 } }. Le champ _id est exclu en le définissant sur 0, et le account_id champ est inclus en le définissant sur 1.

        La $project scène peut également créer de nouveaux champs calculés à partir des données des documents d'origine. Par exemple, elle peut créer un champ projeté contenant le nom complet d'une personne, alors que seuls le prénom et le nom sont stockés dans le document d'origine.

        Dans l'exemple suivant, nous créons un pipeline d'agrégation qui utilise $match, $sort, et $project, et qui recherchera les comptes chèques dont le solde est supérieur ou égal à 1 500 $. Ensuite, nous trions les résultats par solde dans l'ordre décroissant et renvoyons uniquement account_id, account_type, balance, et un nouveau champ calculé nommé gbp_balance, qui correspond au solde en livres sterling (GBP).

            const pipeline = [
                // Stage 1: $match - filter the documents (checking, balance >= 1500)
                    { $match: { account_type: "checking", balance: { $gte: 1500 } } },

                // Stage 2: $sort - sorts the documents in descending order (balance)
                    { $sort: { balance: -1 } },

                // Stage 3: $project - project only the requested fields and one computed field (account_type, account_id, balance, gbp_balance)
                {
                    $project: {
                    _id: 0,
                    account_id: 1,
                    account_type: 1,
                    balance: 1,
                    // GBP stands for Great British Pound
                    gbp_balance: { $divide: ["$balance", 1.3] },
                    },
                },
            ]

        Pour exécuter un pipeline d'agrégation, nous ajoutons la méthode aggregate à la collection. La méthode aggregate prend un tableau d'étapes comme argument, qui est stocké ici en tant que variable. La méthode aggregate renvoie un curseur sur lequel nous pouvons effectuer une itération pour obtenir les résultats.

            const main = async () => {
                try {
                    await client.connect()
                    console.log(`Connected to the database 🌍\n ${uri}`)
                    let accounts = client.db("bank").collection("accounts")
                    let result = await accounts.aggregate(pipeline)
                    for await (const doc of result) {
                    console.log(doc)
                    }
                } catch (err) {
                    console.error(`Error connecting to the database: ${err}`)
                } finally {
                    await client.close()
                }
            }

            main()

D-  Conclusion

    Agrégation MongoDB avec Node.js
    Dans cette compétence, vous avez appris à :

        *   Définissez un pipeline d’agrégation et ses étapes et opérateurs.

        *   Construisez les étapes $matchet $groupd'un pipeline dans Node.js

        *   Construisez les étapes $sortet $projectd'un pipeline dans Node.js.

        https://youtu.be/7pPHMdrnQO0

