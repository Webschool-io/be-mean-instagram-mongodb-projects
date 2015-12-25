# MongoDb - Projeto Final
- **Autor:** Lucas de Oliveira
- **Data:** 1450544430837

## Para qual sistema você usaria o MogoDB (diferente desse)?
Quando comecei a estudar MongoDb poderia dizer que em um sistema onde é necessário manter os dados consistentes, não utilizaria, mas atualmente vejo aplicações como, sistema de pagamento, CMS, E-commerce, Redes Sociais, entre outros, desenvolvido com o MongoDb. Sendo assim, posso dizer que para quase qualquer tipo de sistema, incluindo os citados anteriormente, utilizaria o MongoDB, digo **quase qualquer tipo** por que obviamente dependendo do problema pode ser que exista algum banco de dados especifico como solução.

## Qual a modelagem da sua coleção de `users`?
```js
{
    "name" : String,
    "bio" : String,
    "date_register" : String,
    "avatar_path" : String,
    "background_path" : String,
    "auth" : {
        "username" : String,
        "email" : String,
        "password" : String,
        "last_access" : Date,
        "online" : boolean,
        "disabled" : boolean,
        "hash_token" : String
    }
}
```

## Qual a modelagem da sua coleção de `projects`?

```js
{
    "name" : String,
    "description" : String,
    "date_dream" : Date,
    "date_begin" : Date,
    "date_end" : Date,
    "visible" : boolean,
    "realocate" : boolean,
    "expired" : Date,
    "visualizable_mod" : String,
    "members" : [
        {
            "type_member" : String,
            "user_id" : String,
            "notify" : boolean
        }
    ],
    "tags" : [],
    "goals" : [
        {
            "name" : String,
            "description" : String,
            "date_begin" : Date,
            "date_dream" : Date,
            "date_end" : Date,
            "realocate" : boolean,
            "expired" : Date,
            "tags" : [],
            "activities" : [
                {
                    "activity_id" : String
                }
            ],
            "historic" : [
                {
                    "date_realocate" : Date
                }
            ]
        }
    ]
}
```
## Qual a modelagem da sua coleção retirada de `projects`?

- `activities`
```js
{
    name: String,
    description: String,
    date_begin: Date,
    date_dream: Date,
    date_end: Date,
    realocate: boolean,
    expired: boolean,
    tags: [],
    members: [{
        user_id: String,
        notify: boolean
    }],
    comments: [{
        text: String,
        date_comment: Date,
        members: [{
            user_id: String,
            notify: boolean
        }],
        files: [{
            path: String,
            weight: String,
            name: String
        }]
    }],
    historic: [{
        date_realocate: Date
    }]
}
```

## Create - cadastro

**1.** Cadastre 10 usuários diferentes.

```js
db.users.insert(user)
{
    "_id" : ObjectId("56697bfaf79cf5abc20173f9"),
    "name" : "Lucas",
    "bio" : "Short bio",
    "date_register" : ISODate("2015-12-10T13:19:36.226Z"),
    "avatar_path" : "img.png",
    "background_path" : "back.png",
    "auth" : {
        "username" : "deoliveiralucas",
        "email" : "contato@deoliveiralucas.net",
        "password" : "123",
        "last_access" : ISODate("2015-12-10T13:19:36.226Z"),
        "online" : true,
        "disabled" : false,
        "hash_token" : "5r4t0er4560d4f56g04d56fg40"
    }
}

db.users.insert(user)
{
    "_id" : ObjectId("566aed8d951310bddea47c27"),
    "name" : "João",
    "bio" : "Short bio joão",
    "date_register" : ISODate("2015-12-11T15:36:35.076Z"),
    "avatar_path" : "img.png",
    "background_path" : "back.png",
    "auth" : {
        "username" : "joao",
        "email" : "contato@joao.net",
        "password" : "123",
        "last_access" : ISODate("2015-12-11T15:36:35.076Z"),
        "online" : true,
        "disabled" : false,
        "hash_token" : "5r4t0er4560d4f56g04d56fg40"
    }
}

db.users.insert(user)
{
    "_id" : ObjectId("566aedeba8367b5041ad8013"),
    "name" : "Maria",
    "bio" : "Short bio maria",
    "date_register" : ISODate("2015-12-11T15:38:08.659Z"),
    "avatar_path" : "img.png",
    "background_path" : "back.png",
    "auth" : {
        "username" : "joao",
        "email" : "contato@maria.net",
        "password" : "123",
        "last_access" : ISODate("2015-12-11T15:38:08.659Z"),
        "online" : true,
        "disabled" : false,
        "hash_token" : "5r4t0er4560d4f56g04d56fg40"
    }
}

db.users.insert(user)
{
    "_id" : ObjectId("566aedfba8367b5041ad8014"),
    "name" : "Rafael",
    "bio" : "Short bio rafael",
    "date_register" : ISODate("2015-12-11T15:38:31.946Z"),
    "avatar_path" : "img.png",
    "background_path" : "back.png",
    "auth" : {
        "username" : "joao",
        "email" : "contato@rafael.net",
        "password" : "123",
        "last_access" : ISODate("2015-12-11T15:38:31.946Z"),
        "online" : true,
        "disabled" : false,
        "hash_token" : "5r4t0er4560d4f56g04d56fg40"
    }
}

db.users.insert(user)
{
    "_id" : ObjectId("566aee0ba8367b5041ad8015"),
    "name" : "Edilson",
    "bio" : "Short bio edilson",
    "date_register" : ISODate("2015-12-11T15:38:48.346Z"),
    "avatar_path" : "img.png",
    "background_path" : "back.png",
    "auth" : {
        "username" : "joao",
        "email" : "contato@edilson.net",
        "password" : "123",
        "last_access" : ISODate("2015-12-11T15:38:48.346Z"),
        "online" : true,
        "disabled" : false,
        "hash_token" : "5r4t0er4560d4f56g04d56fg40"
    }
}

db.users.insert(user)
{
    "_id" : ObjectId("566aee19a8367b5041ad8016"),
    "name" : "Marcos",
    "bio" : "Short bio marcos",
    "date_register" : ISODate("2015-12-11T15:39:02.122Z"),
    "avatar_path" : "img.png",
    "background_path" : "back.png",
    "auth" : {
        "username" : "marcos",
        "email" : "contato@marcos.net",
        "password" : "123",
        "last_access" : ISODate("2015-12-11T15:39:02.122Z"),
        "online" : true,
        "disabled" : false,
        "hash_token" : "5r4t0er4560d4f56g04d56fg40"
    }
}

db.users.insert(user)
{
    "_id" : ObjectId("566aefb1a8367b5041ad8017"),
    "name" : "Cesar",
    "bio" : "Short bio cesar",
    "date_register" : ISODate("2015-12-11T15:45:50.728Z"),
    "avatar_path" : "img.png",
    "background_path" : "back.png",
    "auth" : {
        "username" : "marcos",
        "email" : "contato@cesar.net",
        "password" : "123",
        "last_access" : ISODate("2015-12-11T15:45:50.728Z"),
        "online" : true,
        "disabled" : false,
        "hash_token" : "5r4t0er4560d4f56g04d56fg40"
    }
}

db.users.insert(user)
{
    "_id" : ObjectId("566aefbba8367b5041ad8018"),
    "name" : "Alex",
    "bio" : "Short bio alex",
    "date_register" : ISODate("2015-12-11T15:46:01.696Z"),
    "avatar_path" : "img.png",
    "background_path" : "back.png",
    "auth" : {
        "username" : "alex",
        "email" : "contato@alex.net",
        "password" : "123",
        "last_access" : ISODate("2015-12-11T15:46:01.696Z"),
        "online" : true,
        "disabled" : false,
        "hash_token" : "5r4t0er4560d4f56g04d56fg40"
    }
}

db.users.insert(user)
{
    "_id" : ObjectId("566aefc4a8367b5041ad8019"),
    "name" : "Eder",
    "bio" : "Short bio eder",
    "date_register" : ISODate("2015-12-11T15:46:11.313Z"),
    "avatar_path" : "img.png",
    "background_path" : "back.png",
    "auth" : {
        "email" : "contato@eder.net",
        "password" : "123",
        "last_access" : ISODate("2015-12-11T15:46:11.313Z"),
        "online" : true,
        "disabled" : false,
        "hash_token" : "5r4t0er4560d4f56g04d56fg40",
        "username" : "eder"
    }
}

db.users.insert(user)
{
    "_id" : ObjectId("566aefcda8367b5041ad801a"),
    "name" : "Luiz",
    "bio" : "Short bio luiz",
    "date_register" : ISODate("2015-12-11T15:46:19.773Z"),
    "avatar_path" : "img.png",
    "background_path" : "back.png",
    "auth" : {
        "username" : "luiz",
        "email" : "contato@luiz.net",
        "password" : "123",
        "last_access" : ISODate("2015-12-11T15:46:19.773Z"),
        "online" : true,
        "disabled" : false,
        "hash_token" : "5r4t0er4560d4f56g04d56fg40"
    }
}
```

**2.** Cadastre 5 projetos diferentes.

```js
db.projects.insert(project)
{
    "_id" : ObjectId("56697eb7f79cf5abc20173fa"),
    "name" : "Project House",
    "description" : "Build a house",
    "date_begin" : ISODate("2015-12-10T13:31:21.285Z"),
    "date_dream" : ISODate("2016-11-01T02:00:00Z"),
    "date_end" : null,
    "visible" : true,
    "realocate" : null,
    "expired" : null,
    "visualizable_mod" : null,
    "members" : [
        {
            "type_member" : "owner",
            "user_id" : ObjectId("56697bfaf79cf5abc20173f9"),
            "notify" : true
        },
        {
            "type_member" : "member",
            "user_id" : ObjectId("566aefcda8367b5041ad801a"),
            "notify" : true
        },
        {
            "type_member" : "member",
            "user_id" : ObjectId("566aefc4a8367b5041ad8019"),
            "notify" : true
        },
        {
            "type_member" : "member",
            "user_id" : ObjectId("566aefbba8367b5041ad8018"),
            "notify" : true
        },
        {
            "type_member" : "member",
            "user_id" : ObjectId("566aefb1a8367b5041ad8017"),
            "notify" : true
        }
    ],
    "tags" : [
        "build",
        "house",
        "project"
    ],
    "goals" : [
        {
            "name" : "Buy tools",
            "description" : "Buy any necessary tools",
            "date_begin" : ISODate("2015-12-10T13:31:21.286Z"),
            "date_dream" : ISODate("2016-02-01T02:00:00Z"),
            "date_end" : null,
            "realocate" : null,
            "expired" : null,
            "tags" : [
                "buy",
                "tools",
                "cheap"
            ],
            "historic" : [
                {
                    "date_realocate" : ISODate("2015-12-10T13:31:21.286Z")
                }
            ],
            "activities" : [ ]
        }
    ]
}

db.projects.insert(project)
{
    "_id" : ObjectId("566b0a6ba8367b5041ad801b"),
    "name" : "Repair a car",
    "description" : "Repair the car problem",
    "date_begin" : ISODate("2016-01-05T02:00:00Z"),
    "date_dream" : ISODate("2016-03-05T03:00:00Z"),
    "date_end" : null,
    "visible" : true,
    "realocate" : null,
    "expired" : null,
    "visualizable_mod" : null,
    "members" : [
        {
            "type_member" : "owner",
            "user_id" : ObjectId("56697bfaf79cf5abc20173f9"),
            "notify" : true
        },
        {
            "type_member" : "member",
            "user_id" : ObjectId("566aed8d951310bddea47c27"),
            "notify" : false
        },
        {
            "type_member" : "member",
            "user_id" : ObjectId("566aedeba8367b5041ad8013"),
            "notify" : true
        },
        {
            "type_member" : "member",
            "user_id" : ObjectId("566aedfba8367b5041ad8014"),
            "notify" : true
        },
        {
            "type_member" : "member",
            "user_id" : ObjectId("566aee0ba8367b5041ad8015"),
            "notify" : true
        }
    ],
    "tags" : [
        "repair",
        "car",
        "project"
    ],
    "goals" : [
        {
            "name" : "Find the problem",
            "description" : "Find the problem to start fixing",
            "date_begin" : ISODate("2015-12-11T17:39:46.669Z"),
            "date_dream" : ISODate("2016-02-01T02:00:00Z"),
            "date_end" : null,
            "realocate" : null,
            "expired" : null,
            "tags" : [
                "find",
                "problem",
                "fixing"
            ],
            "activities" : [
                {
                    "activity_id" : ObjectId("56704e9b70bcacb43d0cb0ce")
                },
                {
                    "activity_id" : ObjectId("56704e9b70bcacb43d0cb0cf")
                }
            ],
            "historic" : [
                {
                    "date_realocate" : ISODate("2015-12-11T17:39:46.669Z")
                }
            ]
        }
    ]
}

db.projects.insert(project)
{
    "_id" : ObjectId("566b17fda8367b5041ad801c"),
    "name" : "Build a Website",
    "description" : "Build a personal Website",
    "date_begin" : ISODate("2015-12-11T18:37:37.366Z"),
    "date_dream" : ISODate("2016-11-01T02:00:00Z"),
    "date_end" : null,
    "visible" : true,
    "realocate" : null,
    "expired" : null,
    "visualizable_mod" : null,
    "members" : [
        {
            "type_member" : "owner",
            "user_id" : ObjectId("566aedeba8367b5041ad8013"),
            "notify" : true
        },
        {
            "type_member" : "member",
            "user_id" : ObjectId("566aedfba8367b5041ad8014"),
            "notify" : true
        },
        {
            "type_member" : "member",
            "user_id" : ObjectId("566aee0ba8367b5041ad8015"),
            "notify" : true
        },
        {
            "type_member" : "member",
            "user_id" : ObjectId("566aee19a8367b5041ad8016"),
            "notify" : true
        },
        {
            "type_member" : "member",
            "user_id" : ObjectId("566aefb1a8367b5041ad8017"),
            "notify" : true
        }
    ],
    "tags" : [
        "build",
        "website",
        "web"
    ],
    "goals" : [
        {
            "name" : "Get requisites",
            "description" : "Get requisites to start the development",
            "date_begin" : ISODate("2015-12-11T18:37:37.366Z"),
            "date_dream" : ISODate("2016-02-01T02:00:00Z"),
            "date_end" : null,
            "realocate" : null,
            "expired" : null,
            "tags" : [
                "get",
                "requisites",
                "start"
            ],
            "activities" : [
                {
                    "activity_id" : ObjectId("56704e9b70bcacb43d0cb0cc")
                },
                {
                    "activity_id" : ObjectId("56704e9b70bcacb43d0cb0c7")
                }
            ],
            "historic" : [
                {
                    "date_realocate" : ISODate("2015-12-11T18:37:37.366Z")
                }
            ]
        }
    ]
}

db.projects.insert(project)
{
    "_id" : ObjectId("566b190aa8367b5041ad801d"),
    "name" : "Travel",
    "description" : "Travel to go in the vocation",
    "date_begin" : ISODate("2015-12-11T18:42:15.701Z"),
    "date_dream" : ISODate("2016-11-01T02:00:00Z"),
    "date_end" : null,
    "visible" : true,
    "realocate" : null,
    "expired" : null,
    "visualizable_mod" : null,
    "members" : [
        {
            "type_member" : "owner",
            "user_id" : ObjectId("566aedeba8367b5041ad8013"),
            "notify" : true
        },
        {
            "type_member" : "member",
            "user_id" : ObjectId("566aedfba8367b5041ad8014"),
            "notify" : true
        },
        {
            "type_member" : "member",
            "user_id" : ObjectId("566aee0ba8367b5041ad8015"),
            "notify" : true
        },
        {
            "type_member" : "member",
            "user_id" : ObjectId("566aee19a8367b5041ad8016"),
            "notify" : true
        },
        {
            "type_member" : "member",
            "user_id" : ObjectId("566aefb1a8367b5041ad8017"),
            "notify" : true
        }
    ],
    "tags" : [
        "travel",
        "vocation",
        "abroad"
    ],
    "goals" : [
        {
            "name" : "Find the best place",
            "description" : "Find the best place to start the vacation",
            "date_begin" : ISODate("2015-12-11T18:42:15.702Z"),
            "date_dream" : ISODate("2016-02-01T02:00:00Z"),
            "date_end" : null,
            "realocate" : null,
            "expired" : null,
            "tags" : [
                "find",
                "place",
                "vacation"
            ],
            "activities" : [
                {
                    "activity_id" : ObjectId("56704e9b70bcacb43d0cb0cd")
                },
                {
                    "activity_id" : ObjectId("56704e9b70bcacb43d0cb0c8")
                }
            ],
            "historic" : [
                {
                    "date_realocate" : ISODate("2015-12-11T18:42:15.702Z")
                }
            ]
        }
    ]
}

db.projects.insert(project)
{
    "_id" : ObjectId("566b1a27a8367b5041ad801e"),
    "name" : "Final paper",
    "description" : "Do the final paper from university",
    "date_begin" : ISODate("2015-12-11T18:46:59.094Z"),
    "date_dream" : ISODate("2016-11-01T02:00:00Z"),
    "date_end" : null,
    "visible" : true,
    "realocate" : null,
    "expired" : null,
    "visualizable_mod" : null,
    "members" : [
        {
            "type_member" : "owner",
            "user_id" : ObjectId("566aedeba8367b5041ad8013"),
            "notify" : true
        },
        {
            "type_member" : "member",
            "user_id" : ObjectId("56697bfaf79cf5abc20173f9"),
            "notify" : true
        },
        {
            "type_member" : "member",
            "user_id" : ObjectId("566aefc4a8367b5041ad8019"),
            "notify" : true
        },
        {
            "type_member" : "member",
            "user_id" : ObjectId("566aee19a8367b5041ad8016"),
            "notify" : true
        },
        {
            "type_member" : "member",
            "user_id" : ObjectId("566aefcda8367b5041ad801a"),
            "notify" : true
        }
    ],
    "tags" : [
        "do",
        "final paper",
        "university",
        "project"
    ],
    "goals" : [
        {
            "name" : "Find a professor to help",
            "description" : "Find a professor to show the best way to be fallowed",
            "date_begin" : ISODate("2015-12-11T18:46:59.094Z"),
            "date_dream" : ISODate("2016-02-01T02:00:00Z"),
            "date_end" : null,
            "realocate" : null,
            "expired" : null,
            "tags" : [
                "find",
                "professor",
                "best way"
            ],
            "activities" : [
                {
                    "activity_id" : ObjectId("56704e9b70bcacb43d0cb0cb")
                },
                {
                    "activity_id" : ObjectId("56704e9b70bcacb43d0cb0c6")
                }
            ],
            "historic" : [
                {
                    "date_realocate" : ISODate("2015-12-11T18:46:59.094Z")
                }
            ]
        }
    ]
}
```

## Retrieve - busca


**1.** Liste as informações dos membros de 1 projeto específico que deve ser buscado pelo seu nome de forma a não ligar para maiúsculas e minúsculas.

```js
var project = db.projects.findOne({ name: /project house/i })
var members = [];

project.members.forEach(getMember);

getMember = function (member) {
    members.push(db.users.findOne({ _id: member.user_id }))
};

> members
[
    {
        "_id" : ObjectId("56697bfaf79cf5abc20173f9"),
        "name" : "Lucas",
        "bio" : "Short bio",
        "date_register" : ISODate("2015-12-10T13:19:36.226Z"),
        "avatar_path" : "img.png",
        "background_path" : "back.png",
        "auth" : {
            "username" : "deoliveiralucas",
            "email" : "contato@deoliveiralucas.net",
            "password" : "123",
            "last_access" : ISODate("2015-12-10T13:19:36.226Z"),
            "online" : true,
            "disabled" : false,
            "hash_token" : "5r4t0er4560d4f56g04d56fg40"
        }
    },
    {
        "_id" : ObjectId("566aefcda8367b5041ad801a"),
        "name" : "Luiz",
        "bio" : "Short bio luiz",
        "date_register" : ISODate("2015-12-11T15:46:19.773Z"),
        "avatar_path" : "img.png",
        "background_path" : "back.png",
        "auth" : {
            "username" : "luiz",
            "email" : "contato@luiz.net",
            "password" : "123",
            "last_access" : ISODate("2015-12-11T15:46:19.773Z"),
            "online" : true,
            "disabled" : false,
            "hash_token" : "5r4t0er4560d4f56g04d56fg40"
        }
    },
    {
        "_id" : ObjectId("566aefc4a8367b5041ad8019"),
        "name" : "Eder",
        "bio" : "Short bio eder",
        "date_register" : ISODate("2015-12-11T15:46:11.313Z"),
        "avatar_path" : "img.png",
        "background_path" : "back.png",
        "auth" : {
            "email" : "contato@eder.net",
            "password" : "123",
            "last_access" : ISODate("2015-12-11T15:46:11.313Z"),
            "online" : true,
            "disabled" : false,
            "hash_token" : "5r4t0er4560d4f56g04d56fg40",
            "username" : "eder"
        }
    },
    {
        "_id" : ObjectId("566aefbba8367b5041ad8018"),
        "name" : "Alex",
        "bio" : "Short bio alex",
        "date_register" : ISODate("2015-12-11T15:46:01.696Z"),
        "avatar_path" : "img.png",
        "background_path" : "back.png",
        "auth" : {
            "username" : "alex",
            "email" : "contato@alex.net",
            "password" : "123",
            "last_access" : ISODate("2015-12-11T15:46:01.696Z"),
            "online" : true,
            "disabled" : false,
            "hash_token" : "5r4t0er4560d4f56g04d56fg40"
        }
    },
    {
        "_id" : ObjectId("566aefb1a8367b5041ad8017"),
        "name" : "Cesar",
        "bio" : "Short bio cesar",
        "date_register" : ISODate("2015-12-11T15:45:50.728Z"),
        "avatar_path" : "img.png",
        "background_path" : "back.png",
        "auth" : {
            "username" : "marcos",
            "email" : "contato@cesar.net",
            "password" : "123",
            "last_access" : ISODate("2015-12-11T15:45:50.728Z"),
            "online" : true,
            "disabled" : false,
            "hash_token" : "5r4t0er4560d4f56g04d56fg40"
        }
    }
]
```

**2.** Liste todos os projetos com a tag que você escolheu para os 3 projetos em comum.

```js
> db.projects.find({ tags: { $eq: 'project' } }).pretty();
{
    "_id" : ObjectId("56697eb7f79cf5abc20173fa"),
    "name" : "Project House",
    "description" : "Build a house",
    "date_begin" : ISODate("2015-12-10T13:31:21.285Z"),
    "date_dream" : ISODate("2016-11-01T02:00:00Z"),
    "date_end" : null,
    "visible" : true,
    "realocate" : null,
    "expired" : null,
    "visualizable_mod" : null,
    "members" : [
        {
            "type_member" : "owner",
            "user_id" : ObjectId("56697bfaf79cf5abc20173f9"),
            "notify" : true
        },
        {
            "type_member" : "member",
            "user_id" : ObjectId("566aefcda8367b5041ad801a"),
            "notify" : true
        },
        {
            "type_member" : "member",
            "user_id" : ObjectId("566aefc4a8367b5041ad8019"),
            "notify" : true
        },
        {
            "type_member" : "member",
            "user_id" : ObjectId("566aefbba8367b5041ad8018"),
            "notify" : true
        },
        {
            "type_member" : "member",
            "user_id" : ObjectId("566aefb1a8367b5041ad8017"),
            "notify" : true
        }
    ],
    "tags" : [
        "build",
        "house",
        "project"
    ],
    "goals" : [
        {
            "name" : "Buy tools",
            "description" : "Buy any necessary tools",
            "date_begin" : ISODate("2015-12-10T13:31:21.286Z"),
            "date_dream" : ISODate("2016-02-01T02:00:00Z"),
            "date_end" : null,
            "realocate" : null,
            "expired" : null,
            "tags" : [
                "buy",
                "tools",
                "cheap"
            ],
            "historic" : [
                {
                    "date_realocate" : ISODate("2015-12-10T13:31:21.286Z")
                }
            ],
            "activities" : [ ]
        }
    ]
}
{
    "_id" : ObjectId("566b0a6ba8367b5041ad801b"),
    "name" : "Repair a car",
    "description" : "Repair the car problem",
    "date_begin" : ISODate("2016-01-05T02:00:00Z"),
    "date_dream" : ISODate("2016-03-05T03:00:00Z"),
    "date_end" : null,
    "visible" : true,
    "realocate" : null,
    "expired" : null,
    "visualizable_mod" : null,
    "members" : [
        {
            "type_member" : "owner",
            "user_id" : ObjectId("56697bfaf79cf5abc20173f9"),
            "notify" : true
        },
        {
            "type_member" : "member",
            "user_id" : ObjectId("566aed8d951310bddea47c27"),
            "notify" : false
        },
        {
            "type_member" : "member",
            "user_id" : ObjectId("566aedeba8367b5041ad8013"),
            "notify" : true
        },
        {
            "type_member" : "member",
            "user_id" : ObjectId("566aedfba8367b5041ad8014"),
            "notify" : true
        },
        {
            "type_member" : "member",
            "user_id" : ObjectId("566aee0ba8367b5041ad8015"),
            "notify" : true
        }
    ],
    "tags" : [
        "repair",
        "car",
        "project"
    ],
    "goals" : [
        {
            "name" : "Find the problem",
            "description" : "Find the problem to start fixing",
            "date_begin" : ISODate("2015-12-11T17:39:46.669Z"),
            "date_dream" : ISODate("2016-02-01T02:00:00Z"),
            "date_end" : null,
            "realocate" : null,
            "expired" : null,
            "tags" : [
                "find",
                "problem",
                "fixing"
            ],
            "activities" : [
                {
                    "activity_id" : ObjectId("56704e9b70bcacb43d0cb0ce")
                },
                {
                    "activity_id" : ObjectId("56704e9b70bcacb43d0cb0cf")
                }
            ],
            "historic" : [
                {
                    "date_realocate" : ISODate("2015-12-11T17:39:46.669Z")
                }
            ]
        }
    ]
}
{
    "_id" : ObjectId("566b1a27a8367b5041ad801e"),
    "name" : "Final paper",
    "description" : "Do the final paper from university",
    "date_begin" : ISODate("2015-12-11T18:46:59.094Z"),
    "date_dream" : ISODate("2016-11-01T02:00:00Z"),
    "date_end" : null,
    "visible" : true,
    "realocate" : null,
    "expired" : null,
    "visualizable_mod" : null,
    "members" : [
        {
            "type_member" : "owner",
            "user_id" : ObjectId("566aedeba8367b5041ad8013"),
            "notify" : true
        },
        {
            "type_member" : "member",
            "user_id" : ObjectId("56697bfaf79cf5abc20173f9"),
            "notify" : true
        },
        {
            "type_member" : "member",
            "user_id" : ObjectId("566aefc4a8367b5041ad8019"),
            "notify" : true
        },
        {
            "type_member" : "member",
            "user_id" : ObjectId("566aee19a8367b5041ad8016"),
            "notify" : true
        },
        {
            "type_member" : "member",
            "user_id" : ObjectId("566aefcda8367b5041ad801a"),
            "notify" : true
        }
    ],
    "tags" : [
        "do",
        "final paper",
        "university",
        "project"
    ],
    "goals" : [
        {
            "name" : "Find a professor to help",
            "description" : "Find a professor to show the best way to be fallowed",
            "date_begin" : ISODate("2015-12-11T18:46:59.094Z"),
            "date_dream" : ISODate("2016-02-01T02:00:00Z"),
            "date_end" : null,
            "realocate" : null,
            "expired" : null,
            "tags" : [
                "find",
                "professor",
                "best way"
            ],
            "activities" : [
                {
                    "activity_id" : ObjectId("56704e9b70bcacb43d0cb0cb")
                },
                {
                    "activity_id" : ObjectId("56704e9b70bcacb43d0cb0c6")
                }
            ],
            "historic" : [
                {
                    "date_realocate" : ISODate("2015-12-11T18:46:59.094Z")
                }
            ]
        }
    ]
}
```

**3.** Liste apenas os nomes de todas as atividades para todos os projetos.

```js
var activities = [];

var getActivity = function (activity) {
    var data = db.activities.findOne({ _id: activity.activity_id });
    activities.push(data.name);
};

for (var x = 0; x < 5; x++) {
    var project = db.projects.find().skip(x).limit(1).toArray();
    if (Array.isArray(project[0].goals[0].activities)) {
        project[0].goals[0].activities.forEach(getActivity);
    }
}

> activities
[
    "Activity 9",
    "Activity 10",
    "Activity 7",
    "Activity 2",
    "Activity 8",
    "Activity 3",
    "Activity 6",
    "Activity 1"
]
```

**4.** Liste todos os projetos que não possuam uma tag.

```js
> db.projects.find({ tags: { $not: { $in: ['project'] } } }, { name: 1, tags: 1 }).pretty()
{
    "_id" : ObjectId("566b17fda8367b5041ad801c"),
    "name" : "Build a Website",
    "tags" : [
        "build",
        "website",
        "web"
    ]
}
{
    "_id" : ObjectId("566b190aa8367b5041ad801d"),
    "name" : "Travel",
    "tags" : [
        "travel",
        "vocation",
        "abroad"
    ]
}
```

**5.** Liste todos os usuários que não fazem parte do primeiro projeto cadastrado.

```js
var firstProject = db.projects.findOne();
var members = [];

firstProject.members.forEach(getMember);

var getMember = function (member) {
    var data = db.users.findOne({ _id: member.user_id });
    members.push(data._id);
};

> db.users.find({ _id: { $not: { $in: members } } }, { name: 1 })
{ "_id" : ObjectId("566aed8d951310bddea47c27"), "name" : "João" }
{ "_id" : ObjectId("566aedeba8367b5041ad8013"), "name" : "Maria" }
{ "_id" : ObjectId("566aedfba8367b5041ad8014"), "name" : "Rafael" }
{ "_id" : ObjectId("566aee0ba8367b5041ad8015"), "name" : "Edilson" }
{ "_id" : ObjectId("566aee19a8367b5041ad8016"), "name" : "Marcos" }
```

## Update - alteração

**1.** Adicione para todos os projetos o campo `views: 0`.

```js
> db.projects.update({}, { $set: { views: 0 } }, { multi: 1 });
WriteResult({ "nMatched" : 5, "nUpserted" : 0, "nModified" : 5 })
> db.projects.find({}, { name: 1, views: 1 }).pretty()
{
    "_id" : ObjectId("56697eb7f79cf5abc20173fa"),
    "name" : "Project House",
    "views" : 0
}
{
    "_id" : ObjectId("566b0a6ba8367b5041ad801b"),
    "name" : "Repair a car",
    "views" : 0
}
{
    "_id" : ObjectId("566b17fda8367b5041ad801c"),
    "name" : "Build a Website",
    "views" : 0
}
{
    "_id" : ObjectId("566b190aa8367b5041ad801d"),
    "name" : "Travel",
    "views" : 0
}
{
    "_id" : ObjectId("566b1a27a8367b5041ad801e"),
    "name" : "Final paper",
    "views" : 0
}
```

**2.** Adicione 1 tag diferente para cada projeto.

```js
for (var x = 0; x < 5; x++) {
    var project = db.projects.find().skip(x).limit(1).toArray();
    db.projects.update({ _id: project[0]._id }, { $push: { tags: 'tag' + (x +40) } })
}

> db.projects.find({}, {name: 1, tags: 1}).pretty()
{
    "_id" : ObjectId("56697eb7f79cf5abc20173fa"),
    "name" : "Project House",
    "tags" : [
        "build",
        "house",
        "project",
        "tag40"
    ]
}
{
    "_id" : ObjectId("566b0a6ba8367b5041ad801b"),
    "name" : "Repair a car",
    "tags" : [
        "repair",
        "car",
        "project",
        "tag41"
    ]
}
{
    "_id" : ObjectId("566b17fda8367b5041ad801c"),
    "name" : "Build a Website",
    "tags" : [
        "build",
        "website",
        "web",
        "tag42"
    ]
}
{
    "_id" : ObjectId("566b190aa8367b5041ad801d"),
    "name" : "Travel",
    "tags" : [
        "travel",
        "vocation",
        "abroad",
        "tag43"
    ]
}
{
    "_id" : ObjectId("566b1a27a8367b5041ad801e"),
    "name" : "Final paper",
    "tags" : [
        "do",
        "final paper",
        "university",
        "project",
        "tag44"
    ]
}

```

**3.** Adicione 2 membros diferentes para cada projeto.

```js
var users = [];
users[0] = ObjectId("56697bfaf79cf5abc20173f9");
users[1] = ObjectId("566aed8d951310bddea47c27");
users[2] = ObjectId("566aedeba8367b5041ad8013");
users[3] = ObjectId("566aedfba8367b5041ad8014");
users[4] = ObjectId("566aee0ba8367b5041ad8015");
users[5] = ObjectId("566aee19a8367b5041ad8016");
users[6] = ObjectId("566aefb1a8367b5041ad8017");
users[7] = ObjectId("566aefbba8367b5041ad8018");
users[8] = ObjectId("566aefc4a8367b5041ad8019");
users[9] = ObjectId("566aefcda8367b5041ad801a");

var addMembers = function (project) {
    var member1 = {
        type_member : "member",
        user_id : users[i],
        notify : false
    };

    var member2 = {
        type_member : "member",
        user_id : users[i +5],
        notify : false
    };
    i++;

    db.projects.update({ _id: project._id }, { $push: { members: member1 } });
    db.projects.update({ _id: project._id }, { $push: { members: member2 } });
};

var i;
var projects = db.projects.find().toArray();

i = 0;
projects.forEach(addMembers);
```

**4.** Adicione 1 comentário em cada atividade, deixe apenas 1 projeto sem.

```js
var users = [];
users[0] = ObjectId("56697bfaf79cf5abc20173f9");
users[1] = ObjectId("566aed8d951310bddea47c27");
users[2] = ObjectId("566aedeba8367b5041ad8013");
users[3] = ObjectId("566aedfba8367b5041ad8014");
users[4] = ObjectId("566aee0ba8367b5041ad8015");
users[5] = ObjectId("566aee19a8367b5041ad8016");
users[6] = ObjectId("566aefb1a8367b5041ad8017");
users[7] = ObjectId("566aefbba8367b5041ad8018");
users[8] = ObjectId("566aefc4a8367b5041ad8019");
users[9] = ObjectId("566aefcda8367b5041ad801a");

var addComment = function (activity) {
    var comment = {
        "text" : "Comment " + (i +50),
        "date_comment" : new Date(),
        "members" : [
            {
                "user_id" : users[i],
                "notify" : true
            }
        ],
        "files" : [
            {
                "path" : "docs/",
                "weight" : (10 +i) + "MB",
                "name" : "doc.pdf"
            }
        ]
    };

    db.activities.update({ _id: activity._id }, { $push: { comments: comment } });
    i++;
};

var i;
var activities = db.activities.find().skip(1).toArray();

i = 0;
activities.forEach(addComment);
```

**5.** Adicione 1 projeto inteiro com **UPSERT**.

```js
var project = {
    "name" : "New project with upsert",
    "description" : "Add new project with upsert",
    "date_begin" : new Date(),
    "date_dream" : new Date(2016, 10, 1),
    "date_end" : null,
    "visible" : true,
    "realocate" : null,
    "expired" : null,
    "visualizable_mod" : null,
    "members" : [
        {
            "type_member" : "member",
            "user_id" : ObjectId("566aedfba8367b5041ad8014"),
            "notify" : true
        },
        {
            "type_member" : "member",
            "user_id" : ObjectId("566aee0ba8367b5041ad8015"),
            "notify" : true
        },
        {
            "type_member" : "member",
            "user_id" : ObjectId("566aed8d951310bddea47c27"),
            "notify" : true
        }
    ],
    "tags" : [
        "new",
        "project",
        "upsert"
    ],
    "goals" : [
        {
            "name" : "How to use",
            "description" : "Search in the mongoDb documentation how to use it",
            "date_begin" : new Date(),
            "date_dream" : new Date(2016, 1, 1),
            "date_end" : null,
            "realocate" : null,
            "expired" : null,
            "tags" : [],
            "activities" : [],
            "historic" : []
        }
    ],
    "views" : 0
};

var query  = { name: "New project with upsert" };
var option = { upsert: true };

> db.projects.update(query, project, option);
WriteResult({
    "nMatched" : 0,
    "nUpserted" : 1,
    "nModified" : 0,
    "_id" : ObjectId("5672f768196a66d02143407f")
})
```

## Delete - remoção

**1.** Apague todos os projetos que não possuam *tags*.

```js
> db.project.remove({ tags: { $eq: [ ] } }, { multi: 1 })
WriteResult({ "nRemoved" : 0 })
```

**2.** Apague todos os projetos que não possuam comentários nas atividades.

```js
var activities = db.activities.find({ comments: { $eq: [ ] } }).toArray();

var removeProject = function (activity) {
    db.projects.remove({ "goals.activities.activity_id": { $eq: activity._id } }, { multi: 1 });
};

activities.forEach(removeProject);
```

**3.** Apague todos os projetos que não possuam atividades.

```js
> db.projects.remove({ "goals.activities": { $eq: [ ] } }, { multi: 1 })
WriteResult({ "nRemoved" : 2 })
```

**4.** Escolha 2 usuário e apague todos os projetos em que os 2 fazem parte.

```js
> db.projects.remove({ "members.user_id": { $in: [ ObjectId("56697bfaf79cf5abc20173f9"), ObjectId("566aed8d951310bddea47c27") ] } }, { multi: 1 })
WriteResult({ "nRemoved" : 2 })
```

**5.** Apague todos os projetos que possuam uma determinada *tag* em goal.

```js
> db.projects.remove({ "goals.tags": { $eq: 'get' } }, { multi: 1 })
WriteResult({ "nRemoved" : 1 })
```

## Gerenciamento de usuários

**1.** Crie um usuário com permissões **APENAS** de Leitura.

```js
db.createUser(
    {
      user: "Lucas",
      pwd: "123",
      roles: [ "read" ]
    }
)
Successfully added user: { "user" : "Lucas", "roles" : [ "read" ] }
```

**2.** Crie um usuário com permissões de Escrita e Leitura.

```js
db.createUser(
    {
      user: "User2",
      pwd: "123",
      roles: [ "readWrite" ]
    }
)
Successfully added user: { "user" : "User2", "roles" : [ "readWrite" ] }
```

**3.** Adicionar o papel `grantRolesToUser` e `revokeRole` para o usuário com Escrita e Leitura.

- Criando as *roles*
```js
> db.runCommand({ createRole: "grantRolesToUser",
  privileges: [
    { resource: { db: "admin", collection: "" }, actions: [ "grantRole" ] },
  ],
  roles: [
      "readWrite"
  ]
})
{
  "ok": 1
}
```

```js
> db.runCommand({ createRole: "revokeRole",
  privileges: [
    { resource: { db: "admin", collection: "" }, actions: [ "revokeRole" ] },
  ],
  roles: [
      "readWrite"
  ]
})
{
  "ok": 1
}
```

- Concedendo as *roles*
```js
db.grantRolesToUser(
  "User2",
  [
      "grantRolesToUser",
      "revokeRole"
  ]
)
```

**4.** Remover o papel `grantRolesToUser` para o usuário com Escrita e Leitura.

```js
> db.revokeRolesFromUser(
    "User2",
    [
        "grantRolesToUser"
    ]
)
```

**5.** Listar todos os usuários com seus papéis e ações.

```js
> db.runCommand({usersInfo: 1})
{
  "users": [
    {
      "_id": "admin.User2",
      "user": "User2",
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
    },
    {
      "_id": "admin.Lucas",
      "user": "Lucas",
      "db": "admin",
      "roles": [
        {
          "role": "read",
          "db": "admin"
        }
      ]
    }
  ],
  "ok": 1
}
```

## Sharding

```js
> mkdir /data/configdb
> mkdir /data/shard1
> mkdir /data/shard2
> mkdir /data/shard3
```

- 1 Config Server
```js
> mongod --configsvr --port 27010
```

- 1 Router
```js
> mongos --configdb localhost:27010 --port 27011
```

- 3 Shardings *já com a configuração para adicionar replica*
```js
> mongod --replSet rs1 --port 27012 --dbpath /data/shard1

> mongo --port 27012
> rsconf = {
  "_id": "rs1",
  "members": [
    {
      "_id": 0,
      "host": "127.0.0.1:27012"
    }
  ]
}
> rs.initiate(rsconf)
{
  "ok": 1
}
```
```js
> mongod --replSet rs2 --port 27013 --dbpath /data/shard2

> mongo --port 27013
> rsconf = {
  "_id": "rs2",
  "members": [
    {
      "_id": 0,
      "host": "127.0.0.1:27013"
    }
  ]
}
> rs.initiate(rsconf)
{
  "ok": 1
}
```
```js
> mongod --replSet rs3 --port 27014 --dbpath /data/shard3

> mongo --port 27014
> rsconf = {
  "_id": "rs3",
  "members": [
    {
      "_id": 0,
      "host": "127.0.0.1:27014"
    }
  ]
}
> rs.initiate(rsconf)
{
  "ok": 1
}
```

```js
> mongo --port 27011 --host localhost
```

```js
mongos> sh.addShard("rs1/127.0.0.1:27012")
{
  "shardAdded": "rs1",
  "ok": 1
}
mongos> sh.addShard("rs2/127.0.0.1:27013")
{
  "shardAdded": "rs2",
  "ok": 1
}
mongos> sh.addShard("rs3/127.0.0.1:27014")
{
  "shardAdded": "rs3",
  "ok": 1
}
mongos> sh.enableSharding("be-mean-project")
{
  "ok": 1
}
```

- Coleção *shardeada* - `activities` (acredito que será a coleção que mais poderá crescer)
```js
mongos> sh.shardCollection("be-mean-project.activities", { "_id": 1 })
{
  "collectionsharded": "be-mean-project.activities",
  "ok": 1
}
```

## Replica

```js
> mkdir /data/rs1
> mkdir /data/rs2
> mkdir /data/rs3
```

```js
> mongod --replSet rs1 --port 27015 --dbpath /data/rs1
```

```js
> mongod --replSet rs2 --port 27016 --dbpath /data/rs2
```

```js
> mongod --replSet rs3 --port 27018 --dbpath /data/rs3
```

- Adicionando uma replica para cada *shard*

```js
> mongo --port 27012
> rs.add("127.0.0.1:27015")
{
  "ok": 1
}
```

```js
> mongo --port 27013
> rs.add("127.0.0.1:27016")
{
  "ok": 1
}
```

```js
> mongo --port 27014
> rs.add("127.0.0.1:27018")
{
  "ok": 1
}
```

- Status final do Cluster

```js
[mongos] be-mean-project> db.printShardingStatus()
--- Sharding Status ---
  sharding version: {
    "_id": 1,
    "minCompatibleVersion": 5,
    "currentVersion": 6,
    "clusterId": ObjectId("5675a2152f33e0ce77b80cc1")
  }
  shards:
    {  "_id": "rs1",  "host": "rs1/127.0.0.1:27012,127.0.0.1:27015" }
    {  "_id": "rs2",  "host": "rs2/127.0.0.1:27013,127.0.0.1:27016" }
    {  "_id": "rs3",  "host": "rs3/127.0.0.1:27014,127.0.0.1:27018" }
  balancer:
	Currently enabled:  yes
	Currently running:  no
	Failed balancer rounds in last 5 attempts:  0
	Migration Results for the last 24 hours:
		No recent migrations
  databases:
    {  "_id": "admin",  "partitioned": false,  "primary": "config" }
    {  "_id": "test",  "partitioned": false,  "primary": "rs1" }
    {  "_id": "be-mean-project",  "partitioned": true,  "primary": "rs3" }
    be-mean-project.activities
      shard key: { "_id": 1 }
      chunks:
        rs3: 1
        { "_id": { "$minKey" : 1 } } -> { "_id": { "$maxKey" : 1 } } on: rs3 Timestamp(1, 0)
```
