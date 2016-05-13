# MongoDb - Projeto Final
**Autor:** Fernando Lucas
**Data** Date.now() //em timestamp

## Para qual sistema você usaria o MogoDB (diferente desse)?
Para qualquer sistema que tenha como requisito não-funcional performance, que responda de imediato com as respostas adequadas as requisições do usuário. Basicamente o MongoDB se enquadra em qualquer sistema desde que não seja necessário relacionamento de dados, caso for necessário algum relacionamento, ele deve ser realizado a nível de aplicação.



## Qual a modelagem da sua coleção de `users`?
```
users:
{
    name: String,
    bio: String,
    date_register: Date,
    avatar_path: String,
    background_path: String,
    auth: {
        username: String,
        email: String,
        password: String,
        last_access: Date,
        status: Boolean,
        hash_token: String
    }
}
```

## Qual a modelagem da sua coleção de `projects`?
```
projects:
{
    name: String,
    description: String,
    date_begin: Date,
    date_dream: Date,
    date_end: Date,
    visible: Boolean,
    realocate: Boolean,
    expired: Boolean
    visualizable_mod: Boolean,
    tags: [],
    members: [{
        user_id: ObjectId(),
        type_member: String,
        notify: Boolean
    }],
    goals: [{
        name: String,
        description: String,
        date_begin: Date,
        date_dream: Date,
        date_end: Date,
        realocate: Boolean,
        date_realocate: Date,
        expired: Boolean,
        tags: [],
        "activities" : [{
                "activity_id" : ObjectId()
        }]
    }]
}
```


## Qual a modelagem da sua coleção retirada de `projects`?
```
activity:
{
    name: String,
    description: String,
    date_begin: Date,
    date_dream: Date,
    date_end: Date,
    realocate: Boolean,
    date_realocate: Date,
    expired: Boolean,
    tags: [],
    members: [{
        user_id: ObjectId(),
        notify: String
    }],
    comments: [{
        user_id: ObjectId(),
        text: String,
        date_comment: Date,
        files: [{
            path: String,
            weight: Double,
            name: String
        }]
    }]
}
```

##1 Cadastrando 10 usuários.
```
var user = [
{name: "Rafael Jose", username: "rafabd", password: "rafa123" },
{name: "Jose Rafael", username: "zezinho", password: "123ze123" },
{name: "Danilo Souza", username: "dsouza", password: "souzad" },
{name: "Emilio Fagundes", username: "fagundes", password: "efagundes" },
{name: "Diego Amaral", username: "dieguinho", password: "amaraldiego" },
{name: "Diogo Cardozo", username: "cardozod", password: "diogo123" },
{name: "Guilherme Alexandre", username: "guigui", password: "alexgui" },
{name: "Leandro Bruno", username: "brunol", password: "brunoleo" },
{name: "Alisson Santos", username: "alsantos", password: "santosa" },
{name: "Igor Delfino", username: "iguinho", password: "digor" },
];

var uinsert = [];

user.forEach(function(u){
  uinsert.push({
    name: u.name,
    bio: "Biografia sobre o"+u.name,
    date_register: new Date(),
    avatar_path: null,
    background_path: null,
    auth: {
        username: u.username,
        email: u.username+"@gmail.com.br",
        password: u.password,
        last_access: new Date(),
        status: false,
        hash_token: u.name+u.username+u.password
    }
    })
  });

  db.users.insert(uinsert);
```


##2 Cadastrando 5 Atividades
//Usei o distinct para pegar o id dos meus usuários
```
fernando(mongod-3.2.6) projeto> db.users.distinct("_id")
[
  ObjectId("5733838ce91d903cf5e25a68"),
  ObjectId("5733838ce91d903cf5e25a69"),
  ObjectId("5733838ce91d903cf5e25a6a"),
  ObjectId("5733838ce91d903cf5e25a6b"),
  ObjectId("5733838ce91d903cf5e25a6c"),
  ObjectId("5733838ce91d903cf5e25a6d"),
  ObjectId("5733838ce91d903cf5e25a6e"),
  ObjectId("5733838ce91d903cf5e25a6f"),
  ObjectId("5733838ce91d903cf5e25a70"),
  ObjectId("5733838ce91d903cf5e25a71")
]


var act = [{
    name: "Site",
    description: "Development WEB",
    date_begin: new Date(),
    date_dream: new Date(),
    date_end: null,
    realocate: null,
    date_realocate: null,
    expired: null,
    tags: ["web"],
    members: [{
        user_id: ObjectId("5733838ce91d903cf5e25a68"),
        notify: false
    }],
    comments: [{
        user_id: ObjectId("5733838ce91d903cf5e25a68"),
        text: null,
        date_comment: null,
        files: [{
            path: null,
            weight: null,
            name: null
        }]
    }]
},
{
    name: "ERP",
    description: "Development Desktop",
    date_begin: new Date(),
    date_dream: new Date(),
    date_end: null,
    realocate: null,
    date_realocate: null,
    expired: null,
    tags: ["desktop"],
    members: [{
        user_id: ObjectId("5733838ce91d903cf5e25a6a"),
        notify: false
    }],
    comments: [{
        user_id: ObjectId("5733838ce91d903cf5e25a6a"),
        text: null,
        date_comment: null,
        files: [{
            path: null,
            weight: null,
            name: null
        }]
    }]
},
{
    name: "Mobile",
    description: "Development Mobile",
    date_begin: new Date(),
    date_dream: new Date(),
    date_end: null,
    realocate: null,
    date_realocate: null,
    expired: null,
    tags: ["mobile"],
    members: [{
        user_id: ObjectId("5733838ce91d903cf5e25a6d"),
        notify: false
    }],
    comments: [{
        user_id: ObjectId("5733838ce91d903cf5e25a6d"),
        text: null,
        date_comment: null,
        files: [{
            path: null,
            weight: null,
            name: null
        }]
    }]
},
{
    name: "Game 2D",
    description: "Game 3D",
    date_begin: new Date(),
    date_dream: new Date(),
    date_end: null,
    realocate: null,
    date_realocate: null,
    expired: null,
    tags: ["Game 2D"],
    members: [{
        user_id: ObjectId("5733838ce91d903cf5e25a70"),
        notify: false
    }],
    comments: [{
        user_id: ObjectId("5733838ce91d903cf5e25a70"),
        text: null,
        date_comment: null,
        files: [{
            path: null,
            weight: null,
            name: null
        }]
    }]
},
{
    name: "Game 3D",
    description: "Development Game 3D",
    date_begin: new Date(),
    date_dream: new Date(),
    date_end: null,
    realocate: null,
    date_realocate: null,
    expired: null,
    tags: ["Game 2D"],
    members: [{
        user_id: ObjectId("5733838ce91d903cf5e25a6f"),
        notify: false
    }],
    comments: [{
        user_id: ObjectId("5733838ce91d903cf5e25a6f"),
        text: null,
        date_comment: null,
        files: [{
            path: null,
            weight: null,
            name: null
        }]
    }]
}]

db.activities.insert(act);
```


##3 Cadastrando 5 projetos
//Usei o distinct para pegar o id das minhas atividades
```
fernando(mongod-3.2.6) projeto> db.activities.distinct("_id")
[
  ObjectId("57338cb6e91d903cf5e25a72"),
  ObjectId("57338cb6e91d903cf5e25a73"),
  ObjectId("57338cb6e91d903cf5e25a74"),
  ObjectId("57338cb6e91d903cf5e25a75"),
  ObjectId("57338cb6e91d903cf5e25a76")
]
```

##Adicionando os 5 projetos manualmente
```
var pro = [
{
   name: "WebSystem Enterprise",
   description: "Development Web",
   date_begin: new Date(),
   date_dream: new Date(),
   date_end: null,
   visible: true,
   realocate: null,
   expired: null,
   visualizable_mod: null,
   tags: [
   "Development",
   "Technology",
   "Hibrid"
   ],
   members: [

   {
       user_id: ObjectId("5733838ce91d903cf5e25a68"),
       type_member: "CEO",
       notify: false,
   },
   {
       user_id: ObjectId("5733838ce91d903cf5e25a6e"),
       type_member: "Programmer",
       notify: false,
   },
   {
       user_id: ObjectId("5733838ce91d903cf5e25a6d"),
       type_member: "Programmer",
       notify: false,
   },
   {
       user_id: ObjectId("5733838ce91d903cf5e25a70"),
       type_member: "Programmer",
       notify: false,
   },
   {
       user_id: ObjectId("5733838ce91d903cf5e25a71"),
       type_member: "Programmer",
       notify: false,
   }
],
   goals: [{
       name: "WEB",
       description: "Developmente WEB",
       date_begin: new Date(),
       date_dream: new Date(),
       date_end: null,
       realocate: null,
       date_realocate: null,
       expired: null,
       tags: ["performance", "speed", "security"],
       activities : [
       {activity_id: ObjectId("57338cb6e91d903cf5e25a72")     }
     ],
   }]
 },
{
   name: "System ERP Professional",
   description: "Development Desktop",
   date_begin: new Date(),
   date_dream: new Date(),
   date_end: null,
   visible: true,
   realocate: null,
   expired: null,
   visualizable_mod: null,
   tags: [
   "Development",
   "Technology",
   "Desktop"
   ],
   members: [
   {
       user_id: ObjectId("5733838ce91d903cf5e25a6a"),
       type_member: "CEO",
       notify: false,
   },
   {
       user_id: ObjectId("5733838ce91d903cf5e25a69"),
       type_member: "Programmer",
       notify: false,
   },
   {
       user_id: ObjectId("5733838ce91d903cf5e25a70"),
       type_member: "Programmer",
       notify: false,
   },
   {
       user_id: ObjectId("5733838ce91d903cf5e25a71"),
       type_member: "Programmer",
       notify: false,
   },
   {
       user_id: ObjectId("5733838ce91d903cf5e25a68"),
       type_member: "Programmer",
       notify: false,
   }
],
   goals: [{
       name: "Desktop",
       description: "Developmente Desktop",
       date_begin: new Date(),
       date_dream: new Date(),
       date_end: null,
       realocate: null,
       date_realocate: null,
       expired: null,
       tags: ["Auditoriums", "speed", "security"],
       activities : [
       {activity_id: ObjectId("57338cb6e91d903cf5e25a73")      }
     ],
   }]
},
 {
    name: "Mobile as never",
    description: "Development mobile",
    date_begin: new Date(),
    date_dream: new Date(),
    date_end: null,
    visible: true,
    realocate: null,
    expired: null,
    visualizable_mod: null,
    tags: [
    "Development",
    "Technology",
    "Mobile"
    ],
    members: [
    {
        user_id: ObjectId("5733838ce91d903cf5e25a6d"),
        type_member: "CEO",
        notify: false,
    },
    {
        user_id: ObjectId("5733838ce91d903cf5e25a6f"),
        type_member: "Programmer",
        notify: false,
    },
    {
        user_id: ObjectId("5733838ce91d903cf5e25a6a"),
        type_member: "Programmer",
        notify: false,
    },
    {
        user_id: ObjectId("5733838ce91d903cf5e25a6b"),
        type_member: "Programmer",
        notify: false,
    },
    {
        user_id: ObjectId("5733838ce91d903cf5e25a6c"),
        type_member: "Programmer",
        notify: false,
    }
],
    goals: [{
        name: "Aplication Mobile",
        description: "Developmente Mobile",
        date_begin: new Date(),
        date_dream: new Date(),
        date_end: null,
        realocate: null,
        date_realocate: null,
        expired: null,
        tags: ["performance", "speed", "security"],
        activities : [
        {activity_id: ObjectId("57338cb6e91d903cf5e25a74")      }
      ],
    }]
},
{
   name: "GameStore 2D",
   description: "Development Game 2D",
   date_begin: new Date(),
   date_dream: new Date(),
   date_end: null,
   visible: true,
   realocate: null,
   expired: null,
   visualizable_mod: null,
   tags: [
   "Development",
   "Game",
   "Desktop"
   ],
   members: [
   {
       user_id: ObjectId("5733838ce91d903cf5e25a70"),
       type_member: "CEO",
       notify: false,
   },
   {
       user_id: ObjectId("5733838ce91d903cf5e25a6e"),
       type_member: "Programmer",
       notify: false,
   },
   {
       user_id: ObjectId("5733838ce91d903cf5e25a6a"),
       type_member: "Programmer",
       notify: false,
   },
   {
       user_id: ObjectId("5733838ce91d903cf5e25a6b"),
       type_member: "Programmer",
       notify: false,
   },
   {
       user_id: ObjectId("5733838ce91d903cf5e25a6d"),
       type_member: "Programmer",
       notify: false,
   }
],
   goals: [{
       name: "Game 2D",
       description: "Developmente Game 2D",
       date_begin: new Date(),
       date_dream: new Date(),
       date_end: null,
       realocate: null,
       date_realocate: null,
       expired: null,
       tags: ["performance", "usability", "gameplay"],
       activities : [
       {activity_id: ObjectId("57338cb6e91d903cf5e25a75")     }
     ],
   }]
},
{
   name: "GameStore 3D",
   description: "Development Game 3D",
   date_begin: new Date(),
   date_dream: new Date(),
   date_end: null,
   visible: true,
   realocate: null,
   expired: null,
   visualizable_mod: null,
   tags: [
   "Development",
   "Game",
   "Desktop"
   ],
   members: [
   {
       user_id: ObjectId("5733838ce91d903cf5e25a6f"),
       type_member: "CEO",
       notify: false,
   },
   {
       user_id: ObjectId("5733838ce91d903cf5e25a69"),
       type_member: "Programmer",
       notify: false,
   },
   {
       user_id: ObjectId("5733838ce91d903cf5e25a6a"),
       type_member: "Programmer",
       notify: false,
   },
   {
       user_id: ObjectId("5733838ce91d903cf5e25a70"),
       type_member: "Programmer",
       notify: false,
   },
   {
       user_id: ObjectId("5733838ce91d903cf5e25a6c"),
       type_member: "Programmer",
       notify: false,
   }
],
   goals: [{
       name: "Game 3D",
       description: "Development Game 3D",
       date_begin: new Date(),
       date_dream: new Date(),
       date_end: null,
       realocate: null,
       date_realocate: null,
       expired: null,
       tags: ["performance", "usability", "gameplay"],
       activities : [
       {activity_id: ObjectId("57338cb6e91d903cf5e25a76")      }
     ],
   }]
}
]
fernando(mongod-3.2.6) projeto> db.projects.insert(pro)
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


#Retrieve - busca
##1. Liste as informações dos membros de 1 projeto especifico que deve ser buscado pelo seu nome de forma a não ligar para maiúsculas e minúsculas.
```
fernando(mongod-3.2.6) projeto> var query = {name: /mobile as never/i}
fernando(mongod-3.2.6) projeto> var fields = {members:1, _id: 0}
fernando(mongod-3.2.6) projeto> db.projects.find(query, fields)
{
  "members": [
    {
      "user_id": ObjectId("5733838ce91d903cf5e25a6d"),
      "type_member": "CEO",
      "notify": false
    },
    {
      "user_id": ObjectId("5733838ce91d903cf5e25a6f"),
      "type_member": "Programmer",
      "notify": false
    },
    {
      "user_id": ObjectId("5733838ce91d903cf5e25a6a"),
      "type_member": "Programmer",
      "notify": false
    },
    {
      "user_id": ObjectId("5733838ce91d903cf5e25a6b"),
      "type_member": "Programmer",
      "notify": false
    },
    {
      "user_id": ObjectId("5733838ce91d903cf5e25a6c"),
      "type_member": "Programmer",
      "notify": false
    }
  ]
}
Fetched 1 record(s) in 3ms
```

##2. Liste todos os projetos com a tag que voce escolheu para os 3 projetos em comum.
```
fernando(mongod-3.2.6) projeto> var query = {tags: /technology/i}
fernando(mongod-3.2.6) projeto> db.projects.find(query)
{
  "_id": ObjectId("573397ec837c494bb996cda0"),
  "name": "WebSystem Enterprise",
  "description": "Development Web",
  "date_begin": ISODate("2016-05-11T20:36:51.427Z"),
  "date_dream": ISODate("2016-05-11T20:36:51.427Z"),
  "date_end": null,
  "visible": true,
  "realocate": null,
  "expired": null,
  "visualizable_mod": null,
  "tags": [
    "Development",
    "Technology",
    "Hibrid"
  ],
  "members": [
    {
      "user_id": ObjectId("5733838ce91d903cf5e25a68"),
      "type_member": "CEO",
      "notify": false
    },
    {
      "user_id": ObjectId("5733838ce91d903cf5e25a6e"),
      "type_member": "Programmer",
      "notify": false
    },
    {
      "user_id": ObjectId("5733838ce91d903cf5e25a6d"),
      "type_member": "Programmer",
      "notify": false
    },
    {
      "user_id": ObjectId("5733838ce91d903cf5e25a70"),
      "type_member": "Programmer",
      "notify": false
    },
    {
      "user_id": ObjectId("5733838ce91d903cf5e25a71"),
      "type_member": "Programmer",
      "notify": false
    }
  ],
  "goals": [
    {
      "name": "WEB",
      "description": "Developmente WEB",
      "date_begin": ISODate("2016-05-11T20:36:51.427Z"),
      "date_dream": ISODate("2016-05-11T20:36:51.427Z"),
      "date_end": null,
      "realocate": null,
      "date_realocate": null,
      "expired": null,
      "tags": [
        "performance",
        "speed",
        "security"
      ],
      "activities": [
        {
          "activity_id": ObjectId("57338cb6e91d903cf5e25a72")
        }
      ]
    }
  ]
}
{
  "_id": ObjectId("573397ec837c494bb996cda1"),
  "name": "System ERP Professional",
  "description": "Development Desktop",
  "date_begin": ISODate("2016-05-11T20:36:51.427Z"),
  "date_dream": ISODate("2016-05-11T20:36:51.427Z"),
  "date_end": null,
  "visible": true,
  "realocate": null,
  "expired": null,
  "visualizable_mod": null,
  "tags": [
    "Development",
    "Technology",
    "Desktop"
  ],
  "members": [
    {
      "user_id": ObjectId("5733838ce91d903cf5e25a6a"),
      "type_member": "CEO",
      "notify": false
    },
    {
      "user_id": ObjectId("5733838ce91d903cf5e25a69"),
      "type_member": "Programmer",
      "notify": false
    },
    {
      "user_id": ObjectId("5733838ce91d903cf5e25a70"),
      "type_member": "Programmer",
      "notify": false
    },
    {
      "user_id": ObjectId("5733838ce91d903cf5e25a71"),
      "type_member": "Programmer",
      "notify": false
    },
    {
      "user_id": ObjectId("5733838ce91d903cf5e25a68"),
      "type_member": "Programmer",
      "notify": false
    }
  ],
  "goals": [
    {
      "name": "Desktop",
      "description": "Developmente Desktop",
      "date_begin": ISODate("2016-05-11T20:36:51.427Z"),
      "date_dream": ISODate("2016-05-11T20:36:51.427Z"),
      "date_end": null,
      "realocate": null,
      "date_realocate": null,
      "expired": null,
      "tags": [
        "Auditoriums",
        "speed",
        "security"
      ],
      "activities": [
        {
          "activity_id": ObjectId("57338cb6e91d903cf5e25a73")
        }
      ]
    }
  ]
}
{
  "_id": ObjectId("573397ec837c494bb996cda2"),
  "name": "Mobile as never",
  "description": "Development mobile",
  "date_begin": ISODate("2016-05-11T20:36:51.427Z"),
  "date_dream": ISODate("2016-05-11T20:36:51.427Z"),
  "date_end": null,
  "visible": true,
  "realocate": null,
  "expired": null,
  "visualizable_mod": null,
  "tags": [
    "Development",
    "Technology",
    "Mobile"
  ],
  "members": [
    {
      "user_id": ObjectId("5733838ce91d903cf5e25a6d"),
      "type_member": "CEO",
      "notify": false
    },
    {
      "user_id": ObjectId("5733838ce91d903cf5e25a6f"),
      "type_member": "Programmer",
      "notify": false
    },
    {
      "user_id": ObjectId("5733838ce91d903cf5e25a6a"),
      "type_member": "Programmer",
      "notify": false
    },
    {
      "user_id": ObjectId("5733838ce91d903cf5e25a6b"),
      "type_member": "Programmer",
      "notify": false
    },
    {
      "user_id": ObjectId("5733838ce91d903cf5e25a6c"),
      "type_member": "Programmer",
      "notify": false
    }
  ],
  "goals": [
    {
      "name": "Aplication Mobile",
      "description": "Developmente Mobile",
      "date_begin": ISODate("2016-05-11T20:36:51.427Z"),
      "date_dream": ISODate("2016-05-11T20:36:51.427Z"),
      "date_end": null,
      "realocate": null,
      "date_realocate": null,
      "expired": null,
      "tags": [
        "performance",
        "speed",
        "security"
      ],
      "activities": [
        {
          "activity_id": ObjectId("57338cb6e91d903cf5e25a74")
        }
      ]
    }
  ]
}
Fetched 3 record(s) in 17ms


##3. Liste apenas os nomes de todas as atividades para todos os projeto
fernando(mongod-3.2.6) projeto> var query = {}
fernando(mongod-3.2.6) projeto> var fields = {name: 1}
fernando(mongod-3.2.6) projeto> db.activities.find(query, fields)
{
  "_id": ObjectId("57338cb6e91d903cf5e25a72"),
  "name": "Site"
}
{
  "_id": ObjectId("57338cb6e91d903cf5e25a73"),
  "name": "ERP"
}
{
  "_id": ObjectId("57338cb6e91d903cf5e25a74"),
  "name": "Mobile"
}
{
  "_id": ObjectId("57338cb6e91d903cf5e25a75"),
  "name": "Game 2D"
}
{
  "_id": ObjectId("57338cb6e91d903cf5e25a76"),
  "name": "Game 3D"
}
Fetched 5 record(s) in 3ms


##4. Liste todos os projetos que não possuam uma tag.
fernando(mongod-3.2.6) projeto>  var query = {tags: []}
fernando(mongod-3.2.6) projeto> db.projects.find(query)
Fetched 0 record(s) in 1ms


##5. Liste todos os usuários que não fazem parte do primeiro projeto cadastrado.
fernando(mongod-3.2.6) projeto> var query = { _id: ObjectId("573397ec837c494bb996cda0")}
fernando(mongod-3.2.6) projeto> var members = [];
fernando(mongod-3.2.6) projeto> db.projects.findOne(query).members.forEach(function(arr){
... members.push(arr.user_id)})
fernando(mongod-3.2.6) projeto> db.users.find({ _id: { $not: { $in: members } } }, {name: 1})
{
  "_id": ObjectId("5733838ce91d903cf5e25a69"),
  "name": "Jose Rafael"
}
{
  "_id": ObjectId("5733838ce91d903cf5e25a6a"),
  "name": "Danilo Souza"
}
{
  "_id": ObjectId("5733838ce91d903cf5e25a6b"),
  "name": "Emilio Fagundes"
}
{
  "_id": ObjectId("5733838ce91d903cf5e25a6c"),
  "name": "Diego Amaral"
}
{
  "_id": ObjectId("5733838ce91d903cf5e25a6f"),
  "name": "Leandro Bruno"
}
Fetched 5 record(s) in 3ms
```


#Update - alteração
##1. Adicione para todos os projetos o campo views: 0
```
fernando(mongod-3.2.6) projeto> var query = {}
fernando(mongod-3.2.6) projeto> var mod = {$set: {views: 0}}
fernando(mongod-3.2.6) projeto> var options = {multi: true}
fernando(mongod-3.2.6) projeto> db.projects.update(query,mod, options)
Updated 5 existing record(s) in 1ms
WriteResult({
  "nMatched": 5,
  "nUpserted": 0,
  "nModified": 5
})
```

##2. Adicione 1 tag diferente para cada projeto.
```
fernando(mongod-3.2.6) projeto> var projects = db.projects.find()
fernando(mongod-3.2.6) projeto> var tag = function(proj){proj.tags.push(proj.name+" Copyright"); db.projects.save(proj);};
fernando(mongod-3.2.6) projeto> projects.forEach(tag)
Updated 1 existing record(s) in 3ms
Updated 1 existing record(s) in 3ms
Updated 1 existing record(s) in 3ms
Updated 1 existing record(s) in 4ms
Updated 1 existing record(s) in 2ms
```

##3. Adicione 2 membros diferentes para cada projeto.
```
var query = {name: /system erp professional/i}
var mod = {$pushAll :{members: [{user_id: ObjectId("5733838ce91d903cf5e25a6c"), type_member: "Programmer", notify: false},
{user_id: ObjectId("5733838ce91d903cf5e25a6d"), type_member: "Programmer", notify: false}]}}
db.projects.update(query, mod)


var query = {name: /mobile as never/i}
var mod = {$pushAll :{members: [{user_id: ObjectId("5733838ce91d903cf5e25a68"), type_member: "Programmer", notify: false},
{user_id: ObjectId("5733838ce91d903cf5e25a69"), type_member: "Programmer", notify: false}]}}
db.projects.update(query, mod)


var query = {name: /gamestore 2d/i}
var mod = {$pushAll :{members: [{user_id: ObjectId("5733838ce91d903cf5e25a6f"), type_member: "Programmer", notify: false},
{user_id: ObjectId("5733838ce91d903cf5e25a71"), type_member: "Programmer", notify: false}]}}
db.projects.update(query, mod)


var query = {name: /gamestore 3d/i}
var mod = {$pushAll :{members: [{user_id: ObjectId("5733838ce91d903cf5e25a6d"), type_member: "Programmer", notify: false},
{user_id: ObjectId("5733838ce91d903cf5e25a6b"), type_member: "Programmer", notify: false}]}}
db.projects.update(query, mod)


var query = {name: /websystem enterprise/i}
var mod = {$pushAll :{members: [{user_id: ObjectId("5733838ce91d903cf5e25a6a"), type_member: "Programmer", notify: false},
{user_id: ObjectId("5733838ce91d903cf5e25a6b"), type_member: "Programmer", notify: false}]}}
db.projects.update(query, mod)
```

##4. Adicione 1 comentário em cada atividade, deixe apenas 1 projeto sem.
```
var cm = ["Ce é loko mesmo em doido", "Ce é loko mesmo em cachueira", "Ce é loco mesmo em pia", "Ce é loco mesmo em bixo"];
for(var i = 0; i < 4; i++){
  var id = db.activities.find().skip(i).limit(1).toArray();
  var query = {_id: id[0]._id};
  var mod = {$push: {comment: cm[i]}};
  db.activities.update(query, mod)
}
```

##5. Adicione 1 projeto inteiro com UPSERT
```
var query = {name : /project upsert/i};
var mod = {
  "name": "Project Upsert",
  "description": "All Project in Upsert",
  "date_begin": new Date(),
  "date_dream": new Date(),
  "date_end": null,
  "visible": true,
  "realocate": null,
  "expired": null,
  "visualizable_mod": null,
  "tags": [
    "Project",
    "Upsert",
    "Mongo",
  ],
  "members": [
    {
      "user_id": ObjectId("5733838ce91d903cf5e25a70"),
      "type_member": "CEO",
      "notify": false
    },
  ],
  "goals": [
    {
      "name": "Upsert",
      "description": "Project Upsert",
      "date_begin": new Date(),
      "date_dream": new Date(),
      "date_end": null,
      "realocate": null,
      "date_realocate": null,
      "expired": null,
      "tags": [
        "performance",
        "speed",
        "security"
      ],
      "activities": [{
        "activity_id": null,
        }],
    }
  ],
  "views": 0
};

var options = {upsert: true}
fernando(mongod-3.2.6) projeto> db.projects.update(query, mod, options)
Updated 1 new record(s) in 4ms
WriteResult({
  "nMatched": 0,
  "nUpserted": 1,
  "nModified": 0,
  "_id": ObjectId("5735da183a50427519b9ece0")
})
```



#Delete - remoção
##1. Apague todos os projeto que não possuam tags.
```
fernando(mongod-3.2.6) projeto> db.projects.remove({tags: {$exists: 0}})
Removed 0 record(s) in 2ms
WriteResult({
  "nRemoved": 0
})
```


##2. Apague todos os projetos que não possuam comentários.
```
var cm = {comment: {$size: 0}};
var act = db.activities.find(cm, {_id: 1}).toArray();
var query = {"goals.activities._id": {$in: act}};
var options = {multi: true};
db.projects.remove(query, options)
fernando(mongod-3.2.6) projeto> db.projects.remove(query, options)
Removed 0 record(s) in 1ms
WriteResult({
  "nRemoved": 0
})
```

##3. Apague todos os projetos que não possuam atividades.
```
fernando(mongod-3.2.6) projeto> db.projects.remove({activity: {$eq: []}}, {multi: 1})
Removed 0 record(s) in 1ms
WriteResult({
  "nRemoved": 0
})
```

##4. Escolha 2 usuários e apague todos os projetos que os 2 fazem parte
```
var users = [
{"_id": ObjectId("5733838ce91d903cf5e25a70")},
{"_id": ObjectId("5733838ce91d903cf5e25a71")}
]
fernando(mongod-3.2.6) projeto> users.forEach(function(pro){db.projects.remove({"members.user_id": pro._id})})
Removed 5 record(s) in 3ms
Removed 0 record(s) in 2ms
```

##5. Apague todos os projetos que possuam uma determinada tag em goals.
```
var query = {"goals.tags": "speed"}
fernando(mongod-3.2.6) projeto> db.projects.remove(query)
Removed 1 record(s) in 2ms
WriteResult({
  "nRemoved": 1
})
```



#Gerenciamento de Usuários
##1. Crie um usuário com permissão apenas para leitura.
```
fernando(mongod-3.2.6) projeto> db.createUser(
...   {
...     user: "Leitura",
...     pwd: "leitor123",
...     roles: [ "read"]
...   }
...   )
Successfully added user: {
  "user": "Leitura",
  "roles": [
    "read"
  ]
}
```

##2. Crie um usuário com permissão para leitura e escrita.
```
fernando(mongod-3.2.6) projeto> db.createUser(
...   {
...     user: "Leitura-Escrita",
...     pwd: "leitorescritor123",
...     roles: [ "readWrite"]
...   }
...   )
Successfully added user: {
  "user": "Leitura-Escrita",
  "roles": [
    "readWrite"
  ]
}
```

##3. Adicionaro papel grantRolesToUser e revoke para o usuário com leitura e escrita.
```
fernando(mongod-3.2.6) projeto> use admin
switched to db admin
fernando(mongod-3.2.6) admin> db.createRole({
...   role: "grantRolesToUser",
...   privileges: [
...         { resource: { db: "admin", collection: "" }, actions: [ "grantRole" ] },
...     ],
...     roles: [ "readWrite" ]
...   },
... { w: "majority" , wtimeout: 5000 }
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
  "roles": [
    "readWrite"
  ]
}

fernando(mongod-3.2.6) admin> db.createRole({
...  role: "Revoke",
...  privileges: [
...     { resource: { db: "admin", collection: "" }, actions: [ "revokeRole" ] },
...    ],
...  roles: [ "readWrite" ]
...  },
...  { w: "majority" , wtimeout: 5000 }
...  )
{
  "role": "Revoke",
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
  "roles": [
    "readWrite"
  ]
}

fernando(mongod-3.2.6) admin> db.grantRolesToUser(
...   "Leitura-Escrita",
...   ["grantRolesToUser", "Revoke"]
...   )
```

##4. Remover o papel grantRolesToUser do usuário com leitura e escrita.
```
fernando(mongod-3.2.6) admin> db.revokeRolesFromUser(
...   "Leitura-Escrita",
...   ["grantRolesToUser"]
...   )
```

##5. Listar todos os usuários com seus papéis e ações.
```
fernando(mongod-3.2.6) admin> db.runCommand({   usersInfo: 1 })
{
  "users": [
    {
      "_id": "admin.Leitura",
      "user": "Leitura",
      "db": "admin",
      "roles": [
        {
          "role": "read",
          "db": "admin"
        }
      ]
    },
    {
      "_id": "admin.Leitura-Escrita",
      "user": "Leitura-Escrita",
      "db": "admin",
      "roles": [
        {
          "role": "Revoke",
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

#Sharding
```
➜  fernando git:(master) ✗ mkdir /data/configdb
➜  fernando git:(master) ✗ mkdir /data/shard1 && mkdir /data/shard2 && mkdir /data/shard3
➜  fernando git:(master) ✗ mongod --configsvr --port 27420
➜  fernando git:(master) ✗ mongos --configdb --localhost:27420 --port 27421
➜  fernando git:(master) ✗ mkdir /data/shard1 && mkdir /data/shard2 && mkdir /data/shard3 
➜  fernando git:(master) ✗ mongod --port 27422 --dbpath /data/shard1
➜  fernando git:(master) ✗ mongod --port 27423 --dbpath /data/shard2
➜  fernando git:(master) ✗ mongod --port 27424 --dbpath /data/shard3
fernando:27421(mongos-3.2.6)[mongos] test> sh.addShard("localhost:27422")
{
  "shardAdded": "shard0000",
  "ok": 1
}
fernando:27421(mongos-3.2.6)[mongos] test> sh.addShard("localhost:27423")
{
  "shardAdded": "shard0001",
  "ok": 1
}
fernando:27421(mongos-3.2.6)[mongos] test> sh.addShard("localhost:27424")
{
  "shardAdded": "shard0002",
  "ok": 1
}
fernando:27421(mongos-3.2.6)[mongos] test> sh.enableSharding("Projeto")
{
  "ok": 1
}
fernando:27421(mongos-3.2.6)[mongos] test> sh.shardCollection("projeto.projects", {"_id" : 1})
fernando:27421(mongos-3.2.6)[mongos] test> sh.shardCollection("projeto.users", {"_id" : 1})
fernando:27421(mongos-3.2.6)[mongos] test> sh.shardCollection("projeto.activities", {"_id" : 1})
```


#Replica
```
➜  fernando git:(master) ✗ mkdir /data/rs1 && mkdir /data/rs2 && mkdir /data/rs3 
➜  fernando git:(master) ✗ mongod --replSet replica_set --port 27017 -dbpath /data/rs1
➜  fernando git:(master) ✗ mongod --replSet replica_set --port 27018 -dbpath /data/rs2
➜  fernando git:(master) ✗ mongod --replSet replica_set --port 27019 -dbpath /data/rs3
➜  fernando git:(master) ✗ mongo --port 27017
fernando(mongod-3.2.6) test> var rsconf = {
...    _id: "replica_set",
...    members: [
...     {
...      _id: 0,
...      host: "127.0.0.1:27017"
...     }
...   ]
... }
fernando(mongod-3.2.6) test> rs.initiate(conf)
fernando(mongod-3.2.6) test> rs.add("127.0.0.18")
fernando(mongod-3.2.6) test> rs.add("127.0.0.19")
```





