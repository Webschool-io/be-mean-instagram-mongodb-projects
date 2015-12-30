# MongoDb - Projeto Final
**Autor:** Airton Vancin Junior
**Data** 1451499900

## Para qual sistema você usaria o MogoDB (diferente desse)?

Usaria para fazer um sistema de pagamento online, como pagar.me, pagseguro, paypal, moip entre outros.

## Qual a modelagem da sua coleção de `users`?

```js
Usuers = {
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
    },

    settings-system: {
        background-path: String
    }
}
```

## Qual a modelagem da sua coleção de `projects`?

```js
Projects = {
    name: String,
    description: String,
    date_begin: Date,
    date_dream: Date,
    date_end: Date,
    visible: Boolean,
    realocate: Boolean,
    expired: Boolean,
    visualizable_mod: Boolean,
    
    members: [{
        name: String,
        type_member: String        
    }],
    
    tags: [],
    
    goals: [{
        name: String,
        description: String,
        date_begin: Date,
        date_dream: Date,
        date_end: Date,
        visible: Boolean,
        realocate: Boolean,
        expired: Boolean,

        tags: [],

        activities: [
            {
                name: String
            },
            {
                name: String
            }
        ],

        goal_hostoric: [
            {
                date_realocate: Date
            }
        ]
    }]
}
```

## Qual a modelagem da sua coleção retirada de `projects`?

```js
Activities = {
    name: String,
    description: String,
    date_begin: Date,
    date_dream: Date,
    date_end: Date,
    visible: String,
    realocate: String,
    expired: String,

    comments:[{
        text: String,
        date_comment: Date,
        members: [],
        files: [
            { path: String, weight: String, name: String }
        ]
    }],

    historics: [{
        date_realocate: Date
    }]
}
```

## Create - cadastro

**Users**

```js
linux(mongod-3.0.8) projeto-final-mongodb> db.users.save([{
...     name: 'Pablo Escobar',
...     bio: 'O Chefe',
...     data_register: Date(),
...     avatar_path: '',
... 
...     auth: {
...         username: 'pablito',
...         email: 'pablito@narcos.com.br',
...         password: 'plablito123',
...         last_access: Date(),
...         online: true,
...         disabled: false,
...         hash_token: 'Pa3p6p9'
...     },
... 
...     settings_system: {
...         background_path: ''
...     }
... },
... {
...     name: 'Javier Pena',
...     bio: 'Agente da DEA louco pra pegar o pablito e sua rapaziada',
...     data_register: Date(),
...     avatar_path: '',
... 
...     auth: {
...         username: 'javier',
...         email: 'javier@narcos.com.br',
...         password: 'javier123',
...         last_access: Date(),
...         online: true,
...         disabled: false,
...         hash_token: '89ad98f'
...     },
... 
...     settings_system: {
...         background_path: ''
...     }
... },
... {
...     name: 'Steve Murphy',
...     bio: 'Agente da DEA parceiro de Javier',
...     data_register: Date(),
...     avatar_path: '',
... 
...     auth: {
...         username: 'steve',
...         email: 'steve@narcos.com.br',
...         password: 'steve123',
...         last_access: Date(),
...         online: true,
...         disabled: false,
...         hash_token: '12ar7e8'
...     },
... 
...     settings_system: {
...         background_path: ''
...     }
... },
... {
...     name: 'Gustavo Gaviria',
...     bio: 'Primo de Pablito e controla as finanças',
...     data_register: Date(),
...     avatar_path: '',
... 
...     auth: {
...         username: 'gustavo',
...         email: 'gustavo@narcos.com.br',
...         password: 'gustavo123',
...         last_access: Date(),
...         online: true,
...         disabled: false,
...         hash_token: '35g35g35'
...     },
... 
...     settings_system: {
...         background_path: ''
...     }
... },
... {
...     name: 'Valeria Velez',
...     bio: 'Reporter danada, amante de pablito',
...     data_register: Date(),
...     avatar_path: '',
... 
...     auth: {
...         username: 'valeria',
...         email: 'valeria@narcos.com.br',
...         password: 'valeria123',
...         last_access: Date(),
...         online: true,
...         disabled: false,
...         hash_token: 'v7a97a95a'
...     },
... 
...     settings_system: {
...         background_path: ''
...     }
... },
... {
...     name: 'Connie Murphy',
...     bio: 'Esposa do Policial Steve Murphy',
...     data_register: Date(),
...     avatar_path: '',
... 
...     auth: {
...         username: 'connie',
...         email: 'connie@narcos.com.br',
...         password: 'connie123',
...         last_access: Date(),
...         online: true,
...         disabled: false,
...         hash_token: 'co56co12'
...     },
... 
...     settings_system: {
...         background_path: ''
...     }
... },
... {
...     name: 'César Gaviria',
...     bio: 'Presidente da Colombia',
...     data_register: Date(),
...     avatar_path: '',
... 
...     auth: {
...         username: 'raul',
...         email: 'raul@narcos.com.br',
...         password: 'raul123',
...         last_access: Date(),
...         online: true,
...         disabled: false,
...         hash_token: 'ra186ra89'
...     },
... 
...     settings_system: {
...         background_path: ''
...     }
... },
... {
...     name: 'Elisa',
...     bio: 'Guerilheira delícia',
...     data_register: Date(),
...     avatar_path: '',
... 
...     auth: {
...         username: 'elisa',
...         email: 'elisa@narcos.com.br',
...         password: 'elisa123',
...         last_access: Date(),
...         online: true,
...         disabled: false,
...         hash_token: 'e42e9e32'
...     },
... 
...     settings_system: {
...         background_path: ''
...     }
... },
... {
...     name: 'Poison',
...     bio: 'Um dos campangas de Pablito',
...     data_register: Date(),
...     avatar_path: '',
... 
...     auth: {
...         username: 'poison',
...         email: 'poison@narcos.com.br',
...         password: 'poison123',
...         last_access: Date(),
...         online: true,
...         disabled: false,
...         hash_token: 'p45o12po'
...     },
... 
...     settings_system: {
...         background_path: ''
...     }
... },
... {
...     name: 'Jorge Luis Ochoa',
...     bio: 'Um dos fundadores do cartel de Medellím',
...     data_register: Date(),
...     avatar_path: '',
... 
...     auth: {
...         username: 'jorge',
...         email: 'jorge@narcos.com.br',
...         password: 'jorge123',
...         last_access: Date(),
...         online: true,
...         disabled: false,
...         hash_token: 'jo012sj4'
...     },
... 
...     settings_system: {
...         background_path: ''
...     }
... }])
Inserted 1 record(s) in 1ms
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

**Projects**

```
linux(mongod-3.0.8) projeto-final-mongodb> db.projects.save([{
...     "name": "Encontrar o Pablito",
...     "description": "Capturar Pablito e preende-lo",
...     "date_begin": Date(),
...     "date_dream": Date(),
...     "date_end": Date(),
...     "visible": true,
...     "realocate": false,
...     "expired": false,
...     "visualizable_mod": true,
...     
...     "members": [
...         {
...             "name": "Javier Pena",
...             "type_member": "agente"        
...         },
...         {
...             "name": "Steve Murphy",
...             "type_member": "agente"        
...         },
...         {
...             "name": "Valeria Velez",
...             "type_member": "gostosa"        
...         },
...         {
...             "name": "Connie Murphy",
...             "type_member": "agente"        
...         },
...         {
...             "name": "César Gaviria",
...             "type_member": "presidente"        
...         }
...     ],
...     
...     "tags": ["achar", "procurar", "medellim"],
...     
...     "goals": [{
...         "name": "Tráfico de Drogas",
...         "description": "Acabar com a propagação de cartéis de drogas cocaína em todo mundo",
...         "date_begin": Date(),
...         "date_dream": Date(),
...         "date_end": Date(),
...         "visible": true,
...         "realocate": false,
...         "expired": false,
... 
...         "tags": ["carteis", "drogas", "mundo"],
... 
...         "activities": [
...             {
...                 "name": "Procurar Carteis"
...                 
...             },
...             {
...                 "name": "Procurar Drogas"
...             }
...         ],
... 
...         "goal_hostoric": [
...             {
],
 ...                "date_realocate": Date()
...             }
...         ]
"goa    }]
... },
... 
... {
...     "name": "Matar o Presidente",
...     "description": "Deixar os menino em choque e quem manda é o Pablito",
...     "date_begin": Date(),
...     "date_dream": Date(),
...     "date_end": Date(),
...     "visible": true,
...     "realocate": false,
...     "expired": false,
...     "visualizable_mod": true,
...     
...     "members": [
...         {
...             "name": "Pablo Escobar",
...             "type_member": "chefe"        
...         },
...         {
...             "name": "Valeria Velez",
...             "type_member": "gostosa"        
...         },
...         {
...             "name": "Elisa",
...             "type_member": "guerrilheira"        
...         },
...         {
...             "name": "Poison",
...             "type_member": "capanga"        
...         },
...         {
...             "name": "Jorge Luis Ochoa",
...             "type_member": "traira"        
...         }
...     ],
...     e
...     "tags": [ "drogas", "cartel", "medellim"],
...     
...     "goals": [{
...         "name": "Pró-extradição",
...         "description": "Acabar com a pró-extradição de Pablito",
...         "date_begin": Date(),
...         "date_dream": Date(),
...         "date_end": Date(),
...         "visible": false,
...         "realocate": false,
...         "expired": false,
... 
...         "tags": ["extradicao", "acabar", "liberar"],
... 
...         "activities": [
...             {
...                 "name": "Achar Presidente"                
...             },
...             {
...                 "name": "Colocar Bomba"
...             }
...         ],
... 
...         "goal_hostoric": [
...             {
...                 "date_realocate": Date()
...             }
...         ]
...     }]
... },
... 
... {
...     "name": "Sequestrar a Jornalista",
...     "description": "Sequestra jornalista Diana Turbay, filha do ex-presidente Julio Cesar Turbay",
...     "date_begin": Date(),
...     "date_dream": Date(),
...     "date_end": Date(),
...     "visible": true,
...     "realocate": false,
...     "expired": false,
...     "visualizable_mod": true,
...     
...     "members": [
...         {
...             "name": "Pablo Escobar",
...             "type_member": "chefe"        
...         },
...         {
...             "name": "Jorge Luis Ochoa",
...             "type_member": "traira"        
...         },
...         {
...             "name": "Elisa",
...             "type_member": "guerrilheira"        
...         },
...         {
...             "name": "Poison",
...             "type_member": "capanga"        
...         },
...         {
...             "name": "Gustavo Gaviria",
...             "type_member": "presidente"        
...         }
...     ],
...     
...     "tags": ["revidar", "contra", "politicos", "medellim"],
...     
...     "goals": [{
...         "name": "Moéda de Barganha",
...         "description": "Lutar contra planos de extradição que o Presidente eleito César Gaviria colocou em movimento",
...         "date_begin": Date(),
...         "date_dream": Date(),
...         "date_end": Date(),
...         "visible": false,
...         "realocate": false,
...         "expired": false,
... 
...         "tags": ["extradicao", "barganha", "moeda"],
... 
...         "activities": [
...             {
...                 "name": "Achar Jornalista"
...                 
...             },
...             {
...                 "name": "Esconder"
...             }
...         ],
... 
...         "goal_hostoric": [
...             {
...                 "date_realocate": Date()
...             }
...         ]
...     }]
... },
... 
... {
...     "name": "Combater às drogas",
...     "description": "DEA (Drug Enforcement Administration) – instituição dos EUA responsável pelo combate às drogas",
...     "date_begin": Date(),
...     "date_dream": Date(),
...     "date_end": Date(),
...     "visible": true,
...     "realocate": false,
...     "expired": false,
...     "visualizable_mod": true,
...     
...     "members": [
...         {
...             "name": "Gustavo Gaviria",
...             "type_member": "presidente"        
...         },
...         {
...             "name": "Valeria Velez",
...             "type_member": "gostosa"        
...         },
...         {
...             "name": "Javier Pena",
...             "type_member": ""        
...         },
...         {
...             "name": "Steve Murphy",
...             "type_member": ""        
...         },
...         {
...             "name": "Jorge Luis Ochoa",
...             "type_member": "traira"        
...         }
...     ],
...     
...     "tags": ["combater", "drogas", "cartel"],
...     
...     "goals": [{
...         "name": "Acabar com o Tráfico",
...         "description": "Acabar com a festa do pablito",
...         "date_begin": Date(),
...         "date_dream": Date(),
...         "date_end": Date(),
...         "visible": false,
...         "realocate": false,
...         "expired": false,
... 
...         "tags": ["extradicao", "acabar", "liberar"],
... 
...         "activities": [],
... 
...         "goal_hostoric": [
...             {
...                 "date_realocate": Date()
...             }
...         ]
...     }]
... },
... 
... {
...     "name": "Construir La Catedral",
...     "description": "A prisão de pablito o rei da Cocaína",
...     "date_begin": Date(),
...     "date_dream": Date(),
...     "date_end": Date(),
...     "visible": false,
...     "realocate": false,
...     "expired": false,
...     "visualizable_mod": true,
...     
...     "members": [
...         {
...             "name": "Pablo Escobar",
...             "type_member": "chefe"        
...         },
...         {
...             "name": "Gustavo Gaviria",
...             "type_member": "presidente"        
...         },
...         {
...             "name": "Valeria Velez",
...             "type_member": "gostosa"        
...         },
...         {
...             "name": "Poison",
...             "type_member": "capanga"        
...         },
...         {
...             "name": "Jorge Luis Ochoa",
...             "type_member": "traira"        
...         }
...     ],
...     
...     "tags": ["catedral", "prisao", "cartel", "drogas"],
...     
...     "goals": [{
...         "name": "Refúgio",
...         "description": "O local foi refúgio de Pablito",
...         "date_begin": Date(),
...         "date_dream": Date(),
...         "date_end": Date(),
...         "visible": false,
...         "realocate": false,
...         "expired": false,
... 
...         "tags": ["refugio", "casa", "liberdade"],
... 
...         "activities": [
...             {
...                 "name": "Escolher local",
...                 
...             },
...             {
...                 "name": "Construir"
...             }
...         ],
... 
...         "goal_hostoric": [
...             {
...                 "date_realocate": Date()
...             }
...         ]
...     }]
... }])
Inserted 1 record(s) in 7ms
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

**1. Liste as informações dos membros de 1 projeto específico que deve ser buscado pelo seu nome de forma a não ligar para maiúsculas e minúsculas.**

```js
linux(mongod-3.0.8) projeto-final-mongodb> var members = []
linux(mongod-3.0.8) projeto-final-mongodb> var getMembers = function(id){ members.push(db.users.findOne({name: id.name})) }
linux(mongod-3.0.8) projeto-final-mongodb> var project = db.projects.findOne({"name": /Encontrar o Pablito/i})
linux(mongod-3.0.8) projeto-final-mongodb> project.members.forEach(getMembers)
linux(mongod-3.0.8) projeto-final-mongodb> members
[
  {
    "_id": ObjectId("567809c78b6641508227e892"),
    "name": "Javier Pena",
    "bio": "Agente da DEA louco pra pegar o pablito e sua rapaziada",
    "data_register": "Mon Dec 21 2015 12:16:39 GMT-0200 (BRST)",
    "avatar_path": "",
    "auth": {
      "username": "javier",
      "email": "javier@narcos.com.br",
      "password": "javier123",
      "last_access": "Mon Dec 21 2015 12:16:39 GMT-0200 (BRST)",
      "online": true,
      "disabled": false,
      "hash_token": "89ad98f"
    },
    "settings_system": {
      "background_path": ""
    }
  },
  {
    "_id": ObjectId("567809c78b6641508227e893"),
    "name": "Steve Murphy",
    "bio": "Agente da DEA parceiro de Javier",
    "data_register": "Mon Dec 21 2015 12:16:39 GMT-0200 (BRST)",
    "avatar_path": "",
    "auth": {
      "username": "steve",
      "email": "steve@narcos.com.br",
      "password": "steve123",
      "last_access": "Mon Dec 21 2015 12:16:39 GMT-0200 (BRST)",
      "online": true,
      "disabled": false,
      "hash_token": "12ar7e8"
    },
    "settings_system": {
      "background_path": ""
    }
  },
  {
    "_id": ObjectId("567809c78b6641508227e895"),
    "name": "Valeria Velez",
    "bio": "Reporter danada, amante de pablito",
    "data_register": "Mon Dec 21 2015 12:16:39 GMT-0200 (BRST)",
    "avatar_path": "",
    "auth": {
      "username": "valeria",
      "email": "valeria@narcos.com.br",
      "password": "valeria123",
      "last_access": "Mon Dec 21 2015 12:16:39 GMT-0200 (BRST)",
      "online": true,
      "disabled": false,
      "hash_token": "v7a97a95a"
    },
    "settings_system": {
      "background_path": ""
    }
  },
  {
    "_id": ObjectId("567809c78b6641508227e896"),
    "name": "Connie Murphy",
    "bio": "Esposa do Policial Steve Murphy",
    "data_register": "Mon Dec 21 2015 12:16:39 GMT-0200 (BRST)",
    "avatar_path": "",
    "auth": {
      "username": "connie",
      "email": "connie@narcos.com.br",
      "password": "connie123",
      "last_access": "Mon Dec 21 2015 12:16:39 GMT-0200 (BRST)",
      "online": true,
      "disabled": false,
      "hash_token": "co56co12"
    },
    "settings_system": {
      "background_path": ""
    }
  },
  {
    "_id": ObjectId("567809c78b6641508227e897"),
    "auth": {
      "username": "cesar",
      "email": "cesar@narcos.com.br",
      "password": "cesar123",
      "last_access": "Tue Dec 22 2015 17:55:12 GMT-0200 (BRST)",
      "online": true,
      "disabled": false,
      "hash_token": "ce186ce89"
    },
    "name": "César Gaviria",
    "bio": "Presidente da Colombia",
    "data_register": "Tue Dec 22 2015 17:55:12 GMT-0200 (BRST)",
    "avatar_path": "",
    "settings_system": {
      "background_path": ""
    }
  }
]
```

**2. Liste todos os projetos com a tag que você escolheu para os 3 projetos em comum.**

```js
linux(mongod-3.0.8) projeto-final-mongodb> db.projects.find({"tags": /medellim/i})
{
  "_id": ObjectId("5679f767bd1984e398526f81"),
  "name": "Encontrar o Pablito",
  "description": "Capturar Pablito e preende-lo",
  "date_begin": "Tue Dec 22 2015 23:22:47 GMT-0200 (BRST)",
  "date_dream": "Tue Dec 22 2015 23:22:47 GMT-0200 (BRST)",
  "date_end": "Tue Dec 22 2015 23:22:47 GMT-0200 (BRST)",
  "visible": true,
  "realocate": false,
  "expired": false,
  "visualizable_mod": true,
  "members": [
    {
      "name": "Javier Pena",
      "type_member": "agente"
    },
    {
      "name": "Steve Murphy",
      "type_member": "agente"
    },
    {
      "name": "Valeria Velez",
      "type_member": "gostosa"
    },
    {
      "name": "Connie Murphy",
      "type_member": "agente"
    },
    {
      "name": "César Gaviria",
      "type_member": "presidente"
    }
  ],
  "tags": [
    "achar",
    "procurar",
    "medellim"
  ],
  "goals": [
    {
      "name": "Tráfico de Drogas",
      "description": "Acabar com a propagação de cartéis de drogas cocaína em todo mundo",
      "date_begin": "Tue Dec 22 2015 23:22:47 GMT-0200 (BRST)",
      "date_dream": "Tue Dec 22 2015 23:22:47 GMT-0200 (BRST)",
      "date_end": "Tue Dec 22 2015 23:22:47 GMT-0200 (BRST)",
      "visible": true,
      "realocate": false,
      "expired": false,
      "tags": [
        "carteis",
        "drogas",
        "mundo"
      ],
      "activities": [
        {
          "name": "Procurar Carteis"
        },
        {
          "name": "Procurar Drogas"
        }
      ],
      "goal_hostoric": [
        {
          "date_realocate": "Tue Dec 22 2015 23:22:47 GMT-0200 (BRST)"
        }
      ]
    }
  ]
}
{
  "_id": ObjectId("5679f767bd1984e398526f82"),
  "name": "Matar o Presidente",
  "description": "Deixar os menino em choque e quem manda é o Pablito",
  "date_begin": "Tue Dec 22 2015 23:22:47 GMT-0200 (BRST)",
  "date_dream": "Tue Dec 22 2015 23:22:47 GMT-0200 (BRST)",
  "date_end": "Tue Dec 22 2015 23:22:47 GMT-0200 (BRST)",
  "visible": true,
  "realocate": false,
  "expired": false,
  "visualizable_mod": true,
  "members": [
    {
      "name": "Pablo Escobar",
      "type_member": "chefe"
    },
    {
      "name": "Valeria Velez",
      "type_member": "gostosa"
    },
    {
      "name": "Elisa",
      "type_member": "guerrilheira"
    },
    {
      "name": "Poison",
      "type_member": "capanga"
    },
    {
      "name": "Jorge Luis Ochoa",
      "type_member": "traira"
    }
  ],
  "tags": [
    "drogas",
    "cartel",
    "medellim"
  ],
  "goals": [
    {
      "name": "Pró-extradição",
      "description": "Acabar com a pró-extradição de Pablito",
      "date_begin": "Tue Dec 22 2015 23:22:47 GMT-0200 (BRST)",
      "date_dream": "Tue Dec 22 2015 23:22:47 GMT-0200 (BRST)",
      "date_end": "Tue Dec 22 2015 23:22:47 GMT-0200 (BRST)",
      "visible": false,
      "realocate": false,
      "expired": false,
      "tags": [
        "extradicao",
        "acabar",
        "liberar"
      ],
      "activities": [
        {
          "name": "Achar Presidente"
        },
        {
          "name": "Colocar Bomba"
        }
      ],
      "goal_hostoric": [
        {
          "date_realocate": "Tue Dec 22 2015 23:22:47 GMT-0200 (BRST)"
        }
      ]
    }
  ]
}
{
  "_id": ObjectId("5679f767bd1984e398526f83"),
  "name": "Sequestrar a Jornalista",
  "description": "Sequestra jornalista Diana Turbay, filha do ex-presidente Julio Cesar Turbay",
  "date_begin": "Tue Dec 22 2015 23:22:47 GMT-0200 (BRST)",
  "date_dream": "Tue Dec 22 2015 23:22:47 GMT-0200 (BRST)",
  "date_end": "Tue Dec 22 2015 23:22:47 GMT-0200 (BRST)",
  "visible": true,
  "realocate": false,
  "expired": false,
  "visualizable_mod": true,
  "members": [
    {
      "name": "Pablo Escobar",
      "type_member": "chefe"
    },
    {
      "name": "Jorge Luis Ochoa",
      "type_member": "traira"
    },
    {
      "name": "Elisa",
      "type_member": "guerrilheira"
    },
    {
      "name": "Poison",
      "type_member": "capanga"
    },
    {
      "name": "Gustavo Gaviria",
      "type_member": "presidente"
    }
  ],
  "tags": [
    "revidar",
    "contra",
    "politicos",
    "medellim"
  ],
  "goals": [
    {
      "name": "Moéda de Barganha",
      "description": "Lutar contra planos de extradição que o Presidente eleito César Gaviria colocou em movimento",
      "date_begin": "Tue Dec 22 2015 23:22:47 GMT-0200 (BRST)",
      "date_dream": "Tue Dec 22 2015 23:22:47 GMT-0200 (BRST)",
      "date_end": "Tue Dec 22 2015 23:22:47 GMT-0200 (BRST)",
      "visible": false,
      "realocate": false,
      "expired": false,
      "tags": [
        "extradicao",
        "barganha",
        "moeda"
      ],
      "activities": [
        {
          "name": "Achar Jornalista"
        },
        {
          "name": "Esconder"
        }
      ],
      "goal_hostoric": [
        {
          "date_realocate": "Tue Dec 22 2015 23:22:47 GMT-0200 (BRST)"
        }
      ]
    }
  ]
}
Fetched 3 record(s) in 12ms

```

**3. Liste apenas os nomes de todas as atividades para todos os projetos.**

```js
linux(mongod-3.0.8) projeto-final-mongodb> var atividades = []
linux(mongod-3.0.8) projeto-final-mongodb> db.projects.find({},{"goals.activities": 1,  _id: 0}).forEach(function(item){ 
... var activity = item.goals[0].activities
... activity.forEach(function(data){
... atividades.push(data.name);
... })
... })
linux(mongod-3.0.8) projeto-final-mongodb> atividades.sort()
[
  "Achar Jornalista",
  "Achar Presidente",
  "Colocar Bomba",
  "Construir",
  "Escolher local",
  "Esconder",
  "Procurar Carteis",
  "Procurar Drogas"
]

//ou

linux(mongod-3.0.8) projeto-final-mongodb> db.projects.distinct("goals.activities.name").sort()
[
  "Achar Jornalista",
  "Achar Presidente",
  "Colocar Bomba",
  "Construir",
  "Escolher local",
  "Esconder",
  "Procurar Carteis",
  "Procurar Drogas"
]

```

**4. Liste todos os projetos que não possuam uma tag.**

```js
linux(mongod-3.0.8) projeto-final-mongodb> var query = { tags: {$not: /medellim/i} }
linux(mongod-3.0.8) projeto-final-mongodb> var fields = { name: 1, tags: 1}
linux(mongod-3.0.8) projeto-final-mongodb> db.projects.find(query, fields)
{
  "_id": ObjectId("5679f767bd1984e398526f84"),
  "name": "Combater às drogas",
  "tags": [
    "combater",
    "drogas",
    "cartel",
    "tag4"
  ]
}
{
  "_id": ObjectId("5679f767bd1984e398526f85"),
  "name": "Construir La Catedral",
  "tags": [
    "catedral",
    "prisao",
    "cartel",
    "drogas",
    "tag5"
  ]
}
Fetched 2 record(s) in 0ms

```

**5. Liste todos os usuários que não fazem parte do primeiro projeto cadastrado.**

```js

linux(mongod-3.0.8) projeto-final-mongodb> var usuarios = [];
linux(mongod-3.0.8) projeto-final-mongodb> db.users.find().forEach( function(user) { 
... 
... var project = db.projects.findOne({name: /Encontrar o Pablito/i, "members.name": user.name}, {members: 1, _id: 0})
... 
... if(project == null){
... usuarios.push(user.name);
... }
... });
linux(mongod-3.0.8) projeto-final-mongodb> usuarios
[
  "Pablo Escobar",
  "Gustavo Gaviria",
  "Elisa",
  "Jorge Luis Ochoa",
  "Poison"
]


```

## Update - alteração

**1. Adicione para todos os projetos o campo `views: 0.`**

```js
linux(mongod-3.0.8) projeto-final-mongodb> var query = {}
linux(mongod-3.0.8) projeto-final-mongodb> var mod = {$set: {views: 0}}
linux(mongod-3.0.8) projeto-final-mongodb> var options = {multi: true}
linux(mongod-3.0.8) projeto-final-mongodb> db.projects.update(query, mod, options)
Updated 5 existing record(s) in 12ms
WriteResult({
  "nMatched": 5,
  "nUpserted": 0,
  "nModified": 5
})
```

**2. Adicione 1 tag diferente para cada projeto.**

```js
linux(mongod-3.0.8) projeto-final-mongodb> var query = { name: /Encontrar o Pablito/i} 
linux(mongod-3.0.8) projeto-final-mongodb> var mod = {$push: { tags: 'tag1' }}
linux(mongod-3.0.8) projeto-final-mongodb> db.projects.update(query, mod)
Updated 1 existing record(s) in 3ms
WriteResult({
  "nMatched": 1,
  "nUpserted": 0,
  "nModified": 1
})

linux(mongod-3.0.8) projeto-final-mongodb> var query = { name: /Matar o Presidente/i}
linux(mongod-3.0.8) projeto-final-mongodb> var mod = {$push: { tags: 'tag2' }}
linux(mongod-3.0.8) projeto-final-mongodb> db.projects.update(query, mod)
Updated 1 existing record(s) in 3ms
WriteResult({
  "nMatched": 1,
  "nUpserted": 0,
  "nModified": 1
})

linux(mongod-3.0.8) projeto-final-mongodb> var query = { name: /Sequestrar a Jornalista/i}
linux(mongod-3.0.8) projeto-final-mongodb> var mod = {$push: { tags: 'tag3' }}
linux(mongod-3.0.8) projeto-final-mongodb> db.projects.update(query, mod)
Updated 1 existing record(s) in 3ms
WriteResult({
  "nMatched": 1,
  "nUpserted": 0,
  "nModified": 1
})

linux(mongod-3.0.8) projeto-final-mongodb> var query = { name: /Combater às drogas/i}
linux(mongod-3.0.8) projeto-final-mongodb> var mod = {$push: { tags: 'tag4' }}
linux(mongod-3.0.8) projeto-final-mongodb> db.projects.update(query, mod)
Updated 1 existing record(s) in 3ms
WriteResult({
  "nMatched": 1,
  "nUpserted": 0,
  "nModified": 1
})

linux(mongod-3.0.8) projeto-final-mongodb> var query = { name: /Construir La Catedral/i}
linux(mongod-3.0.8) projeto-final-mongodb> var mod = {$push: { tags: 'tag5' }}
linux(mongod-3.0.8) projeto-final-mongodb> db.projects.update(query, mod)
Updated 1 existing record(s) in 2ms
WriteResult({
  "nMatched": 1,
  "nUpserted": 0,
  "nModified": 1
})
```

**3. Adicione 2 membros diferentes para cada projeto.**

```js
linux(mongod-3.0.8) projeto-final-mongodb> var query = { name: /Encontrar o Pablito/i} 
linux(mongod-3.0.8) projeto-final-mongodb> var mod = { $pushAll: 
... { members:
... [
... {
...             "name": "Pablo Escobar",
...             "type_member": "chefe"        
...         },
...         {
...             "name": "Gustavo Gaviria",
...             "type_member": "primo"        
...         }
... ]
... }
... }
linux(mongod-3.0.8) projeto-final-mongodb> db.projects.update(query, mod)
Updated 1 existing record(s) in 12ms
WriteResult({
  "nMatched": 1,
  "nUpserted": 0,
  "nModified": 1
})

linux(mongod-3.0.8) projeto-final-mongodb> var query = { name: /Matar o Presidente/i} 
linux(mongod-3.0.8) projeto-final-mongodb> var mod = { $pushAll: 
... { members:
... [
... {
...             "name": "César Gaviria",
...             "type_member": "presidente"        
...         },
...         {
...             "name": "Gustavo Gaviria",
...             "type_member": "primo"        
...         }
... ]
... }
... }
linux(mongod-3.0.8) projeto-final-mongodb> db.projects.update(query, mod)
Updated 1 existing record(s) in 3ms
WriteResult({
  "nMatched": 1,
  "nUpserted": 0,
  "nModified": 1
})

linux(mongod-3.0.8) projeto-final-mongodb> var query = { name: /Sequestrar a Jornalista/i} 
linux(mongod-3.0.8) projeto-final-mongodb> var mod = { $pushAll: 
... { members:
... [
... {
...             "name": "Steve Murphy",
...             "type_member": "agente"        
...         },
...         {
...             "name": "Connie Murphy",
...             "type_member": "agente"        
...         }
... ]
... }
... }
linux(mongod-3.0.8) projeto-final-mongodb> db.projects.update(query, mod)
Updated 1 existing record(s) in 3ms
WriteResult({
  "nMatched": 1,
  "nUpserted": 0,
  "nModified": 1
})

linux(mongod-3.0.8) projeto-final-mongodb> var query = { name: /Combater às drogas/i} 
linux(mongod-3.0.8) projeto-final-mongodb> var mod = { $pushAll: 
... { members:
... [
... {
...             "name": "César Gaviria",
...             "type_member": "presidente"        
...         },
...         {
...             "name": "Poison",
...             "type_member": "capanga"        
...         }
... ]
... }
... }
linux(mongod-3.0.8) projeto-final-mongodb> db.projects.update(query, mod)
Updated 1 existing record(s) in 2ms
WriteResult({
  "nMatched": 1,
  "nUpserted": 0,
  "nModified": 1
})

linux(mongod-3.0.8) projeto-final-mongodb> var query = { name: /Construir La Catedral/i} 
linux(mongod-3.0.8) projeto-final-mongodb> var mod = { $pushAll: 
... { members:
... [
... {
...             "name": "César Gaviria",
...             "type_member": "presidente"        
...         },
...         {   
...             "name": "Steve Murphy",
...             "type_member": "agente"        
...         }
... ]
... }
... }
linux(mongod-3.0.8) projeto-final-mongodb> db.projects.update(query, mod)
Updated 1 existing record(s) in 3ms
WriteResult({
  "nMatched": 1,
  "nUpserted": 0,
  "nModified": 1
})

```

**4. Adicione 1 comentário em cada atividade, deixe apenas 1 projeto sem.**

```js
linux(mongod-3.0.8) projeto-final-mongodb> var query = {"name": "Procurar Carteis"}
linux(mongod-3.0.8) projeto-final-mongodb> var mod = { $push : { comments :
... {
...         "text": "Procura em todo lugar",
...         "date_comment": Date(),
...         "members": [],
...         "files": [
...             { "path": "", "weight": "", "name": "" }
...         ]
...     }
... }}
linux(mongod-3.0.8) projeto-final-mongodb> db.activities.update(query, mod)
Updated 1 existing record(s) in 2ms
WriteResult({
  "nMatched": 1,
  "nUpserted": 0,
  "nModified": 1
})

linux(mongod-3.0.8) projeto-final-mongodb> var query = {"name": "Procurar Drogas"}
linux(mongod-3.0.8) projeto-final-mongodb> var mod = { $push : { comments :
... {
...         "text": "Procura na casa do Pablito",
...         "date_comment": Date(),
...         "members": [],
...         "files": [
...             { "path": "", "weight": "", "name": "" }
...         ]
...     }
... }}
linux(mongod-3.0.8) projeto-final-mongodb> db.activities.update(query, mod)
Updated 1 existing record(s) in 3ms
WriteResult({
  "nMatched": 1,
  "nUpserted": 0,
  "nModified": 1
})

linux(mongod-3.0.8) projeto-final-mongodb> var query = {"name": "Achar Presidente"}
linux(mongod-3.0.8) projeto-final-mongodb> var mod = { $push : { comments :
... {
...         "text": "Acha logo esse presidente",
...         "date_comment": Date(),
...         "members": [],
...         "files": [
...             { "path": "", "weight": "", "name": "" }
...         ]
...     }
... }}
linux(mongod-3.0.8) projeto-final-mongodb> db.activities.update(query, mod)
Updated 1 existing record(s) in 3ms
WriteResult({
  "nMatched": 1,
  "nUpserted": 0,
  "nModified": 1
})

linux(mongod-3.0.8) projeto-final-mongodb> var query = {"name": "Colocar Bomba"}
linux(mongod-3.0.8) projeto-final-mongodb> var mod = { $push : { comments :
... {
...         "text": "Coloca logo essa bomba no avião",
...         "date_comment": Date(),
...         "members": [],
...         "files": [
...             { "path": "", "weight": "", "name": "" }
...         ]
...     }
... }}
linux(mongod-3.0.8) projeto-final-mongodb> db.activities.update(query, mod)
Updated 1 existing record(s) in 2ms
WriteResult({
  "nMatched": 1,
  "nUpserted": 0,
  "nModified": 1
})

linux(mongod-3.0.8) projeto-final-mongodb> var query = {"name": "Achar Jornalista"}
linux(mongod-3.0.8) projeto-final-mongodb> var mod = { $push : { comments :
... {
...         "text": "Acha essa jornalista gostosa",
...         "date_comment": Date(),
...         "members": [],
...         "files": [
...             { "path": "", "weight": "", "name": "" }
...         ]
...     }
... }}
linux(mongod-3.0.8) projeto-final-mongodb> db.activities.update(query, mod)
Updated 1 existing record(s) in 1ms
WriteResult({
  "nMatched": 1,
  "nUpserted": 0,
  "nModified": 1
})

linux(mongod-3.0.8) projeto-final-mongodb> var query = {"name": "Escolher local"}
linux(mongod-3.0.8) projeto-final-mongodb> var mod = { $push : { comments :
... {
...         "text": "Já achou o local ?",
...         "date_comment": Date(),
...         "members": [],
...         "files": [
...             { "path": "", "weight": "", "name": "" }
...         ]
...     }
... }}
linux(mongod-3.0.8) projeto-final-mongodb> db.activities.update(query, mod)
Updated 1 existing record(s) in 5ms
WriteResult({
  "nMatched": 1,
  "nUpserted": 0,
  "nModified": 1
})

linux(mongod-3.0.8) projeto-final-mongodb> var query = {"name": "Construir"}
linux(mongod-3.0.8) projeto-final-mongodb> var mod = { $push : { comments :
... {
...         "text": "Quando começa a construir La Catedral",
...         "date_comment": Date(),
...         "members": [],
...         "files": [
...             { "path": "", "weight": "", "name": "" }
...         ]
...     }
... }}
linux(mongod-3.0.8) projeto-final-mongodb> db.activities.update(query, mod)
Updated 1 existing record(s) in 3ms
WriteResult({
  "nMatched": 1,
  "nUpserted": 0,
  "nModified": 1
})

```

**5. Adicione 1 projeto inteiro com UPSERT.**

```js
linux(mongod-3.0.8) projeto-final-mongodb> var query = { name: "Entrar para a política"}
linux(mongod-3.0.8) projeto-final-mongodb> var options = {upsert: true}
linux(mongod-3.0.8) projeto-final-mongodb> var mod = { $set: {
...     "description": "Capturar Pablito e preende-lo",
...     "date_begin": Date(),
...     "date_dream": Date(),
...     "date_end": Date(),
...     "visible": true,
...     "realocate": false,
...     "expired": false,
...     "visualizable_mod": true,
...     
...     "members": [
...         {
...             "name": "Pablo Escobar",
...             "type_member": "chefe"        
...         }
...     ],
...     
...     "tags": ["politica", "pais", "medellim"],
...     
...     "goals": [{
...         "name": "Facilitar o trafico de Drogas",
...         "description": "",
...         "date_begin": Date(),
...         "date_dream": Date(),
...         "date_end": Date(),
...         "visible": true,
...         "realocate": false,
...         "expired": false,
... 
...         "tags": ["carteis", "drogas", "mundo", "trafico"],
... 
...         "activities": [
...             {
...                 "name": "Esconder"
...                 
...             }
...         ],
... 
...         "goal_hostoric": [
...             {
...                 "date_realocate": Date()
...             }
...         ]
...     }]
... }}
linux(mongod-3.0.8) projeto-final-mongodb> db.projects.update(query, mod, options);
Updated 1 new record(s) in 3ms
WriteResult({
  "nMatched": 0,
  "nUpserted": 1,
  "nModified": 0,
  "_id": ObjectId("568055a784dab472469a543f")
})

```

## Delete - remoção

**1. Apague todos os projetos que não possuam tags.**

```js
linux(mongod-3.0.8) projeto-final-mongodb> var query = {tags: {$eq: [] } }
linux(mongod-3.0.8) projeto-final-mongodb> db.projects.remove(query)
Removed 0 record(s) in 2ms
WriteResult({
  "nRemoved": 0
})
```

**2. Apague todos os projetos que não possuam comentários nas atividades.**

```js
linux(mongod-3.0.8) projeto-final-mongodb> db.activities.find({"comments.text": {$exists: false} }, {name: 1, _id: 0}).forEach(function(item){
...     db.projects.remove({"goals.activities.name": { $eq: item.name } })
... })
Removed 2 record(s) in 7ms
linux(mongod-3.0.8) projeto-final-mongodb> db.projects.count()
4

```

**3. Apague todos os projetos que não possuam atividades.**

```js
db.projects.remove({"goals.activities": [] })

//ou

db.projects.find({"goals.activities.name": { $exists : false } })

```

**4. Escolha 2 usuário e apague todos os projetos em que os 2 fazem parte.**

```js
linux(mongod-3.0.8) projeto-final-mongodb> db.projects.remove({"members.name": {$in: [/Elisa/i, /Connie Murphy/i]}})
Removed 2 record(s) in 3ms
WriteResult({
  "nRemoved": 2
})
```

**5. Apague todos os projetos que possuam uma determinada tag em goal.**

```js
linux(mongod-3.0.8) projeto-final-mongodb> db.projects.remove({"goals.tags": /extradicao/i })
Removed 1 record(s) in 10ms
WriteResult({
  "nRemoved": 1
})
```

## Gerenciamento de usuários

**1. Crie um usuário com permissões APENAS de Leitura.**

```js
linux(mongod-3.0.8) projeto-final-mongodb> db.createUser(
...   {
...     user: "UserRead",
...     pwd: "read123",
...     roles: [ { role: "read", db: "projeto-final-mongodb" } ]
...   }
... )
Successfully added user: {
  "user": "UserRead",
  "roles": [
    {
      "role": "read",
      "db": "projeto-final-mongodb"
    }
  ]
}
```

**2. Crie um usuário com permissões de Escrita e Leitura.**

```js
linux(mongod-3.0.8) projeto-final-mongodb> db.createUser(
...   {
...     user: "UserReadWrite",
...     pwd: "readwrite123",
...     roles: [ { role: "readWrite", db: "projeto-final-mongodb" } ]
...   }
... )
Successfully added user: {
  "user": "UserReadWrite",
  "roles": [
    {
      "role": "readWrite",
      "db": "projeto-final-mongodb"
    }
  ]
}
```


**3. Adicionar o papel `grantRolesToUser` e `revokeRole` para o usuário com Escrita e Leitura.**

```js

// grantRolesToUser

linux(mongod-3.0.8) projeto-final-mongodb> db.createRole( 
...   { 
...     role : "grantRolesToUser",
... 
...     roles : [
...       { role: "readWrite", db: "projeto-final-mongodb" }
...     ],
... 
...     privileges : [
...       { resource : { db: "projeto-final-mongodb", collection: "" }, actions : ["grantRole"] }
...     ]
...   }
... );
{
  "role": "grantRolesToUser",
  "roles": [
    {
      "role": "readWrite",
      "db": "projeto-final-mongodb"
    }
  ],
  "privileges": [
    {
      "resource": {
        "db": "projeto-final-mongodb",
        "collection": ""
      },
      "actions": [
        "grantRole"
      ]
    }
  ]
}

// revokeRole

linux(mongod-3.0.8) projeto-final-mongodb> db.createRole( 
...   { 
...     role : "revokeRole",
... 
...     roles : [
...       { role: "readWrite", db: "projeto-final-mongodb" }
...     ],
... 
...     privileges : [
...       { resource : { db: "projeto-final-mongodb", collection: "" }, actions : ["grantRole"] }
...     ]
...   }
... );
{
  "role": "revokeRole",
  "roles": [
    {
      "role": "readWrite",
      "db": "projeto-final-mongodb"
    }
  ],
  "privileges": [
    {
      "resource": {
        "db": "projeto-final-mongodb",
        "collection": ""
      },
      "actions": [
        "grantRole"
      ]
    }
  ]
}

// Adicionar papeis

linux(mongod-3.0.8) projeto-final-mongodb> db.runCommand( { grantRolesToUser: "UserReadWrite",
...   roles: [
...     { role: "grantRolesToUser", db: "projeto-final-mongodb"},
...     { role: "revokeRole", db: "projeto-final-mongodb"},
...     "readWrite"
...   ],
...   writeConcern: { w: "majority" , wtimeout: 2000 }
...  })
{
  "ok": 1
}

```

**4. Remover o papel `grantRolesToUser` para o usuário com Escrita e Leitura.**

```js
linux(mongod-3.0.8) projeto-final-mongodb> db.runCommand( { revokeRolesFromUser: "UserReadWrite",
...   roles: [
...           { role: "grantRolesToUser", db: "projeto-final-mongodb" },
...           "readWrite"
...   ],
...   writeConcern: { w: "majority" }
... })
{
  "ok": 1
}
```

**5. Listar todos os usuários com seus papéis e ações.**

```js
linux(mongod-3.0.8) projeto-final-mongodb> db.runCommand({ usersInfo: 
...     [
...         { user: "UserRead", db: "projeto-final-mongodb" }, 
...         { user: "UserReadWrite", db: "projeto-final-mongodb" }
...     ],
...   showCredentials: true,
...   showPrivileges: true
... })
{
  "users": [
    {
      "_id": "projeto-final-mongodb.UserRead",
      "user": "UserRead",
      "db": "projeto-final-mongodb",
      "credentials": {
        "SCRAM-SHA-1": {
          "iterationCount": 10000,
          "salt": "d5gi/OAbvCiJM/YfetsMVg==",
          "storedKey": "g+yDBPpMtsk5Rpd58axdWO8V4mI=",
          "serverKey": "Oyp+oF5FyvS0XLyzdaEh7XZAs5A="
        }
      },
      "roles": [
        {
          "role": "read",
          "db": "projeto-final-mongodb"
        }
      ],
      "inheritedRoles": [
        {
          "role": "read",
          "db": "projeto-final-mongodb"
        }
      ],
      "inheritedPrivileges": [
        {
          "resource": {
            "db": "projeto-final-mongodb",
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
            "db": "projeto-final-mongodb",
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
            "db": "projeto-final-mongodb",
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
            "db": "projeto-final-mongodb",
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
      "_id": "projeto-final-mongodb.UserReadWrite",
      "user": "UserReadWrite",
      "db": "projeto-final-mongodb",
      "credentials": {
        "SCRAM-SHA-1": {
          "iterationCount": 10000,
          "salt": "VhlUFeOBRfDI9H1X0TAIOQ==",
          "storedKey": "2G5Wfg+KcV6MGMe+Whg5T8+kZRo=",
          "serverKey": "lYtojicq16HnoFvS4ZjB+7YTI3E="
        }
      },
      "roles": [
        {
          "role": "revokeRole",
          "db": "projeto-final-mongodb"
        }
      ],
      "inheritedRoles": [
        {
          "role": "readWrite",
          "db": "projeto-final-mongodb"
        },
        {
          "role": "revokeRole",
          "db": "projeto-final-mongodb"
        }
      ],
      "inheritedPrivileges": [
        {
          "resource": {
            "db": "projeto-final-mongodb",
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
            "db": "projeto-final-mongodb",
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
            "db": "projeto-final-mongodb",
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
            "db": "projeto-final-mongodb",
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
// coloque aqui todos os comandos que você executou

```js

// Configdb

junior@linux:~$ mkdir /data/configdb
junior@linux:~$ mongod --configsvr --port 27010
2015-12-28T22:17:21.573-0200 I CONTROL  [initandlisten] MongoDB starting : pid=8155 port=27010 dbpath=/data/configdb master=1 64-bit host=linux
2015-12-28T22:17:21.573-0200 I CONTROL  [initandlisten] db version v3.0.8
2015-12-28T22:17:21.573-0200 I CONTROL  [initandlisten] git version: 83d8cc25e00e42856924d84e220fbe4a839e605d
2015-12-28T22:17:21.573-0200 I CONTROL  [initandlisten] build info: Linux ip-10-187-89-126 3.13.0-24-generic #46-Ubuntu SMP Thu Apr 10 19:11:08 UTC 2014 x86_64 BOOST_LIB_VERSION=1_49
2015-12-28T22:17:21.573-0200 I CONTROL  [initandlisten] allocator: tcmalloc
2015-12-28T22:17:21.573-0200 I CONTROL  [initandlisten] options: { net: { port: 27010 }, sharding: { clusterRole: "configsvr" } }
2015-12-28T22:17:21.594-0200 I JOURNAL  [initandlisten] journal dir=/data/configdb/journal
2015-12-28T22:17:21.594-0200 I JOURNAL  [initandlisten] recover : no journal files present, no recovery needed
2015-12-28T22:17:23.392-0200 I JOURNAL  [initandlisten] preallocateIsFaster=true 16.98
2015-12-28T22:17:25.068-0200 I JOURNAL  [initandlisten] preallocateIsFaster=true 14.44
2015-12-28T22:17:27.693-0200 I JOURNAL  [initandlisten] preallocateIsFaster=true 13.76
2015-12-28T22:17:27.693-0200 I JOURNAL  [initandlisten] preallocateIsFaster check took 6.098 secs
2015-12-28T22:17:27.693-0200 I JOURNAL  [initandlisten] preallocating a journal file /data/configdb/journal/prealloc.0
2015-12-28T22:17:28.508-0200 I JOURNAL  [initandlisten] preallocating a journal file /data/configdb/journal/prealloc.1
2015-12-28T22:17:29.300-0200 I JOURNAL  [initandlisten] preallocating a journal file /data/configdb/journal/prealloc.2
2015-12-28T22:17:30.142-0200 I JOURNAL  [durability] Durability thread started
2015-12-28T22:17:30.143-0200 I JOURNAL  [journal writer] Journal writer thread started
2015-12-28T22:17:30.168-0200 I CONTROL  [initandlisten] 
2015-12-28T22:17:30.168-0200 I CONTROL  [initandlisten] ** WARNING: /sys/kernel/mm/transparent_hugepage/enabled is 'always'.
2015-12-28T22:17:30.168-0200 I CONTROL  [initandlisten] **        We suggest setting it to 'never'
2015-12-28T22:17:30.168-0200 I CONTROL  [initandlisten] 
2015-12-28T22:17:30.168-0200 I CONTROL  [initandlisten] ** WARNING: /sys/kernel/mm/transparent_hugepage/defrag is 'always'.
2015-12-28T22:17:30.168-0200 I CONTROL  [initandlisten] **        We suggest setting it to 'never'
2015-12-28T22:17:30.168-0200 I CONTROL  [initandlisten] 
2015-12-28T22:17:30.169-0200 I INDEX    [initandlisten] allocating new ns file /data/configdb/local.ns, filling with zeroes...
2015-12-28T22:17:30.301-0200 I STORAGE  [FileAllocator] allocating new datafile /data/configdb/local.0, filling with zeroes...
2015-12-28T22:17:30.301-0200 I STORAGE  [FileAllocator] creating directory /data/configdb/_tmp
2015-12-28T22:17:30.335-0200 I STORAGE  [FileAllocator] done allocating datafile /data/configdb/local.0, size: 16MB,  took 0.016 secs
2015-12-28T22:17:30.347-0200 I REPL     [initandlisten] ******
2015-12-28T22:17:30.347-0200 I REPL     [initandlisten] creating replication oplog of size: 5MB...
2015-12-28T22:17:30.368-0200 I REPL     [initandlisten] ******
2015-12-28T22:17:30.370-0200 I NETWORK  [initandlisten] waiting for connections on port 27010

// Router

junior@linux:~$ mongos --configdb localhost:27010 --port 27011
2015-12-28T22:22:16.671-0200 W SHARDING running with 1 config server should be done only for testing purposes and is not recommended for production
2015-12-28T22:22:16.682-0200 I SHARDING [mongosMain] MongoS version 3.0.8 starting: pid=8358 port=27011 64-bit host=linux (--help for usage)
2015-12-28T22:22:16.682-0200 I CONTROL  [mongosMain] db version v3.0.8
2015-12-28T22:22:16.682-0200 I CONTROL  [mongosMain] git version: 83d8cc25e00e42856924d84e220fbe4a839e605d
2015-12-28T22:22:16.682-0200 I CONTROL  [mongosMain] build info: Linux ip-10-187-89-126 3.13.0-24-generic #46-Ubuntu SMP Thu Apr 10 19:11:08 UTC 2014 x86_64 BOOST_LIB_VERSION=1_49
2015-12-28T22:22:16.682-0200 I CONTROL  [mongosMain] allocator: tcmalloc
2015-12-28T22:22:16.682-0200 I CONTROL  [mongosMain] options: { net: { port: 27011 }, sharding: { configDB: "localhost:27010" } }
2015-12-28T22:22:16.719-0200 I SHARDING [LockPinger] creating distributed lock ping thread for localhost:27010 and process linux:27011:1451348536:1804289383 (sleeping for 30000ms)
2015-12-28T22:22:16.911-0200 I SHARDING [LockPinger] cluster localhost:27010 pinged successfully at Mon Dec 28 22:22:16 2015 by distributed lock pinger 'localhost:27010/linux:27011:1451348536:1804289383', sleeping for 30000ms
2015-12-28T22:22:16.912-0200 I SHARDING [mongosMain] distributed lock 'configUpgrade/linux:27011:1451348536:1804289383' acquired, ts : 5681d2382d1f6217d3573728
2015-12-28T22:22:16.912-0200 I SHARDING [mongosMain] starting upgrade of config server from v0 to v6
2015-12-28T22:22:16.912-0200 I SHARDING [mongosMain] starting next upgrade step from v0 to v6
2015-12-28T22:22:16.912-0200 I SHARDING [mongosMain] about to log new metadata event: { _id: "linux-2015-12-29T00:22:16-5681d2382d1f6217d3573729", server: "linux", clientAddr: "N/A", time: new Date(1451348536912), what: "starting upgrade of config database", ns: "config.version", details: { from: 0, to: 6 } }
2015-12-28T22:22:17.019-0200 I SHARDING [mongosMain] writing initial config version at v6
2015-12-28T22:22:17.072-0200 I SHARDING [mongosMain] about to log new metadata event: { _id: "linux-2015-12-29T00:22:17-5681d2392d1f6217d357372b", server: "linux", clientAddr: "N/A", time: new Date(1451348537072), what: "finished upgrade of config database", ns: "config.version", details: { from: 0, to: 6 } }
2015-12-28T22:22:17.171-0200 I SHARDING [mongosMain] upgrade of config server to v6 successful
2015-12-28T22:22:17.172-0200 I SHARDING [mongosMain] distributed lock 'configUpgrade/linux:27011:1451348536:1804289383' unlocked. 
2015-12-28T22:22:17.862-0200 I SHARDING [Balancer] about to contact config servers and shards
2015-12-28T22:22:17.863-0200 I SHARDING [Balancer] config servers and shards contacted successfully
2015-12-28T22:22:17.863-0200 I SHARDING [Balancer] balancer id: linux:27011 started at Dec 28 22:22:17
2015-12-28T22:22:17.868-0200 I SHARDING [Balancer] distributed lock 'balancer/linux:27011:1451348536:1804289383' acquired, ts : 5681d2392d1f6217d357372d
2015-12-28T22:22:17.882-0200 I NETWORK  [mongosMain] waiting for connections on port 27011
2015-12-28T22:22:18.026-0200 I SHARDING [Balancer] distributed lock 'balancer/linux:27011:1451348536:1804289383' unlocked. 
2015-12-28T22:22:28.029-0200 I SHARDING [Balancer] distributed lock 'balancer/linux:27011:1451348536:1804289383' acquired, ts : 5681d2442d1f6217d357372f
2015-12-28T22:22:28.079-0200 I SHARDING [Balancer] distributed lock 'balancer/linux:27011:1451348536:1804289383' unlocked. 

// Sharding

junior@linux:~$ mkdir /data/shard1 && mkdir /data/shard2 && mkdir /data/shard3

// Shard1

junior@linux:~$ mongod --port 27012 --dbpath /data/shard1
2015-12-28T22:28:15.946-0200 I CONTROL  [initandlisten] MongoDB starting : pid=8655 port=27012 dbpath=/data/shard1 64-bit host=linux
2015-12-28T22:28:15.947-0200 I CONTROL  [initandlisten] db version v3.0.8
2015-12-28T22:28:15.947-0200 I CONTROL  [initandlisten] git version: 83d8cc25e00e42856924d84e220fbe4a839e605d
2015-12-28T22:28:15.947-0200 I CONTROL  [initandlisten] build info: Linux ip-10-187-89-126 3.13.0-24-generic #46-Ubuntu SMP Thu Apr 10 19:11:08 UTC 2014 x86_64 BOOST_LIB_VERSION=1_49
2015-12-28T22:28:15.947-0200 I CONTROL  [initandlisten] allocator: tcmalloc
2015-12-28T22:28:15.947-0200 I CONTROL  [initandlisten] options: { net: { port: 27012 }, storage: { dbPath: "/data/shard1" } }
2015-12-28T22:28:15.969-0200 I JOURNAL  [initandlisten] journal dir=/data/shard1/journal
2015-12-28T22:28:15.969-0200 I JOURNAL  [initandlisten] recover : no journal files present, no recovery needed
2015-12-28T22:28:17.579-0200 I JOURNAL  [initandlisten] preallocateIsFaster=true 13.12
2015-12-28T22:28:19.096-0200 I JOURNAL  [initandlisten] preallocateIsFaster=true 12.58
2015-12-28T22:28:21.922-0200 I JOURNAL  [initandlisten] preallocateIsFaster=true 16.06
2015-12-28T22:28:21.922-0200 I JOURNAL  [initandlisten] preallocateIsFaster check took 5.953 secs
2015-12-28T22:28:21.922-0200 I JOURNAL  [initandlisten] preallocating a journal file /data/shard1/journal/prealloc.0
2015-12-28T22:28:28.045-0200 I JOURNAL  [initandlisten] preallocating a journal file /data/shard1/journal/prealloc.1
2015-12-28T22:28:34.431-0200 I JOURNAL  [initandlisten] preallocating a journal file /data/shard1/journal/prealloc.2
2015-12-28T22:28:40.850-0200 I JOURNAL  [durability] Durability thread started
2015-12-28T22:28:40.853-0200 I JOURNAL  [journal writer] Journal writer thread started
2015-12-28T22:28:40.921-0200 I CONTROL  [initandlisten] 
2015-12-28T22:28:40.921-0200 I CONTROL  [initandlisten] ** WARNING: /sys/kernel/mm/transparent_hugepage/enabled is 'always'.
2015-12-28T22:28:40.921-0200 I CONTROL  [initandlisten] **        We suggest setting it to 'never'
2015-12-28T22:28:40.921-0200 I CONTROL  [initandlisten] 
2015-12-28T22:28:40.921-0200 I CONTROL  [initandlisten] ** WARNING: /sys/kernel/mm/transparent_hugepage/defrag is 'always'.
2015-12-28T22:28:40.921-0200 I CONTROL  [initandlisten] **        We suggest setting it to 'never'
2015-12-28T22:28:40.921-0200 I CONTROL  [initandlisten] 
2015-12-28T22:28:40.922-0200 I INDEX    [initandlisten] allocating new ns file /data/shard1/local.ns, filling with zeroes...
2015-12-28T22:28:41.121-0200 I STORAGE  [FileAllocator] allocating new datafile /data/shard1/local.0, filling with zeroes...
2015-12-28T22:28:41.121-0200 I STORAGE  [FileAllocator] creating directory /data/shard1/_tmp
2015-12-28T22:28:41.210-0200 I STORAGE  [FileAllocator] done allocating datafile /data/shard1/local.0, size: 64MB,  took 0.039 secs
2015-12-28T22:28:41.317-0200 I NETWORK  [initandlisten] waiting for connections on port 27012

// Shard2

junior@linux:~$  mongod --port 27013 --dbpath /data/shard2
2015-12-28T22:28:34.499-0200 I CONTROL  [initandlisten] MongoDB starting : pid=8671 port=27013 dbpath=/data/shard2 64-bit host=linux
2015-12-28T22:28:34.500-0200 I CONTROL  [initandlisten] db version v3.0.8
2015-12-28T22:28:34.500-0200 I CONTROL  [initandlisten] git version: 83d8cc25e00e42856924d84e220fbe4a839e605d
2015-12-28T22:28:34.500-0200 I CONTROL  [initandlisten] build info: Linux ip-10-187-89-126 3.13.0-24-generic #46-Ubuntu SMP Thu Apr 10 19:11:08 UTC 2014 x86_64 BOOST_LIB_VERSION=1_49
2015-12-28T22:28:34.500-0200 I CONTROL  [initandlisten] allocator: tcmalloc
2015-12-28T22:28:34.500-0200 I CONTROL  [initandlisten] options: { net: { port: 27013 }, storage: { dbPath: "/data/shard2" } }
2015-12-28T22:28:34.563-0200 I JOURNAL  [initandlisten] journal dir=/data/shard2/journal
2015-12-28T22:28:34.563-0200 I JOURNAL  [initandlisten] recover : no journal files present, no recovery needed
2015-12-28T22:28:42.111-0200 I JOURNAL  [initandlisten] preallocateIsFaster=true 132.58
2015-12-28T22:28:44.014-0200 I JOURNAL  [initandlisten] preallocateIsFaster=true 19.62
2015-12-28T22:28:47.973-0200 I JOURNAL  [initandlisten] preallocateIsFaster=true 15.16
2015-12-28T22:28:47.973-0200 I JOURNAL  [initandlisten] preallocateIsFaster check took 13.41 secs
2015-12-28T22:28:47.974-0200 I JOURNAL  [initandlisten] preallocating a journal file /data/shard2/journal/prealloc.0
2015-12-28T22:28:54.354-0200 I JOURNAL  [initandlisten] preallocating a journal file /data/shard2/journal/prealloc.1
2015-12-28T22:29:00.739-0200 I JOURNAL  [initandlisten] preallocating a journal file /data/shard2/journal/prealloc.2
2015-12-28T22:29:07.345-0200 I JOURNAL  [durability] Durability thread started
2015-12-28T22:29:07.353-0200 I JOURNAL  [journal writer] Journal writer thread started
2015-12-28T22:29:07.369-0200 I CONTROL  [initandlisten] 
2015-12-28T22:29:07.369-0200 I CONTROL  [initandlisten] ** WARNING: /sys/kernel/mm/transparent_hugepage/enabled is 'always'.
2015-12-28T22:29:07.369-0200 I CONTROL  [initandlisten] **        We suggest setting it to 'never'
2015-12-28T22:29:07.369-0200 I CONTROL  [initandlisten] 
2015-12-28T22:29:07.369-0200 I CONTROL  [initandlisten] ** WARNING: /sys/kernel/mm/transparent_hugepage/defrag is 'always'.
2015-12-28T22:29:07.369-0200 I CONTROL  [initandlisten] **        We suggest setting it to 'never'
2015-12-28T22:29:07.369-0200 I CONTROL  [initandlisten] 
2015-12-28T22:29:07.370-0200 I INDEX    [initandlisten] allocating new ns file /data/shard2/local.ns, filling with zeroes...
2015-12-28T22:29:07.537-0200 I STORAGE  [FileAllocator] allocating new datafile /data/shard2/local.0, filling with zeroes...
2015-12-28T22:29:07.537-0200 I STORAGE  [FileAllocator] creating directory /data/shard2/_tmp
2015-12-28T22:29:07.570-0200 I STORAGE  [FileAllocator] done allocating datafile /data/shard2/local.0, size: 64MB,  took 0.016 secs
2015-12-28T22:29:07.580-0200 I NETWORK  [initandlisten] waiting for connections on port 27013

// Shard3

junior@linux:~$  mongod --port 27014 --dbpath /data/shard3
2015-12-28T22:28:46.297-0200 I CONTROL  [initandlisten] MongoDB starting : pid=8704 port=27014 dbpath=/data/shard3 64-bit host=linux
2015-12-28T22:28:46.297-0200 I CONTROL  [initandlisten] db version v3.0.8
2015-12-28T22:28:46.297-0200 I CONTROL  [initandlisten] git version: 83d8cc25e00e42856924d84e220fbe4a839e605d
2015-12-28T22:28:46.297-0200 I CONTROL  [initandlisten] build info: Linux ip-10-187-89-126 3.13.0-24-generic #46-Ubuntu SMP Thu Apr 10 19:11:08 UTC 2014 x86_64 BOOST_LIB_VERSION=1_49
2015-12-28T22:28:46.297-0200 I CONTROL  [initandlisten] allocator: tcmalloc
2015-12-28T22:28:46.297-0200 I CONTROL  [initandlisten] options: { net: { port: 27014 }, storage: { dbPath: "/data/shard3" } }
2015-12-28T22:28:46.332-0200 I JOURNAL  [initandlisten] journal dir=/data/shard3/journal
2015-12-28T22:28:46.332-0200 I JOURNAL  [initandlisten] recover : no journal files present, no recovery needed
2015-12-28T22:28:54.927-0200 I JOURNAL  [initandlisten] preallocateIsFaster check took 8.595 secs
2015-12-28T22:28:55.056-0200 I JOURNAL  [durability] Durability thread started
2015-12-28T22:28:55.057-0200 I JOURNAL  [journal writer] Journal writer thread started
2015-12-28T22:28:55.123-0200 I CONTROL  [initandlisten] 
2015-12-28T22:28:55.123-0200 I CONTROL  [initandlisten] ** WARNING: /sys/kernel/mm/transparent_hugepage/enabled is 'always'.
2015-12-28T22:28:55.123-0200 I CONTROL  [initandlisten] **        We suggest setting it to 'never'
2015-12-28T22:28:55.123-0200 I CONTROL  [initandlisten] 
2015-12-28T22:28:55.123-0200 I CONTROL  [initandlisten] ** WARNING: /sys/kernel/mm/transparent_hugepage/defrag is 'always'.
2015-12-28T22:28:55.123-0200 I CONTROL  [initandlisten] **        We suggest setting it to 'never'
2015-12-28T22:28:55.123-0200 I CONTROL  [initandlisten] 
2015-12-28T22:28:55.124-0200 I INDEX    [initandlisten] allocating new ns file /data/shard3/local.ns, filling with zeroes...
2015-12-28T22:29:00.731-0200 I STORAGE  [FileAllocator] allocating new datafile /data/shard3/local.0, filling with zeroes...
2015-12-28T22:29:00.731-0200 I STORAGE  [FileAllocator] creating directory /data/shard3/_tmp
2015-12-28T22:29:00.856-0200 I STORAGE  [FileAllocator] done allocating datafile /data/shard3/local.0, size: 64MB,  took 0.08 secs
2015-12-28T22:29:00.962-0200 I NETWORK  [initandlisten] waiting for connections on port 27014

// Configurando shards

junior@linux:~$ mongo --port 27011 --host localhost
MongoDB shell version: 3.0.8
connecting to: localhost:27011/test
Mongo-Hacker 0.0.8
mongos> use projeto-final-mongodb
switched to db projeto-final-mongodb

// Adicionando shards no router

mongos> sh.addShard("localhost:27012");
{
  "shardAdded": "shard0000",
  "ok": 1
}
linux:27011(mongos-3.0.8)[mongos] projeto-final-mongodb> sh.addShard("localhost:27013");
{
  "shardAdded": "shard0001",
  "ok": 1
}
linux:27011(mongos-3.0.8)[mongos] projeto-final-mongodb> sh.addShard("localhost:27014");
{
  "shardAdded": "shard0002",
  "ok": 1
}

// Adicionando database que recebera o sharding

linux:27011(mongos-3.0.8)[mongos] projeto-final-mongodb> sh.enableSharding("projeto-final-mongodb")
{
  "ok": 1
}

// Adicionando collection projects que recebera o Sharding

linux:27011(mongos-3.0.8)[mongos] projeto-final-mongodb> sh.shardCollection("projeto-final-mongodb.projects", {_id: 1})
{
  "collectionsharded": "projeto-final-mongodb.projects",
  "ok": 1
}

// Inserir um projeto

linux:27011(mongos-3.0.8)[mongos] projeto-final-mongodb> db.projects.save({
...     "name": "Construir La Catedral",
...     "description": "A prisão de pablito o rei da Cocaína",
...     "date_begin": Date(),
...     "date_dream": Date(),
...     "date_end": Date(),
...     "visible": false,
...     "realocate": false,
...     "expired": false,
...     "visualizable_mod": true,
...     
...     "members": [
...         {
...             "name": "Pablo Escobar",
...             "type_member": "chefe"        
...         },
...         {
...             "name": "Gustavo Gaviria",
...             "type_member": "presidente"        
...         },
...         {
...             "name": "Valeria Velez",
...             "type_member": "gostosa"        
...         },
...         {
...             "name": "Poison",
...             "type_member": "capanga"        
...         },
...         {
...             "name": "Jorge Luis Ochoa",
...             "type_member": "traira"        
...         }
...     ],
...     
...     "tags": ["catedral", "prisao", "cartel", "drogas"],
...     
...     "goals": [{
...         "name": "Refúgio",
...         "description": "O local foi refúgio de Pablito",
...         "date_begin": Date(),
...         "date_dream": Date(),
...         "date_end": Date(),
...         "visible": false,
...         "realocate": false,
...         "expired": false,
... 
...         "tags": ["refugio", "casa", "liberdade"],
... 
...         "activities": [
...             {
...                 "name": "Escolher local",
...                 
...             },
...             {
...                 "name": "Construir"
...             }
...         ],
... 
...         "goal_hostoric": [
...             {
...                 "date_realocate": Date()
...             }
...         ]
...     }]
... })
Inserted 1 record(s) in 30ms
WriteResult({
  "nInserted": 1
})

```

## Replica
// coloque aqui todos os comandos que você executou

```js

// Criando as pastas para as replicas

junior@linux:~$ mkdir /data/rs1
junior@linux:~$ mkdir /data/rs2
junior@linux:~$ mkdir /data/rs3

// Iniciando as replicas

// Replica 1

junior@linux:~$ mongod --replSet replica_projeto_final --port 27017 --dbpath /data/rs1 --logpath /data/rs1/log.txt --fork
about to fork child process, waiting until server is ready for connections.
forked process: 6506
child process started successfully, parent exiting

// Replica 2

junior@linux:~$ mongod --replSet replica_projeto_final --port 27018 --dbpath /data/rs2 --logpath /data/rs2/log.txt --fork
about to fork child process, waiting until server is ready for connections.
forked process: 6512
child process started successfully, parent exiting

// Replica 3

junior@linux:~$ mongod --replSet replica_projeto_final --port 27019 --dbpath /data/rs3 --logpath /data/rs3/log.txt --fork
about to fork child process, waiting until server is ready for connections.
forked process: 6518
child process started successfully, parent exiting

// Configurando e iniciando

junior@linux:~$ mongo --port 27017
MongoDB shell version: 3.0.8
connecting to: 127.0.0.1:27017/test
Mongo-Hacker 0.0.8
Server has startup warnings: 
2015-12-28T21:42:11.546-0200 I CONTROL  [initandlisten] 
2015-12-28T21:42:11.546-0200 I CONTROL  [initandlisten] ** WARNING: /sys/kernel/mm/transparent_hugepage/enabled is 'always'.
2015-12-28T21:42:11.546-0200 I CONTROL  [initandlisten] **        We suggest setting it to 'never'
2015-12-28T21:42:11.546-0200 I CONTROL  [initandlisten] 
2015-12-28T21:42:11.546-0200 I CONTROL  [initandlisten] ** WARNING: /sys/kernel/mm/transparent_hugepage/defrag is 'always'.
2015-12-28T21:42:11.546-0200 I CONTROL  [initandlisten] **        We suggest setting it to 'never'
2015-12-28T21:42:11.546-0200 I CONTROL  [initandlisten] 

linux(mongod-3.0.8) test> rsconf = {
...    _id: "replica_projeto_final",
...    members: [
...     {
...      _id: 0,
...      host: "127.0.0.1:27017"
...     }
...   ]
... }
{
  "_id": "replica_projeto_final",
  "members": [
    {
      "_id": 0,
      "host": "127.0.0.1:27017"
    }
  ]
}
linux(mongod-3.0.8) test> rs.initiate(rsconf)
{
  "ok": 1
}
linux(mongod-3.0.8)[PRIMARY:replica_projeto_final] test>

// Adicionando replicas na configuração

linux(mongod-3.0.8)[PRIMARY:replica_projeto_final] test> rs.add("127.0.0.1:27018")
{
  "ok": 1
}
linux(mongod-3.0.8)[PRIMARY:replica_projeto_final] test> rs.add("127.0.0.1:27019")
{
  "ok": 1
}

// Satatus replicas

linux(mongod-3.0.8)[PRIMARY:replica_projeto_final] test> rs.status()
{
  "set": "replica_projeto_final",
  "date": ISODate("2015-12-29T00:01:24.431Z"),
  "myState": 1,
  "members": [
    {
      "_id": 0,
      "name": "127.0.0.1:27017",
      "health": 1,
      "state": 1,
      "stateStr": "PRIMARY",
      "uptime": 1180,
      "optime": Timestamp(1451346937, 1),
      "optimeDate": ISODate("2015-12-28T23:55:37Z"),
      "electionTime": Timestamp(1451346735, 2),
      "electionDate": ISODate("2015-12-28T23:52:15Z"),
      "configVersion": 3,
      "self": true
    },
    {
      "_id": 1,
      "name": "127.0.0.1:27018",
      "health": 1,
      "state": 2,
      "stateStr": "SECONDARY",
      "uptime": 350,
      "optime": Timestamp(1451346937, 1),
      "optimeDate": ISODate("2015-12-28T23:55:37Z"),
      "lastHeartbeat": ISODate("2015-12-29T00:01:23.265Z"),
      "lastHeartbeatRecv": ISODate("2015-12-29T00:01:23.746Z"),
      "pingMs": 0,
      "configVersion": 3
    },
    {
      "_id": 2,
      "name": "127.0.0.1:27019",
      "health": 1,
      "state": 2,
      "stateStr": "SECONDARY",
      "uptime": 347,
      "optime": Timestamp(1451346937, 1),
      "optimeDate": ISODate("2015-12-28T23:55:37Z"),
      "lastHeartbeat": ISODate("2015-12-29T00:01:23.268Z"),
      "lastHeartbeatRecv": ISODate("2015-12-29T00:01:23.267Z"),
      "pingMs": 0,
      "lastHeartbeatMessage": "could not find member to sync from",
      "configVersion": 3
    }
  ],
  "ok": 1
}

linux(mongod-3.0.8)[PRIMARY:replica_projeto_final] test> rs.printReplicationInfo()
configured oplog size:   990MB
log length start to end: 202secs (0.06hrs)
oplog first event time:  Mon Dec 28 2015 21:52:15 GMT-0200 (BRST)
oplog last event time:   Mon Dec 28 2015 21:55:37 GMT-0200 (BRST)
now:                     Mon Dec 28 2015 22:03:18 GMT-0200 (BRST)


```