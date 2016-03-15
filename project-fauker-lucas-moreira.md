# MongoDb - Projeto Final
**Autor:** Lucas da Silva Moreira

**Data**: 1456674804092

## Para qual sistema você usaria o MogoDB (diferente desse)?
- Chat
- ERP
- Sistemas de CRUD
- Sistemas que eu tenha que mostrar várias informações de uma vez

## Qual a modelagem da sua coleção de `users`?

```
user: {
	id_user: ObjectId,
	name: String,
	bio: String,
	date_register: Date,
	avatar_path: String,
	background_path: String,

	auth: {
		username: String,
		email: String,
		password: String,
		last_acess: Date,
		online: Boolean,
		disabled: Boolean,
		hash-token: String
	}
}
```

## Qual a modelagem da sua coleção de `projects`?

```
project: {
  "id": ObjectId
  "name": String
  "description": String
  "date_begin": Date(),
  "date_dream": Date(),
  "date_end": Date(),
  "visible": boolean
  "realocate": boolean,
  "expired": boolean,
  "visualizable_mod": String
  "tags": [],
  "goals": [
    {
      "name": String
      "description": String,
      "date_begin": Date(),
      "date_dream": Date(),
      "date_end": Date(),
      "realocate": boolean
      "expired": boolean
      "tags": [],
      "historic": {
        "date_realocate": Date()
      },
      "activities": [
        {
          "activity_id": ObjectId
        }
      ]
    }
  ],
  "members": [
    {
      "id_user": ObjectId,
      "notify": String
      "type": String
    }
  ]
}
```

## Qual a modelagem da sua coleção retirada de `projects`?

```
activity: {
	name: String
	description: String
	date_begin: Date(),
	date_dream: Date(),
	date_end: Date(),
	realocate: false,
	expired: false,

	historic: {
		date_realocate: Date()
	},

	tags: [],

	members: [{
		id_user: ObjectId,
		notify: String,
		type: String,
	}],

	comments: [{
		members: [{
			id_user: ObjectId,
			notify: String,
			type: String,
		}],
        text: String,
        date_comment: Date(),
        files: [{
			path: String,
			weight: String,
			name: String
		}]				
	}]
}
```
### Motivo

Separei a coleção `activities` da coleção `projects` porque um projeto pode ter várias atividades, e como uma atividade pode se tornar uma coleção grande, resolvi separar e não carregá-la desnecessariamente toda vez que a coleção projects for buscada.

## Create - cadastro

#### 1. Cadastre 10 usuários diferentes.

```
var users = [
	{name: "Adriana Castelhano", username: "adriana_castelhado"},
	{name: "Cleusa Antas", username: "cleusa_antas"},
	{name: "Eusébio Piñero", username: "eusebio_pinero"},
	{name: "Ilma Doutel", username: "ilma_doutel"},
	{name: "Marina Carvalhosa", username: "marina_carvalhosa"},
	{name: "Milena Nieves", username: "milena_nieves"},
	{name: "Nádia Santos", username: "nadia_santos"},
	{name: "Palo Tamoio", username: "palo_tamoio"},
	{name: "Silvana Jordão", username: "silvana_jordao"},
	{name: "Virgínia Remígio", username: "virginia_remigio"}	
]

var usersList = []

users.forEach(function(user) {
	var query = {
		name: user.name,
		bio: "Lifelong communicator. Pop culture buff. Beer expert. Proud zombie practitioner.",
		date_register: new Date(),
		avatar_path: "http://i2.wp.com/tecnodia.com.br/wp-content/uploads/2013/05/facebook-geek-avatar.jpg",
		background_path: "http://www.pulsarwallpapers.com/data/media/3/Alien%20Ink%202560X1600%20Abstract%20Background.jpg",
		auth: {
			username: user.username,
			email: user.username+"@email.com",
			password: user.username+"@"+user.username.length,
			last_acess: new Date(),
			online: true,
			disabled: false,
			hash_token: "698dc19d489c4e4db73e28a713eab07b"+user.username.length
		}
	}
	usersList.push(query)
})

db.users.insert(usersList)

Inserted 1 record(s) in 553ms
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
- cada um com 5 membros, sempre diferentes dentro dos projetos;
- cada um com pelo menos 3 tags diferentes;
    - escolha 1 *tag* onde deva ficar em 2 projetos;
    - escolha 1 *tag* onde deva ficar em 3 projetos;
- cada projeto com pelo menos 1 *goal*;
    - cada *goal* com pelo menos 3 *tags*;
    - cada *goal* com pelo menos 2 atividades, deixe 1 projeto sem.

```

// Salvando duas atividades para referenciar nos projetos

for (var i = 1; i <= 2; i++) {
	db.activities.insert({ 
		name: 'Atividade número ' + i,
		description: 'Descrição da atividade número ' + i,
		date_begin: new Date(),
		date_dream: new Date(),
		date_end: new Date(),
		realocate: false,
		expired: false,
		historic: {
			date_realocate: new Date()
		},
		tags: [],
		members: [],
		comments: []
	})
}
Inserted 1 record(s) in 555ms
Inserted 1 record(s) in 2ms
WriteResult({
  "nInserted": 1
})
Atividade número 1: "_id": ObjectId("5689a2fbc3f8dbd6487e1c74"),
Atividade número 2: "_id": ObjectId("5689a2fcc3f8dbd6487e1c75"),

var users = db.users.find({}, {id: 1}).toArray()

db.projects.insert([
{
  "name": "Primeiro projeto",
  "description": "Meu primeiro projeto",
  "date_begin": new Date(),
  "date_dream": new Date(),
  "date_end": new Date(),
  "visible": true,
  "realocate": true,
  "expired": false,
  "visualizable_mod": "asopijovjsaopivjasdoivj",
  "tags": [
    "Suissa",
    "Be MEAN",
    "MongoDB"
  ],
  "goals": [
    {
      "name": "Ficar mt loco nas programações",
      "description": "Ce não tá ligado",
      "date_begin": new Date(),
      "date_dream": new Date(),
      "date_end": new Date(),
      "realocate": false,
      "expired": false,
      "tags": [
        "JavaScript",
        "Programação",
        "Code"
      ],
      "historic": {
        "date_realocate": new Date(),
      },
      "activities": [
        {
          "activity_id": ObjectId("5689a2fbc3f8dbd6487e1c74")
        },
        {
          "activity_id": ObjectId("5689a2fcc3f8dbd6487e1c75")
        }
      ]
    }
  ],
  "members": [
    {
      "id_user": ObjectId("56807d83e15bcf96ff94afc6"),
      "notify": "Notificação",
      "type": "Membro"
    },
    {
      "id_user": ObjectId("56807d83e15bcf96ff94afc7"),
      "notify": "Notificação",
      "type": "Membro"
    },
    {
      "id_user": ObjectId("56807d83e15bcf96ff94afc8"),
      "notify": "Notificação",
      "type": "Membro"
    },
    {
      "id_user": ObjectId("56807d83e15bcf96ff94afc9"),
      "notify": "Notificação",
      "type": "Membro"
    },
    {
      "id_user": ObjectId("56807d83e15bcf96ff94afca"),
      "notify": "Notificação",
      "type": "Membro"
    }
  ]
},
{
  "name": "segunro projeto",
  "description": "Meu segunro projeto",
  "date_begin": new Date(),
  "date_dream": new Date(),
  "date_end": new Date(),
  "visible": true,
  "realocate": true,
  "expired": false,
  "visualizable_mod": "asopijovjsaopivjasdoivj",
  "tags": [
    "Suissa",
    "Be MEAN",
    "JavaScript"
  ],
  "goals": [
    {
      "name": "Ficar expert",
      "description": "Sé doido manuuuuu",
      "date_begin": new Date(),
      "date_dream": new Date(),
      "date_end": new Date(),
      "realocate": false,
      "expired": false,
      "tags": [
        "JavaScript",
        "Programação",
        "Code"
      ],
      "historic": {
        "date_realocate": new Date(),
      },
      "activities": [
        {
          "activity_id": ObjectId("5689a2fbc3f8dbd6487e1c74")
        },
        {
          "activity_id": ObjectId("5689a2fcc3f8dbd6487e1c75")
        }
      ]
    }
  ],
  "members": [
    {
      "id_user": ObjectId("56807d83e15bcf96ff94afc6"),
      "notify": "Notificação",
      "type": "Membro"
    },
    {
      "id_user": ObjectId("56807d83e15bcf96ff94afc7"),
      "notify": "Notificação",
      "type": "Membro"
    },
    {
      "id_user": ObjectId("56807d83e15bcf96ff94afc8"),
      "notify": "Notificação",
      "type": "Membro"
    },
    {
      "id_user": ObjectId("56807d83e15bcf96ff94afc9"),
      "notify": "Notificação",
      "type": "Membro"
    },
    {
      "id_user": ObjectId("56807d83e15bcf96ff94afca"),
      "notify": "Notificação",
      "type": "Membro"
    }
  ]
},
{
  "name": "terceiro projeto",
  "description": "Meu terceiro projeto",
  "date_begin": new Date(),
  "date_dream": new Date(),
  "date_end": new Date(),
  "visible": true,
  "realocate": true,
  "expired": false,
  "visualizable_mod": "asopijovjsaopivjasdoivj",
  "tags": [
    "Webschool",
    "Be MEAN",
    "Web"
  ],
  "goals": [
    {
      "name": "Entender mais a stack MEAN",
      "description": "MEAN NA VEIA!!!!",
      "date_begin": new Date(),
      "date_dream": new Date(),
      "date_end": new Date(),
      "realocate": false,
      "expired": false,
      "tags": [
        "JavaScript",
        "Programação",
        "Code"
      ],
      "historic": {
        "date_realocate": new Date(),
      },
      "activities": [
        {
          "activity_id": ObjectId("5689a2fbc3f8dbd6487e1c74")
        },
        {
          "activity_id": ObjectId("5689a2fcc3f8dbd6487e1c75")
        }
      ]
    }
  ],
  "members": [
    {
      "id_user": ObjectId("56807d83e15bcf96ff94afc6"),
      "notify": "Notificação",
      "type": "Membro"
    },
    {
      "id_user": ObjectId("56807d83e15bcf96ff94afc7"),
      "notify": "Notificação",
      "type": "Membro"
    },
    {
      "id_user": ObjectId("56807d83e15bcf96ff94afc8"),
      "notify": "Notificação",
      "type": "Membro"
    },
    {
      "id_user": ObjectId("56807d83e15bcf96ff94afc9"),
      "notify": "Notificação",
      "type": "Membro"
    },
    {
      "id_user": ObjectId("56807d83e15bcf96ff94afca"),
      "notify": "Notificação",
      "type": "Membro"
    }
  ]
},
{
  "name": "quarto projeto",
  "description": "Meu Quarto projeto",
  "date_begin": new Date(),
  "date_dream": new Date(),
  "date_end": new Date(),
  "visible": true,
  "realocate": true,
  "expired": false,
  "visualizable_mod": "asopijovjsaopivjasdoivj",
  "tags": [
    "Frontend",
    "Workshop",
    "http"
  ],
  "goals": [
    {
      "name": "desenvolver tudo com JSS",
      "description": "PQ JS EH TOPPPPP",
      "date_begin": new Date(),
      "date_dream": new Date(),
      "date_end": new Date(),
      "realocate": false,
      "expired": false,
      "tags": [
        "JavaScript",
        "Programação",
        "Code"
      ],
      "historic": {
        "date_realocate": new Date(),
      },
      "activities": [
        {
          "activity_id": ObjectId("5689a2fbc3f8dbd6487e1c74")
        },
        {
          "activity_id": ObjectId("5689a2fcc3f8dbd6487e1c75")
        }
      ]
    }
  ],
  "members": [
    {
      "id_user": ObjectId("56807d83e15bcf96ff94afc6"),
      "notify": "Notificação",
      "type": "Membro"
    },
    {
      "id_user": ObjectId("56807d83e15bcf96ff94afc7"),
      "notify": "Notificação",
      "type": "Membro"
    },
    {
      "id_user": ObjectId("56807d83e15bcf96ff94afc8"),
      "notify": "Notificação",
      "type": "Membro"
    },
    {
      "id_user": ObjectId("56807d83e15bcf96ff94afc9"),
      "notify": "Notificação",
      "type": "Membro"
    },
    {
      "id_user": ObjectId("56807d83e15bcf96ff94afca"),
      "notify": "Notificação",
      "type": "Membro"
    }
  ]
},
{
  "name": "quintoProjeto projeto",
  "description": "Meu Quinto projeto",
  "date_begin": new Date(),
  "date_dream": new Date(),
  "date_end": new Date(),
  "visible": true,
  "realocate": true,
  "expired": false,
  "visualizable_mod": "asopijovjsaopivjasdoivj",
  "tags": [
    "js",
    "mean",
    "stack"
  ],
  "goals": [
    {
      "name": "ficar fodassalhao em js",
      "description": "tudo agora será em js",
      "date_begin": new Date(),
      "date_dream": new Date(),
      "date_end": new Date(),
      "realocate": false,
      "expired": false,
      "tags": [
        "JavaScript",
        "Programação",
        "Code"
      ],
      "historic": {
        "date_realocate": new Date(),
      },
      "activities": [ ]
    }
  ],
  "members": [
    {
      "id_user": ObjectId("56807d83e15bcf96ff94afc6"),
      "notify": "Notificação",
      "type": "Membro"
    },
    {
      "id_user": ObjectId("56807d83e15bcf96ff94afc7"),
      "notify": "Notificação",
      "type": "Membro"
    },
    {
      "id_user": ObjectId("56807d83e15bcf96ff94afc8"),
      "notify": "Notificação",
      "type": "Membro"
    },
    {
      "id_user": ObjectId("56807d83e15bcf96ff94afc9"),
      "notify": "Notificação",
      "type": "Membro"
    },
    {
      "id_user": ObjectId("56807d83e15bcf96ff94afca"),
      "notify": "Notificação",
      "type": "Membro"
    }
  ]
}])

Inserted 1 record(s) in 517ms
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

```
var query = {name: /primeiro projeto/i}
var fields = {_id: 0, members: 1}
var membrosDosProjetos = db.projects.findOne(query, fields)

var membros = [];

membrosDosProjetos.members.forEach(function(membro) {
	membros.push(db.users.findOne({_id: membro.id_user}))	
})

membros

[
  {
    "_id": ObjectId("56807d83e15bcf96ff94afc6"),
    "name": "Cleusa Antas",
    "bio": "Lifelong communicator. Pop culture buff. Beer expert. Proud zombie practitioner.",
    "date_register": ISODate("2015-12-28T00:08:29.811Z"),
    "avatar_path": "http://i2.wp.com/tecnodia.com.br/wp-content/uploads/2013/05/facebook-geek-avatar.jpg",
    "background_path": "http://www.pulsarwallpapers.com/data/media/3/Alien%20Ink%202560X1600%20Abstract%20Background.jpg",
    "auth": {
      "username": "cleusa_antas",
      "email": "cleusa_antas@email.com",
      "password": "cleusa_antas@12",
      "last_acess": ISODate("2015-12-28T00:08:29.811Z"),
      "online": true,
      "disabled": false,
      "hash_token": "698dc19d489c4e4db73e28a713eab07b12"
    }
  },
  {
    "_id": ObjectId("56807d83e15bcf96ff94afc7"),
    "name": "Eusébio Piñero",
    "bio": "Lifelong communicator. Pop culture buff. Beer expert. Proud zombie practitioner.",
    "date_register": ISODate("2015-12-28T00:08:29.811Z"),
    "avatar_path": "http://i2.wp.com/tecnodia.com.br/wp-content/uploads/2013/05/facebook-geek-avatar.jpg",
    "background_path": "http://www.pulsarwallpapers.com/data/media/3/Alien%20Ink%202560X1600%20Abstract%20Background.jpg",
    "auth": {
      "username": "eusebio_pinero",
      "email": "eusebio_pinero@email.com",
      "password": "eusebio_pinero@14",
      "last_acess": ISODate("2015-12-28T00:08:29.811Z"),
      "online": true,
      "disabled": false,
      "hash_token": "698dc19d489c4e4db73e28a713eab07b14"
    }
  },
  {
    "_id": ObjectId("56807d83e15bcf96ff94afc8"),
    "name": "Ilma Doutel",
    "bio": "Lifelong communicator. Pop culture buff. Beer expert. Proud zombie practitioner.",
    "date_register": ISODate("2015-12-28T00:08:29.811Z"),
    "avatar_path": "http://i2.wp.com/tecnodia.com.br/wp-content/uploads/2013/05/facebook-geek-avatar.jpg",
    "background_path": "http://www.pulsarwallpapers.com/data/media/3/Alien%20Ink%202560X1600%20Abstract%20Background.jpg",
    "auth": {
      "username": "ilma_doutel",
      "email": "ilma_doutel@email.com",
      "password": "ilma_doutel@11",
      "last_acess": ISODate("2015-12-28T00:08:29.811Z"),
      "online": true,
      "disabled": false,
      "hash_token": "698dc19d489c4e4db73e28a713eab07b11"
    }
  },
  {
    "_id": ObjectId("56807d83e15bcf96ff94afc9"),
    "name": "Marina Carvalhosa",
    "bio": "Lifelong communicator. Pop culture buff. Beer expert. Proud zombie practitioner.",
    "date_register": ISODate("2015-12-28T00:08:29.811Z"),
    "avatar_path": "http://i2.wp.com/tecnodia.com.br/wp-content/uploads/2013/05/facebook-geek-avatar.jpg",
    "background_path": "http://www.pulsarwallpapers.com/data/media/3/Alien%20Ink%202560X1600%20Abstract%20Background.jpg",
    "auth": {
      "username": "marina_carvalhosa",
      "email": "marina_carvalhosa@email.com",
      "password": "marina_carvalhosa@17",
      "last_acess": ISODate("2015-12-28T00:08:29.811Z"),
      "online": true,
      "disabled": false,
      "hash_token": "698dc19d489c4e4db73e28a713eab07b17"
    }
  },
  {
    "_id": ObjectId("56807d83e15bcf96ff94afca"),
    "name": "Milena Nieves",
    "bio": "Lifelong communicator. Pop culture buff. Beer expert. Proud zombie practitioner.",
    "date_register": ISODate("2015-12-28T00:08:29.811Z"),
    "avatar_path": "http://i2.wp.com/tecnodia.com.br/wp-content/uploads/2013/05/facebook-geek-avatar.jpg",
    "background_path": "http://www.pulsarwallpapers.com/data/media/3/Alien%20Ink%202560X1600%20Abstract%20Background.jpg",
    "auth": {
      "username": "milena_nieves",
      "email": "milena_nieves@email.com",
      "password": "milena_nieves@13",
      "last_acess": ISODate("2015-12-28T00:08:29.811Z"),
      "online": true,
      "disabled": false,
      "hash_token": "698dc19d489c4e4db73e28a713eab07b13"
    }
  }
]
```

#### 2. Liste todos os projetos com a tag que você escolheu para os 3 projetos em comum.

A tag que eu escolhi para inserir em 3 projetos foi **Be MEAN**!!!

```
var query = {tags: {$in: ['Be MEAN']}}
db.projects.find(query)
{
  "_id": ObjectId("568c0576c3f8dbd6487e1c89"),
  "name": "Primeiro projeto",
  "description": "Meu primeiro projeto",
  "date_begin": ISODate("2016-01-05T18:03:34.041Z"),
  "date_dream": ISODate("2016-01-05T18:03:34.041Z"),
  "date_end": ISODate("2016-01-05T18:03:34.041Z"),
  "visible": true,
  "realocate": true,
  "expired": false,
  "visualizable_mod": "asopijovjsaopivjasdoivj",
  "tags": [
    "Suissa",
    "Be MEAN",
    "MongoDB"
  ],
  "goals": [
    {
      "name": "Ficar mt loco nas programações",
      "description": "Ce não tá ligado",
      "date_begin": ISODate("2016-01-05T18:03:34.041Z"),
      "date_dream": ISODate("2016-01-05T18:03:34.041Z"),
      "date_end": ISODate("2016-01-05T18:03:34.041Z"),
      "realocate": false,
      "expired": false,
      "tags": [
        "JavaScript",
        "Programação",
        "Code"
      ],
      "historic": {
        "date_realocate": ISODate("2016-01-05T18:03:34.042Z")
      },
      "activities": [
        {
          "activity_id": ObjectId("5689a2fcc3f8dbd6487e1c74")
        },
        {
          "activity_id": ObjectId("5689a2fcc3f8dbd6487e1c75")
        }
      ]
    }
  ],
  "members": [
    {
      "id_user": ObjectId("56807d83e15bcf96ff94afc6"),
      "notify": "Notificação",
      "type": "Membro"
    },
    {
      "id_user": ObjectId("56807d83e15bcf96ff94afc7"),
      "notify": "Notificação",
      "type": "Membro"
    },
    {
      "id_user": ObjectId("56807d83e15bcf96ff94afc8"),
      "notify": "Notificação",
      "type": "Membro"
    },
    {
      "id_user": ObjectId("56807d83e15bcf96ff94afc9"),
      "notify": "Notificação",
      "type": "Membro"
    },
    {
      "id_user": ObjectId("56807d83e15bcf96ff94afca"),
      "notify": "Notificação",
      "type": "Membro"
    }
  ]
}
{
  "_id": ObjectId("568c0576c3f8dbd6487e1c8a"),
  "name": "segunro projeto",
  "description": "Meu segunro projeto",
  "date_begin": ISODate("2016-01-05T18:03:34.092Z"),
  "date_dream": ISODate("2016-01-05T18:03:34.092Z"),
  "date_end": ISODate("2016-01-05T18:03:34.092Z"),
  "visible": true,
  "realocate": true,
  "expired": false,
  "visualizable_mod": "asopijovjsaopivjasdoivj",
  "tags": [
    "Suissa",
    "Be MEAN",
    "JavaScript"
  ],
  "goals": [
    {
      "name": "Ficar expert",
      "description": "Sé doido manuuuuu",
      "date_begin": ISODate("2016-01-05T18:03:34.092Z"),
      "date_dream": ISODate("2016-01-05T18:03:34.092Z"),
      "date_end": ISODate("2016-01-05T18:03:34.092Z"),
      "realocate": false,
      "expired": false,
      "tags": [
        "JavaScript",
        "Programação",
        "Code"
      ],
      "historic": {
        "date_realocate": ISODate("2016-01-05T18:03:34.092Z")
      },
      "activities": [
        {
          "activity_id": ObjectId("5689a2fcc3f8dbd6487e1c74")
        },
        {
          "activity_id": ObjectId("5689a2fcc3f8dbd6487e1c75")
        }
      ]
    }
  ],
  "members": [
    {
      "id_user": ObjectId("56807d83e15bcf96ff94afc6"),
      "notify": "Notificação",
      "type": "Membro"
    },
    {
      "id_user": ObjectId("56807d83e15bcf96ff94afc7"),
      "notify": "Notificação",
      "type": "Membro"
    },
    {
      "id_user": ObjectId("56807d83e15bcf96ff94afc8"),
      "notify": "Notificação",
      "type": "Membro"
    },
    {
      "id_user": ObjectId("56807d83e15bcf96ff94afc9"),
      "notify": "Notificação",
      "type": "Membro"
    },
    {
      "id_user": ObjectId("56807d83e15bcf96ff94afca"),
      "notify": "Notificação",
      "type": "Membro"
    }
  ]
}
{
  "_id": ObjectId("568c0576c3f8dbd6487e1c8b"),
  "name": "terceiro projeto",
  "description": "Meu terceiro projeto",
  "date_begin": ISODate("2016-01-05T18:03:34.092Z"),
  "date_dream": ISODate("2016-01-05T18:03:34.092Z"),
  "date_end": ISODate("2016-01-05T18:03:34.092Z"),
  "visible": true,
  "realocate": true,
  "expired": false,
  "visualizable_mod": "asopijovjsaopivjasdoivj",
  "tags": [
    "Webschool",
    "Be MEAN",
    "Web"
  ],
  "goals": [
    {
      "name": "Entender mais a stack MEAN",
      "description": "MEAN NA VEIA!!!!",
      "date_begin": ISODate("2016-01-05T18:03:34.092Z"),
      "date_dream": ISODate("2016-01-05T18:03:34.092Z"),
      "date_end": ISODate("2016-01-05T18:03:34.092Z"),
      "realocate": false,
      "expired": false,
      "tags": [
        "JavaScript",
        "Programação",
        "Code"
      ],
      "historic": {
        "date_realocate": ISODate("2016-01-05T18:03:34.092Z")
      },
      "activities": [
        {
          "activity_id": ObjectId("5689a2fcc3f8dbd6487e1c74")
        },
        {
          "activity_id": ObjectId("5689a2fcc3f8dbd6487e1c75")
        }
      ]
    }
  ],
  "members": [
    {
      "id_user": ObjectId("56807d83e15bcf96ff94afc6"),
      "notify": "Notificação",
      "type": "Membro"
    },
    {
      "id_user": ObjectId("56807d83e15bcf96ff94afc7"),
      "notify": "Notificação",
      "type": "Membro"
    },
    {
      "id_user": ObjectId("56807d83e15bcf96ff94afc8"),
      "notify": "Notificação",
      "type": "Membro"
    },
    {
      "id_user": ObjectId("56807d83e15bcf96ff94afc9"),
      "notify": "Notificação",
      "type": "Membro"
    },
    {
      "id_user": ObjectId("56807d83e15bcf96ff94afca"),
      "notify": "Notificação",
      "type": "Membro"
    }
  ]
}
```

#### 3. Liste apenas os nomes de todas as atividades para todos os projetos.

```
var nomesAtividades = []
var projetos = db.projects.find({}, {'goals.activities': 1, _id: 0}).toArray()
  projetos.forEach(function (projeto) {
    var atividades = projeto.goals[0].activities
    atividades.forEach(function (atividade) {
      var atv = db.activities.findOne({_id: atividade.activity_id})
      if (atv) {
          nomesAtividades.push(atv.name)
      }
    })
  })

nomesAtividades
[
  "Atividade número 1",
  "Atividade número 2",
  "Atividade número 1",
  "Atividade número 2",
  "Atividade número 1",
  "Atividade número 2",
  "Atividade número 1",
  "Atividade número 2"
]
```

#### 4. Liste todos os projetos que não possuam uma tag.

```
var query = { tags:  { $not: { $in: ['JavaScript'] } } }
db.projects.find(query, {name: 1})
{
  "_id": ObjectId("568c0576c3f8dbd6487e1c89"),
  "name": "Primeiro projeto"
}
{
  "_id": ObjectId("568c0576c3f8dbd6487e1c8b"),
  "name": "terceiro projeto"
}
{
  "_id": ObjectId("568c0576c3f8dbd6487e1c8c"),
  "name": "quarto projeto"
}
{
  "_id": ObjectId("568c0576c3f8dbd6487e1c8d"),
  "name": "quintoProjeto projeto"
}
```

#### 5. Liste todos os usuários que não fazem parte do primeiro projeto cadastrado.

```
var usuariosPrimeiroProjeto = db.projects.findOne({name: /primeiro projeto/i},{members:1, _id:0})
var idsUsuariosPrimeiroProjeto = []
usuariosPrimeiroProjeto.members.forEach(function (user) {
  idsUsuariosPrimeiroProjeto.push(user.id_user)
})

db.users.find({_id: {$not: {$in: idsUsuariosPrimeiroProjeto}}}, {name: 1})
{
  "_id": ObjectId("56807d83e15bcf96ff94afc5"),
  "name": "Adriana Castelhano"
}
{
  "_id": ObjectId("56807d83e15bcf96ff94afcb"),
  "name": "Nádia Santos"
}
{
  "_id": ObjectId("56807d83e15bcf96ff94afcc"),
  "name": "Palo Tamoio"
}
{
  "_id": ObjectId("56807d83e15bcf96ff94afcd"),
  "name": "Silvana Jordão"
}
{
  "_id": ObjectId("56807d83e15bcf96ff94afce"),
  "name": "Virgínia Remígio"
}
```

## Update - alteração

#### 1. Adicione para todos os projetos o campo views: 0.

```
var query = {}
var mod = {$set: {views: 0}}
var options = {multi: true}
db.projects.update(query, mod, options)

Updated 5 existing record(s) in 118ms
WriteResult({
  "nMatched": 5,
  "nUpserted": 0,
  "nModified": 5
})  
```

#### 2. Adicione 1 tag diferente para cada projeto.

```
var projetos = db.projects.find({}, {name: 1}).toArray()
projetos.forEach(function (projeto) {
  var mods = {$push: {tags: 'Nova tag do projeto ' + projeto.name}}
  db.projects.update({_id: projeto._id}, mods)
})  

Updated 1 existing record(s) in 70ms
Updated 1 existing record(s) in 1ms
Updated 1 existing record(s) in 1ms
Updated 1 existing record(s) in 1ms
Updated 1 existing record(s) in 0ms
```

#### 3. Adicione 2 membros diferentes para cada projeto.

```
var projetos = db.projects.find().toArray()
var idsUsers = db.users.find({}, {_id: 1}).toArray()

for (var i = 0; i < projetos.length; i++) {
  var primeiroMembro = {
    id_user: idsUsers[i],
    notify: 'Notificação',
    type: 'Membro'
  };

  var segundoMembro = {
    id_user: idsUsers[i+1],
    notify: 'Notificação',
    type: 'Membro'
  };
  db.projects.update({ _id: projetos[i]._id }, { $push: { members: primeiroMembro } });
  db.projects.update({ _id: projetos[i]._id }, { $push: { members: segundoMembro } });  
}

Updated 1 existing record(s) in 46ms
Updated 1 existing record(s) in 10ms
Updated 1 existing record(s) in 1ms
Updated 1 existing record(s) in 1ms
Updated 1 existing record(s) in 1ms
Updated 1 existing record(s) in 0ms
Updated 1 existing record(s) in 1ms
Updated 1 existing record(s) in 1ms
Updated 1 existing record(s) in 1ms
Updated 1 existing record(s) in 0ms
WriteResult({
  "nMatched": 1,
  "nUpserted": 0,
  "nModified": 1
})
```

#### 4. Adicione 1 comentário em cada atividade, deixe apenas 1 projeto sem.

```
var atividades = db.activities.find().toArray()
var idsUsers = db.users.find({}, {_id: 1}).toArray()

//comecando pela segunda atividade (1)
for (var i = 1; i < atividades.length; i++) {
  comentario = {
    members: [{
      id_user: idsUsers[i],
      notify: 'Notificacao',
      type: 'Member'
    }],
    text: 'Vejam só!!! Este é o comentário da atividade ' + atividades[i],
    date_comment: new Date(),
    files: [{
      path: '/Users/fauker/',
      weight: i + 'MB',
      name: 'Document' + i + '.pdf'
    }]
  }
  db.activities.update({ _id: atividades[i]._id }, { $push: { comments: comentario } });
}

Updated 1 existing record(s) in 4ms
Updated 1 existing record(s) in 1ms
Updated 1 existing record(s) in 1ms
Updated 1 existing record(s) in 1ms
Updated 1 existing record(s) in 0ms
Updated 1 existing record(s) in 1ms
Updated 1 existing record(s) in 1ms
Updated 1 existing record(s) in 1ms
Updated 1 existing record(s) in 1ms
WriteResult({
  "nMatched": 1,
  "nUpserted": 0,
  "nModified": 1
})
```

#### 5. Adicione 1 projeto inteiro com UPSERT.

```
var mods = {
  "name": "Projeto inserido com upsert",
  "description": "Projeto inserido com upsert",
  "date_begin": new Date(),
  "date_dream": new Date(),
  "date_end": new Date(),
  "visible": true,
  "realocate": true,
  "expired": false,
  "visualizable_mod": "asopijovjsaopivjasdoivj",
  "tags": [
    "upsert",
    "update",
    "collections"
  ],
  "goals": [
    {
      "name": "inserir saporra com upsert",
      "description": "agr eh com upsert man",
      "date_begin": new Date(),
      "date_dream": new Date(),
      "date_end": new Date(),
      "realocate": false,
      "expired": false,
      "tags": [
        "Mouse",
        "Teclado",
        "Fone"
      ],
      "historic": {
        "date_realocate": new Date(),
      },
      "activities": [ ]
    }
  ],
  "members": [
    {
      "id_user": ObjectId("56807d83e15bcf96ff94afc6"),
      "notify": "Notificação",
      "type": "Membro"
    },
    {
      "id_user": ObjectId("56807d83e15bcf96ff94afc7"),
      "notify": "Notificação",
      "type": "Membro"
    },
    {
      "id_user": ObjectId("56807d83e15bcf96ff94afc8"),
      "notify": "Notificação",
      "type": "Membro"
    },
    {
      "id_user": ObjectId("56807d83e15bcf96ff94afc9"),
      "notify": "Notificação",
      "type": "Membro"
    },
    {
      "id_user": ObjectId("56807d83e15bcf96ff94afca"),
      "notify": "Notificação",
      "type": "Membro"
    }
  ]
}

var query = {name: /projeto inserido com upsert/}
var options = {upsert: true}
db.projects.update(query, mods, options)

WriteResult({
  "nMatched": 0,
  "nUpserted": 1,
  "nModified": 0,
  "_id": ObjectId("569bc6dffea6338ac4c762c9")
})
```

## Delete - remoção

#### 1. Apague todos os projetos que não possuam tags.

```
var query = {tags: { $eq:[] }}
db.projects.remove(query)

Removed 0 record(s) in 33ms
WriteResult({
  "nRemoved": 0
})
```

#### 2. Apague todos os projetos que não possuam comentários nas atividades.

```
var projects = db.projects.find().toArray();

projects.forEach(function (project) {
  project.goals[0].activities.forEach(function (activity) {
    activity = db.activities.findOne({_id: activity.activity_id});
    if (activity.comments.length == 0) {
      db.projects.remove(project);
    };
  });
});

Removed 1 record(s) in 250ms
Removed 1 record(s) in 1ms
Removed 1 record(s) in 0ms
Removed 1 record(s) in 2ms
```

#### 3. Apague todos os projetos que não possuam atividades.

```
var projects = db.projects.find().toArray();
projects.forEach(function (project) {
  if (project.goals[0].activities.length == 0) {
      db.projects.remove(project);
  };
});
Removed 1 record(s) in 6ms
Removed 1 record(s) in 1ms
```

#### 4. Escolha 2 usuários e apague todos os projetos em que os 2 fazem parte.

```
var user1 = db.users.find()[0]
var user2 = db.users.find()[1]

var projects = db.projects.find().toArray()

projects.forEach(function (project) {
  project.members.forEach(member) {
    if (member.id_user == user1._id) {
      db.projects.remove(project);
    };
  };
});

Removed 0 record(s) in 16ms
WriteResult({
  "nRemoved": 0
})
```

#### 5. Apague todos os projetos que possuam uma determinada tag em goal.

```
var query = {'goals.tags': {$in: ['JavaScript']}}
{
  "goals.tags": {
    "$in": [
      "JavaScript"
    ]
  }
}

db.projects.remove(query);  
Removed 0 record(s) in 33ms
WriteResult({
  "nRemoved": 0
})
```

## Gerenciamento de usuários

#### 1. Crie um usuário com permissões APENAS de Leitura.

```
db.createUser(
  {
    user: "ReadUser",
    pwd: "read123",
    roles: [ { role: "read", db: "projeto_final" } ]
  }
)

Successfully added user: {
  "user": "ReadUser",
  "roles": [
    {
      "role": "read",
      "db": "projeto_final"
    }
  ]
}
```

#### 2. Crie um usuário com permissões de Escrita e Leitura.

```
db.createUser(
  {
    user: "ReadWriteUser",
    pwd: "readWrite123",
    roles: [ { role: "readWrite", db: "projeto_final" } ]
  }
)

Successfully added user: {
  "user": "ReadWriteUser",
  "roles": [
    {
      "role": "readWrite",
      "db": "projeto_final"
    }
  ]
}
```

#### 3. Adicionar o papel grantRolesToUser e revokeRole para o usuário com Escrita e Leitura.

```
db.createRole(
   {
     role: "grantRolesToUser",
     privileges: [
       { resource: { db: "projeto_final", collection: "" }, actions: [ "grantRole" ] }
     ],
     roles: []
   }
)

{
  "role": "grantRolesToUser",
  "privileges": [
    {
      "resource": {
        "db": "projeto_final",
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
       { resource: { db: "projeto_final", collection: "" }, actions: [ "revokeRole" ] }
     ],
     roles: []
   }
)

{
  "role": "revokeRole",
  "privileges": [
    {
      "resource": {
        "db": "projeto_final",
        "collection": ""
      },
      "actions": [
        "revokeRole"
      ]
    }
  ],
  "roles": [ ]
}

db.grantRolesToUser(
    "ReadWriteUser",
    [
      "grantRolesToUser", "revokeRole"
    ]
)
```

#### 4. Remover o papel grantRolesToUser para o usuário com Escrita e Leitura.

```
db.revokeRolesFromUser(
    "ReadWriteUser",
    [
      { role: "grantRolesToUser", db: "projeto_final" }
    ]
)
```

#### 5. Listar todos os usuários com seus papéis e ações.

```
db.getUsers()
[
  {
    "_id": "projeto_final.ReadUser",
    "user": "ReadUser",
    "db": "projeto_final",
    "roles": [
      {
        "role": "read",
        "db": "projeto_final"
      }
    ]
  },
  {
    "_id": "projeto_final.ReadWriteUser",
    "user": "ReadWriteUser",
    "db": "projeto_final",
    "roles": [
      {
        "role": "revokeRole",
        "db": "projeto_final"
      },
      {
        "role": "readWrite",
        "db": "projeto_final"
      }
    ]
  }
]
```


## Cluster

### Depois de criada toda sua base você deverá criar um cluster utilizando: 1 Router, 1 Config Server, 3 Shardings e 3 Replicas

#### Criando o Config Server

```
mkdir data/configdb
mongod --configsvr --port 27010
```

#### Criando o Router

```
mongos --configdb localhost:27010 --port 27011
```

#### Criando os Shards

```
mkdir data/shard1
mkdir data/shard2
mkdir data/shard3

mongod --port 27012 --dbpath data/shard1
mongod --port 27013 --dbpath data/shard2
mongod --port 27014 --dbpath data/shard3
```

#### Registrando os Shards no Router

```
MacBook-Pro-de-Lucas:data lucasmoreira$ mongo --port 27011 --host localhost
MongoDB shell version: 3.2.0
connecting to: localhost:27011/test
Mongo-Hacker 0.0.9
MacBook-Pro-de-Lucas(mongos-3.2.0)[mongos] test> sh.addShard("localhost:27012")
{
  "shardAdded": "shard0000",
  "ok": 1
}
MacBook-Pro-de-Lucas(mongos-3.2.0)[mongos] test> sh.addShard("localhost:27013")
{
  "shardAdded": "shard0001",
  "ok": 1
}
MacBook-Pro-de-Lucas(mongos-3.2.0)[mongos] test> sh.addShard("localhost:27014")
{
  "shardAdded": "shard0002",
  "ok": 1
}
```

Especificamos qual database iremos shardear:

```
sh.enableSharding("be-mean")
{
  "ok": 1
}
```

E depois especificamos qual coleção dessa database será shardeada com sh.shardCollection:

```
sh.shardCollection("be-mean.notas", {"_id" : 1})
{
  "collectionsharded": "be-mean.notas",
  "ok": 1
}
```

### Réplicas

Criação das pastas para armazenamento das réplicas:

```
mkdir data/rs1
mkdir data/rs2
mkdir data/rs3
```

Iniciando os processos do `mongod` com replSet, da seguinte forma:

```
mongod --replSet replica_set --port 27017 --dbpath data/rs1
2016-02-28T12:30:04.821-0300 I CONTROL  [initandlisten] MongoDB starting : pid=1821 port=27017 dbpath=data/rs1/ 64-bit host=MacBook-Pro-de-Lucas.local
2016-02-28T12:30:04.822-0300 I CONTROL  [initandlisten] db version v3.2.0
2016-02-28T12:30:04.822-0300 I CONTROL  [initandlisten] git version: 45d947729a0315accb6d4f15a6b06be6d9c19fe7
2016-02-28T12:30:04.822-0300 I CONTROL  [initandlisten] allocator: system
2016-02-28T12:30:04.822-0300 I CONTROL  [initandlisten] modules: none
2016-02-28T12:30:04.822-0300 I CONTROL  [initandlisten] build environment:
2016-02-28T12:30:04.822-0300 I CONTROL  [initandlisten]     distarch: x86_64
2016-02-28T12:30:04.823-0300 I CONTROL  [initandlisten]     target_arch: x86_64
2016-02-28T12:30:04.823-0300 I CONTROL  [initandlisten] options: { net: { port: 27017 }, replication: { replSet: "replica_set" }, storage: { dbPath: "data/rs1/" } }
2016-02-28T12:30:04.824-0300 I STORAGE  [initandlisten] wiredtiger_open config: create,cache_size=4G,session_max=20000,eviction=(threads_max=4),config_base=false,statistics=(fast),log=(enabled=true,archive=true,path=journal,compressor=snappy),file_manager=(close_idle_time=100000),checkpoint=(wait=60,log_size=2GB),statistics_log=(wait=0),
2016-02-28T12:30:19.207-0300 I CONTROL  [initandlisten] 
2016-02-28T12:30:19.207-0300 I CONTROL  [initandlisten] ** WARNING: soft rlimits too low. Number of files is 256, should be at least 1000
2016-02-28T12:30:30.608-0300 I REPL     [initandlisten] Did not find local voted for document at startup;  NoMatchingDocument Did not find replica set lastVote document in local.replset.election
2016-02-28T12:30:30.609-0300 I REPL     [initandlisten] Did not find local replica set configuration document at startup;  NoMatchingDocument Did not find replica set configuration document in local.system.replset
2016-02-28T12:30:31.381-0300 I NETWORK  [HostnameCanonicalizationWorker] Starting hostname canonicalization worker
2016-02-28T12:30:31.381-0300 I FTDC     [initandlisten] Initializing full-time diagnostic data capture with directory 'data/rs1/diagnostic.data'
2016-02-28T12:30:40.529-0300 I NETWORK  [initandlisten] waiting for connections on port 27017


mongod --replSet replica_set --port 27018 --dbpath data/rs2
2016-02-28T12:30:27.737-0300 I CONTROL  [initandlisten] MongoDB starting : pid=1838 port=27018 dbpath=data/rs2 64-bit host=MacBook-Pro-de-Lucas.local
2016-02-28T12:30:27.737-0300 I CONTROL  [initandlisten] db version v3.2.0
2016-02-28T12:30:27.737-0300 I CONTROL  [initandlisten] git version: 45d947729a0315accb6d4f15a6b06be6d9c19fe7
2016-02-28T12:30:27.737-0300 I CONTROL  [initandlisten] allocator: system
2016-02-28T12:30:27.737-0300 I CONTROL  [initandlisten] modules: none
2016-02-28T12:30:27.737-0300 I CONTROL  [initandlisten] build environment:
2016-02-28T12:30:27.737-0300 I CONTROL  [initandlisten]     distarch: x86_64
2016-02-28T12:30:27.737-0300 I CONTROL  [initandlisten]     target_arch: x86_64
2016-02-28T12:30:27.737-0300 I CONTROL  [initandlisten] options: { net: { port: 27018 }, replication: { replSet: "replica_set" }, storage: { dbPath: "data/rs2" } }
2016-02-28T12:30:27.738-0300 I STORAGE  [initandlisten] wiredtiger_open config: create,cache_size=4G,session_max=20000,eviction=(threads_max=4),config_base=false,statistics=(fast),log=(enabled=true,archive=true,path=journal,compressor=snappy),file_manager=(close_idle_time=100000),checkpoint=(wait=60,log_size=2GB),statistics_log=(wait=0),
2016-02-28T12:31:16.349-0300 I CONTROL  [initandlisten] 
2016-02-28T12:31:16.349-0300 I CONTROL  [initandlisten] ** WARNING: soft rlimits too low. Number of files is 256, should be at least 1000
2016-02-28T12:31:52.657-0300 I REPL     [initandlisten] Did not find local voted for document at startup;  NoMatchingDocument Did not find replica set lastVote document in local.replset.election
2016-02-28T12:31:52.657-0300 I REPL     [initandlisten] Did not find local replica set configuration document at startup;  NoMatchingDocument Did not find replica set configuration document in local.system.replset
2016-02-28T12:31:52.657-0300 I NETWORK  [HostnameCanonicalizationWorker] Starting hostname canonicalization worker
2016-02-28T12:31:52.657-0300 I FTDC     [initandlisten] Initializing full-time diagnostic data capture with directory 'data/rs2/diagnostic.data'
2016-02-28T12:31:59.829-0300 I NETWORK  [initandlisten] waiting for connections on port 27018

mongod --replSet replica_set --port 27019 --dbpath data/rs3
2016-02-28T12:31:21.174-0300 I CONTROL  [initandlisten] MongoDB starting : pid=1871 port=27019 dbpath=data/rs3 64-bit host=MacBook-Pro-de-Lucas.local
2016-02-28T12:31:21.175-0300 I CONTROL  [initandlisten] db version v3.2.0
2016-02-28T12:31:21.175-0300 I CONTROL  [initandlisten] git version: 45d947729a0315accb6d4f15a6b06be6d9c19fe7
2016-02-28T12:31:21.175-0300 I CONTROL  [initandlisten] allocator: system
2016-02-28T12:31:21.175-0300 I CONTROL  [initandlisten] modules: none
2016-02-28T12:31:21.175-0300 I CONTROL  [initandlisten] build environment:
2016-02-28T12:31:21.175-0300 I CONTROL  [initandlisten]     distarch: x86_64
2016-02-28T12:31:21.175-0300 I CONTROL  [initandlisten]     target_arch: x86_64
2016-02-28T12:31:21.176-0300 I CONTROL  [initandlisten] options: { net: { port: 27019 }, replication: { replSet: "replica_set" }, storage: { dbPath: "data/rs3" } }
2016-02-28T12:31:21.947-0300 I STORAGE  [initandlisten] wiredtiger_open config: create,cache_size=4G,session_max=20000,eviction=(threads_max=4),config_base=false,statistics=(fast),log=(enabled=true,archive=true,path=journal,compressor=snappy),file_manager=(close_idle_time=100000),checkpoint=(wait=60,log_size=2GB),statistics_log=(wait=0),
2016-02-28T12:32:01.877-0300 I CONTROL  [initandlisten] 
2016-02-28T12:32:01.877-0300 I CONTROL  [initandlisten] ** WARNING: soft rlimits too low. Number of files is 256, should be at least 1000
2016-02-28T12:32:03.809-0300 I REPL     [initandlisten] Did not find local voted for document at startup;  NoMatchingDocument Did not find replica set lastVote document in local.replset.election
2016-02-28T12:32:03.809-0300 I REPL     [initandlisten] Did not find local replica set configuration document at startup;  NoMatchingDocument Did not find replica set configuration document in local.system.replset
2016-02-28T12:32:03.809-0300 I NETWORK  [HostnameCanonicalizationWorker] Starting hostname canonicalization worker
2016-02-28T12:32:03.809-0300 I FTDC     [initandlisten] Initializing full-time diagnostic data capture with directory 'data/rs3/diagnostic.data'
2016-02-28T12:32:04.338-0300 I NETWORK  [initandlisten] waiting for connections on port 27019
```

#### Configurando e Iniciando

```
rsconf = {
   _id: "replica_set",
   members: [
    {
     _id: 0,
     host: "127.0.0.1:27017"
    }
  ]
}
rs.initiate(rsconf)

//No rs1 apareceu o seguinte:

2016-02-28T12:37:19.946-0300 I NETWORK  [initandlisten] connection accepted from 127.0.0.1:62316 #1 (1 connection now open)
2016-02-28T12:37:25.819-0300 I REPL     [conn1] replSetInitiate admin command received from client
2016-02-28T12:37:25.846-0300 I REPL     [conn1] replSetInitiate config object with 1 members parses ok
2016-02-28T12:37:25.856-0300 I REPL     [conn1] ******
2016-02-28T12:37:25.856-0300 I REPL     [conn1] creating replication oplog of size: 192MB...
2016-02-28T12:37:26.508-0300 I STORAGE  [conn1] Starting WiredTigerRecordStoreThread local.oplog.rs
2016-02-28T12:37:26.509-0300 I STORAGE  [conn1] The size storer reports that the oplog contains 0 records totaling to 0 bytes
2016-02-28T12:37:26.509-0300 I STORAGE  [conn1] Scanning the oplog to determine where to place markers for truncation
2016-02-28T12:37:27.961-0300 I REPL     [conn1] ******
2016-02-28T12:37:29.203-0300 I REPL     [ReplicationExecutor] New replica set config in use: { _id: "replica_set", version: 1, protocolVersion: 1, members: [ { _id: 0, host: "127.0.0.1:27017", arbiterOnly: false, buildIndexes: true, hidden: false, priority: 1.0, tags: {}, slaveDelay: 0, votes: 1 } ], settings: { chainingAllowed: true, heartbeatIntervalMillis: 2000, heartbeatTimeoutSecs: 10, electionTimeoutMillis: 10000, getLastErrorModes: {}, getLastErrorDefaults: { w: 1, wtimeout: 0 } } }
2016-02-28T12:37:29.203-0300 I REPL     [ReplicationExecutor] This node is 127.0.0.1:27017 in the config
2016-02-28T12:37:29.203-0300 I REPL     [ReplicationExecutor] transition to STARTUP2
2016-02-28T12:37:29.203-0300 I REPL     [conn1] Starting replication applier threads
2016-02-28T12:37:29.204-0300 I COMMAND  [conn1] command local.oplog.rs command: replSetInitiate { replSetInitiate: { _id: "replica_set", members: [ { _id: 0.0, host: "127.0.0.1:27017" } ] } } ntoreturn:1 ntoskip:0 keyUpdates:0 writeConflicts:0 numYields:0 reslen:22 locks:{ Global: { acquireCount: { r: 5, w: 3, W: 2 }, acquireWaitCount: { W: 1 }, timeAcquiringMicros: { W: 1690 } }, Database: { acquireCount: { w: 2, W: 1 } }, Metadata: { acquireCount: { w: 1 } }, oplog: { acquireCount: { w: 2 } } } protocol:op_command 3416ms
2016-02-28T12:37:29.204-0300 I REPL     [ReplicationExecutor] transition to RECOVERING
2016-02-28T12:37:29.226-0300 I REPL     [ReplicationExecutor] transition to SECONDARY
2016-02-28T12:37:29.226-0300 I REPL     [ReplicationExecutor] conducting a dry run election to see if we could be elected
2016-02-28T12:37:29.291-0300 I REPL     [ReplicationExecutor] dry election run succeeded, running for election
2016-02-28T12:37:29.984-0300 I REPL     [ReplicationExecutor] election succeeded, assuming primary role in term 1
2016-02-28T12:37:29.995-0300 I REPL     [ReplicationExecutor] transition to PRIMARY
2016-02-28T12:37:30.231-0300 I REPL     [rsSync] transition to primary complete; database writes are now permitted
```

#### Adicionando Réplicas

```
rs.add("127.0.0.1:27018")
{
  "ok": 1
}
rs.add("127.0.0.1:27019")
{
  "ok": 1
}
```

#### Status das Réplicas

```
rs.status()
{
  "set": "replica_set",
  "date": ISODate("2016-02-28T15:39:44.261Z"),
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
      "uptime": 580,
      "optime": {
        "ts": Timestamp(1456673948, 1),
        "t": NumberLong("1")
      },
      "optimeDate": ISODate("2016-02-28T15:39:08Z"),
      "electionTime": Timestamp(1456673849, 2),
      "electionDate": ISODate("2016-02-28T15:37:29Z"),
      "configVersion": 3,
      "self": true
    },
    {
      "_id": 1,
      "name": "127.0.0.1:27018",
      "health": 1,
      "state": 2,
      "stateStr": "SECONDARY",
      "uptime": 47,
      "optime": {
        "ts": Timestamp(1456673948, 1),
        "t": NumberLong("1")
      },
      "optimeDate": ISODate("2016-02-28T15:39:08Z"),
      "lastHeartbeat": ISODate("2016-02-28T15:39:42.380Z"),
      "lastHeartbeatRecv": ISODate("2016-02-28T15:39:43.379Z"),
      "pingMs": NumberLong("0"),
      "syncingTo": "127.0.0.1:27017",
      "configVersion": 3
    },
    {
      "_id": 2,
      "name": "127.0.0.1:27019",
      "health": 1,
      "state": 2,
      "stateStr": "SECONDARY",
      "uptime": 35,
      "optime": {
        "ts": Timestamp(1456673948, 1),
        "t": NumberLong("1")
      },
      "optimeDate": ISODate("2016-02-28T15:39:08Z"),
      "lastHeartbeat": ISODate("2016-02-28T15:39:42.380Z"),
      "lastHeartbeatRecv": ISODate("2016-02-28T15:39:39.438Z"),
      "pingMs": NumberLong("0"),
      "configVersion": 3
    }
  ],
  "ok": 1
}
```





