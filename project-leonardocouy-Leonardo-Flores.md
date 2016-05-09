# MongoDb - Projeto Final
**Autor:** Leonardo Flores
**Data:** 1459544107357

## Para qual sistema você usaria o MongoDB (diferente desse)?

**Redes Sociais, Noticiários, Chats**. Porque esses sistemas devem utilizar MongoDB ou outro banco não-relacional? Já podemos imaginar quantos joins, quantos relacionamentos teria esse sistemas em um banco relacional. Agora pensando de um modo não-relacional, podemos integrar essas relações como sub-documentos dentro de um documento!!! Isso é demais!! Não precisamos de dar JOINS, pelo fato do noSQL não ter JOIN e TRANSACTIONS! Em apenas uma tacada, podemos retornar tudo em apenas 1 DOCUMENTO. 

## Qual a modelagem da sua coleção de `users`?

```js
user: {
  name            : String,
  bio             : String,
  date_register   : Date,
  avatar_path     : String,
  background_path : String,
  auth: {
    username    : String,
    email       : String,
    password    : String,
    last_access : Date,
    online      : Boolean,
    disabled    : String,
    hash_token  : String,
  }
}
```


## Qual a modelagem da sua coleção de `projects`?

```js
project: {
  _id               : ObjectID,
  name              : String,
  description       : String,
  date_begin        : Date,
  date_dream        : Date,
  date_end          : Date,
  visible           : Boolean,
  realocate         : Boolean,
  expired           : Boolean,
  visualizable_mod  : String,

  members: [{
    user_id     : ObjectID,
    type_member : [String, String, ...],   
    notify      : Boolean,
  }],

  tags: [String, String, ...],

  goals: [{
    name        : String,
    description : String,
    date_begin  : Date,
    date_dream  : Date,
    date_end    : Date,
    realocate   : Boolean,
    expired     : Boolean,
    tags        : [String, String, ...],
    activities  : [ObjectID, ObjectID, ...]
    historics   : [Date, Date, ...]
  }]
}
```

## Qual a modelagem da sua coleção de `activities`?

O motivo da separação do activities da coleção projects, é simplesmente pelo fato de possuir grandes chances de haver muitos registros e estourar o limite de 16MB por exemplo: Um projeto pode haver diversos membros e diversos objetivos, até o então OK, mas imaginemos que dentro desses objetivos tenha em torno de 30 atividades e que cada atividade dessa possua em torno de 30, 40 comentários, devido a essa grande quantidade de registros poderia estourar o limite. A solução é referenciar os ID's das activities dentro dos objetivos.

```js

activities: [{
  _id         : ObjectId,
  name        : String,
  description : String,
  date_begin  : Date,
  date_dream  : Date,
  date_end    : Date,
  realocate   : Boolean,
  expired     : Boolean,
  members: [{
    type_member : [String, String, ...],
    user_id     : ObjectID,
    notify      : Boolean,
  }],
  comments: [{
    text          : String,
    date_comment  : Date,
    members: [{
      type_member : [String, String, ...],
      user_id     : ObjectID,
      notify      : Boolean,
    }],
    files: [{
      name        : String,
      path        : String,
      weight      : double,
    }]
  }],

}]
```

## Create - cadastro

#### 1. Cadastre 10 usuários diferentes.

```js
var users = [
  {
    name            : "Leonardo Flores",
    bio             : "Python Developer, AngularJS Developer and Ionic Developer",
    date_register   : new Date("2016-03-13T20:02:52+00:00"),
    avatar_path     : "~/be-mean/media/avatar/leo.png",
    background_path : "~/be-mean/media/bg/striped.gif",
    auth: {
      username    : "Leo",
      email       : "leonardocouy@hotmail.com",
      password    : "leo123",
      last_access : new Date(),
      online      : false,
      disabled    : false,
      hash_token  : "28d2a43f678a686330aaa42c9486bf720a8e",
    }
  },
  {
    name            : "Roscoe Morar IV",
    bio             : "cupiditate sequi autem aut omnis necessitatibus\nimpedit expedita nostrum molestiae qui saepe numquam dolores pariatur\nplaceat totam sit",
    date_register   : new Date("2016-03-12T10:00:52+00:00"),
    avatar_path     : "~/be-mean/media/avatar/roscoe.png",
    background_path : "~/be-mean/media/bg/cats.gif",
    auth: {
      username    : "Roscoe",
      email       : "Erick@demario.com",
      password    : "idontknow",
      last_access : new Date(),
      online      : true,
      disabled    : false,
      hash_token  : "645b78202bd9a06a4f28dfaa5dc6c00eb21f",
    }
  },
  {
    name            : "Russ Romaguera V",
    bio             : "qui est voluptatem et voluptas\nofficia cumque porro debitis consectetur perferendis est odio\nqui expedita harum beatae assumenda accusamus dolorum corporis",
    date_register   : new Date("2016-03-08T21:02:52+00:00"),
    avatar_path     : "~/be-mean/media/avatar/russ.png",
    background_path : "~/be-mean/media/bg/dogs.gif",
    auth: {
      username    : "RussSex",
      email       : "Angel@jevon.ca",
      password    : "russ123",
      last_access : new Date(),
      online      : true,
      disabled    : false,
      hash_token  : "33bb8899649354d52d385a9c6682ad17ad10",
    }
  },
  {
    name            : "Cora Gusikowski",
    bio             : "voluptatibus beatae officiis molestiae a consequatur quasi expedita perferendis\nofficia necessitatibus error\nquae illo ipsum nulla at vero iure necessitatibus voluptates",
    date_register   : new Date("2016-03-13T19:00:52+00:00"),
    avatar_path     : "~/be-mean/media/avatar/cora.png",
    background_path : "~/be-mean/media/bg/reggae.gif",
    auth: {
      username    : "CatCora",
      email       : "Edyth.Marvin@monroe.info",
      password    : "idk2",
      last_access : new Date(),
      online      : false,
      disabled    : false,
      hash_token  : "88a1d27675c3f59a9b7bca4367f4a4d89414",
    }
  },  
  {
    name            : "Kaylie Hudson",
    bio             : "nemo tempore placeat\naut id quod quo\nomnis eius aut dolores vitae quidem",
    date_register   : new Date("2016-03-16T16:02:52+00:00"),
    avatar_path     : "~/be-mean/media/avatar/kay.png",
    background_path : "~/be-mean/media/bg/4and20.gif",
    auth: {
      username    : "KayHud",
      email       : "Emmie_Boehm@sienna.us",
      password    : "idk2",
      last_access : new Date(),
      online      : false,
      disabled    : false,
      hash_token  : "d5fbee463507a6a64ca6944f21d82744ca6e",
    }
  },
  {
    name            : "Andres Schamberger",
    bio             : "voluptatem ullam corporis eaque officiis sit dolore repellat atque\nbeatae autem saepe id debitis consequatur qui atque recusandae\nid sit qui delectus numquam quaerat tempore laudantium nihil",
    date_register   : new Date("2016-03-15T14:02:52+00:00"),
    avatar_path     : "~/be-mean/media/avatar/andre.png",
    background_path : "~/be-mean/media/bg/green.gif",
    auth: {
      username    : "aschamb",
      email       : "Gracie@stephania.net",
      password    : "youshallnotpass",
      last_access : new Date(),
      online      : false,
      disabled    : true,
      hash_token  : "e4f36af2117e73f0d80756872b21cf2688f5",
    }
  },  
  {
    name            : "Jamal Roberts",
    bio             : "et aut id nemo aut\nlaboriosam qui molestiae et repudiandae dolores\nearum maxime quo rem voluptatem sequi",
    date_register   : new Date("2016-03-17T13:02:52+00:00"),
    avatar_path     : "~/be-mean/media/avatar/jamal.png",
    background_path : "~/be-mean/media/bg/hearts.gif",
    auth: {
      username    : "Jamal",
      email       : "Dominique@jose.ca",
      password    : "baileoffavela",
      last_access : new Date(),
      online      : true,
      disabled    : false,
      hash_token  : "07a81ede55fb7ed4cd4f2563ce58d52a9a1b",
    }
  },
  {
    name            : "Demetris Kunze",
    bio             : "voluptas consectetur ex et odit\net architecto sed voluptate numquam maiores\nlaudantium quo maiores est",
    date_register   : new Date("2016-03-17T14:32:52+00:00"),
    avatar_path     : "~/be-mean/media/avatar/demetris.png",
    background_path : "~/be-mean/media/bg/skies.gif",
    auth: {
      username    : "kunze",
      email       : "Frederique_Lemke@camryn.io",
      password    : "kunzeofparanaue",
      last_access : new Date(),
      online      : true,
      disabled    : false,
      hash_token  : "ac6719b64cbd6506b8773d9cd71860b2a445",
    }
  },
  {
    name            : "Flo Blick",
    bio             : "sit deleniti sint saepe qui\nquasi fugiat officiis\nrepellendus ad quisquam quia consectetur ut",
    date_register   : new Date("2016-02-28T09:02:52+00:00"),
    avatar_path     : "~/be-mean/media/avatar/blick.png",
    background_path : "~/be-mean/media/bg/blue.gif",
    auth: {
      username    : "blick182",
      email       : "Darrin@brayan.name",
      password    : "blick182doyouknow",
      last_access : new Date(),
      online      : false,
      disabled    : false,
      hash_token  : "03d3c7b4f535a55313996523ed9e32dc653d",
    }
  },
  {
    name            : "Lesly Goldner",
    bio             : "voluptas est in voluptas vel nulla cumque porro iusto\nvelit accusamus et minima sequi reiciendis quisquam labore dolorum\nconsequatur eos laborum quis",
    date_register   : new Date("2016-02-08T22:02:52+00:00"),
    avatar_path     : "~/be-mean/media/avatar/lesly.png",
    background_path : "~/be-mean/media/bg/sunset.gif",
    auth: {
      username    : "Legold",
      email       : "Lucy@kraig.me",
      password    : "gold124",
      last_access : new Date(),
      online      : false,
      disabled    : false,
      hash_token  : "00dafd80857f669cfdad17d0ffaeaca1a9e2",
    }
  },
];

```

```
MacBook-Pro-de-Leonardo(mongod-3.2.1) project-manager> db.users.insert(users)
Inserted 1 record(s) in 25ms
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
Armazenando todos usuários na variável **users**

```
var query = {}
var fields = {name: 1}
var users = db.users.find(query, fields).toArray()
```

Criando uma função para adicionar dias baseado na data atual para podermos simular datas

```js
function addDaysFromDateNow(days){
  var result = new Date();
  result.setDate(result.getDate() + days);
  return result;
};
```

Cadastrar as **activities** no banco

```js
var activities = [
  {
    name        : "Class 01",
    description : "Installing Python in Mac OS X, Linux and Mac",
    date_begin  : addDaysFromDateNow(2),
    date_dream  : addDaysFromDateNow(3),
    date_end    : addDaysFromDateNow(4),
    realocate   : false,
    expired     : false,
    members: [{
      user_id     : users[2]._id,
      type_member : ["Python Teacher", "Python Developer"],
      notify      : true
    }],
    comments: [{
      text          : "ISSUE - Low microphone volume",
      date_comment  : addDaysFromDateNow(6),
      members: [{
        user_id     : users[2]._id,
        type_member : ["Python Teacher", "Python Developer"],
        notify      : true
      }]
    }],
    files: [{
      name    : "Class 01",
      path    : "http://youtube.com/leo/python01",
      weight  : null
    }]
  },
  {
    name        : "Class 02",
    description : "Control Statements, List and Dictionaries",
    date_begin  : addDaysFromDateNow(7),
    date_dream  : addDaysFromDateNow(8),
    date_end    : addDaysFromDateNow(9),
    realocate   : false,
    expired     : false,
    members: [{
      user_id     : users[2]._id,
      type_member : ["Python Teacher", "Python Developer"],
      notify      : true,
    },
    {
      user_id     : users[3]._id,
      type_member : ["Senior Python Developer"],
      notify      : false,
    }],
    comments: [],
    files: [{
      name    : "Class 02",
      path    : "http://youtube.com/leo/python02",
      weight  : null,
    }]
  },
  {
    name        : "Section 01",
    description : "Overview and introduction",
    date_begin  : addDaysFromDateNow(2),
    date_dream  : addDaysFromDateNow(20),
    date_end    : addDaysFromDateNow(30),
    realocate   : false,
    expired     : false,
    members: [{
      user_id     : users[5]._id,
      type_member : ["Author", "MongoDB Architect"],
      notify      : true,
    },
    {
      user_id     : users[8]._id,
      type_member : ["Portuguese Teacher"],
      notify      : false,
    }],
    comments: [{
      text          : "Grammar Error",
      date_comment  : addDaysFromDateNow(20),
      members: [{
        user_id     : users[8]._id,
        type_member : ["Portuguese Teacher"],
        notify      : false,
      }]
    }],
    files: [{
      name    : "Section 01",
      path    : "~/Dropbox/mongobook/section01.docx",
      weight  : null,
    }]
  },
  {
    name        : "Section 02",
    description : "Installing MongoDB in Mac OS X, Linux and Windows",
    date_begin  : addDaysFromDateNow(30),
    date_dream  : addDaysFromDateNow(40),
    date_end    : addDaysFromDateNow(45),
    realocate   : false,
    expired     : false,
    members: [{
      user_id     : users[5]._id,
      type_member : ["Founder", "MongoDB Teacher"],
      notify      : true,
    },
    {
      user_id     : users[7]._id,
      type_member : ["MongoDB Founder"],
      notify      : false,
    },
    {
      user_id     : users[8]._id,
      type_member : ["Portuguese Teacher"],
      notify      : false,
    }],
    comments: [],
    files: [{
      name    : "Section 02",
      path    : "~/Dropbox/mongobook/section02.docx",
      weight  : null,
    }]
  },
  {
    name        : "Class 01",
    description : "MongoDB Installation and first steps.",
    date_begin  : addDaysFromDateNow(2),
    date_dream  : addDaysFromDateNow(3),
    date_end    : addDaysFromDateNow(4),
    realocate   : false,
    expired     : false,
    members: [{
      user_id     : users[5]._id,
      type_member : ["Founder", "MongoDB Teacher"],
      notify      : true,
    },
    {
      user_id     : users[8]._id,
      type_member : ["MongoDB Monitor", "Student"],
      notify      : false,
    }],
    comments: [{
      text          : "Missing Link to exercise.",
      date_comment  : addDaysFromDateNow(5),
      members: [{
        user_id     : users[8]._id,
        type_member : ["MongoDB Monitor", "Student"],
        notify      : false,
      }]
    }],
    files: [{
      name    : "Class 01",
      path    : "http://youtube.com/bemean/mongo01",
      weight  : null,
    }]
  },
  {
    name        : "Class 02",
    description : "Using mongoimport and mongoexport",
    date_begin  : addDaysFromDateNow(7),
    date_dream  : addDaysFromDateNow(8),
    date_end    : addDaysFromDateNow(9),
    realocate   : false,
    expired     : false,
    members: [{
      user_id     : users[5]._id,
      type_member : ["Founder", "MongoDB Teacher"],
      notify      : true,
    },
    {
      user_id     : users[8]._id,
      type_member : ["MongoDB Monitor", "Student"],
      notify      : false,
    }],
    comments: [],
    files: [{
      name    : "Class 02",
      path    : "http://youtube.com/bemean/mongo02",
      weight  : null,
    }]
  },
  {
    name        : "Login Functionality",
    description : "Form containing email input and password.",
    date_begin  : addDaysFromDateNow(3),
    date_dream  : addDaysFromDateNow(5),
    date_end    : addDaysFromDateNow(6),
    realocate   : false,
    expired     : false,
    members: [{
      user_id     : users[2]._id,
      type_member : ["Product Owner", "Senior Cobol Developer"],
      notify      : true,
    },
    {
      user_id     : users[3]._id,
      type_member : ["Scrum Master", "Front-end Developer"],
      notify      : true,
    },
    {
      user_id     : users[4]._id,
      type_member : ["Trainee", "Junior Front-end Developer"],
      notify      : true,
    }],
    comments: [{
      text          : "[FEATURE] #1 - Login by Facebook",
      date_comment  : addDaysFromDateNow(5),
      members: [{
        user_id     : users[2]._id,
        type_member : ["Product Owner", "Senior Cobol Developer"],
        notify      : true,
      }]    
    }],
    files: [{
      name    : "Prototype Login",
      path    : "~/project/dev/assets/prototype-login.png",
      weight  : 0.430
    }]
  },
  {
    name        : "Signup Functionality",
    description : "Form containing email, first name, last name, password, password check, captcha.",
    date_begin  : addDaysFromDateNow(4),
    date_dream  : addDaysFromDateNow(5),
    date_end    : addDaysFromDateNow(7),
    realocate   : false,
    expired     : false,
    members: [{
      user_id     : users[2]._id,
      type_member : ["Product Owner", "Senior Cobol Developer"],
      notify      : true,
    },
    {
      user_id     : users[4]._id,
      type_member : ["Trainee", "Junior Front-end Developer"],
      notify      : true,
    }],
    comments: [{
      text          : "[BUG] #1 - Internal Server Error ",
      date_comment  : addDaysFromDateNow(6),
      members: [{
        user_id     : users[2]._id,
        type_member : ["Product Owner", "Senior Cobol Developer"],
        notify      : true,    
      }]
    }],
    files: [{
      name    : null,
      path    : null,
      weight  : null
    }]
  },
]
```

```
MacBook-Pro-de-Leonardo(mongod-3.2.1) project-manager> db.activities.insert(activities)
Inserted 1 record(s) in 26ms
BulkWriteResult({
  "writeErrors": [ ],
  "writeConcernErrors": [ ],
  "nInserted": 8,
  "nUpserted": 0,
  "nMatched": 0,
  "nModified": 0,
  "nRemoved": 0,
  "upserted": [ ]
})
```

Armazenar todas as **activities cadastradas** em uma variável chamada activities:

```
var query = {}
var fields = {name: 1}
var activities = db.activities.find(query, fields).toArray()
```

Por fim, hora de inserir os 5 projetos.

```js
var projects = [
  {
    name              : "Instagram",
    description       : "Transform your experience in photos",
    date_begin        : new Date(),
    date_dream        : addDaysFromDateNow(270),
    date_end          : addDaysFromDateNow(360),
    visible           : true,
    realocate         : false,
    expired           : false,
    visualizable_mod  : null,
    members: [
      {
        user_id     : users[0]._id,
        type_member : ["Founder", "CTO"],
        notify      : true,
      },
      {
        user_id     : users[1]._id,
        type_member : ["Co-founder", "Marketing Manager"],
        notify      : true,
      },
      {
        user_id     : users[2]._id,
        type_member : ["Product Owner", "Senior Cobol Developer"],
        notify      : true,
      },
      {
        user_id     : users[3]._id,
        type_member : ["Scrum Master", "Front-end Developer"],
        notify      : true,
      },
      {
        user_id     : users[4]._id,
        type_member : ["Trainee", "Junior Front-end Developer"],
        notify      : false,
      }
    ],

    tags: ["instagram", "javascript", "cobol", "social media", "money", "be-mean"],

    goals: [{
      name        : "Authentication",
      description : "User login, register and login by oauth.",
      date_begin  : addDaysFromDateNow(2),
      date_dream  : addDaysFromDateNow(10),
      date_end    : addDaysFromDateNow(15),
      realocate   : false,
      expired     : false,
      tags        : ["auth", "login", "register", "oauth"],
      activities  : [activities[6]._id, activities[7]._id],
      historics   : [addDaysFromDateNow(2), addDaysFromDateNow(4), addDaysFromDateNow(5)]
    }]
  },
  {
    name              : "Be Mean Course",
    description       : "Workshop of MongoDB, Express, Angular and Node 100% free.",
    date_begin        : new Date(),
    date_dream        : addDaysFromDateNow(180),
    date_end          : addDaysFromDateNow(360),
    visible           : true,
    realocate         : false,
    expired           : false,
    visualizable_mod  : null,
    members: [
      {
        user_id     : users[5]._id,
        type_member : ["Founder", "MongoDB Teacher"],
        notify      : true,
      },
      {
        user_id     : users[6]._id,
        type_member : ["Co-founder", "NodeJS Teacher", "AngularJS Teacher"],
        notify      : true,
      },
      {
        user_id     : users[7]._id,
        type_member : ["Express Teacher", "NodeJS Teacher"],
        notify      : true,
      },
      {
        user_id     : users[8]._id,
        type_member : ["MongoDB Monitor", "Student"],
        notify      : false,
      },
      {
        user_id     : users[9]._id,
        type_member : ["NodeJS Monitor", "AngularJS Monitor", "Student"],
        notify      : false,
      }
    ],

    tags: ["nodejs", "mongodb", "free", "course", "money", "be-mean"],

    goals: [{
      name        : "MongoDB Classes",
      description : "Capture class and upload to youtube.",
      date_begin  : new Date(),
      date_dream  : addDaysFromDateNow(45),
      date_end    : addDaysFromDateNow(60),
      realocate   : false,
      expired     : false,
      tags        : ["mongodb", "classes", "nosql", "fucksql"],
      activities  : [activities[4]._id, activities[5]._id],
      historics   : [addDaysFromDateNow(3), addDaysFromDateNow(5), addDaysFromDateNow(6)]
    }]
  },
  {  
    name              : "MongoDB Book",
    description       : "This book covers about MongoDB.",
    date_begin        : new Date(),
    date_dream        : addDaysFromDateNow(180),
    date_end          : addDaysFromDateNow(360),
    visible           : true,
    realocate         : false,
    expired           : false,
    visualizable_mod  : null,
    members: [
      {
        user_id     : users[5]._id,
        type_member : ["Founder", "MongoDB Teacher"],
        notify      : true,
      },
      {
        user_id     : users[7]._id,
        type_member : ["MongoDB Founder"],
        notify      : false,
      },
      {
        user_id     : users[8]._id,
        type_member : ["Portuguese Teacher"],
        notify      : false,
      },
      {
        user_id     : users[1]._id,
        type_member : ["Chief Editor of SuissaBooks"],
        notify      : false,
      },
      {
        user_id     : users[2]._id,
        type_member : ["Editor of SuissaBooks"],
        notify      : false,
      }
    ],

    tags: ["book", "read", "suissabooks", "best", "be-mean"],

    goals: [{
      name        : "Write 2 sections in 2 months",
      description : "Section 01 and Section 02",
      date_begin  : new Date(),
      date_dream  : addDaysFromDateNow(45),
      date_end    : addDaysFromDateNow(60),
      realocate   : false,
      expired     : false,
      tags        : ["mongodb", "classes", "nosql", "fucksql"],
      activities  : [activities[2]._id, activities[3]._id],
      historics   : [addDaysFromDateNow(20), addDaysFromDateNow(32), addDaysFromDateNow(33)]
    }]
  },
  {
    name              : "Python Course",
    description       : "100% python beginner course.",
    date_begin        : new Date(),
    date_dream        : addDaysFromDateNow(60),
    date_end          : addDaysFromDateNow(90),
    visible           : true,
    realocate         : false,
    expired           : false,
    visualizable_mod  : null,
    members: [
      {
        user_id     : users[2]._id,
        type_member : ["Python Teacher", "Python Developer"],
        notify      : true,
      },
      {
        user_id     : users[3]._id,
        type_member : ["Senior Python Developer"],
        notify      : false,
      },
      {
        user_id     : users[1]._id,
        type_member : ["Python Monitor", "Student"],
        notify      : false,
      },
      {
        user_id     : users[5]._id,
        type_member : ["Founder PyCourse"],
        notify      : false,
      },
      {
        user_id     : users[6]._id,
        type_member : ["Co-founder PyCourse"],
        notify      : false,
      }
    ],

    tags: ["python", "pycourse", "beginner", "course", "cheap"],

    goals: [{
      name        : "Python Classes",
      description : "Record and upload to youtube each class",
      date_begin  : new Date(),
      date_dream  : addDaysFromDateNow(60),
      date_end    : addDaysFromDateNow(180),
      realocate   : false,
      expired     : false,
      tags        : ["python", "list", "dictionaries", "easy"],
      activities  : [activities[0]._id, activities[1]._id],
      historics   : [addDaysFromDateNow(10), addDaysFromDateNow(22), addDaysFromDateNow(23)]
    }]
  },
  {
    name              : "PHP Course",
    description       : "100% php beginner course.",
    date_begin        : new Date(),
    date_dream        : addDaysFromDateNow(30),
    date_end          : addDaysFromDateNow(60),
    visible           : true,
    realocate         : false,
    expired           : false,
    visualizable_mod  : null,
    members: [
      {
        user_id     : users[9]._id,
        type_member : ["PHP Teacher", "Zend Certified"],
        notify      : true,
      },
      {
        user_id     : users[8]._id,
        type_member : ["PHP Teacher"],
        notify      : true,
      },
      {
        user_id     : users[7]._id,
        type_member : ["Laravel Teacher"],
        notify      : true,
      },
      {
        user_id     : users[6]._id,
        type_member : ["PHP Monitor", "Student"],
        notify      : false,
      },
      {
        user_id     : users[5]._id,
        type_member : ["CodeIgniter Teacher"],
        notify      : true,
      }
    ],

    tags: ["php", "phpcourse", "frameworks", "zend", "laravel"],

    goals: [{
      name        : "PHP Classes",
      description : "Record and upload to youtube each class",
      date_begin  : new Date(),
      date_dream  : addDaysFromDateNow(45),
      date_end    : addDaysFromDateNow(60),
      realocate   : false,
      expired     : false,
      tags        : ["php", "framework", "apache", "easy"],
      activities  : [activities[0]._id, activities[1]._id],
      historics   : [addDaysFromDateNow(8), addDaysFromDateNow(9), addDaysFromDateNow(10)]
    }]
  }
]

```

```
MacBook-Pro-de-Leonardo(mongod-3.2.1) project-manager> db.projects.insert(projects)
Inserted 1 record(s) in 26ms
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


Recuperamos primeiro a lista de membros envolvido neste projeto.

```
var query = {name: /python course/i}
var project = db.projects.findOne(query).members
```

Iteramos por essa lista de membros e recuperamos a lista de **users** em base do **user_id**

```js
var users = []
project.forEach(function(member){
    users.push(db.users.findOne({_id: member.user_id}))
})
MacBook-Pro-de-Leonardo(mongod-3.2.1) project-manager> users
[
  {
    "_id": ObjectId("56ed3d76d8890eb2ccdcd6ea"),
    "name": "Russ Romaguera V",
    "bio": "qui est voluptatem et voluptas\nofficia cumque porro debitis consectetur perferendis est odio\nqui expedita harum beatae assumenda accusamus dolorum corporis",
    "date_register": ISODate("2016-03-08T21:02:52Z"),
    "avatar_path": "~/be-mean/media/avatar/russ.png",
    "background_path": "~/be-mean/media/bg/dogs.gif",
    "auth": {
      "username": "RussSex",
      "email": "Angel@jevon.ca",
      "password": "russ123",
      "last_access": ISODate("2016-03-19T11:48:49.401Z"),
      "online": true,
      "disabled": false,
      "hash_token": "33bb8899649354d52d385a9c6682ad17ad10"
    }
  },
  {
    "_id": ObjectId("56ed3d76d8890eb2ccdcd6eb"),
    "name": "Cora Gusikowski",
    "bio": "voluptatibus beatae officiis molestiae a consequatur quasi expedita perferendis\nofficia necessitatibus error\nquae illo ipsum nulla at vero iure necessitatibus voluptates",
    "date_register": ISODate("2016-03-13T19:00:52Z"),
    "avatar_path": "~/be-mean/media/avatar/cora.png",
    "background_path": "~/be-mean/media/bg/reggae.gif",
    "auth": {
      "username": "CatCora",
      "email": "Edyth.Marvin@monroe.info",
      "password": "idk2",
      "last_access": ISODate("2016-03-19T11:48:49.402Z"),
      "online": false,
      "disabled": false,
      "hash_token": "88a1d27675c3f59a9b7bca4367f4a4d89414"
    }
  },
  {
    "_id": ObjectId("56ed3d76d8890eb2ccdcd6e9"),
    "name": "Roscoe Morar IV",
    "bio": "cupiditate sequi autem aut omnis necessitatibus\nimpedit expedita nostrum molestiae qui saepe numquam dolores pariatur\nplaceat totam sit",
    "date_register": ISODate("2016-03-12T10:00:52Z"),
    "avatar_path": "~/be-mean/media/avatar/roscoe.png",
    "background_path": "~/be-mean/media/bg/cats.gif",
    "auth": {
      "username": "Roscoe",
      "email": "Erick@demario.com",
      "password": "idontknow",
      "last_access": ISODate("2016-03-19T11:48:49.401Z"),
      "online": true,
      "disabled": false,
      "hash_token": "645b78202bd9a06a4f28dfaa5dc6c00eb21f"
    }
  },
  {
    "_id": ObjectId("56ed3d76d8890eb2ccdcd6ed"),
    "name": "Andres Schamberger",
    "bio": "voluptatem ullam corporis eaque officiis sit dolore repellat atque\nbeatae autem saepe id debitis consequatur qui atque recusandae\nid sit qui delectus numquam quaerat tempore laudantium nihil",
    "date_register": ISODate("2016-03-15T14:02:52Z"),
    "avatar_path": "~/be-mean/media/avatar/andre.png",
    "background_path": "~/be-mean/media/bg/green.gif",
    "auth": {
      "username": "aschamb",
      "email": "Gracie@stephania.net",
      "password": "youshallnotpass",
      "last_access": ISODate("2016-03-19T11:48:49.402Z"),
      "online": false,
      "disabled": true,
      "hash_token": "e4f36af2117e73f0d80756872b21cf2688f5"
    }
  },
  {
    "_id": ObjectId("56ed3d76d8890eb2ccdcd6ee"),
    "name": "Jamal Roberts",
    "bio": "et aut id nemo aut\nlaboriosam qui molestiae et repudiandae dolores\nearum maxime quo rem voluptatem sequi",
    "date_register": ISODate("2016-03-17T13:02:52Z"),
    "avatar_path": "~/be-mean/media/avatar/jamal.png",
    "background_path": "~/be-mean/media/bg/hearts.gif",
    "auth": {
      "username": "Jamal",
      "email": "Dominique@jose.ca",
      "password": "baileoffavela",
      "last_access": ISODate("2016-03-19T11:48:49.402Z"),
      "online": true,
      "disabled": false,
      "hash_token": "07a81ede55fb7ed4cd4f2563ce58d52a9a1b"
    }
  }
]
``` 

#### 2. Liste todos os projetos com a tag que você escolheu para os 3 projetos em comum.

```
var query = {tags: {$in: [/be-mean/i]}}
var fields = {tags: 1, name: 1}
MacBook-Pro-de-Leonardo(mongod-3.2.1) project-manager> db.projects.find(query, fields)
{
  "_id": ObjectId("56ed7914d8890eb2ccdcd70e"),
  "name": "Instagram",
  "tags": [
    "instagram",
    "javascript",
    "cobol",
    "social media",
    "money",
    "be-mean"
  ]
}
{
  "_id": ObjectId("56ed7914d8890eb2ccdcd70f"),
  "name": "Be Mean Course",
  "tags": [
    "nodejs",
    "mongodb",
    "free",
    "course",
    "money",
    "be-mean"
  ]
}
{
  "_id": ObjectId("56ed7914d8890eb2ccdcd710"),
  "name": "MongoDB Book",
  "tags": [
    "book",
    "read",
    "suissabooks",
    "best",
    "be-mean"
  ]
}
Fetched 3 record(s) in 3ms
```

#### 3. Liste apenas os nomes de todas as atividades para todos os projetos.

```js
var query = {}
var fields = {goals : 1, name: 1}
var projects = db.projects.find(query,fields).toArray()
var projects_activities = [];

projects.forEach(function(project){
  var project_act = {name: project.name, activities: []}
  project.goals.forEach(function(goal){
    goal.activities.forEach(function(activity_id){
      project_act.activities.push(db.activities.findOne({_id: activity_id}, {name: 1, _id: 0}).name)
    })
  })
  projects_activities.push(project_act)
})

MacBook-Pro-de-Leonardo(mongod-3.2.1) project-manager> projects_activities
[
  {
    "name": "Instagram",
    "activities": [
      "Login Functionality",
      "Signup Functionality"
    ]
  },
  {
    "name": "Be Mean Course",
    "activities": [
      "Class 01",
      "Class 02"
    ]
  },
  {
    "name": "MongoDB Book",
    "activities": [
      "Section 01",
      "Section 02"
    ]
  },
  {
    "name": "Python Course",
    "activities": [
      "Class 01",
      "Class 02"
    ]
  },
  {
    "name": "PHP Course",
    "activities": [
      "Class 01",
      "Class 02"
    ]
  }
]
```


#### 4. Liste todos os projetos que não possuam uma tag.
```
var query = {tags: []}
var fields = {_id: 0, name: 1}
MacBook-Pro-de-Leonardo(mongod-3.2.1) project-manager> db.projects.find(query, fields)
Fetched 0 record(s) in 1ms
MacBook-Pro-de-Leonardo(mongod-3.2.1) project-manager> db.projects.find(query, fields).count()
0
```

#### 5. Liste todos os usuários que não fazem parte do primeiro projeto cadastrado.

Primeiro devemos recuperar o primeiro projeto

```
var query = {}
var fields = {_id: 0, name: 1, 'members.user_id' : 1}
var project = db.projects.findOne(query, fields)
```

Agora extrair os ID's dos membros participantes deste projeto

```
var users_id = []
project.members.forEach(function(member){
  users_id.push(member.user_id)
})
MacBook-Pro-de-Leonardo(mongod-3.2.1) project-manager> users_id
[
  ObjectId("56ed3d76d8890eb2ccdcd6e8"),
  ObjectId("56ed3d76d8890eb2ccdcd6e9"),
  ObjectId("56ed3d76d8890eb2ccdcd6ea"),
  ObjectId("56ed3d76d8890eb2ccdcd6eb"),
  ObjectId("56ed3d76d8890eb2ccdcd6ec")
]
```

Por fim, recuperar a lista dos usuários que não esteja nessa lista

```
var usersQuery = {_id : {$nin: users_id}}
var usersFields = {name: 1}
MacBook-Pro-de-Leonardo(mongod-3.2.1) project-manager> db.users.find(usersQuery, usersFields)
{
  "_id": ObjectId("56ed3d76d8890eb2ccdcd6ed"),
  "name": "Andres Schamberger"
}
{
  "_id": ObjectId("56ed3d76d8890eb2ccdcd6ee"),
  "name": "Jamal Roberts"
}
{
  "_id": ObjectId("56ed3d76d8890eb2ccdcd6ef"),
  "name": "Demetris Kunze"
}
{
  "_id": ObjectId("56ed3d76d8890eb2ccdcd6f0"),
  "name": "Flo Blick"
}
{
  "_id": ObjectId("56ed3d76d8890eb2ccdcd6f1"),
  "name": "Lesly Goldner"
}
Fetched 5 record(s) in 1ms
```


## Update - alteração

#### 1. Adicione para todos os projetos o campo `views: 0`.

```
var query = {}
var mod = {$set: {views: 0}}
var opt = {multi: true}
MacBook-Pro-de-Leonardo(mongod-3.2.1) project-manager> db.projects.update(query, mod, opt)
Updated 5 existing record(s) in 6ms
WriteResult({
  "nMatched": 5,
  "nUpserted": 0,
  "nModified": 5
})
```

#### 2. Adicione 1 tag diferente para cada projeto.

```
var query = {}
var fields = {_id: 1, tags: 1}
var projects = db.projects.find(query, fields).toArray()
var new_tags = ['lula', 'dilma', 'dirceu', 'maluf', 'aécio']
projects.forEach(function(project, index){
  db.projects.update({_id: project._id}, {$push: {tags: new_tags[index]}})
})
Updated 1 existing record(s) in 1ms
Updated 1 existing record(s) in 1ms
Updated 1 existing record(s) in 1ms
Updated 1 existing record(s) in 0ms
Updated 1 existing record(s) in 0ms

MacBook-Pro-de-Leonardo(mongod-3.2.1) project-manager> var projects = db.projects.find(query, fields).toArray()
MacBook-Pro-de-Leonardo(mongod-3.2.1) project-manager> projects
[
  {
    "_id": ObjectId("56ed7914d8890eb2ccdcd70e"),
    "tags": [
      "instagram",
      "javascript",
      "cobol",
      "social media",
      "money",
      "be-mean",
      "lula"
    ]
  },
  {
    "_id": ObjectId("56ed7914d8890eb2ccdcd70f"),
    "tags": [
      "nodejs",
      "mongodb",
      "free",
      "course",
      "money",
      "be-mean",
      "dilma"
    ]
  },
  {
    "_id": ObjectId("56ed7914d8890eb2ccdcd710"),
    "tags": [
      "book",
      "read",
      "suissabooks",
      "best",
      "be-mean",
      "dirceu"
    ]
  },
  {
    "_id": ObjectId("56ed7914d8890eb2ccdcd711"),
    "tags": [
      "python",
      "pycourse",
      "beginner",
      "course",
      "cheap",
      "maluf"
    ]
  },
  {
    "_id": ObjectId("56ed7914d8890eb2ccdcd712"),
    "tags": [
      "php",
      "phpcourse",
      "frameworks",
      "zend",
      "laravel",
      "aécio"
    ]
  }
]
```

#### 3. Adicione 2 membros diferentes para cada projeto.
Primeiro, devemos recuperar os ID's dos membros que estão envolvidos com seu respectivo projeto

```js
var query = {}
var fields = {_id: 1, name: 1, 'members.user_id' : 1}
var projects = db.projects.find(query, fields).toArray()
var projects_with_members = []

projects.forEach(function(project){
  var pj = {
    id      : project._id,
    name    : project.name,
    members : []
  }

  project.members.forEach(function(member){
    pj.members.push(member.user_id)
  })

  projects_with_members.push(pj)
})
```

Agora, vamos iterar por essa lista de projetos com o array de ID's dos membros e recuperar a lista de usuários que não esteja naquele projeto, então limitaremos para apenas 2, onde retornará apenas 2 membros que não esteja neste projeto, logo após eles serão inclusos naquele projeto.

```js
projects_with_members.forEach(function(project){
  var members_non_part = db.users.find({_id: {$nin: project.members}}, {_id: 1}).limit(2).toArray()
  var new_members = [{
    user_id     : members_non_part[0]._id,
    type_member : "new member in this project",
    notify      : false,
  },
  {
    user_id     : members_non_part[1]._id,
    type_member : "I'm the new second member in this project",
    notify      : true,
  }]
  
  db.projects.update({_id: project.id}, {$push: {members: {$each: new_members}}})
})

Updated 1 existing record(s) in 1ms
Updated 1 existing record(s) in 1ms
Updated 1 existing record(s) in 1ms
Updated 1 existing record(s) in 0ms
Updated 1 existing record(s) in 0ms
```
#### 4. Adicione 1 comentário em cada atividade, deixe apenas 1 projeto sem.

Sabendo que temos um projeto que não há atividades, ficará assim

```
var query = {}
var fields = {name: 1, comments: 1}
var activities = db.activities.find(query, fields).toArray()

MBP-de-Leonardo(mongod-3.2.1) project-manager> activities.forEach(function(activity){
...   db.activities.update({_id: activity._id}, {$push: {comments: {text: 'New Comment!', date_comment: new Date(), members: []}}})
... })
Updated 1 existing record(s) in 1ms
Updated 1 existing record(s) in 1ms
Updated 1 existing record(s) in 1ms
Updated 1 existing record(s) in 1ms
Updated 1 existing record(s) in 1ms
Updated 1 existing record(s) in 1ms
Updated 1 existing record(s) in 1ms
Updated 1 existing record(s) in 0ms
```


#### 5. Adicione 1 projeto inteiro com **UPSERT**.

```JS
function addDaysFromDateNow(days){
  var result = new Date();
  result.setDate(result.getDate() + days);
  return result;
};

var users = db.users.find().toArray()
var project_members = [
  {
    user_id     : users[0]._id,
    type_member : ["Full Stack Developer", "Systems Analyst"],
    notify      : true,
  },
  {
    user_id     : users[2]._id,
    type_member : ["Python Developer"],
    notify      : true,
  },
  {
    user_id     : users[4]._id,
    type_member : ["Product Owner"],
    notify      : true,
  },
  {
    user_id     : users[6]._id,
    type_member : ["Scrum Master"],
    notify      : true,
  },
  {
    user_id     : users[8]._id,
    type_member : ["Trainee"],
    notify      : false,
  }
]

var project = {
  description: "Open Source Project, Powered by Be MEAN!",
  date_begin: addDaysFromDateNow(1),
  date_dream: addDaysFromDateNow(180),
  date_end: addDaysFromDateNow(360),
  visible: true,
  realocate: false,
  expired: false,
  visualizable_mod: null,
  members: project_members,
  tags: ["open-source", "free", "be-mean", "ecommerce", "paypal"],
  goals: [{
    name: "Payment Gateway",
    descrption: "Provider service that authorizes credit card payment",
    date_begin: addDaysFromDateNow(4),
    date_dream: addDaysFromDateNow(7),
    date_end: addDaysFromDateNow(12),
    realocate: false,
    expired: false,
    tags: ["credit-card", "paypal", "visa", "mastercard"],
    activities: [],
    historics: [addDaysFromDateNow(5), addDaysFromDateNow(6)]
  }],
  views: 0
}
```
**HORA DO UPDATE!**

```
var query = {name: 'E-commerce'}
var mod = {$setOnInsert: project}
var opt = {upsert: true}

MacBook-Pro-de-Leonardo(mongod-3.2.1) project-manager> db.projects.update(query, mod, opt)
Updated 1 new record(s) in 2ms
WriteResult({
  "nMatched": 0,
  "nUpserted": 1,
  "nModified": 0,
  "_id": ObjectId("56febb6b9484ef680faab512")
})
```

## Delete - remoção

#### 1. Apague todos os projetos que não possuam tags
(no meu projeto não possui nenhum projeto sem tags)
```
var query = {tags: {$size: 0}}
MacBook-Pro-de-Leonardo(mongod-3.2.1) project-manager> db.projects.remove(query)

Removed 0 record(s) in 1ms
WriteResult({
  "nRemoved": 0
})
```

#### 2. Apague todos os projetos que não possuam comentários nas atividades

```
var query = {comments: {$size: 0}}
var activities = db.activities.find(query).toArray()

activities.forEach(function(activity){
  db.projects.remove({'goals.activities': activity._id)})
})
```

#### 3. Apague todos os projetos que não possuam atividades

```
var query = {'goals.activities': {$size: 0}}
MacBook-Pro-de-Leonardo(mongod-3.2.1) project-manager> db.projects.remove(query)

Removed 1 record(s) in 0ms
WriteResult({
  "nRemoved": 1
})
```

#### 4. Escolha 2 usuários e apague todos os projetos em que os 2 fazem parte

```
var usersQuery = {}
var users = db.users.find(usersQuery, {_id: 1}).limit(2).toArray()

var query = {'members.user_id': {$all: [users[0]._id, users[1]._id]}}
MacBook-Pro-de-Leonardo(mongod-3.2.1) project-manager> db.projects.remove(query)

Removed 5 record(s) in 1ms
WriteResult({
  "nRemoved": 5
})
```

#### 5. Apague todos os projetos que possuam uma determinada tag em goal

(todos os projetos foram deletados anteriormente pelo fato de os 2 membros escolhidos fazerem parte dos projetos que havia)

A tag escolhida é: **easy**

```
var query = {'goals.tags': 'easy'}
MacBook-Pro-de-Leonardo(mongod-3.2.1) project-manager> db.projects.remove(query)

Removed 0 record(s) in 1ms
WriteResult({
  "nRemoved": 0
})
```

## Gerenciamento de usuários
#### 1. Crie um usuário com permissões **APENAS** de Leitura.

```
var credentials = {
  user: "billy",
  pwd: "idk",
  roles: [{role: "read", db: "project-manager"}]
}

> use admin
switched to db admin
> db.createUser(credentials)
Successfully added user: {
  "user": "billy",
  "roles": [
    {
      "role": "read",
      "db": "project-manager"
    }
  ]
}

```


#### 2. Crie um usuário com permissões de Escrita e Leitura.

``` 
var credentials = {
  user: "jean",
  pwd: "suissa",
  roles: [{role: "readWrite", db: "project-manager"}]
}

> use admin
switched to db admin
> db.createUser(credentials)
Successfully added user: {
  "user": "jean",
  "roles": [
    {
      "role": "readWrite",
      "db": "project-manager"
    }
  ]
}
```


#### 3. Adicionar o papel `grantRolesToUser` e `revokeRole` para o usuário com Escrita e Leitura.

###### grantRolesToUser

```
var role = {
	role: "grantRolesToUser",
  privileges: [{
    resource: { db: "admin", collection: ""},
    actions: ["grantRole"]
  }],
  roles: []
}

> use admin
switched to db admin
> db.createRole(role)
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
  
```

##### revokeRole
```
var role = {
  role: "revokeRole",
  privileges: [{
    resource: { db: "admin", collection: ""},
    actions: ["revokeRole"]
  }],
  roles: []
}

> use admin
switched to db admin
> db.createRole(role)
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

```

#### Adicionando os roles para o usuário jean
```
> use admin
switched to db admin
> db.grantRolesToUser("jean", ["grantRolesToUser", "revokeRole" ]);
> db.getUser("jean")
{
  "_id": "admin.jean",
  "user": "jean",
  "db": "admin",
  "roles": [
    {
      "role": "revokeRole",
      "db": "admin"
    },
    {
      "role": "grantRolesToUser",
      "db": "admin"
    },
    {
      "role": "readWrite",
      "db": "project-manager"
    }
  ]
}
```
#### 4. Remover o papel `grantRolesToUser` para o usuário com Escrita e Leitura.

```
> use admin
switched to db admin
> db.revokeRolesFromUser("jean", ["grantRolesToUser"])
> db.getUser("jean")
{
  "_id": "admin.jean",
  "user": "jean",
  "db": "admin",
  "roles": [
    {
      "role": "revokeRole",
      "db": "admin"
    },
    {
      "role": "readWrite",
      "db": "project-manager"
    }
  ]
}

```

#### 5. Listar todos os usuários com seus papéis e ações.

```
> use admin
switched to db admin
> db.getUser("jean", {showCredentials: true, showPrivileges: true})
{
  "_id": "admin.jean",
  "user": "jean",
  "db": "admin",
  "credentials": {
    "SCRAM-SHA-1": {
      "iterationCount": 10000,
      "salt": "MIKV2koQr8q5jD6TKkS6qg==",
      "storedKey": "H4zPZ5z1IlAAPh35bOSYFN5rU4M=",
      "serverKey": "fjPmZpXCH4+HIUGaM36YDV+wR+Q="
    }
  },
  "roles": [
    {
      "role": "revokeRole",
      "db": "admin"
    },
    {
      "role": "readWrite",
      "db": "project-manager"
    }
  ],
  "inheritedRoles": [
    {
      "role": "readWrite",
      "db": "project-manager"
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
        "revokeRole"
      ]
    },
    {
      "resource": {
        "db": "project-manager",
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
        "db": "project-manager",
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
        "db": "project-manager",
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
        "db": "project-manager",
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

> db.getUser("billy", {showCredentials: true, showPrivileges: true})
{
  "_id": "admin.billy",
  "user": "billy",
  "db": "admin",
  "credentials": {
    "SCRAM-SHA-1": {
      "iterationCount": 10000,
      "salt": "/LKpifg079CS6eex/tF53Q==",
      "storedKey": "qEHyyLGoHixX1wJPH2FHFA/3ZZ8=",
      "serverKey": "MMtlyXASMuUJpdv+LO3ljAW8Yqo="
    }
  },
  "roles": [
    {
      "role": "read",
      "db": "project-manager"
    }
  ],
  "inheritedRoles": [
    {
      "role": "read",
      "db": "project-manager"
    }
  ],
  "inheritedPrivileges": [
    {
      "resource": {
        "db": "project-manager",
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
        "db": "project-manager",
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
        "db": "project-manager",
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
        "db": "project-manager",
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
```

## Cluster

### 1. Config Server

Será escalado **3 Config Servers**, assim não correremos o risco se algum falhar, terá uma contigência.
(simulando uma situação real).



#### 1.1. Config Server - Porta: 27000

```
leo@MacBook-Pro-de-Leonardo ~ mkdir -p /data/configdb
leo@MacBook-Pro-de-Leonardo ~ mongod --configsvr --replSet configReplSet --port 27000 --dbpath /data/configdb
2016-03-31T13:24:40.674-0300 I CONTROL  [initandlisten] MongoDB starting : pid=8250 port=27000 dbpath=/data/configdb 64-bit host=MacBook-Pro-de-Leonardo.local
2016-03-31T13:24:40.675-0300 I CONTROL  [initandlisten] db version v3.2.1
2016-03-31T13:24:40.675-0300 I CONTROL  [initandlisten] git version: a14d55980c2cdc565d4704a7e3ad37e4e535c1b2
2016-03-31T13:24:40.675-0300 I CONTROL  [initandlisten] allocator: system
2016-03-31T13:24:40.675-0300 I CONTROL  [initandlisten] modules: none
2016-03-31T13:24:40.675-0300 I CONTROL  [initandlisten] build environment:
2016-03-31T13:24:40.675-0300 I CONTROL  [initandlisten]     distarch: x86_64
2016-03-31T13:24:40.675-0300 I CONTROL  [initandlisten]     target_arch: x86_64
2016-03-31T13:24:40.675-0300 I CONTROL  [initandlisten] options: { net: { port: 27000 }, replication: { replSet: "configReplSet" }, sharding: { clusterRole: "configsvr" }, storage: { dbPath: "/data/configdb" } }
2016-03-31T13:24:40.676-0300 I -        [initandlisten] Detected data files in /data/configdb created by the 'wiredTiger' storage engine, so setting the active storage engine to 'wiredTiger'.
2016-03-31T13:24:40.676-0300 W -        [initandlisten] Detected unclean shutdown - /data/configdb/mongod.lock is not empty.
2016-03-31T13:24:40.676-0300 W STORAGE  [initandlisten] Recovering data from the last clean checkpoint.
2016-03-31T13:24:40.679-0300 I STORAGE  [initandlisten] wiredtiger_open config: create,cache_size=9G,session_max=20000,eviction=(threads_max=4),config_base=false,statistics=(fast),log=(enabled=true,archive=true,path=journal,compressor=snappy),file_manager=(close_idle_time=100000),checkpoint=(wait=60,log_size=2GB),statistics_log=(wait=0),
2016-03-31T13:24:41.474-0300 I STORAGE  [initandlisten] Starting WiredTigerRecordStoreThread local.oplog.$main
2016-03-31T13:24:41.474-0300 I STORAGE  [initandlisten] The size storer reports that the oplog contains 268 records totaling to 29120 bytes
2016-03-31T13:24:41.474-0300 I STORAGE  [initandlisten] Scanning the oplog to determine where to place markers for truncation
2016-03-31T13:24:41.487-0300 I CONTROL  [initandlisten]
2016-03-31T13:24:41.487-0300 I CONTROL  [initandlisten] ** WARNING: soft rlimits too low. Number of files is 256, should be at least 1000
2016-03-31T13:24:41.491-0300 I REPL     [initandlisten] Did not find local voted for document at startup;  NoMatchingDocument Did not find replica set lastVote document in local.replset.election
2016-03-31T13:24:41.491-0300 I REPL     [initandlisten] Did not find local replica set configuration document at startup;  NoMatchingDocument Did not find replica set configuration document in local.system.replset
2016-03-31T13:24:41.492-0300 I FTDC     [initandlisten] Initializing full-time diagnostic data capture with directory '/data/configdb/diagnostic.data'
2016-03-31T13:24:41.492-0300 I NETWORK  [HostnameCanonicalizationWorker] Starting hostname canonicalization worker
2016-03-31T13:24:41.492-0300 I NETWORK  [initandlisten] waiting for connections on port 27000
2016-03-31T13:24:42.006-0300 I FTDC     [ftdc] Unclean full-time diagnostic data capture shutdown detected, found interim file, some metrics may have been lost. OK
```

#### 1.2. Config Server - Porta: 27001 

```
leo@MacBook-Pro-de-Leonardo ~ mkdir -p /data/configdb2
leo@MacBook-Pro-de-Leonardo ~ mongod --configsvr --replSet configReplSet --port 27001 --dbpath /data/configdb2
2016-03-31T13:23:27.660-0300 I CONTROL  [initandlisten] MongoDB starting : pid=8169 port=27001 dbpath=/data/configdb2 64-bit host=MacBook-Pro-de-Leonardo.local
2016-03-31T13:23:27.661-0300 I CONTROL  [initandlisten] db version v3.2.1
2016-03-31T13:23:27.661-0300 I CONTROL  [initandlisten] git version: a14d55980c2cdc565d4704a7e3ad37e4e535c1b2
2016-03-31T13:23:27.661-0300 I CONTROL  [initandlisten] allocator: system
2016-03-31T13:23:27.661-0300 I CONTROL  [initandlisten] modules: none
2016-03-31T13:23:27.661-0300 I CONTROL  [initandlisten] build environment:
2016-03-31T13:23:27.661-0300 I CONTROL  [initandlisten]     distarch: x86_64
2016-03-31T13:23:27.661-0300 I CONTROL  [initandlisten]     target_arch: x86_64
2016-03-31T13:23:27.661-0300 I CONTROL  [initandlisten] options: { net: { port: 27001 }, replication: { replSet: "configReplSet" }, sharding: { clusterRole: "configsvr" }, storage: { dbPath: "/data/configdb2" } }
2016-03-31T13:23:27.662-0300 I STORAGE  [initandlisten] wiredtiger_open config: create,cache_size=9G,session_max=20000,eviction=(threads_max=4),config_base=false,statistics=(fast),log=(enabled=true,archive=true,path=journal,compressor=snappy),file_manager=(close_idle_time=100000),checkpoint=(wait=60,log_size=2GB),statistics_log=(wait=0),
2016-03-31T13:23:27.913-0300 I CONTROL  [initandlisten]
2016-03-31T13:23:27.913-0300 I CONTROL  [initandlisten] ** WARNING: soft rlimits too low. Number of files is 256, should be at least 1000
2016-03-31T13:23:27.934-0300 I REPL     [initandlisten] Did not find local voted for document at startup;  NoMatchingDocument Did not find replica set lastVote document in local.replset.election
2016-03-31T13:23:27.934-0300 I REPL     [initandlisten] Did not find local replica set configuration document at startup;  NoMatchingDocument Did not find replica set configuration document in local.system.replset
2016-03-31T13:23:27.934-0300 I FTDC     [initandlisten] Initializing full-time diagnostic data capture with directory '/data/configdb2/diagnostic.data'
2016-03-31T13:23:27.934-0300 I NETWORK  [HostnameCanonicalizationWorker] Starting hostname canonicalization worker
2016-03-31T13:23:27.957-0300 I NETWORK  [initandlisten] waiting for connections on port 27001
```

#### 1.3. Config Server - Porta: 27002

```
leo@MacBook-Pro-de-Leonardo ~ mkdir -p /data/configdb3
leo@MacBook-Pro-de-Leonardo ~ mongod --configsvr --replSet configReplSet --port 27002 --dbpath /data/configdb3
2016-03-31T13:23:43.322-0300 I CONTROL  [initandlisten] MongoDB starting : pid=8218 port=27002 dbpath=/data/configdb3 64-bit host=MacBook-Pro-de-Leonardo.local
2016-03-31T13:23:43.323-0300 I CONTROL  [initandlisten] db version v3.2.1
2016-03-31T13:23:43.323-0300 I CONTROL  [initandlisten] git version: a14d55980c2cdc565d4704a7e3ad37e4e535c1b2
2016-03-31T13:23:43.323-0300 I CONTROL  [initandlisten] allocator: system
2016-03-31T13:23:43.323-0300 I CONTROL  [initandlisten] modules: none
2016-03-31T13:23:43.323-0300 I CONTROL  [initandlisten] build environment:
2016-03-31T13:23:43.323-0300 I CONTROL  [initandlisten]     distarch: x86_64
2016-03-31T13:23:43.323-0300 I CONTROL  [initandlisten]     target_arch: x86_64
2016-03-31T13:23:43.323-0300 I CONTROL  [initandlisten] options: { net: { port: 27002 }, replication: { replSet: "configReplSet" }, sharding: { clusterRole: "configsvr" }, storage: { dbPath: "/data/configdb3" } }
2016-03-31T13:23:43.323-0300 I STORAGE  [initandlisten] wiredtiger_open config: create,cache_size=9G,session_max=20000,eviction=(threads_max=4),config_base=false,statistics=(fast),log=(enabled=true,archive=true,path=journal,compressor=snappy),file_manager=(close_idle_time=100000),checkpoint=(wait=60,log_size=2GB),statistics_log=(wait=0),
2016-03-31T13:23:43.581-0300 I CONTROL  [initandlisten]
2016-03-31T13:23:43.581-0300 I CONTROL  [initandlisten] ** WARNING: soft rlimits too low. Number of files is 256, should be at least 1000
2016-03-31T13:23:43.605-0300 I REPL     [initandlisten] Did not find local voted for document at startup;  NoMatchingDocument Did not find replica set lastVote document in local.replset.election
2016-03-31T13:23:43.605-0300 I REPL     [initandlisten] Did not find local replica set configuration document at startup;  NoMatchingDocument Did not find replica set configuration document in local.system.replset
2016-03-31T13:23:43.605-0300 I FTDC     [initandlisten] Initializing full-time diagnostic data capture with directory '/data/configdb3/diagnostic.data'
2016-03-31T13:23:43.605-0300 I NETWORK  [HostnameCanonicalizationWorker] Starting hostname canonicalization worker
2016-03-31T13:23:43.629-0300 I NETWORK  [initandlisten] waiting for connections on port 27002
```

Agora, devemos iniciar nossa replicaset dos nossos servers de config, para que possamos conectar nosso router.

```
leo@MacBook-Pro-de-Leonardo ~ mongo --host localhost --port 27000
MongoDB shell version: 3.2.1
connecting to: localhost:27000/test
Mongo-Hacker 0.0.13
Server has startup warnings:
2016-03-31T13:24:41.487-0300 I CONTROL  [initandlisten]
2016-03-31T13:24:41.487-0300 I CONTROL  [initandlisten] ** WARNING: soft rlimits too low. Number of files is 256, should be at least 1000
MacBook-Pro-de-Leonardo(mongod-3.2.1) test> rs.initiate( {
...    _id: "configReplSet",
...    configsvr: true,
...    members: [
...       { _id: 0, host: "localhost:27000" },
...       { _id: 1, host: "localhost:27001" },
...       { _id: 2, host: "localhost:27002" }
...    ]
... } )
{
  "ok": 1
}
```

### 2. Router

```
leo@MacBook-Pro-de-Leonardo ~ mongos --configdb configReplSet/localhost:27000,localhost:27001,localhost:27002 --port 27010
2016-03-31T14:00:58.132-0300 I SHARDING [mongosMain] MongoS version 3.2.1 starting: pid=8855 port=27010 64-bit host=MacBook-Pro-de-Leonardo.local (--help for usage)
2016-03-31T14:00:58.132-0300 I CONTROL  [mongosMain] db version v3.2.1
2016-03-31T14:00:58.132-0300 I CONTROL  [mongosMain] git version: a14d55980c2cdc565d4704a7e3ad37e4e535c1b2
2016-03-31T14:00:58.132-0300 I CONTROL  [mongosMain] allocator: system
2016-03-31T14:00:58.132-0300 I CONTROL  [mongosMain] modules: none
2016-03-31T14:00:58.132-0300 I CONTROL  [mongosMain] build environment:
2016-03-31T14:00:58.132-0300 I CONTROL  [mongosMain]     distarch: x86_64
2016-03-31T14:00:58.132-0300 I CONTROL  [mongosMain]     target_arch: x86_64
2016-03-31T14:00:58.132-0300 I CONTROL  [mongosMain] options: { net: { port: 27010 }, sharding: { configDB: "configReplSet/localhost:27000,localhost:27001,localhost:27002" } }
2016-03-31T14:00:58.132-0300 I SHARDING [mongosMain] Updating config server connection string to: configReplSet/localhost:27000,localhost:27001,localhost:27002
2016-03-31T14:00:58.132-0300 I NETWORK  [mongosMain] Starting new replica set monitor for configReplSet/localhost:27000,localhost:27001,localhost:27002
2016-03-31T14:00:58.133-0300 I NETWORK  [ReplicaSetMonitorWatcher] starting
2016-03-31T14:00:58.141-0300 I SHARDING [thread1] creating distributed lock ping thread for process MacBook-Pro-de-Leonardo.local:27010:1459443658:-1838057800 (sleeping for 30000ms)
2016-03-31T14:00:58.144-0300 I ASIO     [NetworkInterfaceASIO-ShardRegistry-0] Successfully connected to localhost:27000
2016-03-31T14:00:58.145-0300 I ASIO     [NetworkInterfaceASIO-ShardRegistry-0] Successfully connected to localhost:27000
2016-03-31T14:00:58.154-0300 W SHARDING [replSetDistLockPinger] pinging failed for distributed lock pinger :: caused by :: LockStateChangeFailed findAndModify query predicate didn't match any lock document
2016-03-31T14:00:58.156-0300 I NETWORK  [HostnameCanonicalizationWorker] Starting hostname canonicalization worker
2016-03-31T14:00:58.156-0300 I SHARDING [Balancer] about to contact config servers and shards
2016-03-31T14:00:58.158-0300 I NETWORK  [mongosMain] waiting for connections on port 27010 
```

### 3. Shardings


```
leo@MacBook-Pro-de-Leonardo ~ mkdir /data/shard1 && mkdir /data/shard2 && mkdir /data/shard3
```

#### SHARD 1 - PORT: 27011

```
leo@MacBook-Pro-de-Leonardo ~ mongod --port 27011 --dbpath /data/shard1
2016-03-31T13:48:21.391-0300 I CONTROL  [initandlisten] MongoDB starting : pid=8549 port=27011 dbpath=/data/shard1 64-bit host=MacBook-Pro-de-Leonardo.local
2016-03-31T13:48:21.391-0300 I CONTROL  [initandlisten] db version v3.2.1
2016-03-31T13:48:21.391-0300 I CONTROL  [initandlisten] git version: a14d55980c2cdc565d4704a7e3ad37e4e535c1b2
2016-03-31T13:48:21.392-0300 I CONTROL  [initandlisten] allocator: system
2016-03-31T13:48:21.392-0300 I CONTROL  [initandlisten] modules: none
2016-03-31T13:48:21.392-0300 I CONTROL  [initandlisten] build environment:
2016-03-31T13:48:21.392-0300 I CONTROL  [initandlisten]     distarch: x86_64
2016-03-31T13:48:21.392-0300 I CONTROL  [initandlisten]     target_arch: x86_64
2016-03-31T13:48:21.392-0300 I CONTROL  [initandlisten] options: { net: { port: 27011 }, storage: { dbPath: "/data/shard1" } }
2016-03-31T13:48:21.392-0300 I STORAGE  [initandlisten] wiredtiger_open config: create,cache_size=9G,session_max=20000,eviction=(threads_max=4),config_base=false,statistics=(fast),log=(enabled=true,archive=true,path=journal,compressor=snappy),file_manager=(close_idle_time=100000),checkpoint=(wait=60,log_size=2GB),statistics_log=(wait=0),
2016-03-31T13:48:21.680-0300 I CONTROL  [initandlisten]
2016-03-31T13:48:21.680-0300 I CONTROL  [initandlisten] ** WARNING: soft rlimits too low. Number of files is 256, should be at least 1000
2016-03-31T13:48:21.680-0300 I FTDC     [initandlisten] Initializing full-tim2016-03-31T13:48:21.707-0300 I NETWORK  [initandlisten] waiting for connections on port 27011
```
#### SHARD 2 - PORT: 27012

```
leo@MacBook-Pro-de-Leonardo ~ mongod --port 27012 --dbpath /data/shard2
2016-03-31T13:48:32.199-0300 I CONTROL  [initandlisten] MongoDB starting : pid=8589 port=27012 dbpath=/data/shard2 64-bit host=MacBook-Pro-de-Leonardo.local
2016-03-31T13:48:32.200-0300 I CONTROL  [initandlisten] db version v3.2.1
2016-03-31T13:48:32.200-0300 I CONTROL  [initandlisten] git version: a14d55980c2cdc565d4704a7e3ad37e4e535c1b2
2016-03-31T13:48:32.200-0300 I CONTROL  [initandlisten] allocator: system
2016-03-31T13:48:32.200-0300 I CONTROL  [initandlisten] modules: none
2016-03-31T13:48:32.200-0300 I CONTROL  [initandlisten] build environment:
2016-03-31T13:48:32.200-0300 I CONTROL  [initandlisten]     distarch: x86_64
2016-03-31T13:48:32.200-0300 I CONTROL  [initandlisten]     target_arch: x86_64
2016-03-31T13:48:32.200-0300 I CONTROL  [initandlisten] options: { net: { port: 27012 }, storage: { dbPath: "/data/shard2" } }
2016-03-31T13:48:32.201-0300 I STORAGE  [initandlisten] wiredtiger_open config: create,cache_size=9G,session_max=20000,eviction=(threads_max=4),config_base=false,statistics=(fast),log=(enabled=true,archive=true,path=journal,compressor=snappy),file_manager=(close_idle_time=100000),checkpoint=(wait=60,log_size=2GB),statistics_log=(wait=0),
2016-03-31T13:48:32.454-0300 I CONTROL  [initandlisten]
2016-03-31T13:48:32.454-0300 I CONTROL  [initandlisten] ** WARNING: soft rlimits too low. Number of files is 256, should be at least 1000
2016-03-31T13:48:32.455-0300 I FTDC     [initandlisten] Initializing full-ti2016-03-31T13:48:32.478-0300 I NETWORK  [initandlisten] waiting for connections on port 27012
```

#### SHARD 3 - PORT: 27013

```
leo@MacBook-Pro-de-Leonardo ~ mongod --port 27013 --dbpath /data/shard3
2016-03-31T13:48:47.951-0300 I CONTROL  [initandlisten] MongoDB starting : pid=8630 port=27013 dbpath=/data/shard3 64-bit host=MacBook-Pro-de-Leonardo.local
2016-03-31T13:48:47.951-0300 I CONTROL  [initandlisten] db version v3.2.1
2016-03-31T13:48:47.951-0300 I CONTROL  [initandlisten] git version: a14d55980c2cdc565d4704a7e3ad37e4e535c1b2
2016-03-31T13:48:47.951-0300 I CONTROL  [initandlisten] allocator: system
2016-03-31T13:48:47.951-0300 I CONTROL  [initandlisten] modules: none
2016-03-31T13:48:47.952-0300 I CONTROL  [initandlisten] build environment:
2016-03-31T13:48:47.952-0300 I CONTROL  [initandlisten]     distarch: x86_64
2016-03-31T13:48:47.952-0300 I CONTROL  [initandlisten]     target_arch: x86_64
2016-03-31T13:48:47.952-0300 I CONTROL  [initandlisten] options: { net: { port: 27013 }, storage: { dbPath: "/data/shard3" } }
2016-03-31T13:48:47.952-0300 I STORAGE  [initandlisten] wiredtiger_open config: create,cache_size=9G,session_max=20000,eviction=(threads_max=4),config_base=false,statistics=(fast),log=(enabled=true,archive=true,path=journal,compressor=snappy),file_manager=(close_idle_time=100000),checkpoint=(wait=60,log_size=2GB),statistics_log=(wait=0),
2016-03-31T13:48:48.231-0300 I CONTROL  [initandlisten]
2016-03-31T13:48:48.231-0300 I CONTROL  [initandlisten] ** WARNING: soft rlimits too low. Number of files is 256, should be at least 1000
2016-03-31T13:48:48.231-0300 I FTDC     [initandlisten] Initializing full-time diagnostic data capture with directory '/data/shard3/diagnostic.data'
2016-03-31T13:48:48.231-0300 I NETWORK  [HostnameCanonicalizationWorker] Starting hostname canonicalization worker
2016-03-31T13:48:48.263-0300 I NETWORK  [initandlisten] waiting for connections on port 27013
```

### 4. Configurações Finais

#### 4.1 - Registrando as shards no router


```
leo@MacBook-Pro-de-Leonardo ~ mongo --port 27010 --host localhost
MongoDB shell version: 3.2.1
connecting to: localhost:27010/test
Mongo-Hacker 0.0.13
MacBook-Pro-de-Leonardo(mongos-3.2.1)[mongos] test> sh.addShard("localhost:27011")
{
  "shardAdded": "shard0000",
  "ok": 1
}
MacBook-Pro-de-Leonardo(mongos-3.2.1)[mongos] test> sh.addShard("localhost:27012")
{
  "shardAdded": "shard0001",
  "ok": 1
}
MacBook-Pro-de-Leonardo(mongos-3.2.1)[mongos] test> sh.addShard("localhost:27013")
{
  "shardAdded": "shard0002",
  "ok": 1
}
```

#### 4.2 - Definindo qual **db** iremos shardear


```
leo@MacBook-Pro-de-Leonardo ~ mongo --port 27010 --host localhost
MongoDB shell version: 3.2.1
connecting to: localhost:27010/test
Mongo-Hacker 0.0.13
MacBook-Pro-de-Leonardo(mongos-3.2.1)[mongos] test> sh.enableSharding("project-manager")
{
  "ok": 1
}

```

#### 4.3 - Definindo qual **collection** será shardeada

```
leo@MacBook-Pro-de-Leonardo ~ mongo --port 27010 --host localhost
MongoDB shell version: 3.2.1
connecting to: localhost:27010/test
Mongo-Hacker 0.0.13
MacBook-Pro-de-Leonardo(mongos-3.2.1)[mongos] test> sh.shardCollection("project-manager.activities", { "_id": 1 })
{
  "collectionsharded": "project-manager.activities",
  "ok": 1
}

```

## 5. Réplica

### 5.1 Replica 1 - Port: 27099

```
leo@MacBook-Pro-de-Leonardo ~  mongod --replSet replica_set --port 27099 --dbpath /data/rsPM1
2016-03-31T14:24:55.434-0300 I CONTROL  [initandlisten] MongoDB starting : pid=8964 port=27099 dbpath=/data/rsPM1 64-bit host=MacBook-Pro-de-Leonardo.local
2016-03-31T14:24:55.435-0300 I CONTROL  [initandlisten] db version v3.2.1
2016-03-31T14:24:55.435-0300 I CONTROL  [initandlisten] git version: a14d55980c2cdc565d4704a7e3ad37e4e535c1b2
2016-03-31T14:24:55.435-0300 I CONTROL  [initandlisten] allocator: system
2016-03-31T14:24:55.435-0300 I CONTROL  [initandlisten] modules: none
2016-03-31T14:24:55.435-0300 I CONTROL  [initandlisten] build environment:
2016-03-31T14:24:55.435-0300 I CONTROL  [initandlisten]     distarch: x86_64
2016-03-31T14:24:55.435-0300 I CONTROL  [initandlisten]     target_arch: x86_64
2016-03-31T14:24:55.435-0300 I CONTROL  [initandlisten] options: { net: { port: 27099 }, replication: { replSet: "replica_set" }, storage: { dbPath: "/data/rsPM1" } }
2016-03-31T14:24:55.436-0300 I STORAGE  [initandlisten] wiredtiger_open config: create,cache_size=9G,session_max=20000,eviction=(threads_max=4),config_base=false,statistics=(fast),log=(enabled=true,archive=true,path=journal,compressor=snappy),file_manager=(close_idle_time=100000),checkpoint=(wait=60,log_size=2GB),statistics_log=(wait=0),
2016-03-31T14:24:55.701-0300 I CONTROL  [initandlisten]
2016-03-31T14:24:55.701-0300 I CONTROL  [initandlisten] ** WARNING: soft rlimits too low. Number of files is 256, should be at least 1000
2016-03-31T14:24:55.723-0300 I REPL     [initandlisten] Did not find local voted for document at startup;  NoMatchingDocument Did not find replica set lastVote document in local.replset.election
2016-03-31T14:24:55.723-0300 I REPL     [initandlisten] Did not find local replica set configuration document at startup;  NoMatchingDocument Did not find replica set configuration document in local.system.replset
2016-03-31T14:24:55.724-0300 I NETWORK  [HostnameCanonicalizationWorker] Starting hostname canonicalization worker
2016-03-31T14:24:55.724-0300 I FTDC     [initandlisten] Initializing full-time diagnostic data capture with directory '/data/rsPM1/diagnostic.data'
2016-03-31T14:24:55.746-0300 I NETWORK  [initandlisten] waiting for connections on port 27099
```

### 5.2 - Configurando e Inicializando

```
leo@MacBook-Pro-de-Leonardo ~ mongo --port 27099 --host localhost
MongoDB shell version: 3.2.1
connecting to: localhost:27099/test
Mongo-Hacker 0.0.13
Server has startup warnings:
2016-03-31T14:24:55.701-0300 I CONTROL  [initandlisten]
2016-03-31T14:24:55.701-0300 I CONTROL  [initandlisten] ** WARNING: soft rlimits too low. Number of files is 256, should be at least 1000
MacBook-Pro-de-Leonardo(mongod-3.2.1) test> rs.initiate({_id: "replica_set", members: [{_id: 0, host: "127.0.0.1:27099"}]})
{
  "ok": 1
}
MacBook-Pro-de-Leonardo(mongod-3.2.1)[PRIMARY:replica_set] test> rs.add("127.0.0.1:28099")
{
  "ok": 1
}
MacBook-Pro-de-Leonardo(mongod-3.2.1)[PRIMARY:replica_set] test> rs.add("127.0.0.1:29099")
{
  "ok": 1
}
```

#### 5.2.1 - Testando conexões


#####**Replica 1 (Port: 27099):**

```
leo@MacBook-Pro-de-Leonardo ~ mongo --port 27099 --host localhost
MongoDB shell version: 3.2.1
connecting to: localhost:27099/test
Mongo-Hacker 0.0.13
Server has startup warnings:
2016-03-31T15:28:30.725-0300 I CONTROL  [initandlisten]
2016-03-31T15:28:30.725-0300 I CONTROL  [initandlisten] ** WARNING: soft rlimits too low. Number of files is 256, should be at least 1000
MacBook-Pro-de-Leonardo(mongod-3.2.1)[PRIMARY:replica_set] test>
```

#####**Replica 2 (Port: 28099):**

```
leo@MacBook-Pro-de-Leonardo ~ mongo --port 28099 --host localhost
MongoDB shell version: 3.2.1
connecting to: localhost:28099/test
Mongo-Hacker 0.0.13
Server has startup warnings:
2016-03-31T15:29:58.872-0300 I CONTROL  [initandlisten]
2016-03-31T15:29:58.872-0300 I CONTROL  [initandlisten] ** WARNING: soft rlimits too low. Number of files is 256, should be at least 1000
MacBook-Pro-de-Leonardo(mongod-3.2.1)[SECONDARY:replica_set] test>
```

#####**Replica 3 (Port: 29099):**

```
leo@MacBook-Pro-de-Leonardo ~ mongo --port 29099 --host localhost
MongoDB shell version: 3.2.1
connecting to: localhost:29099/test
Mongo-Hacker 0.0.13
Server has startup warnings:
2016-03-31T15:30:14.290-0300 I CONTROL  [initandlisten]
2016-03-31T15:30:14.290-0300 I CONTROL  [initandlisten] ** WARNING: soft rlimits too low. Number of files is 256, should be at least 1000
```


##Estrutura


![image](http://i.imgur.com/o4Vij8s.png =600x250)
![image](http://3.bp.blogspot.com/-Oc-3D9nmU6Q/T8DcysCTZ9I/AAAAAAAAH0I/WIiOQZbtnZ8/s200/olhos-brilhando-meme.gif)