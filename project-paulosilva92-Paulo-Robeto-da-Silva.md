# MongoDb - Projeto Final
**Autor:** Paulo Roberto da Silva
**Data** Date.now() //em timestamp


## Para qual sistema você usaria o MogoDB (diferente desse)?

O MongoDb é perfeito pra sistemas que possuem grande processamento de dados não-relacionados entre si. Banco de dados de supermercados onde serão guardadas as notas fiscais ou os preços e dados dos produtos seria uma boa utilização.

## Qual a modelagem da sua coleção de `users`?

```js
user : {
  name: { type : String }
  ,bio: { type : String }
  ,date_register: { type : Date }
  ,avatar_path: { type : String }
  ,auth: {
    username: { type : String }
    ,email: { type : String }
    ,password: { type : String }
    ,last-access: { type : Date }
    ,online: { type: Boolean }
    ,disabled: { type: Boolean }
    ,hash-token: { type : String }
  }
}
```

## Qual a modelagem da sua coleção de `projects`?

```js
project: {
  name: { type : String }
  ,description: { type : String }
  ,date_begin: { type : Date }
  ,date_dream: { type : Date }
  ,date_end: { type : Date }
  ,visible: { type : Boolean }
  ,realocate: { type : Boolean }
  ,expired: { type : Boolean }
  ,visualizable_mod: { type : String }
  ,tags:
    [
      { tag_name: { type : String } }
    ]
  ,goals:
    [
      {
        name: { type : String }
        ,description: { type : String }
        ,date_begin: { type : Date }
        ,date_dream: { type : Date }
        ,date_end: { type : Date }
        ,visible: { type : Boolean }
        ,realocate: { type : Boolean }
        ,expired: { type : Boolean }
        ,tags: [
          { tag_name: { type : String } }
        ],
        ,historic: 
        [
          { date_realocate: { type: Date } }
        ]
      }
      ,activities:
        [
          id_activity : { type : ObjectID}
        ]
    ]
  ,members:
    [
      {
        id_user: { type : ObjectID }
        ,type_member : { type : String }
        ,notify: { type : Boolean }
      }
    ]
}
```

## Qual a modelagem da sua coleção retirada de `projects`?

Foi escolhida a parte de atividades para ser criada uma collection unica por ela ser grande e como um projeto pode ter muitas metas e cada meta muitas atividades, isso pode tornar o documento a ser salvo grande demais para o limite de 16 MB que o MongoDB suporta.

```js
activity: {
  name: { type : String },
  ,description: { type : String }
  ,date_begin: { type : Date }
  ,date_dream: { type : Date }
  ,date_end: { type : Date }
  ,realocate: { type: Boolean }
  ,expired: { type: Boolean }
  ,members_activity :
    [
      {
        id_user : { type : ObjectID }
        ,type_member : { type : String }
        ,notify: { type : Boolean }
      }
    ]
  ,historic:
    [
      { date_realocate: { type : Date } }
    ]
  ,tag:
    [
      { tag_name: { type : String } }
    ]
  ,comment:
    [
      {
        text: { type : String }
        ,date_comment: { type : Date }
        ,member:{
          id_user: { type : ObjectID }
          ,notify: { type : Boolean }
        }

        ,file: {
          path: { type : String }
          ,weight: { type : Number }
          ,name: { type : String }
        }
      }
    ]
}
```

## Create - cadastro

### 1. Cadastre 10 usuários diferentes.

```js
paulo(mongod-3.2.3) be-mean-mongo> function create() {
...   var arr = [];
...   for (var i = 0; i < 10; i++) {
...     arr[i] = {
...         name: 'usuario'+i
...         ,bio: 'some bio'
...         ,date_register: new Date
...         ,avatar_path: 'caminho da imagem'+i
...         ,auth: {
...           username: 'usuario'+i
...           ,email: 'usuario'+i+'@gmail.com'
...           ,password: '123'+i
...           ,last_access: new Date
...           ,online: false
...           ,disabled: false
...           ,hash_token: false
...         }
...       };
...   };
...   db.users.save(arr);
... };
paulo(mongod-3.2.3) be-mean-mongo> create()
Inserted 1 record(s) in 2547ms
paulo(mongod-3.2.3) be-mean-mongo> db.users.find()
{
  "_id": ObjectId("56c65d3e83398baff5bdf9fb"),
  "name": "usuario0",
  "bio": "some bio",
  "date_register": ISODate("2016-02-19T00:09:33.700Z"),
  "avatar_path": "caminho da imagem0",
  "auth": {
    "username": "usuario0",
    "email": "usuario0@gmail.com",
    "password": "1230",
    "last_access": ISODate("2016-02-19T00:09:33.700Z"),
    "online": false,
    "disabled": false,
    "hash_token": false
  }
}
{
  "_id": ObjectId("56c65d3e83398baff5bdf9fc"),
  "name": "usuario1",
  "bio": "some bio",
  "date_register": ISODate("2016-02-19T00:09:33.700Z"),
  "avatar_path": "caminho da imagem1",
  "auth": {
    "username": "usuario1",
    "email": "usuario1@gmail.com",
    "password": "1231",
    "last_access": ISODate("2016-02-19T00:09:33.700Z"),
    "online": false,
    "disabled": false,
    "hash_token": false
  }
}
{
  "_id": ObjectId("56c65d3e83398baff5bdf9fd"),
  "name": "usuario2",
  "bio": "some bio",
  "date_register": ISODate("2016-02-19T00:09:33.700Z"),
  "avatar_path": "caminho da imagem2",
  "auth": {
    "username": "usuario2",
    "email": "usuario2@gmail.com",
    "password": "1232",
    "last_access": ISODate("2016-02-19T00:09:33.700Z"),
    "online": false,
    "disabled": false,
    "hash_token": false
  }
}
{
  "_id": ObjectId("56c65d3e83398baff5bdf9fe"),
  "name": "usuario3",
  "bio": "some bio",
  "date_register": ISODate("2016-02-19T00:09:33.700Z"),
  "avatar_path": "caminho da imagem3",
  "auth": {
    "username": "usuario3",
    "email": "usuario3@gmail.com",
    "password": "1233",
    "last_access": ISODate("2016-02-19T00:09:33.700Z"),
    "online": false,
    "disabled": false,
    "hash_token": false
  }
}
{
  "_id": ObjectId("56c65d3e83398baff5bdf9ff"),
  "name": "usuario4",
  "bio": "some bio",
  "date_register": ISODate("2016-02-19T00:09:33.700Z"),
  "avatar_path": "caminho da imagem4",
  "auth": {
    "username": "usuario4",
    "email": "usuario4@gmail.com",
    "password": "1234",
    "last_access": ISODate("2016-02-19T00:09:33.700Z"),
    "online": false,
    "disabled": false,
    "hash_token": false
  }
}
{
  "_id": ObjectId("56c65d3e83398baff5bdfa00"),
  "name": "usuario5",
  "bio": "some bio",
  "date_register": ISODate("2016-02-19T00:09:33.700Z"),
  "avatar_path": "caminho da imagem5",
  "auth": {
    "username": "usuario5",
    "email": "usuario5@gmail.com",
    "password": "1235",
    "last_access": ISODate("2016-02-19T00:09:33.700Z"),
    "online": false,
    "disabled": false,
    "hash_token": false
  }
}
{
  "_id": ObjectId("56c65d3e83398baff5bdfa01"),
  "name": "usuario6",
  "bio": "some bio",
  "date_register": ISODate("2016-02-19T00:09:33.700Z"),
  "avatar_path": "caminho da imagem6",
  "auth": {
    "username": "usuario6",
    "email": "usuario6@gmail.com",
    "password": "1236",
    "last_access": ISODate("2016-02-19T00:09:33.700Z"),
    "online": false,
    "disabled": false,
    "hash_token": false
  }
}
{
  "_id": ObjectId("56c65d3e83398baff5bdfa02"),
  "name": "usuario7",
  "bio": "some bio",
  "date_register": ISODate("2016-02-19T00:09:33.700Z"),
  "avatar_path": "caminho da imagem7",
  "auth": {
    "username": "usuario7",
    "email": "usuario7@gmail.com",
    "password": "1237",
    "last_access": ISODate("2016-02-19T00:09:33.700Z"),
    "online": false,
    "disabled": false,
    "hash_token": false
  }
}
{
  "_id": ObjectId("56c65d3e83398baff5bdfa03"),
  "name": "usuario8",
  "bio": "some bio",
  "date_register": ISODate("2016-02-19T00:09:33.700Z"),
  "avatar_path": "caminho da imagem8",
  "auth": {
    "username": "usuario8",
    "email": "usuario8@gmail.com",
    "password": "1238",
    "last_access": ISODate("2016-02-19T00:09:33.700Z"),
    "online": false,
    "disabled": false,
    "hash_token": false
  }
}
{
  "_id": ObjectId("56c65d3e83398baff5bdfa04"),
  "name": "usuario9",
  "bio": "some bio",
  "date_register": ISODate("2016-02-19T00:09:33.717Z"),
  "avatar_path": "caminho da imagem9",
  "auth": {
    "username": "usuario9",
    "email": "usuario9@gmail.com",
    "password": "1239",
    "last_access": ISODate("2016-02-19T00:09:33.717Z"),
    "online": false,
    "disabled": false,
    "hash_token": false
  }
}
Fetched 10 record(s) in 789ms
```

### 2. Cadastre 5 projetos diferentes.

```js
paulo-sti-ni-1401(mongod-3.2.3) be-mean-mongo> var projects =
... [
...   {
...     name: 'projeto 1'
...     ,description: 'descrição do projeto numero 1'
...     ,date_begin: new Date
...     ,date_dream: new Date
...     ,date_end: new Date
...     ,visible: false
...     ,realocate: false
...     ,expired: false
...     ,visualizable_mod: 'yes'
...     ,tags:
...       [
...         'maneiro'
...         ,'bacana'
...         ,'supimpa'
...       ]
...     ,goals:
...       [
...         {
...           name: 'meta 1'
...           ,description: 'descricao da meta 1'
...           ,date_begin: new Date
...           ,date_dream: new Date
...           ,date_end: new Date
...           ,visible: true
...           ,realocate: false
...           ,expired: false
...           ,tags:
...             [
...               'rapidez'
...               ,'agilidade'
...               ,'facil'
...             ]
...           ,historic:
...             [
...               { date_realocate:  new Date}
...             ]
...           ,activities:
...             [
...               {
...                 "_id": ObjectId("56cc0ab7694907cadd043404")
...               }
...               ,{
...                 "_id": ObjectId("56cc0ab7694907cadd043405")
...               }
... 
...             ]
...         }
...       ]
...     ,members:
...       [
...         {
...           id_user: ObjectId("56c65d3e83398baff5bdfa00")
...           ,type_member : 'senior'
...           ,notify: true
...         }
...         ,{
...           id_user: ObjectId("56c65d3e83398baff5bdfa01")
...           ,type_member : 'senior'
...           ,notify: true
...         }
...         ,{
...           id_user: ObjectId("56c65d3e83398baff5bdfa02")
...           ,type_member : 'senior'
...           ,notify: true
...         }
...         ,{
...           id_user: ObjectId("56c65d3e83398baff5bdfa04")
...           ,type_member : 'senior'
...           ,notify: true
...         }
...         ,{
...           id_user: ObjectId("56c65d3e83398baff5bdf9ff")
...           ,type_member : 'senior'
...           ,notify: true
            }
...       ]
...   }
...   ,{
...     name: 'projeto 2'
...     ,description: 'descrição do projeto numero 2'
...     ,date_begin: new Date
...     ,date_dream: new Date
...     ,date_end: new Date
...     ,visible: false
...     ,realocate: false
...     ,expired: false
...     ,visualizable_mod: 'yes'
...     ,tags:
...       [
...         'maneiro'
...         ,'bacana'
...         ,'estupendo'
...       ]
...     ,goals:
...       [
...         {
...           name: 'meta 1'
...           ,description: 'descricao da meta 1'
...           ,date_begin: new Date
...           ,date_dream: new Date
...           ,date_end: new Date
...           ,visible: true
...           ,realocate: false
...           ,expired: false
...           ,tags:
...             [
...               'rapidez'
...               ,'agilidade'
   .              ,'facil'
...             ]
...           ,historic:
...             [
...               { date_realocate:  new Date}
...             ]
...           ,activities:
...             [
...               {
...                 "_id": ObjectId("56cc0ab7694907cadd043404")
...               }
...               ,{
...                 "_id": ObjectId("56cc0ab7694907cadd043405")
...               }
...             ]
...         }
...       ]
...     ,members:
 tru      [
...         {
...           id_user: ObjectId("56c65d3e83398baff5bdfa00")
...           ,type_member : 'senior'
...           ,notify: true
...         }
...         ,{
...           id_user: ObjectId("56c65d3e83398baff5bdfa01")
...           ,type_member : 'senior'
...           ,notify: true
...         }
...         ,{
...           id_user: ObjectId("56c65d3e83398baff5bdfa02")
...           ,type_member : 'senior'
...           ,notify: true
aliz        }
...         ,{
...           id_user: ObjectId("56c65d3e83398baff5bdfa04")
...           ,type_member : 'senior'
...           ,notify: true
...         }
...         ,{
...           id_user: ObjectId("56c65d3e83398baff5bdf9ff")
...           ,type_member : 'senior'
...           ,notify: true
...         }
...       ]
...   }
...   ,{
...     name: 'projeto 3'
...     ,description: 'descrição do projeto numero 3'
...     ,date_begin: new Date
...     ,date_dream: new Date
...     ,date_end: new Date
...     ,visible: false
...     ,realocate: false
...     ,expired: false
...     ,visualizable_mod: 'yes'
...     ,tags:
...       [
...         'formoso'
...         ,'bacana'
...         ,'caralhudo'
...       ]
...     ,goals:
...       [
...         {
...           name: 'meta 1'
...           ,description: 'descricao da meta 1'
...           ,date_begin: new Date
...           ,date_dream: new Date
...           ,date_end: new Date
...           ,visible: true
...           ,realocate: false
...           ,expired: false
...           ,tags:
...             [
...               'rapidez'
...               ,'agilidade'
...               ,'facil'
...             ]
...           ,historic:
...             [
...               { date_realocate:  new Date}
...             ]
...           ,activities:
...             [
...               {
...                 "_id": ObjectId("56cc0ab7694907cadd043404")
...               }
...               ,{
...                 "_id": ObjectId("56cc0ab7694907cadd043405")
...               }
...             ]
...         }
...       ]
...     ,members:
...       [
...         {
...           id_user: ObjectId("56c65d3e83398baff5bdf9fb")
...           ,type_member : 'senior'
...           ,notify: true
...         }
...         ,{
...           id_user: ObjectId("56c65d3e83398baff5bdf9fd")
...           ,type_member : 'senior'
...           ,notify: true
...         }
...         ,{
...           id_user: ObjectId("56c65d3e83398baff5bdf9ff")
...           ,type_member : 'senior'
...           ,notify: true
...         }
...         ,{
...           id_user: ObjectId("56c65d3e83398baff5bdfa01")
...           ,type_member : 'senior'
...           ,notify: true
...         }
...         ,{
...           id_user: ObjectId("56c65d3e83398baff5bdfa03")
...           ,type_member : 'senior'
...           ,notify: true
...         }
...       ]
...   }
...   ,{
...     name: 'projeto 4'
...     ,description: 'descrição do projeto numero 4'
...     ,date_begin: new Date
...     ,date_dream: new Date
...     ,date_end: new Date
...     ,visible: false
...     ,realocate: false
...     ,expired: false
...     ,visualizable_mod: 'yes'
...     ,tags:
...       [
...         'gostoso'
...         ,'espetacular'
...         ,'incrivel'
...       ]
...     ,goals:
...       [
...         {
...           name: 'meta 1'
...           ,description: 'descricao da meta 1'
...           ,date_begin: new Date
...           ,date_dream: new Date
...           ,date_end: new Date
...           ,visible: true
...           ,realocate: false
...           ,expired: false
...           ,tags:
...             [
...               'rapidez'
...               ,'agilidade'
...               ,'facil'
...             ]
...           ,historic:
...             [
...               { date_realocate:  new Date}
...             ]
...           ,activities:
...             [
...               {
...                 "_id": ObjectId("56cc0ab7694907cadd043404")
...               }
...               ,{
...                 "_id": ObjectId("56cc0ab7694907cadd043405")
...               }
...             ]
...         }
...       ]
...     ,members:
...       [
...         {
...           id_user: ObjectId("56c65d3e83398baff5bdfa04")
...           ,type_member : 'senior'
...           ,notify: true
...         }
...         ,{
...           id_user: ObjectId("56c65d3e83398baff5bdfa03")
...           ,type_member : 'senior'
...           ,notify: true
...         }
...         ,{
...           id_user:  ObjectId("56c65d3e83398baff5bdfa01")
...           ,type_member : 'senior'
...           ,notify: true
...         }
...         ,{
...           id_user: ObjectId("56c65d3e83398baff5bdf9ff")
...           ,type_member : 'senior'
...           ,notify: true
...         }
...         ,{
...           id_user: ObjectId("56c65d3e83398baff5bdf9fe")
...           ,type_member : 'senior'
...           ,notify: true
...         }
...       ]
...   }
...   ,{
...     name: 'projeto 5'
...     ,description: 'descrição do projeto numero 5'
...     ,date_begin: new Date
...     ,date_dream: new Date
...     ,date_end: new Date
...     ,visible: false
...     ,realocate: false
...     ,expired: false
...     ,visualizable_mod: 'yes'
...     ,tags:
...       [
...         'sensacional'
...         ,'amazing'
...         ,'spiderman'
...       ]
...     ,goals:
...       [
...         {
...           name: 'meta 1'
...           ,description: 'descricao da meta 1'
...           ,date_begin: new Date
...           ,date_dream: new Date
...           ,date_end: new Date
...           ,visible: true
...           ,realocate: false
...           ,expired: false
...           ,tags:
...             [
...               'rapidez'
...               ,'agilidade'
...               ,'facil'
...             ]
...           ,historic:
...             [
...               { date_realocate:  new Date}
...             ]
...           ,activities:
...             [ ]
...         }
...       ]
...     ,members:
...       [
...         {
...           id_user: ObjectId("56c65d3e83398baff5bdf9fc")
...           ,type_member : 'senior'
...           ,notify: true
...         }
...         ,{
...           id_user: ObjectId("56c65d3e83398baff5bdf9fd")
...           ,type_member : 'senior'
...           ,notify: true
...         }
...         ,{
...           id_user:  ObjectId("56c65d3e83398baff5bdf9ff")
...           ,type_member : 'senior'
...           ,notify: true
...         }
...         ,{
...           id_user: ObjectId("56c65d3e83398baff5bdfa01")
...           ,type_member : 'senior'
...           ,notify: true
...         }
...         ,{
...           id_user: ObjectId("56c65d3e83398baff5bdfa02")
...           ,type_member : 'senior'
...           ,notify: true
...         }
...       ]
...   }
... ];
paulo-sti-ni-1401(mongod-3.2.3) be-mean-mongo> db.projects.insert(projects)
Inserted 1 record(s) in 196ms
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

### 1. Liste as informações dos membros de 1 projeto especifico que deve ser buscado pelo seu nome de forma a não ligar para maiúsculas e minúsculas

```js
paulo-sti-ni-1401(mongod-3.2.3) be-mean-mongo>  var query  = { name: /projeto 4/i };
paulo-sti-ni-1401(mongod-3.2.3) be-mean-mongo> var fields = { members: 1, _id: 0 };
paulo-sti-ni-1401(mongod-3.2.3) be-mean-mongo> db.projects.find(query, fields)
{
  "members": [
    {
      "id_user": ObjectId("56c65d3e83398baff5bdfa04"),
      "type_member": "senior",
      "notify": true
    },
    {
      "id_user": ObjectId("56c65d3e83398baff5bdfa03"),
      "type_member": "senior",
      "notify": true
    },
    {
      "id_user": ObjectId("56c65d3e83398baff5bdfa01"),
      "type_member": "senior",
      "notify": true
    },
    {
      "id_user": ObjectId("56c65d3e83398baff5bdf9ff"),
      "type_member": "senior",
      "notify": true
    },
    {
      "id_user": ObjectId("56c65d3e83398baff5bdf9fe"),
      "type_member": "senior",
      "notify": true
    }
  ]
}
Fetched 1 record(s) in 4ms
```

### 2. Liste todos os projetos com a tag que voce escolheu para os 3 projetos em comum

```js
paulo-sti-ni-1401(mongod-3.2.3) be-mean-mongo> var query = { tags: 'bacana' };
paulo-sti-ni-1401(mongod-3.2.3) be-mean-mongo> db.projects.find(query)
{
  "_id": ObjectId("56cc0b6783bfa35d1a884d03"),
  "name": "projeto 1",
  "description": "descrição do projeto numero 1",
  "date_begin": ISODate("2016-02-23T07:33:42.624Z"),
  "date_dream": ISODate("2016-02-23T07:33:42.624Z"),
  "date_end": ISODate("2016-02-23T07:33:42.624Z"),
  "visible": false,
  "realocate": false,
  "expired": false,
  "visualizable_mod": "yes",
  "tags": [
    "maneiro",
    "bacana",
    "supimpa"
  ],
  "goals": [
    {
      "name": "meta 1",
      "description": "descricao da meta 1",
      "date_begin": ISODate("2016-02-23T07:33:42.624Z"),
      "date_dream": ISODate("2016-02-23T07:33:42.624Z"),
      "date_end": ISODate("2016-02-23T07:33:42.624Z"),
      "visible": true,
      "realocate": false,
      "expired": false,
      "tags": [
        "rapidez",
        "agilidade",
        "facil"
      ],
      "historic": [
        {
          "date_realocate": ISODate("2016-02-23T07:33:42.624Z")
        }
      ],
      "activities": [
        {
          "_id": ObjectId("56cc0ab7694907cadd043404")
        },
        {
          "_id": ObjectId("56cc0ab7694907cadd043405")
        }
      ]
    }
  ],
  "members": [
    {
      "id_user": ObjectId("56c65d3e83398baff5bdfa00"),
      "type_member": "senior",
      "notify": true
    },
    {
      "id_user": ObjectId("56c65d3e83398baff5bdfa01"),
      "type_member": "senior",
      "notify": true
    },
    {
      "id_user": ObjectId("56c65d3e83398baff5bdfa02"),
      "type_member": "senior",
      "notify": true
    },
    {
      "id_user": ObjectId("56c65d3e83398baff5bdfa04"),
      "type_member": "senior",
      "notify": true
    },
    {
      "id_user": ObjectId("56c65d3e83398baff5bdf9ff"),
      "type_member": "senior",
      "notify": true
    }
  ]
}
{
  "_id": ObjectId("56cc0b6783bfa35d1a884d04"),
  "name": "projeto 2",
  "description": "descrição do projeto numero 2",
  "date_begin": ISODate("2016-02-23T07:33:42.624Z"),
  "date_dream": ISODate("2016-02-23T07:33:42.624Z"),
  "date_end": ISODate("2016-02-23T07:33:42.624Z"),
  "visible": false,
  "realocate": false,
  "expired": false,
  "visualizable_mod": "yes",
  "tags": [
    "maneiro",
    "bacana",
    "estupendo"
  ],
  "goals": [
    {
      "name": "meta 1",
      "description": "descricao da meta 1",
      "date_begin": ISODate("2016-02-23T07:33:42.624Z"),
      "date_dream": ISODate("2016-02-23T07:33:42.624Z"),
      "date_end": ISODate("2016-02-23T07:33:42.624Z"),
      "visible": true,
      "realocate": false,
      "expired": false,
      "tags": [
        "rapidez",
        "agilidade",
        "facil"
      ],
      "historic": [
        {
          "date_realocate": ISODate("2016-02-23T07:33:42.624Z")
        }
      ],
      "activities": [
        {
          "_id": ObjectId("56cc0ab7694907cadd043404")
        },
        {
          "_id": ObjectId("56cc0ab7694907cadd043405")
        }
      ]
    }
  ],
  "members": [
    {
      "id_user": ObjectId("56c65d3e83398baff5bdfa00"),
      "type_member": "senior",
      "notify": true
    },
    {
      "id_user": ObjectId("56c65d3e83398baff5bdfa01"),
      "type_member": "senior",
      "notify": true
    },
    {
      "id_user": ObjectId("56c65d3e83398baff5bdfa02"),
      "type_member": "senior",
      "notify": true
    },
    {
      "id_user": ObjectId("56c65d3e83398baff5bdfa04"),
      "type_member": "senior",
      "notify": true
    },
    {
      "id_user": ObjectId("56c65d3e83398baff5bdf9ff"),
      "type_member": "senior",
      "notify": true
    }
  ]
}
{
  "_id": ObjectId("56cc0b6783bfa35d1a884d05"),
  "name": "projeto 3",
  "description": "descrição do projeto numero 3",
  "date_begin": ISODate("2016-02-23T07:33:42.624Z"),
  "date_dream": ISODate("2016-02-23T07:33:42.624Z"),
  "date_end": ISODate("2016-02-23T07:33:42.624Z"),
  "visible": false,
  "realocate": false,
  "expired": false,
  "visualizable_mod": "yes",
  "tags": [
    "formoso",
    "bacana",
    "caralhudo"
  ],
  "goals": [
    {
      "name": "meta 1",
      "description": "descricao da meta 1",
      "date_begin": ISODate("2016-02-23T07:33:42.624Z"),
      "date_dream": ISODate("2016-02-23T07:33:42.624Z"),
      "date_end": ISODate("2016-02-23T07:33:42.624Z"),
      "visible": true,
      "realocate": false,
      "expired": false,
      "tags": [
        "rapidez",
        "agilidade",
        "facil"
      ],
      "historic": [
        {
          "date_realocate": ISODate("2016-02-23T07:33:42.624Z")
        }
      ],
      "activities": [
        {
          "_id": ObjectId("56cc0ab7694907cadd043404")
        },
        {
          "_id": ObjectId("56cc0ab7694907cadd043405")
        }
      ]
    }
  ],
  "members": [
    {
      "id_user": ObjectId("56c65d3e83398baff5bdf9fb"),
      "type_member": "senior",
      "notify": true
    },
    {
      "id_user": ObjectId("56c65d3e83398baff5bdf9fd"),
      "type_member": "senior",
      "notify": true
    },
    {
      "id_user": ObjectId("56c65d3e83398baff5bdf9ff"),
      "type_member": "senior",
      "notify": true
    },
    {
      "id_user": ObjectId("56c65d3e83398baff5bdfa01"),
      "type_member": "senior",
      "notify": true
    },
    {
      "id_user": ObjectId("56c65d3e83398baff5bdfa03"),
      "type_member": "senior",
      "notify": true
    }
  ]
}
Fetched 3 record(s) in 7ms
```

### 3. Liste apenas os nomes de todas as atividades para todos os projeto

```js
paulo-sti-ni-1401(mongod-3.2.3) be-mean-mongo> var query = {}
paulo-sti-ni-1401(mongod-3.2.3) be-mean-mongo> var fields = { name : 1}
paulo-sti-ni-1401(mongod-3.2.3) be-mean-mongo> db.activities.find(query, fields)
{
  "_id": ObjectId("56cc0ab7694907cadd043404"),
  "name": "atividade 1"
}
{
  "_id": ObjectId("56cc0ab7694907cadd043405"),
  "name": "atividade 2"
}
Fetched 2 record(s) in 3ms
```

### 4. Liste todos os projetos que não possuam uma tag.

```js
paulo-sti-ni-1401(mongod-3.2.3) be-mean-mongo> var query = { tags : [] }
paulo-sti-ni-1401(mongod-3.2.3) be-mean-mongo> db.projects.find(query)
Fetched 0 record(s) in 1ms
```

### 5. Liste todos os usuários que não fazem parte do primeiro projeto cadastrado.

```js
paulo-sti-ni-1401(mongod-3.2.3) be-mean-mongo> var members = [];
paulo-sti-ni-1401(mongod-3.2.3) be-mean-mongo> function getMembers(member) {
...   members.push(member.id_user);
... };
paulo-sti-ni-1401(mongod-3.2.3) be-mean-mongo> var query = {}
paulo-sti-ni-1401(mongod-3.2.3) be-mean-mongo> db.projects.findOne(query).members.forEach(getMembers);
paulo-sti-ni-1401(mongod-3.2.3) be-mean-mongo> db.users.find( { _id : { $not : { $in : members } } },{ name: 1, _id : 0 } );
{
  "name": "usuario0"
}
{
  "name": "usuario1"
}
{
  "name": "usuario2"
}
{
  "name": "usuario3"
}
{
  "name": "usuario8"
}
Fetched 5 record(s) in 4ms
```

## Update - alteração

### 1. Adicione para todos os projetos o campo views: 0.

```js
paulo-sti-ni-1401(mongod-3.2.3) be-mean-mongo> var query = {};
paulo-sti-ni-1401(mongod-3.2.3) be-mean-mongo> var mod = {$set : { views : 0}};
paulo-sti-ni-1401(mongod-3.2.3) be-mean-mongo> var options = { multi : true};
paulo-sti-ni-1401(mongod-3.2.3) be-mean-mongo> db.projects.update(query, mod , options);
Updated 5 existing record(s) in 3ms
WriteResult({
  "nMatched": 5,
  "nUpserted": 0,
  "nModified": 5
})
```

### 2. Adicione 1 tag diferente para cada projeto.

```js
paulo-sti-ni-1401(mongod-3.2.3) be-mean-mongo> function updateTag() {
...   var tag = ['bom', 'ótimo', 'supimposo', 'maravilhosa', 'formogostoso'];
...   var i = 0;
...   function upt(proj){
...     var query = proj;
...     var mod = { $push : {tags : tag[i] } }
...     db.projects.update(query,mod);
...     i++;
...   }
...   var query = {};
...   db.projects.find(query).forEach(upt);
... }
paulo-sti-ni-1401(mongod-3.2.3) be-mean-mongo> updateTag()
Updated 1 existing record(s) in 2ms
Updated 1 existing record(s) in 2ms
Updated 1 existing record(s) in 1ms
Updated 1 existing record(s) in 3ms
Updated 1 existing record(s) in 2ms
```

### 3. Adicione 2 membros diferentes para cada projeto.

```js
paulo-sti-ni-1401(mongod-3.2.3) be-mean-mongo> function updt(proj){
...   var members = [];
...   function getMembers(member) {
...     members.push(member.id_user);
...   };
...   var query = {}
...   proj.members.forEach(getMembers);
...   var notIn = [];
...   function getNotIn(user){
...     notIn.push(user._id);
...   };
...   db.users.find( { _id : { $not : { $in : members } } } ).forEach(getNotIn);
...   function notInUp(elemento){
...     var user = {id_user : elemento, notify: true, type_member: "senior" }
...     var mod = { $push : { members : user } };
...     db.projects.update({name: proj.name},mod);
...   };
...   notIn.splice(0,2).forEach(notInUp);
... }
paulo-sti-ni-1401(mongod-3.2.3) be-mean-mongo> db.projects.find({}).forEach(updt);
Updated 1 existing record(s) in 2ms
Updated 1 existing record(s) in 1ms
Updated 1 existing record(s) in 1ms
Updated 1 existing record(s) in 1ms
Updated 1 existing record(s) in 2ms
Updated 1 existing record(s) in 3ms
Updated 1 existing record(s) in 2ms
Updated 1 existing record(s) in 1ms
Updated 1 existing record(s) in 1ms
Updated 1 existing record(s) in 1ms
```

### 4. Adicione 1 comentário em cada atividade, deixe apenas 1 projeto sem.

```js
paulo-sti-ni-1401(mongod-3.2.3) be-mean-mongo> comments =
...       {
...         text: "outro comentario muito loco"
...         ,date_comment:new Date
...         ,member:{
...           id_user: ObjectId("56cb8e797cb0fd72a7c50edc")
...           ,notify: true
...         }
... 
...         ,file: {
...           path: ""
...           ,weight: 0
...           ,name: ""
...         }
...       };
{
  "text": "outro comentario muito loco",
  "date_comment": ISODate("2016-02-23T08:03:03.612Z"),
  "member": {
    "id_user": ObjectId("56cb8e797cb0fd72a7c50edc"),
    "notify": true
  },
  "file": {
    "path": "",
    "weight": 0,
    "name": ""
  }
}
paulo-sti-ni-1401(mongod-3.2.3) be-mean-mongo> var query = {};
paulo-sti-ni-1401(mongod-3.2.3) be-mean-mongo> var mod = { $push : { comment : comments } };
paulo-sti-ni-1401(mongod-3.2.3) be-mean-mongo> var opt = { multi : true }
paulo-sti-ni-1401(mongod-3.2.3) be-mean-mongo> db.activities.update(query, mod, opt)
Updated 2 existing record(s) in 2ms
WriteResult({
  "nMatched": 2,
  "nUpserted": 0,
  "nModified": 2
})
```

### 5. Adicione 1 projeto inteiro com UPSERT.

```js
paulo-sti-ni-1401(mongod-3.2.3) be-mean-mongo> var query = { name : /projeto setOnInsert/i};
paulo-sti-ni-1401(mongod-3.2.3) be-mean-mongo> var mod = { 
...   $set : { realocate : true}
...   , $setOnInsert : {
...     name: 'projeto setOnInsert'
...     ,description: 'descrição do projeto setOnInsert'
...     ,date_begin: new Date
...     ,date_dream: new Date
...     ,date_end: new Date
...     ,visible: false
...     ,expired: false
...     ,visualizable_mod: 'yes'
...     ,tags:
...       [
...         'gostoso'
...         ,'espetacular'
...         ,'incrivel'
...       ]
...     ,goals:
...       [
...         {
...           name: 'meta 1'
...           ,description: 'descricao da meta 1'
...           ,date_begin: new Date
...           ,date_dream: new Date
...           ,date_end: new Date
...           ,visible: true
...           ,realocate: false
...           ,expired: false
...           ,tags:
...             [
...               'rapidez'
...               ,'agilidade'
...               ,'facil'
...             ]
...           ,historic:
...             [
...               { date_realocate:  new Date}
...             ]
...           ,activities:
...             [
...              
...             ]
...         }
...       ]
...     ,members:
...       [
...         
...       ]
...   }
...  }
paulo-sti-ni-1401(mongod-3.2.3) be-mean-mongo> var opt = { upsert : true};
paulo-sti-ni-1401(mongod-3.2.3) be-mean-mongo> db.projects.update(query,mod, opt);
Updated 1 new record(s) in 2ms
WriteResult({
  "nMatched": 0,
  "nUpserted": 1,
  "nModified": 0,
  "_id": ObjectId("56cc1cb879c4e7a3cefd119d")
})
```

## Delete - remoção

### 1. Apague todos os projetos que não possuam tags.

```js
paulo-sti-ni-1401(mongod-3.2.3) be-mean-mongo> var query = { tags : [] };
paulo-sti-ni-1401(mongod-3.2.3) be-mean-mongo> db.projects.remove(query);
Removed 0 record(s) in 16ms
WriteResult({
  "nRemoved": 0
})
```

### 2. Apague todos os projetos que não possuam comentários nas atividades.

```js
paulo-sti-ni-1401(mongod-3.2.3) be-mean-mongo> var query = { comment : { $size : 0 } }; 
paulo-sti-ni-1401(mongod-3.2.3) be-mean-mongo> var activities = db.activities.find(query , { _id :1  }).toArray();
paulo-sti-ni-1401(mongod-3.2.3) be-mean-mongo> var query2 = {'goals.activities._id' : { $in : activities } }
paulo-sti-ni-1401(mongod-3.2.3) be-mean-mongo> var opt = { multi : true }
paulo-sti-ni-1401(mongod-3.2.3) be-mean-mongo> db.projects.remove(query2 , opt );
Removed 0 record(s) in 1ms
WriteResult({
  "nRemoved": 0
})
```

### 3. Apague todos os projetos que não possuam atividades.

```js
paulo-sti-ni-1401(mongod-3.2.3) be-mean-mongo> var goals = [];
paulo-sti-ni-1401(mongod-3.2.3) be-mean-mongo> function remove(){
...   function getProj(proj){
...     function getGoal(goal){
...       goals.push(goal.activities)
...       if (goal.activities.length === 0){
...         db.projects.remove({name: proj.name});
...       }
...     };
...     db.projects.findOne({name: proj.name}).goals.forEach(getGoal) 
...   }
...   db.projects.find({}).forEach(getProj)
... }
paulo-sti-ni-1401(mongod-3.2.3) be-mean-mongo> remove()
Removed 1 record(s) in 1ms
Removed 1 record(s) in 1ms
```

### 4. Escolha 2 usuário e apague todos os projetos em que os 2 fazem parte.

```js
paulo-sti-ni-1401(mongod-3.2.3) be-mean-mongo> function remove(){ 
...   var users = db.users.find({},{ _id: 1 }).skip(3).limit(2)
...   var result = db.projects.aggregate (
...     [
...       {
...         $match : {
...           $or : [
...               { 'members.id_user' : users[0]._id }
...               ,{ 'members.id_user' : users[1]._id }
...           ]
...         }
...       }
...     ]
...   ).result[0].projects;
...   for( i = 0 ; i < result.length; i++){
...     db.projects.remove(result[i]);
...   };  
... }
paulo-sti-ni-1401(mongod-3.2.3) be-mean-mongo> remove()
Removed 1 record(s) in 328ms
Removed 0 record(s) in 1ms
Removed 1 record(s) in 79ms
Removed 0 record(s) in 2ms
Removed 1 record(s) in 1ms
Removed 0 record(s) in 1ms
Removed 1 record(s) in 2ms
Removed 0 record(s) in 0ms
```

### 5. Apague todos os projetos que possuam uma determinada tag em goal.

```js
paulo-sti-ni-1401(mongod-3.2.3) be-mean-mongo> var resultado = db.projects.aggregate(
...   [
...     { $match : { 'goals.tags' : /rapidez/i } },
...     { $unwind :  '$goals' },
...     {
...       $group : {
...         _id : null,
...         projects : {
...           $push : '$_id'
...         }
...       }
...     }
...   ]
... )
paulo-sti-ni-1401(mongod-3.2.3) be-mean-mongo> db.projects.remove({ _id : { $in : resultado.result[0].projects } })
Removed 1 record(s) in 25ms
WriteResult({
  "nRemoved": 1
})
```

## Gerenciamento de usuários

### 1. Crie um usuário com permissões APENAS de Leitura.

```js
paulo-sti-ni-1401(mongod-3.2.3) be-mean-mongo> var user = { createUser: "Paulo",
...   pwd: "123",
...   roles: [
...     { role: "read", db: "be-mean-mongo" }
...   ]
... }
paulo-sti-ni-1401(mongod-3.2.3) be-mean-mongo> db.createUser(user);
Successfully added user: {
  "createUser": "Paulo",
  "roles": [
    {
      "role": "read",
      "db": "be-mean-mongo"
    }
  ]
}
```

### 2. Crie um usuário com permissões de Escrita e Leitura.

```js
paulo-sti-ni-1401(mongod-3.2.3) be-mean-mongo> var user = { createUser: "PauloLeituraEscrita",
...   pwd: "123",
...   roles: [
...     { role: "readWrite", db: "be-mean-mongo" }
...   ]
... }
paulo-sti-ni-1401(mongod-3.2.3) be-mean-mongo> db.createUser(user);
Successfully added user: {
  "createUser": "PauloLeituraEscrita",
  "roles": [
    {
      "role": "readWrite",
      "db": "be-mean-mongo"
    }
  ]
}
```

### 3. Adicionar o papel grantRolesToUser e revokeRole para o usuário com Escrita e Leitura.

```js
paulo-sti-ni-1401(mongod-3.2.3) be-mean-mongo> db.runCommand({
...   createRole: "grantRolesToUser",
...   privileges : [
...     { resource : { db : "be-mean-mongo", collection : "" }, actions : [ "grantRolesToUser" ]}
...   ],
...   roles : []
...  })
{
  "ok": 1
}
paulo-sti-ni-1401(mongod-3.2.3) be-mean-mongo> db.runCommand({
...   createRole: "revokeRole",
...   privileges : [
...     { resource : { db : "be-mean-mongo", collection : "" }, actions : [ "revokeRole" ]}
...   ],
...   roles : []
...  })
{
  "ok": 1
}
paulo-sti-ni-1401(mongod-3.2.3) be-mean-mongo> db.runCommand({ grantRolesToUser : "PauloLeituraEscrita",
...       roles : [ "grantRolesToUser", "revokeRole"]
... });
{
  "ok": 1
}
paulo-sti-ni-1401(mongod-3.2.3) be-mean-mongo> db.runCommand({ usersInfo: { user: "PauloLeituraEscrita", db: "be-mean-mongo" }})
{
  "users": [
    {
      "_id": "be-mean-mongo.PauloLeituraEscrita",
      "user": "PauloLeituraEscrita",
      "db": "be-mean-mongo",
      "roles": [
        {
          "role": "revokeRole",
          "db": "be-mean-mongo"
        },
        {
          "role": "grantRolesToUser",
          "db": "be-mean-mongo"
        },
        {
          "role": "readWrite",
          "db": "be-mean-mongo"
        }
      ]
    }
  ],
  "ok": 1
}
```

### 4. Remover o papel grantRolesToUser para o usuário com Escrita e Leitura.

```js
paulo-sti-ni-1401(mongod-3.2.3) be-mean-mongo> db.runCommand( { revokeRolesFromUser: "PauloLeituraEscrita",
...   roles: [
...           { role: "revokeRole", db: "be-mean-mongo" },
...   ]
...  })
{
  "ok": 1
}
paulo-sti-ni-1401(mongod-3.2.3) be-mean-mongo> db.runCommand({ usersInfo: { user: "PauloLeituraEscrita", db: "be-mean-mongo" }})
{
  "users": [
    {
      "_id": "be-mean-mongo.PauloLeituraEscrita",
      "user": "PauloLeituraEscrita",
      "db": "be-mean-mongo",
      "roles": [
        {
          "role": "grantRolesToUser",
          "db": "be-mean-mongo"
        },
        {
          "role": "readWrite",
          "db": "be-mean-mongo"
        }
      ]
    }
  ],
  "ok": 1
}
```

### 5. Listar todos os usuários com seus papéis e ações.

```js
paulo-sti-ni-1401(mongod-3.2.3) be-mean-mongo> db.runCommand( { usersInfo: ["Paulo", "PauloLeituraEscrita"], showPrivileges: true })
{
  "users": [
    {
      "_id": "be-mean-mongo.Paulo",
      "user": "Paulo",
      "db": "be-mean-mongo",
      "roles": [
        {
          "role": "read",
          "db": "be-mean-mongo"
        }
      ],
      "inheritedRoles": [
        {
          "role": "read",
          "db": "be-mean-mongo"
        }
      ],
      "inheritedPrivileges": [
        {
          "resource": {
            "db": "be-mean-mongo",
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
            "db": "be-mean-mongo",
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
            "db": "be-mean-mongo",
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
            "db": "be-mean-mongo",
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
      "_id": "be-mean-mongo.PauloLeituraEscrita",
      "user": "PauloLeituraEscrita",
      "db": "be-mean-mongo",
      "roles": [
        {
          "role": "grantRolesToUser",
          "db": "be-mean-mongo"
        },
        {
          "role": "readWrite",
          "db": "be-mean-mongo"
        }
      ],
      "inheritedRoles": [
        {
          "role": "readWrite",
          "db": "be-mean-mongo"
        },
        {
          "role": "grantRolesToUser",
          "db": "be-mean-mongo"
        }
      ],
      "inheritedPrivileges": [
        {
          "resource": {
            "db": "be-mean-mongo",
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
            "grantRolesToUser",
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
            "db": "be-mean-mongo",
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
            "db": "be-mean-mongo",
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
            "db": "be-mean-mongo",
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

### 1. Criando o Config Server

```js
➜  ~ sudo mkdir /data/configdb
➜  ~ mongod --configsvr --port 27010
2016-02-26T01:09:50.463-0300 I CONTROL  [initandlisten] MongoDB starting : pid=27628 port=27010 dbpath=/data/configdb master=1 64-bit host=paulo-sti-ni-1401
2016-02-26T01:09:50.464-0300 I CONTROL  [initandlisten] db version v3.2.3
2016-02-26T01:09:50.464-0300 I CONTROL  [initandlisten] git version: b326ba837cf6f49d65c2f85e1b70f6f31ece7937
2016-02-26T01:09:50.464-0300 I CONTROL  [initandlisten] OpenSSL version: OpenSSL 1.0.2d 9 Jul 2015
2016-02-26T01:09:50.464-0300 I CONTROL  [initandlisten] allocator: tcmalloc
2016-02-26T01:09:50.464-0300 I CONTROL  [initandlisten] modules: none
2016-02-26T01:09:50.464-0300 I CONTROL  [initandlisten] build environment:
2016-02-26T01:09:50.464-0300 I CONTROL  [initandlisten]     distmod: ubuntu1404
2016-02-26T01:09:50.464-0300 I CONTROL  [initandlisten]     distarch: x86_64
2016-02-26T01:09:50.464-0300 I CONTROL  [initandlisten]     target_arch: x86_64
2016-02-26T01:09:50.464-0300 I CONTROL  [initandlisten] options: { net: { port: 27010 }, sharding: { clusterRole: "configsvr" } }
2016-02-26T01:09:50.498-0300 I STORAGE  [initandlisten] wiredtiger_open config: create,cache_size=1G,session_max=20000,eviction=(threads_max=4),config_base=false,statistics=(fast),log=(enabled=true,archive=true,path=journal,compressor=snappy),file_manager=(close_idle_time=100000),checkpoint=(wait=60,log_size=2GB),statistics_log=(wait=0),
2016-02-26T01:09:51.008-0300 I CONTROL  [initandlisten] ** WARNING: You are running this process as the root user, which is not recommended.
2016-02-26T01:09:51.008-0300 I CONTROL  [initandlisten] 
2016-02-26T01:09:51.009-0300 I CONTROL  [initandlisten] 
2016-02-26T01:09:51.009-0300 I CONTROL  [initandlisten] ** WARNING: /sys/kernel/mm/transparent_hugepage/enabled is 'always'.
2016-02-26T01:09:51.009-0300 I CONTROL  [initandlisten] **        We suggest setting it to 'never'
2016-02-26T01:09:51.009-0300 I CONTROL  [initandlisten] 
2016-02-26T01:09:51.009-0300 I CONTROL  [initandlisten] ** WARNING: /sys/kernel/mm/transparent_hugepage/defrag is 'always'.
2016-02-26T01:09:51.009-0300 I CONTROL  [initandlisten] **        We suggest setting it to 'never'
2016-02-26T01:09:51.009-0300 I CONTROL  [initandlisten] 
2016-02-26T01:09:51.354-0300 I REPL     [initandlisten] ******
2016-02-26T01:09:51.354-0300 I REPL     [initandlisten] creating replication oplog of size: 5MB...
2016-02-26T01:09:51.505-0300 I STORAGE  [initandlisten] Starting WiredTigerRecordStoreThread local.oplog.$main
2016-02-26T01:09:51.506-0300 I STORAGE  [initandlisten] The size storer reports that the oplog contains 0 records totaling to 0 bytes
2016-02-26T01:09:51.506-0300 I STORAGE  [initandlisten] Scanning the oplog to determine where to place markers for truncation
2016-02-26T01:09:51.959-0300 I REPL     [initandlisten] ******
2016-02-26T01:09:51.960-0300 I FTDC     [initandlisten] Initializing full-time diagnostic data capture with directory '/data/configdb/diagnostic.data'
2016-02-26T01:09:51.960-0300 I NETWORK  [HostnameCanonicalizationWorker] Starting hostname canonicalization worker
2016-02-26T01:09:52.071-0300 I NETWORK  [initandlisten] waiting for connections on port 27010
```
### 2. Criando o Router

```js
➜  ~ mongos --configdb localhost:27010 --port 27011
2016-02-26T01:14:56.400-0300 W SHARDING [main] Running a sharded cluster with fewer than 3 config servers should only be done for testing purposes and is not recommended for production.
2016-02-26T01:14:56.498-0300 I SHARDING [mongosMain] MongoS version 3.2.3 starting: pid=27852 port=27011 64-bit host=paulo-sti-ni-1401 (--help for usage)
2016-02-26T01:14:56.498-0300 I CONTROL  [mongosMain] db version v3.2.3
2016-02-26T01:14:56.498-0300 I CONTROL  [mongosMain] git version: b326ba837cf6f49d65c2f85e1b70f6f31ece7937
2016-02-26T01:14:56.498-0300 I CONTROL  [mongosMain] OpenSSL version: OpenSSL 1.0.2d 9 Jul 2015
2016-02-26T01:14:56.498-0300 I CONTROL  [mongosMain] allocator: tcmalloc
2016-02-26T01:14:56.498-0300 I CONTROL  [mongosMain] modules: none
2016-02-26T01:14:56.498-0300 I CONTROL  [mongosMain] build environment:
2016-02-26T01:14:56.498-0300 I CONTROL  [mongosMain]     distmod: ubuntu1404
2016-02-26T01:14:56.498-0300 I CONTROL  [mongosMain]     distarch: x86_64
2016-02-26T01:14:56.498-0300 I CONTROL  [mongosMain]     target_arch: x86_64
2016-02-26T01:14:56.498-0300 I CONTROL  [mongosMain] options: { net: { port: 27011 }, sharding: { configDB: "localhost:27010" } }
2016-02-26T01:14:56.504-0300 I SHARDING [mongosMain] Updating config server connection string to: localhost:27010
2016-02-26T01:14:56.524-0300 I SHARDING [LockPinger] creating distributed lock ping thread for localhost:27010 and process paulo-sti-ni-1401:27011:1456460096:1352948474 (sleeping for 30000ms)
2016-02-26T01:14:56.771-0300 I SHARDING [LockPinger] cluster localhost:27010 pinged successfully at 2016-02-26T01:14:56.525-0300 by distributed lock pinger 'localhost:27010/paulo-sti-ni-1401:27011:1456460096:1352948474', sleeping for 30000ms
2016-02-26T01:14:57.104-0300 I SHARDING [mongosMain] distributed lock 'configUpgrade/paulo-sti-ni-1401:27011:1456460096:1352948474' acquired for 'initializing config database to new format v6', ts : 56cfd140c936b4614bd895f9
2016-02-26T01:14:57.106-0300 I SHARDING [mongosMain] initializing config server version to 6
2016-02-26T01:14:57.106-0300 I SHARDING [mongosMain] writing initial config version at v6
2016-02-26T01:14:57.258-0300 I SHARDING [mongosMain] initialization of config server to v6 successful
2016-02-26T01:14:57.259-0300 I SHARDING [mongosMain] distributed lock 'configUpgrade/paulo-sti-ni-1401:27011:1456460096:1352948474' unlocked. 
2016-02-26T01:14:58.881-0300 I NETWORK  [HostnameCanonicalizationWorker] Starting hostname canonicalization worker
2016-02-26T01:14:58.881-0300 I SHARDING [Balancer] about to contact config servers and shards
2016-02-26T01:14:58.881-0300 I SHARDING [Balancer] config servers and shards contacted successfully
2016-02-26T01:14:58.882-0300 I SHARDING [Balancer] balancer id: paulo-sti-ni-1401:27011 started
2016-02-26T01:14:58.937-0300 I NETWORK  [mongosMain] waiting for connections on port 27011
2016-02-26T01:14:59.117-0300 I SHARDING [Balancer] distributed lock 'balancer/paulo-sti-ni-1401:27011:1456460096:1352948474' acquired for 'doing balance round', ts : 56cfd143c936b4614bd895fc
2016-02-26T01:14:59.316-0300 I SHARDING [Balancer] about to log metadata event into actionlog: { _id: "paulo-sti-ni-1401-2016-02-26T01:14:59.316-0300-56cfd143c936b4614bd895fd", server: "paulo-sti-ni-1401", clientAddr: "", time: new Date(1456460099316), what: "balancer.round", ns: "", details: { executionTimeMillis: 235, errorOccured: false, candidateChunks: 0, chunksMoved: 0 } }
2016-02-26T01:14:59.335-0300 I SHARDING [Balancer] distributed lock 'balancer/paulo-sti-ni-1401:27011:1456460096:1352948474' unlocked. 
2016-02-26T01:15:09.349-0300 I SHARDING [Balancer] distributed lock 'balancer/paulo-sti-ni-1401:27011:1456460096:1352948474' acquired for 'doing balance round', ts : 56cfd14dc936b4614bd895fe
2016-02-26T01:15:09.349-0300 I SHARDING [Balancer] about to log metadata event into actionlog: { _id: "paulo-sti-ni-1401-2016-02-26T01:15:09.349-0300-56cfd14dc936b4614bd895ff", server: "paulo-sti-ni-1401", clientAddr: "", time: new Date(1456460109349), what: "balancer.round", ns: "", details: { executionTimeMillis: 4, errorOccured: false, candidateChunks: 0, chunksMoved: 0 } }
2016-02-26T01:15:09.424-0300 I SHARDING [Balancer] distributed lock 'balancer/paulo-sti-ni-1401:27011:1456460096:1352948474' unlocked. 
2016-02-26T01:15:19.438-0300 I SHARDING [Balancer] distributed lock 'balancer/paulo-sti-ni-1401:27011:1456460096:1352948474' acquired for 'doing balance round', ts : 56cfd157c936b4614bd89600
2016-02-26T01:15:19.438-0300 I SHARDING [Balancer] about to log metadata event into actionlog: { _id: "paulo-sti-ni-1401-2016-02-26T01:15:19.438-0300-56cfd157c936b4614bd89601", server: "paulo-sti-ni-1401", clientAddr: "", time: new Date(1456460119438), what: "balancer.round", ns: "", details: { executionTimeMillis: 3, errorOccured: false, candidateChunks: 0, chunksMoved: 0 } }
2016-02-26T01:15:19.458-0300 I SHARDING [Balancer] distributed lock 'balancer/paulo-sti-ni-1401:27011:1456460096:1352948474' unlocked. 
```

### 3. Criando os Shards

#### Criação das pastas de cada Shard
```js
➜  ~ sudo mkdir /data/shard1 && sudo mkdir /data/shard2 && sudo mkdir /data/shard3 
```

#### Criação das Shards

```js
~ sudo mongod --port 27012 --dbpath /data/shard1
2016-02-26T01:22:35.989-0300 I CONTROL  [initandlisten] MongoDB starting : pid=28205 port=27012 dbpath=/data/shard1 64-bit host=paulo-sti-ni-1401
2016-02-26T01:22:35.990-0300 I CONTROL  [initandlisten] db version v3.2.3
2016-02-26T01:22:35.990-0300 I CONTROL  [initandlisten] git version: b326ba837cf6f49d65c2f85e1b70f6f31ece7937
2016-02-26T01:22:35.990-0300 I CONTROL  [initandlisten] OpenSSL version: OpenSSL 1.0.2d 9 Jul 2015
2016-02-26T01:22:35.990-0300 I CONTROL  [initandlisten] allocator: tcmalloc
2016-02-26T01:22:35.990-0300 I CONTROL  [initandlisten] modules: none
2016-02-26T01:22:35.990-0300 I CONTROL  [initandlisten] build environment:
2016-02-26T01:22:35.990-0300 I CONTROL  [initandlisten]     distmod: ubuntu1404
2016-02-26T01:22:35.990-0300 I CONTROL  [initandlisten]     distarch: x86_64
2016-02-26T01:22:35.990-0300 I CONTROL  [initandlisten]     target_arch: x86_64
2016-02-26T01:22:35.990-0300 I CONTROL  [initandlisten] options: { net: { port: 27012 }, storage: { dbPath: "/data/shard1" } }
2016-02-26T01:22:36.045-0300 I STORAGE  [initandlisten] wiredtiger_open config: create,cache_size=1G,session_max=20000,eviction=(threads_max=4),config_base=false,statistics=(fast),log=(enabled=true,archive=true,path=journal,compressor=snappy),file_manager=(close_idle_time=100000),checkpoint=(wait=60,log_size=2GB),statistics_log=(wait=0),
2016-02-26T01:22:36.592-0300 I CONTROL  [initandlisten] ** WARNING: You are running this process as the root user, which is not recommended.
2016-02-26T01:22:36.592-0300 I CONTROL  [initandlisten] 
2016-02-26T01:22:36.592-0300 I CONTROL  [initandlisten] 
2016-02-26T01:22:36.592-0300 I CONTROL  [initandlisten] ** WARNING: /sys/kernel/mm/transparent_hugepage/enabled is 'always'.
2016-02-26T01:22:36.592-0300 I CONTROL  [initandlisten] **        We suggest setting it to 'never'
2016-02-26T01:22:36.592-0300 I CONTROL  [initandlisten] 
2016-02-26T01:22:36.592-0300 I CONTROL  [initandlisten] ** WARNING: /sys/kernel/mm/transparent_hugepage/defrag is 'always'.
2016-02-26T01:22:36.592-0300 I CONTROL  [initandlisten] **        We suggest setting it to 'never'
2016-02-26T01:22:36.592-0300 I CONTROL  [initandlisten] 
2016-02-26T01:22:36.593-0300 I FTDC     [initandlisten] Initializing full-time diagnostic data capture with directory '/data/shard1/diagnostic.data'
2016-02-26T01:22:36.593-0300 I NETWORK  [HostnameCanonicalizationWorker] Starting hostname canonicalization worker
2016-02-26T01:22:36.769-0300 I NETWORK  [initandlisten] waiting for connections on port 27012
```

```js
sudo mongod --port 27013 --dbpath /data/shard2
[sudo] senha para paulo: 
2016-02-26T01:23:11.604-0300 I CONTROL  [initandlisten] MongoDB starting : pid=28266 port=27013 dbpath=/data/shard2 64-bit host=paulo-sti-ni-1401
2016-02-26T01:23:11.604-0300 I CONTROL  [initandlisten] db version v3.2.3
2016-02-26T01:23:11.604-0300 I CONTROL  [initandlisten] git version: b326ba837cf6f49d65c2f85e1b70f6f31ece7937
2016-02-26T01:23:11.604-0300 I CONTROL  [initandlisten] OpenSSL version: OpenSSL 1.0.2d 9 Jul 2015
2016-02-26T01:23:11.604-0300 I CONTROL  [initandlisten] allocator: tcmalloc
2016-02-26T01:23:11.604-0300 I CONTROL  [initandlisten] modules: none
2016-02-26T01:23:11.604-0300 I CONTROL  [initandlisten] build environment:
2016-02-26T01:23:11.604-0300 I CONTROL  [initandlisten]     distmod: ubuntu1404
2016-02-26T01:23:11.604-0300 I CONTROL  [initandlisten]     distarch: x86_64
2016-02-26T01:23:11.604-0300 I CONTROL  [initandlisten]     target_arch: x86_64
2016-02-26T01:23:11.604-0300 I CONTROL  [initandlisten] options: { net: { port: 27013 }, storage: { dbPath: "/data/shard2" } }
2016-02-26T01:23:11.637-0300 I STORAGE  [initandlisten] wiredtiger_open config: create,cache_size=1G,session_max=20000,eviction=(threads_max=4),config_base=false,statistics=(fast),log=(enabled=true,archive=true,path=journal,compressor=snappy),file_manager=(close_idle_time=100000),checkpoint=(wait=60,log_size=2GB),statistics_log=(wait=0),
2016-02-26T01:23:12.356-0300 I CONTROL  [initandlisten] ** WARNING: You are running this process as the root user, which is not recommended.
2016-02-26T01:23:12.356-0300 I CONTROL  [initandlisten] 
2016-02-26T01:23:12.356-0300 I CONTROL  [initandlisten] 
2016-02-26T01:23:12.356-0300 I CONTROL  [initandlisten] ** WARNING: /sys/kernel/mm/transparent_hugepage/enabled is 'always'.
2016-02-26T01:23:12.357-0300 I CONTROL  [initandlisten] **        We suggest setting it to 'never'
2016-02-26T01:23:12.357-0300 I CONTROL  [initandlisten] 
2016-02-26T01:23:12.357-0300 I CONTROL  [initandlisten] ** WARNING: /sys/kernel/mm/transparent_hugepage/defrag is 'always'.
2016-02-26T01:23:12.357-0300 I CONTROL  [initandlisten] **        We suggest setting it to 'never'
2016-02-26T01:23:12.357-0300 I CONTROL  [initandlisten] 
2016-02-26T01:23:12.358-0300 I FTDC     [initandlisten] Initializing full-time diagnostic data capture with directory '/data/shard2/diagnostic.data'
2016-02-26T01:23:12.358-0300 I NETWORK  [HostnameCanonicalizationWorker] Starting hostname canonicalization worker
2016-02-26T01:23:12.535-0300 I NETWORK  [initandlisten] waiting for connections on port 27013
```

```js
➜  ~ sudo mongod --port 27014 --dbpath /data/shard3
[sudo] senha para paulo: 
2016-02-26T01:24:02.136-0300 I CONTROL  [initandlisten] MongoDB starting : pid=28316 port=27014 dbpath=/data/shard3 64-bit host=paulo-sti-ni-1401
2016-02-26T01:24:02.136-0300 I CONTROL  [initandlisten] db version v3.2.3
2016-02-26T01:24:02.136-0300 I CONTROL  [initandlisten] git version: b326ba837cf6f49d65c2f85e1b70f6f31ece7937
2016-02-26T01:24:02.136-0300 I CONTROL  [initandlisten] OpenSSL version: OpenSSL 1.0.2d 9 Jul 2015
2016-02-26T01:24:02.137-0300 I CONTROL  [initandlisten] allocator: tcmalloc
2016-02-26T01:24:02.137-0300 I CONTROL  [initandlisten] modules: none
2016-02-26T01:24:02.137-0300 I CONTROL  [initandlisten] build environment:
2016-02-26T01:24:02.137-0300 I CONTROL  [initandlisten]     distmod: ubuntu1404
2016-02-26T01:24:02.137-0300 I CONTROL  [initandlisten]     distarch: x86_64
2016-02-26T01:24:02.137-0300 I CONTROL  [initandlisten]     target_arch: x86_64
2016-02-26T01:24:02.137-0300 I CONTROL  [initandlisten] options: { net: { port: 27014 }, storage: { dbPath: "/data/shard3" } }
2016-02-26T01:24:02.177-0300 I STORAGE  [initandlisten] wiredtiger_open config: create,cache_size=1G,session_max=20000,eviction=(threads_max=4),config_base=false,statistics=(fast),log=(enabled=true,archive=true,path=journal,compressor=snappy),file_manager=(close_idle_time=100000),checkpoint=(wait=60,log_size=2GB),statistics_log=(wait=0),
2016-02-26T01:24:03.255-0300 I CONTROL  [initandlisten] ** WARNING: You are running this process as the root user, which is not recommended.
2016-02-26T01:24:03.255-0300 I CONTROL  [initandlisten] 
2016-02-26T01:24:03.255-0300 I CONTROL  [initandlisten] 
2016-02-26T01:24:03.255-0300 I CONTROL  [initandlisten] ** WARNING: /sys/kernel/mm/transparent_hugepage/enabled is 'always'.
2016-02-26T01:24:03.255-0300 I CONTROL  [initandlisten] **        We suggest setting it to 'never'
2016-02-26T01:24:03.255-0300 I CONTROL  [initandlisten] 
2016-02-26T01:24:03.255-0300 I CONTROL  [initandlisten] ** WARNING: /sys/kernel/mm/transparent_hugepage/defrag is 'always'.
2016-02-26T01:24:03.255-0300 I CONTROL  [initandlisten] **        We suggest setting it to 'never'
2016-02-26T01:24:03.255-0300 I CONTROL  [initandlisten] 
2016-02-26T01:24:03.256-0300 I FTDC     [initandlisten] Initializing full-time diagnostic data capture with directory '/data/shard3/diagnostic.data'
2016-02-26T01:24:03.256-0300 I NETWORK  [HostnameCanonicalizationWorker] Starting hostname canonicalization worker
2016-02-26T01:24:03.435-0300 I NETWORK  [initandlisten] waiting for connections on port 27014
```

### Conectando no Router e adicionando os Shards

```js
➜  ~ mongo --port 27011 --host localhost
MongoDB shell version: 3.2.3
connecting to: localhost:27011/test
Mongo-Hacker 0.0.12
paulo-sti-ni-1401:27011(mongos-3.2.3)[mongos] test> sh.addShard("localhost:27012")
{
  "shardAdded": "shard0000",
  "ok": 1
}
paulo-sti-ni-1401:27011(mongos-3.2.3)[mongos] test> sh.addShard("localhost:27013")
{
  "shardAdded": "shard0001",
  "ok": 1
}
paulo-sti-ni-1401:27011(mongos-3.2.3)[mongos] test> sh.addShard("localhost:27014")
{
  "shardAdded": "shard0002",
  "ok": 1
}
```

### Sharding o banco be-mean-mongo

```js
paulo-sti-ni-1401:27011(mongos-3.2.3)[mongos] test> sh.enableSharding("be-mean-mongo")
{
  "ok": 1
}
```

### Sharding a collection users passando _id como shard key

```js
paulo-sti-ni-1401:27011(mongos-3.2.3)[mongos] test> sh.shardCollection("be-mean-mongo.users", {"_id" : 1})
{
  "collectionsharded": "be-mean-mongo.users",
  "ok": 1
}
```

### Enviando dados para o Router

```js
paulo-sti-ni-1401:27011(mongos-3.2.3)[mongos] be-mean-mongo> show collections
users → 0.000MB / 0.004MB
paulo-sti-ni-1401:27011(mongos-3.2.3)[mongos] be-mean-mongo> function create() {
...   var arr = [];
...   for (var i = 0; i < 100000; i++) {
...     arr[i] = {
...          name: 'usuario'+i
...          ,bio: 'some bio'
...         ,date_register: new Date
...         ,avatar_path: 'caminho da imagem'+i
...         ,auth: {
...           username: 'usuario'+i
...            ,email: 'usuario'+i+'@gmail.com'
...            ,password: '123'+i
...            ,last_access: new Date
...            ,online: false
...            ,disabled: false
...            ,hash_token: false
...          }
...        };
...    };
...    db.users.save(arr);
...  };
paulo-sti-ni-1401:27011(mongos-3.2.3)[mongos] be-mean-mongo> create()
Inserted 1 record(s) in 8767ms
paulo-sti-ni-1401:27011(mongos-3.2.3)[mongos] be-mean-mongo> db.users.count()
100000
```

## Replica

### Criando diretório das Réplicas

```
➜  ~ sudo mkdir /data/rs1 && sudo mkdir /data/rs2 && sudo mkdir /data/rs3
```

### Iniciando processos do mongod para cada Réplica

```js
➜  ~ sudo mongod --replSet be-mean-mongo --port 27017 --dbpath /data/rs1
2016-02-26T02:08:17.275-0300 I CONTROL  [initandlisten] MongoDB starting : pid=30091 port=27017 dbpath=/data/rs1 64-bit host=paulo-sti-ni-1401
2016-02-26T02:08:17.276-0300 I CONTROL  [initandlisten] db version v3.2.3
2016-02-26T02:08:17.276-0300 I CONTROL  [initandlisten] git version: b326ba837cf6f49d65c2f85e1b70f6f31ece7937
2016-02-26T02:08:17.276-0300 I CONTROL  [initandlisten] OpenSSL version: OpenSSL 1.0.2d 9 Jul 2015
2016-02-26T02:08:17.276-0300 I CONTROL  [initandlisten] allocator: tcmalloc
2016-02-26T02:08:17.276-0300 I CONTROL  [initandlisten] modules: none
2016-02-26T02:08:17.276-0300 I CONTROL  [initandlisten] build environment:
2016-02-26T02:08:17.276-0300 I CONTROL  [initandlisten]     distmod: ubuntu1404
2016-02-26T02:08:17.276-0300 I CONTROL  [initandlisten]     distarch: x86_64
2016-02-26T02:08:17.276-0300 I CONTROL  [initandlisten]     target_arch: x86_64
2016-02-26T02:08:17.276-0300 I CONTROL  [initandlisten] options: { net: { port: 27017 }, replication: { replSet: "be-mean-mongo" }, storage: { dbPath: "/data/rs1" } }
2016-02-26T02:08:17.312-0300 I STORAGE  [initandlisten] wiredtiger_open config: create,cache_size=1G,session_max=20000,eviction=(threads_max=4),config_base=false,statistics=(fast),log=(enabled=true,archive=true,path=journal,compressor=snappy),file_manager=(close_idle_time=100000),checkpoint=(wait=60,log_size=2GB),statistics_log=(wait=0),
2016-02-26T02:08:17.798-0300 I CONTROL  [initandlisten] ** WARNING: You are running this process as the root user, which is not recommended.
2016-02-26T02:08:17.798-0300 I CONTROL  [initandlisten] 
2016-02-26T02:08:17.799-0300 I CONTROL  [initandlisten] 
2016-02-26T02:08:17.799-0300 I CONTROL  [initandlisten] ** WARNING: /sys/kernel/mm/transparent_hugepage/enabled is 'always'.
2016-02-26T02:08:17.799-0300 I CONTROL  [initandlisten] **        We suggest setting it to 'never'
2016-02-26T02:08:17.799-0300 I CONTROL  [initandlisten] 
2016-02-26T02:08:17.799-0300 I CONTROL  [initandlisten] ** WARNING: /sys/kernel/mm/transparent_hugepage/defrag is 'always'.
2016-02-26T02:08:17.799-0300 I CONTROL  [initandlisten] **        We suggest setting it to 'never'
2016-02-26T02:08:17.799-0300 I CONTROL  [initandlisten] 
2016-02-26T02:08:17.982-0300 I REPL     [initandlisten] Did not find local voted for document at startup;  NoMatchingDocument: Did not find replica set lastVote document in local.replset.election
2016-02-26T02:08:17.982-0300 I REPL     [initandlisten] Did not find local replica set configuration document at startup;  NoMatchingDocument: Did not find replica set configuration document in local.system.replset
2016-02-26T02:08:17.982-0300 I FTDC     [initandlisten] Initializing full-time diagnostic data capture with directory '/data/rs1/diagnostic.data'
2016-02-26T02:08:17.982-0300 I NETWORK  [HostnameCanonicalizationWorker] Starting hostname canonicalization worker
2016-02-26T02:08:18.223-0300 I NETWORK  [initandlisten] waiting for connections on port 27017
```

```js
➜  ~ sudo mongod --replSet be-mean-mongo --port 27018 --dbpath /data/rs2
[sudo] senha para paulo: 
2016-02-26T02:09:01.579-0300 I CONTROL  [initandlisten] MongoDB starting : pid=30141 port=27018 dbpath=/data/rs2 64-bit host=paulo-sti-ni-1401
2016-02-26T02:09:01.579-0300 I CONTROL  [initandlisten] db version v3.2.3
2016-02-26T02:09:01.579-0300 I CONTROL  [initandlisten] git version: b326ba837cf6f49d65c2f85e1b70f6f31ece7937
2016-02-26T02:09:01.579-0300 I CONTROL  [initandlisten] OpenSSL version: OpenSSL 1.0.2d 9 Jul 2015
2016-02-26T02:09:01.579-0300 I CONTROL  [initandlisten] allocator: tcmalloc
2016-02-26T02:09:01.579-0300 I CONTROL  [initandlisten] modules: none
2016-02-26T02:09:01.579-0300 I CONTROL  [initandlisten] build environment:
2016-02-26T02:09:01.579-0300 I CONTROL  [initandlisten]     distmod: ubuntu1404
2016-02-26T02:09:01.579-0300 I CONTROL  [initandlisten]     distarch: x86_64
2016-02-26T02:09:01.579-0300 I CONTROL  [initandlisten]     target_arch: x86_64
2016-02-26T02:09:01.579-0300 I CONTROL  [initandlisten] options: { net: { port: 27018 }, replication: { replSet: "be-mean-mongo" }, storage: { dbPath: "/data/rs2" } }
2016-02-26T02:09:01.609-0300 I STORAGE  [initandlisten] wiredtiger_open config: create,cache_size=1G,session_max=20000,eviction=(threads_max=4),config_base=false,statistics=(fast),log=(enabled=true,archive=true,path=journal,compressor=snappy),file_manager=(close_idle_time=100000),checkpoint=(wait=60,log_size=2GB),statistics_log=(wait=0),
2016-02-26T02:09:02.125-0300 I CONTROL  [initandlisten] ** WARNING: You are running this process as the root user, which is not recommended.
2016-02-26T02:09:02.125-0300 I CONTROL  [initandlisten] 
2016-02-26T02:09:02.125-0300 I CONTROL  [initandlisten] 
2016-02-26T02:09:02.126-0300 I CONTROL  [initandlisten] ** WARNING: /sys/kernel/mm/transparent_hugepage/enabled is 'always'.
2016-02-26T02:09:02.126-0300 I CONTROL  [initandlisten] **        We suggest setting it to 'never'
2016-02-26T02:09:02.126-0300 I CONTROL  [initandlisten] 
2016-02-26T02:09:02.126-0300 I CONTROL  [initandlisten] ** WARNING: /sys/kernel/mm/transparent_hugepage/defrag is 'always'.
2016-02-26T02:09:02.126-0300 I CONTROL  [initandlisten] **        We suggest setting it to 'never'
2016-02-26T02:09:02.126-0300 I CONTROL  [initandlisten] 
2016-02-26T02:09:02.292-0300 I REPL     [initandlisten] Did not find local voted for document at startup;  NoMatchingDocument: Did not find replica set lastVote document in local.replset.election
2016-02-26T02:09:02.292-0300 I REPL     [initandlisten] Did not find local replica set configuration document at startup;  NoMatchingDocument: Did not find replica set configuration document in local.system.replset
2016-02-26T02:09:02.292-0300 I FTDC     [initandlisten] Initializing full-time diagnostic data capture with directory '/data/rs2/diagnostic.data'
2016-02-26T02:09:02.292-0300 I NETWORK  [HostnameCanonicalizationWorker] Starting hostname canonicalization worker
2016-02-26T02:09:02.430-0300 I NETWORK  [initandlisten] waiting for connections on port 27018
```

```js
➜  ~ sudo mongod --replSet be-mean-mongo --port 27019 --dbpath /data/rs3
[sudo] senha para paulo: 
2016-02-26T02:09:22.164-0300 I CONTROL  [initandlisten] MongoDB starting : pid=30229 port=27019 dbpath=/data/rs3 64-bit host=paulo-sti-ni-1401
2016-02-26T02:09:22.165-0300 I CONTROL  [initandlisten] db version v3.2.3
2016-02-26T02:09:22.165-0300 I CONTROL  [initandlisten] git version: b326ba837cf6f49d65c2f85e1b70f6f31ece7937
2016-02-26T02:09:22.165-0300 I CONTROL  [initandlisten] OpenSSL version: OpenSSL 1.0.2d 9 Jul 2015
2016-02-26T02:09:22.165-0300 I CONTROL  [initandlisten] allocator: tcmalloc
2016-02-26T02:09:22.165-0300 I CONTROL  [initandlisten] modules: none
2016-02-26T02:09:22.165-0300 I CONTROL  [initandlisten] build environment:
2016-02-26T02:09:22.165-0300 I CONTROL  [initandlisten]     distmod: ubuntu1404
2016-02-26T02:09:22.165-0300 I CONTROL  [initandlisten]     distarch: x86_64
2016-02-26T02:09:22.165-0300 I CONTROL  [initandlisten]     target_arch: x86_64
2016-02-26T02:09:22.165-0300 I CONTROL  [initandlisten] options: { net: { port: 27019 }, replication: { replSet: "be-mean-mongo" }, storage: { dbPath: "/data/rs3" } }
2016-02-26T02:09:22.194-0300 I STORAGE  [initandlisten] wiredtiger_open config: create,cache_size=1G,session_max=20000,eviction=(threads_max=4),config_base=false,statistics=(fast),log=(enabled=true,archive=true,path=journal,compressor=snappy),file_manager=(close_idle_time=100000),checkpoint=(wait=60,log_size=2GB),statistics_log=(wait=0),
2016-02-26T02:09:22.658-0300 I CONTROL  [initandlisten] ** WARNING: You are running this process as the root user, which is not recommended.
2016-02-26T02:09:22.658-0300 I CONTROL  [initandlisten] 
2016-02-26T02:09:22.658-0300 I CONTROL  [initandlisten] 
2016-02-26T02:09:22.658-0300 I CONTROL  [initandlisten] ** WARNING: /sys/kernel/mm/transparent_hugepage/enabled is 'always'.
2016-02-26T02:09:22.658-0300 I CONTROL  [initandlisten] **        We suggest setting it to 'never'
2016-02-26T02:09:22.658-0300 I CONTROL  [initandlisten] 
2016-02-26T02:09:22.658-0300 I CONTROL  [initandlisten] ** WARNING: /sys/kernel/mm/transparent_hugepage/defrag is 'always'.
2016-02-26T02:09:22.658-0300 I CONTROL  [initandlisten] **        We suggest setting it to 'never'
2016-02-26T02:09:22.658-0300 I CONTROL  [initandlisten] 
2016-02-26T02:09:22.840-0300 I REPL     [initandlisten] Did not find local voted for document at startup;  NoMatchingDocument: Did not find replica set lastVote document in local.replset.election
2016-02-26T02:09:22.840-0300 I REPL     [initandlisten] Did not find local replica set configuration document at startup;  NoMatchingDocument: Did not find replica set configuration document in local.system.replset
2016-02-26T02:09:22.840-0300 I FTDC     [initandlisten] Initializing full-time diagnostic data capture with directory '/data/rs3/diagnostic.data'
2016-02-26T02:09:22.840-0300 I NETWORK  [HostnameCanonicalizationWorker] Starting hostname canonicalization worker
2016-02-26T02:09:23.032-0300 I NETWORK  [initandlisten] waiting for connections on port 27019
```

### Logando na porta que a Réplica que será a primaria está utilizando

```js
➜  ~ mongo --port 27017
MongoDB shell version: 3.2.3
connecting to: 127.0.0.1:27017/test
Mongo-Hacker 0.0.12
Server has startup warnings: 
2016-02-26T02:08:17.798-0300 I CONTROL  [initandlisten] ** WARNING: You are running this process as the root user, which is not recommended.
2016-02-26T02:08:17.798-0300 I CONTROL  [initandlisten] 
2016-02-26T02:08:17.799-0300 I CONTROL  [initandlisten] 
2016-02-26T02:08:17.799-0300 I CONTROL  [initandlisten] ** WARNING: /sys/kernel/mm/transparent_hugepage/enabled is 'always'.
2016-02-26T02:08:17.799-0300 I CONTROL  [initandlisten] **        We suggest setting it to 'never'
2016-02-26T02:08:17.799-0300 I CONTROL  [initandlisten] 
2016-02-26T02:08:17.799-0300 I CONTROL  [initandlisten] ** WARNING: /sys/kernel/mm/transparent_hugepage/defrag is 'always'.
2016-02-26T02:08:17.799-0300 I CONTROL  [initandlisten] **        We suggest setting it to 'never'
2016-02-26T02:08:17.799-0300 I CONTROL  [initandlisten] 
paulo-sti-ni-1401(mongod-3.2.3) test> 
```

### Iniciando ReplicaSet

```js
paulo-sti-ni-1401(mongod-3.2.3) test> rsconf = {
...    _id: "be-mean-mongo",
...    members: [
...     {
...      _id: 0,
...      host: "127.0.0.1:27017"
...     }
...   ]
... }
{
  "_id": "be-mean-mongo",
  "members": [
    {
      "_id": 0,
      "host": "127.0.0.1:27017"
    }
  ]
}
paulo-sti-ni-1401(mongod-3.2.3) test> rs.initiate(rsconf)
{
  "ok": 1
}
```

### Adicionando as Réplicas secundárias

```js
paulo-sti-ni-1401(mongod-3.2.3)[PRIMARY:be-mean-mongo] test> rs.add("127.0.0.1:27018")
{
  "ok": 1
}
paulo-sti-ni-1401(mongod-3.2.3)[PRIMARY:be-mean-mongo] test> rs.add("127.0.0.1:27019")
{
  "ok": 1
}
```

### Conectando nas replicas secundárias para verificar se deu certo

```js
➜  ~ mongo --port 27018                                                 
MongoDB shell version: 3.2.3
connecting to: 127.0.0.1:27018/test
Mongo-Hacker 0.0.12
Server has startup warnings: 
2016-02-26T02:09:02.125-0300 I CONTROL  [initandlisten] ** WARNING: You are running this process as the root user, which is not recommended.
2016-02-26T02:09:02.125-0300 I CONTROL  [initandlisten] 
2016-02-26T02:09:02.125-0300 I CONTROL  [initandlisten] 
2016-02-26T02:09:02.126-0300 I CONTROL  [initandlisten] ** WARNING: /sys/kernel/mm/transparent_hugepage/enabled is 'always'.
2016-02-26T02:09:02.126-0300 I CONTROL  [initandlisten] **        We suggest setting it to 'never'
2016-02-26T02:09:02.126-0300 I CONTROL  [initandlisten] 
2016-02-26T02:09:02.126-0300 I CONTROL  [initandlisten] ** WARNING: /sys/kernel/mm/transparent_hugepage/defrag is 'always'.
2016-02-26T02:09:02.126-0300 I CONTROL  [initandlisten] **        We suggest setting it to 'never'
2016-02-26T02:09:02.126-0300 I CONTROL  [initandlisten] 
paulo-sti-ni-1401:27018(mongod-3.2.3)[SECONDARY:be-mean-mongo] test> 
```

```js
➜  ~ mongo --port 27019
MongoDB shell version: 3.2.3
connecting to: 127.0.0.1:27019/test
Mongo-Hacker 0.0.12
Server has startup warnings: 
2016-02-26T02:09:22.658-0300 I CONTROL  [initandlisten] ** WARNING: You are running this process as the root user, which is not recommended.
2016-02-26T02:09:22.658-0300 I CONTROL  [initandlisten] 
2016-02-26T02:09:22.658-0300 I CONTROL  [initandlisten] 
2016-02-26T02:09:22.658-0300 I CONTROL  [initandlisten] ** WARNING: /sys/kernel/mm/transparent_hugepage/enabled is 'always'.
2016-02-26T02:09:22.658-0300 I CONTROL  [initandlisten] **        We suggest setting it to 'never'
2016-02-26T02:09:22.658-0300 I CONTROL  [initandlisten] 
2016-02-26T02:09:22.658-0300 I CONTROL  [initandlisten] ** WARNING: /sys/kernel/mm/transparent_hugepage/defrag is 'always'.
2016-02-26T02:09:22.658-0300 I CONTROL  [initandlisten] **        We suggest setting it to 'never'
2016-02-26T02:09:22.658-0300 I CONTROL  [initandlisten] 
paulo-sti-ni-1401:27019(mongod-3.2.3)[SECONDARY:be-mean-mongo] test> 
```

### Árbitro

#### Criando uma Réplica que servirá de árbitro

```js
➜  ~ sudo mkdir /data/arb
➜  ~ sudo mongod --port 30000 --dbpath /data/arb --replSet be-mean-mongo
2016-02-26T02:26:55.310-0300 I CONTROL  [initandlisten] MongoDB starting : pid=31331 port=30000 dbpath=/data/arb 64-bit host=paulo-sti-ni-1401
2016-02-26T02:26:55.310-0300 I CONTROL  [initandlisten] db version v3.2.3
2016-02-26T02:26:55.310-0300 I CONTROL  [initandlisten] git version: b326ba837cf6f49d65c2f85e1b70f6f31ece7937
2016-02-26T02:26:55.310-0300 I CONTROL  [initandlisten] OpenSSL version: OpenSSL 1.0.2d 9 Jul 2015
2016-02-26T02:26:55.310-0300 I CONTROL  [initandlisten] allocator: tcmalloc
2016-02-26T02:26:55.310-0300 I CONTROL  [initandlisten] modules: none
2016-02-26T02:26:55.310-0300 I CONTROL  [initandlisten] build environment:
2016-02-26T02:26:55.310-0300 I CONTROL  [initandlisten]     distmod: ubuntu1404
2016-02-26T02:26:55.310-0300 I CONTROL  [initandlisten]     distarch: x86_64
2016-02-26T02:26:55.310-0300 I CONTROL  [initandlisten]     target_arch: x86_64
2016-02-26T02:26:55.310-0300 I CONTROL  [initandlisten] options: { net: { port: 30000 }, replication: { replSet: "be-mean-mongo" }, storage: { dbPath: "/data/arb" } }
2016-02-26T02:26:55.350-0300 I STORAGE  [initandlisten] wiredtiger_open config: create,cache_size=1G,session_max=20000,eviction=(threads_max=4),config_base=false,statistics=(fast),log=(enabled=true,archive=true,path=journal,compressor=snappy),file_manager=(close_idle_time=100000),checkpoint=(wait=60,log_size=2GB),statistics_log=(wait=0),
2016-02-26T02:26:56.051-0300 I CONTROL  [initandlisten] ** WARNING: You are running this process as the root user, which is not recommended.
2016-02-26T02:26:56.051-0300 I CONTROL  [initandlisten] 
2016-02-26T02:26:56.051-0300 I CONTROL  [initandlisten] 
2016-02-26T02:26:56.051-0300 I CONTROL  [initandlisten] ** WARNING: /sys/kernel/mm/transparent_hugepage/enabled is 'always'.
2016-02-26T02:26:56.051-0300 I CONTROL  [initandlisten] **        We suggest setting it to 'never'
2016-02-26T02:26:56.051-0300 I CONTROL  [initandlisten] 
2016-02-26T02:26:56.051-0300 I CONTROL  [initandlisten] ** WARNING: /sys/kernel/mm/transparent_hugepage/defrag is 'always'.
2016-02-26T02:26:56.051-0300 I CONTROL  [initandlisten] **        We suggest setting it to 'never'
2016-02-26T02:26:56.051-0300 I CONTROL  [initandlisten] 
2016-02-26T02:26:56.309-0300 I REPL     [initandlisten] Did not find local voted for document at startup;  NoMatchingDocument: Did not find replica set lastVote document in local.replset.election
2016-02-26T02:26:56.309-0300 I REPL     [initandlisten] Did not find local replica set configuration document at startup;  NoMatchingDocument: Did not find replica set configuration document in local.system.replset
2016-02-26T02:26:56.310-0300 I FTDC     [initandlisten] Initializing full-time diagnostic data capture with directory '/data/arb/diagnostic.data'
2016-02-26T02:26:56.310-0300 I NETWORK  [HostnameCanonicalizationWorker] Starting hostname canonicalization worker
2016-02-26T02:26:56.621-0300 I NETWORK  [initandlisten] waiting for connections on port 30000
```

#### Adicionando o árbitro na replica primária

```js
paulo-sti-ni-1401(mongod-3.2.3)[PRIMARY:be-mean-mongo] test> rs.addArb("127.0.0.1:30000")
{
  "ok": 1
}
```


### Conferindo o status da ReplicaSet

```js
paulo-sti-ni-1401(mongod-3.2.3)[PRIMARY:be-mean-mongo] test> rs.status()
{
  "set": "be-mean-mongo",
  "date": ISODate("2016-02-26T05:51:05.941Z"),
  "myState": 1,
  "term": NumberLong("1"),
  "heartbeatIntervalMillis": NumberLong("2000"),
  "members": [
    {
      "_id": 0,
      "name": "127.0.0.1:27017",
      "health": 1,
      "state": 1,
      "stateStr": "PRIMARY",
      "uptime": 2568,
      "optime": {
        "ts": Timestamp(1456464523, 1),
        "t": NumberLong("1")
      },
      "optimeDate": ISODate("2016-02-26T05:28:43Z"),
      "electionTime": Timestamp(1456463904, 2),
      "electionDate": ISODate("2016-02-26T05:18:24Z"),
      "configVersion": 4,
      "self": true
    },
    {
      "_id": 1,
      "name": "127.0.0.1:27018",
      "health": 1,
      "state": 2,
      "stateStr": "SECONDARY",
      "uptime": 1891,
      "optime": {
        "ts": Timestamp(1456464523, 1),
        "t": NumberLong("1")
      },
      "optimeDate": ISODate("2016-02-26T05:28:43Z"),
      "lastHeartbeat": ISODate("2016-02-26T05:51:05.798Z"),
      "lastHeartbeatRecv": ISODate("2016-02-26T05:51:04.763Z"),
      "pingMs": NumberLong("0"),
      "syncingTo": "127.0.0.1:27017",
      "configVersion": 4
    },
    {
      "_id": 2,
      "name": "127.0.0.1:27019",
      "health": 1,
      "state": 2,
      "stateStr": "SECONDARY",
      "uptime": 1888,
      "optime": {
        "ts": Timestamp(1456464523, 1),
        "t": NumberLong("1")
      },
      "optimeDate": ISODate("2016-02-26T05:28:43Z"),
      "lastHeartbeat": ISODate("2016-02-26T05:51:05.798Z"),
      "lastHeartbeatRecv": ISODate("2016-02-26T05:51:04.757Z"),
      "pingMs": NumberLong("0"),
      "syncingTo": "127.0.0.1:27017",
      "configVersion": 4
    },
    {
      "_id": 3,
      "name": "127.0.0.1:30000",
      "health": 1,
      "state": 7,
      "stateStr": "ARBITER",
      "uptime": 1342,
      "lastHeartbeat": ISODate("2016-02-26T05:51:05.798Z"),
      "lastHeartbeatRecv": ISODate("2016-02-26T05:51:03.793Z"),
      "pingMs": NumberLong("0"),
      "configVersion": 4
    }
  ],
  "ok": 1
}
```

### Oplog Status

```js
paulo-sti-ni-1401(mongod-3.2.3)[PRIMARY:be-mean-mongo] test> rs.printReplicationInfo()
configured oplog size:   8268.15546798706MB
log length start to end: 619secs (0.17hrs)
oplog first event time:  Fri Feb 26 2016 02:18:24 GMT-0300 (BRT)
oplog last event time:   Fri Feb 26 2016 02:28:43 GMT-0300 (BRT)
now:                     Fri Feb 26 2016 02:52:44 GMT-0300 (BRT)
```

### Rebaixando a Replica primária

```js
paulo-sti-ni-1401(mongod-3.2.3)[PRIMARY:be-mean-mongo] test> rs.stepDown()
2016-02-26T02:53:25.278-0300 E QUERY    [thread1] Error: error doing query: failed: network error while attempting to run command 'replSetStepDown' on host '127.0.0.1:27017'  :
DB.prototype.runCommand@src/mongo/shell/db.js:132:1
DB.prototype.adminCommand@src/mongo/shell/db.js:149:12
rs.stepDown@src/mongo/shell/utils.js:1080:12
@(shell):1:1

2016-02-26T02:53:25.280-0300 I NETWORK  [thread1] trying reconnect to 127.0.0.1:27017 (127.0.0.1) failed
2016-02-26T02:53:25.280-0300 I NETWORK  [thread1] reconnect 127.0.0.1:27017 (127.0.0.1) ok
paulo-sti-ni-1401(mongod-3.2.3)[SECONDARY:be-mean-mongo] test> 
```

#### Verificando qual passou a ser a Réplica primária

```js
➜  ~ mongo --port 27019
MongoDB shell version: 3.2.3
connecting to: 127.0.0.1:27019/test
Mongo-Hacker 0.0.12
Server has startup warnings: 
2016-02-26T02:09:22.658-0300 I CONTROL  [initandlisten] ** WARNING: You are running this process as the root user, which is not recommended.
2016-02-26T02:09:22.658-0300 I CONTROL  [initandlisten] 
2016-02-26T02:09:22.658-0300 I CONTROL  [initandlisten] 
2016-02-26T02:09:22.658-0300 I CONTROL  [initandlisten] ** WARNING: /sys/kernel/mm/transparent_hugepage/enabled is 'always'.
2016-02-26T02:09:22.658-0300 I CONTROL  [initandlisten] **        We suggest setting it to 'never'
2016-02-26T02:09:22.658-0300 I CONTROL  [initandlisten] 
2016-02-26T02:09:22.658-0300 I CONTROL  [initandlisten] ** WARNING: /sys/kernel/mm/transparent_hugepage/defrag is 'always'.
2016-02-26T02:09:22.658-0300 I CONTROL  [initandlisten] **        We suggest setting it to 'never'
2016-02-26T02:09:22.658-0300 I CONTROL  [initandlisten] 
paulo-sti-ni-1401:27019(mongod-3.2.3)[PRIMARY:be-mean-mongo] test> 
```
