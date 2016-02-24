# MongoDb - Projeto Final
**Autor:** Fábio de Carvalho
**Data:** 1453940267012

## Para qual sistema você usaria o MongoDB (diferente desse)?
- ERP
- E-commerce
- Crawler
- Chat
- Social Network

## Qual a modelagem da sua coleção de `users`?

```js
{
    _id: ObjectId,
    name: String,
    bio: String,
    registered: Date,
    avatar: String,
    background: String
    auth: {
        username: String,
        email: String,
        password: String,
        last_access: Date,
        online: Boolean,
        disabled: Boolean,
        token_hash: String
    }
}
```

## Qual a modelagem da sua coleção de `projects`?

```js
{
    _id: ObjectId,
    name: String,
    description: String,
    begin_date: Date,
    dream_date: Date,
    end_date: Date,
    visible: Boolean,
    realocate: Boolean,
    expired: Date,
    visualizable_mod: Boolean,
    tags: [],
    members: [{
        _id: ObjectId,
        type: [],
        notify: Boolean
    }],
    goals: [{
        name: String,
        description: String,
        begin_date: Date,
        dream_date: Date,
        end_date: Date,
        realocate: Boolean,
        expired: Date,
        tags: [],
        historics: [
            { realocated: Date }
        ],
        activities: [
            { activity_id: ObjectId }
        ]
    }]
}
```

## Qual a modelagem da sua coleção retirada de `projects`?

A coleção retirada de `projects` foi a `activities`. Sua modelagem ficou assim:

```js
{
    _id: ObjectId,
    name: String,
    description: String,
    begin_date: Date,
    dream_date: Date,
    end_date: Date,
    realocate: Boolean,
    expired: Date,
    tags: [],
    historics: [
        { name: String }
    ],
    members: [{
        _id: ObjectId,
        type: [],
        notify: Boolean
    }],
    comments: [{
        _id: ObjectId,
        text: String,
        comment_date: Date,
        members: [{
            _id: ObjectId,
            type: [],
            notify: Boolean
        }],
        files: [{
            name: String,
            path: String,
            size: Integer,
        }]
    }]
}
```
#### Breve explicação sobre a escolha desta modelagem
A escolha de criar a coleção **`activities`** foi feita para tentar manter no tamanho limite de um documento no MongoDB e a complexidade da coleção **`projects`**, levando em consideração a importância hierárquica de cada objeto que compõe a coleção **`projects`** e o possível crescimento desta coleção.
Ao meu ver, as **`activities`** relacionadas com um objetivo (goal) não tem uma importância significativa para estar presente em todas requisições na coleção **`projects`** devido ao seu tamanho e fluxo de criação dessas **`activities`**, levando em consideração que um objetivo (goal) pode conter **N** **`activities`**, a possibilidade do tamanho limite por documento que o MongoDB possuí estourar aumenta consideravelmente.

## Create - cadastro

#### 1. Cadastre 10 usuários diferentes.

```js

    var users = [];
    for(var x = 1; x <= 10; x++) {
        var user = {
            name: 'User ' + x,
            bio: 'Some shit biography of the user ' + x,
            registered: new Date().toJSON(),
            avatar: 'http://pathtoassets.com/users/avatars/' + x + '.jpg',
            background: 'http://pathtoassets.com/users/backgrounds/' + x + '.jpg',
            auth: {
                username: 'user_' + x,
                email: 'user_' + x + '@webschool.io',
                password: new Date().getTime(),
                last_access: new Date().toJSON(),
                online: true,
                disabled: false,
                token_hash: 'da39a3ee5e6b4b0d3255bfef95601890afd80709'
            }
        };
        users.push(user);
    }

```
```js
    db.users.insert(users);

    Inserted 1 record(s) in 26ms
    BulkWriteResult({
        "writeErrors": [ ],
        "writeConcernErrors": [ ],
        "nInserted": 10,
        "nUpserted": 0,
        "nMatched": 0,
        "nModified": 0,
        "nRemoved": 0,
        "upserted": [ ]
    })
```

#### 2. Cadastre 5 projetos diferentes.
- cada um com 5 membros, sempre diferentes dentro dos projetos;
- cada um com pelo menos 3 tags diferentes;
    - escolha 1 *tag* onde deva ficar em 2 projetos;
    - escolha 1 *tag* onde deva ficar em 3 projetos;
- cada projeto com pelo menos 1 *goal*;
    - cada *goal* com pelo menos 3 *tags*;
    - cada *goal* com pelo menos 2 atividades, deixe 1 projeto sem.

```js

    // Array com todos os ObjectId dos users
    var usersId = db.users.find({}, {_id: 1}).toArray();

    // Array de objetos com as informações básicas dos projetos
    var projects = [
        {
            name: 'Project 1',
            tags: ['NodeJS', 'MongoDB', 'Mongoose'],
            goals: 2,
            activities: 3
        },
        {
            name: 'Project 2',
            tags: ['NodeJS', 'MongoDB', 'Atomic Design'],
            goals: 3,
            activities: 5
        },
        {
            name: 'Project 3',
            tags: ['Angular', 'MongoDB', 'ElasticSearch'],
            goals: 5,
            activities: 2
        },
        {
            name: 'Project 4',
            tags: ['Redis', 'Express', 'TDD'],
            goals: 1,
            activities: 4
        },
        {
            name: 'Project 5',
            tags: ['ReactJS', 'Security', 'MEAN Stack'],
            goals: 5,
            activities: 0
        }
    ];

    function randomNumber(base) {
        var pow = Math.pow(base, 2);
        return Math.floor(Math.random() * (base - pow)) + pow;
    }

    function generateTags() {
        var tags = [];

        for(x = 1; x <= 5; x++) {
            var number = Math.floor(Math.random() * (1 - 100)) + 100;
            tags.push('Tag ' + number);
        }

        return tags;
    }

    function generateMembers() {

        // Sempre iniciar com o array vazio
        var members = [];

        for(i = 0, count = projects.length; i < count; i++) {
            members.push({
                _id: usersId[randomNumber(3)]._id,
                type: ['Admin'],
                notify: false
            });
        }

        return members;
    }

    function generateActivity(project, randomNumber) {

        var activities = [];

        if(project.activities < 1) return activities;

        for(i = 1; i <= randomNumber; i++) {
            // Array de Tags
            var tags = generateTags();

            // ObjectId que será salvo na coleção
            var _id = new ObjectId();

            activities.push({ activity_id: _id });

            // Insere todos os dados na collection activities.
            db.activities.insert({
                _id: _id,
                name: 'Activity ' + i + ' of the project: ' + project.name,
                description: 'Description of activity ' + i + ' for project: ' + project.name,
                begin_date: new Date().toJSON(),
                dream_date: new Date().toJSON(),
                end_date: new Date().toJSON(),
                realocate: false,
                expired: new Date().toJSON(),
                tags: tags,
                historics: [],
                members: [],
                comments: []
            });
        }

        return activities;
    }

    function generateGoals(project) {
        // Array para armazenar todos os objetivos
        var goals = [];

        // Array que irá conter todas as atividades
        var activities = generateActivity(project, randomNumber(project.activities));

        // ObjectId que será salvo na coleção
        var _id = new ObjectId();

        for(i = 1; i <= project.goals; i++) {

            // Array de Tags Aleatórias
            var tags = generateTags();

            goals.push({
                _id: _id,
                name: 'Goal ' + i + ' for the project: ' + project.name,
                description: 'Description for a goal ' + i + ' of project: ' + project.name,
                begin_date: new Date().toJSON(),
                dream_date: new Date().toJSON(),
                end_date: new Date().toJSON(),
                realocate: false,
                expired: new Date().toJSON(),
                tags: tags,
                historics: [],
                activities: activities
            });

        }

        return goals;
    }

```

```js

    var projectsToInsert = [];

    projects.forEach(function(project, index) {
        projectsToInsert.push({
            name: project.name,
            description: 'Description of project: ' + project.name,
            begin_date: new Date().toJSON(),
            dream_date: new Date().toJSON(),
            end_date: new Date().toJSON(),
            visible: true,
            realocate: false,
            expired: new Date().toJSON(),
            visualizable_mod: true,
            tags: project.tags,
            members: generateMembers(index),
            goals: generateGoals(project)
        });
    });

    db.projects.insert(projectsToInsert);

    Inserted 1 record(s) in 16ms
    BulkWriteResult({
        "writeErrors": [ ],
        "writeConcernErrors": [ ],
        "nInserted": 5,
        "nUpserted": 0,
        "nMatched": 0,
        "nModified": 0,
        "nRemoved": 0,
        "upserted": [ ]
    })

```

## Retrieve - busca

#### 1. Liste as informações dos membros de 1 projeto específico que deve ser buscado pelo seu nome de forma a não ligar para maiúsculas e minúsculas.

```js

    var members = [];
    var projectMembers = db.projects.findOne({ name: /Project 1/i }, {_id: 0, members: 1});

    // Percorre o array de membros e insere as informações dos respectivos membros na variável 'members'
    projectMembers.members.forEach(function(member) {
        members.push(db.users.findOne({ _id: member._id }));
    });
```

##### Resultado da busca

```js

    [
        {
            "_id": ObjectId("56ae1f4a6dc690feb40a5d33"),
            "name": "User 1",
            "bio": "Some shit biography of the user 1",
            "registered": "2016-01-31T14:50:21.792Z",
            "avatar": "http://pathtoassets.com/users/avatars/1.jpg",
            "background": "http://pathtoassets.com/users/backgrounds/1.jpg",
            "auth": {
                "username": "user_1",
                "email": "user_1@webschool.io",
                "password": 1454251821798,
                "last_access": "2016-01-31T14:50:21.798Z",
                "online": true,
                "disabled": false,
                "token_hash": "da39a3ee5e6b4b0d3255bfef95601890afd80709"
            }
        },
        {
            "_id": ObjectId("56ae1f4a6dc690feb40a5d34"),
            "name": "User 2",
            "bio": "Some shit biography of the user 2",
            "registered": "2016-01-31T14:50:21.798Z",
            "avatar": "http://pathtoassets.com/users/avatars/2.jpg",
            "background": "http://pathtoassets.com/users/backgrounds/2.jpg",
            "auth": {
                "username": "user_2",
                "email": "user_2@webschool.io",
                "password": 1454251821798,
                "last_access": "2016-01-31T14:50:21.798Z",
                "online": true,
                "disabled": false,
                "token_hash": "da39a3ee5e6b4b0d3255bfef95601890afd80709"
            }
        },
        {
            "_id": ObjectId("56ae1f4a6dc690feb40a5d35"),
            "name": "User 3",
            "bio": "Some shit biography of the user 3",
            "registered": "2016-01-31T14:50:21.798Z",
            "avatar": "http://pathtoassets.com/users/avatars/3.jpg",
            "background": "http://pathtoassets.com/users/backgrounds/3.jpg",
            "auth": {
                "username": "user_3",
                "email": "user_3@webschool.io",
                "password": 1454251821798,
                "last_access": "2016-01-31T14:50:21.798Z",
                "online": true,
                "disabled": false,
                "token_hash": "da39a3ee5e6b4b0d3255bfef95601890afd80709"
            }
        },
        {
            "_id": ObjectId("56ae1f4a6dc690feb40a5d36"),
            "name": "User 4",
            "bio": "Some shit biography of the user 4",
            "registered": "2016-01-31T14:50:21.798Z",
            "avatar": "http://pathtoassets.com/users/avatars/4.jpg",
            "background": "http://pathtoassets.com/users/backgrounds/4.jpg",
            "auth": {
                "username": "user_4",
                "email": "user_4@webschool.io",
                "password": 1454251821798,
                "last_access": "2016-01-31T14:50:21.798Z",
                "online": true,
                "disabled": false,
                "token_hash": "da39a3ee5e6b4b0d3255bfef95601890afd80709"
            }
        },
        {
            "_id": ObjectId("56ae1f4a6dc690feb40a5d37"),
            "name": "User 5",
            "bio": "Some shit biography of the user 5",
            "registered": "2016-01-31T14:50:21.798Z",
            "avatar": "http://pathtoassets.com/users/avatars/5.jpg",
            "background": "http://pathtoassets.com/users/backgrounds/5.jpg",
            "auth": {
                "username": "user_5",
                "email": "user_5@webschool.io",
                "password": 1454251821798,
                "last_access": "2016-01-31T14:50:21.798Z",
                "online": true,
                "disabled": false,
                "token_hash": "da39a3ee5e6b4b0d3255bfef95601890afd80709"
            }
        }
    ]
```

#### 2. Liste todos os projetos com a tag que você escolheu para os 3 projetos em comum.

```js

    var query = { tags: { $in: ['MongoDB'] } };
    var mod = { _id: 1, name: 1, tags: 1 };
    var projects = db.projects.find(query, mod);

```

##### Resultado da busca

```js

    {
        "_id": ObjectId("56b60a240c648b9c842d6eee"),
        "name": "Project 1",
        "tags": [
            "NodeJS",
            "MongoDB",
            "Mongoose"
        ]
    }
    {
        "_id": ObjectId("56b60a240c648b9c842d6eef"),
        "name": "Project 2",
        "tags": [
            "NodeJS",
            "MongoDB",
            "Atomic Design"
        ]
    }
    {
        "_id": ObjectId("56b60a240c648b9c842d6ef0"),
        "name": "Project 3",
        "tags": [
            "Angular",
            "MongoDB",
            "ElasticSearch"
        ]
    }

    Fetched 3 record(s) in 9ms

```

#### 3. Liste apenas os nomes de todas as atividades para todos os projetos.

```js

    var activities = db.activities.find({}, { _id: 1, name: 1 });
    var projects = [];

    activities.forEach(function(activity) {

        var query = {'goals.activities.activity_id': activity._id};
        var project = db.projects.findOne(query, { _id: 0, name: 1 });

        projects.push({
            name: project.name,
            activity: activity.name
        });

    });

```

##### Resultado da busca

```js

    [
        {"name": "Project 1", "activity": "Activity 1 of the project: Project 1"},
        {"name": "Project 1", "activity": "Activity 2 of the project: Project 1"},
        {"name": "Project 1", "activity": "Activity 3 of the project: Project 1"},
        {"name": "Project 1", "activity": "Activity 4 of the project: Project 1"},
        {"name": "Project 2", "activity": "Activity 1 of the project: Project 2"},
        {"name": "Project 2", "activity": "Activity 2 of the project: Project 2"},
        {"name": "Project 2", "activity": "Activity 3 of the project: Project 2"},
        {"name": "Project 2", "activity": "Activity 4 of the project: Project 2"},
        {"name": "Project 2", "activity": "Activity 5 of the project: Project 2"},
        {"name": "Project 2", "activity": "Activity 6 of the project: Project 2"},
        {"name": "Project 2", "activity": "Activity 7 of the project: Project 2"},
        {"name": "Project 2", "activity": "Activity 8 of the project: Project 2"},
        {"name": "Project 2", "activity": "Activity 9 of the project: Project 2"},
        {"name": "Project 3", "activity": "Activity 1 of the project: Project 3"},
        {"name": "Project 3", "activity": "Activity 2 of the project: Project 3"},
        {"name": "Project 3", "activity": "Activity 3 of the project: Project 3"},
        {"name": "Project 4", "activity": "Activity 1 of the project: Project 4"},
        {"name": "Project 4", "activity": "Activity 2 of the project: Project 4"},
        {"name": "Project 4", "activity": "Activity 3 of the project: Project 4"},
        {"name": "Project 4", "activity": "Activity 4 of the project: Project 4"},
        {"name": "Project 4", "activity": "Activity 5 of the project: Project 4"},
        {"name": "Project 4", "activity": "Activity 6 of the project: Project 4"},
        {"name": "Project 4", "activity": "Activity 7 of the project: Project 4"},
        {"name": "Project 4", "activity": "Activity 8 of the project: Project 4"},
        {"name": "Project 4", "activity": "Activity 9 of the project: Project 4"},
        {"name": "Project 4", "activity": "Activity 10 of the project: Project 4"},
        {"name": "Project 4", "activity": "Activity 11 of the project: Project 4"},
        {"name": "Project 4", "activity": "Activity 12 of the project: Project 4"},
        {"name": "Project 4", "activity": "Activity 13 of the project: Project 4"}
    ]

```

#### 4. Liste todos os projetos que não possuam uma tag.
```js

    var query = { tags: { $not: { $in: [/MongoDB/i] } } }
    var projects = db.projects.find(query, { _id: 1, name: 1 });

```
##### Resultado da busca
```js

    {
        "_id": ObjectId("56b60a240c648b9c842d6ef1"),
        "name": "Project 4"
    }
    {
        "_id": ObjectId("56b60a240c648b9c842d6ef2"),
        "name": "Project 5"
    }
    Fetched 2 record(s) in 3ms

```

#### 5. Liste todos os usuários que não fazem parte do primeiro projeto cadastrado.

```js

    var members = [];
    var project = db.projects.findOne({ name: /Project 1/i }, { members: 1 });

    project.members.forEach(function(member) {
        members.push(member._id)
    });

    db.users.find({ _id : { $not : { $in : members } } }, { _id: 0, name: 1 });

```

##### Resultado da busca
```js

    { "name": "User 6"},
    { "name": "User 7"},
    { "name": "User 8"},
    { "name": "User 9"},
    { "name": "User 10"}
    Fetched 5 record(s) in 5ms

```

## Update - alteração

#### 1. Adicione para todos os projetos o campo `views: 0`.

```js

    var query = {};
    var mod = { $set: { views: 0 } };

    db.projects.update(query, mod, { multi: true });

```
##### Resultado da alteração
```js

    Updated 5 existing record(s) in 10ms
    WriteResult({
        "nMatched": 5,
        "nUpserted": 0,
        "nModified": 5
    });

```

#### 2. Adicione 1 tag diferente para cada projeto.

```js

    var projects = db.projects.find({}, { _id: 1, name: 1, tags: 1 });
    projects.forEach(function(project) {
        db.projects.update({ _id: project._id }, { $push: { tags: 'Tag ' + randomNumber(10) } });
    });

```

##### Resultado da alteração
```js

    Updated 1 existing record(s) in 13ms
    Updated 1 existing record(s) in 1ms
    Updated 1 existing record(s) in 1ms
    Updated 1 existing record(s) in 3ms
    Updated 1 existing record(s) in 1ms

```

#### 3. Adicione 2 membros diferentes para cada projeto.
```js

    function buildMemberObject(member) {
        return {
            _id: member._id,
            type: ['Admin'],
            notify: false
        };
    }

    var projects = db.projects.find({}, { name: 1, members: 1 }).toArray();

    projects.forEach(function(project) {
        var members = [];
        var newMembers = [];

        project.members.forEach(function(member) {
            members.push(member._id);
        });

        // Busca apenas os usuários que não fazem parte deste projeto
        newMembers = db.users.aggregate([
            { $project : { _id: 1 } },
            { $match: { _id : { $not : { $in : members } } } },
            { $sample: { size: 2 } }
        ]).result.map(buildMemberObject); // Constrói o objeto que será armazenado na coleção

        // Salva na coleção os novos membros.
        db.projects.update({ _id: project._id }, { $pushAll: { members: newMembers } });
    });

```

##### Resultado da alteração
```js

    Updated 1 existing record(s) in 1ms
    Updated 1 existing record(s) in 2ms
    Updated 1 existing record(s) in 1ms
    Updated 1 existing record(s) in 1ms
    Updated 1 existing record(s) in 1ms

```

#### 4. Adicione 1 comentário em cada atividade, deixe apenas 1 projeto sem.

```js

    var ids = [];
    var comments = [];

    db.activities.find({}, { _id: 1 }).forEach(function(activity, index) {

        // Armazena no array para facilitar o update
        ids.push(activity._id);

        var members = db.users.aggregate([
            { $project : { _id: 1 } },
            { $sample: { size: randomNumber(3) } }
        ]).result.map(buildMemberObject); // Constrói o objeto que será armazenado na coleção

        // ObjectId que será salvo na coleção
        var _id = new ObjectId();

        var comment = {
            _id: _id,
            text: 'Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua.',
            comment_date: new Date().toJSON(),
            members: members,
            files: [{
                name: 'Name of file for activity: ' + index,
                path: 'http://pathtostorage.com/activities/' + activity._id + '/comments/' + _id + '/' + randomNumber(1000) + '.json',
                size: randomNumber(1000)
            }]
        };

        db.activities.update({ _id: activity._id }, { $push: { comments: comment } });
    });

```

##### Resultado da alteração
```js

    Updated 1 existing record(s) in 1ms
    Updated 1 existing record(s) in 1ms
    Updated 1 existing record(s) in 2ms
    Updated 1 existing record(s) in 1ms
    Updated 1 existing record(s) in 2ms
    Updated 1 existing record(s) in 3ms
    Updated 1 existing record(s) in 3ms
    Updated 1 existing record(s) in 1ms
    Updated 1 existing record(s) in 1ms
    Updated 1 existing record(s) in 2ms
    Updated 1 existing record(s) in 3ms
    Updated 1 existing record(s) in 1ms
    Updated 1 existing record(s) in 2ms
    Updated 1 existing record(s) in 2ms
    Updated 1 existing record(s) in 2ms
    Updated 1 existing record(s) in 3ms
    Updated 1 existing record(s) in 2ms
    Updated 1 existing record(s) in 2ms
    Updated 1 existing record(s) in 2ms
    Updated 1 existing record(s) in 2ms
    Updated 1 existing record(s) in 2ms
    Updated 1 existing record(s) in 2ms
    Updated 1 existing record(s) in 1ms
    Updated 1 existing record(s) in 1ms
    Updated 1 existing record(s) in 4ms
    Updated 1 existing record(s) in 1ms
    Updated 1 existing record(s) in 2ms
    Updated 1 existing record(s) in 3ms
    Updated 1 existing record(s) in 2ms
    Updated 1 existing record(s) in 4ms
    Updated 1 existing record(s) in 1ms
    Updated 1 existing record(s) in 2ms
    Updated 1 existing record(s) in 2ms
    Updated 1 existing record(s) in 1ms
    Updated 1 existing record(s) in 1ms
    Updated 1 existing record(s) in 2ms
    Updated 1 existing record(s) in 1ms

```

#### 5. Adicione 1 projeto inteiro com **UPSERT**.

```js

    var project = {
        name: 'Project 6',
        tags: ['Laravel', 'VueJS', 'ElasticSearch'],
        goals: 3,
        activities: 2
    };

    var query = { name: /Project 6/i };
    var options = { upsert: true };
    var mod = {
        $set: { views: 0 },
        $setOnInsert: {
            name: project.name,
            description: 'Description of project: ' + project.name,
            begin_date: new Date().toJSON(),
            dream_date: new Date().toJSON(),
            end_date: new Date().toJSON(),
            visible: true,
            realocate: false,
            expired: new Date().toJSON(),
            visualizable_mod: true,
            tags: project.tags,
            members: generateMembers(),
            goals: generateGoals(project)
        }
    }

    db.projects.update(query, mod, options);

```

##### Resultado da alteração
```js

    Updated 1 new record(s) in 2ms
    WriteResult({
        "nMatched": 0,
        "nUpserted": 1,
        "nModified": 0,
        "_id": ObjectId("56b83e47cdf243f8789f4ccf")
    })

```

## Delete - remoção

##### Função criada somente para esta sessão
```js

    function getActivities(query) {

        if(!typeof query === 'object') return [];

        var ids = db.projects.aggregate([
            { $match: query },
            { $unwind: "$goals" },
            { $unwind: "$goals.activities" },
            { $group: {
                _id: null,
                activities: { $push: "$goals.activities.activity_id" }
            } }
        ]);

        if(ids.result.length > 0) {
            return ids.result[0].activities
        }

        return [];

    }

```

#### 1. Apague todos os projetos que não possuam *tags*.

```js

    var query = { tags: { $size: 0 } };

    var activities = getActivities(query);

    // Deleta as atividades correspondentes
    db.activities.remove({ _id: { $in: activities } });

    // Deleta os projetos correspondentes
    db.projects.remove(query);

```

##### Resultado da remoção
```js

    Removed 0 record(s) in 2ms
    WriteResult({
        "nRemoved": 0
    })

    Removed 0 record(s) in 3ms
    WriteResult({
        "nRemoved": 0
    })

```

#### 2. Apague todos os projetos que não possuam comentários nas atividades.

```js

    var query = {
        $or: [{
            comments: { $exists: 0 },
            comments: { $eq: [ ] }
        }]
    };

    var ids = [];
    db.activities.find(query, { _id: 1 }).forEach(function(activity) {
        ids.push(activity._id);
    });

    // Deleta as atividades correspondentes
    db.activities.remove({ _id: { $in: ids } });

    // Deleta a coleção correspondente as atividades
    db.projects.remove({ "goals.activities.activity_id": { $in: ids } });

```
##### Resultado da remoção
```js

    Removed 2 record(s) in 1ms
    WriteResult({ "nRemoved": 2 })

    Removed 1 record(s) in 4ms
    WriteResult({ "nRemoved": 1 })

```

#### 3. Apague todos os projetos que não possuam atividades.

```js

    var query = {
        $or: [{
            "goals.activities": { $exists: 0 },
            "goals.activities": { $eq: [ ] }
        }]
    };

    // Deleta a coleção
    db.projects.remove(query);

```
##### Resultado da remoção
```js

    Removed 1 record(s) in 2ms
    WriteResult({
        "nRemoved": 1
    })

```

#### 4. Escolha 2 usuário e apague todos os projetos em que os 2 fazem parte.

```js

    var query = {
        $or: [
            { name: /User 2/i },
            { name: /User 3/i }
        ]
    };

    var members = [];
    db.users.find(query, { _id: 1 }).forEach(function(member) {
        members.push(member._id);
    });

    var query = { 'members._id': { $in: members } };

    var activities = getActivities(query);

    // Deleta as atividades correspondentes
    db.activities.remove({ _id: { $in: activities } });

    // Deleta os projetos correspondentes
    db.projects.remove(query);

```
##### Resultado da remoção
```js

    Removed 0 record(s) in 0ms
    WriteResult({
        "nRemoved": 0
    })

    Removed 3 record(s) in 1ms
    WriteResult({
        "nRemoved": 0
    })

```

#### 5. Apague todos os projetos que possuam uma determinada *tag* em *goal*.

```js

    var query = {
        "goals.tags": {
            $in: [/Tag 1/i]
        }
    };

    var activities = getActivities(query);

    // Deleta as atividades correspondentes
    db.activities.remove({ _id: { $in: activities } });

    // Deleta os projetos correspondentes
    db.projects.remove(query, mod);

```
##### Resultado da remoção
```js

    Removed 30 record(s) in 3ms
    WriteResult({
        "nRemoved": 30
    })

    Removed 3 record(s) in 1ms
    WriteResult({
        "nRemoved": 3
    })

```

## Gerenciamento de usuários
#### 1. Crie um usuário com permissões **APENAS** de Leitura.

```js

    use admin

    db.createUser({
        user: "callmefabioo",
        pwd: "123123",
        roles: [ "read" ]
    });

```

##### Resultado
```js

    Successfully added user: { "user": "callmefabioo", "roles": ["read" ] }

```

#### 2. Crie um usuário com permissões de Escrita e Leitura.

```js

    use admin

    db.createUser({
        user: "masterblaster",
        pwd: "321321",
        roles: [ "readWrite" ]
    });

```

##### Resultado
```js

    Successfully added user: { "user": "masterblaster", "roles": [ "readWrite" ] }

```

#### 3. Adicionar o papel `grantRolesToUser` e `revokeRole` para o usuário com Escrita e Leitura.

```js

    use admin

    db.createRole({
        role: "grantRolesToUser",
        privileges: [{
            resource: { db: "admin", collection: "" },
            actions: [ "grantRole" ]
        }],
        roles: []
    })

    db.createRole({
        role: "revokeRole",
        privileges: [{
            resource: { db: "admin", collection: "" },
            actions: [ "grantRole" ]
        }],
        roles: []
    })

    db.grantRolesToUser("masterblaster", [ "grantRolesToUser", "revokeRole" ]);

```

#### 4. Remover o papel `grantRolesToUser` para o usuário com Escrita e Leitura.

```js

    db.runCommand({
        revokeRolesFromUser: "masterblaster",
        roles: [ "grantRolesToUser" ]
    });

    // Resultado
    { "ok" : 1 }

```

#### 5. Listar todos os usuários com seus papéis e ações.

```js

    db.runCommand({
        usersInfo: [
            { user: "callmefabioo", db: "admin" },
            { user: "masterblaster", db: "admin" }
        ],
        showCredentials: true,
        showPrivileges: true
    });

```
##### Resultado
```js

    {
        "users": [
            {
                "_id": "admin.callmefabioo",
                "user": "callmefabioo",
                "db": "admin",
                "credentials": {
                    "SCRAM-SHA-1": {
                        "iterationCount": 10000,
                        "salt": "x8NoEklKFkByAQo/SwOOBg==",
                        "storedKey": "WzQyJn8HpuooqHpjDLkslkzSzCU=",
                        "serverKey": "u/hfD83dqC+x8wSsn8CalAk1rJU="
                    }
                },
                "roles": [
                    {
                        "role": "read",
                        "db": "admin"
                    }
                ],
                "inheritedRoles": [
                    {
                        "role": "read",
                        "db": "admin"
                    }
                ],
                "inheritedPrivileges": [
                    {
                        "resource": {
                            "db": "admin",
                            "collection": ""
                        },
                        "actions": [
                            "collStats",
                            "dbHash",
                            "dbStats",
                            "find",
                            "killCursors",
                            "listCollections",
                            "listIndexes",
                            "planCacheRead"
                        ]
                    },
                    {
                        "resource": {
                            "anyResource": true
                        },
                        "actions": [
                            "listCollections"
                        ]
                    },
                    {
                        "resource": {
                            "db": "admin",
                            "collection": "system.indexes"
                        },
                        "actions": [
                            "collStats",
                            "dbHash",
                            "dbStats",
                            "find",
                            "killCursors",
                            "listCollections",
                            "listIndexes",
                            "planCacheRead"
                        ]
                    },
                    {
                        "resource": {
                            "db": "admin",
                            "collection": "system.js"
                        },
                        "actions": [
                            "collStats",
                            "dbHash",
                            "dbStats",
                            "find",
                            "killCursors",
                            "listCollections",
                            "listIndexes",
                            "planCacheRead"
                        ]
                    },
                    {
                        "resource": {
                            "db": "admin",
                            "collection": "system.namespaces"
                        },
                        "actions": [
                            "collStats",
                            "dbHash",
                            "dbStats",
                            "find",
                            "killCursors",
                            "listCollections",
                            "listIndexes",
                            "planCacheRead"
                        ]
                    }
                ]
            },
            {
                "_id": "admin.masterblaster",
                "user": "masterblaster",
                "db": "admin",
                "credentials": {
                    "SCRAM-SHA-1": {
                        "iterationCount": 10000,
                        "salt": "ZK7y7uiJsP1hTsgazTFzAQ==",
                        "storedKey": "HnkKGnZnAnYSHx0t03+Vw+ktpi4=",
                        "serverKey": "yUtlDSDTjgm/AQSX0Twawqn4WP0="
                    }
                },
                "roles": [
                    {
                        "role": "readWrite",
                        "db": "admin"
                    },
                    {
                        "role": "revokeRole",
                        "db": "admin"
                    }
                ],
                "inheritedRoles": [
                    {
                        "role": "readWrite",
                        "db": "admin"
                    },
                    {
                        "role": "revokeRole",
                        "db": "admin"
                    }
                ],
                "inheritedPrivileges": [
                    {
                        "resource": {
                            "db": "admin",
                            "collection": ""
                        },
                        "actions": [
                            "collStats",
                            "convertToCapped",
                            "createCollection",
                            "createIndex",
                            "dbHash",
                            "dbStats",
                            "dropCollection",
                            "dropIndex",
                            "emptycapped",
                            "find",
                            "grantRole",
                            "insert",
                            "killCursors",
                            "listCollections",
                            "listIndexes",
                            "planCacheRead",
                            "remove",
                            "renameCollectionSameDB",
                            "update"
                        ]
                    },
                    {
                        "resource": {
                            "anyResource": true
                        },
                        "actions": [
                            "listCollections"
                        ]
                    },
                    {
                        "resource": {
                            "db": "admin",
                            "collection": "system.indexes"
                        },
                        "actions": [
                            "collStats",
                            "dbHash",
                            "dbStats",
                            "find",
                            "killCursors",
                            "listCollections",
                            "listIndexes",
                            "planCacheRead"
                        ]
                    },
                    {
                        "resource": {
                            "db": "admin",
                            "collection": "system.js"
                        },
                        "actions": [
                            "collStats",
                            "convertToCapped",
                            "createCollection",
                            "createIndex",
                            "dbHash",
                            "dbStats",
                            "dropCollection",
                            "dropIndex",
                            "emptycapped",
                            "find",
                            "insert",
                            "killCursors",
                            "listCollections",
                            "listIndexes",
                            "planCacheRead",
                            "remove",
                            "renameCollectionSameDB",
                            "update"
                        ]
                    },
                    {
                        "resource": {
                            "db": "admin",
                            "collection": "system.namespaces"
                        },
                        "actions": [
                            "collStats",
                            "dbHash",
                            "dbStats",
                            "find",
                            "killCursors",
                            "listCollections",
                            "listIndexes",
                            "planCacheRead"
                        ]
                    }
                ]
            }
        ],
        "ok": 1
    }

```

## Sharding
### 1 Config Server
##### Criação do ***Config Server*** setando a porta `27010`

```js

    Fabio@Fabio MINGW64 /c
    $ mkdir data/configdb
    $ mongod --configsvr --port 27010
    2016-02-10T23:25:46.181-0200 I CONTROL  [initandlisten] MongoDB starting : pid=1316 port=27010 dbpath=C:\data\configdb\ master=1 64-bit host=Fabio
    2016-02-10T23:25:46.183-0200 I CONTROL  [initandlisten] targetMinOS: Windows 7/Windows Server 2008 R2
    2016-02-10T23:25:46.183-0200 I CONTROL  [initandlisten] db version v3.2.1
    2016-02-10T23:25:46.183-0200 I CONTROL  [initandlisten] git version: a14d55980c2cdc565d4704a7e3ad37e4e535c1b2
    2016-02-10T23:25:46.183-0200 I CONTROL  [initandlisten] OpenSSL version: OpenSSL 1.0.1p-fips 9 Jul 2015
    2016-02-10T23:25:46.183-0200 I CONTROL  [initandlisten] allocator: tcmalloc
    2016-02-10T23:25:46.183-0200 I CONTROL  [initandlisten] modules: none
    2016-02-10T23:25:46.183-0200 I CONTROL  [initandlisten] build environment:
    2016-02-10T23:25:46.183-0200 I CONTROL  [initandlisten]     distmod: 2008plus-ssl
    2016-02-10T23:25:46.183-0200 I CONTROL  [initandlisten]     distarch: x86_64
    2016-02-10T23:25:46.183-0200 I CONTROL  [initandlisten]     target_arch: x86_64
    2016-02-10T23:25:46.183-0200 I CONTROL  [initandlisten] options: { net: { port: 27010 }, sharding: { clusterRole: "configsvr" } }
    2016-02-10T23:25:46.185-0200 I STORAGE  [initandlisten] wiredtiger_open config: create,cache_size=1G,session_max=20000,eviction=(threads_max=4),config_base=false,statistics=(fast),log=(enabled=true,archive=true,path=journal,compressor=snappy),file_manager=(close_idle_time=100000),checkpoint=(wait=60,log_size=2GB),statistics_log=(wait=0),
    2016-02-10T23:25:46.243-0200 I REPL     [initandlisten] ******
    2016-02-10T23:25:46.244-0200 I REPL     [initandlisten] creating replication oplog of size: 5MB...
    2016-02-10T23:25:46.247-0200 I STORAGE  [initandlisten] Starting WiredTigerRecordStoreThread local.oplog.$main 2016-02-10T23:25:46.247-0200 I STORAGE  [initandlisten] The size storer reports that the oplog contains 0 records totaling to 0 bytes
    2016-02-10T23:25:46.248-0200 I STORAGE  [initandlisten] Scanning the oplog to determine where to place markers for truncation
    2016-02-10T23:25:46.259-0200 I REPL     [initandlisten] ******
    2016-02-10T23:25:46.261-0200 I NETWORK  [HostnameCanonicalizationWorker] Starting hostname canonicalization worker
    2016-02-10T23:25:46.261-0200 I FTDC     [initandlisten] Initializing full-time diagnostic data capture with directory 'C:/data/configdb/diagnostic.data'
    2016-02-10T23:25:46.271-0200 I NETWORK  [initandlisten] waiting for connections on port 27010

```

### 1 Router
##### Crio o ***Router*** utilizando o `mongos`, setando o ***Config Server*** que ele acessará para ter as informações dos ***Shards***

```js

    Fabio@Fabio MINGW64 /c
    $ mongos --configdb localhost:27010 --port 27011
    2016-02-10T23:30:20.360-0200 W SHARDING [main] Running a sharded cluster with fewer than 3 config servers should only be done for testing purposes and is not recommended for production.
    2016-02-10T23:30:20.624-0200 I SHARDING [mongosMain] MongoS version 3.2.1 starting: pid=3380 port=27011 64-bit host=Fabio (--help for usage)
    2016-02-10T23:30:20.624-0200 I CONTROL  [mongosMain] db version v3.2.1
    2016-02-10T23:30:20.624-0200 I CONTROL  [mongosMain] git version: a14d55980c2cdc565d4704a7e3ad37e4e535c1b2
    2016-02-10T23:30:20.625-0200 I CONTROL  [mongosMain] OpenSSL version: OpenSSL 1.0.1p-fips 9 Jul 2015
    2016-02-10T23:30:20.625-0200 I CONTROL  [mongosMain] allocator: tcmalloc
    2016-02-10T23:30:20.625-0200 I CONTROL  [mongosMain] modules: none
    2016-02-10T23:30:20.626-0200 I CONTROL  [mongosMain] build environment:
    2016-02-10T23:30:20.626-0200 I CONTROL  [mongosMain]     distmod: 2008plus-ssl
    2016-02-10T23:30:20.626-0200 I CONTROL  [mongosMain]     distarch: x86_64
    2016-02-10T23:30:20.626-0200 I CONTROL  [mongosMain]     target_arch: x86_64
    2016-02-10T23:30:20.626-0200 I CONTROL  [mongosMain] options: { net: { port: 27011 }, sharding: { configDB: "localhost:27010" } }
    2016-02-10T23:30:20.627-0200 I SHARDING [mongosMain] Updating config server connection string to: localhost:27010
    2016-02-10T23:30:20.638-0200 I NETWORK  [HostnameCanonicalizationWorker] Starting hostname canonicalization worker
    2016-02-10T23:30:20.638-0200 I SHARDING [Balancer] about to contact config servers and shards
    2016-02-10T23:30:20.639-0200 I SHARDING [Balancer] config servers and shards contacted successfully
    2016-02-10T23:30:20.640-0200 I SHARDING [Balancer] balancer id: Fabio:27011 started
    2016-02-10T23:30:20.641-0200 I NETWORK  [mongosMain] waiting for connections on port 27011
    2016-02-10T23:30:20.650-0200 I SHARDING [LockPinger] creating distributed lock ping thread for localhost:27010 and process Fabio:27011:1455154220:41 (sleeping for 30000ms)
    2016-02-10T23:30:20.653-0200 I SHARDING [LockPinger] cluster localhost:27010 pinged successfully at 2016-02-10T23:30:20.652-0200 by distributed lock pinger 'localhost:27010/Fabio:27011:1455154220:41', sleeping for 30000ms 2016-02-10T23:30:20.654-0200 I SHARDING [Balancer] distributed lock 'balancer/Fabio:27011:1455154220:41' acquired for 'doing balance round', ts : 56bbe42cfe01f4e9643ef1e2
    2016-02-10T23:30:20.655-0200 I SHARDING [Balancer] about to log metadata event into actionlog: { _id: "Fabio-2016-02-10T23:30:20.655-0200-56bbe42cfe01f4e9643ef1e3", server: "Fabio", clientAddr: "", time: new Date(1455154220655), what: "balancer.round", ns: "", details: { executionTimeMillis: 13, errorOccured: false, candidateChunks: 0, chunksMoved: 0 } }
    2016-02-10T23:30:20.659-0200 I SHARDING [Balancer] distributed lock 'balancer/Fabio:27011:1455154220:41' unlocked.

```

### 3 Shardings

```js

    Fabio@Fabio MINGW64 /c
    $ mkdir data/shard1 && mkdir data/shard2 && mkdir data/shard3

    Fabio@Fabio MINGW64 /c
    $ mongod --port 27012 --dbpath data/shard1
    2016-02-10T23:33:47.998-0200 I CONTROL  [initandlisten] MongoDB starting : pid=6616 port=27012 dbpath=data/shard1 64-bit host=Fabio
    2016-02-10T23:33:48.000-0200 I CONTROL  [initandlisten] targetMinOS: Windows 7/Windows Server 2008 R2
    2016-02-10T23:33:48.000-0200 I CONTROL  [initandlisten] db version v3.2.1
    2016-02-10T23:33:48.000-0200 I CONTROL  [initandlisten] git version: a14d55980c2cdc565d4704a7e3ad37e4e535c1b2
    2016-02-10T23:33:48.000-0200 I CONTROL  [initandlisten] OpenSSL version: OpenSSL 1.0.1p-fips 9 Jul 2015
    2016-02-10T23:33:48.001-0200 I CONTROL  [initandlisten] allocator: tcmalloc
    2016-02-10T23:33:48.001-0200 I CONTROL  [initandlisten] modules: none
    2016-02-10T23:33:48.001-0200 I CONTROL  [initandlisten] build environment:
    2016-02-10T23:33:48.002-0200 I CONTROL  [initandlisten]     distmod: 2008plus-ssl
    2016-02-10T23:33:48.002-0200 I CONTROL  [initandlisten]     distarch: x86_64
    2016-02-10T23:33:48.003-0200 I CONTROL  [initandlisten]     target_arch: x86_64
    2016-02-10T23:33:48.003-0200 I CONTROL  [initandlisten] options: { net: { port: 27012 }, storage: { dbPath: "data/shard1" } }
    2016-02-10T23:33:48.005-0200 I STORAGE  [initandlisten] wiredtiger_open config: create,cache_size=1G,session_max=20000,eviction=(threads_max=4),config_base=false,statistics=(fast),log=(enabled=true,archive=true,path=journal,compressor=snappy),file_manager=(close_idle_time=100000),checkpoint=(wait=60,log_size=2GB),statistics_log=(wait=0),
    2016-02-10T23:33:48.058-0200 I NETWORK  [HostnameCanonicalizationWorker] Starting hostname canonicalization worker
    2016-02-10T23:33:48.058-0200 I FTDC     [initandlisten] Initializing full-time diagnostic data capture with directory 'data/shard1/diagnostic.data'
    2016-02-10T23:33:48.066-0200 I NETWORK  [initandlisten] waiting for connections on port 27012

```

```js

    Fabio@Fabio MINGW64 /c
    $ mongod --port 27013 --dbpath data/shard2
    2016-02-10T23:34:24.452-0200 I CONTROL  [initandlisten] MongoDB starting : pid=6540 port=27013 dbpath=data/shard2 64-bit host=Fabio
    2016-02-10T23:34:24.454-0200 I CONTROL  [initandlisten] targetMinOS: Windows 7/Windows Server 2008 R2
    2016-02-10T23:34:24.454-0200 I CONTROL  [initandlisten] db version v3.2.1
    2016-02-10T23:34:24.454-0200 I CONTROL  [initandlisten] git version: a14d55980c2cdc565d4704a7e3ad37e4e535c1b2
    2016-02-10T23:34:24.454-0200 I CONTROL  [initandlisten] OpenSSL version: OpenSSL 1.0.1p-fips 9 Jul 2015
    2016-02-10T23:34:24.454-0200 I CONTROL  [initandlisten] allocator: tcmalloc
    2016-02-10T23:34:24.454-0200 I CONTROL  [initandlisten] modules: none
    2016-02-10T23:34:24.454-0200 I CONTROL  [initandlisten] build environment:
    2016-02-10T23:34:24.454-0200 I CONTROL  [initandlisten]     distmod: 2008plus-ssl
    2016-02-10T23:34:24.454-0200 I CONTROL  [initandlisten]     distarch: x86_64
    2016-02-10T23:34:24.454-0200 I CONTROL  [initandlisten]     target_arch: x86_64
    2016-02-10T23:34:24.454-0200 I CONTROL  [initandlisten] options: { net: { port: 27013 }, storage: { dbPath: "data/shard2" } }
    2016-02-10T23:34:24.455-0200 I STORAGE  [initandlisten] wiredtiger_open config: create,cache_size=1G,session_max=20000,eviction=(threads_max=4),config_base=false,statistics=(fast),log=(enabled=true,archive=true,path=journal,compressor=snappy),file_manager=(close_idle_time=100000),checkpoint=(wait=60,log_size=2GB),statistics_log=(wait=0),
    2016-02-10T23:34:24.522-0200 I NETWORK  [HostnameCanonicalizationWorker] Starting hostname canonicalization worker
    2016-02-10T23:34:24.522-0200 I FTDC     [initandlisten] Initializing full-time diagnostic data capture with directory 'data/shard2/diagnostic.data'
    2016-02-10T23:34:24.530-0200 I NETWORK  [initandlisten] waiting for connections on port 27013
```

```js

    Fabio@Fabio MINGW64 /c
    $ mongod --port 27014 --dbpath data/shard3
    2016-02-10T23:34:48.084-0200 I CONTROL  [initandlisten] MongoDB starting : pid=3372 port=27014 dbpath=data/shard3 64-bit host=Fabio
    2016-02-10T23:34:48.086-0200 I CONTROL  [initandlisten] targetMinOS: Windows 7/Windows Server 2008 R2
    2016-02-10T23:34:48.086-0200 I CONTROL  [initandlisten] db version v3.2.1
    2016-02-10T23:34:48.086-0200 I CONTROL  [initandlisten] git version: a14d55980c2cdc565d4704a7e3ad37e4e535c1b2
    2016-02-10T23:34:48.086-0200 I CONTROL  [initandlisten] OpenSSL version: OpenSSL 1.0.1p-fips 9 Jul 2015
    2016-02-10T23:34:48.086-0200 I CONTROL  [initandlisten] allocator: tcmalloc
    2016-02-10T23:34:48.086-0200 I CONTROL  [initandlisten] modules: none
    2016-02-10T23:34:48.086-0200 I CONTROL  [initandlisten] build environment:
    2016-02-10T23:34:48.086-0200 I CONTROL  [initandlisten]     distmod: 2008plus-ssl
    2016-02-10T23:34:48.086-0200 I CONTROL  [initandlisten]     distarch: x86_64
    2016-02-10T23:34:48.087-0200 I CONTROL  [initandlisten]     target_arch: x86_64
    2016-02-10T23:34:48.087-0200 I CONTROL  [initandlisten] options: { net: { port: 27014 }, storage: { dbPath: "data/shard3" } }
    2016-02-10T23:34:48.088-0200 I STORAGE  [initandlisten] wiredtiger_open config: create,cache_size=1G,session_max=20000,eviction=(threads_max=4),config_base=false,statistics=(fast),log=(enabled=true,archive=true,path=journal,compressor=snappy),file_manager=(close_idle_time=100000),checkpoint=(wait=60,log_size=2GB),statistics_log=(wait=0),
    2016-02-10T23:34:48.137-0200 I NETWORK  [HostnameCanonicalizationWorker] Starting hostname canonicalization worker
    2016-02-10T23:34:48.138-0200 I FTDC     [initandlisten] Initializing full-time diagnostic data capture with directory 'data/shard3/diagnostic.data'
    2016-02-10T23:34:48.144-0200 I NETWORK  [initandlisten] waiting for connections on port 27014

```

##### Conectar no ***Router*** e registrar as ***Shards***

```js

    Fabio@Fabio MINGW64 /c
    $ mongo --port 27011 --host localhost

    sh.addShard("localhost:27012")
    // Resultado
    { "shardAdded": "shard0000", "ok": 1 }

    sh.addShard("localhost:27013")
    // Resultado
    { "shardAdded": "shard0001", "ok": 1 }

    sh.addShard("localhost:27014")
    // Resultado
    { "shardAdded": "shard0002", "ok": 1 }

```

##### Especificar qual ***database*** ***shardear***

```js

    Fabio@Fabio MINGW64 /c
    $ mongo --port 27011 --host localhost
    sh.enableSharding("be-mean")
    // Resultado
    { "ok": 1 }

```

##### Especificar qual ***coleção*** do ***database*** será ***shardeada***

```js

    Fabio@Fabio MINGW64 /c
    $ mongo --port 27011 --host localhost
    sh.shardCollection("be-mean.projects", { "_id": 1 })
    // Resultado
    { "collectionsharded": "be-mean.projects", "ok": 1 }

```

##### Envio de dados fake para o ***Router***

```js

    Fabio@Fabio MINGW64 /c
    $ mongo --port 27011 --host localhost

    var usersId = db.users.find({}, {_id: 1}).toArray();

    var projects = [];
    var projectsToInsert = [];

    for ( var i = 1; i < 10000; i++ ) {
        projects.push({
            name: 'Project ' + i,
            tags: ['Tag ' + i, 'Tag ' + (i + 1), 'Tag ' + (i + 2)]
        });
    }

    projects.forEach(function(project, index) {
        projectsToInsert.push({
            name: project.name,
            description: 'Description of project: ' + project.name,
            begin_date: new Date().toJSON(),
            dream_date: new Date().toJSON(),
            end_date: new Date().toJSON(),
            visible: true,
            realocate: false,
            expired: new Date().toJSON(),
            visualizable_mod: true,
            tags: project.tags,
            members: [],
            goals: []
        });
    });

    // Salva as coleções
    db.projects.insert(projectsToInsert);

```

## Réplica
###  3 Réplicas

```js

    Fabio@Fabio MINGW64 /c
    $ mkdir data/rs1 && mkdir data/rs2 && mkdir data/rs3

    Fabio@Fabio MINGW64 /c
    $ mongod --replSet replica_set --port 27017 --dbpath data/rs1
    2016-02-11T00:43:50.249-0200 I CONTROL  [initandlisten] MongoDB starting : pid=860 port=27017 dbpath=data/rs1 64-bit host=Fabio
    2016-02-11T00:43:50.253-0200 I CONTROL  [initandlisten] targetMinOS: Windows 7/Windows Server 2008 R2
    2016-02-11T00:43:50.253-0200 I CONTROL  [initandlisten] db version v3.2.1
    2016-02-11T00:43:50.254-0200 I CONTROL  [initandlisten] git version: a14d55980c2cdc565d4704a7e3ad37e4e535c1b2
    2016-02-11T00:43:50.254-0200 I CONTROL  [initandlisten] OpenSSL version: OpenSSL 1.0.1p-fips 9 Jul 2015
    2016-02-11T00:43:50.254-0200 I CONTROL  [initandlisten] allocator: tcmalloc
    2016-02-11T00:43:50.254-0200 I CONTROL  [initandlisten] modules: none
    2016-02-11T00:43:50.254-0200 I CONTROL  [initandlisten] build environment:
    2016-02-11T00:43:50.254-0200 I CONTROL  [initandlisten]     distmod: 2008plus-ssl
    2016-02-11T00:43:50.254-0200 I CONTROL  [initandlisten]     distarch: x86_64
    2016-02-11T00:43:50.254-0200 I CONTROL  [initandlisten]     target_arch: x86_64
    2016-02-11T00:43:50.254-0200 I CONTROL  [initandlisten] options: { net: { port: 27017 }, replication: { replSet: "replica_set" }, storage: { dbPath: "data/rs1" } }
    2016-02-11T00:43:50.258-0200 I STORAGE  [initandlisten] wiredtiger_open config: create,cache_size=1G,session_max=20000,eviction=(threads_max=4),config_base=false,statistics=(fast),log=(enabled=true,archive=true,path=journal,compressor=snappy),file_manager=(close_idle_time=100000),checkpoint=(wait=60,log_size=2GB),statistics_log=(wait=0),
    2016-02-11T00:43:50.339-0200 I REPL     [initandlisten] Did not find local voted for document at startup;  NoMatchingDocument Did not find replica set lastVote document in local.replset.election
    2016-02-11T00:43:50.339-0200 I REPL     [initandlisten] Did not find local replica set configuration document at startup;  NoMatchingDocument Did not find replica set configuration document in local.system.replset
    2016-02-11T00:43:50.340-0200 I NETWORK  [HostnameCanonicalizationWorker] Starting hostname canonicalization worker
    2016-02-11T00:43:50.340-0200 I FTDC     [initandlisten] Initializing full-time diagnostic data capture with directory 'data/rs1/diagnostic.data'
    2016-02-11T00:43:50.349-0200 I NETWORK  [initandlisten] waiting for connections on port 27017

```

```js

    Fabio@Fabio MINGW64 /c
    $ mongod --replSet replica_set --port 27018 --dbpath data/rs2
    2016-02-11T00:45:13.699-0200 I CONTROL  [initandlisten] MongoDB starting : pid=4180 port=27018 dbpath=data/rs2 64-bit host=Fabio
    2016-02-11T00:45:13.701-0200 I CONTROL  [initandlisten] targetMinOS: Windows 7/Windows Server 2008 R2
    2016-02-11T00:45:13.701-0200 I CONTROL  [initandlisten] db version v3.2.1
    2016-02-11T00:45:13.701-0200 I CONTROL  [initandlisten] git version: a14d55980c2cdc565d4704a7e3ad37e4e535c1b2
    2016-02-11T00:45:13.701-0200 I CONTROL  [initandlisten] OpenSSL version: OpenSSL 1.0.1p-fips 9 Jul 2015
    2016-02-11T00:45:13.702-0200 I CONTROL  [initandlisten] allocator: tcmalloc
    2016-02-11T00:45:13.702-0200 I CONTROL  [initandlisten] modules: none
    2016-02-11T00:45:13.702-0200 I CONTROL  [initandlisten] build environment:
    2016-02-11T00:45:13.702-0200 I CONTROL  [initandlisten]     distmod: 2008plus-ssl
    2016-02-11T00:45:13.702-0200 I CONTROL  [initandlisten]     distarch: x86_64
    2016-02-11T00:45:13.702-0200 I CONTROL  [initandlisten]     target_arch: x86_64
    2016-02-11T00:45:13.702-0200 I CONTROL  [initandlisten] options: { net: { port: 27018 }, replication: { replSet: "replica_set" }, storage: { dbPath: "data/rs2" } }
    2016-02-11T00:45:13.706-0200 I STORAGE  [initandlisten] wiredtiger_open config: create,cache_size=1G,session_max=20000,eviction=(threads_max=4),config_base=false,statistics=(fast),log=(enabled=true,archive=true,path=journal,compressor=snappy),file_manager=(close_idle_time=100000),checkpoint=(wait=60,log_size=2GB),statistics_log=(wait=0),
    2016-02-11T00:45:13.787-0200 I REPL     [initandlisten] Did not find local voted for document at startup;  NoMatchingDocument Did not find replica set lastVote document in local.replset.election
    2016-02-11T00:45:13.788-0200 I REPL     [initandlisten] Did not find local replica set configuration document at startup;  NoMatchingDocument Did not find replica set configuration document in local.system.replset
    2016-02-11T00:45:13.788-0200 I NETWORK  [HostnameCanonicalizationWorker] Starting hostname canonicalization worker
    2016-02-11T00:45:13.788-0200 I FTDC     [initandlisten] Initializing full-time diagnostic data capture with directory 'data/rs2/diagnostic.data'
    2016-02-11T00:45:13.799-0200 I NETWORK  [initandlisten] waiting for connections on port 27018

```

```js

    Fabio@Fabio MINGW64 /c
    $ mongod --replSet replica_set --port 27019 --dbpath data/rs3
    2016-02-11T00:45:35.440-0200 I CONTROL  [initandlisten] MongoDB starting : pid=2336 port=27019 dbpath=data/rs3 64-bit host=Fabio
    2016-02-11T00:45:35.443-0200 I CONTROL  [initandlisten] targetMinOS: Windows 7/Windows Server 2008 R2
    2016-02-11T00:45:35.443-0200 I CONTROL  [initandlisten] db version v3.2.1
    2016-02-11T00:45:35.443-0200 I CONTROL  [initandlisten] git version: a14d55980c2cdc565d4704a7e3ad37e4e535c1b2
    2016-02-11T00:45:35.443-0200 I CONTROL  [initandlisten] OpenSSL version: OpenSSL 1.0.1p-fips 9 Jul 2015
    2016-02-11T00:45:35.443-0200 I CONTROL  [initandlisten] allocator: tcmalloc
    2016-02-11T00:45:35.443-0200 I CONTROL  [initandlisten] modules: none
    2016-02-11T00:45:35.444-0200 I CONTROL  [initandlisten] build environment:
    2016-02-11T00:45:35.444-0200 I CONTROL  [initandlisten]     distmod: 2008plus-ssl
    2016-02-11T00:45:35.444-0200 I CONTROL  [initandlisten]     distarch: x86_64
    2016-02-11T00:45:35.444-0200 I CONTROL  [initandlisten]     target_arch: x86_64
    2016-02-11T00:45:35.444-0200 I CONTROL  [initandlisten] options: { net: { port: 27019 }, replication: { replSet: "replica_set" }, storage: { dbPath: "data/rs3" } }
    2016-02-11T00:45:35.446-0200 I STORAGE  [initandlisten] wiredtiger_open config: create,cache_size=1G,session_max=20000,eviction=(threads_max=4),config_base=false,statistics=(fast),log=(enabled=true,archive=true,path=journal,compressor=snappy),file_manager=(close_idle_time=100000),checkpoint=(wait=60,log_size=2GB),statistics_log=(wait=0),
    2016-02-11T00:45:35.528-0200 I REPL     [initandlisten] Did not find local voted for document at startup;  NoMatchingDocument Did not find replica set lastVote document in local.replset.election
    2016-02-11T00:45:35.528-0200 I REPL     [initandlisten] Did not find local replica set configuration document at startup;  NoMatchingDocument Did not find replica set configuration document in local.system.replset
    2016-02-11T00:45:35.529-0200 I NETWORK  [HostnameCanonicalizationWorker] Starting hostname canonicalization worker
    2016-02-11T00:45:35.529-0200 I FTDC     [initandlisten] Initializing full-time diagnostic data capture with directory 'data/rs3/diagnostic.data'
    2016-02-11T00:45:35.538-0200 I NETWORK  [initandlisten] waiting for connections on port 27019

```

##### Iniciar a `replicaSet` no mongo --port 27017, que será o `PRIMARY`.

```js

    Fabio@Fabio MINGW64 /c
    $ mongo --port 27017
    var rsconf = {
        _id: "replica_set",
        members: [
            {
                _id: 0,
                host: "127.0.0.1:27017"
            }
        ]
    }

    $ rs.initiate(rsconf)
    // Resultado
    { "ok" : 1 }

```

##### Adicionar outras réplicas na `replicaSet` criada

```js

    Fabio(C:\mongodb\bin\mongod.exe-3.2.1)[SECONDARY] test> rs.add("127.0.0.1:27018")
    { "ok": 1 }
    Fabio(C:\mongodb\bin\mongod.exe-3.2.1)[PRIMARY] test> rs.add("127.0.0.1:27019")
    { "ok": 1 }

```

##### Conectar nas réplicas `SECUNDARY`

```js

    Fabio@Fabio MINGW64 /c
    $ mongo --port 27018
    MongoDB shell version: 3.2.1
    connecting to: 127.0.0.1:27018/test
    Mongo-Hacker 0.0.4
    Fabio:27018(C:\mongodb\bin\mongod.exe-3.2.1)[SECONDARY] test>

    Fabio@Fabio MINGW64 /c
    $ mongo --port 27019
    MongoDB shell version: 3.2.1
    connecting to: 127.0.0.1:27019/test
    Mongo-Hacker 0.0.4
    Fabio:27019(C:\mongodb\bin\mongod.exe-3.2.1)[SECONDARY] test>

```
