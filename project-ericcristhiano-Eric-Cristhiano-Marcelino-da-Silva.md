# MongoDb - Projeto Final
**Autor:** Eric Cristhiano Marcelino da Silva
**Data** 1454948127946

## Para qual sistema você usaria o MongoDB (diferente desse)?
Basicamente, é possível utilizar o MongoDB para os mais diversos tipos de sistemas, tais como redes sociais, CMS e ERP's... Além disso, o MongoDB é altamente recomendado para sistemas em que hajam altas cargas de escrita, pois o MongoDB foi construído para escalabilidade, performance e alta disponibilidade. Entretanto, como toda tecnologia, há vantagens e desvantagens, no MongoDB, por exemplo, pode citar-se a falta de transactions, como há em bancos relacionais.

## Qual a modelagem da sua coleção de `users`?

```js
user: {
    _id: ObjectID,
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
        hash_token: String,
    },
    background_path: String,
}
```

## Qual a modelagem da sua coleção de `projects`?

```js
project: {
    _id: ObjectID,
    name: String,
    description: Date,
    date_begin: Date,
    date_dream: Date,
    date_end: Date,
    visible: Boolean,
    realocate: Boolean,
    expired: Boolean,
    visualizable_mod: String,
    members: [{
        type_member: String,
        user_id: ObjectID,
        notify: Boolean,
    }],
    tags: [],
    goals: [{
        name: String,
        description: String,
        data_begin: Date,
        data_dream: Date,
        data_end: Date,
        data_realocate: Date,
        realocate: Boolean,
        expired: Boolean,
        tags: [],
        activity: [
            { activity_id: ObjectID }
        ],
    }],
}
```

## Qual a modelagem da sua coleção retirada de `projects`?
A collection `activity` foi retirada de projects por ser uma `collection` que pode crescer bastante, logo ela foi retirada para evitar que atinja o limite de tamanho máximo de documento.

```js
activity: {
    _id: ObjectID,
    name: String,
    description: String,
    date_begin: Date,
    date_dream: Date,
    date_end: Date,
    date_realocate: Date,
    realocate: Boolean,
    expired: Boolean,
    members: [{
        type_member: String,
        user_id: ObjectID,
        notify: Boolean,
    }],
    tags: [],
    comments: [{
        text: String,
        date_comment: Date,
        members: [],
        files: [{
            path: String,
            weight: Double,
            name: Date,
            members: [{
                type_member: String,
                user_id: ObjectID,
                notify: Boolean,
            }],
        }],
    }],
    historics: [],
}
```

## Create - cadastro

**1.** Cadastre 10 usuários diferentes.

```js
use projeto-final
switched to db projeto-final

var users = [
{
 name: 'Angelita L. Garcia',
 bio: 'Bio of Angelita',
 data_register: new Date(),
 avatar_path: 'img/avatares/b5e7d988cfdb78bc3be1a9c221a8f744.jpg',
 auth: {
     username: 'angelita',
     email: 'angelita@email.com',
     password: '7dfdbab994985aef32beeebf64fd66fe50233a2361266cf7d456b442c9d021fb',
     last_access: new Date(),
     online: true,
     disabled: false,
     hash_token: '9cI7nOZgHhobGtxh',
 },
 background_path: 'img/backgrounds/943359f44dc87f6a1679c987280e8dc7.jpg',
},

{
 name: 'Carolyn J. McLean',
 bio: 'Bio of Carolyn',
 data_register: new Date(),
 avatar_path: 'img/avatares/8c3fa8196ef3e2c81e88d4c24d6b7c06.jpg',
 auth: {
     username: 'carolyn',
     email: 'carolyn@email.com',
     password: 'a91980b205f2857e028c9c41b26924c48cb8be052c62224a6e244f82aac52b31',
     last_access: new Date(),
     online: true,
     disabled: false,
     hash_token: '1MErBJdDmvqHqc9x',
 },
 background_path: 'img/backgrounds/3bfa569280f763df385e8ba9b5502dd4.jpg',
},

{
 name: 'Kevin L. Patterson',
 bio: 'Bio of Kevin',
 data_register: new Date(),
 avatar_path: 'img/avatares/5f01b70e5fc11f9aa2abf83d466cb482.jpg',
 auth: {
     username: 'kevin',
     email: 'kevin@email.com',
     password: 'cee83618119362859f910670a281f118dbeba1ea736b4d12dcbb3a353f4e5df9',
     last_access: new Date(),
     online: true,
     disabled: false,
     hash_token: '4IaubIfQNqVj3sGL',
 },
 background_path: 'img/backgrounds/490f17474880c2c900a57a1c6932436d.jpg',
},

{
 name: 'Robert M. Armstrong',
 bio: 'Bio of Robert',
 data_register: new Date(),
 avatar_path: 'img/avatares/4e2c0b6a599694f4a364ac1056e3ce15.jpg',
 auth: {
     username: 'robert',
     email: 'robert@email.com',
     password: '4976e6bc4a6addb1ff9db00d414f48ab8a788ac939cb9c9c6fb403e4c0dc052a',
     last_access: new Date(),
     online: true,
     disabled: false,
     hash_token: '3UXSXQPo03fEdRnu',
 },
 background_path: 'img/backgrounds/55f21e8ebaf3479d1e14f653b1774843.jpg',
},

{
 name: 'Amy J. Hood',
 bio: 'Bio of Amy',
 data_register: new Date(),
 avatar_path: 'img/avatares/5f9d3d8ac309fc2105a615fd57a6e1c0.jpg',
 auth: {
     username: 'amy',
     email: 'amy@email.com',
     password: '8826e7fb43bc616f8cee6bb96578d6eaaa7561b30d99d53ab20023b082cfa44f',
     last_access: new Date(),
     online: true,
     disabled: false,
     hash_token: '2AKJZwoxzTdidhk2',
 },
 background_path: 'img/backgrounds/e0a5afb4889a5219cdef6b2c5eb5fd24.jpg',
},

{
 name: 'Lorene R. Gates',
 bio: 'Bio of Lorene',
 data_register: new Date(),
 avatar_path: 'img/avatares/44e002fceac5b190cb8a6e95aefcd5fd.jpg',
 auth: {
     username: 'lorene',
     email: 'lorene@email.com',
     password: '180ca79a28199b84aafa1aa25cde8225d40b1938497ea68a587404f90fb80682',
     last_access: new Date(),
     online: true,
     disabled: false,
     hash_token: '8a9uNJ8jFzMas7FQ',
 },
 background_path: 'img/backgrounds/06502cae73f3c50507665afddb73b92c.jpg',
},

{
 name: 'Karin G. Alberto',
 bio: 'Bio of Karin',
 data_register: new Date(),
 avatar_path: 'img/avatares/0f8de2ed0f889132d0e2decf3c3c8572.jpg',
 auth: {
     username: 'karin',
     email: 'karin@email.com',
     password: '070a0a03c421f1614bc3edbae42d0c5480089485dc069eb62e275b041de34c2e',
     last_access: new Date(),
     online: true,
     disabled: false,
     hash_token: '9cI7nOZgHhobGtxh',
 },
 background_path: 'img/backgrounds/08a5dbeda246094be00a87cb6cdd18e2.jpg',
},

{
 name: 'William F. King',
 bio: 'Bio of William',
 data_register: new Date(),
 avatar_path: 'img/avatares/b047fffc6cf87e1dda4e3f7d081b5132.jpg',
 auth: {
     username: 'william',
     email: 'william@email.com',
     password: '59cb409dd7b15cb1971b3120a6a2a2957b17472ca8989d9e52a15367418e4d41',
     last_access: new Date(),
     online: true,
     disabled: false,
     hash_token: '0cLgDRuMVIDxUE9A',
 },
 background_path: 'img/backgrounds/0c936eb83a267c17b545b3e3422420c3.jpg',
},

{
 name: 'Robert M. Bell',
 bio: 'Bio of Robert',
 data_register: new Date(),
 avatar_path: 'img/avatares/a733473e4b6d70648e6d13a72e9e4b03.jpg',
 auth: {
     username: 'robert',
     email: 'robert@email.com',
     password: '9e0e6225cf7f46829a5b964512dcb16f23331068c1a71989815271d1721e45c6',
     last_access: new Date(),
     online: true,
     disabled: false,
     hash_token: '3Th7a3HvKfAvSE8n',
 },
 background_path: 'img/backgrounds/493cb4724ee237797c8e05451e9a2d81.jpg',
},

{
 name: 'James C. Acosta',
 bio: 'Bio of James',
 data_register: new Date(),
 avatar_path: 'img/avatares/d32078b8267cedd2b08cf5c528a1c150.jpg',
 auth: {
     username: 'james',
     email: 'james@email.com',
     password: 'e2ea71802a6c750adc7180f6083bf9123214c3172eb05532efa897167557e783',
     last_access: new Date(),
     online: true,
     disabled: false,
     hash_token: '3mNehBgKPYtamYjh',
 },
 background_path: 'img/backgrounds/83659def42fb1e7291d3091e8533550a.jpg',
}
]

users.forEach(function(user) {
 db.users.insert(user);
})

Inserted 1 record(s) in 698ms
Inserted 1 record(s) in 1ms
Inserted 1 record(s) in 1ms
Inserted 1 record(s) in 1ms
Inserted 1 record(s) in 2ms
Inserted 1 record(s) in 1ms
Inserted 1 record(s) in 1ms
Inserted 1 record(s) in 1ms
Inserted 1 record(s) in 2ms
Inserted 1 record(s) in 1ms
```

**2.** Cadastre 5 projetos diferentes.

```js
var users = db.users.find({}).toArray();

var activities = [
{
    name: 'Activity one',
    description: 'Description of the first activity',
    date_begin: new Date(),
    date_dream: new Date(),
    date_end: new Date(),
    date_realocate: new Date(),
    realocate: true,
    expired: false,
    members: [{
        type_member: 'Programmer',
        user_id: users[0]._id,
        notify: false,
    }],
    tags: [],
    comments: [],
    historics: [],
},
{
    name: 'Activity two',
    description: 'Description of the second activity',
    date_begin: new Date(),
    date_dream: new Date(),
    date_end: new Date(),
    date_realocate: new Date(),
    realocate: true,
    expired: false,
    members: [{
        type_member: 'Programmer',
        user_id: users[1]._id,
        notify: false,
    }],
    tags: [],
    comments: [],
    historics: [],
},
{
    name: 'Activity three',
    description: 'Description of the third activity',
    date_begin: new Date(),
    date_dream: new Date(),
    date_end: new Date(),
    date_realocate: new Date(),
    realocate: true,
    expired: false,
    members: [{
        type_member: 'Programmer',
        user_id: users[2]._id,
        notify: false,
    }],
    tags: [],
    comments: [],
    historics: [],
},
{
    name: 'Activity four',
    description: 'Description of the fourth activity',
    date_begin: new Date(),
    date_dream: new Date(),
    date_end: new Date(),
    date_realocate: new Date(),
    realocate: true,
    expired: false,
    members: [{
        type_member: 'Programmer',
        user_id: users[3]._id,
        notify: false,
    }],
    tags: [],
    comments: [],
    historics: [],
},
{
    name: 'Activity five',
    description: 'Description of the fifth activity',
    date_begin: new Date(),
    date_dream: new Date(),
    date_end: new Date(),
    date_realocate: new Date(),
    realocate: true,
    expired: false,
    members: [{
        type_member: 'Programmer',
        user_id: users[4]._id,
        notify: false,
    }],
    tags: [],
    comments: [],
    historics: [],
}
]

activities.forEach(function(activity) {
     db.activities.insert(activity);
})

Inserted 1 record(s) in 8ms
Inserted 1 record(s) in 1ms
Inserted 1 record(s) in 2ms
Inserted 1 record(s) in 1ms
Inserted 1 record(s) in 2ms

var activities = db.activities.find({}).toArray();

var projects = [{
    name: 'Project number one',
    description: new Date(),
    date_begin: new Date(),
    date_dream: new Date(),
    date_end: new Date(),
    visible: true,
    realocate: false,
    expired: false,
    visualizable_mod: 1,

    members: [
        {
            type_member: 'Programmer',
            user_id: users[0]._id,
            notify: false,
        },
        {
            type_member: 'Programmer',
            user_id: users[1]._id,
            notify: false,
        },
        {
            type_member: 'Programmer',
            user_id: users[2]._id,
            notify: false,
        },
        {
            type_member: 'Programmer',
            user_id: users[3]._id,
            notify: false,
        },
        {
            type_member: 'Programmer',
            user_id: users[4]._id,
            notify: false,
        },
    ],

    tags: ['Javascript', 'HTML', 'CSS'],

    goals: [
        {
            name: 'First goal',
            description: 'Description of the first goal',
            data_begin: new Date(),
            data_dream: new Date(),
            data_end: new Date(),
            data_realocate: new Date(),
            realocate: false,
            expired: false,
            tags: ['First', 'Goal 1', 'Start'],
            activity: [
                { activity_id: activities[0]._id },
                { activity_id: activities[1]._id },
            ],
        },
    ],
},
{
    name: 'Project number two',
    description: new Date(),
    date_begin: new Date(),
    date_dream: new Date(),
    date_end: new Date(),
    visible: true,
    realocate: false,
    expired: false,
    visualizable_mod: 1,

    members: [
        {
            type_member: 'Programmer',
            user_id: users[1]._id,
            notify: false,
        },
        {
            type_member: 'Programmer',
            user_id: users[2]._id,
            notify: false,
        },
        {
            type_member: 'Programmer',
            user_id: users[3]._id,
            notify: false,
        },
        {
            type_member: 'Programmer',
            user_id: users[4]._id,
            notify: false,
        },
        {
            type_member: 'Programmer',
            user_id: users[5]._id,
            notify: false,
        },
    ],

    tags: ['PHP', 'Java', 'Ruby'],

    goals: [
        {
            name: 'Second goal',
            description: 'Description of the second goal',
            data_begin: new Date(),
            data_dream: new Date(),
            data_end: new Date(),
            data_realocate: new Date(),
            realocate: false,
            expired: false,
            tags: ['Second', 'Goal 2', 'Start'],
            activity: [
                { activity_id: activities[2]._id },
                { activity_id: activities[3]._id },
            ],
        },
    ],
},
{
    name: 'Project number three',
    description: new Date(),
    date_begin: new Date(),
    date_dream: new Date(),
    date_end: new Date(),
    visible: true,
    realocate: false,
    expired: false,
    visualizable_mod: 1,

    members: [
        {
            type_member: 'Programmer',
            user_id: users[2]._id,
            notify: false,
        },
        {
            type_member: 'Programmer',
            user_id: users[3]._id,
            notify: false,
        },
        {
            type_member: 'Programmer',
            user_id: users[4]._id,
            notify: false,
        },
        {
            type_member: 'Programmer',
            user_id: users[5]._id,
            notify: false,
        },
        {
            type_member: 'Programmer',
            user_id: users[6]._id,
            notify: false,
        },
    ],

    tags: ['Javascript', 'HTML', 'CSS'],

    goals: [
        {
            name: 'First goal',
            description: 'Description of the first goal',
            data_begin: new Date(),
            data_dream: new Date(),
            data_end: new Date(),
            data_realocate: new Date(),
            realocate: false,
            expired: false,
            tags: ['First', 'Goal 1', 'Start'],
            activity: [
                { activity_id: activities[0]._id },
                { activity_id: activities[1]._id },
            ],
        },
    ],
},
{
    name: 'Project number four',
    description: new Date(),
    date_begin: new Date(),
    date_dream: new Date(),
    date_end: new Date(),
    visible: true,
    realocate: false,
    expired: false,
    visualizable_mod: 1,

    members: [
        {
            type_member: 'Programmer',
            user_id: users[3]._id,
            notify: false,
        },
        {
            type_member: 'Programmer',
            user_id: users[4]._id,
            notify: false,
        },
        {
            type_member: 'Programmer',
            user_id: users[5]._id,
            notify: false,
        },
        {
            type_member: 'Programmer',
            user_id: users[6]._id,
            notify: false,
        },
        {
            type_member: 'Programmer',
            user_id: users[7]._id,
            notify: false,
        },
    ],

    tags: ['PHP', 'Java', 'Ruby'],

    goals: [
        {
            name: 'First goal',
            description: 'Description of the first goal',
            data_begin: new Date(),
            data_dream: new Date(),
            data_end: new Date(),
            data_realocate: new Date(),
            realocate: false,
            expired: false,
            tags: ['First', 'Goal 1', 'Start'],
            activity: [],
        },
    ],
},
{
    name: 'Project number five',
    description: new Date(),
    date_begin: new Date(),
    date_dream: new Date(),
    date_end: new Date(),
    visible: true,
    realocate: false,
    expired: false,
    visualizable_mod: 1,

    members: [
        {
            type_member: 'Programmer',
            user_id: users[4]._id,
            notify: false,
        },
        {
            type_member: 'Programmer',
            user_id: users[5]._id,
            notify: false,
        },
        {
            type_member: 'Programmer',
            user_id: users[6]._id,
            notify: false,
        },
        {
            type_member: 'Programmer',
            user_id: users[7]._id,
            notify: false,
        },
        {
            type_member: 'Programmer',
            user_id: users[8]._id,
            notify: false,
        },
    ],

    tags: ['Javascript', 'HTML', 'CSS'],

    goals: [
        {
            name: 'First goal',
            description: 'Description of the first goal',
            data_begin: new Date(),
            data_dream: new Date(),
            data_end: new Date(),
            data_realocate: new Date(),
            realocate: false,
            expired: false,
            tags: ['First', 'Goal 1', 'Start'],
            activity: [
                { activity_id: activities[3]._id },
                { activity_id: activities[1]._id },
            ],
        },
    ],
}]

projects.forEach(function(project) {
     db.projects.insert(project);
})
```

## Retrieve - busca

**1.** Liste as informações dos membros de 1 projeto específico que deve ser buscado pelo seu nome de forma a não ligar para maiúsculas e minúsculas.

```js
var project_members = db.projects.findOne({name: /project number one/i}, {members: 1, _id: 0})

members_info = [];

project_members.members.forEach(function(member) {
    members_info.push(db.users.findOne({ _id: member.user_id }));
});

members_info;

[
  {
    "_id": ObjectId("56ab7c33a204194927aa6d32"),
    "name": "Angelita L. Garcia",
    "bio": "Bio of Angelita",
    "data_register": ISODate("2016-01-29T14:50:19.273Z"),
    "avatar_path": "img/avatares/angelita.jpg",
    "auth": {
      "username": "angelita",
      "email": "angelita@email.com",
      "password": "4567912348",
      "last_access": ISODate("2016-01-29T14:50:19.273Z"),
      "online": true,
      "disabled": false,
      "hash_token": "9cI7nOZgHhobGtxh"
    },
    "background_path": "img/backgrounds/angelita.jpg"
  },
  {
    "_id": ObjectId("56ab7c33a204194927aa6d33"),
    "name": "Carolyn J. McLean",
    "bio": "Bio of Carolyn",
    "data_register": ISODate("2016-01-29T14:50:19.273Z"),
    "avatar_path": "img/avatares/carolyn.jpg",
    "auth": {
      "username": "carolyn",
      "email": "carolyn@email.com",
      "password": "9887844",
      "last_access": ISODate("2016-01-29T14:50:19.273Z"),
      "online": true,
      "disabled": false,
      "hash_token": "1MErBJdDmvqHqc9x"
    },
    "background_path": "img/backgrounds/carolyn.jpg"
  },
  {
    "_id": ObjectId("56ab7c33a204194927aa6d34"),
    "name": "Kevin L. Patterson",
    "bio": "Bio of Kevin",
    "data_register": ISODate("2016-01-29T14:50:19.273Z"),
    "avatar_path": "img/avatares/kevin.jpg",
    "auth": {
      "username": "kevin",
      "email": "kevin@email.com",
      "password": "9874564",
      "last_access": ISODate("2016-01-29T14:50:19.273Z"),
      "online": true,
      "disabled": false,
      "hash_token": "4IaubIfQNqVj3sGL"
    },
    "background_path": "img/backgrounds/kevin.jpg"
  },
  {
    "_id": ObjectId("56ab7c33a204194927aa6d35"),
    "name": "Robert M. Armstrong",
    "bio": "Bio of Robert",
    "data_register": ISODate("2016-01-29T14:50:19.273Z"),
    "avatar_path": "img/avatares/robert.jpg",
    "auth": {
      "username": "robert",
      "email": "robert@email.com",
      "password": "4567912348",
      "last_access": ISODate("2016-01-29T14:50:19.273Z"),
      "online": true,
      "disabled": false,
      "hash_token": "3UXSXQPo03fEdRnu"
    },
    "background_path": "img/backgrounds/robert.jpg"
  },
  {
    "_id": ObjectId("56ab7c33a204194927aa6d36"),
    "name": "Amy J. Hood",
    "bio": "Bio of Amy",
    "data_register": ISODate("2016-01-29T14:50:19.273Z"),
    "avatar_path": "img/avatares/amy.jpg",
    "auth": {
      "username": "amy",
      "email": "amy@email.com",
      "password": "852254657",
      "last_access": ISODate("2016-01-29T14:50:19.273Z"),
      "online": true,
      "disabled": false,
      "hash_token": "2AKJZwoxzTdidhk2"
    },
    "background_path": "img/backgrounds/amy.jpg"
  }
]
```

**2.** Liste todos os projetos com a tag que você escolheu para os 3 projetos em comum.

```js
db.projects.find({ tags : { $in : [/Javascript/i] }  })
{
  "_id": ObjectId("56ace29972a09b52fdfb0123"),
  "name": "Project number one",
  "description": ISODate("2016-01-30T16:19:32.309Z"),
  "date_begin": ISODate("2016-01-30T16:19:32.309Z"),
  "date_dream": ISODate("2016-01-30T16:19:32.309Z"),
  "date_end": ISODate("2016-01-30T16:19:32.309Z"),
  "visible": true,
  "realocate": false,
  "expired": false,
  "visualizable_mod": 1,
  "members": [
    {
      "type_member": "Programmer",
      "user_id": ObjectId("56ace25272a09b52fdfb0114"),
      "notify": false
    },
    {
      "type_member": "Programmer",
      "user_id": ObjectId("56ace25372a09b52fdfb0115"),
      "notify": false
    },
    {
      "type_member": "Programmer",
      "user_id": ObjectId("56ace25372a09b52fdfb0116"),
      "notify": false
    },
    {
      "type_member": "Programmer",
      "user_id": ObjectId("56ace25372a09b52fdfb0117"),
      "notify": false
    },
    {
      "type_member": "Programmer",
      "user_id": ObjectId("56ace25372a09b52fdfb0118"),
      "notify": false
    }
  ],
  "tags": [
    "Javascript",
    "HTML",
    "CSS"
  ],
  "goals": [
    {
      "name": "First goal",
      "description": "Description of the first goal",
      "data_begin": ISODate("2016-01-30T16:19:32.309Z"),
      "data_dream": ISODate("2016-01-30T16:19:32.309Z"),
      "data_end": ISODate("2016-01-30T16:19:32.309Z"),
      "data_realocate": ISODate("2016-01-30T16:19:32.309Z"),
      "realocate": false,
      "expired": false,
      "tags": [
        "First",
        "Goal 1",
        "Start"
      ],
      "activity": [
        {
          "activity_id": ObjectId("56ace26b72a09b52fdfb011e")
        },
        {
          "activity_id": ObjectId("56ace26b72a09b52fdfb011f")
        }
      ]
    }
  ]
}
{
  "_id": ObjectId("56ace29972a09b52fdfb0125"),
  "name": "Project number three",
  "description": ISODate("2016-01-30T16:19:32.309Z"),
  "date_begin": ISODate("2016-01-30T16:19:32.309Z"),
  "date_dream": ISODate("2016-01-30T16:19:32.309Z"),
  "date_end": ISODate("2016-01-30T16:19:32.309Z"),
  "visible": true,
  "realocate": false,
  "expired": false,
  "visualizable_mod": 1,
  "members": [
    {
      "type_member": "Programmer",
      "user_id": ObjectId("56ace25372a09b52fdfb0116"),
      "notify": false
    },
    {
      "type_member": "Programmer",
      "user_id": ObjectId("56ace25372a09b52fdfb0117"),
      "notify": false
    },
    {
      "type_member": "Programmer",
      "user_id": ObjectId("56ace25372a09b52fdfb0118"),
      "notify": false
    },
    {
      "type_member": "Programmer",
      "user_id": ObjectId("56ace25372a09b52fdfb0119"),
      "notify": false
    },
    {
      "type_member": "Programmer",
      "user_id": ObjectId("56ace25372a09b52fdfb011a"),
      "notify": false
    }
  ],
  "tags": [
    "Javascript",
    "HTML",
    "CSS"
  ],
  "goals": [
    {
      "name": "First goal",
      "description": "Description of the first goal",
      "data_begin": ISODate("2016-01-30T16:19:32.309Z"),
      "data_dream": ISODate("2016-01-30T16:19:32.309Z"),
      "data_end": ISODate("2016-01-30T16:19:32.309Z"),
      "data_realocate": ISODate("2016-01-30T16:19:32.309Z"),
      "realocate": false,
      "expired": false,
      "tags": [
        "First",
        "Goal 1",
        "Start"
      ],
      "activity": [
        {
          "activity_id": ObjectId("56ace26b72a09b52fdfb011e")
        },
        {
          "activity_id": ObjectId("56ace26b72a09b52fdfb011f")
        }
      ]
    }
  ]
}
{
  "_id": ObjectId("56ace29972a09b52fdfb0127"),
  "name": "Project number five",
  "description": ISODate("2016-01-30T16:19:32.309Z"),
  "date_begin": ISODate("2016-01-30T16:19:32.309Z"),
  "date_dream": ISODate("2016-01-30T16:19:32.309Z"),
  "date_end": ISODate("2016-01-30T16:19:32.309Z"),
  "visible": true,
  "realocate": false,
  "expired": false,
  "visualizable_mod": 1,
  "members": [
    {
      "type_member": "Programmer",
      "user_id": ObjectId("56ace25372a09b52fdfb0118"),
      "notify": false
    },
    {
      "type_member": "Programmer",
      "user_id": ObjectId("56ace25372a09b52fdfb0119"),
      "notify": false
    },
    {
      "type_member": "Programmer",
      "user_id": ObjectId("56ace25372a09b52fdfb011a"),
      "notify": false
    },
    {
      "type_member": "Programmer",
      "user_id": ObjectId("56ace25372a09b52fdfb011b"),
      "notify": false
    },
    {
      "type_member": "Programmer",
      "user_id": ObjectId("56ace25372a09b52fdfb011c"),
      "notify": false
    }
  ],
  "tags": [
    "Javascript",
    "HTML",
    "CSS"
  ],
  "goals": [
    {
      "name": "First goal",
      "description": "Description of the first goal",
      "data_begin": ISODate("2016-01-30T16:19:32.309Z"),
      "data_dream": ISODate("2016-01-30T16:19:32.309Z"),
      "data_end": ISODate("2016-01-30T16:19:32.309Z"),
      "data_realocate": ISODate("2016-01-30T16:19:32.309Z"),
      "realocate": false,
      "expired": false,
      "tags": [
        "First",
        "Goal 1",
        "Start"
      ],
      "activity": [
        {
          "activity_id": ObjectId("56ace26b72a09b52fdfb0121")
        },
        {
          "activity_id": ObjectId("56ace26b72a09b52fdfb0122")
        }
      ]
    }
  ]
}
Fetched 3 record(s) in 14ms

```

**3.** Liste apenas os nomes de todas as atividades para todos os projetos.

```js
var activities = [];
var activitiesId = [];
var projects = db.projects.find({}).toArray();
var getActivity = function(objectId) {
    return db.activities.findOne({ _id : objectId }, {_id : 0, name: 1});
}

projects.forEach(function(project) {
    project.goals.forEach(function(goal) {
        goal.activity.forEach(function(activity) {
            if (activitiesId.indexOf(activity.activity_id.valueOf()) < 0) {
                activitiesId.push(activity.activity_id.valueOf());
            }
        });
    });
});

activitiesId.forEach(function(id) {
    activities.push(getActivity(ObjectId(id)))
});

activities

[
  {
    "name": "Activity one"
  },
  {
    "name": "Activity two"
  },
  {
    "name": "Activity three"
  },
  {
    "name": "Activity four"
  }
]
```

**4.** Liste todos os projetos que não possuam uma tag.

```js
db.projects.find({ tags : { $not : { $in : [/PHP/i] }  } })
{
  "_id": ObjectId("56ace29972a09b52fdfb0123"),
  "name": "Project number one",
  "description": ISODate("2016-01-30T16:19:32.309Z"),
  "date_begin": ISODate("2016-01-30T16:19:32.309Z"),
  "date_dream": ISODate("2016-01-30T16:19:32.309Z"),
  "date_end": ISODate("2016-01-30T16:19:32.309Z"),
  "visible": true,
  "realocate": false,
  "expired": false,
  "visualizable_mod": 1,
  "members": [
    {
      "type_member": "Programmer",
      "user_id": ObjectId("56ace25272a09b52fdfb0114"),
      "notify": false
    },
    {
      "type_member": "Programmer",
      "user_id": ObjectId("56ace25372a09b52fdfb0115"),
      "notify": false
    },
    {
      "type_member": "Programmer",
      "user_id": ObjectId("56ace25372a09b52fdfb0116"),
      "notify": false
    },
    {
      "type_member": "Programmer",
      "user_id": ObjectId("56ace25372a09b52fdfb0117"),
      "notify": false
    },
    {
      "type_member": "Programmer",
      "user_id": ObjectId("56ace25372a09b52fdfb0118"),
      "notify": false
    }
  ],
  "tags": [
    "Javascript",
    "HTML",
    "CSS"
  ],
  "goals": [
    {
      "name": "First goal",
      "description": "Description of the first goal",
      "data_begin": ISODate("2016-01-30T16:19:32.309Z"),
      "data_dream": ISODate("2016-01-30T16:19:32.309Z"),
      "data_end": ISODate("2016-01-30T16:19:32.309Z"),
      "data_realocate": ISODate("2016-01-30T16:19:32.309Z"),
      "realocate": false,
      "expired": false,
      "tags": [
        "First",
        "Goal 1",
        "Start"
      ],
      "activity": [
        {
          "activity_id": ObjectId("56ace26b72a09b52fdfb011e")
        },
        {
          "activity_id": ObjectId("56ace26b72a09b52fdfb011f")
        }
      ]
    }
  ]
}
{
  "_id": ObjectId("56ace29972a09b52fdfb0125"),
  "name": "Project number three",
  "description": ISODate("2016-01-30T16:19:32.309Z"),
  "date_begin": ISODate("2016-01-30T16:19:32.309Z"),
  "date_dream": ISODate("2016-01-30T16:19:32.309Z"),
  "date_end": ISODate("2016-01-30T16:19:32.309Z"),
  "visible": true,
  "realocate": false,
  "expired": false,
  "visualizable_mod": 1,
  "members": [
    {
      "type_member": "Programmer",
      "user_id": ObjectId("56ace25372a09b52fdfb0116"),
      "notify": false
    },
    {
      "type_member": "Programmer",
      "user_id": ObjectId("56ace25372a09b52fdfb0117"),
      "notify": false
    },
    {
      "type_member": "Programmer",
      "user_id": ObjectId("56ace25372a09b52fdfb0118"),
      "notify": false
    },
    {
      "type_member": "Programmer",
      "user_id": ObjectId("56ace25372a09b52fdfb0119"),
      "notify": false
    },
    {
      "type_member": "Programmer",
      "user_id": ObjectId("56ace25372a09b52fdfb011a"),
      "notify": false
    }
  ],
  "tags": [
    "Javascript",
    "HTML",
    "CSS"
  ],
  "goals": [
    {
      "name": "First goal",
      "description": "Description of the first goal",
      "data_begin": ISODate("2016-01-30T16:19:32.309Z"),
      "data_dream": ISODate("2016-01-30T16:19:32.309Z"),
      "data_end": ISODate("2016-01-30T16:19:32.309Z"),
      "data_realocate": ISODate("2016-01-30T16:19:32.309Z"),
      "realocate": false,
      "expired": false,
      "tags": [
        "First",
        "Goal 1",
        "Start"
      ],
      "activity": [
        {
          "activity_id": ObjectId("56ace26b72a09b52fdfb011e")
        },
        {
          "activity_id": ObjectId("56ace26b72a09b52fdfb011f")
        }
      ]
    }
  ]
}
{
  "_id": ObjectId("56ace29972a09b52fdfb0127"),
  "name": "Project number five",
  "description": ISODate("2016-01-30T16:19:32.309Z"),
  "date_begin": ISODate("2016-01-30T16:19:32.309Z"),
  "date_dream": ISODate("2016-01-30T16:19:32.309Z"),
  "date_end": ISODate("2016-01-30T16:19:32.309Z"),
  "visible": true,
  "realocate": false,
  "expired": false,
  "visualizable_mod": 1,
  "members": [
    {
      "type_member": "Programmer",
      "user_id": ObjectId("56ace25372a09b52fdfb0118"),
      "notify": false
    },
    {
      "type_member": "Programmer",
      "user_id": ObjectId("56ace25372a09b52fdfb0119"),
      "notify": false
    },
    {
      "type_member": "Programmer",
      "user_id": ObjectId("56ace25372a09b52fdfb011a"),
      "notify": false
    },
    {
      "type_member": "Programmer",
      "user_id": ObjectId("56ace25372a09b52fdfb011b"),
      "notify": false
    },
    {
      "type_member": "Programmer",
      "user_id": ObjectId("56ace25372a09b52fdfb011c"),
      "notify": false
    }
  ],
  "tags": [
    "Javascript",
    "HTML",
    "CSS"
  ],
  "goals": [
    {
      "name": "First goal",
      "description": "Description of the first goal",
      "data_begin": ISODate("2016-01-30T16:19:32.309Z"),
      "data_dream": ISODate("2016-01-30T16:19:32.309Z"),
      "data_end": ISODate("2016-01-30T16:19:32.309Z"),
      "data_realocate": ISODate("2016-01-30T16:19:32.309Z"),
      "realocate": false,
      "expired": false,
      "tags": [
        "First",
        "Goal 1",
        "Start"
      ],
      "activity": [
        {
          "activity_id": ObjectId("56ace26b72a09b52fdfb0121")
        },
        {
          "activity_id": ObjectId("56ace26b72a09b52fdfb0122")
        }
      ]
    }
  ]
}
```

**5.** Liste todos os usuários que não fazem parte do primeiro projeto cadastrado.

```js
var firstProject = db.projects.findOne();

var members = [];

firstProject.members.forEach(function(member) {
    members.push(member.user_id);
});

db.users.find({ _id : { $not : { $in : members } }})
{
  "_id": ObjectId("56ace25372a09b52fdfb0119"),
  "name": "Lorene R. Gates",
  "bio": "Bio of Lorene",
  "data_register": ISODate("2016-01-30T16:18:19.937Z"),
  "avatar_path": "img/avatares/lorene.jpg",
  "auth": {
    "username": "lorene",
    "email": "lorene@email.com",
    "password": "23576767",
    "last_access": ISODate("2016-01-30T16:18:19.937Z"),
    "online": true,
    "disabled": false,
    "hash_token": "8a9uNJ8jFzMas7FQ"
  },
  "background_path": "img/backgrounds/lorene.jpg"
}
{
  "_id": ObjectId("56ace25372a09b52fdfb011a"),
  "name": "Karin G. Alberto",
  "bio": "Bio of Karin",
  "data_register": ISODate("2016-01-30T16:18:19.937Z"),
  "avatar_path": "img/avatares/karin.jpg",
  "auth": {
    "username": "karin",
    "email": "karin@email.com",
    "password": "25467687",
    "last_access": ISODate("2016-01-30T16:18:19.937Z"),
    "online": true,
    "disabled": false,
    "hash_token": "9cI7nOZgHhobGtxh"
  },
  "background_path": "img/backgrounds/karin.jpg"
}
{
  "_id": ObjectId("56ace25372a09b52fdfb011b"),
  "name": "William F. King",
  "bio": "Bio of William",
  "data_register": ISODate("2016-01-30T16:18:19.937Z"),
  "avatar_path": "img/avatares/william.jpg",
  "auth": {
    "username": "william",
    "email": "william@email.com",
    "password": "587456782004",
    "last_access": ISODate("2016-01-30T16:18:19.937Z"),
    "online": true,
    "disabled": false,
    "hash_token": "0cLgDRuMVIDxUE9A"
  },
  "background_path": "img/backgrounds/william.jpg"
}
{
  "_id": ObjectId("56ace25372a09b52fdfb011c"),
  "name": "Robert M. Bell",
  "bio": "Bio of Robert",
  "data_register": ISODate("2016-01-30T16:18:19.937Z"),
  "avatar_path": "img/avatares/robert.jpg",
  "auth": {
    "username": "robert",
    "email": "robert@email.com",
    "password": "4563546750",
    "last_access": ISODate("2016-01-30T16:18:19.937Z"),
    "online": true,
    "disabled": false,
    "hash_token": "3Th7a3HvKfAvSE8n"
  },
  "background_path": "img/backgrounds/robert.jpg"
}
{
  "_id": ObjectId("56ace25372a09b52fdfb011d"),
  "name": "James C. Acosta",
  "bio": "Bio of James",
  "data_register": ISODate("2016-01-30T16:18:19.937Z"),
  "avatar_path": "img/avatares/james.jpg",
  "auth": {
    "username": "james",
    "email": "james@email.com",
    "password": "85764564110",
    "last_access": ISODate("2016-01-30T16:18:19.937Z"),
    "online": true,
    "disabled": false,
    "hash_token": "3mNehBgKPYtamYjh"
  },
  "background_path": "img/backgrounds/james.jpg"
}
Fetched 5 record(s) in 13ms
```

## Update - alteração

**1.** Adicione para todos os projetos o campo `views: 0`.
```js
var query = {}
var mod = {$set : {views: 0}}
var options = {multi: true}
db.projects.update(query, mod, options)

Updated 5 existing record(s) in 89ms
WriteResult({
  "nMatched": 5,
  "nUpserted": 0,
  "nModified": 5
})
```

**2.** Adicione 1 tag diferente para cada projeto.

```js
var projects = db.projects.find({});
var x = 0;
projects.forEach(function(project) {
    db.projects.update({_id: project._id}, {$push: {tags: 'new tag' + x}})
    x++;
})

Updated 1 existing record(s) in 7ms
Updated 1 existing record(s) in 1ms
Updated 1 existing record(s) in 1ms
Updated 1 existing record(s) in 1ms
```

**3.** Adicione 2 membros diferentes para cada projeto.

```js
var projects = db.projects.find({});
var users = db.users.find({}).toArray();

var query = {"_id": projects[0]._id};
var insertUsers = [{'user_id': users[5]._id, 'notifiy': false, 'type_member' : 'Programmer'}, {'user_id': users[6]._id, 'notifiy': false, 'type_member' : 'Programmer'}];
var mod = {$pushAll: {members: insertUsers}}
db.projects.update(query, mod);

Updated 1 existing record(s) in 2ms
WriteResult({
  "nMatched": 1,
  "nUpserted": 0,
  "nModified": 1
})

var query = {"_id": projects[1]._id};
var insertUsers = [{'user_id': users[6]._id, 'notifiy': false, 'type_member' : 'Programmer'}, {'user_id': users[7]._id, 'notifiy': false, 'type_member' : 'Programmer'}];
var mod = {$pushAll: {members: insertUsers}}
db.projects.update(query, mod);

Updated 1 existing record(s) in 2ms
WriteResult({
  "nMatched": 1,
  "nUpserted": 0,
  "nModified": 1
})

var query = {"_id": projects[2]._id};
var insertUsers = [{'user_id': users[7]._id, 'notifiy': false, 'type_member' : 'Programmer'}, {'user_id': users[8]._id, 'notifiy': false, 'type_member' : 'Programmer'}];
var mod = {$pushAll: {members: insertUsers}}
db.projects.update(query, mod);

Updated 1 existing record(s) in 2ms
WriteResult({
  "nMatched": 1,
  "nUpserted": 0,
  "nModified": 1
})

var query = {"_id": projects[3]._id};
var insertUsers = [{'user_id': users[8]._id, 'notifiy': false, 'type_member' : 'Programmer'}, {'user_id': users[9]._id, 'notifiy': false, 'type_member' : 'Programmer'}];
var mod = {$pushAll: {members: insertUsers}}
db.projects.update(query, mod);

Updated 1 existing record(s) in 2ms
WriteResult({
  "nMatched": 1,
  "nUpserted": 0,
  "nModified": 1
})

var query = {"_id": projects[4]._id};
var insertUsers = [{'user_id': users[9]._id, 'notifiy': false, 'type_member' : 'Programmer'}, {'user_id': users[0]._id, 'notifiy': false, 'type_member' : 'Programmer'}];
var mod = {$pushAll: {members: insertUsers}}
db.projects.update(query, mod);

Updated 1 existing record(s) in 2ms
WriteResult({
  "nMatched": 1,
  "nUpserted": 0,
  "nModified": 1
})

```

**4.** Adicione 1 comentário em cada atividade, deixe apenas 1 projeto sem.

```js
var users = db.users.find({}).toArray();

var activities = db.activities.find({
    $and : [
        {'_id': {$ne: ObjectId("56ace26b72a09b52fdfb011e")}},
        {'_id': {$ne: ObjectId("56ace26b72a09b52fdfb011f")}},
    ]
});

activities.forEach(function(activity) {
    var comment = {
                    text: 'New comment',
                    date_comment: new Date(),
                    members: [
                        { 'user_id': users[0]._id }
                    ],
                    files: [],
                };

    db.activities.update({_id: activity._id}, {$push : {comments: comment}});
});

Updated 1 existing record(s) in 2ms
Updated 1 existing record(s) in 1ms
Updated 1 existing record(s) in 1ms
Updated 1 existing record(s) in 1ms
Updated 1 existing record(s) in 1ms
```

**5.** Adicione 1 projeto inteiro com **UPSERT**.

```js
var users = db.users.find({}).toArray();
var activities = db.activities.find({}).toArray();

var query = { name: /new project/i }

var mod = {
    $set: {active: true},
    $setOnInsert: {
        name: 'New Project',
        description: new Date(),
        date_begin: new Date(),
        date_dream: new Date(),
        date_end: new Date(),
        visible: true,
        realocate: false,
        expired: false,
        visualizable_mod: 1,

        members: [
            {
                type_member: 'Programmer',
                user_id: users[0]._id,
                notify: false,
            },
            {
                type_member: 'Programmer',
                user_id: users[1]._id,
                notify: false,
            },
            {
                type_member: 'Programmer',
                user_id: users[2]._id,
                notify: false,
            },
            {
                type_member: 'Programmer',
                user_id: users[3]._id,
                notify: false,
            },
            {
                type_member: 'Programmer',
                user_id: users[4]._id,
                notify: false,
            },
        ],

        tags: ['Javascript', 'HTML', 'CSS'],

        goals: [
            {
                name: 'First goal',
                description: 'Description of the first goal',
                data_begin: new Date(),
                data_dream: new Date(),
                data_end: new Date(),
                data_realocate: new Date(),
                realocate: false,
                expired: false,
                tags: ['First', 'Goal 1', 'Start'],
                activity: [
                    { activity_id: activities[0]._id },
                    { activity_id: activities[1]._id },
                ],
            },
        ],
    }
}

var options = {upsert: true}

db.projects.update(query, mod, options)

Updated 1 new record(s) in 6ms
WriteResult({
  "nMatched": 0,
  "nUpserted": 1,
  "nModified": 0,
  "_id": ObjectId("56b5f25022d43d23d8dfdb53")
})

```

## Delete - remoção


**1.** Apague todos os projetos que não possuam *tags*.

```js
db.projects.remove({tags: {$size: 0}}, {multi: 1})
Removed 0 record(s) in 38ms
WriteResult({
  "nRemoved": 0
})
```

**2.** Apague todos os projetos que não possuam comentários nas atividades.

```js
var activities = db.activities.find({ $or: [ { comments: { $eq : [] } }, { comments: { $exists: 0 } } ] }).toArray();

activities.forEach(function(activity) {
    db.projects.remove({'goals.activity.activity_id' : { $eq : activity._id } }, { multi: 1 });
});
```

**3.** Apague todos os projetos que não possuam atividades.

```js
db.projects.remove({ 'goal.activity' : {$size: 0} });
Removed 0 record(s) in 4ms
WriteResult({
  "nRemoved": 0
})
```

**4.** Escolha 2 usuário e apague todos os projetos em que os 2 fazem parte.

```js
var users = db.users.find({}, {_id: 1}).limit(2);
var usersDelete = [];
users.forEach(function(user) {
    usersDelete.push(user._id);
});
db.projects.remove({'members.user_id': {$in: usersDelete}}, {multi:1});

Removed 4 record(s) in 1ms
WriteResult({
  "nRemoved": 4
})
```

**5.** Apague todos os projetos que possuam uma determinada *tag* em goal.
```js
db.projects.remove({ 'goals.tags': {$eq: 'Start'}  }, {multi: 1})
Removed 2 record(s) in 1ms
WriteResult({
  "nRemoved": 2
})
```

## Gerenciamento de usuários

**1.** Crie um usuário com permissões **APENAS** de Leitura.

```js
db.createUser({user: 'user1', pwd: 'user1@bemean', roles: ['read'] })
Successfully added user: {
  "user": "user1",
  "roles": [
    "read"
  ]
}
```

**2.** Crie um usuário com permissões de Escrita e Leitura.

```js
db.createUser({user: 'user2', pwd: 'user2@bemean', roles: ['readWrite'] })
Successfully added user: {
  "user": "user2",
  "roles": [
    "readWrite"
  ]
}
```

**3.** Adicionar o papel `grantRolesToUser` e `revokeRole` para o usuário com Escrita e Leitura.

```js
db.runCommand({ createRole: 'grantRolesToUser', privileges: [
    { resource: { db: 'projeto-final', collection: '' }, actions: ['grantRole'] }
],
roles: [
    'readWrite'
]
})

{
  "ok": 1
}

db.runCommand({ createRole: 'revokeRole', privileges: [
    { resource: { db: 'projeto-final', collection: '' }, actions: ['revokeRole'] }
],
roles: [
    'readWrite'
]
})

{
  "ok": 1
}

db.grantRolesToUser(
    'user2',
    [
        'grantRolesToUser',
        'revokeRole'
    ]
)
```

**4.** Remover o papel `grantRolesToUser` para o usuário com Escrita e Leitura.

```js
db.revokeRolesFromUser(
    'user2',
    [
        'grantRolesToUser'
    ]
)
```

**5.** Listar todos os usuários com seus papéis e ações.

```js
db.runCommand({usersInfo: 1})

{
  "users": [
    {
      "_id": "projeto-final.user1",
      "user": "user1",
      "db": "projeto-final",
      "roles": [
        {
          "role": "read",
          "db": "projeto-final"
        }
      ]
    },
    {
      "_id": "projeto-final.user2",
      "user": "user2",
      "db": "projeto-final",
      "roles": [
        {
          "role": "revokeRole",
          "db": "projeto-final"
        },
        {
          "role": "readWrite",
          "db": "projeto-final"
        }
      ]
    }
  ],
  "ok": 1
}
```

## Sharding

### Config Server

```js
mkdir /data/configdb
mongod --configsvr --port 27010

2016-02-06T11:51:19.957-0200 I CONTROL  [initandlisten] MongoDB starting : pid=5623 port=27010 dbpath=/data/configdb master=1 64-bit host=eric-ubuntu
2016-02-06T11:51:19.957-0200 I CONTROL  [initandlisten] db version v3.0.9
2016-02-06T11:51:19.957-0200 I CONTROL  [initandlisten] git version: 20d60d3491908f1ae252fe452300de3978a040c7
2016-02-06T11:51:19.957-0200 I CONTROL  [initandlisten] build info: Linux ip-10-237-6-122 3.13.0-24-generic #46-Ubuntu SMP Thu Apr 10 19:11:08 UTC 2014 x86_64 BOOST_LIB_VERSION=1_49
2016-02-06T11:51:19.957-0200 I CONTROL  [initandlisten] allocator: tcmalloc
2016-02-06T11:51:19.957-0200 I CONTROL  [initandlisten] options: { net: { port: 27010 }, sharding: { clusterRole: "configsvr" } }
2016-02-06T11:51:20.008-0200 I JOURNAL  [initandlisten] journal dir=/data/configdb/journal
2016-02-06T11:51:20.012-0200 I JOURNAL  [initandlisten] recover : no journal files present, no recovery needed
2016-02-06T11:51:20.093-0200 I JOURNAL  [durability] Durability thread started
2016-02-06T11:51:20.093-0200 I JOURNAL  [journal writer] Journal writer thread started
2016-02-06T11:51:20.124-0200 I CONTROL  [initandlisten] 
2016-02-06T11:51:20.124-0200 I CONTROL  [initandlisten] ** WARNING: /sys/kernel/mm/transparent_hugepage/enabled is 'always'.
2016-02-06T11:51:20.124-0200 I CONTROL  [initandlisten] **        We suggest setting it to 'never'
2016-02-06T11:51:20.124-0200 I CONTROL  [initandlisten] 
2016-02-06T11:51:20.124-0200 I CONTROL  [initandlisten] ** WARNING: /sys/kernel/mm/transparent_hugepage/defrag is 'always'.
2016-02-06T11:51:20.124-0200 I CONTROL  [initandlisten] **        We suggest setting it to 'never'
2016-02-06T11:51:20.124-0200 I CONTROL  [initandlisten] 
2016-02-06T11:51:21.375-0200 I NETWORK  [initandlisten] waiting for connections on port 27010
```

### Router

```js
mongos -configdb localhost:27010 --port 27011

2016-02-06T11:55:51.330-0200 W SHARDING running with 1 config server should be done only for testing purposes and is not recommended for production
2016-02-06T11:55:51.358-0200 I SHARDING [mongosMain] MongoS version 3.0.9 starting: pid=5793 port=27011 64-bit host=eric-ubuntu (--help for usage)
2016-02-06T11:55:51.358-0200 I CONTROL  [mongosMain] db version v3.0.9
2016-02-06T11:55:51.358-0200 I CONTROL  [mongosMain] git version: 20d60d3491908f1ae252fe452300de3978a040c7
2016-02-06T11:55:51.358-0200 I CONTROL  [mongosMain] build info: Linux ip-10-237-6-122 3.13.0-24-generic #46-Ubuntu SMP Thu Apr 10 19:11:08 UTC 2014 x86_64 BOOST_LIB_VERSION=1_49
2016-02-06T11:55:51.358-0200 I CONTROL  [mongosMain] allocator: tcmalloc
2016-02-06T11:55:51.358-0200 I CONTROL  [mongosMain] options: { net: { port: 27011 }, sharding: { configDB: "localhost:27010" } }
2016-02-06T11:55:51.485-0200 I SHARDING [Balancer] about to contact config servers and shards
2016-02-06T11:55:51.486-0200 W NETWORK  [Balancer] Failed to connect to 127.0.0.1:27012, reason: errno:111 Connection refused
2016-02-06T11:55:51.513-0200 W SHARDING [Balancer] could not initialize balancer, please check that all shards and config servers are up: socket exception [CONNECT_ERROR] for localhost:27012
2016-02-06T11:55:51.513-0200 I SHARDING [Balancer] will retry to initialize balancer in one minute
2016-02-06T11:55:51.518-0200 I NETWORK  [mongosMain] waiting for connections on port 27011
```

### Shards

```js
mkdir /data/shard1 && mkdir /data/shard2 && mkdir /data/shard3
```

#### Shard 1
```js
mongod --port 27012 --dbpath /data/shard1

2016-02-06T12:18:33.220-0200 I CONTROL  [initandlisten] MongoDB starting : pid=6234 port=27012 dbpath=/data/shard1 64-bit host=eric-ubuntu
2016-02-06T12:18:33.220-0200 I CONTROL  [initandlisten] db version v3.0.9
2016-02-06T12:18:33.220-0200 I CONTROL  [initandlisten] git version: 20d60d3491908f1ae252fe452300de3978a040c7
2016-02-06T12:18:33.220-0200 I CONTROL  [initandlisten] build info: Linux ip-10-237-6-122 3.13.0-24-generic #46-Ubuntu SMP Thu Apr 10 19:11:08 UTC 2014 x86_64 BOOST_LIB_VERSION=1_49
2016-02-06T12:18:33.220-0200 I CONTROL  [initandlisten] allocator: tcmalloc
2016-02-06T12:18:33.220-0200 I CONTROL  [initandlisten] options: { net: { port: 27012 }, storage: { dbPath: "/data/shard1" } }
2016-02-06T12:18:33.266-0200 I JOURNAL  [initandlisten] journal dir=/data/shard1/journal
2016-02-06T12:18:33.271-0200 I JOURNAL  [initandlisten] recover : no journal files present, no recovery needed
2016-02-06T12:18:33.363-0200 I JOURNAL  [durability] Durability thread started
2016-02-06T12:18:33.363-0200 I JOURNAL  [journal writer] Journal writer thread started
2016-02-06T12:18:33.394-0200 I CONTROL  [initandlisten] 
2016-02-06T12:18:33.394-0200 I CONTROL  [initandlisten] ** WARNING: /sys/kernel/mm/transparent_hugepage/enabled is 'always'.
2016-02-06T12:18:33.394-0200 I CONTROL  [initandlisten] **        We suggest setting it to 'never'
2016-02-06T12:18:33.394-0200 I CONTROL  [initandlisten] 
2016-02-06T12:18:33.394-0200 I CONTROL  [initandlisten] ** WARNING: /sys/kernel/mm/transparent_hugepage/defrag is 'always'.
2016-02-06T12:18:33.394-0200 I CONTROL  [initandlisten] **        We suggest setting it to 'never'
2016-02-06T12:18:33.394-0200 I CONTROL  [initandlisten] 
2016-02-06T12:18:34.380-0200 I NETWORK  [initandlisten] waiting for connections on port 27012
```

#### Shard 2
```js
mongod --port 27013 --dbpath /data/shard2

2016-02-06T12:19:41.146-0200 I CONTROL  [initandlisten] MongoDB starting : pid=6274 port=27013 dbpath=/data/shard2 64-bit host=eric-ubuntu
2016-02-06T12:19:41.146-0200 I CONTROL  [initandlisten] db version v3.0.9
2016-02-06T12:19:41.146-0200 I CONTROL  [initandlisten] git version: 20d60d3491908f1ae252fe452300de3978a040c7
2016-02-06T12:19:41.147-0200 I CONTROL  [initandlisten] build info: Linux ip-10-237-6-122 3.13.0-24-generic #46-Ubuntu SMP Thu Apr 10 19:11:08 UTC 2014 x86_64 BOOST_LIB_VERSION=1_49
2016-02-06T12:19:41.147-0200 I CONTROL  [initandlisten] allocator: tcmalloc
2016-02-06T12:19:41.147-0200 I CONTROL  [initandlisten] options: { net: { port: 27013 }, storage: { dbPath: "/data/shard2" } }
2016-02-06T12:19:41.204-0200 I JOURNAL  [initandlisten] journal dir=/data/shard2/journal
2016-02-06T12:19:41.227-0200 I JOURNAL  [initandlisten] recover : no journal files present, no recovery needed
2016-02-06T12:19:41.517-0200 I JOURNAL  [durability] Durability thread started
2016-02-06T12:19:41.518-0200 I JOURNAL  [journal writer] Journal writer thread started
2016-02-06T12:19:41.555-0200 I CONTROL  [initandlisten] 
2016-02-06T12:19:41.555-0200 I CONTROL  [initandlisten] ** WARNING: /sys/kernel/mm/transparent_hugepage/enabled is 'always'.
2016-02-06T12:19:41.555-0200 I CONTROL  [initandlisten] **        We suggest setting it to 'never'
2016-02-06T12:19:41.555-0200 I CONTROL  [initandlisten] 
2016-02-06T12:19:41.555-0200 I CONTROL  [initandlisten] ** WARNING: /sys/kernel/mm/transparent_hugepage/defrag is 'always'.
2016-02-06T12:19:41.555-0200 I CONTROL  [initandlisten] **        We suggest setting it to 'never'
2016-02-06T12:19:41.555-0200 I CONTROL  [initandlisten] 
2016-02-06T12:19:42.203-0200 I NETWORK  [initandlisten] waiting for connections on port 27013
```

#### Shard 3

```js
mongod --port 27014 --dbpath /data/shard3

2016-02-06T12:24:08.335-0200 I CONTROL  [initandlisten] MongoDB starting : pid=6382 port=27014 dbpath=/data/shard3 64-bit host=eric-ubuntu
2016-02-06T12:24:08.335-0200 I CONTROL  [initandlisten] db version v3.0.9
2016-02-06T12:24:08.335-0200 I CONTROL  [initandlisten] git version: 20d60d3491908f1ae252fe452300de3978a040c7
2016-02-06T12:24:08.335-0200 I CONTROL  [initandlisten] build info: Linux ip-10-237-6-122 3.13.0-24-generic #46-Ubuntu SMP Thu Apr 10 19:11:08 UTC 2014 x86_64 BOOST_LIB_VERSION=1_49
2016-02-06T12:24:08.335-0200 I CONTROL  [initandlisten] allocator: tcmalloc
2016-02-06T12:24:08.335-0200 I CONTROL  [initandlisten] options: { net: { port: 27014 }, storage: { dbPath: "/data/shard3" } }
2016-02-06T12:24:08.384-0200 I JOURNAL  [initandlisten] journal dir=/data/shard3/journal
2016-02-06T12:24:08.395-0200 I JOURNAL  [initandlisten] recover : no journal files present, no recovery needed
2016-02-06T12:24:08.495-0200 I JOURNAL  [durability] Durability thread started
2016-02-06T12:24:08.495-0200 I JOURNAL  [journal writer] Journal writer thread started
2016-02-06T12:24:08.525-0200 I CONTROL  [initandlisten] 
2016-02-06T12:24:08.525-0200 I CONTROL  [initandlisten] ** WARNING: /sys/kernel/mm/transparent_hugepage/enabled is 'always'.
2016-02-06T12:24:08.525-0200 I CONTROL  [initandlisten] **        We suggest setting it to 'never'
2016-02-06T12:24:08.525-0200 I CONTROL  [initandlisten] 
2016-02-06T12:24:08.525-0200 I CONTROL  [initandlisten] ** WARNING: /sys/kernel/mm/transparent_hugepage/defrag is 'always'.
2016-02-06T12:24:08.525-0200 I CONTROL  [initandlisten] **        We suggest setting it to 'never'
2016-02-06T12:24:08.525-0200 I CONTROL  [initandlisten] 
2016-02-06T12:24:08.925-0200 I NETWORK  [initandlisten] waiting for connections on port 27014
```

#### Registrando shards no router

```js
mongo --port 27011 --host localhost

MongoDB shell version: 3.0.9
connecting to: localhost:27011/test
Mongo-Hacker 0.0.8

sh.addShard("localhost:27012")
{
  "shardAdded": "shard0000",
  "ok": 1
}
sh.addShard("localhost:27013")
{
  "shardAdded": "shard0001",
  "ok": 1
}
sh.addShard("localhost:27014")
{
  "shardAdded": "shard0002",
  "ok": 1
}

sh.enableSharding("projeto-final")
{
  "ok": 1
}

sh.shardCollection("projeto-final.activities", {"_id" : 1})
{
  "collectionsharded": "projeto-final.activities",
  "ok": 1
}
```

## Replica
```js
mkdir /data/rs1
mkdir /data/rs2
mkdir /data/rs3
```

### Replica 1

```js
mongod --replSet replica_set --port 27027 --dbpath /data/rs1
2016-02-06T12:52:09.403-0200 I CONTROL  [initandlisten] MongoDB starting : pid=8586 port=27027 dbpath=/data/rs1 64-bit host=eric-ubuntu
2016-02-06T12:52:09.403-0200 I CONTROL  [initandlisten] db version v3.0.9
2016-02-06T12:52:09.403-0200 I CONTROL  [initandlisten] git version: 20d60d3491908f1ae252fe452300de3978a040c7
2016-02-06T12:52:09.403-0200 I CONTROL  [initandlisten] build info: Linux ip-10-237-6-122 3.13.0-24-generic #46-Ubuntu SMP Thu Apr 10 19:11:08 UTC 2014 x86_64 BOOST_LIB_VERSION=1_49
2016-02-06T12:52:09.403-0200 I CONTROL  [initandlisten] allocator: tcmalloc
2016-02-06T12:52:09.403-0200 I CONTROL  [initandlisten] options: { net: { port: 27027 }, replication: { replSet: "replica_set" }, storage: { dbPath: "/data/rs1" } }
2016-02-06T12:52:09.443-0200 I JOURNAL  [initandlisten] journal dir=/data/rs1/journal
2016-02-06T12:52:09.443-0200 I JOURNAL  [initandlisten] recover : no journal files present, no recovery needed
2016-02-06T12:52:09.524-0200 I JOURNAL  [durability] Durability thread started
2016-02-06T12:52:09.524-0200 I JOURNAL  [journal writer] Journal writer thread started
2016-02-06T12:52:09.562-0200 I CONTROL  [initandlisten] 
2016-02-06T12:52:09.563-0200 I CONTROL  [initandlisten] ** WARNING: /sys/kernel/mm/transparent_hugepage/enabled is 'always'.
2016-02-06T12:52:09.563-0200 I CONTROL  [initandlisten] **        We suggest setting it to 'never'
2016-02-06T12:52:09.563-0200 I CONTROL  [initandlisten] 
2016-02-06T12:52:09.563-0200 I CONTROL  [initandlisten] ** WARNING: /sys/kernel/mm/transparent_hugepage/defrag is 'always'.
2016-02-06T12:52:09.563-0200 I CONTROL  [initandlisten] **        We suggest setting it to 'never'
2016-02-06T12:52:09.563-0200 I CONTROL  [initandlisten] 
2016-02-06T12:52:09.564-0200 I REPL     [initandlisten] Did not find local replica set configuration document at startup;  NoMatchingDocument Did not find replica set configuration document in local.system.replset
2016-02-06T12:52:09.565-0200 I NETWORK  [initandlisten] waiting for connections on port 27027
```

### Replica 2

```js
mongod --replSet replica_set --port 27028 --dbpath /data/rs2
2016-02-06T13:01:27.196-0200 I CONTROL  [initandlisten] MongoDB starting : pid=8901 port=27028 dbpath=/data/rs2 64-bit host=eric-ubuntu
2016-02-06T13:01:27.196-0200 I CONTROL  [initandlisten] db version v3.0.9
2016-02-06T13:01:27.196-0200 I CONTROL  [initandlisten] git version: 20d60d3491908f1ae252fe452300de3978a040c7
2016-02-06T13:01:27.196-0200 I CONTROL  [initandlisten] build info: Linux ip-10-237-6-122 3.13.0-24-generic #46-Ubuntu SMP Thu Apr 10 19:11:08 UTC 2014 x86_64 BOOST_LIB_VERSION=1_49
2016-02-06T13:01:27.196-0200 I CONTROL  [initandlisten] allocator: tcmalloc
2016-02-06T13:01:27.196-0200 I CONTROL  [initandlisten] options: { net: { port: 27028 }, replication: { replSet: "replica_set" }, storage: { dbPath: "/data/rs2" } }
2016-02-06T13:01:27.236-0200 I JOURNAL  [initandlisten] journal dir=/data/rs2/journal
2016-02-06T13:01:27.236-0200 I JOURNAL  [initandlisten] recover : no journal files present, no recovery needed
2016-02-06T13:01:27.306-0200 I JOURNAL  [durability] Durability thread started
2016-02-06T13:01:27.306-0200 I JOURNAL  [journal writer] Journal writer thread started
2016-02-06T13:01:27.339-0200 I CONTROL  [initandlisten] 
2016-02-06T13:01:27.339-0200 I CONTROL  [initandlisten] ** WARNING: /sys/kernel/mm/transparent_hugepage/enabled is 'always'.
2016-02-06T13:01:27.339-0200 I CONTROL  [initandlisten] **        We suggest setting it to 'never'
2016-02-06T13:01:27.339-0200 I CONTROL  [initandlisten] 
2016-02-06T13:01:27.339-0200 I CONTROL  [initandlisten] ** WARNING: /sys/kernel/mm/transparent_hugepage/defrag is 'always'.
2016-02-06T13:01:27.339-0200 I CONTROL  [initandlisten] **        We suggest setting it to 'never'
2016-02-06T13:01:27.339-0200 I CONTROL  [initandlisten] 
2016-02-06T13:01:27.341-0200 I REPL     [initandlisten] Did not find local replica set configuration document at startup;  NoMatchingDocument Did not find replica set configuration document in local.system.replset
2016-02-06T13:01:27.342-0200 I NETWORK  [initandlisten] waiting for connections on port 27028
```

### Replica 3

```js
mongod --replSet replica_set --port 27029 --dbpath /data/rs3
2016-02-06T13:03:45.351-0200 I CONTROL  [initandlisten] MongoDB starting : pid=9018 port=27029 dbpath=/data/rs3 64-bit host=eric-ubuntu
2016-02-06T13:03:45.351-0200 I CONTROL  [initandlisten] db version v3.0.9
2016-02-06T13:03:45.351-0200 I CONTROL  [initandlisten] git version: 20d60d3491908f1ae252fe452300de3978a040c7
2016-02-06T13:03:45.351-0200 I CONTROL  [initandlisten] build info: Linux ip-10-237-6-122 3.13.0-24-generic #46-Ubuntu SMP Thu Apr 10 19:11:08 UTC 2014 x86_64 BOOST_LIB_VERSION=1_49
2016-02-06T13:03:45.351-0200 I CONTROL  [initandlisten] allocator: tcmalloc
2016-02-06T13:03:45.351-0200 I CONTROL  [initandlisten] options: { net: { port: 27029 }, replication: { replSet: "replica_set" }, storage: { dbPath: "/data/rs3" } }
2016-02-06T13:03:45.391-0200 I JOURNAL  [initandlisten] journal dir=/data/rs3/journal
2016-02-06T13:03:45.391-0200 I JOURNAL  [initandlisten] recover : no journal files present, no recovery needed
2016-02-06T13:03:45.452-0200 I JOURNAL  [durability] Durability thread started
2016-02-06T13:03:45.452-0200 I JOURNAL  [journal writer] Journal writer thread started
2016-02-06T13:03:45.485-0200 I CONTROL  [initandlisten] 
2016-02-06T13:03:45.485-0200 I CONTROL  [initandlisten] ** WARNING: /sys/kernel/mm/transparent_hugepage/enabled is 'always'.
2016-02-06T13:03:45.485-0200 I CONTROL  [initandlisten] **        We suggest setting it to 'never'
2016-02-06T13:03:45.485-0200 I CONTROL  [initandlisten] 
2016-02-06T13:03:45.485-0200 I CONTROL  [initandlisten] ** WARNING: /sys/kernel/mm/transparent_hugepage/defrag is 'always'.
2016-02-06T13:03:45.485-0200 I CONTROL  [initandlisten] **        We suggest setting it to 'never'
2016-02-06T13:03:45.485-0200 I CONTROL  [initandlisten] 
2016-02-06T13:03:45.487-0200 I REPL     [initandlisten] Did not find local replica set configuration document at startup;  NoMatchingDocument Did not find replica set configuration document in local.system.replset
2016-02-06T13:03:45.488-0200 I NETWORK  [initandlisten] waiting for connections on port 27029
```

### Config

```js
mongo --port 27027

rsconf = {
   _id: "replica_set",
   members: [
    {
     _id: 0,
     host: "127.0.0.1:27027"
    }
  ]
}

rs.initiate(rsconf)
{
  "ok": 1
}

rs.add("127.0.0.1:27028")
{
  "ok": 1
}

rs.add("127.0.0.1:27029")
{
  "ok": 1
}
```