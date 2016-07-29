# MongoDb - Projeto Final
**Autor:** Patrick Adelino
**Data:** 1469032886412

## Para qual sistema você usaria o MongoDB (diferente desse)?

```
 CRM - ERP

```
## Qual a modelagem da sua coleção de `users`?

```
user: {
	name: String
	, bio : String
	, date-register: Date
	, avatar-path : String
	, background-path : String
	, auth: {
		username: String
		, email: String
		, password: String
		, last-access: Date
		, online : Boolean
		, disabled : Boolean
		, hash-token : String
	}
}
```

## Qual a modelagem da sua coleção de `projects`?

```
project: {
	name: String
	, description: String
	, date_begin: Date
	, date_dream : Date
	, date_end : Date
	, visible: Boolean
	, realocate: Boolean
	, expired: Boolean 
	, visualizable_mod: Number
	, members: [{ObjectId(user_id), type_member: String}, {ObjectId(user_id), type_member: String}]
	, tags: [ObjectId(tag_id), ObjectId(tag_id)]
	, goals : [{
		name: String
		, description: String
		, date_begin: Date
		, date_dream : Date
		, date_end : Date
		, realocate: Boolean
		, expired: Boolean
		, historics: [] 
		, tags: [ObjectId(tag_id)]
		, activities: [ObjectId(activity._id)]
	}]  
}
```

## Qual a modelagem da sua coleção retirada de `projects`?

```

activity: {
	name: String
	, description: String
	, date_begin: Date
	, date_dream : Date
	, date_end : Date
	, visible: Boolean
	, tags: [ObjectId(tag_id)]
	, realocate: Boolean
	, expired: Boolean
	, comments: [{
		text: String
		, user : ObjectId(user_id)
		, date_comment: Date
		, file: {
			path: String
			, weight: String
			, name: String
		}
	}] 
}

```
  Retirei o activity do goal por questão de crescimento do documento, pensei em criar uma coleção para o goal também mas quando não se sabe muito sobre o sistema complica um pouco as coisas, optei por deixar por achar que um project não terá uma grande quantidade de goal. Como a tags está em vários locais, eu transformei em uma coleção e coloquei apenas os Id's nos arrays, isso ajudará na alteração de nome, uma exclusão de tag e até um relátorio de onde estão sendo usadas...

## Create - cadastro

```
 1 - Cadastre 10 usuários diferentes.

 pontocom-Aspire-4252(mongod-3.2.7) be-mean-project> query
[
  {
    "name": "Patrick Adelino",
    "bio": "Jiu-jitsu <3 - 20 anos - Curitiba-PR",
    "date-register": ISODate("2016-07-20T17:51:01.745Z"),
    "avatar-path": "",
    "background-path": "",
    "auth": {
      "username": "patrickadeelino",
      "email": "patrick@gmail.com",
      "password": "Pa123#",
      "last-access": ISODate("2016-07-20T17:51:01.745Z"),
      "online": 0,
      "disabled": 0,
      "hash-token": "578faf152f3604d688dbf0cf"
    }
  },
  {
    "name": "Laura Cristina",
    "bio": "Só durmo - 16 anos - Curitiba-PR",
    "date-register": ISODate("2016-07-20T17:51:01.745Z"),
    "avatar-path": "",
    "background-path": "",
    "auth": {
      "username": "larcc",
      "email": "laura@gmail.com",
      "password": "Lau1#!",
      "last-access": ISODate("2016-07-20T17:51:01.745Z"),
      "online": 0,
      "disabled": 0,
      "hash-token": "578fb5082f3604d688dbf0d0"
    }
  },
  {
    "name": "Rosinha",
    "bio": "Arroz com ovo - 40 anos - Curitiba-PR",
    "date-register": ISODate("2016-07-20T17:51:01.745Z"),
    "avatar-path": "",
    "background-path": "",
    "auth": {
      "username": "rosinha",
      "email": "rosinha@gmail.com",
      "password": "Rosinha3#",
      "last-access": ISODate("2016-07-20T17:51:01.745Z"),
      "online": 0,
      "disabled": 0,
      "hash-token": "578fb5412f3604d688dbf0d1"
    }
  },
  {
    "name": "Cidão",
    "bio": "PS3 - 40 anos - Curitiba-PR",
    "date-register": ISODate("2016-07-20T17:51:01.745Z"),
    "avatar-path": "",
    "background-path": "",
    "auth": {
      "username": "cidao",
      "email": "cidao@gmail.com",
      "password": "cidaoDoBar1#",
      "last-access": ISODate("2016-07-20T17:51:01.745Z"),
      "online": 0,
      "disabled": 0,
      "hash-token": "578fb5422f3604d688dbf0d2"
    }
  },
  {
    "name": "Nino",
    "bio": "Calopsita - 1 ano - Curitiba-PR",
    "date-register": ISODate("2016-07-20T17:51:01.745Z"),
    "avatar-path": "",
    "background-path": "",
    "auth": {
      "username": "nino",
      "email": "nino@gmail.com",
      "password": "NinoVoador#",
      "last-access": ISODate("2016-07-20T17:51:01.745Z"),
      "online": 0,
      "disabled": 0,
      "hash-token": "578fb5b02f3604d688dbf0d3"
    }
  },
  {
    "name": "Susana",
    "bio": "Candy Crush - 49 anos - Curitiba-PR",
    "date-register": ISODate("2016-07-20T17:51:01.745Z"),
    "avatar-path": "",
    "background-path": "",
    "auth": {
      "username": "susanadocandy",
      "email": "amocandy@gmail.com",
      "password": "Baralho1#",
      "last-access": ISODate("2016-07-20T17:51:01.745Z"),
      "online": 0,
      "disabled": 0,
      "hash-token": "578fb5e92f3604d688dbf0d4"
    }
  },
  {
    "name": "Amilton",
    "bio": "Discovery - 47 anos - Curitiba-PR",
    "date-register": ISODate("2016-07-20T17:51:01.745Z"),
    "avatar-path": "",
    "background-path": "",
    "auth": {
      "username": "amiltonzin",
      "email": "todeferias@gmail.com",
      "password": "Sofa1#",
      "last-access": ISODate("2016-07-20T17:51:01.745Z"),
      "online": 0,
      "disabled": 0,
      "hash-token": "578fb6442f3604d688dbf0d5"
    }
  },
  {
    "name": "Zé",
    "bio": "é memo - 37 anos - Curitiba-PR",
    "date-register": ISODate("2016-07-20T17:51:01.745Z"),
    "avatar-path": "",
    "background-path": "",
    "auth": {
      "username": "émemoze",
      "email": "zé@gmail.com",
      "password": "Ememo#",
      "last-access": ISODate("2016-07-20T17:51:01.745Z"),
      "online": 0,
      "disabled": 0,
      "hash-token": "578fb6942f3604d688dbf0d6"
    }
  },
  {
    "name": "Geraldo",
    "bio": "Cantor - 52 anos - Curitiba-PR",
    "date-register": ISODate("2016-07-20T17:51:01.745Z"),
    "avatar-path": "",
    "background-path": "",
    "auth": {
      "username": "geraldoocantor",
      "email": "gege@gmail.com",
      "password": "CantorGe12!",
      "last-access": ISODate("2016-07-20T17:51:01.745Z"),
      "online": 0,
      "disabled": 0,
      "hash-token": "578fb6f02f3604d688dbf0d7"
    }
  },
  {
    "name": "Claudio",
    "bio": "Ando de bike - 34 anos - Curitiba-PR",
    "date-register": ISODate("2016-07-20T17:51:01.745Z"),
    "avatar-path": "",
    "background-path": "",
    "auth": {
      "username": "claudinbmx",
      "email": "claudinbmx@gmail.com",
      "password": "Claudinbmx88!",
      "last-access": ISODate("2016-07-20T17:51:01.745Z"),
      "online": 0,
      "disabled": 0,
      "hash-token": "578fb7392f3604d688dbf0d8"
    }
  }
]

pontocom-Aspire-4252(mongod-3.2.7) be-mean-project> db.users.insert(query)

Inserted 1 record(s) in 5ms
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

2- Cadastre 5 projetos diferentes.
   cada um com 5 membros, sempre diferentes dentro dos projetos;
   cada um com pelo menos 3 tags diferentes;
   escolha 1 tag onde deva ficar em 2 projetos;
   escolha 1 tag onde deva ficar em 3 projetos;
   cada projeto com pelo menos 1 goal;
   cada goal com pelo menos 3 tags;
   cada goal com pelo menos 2 atividades, deixe 1 projeto sem.

//Criando 50 tags

pontocom-Aspire-4252(mongod-3.2.7) be-mean-project> 
for (var i = 0; i <= 50; i++){
	db.tags.insert({
	"name": "Tag "+[i] 
	}) 
}

//Criando 20 atividades

pontocom-Aspire-4252(mongod-3.2.7) be-mean-project> 
for (var i = 0; i <= 20; i++){
	db.activities.insert({
	"name": "Atividade "+[i]
	, "description": "Descrição "+[i]
	, "date_begin": new Date()
	, "visible": 0
	, "realocate": 0
	, "expired": 0
	, "tags": [] 
	, "comments": [{
		"text": "Comentário "+[i],
		"date_comment": new Date(),
		"user_id": ObjectId("578fba3f19757617cf8dce5c")
	}] 
	}) 
}

pontocom-Aspire-4252(mongod-3.2.7) be-mean-project> query
[
  {
    "name": "Projeto 1",
    "description": "Este é o projeto 1",
    "date_begin": ISODate("2016-07-20T19:06:52.902Z"),
    "date_dream": null,
    "date_end": null,
    "visible": 1,
    "realocate": 0,
    "expired": 0,
    "visualizable_mod": 1,
    "members": [
      {
        "user_id": ObjectId("578fba3f19757617cf8dce5c"),
        "type_member": "Type Member 1 - Projeto 1"
      },
      {
        "user_id": ObjectId("578fba3f19757617cf8dce5d"),
        "type_member": "Type Member 2 - Projeto 1"
      },
      {
        "user_id": ObjectId("578fba3f19757617cf8dce5e"),
        "type_member": "Type Member 3 - Projeto 1"
      },
      {
        "user_id": ObjectId("578fba3f19757617cf8dce5f"),
        "type_member": "Type Member 4 - Projeto 1"
      },
      {
        "user_id": ObjectId("578fba3f19757617cf8dce60"),
        "type_member": "Type Member 5 - Projeto 1"
      }
    ],
    "tags": [
      ObjectId("578fc1af19757617cf8dce66"),
      ObjectId("578fc1b019757617cf8dce67"),
      ObjectId("578fc1b019757617cf8dce68")
    ],
    "goals": [
      {
        "name": "Goal 1 Projeto 1",
        "description": "Goal 1 Projeto 1",
        "date_begin": ISODate("2016-07-20T19:06:52.902Z"),
        "realocate": 0,
        "expired": 0,
        "historics": [ ],
        "tags": [
          ObjectId("578fc1b019757617cf8dce69"),
          ObjectId("578fc1b019757617cf8dce6a"),
          ObjectId("578fc1b019757617cf8dce6b")
        ],
        "activities": [
          ObjectId("578fc41119757617cf8dce99"),
          ObjectId("578fc41219757617cf8dce9a")
        ]
      }
    ]
  },
  {
    "name": "Projeto 2",
    "description": "Este é o projeto 2",
    "date_begin": ISODate("2016-07-20T19:06:52.902Z"),
    "date_dream": null,
    "date_end": null,
    "visible": 1,
    "realocate": 0,
    "expired": 0,
    "visualizable_mod": 1,
    "members": [
      {
        "user_id": ObjectId("578fba3f19757617cf8dce5c"),
        "type_member": "Type Member 1 - Projeto 2"
      },
      {
        "user_id": ObjectId("578fba3f19757617cf8dce5d"),
        "type_member": "Type Member 2 - Projeto 2"
      },
      {
        "user_id": ObjectId("578fba3f19757617cf8dce5e"),
        "type_member": "Type Member 3 - Projeto 2"
      },
      {
        "user_id": ObjectId("578fba3f19757617cf8dce5f"),
        "type_member": "Type Member 4 - Projeto 2"
      },
      {
        "user_id": ObjectId("578fba3f19757617cf8dce61"),
        "type_member": "Type Member 5 - Projeto 2"
      }
    ],
    "tags": [
      ObjectId("578fc1af19757617cf8dce66"),
      ObjectId("578fc1b019757617cf8dce6b"),
      ObjectId("578fc1b019757617cf8dce6e")
    ],
    "goals": [
      {
        "name": "Goal 1 Projeto 2",
        "description": "Goal 1 Projeto 2",
        "date_begin": ISODate("2016-07-20T19:06:52.902Z"),
        "realocate": 0,
        "expired": 0,
        "historics": [ ],
        "tags": [
          ObjectId("578fc1b019757617cf8dce6f"),
          ObjectId("578fc1b019757617cf8dce70"),
          ObjectId("578fc1b019757617cf8dce71")
        ],
        "activities": [
          ObjectId("578fc41219757617cf8dce9b"),
          ObjectId("578fc41219757617cf8dce9c")
        ]
      }
    ]
  },
  {
    "name": "Projeto 3",
    "description": "Este é o projeto 3",
    "date_begin": ISODate("2016-07-20T19:06:52.902Z"),
    "date_dream": null,
    "date_end": null,
    "visible": 1,
    "realocate": 0,
    "expired": 0,
    "visualizable_mod": 1,
    "members": [
      {
        "user_id": ObjectId("578fba3f19757617cf8dce61"),
        "type_member": "Type Member 1 - Projeto 3"
      },
      {
        "user_id": ObjectId("578fba3f19757617cf8dce5d"),
        "type_member": "Type Member 2 - Projeto 3"
      },
      {
        "user_id": ObjectId("578fba3f19757617cf8dce5e"),
        "type_member": "Type Member 3 - Projeto 3"
      },
      {
        "user_id": ObjectId("578fba3f19757617cf8dce62"),
        "type_member": "Type Member 4 - Projeto 3"
      },
      {
        "user_id": ObjectId("578fba3f19757617cf8dce60"),
        "type_member": "Type Member 5 - Projeto 3"
      }
    ],
    "tags": [
      ObjectId("578fc1b019757617cf8dce6f"),
      ObjectId("578fc1b019757617cf8dce70"),
      ObjectId("578fc1b019757617cf8dce6e")
    ],
    "goals": [
      {
        "name": "Goal 1 Projeto 3",
        "description": "Goal 1 Projeto 3",
        "date_begin": ISODate("2016-07-20T19:06:52.902Z"),
        "realocate": 0,
        "expired": 0,
        "historics": [ ],
        "tags": [
          ObjectId("578fc1b019757617cf8dce72"),
          ObjectId("578fc1b019757617cf8dce72"),
          ObjectId("578fc1b019757617cf8dce73")
        ],
        "activities": [
          ObjectId("578fc41219757617cf8dce9d"),
          ObjectId("578fc41219757617cf8dce9e")
        ]
      }
    ]
  },
  {
    "name": "Projeto 4",
    "description": "Este é o projeto 4",
    "date_begin": ISODate("2016-07-20T19:06:52.902Z"),
    "date_dream": null,
    "date_end": null,
    "visible": 1,
    "realocate": 0,
    "expired": 0,
    "visualizable_mod": 1,
    "members": [
      {
        "user_id": ObjectId("578fba3f19757617cf8dce61"),
        "type_member": "Type Member 1 - Projeto 4"
      },
      {
        "user_id": ObjectId("578fba3f19757617cf8dce63"),
        "type_member": "Type Member 2 - Projeto 4"
      },
      {
        "user_id": ObjectId("578fba3f19757617cf8dce5e"),
        "type_member": "Type Member 3 - Projeto 4"
      },
      {
        "user_id": ObjectId("578fba3f19757617cf8dce62"),
        "type_member": "Type Member 4 - Projeto 4"
      },
      {
        "user_id": ObjectId("578fba3f19757617cf8dce64"),
        "type_member": "Type Member 5 - Projeto 4"
      }
    ],
    "tags": [
      ObjectId("578fc1b019757617cf8dce75"),
      ObjectId("578fc1b019757617cf8dce76"),
      ObjectId("578fc1b019757617cf8dce6e")
    ],
    "goals": [
      {
        "name": "Goal 1 Projeto 4",
        "description": "Goal 1 Projeto 4",
        "date_begin": ISODate("2016-07-20T19:06:52.902Z"),
        "realocate": 0,
        "expired": 0,
        "historics": [ ],
        "tags": [
          ObjectId("578fc1b019757617cf8dce78"),
          ObjectId("578fc1b019757617cf8dce79"),
          ObjectId("578fc1b019757617cf8dce7a")
        ],
        "activities": [
          ObjectId("578fc41219757617cf8dce9f"),
          ObjectId("578fc41219757617cf8dcea0")
        ]
      }
    ]
  },
  {
    "name": "Projeto 5",
    "description": "Este é o projeto 5",
    "date_begin": ISODate("2016-07-20T19:06:52.902Z"),
    "date_dream": null,
    "date_end": null,
    "visible": 1,
    "realocate": 0,
    "expired": 0,
    "visualizable_mod": 1,
    "members": [
      {
        "user_id": ObjectId("578fba3f19757617cf8dce61"),
        "type_member": "Type Member 1 - Projeto 5"
      },
      {
        "user_id": ObjectId("578fba3f19757617cf8dce63"),
        "type_member": "Type Member 2 - Projeto 5"
      },
      {
        "user_id": ObjectId("578fba3f19757617cf8dce5e"),
        "type_member": "Type Member 3 - Projeto 5"
      },
      {
        "user_id": ObjectId("578fba3f19757617cf8dce65"),
        "type_member": "Type Member 4 - Projeto 5"
      },
      {
        "user_id": ObjectId("578fc41219757617cf8dce9f"),
        "type_member": "Type Member 5 - Projeto 5"
      }
    ],
    "tags": [
      ObjectId("578fc1b019757617cf8dce7e"),
      ObjectId("578fc1b019757617cf8dce7f"),
      ObjectId("578fc1b019757617cf8dce80")
    ],
    "goals": [
      {
        "name": "Goal 1 Projeto 5",
        "description": "Goal 1 Projeto 5",
        "date_begin": ISODate("2016-07-20T19:06:52.902Z"),
        "realocate": 0,
        "expired": 0,
        "historics": [ ],
        "tags": [
          ObjectId("578fc1b019757617cf8dce7b"),
          ObjectId("578fc1b019757617cf8dce7c"),
          ObjectId("578fc1b019757617cf8dce7d")
        ],
        "activities": [ ]
      }
    ]
  }
]


pontocom-Aspire-4252(mongod-3.2.7) be-mean-project> db.projects.insert(query)

Inserted 1 record(s) in 815ms
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
```
1 - Liste as informações dos membros de 1 projeto específico que deve ser buscado pelo seu nome de forma a não ligar para maiúsculas e minúsculas.

var pipeline = [
  {
    "$match": {
      "name": /projeto 1/i
    }
  },
  {
    "$unwind": "$members"
  },
  {
    "$lookup": {
      "from": "users",
      "localField": "members.user_id",
      "foreignField": "_id",
      "as": "members"
    }
  },
  {
    "$unwind": "$members"
  },
  {
    "$group": {
      "_id": "$_id",
      "name": {
        "$addToSet": "$name"
      },
      "members": {
        "$addToSet": {
          "member": "$members"
        }
      }
    }
  },
  {
    "$unwind": "$name"
  }
]

pontocom-Aspire-4252(mongod-3.2.7) be-mean-project> db.projects.aggregate(pipeline)

- RETORNO - 
{
  "waitedMS": NumberLong("0"),
  "result": [
    {
      "_id": ObjectId("578fcbdea7842bdb5bff7a60"),
      "name": "Projeto 1",
      "members": [
        {
          "member": {
            "_id": ObjectId("578fba3f19757617cf8dce60"),
            "name": "Nino",
            "bio": "Calopsita - 1 ano - Curitiba-PR",
            "date-register": ISODate("2016-07-20T17:51:01.745Z"),
            "avatar-path": "",
            "background-path": "",
            "auth": {
              "username": "nino",
              "email": "nino@gmail.com",
              "password": "NinoVoador#",
              "last-access": ISODate("2016-07-20T17:51:01.745Z"),
              "online": 0,
              "disabled": 0,
              "hash-token": "578fb5b02f3604d688dbf0d3"
            }
          }
        },
        {
          "member": {
            "_id": ObjectId("578fba3f19757617cf8dce5f"),
            "name": "Cidão",
            "bio": "PS3 - 40 anos - Curitiba-PR",
            "date-register": ISODate("2016-07-20T17:51:01.745Z"),
            "avatar-path": "",
            "background-path": "",
            "auth": {
              "username": "cidao",
              "email": "cidao@gmail.com",
              "password": "cidaoDoBar1#",
              "last-access": ISODate("2016-07-20T17:51:01.745Z"),
              "online": 0,
              "disabled": 0,
              "hash-token": "578fb5422f3604d688dbf0d2"
            }
          }
        },
        {
          "member": {
            "_id": ObjectId("578fba3f19757617cf8dce5e"),
            "name": "Rosinha",
            "bio": "Arroz com ovo - 40 anos - Curitiba-PR",
            "date-register": ISODate("2016-07-20T17:51:01.745Z"),
            "avatar-path": "",
            "background-path": "",
            "auth": {
              "username": "rosinha",
              "email": "rosinha@gmail.com",
              "password": "Rosinha3#",
              "last-access": ISODate("2016-07-20T17:51:01.745Z"),
              "online": 0,
              "disabled": 0,
              "hash-token": "578fb5412f3604d688dbf0d1"
            }
          }
        },
        {
          "member": {
            "_id": ObjectId("578fba3f19757617cf8dce5d"),
            "name": "Laura Cristina",
            "bio": "Só durmo - 16 anos - Curitiba-PR",
            "date-register": ISODate("2016-07-20T17:51:01.745Z"),
            "avatar-path": "",
            "background-path": "",
            "auth": {
              "username": "larcc",
              "email": "laura@gmail.com",
              "password": "Lau1#!",
              "last-access": ISODate("2016-07-20T17:51:01.745Z"),
              "online": 0,
              "disabled": 0,
              "hash-token": "578fb5082f3604d688dbf0d0"
            }
          }
        },
        {
          "member": {
            "_id": ObjectId("578fba3f19757617cf8dce5c"),
            "name": "Patrick Adelino",
            "bio": "Jiu-jitsu <3 - 20 anos - Curitiba-PR",
            "date-register": ISODate("2016-07-20T17:51:01.745Z"),
            "avatar-path": "",
            "background-path": "",
            "auth": {
              "username": "patrickadeelino",
              "email": "patrick@gmail.com",
              "password": "Pa123#",
              "last-access": ISODate("2016-07-20T17:51:01.745Z"),
              "online": 0,
              "disabled": 0,
              "hash-token": "578faf152f3604d688dbf0cf"
            }
          }
        }
      ]
    }
  ],
  "ok": 1
}

2- Liste todos os projetos com a tag que você escolheu para os 3 projetos em comum.

	var query = { "tags": { $in: [ObjectId("578fc1b019757617cf8dce6e")] } }

pontocom-Aspire-4252(mongod-3.2.7) be-mean-project> db.projects.find(query)

{
  "_id": ObjectId("578fcbdea7842bdb5bff7a61"),
  "name": "Projeto 2",
  "description": "Este é o projeto 2",
  "date_begin": ISODate("2016-07-20T19:06:52.902Z"),
  "date_dream": null,
  "date_end": null,
  "visible": 1,
  "realocate": 0,
  "expired": 0,
  "visualizable_mod": 1,
  "members": [
    {
      "user_id": ObjectId("578fba3f19757617cf8dce5c"),
      "type_member": "Type Member 1 - Projeto 2"
    },
    {
      "user_id": ObjectId("578fba3f19757617cf8dce5d"),
      "type_member": "Type Member 2 - Projeto 2"
    },
    {
      "user_id": ObjectId("578fba3f19757617cf8dce5e"),
      "type_member": "Type Member 3 - Projeto 2"
    },
    {
      "user_id": ObjectId("578fba3f19757617cf8dce5f"),
      "type_member": "Type Member 4 - Projeto 2"
    },
    {
      "user_id": ObjectId("578fba3f19757617cf8dce61"),
      "type_member": "Type Member 5 - Projeto 2"
    }
  ],
  "tags": [
    ObjectId("578fc1af19757617cf8dce66"),
    ObjectId("578fc1b019757617cf8dce6b"),
    ObjectId("578fc1b019757617cf8dce6e")
  ],
  "goals": [
    {
      "name": "Goal 1 Projeto 2",
      "description": "Goal 1 Projeto 2",
      "date_begin": ISODate("2016-07-20T19:06:52.902Z"),
      "realocate": 0,
      "expired": 0,
      "historics": [ ],
      "tags": [
        ObjectId("578fc1b019757617cf8dce6f"),
        ObjectId("578fc1b019757617cf8dce70"),
        ObjectId("578fc1b019757617cf8dce71")
      ],
      "activities": [
        ObjectId("578fc41219757617cf8dce9b"),
        ObjectId("578fc41219757617cf8dce9c")
      ]
    }
  ]
}
{
  "_id": ObjectId("578fcbdea7842bdb5bff7a62"),
  "name": "Projeto 3",
  "description": "Este é o projeto 3",
  "date_begin": ISODate("2016-07-20T19:06:52.902Z"),
  "date_dream": null,
  "date_end": null,
  "visible": 1,
  "realocate": 0,
  "expired": 0,
  "visualizable_mod": 1,
  "members": [
    {
      "user_id": ObjectId("578fba3f19757617cf8dce61"),
      "type_member": "Type Member 1 - Projeto 3"
    },
    {
      "user_id": ObjectId("578fba3f19757617cf8dce5d"),
      "type_member": "Type Member 2 - Projeto 3"
    },
    {
      "user_id": ObjectId("578fba3f19757617cf8dce5e"),
      "type_member": "Type Member 3 - Projeto 3"
    },
    {
      "user_id": ObjectId("578fba3f19757617cf8dce62"),
      "type_member": "Type Member 4 - Projeto 3"
    },
    {
      "user_id": ObjectId("578fba3f19757617cf8dce60"),
      "type_member": "Type Member 5 - Projeto 3"
    }
  ],
  "tags": [
    ObjectId("578fc1b019757617cf8dce6f"),
    ObjectId("578fc1b019757617cf8dce70"),
    ObjectId("578fc1b019757617cf8dce6e")
  ],
  "goals": [
    {
      "name": "Goal 1 Projeto 3",
      "description": "Goal 1 Projeto 3",
      "date_begin": ISODate("2016-07-20T19:06:52.902Z"),
      "realocate": 0,
      "expired": 0,
      "historics": [ ],
      "tags": [
        ObjectId("578fc1b019757617cf8dce72"),
        ObjectId("578fc1b019757617cf8dce72"),
        ObjectId("578fc1b019757617cf8dce73")
      ],
      "activities": [
        ObjectId("578fc41219757617cf8dce9d"),
        ObjectId("578fc41219757617cf8dce9e")
      ]
    }
  ]
}
{
  "_id": ObjectId("578fcbdea7842bdb5bff7a63"),
  "name": "Projeto 4",
  "description": "Este é o projeto 4",
  "date_begin": ISODate("2016-07-20T19:06:52.902Z"),
  "date_dream": null,
  "date_end": null,
  "visible": 1,
  "realocate": 0,
  "expired": 0,
  "visualizable_mod": 1,
  "members": [
    {
      "user_id": ObjectId("578fba3f19757617cf8dce61"),
      "type_member": "Type Member 1 - Projeto 4"
    },
    {
      "user_id": ObjectId("578fba3f19757617cf8dce63"),
      "type_member": "Type Member 2 - Projeto 4"
    },
    {
      "user_id": ObjectId("578fba3f19757617cf8dce5e"),
      "type_member": "Type Member 3 - Projeto 4"
    },
    {
      "user_id": ObjectId("578fba3f19757617cf8dce62"),
      "type_member": "Type Member 4 - Projeto 4"
    },
    {
      "user_id": ObjectId("578fba3f19757617cf8dce64"),
      "type_member": "Type Member 5 - Projeto 4"
    }
  ],
  "tags": [
    ObjectId("578fc1b019757617cf8dce75"),
    ObjectId("578fc1b019757617cf8dce76"),
    ObjectId("578fc1b019757617cf8dce6e")
  ],
  "goals": [
    {
      "name": "Goal 1 Projeto 4",
      "description": "Goal 1 Projeto 4",
      "date_begin": ISODate("2016-07-20T19:06:52.902Z"),
      "realocate": 0,
      "expired": 0,
      "historics": [ ],
      "tags": [
        ObjectId("578fc1b019757617cf8dce78"),
        ObjectId("578fc1b019757617cf8dce79"),
        ObjectId("578fc1b019757617cf8dce7a")
      ],
      "activities": [
        ObjectId("578fc41219757617cf8dce9f"),
        ObjectId("578fc41219757617cf8dcea0")
      ]
    }
  ]
}
Fetched 3 record(s) in 99m


var pipeline = [
  {
    "$match": {
      "goals": {
        "$ne": null
      }
    }
  },
  {
    "$unwind": "$goals"
  },
  {
    "$unwind": "$goals.activities"
  },
  {
    "$lookup": {
      "from": "activities",
      "localField": "goals.activities",
      "foreignField": "_id",
      "as": "activities"
    }
  },
  {
    "$unwind": "$activities"
  },
  {
    "$group": {
      "_id": "$_id",
      "name": {
        "$addToSet": "$name"
      },
      "activities": {
        "$addToSet": "$activities"
      }
    }
  },
  {
    "$unwind": "$name"
  },
  {
    "$project": {
      "name": 1,
      "activities.name": 1
    }
  }
]

pontocom-Aspire-4252(mongod-3.2.7) be-mean-project> db.projects.aggregate(pipeline)

{
  "waitedMS": NumberLong("0"),
  "result": [
    {
      "_id": ObjectId("578fcbdea7842bdb5bff7a63"),
      "name": "Projeto 4",
      "activities": [
        {
          "name": "Atividade 7"
        },
        {
          "name": "Atividade 6"
        }
      ]
    },
    {
      "_id": ObjectId("578fcbdea7842bdb5bff7a62"),
      "name": "Projeto 3",
      "activities": [
        {
          "name": "Atividade 5"
        },
        {
          "name": "Atividade 4"
        }
      ]
    },
    {
      "_id": ObjectId("578fcbdea7842bdb5bff7a61"),
      "name": "Projeto 2",
      "activities": [
        {
          "name": "Atividade 3"
        },
        {
          "name": "Atividade 2"
        }
      ]
    },
    {
      "_id": ObjectId("578fcbdea7842bdb5bff7a60"),
      "name": "Projeto 1",
      "activities": [
        {
          "name": "Atividade 1"
        },
        {
          "name": "Atividade 0"
        }
      ]
    }
  ],
  "ok": 1
}

4- Liste todos os projetos que não possuam uma tag.

pontocom-Aspire-4252(mongod-3.2.7) be-mean-project> var query = {'tags': null}

pontocom-Aspire-4252(mongod-3.2.7) be-mean-project> db.projects.find(query)
Fetched 0 record(s) in 1ms

5- Liste todos os usuários que não fazem parte do primeiro projeto cadastrado.

var query = {name: "Projeto 1"}
var options = {members: 1, _id: 0}

pontocom-Aspire-4252(mongod-3.2.7) be-mean-project> var membersFirstProject = db.projects.findOne(query, options).members
pontocom-Aspire-4252(mongod-3.2.7) be-mean-project> var arrayUsersId = []
pontocom-Aspire-4252(mongod-3.2.7) be-mean-project> for (var k in membersFirstProject) { arrayUsersId.push( membersFirstProject[k].user_id) }

pontocom-Aspire-4252(mongod-3.2.7) be-mean-project> arrayUsersId
[
  ObjectId("578fba3f19757617cf8dce5c"),
  ObjectId("578fba3f19757617cf8dce5d"),
  ObjectId("578fba3f19757617cf8dce5e"),
  ObjectId("578fba3f19757617cf8dce5f"),
  ObjectId("578fba3f19757617cf8dce60")
]

pontocom-Aspire-4252(mongod-3.2.7) be-mean-project> db.users.find({ _id: {$nin: arr} } )
{
  "_id": ObjectId("578fba3f19757617cf8dce61"),
  "name": "Susana",
  "bio": "Candy Crush - 49 anos - Curitiba-PR",
  "date-register": ISODate("2016-07-20T17:51:01.745Z"),
  "avatar-path": "",
  "background-path": "",
  "auth": {
    "username": "susanadocandy",
    "email": "amocandy@gmail.com",
    "password": "Baralho1#",
    "last-access": ISODate("2016-07-20T17:51:01.745Z"),
    "online": 0,
    "disabled": 0,
    "hash-token": "578fb5e92f3604d688dbf0d4"
  }
}
{
  "_id": ObjectId("578fba3f19757617cf8dce62"),
  "name": "Amilton",
  "bio": "Discovery - 47 anos - Curitiba-PR",
  "date-register": ISODate("2016-07-20T17:51:01.745Z"),
  "avatar-path": "",
  "background-path": "",
  "auth": {
    "username": "amiltonzin",
    "email": "todeferias@gmail.com",
    "password": "Sofa1#",
    "last-access": ISODate("2016-07-20T17:51:01.745Z"),
    "online": 0,
    "disabled": 0,
    "hash-token": "578fb6442f3604d688dbf0d5"
  }
}
{
  "_id": ObjectId("578fba3f19757617cf8dce63"),
  "name": "Zé",
  "bio": "é memo - 37 anos - Curitiba-PR",
  "date-register": ISODate("2016-07-20T17:51:01.745Z"),
  "avatar-path": "",
  "background-path": "",
  "auth": {
    "username": "émemoze",
    "email": "zé@gmail.com",
    "password": "Ememo#",
    "last-access": ISODate("2016-07-20T17:51:01.745Z"),
    "online": 0,
    "disabled": 0,
    "hash-token": "578fb6942f3604d688dbf0d6"
  }
}
{
  "_id": ObjectId("578fba3f19757617cf8dce64"),
  "name": "Geraldo",
  "bio": "Cantor - 52 anos - Curitiba-PR",
  "date-register": ISODate("2016-07-20T17:51:01.745Z"),
  "avatar-path": "",
  "background-path": "",
  "auth": {
    "username": "geraldoocantor",
    "email": "gege@gmail.com",
    "password": "CantorGe12!",
    "last-access": ISODate("2016-07-20T17:51:01.745Z"),
    "online": 0,
    "disabled": 0,
    "hash-token": "578fb6f02f3604d688dbf0d7"
  }
}
{
  "_id": ObjectId("578fba3f19757617cf8dce65"),
  "name": "Claudio",
  "bio": "Ando de bike - 34 anos - Curitiba-PR",
  "date-register": ISODate("2016-07-20T17:51:01.745Z"),
  "avatar-path": "",
  "background-path": "",
  "auth": {
    "username": "claudinbmx",
    "email": "claudinbmx@gmail.com",
    "password": "Claudinbmx88!",
    "last-access": ISODate("2016-07-20T17:51:01.745Z"),
    "online": 0,
    "disabled": 0,
    "hash-token": "578fb7392f3604d688dbf0d8"
  }
}
Fetched 5 record(s) in 622ms

```

## Update - alteração

1- Adicone para todos os projetos o campo view: 0

```
	pontocom-Aspire-4252(mongod-3.2.7) be-mean-project> var query = {}
	pontocom-Aspire-4252(mongod-3.2.7) be-mean-project> var mod = {$set: { views: 0} }
	pontocom-Aspire-4252(mongod-3.2.7) be-mean-project> var options = {multi: true}
	pontocom-Aspire-4252(mongod-3.2.7) be-mean-project> db.projects.update(query, mod, options)
	Updated 5 existing record(s) in 244ms
	WriteResult({
	  "nMatched": 5,
	  "nUpserted": 0,
	  "nModified": 5
	})

2- Adicione uma tag diferente para cada projeto

pontocom-Aspire-4252(mongod-3.2.7) be-mean-project> var query = {name: "Projeto 1" }
pontocom-Aspire-4252(mongod-3.2.7) be-mean-project> var mod = { $push : { tags: ObjectId("578fc1b019757617cf8dce96")} }

pontocom-Aspire-4252(mongod-3.2.7) be-mean-project> db.projects.update(query, mod)
Updated 1 existing record(s) in 3678ms
WriteResult({
  "nMatched": 1,
  "nUpserted": 0,
  "nModified": 1
})

pontocom-Aspire-4252(mongod-3.2.7) be-mean-project> var query = {name: "Projeto 2" }
pontocom-Aspire-4252(mongod-3.2.7) be-mean-project> var mod = { $push : { tags: ObjectId("578fc1b019757617cf8dce97") }  }

pontocom-Aspire-4252(mongod-3.2.7) be-mean-project> db.projects.update(query, mod)
Updated 1 existing record(s) in 77ms
WriteResult({
  "nMatched": 1,
  "nUpserted": 0,
  "nModified": 1
})

pontocom-Aspire-4252(mongod-3.2.7) be-mean-project> var query = {name: "Projeto 3" }
pontocom-Aspire-4252(mongod-3.2.7) be-mean-project> var mod = { $push : { tags: ObjectId("578fc1b019757617cf8dce95")} }
pontocom-Aspire-4252(mongod-3.2.7) be-mean-project> db.projects.update(query, mod)
Updated 1 existing record(s) in 4ms
WriteResult({
  "nMatched": 1,
  "nUpserted": 0,
  "nModified": 1
})

pontocom-Aspire-4252(mongod-3.2.7) be-mean-project> var query = {name: "Projeto 4" }
pontocom-Aspire-4252(mongod-3.2.7) be-mean-project> var mod = { $push : { tags: ObjectId("578fc1b019757617cf8dce94")} }
pontocom-Aspire-4252(mongod-3.2.7) be-mean-project> db.projects.update(query, mod)
Updated 1 existing record(s) in 1ms
WriteResult({
  "nMatched": 1,
  "nUpserted": 0,
  "nModified": 1
})

pontocom-Aspire-4252(mongod-3.2.7) be-mean-project> var query = {name: "Projeto 5" }
pontocom-Aspire-4252(mongod-3.2.7) be-mean-project> var mod = { $push : { tags: ObjectId("578fc1b019757617cf8dce93")} }
pontocom-Aspire-4252(mongod-3.2.7) be-mean-project> db.projects.update(query, mod)
Updated 1 existing record(s) in 3ms
WriteResult({
  "nMatched": 1,
  "nUpserted": 0,
  "nModified": 1
})


3- Adicone 2 membros diferentes para cada projeto

var query = {
  "name": "Projeto 1"
}

var mod = {
  "$pushAll": {
    "members": [
      {
        "user_id": ObjectId("578fba3f19757617cf8dce65"),
        "type_member": "Type Member 6 - Projeto 1"
      },
      {
        "user_id": ObjectId("578fba3f19757617cf8dce64"),
        "type_member": "Type Member 7 - Projeto 1"
      }
    ]
  }
}

pontocom-Aspire-4252(mongod-3.2.7) be-mean-project> db.projects.update(query, mod)
Updated 1 existing record(s) in 37ms
WriteResult({
  "nMatched": 1,
  "nUpserted": 0,
  "nModified": 1
})


var query = {name: "Projeto 2"}

var mod = {
  "$pushAll": {
    "members": [
      {
        "user_id": ObjectId("578fba3f19757617cf8dce60"),
        "type_member": "Type Member 6 - Projeto 2"
      },
      {
        "user_id": ObjectId("578fba3f19757617cf8dce62"),
        "type_member": "Type Member 7 - Projeto 2"
      }
    ]
  }
}

pontocom-Aspire-4252(mongod-3.2.7) be-mean-project> db.projects.update(query, mod)
Updated 1 existing record(s) in 4ms
WriteResult({
  "nMatched": 1,
  "nUpserted": 0,
  "nModified": 1
})

var query = {name: "Projeto 3"}

var mod = {
  "$pushAll": {
    "members": [
      {
        "user_id": ObjectId("578fba3f19757617cf8dce64"),
        "type_member": "Type Member 6 - Projeto 3"
      },
      {
        "user_id": ObjectId("578fba3f19757617cf8dce65"),
        "type_member": "Type Member 7 - Projeto 3"
      }
    ]
  }
}

pontocom-Aspire-4252(mongod-3.2.7) be-mean-project> db.projects.update(query, mod)
Updated 1 existing record(s) in 9ms
WriteResult({
  "nMatched": 1,
  "nUpserted": 0,
  "nModified": 1
})


var query = {name: "Projeto 4"}

var mod = {
  "$pushAll": {
    "members": [
      {
        "user_id": ObjectId("578fba3f19757617cf8dce60"),
        "type_member": "Type Member 6 - Projeto 4"
      },
      {
        "user_id": ObjectId("578fba3f19757617cf8dce65"),
        "type_member": "Type Member 7 - Projeto 4"
      }
    ]
  }
}

pontocom-Aspire-4252(mongod-3.2.7) be-mean-project> db.projects.update(query, mod)
Updated 1 existing record(s) in 2ms
WriteResult({
  "nMatched": 1,
  "nUpserted": 0,
  "nModified": 1
})

var query = {name: "Projeto 5"}

var mod = {
  "$pushAll": {
    "members": [
      {
        "user_id": ObjectId("578fba3f19757617cf8dce60"),
        "type_member": "Type Member 6 - Projeto 5"
      },
      {
        "user_id": ObjectId("578fba3f19757617cf8dce62"),
        "type_member": "Type Member 7 - Projeto 5"
      }
    ]
  }
}

pontocom-Aspire-4252(mongod-3.2.7) be-mean-project> db.projects.update(query, mod)
Updated 1 existing record(s) in 4ms
WriteResult({
  "nMatched": 1,
  "nUpserted": 0,
  "nModified": 1
})

4- Adicione 1 comentário em cada atividade, deixe apenas 1 projeto sem.

pontocom-Aspire-4252(mongod-3.2.7) be-mean-project> query
{
  "name": "Projeto 1"
}
pontocom-Aspire-4252(mongod-3.2.7) be-mean-project> var goals = db.projects.findOne(query).goals
pontocom-Aspire-4252(mongod-3.2.7) be-mean-project> goals

pontocom-Aspire-4252(mongod-3.2.7) be-mean-project> var activities = [];
pontocom-Aspire-4252(mongod-3.2.7) be-mean-project> for (var k in goals){ activities.push(goals[k].activities)}

pontocom-Aspire-4252(mongod-3.2.7) be-mean-project> query
{
  "name": "Projeto 2"
}
pontocom-Aspire-4252(mongod-3.2.7) be-mean-project> var goals = db.projects.findOne(query).goals
pontocom-Aspire-4252(mongod-3.2.7) be-mean-project> goals

pontocom-Aspire-4252(mongod-3.2.7) be-mean-project> var activities = [];
pontocom-Aspire-4252(mongod-3.2.7) be-mean-project> for (var k in goals){ activities.push(goals[k].activities)}

pontocom-Aspire-4252(mongod-3.2.7) be-mean-project> query
{
  "name": "Projeto 3"
}
pontocom-Aspire-4252(mongod-3.2.7) be-mean-project> var goals = db.projects.findOne(query).goals
pontocom-Aspire-4252(mongod-3.2.7) be-mean-project> goals

pontocom-Aspire-4252(mongod-3.2.7) be-mean-project> var activities = [];
pontocom-Aspire-4252(mongod-3.2.7) be-mean-project> for (var k in goals){ activities.push(goals[k].activities)}


pontocom-Aspire-4252(mongod-3.2.7) be-mean-project> query
{
  "name": "Projeto 4"
}
pontocom-Aspire-4252(mongod-3.2.7) be-mean-project> var goals = db.projects.findOne(query).goals
pontocom-Aspire-4252(mongod-3.2.7) be-mean-project> goals

pontocom-Aspire-4252(mongod-3.2.7) be-mean-project> var activities = [];
pontocom-Aspire-4252(mongod-3.2.7) be-mean-project> for (var k in goals){ activities.push(goals[k].activities)}

// Todas atividades que estão nos projetos, tirando um projeto conforme pedido.
[
  [
    ObjectId("578fc41119757617cf8dce99"),
    ObjectId("578fc41219757617cf8dce9a")
  ],
  [
    ObjectId("578fc41219757617cf8dce9b"),
    ObjectId("578fc41219757617cf8dce9c")
  ],
  [
    ObjectId("578fc41219757617cf8dce9d"),
    ObjectId("578fc41219757617cf8dce9e")
  ],
  [
    ObjectId("578fc41219757617cf8dce9f"),
    ObjectId("578fc41219757617cf8dcea0")
  ]
]

// Removendo os arrays para fazer a query

pontocom-Aspire-4252(mongod-3.2.7) be-mean-project> var newActivities = []
pontocom-Aspire-4252(mongod-3.2.7) be-mean-project> for (var k in activities) { for (var key in activities[k]) { newActivities.push(activities[k][key])} }
8
pontocom-Aspire-4252(mongod-3.2.7) be-mean-project> newActivities
[
  ObjectId("578fc41119757617cf8dce99"),
  ObjectId("578fc41219757617cf8dce9a"),
  ObjectId("578fc41219757617cf8dce9b"),
  ObjectId("578fc41219757617cf8dce9c"),
  ObjectId("578fc41219757617cf8dce9d"),
  ObjectId("578fc41219757617cf8dce9e"),
  ObjectId("578fc41219757617cf8dce9f"),
  ObjectId("578fc41219757617cf8dcea0")
]

// Encontrando e adicionando um comentário teste em cada

pontocom-Aspire-4252(mongod-3.2.7) be-mean-project> query
{
  "_id": {
    "$in": [
      ObjectId("578fc41119757617cf8dce99"),
      ObjectId("578fc41219757617cf8dce9a"),
      ObjectId("578fc41219757617cf8dce9b"),
      ObjectId("578fc41219757617cf8dce9c"),
      ObjectId("578fc41219757617cf8dce9d"),
      ObjectId("578fc41219757617cf8dce9e"),
      ObjectId("578fc41219757617cf8dce9f"),
      ObjectId("578fc41219757617cf8dcea0")
    ]
  }
}

pontocom-Aspire-4252(mongod-3.2.7) be-mean-project> mod
{
  "$push": {
    "comments": {
      "text": "Comentário Teste",
      "user": ObjectId("578fba3f19757617cf8dce61"),
      "date_comment": ISODate("2016-07-23T02:37:23.155Z")
    }
  }
}

pontocom-Aspire-4252(mongod-3.2.7) be-mean-project> var options = {multi: true}

pontocom-Aspire-4252(mongod-3.2.7) be-mean-project> db.activities.update(query,mod,options)

Updated 8 existing record(s) in 45ms
WriteResult({
  "nMatched": 8,
  "nUpserted": 0,
  "nModified": 8
})

5- Adicione 1 projeto inteiro com UPSERT.

var query = {name: "Projeto 6"}
var mod = {
  "$set": {
    "views": 0
  },
  "$setOnInsert": {
    "name": "Projeto 6",
    "description": "Este é o projeto 6",
    "date_begin": ISODate("2016-07-23T02:46:37.273Z"),
    "date_dream": null,
    "date_end": null,
    "visible": 1,
    "realocate": 0,
    "expired": 0,
    "visualizable_mod": 1,
    "members": [
      {
        "user_id": ObjectId("578fba3f19757617cf8dce65"),
        "type_member": "Type Member 1 - Projeto 6"
      },
      {
        "user_id": ObjectId("578fba3f19757617cf8dce63"),
        "type_member": "Type Member 2 - Projeto 6"
      },
      {
        "user_id": ObjectId("578fba3f19757617cf8dce5e"),
        "type_member": "Type Member 3 - Projeto 6"
      },
      {
        "user_id": ObjectId("578fba3f19757617cf8dce5f"),
        "type_member": "Type Member 4 - Projeto 6"
      },
      {
        "user_id": ObjectId("578fba3f19757617cf8dce60"),
        "type_member": "Type Member 5 - Projeto 6"
      }
    ],
    "tags": [
      ObjectId("578fc1b019757617cf8dce8a"),
      ObjectId("578fc1b019757617cf8dce8b"),
      ObjectId("578fc1b019757617cf8dce8c")
    ],
    "goals": [
      {
        "name": "Goal 1 Projeto 6",
        "description": "Goal 1 Projeto 6",
        "date_begin": ISODate("2016-07-23T02:46:37.273Z"),
        "realocate": 0,
        "expired": 0,
        "historics": [ ],
        "tags": [
          ObjectId("578fc1b019757617cf8dce8d"),
          ObjectId("578fc1b019757617cf8dce8f"),
          ObjectId("578fc1b019757617cf8dce91")
        ],
        "activities": [ ]
      }
    ]
  }
}

var options = {upsert: 1}

```
## Delete - remoção
```
1 - Apague todos os projetos que não possuam tags.

pontocom-Aspire-4252(mongod-3.2.7) be-mean-project> query = {tags: null}
{
  "tags": null
}
pontocom-Aspire-4252(mongod-3.2.7) be-mean-project> db.projects.remove(query)
Removed 0 record(s) in 201ms
WriteResult({
  "nRemoved": 0
})

2 - Apague todos os projetos que não possuam comentários nas atividades.
 var pipeline = [
  {
    "$unwind": "$goals"
  },
  {
    "$unwind": {
      "path": "$goals.activities",
      "preserveNullAndEmptyArrays": true
    }
  },
  {
    "$lookup": {
      "from": "activities",
      "localField": "goals.activities",
      "foreignField": "_id",
      "as": "goals.activities"
    }
  },
  {
    "$unwind": {
      "path": "$goals.activities",
      "preserveNullAndEmptyArrays": true
    }
  },
  {
    "$match": {
      "goals.activities.comments": {
        "$eq": null
      }
    }
  },
  {
    "$group": {
      "_id": "$_id"
    }
  }
]

var projects = db.projects.aggregate(pipeline)

{
  "waitedMS": NumberLong("0"),
  "result": [
    {
      "_id": ObjectId("5792dafb2b7d5e764c2bcaa0")
    },
    {
      "_id": ObjectId("578fcbdea7842bdb5bff7a64")
    }
  ],
  "ok": 1
}

pontocom-Aspire-4252(mongod-3.2.7) be-mean-project> projects.result.forEach(function(project){ db.projects.remove(project)})

Removed 1 record(s) in 288ms
Removed 1 record(s) in 145ms

pontocom-Aspire-4252(mongod-3.2.7) be-mean-project> db.projects.aggregate(pipeline)
{
  "waitedMS": NumberLong("0"),
  "result": [ ],
  "ok": 1
}

3 - Apague todos os projetos que não possuam atividades.

var query = {
  "$or": [
    {
      "goals.activities": {
        "$size": 0
      }
    },
    {
      "goals.activities": null
    }
  ]
}

db.projects.remove(query)

pontocom-Aspire-4252(mongod-3.2.7) be-mean-project> db.projects.remove(query)
Removed 0 record(s) in 201ms
WriteResult({
  "nRemoved": 0
})


4 - Escolha 2 usuário e apague todos os projetos em que os 2 fazem parte.

pontocom-Aspire-4252(mongod-3.2.7) be-mean-project> var users = db.users.find({}, {_id: 1}).limit(2).skip(_rand() * db.users.count()).toArray()
pontocom-Aspire-4252(mongod-3.2.7) be-mean-project> users = users.map(function(user){return user._id})

pontocom-Aspire-4252(mongod-3.2.7) be-mean-project> query = {'members.user_id': {$in: users}}
{
  "members.user_id": {
    "$in": [
      ObjectId("578fba3f19757617cf8dce5e"),
      ObjectId("578fba3f19757617cf8dce5f")
    ]
  }
}
pontocom-Aspire-4252(mongod-3.2.7) be-mean-project> db.projects.remove(query)
Removed 4 record(s) in 280ms
WriteResult({
  "nRemoved": 4
})

5 - Apague todos os projetos que possuam uma determinada tag em goal.

var query = {
  "goals.tags": ObjectId("578fc1b019757617cf8dce78")
}

pontocom-Aspire-4252(mongod-3.2.7) be-mean-project> db.projects.remove(query)
Removed 0 record(s) in 3ms
WriteResult({
  "nRemoved": 0
})

```

## Gerenciamento de usuários

- Crie um usuário com permissões APENAS de Leitura.

```
var user = { user: "Patrick",
  pwd: "123",
  roles: [
    { role: "read", db: "be-mean-project" } 
  ]
}

pontocom-Aspire-4252(mongod-3.2.7) be-mean-project> db.createUser(user)
Successfully added user: {
  "user": "Patrick",
  "roles": [
    {
      "role": "read",
      "db": "be-mean-project"
    }
  ]

```
- Crie um usuário com permissões de Escrita e Leitura.

```
var user = { user: "Patrick Escritor",
  pwd: "123",
  roles: [
    { role: "readWrite", db: "be-mean-project" }  ]
}

pontocom-Aspire-4252(mongod-3.2.7) be-mean-project> db.createUser(user)
Successfully added user: {
  "user": "Patrick Escritor",
  "roles": [
    {
      "role": "readWrite",
      "db": "be-mean-project"
    }
  ]
}
```
- Adicionar o papel grantRolesToUser e revokeRole para o usuário com Escrita e Leitura.

```
db.createRole(
   {
     role: "grantRolesToUser",
     privileges: [
       { resource: { db: "be-mean-project", collection: "" }, actions: [ "grantRole" ] }
     ],
     roles: []
   }
)

{
  "role": "grantRolesToUser",
  "privileges": [
    {
      "resource": {
        "db": "be-mean-project",
        "collection": ""
      },
      "actions": [
        "grantRole"
      ]
    }
  ],
  "roles": [ ]
}

db.createRole(
   {
     role: "revokeRole",
     privileges: [
       { resource: { db: "be-mean-project", collection: "" }, actions: [ "revokeRole" ] }
     ],
     roles: []
   }
)
{
  "role": "revokeRole",
  "privileges": [
    {
      "resource": {
        "db": "be-mean-project",
        "collection": ""
      },
      "actions": [
        "revokeRole"
      ]
    }
  ],
  "roles": [ ]
}

var query = {
  "grantRolesToUser": "Patrick Escritor",
  "roles": [
    "grantRolesToUser",
    "revokeRole"
  ],
  "writeConcern": {
    "w": "majority"
  }
}

pontocom-Aspire-4252(mongod-3.2.7) be-mean-project> db.runCommand(query)
{
  "ok": 1
}


```

- Remover o papel grantRolesToUser para o usuário com Escrita e Leitura.
```
	var query = {
	  "revokeRolesFromUser": "Patrick Escritor",
	  "roles": [
	    "grantRolesToUser"
	  ],
	  "writeConcern": {
	    "w": "majority"
	  }
	}

	pontocom-Aspire-4252(mongod-3.2.7) be-mean-project> db.runCommand(query)
	{
	  "ok": 1
	}

```
- Listar todos os usuários com seus papéis e ações.
```
pontocom-Aspire-4252(mongod-3.2.7) be-mean-project> db.getUsers()
[
  {
    "_id": "be-mean-project.Patrick",
    "user": "Patrick",
    "db": "be-mean-project",
    "roles": [
      {
        "role": "read",
        "db": "be-mean-project"
      }
    ]
  },
  {
    "_id": "be-mean-project.Patrick Escritor",
    "user": "Patrick Escritor",
    "db": "be-mean-project",
    "roles": [
      {
        "role": "revokeRole",
        "db": "be-mean-project"
      },
      {
        "role": "readWrite",
        "db": "be-mean-project"
      }
    ]
  }
]

```

## Sharding
```
	CRIANDO CONFIG SERVER

	sudo mkdir configdb
	mongod --configsvr --port 27010

	CRIANDO ROUTER
	mongos --configdb localhost:27010 --port 27011
	2016-07-29T06:35:37.398+0100 W SHARDING [main] Running a sharded cluster with fewer than 3 config servers should only be done for testing 	purposes and is not recommended for production.

	CRIANDO SHARDINGS
	
	sudo mongod --port 27012 --dbpath /data/shard1 
	2016-07-29T06:40:04.588+0100 I CONTROL  [initandlisten] MongoDB starting : pid=4057 port=27012 dbpath=/data/shard1 64-bit host=pontocom-Aspire-4252

	sudo mongod --port 27013 --dbpath /data/shard2
	2016-07-29T06:40:53.875+0100 I CONTROL  [initandlisten] MongoDB starting : pid=4105 port=27013 dbpath=/data/shard2 64-bit host=pontocom-Aspire-4252

	sudo mongod --port 27014 --dbpath /data/shard3
	2016-07-29T06:41:29.199+0100 I CONTROL  [initandlisten] MongoDB starting : pid=4143 port=27014 dbpath=/data/shard3 64-bit host=pontocom-Aspire-4252

	ADICIONANDO SHARDINGS AO ROUTER

	mongos> sh.addShard("localhost:27012")
	{
	  "shardAdded": "shard0000",
	  "ok": 1
	}
	pontocom-Aspire-4252:27011(mongos-3.2.7)[mongos] test> sh.addShard("localhost:27013")
	{
	  "shardAdded": "shard0001",
	  "ok": 1
	}
	pontocom-Aspire-4252:27011(mongos-3.2.7)[mongos] test> sh.addShard("localhost:27014")
	{
	  "shardAdded": "shard0002",
	  "ok": 1
	}

	ESPECIFICANDO DATABASE PARA SHARD

	pontocom-Aspire-4252:27011(mongos-3.2.7)[mongos] test> sh.enableSharding("be-mean-project")
	{
	  "ok": 1
	}

	ESPECIFICANDO COLEÇÃO

	pontocom-Aspire-4252:27011(mongos-3.2.7)[mongos] test> sh.shardCollection("be-mean-project.projects", {"_id": 1})
	{
	  "collectionsharded": "be-mean-project.projects",
	  "ok": 1
	}


```

## Replica
```
	STARTANDO REPLICAS
	sudo mongod --replSet replica_set --port 27017 --dbpath /data/replica1
	sudo mongod --replSet replica_set --port 27018 --dbpath /data/replica2
	sudo mongod --replSet replica_set --port 27019 --dbpath /data/replica3

	CONFIGURANDO E ADICIONANDO

	replicaConf = { "_id": "replica_set", members: [ { "_id": 0, "host": "127.0.0.1:27017" } ] })
	
	rs.initiate(replicaConf)

	rs.add("localhost:27018")
	rs.add("localhost:27019")

```
