# MongoDb - Projeto Final
**Autor:** Emídio de Paiva Neto
**Data:** 1453154425965


## Para qual sistema você usaria o MongoDB (diferente desse)?
A princípio para sistemas que prezam por velocidade de leitura, e que seus dados venham a crescer repentinamente, sendo possível dividi-los e continuar trabalhando em boa performance. Como exemplo, sistemas que usam geolocation, E-commerce, chats com mensagens em real-time, redes sociais de compartilhamento de músicas e de vídeos em alta resolução.
## Qual a modelagem da sua coleção de `users`?
```js
{
        _id,
        name,
        bio,
        date_register,
        avatar,
        background,
        auth: {
            username,
            email,
            password,
            last_access,
            online,
            disabled,
            hash_token
        }
}
```
## Qual a modelagem da sua coleção de `projects`?
```js
{
        _id,
        name,
        description,
        date_begin,
        date_dream,
        date_end,
        visible,
        realocate,
        expired,
        visualizable_mod,
        tags: [],
        members: [{
            user_id,
            type,
            notify
        }],
        goals: [{
            name,
            description,
            date_begin,
            date_dream,
            date_end,
            realocate,
            expired,
            tags: [],
            historic: [{
                    date_realocate
                }],
            activities: [
                { activity_id }
            ]
        }]
    }
```
## Qual a modelagem da sua coleção retirada de `projects`?
Retirar activities de projects, foi pensando na melhor forma de retornar os dados do sistema. Imagine se fossem várias activities, e cada activities com vários comentários, membros, etc, visto que, teriam campos que iriam crescer rapidamente e seria bem mais complicado de retornar os dados se activities ficasse dentro de projects. Sendo assim partimos do conceito que o mongodb preza pela velocidade e para isso, os dados precisam de certa forma estar os mais fáceis possíveis para serem consultados, partindo do pressuposto de como o nosso sistema fará perguntas ao banco de dados.
```js
{
        _id,
        name,
        description,
        date_begin,
        date_dream,
        date_end,
        realocate,
        expired,
        tags: [],
        members: [{
            user_id,
            type: [],
            notify
        }],
        comments: [{
            text,
            date_comment,
            members: [{
                user_id,
                type: [],
                notify
            }],
            files: [{
                name,
                path,
                weigth
            }]
        }],
        historic: [
        { date_realocate }
      ]
}
```
## Create - cadastro
1. Cadastrar 10 usuários
```js
var query = [
{
        name: "userOne",
        bio: "No bio.",
        date_register: Date.now(),
        avatar: "avatarimage.png",
        backgound: "background.png",
        auth: {
            username: "userone",
            email: "userone@user.com.br",
            password: "123456",
            last_access: Date.now(),
            online: false,
            disabled: false,
            hash_token: "d41d8cd98f00b204e9800998ecf8427e"
        }
},
{
        name: "userTwo",
        bio: "No bio.",
        date_register: Date.now(),
        avatar: "avatarimage.png",
        backgound: "background.png",
        auth: {
            username: "usertwo",
            email: "usertwo@user.com.br",
            password: "123456",
            last_access: Date.now(),
            online: false,
            disabled: false,
            hash_token: "d41d8cd98f00b204e9800998ecf8427e"
        }
},
{
        name: "userThree",
        bio: "No bio.",
        date_register: Date.now(),
        avatar: "avatarimage.png",
        backgound: "background.png",
        auth: {
            username: "userthree",
            email: "userthree@user.com.br",
            password: "123456",
            last_access: Date.now(),
            online: false,
            disabled: false,
            hash_token: "d41d8cd98f00b204e9800998ecf8427e"
        }
},
{
        name: "userFour",
        bio: "No bio.",
        date_register: Date.now(),
        avatar: "avatarimage.png",
        backgound: "background.png",
        auth: {
            username: "userfour",
            email: "userfour@user.com.br",
            password: "123456",
            last_access: Date.now(),
            online: false,
            disabled: false,
            hash_token: "d41d8cd98f00b204e9800998ecf8427e"
        }
},
{
        name: "userFive",
        bio: "No bio.",
        date_register: Date.now(),
        avatar: "avatarimage.png",
        backgound: "background.png",
        auth: {
            username: "userfive",
            email: "userfive@user.com.br",
            password: "123456",
            last_access: Date.now(),
            online: false,
            disabled: false,
            hash_token: "d41d8cd98f00b204e9800998ecf8427e"
        }
},
{
        name: "userSix",
        bio: "No bio.",
        date_register: Date.now(),
        avatar: "avatarimage.png",
        backgound: "background.png",
        auth: {
            username: "usersix",
            email: "usersix@user.com.br",
            password: "123456",
            last_access: Date.now(),
            online: false,
            disabled: false,
            hash_token: "d41d8cd98f00b204e9800998ecf8427e"
        }
},
{
        name: "userSeven",
        bio: "No bio.",
        date_register: Date.now(),
        avatar: "avatarimage.png",
        backgound: "background.png",
        auth: {
            username: "userseven",
            email: "userseven@user.com.br",
            password: "123456",
            last_access: Date.now(),
            online: false,
            disabled: false,
            hash_token: "d41d8cd98f00b204e9800998ecf8427e"
        }
},
{
        name: "userEigth",
        bio: "No bio.",
        date_register: Date.now(),
        avatar: "avatarimage.png",
        backgound: "background.png",
        auth: {
            username: "usereight",
            email: "usereigth@user.com.br",
            password: "123456",
            last_access: Date.now(),
            online: false,
            disabled: false,
            hash_token: "d41d8cd98f00b204e9800998ecf8427e"
        }
},
{
        name: "userNine",
        bio: "No bio.",
        date_register: Date.now(),
        avatar: "avatarimage.png",
        backgound: "background.png",
        auth: {
            username: "usernine",
            email: "usernine@user.com.br",
            password: "123456",
            last_access: Date.now(),
            online: false,
            disabled: false,
            hash_token: "d41d8cd98f00b204e9800998ecf8427e"
        }
},
{
        name: "userTen",
        bio: "No bio.",
        date_register: Date.now(),
        avatar: "avatarimage.png",
        backgound: "background.png",
        auth: {
            username: "userten",
            email: "userten@user.com.br",
            password: "123456",
            last_access: Date.now(),
            online: false,
            disabled: false,
            hash_token: "d41d8cd98f00b204e9800998ecf8427e"
        }
}
]

db.users.insert(query)
Inserted 1 record(s) in 19ms
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
2. Cadastre 5 projetos diferentes.
    - cada um com 5 membros, sempre diferentes dentro dos projetos;
    - cada um com pelo menos 3 tags diferentes;
        - escolha 1 *tag* onde deva ficar em 2 projetos;
        - escolha 1 *tag* onde deva ficar em 3 projetos;
    - cada projeto com pelo menos 1 *goal*;
        - cada *goal* com pelo menos 3 *tags*;
        - cada *goal* com pelo menos 2 atividades, deixe 1 projeto sem.

```js
for (var i = 0; i < 10 ; i++) {
var activity = {
        name: "Atividade"+ i,
        description: "Atividade" + i,
        date_begin: new Date(),
        date_dream: null,
        date_end: null,
        realocate: false,
        expired: false,
        tags: [],
        members: [],
        comments: [],
        historic: [
        { date_realocate: new Date() }
        ]
}
db.activities.insert(activity)
}

var projects = [
{
        name: "Projeto 1",
        description: "Projeto 1 em breve.",
        date_begin: new Date(),
        date_dream: null,
        date_end: null,
        visible: true,
        realocate: false,
        expired: false,
        visualizable_mod: null,
        tags: ["awp", "books", "music"],
        members: [{
            user_id: ObjectId("56882244ed3bac813c59054b"),
            type: ["owner"],
            notify: true
        },
        {
            user_id: ObjectId("56882244ed3bac813c59054c"),
            type: ["escritor"],
            notify: false
        },
        {
            user_id: ObjectId("56882244ed3bac813c59054d"),
            type: ["escritor"],
            notify: false
        },
        {
            user_id: ObjectId("56882244ed3bac813c59054e"),
            type: ["analista"],
            notify: true
        },
        {
            user_id: ObjectId("56882244ed3bac813c59054f"),
            type: ["user"],
            notify: true
        }],
        goals: [{
            name: "Goal One",
            description: "GoalOne...",
            date_begin: new Date(),
            date_dream: null,
            date_end: null,
            realocate: false,
            expired: false,
            tags: ["GoalOne", "em", "breve"],
            historic: [{
                    date_realocate : new Date()
                }],
            activities: [
                {activity_id: ObjectId("56896393053a1e7b71a849f0")},
                {activity_id: ObjectId("56896393053a1e7b71a849f1")}
            ]
        }]
},
{
        name: "Projeto 2",
        description: "Projeto 2 em breve.",
        date_begin: new Date(),
        date_dream: null,
        date_end: null,
        visible: true,
        realocate: false,
        expired: false,
        visualizable_mod: null,
        tags: ["awp", "books", "rock"],
        members: [{
            user_id: ObjectId("56882244ed3bac813c590550"),
            type: ["owner"],
            notify: true
        },
        {
            user_id: ObjectId("56882244ed3bac813c590551"),
            type: ["escritor"],
            notify: false
        },
        {
            user_id: ObjectId("56882244ed3bac813c590552"),
            type: ["escritor"],
            notify: false
        },
        {
            user_id: ObjectId("56882244ed3bac813c590553"),
            type: ["analista"],
            notify: true
        },
        {
            user_id: ObjectId("56882244ed3bac813c590554"),
            type: ["user"],
            notify: true
        }],
        goals: [{
            name: "Goal Two",
            description: "GoalTwo...",
            date_begin: new Date(),
            date_dream: null,
            date_end: null,
            realocate: false,
            expired: false,
            tags: ["GoalTwo", "better", "goal"],
            historic: [{
                    date_realocate : new Date()
                }],
            activities: [
                {activity_id: ObjectId("56896393053a1e7b71a849f2")},
                {activity_id: ObjectId("56896393053a1e7b71a849f3")}
            ]
        }]
},
{
        name: "Projeto 3",
        description: "Projeto 3 em breve.",
        date_begin: new Date(),
        date_dream: null,
        date_end: null,
        visible: true,
        realocate: false,
        expired: false,
        visualizable_mod: null,
        tags: ["create", "books", "read"],
        members: [{
            user_id: ObjectId("56882244ed3bac813c590554"),
            type: ["owner"],
            notify: true
        },
        {
            user_id: ObjectId("56882244ed3bac813c590553"),
            type: ["escritor"],
            notify: false
        },
        {
            user_id: ObjectId("56882244ed3bac813c590552"),
            type: ["escritor"],
            notify: false
        },
        {
            user_id: ObjectId("56882244ed3bac813c590551"),
            type: ["analista"],
            notify: true
        },
        {
            user_id: ObjectId("56882244ed3bac813c59054c"),
            type: ["user"],
            notify: true
        }],
        goals: [{
            name: "Goal Three",
            description: "GoalThree.",
            date_begin: new Date(),
            date_dream: null,
            date_end: null,
            realocate: false,
            expired: false,
            tags: ["GoalThree", "essa", "semana"],
            historic: [{
                    date_realocate : new Date()
                }],
            activities: []
        }]
},
{
        name: "Projeto 4",
        description: "Projeto 4 em breve.",
        date_begin: new Date(),
        date_dream: null,
        date_end: null,
        visible: true,
        realocate: false,
        expired: false,
        visualizable_mod: null,
        tags: ["música", "interação", "compartilhamento"],
        members: [{
            user_id: ObjectId("56882244ed3bac813c59054c"),
            type: ["owner"],
            notify: true
        },
        {
            user_id: ObjectId("56882244ed3bac813c59054e"),
            type: ["escritor"],
            notify: false
        },
        {
            user_id: ObjectId("56882244ed3bac813c590550"),
            type: ["escritor"],
            notify: false
        },
        {
            user_id: ObjectId("56882244ed3bac813c590552"),
            type: ["analista"],
            notify: true
        },
        {
            user_id: ObjectId("56882244ed3bac813c590554"),
            type: ["user"],
            notify: true
        }],
        goals: [{
            name: "Goal Four",
            description: "GoalFour.",
            date_begin: new Date(),
            date_dream: null,
            date_end: null,
            realocate: false,
            expired: false,
            tags: ["GoalFour", "mic", "social"],
            historic: [{
                    date_realocate : new Date()
                }],
                activities: [
                    {activity_id: ObjectId("56896393053a1e7b71a849f4")},
                    {activity_id: ObjectId("56896393053a1e7b71a849f5")}
                ]
        }]
},
{
        name: "Projeto 5",
        description: "Projeto 5 em breve.",
        date_begin: new Date(),
        date_dream: null,
        date_end: null,
        visible: true,
        realocate: false,
        expired: false,
        visualizable_mod: null,
        tags: ["project", "final", "cool"],
        members: [{
            user_id: ObjectId("56882244ed3bac813c59054b"),
            type: ["owner"],
            notify: true
        },
        {
            user_id: ObjectId("56882244ed3bac813c59054d"),
            type: ["escritor"],
            notify: false
        },
        {
            user_id: ObjectId("56882244ed3bac813c59054f"),
            type: ["escritor"],
            notify: false
        },
        {
            user_id: ObjectId("56882244ed3bac813c590551"),
            type: ["analista"],
            notify: true
        },
        {
            user_id: ObjectId("56882244ed3bac813c590553"),
            type: ["user"],
            notify: true
        }],
        goals: [{
            name: "Goal Five",
            description: "GoalFive.",
            date_begin: new Date(),
            date_dream: null,
            date_end: null,
            realocate: false,
            expired: false,
            tags: ["GoalFive", "mês", "janeiro"],
            historic: [{
                    date_realocate : new Date()
                }],
                activities: [
                    {activity_id: ObjectId("56896393053a1e7b71a849f8")},
                    {activity_id: ObjectId("56896393053a1e7b71a849f7")}
                ]
        }]
}
]

db.projects.insert(projects)
Inserted 1 record(s) in 19ms
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
1. Liste as informações dos membros de 1 projeto específico que deve ser buscado pelo seu nome de forma a não ligar para maiúsculas e minúsculas.

```js
pMembers = []
getInfo = function(id) {
pMembers.push(db.users.findOne({_id: id.user_id}))
}
db.projects.findOne({name: /projeto 4/i}).members.forEach(getInfo)

pMembers
[
  {
    "_id": ObjectId("56882244ed3bac813c59054c"),
    "name": "userTwo",
    "bio": "No bio.",
    "date_register": 1451762235890,
    "avatar": "avatarimage.png",
    "backgound": "background.png",
    "auth": {
      "username": "usertwo",
      "email": "usertwo@user.com.br",
      "password": "123456",
      "last_access": 1451762235890,
      "online": false,
      "disabled": false,
      "hash_token": "d41d8cd98f00b204e9800998ecf8427e"
    }
  },
  {
    "_id": ObjectId("56882244ed3bac813c59054e"),
    "name": "userFour",
    "bio": "No bio.",
    "date_register": 1451762235890,
    "avatar": "avatarimage.png",
    "backgound": "background.png",
    "auth": {
      "username": "userfour",
      "email": "userfour@user.com.br",
      "password": "123456",
      "last_access": 1451762235890,
      "online": false,
      "disabled": false,
      "hash_token": "d41d8cd98f00b204e9800998ecf8427e"
    }
  },
  {
    "_id": ObjectId("56882244ed3bac813c590550"),
    "name": "userSix",
    "bio": "No bio.",
    "date_register": 1451762235890,
    "avatar": "avatarimage.png",
    "backgound": "background.png",
    "auth": {
      "username": "usersix",
      "email": "usersix@user.com.br",
      "password": "123456",
      "last_access": 1451762235890,
      "online": false,
      "disabled": false,
      "hash_token": "d41d8cd98f00b204e9800998ecf8427e"
    }
  },
  {
    "_id": ObjectId("56882244ed3bac813c590552"),
    "name": "userEigth",
    "bio": "No bio.",
    "date_register": 1451762235890,
    "avatar": "avatarimage.png",
    "backgound": "background.png",
    "auth": {
      "username": "usereight",
      "email": "usereigth@user.com.br",
      "password": "123456",
      "last_access": 1451762235890,
      "online": false,
      "disabled": false,
      "hash_token": "d41d8cd98f00b204e9800998ecf8427e"
    }
  },
  {
    "_id": ObjectId("56882244ed3bac813c590554"),
    "name": "userTen",
    "bio": "No bio.",
    "date_register": 1451762235890,
    "avatar": "avatarimage.png",
    "backgound": "background.png",
    "auth": {
      "username": "userten",
      "email": "userten@user.com.br",
      "password": "123456",
      "last_access": 1451762235890,
      "online": false,
      "disabled": false,
      "hash_token": "d41d8cd98f00b204e9800998ecf8427e"
    }
  }
]
```
2. Liste todos os projetos com a tag que você escolheu para os 3 projetos em comum.
```js
db.projects.find({tags: { $in: [/books/i]}}, {name: 1, tags: 1})
{
  "_id": ObjectId("56896d326bd73990ade0e32f"),
  "name": "Projeto 1",
  "tags": [
    "awp",
    "books",
    "music"
  ]
}
{
  "_id": ObjectId("56896d326bd73990ade0e330"),
  "name": "Projeto 2",
  "tags": [
    "awp",
    "books",
    "rock"
  ]
}
{
  "_id": ObjectId("56896d326bd73990ade0e331"),
  "name": "Projeto 3",
  "tags": [
    "create",
    "books",
    "read"
  ]
}
Fetched 3 record(s) in 1ms
```
3. Liste apenas os nomes de todas as atividades para todos os projetos.
```js
var pActivity = db.projects.distinct("goals.activities.activity_id")
pActivity
[
  ObjectId("56896393053a1e7b71a849f0"),
  ObjectId("56896393053a1e7b71a849f1"),
  ObjectId("56896393053a1e7b71a849f2"),
  ObjectId("56896393053a1e7b71a849f3"),
  ObjectId("56896393053a1e7b71a849f4"),
  ObjectId("56896393053a1e7b71a849f5"),
  ObjectId("56896393053a1e7b71a849f7"),
  ObjectId("56896393053a1e7b71a849f8")
]

  for(var i = 0; i < pActivity.length; i++){
    var search = db.activities.findOne(pActivity[i]);
    a.push(search.name);
  }
8
bemean> a
[
  "Atividade0",
  "Atividade1",
  "Atividade2",
  "Atividade3",
  "Atividade4",
  "Atividade5",
  "Atividade7",
  "Atividade8"
]
```
4. Liste todos os projetos que não possuam uma tag.
```js
db.projects.find({ tags: {$not: {$in: [/books/i]}}}, {name:1, _id:0, tags:1})
{
  "name": "Projeto 4",
  "tags": [
    "música",
    "interação",
    "compartilhamento"
  ]
}
{
  "name": "Projeto 5",
  "tags": [
    "project",
    "final",
    "cool"
  ]
}
```
5. Liste todos os usuários que não fazem parte do primeiro projeto cadastrado.
```js
 var fUsers = db.projects.distinct( "members.user_id", { name: /projeto 1/i } )
 db.users.find({_id: { $not: { $in : fUsers } } }, {name:1})
{
  "_id": ObjectId("56882244ed3bac813c590550"),
  "name": "userSix"
}
{
  "_id": ObjectId("56882244ed3bac813c590551"),
  "name": "userSeven"
}
{
  "_id": ObjectId("56882244ed3bac813c590552"),
  "name": "userEigth"
}
{
  "_id": ObjectId("56882244ed3bac813c590553"),
  "name": "userNine"
}
{
  "_id": ObjectId("56882244ed3bac813c590554"),
  "name": "userTen"
}
```
## Update - alteração
1. Adicione para todos os projetos o campo `views: 0`.
```js
db.projects.update({}, {$set: { views: 0 }}, {multi: true})
Updated 5 existing record(s) in 4ms
WriteResult({
  "nMatched": 5,
  "nUpserted": 0,
  "nModified": 5
})
db.projects.find({}, {views: 1, name: 1})
{
  "_id": ObjectId("56896d326bd73990ade0e32f"),
  "name": "Projeto 1",
  "views": 0
}
{
  "_id": ObjectId("56896d326bd73990ade0e330"),
  "name": "Projeto 2",
  "views": 0
}
{
  "_id": ObjectId("56896d326bd73990ade0e331"),
  "name": "Projeto 3",
  "views": 0
}
{
  "_id": ObjectId("56896d326bd73990ade0e332"),
  "name": "Projeto 4",
  "views": 0
}
{
  "_id": ObjectId("56896d326bd73990ade0e333"),
  "name": "Projeto 5",
  "views": 0
}
```
2. Adicione 1 tag diferente para cada projeto.
```js
var pId = db.projects.distinct("_id")
for( var i = 0; i < pId.length; i++){
db.projects.update({_id: pId[i]}, {$push: {tags: "new tag " + i}})
}
Updated 1 existing record(s) in 1ms
Updated 1 existing record(s) in 1ms
Updated 1 existing record(s) in 1ms
Updated 1 existing record(s) in 1ms
Updated 1 existing record(s) in 0ms
Updated 1 existing record(s) in 0ms
WriteResult({
  "nMatched": 1,
  "nUpserted": 0,
  "nModified": 1
})

db.projects.find({},{tags:1, name:1})
{
  "_id": ObjectId("56896471053a1e7b71a849fa"),
  "name": "Projeto 1",
  "tags": [
    "awp",
    "books",
    "music",
    "new tag 0"
  ]
}
{
  "_id": ObjectId("5689ce1395d7b39e3724455f"),
  "name": "Projeto 2",
  "tags": [
    "awp",
    "books",
    "rock",
    "new tag 1"
  ]
}
{
  "_id": ObjectId("5689ce1395d7b39e37244560"),
  "name": "Projeto 3",
  "tags": [
    "create",
    "books",
    "read",
    "new tag 2"
  ]
}
{
  "_id": ObjectId("5689ce1395d7b39e37244561"),
  "name": "Projeto 4",
  "tags": [
    "música",
    "interação",
    "compartilhamento",
    "new tag 3"
  ]
}
{
  "_id": ObjectId("5689ce1395d7b39e37244562"),
  "name": "Projeto 5",
  "tags": [
    "project",
    "final",
    "cool",
    "new tag 4"
  ]
}
```
3. Adicione 2 membros diferentes para cada projeto.
```js
var fUsers = db.projects.distinct( "members.user_id")

db.projects.find({}, {_id: 1, members: 1}).forEach(function(project){  
    var sixMemb = {
        user_id : fUsers[3],
        type_member : ["analista"],
        notify : true
    };

    var sevMemb = {
        user_id : fUsers[6],
        type_member : ["escritor"],
        notify : true
    };

    db.projects.update({ _id: project._id }, { $push: { members: sevMemb } });
    db.projects.update({ _id: project._id }, { $push: { members: sixMemb } });
    })
```
4. Adicione 1 comentário em cada atividade, deixe apenas 1 projeto sem.
```js
var activities = db.activities.find({}, {_id: 1, name: 1}).toArray();
var atvID = []
var filter = activities.filter(function(value) {
    if(value.name != "Atividade0") {
        atvID.push(value._id)
      }

})
var users = db.users.distinct("_id")

var newComment = {
        text: "Comentário X",
        date_comment : new Date(),
        members: [{
            user_id: users[Math.floor(Math.random()*users.length)],
            type: ["comentario"],
            notify: false
            }],
        files : [{
                path : "images/",
                weight : "10Kb",
                name : "emoticon.png"
            }]
}

db.activities.update({_id: {$in: atvID}}, { $push: { comments: newComment } }, {multi: true})

Updated 9 existing record(s) in 1ms
WriteResult({
  "nMatched": 9,
  "nUpserted": 0,
  "nModified": 9
})
db.activities.count()
10
```
5. Adicione 1 projeto inteiro com **UPSERT**.
```js
var query = {name: /The last/i}
var mod = {
 $setOnInsert : {
   "name": "The Last Project",
   "description": "Projeto em breve...",
   "date_begin": ISODate("2016-01-04T21:46:41.051Z"),
   "date_dream": null,
   "date_end": null,
   "visible": true,
   "realocate": false,
   "expired": false,
   "visualizable_mod": null,
   "tags": [
     "the",
     "last",
     "one"
   ],
   "members": [
     {
       "user_id": ObjectId("56882244ed3bac813c59054b"),
       "type": [
         "owner"
       ],
       "notify": true
     },
     {
       "user_id": ObjectId("56882244ed3bac813c59054c"),
       "type": [
         "member"
       ],
       "notify": true
     },
     {
       "user_id": ObjectId("56882244ed3bac813c59054d"),
       "type": [
         "escritor"
       ],
       "notify": true
     },
     {
       "user_id": ObjectId("56882244ed3bac813c59054e"),
       "type": [
         "analista"
       ],
       "notify": true
     },
     {
       "user_id": ObjectId("56882244ed3bac813c59054f"),
       "type": [
         "analista"
       ],
       "notify": true
     }
   ],
   "goals": [
     {
       "name": "The last Goal",
       "description": "LastGoal.",
       "date_begin": ISODate("2016-01-04T21:46:41.051Z"),
       "date_dream": null,
       "date_end": null,
       "realocate": false,
       "expired": false,
       "tags": [
         "LastGoal",
         "último",
         "last"
       ],
       "historic": [
         {
           "date_realocate": ISODate("2016-01-04T21:46:41.051Z")
         }
       ],
       "activities": [
         {
           "activity_id": ObjectId("56896393053a1e7b71a849f9")
         },
         {
           "activity_id": ObjectId("56896393053a1e7b71a849f5")
         }
       ]
     }
   ],
   "views": 0
 }
}
var options = {upsert: true}
db.projects.update(query, mod, options)
Updated 1 new record(s) in 1ms
WriteResult({
  "nMatched": 0,
  "nUpserted": 1,
  "nModified": 0,
  "_id": ObjectId("568aeb08bff1ddb0929536bf")
})
```
## Delete - remoção
1. Apague todos os projetos que não possuam *tags*.
```js
db.projects.remove({ tags: { $size: 0 } })
Removed 0 record(s) in 1ms
WriteResult({
  "nRemoved": 0
})

```
2. Apague todos os projetos que não possuam comentários nas atividades.
```js
var activities = db.activities.find({}, {_id:1, comments:1}).toArray();
var ids = []
var filter = activities.filter(function(act) {
    if(act.comments.length == 0) {
    ids.push(act._id)
  }
})

db.projects.remove({"goals.activities.activity_id": { $in: ids}}, {multi: 1})
Removed 1 record(s) in 1ms
WriteResult({
  "nRemoved": 1
})
```
3. Apague todos os projetos que não possuam atividades.
```js
db.projects.remove({"goals.activities": {$size: 0}}, {multi: 1})
Removed 1 record(s) in 1ms
WriteResult({
  "nRemoved": 1
})

```
4. Escolha 2 usuário e apague todos os projetos em que os 2 fazem parte.
```js
var ids = db.users.find({name: {$in: [/userten/i, /usereigth/i]}}, {_id:1, name:1}).toArray();
var aID = []
ids.forEach(function(user){
    aID.push(user._id)
})
db.projects.remove({ "members.user_id": { $in: aID } }, { multi: 1 })
Removed 2 record(s) in 1ms
WriteResult({
  "nRemoved": 2
})
```
5. Apague todos os projetos que possuam uma determinada *tag* em *goal*.
```js
db.projects.remove({"goals.tags": {$eq: "GoalFive"}}, {multi: 1})
Removed 1 record(s) in 1ms
WriteResult({
  "nRemoved": 1
})
```
## Gerenciamento de usuários

1. Crie um usuário com permissões **APENAS** de Leitura.
```js
use admin
db.createUser(
  {
    user: "member",
    pwd: "membx1",
    roles: [ { role: "read", db: "admin" } ]
  }
)
Successfully added user: {
  "user": "member",
  "roles": [
    {
      "role": "read",
      "db": "admin"
    }
  ]
}
```
2. Crie um usuário com permissões de Escrita e Leitura.
```js
db.runCommand( { createUser: "escritor",
  pwd: "escritorx1",
   roles: [
      { role: "readWrite", db: "admin" }
     ]
   } )

{
  "ok": 1
}

```
3. Adicionar o papel `grantRolesToUser` e `revokeRole` para o usuário com Escrita e Leitura.
```js
//Criando a role grantRolesToUser
db.createRole(
{        
        role: "grantRolesToUser",
        privileges: [
        {resource: {db: "admin", collection: ""}, actions: ["grantRole"] }       
         ],
         roles: ["readWrite"]
       }
)

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
// Criando a role RevokeRole
db.createRole(
{        
        role: "revokeRole",
        privileges: [
        {resource: {db: "admin", collection: "" }, actions: ["revokeRole"] }       
         ],
         roles: ["readWrite"]
       }
)
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
  "roles": [
    "readWrite"
  ]
}
// Adicionando as roles
db.grantRolesToUser("escritor",["grantRolesToUser","revokeRole"])
```
4. Remover o papel `grantRolesToUser` para o usuário com Escrita e Leitura.
```js
db.revokeRolesFromUser("escritor",["grantRolesToUser"])
```
5. Listar todos os usuários com seus papéis e ações.
```js
db.runCommand({ usersInfo:[{ user: "escritor", db: "admin" }, { user: "member", db: "admin" }],
showCredentials: 1,
showPrivileges: 1
}
)

{
"users": [
  {
    "_id": "admin.escritor",
    "user": "escritor",
    "db": "admin",
    "credentials": {
      "SCRAM-SHA-1": {
        "iterationCount": 10000,
        "salt": "XG/5n9CMqpjquzFZd1yzlg==",
        "storedKey": "8+AKzAVzJ57lIkNNazObtGtllKk=",
        "serverKey": "s1cAT9fCA1Jwwkrvgm7R7RZ6btY="
      }
    },
    "roles": [
      {
        "role": "revokeRole",
        "db": "admin"
      },
      {
        "role": "readWrite",
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
  },
  {
    "_id": "admin.member",
    "user": "member",
    "db": "admin",
    "credentials": {
      "SCRAM-SHA-1": {
        "iterationCount": 10000,
        "salt": "yF0HYd8w/KTLsOfHLH7fWA==",
        "storedKey": "S9SvckOMJDP91l2d46vc5vkb/Vk=",
        "serverKey": "pyG9B1ofQ2cjzUYuYeRYCeQ04nw="
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
  }
],
"ok": 1
}
```

## Sharding
Primeiramente criamos um Config Server, usando o atributo --configsvr
```js
  mkdir /data/configdb
  mongod --configsvr --port 27010
```
  > A indicação oficial é de criar pelo menos 3 Config Server para não ter um ponto único de falha.

  Depois, criamos o Router utilizando o mongos informando qual Config Server que ele irá acessar para obter as informações dos Shards.
```js
  mongos --configdb localhost:27010 --port 27011
```
Criando os Shards
```js
   mkdir /data/shard1
   mkdir /data/shard2
   mkdir /data/shard3
```
   Shard 1
```js
   mongod --port 27012 --dbpath /data/shard1
```
   Shard 2
```js
   mongod --port 27013 --dbpath /data/shard2
```
   Shard 3
```js
   mongod --port 27014 --dbpath /data/shard3
```
Registrando os Shards no Router
```js
   mongo --port 27011 --host localhost
```
```js
   sh.addShard("localhost:27012")
{
  "shardAdded": "shard0000",
  "ok": 1
}
```
```js
   sh.addShard("localhost:27013")
{
  "shardAdded": "shard0001",
  "ok": 1
}
```
```js
   sh.addShard("localhost:27014")
{
  "shardAdded": "shard0002",
  "ok": 1
}
```
   Especificando a Database e Coleção a ser shardeada.
```js
   sh.enableSharding("bemean")
{
  "ok": 1
}
```
```js
   sh.shardCollection("bemean.activities", {"_id": 1})
{
  "collectionsharded": "bemean.activities",
  "ok": 1
}
```

## Replica
Replica
```js
mkdir /data/rs1
mkdir /data/rs2
mkdir /data/rs3
```
Replica 1
```js
mongod --replSet replica_set --port 27018 --dbpath /data/rs1
```

Replica 2
```js
mongod --replSet replica_set --port 27019 --dbpath /data/rs2
```

Replica 3
```js
mongod --replSet replica_set --port 27020 --dbpath /data/rs3
```
Configurando e iniciando
```js
mongo --port 27018
json = {
   _id: "replica_set",
   members: [
    {
     _id: 0,
     host: "127.0.0.1:27012"
    }
  ]
}
rs.initiate(json)
rs.add("127.0.0.1:27019")
rs.add("127.0.0.1:27020")
```
```js
db.printShardingStatus()
--- Sharding Status ---
  sharding version: {
    "_id": 1,
    "minCompatibleVersion": 5,
    "currentVersion": 6,
    "clusterId": ObjectId("568bf86eb47f4c081048c36b")
  }
  shards:
    {  "_id": "shard0000",  "host": "localhost:27012" }
    {  "_id": "shard0001",  "host": "localhost:27013" }
    {  "_id": "shard0002",  "host": "localhost:27014" }
  balancer:
	Currently enabled:  yes
	Currently running:  no
	Failed balancer rounds in last 5 attempts:  0
	Migration Results for the last 24 hours:
		No recent migrations
  databases:
    {  "_id": "bemean",  "primary": "shard0000",  "partitioned": true }
    bemean.activities
      shard key: { "_id": 1 }
      chunks:
        shard0000: 1
        { "_id": [object MinKey] } -> { "_id": [object MaxKey] } on: shard0000 Timestamp(1, 0)

```
```js
rs.status()
{
  "set": "replica_set",
  "date": ISODate("2016-01-18T21:56:32.245Z"),
  "myState": 1,
  "term": NumberLong("2"),
  "heartbeatIntervalMillis": NumberLong("2000"),
  "members": [
    {
      "_id": 0,
      "name": "127.0.0.1:27018",
      "health": 1,
      "state": 1,
      "stateStr": "PRIMARY",
      "uptime": 117,
      "optime": {
        "ts": Timestamp(1453154172, 1),
        "t": NumberLong("2")
      },
      "optimeDate": ISODate("2016-01-18T21:56:12Z"),
      "electionTime": Timestamp(1453154171, 1),
      "electionDate": ISODate("2016-01-18T21:56:11Z"),
      "configVersion": 3,
      "self": true
    },
    {
      "_id": 1,
      "name": "127.0.0.1:27019",
      "health": 1,
      "state": 2,
      "stateStr": "SECONDARY",
      "uptime": 25,
      "optime": {
        "ts": Timestamp(1453154172, 1),
        "t": NumberLong("2")
      },
      "optimeDate": ISODate("2016-01-18T21:56:12Z"),
      "lastHeartbeat": ISODate("2016-01-18T21:56:31.868Z"),
      "lastHeartbeatRecv": ISODate("2016-01-18T21:56:32.192Z"),
      "pingMs": NumberLong("0"),
      "syncingTo": "127.0.0.1:27018",
      "configVersion": 3
    },
    {
      "_id": 2,
      "name": "127.0.0.1:27020",
      "health": 1,
      "state": 2,
      "stateStr": "SECONDARY",
      "uptime": 14,
      "optime": {
        "ts": Timestamp(1453154172, 1),
        "t": NumberLong("2")
      },
      "optimeDate": ISODate("2016-01-18T21:56:12Z"),
      "lastHeartbeat": ISODate("2016-01-18T21:56:31.868Z"),
      "lastHeartbeatRecv": ISODate("2016-01-18T21:56:31.480Z"),
      "pingMs": NumberLong("0"),
      "syncingTo": "127.0.0.1:27019",
      "configVersion": 3
    }
  ],
  "ok": 1
}
```
