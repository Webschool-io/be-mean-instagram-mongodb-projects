# Projeto Final

**Autor:** Leonardo Nascimento de Oliveira

**Data: 1450742400000**

## Para qual sistema você usaria o MogoDB (diferente desse)?

* Ecommerce
* CMS
* ERP

## Qual a modelagem da sua coleção de `users`?

```JS
/*
+--------------------- +
|          User        |
+----------------------+
*/
user : {

  name        : String ,
  bio         : String ,
  register    : Date   ,
  avatar_path : String ,
  
  settings : [
    { background-path : String }
  ],

  auth : {
    username    : String   ,
    email       : String   ,
    passsword   : Password ,
    last-access : Date     ,
    status      : Boolean  ,
    hash_token  : String   ,
    online      : Boolean
  }
  
}
```

## Qual a modelagem da sua coleção de `projects`?

```JS
/*
+--------------------- +
|      Projects        |
+----------------------+
*/
project : { 

  name             : String  ,
  description      : String  ,
  date_begin       : Date    ,
  date_dream       : Date    ,
  date_end         : Date    ,
  visible          : Boolean ,
  realocate        : Boolean ,
  expired          : Boolean ,
  visualizable_mod : String  , 
  
  members : [
    {
      user_id      : ObjectId(),
      name         : String    ,
      avathar_path : String    ,
      type_name    : String    ,
      avatar_path  : String    ,
      notify       : Boolean
    }
  ],

  goal : [{

    realocate          : Boolean ,
    tags               : []      ,

    historic_realocate : [
      { date_realocate : Date }
    ],

    activities         : [
      { 
        activities_id : ObjectId(),
        name_activity : String 
      }
    ]
  }],

  tags : [],

};
```

## Qual a modelagem da sua coleção retirada de `projects`?

```JS
activities : {

  name           : String  ,
  description    : String  ,
  date_begin     : Date    ,
  date_dream     : Date    ,
  date_end       : Date    ,
  realocate      : Boolean ,
  expired        : Boolean ,
  tags           : [],

  historic_realocate : [
    {date_realocate : Date}
  ],

  members        : [{
    user_id      : ObjectId(),
    name         : String  ,
    avathar_path : String  ,
    type_name    : String  ,
    avatar_path  : String  ,
    notify       : Boolean
  }],

  comments : [
    text : String, 
    date_comment : Date

    file : [{
      path   : String ,
      weight : int    ,
      name   : String
    }],

    member : {
      user_id      : ObjectId(),
      name         : String    ,
      avathar_path : String    ,
      type_name    : String    ,
      avatar_path  : String    ,
      notify       : Boolean
    },

  }]

};

```

## Create - Cadastro

### 1. Cadastre até 10 usuários diferentes. 

```JS

function randomDate(start, end) {
  return new Date(start.getTime() + Math.random() * (end.getTime() - start.getTime()));
};

function insertUser ( users ) {
  for ( var i = 0; i < users.length; i++ ) {
    var doc = {
      name         : users[i],
      bio          : "lorem bio",
      register     : randomDate(new Date(2012, 0, 1), new Date()),
      avatar_path : "http://nineprovince.org/wp-content/uploads/2015/08/no-image.png",
      settings     : [
        { "background-path" : "http://background.com" }
      ],
      auth  : {
        username    : users[i].toLowerCase().replace(/\s/g,''),
        email       : users[i].toLowerCase().replace(/\s/g,'')+"@gmail.com",
        passsword   : "123456",
        lastaccess  : randomDate(new Date(2015, 0, 1), new Date()).toISOString(),
        status      : true,
        hash_token  : "hkwow656asd6565sdsd6s7898sdsd9a12dsad"
      }
    };

    db.users.insert(doc);
  };
};

var users = [ 
  "Braidy Jools", 
  "Shirley", 
  "Connie", 
  "Anthonyson", 
  "Wisdom", 
  "Mel", 
  "Dickenson",
  "Dana Lindsay", 
  "Parris Hildred", 
  "Skylar Tommie Rowe"
];

leonardo(mongod-2.6.11) project> insertUser(users);

/* 
Inserted 1 record(s) in 7ms
Inserted 1 record(s) in 1ms
Inserted 1 record(s) in 89ms
Inserted 1 record(s) in 2ms
Inserted 1 record(s) in 62ms
Inserted 1 record(s) in 2ms
Inserted 1 record(s) in 1ms
Inserted 1 record(s) in 2ms
Inserted 1 record(s) in 3ms
Inserted 1 record(s) in 3ms

leonardo(mongod-2.6.11) project> db.user.count();
10
*/
```

### 2. Cadastre 5 projetos diferentes.

* Cada projeto deve possui 5 membros, sempre diferentes dentro dos projetos;
* Cada um com pelo menos 3 tags diferentes;
* Escolha 1 tag onde deva ficar em 2 projetos;
* Escolha 1 tag onde deva ficar em 3 projetos;
* Cada projeto com pelo menos 1 goal;
* Cada goal com pelo menos 3 tags;
* Cada goal com pelo menos 2 atividades, deixe 1 projeto sem.

```JS
/**
 * 
 * members: Guarda array que vai com 8 ids de users da consulta
 *
 **/
var members = [];

/*
 *
 * pega um id aleatório de um usuáiro e insere no array members
 * 
 **/

for ( var iMember = 7; iMember >= 0; iMember-- ) {

  var collection = db.users;
  
  var rand = function(){
    return Math.floor( Math.random() * collection.count() )
  };

  var query  = {};
  var fields = {};
  var member = collection.find(query, fields).limit(5).skip(rand()).next();
  var schemaMember = {
    user_id      : member._id,
    name         : member.name,
    avatar_path  : member.avatar_path,
    type_name    : "sometype",
    notify       : false
  }; 
  members.push(schemaMember);

};

/*
 *
 * onlyUnique: recebe o array members e filtra os ids únicos evitando repetições
 *  
 **/
function onlyUnique(value, index, self) { 
  return self.indexOf(value) === index;
}

var unique = members.filter( onlyUnique ); 

/*
 *
 * unique.slice: recortamos o array agora com 5 users
 *  
 **/
members = unique.slice(0,5);

/*
 *
 * forEach: gera cada membro que é inserido no array generateMember
 *
 **/
var generateMember = [];

members.forEach( function (el,idx) {
  generateMember.push(el);
});


/*
 *
 * Guarda todas as tags do projeto
 *
 **/
var all_tags = [
  [ "erp"      ,"dashboard"      , "help-desk"  , "web system" ],
  [ "ecommerce","seo"            , "shopcart"   , "web system" ],
  [ "forum"    ,"hep-desk"       , "faq"        , "web system" ],
  [ "blog"     ,"static generate", "single-page","simple site" ],
  [ "locadora" ,"catalogo"       , "filmes"     , "simple site"]
];

/*
 *
 * Cadastra 7 atividades
 *
 **/
for (var i = 0; i < 8 ; i++) {
  var query = {
    name               : "Activity" + i,
    description        : "This is a some Activity" + i,
    date_begin         : new Date('January 15, 2015 03:24:00'),
    date_dream         : new Date('Februray 30, 2016 03:24:00'),
    date_end           : new Date('Februray 25, 2016 03:24:00'),
    realocate          : false,
    expired            : false,
    comments           : [],
    tags               : [],
    historic_realocate : []
  };
  db.activities.insert(query);
};

/*
 *
 * Guarda os 7 ids das atividades cadastradas
 *
 **/
var idActivities = db.activities.find({},{_id:1,name:1}).toArray();

/*
 *
 * Guarda cada goal do projeto
 *
 **/
var goals = [
  {
    realocate : false,
    tags : ["page admin","manager admin","erp admin"],
    historic_realocate : [
      { date_realocate : new Date('January 15, 2015 03:24:00') }
    ],
    activities : [
      { _id : idActivities[0]._id, name : idActivities[0].name },
      { _id : idActivities[1]._id, name : idActivities[1].name }
    ]
  },
  {
    realocate : false,
    tags : ["shopcart","purchase","ecommerce"],
    historic_realocate : [
      { date_realocate : new Date('January 15, 2015 03:24:00') }
    ],
    activities : [
      { _id :idActivities[2]._id, name : idActivities[2].name  },
      { _id :idActivities[3]._id, name : idActivities[3].name  }
    ]
  },
  {
    realocate : false,
    tags : ["forum","messages","intern page"],
    historic_realocate : [
      { date_realocate : new Date('January 15, 2015 03:24:00') }
    ],
    activities : [
      { _id :idActivities[4]._id , name : idActivities[4].name},
      { _id :idActivities[5]._id , name : idActivities[5].name}
    ]
  },
  {
    realocate : false,
    tags : ["blog","post","public page"],
    historic_realocate : [
      { date_realocate : new Date('January 15, 2015 03:24:00') }
    ],
    activities : [
      { _id :idActivities[6]._id, name : idActivities[6].name},
      { _id :idActivities[7]._id, name : idActivities[7].name}
    ]
  },
  {
    realocate : false,
    tags : ["catalog","movies","rental of films"],
    historic_realocate : [
      { date_realocate : new Date('January 15, 2015 03:24:00') }
    ],
    activities : []
  }
];

/*
 *
 * Insere os 5 Projetos
 *
 **/

for ( var project = 0; project < 5; project++ ) {
  
  var doc = { 
    name             : "project"+project,
    description      : "description"+project,
    date_begin       : new Date('January 15, 2015 03:24:00'),
    date_dream       : new Date('December 15, 2016 03:24:00'),
    date_end         : new Date('November 25, 2016 03:24:00'),
    visible          : true,
    realocate        : null,
    expired          : false,
    visualizable_mod : "somevalue",
    members          : generateMember,
    goal             : [ goals[project] ],
    tags             : all_tags[project]
  };

  db.projects.insert(doc);

};

/* 
Inserted 1 record(s) in 4ms
Inserted 1 record(s) in 3ms
Inserted 1 record(s) in 2ms
Inserted 1 record(s) in 4ms
Inserted 1 record(s) in 1ms
WriteResult({
  "nInserted": 1
})
*/
```
## Retrieve - busca

### 1. Liste as informações dos membros de 1 projeto específico que deve ser buscado pelo seu nome de forma a não ligar 
para maiúsculas e minúsculas.

```JS
var membersId = [];

db.projects.findOne( {name:/project0/i},{"members.user_id":1} ).members.forEach(function(member) {
  membersId.push(member.user_id);
});

db.users.find({ _id : { $in : membersId } });

// Fetched 5 record(s) in 5ms

```

### 2. Liste todos os projetos com a tag que você escolheu para os 3 projetos em comum.

```JS
db.projects.find({ tags : { $in : [/web system/i]} }, {name:1});
// Fetched 3 record(s) in 1ms
```

### 3. Liste apenas os nomes de todas as atividades para todos os projetos.

```JS
var query  = { 
  $and : [
    { "goal.activities": { $exists : true         } },
    { "goal.activities": { $not    : { $eq : [] } } }
  ]
};
var fields = {"goal.activities.name":1};
db.projects.find(query, fields);

// Fetched 4 record(s) in 5ms
```
    
### 4. Liste todos os projetos que não possuam uma tag.

```JS
var query = { tags:  { $not: { $in: ['blog'] } } };
db.projects.find(query);
// Fetched 4 record(s) in 16ms

```

### 5. Liste todos os usuários que não fazem parte do primeiro projeto cadastrado.

```JS

var members = [];

function getId(member) {
  members.push(member.user_id);
};

var project = db.projects.findOne({ name : /project1/i }).members.forEach(getId);

db.users.find( 
  { 
    _id : { 
      $not : { 
        $in : members 
      }
    }
  },
  {
    name: 1,
    _id : 0
  }
);
// Fetched 5 record(s) in 1ms
```

## Update - alteração

### 1. Adicione para todos os projetos o campo views: 0.

```JS
var query = {};
var mod   = { 
  $set : {
    views : 0
  }
};
var opt = { multi : true };

db.projects.update( query, mod, opt );

/* 
Updated 5 existing record(s) in 3ms
WriteResult({
  "nMatched": 5,
  "nUpserted": 0,
  "nModified": 5
})
*/
```

### 2. Adicione 1 tag diferente para cada projeto.

```JS

var id_projects = db.projects.find({},{id:1}).toArray();

function updateTag(obj) {
  var query = obj;
  var mod   = {
    $push : {
      tags : "newTag"+ Math.floor((Math.random() * 20) + 1)
    }
  };
  db.projects.update( query, mod );
} 

id_projects.forEach(updateTag);

/*
Updated 1 existing record(s) in 2ms
Updated 1 existing record(s) in 1ms
Updated 1 existing record(s) in 4ms
Updated 1 existing record(s) in 2ms
Updated 1 existing record(s) in 1ms
*/
```

### 3. Adicione 2 membros diferentes para cada projeto.

```JS

// inserindo 12 membros pq tenho 6 projetos
var newUsers = [ 
  "Traci  Davidson",
  "Sylvia  Gibbs",
  "Robert  Park",
  "Austin  Santos",
  "Brittany  White",
  "Jean  Hill",
  "Constance Farmer",
  "Sophie  Hanson",
  "Lori  Anderson",
  "Frankie Douglas",
  "Noel  Little",
  "Cedric  Glover",
];

// Aprovetei a função insertUser da questão01 de Create
insertUser(newUsers);

/* 
Inserted 1 record(s) in 2ms
Inserted 1 record(s) in 2ms
Inserted 1 record(s) in 2ms
Inserted 1 record(s) in 2ms
Inserted 1 record(s) in 1ms
Inserted 1 record(s) in 2ms
Inserted 1 record(s) in 1ms
Inserted 1 record(s) in 2ms
Inserted 1 record(s) in 2ms
Inserted 1 record(s) in 1ms
Inserted 1 record(s) in 1ms
Inserted 1 record(s) in 1ms
*/

// ids dos projetos
var qtdProject = db.projects.find({},{_id:1}).toArray();

// guarda todos os ids dos users inseridos
var idUsers = db.users.find({name : { $in : newUsers }}).toArray();

// divide array
function split(arr, n) {
  var res = [];
  while (arr.length) {
    res.push(arr.splice(0, n));
  }
  return res;
}

// separa 2 users
var parts = split(idUsers,2);

// Insere dois membros diferentes em cada projeto
for (var iProject=0; iProject < qtdProject.length; iProject++ ) {

  if( !!parts[iProject] ) {

    var member1 = {
      user_id      : parts[iProject][0]._id,
      name         : parts[iProject][0].name,
      avatar_path  : parts[iProject][0].avatar_path,
      type_name    : "Developer",
      notify       : false
    }; 
  
    var member2 = {
      user_id      : parts[iProject][1]._id,
      name         : parts[iProject][1].name,
      avatar_path  : parts[iProject][0].avatar_path,
      type_name    : "Developer",
      notify       : false
    };

    var query = qtdProject[iProject];
    var mod = {
      $pushAll : {
        members :  [ member1,member2 ]
      }
    };
    db.projects.update( query, mod ); 

  }

};  

/* 
Updated 1 existing record(s) in 2ms
Updated 1 existing record(s) in 1ms
Updated 1 existing record(s) in 2ms
Updated 1 existing record(s) in 2ms
Updated 1 existing record(s) in 1ms
WriteResult({
  "nMatched": 1,
  "nUpserted": 0,
  "nModified": 1
})
*/
``` 

### 4. Adicione 1 comentário em cada atividade, deixe apenas 1 projeto sem.

```JS

// Guarda os ids de pelo menos um membro de cada projeto
var idMembers    = [];
var idActivities = [];

function prepateActivities( project, idx ) {
  var rand = Math.floor((Math.random() * 3) + 1);
  if ( project.members[rand] ) {
    idMembers.push(project.members[rand]);
    idActivities.push(project.goal); 
  }
};

// Consulta os ids dos membros e os ids das atividades
var query =  { 
  $and : [
    { members           : { $exists:true       } }, 
    { "goal.activities" : { $not : {$size : 0} } },
    { goal              : { $not : {$size : 0} } }
  ]
};
var fields = { _id:0, "goal.activities._id":1, "members":1 };
db.projects.find( query,fields ).toArray().forEach(prepateActivities);

// Adiciona dois comentários em cada atividade
for ( var iActivity=0; iActivity < idActivities.length; iActivity++ ) {
  
  var commentActivity1 = {
    text : "comment Activity 1",
    date_comment : new Date(),
    file : [
      {
        path   : "http://urlproject/folderimg/img.jpg",
        weight : null,
        name   : "image"
      }
    ],
    member : idMembers[iActivity]
  };
  
  var query = { _id : idActivities[iActivity][0].activities[0]._id };
  var mod = {
    $push : {
      "comments" : commentActivity1
    }
  };
  db.activities.update( query, mod ); 
  
  var commentActivity2 = {
    text : "comment Activity 2",
    date_comment : new Date(),

    file : [
      {
        path   : "http://urlproject/folderimg/img.jpg",
        weight : null,
        name   : "image"
      }
    ],
    member : idMembers[iActivity]
  };
  
  var query2 = { _id : idActivities[iActivity][0].activities[1]._id };
  var mod2 = {
    $push : {
      "comments" :  commentActivity2
    }
  };
  if ( iActivity < (idActivities.length-1) ) { 
    db.activities.update( query2, mod2 );
  }
  
};
/* 
Updated 1 existing record(s) in 2ms
Updated 1 existing record(s) in 2ms
Updated 1 existing record(s) in 1ms
Updated 1 existing record(s) in 2ms
Updated 1 existing record(s) in 1ms
WriteResult({
  "nMatched": 1,
  "nUpserted": 0,
  "nModified": 1
})

*/

```

### 5. Adicione 1 projeto inteiro com UPSERT.

```JS
var members = [];

for ( var iMember = 4; iMember >= 0; iMember-- ) {

  var collection = db.users;
  var rand = function(){
    return Math.floor( Math.random() * collection.count() )
  };
  var query = {};
  var fields = { name: 1, avatar_path:1 };
  var member = collection.find(query, fields).limit(5).skip(rand()).next();
  var schemaMember = {
    user_id      : member._id,
    name         : member.name,
    avatar_path  : member.avatar_path,
    type_name    : "sometype",
    notify       : false
  }; 
  members.push(schemaMember);

};

function onlyUnique(value, index, self) { 
  return self.indexOf(value) === index;
}

var unique = members.filter( onlyUnique ); 

members = unique.slice(0,5);

var generateMember = [];

members.forEach( function (el,idx) {
  generateMember.push(el);
});

// tags
var all_tags = [
  [ "erp"      ,"dashboard"      , "help-desk"  , "web system" ],
  [ "ecommerce","seo"            , "shopcart"   , "web system" ],
  [ "forum"    ,"hep-desk"       , "faq"        , "web system" ],
  [ "blog"     ,"static generate", "single-page","simple site" ],
  [ "locadora" ,"catalogo"       , "filmes"     , "simple site"]
];

// activity
var query = {
  name           : "upsert1",
  description    : "This is a some Activity",
  date_begin     : new Date('January 15, 2015 03:24:00'),
  date_dream     : new Date('Februray 30, 2016 03:24:00'),
  date_end       : new Date('Februray 25, 2016 03:24:00'),
  realocate      : true,
  expired        : false,

  historic_realocate : [
    {date_realocate : new Date('January 15, 2015 03:24:00') }
  ],

  members : [
    members[0],
    members[1]
  ],

  comments: [{
    text : "comment  upsert",
    date_comment : new Date(),
    file : [
      {
        path   : "http://urlproject/folderimg/img.jpg",
        weight : null,
        name   : "image"
      }
    ],
    member : members[0],
  }],

  tags : ["tag1","tag2"]
};
db.activities.insert(query);
 
var goals = [
  {
    realocate : false,
    tags : ["page admin","manager admin","erp admin"],
    historic_realocate : [
      { date_realocate : new Date('January 15, 2015 03:24:00') }
    ],
    activities : [ db.activities.findOne({name:"upsert1"},{_id:1, name:1}) ]
  }
];

var query = { name: /project220/i };

var mod = { 

  $setOnInsert : {
    name             : "project220",
    description      : "description220",
    date_begin       : new Date('January 15, 2015 03:24:00'),
    date_dream       : new Date('December 15, 2016 03:24:00'),
    date_end         : new Date('November 25, 2016 03:24:00'),
    visible          : true,
    realocate        : null,
    expired          : false,
    visualizable_mod : "somevalue",
    members          : generateMember,
    goal             : [ goals[0] ], 
    tags             : all_tags[3]
  }  
};

var options = {upsert: true};
db.projects.update( query, mod, options );

/*
Updated 1 new record(s) in 97ms
WriteResult({
  "nMatched": 0,
  "nUpserted": 1,
  "nModified": 0,
  "_id": ObjectId("56798c668b2034d8ac2e407c")
})
*/
```
## Delete - remoção

### 1. Apague todos os projetos que não possuam tags.

```JS

// Insert de projeto sem tags
var doc = { 
  name             : "100tags",
  description      : "description without tags",
  date_begin       : new Date('January 15, 2015 03:24:00'),
  date_dream       : new Date('December 15, 2016 03:24:00'),
  date_end         : new Date('November 25, 2016 03:24:00'),
  visible          : true,
  realocate        : null,
  expired          : false,
  visualizable_mod : "somevalue",
  members          : [],
  goal             : []
};

db.project.insert(doc);

var query = { 
  $or : [ 
    { tags: { $size   : 0     } }, 
    { tags: { $exists : false } },
    { tags: { $eq     : []    } }
  ]  
};
db.project.remove(query);

/*
Removed 1 record(s) in 50ms
WriteResult({
  "nRemoved": 1
})
*/

```

### 2. Apague todos os projetos que não possuam comentários nas atividades.


```JS
// Inserindo uma nova atividade sem comentários e atualiza um projeto com ela
var queryNewActivity = {
  name               : "activity without comment",
  description        : "This is a some Activity",
  date_begin         : new Date('January 15, 2015 03:24:00'),
  date_dream         : new Date('Februray 30, 2016 03:24:00'),
  date_end           : new Date('Februray 25, 2016 03:24:00'),
  realocate          : true,
  expired            : false,
  members            : [],
  comments           : [],
  tags               : [],
  historic_realocate : []
};
db.activities.insert(queryNewActivity);

/*
Inserted 1 record(s) in 8ms
WriteResult({
  "nInserted": 1
})
*/

// Guarda o id dessa atividade sem comentário
var activity = db.activities.find( { name:/activity without comment/i }, { _id:1, name: 1}).toArray();

// atualiza um projeto adicionando essa atividade
var query = {name :/project4/i};
var project = db.projects.findOne(query);
project.goal = {
  "realocate": false,
  "tags": [
    "teste"
  ],
  "historic_realocate": [
    {
      "date_realocate": new Date
    }
  ],
  "activities": activity

};
db.projects.save(project);

/*
Updated 1 existing record(s) in 5ms
WriteResult({
  "nMatched": 1,
  "nUpserted": 0,
  "nModified": 1
})
*/

// CONSULTA TODAS AS ATIVIDADES SEM COMENTÁRIOS E AS GUARDA EM idsActivities
var queryActivities = {
  $or : [
    { comments : { $size   : 0     } },
    { comments : { $exists : false } },
    { comments : { $eq     :  [ ]  } }
  ]
};

var idsActivities = [];

db.activities.find( queryActivities, { _id : 1 }).toArray().forEach(function(activity) {
  idsActivities.push(activity._id);
});

db.projects.remove({ "goal.activities._id" : { $in : idsActivities } }, {multi:1});

/*
Removed 2 record(s) in 2ms
WriteResult({
  "nRemoved": 2
})
*/
```

### 3. Apague todos os projetos que não possuam atividades.

```JS

// Insert de projeto sem activities
var doc = { 
  name             : "100activities",
  description      : "description without tags",
  date_begin       : new Date('January 15, 2015 03:24:00'),
  date_dream       : new Date('December 15, 2016 03:24:00'),
  date_end         : new Date('November 25, 2016 03:24:00'),
  visible          : true,
  realocate        : null,
  expired          : false,
  visualizable_mod : "somevalue",
  members          : [],
  goal             : []
};

db.project.insert(doc);
/*
Inserted 1 record(s) in 2ms
WriteResult({
  "nInserted": 1
})
*/

var query = {
  $or : [
    {"goal.activities" : { $size   : 0     }},
    {"goal.activities" : { $exists : false } },
    {"goal.activities" : { $eq     : [ ] } }
  ]
};

db.project.remove(query, {multi:true});

/*
Removed 2 record(s) in 2ms
WriteResult({
  "nRemoved": 2
}) 
*/
```

### 4. Escolha 2 usuário e apague todos os projetos em que os 2 fazem parte.

```JS
// Procura nomes
leonardo(mongod-2.6.11) project> db.projects.find({},{"members.name":1});

// OBS: esses nomes devem ser alterados e escolhidos mediante a query anterior
var users = [];
var query = { name : { $in : [/Anthonyson/i,/Robert  Park/i] }};
var fields = {_id:1};

db.users.find( query, fields ).toArray().forEach(function (id) {
  users.push(id._id);
});

// remove
db.projects.remove({ "members.user_id" : { $in : users } }, {_id:1} );

/*
Removed 3 record(s) in 1ms
WriteResult({
  "nRemoved": 3
})
*/
```

### 5. Apague todos os projetos que possuam uma determinada tag em goal.

```JS
var query  = { "goal.tags" : { $eq : "page admin" } };
db.projects.remove(query, {multi:true});
/*
Removed 1 record(s) in 2ms
WriteResult({
  "nRemoved": 1
})
*/
```

## Gerenciamento de Usuários

### 1. Crie um usuário com permissões APENAS de Leitura.

```JS
db.createUser(
  { 
    createUser: "user1",
    pwd: "user123",
    roles : [
      { role: "read", db: "admin" }
    ]
  }
)
/*
Successfully added user: {
  "createUser": "user1",
  "roles": [
    {
      "role": "read",
      "db": "admin"
    }
  ]
}
*/
```

### 2. Crie um usuário com permissões de Escrita e Leitura.

```JS
db.createUser(
  { 
    createUser: "user2",
    pwd: "user123",
    roles : [
      { role: "readWrite", db: "admin" }
    ]
  }
)
/*
Successfully added user: {
  "createUser": "user2",
  "roles": [
    {
      "role": "readWrite",
      "db": "admin"
    }
  ]
}
*/
```

### 3. Adicionar o papel grantRolesToUser e revokeRole para o usuário com Escrita e Leitura.


```JS

// grantRolesToUser

db.createRole( 
  { 
    role : "grantRolesToUser",
    
    roles : [
      { role: "readWrite", db: "admin" }
    ],

    privileges : [
      { resource : { db: "admin", collection: "" }, actions : ["grantRole"] }
    ]
  }
);

/* 
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

*/

// revokeRole 

db.createRole( 
  { 
    role : "revokeRole",
    
    roles : [
      { role: "readWrite", db: "admin" }
    ],

    privileges : [
      { resource : { db: "admin", collection: "" }, actions : ["grantRole"] }
    ]
  }
);
/*
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

*/

db.grantRolesToUser(
  "user2",
  [ "grantRolesToUser" , "revokeRole" ]
);

/* 
{
  "_id": "admin.user2",
  "user": "user2",
  "db": "admin",
  "credentials": {
    "MONGODB-CR": "91d4495d9064403badbea15f28cf06af"
  },
  "roles": [
    {
      "role": "revokeRole",
      "db": "admin"
    },
    {
      "role": "readWrite",
      "db": "admin"
    },
    {
      "role": "grantRolesToUser",
      "db": "admin"
    }
  ]
}
*/

```

### 4. Remover o papel grantRolesToUser para o usuário com Escrita e Leitura.

```JS

db.runCommand( { 
  revokeRolesFromUser: "user2",
  roles: [
    { role: "grantRolesToUser", db: "admin" }
  ]
});

/*
{
  "ok": 1
}
*/
db.system.users.find()
/*
{
  "_id": "admin.user2",
  "user": "user2",
  "db": "admin",
  "credentials": {
    "MONGODB-CR": "91d4495d9064403badbea15f28cf06af"
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
  ]
}
*/
```

### 5. Listar todos os usuários com seus papéis e ações.
```JS
```JS
leonardo(mongod-2.6.11) admin> db.runCommand({ usersInfo: 1 });
/*
{
  "users": [
    {
      "_id": "admin.user1",
      "user": "user1",
      "db": "admin",
      "roles": [
        {
          "role": "read",
          "db": "admin"
        }
      ]
    },
    {
      "_id": "admin.user2",
      "user": "user2",
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
*/

```
```

## Cluster

Depois de criada toda sua base você deverá criar um cluster utilizando:

* 1 Router
* 1 Config Server
* 3 Shardings
* 3 Replicas

## Réplicas

```JS
// Cria pasta data
leonardo@leonardo:$ sudo mkdir data

// Dentro de data cria pastas das réplicas
leonardo@leonardo:data$ sudo mkdir rs1
leonardo@leonardo:data$ sudo mkdir rs2
leonardo@leonardo:data$ sudo mkdir rs3

//Conecta as réplicas em cada porta abrindo cada um em um terminal diferente
leonardo@leonardo:/data$ sudo mongod --replSet replica_set --port 27017 --dbpath /data/rs1
[sudo] password for leonardo: 
/*
2015-12-17T12:20:43.070-0300 
2015-12-17T12:20:43.070-0300 warning: 32-bit servers don't have journaling enabled by default. Please use --journal if you want durability.
2015-12-17T12:20:43.070-0300 
2015-12-17T12:20:43.300-0300 [initandlisten] MongoDB starting : pid=6271 port=27017 dbpath=/data/rs1 32-bit host=leonardo
2015-12-17T12:20:43.300-0300 [initandlisten] 
2015-12-17T12:20:43.300-0300 [initandlisten] ** NOTE: This is a 32 bit MongoDB binary.
2015-12-17T12:20:43.300-0300 [initandlisten] **       32 bit builds are limited to less than 2GB of data (or less with --journal).
2015-12-17T12:20:43.300-0300 [initandlisten] **       Note that journaling defaults to off for 32 bit and is currently off.
2015-12-17T12:20:43.300-0300 [initandlisten] **       See http://dochub.mongodb.org/core/32bit
2015-12-17T12:20:43.301-0300 [initandlisten] 
2015-12-17T12:20:43.301-0300 [initandlisten] db version v2.6.11
2015-12-17T12:20:43.301-0300 [initandlisten] git version: d00c1735675c457f75a12d530bee85421f0c5548
2015-12-17T12:20:43.301-0300 [initandlisten] build info: Linux ip-10-93-139-4 2.6.18-194.el5xen #1 SMP Tue Mar 16 22:08:06 EDT 2010 i686 BOOST_LIB_VERSION=1_49
2015-12-17T12:20:43.301-0300 [initandlisten] allocator: system
2015-12-17T12:20:43.301-0300 [initandlisten] options: { net: { port: 27017 }, replication: { replSet: "replica_set" }, storage: { dbPath: "/data/rs1" } }
2015-12-17T12:20:43.529-0300 [initandlisten] allocating new ns file /data/rs1/local.ns, filling with zeroes...
2015-12-17T12:20:43.819-0300 [FileAllocator] allocating new datafile /data/rs1/local.0, filling with zeroes...
2015-12-17T12:20:43.819-0300 [FileAllocator] creating directory /data/rs1/_tmp
2015-12-17T12:20:43.981-0300 [FileAllocator] done allocating datafile /data/rs1/local.0, size: 64MB,  took 0.088 secs
2015-12-17T12:20:43.983-0300 [initandlisten] build index on: local.startup_log properties: { v: 1, key: { _id: 1 }, name: "_id_", ns: "local.startup_log" }
2015-12-17T12:20:43.983-0300 [initandlisten]   added index to empty collection
2015-12-17T12:20:43.983-0300 [initandlisten] command local.$cmd command: create { create: "startup_log", size: 10485760, capped: true } ntoreturn:1 keyUpdates:0 numYields:0  reslen:37 454ms
2015-12-17T12:20:44.002-0300 [initandlisten] waiting for connections on port 27017
2015-12-17T12:20:44.002-0300 [rsStart] replSet can't get local.system.replset config from self or any seed (EMPTYCONFIG)
2015-12-17T12:20:44.002-0300 [rsStart] replSet info you may need to run replSetInitiate -- rs.initiate() in the shell -- if that is not already done
*/
leonardo@leonardo:/data$ sudo mongod --replSet replica_set --port 27018 --dbpath /data/rs2
/*
[sudo] password for leonardo: 
2015-12-17T12:21:24.617-0300 
2015-12-17T12:21:24.617-0300 warning: 32-bit servers don't have journaling enabled by default. Please use --journal if you want durability.
2015-12-17T12:21:24.617-0300 
2015-12-17T12:21:24.621-0300 [initandlisten] MongoDB starting : pid=6314 port=27018 dbpath=/data/rs2 32-bit host=leonardo
2015-12-17T12:21:24.621-0300 [initandlisten] 
2015-12-17T12:21:24.621-0300 [initandlisten] ** NOTE: This is a 32 bit MongoDB binary.
2015-12-17T12:21:24.621-0300 [initandlisten] **       32 bit builds are limited to less than 2GB of data (or less with --journal).
2015-12-17T12:21:24.621-0300 [initandlisten] **       Note that journaling defaults to off for 32 bit and is currently off.
2015-12-17T12:21:24.621-0300 [initandlisten] **       See http://dochub.mongodb.org/core/32bit
2015-12-17T12:21:24.621-0300 [initandlisten] 
2015-12-17T12:21:24.621-0300 [initandlisten] db version v2.6.11
2015-12-17T12:21:24.621-0300 [initandlisten] git version: d00c1735675c457f75a12d530bee85421f0c5548
2015-12-17T12:21:24.621-0300 [initandlisten] build info: Linux ip-10-93-139-4 2.6.18-194.el5xen #1 SMP Tue Mar 16 22:08:06 EDT 2010 i686 BOOST_LIB_VERSION=1_49
2015-12-17T12:21:24.621-0300 [initandlisten] allocator: system
2015-12-17T12:21:24.621-0300 [initandlisten] options: { net: { port: 27018 }, replication: { replSet: "replica_set" }, storage: { dbPath: "/data/rs2" } }
2015-12-17T12:21:24.662-0300 [initandlisten] allocating new ns file /data/rs2/local.ns, filling with zeroes...
2015-12-17T12:21:24.940-0300 [FileAllocator] allocating new datafile /data/rs2/local.0, filling with zeroes...
2015-12-17T12:21:24.940-0300 [FileAllocator] creating directory /data/rs2/_tmp
2015-12-17T12:21:24.983-0300 [FileAllocator] done allocating datafile /data/rs2/local.0, size: 64MB,  took 0.022 secs
2015-12-17T12:21:24.984-0300 [initandlisten] build index on: local.startup_log properties: { v: 1, key: { _id: 1 }, name: "_id_", ns: "local.startup_log" }
2015-12-17T12:21:24.984-0300 [initandlisten]   added index to empty collection
2015-12-17T12:21:24.984-0300 [initandlisten] command local.$cmd command: create { create: "startup_log", size: 10485760, capped: true } ntoreturn:1 keyUpdates:0 numYields:0  reslen:37 321ms
2015-12-17T12:21:24.984-0300 [initandlisten] waiting for connections on port 27018
2015-12-17T12:21:24.985-0300 [rsStart] replSet can't get local.system.replset config from self or any seed (EMPTYCONFIG)
2015-12-17T12:21:24.985-0300 [rsStart] replSet info you may need to run replSetInitiate -- rs.initiate() in the shell -- if that is not already done
*/

leonardo@leonardo:/data$ sudo mongod --replSet replica_set --port 27019 --dbpath /data/rs3
/*
[sudo] password for leonardo: 
2015-12-17T12:21:54.603-0300 
2015-12-17T12:21:54.603-0300 warning: 32-bit servers don't have journaling enabled by default. Please use --journal if you want durability.
2015-12-17T12:21:54.603-0300 
2015-12-17T12:21:54.606-0300 [initandlisten] MongoDB starting : pid=6344 port=27019 dbpath=/data/rs3 32-bit host=leonardo
2015-12-17T12:21:54.606-0300 [initandlisten] 
2015-12-17T12:21:54.606-0300 [initandlisten] ** NOTE: This is a 32 bit MongoDB binary.
2015-12-17T12:21:54.606-0300 [initandlisten] **       32 bit builds are limited to less than 2GB of data (or less with --journal).
2015-12-17T12:21:54.606-0300 [initandlisten] **       Note that journaling defaults to off for 32 bit and is currently off.
2015-12-17T12:21:54.606-0300 [initandlisten] **       See http://dochub.mongodb.org/core/32bit
2015-12-17T12:21:54.606-0300 [initandlisten] 
2015-12-17T12:21:54.606-0300 [initandlisten] db version v2.6.11
2015-12-17T12:21:54.606-0300 [initandlisten] git version: d00c1735675c457f75a12d530bee85421f0c5548
2015-12-17T12:21:54.606-0300 [initandlisten] build info: Linux ip-10-93-139-4 2.6.18-194.el5xen #1 SMP Tue Mar 16 22:08:06 EDT 2010 i686 BOOST_LIB_VERSION=1_49
2015-12-17T12:21:54.606-0300 [initandlisten] allocator: system
2015-12-17T12:21:54.606-0300 [initandlisten] options: { net: { port: 27019 }, replication: { replSet: "replica_set" }, storage: { dbPath: "/data/rs3" } }
2015-12-17T12:21:54.680-0300 [initandlisten] allocating new ns file /data/rs3/local.ns, filling with zeroes...
2015-12-17T12:21:54.937-0300 [FileAllocator] allocating new datafile /data/rs3/local.0, filling with zeroes...
2015-12-17T12:21:54.937-0300 [FileAllocator] creating directory /data/rs3/_tmp
2015-12-17T12:21:55.012-0300 [FileAllocator] done allocating datafile /data/rs3/local.0, size: 64MB,  took 0.043 secs
2015-12-17T12:21:55.014-0300 [initandlisten] build index on: local.startup_log properties: { v: 1, key: { _id: 1 }, name: "_id_", ns: "local.startup_log" }
2015-12-17T12:21:55.014-0300 [initandlisten]   added index to empty collection
2015-12-17T12:21:55.014-0300 [initandlisten] command local.$cmd command: create { create: "startup_log", size: 10485760, capped: true } ntoreturn:1 keyUpdates:0 numYields:0  reslen:37 334ms
2015-12-17T12:21:55.014-0300 [initandlisten] waiting for connections on port 27019
2015-12-17T12:21:55.015-0300 [rsStart] replSet can't get local.system.replset config from self or any seed (EMPTYCONFIG)
2015-12-17T12:21:55.015-0300 [rsStart] replSet info you may need to run replSetInitiate -- rs.initiate() in the shell -- if that is not already done
*/

//Conectando no primário
leonardo@leonardo:data$ mongo --port 27017
/*
MongoDB shell version: 2.6.11
connecting to: 127.0.0.1:27017/test
Mongo-Hacker 0.0.9
Server has startup warnings: 
2015-12-17T12:20:43.300-0300 [initandlisten] 
2015-12-17T12:20:43.300-0300 [initandlisten] ** NOTE: This is a 32 bit MongoDB binary.
2015-12-17T12:20:43.300-0300 [initandlisten] **       32 bit builds are limited to less than 2GB of data (or less with --journal).
2015-12-17T12:20:43.300-0300 [initandlisten] **       Note that journaling defaults to off for 32 bit and is currently off.
2015-12-17T12:20:43.300-0300 [initandlisten] **       See http://dochub.mongodb.org/core/32bit
2015-12-17T12:20:43.301-0300 [initandlisten] 
leonardo(mongod-2.6.11) test>
*/

// Cria json de configuração do replicaset
var rsconf = {
   _id: "replica_set",
   members: [
    {
     _id: 0,
     host: "127.0.0.1:27017"
    }
  ]
};

leonardo(mongod-2.6.11) test> rs.initiate(rsconf)
/*{
  "info": "Config now saved locally.  Should come online in about a minute.",
  "ok": 1
}
leonardo(mongod-2.6.11)[PRIMARY:replica_set] test> 
*/

// Terminal do rs1
/*
2015-12-17T12:23:17.370-0300 [conn1] replSet replSetInitiate admin command received from client
2015-12-17T12:23:17.385-0300 [conn1] replSet replSetInitiate config object parses ok, 1 members specified
2015-12-17T12:23:17.386-0300 [conn1] replSet replSetInitiate all members seem up
2015-12-17T12:23:17.386-0300 [conn1] ******
2015-12-17T12:23:17.386-0300 [conn1] creating replication oplog of size: 50MB...
2015-12-17T12:23:17.493-0300 [conn1] ******
2015-12-17T12:23:17.493-0300 [conn1] replSet info saving a newer config version to local.system.replset: { _id: "replica_set", version: 1, members: [ { _id: 0, host: "127.0.0.1:27017" } ] }
2015-12-17T12:23:17.494-0300 [conn1] build index on: local.system.replset properties: { v: 1, key: { _id: 1 }, name: "_id_", ns: "local.system.replset" }
2015-12-17T12:23:17.494-0300 [conn1]   added index to empty collection
2015-12-17T12:23:17.494-0300 [conn1] replSet saveConfigLocally done
2015-12-17T12:23:17.495-0300 [conn1] replSet replSetInitiate config now saved locally.  Should come online in about a minute.
2015-12-17T12:23:17.495-0300 [conn1] command admin.$cmd command: replSetInitiate { replSetInitiate: { _id: "replica_set", members: [ { _id: 0.0, host: "127.0.0.1:27017" } ] } } keyUpdates:0 numYields:0 locks(micros) W:109128 reslen:112 124ms
2015-12-17T12:23:18.057-0300 [rsStart] replSet I am 127.0.0.1:27017
2015-12-17T12:23:18.059-0300 [rsStart] build index on: local.me properties: { v: 1, key: { _id: 1 }, name: "_id_", ns: "local.me" }
2015-12-17T12:23:18.059-0300 [rsStart]   added index to empty collection
2015-12-17T12:23:18.059-0300 [rsStart] replSet STARTUP2
2015-12-17T12:23:18.060-0300 [rsSync] replSet SECONDARY
2015-12-17T12:23:18.060-0300 [rsMgr] replSet info electSelf 0
2015-12-17T12:23:18.060-0300 [rsMgr] replSet PRIMARY
2015-12-17T12:23:43.536-0300 [clientcursormon] mem (MB) res:35 virt:282
2015-12-17T12:23:43.536-0300 [clientcursormon]  mapped:80
2015-12-17T12:23:43.536-0300 [clientcursormon]  connections:1
2015-12-17T12:23:43.536-0300 [clientcursormon]  replication threads:4
2015-12-17T12:28:43.553-0300 [clientcursormon] mem (MB) res:35 virt:282
2015-12-17T12:28:43.553-0300 [clientcursormon]  mapped:80
2015-12-17T12:28:43.553-0300 [clientcursormon]  connections:1
2015-12-17T12:28:43.553-0300 [clientcursormon]  replication threads:4
2015-12-17T12:33:43.570-0300 [clientcursormon] mem (MB) res:35 virt:282
2015-12-17T12:33:43.570-0300 [clientcursormon]  mapped:80
2015-12-17T12:33:43.570-0300 [clientcursormon]  connections:1
2015-12-17T12:33:43.570-0300 [clientcursormon]  replication threads:4
2015-12-17T12:38:43.586-0300 [clientcursormon] mem (MB) res:35 virt:282
2015-12-17T12:38:43.586-0300 [clientcursormon]  mapped:80
2015-12-17T12:38:43.587-0300 [clientcursormon]  connections:1
2015-12-17T12:38:43.587-0300 [clientcursormon]  replication threads:4
*/

// Adiciona secundário
leonardo(mongod-2.6.11)[PRIMARY:replica_set] test> rs.add("127.0.0.1:27018")
{
  "ok": 1
}
leonardo(mongod-2.6.11)[PRIMARY:replica_set] test> rs.add("127.0.0.1:27019")
{
  "ok": 1
}

// Verificando status das replicaset
leonardo(mongod-2.6.11)[PRIMARY:replica_set] test> rs.status()
{
  "set": "replica_set",
  "date": ISODate("2015-12-17T15:43:30Z"),
  "myState": 1,
  "members": [
    {
      "_id": 0,
      "name": "127.0.0.1:27017",
      "health": 1,
      "state": 1,
      "stateStr": "PRIMARY",
      "uptime": 1370,
      "optime": Timestamp(1450366944, 1),
      "optimeDate": ISODate("2015-12-17T15:42:24Z"),
      "electionTime": Timestamp(1450365798, 1),
      "electionDate": ISODate("2015-12-17T15:23:18Z"),
      "self": true
    },
    {
      "_id": 1,
      "name": "127.0.0.1:27018",
      "health": 1,
      "state": 2,
      "stateStr": "SECONDARY",
      "uptime": 86,
      "optime": Timestamp(1450366944, 1),
      "optimeDate": ISODate("2015-12-17T15:42:24Z"),
      "lastHeartbeat": ISODate("2015-12-17T15:43:28Z"),
      "lastHeartbeatRecv": ISODate("2015-12-17T15:43:29Z"),
      "pingMs": 0,
      "syncingTo": "127.0.0.1:27017"
    },
    {
      "_id": 2,
      "name": "127.0.0.1:27019",
      "health": 1,
      "state": 2,
      "stateStr": "SECONDARY",
      "uptime": 66,
      "optime": Timestamp(1450366944, 1),
      "optimeDate": ISODate("2015-12-17T15:42:24Z"),
      "lastHeartbeat": ISODate("2015-12-17T15:43:28Z"),
      "lastHeartbeatRecv": ISODate("2015-12-17T15:43:29Z"),
      "pingMs": 0,
      "syncingTo": "127.0.0.1:27017"
    }
  ],
  "ok": 1
}

// Status do oplog
leonardo(mongod-2.6.11)[PRIMARY:replica_set] test> rs.printReplicationInfo()
/*
configured oplog size:   50MB
log length start to end: 1147secs (0.32hrs)
oplog first event time:  Thu Dec 17 2015 12:23:17 GMT-0300 (BRT)
oplog last event time:   Thu Dec 17 2015 12:42:24 GMT-0300 (BRT)
now:                     Thu Dec 17 2015 12:44:21 GMT-0300 (BRT)
*/
```

## Shardings

### 1. Criando 1 Config Server

```JS

leonardo@leonardo:$ sudo mkdir data

leonardo@leonardo:data$ mongod --configsvr --port 27010
/*
2015-12-17T13:04:19.900-0300 
2015-12-17T13:04:19.900-0300 warning: 32-bit servers don't have journaling enabled by default. Please use --journal if you want durability.
2015-12-17T13:04:19.900-0300 
2015-12-17T13:04:19.903-0300 [initandlisten] MongoDB starting : pid=8073 port=27010 dbpath=/data/configdb master=1 32-bit host=leonardo
2015-12-17T13:04:19.903-0300 [initandlisten] 
2015-12-17T13:04:19.903-0300 [initandlisten] ** NOTE: This is a 32 bit MongoDB binary.
2015-12-17T13:04:19.903-0300 [initandlisten] **       32 bit builds are limited to less than 2GB of data (or less with --journal).
2015-12-17T13:04:19.903-0300 [initandlisten] **       See http://dochub.mongodb.org/core/32bit
2015-12-17T13:04:19.903-0300 [initandlisten] 
2015-12-17T13:04:19.904-0300 [initandlisten] db version v2.6.11
2015-12-17T13:04:19.904-0300 [initandlisten] git version: d00c1735675c457f75a12d530bee85421f0c5548
2015-12-17T13:04:19.904-0300 [initandlisten] build info: Linux ip-10-93-139-4 2.6.18-194.el5xen #1 SMP Tue Mar 16 22:08:06 EDT 2010 i686 BOOST_LIB_VERSION=1_49
2015-12-17T13:04:19.904-0300 [initandlisten] allocator: system
2015-12-17T13:04:19.904-0300 [initandlisten] options: { net: { port: 27010 }, sharding: { clusterRole: "configsvr" } }
2015-12-17T13:04:19.961-0300 [initandlisten] journal dir=/data/configdb/journal
2015-12-17T13:04:19.961-0300 [initandlisten] recover : no journal files present, no recovery needed
2015-12-17T13:04:22.803-0300 [initandlisten] preallocateIsFaster=true 32.3
2015-12-17T13:04:25.649-0300 [initandlisten] preallocateIsFaster=true 29.26
2015-12-17T13:04:29.457-0300 [initandlisten] preallocateIsFaster=true 33.3
2015-12-17T13:04:29.457-0300 [initandlisten] preallocateIsFaster check took 9.495 secs
2015-12-17T13:04:29.457-0300 [initandlisten] preallocating a journal file /data/configdb/journal/prealloc.0
2015-12-17T13:04:31.408-0300 [initandlisten] preallocating a journal file /data/configdb/journal/prealloc.1
2015-12-17T13:04:33.423-0300 [initandlisten] preallocating a journal file /data/configdb/journal/prealloc.2
2015-12-17T13:04:35.471-0300 [initandlisten] allocating new ns file /data/configdb/local.ns, filling with zeroes...
2015-12-17T13:04:35.762-0300 [FileAllocator] allocating new datafile /data/configdb/local.0, filling with zeroes...
2015-12-17T13:04:35.762-0300 [FileAllocator] creating directory /data/configdb/_tmp
2015-12-17T13:04:35.805-0300 [FileAllocator] done allocating datafile /data/configdb/local.0, size: 16MB,  took 0.022 secs
2015-12-17T13:04:35.806-0300 [initandlisten] build index on: local.startup_log properties: { v: 1, key: { _id: 1 }, name: "_id_", ns: "local.startup_log" }
2015-12-17T13:04:35.806-0300 [initandlisten]   added index to empty collection
2015-12-17T13:04:35.806-0300 [initandlisten] command local.$cmd command: create { create: "startup_log", size: 10485760, capped: true } ntoreturn:1 keyUpdates:0 numYields:0  reslen:37 335ms
2015-12-17T13:04:35.807-0300 [initandlisten] ******
2015-12-17T13:04:35.807-0300 [initandlisten] creating replication oplog of size: 5MB...
2015-12-17T13:04:35.807-0300 [initandlisten] ******
2015-12-17T13:04:35.821-0300 [initandlisten] waiting for connections on port 27010
*/
```

### Cria 1 Router

```JS
leonardo@leonardo:data$ mongos --configdb localhost:27010 --port 27011
/*
2015-12-17T13:05:36.622-0300 warning: running with 1 config server should be done only for testing purposes and is not recommended for production
2015-12-17T13:05:36.657-0300 [mongosMain] MongoS version 2.6.11 starting: pid=8130 port=27011 32-bit host=leonardo (--help for usage)
2015-12-17T13:05:36.657-0300 [mongosMain] db version v2.6.11
2015-12-17T13:05:36.657-0300 [mongosMain] git version: d00c1735675c457f75a12d530bee85421f0c5548
2015-12-17T13:05:36.657-0300 [mongosMain] build info: Linux ip-10-93-139-4 2.6.18-194.el5xen #1 SMP Tue Mar 16 22:08:06 EDT 2010 i686 BOOST_LIB_VERSION=1_49
2015-12-17T13:05:36.657-0300 [mongosMain] allocator: system
2015-12-17T13:05:36.657-0300 [mongosMain] options: { net: { port: 27011 }, sharding: { configDB: "localhost:27010" } }
2015-12-17T13:05:36.689-0300 [LockPinger] creating distributed lock ping thread for localhost:27010 and process leonardo:27011:1450368336:1804289383 (sleeping for 30000ms)
2015-12-17T13:05:37.002-0300 [LockPinger] cluster localhost:27010 pinged successfully at Thu Dec 17 13:05:36 2015 by distributed lock pinger 'localhost:27010/leonardo:27011:1450368336:1804289383', sleeping for 30000ms
2015-12-17T13:05:37.023-0300 [mongosMain] distributed lock 'configUpgrade/leonardo:27011:1450368336:1804289383' acquired, ts : 5672dd504578da188c9b7a71
2015-12-17T13:05:37.025-0300 [mongosMain] starting upgrade of config server from v0 to v5
2015-12-17T13:05:37.025-0300 [mongosMain] starting next upgrade step from v0 to v5
2015-12-17T13:05:37.025-0300 [mongosMain] about to log new metadata event: { _id: "leonardo-2015-12-17T16:05:37-5672dd514578da188c9b7a72", server: "leonardo", clientAddr: "N/A", time: new Date(1450368337025), what: "starting upgrade of config database", ns: "config.version", details: { from: 0, to: 5 } }
2015-12-17T13:05:37.026-0300 [mongosMain] creating WriteBackListener for: localhost:27010 serverID: 000000000000000000000000
2015-12-17T13:05:37.176-0300 [mongosMain] writing initial config version at v5
2015-12-17T13:05:37.275-0300 [mongosMain] about to log new metadata event: { _id: "leonardo-2015-12-17T16:05:37-5672dd514578da188c9b7a74", server: "leonardo", clientAddr: "N/A", time: new Date(1450368337275), what: "finished upgrade of config database", ns: "config.version", details: { from: 0, to: 5 } }
2015-12-17T13:05:37.398-0300 [mongosMain] upgrade of config server to v5 successful
2015-12-17T13:05:37.399-0300 [mongosMain] distributed lock 'configUpgrade/leonardo:27011:1450368336:1804289383' unlocked. 
2015-12-17T13:05:38.475-0300 [mongosMain] scoped connection to localhost:27010 not being returned to the pool
2015-12-17T13:05:38.475-0300 [Balancer] about to contact config servers and shards
2015-12-17T13:05:38.476-0300 [mongosMain] waiting for connections on port 27011
2015-12-17T13:05:38.476-0300 [Balancer] config servers and shards contacted successfully
2015-12-17T13:05:38.476-0300 [Balancer] balancer id: leonardo:27011 started at Dec 17 13:05:38
2015-12-17T13:05:38.481-0300 [Balancer] distributed lock 'balancer/leonardo:27011:1450368336:1804289383' acquired, ts : 5672dd524578da188c9b7a77
2015-12-17T13:05:38.482-0300 [Balancer] distributed lock 'balancer/leonardo:27011:1450368336:1804289383' unlocked. 
2015-12-17T13:05:44.486-0300 [Balancer] distributed lock 'balancer/leonardo:27011:1450368336:1804289383' acquired, ts : 5672dd584578da188c9b7a78
2015-12-17T13:05:44.487-0300 [Balancer] distributed lock 'balancer/leonardo:27011:1450368336:1804289383' unlocked.
*/
```

### Cria 3 Shards

```JS
leonardo@leonardo:data$ chmod 777 -R shar1 && shard2 && shard3

leonardo@leonardo:data$ ls
configdb  db  rs1  rs2  rs3  shard1  shard2  shard3

// Executando shards
leonardo@leonardo:data$ mongod --port 27012 --dbpath /data/shard1
/*
2015-12-17T13:09:02.690-0300 
2015-12-17T13:09:02.691-0300 warning: 32-bit servers don't have journaling enabled by default. Please use --journal if you want durability.
2015-12-17T13:09:02.691-0300 
2015-12-17T13:09:02.694-0300 [initandlisten] MongoDB starting : pid=8279 port=27012 dbpath=/data/shard1 32-bit host=leonardo
2015-12-17T13:09:02.694-0300 [initandlisten] 
2015-12-17T13:09:02.694-0300 [initandlisten] ** NOTE: This is a 32 bit MongoDB binary.
2015-12-17T13:09:02.694-0300 [initandlisten] **       32 bit builds are limited to less than 2GB of data (or less with --journal).
2015-12-17T13:09:02.694-0300 [initandlisten] **       Note that journaling defaults to off for 32 bit and is currently off.
2015-12-17T13:09:02.694-0300 [initandlisten] **       See http://dochub.mongodb.org/core/32bit
2015-12-17T13:09:02.694-0300 [initandlisten] 
2015-12-17T13:09:02.694-0300 [initandlisten] db version v2.6.11
2015-12-17T13:09:02.694-0300 [initandlisten] git version: d00c1735675c457f75a12d530bee85421f0c5548
2015-12-17T13:09:02.694-0300 [initandlisten] build info: Linux ip-10-93-139-4 2.6.18-194.el5xen #1 SMP Tue Mar 16 22:08:06 EDT 2010 i686 BOOST_LIB_VERSION=1_49
2015-12-17T13:09:02.694-0300 [initandlisten] allocator: system
2015-12-17T13:09:02.694-0300 [initandlisten] options: { net: { port: 27012 }, storage: { dbPath: "/data/shard1" } }
2015-12-17T13:09:02.799-0300 [initandlisten] allocating new ns file /data/shard1/local.ns, filling with zeroes...
2015-12-17T13:09:03.100-0300 [FileAllocator] allocating new datafile /data/shard1/local.0, filling with zeroes...
2015-12-17T13:09:03.101-0300 [FileAllocator] creating directory /data/shard1/_tmp
2015-12-17T13:09:03.142-0300 [FileAllocator] done allocating datafile /data/shard1/local.0, size: 64MB,  took 0.022 secs
2015-12-17T13:09:03.144-0300 [initandlisten] build index on: local.startup_log properties: { v: 1, key: { _id: 1 }, name: "_id_", ns: "local.startup_log" }
2015-12-17T13:09:03.144-0300 [initandlisten]   added index to empty collection
2015-12-17T13:09:03.144-0300 [initandlisten] command local.$cmd command: create { create: "startup_log", size: 10485760, capped: true } ntoreturn:1 keyUpdates:0 numYields:0  reslen:37 344ms
2015-12-17T13:09:03.144-0300 [initandlisten] waiting for connections on port 27012
*/

leonardo@leonardo:data$ mongod --port 27013 --dbpath /data/shard2
/*
2015-12-17T13:09:37.551-0300 
2015-12-17T13:09:37.551-0300 warning: 32-bit servers don't have journaling enabled by default. Please use --journal if you want durability.
2015-12-17T13:09:37.551-0300 
2015-12-17T13:09:37.554-0300 [initandlisten] MongoDB starting : pid=8337 port=27013 dbpath=/data/shard2 32-bit host=leonardo
2015-12-17T13:09:37.554-0300 [initandlisten] 
2015-12-17T13:09:37.554-0300 [initandlisten] ** NOTE: This is a 32 bit MongoDB binary.
2015-12-17T13:09:37.554-0300 [initandlisten] **       32 bit builds are limited to less than 2GB of data (or less with --journal).
2015-12-17T13:09:37.555-0300 [initandlisten] **       Note that journaling defaults to off for 32 bit and is currently off.
2015-12-17T13:09:37.555-0300 [initandlisten] **       See http://dochub.mongodb.org/core/32bit
2015-12-17T13:09:37.555-0300 [initandlisten] 
2015-12-17T13:09:37.555-0300 [initandlisten] db version v2.6.11
2015-12-17T13:09:37.555-0300 [initandlisten] git version: d00c1735675c457f75a12d530bee85421f0c5548
2015-12-17T13:09:37.555-0300 [initandlisten] build info: Linux ip-10-93-139-4 2.6.18-194.el5xen #1 SMP Tue Mar 16 22:08:06 EDT 2010 i686 BOOST_LIB_VERSION=1_49
2015-12-17T13:09:37.555-0300 [initandlisten] allocator: system
2015-12-17T13:09:37.555-0300 [initandlisten] options: { net: { port: 27013 }, storage: { dbPath: "/data/shard2" } }
2015-12-17T13:09:37.595-0300 [initandlisten] allocating new ns file /data/shard2/local.ns, filling with zeroes...
2015-12-17T13:09:37.884-0300 [FileAllocator] allocating new datafile /data/shard2/local.0, filling with zeroes...
2015-12-17T13:09:37.884-0300 [FileAllocator] creating directory /data/shard2/_tmp
2015-12-17T13:09:37.927-0300 [FileAllocator] done allocating datafile /data/shard2/local.0, size: 64MB,  took 0.022 secs
2015-12-17T13:09:37.928-0300 [initandlisten] build index on: local.startup_log properties: { v: 1, key: { _id: 1 }, name: "_id_", ns: "local.startup_log" }
2015-12-17T13:09:37.928-0300 [initandlisten]   added index to empty collection
2015-12-17T13:09:37.929-0300 [initandlisten] command local.$cmd command: create { create: "startup_log", size: 10485760, capped: true } ntoreturn:1 keyUpdates:0 numYields:0  reslen:37 333ms
2015-12-17T13:09:37.929-0300 [initandlisten] waiting for connections on port 27013
*/

leonardo@leonardo:data$ mongod --port 27014 --dbpath /data/shard3
/*
2015-12-17T13:10:31.249-0300 
2015-12-17T13:10:31.249-0300 warning: 32-bit servers don't have journaling enabled by default. Please use --journal if you want durability.
2015-12-17T13:10:31.249-0300 
2015-12-17T13:10:31.253-0300 [initandlisten] MongoDB starting : pid=8454 port=27014 dbpath=/data/shard3 32-bit host=leonardo
2015-12-17T13:10:31.253-0300 [initandlisten] 
2015-12-17T13:10:31.253-0300 [initandlisten] ** NOTE: This is a 32 bit MongoDB binary.
2015-12-17T13:10:31.253-0300 [initandlisten] **       32 bit builds are limited to less than 2GB of data (or less with --journal).
2015-12-17T13:10:31.253-0300 [initandlisten] **       Note that journaling defaults to off for 32 bit and is currently off.
2015-12-17T13:10:31.253-0300 [initandlisten] **       See http://dochub.mongodb.org/core/32bit
2015-12-17T13:10:31.253-0300 [initandlisten] 
2015-12-17T13:10:31.253-0300 [initandlisten] db version v2.6.11
2015-12-17T13:10:31.253-0300 [initandlisten] git version: d00c1735675c457f75a12d530bee85421f0c5548
2015-12-17T13:10:31.253-0300 [initandlisten] build info: Linux ip-10-93-139-4 2.6.18-194.el5xen #1 SMP Tue Mar 16 22:08:06 EDT 2010 i686 BOOST_LIB_VERSION=1_49
2015-12-17T13:10:31.253-0300 [initandlisten] allocator: system
2015-12-17T13:10:31.253-0300 [initandlisten] options: { net: { port: 27014 }, storage: { dbPath: "/data/shard3" } }
2015-12-17T13:10:31.295-0300 [initandlisten] allocating new ns file /data/shard3/local.ns, filling with zeroes...
2015-12-17T13:10:31.573-0300 [FileAllocator] allocating new datafile /data/shard3/local.0, filling with zeroes...
2015-12-17T13:10:31.573-0300 [FileAllocator] creating directory /data/shard3/_tmp
2015-12-17T13:10:31.615-0300 [FileAllocator] done allocating datafile /data/shard3/local.0, size: 64MB,  took 0.022 secs
2015-12-17T13:10:31.617-0300 [initandlisten] build index on: local.startup_log properties: { v: 1, key: { _id: 1 }, name: "_id_", ns: "local.startup_log" }
2015-12-17T13:10:31.617-0300 [initandlisten]   added index to empty collection
2015-12-17T13:10:31.617-0300 [initandlisten] command local.$cmd command: create { create: "startup_log", size: 10485760, capped: true } ntoreturn:1 keyUpdates:0 numYields:0  reslen:37 322ms
2015-12-17T13:10:31.617-0300 [initandlisten] waiting for connections on port 27014
*/

// Resgistrando os Shards no Router
leonardo@leonardo:/data$ mongo --port 27011 --host localhost
MongoDB shell version: 2.6.11
connecting to: localhost:27011/test
Mongo-Hacker 0.0.9
mongos> 

mongos> sh.addShard("localhost:27012")
{
  "shardAdded": "shard0000",
  "ok": 1
}

leonardo:27011(mongos-2.6.11)[mongos] test> sh.addShard("localhost:27013")
{
  "shardAdded": "shard0001",
  "ok": 1
}

leonardo:27011(mongos-2.6.11)[mongos] test> sh.addShard("localhost:27014")
{
  "shardAdded": "shard0002",
  "ok": 1
}

// Indicando a databse que vamos shardear
leonardo:27011(mongos-2.6.11)[mongos] test> sh.enableSharding("finalproject")
{
  "ok": 1
}

// Qual collections vamos shardear
leonardo:27011(mongos-2.6.11)[mongos] finalproject> sh.shardCollection("finalproject.project", {"_id" : 1})
{
  "collectionsharded": "finalproject.project",
  "ok": 1
}

// Envia dados para o Router
for ( i = 1; i < 100000; i++ ) {
  var doc = { 
    name             : "project"+i,
    description      : "description"+i,
    date_begin       : new Date('January 15, 2015 03:24:00'),
    date_dream       : new Date('December 15, 2016 03:24:00'),
    date_end         : new Date('November 25, 2016 03:24:00'),
    visible          : true,
    realocate        : null,
    expired          : false,
    visualizable_mod : "somevalue",
    members          :[],
    goal             : [],
    tags             : []
  };

  db.project.insert(doc);
}

```
