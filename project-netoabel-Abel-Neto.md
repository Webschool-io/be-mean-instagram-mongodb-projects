
# MongoDb - Projeto Final
**Autor:** Abel Neto

**Data:** 1462977026837

## Índice

##### [Introdução](#introduction)

* [Caso de uso onde seria interessante utilizar o MongoDB](#when)

##### [Modelagem](#modeling)

* [Algumas considerações](#considerations)

##### [Operações CRUD](#crud_operations)

* [Inserindo usuários](#inserting_users)
* [Inserindo projetos](#inserting_projects)
* [Listando dados dos membros de um projeto](#listing_members)
* [Listando projetos com uma tag específica](#listing_projects_by_tag)
* [Listando apenas os nomes de todas as atividades](#listing_activities_names)
* [Listando todos os projetos sem tags](#listing_projects_no_tags)
* [Listando todos os usuários que não fazem parte do primeiro projeto](#listing_users_not_first_project)
* [Adicionando o campo views com valor 0 em todos os projetos](#adding_view)
* [Adicionando uma tag nova a cada projeto](#adding_new_tag)
* [Adicionando 2 novos membros para cada projeto](#adding_2_members)
* [Adicionando 1 comentário em cada atividade (exceto nas atividades 7 e 8](#adding_comment)
* [Adicionando um projeto com upsert](#adding_project_upsert)
* [Removendo todos os projetos que não possuem tags](#removing_projects_no_tags)
* [Removendo todos os projetos que não possuem comentários em suas atividades](#removing_projects_no_comments)
* [Removendo todos os projetos que não possuem atividades](#removing_projects_no_activities)
* [Removendo todos os projetos dos quais dois usuários específicos fazem parte](#removing_projects_specific_members)
* [Removendo todos os projetos com uma tag específica em goals](#removing_projects_specific_tag)

##### [Controle de acesso](#access_control)

* [Criando um usuário com permisões de leitura apenas](#creating_user_read_only)
* [Criando um usuário com permissões de escrita e leitura](#create_user_read_write)
* [Habilitando as ações grantRole e revokeRole em um usuário através do papel userAdmin](#granting_useradmin)
* [Removendo o papel userAdmin](#revoking_useradmin)
* [Listando todos os usuários e seus papéis](#listing_all_users)

##### [Sharding e replica set](#sharding)

* [Iniciando um config server](#starting_config_server)
* [Iniciando um router](#starting_router)
* [Iniciando os shards](#starting_shards)
* [Registrando os shards](#registering_shards)
* [Habilitando sharding para a collection activities na database be-bean-project](#enabling_sharding)
* [Criando uma réplica para cada shard](#creating_replicaset)

# <a name="introduction"></a>Introdução

## <a name="when"></a>Caso de uso onde seria interessante utilizar o MongoDB

MongoDB é um banco de dados NoSQL, o que significa que ele não segue a implementação mais tradicional e conhecida de bancos de dados em geral, a relacional. Isso pode ser bom ou ruim, a depender do cenário onde ele for utilizado. 

Um caso onde eu o utilizaria (e de fato utilizo) é o meu TCC, que é um projeto de automação residencial, onde eu armazeno informações sobre atualizações de estado dos dispositivos em uma base de dados. Como pode haver um alto volume de dados e operações de escrita e não há a necessidade de garantir transações atômicas, por exemplo, o MongoDB se mostrou a melhor alternativa para o caso. 

Além disso, diferentes dispositivos possuem diferentes modelagens no banco. Modelar esse tipo de estrutura é muito mais fácil no MongoDB. Incluir novas estruturas e tipos de dispositivos não é um problema, por exemplo.

# <a name="modeling"></a>Modelagem

Vejamos como ficaria a modelagem relacional a seguir no MongoDB: 

![](https://raw.githubusercontent.com/netoabel/be-mean-instagram-mongodb-projects/master/modeling/relational.jpg)

### Collection users

```js
{
    name,
    bio,
    register_date,
    auth: {
        username,
        email,
        password,
        last_access,
        online,
        disabled,
        hash_token
    },
    settings: { 
        system: {
            background_path
        }
    }
}
```

### Collection projects

```js
{
    name,
    description,
    date_begin,
    date_dream,
    date_end,
    realocate,
    visible,
    expired,
    visualizable_mod,
    members: [
        { 
            user_id
,            member_type,
            notify
        }
    ],
    tags: [],
    goals: [
        {
            name,
            description,
            date_begin,
            date_dream,
            date_end,
            realocate,
            expired,
            tags: [],
            historic: [
                { date_realocate }
            ],
            activities: [
                { activity_id }
            ]
        }
    ]
}
```

## Collection activites

```js
{ 
    name,
    description,
    date_begin,
    date_dream,
    date_end,
    realocate,
    expired,
    tags: [],
    historic: [
        { date_realocate }
    ], 
    members: [
        { 
            user_id,
            member_type,
            notify
        }
    ],
    comments: [
        {
            text,
            date,
            files: [
                { path, weight, name }
            ],
            members: [
                { 
                    user_id,
                    member_type, 
                    notify 
                }
            ]
        }
    ]
}
```

## <a name="considerations"></a>Algumas considerações

Poderíamos ter criado apenas uma collection `projects` com todos os documentos de `activities` dentro dela, como arrays. Porém, o MongoDB estabelece um limite de 16MB por documento armazenado. No nosso cenário, a probabilidade de a quantidade de atividades crescer consideravelmente é muito grande. Isso poderia facilmente levar um documento da collection `projects` a exceder o limite de tamanho estabelecido pelo Mongo.

Ao criar uma collection `activities` separada, fazemos com que cada atividade seja armazenada como um novo documento. Além de evitar a extrapolação do limite, esse formato evita que um único documento use uma quantidade excessiva de memória RAM ou consuma uma largura de banda muito grande durante uma transmissão.

No caso dos comentários, como é muito pouco provável que a quantidade de comentários em uma única atividade cresça a ponto de fazer com que um documento da collection `activities` se torne muito grande, optamos por mantê-los em um array interno a essa collection, a fim de tornar o acesso a eles mais fácil e performático.

# <a name="crud_operations"></a> Operações CRUD

## <a name="inserting_users"></a>Inserindo usuários

```js
be-mean-project> var users = [];
be-mean-project> 
... for (var i = 0; i < 10; i++) {
...     var user = {
...         name: "User " + i,
...         bio: "Once upon a time...",
...         register_date: new Date(), 
...         auth: {
...             username: "user_" + i,
...             email: "user_" + i + "@gmail.com",
...             password: "password",
...             last_access: new Date(),
...             online: true,
...             disabled: false,
...             hash_token: "572f78aed7193a654cecb74b",
...         },
...         settings: { 
...             system: {
...                 background_path: "images/user_" + i + "_bg.png"
...             }
...         },
...     };
...     users.push(user);
... }
10
be-mean-project> db.users.save(users);
Inserted 1 record(s) in 355ms
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

## <a name="inserting_projects"></a>Inserindo projetos

Primeiro, criaremos as atividades que serão atribuídas aos projetos:

```js
be-mean-project> var activity = { 
...     name: "Activity 1",
...     description: "Description",
...     date_begin: new Date(),
...     date_dream: new Date(),
...     date_end: new Date(),
...     realocate: new Date(),
...     expired: false,
...     tags: ["activity"],
...     historic: [
...         { date_realocate: new Date() }
...     ], 
...     members: [
...         { 
...             user_id: ObjectId("5734c206a551a48f44c48321"),
...             member_type: "type1",
...             notify: true
...         }
...     ],
...     comments: [
...         {
...             text: "Comment!",
...             date: new Date(),
...             files: [
...                 { 
...                     path: "files/file",
...                     weight: 10, 
...                     name: "file" 
...                 }
...             ],
...             members: [
...                 { 
...                     user_id: ObjectId("5734c206a551a48f44c48321"),
...                     member_type: "type1", 
...                     notify: true 
...                 }
...             ]
...         }
...     ]
... };
be-mean-project> db.activities.save(activity);
Inserted 1 record(s) in 3ms
WriteResult({
  "nInserted": 1
})
```

Seguindo o mesmo processo, foram inseridas 8 atividades:

```js
be-mean-project> db.activities.count()
8
```

Agora, inserimos os projetos:

```js
var goal_id = new ObjectId();
be-mean-project> var project = {
...     name: "Project 1",
...     description: "Description",
...     date_begin: new Date(),
...     date_dream: new Date(),
...     date_end: new Date(),
...     realocate: new Date(),
...     visible: true,
...     expired: false,
...     visualizable_mod: true,
...     members: [
...         { 
...             user_id: ObjectId("5734c206a551a48f44c48321"),
...             member_type: "type 1",
...             notify: true
...         },
...         { 
...             user_id: ObjectId("5734c206a551a48f44c48322"),
...             member_type: "type 1",
...             notify: true
...         },
...         { 
...             user_id: ObjectId("5734c206a551a48f44c48323"),
...             member_type: "type 1",
...             notify: true
...         },
...         { 
...             user_id: ObjectId("5734c206a551a48f44c48324"),
...             member_type: "type 1",
...             notify: true
...         },
...         { 
...             user_id: ObjectId("5734c206a551a48f44c48325"),
...             member_type: "type 1",
...             notify: true
...         }
...     ],
...     tags: ['tag1', 'tag2', 'tag3'],
...     goals: [
...         {   
...             _id: goal_id,
...             name: "Goal",
...             description: "Description",
...             date_begin: new Date(),
...             date_dream: new Date(),
...             date_end: new Date(),
...             realocate: new Date(),
...             expired: false,
...             tags: ['tag1', 'tag2', 'tag3'],
...             historic: [
...                 { date_realocate: new Date() }
...             ],
...             activities: [
...                 { activity_id: ObjectId("5734d192a551a48f44c4832b") },
...                 { activity_id: ObjectId("5734d287a551a48f44c4832d") }
...             ]
...         }
...     ]
... };
be-mean-project> db.projects.save(project);
Inserted 1 record(s) in 4ms
WriteResult({
  "nInserted": 1
})

be-mean-project> var query = { $or: [{ _id: ObjectId("5734d192a551a48f44c4832b")}, { _id: ObjectId("5734d287a551a48f44c4832d") }] }
be-mean-project> var modifier = { $set: { goal_id: goal_id } }
be-mean-project> var options = { multi: true } 
be-mean-project> db.activities.update(query, modifier, options)
Updated 2 existing record(s) in 2ms
WriteResult({
  "nMatched": 2,
  "nUpserted": 0,
  "nModified": 2
})

var goal_id = new ObjectId();
be-mean-project> var project = {
...     name: "Project 2",
...     description: "Description",
...     date_begin: new Date(),
...     date_dream: new Date(),
...     date_end: new Date(),
...     realocate: new Date(),
...     visible: true,
...     expired: false,
...     visualizable_mod: true,
...     members: [
...         { 
...             user_id: ObjectId("5734c206a551a48f44c48322"),
...             member_type: "type 1",
...             notify: true
...         },
...         { 
...             user_id: ObjectId("5734c206a551a48f44c48323"),
...             member_type: "type 1",
...             notify: true
...         },
...         { 
...             user_id: ObjectId("5734c206a551a48f44c48324"),
...             member_type: "type 1",
...             notify: true
...         },
...         { 
...             user_id: ObjectId("5734c206a551a48f44c48325"),
...             member_type: "type 1",
...             notify: true
...         },
...         { 
...             user_id: ObjectId("5734c206a551a48f44c48326"),
...             member_type: "type 1",
...             notify: true
...         }
...     ],
...     tags: ['tag2', 'tag3', 'tag4'],
...     goals: [
...         {
...             _id: goal_id,
...             name: "Goal",
...             description: "Description",
...             date_begin: new Date(),
...             date_dream: new Date(),
...             date_end: new Date(),
...             realocate: new Date(),
...             expired: false,
...             tags: ['tag1', 'tag2', 'tag3'],
...             historic: [
...                 { date_realocate: new Date() }
...             ],
...             activities: [
...                 { activity_id: ObjectId("5734d2a0a551a48f44c4832e") },
...                 { activity_id: ObjectId("5734d2bda551a48f44c4832f") }
...             ]
...         }
...     ]
... };
be-mean-project> db.projects.save(project);
Inserted 1 record(s) in 3ms
WriteResult({
  "nInserted": 1
})

be-mean-project> var query = { $or: [{ _id: ObjectId("5734d2a0a551a48f44c4832e")}, { _id: ObjectId("5734d2bda551a48f44c4832f") }] }
be-mean-project> var modifier = { $set: { goal_id: goal_id } }
be-mean-project> var options = { multi: true }
be-mean-project> db.activities.update(query, modifier, options)
Updated 2 existing record(s) in 2ms
WriteResult({
  "nMatched": 2,
  "nUpserted": 0,
  "nModified": 2
})

var goal_id = new ObjectId();
be-mean-project> var project = {
...     name: "Project 3",
...     description: "Description",
...     date_begin: new Date(),
...     date_dream: new Date(),
...     date_end: new Date(),
...     realocate: new Date(),
...     visible: true,
...     expired: false,
...     visualizable_mod: true,
...     members: [
...         { 
...             user_id: ObjectId("5734c206a551a48f44c48323"),
...             member_type: "type 1",
...             notify: true
...         },
...         { 
...             user_id: ObjectId("5734c206a551a48f44c48324"),
...             member_type: "type 1",
...             notify: true
...         },
...         { 
...             user_id: ObjectId("5734c206a551a48f44c48325"),
...             member_type: "type 1",
...             notify: true
...         },
...         { 
...             user_id: ObjectId("5734c206a551a48f44c48326"),
...             member_type: "type 1",
...             notify: true
...         },
...         { 
...             user_id: ObjectId("5734c206a551a48f44c48327"),
...             member_type: "type 1",
...             notify: true
...         }
...     ],
...     tags: ['tag3', 'tag4', 'tag5'],
...     goals: [
...         {
...             _id: goal_id,
...             name: "Goal",
...             description: "Description",
...             date_begin: new Date(),
...             date_dream: new Date(),
...             date_end: new Date(),
...             realocate: new Date(),
...             expired: false,
...             tags: ['tag1', 'tag2', 'tag3'],
...             historic: [
...                 { date_realocate: new Date() }
...             ],
...             activities: [
...                 { activity_id: ObjectId("5734d2daa551a48f44c48330") },
...                 { activity_id: ObjectId("5734d2ffa551a48f44c48331") }
...             ]
...         }
...     ]
... };
be-mean-project> db.projects.save(project);
Inserted 1 record(s) in 3ms
WriteResult({
  "nInserted": 1
})

be-mean-project> var query = { $or: [{ _id: ObjectId("5734d2daa551a48f44c48330")}, { _id: ObjectId("5734d2ffa551a48f44c48331") }] }
be-mean-project> var modifier = { $set: { goal_id: goal_id } }
be-mean-project> var options = { multi: true }
be-mean-project> db.activities.update(query, modifier, options)
Updated 2 existing record(s) in 2ms
WriteResult({
  "nMatched": 2,
  "nUpserted": 0,
  "nModified": 2
})

var goal_id = new ObjectId();
be-mean-project> var project = {
...     name: "Project 4",
...     description: "Description",
...     date_begin: new Date(),
...     date_dream: new Date(),
...     date_end: new Date(),
...     realocate: new Date(),
...     visible: true,
...     expired: false,
...     visualizable_mod: true,
...     members: [
...         { 
...             user_id: ObjectId("5734c206a551a48f44c48324"),
...             member_type: "type 1",
...             notify: true
...         },
...         { 
...             user_id: ObjectId("5734c206a551a48f44c48325"),
...             member_type: "type 1",
...             notify: true
...         },
...         { 
...             user_id: ObjectId("5734c206a551a48f44c48326"),
...             member_type: "type 1",
...             notify: true
...         },
...         { 
...             user_id: ObjectId("5734c206a551a48f44c48327"),
...             member_type: "type 1",
...             notify: true
...         },
...         { 
...             user_id: ObjectId("5734c206a551a48f44c48328"),
...             member_type: "type 1",
...             notify: true
...         }
...     ],
...     tags: ['tag4', 'tag5', 'tag6'],
...     goals: [
...         {
...             _id: goal_id,
...             name: "Goal",
...             description: "Description",
...             date_begin: new Date(),
...             date_dream: new Date(),
...             date_end: new Date(),
...             realocate: new Date(),
...             expired: false,
...             tags: ['tag1', 'tag2', 'tag3'],
...             historic: [
...                 { date_realocate: new Date() }
...             ],
...             activities: [
...                 { activity_id: ObjectId("5734d319a551a48f44c48332") },
...                 { activity_id: ObjectId("5734d332a551a48f44c48333") }
...             ]
...         }
...     ]
... };
be-mean-project> db.projects.save(project);
Inserted 1 record(s) in 2ms
WriteResult({
  "nInserted": 1
})

be-mean-project> var query = { $or: [{ _id: ObjectId("5734d319a551a48f44c48332")}, { _id: ObjectId("5734d332a551a48f44c48333") }] }
be-mean-project> var modifier = { $set: { goal_id: goal_id } }
be-mean-project> var options = { multi: true }
be-mean-project> db.activities.update(query, modifier, options)
Updated 2 existing record(s) in 2ms
WriteResult({
  "nMatched": 2,
  "nUpserted": 0,
  "nModified": 2
})

var goal_id = new ObjectId();
be-mean-project> var project = {
...     name: "Project 5",
...     description: "Description",
...     date_begin: new Date(),
...     date_dream: new Date(),
...     date_end: new Date(),
...     realocate: new Date(),
...     visible: true,
...     expired: false,
...     visualizable_mod: true,
...     members: [
...         { 
...             user_id: ObjectId("5734c206a551a48f44c48325"),
...             member_type: "type 1",
...             notify: true
...         },
...         { 
...             user_id: ObjectId("5734c206a551a48f44c48326"),
...             member_type: "type 1",
...             notify: true
...         },
...         { 
...             user_id: ObjectId("5734c206a551a48f44c48327"),
...             member_type: "type 1",
...             notify: true
...         },
...         { 
...             user_id: ObjectId("5734c206a551a48f44c48328"),
...             member_type: "type 1",
...             notify: true
...         },
...         { 
...             user_id: ObjectId("5734c206a551a48f44c48329"),
...             member_type: "type 1",
...             notify: true
...         }
...     ],
...     tags: ['tag5', 'tag6', 'tag7'],
...     goals: [
...         {
...             _id: goal_id,
...             name: "Goal",
...             description: "Description",
...             date_begin: new Date(),
...             date_dream: new Date(),
...             date_end: new Date(),
...             realocate: new Date(),
...             expired: false,
...             tags: ['tag1', 'tag2', 'tag3'],
...             historic: [
...                 { date_realocate: new Date() }
...             ]
...         }
...     ]
... };
be-mean-project> db.projects.save(project);
Inserted 1 record(s) in 3ms
WriteResult({
  "nInserted": 1
})
```

## <a name="listing_members"></a>Listando dados dos membros de um projeto

```js
be-mean-project> var members = [];
be-mean-project> var getMemberData = function (member) {
...     var memberData = db.users.findOne({ _id: member.user_id });
...     memberData.member_type = member.member_type;
...     memberData.notify = member.notify;
...     members.push(memberData);
... };
be-mean-project> var project = db.projects.findOne({ name: /project 1/i }, { members: 1 });
be-mean-project> project.members.forEach(getMemberData);
be-mean-project> members
[
  {
    "_id": ObjectId("5734c206a551a48f44c48321"),
    "name": "User 0",
    "bio": "Once upon a time...",
    "register_date": ISODate("2016-05-12T17:48:42.462Z"),
    "auth": {
      "username": "user_0",
      "email": "user_0@gmail.com",
      "password": "password",
      "last_access": ISODate("2016-05-12T17:48:42.462Z"),
      "online": true,
      "disabled": false,
      "hash_token": "572f78aed7193a654cecb74b"
    },
    "settings": {
      "system": {
        "background_path": "images/user_0_bg.png"
      }
    },
    "member_type": "type 1",
    "notify": true
  },
  {
    "_id": ObjectId("5734c206a551a48f44c48322"),
    "name": "User 1",
    "bio": "Once upon a time...",
    "register_date": ISODate("2016-05-12T17:48:42.462Z"),
    "auth": {
      "username": "user_1",
      "email": "user_1@gmail.com",
      "password": "password",
      "last_access": ISODate("2016-05-12T17:48:42.462Z"),
      "online": true,
      "disabled": false,
      "hash_token": "572f78aed7193a654cecb74b"
    },
    "settings": {
      "system": {
        "background_path": "images/user_1_bg.png"
      }
    },
    "member_type": "type 1",
    "notify": true
  },
  {
    "_id": ObjectId("5734c206a551a48f44c48323"),
    "name": "User 2",
    "bio": "Once upon a time...",
    "register_date": ISODate("2016-05-12T17:48:42.462Z"),
    "auth": {
      "username": "user_2",
      "email": "user_2@gmail.com",
      "password": "password",
      "last_access": ISODate("2016-05-12T17:48:42.462Z"),
      "online": true,
      "disabled": false,
      "hash_token": "572f78aed7193a654cecb74b"
    },
    "settings": {
      "system": {
        "background_path": "images/user_2_bg.png"
      }
    },
    "member_type": "type 1",
    "notify": true
  },
  {
    "_id": ObjectId("5734c206a551a48f44c48324"),
    "name": "User 3",
    "bio": "Once upon a time...",
    "register_date": ISODate("2016-05-12T17:48:42.462Z"),
    "auth": {
      "username": "user_3",
      "email": "user_3@gmail.com",
      "password": "password",
      "last_access": ISODate("2016-05-12T17:48:42.462Z"),
      "online": true,
      "disabled": false,
      "hash_token": "572f78aed7193a654cecb74b"
    },
    "settings": {
      "system": {
        "background_path": "images/user_3_bg.png"
      }
    },
    "member_type": "type 1",
    "notify": true
  },
  {
    "_id": ObjectId("5734c206a551a48f44c48325"),
    "name": "User 4",
    "bio": "Once upon a time...",
    "register_date": ISODate("2016-05-12T17:48:42.462Z"),
    "auth": {
      "username": "user_4",
      "email": "user_4@gmail.com",
      "password": "password",
      "last_access": ISODate("2016-05-12T17:48:42.462Z"),
      "online": true,
      "disabled": false,
      "hash_token": "572f78aed7193a654cecb74b"
    },
    "settings": {
      "system": {
        "background_path": "images/user_4_bg.png"
      }
    },
    "member_type": "type 1",
    "notify": true
  }
]
```

## <a name="listing_projects_by_tag"></a>Listando projetos com uma tag específica

```js
be-mean-project> db.projects.find({ tags: 'tag3' }); 
{
  "_id": ObjectId("5734d997a551a48f44c48334"),
  "name": "Project 1",
  "description": "Description",
  "date_begin": ISODate("2016-05-12T19:29:27.728Z"),
  "date_dream": ISODate("2016-05-12T19:29:27.728Z"),
  "date_end": ISODate("2016-05-12T19:29:27.728Z"),
  "realocate": ISODate("2016-05-12T19:29:27.728Z"),
  "visible": true,
  "expired": false,
  "visualizable_mod": true,
  "members": [
    {
      "user_id": ObjectId("5734c206a551a48f44c48321"),
      "member_type": "type 1",
      "notify": true
    },
    {
      "user_id": ObjectId("5734c206a551a48f44c48322"),
      "member_type": "type 1",
      "notify": true
    },
    {
      "user_id": ObjectId("5734c206a551a48f44c48323"),
      "member_type": "type 1",
      "notify": true
    },
    {
      "user_id": ObjectId("5734c206a551a48f44c48324"),
      "member_type": "type 1",
      "notify": true
    },
    {
      "user_id": ObjectId("5734c206a551a48f44c48325"),
      "member_type": "type 1",
      "notify": true
    }
  ],
  "tags": [
    "tag1",
    "tag2",
    "tag3"
  ],
  "goals": [
    {
      "name": "Goal",
      "description": "Description",
      "date_begin": ISODate("2016-05-12T19:29:27.728Z"),
      "date_dream": ISODate("2016-05-12T19:29:27.728Z"),
      "date_end": ISODate("2016-05-12T19:29:27.728Z"),
      "realocate": ISODate("2016-05-12T19:29:27.728Z"),
      "expired": false,
      "tags": [
        "tag1",
        "tag2",
        "tag3"
      ],
      "historic": [
        {
          "date_realocate": ISODate("2016-05-12T19:29:27.728Z")
        }
      ],
      "activities": [
        {
          "activity_id": ObjectId("5734d192a551a48f44c4832b")
        },
        {
          "activity_id": ObjectId("5734d287a551a48f44c4832d")
        }
      ]
    }
  ]
}
{
  "_id": ObjectId("5734da0ea551a48f44c48335"),
  "name": "Project 2",
  "description": "Description",
  "date_begin": ISODate("2016-05-12T19:31:26.721Z"),
  "date_dream": ISODate("2016-05-12T19:31:26.721Z"),
  "date_end": ISODate("2016-05-12T19:31:26.721Z"),
  "realocate": ISODate("2016-05-12T19:31:26.721Z"),
  "visible": true,
  "expired": false,
  "visualizable_mod": true,
  "members": [
    {
      "user_id": ObjectId("5734c206a551a48f44c48322"),
      "member_type": "type 1",
      "notify": true
    },
    {
      "user_id": ObjectId("5734c206a551a48f44c48323"),
      "member_type": "type 1",
      "notify": true
    },
    {
      "user_id": ObjectId("5734c206a551a48f44c48324"),
      "member_type": "type 1",
      "notify": true
    },
    {
      "user_id": ObjectId("5734c206a551a48f44c48325"),
      "member_type": "type 1",
      "notify": true
    },
    {
      "user_id": ObjectId("5734c206a551a48f44c48326"),
      "member_type": "type 1",
      "notify": true
    }
  ],
  "tags": [
    "tag2",
    "tag3",
    "tag4"
  ],
  "goals": [
    {
      "name": "Goal",
      "description": "Description",
      "date_begin": ISODate("2016-05-12T19:31:26.721Z"),
      "date_dream": ISODate("2016-05-12T19:31:26.721Z"),
      "date_end": ISODate("2016-05-12T19:31:26.721Z"),
      "realocate": ISODate("2016-05-12T19:31:26.721Z"),
      "expired": false,
      "tags": [
        "tag1",
        "tag2",
        "tag3"
      ],
      "historic": [
        {
          "date_realocate": ISODate("2016-05-12T19:31:26.721Z")
        }
      ],
      "activities": [
        {
          "activity_id": ObjectId("5734d2a0a551a48f44c4832e")
        },
        {
          "activity_id": ObjectId("5734d2bda551a48f44c4832f")
        }
      ]
    }
  ]
}
{
  "_id": ObjectId("5734da8ca551a48f44c48336"),
  "name": "Project 3",
  "description": "Description",
  "date_begin": ISODate("2016-05-12T19:33:32.624Z"),
  "date_dream": ISODate("2016-05-12T19:33:32.624Z"),
  "date_end": ISODate("2016-05-12T19:33:32.624Z"),
  "realocate": ISODate("2016-05-12T19:33:32.624Z"),
  "visible": true,
  "expired": false,
  "visualizable_mod": true,
  "members": [
    {
      "user_id": ObjectId("5734c206a551a48f44c48323"),
      "member_type": "type 1",
      "notify": true
    },
    {
      "user_id": ObjectId("5734c206a551a48f44c48324"),
      "member_type": "type 1",
      "notify": true
    },
    {
      "user_id": ObjectId("5734c206a551a48f44c48325"),
      "member_type": "type 1",
      "notify": true
    },
    {
      "user_id": ObjectId("5734c206a551a48f44c48326"),
      "member_type": "type 1",
      "notify": true
    },
    {
      "user_id": ObjectId("5734c206a551a48f44c48327"),
      "member_type": "type 1",
      "notify": true
    }
  ],
  "tags": [
    "tag3",
    "tag4",
    "tag5"
  ],
  "goals": [
    {
      "name": "Goal",
      "description": "Description",
      "date_begin": ISODate("2016-05-12T19:33:32.624Z"),
      "date_dream": ISODate("2016-05-12T19:33:32.624Z"),
      "date_end": ISODate("2016-05-12T19:33:32.624Z"),
      "realocate": ISODate("2016-05-12T19:33:32.624Z"),
      "expired": false,
      "tags": [
        "tag1",
        "tag2",
        "tag3"
      ],
      "historic": [
        {
          "date_realocate": ISODate("2016-05-12T19:33:32.624Z")
        }
      ],
      "activities": [
        {
          "activity_id": ObjectId("5734d2daa551a48f44c48330")
        },
        {
          "activity_id": ObjectId("5734d2ffa551a48f44c48331")
        }
      ]
    }
  ]
}
Fetched 3 record(s) in 7ms
```

## <a name="listing_activities_names"></a>Listando apenas os nomes de todas as atividades

```js
be-mean-project> db.activities.find({}, { _id: 0, name: 1 })
{
  "name": "Activity 1"
}
{
  "name": "Activity 2"
}
{
  "name": "Activity 3"
}
{
  "name": "Activity 4"
}
{
  "name": "Activity 5"
}
{
  "name": "Activity 6"
}
{
  "name": "Activity 7"
}
{
  "name": "Activity 8"
}
Fetched 8 record(s) in 3ms
```

## <a name="listing_projects_no_tags"></a>Listando todos os projetos sem tags

```js
be-mean-project> db.projects.find({ tags: { $exists: false } })
Fetched 0 record(s) in 1ms
```

P.s.: a query não retorna nada porque não temos nenhum projeto sem tags. :P

## <a name="listing_users_not_first_project"></a>Listando todos os usuários que não fazem parte do primeiro projeto

```js
be-mean-project> var projectMembers = [];
be-mean-project> var project = db.projects.findOne({}, { members: 1 });
be-mean-project> project.members.forEach(function(member) {
...     projectMembers.push(member.user_id);
... });
be-mean-project> db.users.find({ _id: { $not: { $in: projectMembers } } });
{
  "_id": ObjectId("5734c206a551a48f44c48326"),
  "name": "User 5",
  "bio": "Once upon a time...",
  "register_date": ISODate("2016-05-12T17:48:42.462Z"),
  "auth": {
    "username": "user_5",
    "email": "user_5@gmail.com",
    "password": "password",
    "last_access": ISODate("2016-05-12T17:48:42.462Z"),
    "online": true,
    "disabled": false,
    "hash_token": "572f78aed7193a654cecb74b"
  },
  "settings": {
    "system": {
      "background_path": "images/user_5_bg.png"
    }
  }
}
{
  "_id": ObjectId("5734c206a551a48f44c48327"),
  "name": "User 6",
  "bio": "Once upon a time...",
  "register_date": ISODate("2016-05-12T17:48:42.462Z"),
  "auth": {
    "username": "user_6",
    "email": "user_6@gmail.com",
    "password": "password",
    "last_access": ISODate("2016-05-12T17:48:42.462Z"),
    "online": true,
    "disabled": false,
    "hash_token": "572f78aed7193a654cecb74b"
  },
  "settings": {
    "system": {
      "background_path": "images/user_6_bg.png"
    }
  }
}
{
  "_id": ObjectId("5734c206a551a48f44c48328"),
  "name": "User 7",
  "bio": "Once upon a time...",
  "register_date": ISODate("2016-05-12T17:48:42.462Z"),
  "auth": {
    "username": "user_7",
    "email": "user_7@gmail.com",
    "password": "password",
    "last_access": ISODate("2016-05-12T17:48:42.462Z"),
    "online": true,
    "disabled": false,
    "hash_token": "572f78aed7193a654cecb74b"
  },
  "settings": {
    "system": {
      "background_path": "images/user_7_bg.png"
    }
  }
}
{
  "_id": ObjectId("5734c206a551a48f44c48329"),
  "name": "User 8",
  "bio": "Once upon a time...",
  "register_date": ISODate("2016-05-12T17:48:42.462Z"),
  "auth": {
    "username": "user_8",
    "email": "user_8@gmail.com",
    "password": "password",
    "last_access": ISODate("2016-05-12T17:48:42.462Z"),
    "online": true,
    "disabled": false,
    "hash_token": "572f78aed7193a654cecb74b"
  },
  "settings": {
    "system": {
      "background_path": "images/user_8_bg.png"
    }
  }
}
{
  "_id": ObjectId("5734c206a551a48f44c4832a"),
  "name": "User 9",
  "bio": "Once upon a time...",
  "register_date": ISODate("2016-05-12T17:48:42.462Z"),
  "auth": {
    "username": "user_9",
    "email": "user_9@gmail.com",
    "password": "password",
    "last_access": ISODate("2016-05-12T17:48:42.462Z"),
    "online": true,
    "disabled": false,
    "hash_token": "572f78aed7193a654cecb74b"
  },
  "settings": {
    "system": {
      "background_path": "images/user_9_bg.png"
    }
  }
}
Fetched 5 record(s) in 6ms
```

## <a name="adding_view"></a>Adicionando o campo views com valor 0 em todos os projetos

```js
be-mean-project> var query = {}
be-mean-project> var modifier = { $set: { views: 0 } }
be-mean-project> var options = { multi: true }
be-mean-project> db.projects.update(query, modifier, options)
Updated 5 existing record(s) in 3ms
WriteResult({
  "nMatched": 5,
  "nUpserted": 0,
  "nModified": 5
})
```

## <a name="adding_new_tag"></a>Adicionando uma tag nova a cada projeto

```js
be-mean-project> var query = { name: /project 1/i }
be-mean-project> var modifier = { $push: { tags:  'tag10' } }
be-mean-project> db.projects.update(query, modifier)
Updated 1 existing record(s) in 24ms
WriteResult({
  "nMatched": 1,
  "nUpserted": 0,
  "nModified": 1
})

be-mean-project> var query = { name: /project 2/i }
be-mean-project> var modifier = { $push: { tags:  'tag11' } }
be-mean-project> db.projects.update(query, modifier)
Updated 1 existing record(s) in 3ms
WriteResult({
  "nMatched": 1,
  "nUpserted": 0,
  "nModified": 1
})

be-mean-project> var query = { name: /project 3/i }
be-mean-project> var modifier = { $push: { tags:  'tag12' } }
be-mean-project> db.projects.update(query, modifier)
Updated 1 existing record(s) in 4ms
WriteResult({
  "nMatched": 1,
  "nUpserted": 0,
  "nModified": 1
})

be-mean-project> var query = { name: /project 4/i }
be-mean-project> var modifier = { $push: { tags:  'tag13' } }
be-mean-project> db.projects.update(query, modifier)
Updated 1 existing record(s) in 3ms
WriteResult({
  "nMatched": 1,
  "nUpserted": 0,
  "nModified": 1
})

be-mean-project> var query = { name: /project 5/i }
be-mean-project> var modifier = { $push: { tags:  'tag14' } }
be-mean-project> db.projects.update(query, modifier)
Updated 1 existing record(s) in 2ms
WriteResult({
  "nMatched": 1,
  "nUpserted": 0,
  "nModified": 1
})
```

## <a name="adding_2_members"></a>Adicionando 2 novos membros para cada projeto

```js
be-mean-project> var modifier = { 
...     $push: { 
...         members:  {
...             $each: [{
...                 user_id: ObjectId("5734c206a551a48f44c48326"),
...                 member_type: "type 1",
...                 notify: true
...             },{
...                 user_id: ObjectId("5734c206a551a48f44c48327"),
...                 member_type: "type 1",
...                 notify: true
...             }]  
...         } 
...     } 
... }
be-mean-project> db.projects.update(query, modifier)
Updated 1 existing record(s) in 3ms
WriteResult({
  "nMatched": 1,
  "nUpserted": 0,
  "nModified": 1
})

be-mean-project> var query = { name: /project 2/i }
be-mean-project> var modifier = { 
...     $push: { 
...         members:  {
...             $each: [{
...                 user_id: ObjectId("5734c206a551a48f44c48327"),
...                 member_type: "type 1",
...                 notify: true
...             },{
...                 user_id: ObjectId("5734c206a551a48f44c48328"),
...                 member_type: "type 1",
...                 notify: true
...             }]  
...         } 
...     } 
... }
be-mean-project> db.projects.update(query, modifier)
Updated 1 existing record(s) in 3ms
WriteResult({
  "nMatched": 1,
  "nUpserted": 0,
  "nModified": 1
})

be-mean-project> var query = { name: /project 3/i }
be-mean-project> var modifier = { 
...     $push: { 
...         members:  {
...             $each: [{
...                 user_id: ObjectId("5734c206a551a48f44c48328"),
...                 member_type: "type 1",
...                 notify: true
...             },{
...                 user_id: ObjectId("5734c206a551a48f44c48329"),
...                 member_type: "type 1",
...                 notify: true
...             }]  
...         } 
...     } 
... }
be-mean-project> db.projects.update(query, modifier)
Updated 1 existing record(s) in 1ms
WriteResult({
  "nMatched": 1,
  "nUpserted": 0,
  "nModified": 1
})

be-mean-project> var query = { name: /project 4/i }
be-mean-project> var modifier = { 
...     $push: { 
...         members:  {
...             $each: [{
...                 user_id: ObjectId("5734c206a551a48f44c48329"),
...                 member_type: "type 1",
...                 notify: true
...             },{
...                 user_id: ObjectId("5734c206a551a48f44c4832a"),
...                 member_type: "type 1",
...                 notify: true
...             }]  
...         } 
...     } 
... }
be-mean-project> db.projects.update(query, modifier)
Updated 1 existing record(s) in 3ms
WriteResult({
  "nMatched": 1,
  "nUpserted": 0,
  "nModified": 1
})

be-mean-project> var query = { name: /project 5/i }
be-mean-project> var modifier = { 
...     $push: { 
...         members:  {
...             $each: [{
...                 user_id: ObjectId("5734c206a551a48f44c4832a"),
...                 member_type: "type 1",
...                 notify: true
...             },{
...                 user_id: ObjectId("5734c206a551a48f44c48321"),
...                 member_type: "type 1",
...                 notify: true
...             }]  
...         } 
...     } 
... }
be-mean-project> db.projects.update(query, modifier)
Updated 1 existing record(s) in 5ms
WriteResult({
  "nMatched": 1,
  "nUpserted": 0,
  "nModified": 1
})
```

## <a name="adding_comment"></a>Adicionando 1 comentário em cada atividade (exceto nas atividades 7 e 8)

```js
be-mean-project> var query = { 
...     name: {
...         $not: {
...             $in: [/activity 7/i, /activity 8/i]
...         }
...     }
... }
be-mean-project> var modifier = { 
...     $push: { 
...         comments:  {
...                 text: "New comment",
...                 date: new Date()
...         }
...     } 
... }
be-mean-project> var options = {
...     multi: true
... }
be-mean-project> db.activities.update(query, modifier, options)
Updated 6 existing record(s) in 2ms
WriteResult({
  "nMatched": 6,
  "nUpserted": 0,
  "nModified": 6
})
```

## <a name="adding_project_upsert"></a>Adicionando um projeto com upsert

```js
be-mean-project> var query = { name: /project 6/i };
be-mean-project> var project = {
...     name: "Project 6",
...     description: "Description",
...     date_begin: new Date(),
...     date_dream: new Date(),
...     date_end: new Date(),
...     realocate: new Date(),
...     visible: true,
...     expired: false,
...     visualizable_mod: true,
...     members: [
...          { 
...             user_id: ObjectId("5734c206a551a48f44c48321"),
...             member_type: "type 1",
...             notify: true
...         }
...     ],
...     goals: [
...         {
...             _id: goal_id,
...             name: "Goal",
...             description: "Description",
...             date_begin: new Date(),
...             date_dream: new Date(),
...             date_end: new Date(),
...             realocate: new Date(),
...             expired: false,
...             tags: ['tag1', 'tag2', 'tag3'],
...             historic: [
...                 { date_realocate: new Date() }
...             ]
...         }
...     ]
... };
be-mean-project> var modifier = { $setOnInsert: project };
be-mean-project> var options = { upsert: true };
be-mean-project> db.projects.update(query, modifier, options);
Updated 1 new record(s) in 3ms
WriteResult({
  "nMatched": 0,
  "nUpserted": 1,
  "nModified": 0,
  "_id": ObjectId("573521f5594ac7cbdb4beba9")
})
```

## <a name="removing_projects_no_tags"></a>Removendo todos os projetos que não possuem tags

```js
be-mean-project> var query = { tags: { $exists: false } }
be-mean-project> db.projects.remove(query)
Removed 1 record(s) in 3ms
WriteResult({
  "nRemoved": 1
})
```

## <a name="removing_projects_no_comments"></a>Removendo todos os projetos que não possuem comentários em suas atividades

```js
be-mean-project> var activitiesQuery = {
...   comments: {
...     $exists: false
...   }
... };
be-mean-project> var activityIds = [];
be-mean-project> db.activities.find(query).forEach(function(activity) {
...   activityIds.push(activity._id);
... });
be-mean-project> var projectsQuery = {
...   goals: {
...     $not: {
...       $elemMatch: {
...         "activities.activity_id": {
...           $nin: activityIds
...         }
...       }
...     }
...   }
... };
be-mean-project> db.projects.remove(projectsQuery);
Removed 1 record(s) in 19ms
WriteResult({
  "nRemoved": 1
})
```

## <a name="removing_projects_no_activities"></a>Removendo todos os projetos que não possuem atividades

```js
be-mean-project> var query = {
...   goals: { $not: { $elemMatch: { activities: { $exists: true } } } }
... };
be-mean-project> db.projects.remove(query);
Removed 1 record(s) in 17ms
WriteResult({
  "nRemoved": 1
})
```

## <a name="removing_projects_specific_members"></a>Removendo todos os projetos dos quais dois usuários específicos fazem parte

```js
be-mean-project> var query = {
...   members: {
...     $elemMatch: {
...       user_id: {
...         $all: [
...           ObjectId("5734c206a551a48f44c48324"),
...           ObjectId("5734c206a551a48f44c48330")
...         ]
...       }
...     }
...   }
... }
be-mean-project> db.projects.remove(query);
Removed 1 record(s) in 1ms
WriteResult({
  "nRemoved": 1
})
```

## <a name="removing_projects_specific_tag"></a>Removendo todos os projetos com uma tag específica em goals

```js
var query = {
  goals: { $elemMatch: { tags: { $in: ["tag3"] } } }
}
db.projects.remove(query);
```

# <a name="access_control"></a>Controle de acesso

## <a name="creating_user_read_only"></a>Criando um usuário com permisões de leitura apenas

```js
be-mean-project> db.createUser({ user: 'db_user1', pwd: 'password', roles: ['read'] })
Successfully added user: {
  "user": "db_user1",
  "roles": [
    "read"
  ]
}
```

## <a name="create_user_read_write"></a>Criando um usuário com permissões de escrita e leitura

```js
be-mean-project> db.createUser({ user: 'db_user2', pwd: 'password', roles: ['readWrite'] })
Successfully added user: {
  "user": "db_user2",
  "roles": [
    "readWrite"
  ]
}
```

## <a name="granting_useradmin"></a>Habilitando as ações grantRole e revokeRole em um usuário através do papel userAdmin

```js
be-mean-project> db.grantRolesToUser("db_user2", ["userAdmin"])
```

## <a name="revoking_useradmin"></a>Removendo o papel userAdmin

```js
be-mean-project> db.revokeRolesFromUser("db_user2", ["userAdmin"])
```

## <a name="listing_all_users"></a>Listando todos os usuários e seus papéis

```js
be-mean-project> db.runCommand({ usersInfo: 1 }).users
[
  {
    "_id": "be-mean-project.db_user1",
    "user": "db_user1",
    "db": "be-mean-project",
    "roles": [
      {
        "role": "read",
        "db": "be-mean-project"
      }
    ]
  },
  {
    "_id": "be-mean-project.db_user2",
    "user": "db_user2",
    "db": "be-mean-project",
    "roles": [
      {
        "role": "readWrite",
        "db": "be-mean-project"
      }
    ]
  }
]
```

# <a name="sharding"></a>Sharding e replica set

## <a name="starting_config_server"></a>Iniciando um config server

```
$ sudo mkdir /data/configdb
$ sudo mongod --configsvr --port 27019
...
2016-05-13T15:52:58.438-0300 I NETWORK  [initandlisten] waiting for connections on port 27019
```

## <a name="starting_router"></a>Iniciando um router

```
$ mongos -configdb localhost:27019 --port 27020
...
2016-05-13T15:55:55.690-0300 I SHARDING [Balancer] config servers and shards contacted successfully
2016-05-13T15:55:55.691-0300 I SHARDING [Balancer] balancer id: bart:27020 started
2016-05-13T15:55:55.710-0300 I NETWORK  [mongosMain] waiting for connections on port 27020
```

## <a name="starting_shards"></a>Iniciando os shards

Criamos apenas duas novas instâncias do mongo, pois já possuímos uma, a qual contém nossos dados, que vamos utilizar como o primeiro shard.

```
$ sudo mkdir /data/shard2
$ sudo mkdir /data/shard3
```

```
$ sudo mongod --replSet shard2_rs --port 27022 --dbpath /data/shard2
...
2016-05-13T16:01:03.448-0300 I NETWORK  [initandlisten] waiting for connections on port 27022
```

```
$ sudo mongod --replSet shard3_rs --port 27023 --dbpath /data/shard3
...
2016-05-13T16:01:37.271-0300 I NETWORK  [initandlisten] waiting for connections on port 27023
```

## Iniciando a nossa instância existente com o replica set

```
$ sudo mongod --replSet shard1_rs
...
2016-05-13T16:02:06.271-0300 I NETWORK  [initandlisten] waiting for connections on port 27017
```


## <a name="registering_shards"></a>Registrando os shards

Aqui registramos a instância que já tinhamos, onde temos a database `be-mean-project` com a collection `activities` e as duas novas instâncias do Mongo:

```
$ mongo --port 27020
MongoDB shell version: 3.2.6
connecting to: 127.0.0.1:27020/test
Mongo-Hacker 0.0.13
bart:27020(mongos-3.2.6)[mongos] test> sh.addShard("localhost:27017")
{
  "shardAdded": "shard0000",
  "ok": 1
}
bart:27020(mongos-3.2.6)[mongos] test> sh.addShard("localhost:27022")
{
  "shardAdded": "shard0001",
  "ok": 1
}
bart:27020(mongos-3.2.6)[mongos] test> sh.addShard("localhost:27023")
{
  "shardAdded": "shard0002",
  "ok": 1
}
```

## <a name="enabling_sharding"></a>Habilitando sharding para a collection activities na database be-bean-project

```
bart:27020(mongos-3.2.6)[mongos] test> sh.enableSharding("be-mean-project")
{
  "ok": 1
}
bart:27020(mongos-3.2.6)[mongos] test> sh.shardCollection("be-mean-project.activities", {"_id" : 1})
{
  "collectionsharded": "be-mean-project.activities",
  "ok": 1
}
```

## <a name="creating_replicaset"></a>Criando uma réplica para cada shard

```
$ sudo mkdir /data/shard1_replica
$ sudo mkdir /data/shard2_replica
$ sudo mkdir /data/shard3_replica
```

```
$ sudo mongod --replSet shard1_rs --port 27031 --dbpath /data/shard1_replica
...
2016-05-13T17:28:06.975-0300 I NETWORK  [initandlisten] waiting for connections on port 27031
```

```
$ sudo mongod --replSet shard2_rs --port 27032 --dbpath /data/shard2_replica
...
2016-05-13T17:28:24.975-0300 I NETWORK  [initandlisten] waiting for connections on port 27032
```

```
$ sudo mongod --replSet shard3_rs --port 27033 --dbpath /data/shard3_replica
...
2016-05-13T17:28:55.975-0300 I NETWORK  [initandlisten] waiting for connections on port 27032
```

```
$ mongo --host localhost --port 27017
MongoDB shell version: 3.2.6
connecting to: localhost:27017/test
Mongo-Hacker 0.0.13
bart:27017(mongod-3.2.6) test> rs.initiate()
{
  "ok": 1
}
bart:27017(mongod-3.2.6) test> rs.add("127.0.0.1:27031")
{
  "ok": 1
}
```

```
$ mongo --host localhost --port 27022
MongoDB shell version: 3.2.6
connecting to: localhost:27022/test
Mongo-Hacker 0.0.13
bart:27022(mongod-3.2.6) test> rs.initiate()
{
  "ok": 1
}
bart:27022(mongod-3.2.6) test> rs.add("127.0.0.1:27032")
{
  "ok": 1
}
```

```
$ mongo --host localhost --port 27023
MongoDB shell version: 3.2.6
connecting to: localhost:27023/test
Mongo-Hacker 0.0.13
bart:27023(mongod-3.2.6) test> rs.initiate()
{
  "ok": 1
}
bart:27023(mongod-3.2.6) test> rs.add("127.0.0.1:27033")
{
  "ok": 1
}
```