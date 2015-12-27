# MongoDb - Projeto Final
**Autor:** Rafael Crispim Ignácio

**Data** 1451086395767

## Para qual sistema você usaria o MogoDB (diferente desse)?
- Protótipos em geral
- Controle de Log
- Chat

## Qual a modelagem da sua coleção de `users`?

```js
users: {
    name: String,
    bio: String,
    data_register: Date,
    avatar_path: String,

    auth: {
        username: String,
        email: String,
        password: String,
        last_access: Date,
        online: Boolean,
        disabled: Boolean,
        hash_token: String
    }

    system_settings: {
        background_path: String
    }
}
```

## Qual a modelagem da sua coleção de `projects`?

```js
projects: {
    name: String,
    description: String,
    data_begin: Date,
    data_dream: Date,
    data_end: Date,
    visible: Boolean,
    realocate: Boolean,
    expired: Boolean,
    visualizable_mod: Number,

    members: [
        {
            type_member: String,
            user_id: String,
            name: String,
            avatar_path: String,
            notify: Boolean
        }
    ],

    tags: [],

    goals: [
        {
            id: String,
            name: String,
            description: String,
            data_begin: Date,
            data_dream: Date,
            data_end: Date,
            realocate: Boolean,
            expired: Boolean,

            tags: [],

        }
    ]
}
```

## Qual a modelagem da sua coleção retirada de `projects`?

```js
activities: [
    {
        project_id: String,
        goal_id: String,
        name: String,
        description: String,
        data_begin: Date,
        data_dream: Date,
        data_end: Date,
        realocate: Boolean,
        expired: Boolean,

        comments: [
            {
                text: String,
                date_coment: Date,
                member: {
                    user_id: String,
                    name: String,
                    avatar_path: String
                },
                file: []
            }
        ],

        historics: [
            {"date_realocate": Date}
        ]
    }
]
```

A modelagem foi decidida pensando no número de repetições que existiriam de `users` no elemento `members` da coleção `projects`. Como o número de usuários cumprindo o papel de membro pode ser alto, optei por não alocar a coleção iteira de usuários na coleção de projetos. Alguns dados do usuário foram adicionados nos indices `projects.members`/`projects.goals.activities.comments.member` para facilitar a identificação do mesmo em uma possível consulta, são eles: `type_member`, `user_id`, `name` e  `avatar_path`. O mesmo problema ocorre com a coleção `activities`, que possui um potencial de crecimento alto visto que podem existir **n** atividades e dentro de cada atividade podem existir **n** comentários.

## Create - cadastro

```js
// users
be-mean-mongodb> db.users.insert({
...     name: 'João',
...     data_register: Date.now(),
...     auth: {
...         username: 'joao',
...         email: 'joao@project-bemean-mongo.com',
...         password: 'joao123',
...         hash_token: Math.random().toString(36).substr(2)
...     }
... })
Inserted 1 record(s) in 347ms
WriteResult({
  "nInserted": 1
})
be-mean-mongodb> db.users.insert({
...     name: 'Maria',
...     data_register: Date.now(),
...     auth: {
...         username: 'maria',
...         email: 'maria@project-bemean-mongo.com',
...         password: 'maria123',
...         hash_token: Math.random().toString(36).substr(2)
...     }
... })
Inserted 1 record(s) in 6ms
WriteResult({
  "nInserted": 1
})
be-mean-mongodb> db.users.insert({
...     name: 'José',
...     data_register: Date.now(),
...     auth: {
...         username: 'jose',
...         email: 'jose@project-bemean-mongo.com',
...         password: 'jose123',
...         hash_token: Math.random().toString(36).substr(2)
...     }
... })
Inserted 1 record(s) in 1ms
WriteResult({
  "nInserted": 1
})
be-mean-mongodb> db.users.insert({
...     name: 'Mendes',
...     data_register: Date.now(),
...     auth: {
...         username: 'mendes',
...         email: 'mendes@project-bemean-mongo.com',
...         password: 'mendes123',
...         hash_token: Math.random().toString(36).substr(2)
...     }
... })
Inserted 1 record(s) in 10ms
WriteResult({
  "nInserted": 1
})
be-mean-mongodb> db.users.insert({
...     name: 'Augusto',
...     data_register: Date.now(),
...     auth: {
...         username: 'augusto',
...         email: 'augusto@project-bemean-mongo.com',
...         password: 'augusto123',
...         hash_token: Math.random().toString(36).substr(2)
...     }
... })
Inserted 1 record(s) in 1ms
WriteResult({
  "nInserted": 1
})
be-mean-mongodb> db.users.insert({
...     name: 'Beatriz',
...     data_register: Date.now(),
...     auth: {
...         username: 'beatriz',
...         email: 'beatriz@project-bemean-mongo.com',
...         password: 'beatriz123',
...         hash_token: Math.random().toString(36).substr(2)
...     }
... })
Inserted 1 record(s) in 2ms
WriteResult({
  "nInserted": 1
})
be-mean-mongodb> db.users.insert({
...     name: 'Gustavo',
...     data_register: Date.now(),
...     auth: {
...         username: 'gustavo',
...         email: 'gustavo@project-bemean-mongo.com',
...         password: 'gustavo123',
...         hash_token: Math.random().toString(36).substr(2)
...     }
... })
Inserted 1 record(s) in 2ms
WriteResult({
  "nInserted": 1
})
be-mean-mongodb> db.users.insert({
...     name: 'Tais',
...     data_register: Date.now(),
...     auth: {
...         username: 'tais',
...         email: 'tais@project-bemean-mongo.com',
...         password: 'tais123',
...         hash_token: Math.random().toString(36).substr(2)
...     }
... })
Inserted 1 record(s) in 8ms
WriteResult({
  "nInserted": 1
})
be-mean-mongodb> db.users.insert({
...     name: 'Barbara',
...     data_register: Date.now(),
...     auth: {
...         username: 'barbara',
...         email: 'barbara@project-bemean-mongo.com',
...         password: 'barbara123',
...         hash_token: Math.random().toString(36).substr(2)
...     }
... })
Inserted 1 record(s) in 1ms
WriteResult({
  "nInserted": 1
})
be-mean-mongodb> db.users.insert({
...     name: 'Chico',
...     data_register: Date.now(),
...     auth: {
...         username: 'chico',
...         email: 'chico@project-bemean-mongo.com',
...         password: 'chico123',
...         hash_token: Math.random().toString(36).substr(2)
...     }
... })
Inserted 1 record(s) in 1ms
WriteResult({
  "nInserted": 1
})

// projects
be-mean-mongodb> db.projects.insert({
...     "name": "Mussum ipsum cacilds",
...     "description": "Mussum ipsum cacilds, vidis litro abertis.",
...     "date_begin": Date.now(),
...     "date_dream": new Date().setYear(2016),
...     "visible": true,
...     "members": [
...         {"type_member": "admin", "_id": ObjectId("56733ed09c184540ae8fc252"), "name": "João"},
...         {"type_member": "developer", "_id": ObjectId("56733ee39c184540ae8fc253"), "name": "Maria"},
...         {"type_member": "analyst", "_id": ObjectId("56733eea9c184540ae8fc254"), "name": "José"},
...         {"type_member": "analyst", "_id": ObjectId("56733ef49c184540ae8fc256"), "name": "Mendes"},
...         {"type_member": "developer", "_id": ObjectId("56733f079c184540ae8fc257"), "name": "Augusto"}
...     ],
...     "tags": [
...         "cevadis",
...         "golada",
...         "jamaikis"
...     ],
...     "goals": [
...         {
...             "_id": ObjectId(),
...             "name": "Consetis",
...             "description": "Consetis adipiscings elitis.",
...             "data_begin": Date.now(),
...             "data_dream": new Date().setDate(new Date().getDate() +60),
...             "tags": [
...                 "cacilds",
...                 "vidis",
...                 "abertis"
...             ]
...         }
...     ]
... })
Inserted 1 record(s) in 4ms
WriteResult({
  "nInserted": 1
})
be-mean-mongodb> db.activities.insert({
...     "project_id": ObjectId("567c147924393ab50453ec2f"),
...     "goal_id": ObjectId("567c147924393ab50453ec2e"),
...     "name": "Pra lá , depois divoltis",
...     "description": "Pra lá , depois divoltis porris, paradis.",
...     "data_begin": Date.now(),
...     "data_dream": new Date().setDate(new Date().getDate() +40),
...     "comments": []
... })
Inserted 1 record(s) in 2ms
WriteResult({
  "nInserted": 1
})
be-mean-mongodb> db.activities.insert({
...     "project_id": ObjectId("567c147924393ab50453ec2f"),
...     "goal_id": ObjectId("567c147924393ab50453ec2e"),
...     "name": "Paisis, filhis",
...     "description": "Paisis, filhis, espiritis santis, aménzis.",
...     "data_begin": new Date().setDate(new Date().getDate() +40),
...     "data_dream": new Date().setDate(new Date().getDate() +60),
...     "comments": []
... })
Inserted 1 record(s) in 5ms
WriteResult({
  "nInserted": 1
})

be-mean-mongodb> db.projects.insert({
...     "name": "Suco de cevadiss",
...     "description": "Suco de cevadiss, é um leite divinis",
...     "date_begin": Date.now(),
...     "date_dream": new Date().setYear(2016),
...     "visible": true,
...     "members": [
...         {"type_member": "analyst", "_id": ObjectId("56733ee39c184540ae8fc253"), "name": "Maria"},
...         {"type_member": "analyst", "_id": ObjectId("56733eea9c184540ae8fc254"), "name": "José"},
...         {"type_member": "analyst", "_id": ObjectId("56733ef49c184540ae8fc256"), "name": "Mendes"},
...         {"type_member": "developer", "_id": ObjectId("56733f079c184540ae8fc257"), "name": "Augusto"},
...         {"type_member": "developer", "_id": ObjectId("56733f0f9c184540ae8fc258"), "name": "Beatriz"}
...     ],
...     "tags": [
...         "uiski",
...         "malandris",
...         "pirulitá"
...     ],
...     "goals": [
...         {
...             "_id": ObjectId(),
...             "name": "Pharetra in mattis molestie",
...             "description": "Pharetra in mattis molestie, volutpat elementum justo.",
...             "data_begin": Date.now(),
...             "data_dream": new Date().setDate(new Date().getDate() +60),
...             "tags": [
...                 "divinis",
...                 "lupuliz",
...                 "matis"
...             ]
...         }
...     ]
... })
Inserted 1 record(s) in 11ms
WriteResult({
  "nInserted": 1
})
be-mean-mongodb> db.activities.insert({
...     "project_id": ObjectId("567da1a3a32f26a3a5fe684f"),
...     "goal_id": ObjectId("567da1a3a32f26a3a5fe684e"),
...     "name": "Interagi no mé",
...     "description": "Interagi no mé, cursus quis, vehicula ac nisi.",
...     "data_begin": Date.now(),
...     "data_dream": new Date().setDate(new Date().getDate() +40),
...     "comments": []
... })
Inserted 1 record(s) in 2ms
WriteResult({
  "nInserted": 1
})
be-mean-mongodb> db.activities.insert({
...     "project_id": ObjectId("567da1a3a32f26a3a5fe684f"),
...     "goal_id": ObjectId("567da1a3a32f26a3a5fe684e"),
...     "name": "Nullam leo erat",
...     "description": "Nullam leo erat, aliquet quis tempus a, posuere ut mi.",
...     "data_begin": new Date().setDate(new Date().getDate() +40),
...     "data_dream": new Date().setDate(new Date().getDate() +60),
...     "comments": []
... })
Inserted 1 record(s) in 1ms
WriteResult({
  "nInserted": 1
})

be-mean-mongodb> db.projects.insert({
...     "name": "Casamentiss faiz",
...     "description": "Casamentiss faiz malandris se pirulitá",
...     "date_begin": Date.now(),
...     "date_dream": new Date().setYear(2016),
...     "visible": true,
...     "members": [
...         {"type_member": "developer", "_id": ObjectId("56733eea9c184540ae8fc254"), "name": "José"},
...         {"type_member": "analyst", "_id": ObjectId("56733ef49c184540ae8fc256"), "name": "Mendes"},
...         {"type_member": "analyst", "_id": ObjectId("56733f079c184540ae8fc257"), "name": "Augusto"},
...         {"type_member": "developer", "_id": ObjectId("56733f0f9c184540ae8fc258"), "name": "Beatriz"},
...         {"type_member": "developer", "_id": ObjectId("56733f149c184540ae8fc259"), "name": "Gustavo"}
...     ],
...     "tags": [
...         "matis",
...         "cevadis",
...         "aguis"
...     ],
...     "goals": [
...         {
...             "_id": ObjectId(),
...             "name": "Nam liber tempor",
...             "description": "Nam liber tempor cum soluta nobis eleifend option.",
...             "data_begin": Date.now(),
...             "data_dream": new Date().setDate(new Date().getDate() +60),
...             "tags": [
...                 "nobis",
...                 "mazim",
...                 "possim"
...             ]
...         }
...     ]
... })
Inserted 1 record(s) in 5ms
WriteResult({
  "nInserted": 1
})
be-mean-mongodb> db.activities.insert({
...     "project_id": ObjectId("567da243a32f26a3a5fe6853"),
...     "goal_id": ObjectId("567da243a32f26a3a5fe6852"),
...     "name": "Ispecialista im mé",
...     "description": "Ispecialista im mé intende tudis nuam golada",
...     "data_begin": Date.now(),
...     "data_dream": new Date().setDate(new Date().getDate() +40),
...     "comments": []
... })
Inserted 1 record(s) in 2ms
WriteResult({
  "nInserted": 1
})
be-mean-mongodb> db.activities.insert({
...     "project_id": ObjectId("567da243a32f26a3a5fe6853"),
...     "goal_id": ObjectId("567da243a32f26a3a5fe6852"),
...     "name": "Adipiscing elit",
...     "description": "Adipiscing elit, sed diam nonummy nibh euismod tincidunt",
...     "data_begin": new Date().setDate(new Date().getDate() +40),
...     "data_dream": new Date().setDate(new Date().getDate() +60),
...     "comments": []
... })
Inserted 1 record(s) in 2ms
WriteResult({
  "nInserted": 1
})

be-mean-mongodb> db.projects.insert({
...     "name": "Cevadis im ampola",
...     "description": "Cevadis im ampola pa arma uma pindureta.",
...     "date_begin": Date.now(),
...     "date_dream": new Date().setYear(2016),
...     "visible": true,
...     "members": [
...         {"type_member": "analyst", "_id": ObjectId("56733f079c184540ae8fc257"), "name": "Augusto"},
...         {"type_member": "analyst", "_id": ObjectId("56733f0f9c184540ae8fc258"), "name": "Beatriz"},
...         {"type_member": "developer", "_id": ObjectId("56733f149c184540ae8fc259"), "name": "Gustavo"},
...         {"type_member": "developer", "_id": ObjectId("56733f199c184540ae8fc25a"), "name": "Tais"},
...         {"type_member": "developer", "_id": ObjectId("56733f1e9c184540ae8fc25b"), "name": "Barbara"}
...     ],
...     "tags": [
...         "aguis",
...         "fermentis",
...         "uiski"
...     ],
...     "goals": [
...         {
...             "_id": ObjectId(),
...             "name": "Nam varius",
...             "description": "Nam varius eleifend orci, sed viverra nisl condimentum ut.",
...             "data_begin": Date.now(),
...             "data_dream": new Date().setDate(new Date().getDate() +60),
...             "tags": [
...                 "taciti",
...                 "inceptos",
...                 "furadis"
...             ]
...         }
...     ]
... })
Inserted 1 record(s) in 10ms
WriteResult({
  "nInserted": 1
})
be-mean-mongodb> db.activities.insert({
...     "project_id": ObjectId("567da2daa32f26a3a5fe6857"),
...     "goal_id": ObjectId("567da2daa32f26a3a5fe6856"),
...     "name": "Donec eget",
...     "description": "Donec eget justis enim.",
...     "data_begin": Date.now(),
...     "data_dream": new Date().setDate(new Date().getDate() +40),
...     "comments": []
... })
Inserted 1 record(s) in 1ms
WriteResult({
  "nInserted": 1
})
MacBook-Pro-de-Rafael(mongod-3.0.7) be-mean-mongodb> db.activities.insert({
...     "project_id": ObjectId("567da2daa32f26a3a5fe6857"),
...     "goal_id": ObjectId("567da2daa32f26a3a5fe6856"),
...     "name": "Atirei o pau",
...     "description": "Atirei o pau no gatis.",
...     "data_begin": new Date().setDate(new Date().getDate() +40),
...     "data_dream": new Date().setDate(new Date().getDate() +60),
...     "comments": []
... })
Inserted 1 record(s) in 3ms
WriteResult({
  "nInserted": 1
})

be-mean-mongodb> db.projects.insert({
...     "name": "Forevis aptent",
...     "description": "Forevis aptent taciti sociosqu ad litora",
...     "date_begin": Date.now(),
...     "date_dream": new Date().setYear(2016),
...     "visible": true,
...     "members": [
...         {"type_member": "admin", "_id": ObjectId("56733f0f9c184540ae8fc258"), "name": "Beatriz"},
...         {"type_member": "developer", "_id": ObjectId("56733f149c184540ae8fc259"), "name": "Gustavo"},
...         {"type_member": "developer", "_id": ObjectId("56733f199c184540ae8fc25a"), "name": "Tais"},
...         {"type_member": "developer", "_id": ObjectId("56733f1e9c184540ae8fc25b"), "name": "Barbara"},
...         {"type_member": "analyst", "_id": ObjectId("56733f249c184540ae8fc25c"), "name": "Chico"}
...     ],
...     "tags": [
...         "paisis",
...         "cevadis",
...         "mé"
...     ],
...     "goals": [
...         {
...             "_id": ObjectId(),
...             "name": "Copo furadis",
...             "description": "Copo furadis é disculpa de babadis",
...             "data_begin": Date.now(),
...             "data_dream": new Date().setDate(new Date().getDate() +60),
...             "tags": [
...                 "mijis",
...                 "nonummy",
...                 "euismod"
...             ],
...         }
...     ]
... })
Inserted 1 record(s) in 6ms
WriteResult({
  "nInserted": 1
})
```

## Retrieve - busca
- Liste as informações dos membros de 1 projeto específico que deve ser buscado pelo seu nome de forma a não ligar para maiúsculas e minúsculas.

```js
be-mean-mongodb> db.users.find({ $or: db.projects.findOne({}, {'members._id': 1, _id: 0}).members })
{
  "_id": ObjectId("56733ed09c184540ae8fc252"),
  "name": "João",
  "data_register": 1450393296187,
  "auth": {
    "username": "joao",
    "email": "joao@project-bemean-mongo.com",
    "password": "joao123",
    "hash_token": "htgb8dx2tn265hfr"
  }
}
{
  "_id": ObjectId("56733ee39c184540ae8fc253"),
  "name": "Maria",
  "data_register": 1450393315854,
  "auth": {
    "username": "maria",
    "email": "maria@project-bemean-mongo.com",
    "password": "maria123",
    "hash_token": "mmwm2o87243g14i"
  }
}
{
  "_id": ObjectId("56733eea9c184540ae8fc254"),
  "name": "José",
  "data_register": 1450393322122,
  "auth": {
    "username": "jose",
    "email": "jose@project-bemean-mongo.com",
    "password": "jose123",
    "hash_token": "caqupmgx7833di"
  }
}
{
  "_id": ObjectId("56733ef49c184540ae8fc256"),
  "name": "Mendes",
  "data_register": 1450393332898,
  "auth": {
    "username": "mendes",
    "email": "mendes@project-bemean-mongo.com",
    "password": "mendes123",
    "hash_token": "a4mw8zy50vp74x6r"
  }
}
{
  "_id": ObjectId("56733f079c184540ae8fc257"),
  "name": "Augusto",
  "data_register": 1450393351482,
  "auth": {
    "username": "augusto",
    "email": "augusto@project-bemean-mongo.com",
    "password": "augusto123",
    "hash_token": "wfdvxvpzloe9izfr"
  }
}
Fetched 5 record(s) in 16ms
```

- Liste todos os projetos com a tag que você escolheu para os 3 projetos em comum.

```js
be-mean-mongodb> db.projects.find( { tags: { $in: ['cevadis'] } } )
{
  "_id": ObjectId("5673635d9c184540ae8fc25d"),
  "name": "Mussum ipsum cacilds",
  "description": "Mussum ipsum cacilds, vidis litro abertis.",
  "date_begin": 1450402653389,
  "date_dream": 1482025053389,
  "visible": true,
  "members": [
    {
      "type_member": "admin",
      "_id": ObjectId("56733ed09c184540ae8fc252"),
      "name": "João"
    },
    {
      "type_member": "developer",
      "_id": ObjectId("56733ee39c184540ae8fc253"),
      "name": "Maria"
    },
    {
      "type_member": "analyst",
      "_id": ObjectId("56733eea9c184540ae8fc254"),
      "name": "José"
    },
    {
      "type_member": "analyst",
      "_id": ObjectId("56733ef49c184540ae8fc256"),
      "name": "Mendes"
    },
    {
      "type_member": "developer",
      "_id": ObjectId("56733f079c184540ae8fc257"),
      "name": "Augusto"
    }
  ],
  "tags": [
    "cevadis",
    "golada",
    "jamaikis"
  ],
  "goals": [
    {
      "name": "Consetis",
      "description": "Consetis adipiscings elitis.",
      "data_begin": 1450402653390,
      "data_dream": 1455586653390,
      "tags": [
        "cacilds",
        "vidis",
        "abertis"
      ],
      "activities": [
        {
          "name": "Pra lá , depois divoltis",
          "description": "Pra lá , depois divoltis porris, paradis.",
          "data_begin": 1450402653390,
          "data_dream": 1453858653390,
          "comments": [ ]
        },
        {
          "name": "Paisis, filhis",
          "description": "Paisis, filhis, espiritis santis, aménzis.",
          "data_begin": 1453858653390,
          "data_dream": 1455586653390,
          "comments": [ ]
        }
      ]
    }
  ]
}
{
  "_id": ObjectId("567363699c184540ae8fc25f"),
  "name": "Casamentiss faiz",
  "description": "Casamentiss faiz malandris se pirulitá",
  "date_begin": 1450402665212,
  "date_dream": 1482025065212,
  "visible": true,
  "members": [
    {
      "type_member": "developer",
      "_id": ObjectId("56733eea9c184540ae8fc254"),
      "name": "José"
    },
    {
      "type_member": "analyst",
      "_id": ObjectId("56733ef49c184540ae8fc256"),
      "name": "Mendes"
    },
    {
      "type_member": "analyst",
      "_id": ObjectId("56733f079c184540ae8fc257"),
      "name": "Augusto"
    },
    {
      "type_member": "developer",
      "_id": ObjectId("56733f0f9c184540ae8fc258"),
      "name": "Beatriz"
    },
    {
      "type_member": "developer",
      "_id": ObjectId("56733f149c184540ae8fc259"),
      "name": "Gustavo"
    }
  ],
  "tags": [
    "matis",
    "cevadis",
    "aguis"
  ],
  "goals": [
    {
      "name": "Nam liber tempor",
      "description": "Nam liber tempor cum soluta nobis eleifend option.",
      "data_begin": 1450402665212,
      "data_dream": 1455586665212,
      "tags": [
        "nobis",
        "mazim",
        "possim"
      ],
      "activities": [
        {
          "name": "Ispecialista im mé",
          "description": "Ispecialista im mé intende tudis nuam golada",
          "data_begin": 1450402665212,
          "data_dream": 1453858665212,
          "comments": [ ]
        },
        {
          "name": "Adipiscing elit",
          "description": "Adipiscing elit, sed diam nonummy nibh euismod tincidunt",
          "data_begin": 1453858665212,
          "data_dream": 1455586665212,
          "comments": [ ]
        }
      ]
    }
  ]
}
{
  "_id": ObjectId("567363729c184540ae8fc261"),
  "name": "Forevis aptent",
  "description": "Forevis aptent taciti sociosqu ad litora",
  "date_begin": 1450402674724,
  "date_dream": 1482025074724,
  "visible": true,
  "members": [
    {
      "type_member": "admin",
      "_id": ObjectId("56733f0f9c184540ae8fc258"),
      "name": "Beatriz"
    },
    {
      "type_member": "developer",
      "_id": ObjectId("56733f149c184540ae8fc259"),
      "name": "Gustavo"
    },
    {
      "type_member": "developer",
      "_id": ObjectId("56733f199c184540ae8fc25a"),
      "name": "Tais"
    },
    {
      "type_member": "developer",
      "_id": ObjectId("56733f1e9c184540ae8fc25b"),
      "name": "Barbara"
    },
    {
      "type_member": "analyst",
      "_id": ObjectId("56733f249c184540ae8fc25c"),
      "name": "Chico"
    }
  ],
  "tags": [
    "paisis",
    "mé",
    "cevadis"
  ],
  "goals": [
    {
      "name": "Copo furadis",
      "description": "Copo furadis é disculpa de babadis",
      "data_begin": 1450402674724,
      "data_dream": 1455586674724,
      "tags": [
        "mijis",
        "nonummy",
        "euismod"
      ],
      "activities": [ ]
    }
  ]
}
Fetched 3 record(s) in 11ms
```

- Liste apenas os nomes de todas as atividades para todos os projetos.

```js
be-mean-mongodb> db.activities.find({ }, { '_id': 0, 'name': 1 })
{
  "name": "Pra lá , depois divoltis"
}
{
  "name": "Paisis, filhis"
}
{
  "name": "Interagi no mé"
}
{
  "name": "Nullam leo erat"
}
{
  "name": "Ispecialista im mé"
}
{
  "name": "Adipiscing elit"
}
{
  "name": "Donec eget"
}
{
  "name": "Atirei o pau"
}
Fetched 8 record(s) in 7ms
```

- Liste todos os projetos que não possuam uma tag.

```js
be-mean-mongodb> db.projects.find( { $or: [ { tags: { $exists: false } }, { tags: { $size: 0 } } ] } )
Fetched 0 record(s) in 1ms
```

- Liste todos os usuários que não fazem parte do primeiro projeto cadastrado.

```js
be-mean-mongodb> db.users.find( { $nor: db.projects.findOne({ _id: ObjectId("5673635d9c184540ae8fc25d") }, {'members._id': 1, _id: 0}).members } )
{
  "_id": ObjectId("56733f0f9c184540ae8fc258"),
  "name": "Beatriz",
  "data_register": 1450393359730,
  "auth": {
    "username": "beatriz",
    "email": "beatriz@project-bemean-mongo.com",
    "password": "beatriz123",
    "hash_token": "fu4exlf0n0o1or"
  }
}
{
  "_id": ObjectId("56733f149c184540ae8fc259"),
  "name": "Gustavo",
  "data_register": 1450393364305,
  "auth": {
    "username": "gustavo",
    "email": "gustavo@project-bemean-mongo.com",
    "password": "gustavo123",
    "hash_token": "l4bcvorc2w23ayvi"
  }
}
{
  "_id": ObjectId("56733f199c184540ae8fc25a"),
  "name": "Tais",
  "data_register": 1450393369369,
  "auth": {
    "username": "tais",
    "email": "tais@project-bemean-mongo.com",
    "password": "tais123",
    "hash_token": "5812rs54mx36usor"
  }
}
{
  "_id": ObjectId("56733f1e9c184540ae8fc25b"),
  "name": "Barbara",
  "data_register": 1450393374905,
  "auth": {
    "username": "barbara",
    "email": "barbara@project-bemean-mongo.com",
    "password": "barbara123",
    "hash_token": "vef1kathuxr"
  }
}
{
  "_id": ObjectId("56733f249c184540ae8fc25c"),
  "name": "Chico",
  "data_register": 1450393380313,
  "auth": {
    "username": "chico",
    "email": "chico@project-bemean-mongo.com",
    "password": "chico123",
    "hash_token": "hm0odcy5teghkt9"
  }
}
Fetched 5 record(s) in 3ms
```

## Update - alteração
- Adicione para todos os projetos o campo views: 0.

```js
be-mean-mongodb> db.projects.update( {}, { $set: { views: 0 }  }, { multi: true } )
Updated 5 existing record(s) in 6ms
WriteResult({
  "nMatched": 5,
  "nUpserted": 0,
  "nModified": 5
})
```

- Adicione 1 tag diferente para cada projeto.

```js
be-mean-mongodb> db.projects.update( { "_id": ObjectId("5673635d9c184540ae8fc25d") }, { $push: { tags: 'torquent' }  } )
Updated 1 existing record(s) in 6ms
WriteResult({
  "nMatched": 1,
  "nUpserted": 0,
  "nModified": 1
})
be-mean-mongodb> db.projects.update( { "_id": ObjectId("567363649c184540ae8fc25e") }, { $push: { tags: 'babadis' }  } )
Updated 1 existing record(s) in 2ms
WriteResult({
  "nMatched": 1,
  "nUpserted": 0,
  "nModified": 1
})
be-mean-mongodb> db.projects.update( { "_id": ObjectId("567363699c184540ae8fc25f") }, { $push: { tags: 'espiritis' }  } )
Updated 1 existing record(s) in 2ms
WriteResult({
  "nMatched": 1,
  "nUpserted": 0,
  "nModified": 1
})
be-mean-mongodb> db.projects.update( { "_id": ObjectId("567363729c184540ae8fc261") }, { $push: { tags: 'divoltis' }  } )
Updated 1 existing record(s) in 2ms
WriteResult({
  "nMatched": 1,
  "nUpserted": 0,
  "nModified": 1
})
be-mean-mongodb> db.projects.update( { "_id": ObjectId("5673636e9c184540ae8fc260") }, { $push: { tags: 'lupuliz' }  } )
Updated 1 existing record(s) in 2ms
WriteResult({
  "nMatched": 1,
  "nUpserted": 0,
  "nModified": 1
})
```

- Adicione 2 membros diferentes para cada projeto.

```js
// Projeto 1
be-mean-mongodb> db.projects.update( { "_id": ObjectId("5673635d9c184540ae8fc25d") }, { $push: {
...     "members": {"type_member": "admin", "_id": ObjectId("56733f0f9c184540ae8fc258"), "name": "Beatriz"}
... }  } )
Updated 1 existing record(s) in 16ms
WriteResult({
  "nMatched": 1,
  "nUpserted": 0,
  "nModified": 1
})
be-mean-mongodb> db.projects.update( { "_id": ObjectId("5673635d9c184540ae8fc25d") }, { $push: {
...     "members": {"type_member": "developer", "_id": ObjectId("56733f149c184540ae8fc259"), "name": "Gustavo"}
... }  } )
Updated 1 existing record(s) in 3ms
WriteResult({
  "nMatched": 1,
  "nUpserted": 0,
  "nModified": 1
})

// Projeto 2
be-mean-mongodb> db.projects.update( { "_id": ObjectId("567363649c184540ae8fc25e") }, { $push: {
...     "members": {"type_member": "developer", "_id": ObjectId("56733f149c184540ae8fc259"), "name": "Gustavo"}
... }  } )
Updated 1 existing record(s) in 2ms
WriteResult({
  "nMatched": 1,
  "nUpserted": 0,
  "nModified": 1
})
be-mean-mongodb> db.projects.update( { "_id": ObjectId("567363649c184540ae8fc25e") }, { $push: {
...     "members": {"type_member": "developer", "_id": ObjectId("56733f199c184540ae8fc25a"), "name": "Tais"}
... }  } )
Updated 1 existing record(s) in 1ms
WriteResult({
  "nMatched": 1,
  "nUpserted": 0,
  "nModified": 1
})

// Projeto 3
be-mean-mongodb> db.projects.update( { "_id": ObjectId("567363699c184540ae8fc25f") }, { $push: {
...     "members": {"type_member": "developer", "_id": ObjectId("56733f199c184540ae8fc25a"), "name": "Tais"}
... }  } )
Updated 1 existing record(s) in 12ms
WriteResult({
  "nMatched": 1,
  "nUpserted": 0,
  "nModified": 1
})
be-mean-mongodb> db.projects.update( { "_id": ObjectId("567363699c184540ae8fc25f") }, { $push: {
...     "members": {"type_member": "developer", "_id": ObjectId("56733f1e9c184540ae8fc25b"), "name": "Barbara"}
... }  } )
Updated 1 existing record(s) in 2ms
WriteResult({
  "nMatched": 1,
  "nUpserted": 0,
  "nModified": 1
})

// Projeto 4
be-mean-mongodb> db.projects.update( { "_id": ObjectId("567363729c184540ae8fc261") }, { $push: {
...     "members": {"type_member": "analyst", "_id": ObjectId("56733f1e9c184540ae8fc25b"), "name": "Barbara"}
... }  } )
Updated 1 existing record(s) in 2ms
WriteResult({
  "nMatched": 1,
  "nUpserted": 0,
  "nModified": 1
})
be-mean-mongodb> db.projects.update( { "_id": ObjectId("567363729c184540ae8fc261") }, { $push: {
...     "members": {"type_member": "developer", "_id": ObjectId("56733f249c184540ae8fc25c"), "name": "Chico"}
... }  } )
Updated 1 existing record(s) in 1ms
WriteResult({
  "nMatched": 1,
  "nUpserted": 0,
  "nModified": 1
})

// Projeto 5
be-mean-mongodb> db.projects.update( { "_id": ObjectId("5673636e9c184540ae8fc260") }, { $push: {
...     "members": {"type_member": "analyst", "_id": ObjectId("56733f249c184540ae8fc25c"), "name": "Chico"}
... }  } )
Updated 1 existing record(s) in 7ms
WriteResult({
  "nMatched": 1,
  "nUpserted": 0,
  "nModified": 1
})
be-mean-mongodb> db.projects.update( { "_id": ObjectId("5673636e9c184540ae8fc260") }, { $push: {
...     "members": {"type_member": "developer", "_id": ObjectId("56733ed09c184540ae8fc252"), "name": "João"}
... }  } )
Updated 1 existing record(s) in 1ms
WriteResult({
  "nMatched": 1,
  "nUpserted": 0,
  "nModified": 1
})
```

- Adicione 1 comentário em cada atividade, deixe apenas 1 projeto sem.

```js
be-mean-mongodb> db.activities.update( {
...     "_id": ObjectId("567c14bd24393ab50453ec30")
... }, { $push: {
...     "comments": {
...         "text": "Comentario desnecessário novamente",
...         "date_comment": Date.now(),
...         "member": {
...             "_id": ObjectId("56733ed09c184540ae8fc252"),
...             "name": "João"
...         }
...     }
... }  } )
Updated 1 existing record(s) in 8ms
WriteResult({
  "nMatched": 1,
  "nUpserted": 0,
  "nModified": 1
})
be-mean-mongodb> db.activities.update( {
...     "_id": ObjectId("567c14c024393ab50453ec31")
... }, { $push: {
...     "comments": {
...         "text": "Comentario desnecessário novamente 2",
...         "date_comment": Date.now(),
...         "member": {
...             "_id": ObjectId("56733ef49c184540ae8fc256"),
...             "name": "Mendes"
...         }
...     }
... }  } )
Updated 1 existing record(s) in 4ms
WriteResult({
  "nMatched": 1,
  "nUpserted": 0,
  "nModified": 1
})
be-mean-mongodb> db.activities.update( {
...     "_id": ObjectId("567da20ba32f26a3a5fe6850")
... }, { $push: {
...     "comments": {
...         "text": "Comentario desnecessário",
...         "date_comment": Date.now(),
...         "member": {
...             "_id": ObjectId("56733eea9c184540ae8fc254"),
...             "name": "José"
...         }
...     }
... }  } )
Updated 1 existing record(s) in 1ms
WriteResult({
  "nMatched": 1,
  "nUpserted": 0,
  "nModified": 1
})
be-mean-mongodb> db.activities.update( {
...     "_id": ObjectId("567da20ba32f26a3a5fe6851")
... }, { $push: {
...     "comments": {
...         "text": "Comentario desnecessário 2",
...         "date_comment": Date.now(),
...         "member": {
...             "_id": ObjectId("56733ee39c184540ae8fc253"),
...             "name": "Maria"
...         }
...     }
... }  } )
Updated 1 existing record(s) in 3ms
WriteResult({
  "nMatched": 1,
  "nUpserted": 0,
  "nModified": 1
})
be-mean-mongodb> db.activities.update( {
...     "_id": ObjectId("567da2b3a32f26a3a5fe6854")
... }, { $push: {
...     "comments": {
...         "text": "Comentario desnecessário parte 1",
...         "date_comment": Date.now(),
...         "member": {
...             "_id": ObjectId("56733f079c184540ae8fc257"),
...             "name": "Augusto"
...         }
...     }
... }  } )
Updated 1 existing record(s) in 2ms
WriteResult({
  "nMatched": 1,
  "nUpserted": 0,
  "nModified": 1
})
be-mean-mongodb> db.activities.update( {
...     "_id": ObjectId("567da2b3a32f26a3a5fe6855")
... }, { $push: {
...     "comments": {
...         "text": "Comentario desnecessário parte 2",
...         "date_comment": Date.now(),
...         "member": {
...             "_id": ObjectId("56733f0f9c184540ae8fc258"),
...             "name": "Beatriz"
...         }
...     }
... }  } )
Updated 1 existing record(s) in 2ms
WriteResult({
  "nMatched": 1,
  "nUpserted": 0,
  "nModified": 1
})
be-mean-mongodb> db.activities.update( {
...     "_id": ObjectId("567da326a32f26a3a5fe6858")
... }, { $push: {
...     "comments": {
...         "text": "Comentario desnecessário, sério?",
...         "date_comment": Date.now(),
...         "member": {
...             "_id": ObjectId("56733ef49c184540ae8fc256"),
...             "name": "Mendes"
...         }
...     }
... }  } )
Updated 1 existing record(s) in 1ms
WriteResult({
  "nMatched": 1,
  "nUpserted": 0,
  "nModified": 1
})
be-mean-mongodb> db.activities.update( {
...     "_id": ObjectId("567da326a32f26a3a5fe6859")
... }, { $push: {
...     "comments": {
...         "text": "Comentario desnecessário a revanche",
...         "date_comment": Date.now(),
...         "member": {
...             "_id": ObjectId("56733f199c184540ae8fc25a"),
...             "name": "Tais"
...         }
...     }
... }  } )
Updated 1 existing record(s) in 0ms
WriteResult({
  "nMatched": 1,
  "nUpserted": 0,
  "nModified": 1
})
```

- Adicione 1 projeto inteiro com UPSERT.

```js
be-mean-mongodb> db.projects.update(
...     { "name": "Pharetra in mattis molestie" },
...     { $setOnInsert: {
...         "name": "Pharetra in mattis molestie",
...         "description": "Pharetra in mattis molestie, aenean justo massa",
...         "date_begin": Date.now(),
...         "date_dream": new Date().setYear(2017),
...         "visible": true,
...         "members": [
...             {"type_member": "admin", "_id": ObjectId("56733f0f9c184540ae8fc258"), "name": "Beatriz"},
...             {"type_member": "developer", "_id": ObjectId("56733f149c184540ae8fc259"), "name": "Gustavo"},
...         ],
...         "tags": [],
...         "goals": [
...             {
...                 "_id": ObjectId(),
...                 "name": "Copo furadis",
...                 "description": "Copo furadis é disculpa de babadis",
...                 "data_begin": Date.now(),
...                 "data_dream": new Date().setDate(new Date().getDate() +60),
...                 "tags": [
...                     "mijis",
...                     "nonummy",
...                     "euismod"
...                 ]
...             }
...         ]
...     } },
...     { upsert: true }
... )
Updated 1 new record(s) in 6ms
WriteResult({
  "nMatched": 0,
  "nUpserted": 1,
  "nModified": 0,
  "_id": ObjectId("567da656e6b106305d55116c")
})
```

## Delete - remoção
- Apague todos os projetos que não possuam tags.

```js
be-mean-mongodb> db.projects.remove({ $or: [ { "tags": {$exists:0} }, { "tags": {$size: 0} } ] })
Removed 1 record(s) in 37ms
WriteResult({
  "nRemoved": 1
})
```

- Apague todos os projetos que não possuam comentários nas atividades.

```js
be-mean-mongodb> db.projects.remove({
...     "_id": {
...         $nin: db.activities.distinct("project_id", {
...             $and: [
...                 { "comments": { $exists: 1 } },
...                 { "comments": { $ne: [] } }
...             ]
...         })
...     }
... })
Removed 2 record(s) in 8ms
WriteResult({
  "nRemoved": 2
})
```

- Apague todos os projetos que não possuam atividades.

```js
be-mean-mongodb> db.projects.remove({
...     "_id": {
...         $nin: db.activities.distinct("project_id")
...     }
... })
Removed 0 record(s) in 3ms
WriteResult({
  "nRemoved": 0
})
```

- Escolha 2 usuário e apague todos os projetos em que os 2 fazem parte.

```js
be-mean-mongodb> db.projects.remove({ $and: [
...     {
...         "members._id": { $in: [
...             ObjectId("56733f079c184540ae8fc257")
...         ] }
...     },
...     {
...         "members._id": { $in: [
...             ObjectId("56733ed09c184540ae8fc252")
...         ] }
...     }
... ] }, { multi: 1 })
Removed 1 record(s) in 6ms
WriteResult({
  "nRemoved": 1
})
```

- Apague todos os projetos que possuam uma determinada tag em goal.

```js
be-mean-mongodb> db.projects.remove({ "goals.tags": { $eq: "furadis" } }, { multi: 1 })
Removed 1 record(s) in 6ms
WriteResult({
  "nRemoved": 1
})
```

## Gerenciamento de Usuários
- Crie um usuário com permissões APENAS de Leitura.

```js
admin> db.createUser(
...   {
...     user: "UserRead",
...     pwd: "user123",
...     roles: [ { role: "readAnyDatabase", db: "admin" }, "read" ]
...   }
... )
Successfully added user: {
  "user": "UserRead",
  "roles": [
    {
      "role": "readAnyDatabase",
      "db": "admin"
    },
    "read"
  ]
}
```

- Crie um usuário com permissões de Escrita e Leitura.

```js
admin> db.createUser(
...   {
...     user: "UserReadWrite",
...     pwd: "user123",
...     roles: [ { role: "readWriteAnyDatabase", db: "admin" }, "readWrite" ]
...   }
... )
Successfully added user: {
  "user": "UserReadWrite",
  "roles": [
    {
      "role": "readWriteAnyDatabase",
      "db": "admin"
    },
    "readWrite"
  ]
}
```

- Adicionar o papel grantRolesToUser e revokeRole para o usuário com Escrita e Leitura.

```js
admin> db.createRole(
...    {
...      role: "grantRolesToUser",
...      privileges: [
...        { resource: { db: "admin", collection: "" }, actions: [ "grantRole" ] }
...      ],
...      roles: []
...    }
... )
{
  "role": "grantRolesToUser",
  "privileges": [
    {
      "resource": {
        "db": "admin",
        "collection": ""
      },
      "actions": [
        "grantRole"
      ]
    }
  ],
  "roles": [ ]
}
admin> db.createRole(
...    {
...      role: "revokeRole",
...      privileges: [
...          { resource: { db: "admin", collection: "" }, actions: [ "revokeRole" ] }
...      ],
...      roles: []
...    }
... )
{
  "role": "revokeRole",
  "privileges": [
    {
      "resource": {
        "db": "admin",
        "collection": ""
      },
      "actions": [
        "revokeRole"
      ]
    }
  ],
  "roles": [ ]
}
admin> db.runCommand( {
...     grantRolesToUser: "UserReadWrite",
...     roles: [
...         "grantRolesToUser",
...         "revokeRole"
...     ],
...     writeConcern: { w: "majority" , wtimeout: 2000 }
... })
{
  "ok": 1
}
```

- Remover o papel grantRolesToUser para o usuário com Escrita e Leitura.

```js
admin> db.revokeRolesFromUser( "UserReadWrite", [ "grantRolesToUser" ] )
```

- Listar todos os usuários com seus papéis e ações.

```js
admin> db.runCommand({usersInfo: 1})
{
  "users": [
    {
      "_id": "admin.UserRead",
      "user": "UserRead",
      "db": "admin",
      "roles": [
        {
          "role": "readAnyDatabase",
          "db": "admin"
        },
        {
          "role": "read",
          "db": "admin"
        }
      ]
    },
    {
      "_id": "admin.UserReadWrite",
      "user": "UserReadWrite",
      "db": "admin",
      "roles": [
        {
          "role": "revokeRole",
          "db": "admin"
        },
        {
          "role": "readWriteAnyDatabase",
          "db": "admin"
        },
        {
          "role": "readWrite",
          "db": "admin"
        }
      ]
    }
  ],
  "ok": 1
}
```

## Sharding
1 Router, 1 Config Server e 3 Shardings
- Config Server

```js
> mongod --configsvr --port 27100
```

- Router

```js
> mongos --configdb localhost:27100 --port 27101
```

- Sharding

```js
// Diretórios
> mkdir data/sh1
> mkdir data/sh2
> mkdir data/sh3

// Iniciando instâncias de Sharding e pré-configurando para receber replica
> mongod --replSet rs1 --port 27151 --dbpath /data/sh1
> mongo --port 27151
> rsconf = {
  "_id": "rs1",
  "members": [
    {
      "_id": 0,
      "host": "127.0.0.1:27151"
    }
  ]
}
> rs.initiate(rsconf)
{
  "ok": 1
}

> mongod --replSet rs2 --port 27152 --dbpath /data/sh2
> mongo --port 27152
> rsconf = {
  "_id": "rs2",
  "members": [
    {
      "_id": 0,
      "host": "127.0.0.1:27152"
    }
  ]
}
> rs.initiate(rsconf)
{
  "ok": 1
}

> mongod --replSet rs3 --port 27153 --dbpath /data/sh3
> mongo --port 27153
> rsconf = {
  "_id": "rs3",
  "members": [
    {
      "_id": 0,
      "host": "127.0.0.1:27153"
    }
  ]
}
> rs.initiate(rsconf)
{
  "ok": 1
}

// Registrando no Router
be-mean-mongodb> sh.addShard("localhost:27151")  
{  
  "shardAdded": "shard0000",  
  "ok": 1  
}  

be-mean-mongodb> sh.addShard("localhost:27152")  
{  
  "shardAdded": "shard0001",  
  "ok": 1  
}  

be-mean-mongodb> sh.addShard("localhost:27153")  
{  
  "shardAdded": "shard0002",  
  "ok": 1  
}

// Definindo collection
be-mean-mongodb> sh.enableSharding("be-mean-mongodb")  
{  
  "ok": 1  
}  

be-mean-mongodb> sh.shardCollection("be-mean-mongodb.activities", {"_id": 1})  
{  
  "collectionsharded": "be-mean-mongodb.activities",  
  "ok": 1  
}
```

## Replica
3 Replicas

```js
// Diretórios
> mkdir data/rs1
> mkdir data/rs2
> mkdir data/rs3

// Iniciando instâncias de Replicas
> mongod --replSet replica_set --port 27171 --dbpath data/rs1
> mongod --replSet replica_set --port 27172 --dbpath data/rs2
> mongod --replSet replica_set --port 27173 --dbpath data/rs3

// Distribuindo replicas entre os shardings
> mongo --port 27151
> rs.add("127.0.0.1:27171")
{
  "ok": 1
}
> mongo --port 27152
> rs.add("127.0.0.1:27172")
{
  "ok": 1
}
> mongo --port 27153
> rs.add("127.0.0.1:27173")
{
  "ok": 1
}
```
