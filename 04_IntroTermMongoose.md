Chapitre : INTRODUCTION ET TERMINOLOGIES DE MONGOOSE

A-  Qu'est ce que Mongoose ?

    Mongoose est une bibliothèque de modélisation de données d'objets (ODM) pour MongoDB et Node.JS. Elle est responsable de :

        *   Gestion des relations entre les données.

        *   Fournir une validation de schéma.

        *   Implémentation de la traduction entre les objets de code et la représentation de ces objets dans MongoDB.

    https://i.imgur.com/s1kR3ns.png

B-  Comparaison MongoDB et SQL

    Vous savez maintenant que MongoDB est une base de données de documents NoSQL sans schéma . Cela signifie que vous pouvez y stocker des documents JSON. La structure de ces documents peut varier car elle n'est pas imposée comme les bases de données SQL. C'est l'un des avantages de l'utilisation de NoSQL car elle accélère le développement des applications et réduit la complexité des déploiements.

    L'exemple ci-dessous est un rappel de la manière dont les données sont stockées dans Mongo par rapport à la base de données SQL :

        https://i.imgur.com/jGIhMkJ.png

C-  Terminologies

    Pour rappel, voici quelques terminologies utilisées avec la base de données NoSQL :

    *   Collections : Les collections dans Mongo sont l'équivalent des tables dans les bases de données relationnelles. Elles peuvent contenir plusieurs documents JSON.

    *   Documents : les documents sont l'équivalent des enregistrements ou des lignes de données dans SQL. Alors qu'une ligne SQL peut référencer des données dans d'autres tables, les documents Mongo les combinent généralement dans un seul document.

    *   champs ou attributs similaires aux colonnes d'une table SQL.

    *   Schéma : alors que Mongo n'a pas de schéma, SQL définit un schéma via la définition de table. Un « schéma » Mongoose est une structure de données de document (ou une forme du document) qui est appliquée via la couche application.

    *   Modèles : Les modèles sont des constructeurs d’ordre supérieur qui prennent un schéma et créent une instance d’un document équivalent aux enregistrements d’une base de données relationnelle.

    *   Références : Les références sont référencées d'un document à un autre en utilisant la valeur du document référencé (parent).

        https://i.imgur.com/YkjquXc.png

    