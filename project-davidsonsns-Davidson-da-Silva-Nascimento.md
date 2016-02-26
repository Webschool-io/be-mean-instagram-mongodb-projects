# MongoDb - Projeto Final
**Autor:** Davidson da Silva Nascimento
**Data:** 1451161276592

## Para qual sistema você usaria o MogoDB (diferente desse)?
- Chat
- Rede Social
- Fórum
- Logs
- ERP
- Fantasy Game (Ex. Cartola FC)
- E-commerce

Ou em qualquer outro sistemas robusto de grande escala e alta disponibilidade.

## Qual a modelagem da sua coleção de `users`?
```js
    {
        _id: ObjectId,
        name: <value>,
        bio: <value>,
        date_register: <value>,
        auth: {
            username: <value>,
            email: <value>,
            password: <value>,
            last_access: <value>,
            online: <value>,
            disabled: <value>,
            hash_token: <value>
        },
        avatar: <valueBinary>,
        backgound: <valueBinary>
    }
```

## Qual a modelagem da sua coleção de `projects`?
```js
    {
        _id: ObjectId,
        name: <value>,
        description: <value>,
        date_begin: <value>,
        date_dream: <value>,
        date_end: <value>,
        visible: <value>,
        realocate: <value>,
        expired: <value>,
        visualizable_mod: <value>,
        tags: [],
        members: [{
            member_id: <value>,
            type: [],
            notify: <value>
        }],
        goals: [{
            name: <value>,
            description: <value>,
            date_begin: <value>,
            date_dream: <value>,
            date_end: <value>,
            realocate: <value>,
            expired: <value>,
            tags: [],
            historic: [],
            activities: [
                { activity_id: ObjectId }
            ]
        }]
    }
```

## Qual a modelagem da sua coleção retirada de `projects`?
`activities` foi a coleção retirada de `projects`. Segue logo após a exibição de sua modelagem, uma breve explicação do porque desta escolha.
```js
    {
        _id: ObjectId,
        name: <value>,
        description: <value>,
        date_begin: <value>,
        date_dream: <value>,
        date_end: <value>,
        realocate: <value>,
        expired: <value>,
        tags: [],
        historic: [],
        members: [{
            member_id: <value>,
            type: [],
            notify: <value>
        }],
        comments: [{
            text: <value>,
            date_comment: <value>,
            members: [{
                member_id: <value>,
                type: [],
                notify: <value>
            }],
            files: [{
                name: <value>,
                size: <value>,
                data: <valueBinary>,
                type: <value>
            }]
        }]
    }
```

> Esta escolha foi realizada devido a **modelagem** ter sido arquitetada pensando fielmente conforme a atender da melhor forma a **aplicação**, refletindo de forma positiva no resultado.

> **Segue** uma breve intrudução de como esta aplicação funcionaria:

> Acreditando-se que o projeto já foi selecionado dentre os existentes, serão listadas os `goals`(objetivos) do mesmo. Após selecionar um `goal` serão listadas as `activities`(atividade) correspondentes juntamente com os `comments` (comentários), podendo haver neste ponto a postagem e leitura dos mesmos. 

> Pode ser notado que nenhuma informação será consultada de forma desnecessária.

## Create - cadastro

#### 1. Cadastre 10 usuários diferentes.
```js
    var usersHackaton = [
      {name: "Davidson Silva", username: "dnascimento"},
      {name: "Diego Araújo", username: "daraujo "},
      {name: "Erni Augusto", username: "eaugusto"},
      {name: "Wellington Fernandes", username: "wfernandes"},
      {name: "Pablo Dinella", username: "pdinella"},
      {name: "Felipe Marchant", username: "fmarchant"},
      {name: "Samuel Verneck", username: "sverneck"},
      {name: "Diego Ferreira", username: "dferreira"},
      {name: "Jean Nascimento", username: "jnascimento"},
      {name: "Willian Bruno", username: "wbruno"}
    ]
    var usersInsert = []
    
    // cria array para com todos dados de inserção de users
    usersHackaton.forEach(function(user){
        var query = {
            name: user.name,
            bio: "Vai q é tuma lek",
            date_register: new Date(),
            auth: {
                username: user.username,
                email: user.username+"@"+user.username+".com.br",
                password: user.username.split("").reverse().join(""),
                last_access: new Date(),
                online: (_rand() * 10 % 2) > 1 ? true : false,
                disabled: (_rand() * 10 % 2) > 1 ? true : false,
                hash_token: "d41d8cd98f00b204e9800998ecf8427e"
            },
            avatar: null,
            backgound: null
        }
        usersInsert.push(query)
    })
    db.users.insert(usersInsert)
    
    BulkWriteResult({
        "writeErrors" : [ ],
        "writeConcernErrors" : [ ],
        "nInserted" : 10,
        "nUpserted" : 0,
        "nMatched" : 0,
        "nModified" : 0,
        "nRemoved" : 0,
        "upserted" : [ ]
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
    // Realiza inserção das activities
    function insertActivity(nameProject) {

        // id a ser inserido na atividade e na coleção projects
        var id = new ObjectId()

        db.activities.insert({
            _id: id,
            name: "Name Activite " + nameProject,
            description: "Description " + nameProject,
            date_begin: new Date(),
            date_dream: new Date(),
            date_end: new Date(),
            realocate: false,
            expired: true,
            tags: [nameProject],
            historic: [],
            members: [],
            comments: []
        })

        return id
    }

    // ObjectId de todos users
    var usersID = db.users.find({}, {_id: 1}).toArray()

    // inserção direta dos projetos através de um único array contendo os documentos
    db.projects.insert([{
        name: "Primeiro Projeto",
        description: "consectetur adipiscing elit",
        date_begin: new Date(),
        date_dream: new Date(),
        date_end: new Date(),
        visible: true,
        realocate: false,
        expired: false,
        visualizable_mod: null,
        tags: ["BeMean", "Node.js", "MongoDB"],
        members: [{
            member_id: usersID[0]._id,
            type: ["admin"],
            notify: false
        }, {
            member_id: usersID[1]._id,
            type: ["view"],
            notify: true
        }, {
            member_id: usersID[2]._id,
            type: ["admin"],
            notify: false
        }, {
            member_id: usersID[3]._id,
            type: ["view"],
            notify: true
        }, {
            member_id: usersID[4]._id,
            type: ["contributor"],
            notify: false
        }],
        goals: [{
            name: "Ficar foda em JS",
            description: "Duis quis auctor magna",
            date_begin: new Date(),
            date_dream: new Date(),
            date_end: new Date(),
            realocate: false,
            expired: false,
            tags: ["JavaScript", "V8", "Closure"],
            historic: [],
            activities: [{
                activity_id: insertActivity("1 Primeiro Projeto")
            }, {
                activity_id: insertActivity("2 Primeiro Projeto")
            }]
        }]
    }, {
        name: "Segundo Projeto",
        description: "Vivamus et rutrum sem, quis fringilla mi. In libero mi",
        date_begin: new Date(),
        date_dream: new Date(),
        date_end: new Date(),
        visible: false,
        realocate: true,
        expired: false,
        visualizable_mod: true,
        tags: ["BeMean", "Node.js", "AngularJS"],
        members: [{
            member_id: usersID[1]._id,
            type: ["admin"],
            notify: false
        }, {
            member_id: usersID[2]._id,
            type: ["contributor"],
            notify: true
        }, {
            member_id: usersID[3]._id,
            type: ["view"],
            notify: false
        }, {
            member_id: usersID[4]._id,
            type: ["contributor"],
            notify: true
        }, {
            member_id: usersID[5]._id,
            type: ["contributor"],
            notify: false
        }],
        goals: [{
            name: "Dobrar a meta",
            description: "Nullam ac rhoncus",
            date_begin: new Date(),
            date_dream: new Date(),
            date_end: new Date(),
            realocate: false,
            expired: true,
            tags: ["JavaScript", "Variable", "Global"],
            historic: [],
            activities: [{
                activity_id: insertActivity("1 Segundo Projeto")
            }, {
                activity_id: insertActivity("2 Segundo Projeto")
            }]
        }]
    }, {
        name: "Terceiro projeto",
        description: "suscipit scelerisque viverra ac",
        date_begin: new Date(),
        date_dream: new Date(),
        date_end: new Date(),
        visible: true,
        realocate: false,
        expired: true,
        visualizable_mod: true,
        tags: ["BeMean", "Hibrid", "Ionic"],
        members: [{
            member_id: usersID[2]._id,
            type: ["contributor"],
            notify: false
        }, {
            member_id: usersID[3]._id,
            type: ["contributor"],
            notify: true
        }, {
            member_id: usersID[4]._id,
            type: ["view"],
            notify: false
        }, {
            member_id: usersID[5]._id,
            type: ["admin"],
            notify: true
        }, {
            member_id: usersID[6]._id,
            type: ["admin"],
            notify: false
        }],
        goals: [{
            name: "Virar Full foda full",
            description: "In id sapien ex",
            date_begin: new Date(),
            date_dream: new Date(),
            date_end: new Date(),
            realocate: false,
            expired: true,
            tags: ["Scope", "Function", "JS"],
            historic: [],
            activities: [{
                activity_id: insertActivity("1 Terceiro Projeto")
            }, {
                activity_id: insertActivity("2 Terceiro Projeto")
            }]
        }]
    }, {
        name: "Quarto projeto",
        description: "Integer sed sem vel leo egestas tristique eget eu tellus",
        date_begin: new Date(),
        date_dream: new Date(),
        date_end: new Date(),
        visible: false,
        realocate: true,
        expired: false,
        visualizable_mod: null,
        tags: ["Cordova", "Express", "NoSQL"],
        members: [{
            member_id: usersID[3]._id,
            type: ["admin"],
            notify: false
        }, {
            member_id: usersID[4]._id,
            type: ["contributor"],
            notify: true
        }, {
            member_id: usersID[5]._id,
            type: ["view"],
            notify: false
        }, {
            member_id: usersID[6]._id,
            type: ["admin"],
            notify: true
        }, {
            member_id: usersID[7]._id,
            type: ["admin"],
            notify: false
        }],
        goals: [{
            name: "Ao infinito e além",
            description: "Donec convallis ac tellus eu laoreet",
            date_begin: new Date(),
            date_dream: new Date(),
            date_end: new Date(),
            realocate: true,
            expired: false,
            tags: ["Code", "Funcional", "JS"],
            historic: [],
            activities: [{
                activity_id: insertActivity("1 Quarto Projeto")
            }, {
                activity_id: insertActivity("2 Quarto Projeto")
            }]
        }]
    }, {
        name: "Quinto projeto",
        description: "Curabitur efficitur tellus id augue sodales",
        date_begin: new Date(),
        date_dream: new Date(),
        date_end: new Date(),
        visible: true,
        realocate: false,
        expired: true,
        visualizable_mod: false,
        tags: ["Tecnology", "HTTPS", "Socket"],
        members: [{
            member_id: usersID[4]._id,
            type: ["view"],
            notify: false
        }, {
            member_id: usersID[5]._id,
            type: ["admin"],
            notify: true
        }, {
            member_id: usersID[6]._id,
            type: ["admin"],
            notify: false
        }, {
            member_id: usersID[7]._id,
            type: ["view"],
            notify: true
        }, {
            member_id: usersID[8]._id,
            type: ["contributor"],
            notify: false
        }],
        goals: [{
            name: "Q coisa não?",
            description: "nascetur ridiculus mus",
            date_begin: new Date(),
            date_dream: new Date(),
            date_end: new Date(),
            realocate: false,
            expired: true,
            tags: ["Teacher", "MEAN", "JS"],
            historic: [],
            activities: []
        }]
    }])
    
    BulkWriteResult({
        "writeErrors" : [ ],
        "writeConcernErrors" : [ ],
        "nInserted" : 5,
        "nUpserted" : 0,
        "nMatched" : 0,
        "nModified" : 0,
        "nRemoved" : 0,
        "upserted" : [ ]
    })
```

## Retrieve - busca

#### 1. Liste as informações dos membros de 1 projeto específico que deve ser buscado pelo seu nome de forma a não ligar para maiúsculas e minúsculas.
```js
    // Armazenará os membros
    var members = []

    // Função que insere as informações dos membros na variável members
    var getMember = function(cur){ members.push(db.users.findOne({_id: cur.member_id})) }

    // Retorna as referências (ids) dos membros no projeto
    var infoMembers = db.projects.findOne({name: /quinto projeto/i}, {_id: 0, members: 1})

    // itera sobre cada membro do projeto
    infoMembers.members.forEach(getMember)
    
    // Exibe as informações dos membros
    members   
    [
        {
                "_id" : ObjectId("567aec56b247464fb5a13d6e"),
                "name" : "Pablo Dinella",
                "bio" : "Vai q é tuma lek",
                "date_register" : ISODate("2015-12-23T18:47:49.031Z"),
                "auth" : {
                        "username" : "pdinella",
                        "email" : "pdinella@pdinella.com.br",
                        "password" : "allenidp",
                        "last_access" : ISODate("2015-12-23T18:47:49.031Z"),
                        "online" : true,
                        "disabled" : true,
                        "hash_token" : "d41d8cd98f00b204e9800998ecf8427e"
                },
                "avatar" : null,
                "backgound" : null
        },
        {
                "_id" : ObjectId("567aec56b247464fb5a13d6f"),
                "name" : "Felipe Marchant",
                "bio" : "Vai q é tuma lek",
                "date_register" : ISODate("2015-12-23T18:47:49.031Z"),
                "auth" : {
                        "username" : "fmarchant",
                        "email" : "fmarchant@fmarchant.com.br",
                        "password" : "tnahcramf",
                        "last_access" : ISODate("2015-12-23T18:47:49.031Z"),
                        "online" : false,
                        "disabled" : false,
                        "hash_token" : "d41d8cd98f00b204e9800998ecf8427e"
                },
                "avatar" : null,
                "backgound" : null
        },
        {
                "_id" : ObjectId("567aec56b247464fb5a13d70"),
                "name" : "Samuel Verneck",
                "bio" : "Vai q é tuma lek",
                "date_register" : ISODate("2015-12-23T18:47:49.031Z"),
                "auth" : {
                        "username" : "sverneck",
                        "email" : "sverneck@sverneck.com.br",
                        "password" : "kcenrevs",
                        "last_access" : ISODate("2015-12-23T18:47:49.031Z"),
                        "online" : true,
                        "disabled" : true,
                        "hash_token" : "d41d8cd98f00b204e9800998ecf8427e"
                },
                "avatar" : null,
                "backgound" : null
        },
        {
                "_id" : ObjectId("567aec56b247464fb5a13d71"),
                "name" : "Diego Ferreira",
                "bio" : "Vai q é tuma lek",
                "date_register" : ISODate("2015-12-23T18:47:49.031Z"),
                "auth" : {
                        "username" : "dferreira",
                        "email" : "dferreira@dferreira.com.br",
                        "password" : "arierrefd",
                        "last_access" : ISODate("2015-12-23T18:47:49.031Z"),
                        "online" : false,
                        "disabled" : false,
                        "hash_token" : "d41d8cd98f00b204e9800998ecf8427e"
                },
                "avatar" : null,
                "backgound" : null
        },
        {
                "_id" : ObjectId("567aec56b247464fb5a13d72"),
                "name" : "Jean Nascimento",
                "bio" : "Vai q é tuma lek",
                "date_register" : ISODate("2015-12-23T18:47:49.031Z"),
                "auth" : {
                        "username" : "jnascimento",
                        "email" : "jnascimento@jnascimento.com.br",
                        "password" : "otnemicsanj",
                        "last_access" : ISODate("2015-12-23T18:47:49.031Z"),
                        "online" : true,
                        "disabled" : true,
                        "hash_token" : "d41d8cd98f00b204e9800998ecf8427e"
                },
                "avatar" : null,
                "backgound" : null
        }
    ]
```
#### 2. Liste todos os projetos com a tag que você escolheu para os 3 projetos em comum.
```js
    db.projects.find({tags: {$in: [/bemean/i]}}, {name: 1})
    { "_id" : ObjectId("567aed7ab247464fb5a13d84"), "name" : "Primeiro Projeto" }
    { "_id" : ObjectId("567aed7ab247464fb5a13d85"), "name" : "Segundo Projeto" }
    { "_id" : ObjectId("567aed7ab247464fb5a13d86"), "name" : "Terceiro projeto" }
```
#### 3. Liste apenas os nomes de todas as atividades para todos os projetos.
```js
    var projects = {}
    
    // retorna o name e o _id de todas activities
    var activitiesId = db.activities.find({}, {_id: 1, name: 1}).toArray()
    
    // itera sobre as activities e retorna o name do project de cada
    activitiesId.forEach(function(cur){
        var nameProject = db.projects.findOne({'goals.activities.activity_id': cur._id}, {name: 1, _id: 0}).name
    
        if(nameProject in projects == false)
            projects[nameProject] = {} 
            
        if('activities' in projects[nameProject] == false)
            projects[nameProject]['activities'] = []
            
        projects[nameProject]['activities'].push(cur.name) 
    })
    
    projects
    {
        "Primeiro Projeto" : {
                "activities" : [
                        "Name Activite 1 Primeiro Projeto",
                        "Name Activite 2 Primeiro Projeto"
                ]
        },
        "Segundo Projeto" : {
                "activities" : [
                        "Name Activite 1 Segundo Projeto",
                        "Name Activite 2 Segundo Projeto"
                ]
        },
        "Terceiro projeto" : {
                "activities" : [
                        "Name Activite 1 Terceiro Projeto",
                        "Name Activite 2 Terceiro Projeto"
                ]
        },
        "Quarto projeto" : {
                "activities" : [
                        "Name Activite 1 Quarto Projeto",
                        "Name Activite 2 Quarto Projeto"
                ]
        }
    }
```
#### 4. Liste todos os projetos que não possuam uma tag.
```js
    var query = {tags: {$not: {$in: [/node.js/i]}}}

    var projection = {name: 1}

    db.projects.find(query, projection)
    > var query = {tags: {$not: {$in: [/node.js/i]}}}
    { "_id" : ObjectId("567afc85068a0d0c0f7f0495"), "name" : "Terceiro projeto" }
    { "_id" : ObjectId("567afc85068a0d0c0f7f0496"), "name" : "Quarto projeto" }
    { "_id" : ObjectId("567afc85068a0d0c0f7f0497"), "name" : "Quinto projeto" }
```
#### 5. Liste todos os usuários que não fazem parte do primeiro projeto cadastrado.
```js
    var usersNot = []

    var usersFirstProject = db.projects.findOne({name: "Primeiro Projeto"}, {members: 1, _id: 0})
    
    var getUser = function(cur){usersNot.push(db.users.findOne(cur.member_id, {name: 1})._id)}
    
    usersFirstProject.members.forEach(getUser)
    
    db.users.find({_id: {$not: {$in: usersNot}}}, {name: 1})
    { "_id" : ObjectId("567aec56b247464fb5a13d6f"), "name" : "Felipe Marchant" }
    { "_id" : ObjectId("567aec56b247464fb5a13d70"), "name" : "Samuel Verneck" }
    { "_id" : ObjectId("567aec56b247464fb5a13d71"), "name" : "Diego Ferreira" }
    { "_id" : ObjectId("567aec56b247464fb5a13d72"), "name" : "Jean Nascimento" }
    { "_id" : ObjectId("567aec56b247464fb5a13d73"), "name" : "Willian Bruno" }
```

## Update - alteração

#### 1. Adicione para todos os projetos o campo `views: 0`.
```js
    var query = {}
    var mod = {$set: {views: 0}}
    var options = {multi: true}
    
    db.projects.update(query, mod, options)
    WriteResult({ "nMatched" : 5, "nUpserted" : 0, "nModified" : 5 })
```

#### 2. Adicione 1 tag diferente para cada projeto.
```js
    var projects = db.projects.find({}, {_id: 1, name: 1}).toArray()
    
    projects.forEach(function(project){
        db.projects.update({_id: project._id}, {$push: {tags: "new tag " + project.name}})
    })
```

#### 3. Adicione 2 membros diferentes para cada projeto.
```js

    var usersID = db.users.find({}, {_id: 1}).toArray()

    db.projects.find({}, {_id: 1, members: 1}).forEach(function(project){
        var i = 0, y = 0
        do{
        
            var newUser = usersID[i]._id
        
            // verifica se o usuário já esta inserido no projeto
            var filtered = project.members.filter(function(value){
                return value.member_id.equals(newUser)
            }).length;
            
            if(filtered == 0){
                db.projects.update({_id: project._id}, {$push: {members: {member_id: newUser}}})
                y++
            }
            i++
            
        }while(y < 2 && i < 10)
    })
```

#### 4. Adicione 1 comentário em cada atividade, deixe apenas 1 projeto sem.
```js
    var activitiesId = []

    db.activities.find({name: {$not: /quinto/i}}, {_id: 1}).forEach(function(activity){
        activitiesId.push(activity._id)
    })
    
    var comment = {comments: {text: "New Comment", date_comment: new Date(), members: [], files: []}}
    
    db.activities.update({_id: {$in: activitiesId}}, {$push: comment}, {multi: true})
    WriteResult({ "nMatched" : 8, "nUpserted" : 0, "nModified" : 8 })
```

#### 5. Adicione 1 projeto inteiro com **UPSERT**.
```js
    var usersID = db.users.find({}, {_id: 1}).toArray()
    
    var query = {name: /sexto projeto/i}
    var mod = {
        $set: {active: true},
        $setOnInsert: {
            name: "Sexto Projeto",
            description: "consectetur adipiscing elit",
            date_begin: new Date(),
            date_dream: new Date(),
            date_end: new Date(),
            visible: true,
            realocate: false,
            expired: false,
            visualizable_mod: null,
            tags: ["BeMean", "Node.js", "MongoDB"],
            members: [{
                member_id: usersID[0]._id,
                type: ["admin"],
                notify: false
            }, {
                member_id: usersID[1]._id,
                type: ["view"],
                notify: true
            }, {
                member_id: usersID[2]._id,
                type: ["admin"],
                notify: false
            }, {
                member_id: usersID[5]._id,
                type: ["view"],
                notify: true
            }, {
                member_id: usersID[8]._id,
                type: ["contributor"],
                notify: false
            }],
            goals: [{
                name: "Viva JS",
                description: "Duis quis auctor magna",
                date_begin: new Date(),
                date_dream: new Date(),
                date_end: new Date(),
                realocate: false,
                expired: false,
                tags: ["Sei lá", "Google", "DELL"],
                historic: [],
                activities: []
            }]
        }
    }
    var options = {upsert: true}
    
    db.projects.update(query, mod, options)
    
    WriteResult({
            "nMatched" : 0,
            "nUpserted" : 1,
            "nModified" : 0,
            "_id" : ObjectId("567b1b05ce17611f50562b85")
    })
```

## Delete - remoção
#### 1. Apague todos os projetos que não possuam *tags*.
```js
    db.projects.remove({tags: {$size: 0}})
    WriteResult({ "nRemoved" : 0 })    
```

#### 2. Apague todos os projetos que não possuam comentários nas atividades.
```js
    var query = {$or: [{comments: {$exists: 0}}, {comments: {$size: 0}}, {comments: {$eq: [ ]}}]}
    var activities = db.activities.find(query, {_id: 1})
    
    var activitiesId = []
    
    activities.forEach(function(cur){
        activitiesId.push(cur._id)    
    })
    
    // apaga as atividades
    db.activities.remove({'_id' : { $in : activitiesId}}, {multi:1})
    WriteResult({ "nRemoved" : 0 })  
    
    // apaga os projetos
    db.projects.remove({'goals.activities.activity_id' : { $in : activitiesId } }, {multi:1})
    WriteResult({ "nRemoved" : 0 })  
```

#### 3. Apague todos os projetos que não possuam atividades.
```js
    var query = {$or: [{'goals.activities': {$exists: 0}}, {'goals.activities': {$size: 0}}, {'goals.activities': {$eq: [ ]}}]}
    
    db.projects.remove(query, {multi:1})
    WriteResult({ "nRemoved" : 2 })
```

#### 4. Escolha 2 usuário e apague todos os projetos em que os 2 fazem parte.
```js
    var query = {
        $or: [{
            'auth.username': 'dnascimento'
        }, {
            'auth.username': 'daraujo '
        }]
    }
    
    // retorna apenas os ID's dos users
    var usersId = db.users.find(query, {
        _id: 1
    }).toArray()

    // retorna os ID's dos projetos e e os ID's das atividades dos projetos que os users pertencem
    var Ids = db.projects.aggregate([{
        $match: {
            $and: [{
                'members.member_id': usersId[0]._id
            }, {
                'members.member_id': usersId[0]._id
            }]
        }
    },
    {$unwind: "$goals"},
    {$unwind: "$goals.activities"}, 
    {
        $group: {
            _id: null,
            projects: {
                $push: "$_id"
            },
            activities: {
                $push: "$goals.activities.activity_id"
            }
        }
    }]).toArray()
    
    // apaga todos os documentos da coleção activities, não foi pedido porém faz sentido apagar hehe
    db.activities.remove({_id: {$in: Ids[0].activities}})
    WriteResult({ "nRemoved" : 8 })
    
    // apaga todos os projetos
    db.projects.remove({_id: {$in: Ids[0].projects}})   
    WriteResult({ "nRemoved" : 4 })
```

#### 5. Apague todos os projetos que possuam uma determinada *tag* em *goal*.
```js
    var Ids = db.projects.aggregate([
        {$match: {'goals.tags': /JS/i}},
        {$unwind: "$goals"},
        {$unwind: "$goals.activities"}, 
        {
            $group: {
                _id: null,
                projects: {
                    $push: "$_id"
                },
                activities: {
                    $push: "$goals.activities.activity_id"
                }
            }
    }]).toArray()    
    
    // apaga todos os documentos da coleção activities, não foi pedido porém faz sentido apagar hehe
    db.activities.remove({_id: {$in: Ids[0].activities}})
    WriteResult({ "nRemoved" : 4 })
    
    // apaga todos os projetos
    db.projects.remove({_id: {$in: Ids[0].projects}})
    WriteResult({ "nRemoved" : 2 })
```

## Gerenciamento de usuários
#### 1. Crie um usuário com permissões **APENAS** de Leitura.
```js
    use admin

    db.createUser(
      {
        user: "dnascimento",
        pwd: "d123",
        roles: [ 
            "read"
        ]
      }
    )
    Successfully added user: { "user" : "dnascimento", "roles" : [ "read" ] }
```

#### 2. Crie um usuário com permissões de Escrita e Leitura.
```js
    use admin

    db.createUser(
      {
        user: "alguem",
        pwd: "sei_nao123",
        roles: [ "readWrite" ]
      }
    )
    Successfully added user: { "user" : "alguem", "roles" : [ "readWrite" ] }    
```

#### 3. Adicionar o papel `grantRolesToUser` e `revokeRole` para o usuário com Escrita e Leitura.
> Como os papeis solicitados não existem, os mesmo inicialmente são criados para após serem atribuidos ao usuário


```js
    use admin
    
    // create grantRolesToUser
    db.createRole(
       {
         role: "grantRolesToUser",
         privileges: [
           { resource: { db: "admin", collection: "" }, actions: [ "grantRole" ] }       
         ],
         roles: []
       },
       { w: "majority" , wtimeout: 5000 }
    )
    {
            "role" : "grantRolesToUser",
            "privileges" : [
                    {
                            "resource" : {
                                    "db" : "admin",
                                    "collection" : ""
                            },
                            "actions" : [
                                    "grantRole"
                            ]
                    }
            ],
            "roles" : [ ]
    }

    // create revokeRole
    db.createRole(
       {
         role: "revokeRole",
         privileges: [
           { resource: { db: "admin", collection: "" }, actions: [ "revokeRole" ] }       
         ],
         roles: []
       },
       { w: "majority" , wtimeout: 5000 }
    )
    {
            "role" : "revokeRole",
            "privileges" : [
                    {
                            "resource" : {
                                    "db" : "admin",
                                    "collection" : ""
                            },
                            "actions" : [
                                    "revokeRole"
                            ]
                    }
            ],
            "roles" : [ ]
    }
    
    // adiciona os papeis ao usuário
    db.grantRolesToUser(
      "alguem", 
      [ "grantRolesToUser" , "revokeRole" ]
    )
```

#### 4. Remover o papel `grantRolesToUser` para o usuário com Escrita e Leitura.
```js
    db.runCommand( { 
      revokeRolesFromUser: "alguem",
      roles: [
        "grantRolesToUser"
      ]
    })
    { "ok" : 1 }
```
#### 5. Listar todos os usuários com seus papéis e ações.
```js
    db.runCommand({
        usersInfo: [
            {user: "alguem", db: "admin"}, 
            {user: "dnascimento", db: "admin"}
        ], 
        showCredentials: true, 
        showPrivileges: true
    })
    {
            "users" : [
                    {
                            "_id" : "admin.alguem",
                            "user" : "alguem",
                            "db" : "admin",
                            "credentials" : {
                                    "SCRAM-SHA-1" : {
                                            "iterationCount" : 10000,
                                            "salt" : "cP004TAjgCdVHGb3qGMCnw==",
                                            "storedKey" : "Zqgs05a9Dqz1tqfeGUfKSils1b0=",
                                            "serverKey" : "p5DVonbiUlQ8nrHw6P5kqJLLmq8="
                                    }
                            },
                            "roles" : [
                                    {
                                            "role" : "readWrite",
                                            "db" : "admin"
                                    },
                                    {
                                            "role" : "revokeRole",
                                            "db" : "admin"
                                    }
                            ],
                            "inheritedRoles" : [
                                    {
                                            "role" : "readWrite",
                                            "db" : "admin"
                                    },
                                    {
                                            "role" : "revokeRole",
                                            "db" : "admin"
                                    }
                            ],
                            "inheritedPrivileges" : [
                                    {
                                            "resource" : {
                                                    "db" : "admin",
                                                    "collection" : ""
                                            },
                                            "actions" : [
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
                                                    "revokeRole",
                                                    "update"
                                            ]
                                    },
                                    {
                                            "resource" : {
                                                    "anyResource" : true
                                            },
                                            "actions" : [
                                                    "listCollections"
                                            ]
                                    },
                                    {
                                            "resource" : {
                                                    "db" : "admin",
                                                    "collection" : "system.indexes"
                                            },
                                            "actions" : [
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
                                            "resource" : {
                                                    "db" : "admin",
                                                    "collection" : "system.js"
                                            },
                                            "actions" : [
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
                                            "resource" : {
                                                    "db" : "admin",
                                                    "collection" : "system.namespaces"
                                            },
                                            "actions" : [
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
                            "_id" : "admin.dnascimento",
                            "user" : "dnascimento",
                            "db" : "admin",
                            "credentials" : {
                                    "SCRAM-SHA-1" : {
                                            "iterationCount" : 10000,
                                            "salt" : "3/sXw/LGfvWHuyuL4ZplLw==",
                                            "storedKey" : "DgDmrdLQ6c7JC6VgNAPR+oc7Lnw=",
                                            "serverKey" : "ai8LN8VgjYjqzHFZiVAiLmGXgHM="
                                    }
                            },
                            "roles" : [
                                    {
                                            "role" : "read",
                                            "db" : "admin"
                                    }
                            ],
                            "inheritedRoles" : [
                                    {
                                            "role" : "read",
                                            "db" : "admin"
                                    }
                            ],
                            "inheritedPrivileges" : [
                                    {
                                            "resource" : {
                                                    "db" : "admin",
                                                    "collection" : ""
                                            },
                                            "actions" : [
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
                                            "resource" : {
                                                    "anyResource" : true
                                            },
                                            "actions" : [
                                                    "listCollections"
                                            ]
                                    },
                                    {
                                            "resource" : {
                                                    "db" : "admin",
                                                    "collection" : "system.indexes"
                                            },
                                            "actions" : [
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
                                            "resource" : {
                                                    "db" : "admin",
                                                    "collection" : "system.js"
                                            },
                                            "actions" : [
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
                                            "resource" : {
                                                    "db" : "admin",
                                                    "collection" : "system.namespaces"
                                            },
                                            "actions" : [
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
            "ok" : 1
    }
```

## Sharding
### 1 Config Server
##### Criação do ***Config Server*** setando a porta `27010`
```js
    DavidsonSilva@DAVIDSON /c$ mkdir data/configdb

    DavidsonSilva@DAVIDSON /c$ mongod --configsvr --port 27010
    2015-12-26T09:32:10.526-0200 I JOURNAL  [initandlisten] journal dir=c:\data\configdb\journal
    2015-12-26T09:32:10.527-0200 I JOURNAL  [initandlisten] recover : no journal files present, no recovery needed
    2015-12-26T09:32:10.665-0200 I JOURNAL  [durability] Durability thread started
    2015-12-26T09:32:10.667-0200 I JOURNAL  [journal writer] Journal writer thread started
    2015-12-26T09:32:10.858-0200 I CONTROL  [initandlisten] MongoDB starting : pid=8456 port=27010 dbpath=c:\data\configdb\ master=1 64-bit host=Davidson
    2015-12-26T09:32:10.859-0200 I CONTROL  [initandlisten] targetMinOS: Windows Server 2003 SP2
    2015-12-26T09:32:10.860-0200 I CONTROL  [initandlisten] db version v3.0.7
    2015-12-26T09:32:10.860-0200 I CONTROL  [initandlisten] git version: 6ce7cbe8c6b899552dadd907604559806aa2e9bd
    2015-12-26T09:32:10.861-0200 I CONTROL  [initandlisten] build info: windows sys.getwindowsversion(major=6, minor=1, build=7601, platform=2, service_pack='Service Pack 1') BOOST_LIB_VERSION=1_49
    2015-12-26T09:32:10.861-0200 I CONTROL  [initandlisten] allocator: tcmalloc
    2015-12-26T09:32:10.862-0200 I CONTROL  [initandlisten] options: { net: { port:27010 }, sharding: { clusterRole: "configsvr" } }
    2015-12-26T09:32:10.865-0200 I INDEX    [initandlisten] allocating new ns file c:\data\configdb\local.ns, filling with zeroes...
    2015-12-26T09:32:11.337-0200 I STORAGE  [FileAllocator] allocating new datafile c:\data\configdb\local.0, filling with zeroes...
    2015-12-26T09:32:11.338-0200 I STORAGE  [FileAllocator] creating directory c:\data\configdb\_tmp
    2015-12-26T09:32:11.348-0200 I STORAGE  [FileAllocator] done allocating datafile c:\data\configdb\local.0, size: 16MB,  took 0.007 secs
    2015-12-26T09:32:11.402-0200 I REPL     [initandlisten] ******
    2015-12-26T09:32:11.403-0200 I REPL     [initandlisten] creating replication oplog of size: 5MB...
    2015-12-26T09:32:11.450-0200 I REPL     [initandlisten] ******
    2015-12-26T09:32:11.452-0200 I NETWORK  [initandlisten] waiting for connectionson port 27010
```

### 1 Router
##### Crio o ***Router*** utilizando o `mongos`, setando o ***Config Server*** que ele acessará para ter as informações dos ***Shards***
```js
    DavidsonSilva@DAVIDSON ~$ mongos -configdb localhost:27010 --port 27011
    2015-12-26T09:39:39.549-0200 W SHARDING running with 1 config server should be done only for testing purposes and is not recommended for production
    2015-12-26T09:39:39.555-0200 I SHARDING [mongosMain] MongoS version 3.0.7 starting: pid=7820 port=27011 64-bit host=Davidson (--help for usage)
    2015-12-26T09:39:39.555-0200 I CONTROL  [mongosMain] db version v3.0.7
    2015-12-26T09:39:39.555-0200 I CONTROL  [mongosMain] git version: 6ce7cbe8c6b899552dadd907604559806aa2e9bd
    2015-12-26T09:39:39.555-0200 I CONTROL  [mongosMain] build info: windows sys.getwindowsversion(major=6, minor=1, build=7601, platform=2, service_pack='Service Pack 1') BOOST_LIB_VERSION=1_49
    2015-12-26T09:39:39.555-0200 I CONTROL  [mongosMain] allocator: tcmalloc
    2015-12-26T09:39:39.555-0200 I CONTROL  [mongosMain] options: { net: { port: 27011 }, sharding: { configDB: "localhost:27010" } }
    2015-12-26T09:39:39.718-0200 I SHARDING [LockPinger] creating distributed lock ping thread for localhost:27010 and process Davidson:27011:1451129979:41 (sleeping for 30000ms)
    2015-12-26T09:39:40.113-0200 I SHARDING [LockPinger] cluster localhost:27010 pinged successfully at Sat Dec 26 09:39:39 2015 by distributed lock pinger 'localhost:27010/Davidson:27011:1451129979:41', sleeping for 30000ms
    2015-12-26T09:39:40.113-0200 I SHARDING [mongosMain] distributed lock 'configUpgrade/Davidson:27011:1451129979:41' acquired, ts : 567e7c7bcf1f2b73e81f7f3c
    2015-12-26T09:39:40.115-0200 I SHARDING [mongosMain] starting upgrade of configserver from v0 to v6
    2015-12-26T09:39:40.116-0200 I SHARDING [mongosMain] starting next upgrade stepfrom v0 to v6
    2015-12-26T09:39:40.116-0200 I SHARDING [mongosMain] about to log new metadata event: { _id: "Davidson-2015-12-26T11:39:40-567e7c7ccf1f2b73e81f7f3d", server: "Davidson", clientAddr: "N/A", time: new Date(1451129980116), what: "starting upgrade of config database", ns: "config.version", details: { from: 0, to: 6 } }
    2015-12-26T09:39:40.551-0200 I SHARDING [mongosMain] writing initial config version at v6
    2015-12-26T09:39:40.726-0200 I SHARDING [mongosMain] about to log new metadata event: { _id: "Davidson-2015-12-26T11:39:40-567e7c7ccf1f2b73e81f7f3f", server: "Davidson", clientAddr: "N/A", time: new Date(1451129980726), what: "finished upgr
    ade of config database", ns: "config.version", details: { from: 0, to: 6 } }
    2015-12-26T09:39:40.882-0200 I SHARDING [mongosMain] upgrade of config server tov6 successful
    2015-12-26T09:39:40.884-0200 I SHARDING [mongosMain] distributed lock 'configUpgrade/Davidson:27011:1451129979:41' unlocked.
    2015-12-26T09:39:42.553-0200 I SHARDING [Balancer] about to contact config servers and shards
    2015-12-26T09:39:42.554-0200 I SHARDING [Balancer] config servers and shards contacted successfully
    2015-12-26T09:39:42.554-0200 I SHARDING [Balancer] balancer id: Davidson:27011 started at Dec 26 09:39:42
    2015-12-26T09:39:42.571-0200 I SHARDING [Balancer] distributed lock 'balancer/Davidson:27011:1451129979:41' acquired, ts : 567e7c7ecf1f2b73e81f7f41
    2015-12-26T09:39:42.571-0200 I NETWORK  [mongosMain] waiting for connections onport 27011
    2015-12-26T09:39:42.956-0200 I SHARDING [Balancer] distributed lock 'balancer/Davidson:27011:1451129979:41' unlocked.
```

### 3 Shardings
##### Crio as pastas onde os ***Shards*** irá persistir os dados
```js
    mkdir /data/shard1 && mkdir /data/shard2 && mkdir /data/shard3
```

##### Levanto cada `Shard`, especificando a porta de cada
```js    
    DavidsonSilva@DAVIDSON /c$ mongod --port 27012 --dbpath data/shard1
    2015-12-26T09:55:17.249-0200 I JOURNAL  [initandlisten] journal dir=data/shard1\journal
    2015-12-26T09:55:17.250-0200 I JOURNAL  [initandlisten] recover : no journal files present, no recovery needed
    2015-12-26T09:55:17.291-0200 I JOURNAL  [durability] Durability thread started
    2015-12-26T09:55:17.292-0200 I JOURNAL  [journal writer] Journal writer thread started
    2015-12-26T09:55:17.461-0200 I CONTROL  [initandlisten] MongoDB starting : pid=3980 port=27012 dbpath=data/shard1 64-bit host=Davidson
    2015-12-26T09:55:17.462-0200 I CONTROL  [initandlisten] targetMinOS: Windows Server 2003 SP2
    2015-12-26T09:55:17.462-0200 I CONTROL  [initandlisten] db version v3.0.7
    2015-12-26T09:55:17.463-0200 I CONTROL  [initandlisten] git version: 6ce7cbe8c6b899552dadd907604559806aa2e9bd
    2015-12-26T09:55:17.463-0200 I CONTROL  [initandlisten] build info: windows sys.getwindowsversion(major=6, minor=1, build=7601, platform=2, service_pack='Service Pack 1') BOOST_LIB_VERSION=1_49
    2015-12-26T09:55:17.464-0200 I CONTROL  [initandlisten] allocator: tcmalloc
    2015-12-26T09:55:17.464-0200 I CONTROL  [initandlisten] options: { net: { port:27012 }, storage: { dbPath: "data/shard1" } }
    2015-12-26T09:55:17.467-0200 I INDEX    [initandlisten] allocating new ns file data/shard1\local.ns, filling with zeroes...
    2015-12-26T09:55:18.125-0200 I STORAGE  [FileAllocator] allocating new datafiledata/shard1\local.0, filling with zeroes...
    2015-12-26T09:55:18.126-0200 I STORAGE  [FileAllocator] creating directory data/shard1\_tmp
    2015-12-26T09:55:18.137-0200 I STORAGE  [FileAllocator] done allocating datafile data/shard1\local.0, size: 64MB,  took 0.008 secs
    2015-12-26T09:55:18.158-0200 I NETWORK  [initandlisten] waiting for connections on port 27012
```

```js    
    DavidsonSilva@DAVIDSON /c
    $ mongod --port 27013 --dbpath data/shard2
    2015-12-26T09:57:13.123-0200 I JOURNAL  [initandlisten] journal dir=data/shard2\journal
    2015-12-26T09:57:13.124-0200 I JOURNAL  [initandlisten] recover : no journal files present, no recovery needed
    2015-12-26T09:57:13.251-0200 I JOURNAL  [durability] Durability thread started
    2015-12-26T09:57:13.253-0200 I JOURNAL  [journal writer] Journal writer thread started
    2015-12-26T09:57:13.373-0200 I CONTROL  [initandlisten] MongoDB starting : pid=8324 port=27013 dbpath=data/shard2 64-bit host=Davidson
    2015-12-26T09:57:13.374-0200 I CONTROL  [initandlisten] targetMinOS: Windows Server 2003 SP2
    2015-12-26T09:57:13.374-0200 I CONTROL  [initandlisten] db version v3.0.7
    2015-12-26T09:57:13.375-0200 I CONTROL  [initandlisten] git version: 6ce7cbe8c6b899552dadd907604559806aa2e9bd
    2015-12-26T09:57:13.375-0200 I CONTROL  [initandlisten] build info: windows sys.getwindowsversion(major=6, minor=1, build=7601, platform=2, service_pack='Service Pack 1') BOOST_LIB_VERSION=1_49
    2015-12-26T09:57:13.376-0200 I CONTROL  [initandlisten] allocator: tcmalloc
    2015-12-26T09:57:13.376-0200 I CONTROL  [initandlisten] options: { net: { port:27013 }, storage: { dbPath: "data/shard2" } }
    2015-12-26T09:57:13.379-0200 I INDEX    [initandlisten] allocating new ns file data/shard2\local.ns, filling with zeroes...
    2015-12-26T09:57:13.765-0200 I STORAGE  [FileAllocator] allocating new datafile data/shard2\local.0, filling with zeroes...
    2015-12-26T09:57:13.766-0200 I STORAGE  [FileAllocator] creating directory data/shard2\_tmp
    2015-12-26T09:57:13.776-0200 I STORAGE  [FileAllocator] done allocating datafile data/shard2\local.0, size: 64MB,  took 0.007 secs
    2015-12-26T09:57:13.796-0200 I NETWORK  [initandlisten] waiting for connections on port 27013
```

```js
    DavidsonSilva@DAVIDSON /c$ mongod --port 27014 --dbpath data/shard3
    2015-12-26T10:01:19.249-0200 I JOURNAL  [initandlisten] journal dir=data/shard3\journal
    2015-12-26T10:01:19.250-0200 I JOURNAL  [initandlisten] recover : no journal files present, no recovery needed
    2015-12-26T10:01:19.434-0200 I JOURNAL  [durability] Durability thread started
    2015-12-26T10:01:19.435-0200 I JOURNAL  [journal writer] Journal writer thread started
    2015-12-26T10:01:19.617-0200 I CONTROL  [initandlisten] MongoDB starting : pid=6908 port=27014 dbpath=data/shard3 64-bit host=Davidson
    2015-12-26T10:01:19.618-0200 I CONTROL  [initandlisten] targetMinOS: Windows Server 2003 SP2
    2015-12-26T10:01:19.618-0200 I CONTROL  [initandlisten] db version v3.0.7
    2015-12-26T10:01:19.620-0200 I CONTROL  [initandlisten] git version: 6ce7cbe8c6b899552dadd907604559806aa2e9bd
    2015-12-26T10:01:19.620-0200 I CONTROL  [initandlisten] build info: windows sys.getwindowsversion(major=6, minor=1, build=7601, platform=2, service_pack='Service Pack 1') BOOST_LIB_VERSION=1_49
    2015-12-26T10:01:19.621-0200 I CONTROL  [initandlisten] allocator: tcmalloc
    2015-12-26T10:01:19.621-0200 I CONTROL  [initandlisten] options: { net: { port:27014 }, storage: { dbPath: "data/shard3" } }
    2015-12-26T10:01:19.624-0200 I INDEX    [initandlisten] allocating new ns file data/shard3\local.ns, filling with zeroes...
    2015-12-26T10:01:20.040-0200 I STORAGE  [FileAllocator] allocating new datafiledata/shard3\local.0, filling with zeroes...
    2015-12-26T10:01:20.041-0200 I STORAGE  [FileAllocator] creating directory data/shard3\_tmp
    2015-12-26T10:01:20.051-0200 I STORAGE  [FileAllocator] done allocating datafile data/shard3\local.0, size: 64MB,  took 0.007 secs
    2015-12-26T10:01:20.070-0200 I NETWORK  [initandlisten] waiting for connections on port 27014
```

##### Conecto no ***Router*** e registramos os ***Shards***
```js    
    DavidsonSilva@DAVIDSON ~$ mongo --port 27011 --host localhost
    MongoDB shell version: 3.0.7
    connecting to: localhost:27011/test
    mongos> use project
    switched to db project
    mongos>
    mongos> sh.addShard("localhost:27012")
    { "shardAdded" : "shard0000", "ok" : 1 }
    mongos> sh.addShard("localhost:27013")
    { "shardAdded" : "shard0001", "ok" : 1 }
    mongos> sh.addShard("localhost:27014")
    { "shardAdded" : "shard0002", "ok" : 1 }
    mongos>
```

##### Especifico qual ***database*** iremos ***shardear***
```js
    mongos> sh.enableSharding("project")
    { "ok" : 1 }
    mongos>
```

##### Especifico qual ***coleção*** dessa ***database*** será ***shardeada***
```js
    mongos> sh.shardCollection("project.activities", {"_id" : 1})
    { "collectionsharded" : "project.activities", "ok" : 1 }
    mongos>
```

##### Envio de dados para o ***Router***
```js
    mongos> for ( var i = 1; i < 100000; i++ ) {
      db.activities.insert({
            name : 'Activitie number ' + i,
            description : 'Description ' + i,
            date_begin : new Date(),
            date_dream : new Date(),
            date_end : new Date(),
            realocate : false,
            expired : true,
            tags : [],
            historic : [],
            members : [],
            comments : []
        })
    }
    WriteResult({ "nInserted" : 1 })
    mongos>
```

## Replica
###  3 Replicas

##### Criação de 3 novas pastas onde o `mongod` será levantado
```js
    DavidsonSilva@DAVIDSON /C$ mkdir data/rs1
    DavidsonSilva@DAVIDSON /C$ mkdir data/rs2
    DavidsonSilva@DAVIDSON /C$ mkdir data/rs3
```

##### Levantar o `mongod` com --replSet em todos diretórios
```js
    DavidsonSilva@DAVIDSON /C$ mongod --replSet replica_set --port 27017 --dbpath data/rs1
    2015-12-24T17:05:54.844-0200 I JOURNAL  [initandlisten] journal dir=data/rs1\journal
    2015-12-24T17:05:54.845-0200 I JOURNAL  [initandlisten] recover : no journal files present, no recovery needed
    2015-12-24T17:05:54.863-0200 I JOURNAL  [durability] Durability thread started
    2015-12-24T17:05:54.864-0200 I JOURNAL  [journal writer] Journal writer thread started
    2015-12-24T17:05:54.958-0200 I CONTROL  [initandlisten] MongoDB starting : pid=8812 port=27017 dbpath=data/rs1 64-bit host=Davidson
    2015-12-24T17:05:54.959-0200 I CONTROL  [initandlisten] targetMinOS: Windows Server 2003 SP2
    2015-12-24T17:05:54.959-0200 I CONTROL  [initandlisten] db version v3.0.7
    2015-12-24T17:05:54.959-0200 I CONTROL  [initandlisten] git version: 6ce7cbe8c6b899552dadd907604559806aa2e9bd
    2015-12-24T17:05:54.959-0200 I CONTROL  [initandlisten] build info: windows sys.getwindowsversion(major=6, minor=1, build=7601, platform=2, service_pack='Servic
    e Pack 1') BOOST_LIB_VERSION=1_49
    2015-12-24T17:05:54.959-0200 I CONTROL  [initandlisten] allocator: tcmalloc 
    2015-12-24T17:05:54.959-0200 I CONTROL  [initandlisten] options: { net: { port: 27017 }, replication: { replSet: "replica_set" }, storage: { dbPath: "data/rs1" } }
    2015-12-24T17:05:54.965-0200 I INDEX    [initandlisten] allocating new ns file data/rs1\local.ns, filling with zeroes...
    2015-12-24T17:05:55.406-0200 I STORAGE  [FileAllocator] allocating new datafile data/rs1\local.0, filling with zeroes...
    2015-12-24T17:05:55.407-0200 I STORAGE  [FileAllocator] creating directory data/rs1\_tmp
    2015-12-24T17:05:55.414-0200 I STORAGE  [FileAllocator] done allocating datafile data/rs1\local.0, size: 64MB,  took 0.006 secs
    2015-12-24T17:05:55.456-0200 I REPL     [initandlisten] Did not find local replica set configuration document at startup; NoMatchingDocument Did not find replica set configuration document in local.system.replset
    2015-12-24T17:05:55.457-0200 I NETWORK  [initandlisten] waiting for connections on port 27017
```

```js
    DavidsonSilva@DAVIDSON /C$ mongod --replSet replica_set --port 27018 --dbpath data/rs2
    2015-12-24T17:07:33.185-0200 I JOURNAL  [initandlisten] journal dir=data/rs2\journal
    2015-12-24T17:07:33.186-0200 I JOURNAL  [initandlisten] recover : no journal files present, no recovery needed
    2015-12-24T17:07:33.254-0200 I JOURNAL  [durability] Durability thread started
    2015-12-24T17:07:33.256-0200 I JOURNAL  [journal writer] Journal writer thread started
    2015-12-24T17:07:33.382-0200 I CONTROL  [initandlisten] MongoDB starting : pid=3584 port=27018 dbpath=data/rs2 64-bit host=Davidson
    2015-12-24T17:07:33.382-0200 I CONTROL  [initandlisten] targetMinOS: Windows Server 2003 SP2
    2015-12-24T17:07:33.383-0200 I CONTROL  [initandlisten] db version v3.0.7
    2015-12-24T17:07:33.383-0200 I CONTROL  [initandlisten] git version: 6ce7cbe8c6b899552dadd907604559806aa2e9bd
    2015-12-24T17:07:33.383-0200 I CONTROL  [initandlisten] build info: windows sys.getwindowsversion(major=6, minor=1, build=7601, platform=2, service_pack='Service Pack 1') BOOST_LIB_VERSION=1_49
    2015-12-24T17:07:33.384-0200 I CONTROL  [initandlisten] allocator: tcmalloc
    2015-12-24T17:07:33.384-0200 I CONTROL  [initandlisten] options: { net: { port: 27018 }, replication: { replSet: "replica_set" }, storage: { dbPath: "data/rs2" } }
    2015-12-24T17:07:33.386-0200 I INDEX    [initandlisten] allocating new ns file data/rs2\local.ns, filling with zeroes...
    2015-12-24T17:07:33.938-0200 I STORAGE  [FileAllocator] allocating new datafiledata/rs2\local.0, filling with zeroes...
    2015-12-24T17:07:33.939-0200 I STORAGE  [FileAllocator] creating directory data/rs2\_tmp
    2015-12-24T17:07:33.949-0200 I STORAGE  [FileAllocator] done allocating datafile data/rs2\local.0, size: 64MB,  took 0.007 secs
    2015-12-24T17:07:33.969-0200 I REPL     [initandlisten] Did not find local replica set configuration document at startup;  NoMatchingDocument Did not find replica set configuration document in local.system.replset 
    2015-12-24T17:07:33.972-0200 I NETWORK  [initandlisten] waiting for connections on port 27018
```

```js
    DavidsonSilva@DAVIDSON /C$ mongod --replSet replica_set --port 27019 --dbpath data/rs3
    2015-12-24T17:07:59.764-0200 I JOURNAL  [initandlisten] journal dir=data/rs3\journal
    2015-12-24T17:07:59.766-0200 I JOURNAL  [initandlisten] recover : no journal files present, no recovery needed
    2015-12-24T17:07:59.903-0200 I JOURNAL  [durability] Durability thread started
    2015-12-24T17:07:59.905-0200 I JOURNAL  [journal writer] Journal writer thread started
    2015-12-24T17:08:00.084-0200 I CONTROL  [initandlisten] MongoDB starting : pid=216 port=27019 dbpath=data/rs3 64-bit host=Davidson
    2015-12-24T17:08:00.084-0200 I CONTROL  [initandlisten] targetMinOS: Windows Server 2003 SP2
    2015-12-24T17:08:00.085-0200 I CONTROL  [initandlisten] db version v3.0.7
    2015-12-24T17:08:00.085-0200 I CONTROL  [initandlisten] git version: 6ce7cbe8c6b899552dadd907604559806aa2e9bd
    2015-12-24T17:08:00.086-0200 I CONTROL  [initandlisten] build info: windows sys.
    getwindowsversion(major=6, minor=1, build=7601, platform=2, service_pack='Service Pack 1') BOOST_LIB_VERSION=1_49
    2015-12-24T17:08:00.086-0200 I CONTROL  [initandlisten] allocator: tcmalloc
    2015-12-24T17:08:00.087-0200 I CONTROL  [initandlisten] options: { net: { port: 27019 }, replication: { replSet: "replica_set" }, storage: { dbPath: "data/rs3" } }
    2015-12-24T17:08:00.090-0200 I INDEX    [initandlisten] allocating new ns file data/rs3\local.ns, filling with zeroes...
    2015-12-24T17:08:00.489-0200 I STORAGE  [FileAllocator] allocating new datafile data/rs3\local.0, filling with zeroes...
    2015-12-24T17:08:00.491-0200 I STORAGE  [FileAllocator] creating directory data/rs3\_tmp
    2015-12-24T17:08:00.515-0200 I STORAGE  [FileAllocator] done allocating datafile data/rs3\local.0, size: 64MB,  took 0.008 secs
    2015-12-24T17:08:00.546-0200 I REPL     [initandlisten] Did not find local replica set configuration document at startup;  NoMatchingDocument Did not find replica set configuration document in local.system.replset
    2015-12-24T17:08:00.549-0200 I NETWORK  [initandlisten] waiting for connections on port 27019
```

##### Conectamos no mongo --port 27017, que será o `PRIMARY` e iniciamos a `replicaSet`
```js
    DavidsonSilva@DAVIDSON ~$ mongo --port 27017
    MongoDB shell version: 3.0.7
    connecting to: 127.0.0.1:27017/test
    > var rsconf = {
    ...    _id: "replica_set",
    ...    members: [
    ...     {
    ...      _id: 0,
    ...      host: "127.0.0.1:27017"
    ...     }
    ...   ]
    ... }
    > rs.initiate(rsconf)
    { "ok" : 1 }
    replica_set:OTHER>
```

##### Adicionamos as outras réplicas na `replicaSet` criada
```js
    replica_set:OTHER> rs.add("127.0.0.1:27018")
    { "ok" : 1 }
    replica_set:PRIMARY> rs.add("127.0.0.1:27019")
    { "ok" : 1 }
    replica_set:PRIMARY>
```    

##### Conectamos a cada réplica `SECUNDARY` afim de verificação (em terminais diferentes)
```js 
    DavidsonSilva@DAVIDSON ~$ mongo --port 27018
    MongoDB shell version: 3.0.7
    connecting to: 127.0.0.1:27018/test
    replica_set:SECONDARY>

    DavidsonSilva@DAVIDSON ~$ mongo --port 27019
    MongoDB shell version: 3.0.7
    connecting to: 127.0.0.1:27019/test
    replica_set:SECONDARY>
```

##### Árbitro
```js    
    DavidsonSilva@DAVIDSON /c$ mkdir data/arb
    DavidsonSilva@DAVIDSON /c$ mongod --port 30000 --dbpath data/arb --replSet replica_set
    2015-12-24T17:46:52.128-0200 I JOURNAL  [initandlisten] journal dir=data/arb\journal
    2015-12-24T17:46:52.129-0200 I JOURNAL  [initandlisten] recover : no journal files present, no recovery needed
    2015-12-24T17:46:52.335-0200 I JOURNAL  [durability] Durability thread started
    2015-12-24T17:46:52.336-0200 I JOURNAL  [journal writer] Journal writer thread started
    2015-12-24T17:46:52.515-0200 I CONTROL  [initandlisten] MongoDB starting : pid=8324 port=30000 dbpath=data/arb 64-bit host=Davidson
    2015-12-24T17:46:52.516-0200 I CONTROL  [initandlisten] targetMinOS: Windows Server 2003 SP2
    2015-12-24T17:46:52.516-0200 I CONTROL  [initandlisten] db version v3.0.7
    2015-12-24T17:46:52.516-0200 I CONTROL  [initandlisten] git version: 6ce7cbe8c6b899552dadd907604559806aa2e9bd
    2015-12-24T17:46:52.517-0200 I CONTROL  [initandlisten] build info: windows sys.getwindowsversion(major=6, minor=1, build=7601, platform=2, service_pack='Service Pack 1') BOOST_LIB_VERSION=1_49
    2015-12-24T17:46:52.517-0200 I CONTROL  [initandlisten] allocator: tcmalloc
    2015-12-24T17:46:52.518-0200 I CONTROL  [initandlisten] options: { net: { port:30000 }, replication: { replSet: "replica_set" }, storage: { dbPath: "data/arb"} }
    2015-12-24T17:46:52.521-0200 I INDEX    [initandlisten] allocating new ns file data/arb\local.ns, filling with zeroes...
    2015-12-24T17:46:53.099-0200 I STORAGE  [FileAllocator] allocating new datafiledata/arb\local.0, filling with zeroes...
    2015-12-24T17:46:53.100-0200 I STORAGE  [FileAllocator] creating directory data/arb\_tmp
    2015-12-24T17:46:53.110-0200 I STORAGE  [FileAllocator] done allocating datafile data/arb\local.0, size: 64MB,  took 0.007 secs
    2015-12-24T17:46:53.131-0200 I REPL     [initandlisten] Did not find local replica set configuration document at startup;  NoMatchingDocument Did not find replica set configuration document in local.system.replset
    2015-12-24T17:46:53.132-0200 I NETWORK  [initandlisten] waiting for connectionson port 30000
```

##### Adicione o árbitro criado com `rs.addArb()` na replica `PRIMARY`
```js
    replica_set:PRIMARY> rs.addArb("127.0.0.1:30000")
    { "ok" : 1 }
    replica_set:PRIMARY>
```

##### Status do `replicaSet`
```js
    replica_set:PRIMARY> rs.status()
    {
            "set" : "replica_set",
            "date" : ISODate("2015-12-24T19:52:27.935Z"),
            "myState" : 1,
            "members" : [
                    {
                            "_id" : 0,
                            "name" : "127.0.0.1:27017",
                            "health" : 1,
                            "state" : 1,
                            "stateStr" : "PRIMARY",
                            "uptime" : 2793,
                            "optime" : Timestamp(1450986428, 1),
                            "optimeDate" : ISODate("2015-12-24T19:47:08Z"),
                            "electionTime" : Timestamp(1450985266, 2),
                            "electionDate" : ISODate("2015-12-24T19:27:46Z"),
                            "configVersion" : 4,
                            "self" : true
                    },
                    {
                            "_id" : 1,
                            "name" : "127.0.0.1:27018",
                            "health" : 1,
                            "state" : 2,
                            "stateStr" : "SECONDARY",
                            "uptime" : 1036,
                            "optime" : Timestamp(1450986428, 1),
                            "optimeDate" : ISODate("2015-12-24T19:47:08Z"),
                            "lastHeartbeat" : ISODate("2015-12-24T19:52:26.589Z"),
                            "lastHeartbeatRecv" : ISODate("2015-12-24T19:52:26.034Z"
    ),
                            "pingMs" : 0,
                            "configVersion" : 4
                    },
                    {
                            "_id" : 2,
                            "name" : "127.0.0.1:27019",
                            "health" : 1,
                            "state" : 2,
                            "stateStr" : "SECONDARY",
                            "uptime" : 1020,
                            "optime" : Timestamp(1450986428, 1),
                            "optimeDate" : ISODate("2015-12-24T19:47:08Z"),
                            "lastHeartbeat" : ISODate("2015-12-24T19:52:26.589Z"),
                            "lastHeartbeatRecv" : ISODate("2015-12-24T19:52:27.650Z"
    ),
                            "pingMs" : 0,
                            "configVersion" : 4
                    },
                    {
                            "_id" : 3,
                            "name" : "127.0.0.1:30000",
                            "health" : 1,
                            "state" : 7,
                            "stateStr" : "ARBITER",
                            "uptime" : 319,
                            "lastHeartbeat" : ISODate("2015-12-24T19:52:26.595Z"),
                            "lastHeartbeatRecv" : ISODate("2015-12-24T19:52:26.597Z"
    ),
                            "pingMs" : 0,
                            "configVersion" : 4
                    }
            ],
            "ok" : 1
    }
    replica_set:PRIMARY>
```

##### Status do `oplog`
```js
    replica_set:PRIMARY> rs.printReplicationInfo()
    configured oplog size:   9682.703063964844MB
    log length start to end: 1162secs (0.32hrs)
    oplog first event time:  Thu Dec 24 2015 17:27:46 GMT-0200 (Horário brasileiro de verão)
    oplog last event time:   Thu Dec 24 2015 17:47:08 GMT-0200 (Horário brasileiro de verão)
    now:                     Thu Dec 24 2015 17:52:57 GMT-0200 (Horário brasileiro de verão)
    replica_set:PRIMARY>
```