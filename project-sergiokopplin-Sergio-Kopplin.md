# MongoDb - Projeto Final
**Autor:** Sergio Kopplin
**Data:** 1454027896144

## Para qual sistema você usaria o MogoDB (diferente desse)?
Chat, um game, um robô de monitoramento ou algo relacionado a APIs.

## Qual a modelagem da sua coleção de `users`?

```JS
users: {
    _id: ObjectId(),
    name: String,
    bio: String,
    date-register: Date,
    avatar-path: String,
    background-path: String,

    auth: {
        username: String,
        email: String,
        password: String,
        last-access: Date,
        online: String,
        disabled: Boolean,
        hash-token: String
    }
}
```

Foi modelado dessa maneira para seguir os padrões aprendidos em aula acerca de banco de dados não relacionais. As escolhas tem como objetivo diminuir os relacionamento entre as tabelas e criar collections que contenham todos os dados necessários para cada usuário.

## Qual a modelagem da sua coleção de `projects`?

```JS
project: {
    _id: ObjectId(),
    name: String,
    description: String,
    date_begin: Date,
    date_dream: Date,
    date_end: Date,
    visible: Boolean,
    realocate: Boolean,
    expired: Date,
    visualizable_mod: String,

    members: [{
        _id: ObjectId(),
        type_member: String,
        notify: String
    }],

    goal: [{
        _id: ObjectId(),
        name: String,
        date_begin: Date,
        date_dream: Date,
        date_end: Date,
        realocate: Boolean,
        expired: Date,

        historic: {
            date_realocate: Date
        },

        tags: [{
            tag_name: String
        }]
    }],

    tag: [{
        name: String
    }]
}
```

Neste exercício tentei isolar as atividade dos projetos.

## Qual a modelagem da sua coleção retirada de `projects`?

```JS
activity: [{
    _id: ObjectId(),
    name: String,
    description: String,
    date_begin: Date,
    date_dream: Date,
    date_end: Date,
    realocate: Boolean,
    expired: Date,
    id_goal: ObjectId(),

    historic: {
        date_realocate: Date
    },

    tag: [],

    members: [{
        _id: ObjectId(),
        notify: Boolean
    }],

    comment: [{
        _id: ObjectId(),
        text: String,
        date_comment: Date,

        members: [{
            _id: ObjectId(),
            notify: Boolean
        }],

        file: [{
            _id: ObjectId(),
            path: String,
            weight: String,
            name: String
        }]
    }],
}],
```

## Create - cadastro

1. Cadastre 10 usuários diferentes.

```JS
var users = [
    "Renato Galvao",
    "Rosangela Maria",
    "Mario Quintana",
    "Jose Saramago",
    "Diego Silva",
    "Eduardo Pires",
    "Valeska Popozuda",
    "Mirtes Aragao",
    "Flavio Amantes",
    "Maria Jose"
];

insert = [];

for(i = 0; i <= 9; i++){
    insert[i] = {
        "_id": new ObjectId(),
        "name": users[i],
        "bio": "Lorem Ipsum Dolor",
        "date-register": new Date(),
        "avatar-path": "http://lorempixel.com/400/200/people/",
        "background-path": "http://lorempixel.com/400/200/people/",

        "auth": {
            "username": users[i].toLowerCase().replace(/\s/g,''),
            "email": users[i].toLowerCase().replace(/\s/g,'') + "@email.com",
            "password": Math.floor(Math.random() * 10000),
            "last-access": new Date(),
            "online": true,
            "disabled": false,
            "hash-token": "ff2e7d2c723b917742597eb8d9b68653"
        }
    };

    db.users.insert(insert[i]);
}

/*
WriteResult({ "nInserted" : 1 })
*/
```

2. Cadastre 5 projetos diferentes.
    - cada um com 5 membros, sempre diferentes dentro dos projetos;
    - cada um com pelo menos 3 tags diferentes;
        - escolha 1 *tag* onde deva ficar em 2 projetos;
        - escolha 1 *tag* onde deva ficar em 3 projetos;
    - cada projeto com pelo menos 1 *goal*;
        - cada goal com pelo menos 3 tags;
        - cada goal com pelo menos 2 atividades, deixe 1 projeto sem.

```JS
// 2. Cadastre 5 projetos diferentes.
var projects = [ "Bootstrap", "Foundation", "Pure", "Normalize", "Reset" ];

// - cada um com 5 membros, sempre diferentes dentro dos projetos;
var members = db.users.find({ }, { _id: 1, name: 1 });

var membersByProject = [
    [ members[0], members[1], members[2], members[3], members[4] ],
    [ members[1], members[2], members[3], members[4], members[5] ],
    [ members[2], members[3], members[4], members[5], members[6] ],
    [ members[3], members[4], members[5], members[6], members[7] ],
    [ members[4], members[5], members[6], members[7], members[8] ]
];

membersByProject.forEach(function(obj, x){
    obj.forEach(function(single, y){
        var type_member = y % 2 == 0 ? true : false;
        var notify = y % 3 == 0 ? true : false;
        membersByProject[x][y]["type_member"] = type_member;
        membersByProject[x][y]["notify"] = notify;
    });
});

// - cada um com pelo menos 3 tags diferentes;
// - escolha 1 *tag* onde deva ficar em 2 projetos;
// - escolha 1 *tag* onde deva ficar em 3 projetos;
var tags = [ "framework", "css", "less", "sass", "stylus", "js", "angular", "react", "mvc", "jquery", "backbone", "ember" ];
var tagsList = [
    [ "framework", "less", "sass" ],
    [ "framework", "stylus", "js" ],
    [ "css", "angular", "react" ],
    [ "css", "mvc", "jquery" ],
    [ "css", "backbone", "ember" ]
];

// - cada projeto com pelo menos 1 *goal*;
//    - cada goal com pelo menos 3 tags;
//    - cada goal com pelo menos 2 atividades, deixe 1 projeto sem.
for (i = 1; i <= 5 ; i++) {

    var insert = {
        _id: new ObjectId(),
        name: "activity-" + i,
        description: "description activity " + i,
        date_begin: new Date(),
        date_dream: new Date(),
        date_end: new Date(),
        realocate: i % 2 == 0 ? true : false,
        expired: new Date(),

        members: [],

        historic: {},

        comments: [],

        tag: []
    };

    db.activity.insert(insert);
};

// Inserted 1 record(s) in 74ms
// Inserted 1 record(s) in 1ms
// Inserted 1 record(s) in 1ms
// Inserted 1 record(s) in 0ms
// Inserted 1 record(s) in 1ms
// WriteResult({
  // "nInserted": 1
// })

var activity = db.activity.find();
var activityByGoal = [
    [ activity[0]._id, activity[1]._id ],
    [ activity[1]._id, activity[2]._id ],
    [ activity[2]._id, activity[3]._id ],
    [ activity[3]._id, activity[4]._id ],
    []
];

var goals = [];
var goal = [ "expansao", "bugs", "improvements", "ideas", "todo" ];
goal.forEach(function (value, index) {
    goals[index] = {
        _id: new ObjectId(),
        name: goal[index],
        date_begin: new Date(),
        date_dream: new Date(),
        date_end: new Date(),
        realocate: index % 3 == 0 ? true : false,
        expired: new Date(),

        historic: {
            date_realocate: new Date()
        },

        tags: tagsList[index],

        activity: activityByGoal[index]
    }
});

projects.forEach(function (value, index) {
    insert[index] = {
        "_id": new ObjectId(),
        "name": projects[index],
        "description": projects[index] + ": Lorem Ipsum Dolor",
        "date_begin": new Date(),
        "date_dream": new Date(),
        "date_end": new Date(),
        "visible": index % 2 == 0 ? true : false,
        "realocate": index % 3 == 0 ? true : false,
        "expired": new Date(),
        "visualizable_mod": index % 5 == 0 ? true : false,

        members: membersByProject[index],

        goal: goals[index],

        tag: tagsList[index]
    };

    db.project.insert(insert[index]);
});

// Inserted 1 record(s) in 2ms
// Inserted 1 record(s) in 1ms
// Inserted 1 record(s) in 0ms
// Inserted 1 record(s) in 1ms
// Inserted 1 record(s) in 1ms
```

## Retrieve - busca

1: Liste as informações dos membros de 1 projeto específico que deve ser buscado pelo seu nome de forma a não ligar para maiúsculas e minúsculas.
```JS
db.project.find({ "members.name": /dIego siLva/i }, { "members.name": 1 })

// {
//   "_id": ObjectId("56bd2fbd0db8f24d71358736"),
//   "members": [
//     {
//       "name": "Renato Galvao"
//     },
//     {
//       "name": "Rosangela Maria"
//     },
//     {
//       "name": "Mario Quintana"
//     },
//     {
//       "name": "Jose Saramago"
//     },
//     {
//       "name": "Diego Silva"
//     }
//   ]
// }
// {
//   "_id": ObjectId("56bd2fbd0db8f24d71358737"),
//   "members": [
//     {
//       "name": "Rosangela Maria"
//     },
//     {
//       "name": "Mario Quintana"
//     },
//     {
//       "name": "Jose Saramago"
//     },
//     {
//       "name": "Diego Silva"
//     },
//     {
//       "name": "Eduardo Pires"
//     }
//   ]
// }
// {
//   "_id": ObjectId("56bd2fbd0db8f24d71358738"),
//   "members": [
//     {
//       "name": "Mario Quintana"
//     },
//     {
//       "name": "Jose Saramago"
//     },
//     {
//       "name": "Diego Silva"
//     },
//     {
//       "name": "Eduardo Pires"
//     },
//     {
//       "name": "Valeska Popozuda"
//     }
//   ]
// }
// {
//   "_id": ObjectId("56bd2fbd0db8f24d71358739"),
//   "members": [
//     {
//       "name": "Jose Saramago"
//     },
//     {
//       "name": "Diego Silva"
//     },
//     {
//       "name": "Eduardo Pires"
//     },
//     {
//       "name": "Valeska Popozuda"
//     },
//     {
//       "name": "Mirtes Aragao"
//     }
//   ]
// }
// {
//   "_id": ObjectId("56bd2fbd0db8f24d7135873a"),
//   "members": [
//     {
//       "name": "Diego Silva"
//     },
//     {
//       "name": "Eduardo Pires"
//     },
//     {
//       "name": "Valeska Popozuda"
//     },
//     {
//       "name": "Mirtes Aragao"
//     },
//     {
//       "name": "Flavio Amantes"
//     }
//   ]
// }

// Fetched 5 record(s) in 2ms
```

2: Liste todos os projetos com a tag que você escolheu para os 3 projetos em comum.
```JS
db.project.find({ "tag": /css/i }, { tag: 1 })

// {
//   "_id": ObjectId("56bd2fbd0db8f24d71358738"),
//   "tag": [
//     "css",
//     "angular",
//     "react"
//   ]
// }
// {
//   "_id": ObjectId("56bd2fbd0db8f24d71358739"),
//   "tag": [
//     "css",
//     "mvc",
//     "jquery"
//   ]
// }
// {
//   "_id": ObjectId("56bd2fbd0db8f24d7135873a"),
//   "tag": [
//     "css",
//     "backbone",
//     "ember"
//   ]
// }
// Fetched 3 record(s) in 1ms
```

3: Liste apenas os nomes de todas as atividades para todos os projetos.

```JS
db.activity.find({}, { name: 1, _id: 0 })

// {
//   "name": "activity-1"
// }
// {
//   "name": "activity-2"
// }
// {
//   "name": "activity-3"
// }
// {
//   "name": "activity-4"
// }
// {
//   "name": "activity-5"
// }

// Fetched 5 record(s) in 2ms
```

4. Liste todos os projetos que não possuam uma tag
```js
db.project.find({ tag: {} }, { tag: 1 });

// Fetched 0 record(s) in 1ms
```

5. Liste todos os usuários que não fazem parte do primeiro projeto cadastrado.
```JS
var members = [];

function getId(member) {
    members.push(member._id);
};

var project = db.project.findOne({ name : /bootstrap/i }).members.forEach(getId);

db.users.find({ _id : { $not : { $in : members }}});

// {
//   "_id": ObjectId("56bd2f7b0db8f24d71358727"),
//   "name": "Eduardo Pires",
//   "bio": "Lorem Ipsum Dolor",
//   "date-register": ISODate("2016-02-12T01:03:55.647Z"),
//   "avatar-path": "http://lorempixel.com/400/200/people/",
//   "background-path": "http://lorempixel.com/400/200/people/",
//   "auth": {
//     "username": "eduardopires",
//     "email": "eduardopires@email.com",
//     "password": 4843,
//     "last-access": ISODate("2016-02-12T01:03:55.647Z"),
//     "online": true,
//     "disabled": false,
//     "hash-token": "ff2e7d2c723b917742597eb8d9b68653"
//   }
// }
// {
//   "_id": ObjectId("56bd2f7b0db8f24d71358728"),
//   "name": "Valeska Popozuda",
//   "bio": "Lorem Ipsum Dolor",
//   "date-register": ISODate("2016-02-12T01:03:55.648Z"),
//   "avatar-path": "http://lorempixel.com/400/200/people/",
//   "background-path": "http://lorempixel.com/400/200/people/",
//   "auth": {
//     "username": "valeskapopozuda",
//     "email": "valeskapopozuda@email.com",
//     "password": 6924,
//     "last-access": ISODate("2016-02-12T01:03:55.648Z"),
//     "online": true,
//     "disabled": false,
//     "hash-token": "ff2e7d2c723b917742597eb8d9b68653"
//   }
// }
// {
//   "_id": ObjectId("56bd2f7b0db8f24d71358729"),
//   "name": "Mirtes Aragao",
//   "bio": "Lorem Ipsum Dolor",
//   "date-register": ISODate("2016-02-12T01:03:55.648Z"),
//   "avatar-path": "http://lorempixel.com/400/200/people/",
//   "background-path": "http://lorempixel.com/400/200/people/",
//   "auth": {
//     "username": "mirtesaragao",
//     "email": "mirtesaragao@email.com",
//     "password": 2168,
//     "last-access": ISODate("2016-02-12T01:03:55.648Z"),
//     "online": true,
//     "disabled": false,
//     "hash-token": "ff2e7d2c723b917742597eb8d9b68653"
//   }
// }
// {
//   "_id": ObjectId("56bd2f7b0db8f24d7135872a"),
//   "name": "Flavio Amantes",
//   "bio": "Lorem Ipsum Dolor",
//   "date-register": ISODate("2016-02-12T01:03:55.649Z"),
//   "avatar-path": "http://lorempixel.com/400/200/people/",
//   "background-path": "http://lorempixel.com/400/200/people/",
//   "auth": {
//     "username": "flavioamantes",
//     "email": "flavioamantes@email.com",
//     "password": 3127,
//     "last-access": ISODate("2016-02-12T01:03:55.649Z"),
//     "online": true,
//     "disabled": false,
//     "hash-token": "ff2e7d2c723b917742597eb8d9b68653"
//   }
// }
// {
//   "_id": ObjectId("56bd2f7b0db8f24d7135872b"),
//   "name": "Maria Jose",
//   "bio": "Lorem Ipsum Dolor",
//   "date-register": ISODate("2016-02-12T01:03:55.649Z"),
//   "avatar-path": "http://lorempixel.com/400/200/people/",
//   "background-path": "http://lorempixel.com/400/200/people/",
//   "auth": {
//     "username": "mariajose",
//     "email": "mariajose@email.com",
//     "password": 9628,
//     "last-access": ISODate("2016-02-12T01:03:55.649Z"),
//     "online": true,
//     "disabled": false,
//     "hash-token": "ff2e7d2c723b917742597eb8d9b68653"
//   }
// }
// Fetched 5 record(s) in 3ms
```

## Update - alteração

1. Adicione para todos os projetos o campo `views: 0`.
```JS
db.project.update( {}, { $set: { views: 0 } }, { multi: true } );

// Updated 5 existing record(s) in 11ms
// WriteResult({
//   "nMatched": 5,
//   "nUpserted": 0,
//   "nModified": 5
// })
```

2. Adicione 1 tag diferente para cada projeto.
```JS
tags = [ "html5", "css3", "zepto", "emmet", "editorconfig" ];

db.project.update({ name: /Bootstrap/i }, { $push: { tag: tags[0] } });
// Updated 1 existing record(s) in 4ms
// WriteResult({
//   "nMatched": 1,
//   "nUpserted": 0,
//   "nModified": 1
// })

db.project.update({ name: /Foundation/i }, { $push: { tag: tags[1] } });
// Updated 1 existing record(s) in 1ms
// WriteResult({
//   "nMatched": 1,
//   "nUpserted": 0,
//   "nModified": 1
// })

db.project.update({ name: /Pure/i }, { $push: { tag: tags[2] } });
// Updated 1 existing record(s) in 1ms
// WriteResult({
//   "nMatched": 1,
//   "nUpserted": 0,
//   "nModified": 1
// })

db.project.update({ name: /Reset/i }, { $push: { tag: tags[3] } });
// Updated 1 existing record(s) in 2ms
// WriteResult({
//   "nMatched": 1,
//   "nUpserted": 0,
//   "nModified": 1
// })

db.project.update({ name: /Normalize/i }, { $push: { tag: tags[4] } });
// Updated 1 existing record(s) in 1ms
// WriteResult({
//   "nMatched": 1,
//   "nUpserted": 0,
//   "nModified": 1
// })
```

3. Adicione 2 membros diferentes para cada projeto.
```JS
var members = db.users.find({ }, { _id: 1, name: 1 });

var membersByProject = [
    [ members[0], members[1] ],
    [ members[1], members[2] ],
    [ members[2], members[3] ],
    [ members[3], members[4] ],
    [ members[4], members[5] ]
];

membersByProject.forEach(function(obj, x){
    obj.forEach(function(single, y){
        var type_member = y % 2 == 0 ? true : false;
        var notify = y % 3 == 0 ? true : false;
        membersByProject[x][y]["type_member"] = type_member;
        membersByProject[x][y]["notify"] = notify;
    });
});

db.project.update({ name: /bootstrap/i }, { $push: { members: membersByProject[0][0] } });
// Updated 1 existing record(s) in 6ms
// WriteResult({
//   "nMatched": 1,
//   "nUpserted": 0,
//   "nModified": 1
// })
db.project.update({ name: /bootstrap/i }, { $push: { members: membersByProject[0][1] } });
// Updated 1 existing record(s) in 6ms
// WriteResult({
//   "nMatched": 1,
//   "nUpserted": 0,
//   "nModified": 1
// })

db.project.update({ name: /Foundation/i }, { $push: { members: membersByProject[1][0] } });
// Updated 1 existing record(s) in 6ms
// WriteResult({
//   "nMatched": 1,
//   "nUpserted": 0,
//   "nModified": 1
// })
db.project.update({ name: /Foundation/i }, { $push: { members: membersByProject[1][1] } });
// Updated 1 existing record(s) in 2ms
// WriteResult({
//   "nMatched": 1,
//   "nUpserted": 0,
//   "nModified": 1
// })

db.project.update({ name: /Pure/i }, { $push: { members: membersByProject[2][0] } });
// Updated 1 existing record(s) in 6ms
// WriteResult({
//   "nMatched": 1,
//   "nUpserted": 0,
//   "nModified": 1
// })
db.project.update({ name: /Pure/i }, { $push: { members: membersByProject[2][1] } });
// Updated 1 existing record(s) in 2ms
// WriteResult({
//   "nMatched": 1,
//   "nUpserted": 0,
//   "nModified": 1
// })

db.project.update({ name: /Reset/i }, { $push: { members: membersByProject[3][0] } })
// Updated 1 existing record(s) in 6ms
// WriteResult({
//   "nMatched": 1,
//   "nUpserted": 0,
//   "nModified": 1
// })
db.project.update({ name: /Reset/i }, { $push: { members: membersByProject[3][1] } })
// WriteResult({
//   "nMatched": 1,
//   "nUpserted": 0,
//   "nModified": 1
// })

db.project.update({ name: /Normalize/i }, { $push: { members: membersByProject[4][0] } });
// Updated 1 existing record(s) in 6ms
// WriteResult({
//   "nMatched": 1,
//   "nUpserted": 0,
//   "nModified": 1
// })
db.project.update({ name: /Normalize/i }, { $push: { members: membersByProject[4][1] } });
// Updated 1 existing record(s) in 2ms
// WriteResult({
//   "nMatched": 1,
//   "nUpserted": 0,
//   "nModified": 1
// })
```

4. Adicione 1 comentário em cada atividade, deixe apenas 1 projeto sem.
```JS

var comments = {
    "_id": new ObjectId(),
    "text": "Lorem Ipsum Dolor",
    "date_comment": new Date(),

    "members": [],

    "file": []
};

db.activity.update({ "name": /activity-1/i }, { $push: { comments } } );
// Updated 1 existing record(s) in 1ms
// WriteResult({
  // "nMatched": 1,
  // "nUpserted": 0,
  // "nModified": 1
// })
db.activity.update({ "name": /activity-2/i }, { $push: { comments } } );
// Updated 1 existing record(s) in 1ms
// WriteResult({
  // "nMatched": 1,
  // "nUpserted": 0,
  // "nModified": 1
// })
db.activity.update({ "name": /activity-3/i }, { $push: { comments } } );
// Updated 1 existing record(s) in 1ms
// WriteResult({
  // "nMatched": 1,
  // "nUpserted": 0,
  // "nModified": 1
// })
db.activity.update({ "name": /activity-4/i }, { $push: { comments } } );
// Updated 1 existing record(s) in 1ms
// WriteResult({
  // "nMatched": 1,
  // "nUpserted": 0,
  // "nModified": 1
// })
```

5. Adicione 1 projeto inteiro com **UPSERT**.
```JS
var projects = [ "Yahoo" ];

var members = db.users.find({ }, { _id: 1, name: 1 });

var membersByProject = [
    [ members[0], members[1], members[2], members[3], members[4] ]
];

membersByProject.forEach(function(obj, x){
    obj.forEach(function(single, y){
        var type_member = y % 2 == 0 ? true : false;
        var notify = y % 3 == 0 ? true : false;
        membersByProject[x][y]["type_member"] = type_member;
        membersByProject[x][y]["notify"] = notify;
    });
});

var tags = [ "framework", "css", "less", "sass", "stylus", "js", "angular", "react", "mvc", "jquery" ];
var tagsList = [
    [ "jquery", tags[3], tags[2] ]
];

var activity = db.activity.find();
var activityByGoal = [
    [ activity[0]._id, activity[1]._id ]
];

var goals = [];
var goal = [ "expansao" ];
goal.forEach(function (value, index) {
    goals[index] = {
        _id: new ObjectId(),
        name: goal[index],
        date_begin: new Date(),
        date_dream: new Date(),
        date_end: new Date(),
        realocate: index % 3 == 0 ? true : false,
        expired: new Date(),

        historic: {
            date_realocate: new Date()
        },

        tags: tagsList[index],

        activity: activityByGoal[index]
    }
});

insert = {
    $setOnInsert: {
        "_id": new ObjectId(),
        "name": "Yahoo",
        "description": "Yahoo Lorem Ipsum Dolor",
        "date_begin": new Date(),
        "date_dream": new Date(),
        "date_end": new Date(),
        "visible": 1 % 2 == 0 ? true : false,
        "realocate": 1 % 3 == 0 ? true : false,
        "expired": new Date(),
        "visualizable_mod": 1 % 5 == 0 ? true : false,

        members: membersByProject[0],

        goal: goals[0],

        tag: tagsList[0]
    }

    db.project.update({ name: /Yahoo/i }, insert, { upsert: true });
});

// Updated 1 new record(s) in 1ms
// WriteResult({
  // "nMatched": 0,
  // "nUpserted": 1,
  // "nModified": 0,
  // "_id": ObjectId("56b41579a38ae7ad096fe02d")
// })
```

## Delete - remoção

1. Apague todos os projetos que não possuam *tags*.
```JS
db.project.remove({ tag: [] })

// Removed 0 record(s) in 50ms
// WriteResult({
  // "nRemoved": 0
// })
```

2. Apague todos os projetos que não possuam comentários nas atividades.
```JS
semComentarios = [];

db.activity.find({ comments: [] }, { }).forEach(function(object){ semComentarios.push(object._id) });

db.project.find({ "goal.activity": { $in: semComentarios } }).forEach(function(object){
    db.project.remove({ name: object.name });
});

// Removed 1 record(s) in 1ms
```

3. Apague todos os projetos que não possuam atividades.
```JS
db.project.remove({ "goal.activity": [] })

// Removed 1 record(s) in 1ms
// WriteResult({
  // "nRemoved": 1
// })
```

4. Escolha 2 usuário e apague todos os projetos em que os 2 fazem parte.
```JS
var users = [ ObjectId("56b396a9c490c6f452623369"), ObjectId("56b396a6c490c6f452623368") ];

db.project.remove({ "members._id": { $in: users } })

// Removed 3 record(s) in 1ms
// WriteResult({
//   "nRemoved": 3
// })
```

5. Apague todos os projetos que possuam uma determinada *tag* em *goal*.
```JS
var tag = [ /react/i ];

db.project.remove({ tag: { $in: tag } })

// Removed 1 record(s) in 1ms
// WriteResult({
//   "nRemoved": 1
// })
```

## Gerenciamento de usuários

1. Crie um usuário com permissões **APENAS** de Leitura.
```JS
db.createUser({
    user: "usernew",
    pwd: "123456",
    roles: [{ role: "read", db: "projects" }]
});

// Successfully added user: {
//   "user": "usernew",
//   "roles": [
//     {
//       "role": "read",
//       "db": "projects"
//     }
//   ]
// }
```

2. Crie um usuário com permissões de Escrita e Leitura.
```JS
db.createUser({
    user: "usernew2",
    pwd: "123456",
    roles: [{ role: "readWrite", db: "projects" }]
});

// Successfully added user: {
//   "user": "usernew2",
//   "roles": [
//     {
//       "role": "readWrite",
//       "db": "projects"
//     }
//   ]
// };
```

3. Adicionar o papel `grantRolesToUser` e `revokeRole` para o usuário com Escrita e Leitura.
```JS
db.createRole({
    role: "grantRolesToUser",
    privileges: [
        { resource: { db: "projects", collection: "project" }, actions: [ "find" ] },
        { resource: { db: "projects", collection: "project" }, actions: ["grantRole"] }
    ],
    roles: [{ role: "readWrite", db: "projects" }]
});

// {
//   "role": "grantRolesToUser",
//   "privileges": [
//     {
//       "resource": {
//         "db": "projects",
//         "collection": "project"
//       },
//      "actions": [
//        "find"
//      ]
//    },
//   {
//     "resource": {
//       "db": "projects",
//        "collection": "project"
//      },
//      "actions": [
//        "grantRole"
//      ]
//    }
//  ],
//  "roles": [
//    {
//      "role": "readWrite",
//      "db": "projects"
//    }
//  ]
//}

db.createRole({
    role: "revokeRole",
    privileges: [
        { resource: { db: "projects", collection: "project" }, actions: [ "find" ] },
        { resource: { db: "projects", collection: "project" }, actions: ["grantRole"] }
    ],
    roles: [{ role: "readWrite", db: "projects" }]
});

// {
//   "role": "revokeRole",
//   "privileges": [
//     {
//       "resource": {
//        "db": "projects",
//         "collection": "project"
//       },
//       "actions": [
//         "find"
//       ]
//     },
//     {
//       "resource": {
//         "db": "projects",
//         "collection": "project"
//       },
//       "actions": [
//         "grantRole"
//       ]
//     }
//   ],
//   "roles": [
//     {
//       "role": "readWrite",
//       "db": "projects"
//     }
//   ]
// }

db.updateUser( "usernew2", {
    roles: [
        { role: "grantRolesToUser", db: "projects"  },
        { role: "revokeRole", db: "projects"  }
    ]
});
```

4. Remover o papel `grantRolesToUser` para o usuário com Escrita e Leitura.
```JS
db.runCommand({
    revokeRolesFromUser: "usernew2",
    roles: [
        { role: "grantRolesToUser", db: "projects" }
    ]
});

// {
//   "ok": 1
// }
```

5. Listar todos os usuários com seus papéis e ações.
```JS
db.getUsers();

// [
//   {
//     "_id": "projects.usernew",
//     "user": "usernew",
//     "db": "projects",
//     "roles": [
//       {
//         "role": "read",
//         "db": "projects"
//       }
//     ]
//   },
//   {
//     "_id": "projects.usernew2",
//     "user": "usernew2",
//     "db": "projects",
//     "roles": [
//       {
//         "role": "revokeRole",
//         "db": "projects"
//       }
//     ]
//   }
// ]
```

## Cluster

Depois de criada toda sua base você deverá criar um cluster utilizando:

* 1 Router
* 1 Config Server
* 3 Shardings
* 3 Replicas

- Replicas

```JS
sudo mkdir data

sudo mkdir rs1
sudo mkdir rs2
sudo mkdir rs3

sudo mongod --replSet replicacao --port 27017 --dbpath /data/rs1
sudo mongod --replSet replicacao --port 27018 --dbpath /data/rs2
sudo mongod --replSet replicacao --port 27019 --dbpath /data/rs3
mongo --port 27017

var rsconf = { _id: "replicacao", members: [{ _id: 0, host: "127.0.0.1:27017" }]};

rs.initiate(rsconf)

rs.add("127.0.0.1:27018")
rs.add("127.0.0.1:27019")
```

- Shardings

```JS
/data

mongod --configsvr --port 27010
```

- Cria Router

```JS
mongos --configdb localhost:27010 --port 27011
```

- Cria Shards

```JS
chmod 777 -R sha1
chmod 777 -R sha2
chmod 777 -R sha3

mongod --port 27012 --dbpath /data/sha1

mongod --port 27013 --dbpath /data/sha2

mongod --port 27014 --dbpath /data/sha3

mongo --port 27011 --host localhost

> sh.addShard("localhost:27012")
> sh.addShard("localhost:27013")
> sh.addShard("localhost:27014")

> sh.enableSharding("projeto")

> sh.shaCollection("projeto.project", {"_id" : 1})
