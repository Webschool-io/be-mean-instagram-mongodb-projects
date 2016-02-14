# MongoDb - Projeto Final  
**Autor:** Igor Luíz Carneiro de Oliveira.  
**Data:** 1455449534611

## Para qual sistema você usaria o MongoDB (diferente desse)?
> Qualquer sistema que precisse de velocidade para acesso dos dados, pois além contar com uma ótima facilidade de escalabilidade, a visualização de sua extrutura é simples e limpa.

## Qual a modelagem da sua coleção de `users`?

```js

user: {
    name: String,
    bio: String,
    register: Date,
    avatar_path: String,

    system_settings : [
        { background_path: String }
    ],

    auth: {
        username: String,
        email: String,
        password: Password,
        last_access: Date,
        online: Boolean,
        disabled: Boolean,
        hash_token: String
    }
}

```


## Qual a modelagem da sua coleção de `projects`?

```js

project: {
    name: String,
    description: String,
    date_begin: Date,
    date_dream: Date,
    date_end: Date,
    visible: Boolean,
    realocate: Boolean,
    expired: Boolean,
    visualizable_mod: String,

    members: [
        {
            user_id: ObjectId(),
            type_name: String,
            notify: Boolean
        }
    ],

    tags: [],

    goal: [
        {
            realocate: Boolean,
            tags: [],

            historic: [
                { date_realocate: Date }
            ],

            activities: [
                {
                    activities_id: ObjectId(),
                    name_activity: String
                }
            ]
        }
    ]
}

```

## Qual a modelagem da sua coleção retirada de `projects`?

```js

activities: {

  name: String,
  description: String,
  date_begin: Date,
  date_dream: Date,
  date_end: Date,
  realocate: Boolean,
  expired: Boolean,
  tags: [],

  historic: [
    { date_realocate: Date }
  ],

  members: [
    {
        user_id: ObjectId(),
        notify: Boolean
    }
  ],

  comments: [
    {
        text: String, 
        date: Date,

        file : [
            {
                path: String,
                weight: Number,
                name: String
            }
        ],

        members: {
          user_id: ObjectId(),
          notify: Boolean
        }
    }
  ]
}

```

## Create - cadastro

#### Cadastre até 10 usuários diferentes.

```js
var users = ["André","Isis","Alice","Carlos","Igor","Jaqueline","Vitôria","Lucas","Luiza","Amanda"];

function serialGenerator() {
    var serial = "";
    while(serial.length < 20) {
        serial += String.fromCharCode(Math.floor(Math.random() * 64) + 32);
    }
    return serial;
}

function randomDate() {
    var time = new Date().getTime(),
        randomTime = time * Math.random();

    return new Date(randomTime);
}

function randomNumber() {
    var number = (Math.random() * 2) + 1;
    return number.toFixed(3);
}

function insert(arrayUsers) {

    arrayUsers.forEach(function(user) {
        db.users.insert({

            name: user,
            bio: "bio",
            register: randomDate(),
            avatar_path: "/img/",

            system_settings : [
                { background_path: "config/background.conf" }
            ],          

            auth: {

                username: user + randomNumber(),
                email: user + '.bemean@gmail.com',
                password: randomNumber(),
                last_access: new Date(),
                online: true,
                hash_token: serialGenerator()
            }
        });
    });
}

fedora(mongod-3.0.8) project> insert(users)
Inserted 1 record(s) in 7ms
Inserted 1 record(s) in 1ms
Inserted 1 record(s) in 1ms
Inserted 1 record(s) in 1ms
Inserted 1 record(s) in 4ms
Inserted 1 record(s) in 1ms
Inserted 1 record(s) in 1ms
Inserted 1 record(s) in 3ms
Inserted 1 record(s) in 8ms
Inserted 1 record(s) in 3ms
```

#### Cadastre 5 projetos diferentes.
> + Cada um com 5 membros, sempre diferentes dentro dos projetos;  
+ Cada um com pelo menos 3 tags diferentes;  
+ Escolha 1 tag onde deva ficar em 2 projetos;  
+ Escolha 1 tag onde deva ficar em 3 projetos;  
+ Cada projeto com pelo menos 1 goal;  
+ Cada goal com pelo menos 3 tags;  
+ Cada goal com pelo menos 2 atividades, deixe 1 projeto sem.  

```js

function randomDate() {
    var time = new Date().getTime(),
        randomTime = time * Math.random();

    return new Date(randomTime);
}

function randomNumber() {
    var number = (Math.random() * 2) + 1;
    return number.toFixed(3);
}


var projectName = [];

while(projectName.length < 5) {
    projectName.push({ 
        project: "project - " + randomNumber(), 
        decription: "CreatedAt - " + randomDate()
    });
}

var allTags = [
    "PHP","Python","Javascript",
    "Ruby","Java","Lisp","Lua",
    "Dart","C#","ASP",".NET",
    "Visal Basic", "Pascal","Assembly"
];

/**
 * Início
 */

var members = db.users.find({}, {_id: true}).toArray();

function user() {
    var membersProject = members.slice(0,5);

    members.sort();
    members.reverse();
    return membersProject;
}

function goalInsert() {

    return  [{
        realocate: true,
        tags: ["blog","site","app"],

        historic: [{
            date_realocate: new Date("Tue Jan 08 1980 03:31:44 GMT-0300 (BRT)"),
        }],

        activities: [{
            activities_id: db.activities.find({}, {_id: true}).toArray()
        }]
    }]
}

function registerProjects() {

    for(var i=0; i<5; i++) {
        db.project.insert({
            name: projectName[i].project,
            decription: projectName[i].decription,
            date_begin: new Date("Tue Jan 08 1980 03:31:44 GMT-0300 (BRT)"),
            date_dream: new Date("Sun Oct 20 2002 17:17:13 GMT-0300 (BRT)"),
            date_end: new Date("Mon Mar 09 2015 05:38:07 GMT-0300 (BRT)"),
            visible: true,
            realocate: undefined,
            expired: false,
            visualizable_mod: null,

            members: user(),

            tags: allTags.slice(i,10),

            goal: goalInsert()
                
        });
    }
}

// Registrando

fedora(mongod-3.0.8) project> registerProjects()
Inserted 1 record(s) in 2ms
Inserted 1 record(s) in 1ms
Inserted 1 record(s) in 1ms
Inserted 1 record(s) in 0ms
Inserted 1 record(s) in 1ms

// Removendo todas as atividades de um projeto

fedora(mongod-3.0.8) project> db.project.update({name: "project - 2.521"}, {$set: {goal: [{activities: []}]}})
Updated 1 existing record(s) in 1ms
WriteResult({
  "nMatched": 1,
  "nUpserted": 0,
  "nModified": 1
})
```

## Retrieve - busca

#### Liste as informações dos membros de 1 projeto específico que deve ser buscado pelo seu nome de forma a não ligar para maiúsculas e minúsculas.

```js
fedora(mongod-3.0.8) project> db.project.find({name: "project - 2.521"}, {members: true})
{
  "_id": ObjectId("56bfedf544fa400ec8996ff8"),
  "members": [
    {
      "_id": ObjectId("56bfed9944fa400ec8996fee")
    },
    {
      "_id": ObjectId("56bfed9944fa400ec8996fef")
    },
    {
      "_id": ObjectId("56bfed9944fa400ec8996ff0")
    },
    {
      "_id": ObjectId("56bfed9944fa400ec8996ff1")
    },
    {
      "_id": ObjectId("56bfed9944fa400ec8996ff2")
    }
  ]
}
Fetched 1 record(s) in 1ms
```

#### Liste todos os projetos com a tag que você escolheu para os 3 projetos em comum.

```js
fedora(mongod-3.0.8) project> db.project.find({tags: /javascript/i})
{
  "_id": ObjectId("56bfedf544fa400ec8996ff8"),
  "name": "project - 2.521",
  "decription": "CreatedAt - Sat Jun 10 1972 21:01:36 GMT-0300 (BRT)",
  "date_begin": ISODate("1980-01-08T06:31:44Z"),
  "date_dream": ISODate("2002-10-20T20:17:13Z"),
  "date_end": ISODate("2015-03-09T08:38:07Z"),
  "visible": true,
  "realocate": null,
  "expired": false,
  "visualizable_mod": null,
  "members": [
    {
      "_id": ObjectId("56bfed9944fa400ec8996fee")
    },
    {
      "_id": ObjectId("56bfed9944fa400ec8996fef")
    },
    {
      "_id": ObjectId("56bfed9944fa400ec8996ff0")
    },
    {
      "_id": ObjectId("56bfed9944fa400ec8996ff1")
    },
    {
      "_id": ObjectId("56bfed9944fa400ec8996ff2")
    }
  ],
  "tags": [
    "PHP",
    "Python",
    "Javascript",
    "Ruby",
    "Java",
    "Lisp",
    "Lua",
    "Dart",
    "C#",
    "ASP"
  ],
  "goal": [
    {
      "activities": [ ]
    }
  ]
}
{
  "_id": ObjectId("56bfedf544fa400ec8996ff9"),
  "name": "project - 2.649",
  "decription": "CreatedAt - Fri Apr 06 1990 02:12:19 GMT-0300 (BRT)",
  "date_begin": ISODate("1980-01-08T06:31:44Z"),
  "date_dream": ISODate("2002-10-20T20:17:13Z"),
  "date_end": ISODate("2015-03-09T08:38:07Z"),
  "visible": true,
  "realocate": null,
  "expired": false,
  "visualizable_mod": null,
  "members": [
    {
      "_id": ObjectId("56bfed9944fa400ec8996ff7")
    },
    {
      "_id": ObjectId("56bfed9944fa400ec8996ff6")
    },
    {
      "_id": ObjectId("56bfed9944fa400ec8996ff5")
    },
    {
      "_id": ObjectId("56bfed9944fa400ec8996ff4")
    },
    {
      "_id": ObjectId("56bfed9944fa400ec8996ff3")
    }
  ],
  "tags": [
    "Python",
    "Javascript",
    "Ruby",
    "Java",
    "Lisp",
    "Lua",
    "Dart",
    "C#",
    "ASP"
  ],
  "goal": [
    {
      "realocate": true,
      "tags": [
        "blog",
        "site",
        "app"
      ],
      "historic": [
        {
          "date_realocate": ISODate("1980-01-08T06:31:44Z")
        }
      ],
      "activities": [
        {
          "activities_id": [
            {
              "_id": ObjectId("56beb56073addd2e9028c520")
            },
            {
              "_id": ObjectId("56beb56073addd2e9028c521")
            },
            {
              "_id": ObjectId("56beb56073addd2e9028c522")
            },
            {
              "_id": ObjectId("56beb56073addd2e9028c523")
            }
          ]
        }
      ]
    }
  ]
}
{
  "_id": ObjectId("56bfedf544fa400ec8996ffa"),
  "name": "project - 2.243",
  "decription": "CreatedAt - Fri Sep 11 2009 05:07:17 GMT-0300 (BRT)",
  "date_begin": ISODate("1980-01-08T06:31:44Z"),
  "date_dream": ISODate("2002-10-20T20:17:13Z"),
  "date_end": ISODate("2015-03-09T08:38:07Z"),
  "visible": true,
  "realocate": null,
  "expired": false,
  "visualizable_mod": null,
  "members": [
    {
      "_id": ObjectId("56bfed9944fa400ec8996fee")
    },
    {
      "_id": ObjectId("56bfed9944fa400ec8996fef")
    },
    {
      "_id": ObjectId("56bfed9944fa400ec8996ff0")
    },
    {
      "_id": ObjectId("56bfed9944fa400ec8996ff1")
    },
    {
      "_id": ObjectId("56bfed9944fa400ec8996ff2")
    }
  ],
  "tags": [
    "Javascript",
    "Ruby",
    "Java",
    "Lisp",
    "Lua",
    "Dart",
    "C#",
    "ASP"
  ],
  "goal": [
    {
      "realocate": true,
      "tags": [
        "blog",
        "site",
        "app"
      ],
      "historic": [
        {
          "date_realocate": ISODate("1980-01-08T06:31:44Z")
        }
      ],
      "activities": [
        {
          "activities_id": [
            {
              "_id": ObjectId("56beb56073addd2e9028c520")
            },
            {
              "_id": ObjectId("56beb56073addd2e9028c521")
            },
            {
              "_id": ObjectId("56beb56073addd2e9028c522")
            },
            {
              "_id": ObjectId("56beb56073addd2e9028c523")
            }
          ]
        }
      ]
    }
  ]
}
Fetched 3 record(s) in 2ms
```

#### Liste apenas os nomes de todas as atividades para todos os projetos.

```js
fedora(mongod-3.0.8) project> db.activities.find({},{name: true, _id: false})
{
  "name": "Programar"
}
{
  "name": "Trabalhar"
}
{
  "name": "Viajar"
}
{
  "name": "Aprender"
}
Fetched 4 record(s) in 1ms
```

#### Liste todos os projetos que não possuam uma tag.
```js
fedora(mongod-3.0.8) project> db.project.find({tags: []})
Fetched 0 record(s) in 1ms
```

#### Liste todos os usuários que não fazem parte do primeiro projeto cadastrado.

```js
var users = [];


var project = db.project.findOne({name: "project - 2.521" }, {members: true, _id: false}).members;


project.forEach(function(elem) {
    users.push(elem._id);
});

db.users.find(
    {
        _id: {
            $not: {
                $in: users
            }
        }
    },
    {
        name: true
    }
)
{
  "_id": ObjectId("56bfed9944fa400ec8996ff3"),
  "name": "Jaqueline"
}
{
  "_id": ObjectId("56bfed9944fa400ec8996ff4"),
  "name": "Vitôria"
}
{
  "_id": ObjectId("56bfed9944fa400ec8996ff5"),
  "name": "Lucas"
}
{
  "_id": ObjectId("56bfed9944fa400ec8996ff6"),
  "name": "Luiza"
}
{
  "_id": ObjectId("56bfed9944fa400ec8996ff7"),
  "name": "Amanda"
}
Fetched 5 record(s) in 1ms
```


## Update - alteração

#### Adicione para todos os projetos o campo views: 0.

```js
var mod = {$set: {views: 0}};
var opts = {multi: true}

fedora(mongod-3.0.8) project> db.project.update({},mod,opts)
Updated 5 existing record(s) in 1ms
WriteResult({
  "nMatched": 5,
  "nUpserted": 0,
  "nModified": 5
})
```

#### Adicione 1 tag diferente para cada projeto.

```js

var mod = {$push: {tags: 'developer'}}
var opts = {multi: true}

fedora(mongod-3.0.8) project> db.project.update({},mod,opts)
Updated 5 existing record(s) in 1ms
WriteResult({
  "nMatched": 5,
  "nUpserted": 0,
  "nModified": 5
})
```

#### Adicione 2 membros diferentes para cada projeto.

```js
// Adicionando os novos usuarios na collections users

var users = [
    "John",
    "Clevis",
    "Sarah",
    "Paul",
    "Amelia",
    "Alexsander",
    "Lara",
    "Brian",
    "Jessica",
    "Britney"
];


function serialGenerator() {
    var serial = "";
    while(serial.length < 20) {
        serial += String.fromCharCode(Math.floor(Math.random() * 64) + 32);
    }
    return serial;
}

function randomDate() {
    var time = new Date().getTime(),
        randomTime = time * Math.random();

    return new Date(randomTime);
}

function randomNumber() {
    var number = (Math.random() * 2) + 1;
    return number.toFixed(3);
}

function insert(arrayUsers) {

    arrayUsers.forEach(function(user) {
        db.users.insert({

            name: user,
            bio: "bio",
            register: randomDate(),
            avatar_path: "/img/",

            system_settings : [
                { background_path: "config/background.conf" }
            ],          

            auth: {

                username: user + randomNumber(),
                email: user + '.bemean@gmail.com',
                password: randomNumber(),
                last_access: new Date(),
                online: true,
                hash_token: serialGenerator()
            }
        });
    });
}

fedora(mongod-3.0.8) project> insert(users)
Inserted 1 record(s) in 4ms
Inserted 1 record(s) in 1ms
Inserted 1 record(s) in 2ms
Inserted 1 record(s) in 2ms
Inserted 1 record(s) in 2ms
Inserted 1 record(s) in 3ms
Inserted 1 record(s) in 0ms
Inserted 1 record(s) in 2ms
Inserted 1 record(s) in 1ms
Inserted 1 record(s) in 0ms


// Adicionando os 2 usuários nos projetos

var members = db.users.find({}, {_id: true}).toArray().slice(10,20);
var infoProjects = db.project.find({}, {name: true}).toArray();

function user() {
    var membersProject = members.slice(0,2);

    members.sort();
    members.reverse();
    return membersProject;
}


function updateProjects() {

    var mod = {$pushAll: {members: user()}};

    var x = 0;

    while(x < infoProjects.length) {
        db.project.update({name: infoProjects[x].name}, mod)
        x++;
    }

}

fedora(mongod-3.0.8) project> updateProjects()
Updated 1 existing record(s) in 3ms
Updated 1 existing record(s) in 1ms
Updated 1 existing record(s) in 1ms
Updated 1 existing record(s) in 1ms
Updated 1 existing record(s) in 0ms
```

#### Adicione 1 comentário em cada atividade, deixe apenas 1 projeto sem.

```js

// Adicionando 1 comentário para cada atividade

function randomNumber() {
    var number = (Math.random() * 2) + 1;
    return number.toFixed(3);
}

function moreComment() {

    var i = 0;

    var numberAct = db.activities.count();

    while(i < numberAct) {
        db.activities.update({},{$push: {

            comments: 
                {
                   text: "Uma simples variação - " + randomNumber(),
                   date: ISODate("2003-10-01T20:17:13Z"),
                   file: [
                    {
                      path: "/comments/2",
                      weight: randomNumber(),
                      name: "files"
                    }
                  ],
                  members: [
                    {
                      _id: ObjectId("56bd4a8285cfc160a30c1a60")
                    },
                    {
                      _id: ObjectId("56bd4a8285cfc160a30c1a5f")
                    },
                    {
                      _id: ObjectId("56bd4a8285cfc160a30c1a5e")
                    },
                    {
                      _id: ObjectId("56bd4a8285cfc160a30c1a5d")
                    },
                    {
                      _id: ObjectId("56bd4a8285cfc160a30c1a5c")
                    }
                  ]
                }
        }})
        i++;
    }
}

fedora(mongod-3.0.8) project> moreComment()
Updated 1 existing record(s) in 1ms
Updated 1 existing record(s) in 1ms
Updated 1 existing record(s) in 1ms
Updated 1 existing record(s) in 1ms


// Removendo os comentários de uma das atividades, logo quando for dado o "populate", o projeto que ficou com essa atividade irá ficar sem comments

fedora(mongod-3.0.8) project> db.activities.update({name: /aprender/i}, {$set: {comments: []}} )
Updated 1 existing record(s) in 1ms
WriteResult({
  "nMatched": 1,
  "nUpserted": 0,
  "nModified": 1
})
```

#### Adicione 1 projeto inteiro com **UPSERT**.

```js

function randomDate() {
    var time = new Date().getTime(),
        randomTime = time * Math.random();

    return new Date(randomTime);
}

function randomNumber() {
    var number = (Math.random() * 2) + 1;
    return number.toFixed(3);
}

/**
 * Início
 */

var members = db.users.find({}, {_id: true}).toArray();

function user() {
    var membersProject = members.slice(0,5);

    members.sort();
    members.reverse();
    return membersProject;
}

function goalInsert() {

    return  [{
        realocate: true,
        tags: ["blog","site","app"],

        historic: [{
            date_realocate: new Date("Tue Jan 08 1980 03:31:44 GMT-0300 (BRT)"),
        }],

        activities: [{
            activities_id: db.activities.find({}, {_id: true}).toArray()
        }]
    }]
}

function registerProject() {

    db.project.insert({
        name: "project - " + randomNumber(),
        decription: "CreatedAt - " + randomDate(),
        date_begin: new Date("Tue Jan 08 1980 03:31:44 GMT-0300 (BRT)"),
        date_dream: new Date("Sun Oct 20 2002 17:17:13 GMT-0300 (BRT)"),
        date_end: new Date("Mon Mar 09 2015 05:38:07 GMT-0300 (BRT)"),
        visible: true,
        realocate: undefined,
        expired: false,
        visualizable_mod: null,

        members: user(),

        tags: ["Ruby","Java","Lisp"],

        goal: goalInsert()
            
    });
}

fedora(mongod-3.0.8) project> registerProject()
Inserted 1 record(s) in 2ms
```


## Delete - remoção

#### Apague todos os projetos que não possuam tags.

```js
fedora(mongod-3.0.8) project> db.project.remove({tags: []})
Removed 0 record(s) in 3ms
WriteResult({
  "nRemoved": 0
})
```

#### Apague todos os projetos que não possuam comentários nas atividades.

```js

// Pequisei na collection 'activities' qual actividade não possui comments.

fedora(mongod-3.0.8) project> db.activities.find({comments: {$eq: [] }})
{
  "_id": ObjectId("56c00434f7dd4d5c1d141dc4"),
  "name": "Ensinar",
  "description": "Lorem Ipsum é simplesmente uma simulação de texto da indústria.",
  "date_begin": ISODate("1980-01-08T06:31:44Z"),
  "date_dream": ISODate("2002-10-20T20:17:13Z"),
  "date_end": ISODate("2015-03-09T08:38:07Z"),
  "realocate": true,
  "expired": false,
  "tags": [
    "computador",
    "laptop"
  ],
  "historic": [
    {
      "date_realocate": ISODate("2003-10-20T20:17:13Z")
    }
  ],
  "members": [
    {
      "_id": ObjectId("56bd4a8185cfc160a30c1a57")
    },
    {
      "_id": ObjectId("56bd4a8285cfc160a30c1a58")
    },
    {
      "_id": ObjectId("56bd4a8285cfc160a30c1a59")
    },
    {
      "_id": ObjectId("56bd4a8285cfc160a30c1a5a")
    },
    {
      "_id": ObjectId("56bd4a8285cfc160a30c1a5b")
    }
  ],
  "comments": [ ]
}
Fetched 1 record(s) in 2ms

// removendo o projeto com o _id da atividade

var mod = {
    goal: [
    {
      activities: [
        {
          activities_id: {
            _id: ObjectId("56c00434f7dd4d5c1d141dc4")
          }
        }
      ]
    }
  ]
}

db.project.remove(mod)

fedora(mongod-3.0.8) project> db.project.remove(mod)
Removed 1 record(s) in 2ms
WriteResult({
  "nRemoved": 1
})  
```


#### Apague todos os projetos que não possuam atividades.

```js
fedora(mongod-3.0.8) project> db.project.remove({name: "project - 1.826"})
Removed 1 record(s) in 1ms
WriteResult({
  "nRemoved": 1
})
```

#### Escolha 2 usuário e apague todos os projetos em que os 2 fazem parte.

```js
fedora(mongod-3.0.8) project> db.project.remove({ members: {$in: [{"_id": ObjectId("56bfed9944fa400ec8996fee")}] } })
Removed 2 record(s) in 1ms
WriteResult({
  "nRemoved": 2
})
```

#### Apague todos os projetos que possuam uma determinada tag em goal.

```js
var query = {"goal.tags": { $eq: "be-mean"}}
fedora(mongod-3.0.8) project> db.project.remove(query)
Removed 1 record(s) in 3ms
WriteResult({
  "nRemoved": 1
})
```



## Gerenciamento de Usuários

#### Crie um usuário com permissões APENAS de Leitura.

```js
fedora(mongod-3.0.8) admin> db.createUser({
 createUser: "user-read",
 pwd: "senha-read",
 roles: [
    { role: "read", db: "admin" }
 ]
})
Successfully added user: {
  "createUser": "user-read",
  "roles": [
    {
      "role": "read",
      "db": "admin"
    }
  ]
}
```

#### Crie um usuário com permissões de Escrita e Leitura.

```js
fedora(mongod-3.0.8) admin> db.createUser({
 createUser: "user-read-write",
 pwd: "senha-read-write",
 roles: [
    {role: "readWrite", db: "admin"}
]
 })
Successfully added user: {
  "createUser": "user-read-write",
  "roles": [
    {
      "role": "readWrite",
      "db": "admin"
    }
  ]
}
```

#### Adicionar o papel `grantRolesToUser` e `revokeRole` para o usuário com Escrita e Leitura.

```js
db.createRole( 
  { 
    role : "grantRolesToUser",
    roles : [
      { role: "readWrite", db: "admin" }
    ],
    privileges : [{ 
        resource : { 
            db: "admin", 
            collection: "" 
        }, actions : ["grantRole"] }
    ]
  }
);

{
  "role": "grantRolesToUser",
  "roles": [
    {
      "role": "readWrite",
      "db": "admin"
    }
  ],
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
  ]
}

db.createRole( 
  { 
    role : "revokeRole",
    roles : [
      { role: "readWrite", db: "admin" }
    ],
    privileges : [
      { resource : { 
            db: "admin", 
            collection: "" 
        }, actions : ["grantRole"] }
    ]
  }
);

{
  "role": "revokeRole",
  "roles": [
    {
      "role": "readWrite",
      "db": "admin"
    }
  ],
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
  ]
}

db.grantRolesToUser("user-read-write",["grantRolesToUser","revokeRole"])

```

#### Remover o papel `grantRolesToUser` para o usuário com Escrita e Leitura.

```js

db.runCommand( { 
  revokeRolesFromUser: "user-read-write",
  roles: [
    { role: "grantRolesToUser", db: "admin" }
  ]
});

{
  "ok": 1
}

```

#### Listar todos os usuários com seus papéis e ações.

```js
fedora(mongod-3.0.8) admin> db.runCommand({ usersInfo: true });
{
  "users": [
    {
      "_id": "admin.user-read",
      "user": "user-read",
      "db": "admin",
      "roles": [
        {
          "role": "read",
          "db": "admin"
        }
      ]
    },
    {
      "_id": "admin.user-read-write",
      "user": "user-read-write",
      "db": "admin",
      "roles": [
        {
          "role": "revokeRole",
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


## Replica

```js

// Criando pastas
mkdir /data/rs1
mkdir /data/rs2
mkdir /data/rs3


// Primeira replica-set

[Igor@fedora ~]$ mongod --replSet replica_set --port 27017 --dbpath /data/rs1
2016-02-14T08:01:33.838-0200 I CONTROL  [initandlisten] MongoDB starting : pid=25698 port=27017 dbpath=/data/rs1 64-bit host=fedora
2016-02-14T08:01:33.838-0200 I CONTROL  [initandlisten] db version v3.0.8
2016-02-14T08:01:33.838-0200 I CONTROL  [initandlisten] git version: 83d8cc25e00e42856924d84e220fbe4a839e605d
2016-02-14T08:01:33.838-0200 I CONTROL  [initandlisten] build info: Linux buildhw-03.phx2.fedoraproject.org 4.3.0-1.fc24.x86_64 #1 SMP Mon Nov 2 16:27:20 UTC 2015 x86_64 BOOST_LIB_VERSION=1_58
2016-02-14T08:01:33.838-0200 I CONTROL  [initandlisten] allocator: tcmalloc
2016-02-14T08:01:33.838-0200 I CONTROL  [initandlisten] options: { net: { port: 27017 }, replication: { replSet: "replica_set" }, storage: { dbPath: "/data/rs1" } }
2016-02-14T08:01:33.877-0200 I JOURNAL  [initandlisten] journal dir=/data/rs1/journal
2016-02-14T08:01:33.877-0200 I JOURNAL  [initandlisten] recover : no journal files present, no recovery needed
2016-02-14T08:01:37.651-0200 I JOURNAL  [initandlisten] preallocateIsFaster=true 21.5
2016-02-14T08:01:40.679-0200 I JOURNAL  [initandlisten] preallocateIsFaster=true 36.82
2016-02-14T08:01:44.950-0200 I JOURNAL  [initandlisten] preallocateIsFaster=true 40.82
2016-02-14T08:01:44.951-0200 I JOURNAL  [initandlisten] preallocateIsFaster check took 11.073 secs
2016-02-14T08:01:44.951-0200 I JOURNAL  [initandlisten] preallocating a journal file /data/rs1/journal/prealloc.0
2016-02-14T08:01:47.013-0200 I -        [initandlisten]   File Preallocator Progress: 1027604480/1073741824 95%
2016-02-14T08:02:05.326-0200 I JOURNAL  [initandlisten] preallocating a journal file /data/rs1/journal/prealloc.1
2016-02-14T08:02:08.444-0200 I -        [initandlisten]   File Preallocator Progress: 996147200/1073741824 92%
2016-02-14T08:02:25.029-0200 I JOURNAL  [initandlisten] preallocating a journal file /data/rs1/journal/prealloc.2
2016-02-14T08:02:46.100-0200 I JOURNAL  [durability] Durability thread started
2016-02-14T08:02:46.100-0200 I JOURNAL  [journal writer] Journal writer thread started
2016-02-14T08:02:46.312-0200 I CONTROL  [initandlisten] 
2016-02-14T08:02:46.312-0200 I CONTROL  [initandlisten] ** WARNING: /sys/kernel/mm/transparent_hugepage/defrag is 'always'.
2016-02-14T08:02:46.312-0200 I CONTROL  [initandlisten] **        We suggest setting it to 'never'
2016-02-14T08:02:46.312-0200 I CONTROL  [initandlisten] 
2016-02-14T08:02:46.312-0200 I INDEX    [initandlisten] allocating new ns file /data/rs1/local.ns, filling with zeroes...
2016-02-14T08:02:46.731-0200 I STORAGE  [FileAllocator] allocating new datafile /data/rs1/local.0, filling with zeroes...
2016-02-14T08:02:46.731-0200 I STORAGE  [FileAllocator] creating directory /data/rs1/_tmp
2016-02-14T08:02:46.925-0200 I STORAGE  [FileAllocator] done allocating datafile /data/rs1/local.0, size: 64MB,  took 0.067 secs
2016-02-14T08:02:47.032-0200 I REPL     [initandlisten] Did not find local replica set configuration document at startup;  NoMatchingDocument Did not find replica set configuration document in local.system.replset
2016-02-14T08:02:47.033-0200 I NETWORK  [initandlisten] waiting for connections on port 27017

// Segunda replica-set

[Igor@fedora ~]$ mongod --replSet replica_set --port 27018 --dbpath /data/rs2
2016-02-14T08:01:47.127-0200 I CONTROL  [initandlisten] MongoDB starting : pid=25805 port=27018 dbpath=/data/rs2 64-bit host=fedora
2016-02-14T08:01:47.127-0200 I CONTROL  [initandlisten] db version v3.0.8
2016-02-14T08:01:47.127-0200 I CONTROL  [initandlisten] git version: 83d8cc25e00e42856924d84e220fbe4a839e605d
2016-02-14T08:01:47.127-0200 I CONTROL  [initandlisten] build info: Linux buildhw-03.phx2.fedoraproject.org 4.3.0-1.fc24.x86_64 #1 SMP Mon Nov 2 16:27:20 UTC 2015 x86_64 BOOST_LIB_VERSION=1_58
2016-02-14T08:01:47.127-0200 I CONTROL  [initandlisten] allocator: tcmalloc
2016-02-14T08:01:47.127-0200 I CONTROL  [initandlisten] options: { net: { port: 27018 }, replication: { replSet: "replica_set" }, storage: { dbPath: "/data/rs2" } }
2016-02-14T08:01:47.166-0200 I JOURNAL  [initandlisten] journal dir=/data/rs2/journal
2016-02-14T08:01:47.166-0200 I JOURNAL  [initandlisten] recover : no journal files present, no recovery needed
2016-02-14T08:02:48.515-0200 I JOURNAL  [initandlisten] preallocateIsFaster=true 1148.4
2016-02-14T08:02:54.080-0200 I JOURNAL  [initandlisten] preallocateIsFaster=true 61.88
2016-02-14T08:03:00.032-0200 I JOURNAL  [initandlisten] preallocateIsFaster=true 57.34
2016-02-14T08:03:00.032-0200 I JOURNAL  [initandlisten] preallocateIsFaster check took 72.865 secs
2016-02-14T08:03:00.032-0200 I JOURNAL  [initandlisten] preallocating a journal file /data/rs2/journal/prealloc.0
2016-02-14T08:03:03.133-0200 I -        [initandlisten]   File Preallocator Progress: 828375040/1073741824 77%
2016-02-14T08:03:08.274-0200 I -        [initandlisten]   File Preallocator Progress: 880803840/1073741824 82%
2016-02-14T08:03:11.269-0200 I -        [initandlisten]   File Preallocator Progress: 1059061760/1073741824 98%
2016-02-14T08:03:35.255-0200 I JOURNAL  [initandlisten] preallocating a journal file /data/rs2/journal/prealloc.1
2016-02-14T08:03:38.167-0200 I -        [initandlisten]   File Preallocator Progress: 754974720/1073741824 70%
2016-02-14T08:03:41.138-0200 I -        [initandlisten]   File Preallocator Progress: 870318080/1073741824 81%
2016-02-14T08:03:44.051-0200 I -        [initandlisten]   File Preallocator Progress: 954204160/1073741824 88%
2016-02-14T08:03:47.071-0200 I -        [initandlisten]   File Preallocator Progress: 1027604480/1073741824 95%
2016-02-14T08:04:06.197-0200 I JOURNAL  [initandlisten] preallocating a journal file /data/rs2/journal/prealloc.2
2016-02-14T08:04:09.039-0200 I -        [initandlisten]   File Preallocator Progress: 587202560/1073741824 54%
2016-02-14T08:04:13.400-0200 I -        [initandlisten]   File Preallocator Progress: 713031680/1073741824 66%
2016-02-14T08:04:16.073-0200 I -        [initandlisten]   File Preallocator Progress: 954204160/1073741824 88%
2016-02-14T08:04:19.290-0200 I -        [initandlisten]   File Preallocator Progress: 1006632960/1073741824 93%
2016-02-14T08:04:33.985-0200 I JOURNAL  [durability] Durability thread started
2016-02-14T08:04:33.986-0200 I JOURNAL  [journal writer] Journal writer thread started
2016-02-14T08:04:35.192-0200 I CONTROL  [initandlisten] 
2016-02-14T08:04:35.192-0200 I CONTROL  [initandlisten] ** WARNING: /sys/kernel/mm/transparent_hugepage/defrag is 'always'.
2016-02-14T08:04:35.192-0200 I CONTROL  [initandlisten] **        We suggest setting it to 'never'
2016-02-14T08:04:35.192-0200 I CONTROL  [initandlisten] 
2016-02-14T08:04:35.192-0200 I INDEX    [initandlisten] allocating new ns file /data/rs2/local.ns, filling with zeroes...
2016-02-14T08:04:50.712-0200 I STORAGE  [FileAllocator] allocating new datafile /data/rs2/local.0, filling with zeroes...
2016-02-14T08:04:50.712-0200 I STORAGE  [FileAllocator] creating directory /data/rs2/_tmp
2016-02-14T08:04:51.633-0200 I STORAGE  [FileAllocator] done allocating datafile /data/rs2/local.0, size: 64MB,  took 0.508 secs
2016-02-14T08:04:51.882-0200 I REPL     [initandlisten] Did not find local replica set configuration document at startup;  NoMatchingDocument Did not find replica set configuration document in local.system.replset
2016-02-14T08:04:51.883-0200 I NETWORK  [initandlisten] waiting for connections on port 27018


// Terceira replica-set

[Igor@fedora ~]$ mongod --replSet replica_set --port 27019 --dbpath /data/rs3
2016-02-14T08:01:54.553-0200 I CONTROL  [initandlisten] MongoDB starting : pid=25906 port=27019 dbpath=/data/rs3 64-bit host=fedora
2016-02-14T08:01:54.553-0200 I CONTROL  [initandlisten] db version v3.0.8
2016-02-14T08:01:54.553-0200 I CONTROL  [initandlisten] git version: 83d8cc25e00e42856924d84e220fbe4a839e605d
2016-02-14T08:01:54.553-0200 I CONTROL  [initandlisten] build info: Linux buildhw-03.phx2.fedoraproject.org 4.3.0-1.fc24.x86_64 #1 SMP Mon Nov 2 16:27:20 UTC 2015 x86_64 BOOST_LIB_VERSION=1_58
2016-02-14T08:01:54.553-0200 I CONTROL  [initandlisten] allocator: tcmalloc
2016-02-14T08:01:54.553-0200 I CONTROL  [initandlisten] options: { net: { port: 27019 }, replication: { replSet: "replica_set" }, storage: { dbPath: "/data/rs3" } }
2016-02-14T08:01:54.646-0200 I JOURNAL  [initandlisten] journal dir=/data/rs3/journal
2016-02-14T08:01:54.647-0200 I JOURNAL  [initandlisten] recover : no journal files present, no recovery needed
2016-02-14T08:02:49.496-0200 I JOURNAL  [initandlisten] preallocateIsFaster=true 979.68
2016-02-14T08:02:54.416-0200 I JOURNAL  [initandlisten] preallocateIsFaster=true 67.24
2016-02-14T08:03:00.229-0200 I JOURNAL  [initandlisten] preallocateIsFaster=true 67.88
2016-02-14T08:03:00.229-0200 I JOURNAL  [initandlisten] preallocateIsFaster check took 65.582 secs
2016-02-14T08:03:00.229-0200 I JOURNAL  [initandlisten] preallocating a journal file /data/rs3/journal/prealloc.0
2016-02-14T08:03:03.162-0200 I -        [initandlisten]   File Preallocator Progress: 367001600/1073741824 34%
2016-02-14T08:03:08.291-0200 I -        [initandlisten]   File Preallocator Progress: 419430400/1073741824 39%
2016-02-14T08:03:11.033-0200 I -        [initandlisten]   File Preallocator Progress: 587202560/1073741824 54%
2016-02-14T08:03:14.310-0200 I -        [initandlisten]   File Preallocator Progress: 629145600/1073741824 58%
2016-02-14T08:03:17.106-0200 I -        [initandlisten]   File Preallocator Progress: 807403520/1073741824 75%
2016-02-14T08:03:20.551-0200 I -        [initandlisten]   File Preallocator Progress: 912261120/1073741824 84%
2016-02-14T08:03:35.785-0200 I JOURNAL  [initandlisten] preallocating a journal file /data/rs3/journal/prealloc.1
2016-02-14T08:03:38.189-0200 I -        [initandlisten]   File Preallocator Progress: 325058560/1073741824 30%
2016-02-14T08:03:41.193-0200 I -        [initandlisten]   File Preallocator Progress: 440401920/1073741824 41%
2016-02-14T08:03:44.113-0200 I -        [initandlisten]   File Preallocator Progress: 524288000/1073741824 48%
2016-02-14T08:03:47.294-0200 I -        [initandlisten]   File Preallocator Progress: 597688320/1073741824 55%
2016-02-14T08:03:50.261-0200 I -        [initandlisten]   File Preallocator Progress: 681574400/1073741824 63%
2016-02-14T08:03:53.194-0200 I -        [initandlisten]   File Preallocator Progress: 754974720/1073741824 70%
2016-02-14T08:03:56.105-0200 I -        [initandlisten]   File Preallocator Progress: 933232640/1073741824 86%
2016-02-14T08:04:01.393-0200 I -        [initandlisten]   File Preallocator Progress: 1006632960/1073741824 93%
2016-02-14T08:04:13.793-0200 I JOURNAL  [initandlisten] preallocating a journal file /data/rs3/journal/prealloc.2
2016-02-14T08:04:16.213-0200 I -        [initandlisten]   File Preallocator Progress: 125829120/1073741824 11%
2016-02-14T08:04:19.001-0200 I -        [initandlisten]   File Preallocator Progress: 178257920/1073741824 16%
2016-02-14T08:04:22.093-0200 I -        [initandlisten]   File Preallocator Progress: 325058560/1073741824 30%
2016-02-14T08:04:25.123-0200 I -        [initandlisten]   File Preallocator Progress: 482344960/1073741824 44%
2016-02-14T08:04:28.094-0200 I -        [initandlisten]   File Preallocator Progress: 650117120/1073741824 60%
2016-02-14T08:04:31.012-0200 I -        [initandlisten]   File Preallocator Progress: 786432000/1073741824 73%
2016-02-14T08:04:34.192-0200 I -        [initandlisten]   File Preallocator Progress: 996147200/1073741824 92%
2016-02-14T08:04:52.054-0200 I JOURNAL  [durability] Durability thread started
2016-02-14T08:04:52.054-0200 I JOURNAL  [journal writer] Journal writer thread started
2016-02-14T08:04:52.091-0200 I CONTROL  [initandlisten] 
2016-02-14T08:04:52.091-0200 I CONTROL  [initandlisten] ** WARNING: /sys/kernel/mm/transparent_hugepage/defrag is 'always'.
2016-02-14T08:04:52.091-0200 I CONTROL  [initandlisten] **        We suggest setting it to 'never'
2016-02-14T08:04:52.091-0200 I CONTROL  [initandlisten] 
2016-02-14T08:04:52.091-0200 I INDEX    [initandlisten] allocating new ns file /data/rs3/local.ns, filling with zeroes...
2016-02-14T08:04:52.487-0200 I STORAGE  [FileAllocator] allocating new datafile /data/rs3/local.0, filling with zeroes...
2016-02-14T08:04:52.487-0200 I STORAGE  [FileAllocator] creating directory /data/rs3/_tmp
2016-02-14T08:04:52.536-0200 I STORAGE  [FileAllocator] done allocating datafile /data/rs3/local.0, size: 64MB,  took 0.022 secs
2016-02-14T08:04:52.555-0200 I REPL     [initandlisten] Did not find local replica set configuration document at startup;  NoMatchingDocument Did not find replica set configuration document in local.system.replset
2016-02-14T08:04:52.555-0200 I NETWORK  [initandlisten] waiting for connections on port 27019


// Iniciando o PRIMARY e adicionando replicas

var rsconf = {
   _id: "replica_set",
   members: [
    {
     _id: 0,
     host: "127.0.0.1:27017"
    }
  ]
};

rs.initiate(rsconf)
{
  "ok": 1
}

rs.add("127.0.0.1:27018")
{
  "ok": 1
}

rs.add("127.0.0.1:27019")
{
  "ok": 1
}


// Verificando replica

fedora(mongod-3.0.8)[PRIMARY:replica_set] test> rs.status()
{
  "set": "replica_set",
  "date": ISODate("2016-02-14T10:08:28.839Z"),
  "myState": 1,
  "members": [
    {
      "_id": 0,
      "name": "127.0.0.1:27017",
      "health": 1,
      "state": 1,
      "stateStr": "PRIMARY",
      "uptime": 415,
      "optime": Timestamp(1455444455, 1),
      "optimeDate": ISODate("2016-02-14T10:07:35Z"),
      "electionTime": Timestamp(1455444368, 2),
      "electionDate": ISODate("2016-02-14T10:06:08Z"),
      "configVersion": 3,
      "self": true
    },
    {
      "_id": 1,
      "name": "127.0.0.1:27018",
      "health": 1,
      "state": 2,
      "stateStr": "SECONDARY",
      "uptime": 80,
      "optime": Timestamp(1455444455, 1),
      "optimeDate": ISODate("2016-02-14T10:07:35Z"),
      "lastHeartbeat": ISODate("2016-02-14T10:08:27.156Z"),
      "lastHeartbeatRecv": ISODate("2016-02-14T10:08:28.055Z"),
      "pingMs": 0,
      "syncingTo": "127.0.0.1:27017",
      "configVersion": 3
    },
    {
      "_id": 2,
      "name": "127.0.0.1:27019",
      "health": 1,
      "state": 2,
      "stateStr": "SECONDARY",
      "uptime": 53,
      "optime": Timestamp(1455444455, 1),
      "optimeDate": ISODate("2016-02-14T10:07:35Z"),
      "lastHeartbeat": ISODate("2016-02-14T10:08:27.156Z"),
      "lastHeartbeatRecv": ISODate("2016-02-14T10:08:27.160Z"),
      "pingMs": 0,
      "configVersion": 3
    }
  ],
  "ok": 1
}


// Verificando o OPLOG

fedora(mongod-3.0.8)[PRIMARY:replica_set] test> rs.printReplicationInfo()
configured oplog size:   5186.882781982422MB
log length start to end: 87secs (0.02hrs)
oplog first event time:  Sun Feb 14 2016 08:06:08 GMT-0200 (BRST)
oplog last event time:   Sun Feb 14 2016 08:07:35 GMT-0200 (BRST)
now:                     Sun Feb 14 2016 08:08:58 GMT-0200 (BRST)


```


## Sharding

#### Criando Config Server
```js

[Igor@fedora ~]$ mkdir /data/configdb

[Igor@fedora ~]$ mongod --configsvr --port 27010
2016-02-14T09:04:37.230-0200 I CONTROL  [initandlisten] MongoDB starting : pid=29752 port=27010 dbpath=/data/configdb master=1 64-bit host=fedora
2016-02-14T09:04:37.230-0200 I CONTROL  [initandlisten] db version v3.0.8
2016-02-14T09:04:37.230-0200 I CONTROL  [initandlisten] git version: 83d8cc25e00e42856924d84e220fbe4a839e605d
2016-02-14T09:04:37.230-0200 I CONTROL  [initandlisten] build info: Linux buildhw-03.phx2.fedoraproject.org 4.3.0-1.fc24.x86_64 #1 SMP Mon Nov 2 16:27:20 UTC 2015 x86_64 BOOST_LIB_VERSION=1_58
2016-02-14T09:04:37.230-0200 I CONTROL  [initandlisten] allocator: tcmalloc
2016-02-14T09:04:37.230-0200 I CONTROL  [initandlisten] options: { net: { port: 27010 }, sharding: { clusterRole: "configsvr" } }
2016-02-14T09:04:37.264-0200 I JOURNAL  [initandlisten] journal dir=/data/configdb/journal
2016-02-14T09:04:37.264-0200 I JOURNAL  [initandlisten] recover : no journal files present, no recovery needed
2016-02-14T09:04:39.976-0200 I JOURNAL  [initandlisten] preallocateIsFaster=true 30.84
2016-02-14T09:04:42.915-0200 I JOURNAL  [initandlisten] preallocateIsFaster=true 33.5
2016-02-14T09:04:47.076-0200 I JOURNAL  [initandlisten] preallocateIsFaster=true 34.5
2016-02-14T09:04:47.076-0200 I JOURNAL  [initandlisten] preallocateIsFaster check took 9.811 secs
2016-02-14T09:04:47.076-0200 I JOURNAL  [initandlisten] preallocating a journal file /data/configdb/journal/prealloc.0
2016-02-14T09:04:49.516-0200 I JOURNAL  [initandlisten] preallocating a journal file /data/configdb/journal/prealloc.1
2016-02-14T09:04:51.795-0200 I JOURNAL  [initandlisten] preallocating a journal file /data/configdb/journal/prealloc.2
2016-02-14T09:04:54.143-0200 I JOURNAL  [durability] Durability thread started
2016-02-14T09:04:54.143-0200 I JOURNAL  [journal writer] Journal writer thread started
2016-02-14T09:04:54.180-0200 I CONTROL  [initandlisten] 
2016-02-14T09:04:54.180-0200 I CONTROL  [initandlisten] ** WARNING: /sys/kernel/mm/transparent_hugepage/defrag is 'always'.
2016-02-14T09:04:54.180-0200 I CONTROL  [initandlisten] **        We suggest setting it to 'never'
2016-02-14T09:04:54.180-0200 I CONTROL  [initandlisten] 
2016-02-14T09:04:54.180-0200 I INDEX    [initandlisten] allocating new ns file /data/configdb/local.ns, filling with zeroes...
2016-02-14T09:04:54.532-0200 I STORAGE  [FileAllocator] allocating new datafile /data/configdb/local.0, filling with zeroes...
2016-02-14T09:04:54.532-0200 I STORAGE  [FileAllocator] creating directory /data/configdb/_tmp
2016-02-14T09:04:54.580-0200 I STORAGE  [FileAllocator] done allocating datafile /data/configdb/local.0, size: 16MB,  took 0.022 secs
2016-02-14T09:04:54.600-0200 I REPL     [initandlisten] ******
2016-02-14T09:04:54.600-0200 I REPL     [initandlisten] creating replication oplog of size: 5MB...
2016-02-14T09:04:54.647-0200 I REPL     [initandlisten] ******
2016-02-14T09:04:54.648-0200 I NETWORK  [initandlisten] waiting for connections on port 27010
```


#### Criando Router

```js

[Igor@fedora ~]$ mongos --configdb localhost:27010 --port 27011
2016-02-14T09:07:02.968-0200 W SHARDING running with 1 config server should be done only for testing purposes and is not recommended for production
2016-02-14T09:07:02.972-0200 I SHARDING [mongosMain] MongoS version 3.0.8 starting: pid=29985 port=27011 64-bit host=fedora (--help for usage)
2016-02-14T09:07:02.972-0200 I CONTROL  [mongosMain] db version v3.0.8
2016-02-14T09:07:02.972-0200 I CONTROL  [mongosMain] git version: 83d8cc25e00e42856924d84e220fbe4a839e605d
2016-02-14T09:07:02.972-0200 I CONTROL  [mongosMain] build info: Linux buildhw-03.phx2.fedoraproject.org 4.3.0-1.fc24.x86_64 #1 SMP Mon Nov 2 16:27:20 UTC 2015 x86_64 BOOST_LIB_VERSION=1_58
2016-02-14T09:07:02.972-0200 I CONTROL  [mongosMain] allocator: tcmalloc
2016-02-14T09:07:02.972-0200 I CONTROL  [mongosMain] options: { net: { port: 27011 }, sharding: { configDB: "localhost:27010" } }
2016-02-14T09:07:02.993-0200 I SHARDING [LockPinger] creating distributed lock ping thread for localhost:27010 and process fedora:27011:1455448022:1804289383 (sleeping for 30000ms)
2016-02-14T09:07:03.372-0200 I SHARDING [LockPinger] cluster localhost:27010 pinged successfully at Sun Feb 14 09:07:02 2016 by distributed lock pinger 'localhost:27010/fedora:27011:1455448022:1804289383', sleeping for 30000ms
2016-02-14T09:07:03.372-0200 I SHARDING [mongosMain] distributed lock 'configUpgrade/fedora:27011:1455448022:1804289383' acquired, ts : 56c05fd6c4e4451ffd3c1031
2016-02-14T09:07:03.373-0200 I SHARDING [mongosMain] starting upgrade of config server from v0 to v6
2016-02-14T09:07:03.373-0200 I SHARDING [mongosMain] starting next upgrade step from v0 to v6
2016-02-14T09:07:03.373-0200 I SHARDING [mongosMain] about to log new metadata event: { _id: "fedora-2016-02-14T11:07:03-56c05fd7c4e4451ffd3c1032", server: "fedora", clientAddr: "N/A", time: new Date(1455448023373), what: "starting upgrade of config database", ns: "config.version", details: { from: 0, to: 6 } }
2016-02-14T09:07:03.575-0200 I SHARDING [mongosMain] writing initial config version at v6
2016-02-14T09:07:03.636-0200 I SHARDING [mongosMain] about to log new metadata event: { _id: "fedora-2016-02-14T11:07:03-56c05fd7c4e4451ffd3c1034", server: "fedora", clientAddr: "N/A", time: new Date(1455448023636), what: "finished upgrade of config database", ns: "config.version", details: { from: 0, to: 6 } }
2016-02-14T09:07:03.759-0200 I SHARDING [mongosMain] upgrade of config server to v6 successful
2016-02-14T09:07:03.759-0200 I SHARDING [mongosMain] distributed lock 'configUpgrade/fedora:27011:1455448022:1804289383' unlocked. 
2016-02-14T09:07:04.825-0200 I SHARDING [Balancer] about to contact config servers and shards
2016-02-14T09:07:04.825-0200 I SHARDING [Balancer] config servers and shards contacted successfully
2016-02-14T09:07:04.825-0200 I SHARDING [Balancer] balancer id: fedora:27011 started at Feb 14 09:07:04
2016-02-14T09:07:04.827-0200 I SHARDING [Balancer] distributed lock 'balancer/fedora:27011:1455448022:1804289383' acquired, ts : 56c05fd8c4e4451ffd3c1036
2016-02-14T09:07:04.858-0200 I NETWORK  [mongosMain] waiting for connections on port 27011
2016-02-14T09:07:05.025-0200 I SHARDING [Balancer] distributed lock 'balancer/fedora:27011:1455448022:1804289383' unlocked. 

```

#### Criando os 3 Shards


```js
mkdir /data/shard1
mkdir /data/shard2
mkdir /data/shard3

// Shard 1

[Igor@fedora ~]$ mongod --port 27012 --dbpath /data/shard1
2016-02-14T09:10:06.664-0200 I CONTROL  [initandlisten] MongoDB starting : pid=30548 port=27012 dbpath=/data/shard1 64-bit host=fedora
2016-02-14T09:10:06.664-0200 I CONTROL  [initandlisten] db version v3.0.8
2016-02-14T09:10:06.664-0200 I CONTROL  [initandlisten] git version: 83d8cc25e00e42856924d84e220fbe4a839e605d
2016-02-14T09:10:06.664-0200 I CONTROL  [initandlisten] build info: Linux buildhw-03.phx2.fedoraproject.org 4.3.0-1.fc24.x86_64 #1 SMP Mon Nov 2 16:27:20 UTC 2015 x86_64 BOOST_LIB_VERSION=1_58
2016-02-14T09:10:06.664-0200 I CONTROL  [initandlisten] allocator: tcmalloc
2016-02-14T09:10:06.665-0200 I CONTROL  [initandlisten] options: { net: { port: 27012 }, storage: { dbPath: "/data/shard1" } }
2016-02-14T09:10:06.699-0200 I JOURNAL  [initandlisten] journal dir=/data/shard1/journal
2016-02-14T09:10:06.699-0200 I JOURNAL  [initandlisten] recover : no journal files present, no recovery needed
2016-02-14T09:10:09.705-0200 I JOURNAL  [initandlisten] preallocateIsFaster=true 27.24
2016-02-14T09:10:12.312-0200 I JOURNAL  [initandlisten] preallocateIsFaster=true 29.12
2016-02-14T09:10:15.962-0200 I JOURNAL  [initandlisten] preallocateIsFaster=true 30.02
2016-02-14T09:10:15.962-0200 I JOURNAL  [initandlisten] preallocateIsFaster check took 9.263 secs
2016-02-14T09:10:15.962-0200 I JOURNAL  [initandlisten] preallocating a journal file /data/shard1/journal/prealloc.0
2016-02-14T09:10:18.086-0200 I -        [initandlisten]   File Preallocator Progress: 954204160/1073741824 88%
2016-02-14T09:10:37.813-0200 I JOURNAL  [initandlisten] preallocating a journal file /data/shard1/journal/prealloc.1
2016-02-14T09:10:40.974-0200 I -        [initandlisten]   File Preallocator Progress: 922746880/1073741824 85%
2016-02-14T09:10:43.169-0200 I -        [initandlisten]   File Preallocator Progress: 975175680/1073741824 90%
2016-02-14T09:10:58.776-0200 I JOURNAL  [initandlisten] preallocating a journal file /data/shard1/journal/prealloc.2
2016-02-14T09:11:01.004-0200 I -        [initandlisten]   File Preallocator Progress: 943718400/1073741824 87%
2016-02-14T09:11:04.224-0200 I -        [initandlisten]   File Preallocator Progress: 1017118720/1073741824 94%
2016-02-14T09:11:19.485-0200 I JOURNAL  [durability] Durability thread started
2016-02-14T09:11:19.485-0200 I JOURNAL  [journal writer] Journal writer thread started
2016-02-14T09:11:19.708-0200 I CONTROL  [initandlisten] 
2016-02-14T09:11:19.708-0200 I CONTROL  [initandlisten] ** WARNING: /sys/kernel/mm/transparent_hugepage/defrag is 'always'.
2016-02-14T09:11:19.708-0200 I CONTROL  [initandlisten] **        We suggest setting it to 'never'
2016-02-14T09:11:19.708-0200 I CONTROL  [initandlisten] 
2016-02-14T09:11:19.709-0200 I INDEX    [initandlisten] allocating new ns file /data/shard1/local.ns, filling with zeroes...
2016-02-14T09:11:20.255-0200 I STORAGE  [FileAllocator] allocating new datafile /data/shard1/local.0, filling with zeroes...
2016-02-14T09:11:20.255-0200 I STORAGE  [FileAllocator] creating directory /data/shard1/_tmp
2016-02-14T09:11:20.508-0200 I STORAGE  [FileAllocator] done allocating datafile /data/shard1/local.0, size: 64MB,  took 0.111 secs
2016-02-14T09:11:20.567-0200 I NETWORK  [initandlisten] waiting for connections on port 27012


// Shard 2

[Igor@fedora ~]$ mongod --port 27013 --dbpath /data/shard2
2016-02-14T09:10:29.225-0200 I CONTROL  [initandlisten] MongoDB starting : pid=30663 port=27013 dbpath=/data/shard2 64-bit host=fedora
2016-02-14T09:10:29.226-0200 I CONTROL  [initandlisten] db version v3.0.8
2016-02-14T09:10:29.226-0200 I CONTROL  [initandlisten] git version: 83d8cc25e00e42856924d84e220fbe4a839e605d
2016-02-14T09:10:29.226-0200 I CONTROL  [initandlisten] build info: Linux buildhw-03.phx2.fedoraproject.org 4.3.0-1.fc24.x86_64 #1 SMP Mon Nov 2 16:27:20 UTC 2015 x86_64 BOOST_LIB_VERSION=1_58
2016-02-14T09:10:29.226-0200 I CONTROL  [initandlisten] allocator: tcmalloc
2016-02-14T09:10:29.226-0200 I CONTROL  [initandlisten] options: { net: { port: 27013 }, storage: { dbPath: "/data/shard2" } }
2016-02-14T09:10:29.259-0200 I JOURNAL  [initandlisten] journal dir=/data/shard2/journal
2016-02-14T09:10:29.260-0200 I JOURNAL  [initandlisten] recover : no journal files present, no recovery needed
2016-02-14T09:11:21.665-0200 I JOURNAL  [initandlisten] preallocateIsFaster=true 935.56
2016-02-14T09:11:27.139-0200 I JOURNAL  [initandlisten] preallocateIsFaster=true 39.34
2016-02-14T09:11:33.608-0200 I JOURNAL  [initandlisten] preallocateIsFaster=true 39.44
2016-02-14T09:11:33.608-0200 I JOURNAL  [initandlisten] preallocateIsFaster check took 64.348 secs
2016-02-14T09:11:33.608-0200 I JOURNAL  [initandlisten] preallocating a journal file /data/shard2/journal/prealloc.0
2016-02-14T09:11:36.105-0200 I -        [initandlisten]   File Preallocator Progress: 943718400/1073741824 87%
2016-02-14T09:11:54.694-0200 I JOURNAL  [initandlisten] preallocating a journal file /data/shard2/journal/prealloc.1
2016-02-14T09:11:57.112-0200 I -        [initandlisten]   File Preallocator Progress: 73400320/1073741824 6%
2016-02-14T09:12:09.257-0200 I -        [initandlisten]   File Preallocator Progress: 146800640/1073741824 13%
2016-02-14T09:12:12.045-0200 I -        [initandlisten]   File Preallocator Progress: 859832320/1073741824 80%
2016-02-14T09:12:15.133-0200 I -        [initandlisten]   File Preallocator Progress: 1027604480/1073741824 95%
2016-02-14T09:12:31.905-0200 I JOURNAL  [initandlisten] preallocating a journal file /data/shard2/journal/prealloc.2
2016-02-14T09:12:34.106-0200 I -        [initandlisten]   File Preallocator Progress: 104857600/1073741824 9%
2016-02-14T09:12:37.015-0200 I -        [initandlisten]   File Preallocator Progress: 199229440/1073741824 18%
2016-02-14T09:12:40.175-0200 I -        [initandlisten]   File Preallocator Progress: 335544320/1073741824 31%
2016-02-14T09:12:43.168-0200 I -        [initandlisten]   File Preallocator Progress: 482344960/1073741824 44%
2016-02-14T09:12:46.908-0200 I -        [initandlisten]   File Preallocator Progress: 608174080/1073741824 56%
2016-02-14T09:12:49.652-0200 I -        [initandlisten]   File Preallocator Progress: 713031680/1073741824 66%
2016-02-14T09:12:52.170-0200 I -        [initandlisten]   File Preallocator Progress: 922746880/1073741824 85%
2016-02-14T09:13:13.941-0200 I JOURNAL  [durability] Durability thread started
2016-02-14T09:13:13.941-0200 I JOURNAL  [journal writer] Journal writer thread started
2016-02-14T09:13:14.497-0200 I CONTROL  [initandlisten] 
2016-02-14T09:13:14.497-0200 I CONTROL  [initandlisten] ** WARNING: /sys/kernel/mm/transparent_hugepage/defrag is 'always'.
2016-02-14T09:13:14.497-0200 I CONTROL  [initandlisten] **        We suggest setting it to 'never'
2016-02-14T09:13:14.497-0200 I CONTROL  [initandlisten] 
2016-02-14T09:13:14.497-0200 I INDEX    [initandlisten] allocating new ns file /data/shard2/local.ns, filling with zeroes...
2016-02-14T09:13:15.329-0200 I STORAGE  [FileAllocator] allocating new datafile /data/shard2/local.0, filling with zeroes...
2016-02-14T09:13:15.329-0200 I STORAGE  [FileAllocator] creating directory /data/shard2/_tmp
2016-02-14T09:13:15.885-0200 I STORAGE  [FileAllocator] done allocating datafile /data/shard2/local.0, size: 64MB,  took 0.444 secs
2016-02-14T09:13:16.151-0200 I NETWORK  [initandlisten] waiting for connections on port 27013

// Shard 3

[Igor@fedora ~]$ mongod --port 27014 --dbpath /data/shard3
2016-02-14T09:10:39.605-0200 I CONTROL  [initandlisten] MongoDB starting : pid=30765 port=27014 dbpath=/data/shard3 64-bit host=fedora
2016-02-14T09:10:39.605-0200 I CONTROL  [initandlisten] db version v3.0.8
2016-02-14T09:10:39.605-0200 I CONTROL  [initandlisten] git version: 83d8cc25e00e42856924d84e220fbe4a839e605d
2016-02-14T09:10:39.605-0200 I CONTROL  [initandlisten] build info: Linux buildhw-03.phx2.fedoraproject.org 4.3.0-1.fc24.x86_64 #1 SMP Mon Nov 2 16:27:20 UTC 2015 x86_64 BOOST_LIB_VERSION=1_58
2016-02-14T09:10:39.605-0200 I CONTROL  [initandlisten] allocator: tcmalloc
2016-02-14T09:10:39.605-0200 I CONTROL  [initandlisten] options: { net: { port: 27014 }, storage: { dbPath: "/data/shard3" } }
2016-02-14T09:10:39.640-0200 I JOURNAL  [initandlisten] journal dir=/data/shard3/journal
2016-02-14T09:10:39.640-0200 I JOURNAL  [initandlisten] recover : no journal files present, no recovery needed
2016-02-14T09:11:23.897-0200 I JOURNAL  [initandlisten] preallocateIsFaster=true 814.08
2016-02-14T09:11:28.100-0200 I JOURNAL  [initandlisten] preallocateIsFaster=true 58.44
2016-02-14T09:11:36.455-0200 I JOURNAL  [initandlisten] preallocateIsFaster=true 23.72
2016-02-14T09:11:36.455-0200 I JOURNAL  [initandlisten] preallocateIsFaster check took 56.814 secs
2016-02-14T09:11:36.455-0200 I JOURNAL  [initandlisten] preallocating a journal file /data/shard3/journal/prealloc.0
2016-02-14T09:11:39.233-0200 I -        [initandlisten]   File Preallocator Progress: 136314880/1073741824 12%
2016-02-14T09:11:42.127-0200 I -        [initandlisten]   File Preallocator Progress: 272629760/1073741824 25%
2016-02-14T09:11:48.719-0200 I -        [initandlisten]   File Preallocator Progress: 346030080/1073741824 32%
2016-02-14T09:11:51.085-0200 I -        [initandlisten]   File Preallocator Progress: 734003200/1073741824 68%
2016-02-14T09:11:54.022-0200 I -        [initandlisten]   File Preallocator Progress: 922746880/1073741824 85%
2016-02-14T09:12:14.223-0200 I JOURNAL  [initandlisten] preallocating a journal file /data/shard3/journal/prealloc.1
2016-02-14T09:12:17.066-0200 I -        [initandlisten]   File Preallocator Progress: 125829120/1073741824 11%
2016-02-14T09:12:20.080-0200 I -        [initandlisten]   File Preallocator Progress: 209715200/1073741824 19%
2016-02-14T09:12:23.008-0200 I -        [initandlisten]   File Preallocator Progress: 377487360/1073741824 35%
2016-02-14T09:12:26.096-0200 I -        [initandlisten]   File Preallocator Progress: 566231040/1073741824 52%
2016-02-14T09:12:29.144-0200 I -        [initandlisten]   File Preallocator Progress: 744488960/1073741824 69%
2016-02-14T09:12:32.114-0200 I -        [initandlisten]   File Preallocator Progress: 912261120/1073741824 84%
2016-02-14T09:12:35.630-0200 I -        [initandlisten]   File Preallocator Progress: 1048576000/1073741824 97%
2016-02-14T09:12:54.206-0200 I JOURNAL  [initandlisten] preallocating a journal file /data/shard3/journal/prealloc.2
2016-02-14T09:12:57.201-0200 I -        [initandlisten]   File Preallocator Progress: 10485760/1073741824 0%
2016-02-14T09:13:00.049-0200 I -        [initandlisten]   File Preallocator Progress: 241172480/1073741824 22%
2016-02-14T09:13:03.081-0200 I -        [initandlisten]   File Preallocator Progress: 419430400/1073741824 39%
2016-02-14T09:13:06.091-0200 I -        [initandlisten]   File Preallocator Progress: 587202560/1073741824 54%
2016-02-14T09:13:09.131-0200 I -        [initandlisten]   File Preallocator Progress: 754974720/1073741824 70%
2016-02-14T09:13:12.040-0200 I -        [initandlisten]   File Preallocator Progress: 912261120/1073741824 84%
2016-02-14T09:13:15.216-0200 I -        [initandlisten]   File Preallocator Progress: 1017118720/1073741824 94%
2016-02-14T09:13:32.980-0200 I JOURNAL  [durability] Durability thread started
2016-02-14T09:13:32.980-0200 I JOURNAL  [journal writer] Journal writer thread started
2016-02-14T09:13:33.028-0200 I CONTROL  [initandlisten] 
2016-02-14T09:13:33.028-0200 I CONTROL  [initandlisten] ** WARNING: /sys/kernel/mm/transparent_hugepage/defrag is 'always'.
2016-02-14T09:13:33.028-0200 I CONTROL  [initandlisten] **        We suggest setting it to 'never'
2016-02-14T09:13:33.028-0200 I CONTROL  [initandlisten] 
2016-02-14T09:13:33.028-0200 I INDEX    [initandlisten] allocating new ns file /data/shard3/local.ns, filling with zeroes...
2016-02-14T09:13:33.512-0200 I STORAGE  [FileAllocator] allocating new datafile /data/shard3/local.0, filling with zeroes...
2016-02-14T09:13:33.513-0200 I STORAGE  [FileAllocator] creating directory /data/shard3/_tmp
2016-02-14T09:13:34.166-0200 I STORAGE  [FileAllocator] done allocating datafile /data/shard3/local.0, size: 64MB,  took 0.448 secs
2016-02-14T09:13:34.203-0200 I NETWORK  [initandlisten] waiting for connections on port 27014



// Registrando Shards

[Igor@fedora ~]$ mongo --port 27011 --host localhost
MongoDB shell version: 3.0.8
connecting to: localhost:27011/test
Mongo-Hacker 0.0.12
mongos> sh.addShard("localhost:27013")
{
  "shardAdded": "shard0000",
  "ok": 1
}
fedora:27011(mongos-3.0.8)[mongos] test> sh.addShard("localhost:27012")
{
  "shardAdded": "shard0001",
  "ok": 1
}
fedora:27011(mongos-3.0.8)[mongos] test> sh.addShard("localhost:27014")
{
  "shardAdded": "shard0002",
  "ok": 1
}


// Colocando Dados

fedora:27011(mongos-3.0.8)[mongos] test> sh.enableSharding("project")
{
  "ok": 1
}

fedora:27011(mongos-3.0.8)[mongos] project> sh.shardCollection("project.new",{"_id": 1})
{
  "collectionsharded": "project.new",
  "ok": 1
}

function randomDate() {
    var time = new Date().getTime(),
        randomTime = time * Math.random();

    return new Date(randomTime);
}

function randomNumber() {
    var number = (Math.random() * 2) + 1;
    return number.toFixed(3);
}


var projectName = [];

while(projectName.length < 10000) {
    projectName.push({ 
        project: "project - " + randomNumber(), 
        decription: "CreatedAt - " + randomDate()
    });
}

var allTags = [
    "PHP","Python","Javascript",
    "Ruby","Java","Lisp","Lua",
    "Dart","C#","ASP",".NET",
    "Visal Basic", "Pascal","Assembly"
];



function registerProjects() {

    for(var i=0; i<10000; i++) {
        db.new.insert({
            name: projectName[i].project,
            decription: projectName[i].decription,
            date_begin: new Date("Tue Jan 08 1980 03:31:44 GMT-0300 (BRT)"),
            date_dream: new Date("Sun Oct 20 2002 17:17:13 GMT-0300 (BRT)"),
            date_end: new Date("Mon Mar 09 2015 05:38:07 GMT-0300 (BRT)"),
            visible: true,
            realocate: undefined,
            expired: false,
            visualizable_mod: null,

            members: [],

            tags: allTags.slice(i,10),

            goal: []

        });
    }
}

registerProjects()


```

