# MongoDb - Projeto Final
**Autor:** Matheus Jose Krumenauer Weber
**Data:** 1452369657536

## Para qual sistema você usaria o MongoDB (diferente desse)?
Eu utilizaria o MongoDB para sistemas que possuem muitos relacionamentos como esse que fizemos o projeto. Acredito que o mesmo seja muito mais fácil de procurar nesses relacionamentos, fica muito mais fácil localizar os membros de um projeto nesse somente buscando o nome do projeto, num banco de dados relacional teríamos que pesquisar o projeto e comparar a id desse projeto com a id de todos os membros cadastrados em uma tabela projetos_membros(exemplo) para só depois recuperar os membros, no MongoDB só precisamos recuperar o projeto que já vem com todos os membros desse.

Também utilizaria o MongoDB para sistemas que possuem um grande volume de dados por causa de seu Sharding, podendo separar esses dados em diversos servidores, assim aumentando a velocidade de meu sistema e a capacidade de dados do mesmo.

Alguns exemplos de projetos que eu utilizaria o MongoDB:

- ERP de grandes empresas.
- Imobiliárias.
- Redes Sociais.
- E-Commerces.


## Qual a modelagem da sua coleção de `users`?
```js
	user : {
		name : String,
		bio : String, 
		date-register : String,
		avatar-path : String,
		auth : {
			username : String,
			email : String,
			password : String,
			last_access : Date,
			online : Boolean,
			disabled : Boolean,
			hash_token : String
		}
	}
```
## Qual a modelagem da sua coleção de `projects`?
```js
	project : {
		name : String,
		description : String,
		date_begin : Date,
		date_dream : Date,
		realocate : Boolean,
		expired : Boolean,

		goals : [{
			name : String,
			description : String,
			date_begin : Date,
			date_end : Date,
			realocate : Boolean,
			expired : Boolean,
			date_realocate : [Date]
			tags : [],
			activities : [{
				activity_id : ObjectId(),
				name : String
			}]
		}],

		tags : [],

		members : [{
			user_id : ObjectId(),
			type_name : String,
			notify : String,
			name : String
		}],

	}
```	

## Qual a modelagem da sua coleção retirada de `projects`?
```js
	activity : {
		name : String,
		description : String,
		date_begin : Date,
		date_dream : Date,
		realocate : Boolean,
		expired : Boolean,
		tags : [],
		date_realocate : [Date],

		members : [{
			user_id : ObjectId(),
			type_name : String,
			notify : String,
			name : String
		}],

		comments : [{
			text : String,
			date_comment : Date,
			files : [{
				path : String,
				weight : int,
				name : String
			}]
		}]
	}
```

## Create - cadastro
```
var users = [{'name':'Matheus', 'bio' : 'Sou o primeiro usuario.', 'date-register' : new Date(2000,1,1), 'avatar-path' : 'image/matheus.jpg', auth : {username : 'matheus@gmail.com', 'password' : '12345', 'last_access' : new Date(), 'online' : true, 'disabled' : false, 'hash_token' : '8cb2237d0679ca88db6464eac60da96345513964'}},

{'name':'Eduardo', 'bio' : 'Sou o segundo usuario.', 'date-register' : new Date(2002,4,1), 'avatar-path' : 'image/eduardo.jpg', auth : {username : 'eduardo@gmail.com', 'password' : '12345', 'last_access' : new Date(), 'online' : false, 'disabled' : false, 'hash_token' : '284346dc3a552cb8035949c8ba999521225b3b1a'}},

{'name':'Leticia', 'bio' : 'Sou a terceira usuaria.', 'date-register' : new Date(2007,1,12), 'avatar-path' : 'image/leticia.jpg', auth : {username : 'leticia@gmail.com', 'password' : '12345', 'last_access' : new Date(), 'online' : true, 'disabled' : false, 'hash_token' : '21225b3b1aca88db6464eac60da96345513964'}},

{'name':'Jamila', 'bio' : 'Sou a quarta usuaria.', 'date-register' : new Date(2010,10,1), 'avatar-path' : 'image/jamila.jpg', auth : {username : 'jamila@gmail.com', 'password' : '12345', 'last_access' : new Date(), 'online' : true, 'disabled' : false, 'hash_token' : 'b8035949c8ba9998db6464eac60da96345513964'}},

{'name':'Guilherme', 'bio' : 'Sou o quinto usuario.', 'date-register' : new Date(1999,12,11), 'avatar-path' : 'image/guilherme.jpg', auth : {username : 'guilherme@gmail.com', 'password' : '12345', 'last_access' : new Date(), 'online' : true, 'disabled' : false, 'hash_token' : '8cb2237d0679ca88db6464eac60da96345513964'}},

{'name':'Alex', 'bio' : 'Sou o sexto usuario.', 'date-register' : new Date(2010,11,17), 'avatar-path' : 'image/alex.jpg', auth : {username : 'alex@gmail.com', 'password' : '12345', 'last_access' : new Date(), 'online' : false, 'disabled' : false, 'hash_token' : '8cb2237d0679ca88db6464eac60da96345513964'}},

{'name':'Thais', 'bio' : 'Sou o setimo usuario.', 'date-register' : new Date(2006,5,4), 'avatar-path' : 'image/thais.jpg', auth : {username : 'thais@gmail.com', 'password' : '12345', 'last_access' : new Date(), 'online' : true, 'disabled' : false, 'hash_token' : 'd0679ca88db6464e8db6464eac60da96345513964'}},

{'name':'Raphael', 'bio' : 'Sou o oitavo usuario.', 'date-register' : new Date(2007,3,5), 'avatar-path' : 'image/raphael.jpg', auth : {username : 'raphael@gmail.com', 'password' : '12345', 'last_access' : new Date(), 'online' : false, 'disabled' : false, 'hash_token' : '79ca88db6464eac60da96345513964'}},

{'name':'Fernanda', 'bio' : 'Sou o nono usuario.', 'date-register' : new Date(2009,2,1), 'avatar-path' : 'image/fernanda.jpg', auth : {username : 'fernanda@gmail.com', 'password' : '12345', 'last_access' : new Date(), 'online' : false, 'disabled' : false, 'hash_token' : '8cb2237d0679ca88db6464eac60da96345513964'}},

{'name':'Pedro', 'bio' : 'Sou o decimo usuario.', 'date-register' : new Date(2010,10,11), 'avatar-path' : 'image/pedro.jpg', auth : {username : 'pedro@gmail.com', 'password' : '12345', 'last_access' : new Date(), 'online' : true, 'disabled' : false, 'hash_token' : '9ca88db6464679ca88db6464eac60da96345513964'}}]

 db.user.insert(users)
BulkWriteResult({
	"writeErrors" : [ ],
	"writeConcernErrors" : [ ],
	"nInserted" : 10,
	"nUpserted" : 0,
	"nMatched" : 0,
	"nModified" : 0,
	"nRemoved" : 0,
	"upserted" : [ ]
})
```
2 - Passos:
	1 - Pego todos os usuarios e salvo em users, para usar o _id.
	var users = db.user.find()

	2 - Crio todas as activities e insiro no banco.

	```
	var activities = [{'name' : 'Primeira Atividade', 'description' : 'Fazer o commit inicial.', 'date_begin' : new Date(2016,0,9), 'date_dream' : new Date(2016,0,9), 'realocate' : false, 'expired' : false, 'tags' : ['github','projeto','commit'], 'date_realocate' : {}, 'members' : [{'user_id' : users[0]._id, 'type_name' : 'Programador', 'notify' : '', 'name' : users[0].name}], comments : [{'text' : 'Nada a comenta, só commitar', date_comment : new Date(2016,0,9), files : []}]}, 

{'name' : 'Segunda Atividade', 'description' : 'Layout das Paginas.', 'date_begin' : new Date(2016,0,10), 'date_dream' : new Date(2016,0,25), 'realocate' : false, 'expired' : false, 'tags' : ['layout','projeto','design','designer'], 'date_realocate' : {}, 'members' : [{'user_id' : users[0]._id, 'type_name' : 'Programador', 'notify' : '', 'name' : users[0].name}], comments : [{'text' : 'Fazer paginas bonitas', date_comment : new Date(2016,0,9), files : []}]},

{'name' : 'Primeira Atividade', 'description' : 'Fazer o commit inicial.', 'date_begin' : new Date(2016,0,15), 'date_dream' : new Date(2016,0,15), 'realocate' : false, 'expired' : false, 'tags' : ['github','projeto','commit'], 'date_realocate' : {}, 'members' : [{'user_id' : users[1]._id, 'type_name' : 'Programador', 'notify' : '', 'name' : users[1].name}], comments : [{'text' : 'Nada a comenta, só commitar', date_comment : new Date(2016,0,15), files : []}]}, 

{'name' : 'Segunda Atividade', 'description' : 'Programação das telas.', 'date_begin' : new Date(2016,0,15), 'date_dream' : new Date(2016,1,15), 'realocate' : false, 'expired' : false, 'tags' : ['programacao','projeto','swift','programador'], 'date_realocate' : {}, 'members' : [{'user_id' : users[1]._id, 'type_name' : 'Programador', 'notify' : '', 'name' : users[1].name}], comments : [{'text' : 'Fazer umas gambiarras loucas em swift.', date_comment : new Date(2016,0,15), files : []}]},

{'name' : 'Primeira Atividade', 'description' : 'Fazer o commit inicial.', 'date_begin' : new Date(2016,0,15), 'date_dream' : new Date(2016,0,15), 'realocate' : false, 'expired' : false, 'tags' : ['github','projeto','commit'], 'date_realocate' : {}, 'members' : [{'user_id' : users[1]._id, 'type_name' : 'Programador', 'notify' : '', 'name' : users[1].name}], comments : [{'text' : 'Nada a comenta, só commitar', date_comment : new Date(2016,0,15), files : []}]}, 

{'name' : 'Segunda Atividade', 'description' : 'Programação das telas.', 'date_begin' : new Date(2016,0,15), 'date_dream' : new Date(2016,1,15), 'realocate' : false, 'expired' : false, 'tags' : ['programacao','projeto','android','programador'], 'date_realocate' : {}, 'members' : [{'user_id' : users[2]._id, 'type_name' : 'Programador', 'notify' : '', 'name' : users[2].name}], comments : [{'text' : 'Fazer umas gambiarras loucas em java.', date_comment : new Date(2016,0,15), files : []}]},

{'name' : 'Primeira Atividade', 'description' : 'Instalar aplicativos.', 'date_begin' : new Date(2016,1,15), 'date_dream' : new Date(2016,1,15), 'realocate' : false, 'expired' : false, 'tags' : ['teste','projeto','android','ios'], 'date_realocate' : {}, 'members' : [{'user_id' : users[2]._id, 'type_name' : 'Programador', 'notify' : '', 'name' : users[2].name}], comments : [{'text' : 'Instalar os aplicativos para poder testar', date_comment : new Date(2016,0,15), files : []}]}, 

{'name' : 'Segunda Atividade', 'description' : 'Testar aplicativos.', 'date_begin' : new Date(2016,1,15), 'date_dream' : new Date(2016,1,25), 'realocate' : false, 'expired' : false, 'tags' : ['teste','projeto','android','ios'], 'date_realocate' : {}, 'members' : [{'user_id' : users[2]._id, 'type_name' : 'Programador', 'notify' : '', 'name' : users[2].name}], comments : [{'text' : 'Testar as gambiarras dos outros.', date_comment : new Date(2016,0,15), files : []}]}]

	db.activity.insert(activities)
	BulkWriteResult({
		"writeErrors" : [ ],
		"writeConcernErrors" : [ ],
		"nInserted" : 8,
		"nUpserted" : 0,
		"nMatched" : 0,
		"nModified" : 0,
		"nRemoved" : 0,
		"upserted" : [ ]
	})
```
	3 - Pego as activites do db e salvo numa variavel
	```
	var act = db.activity.find()
```
	4 - Crio os projetos e insiro no banco.
```
	var projects = [{'name' : 'Desenvolvimento do Layout', 'description' : 'Desenvolvimento de todas as paginas dos aplicativos.', 'date_begin' : new Date(2016,0,9), 'date_dream' : new Date(2016,0,25), 'realocate' : false, 'expired' : false, 'goals' : [{'name' : 'Finalizar o layout', 'description' : 'Finalizar esse layout.', 'date_begin' : new Date(2016,0,9), 'date_end' : new Date(2016,0,25), 'realocate' : false, 'date_realocate' : [], 'tags' : ['layout','design','aplicativo'], 'activities' : [{'activity_id' : act[0]._id, 'name' : act[0].name},{'activity_id' : act[1]._id, 'name' : act[1].name}]}], 'tags' : ['layout','design','eh rei'], members : [{'user_id' : users[0]._id, 'type_name' : 'Programador', 'notify' : '', 'name' : users[0].name},{'user_id' : users[1]._id, 'type_name' : 'Programador', 'notify' : '', 'name' : users[1].name},{'user_id' : users[2]._id, 'type_name' : 'Programador', 'notify' : '', 'name' : users[2].name},{'user_id' : users[3]._id, 'type_name' : 'Programador', 'notify' : '', 'name' : users[3].name},{'user_id' : users[4]._id, 'type_name' : 'Programador', 'notify' : '', 'name' : users[4].name}]},

{'name' : 'Desenvolvimento do Aplicativo para Android', 'description' : 'Desenvolvimento de toda programação para Android.', 'date_begin' : new Date(2016,0,15), 'date_dream' : new Date(2016,1,25), 'realocate' : false, 'expired' : false, 'goals' : [{'name' : 'Fazer um aplicativo', 'description' : 'Fazer funcionar.', 'date_begin' : new Date(2016,0,15), 'date_end' : new Date(2016,1,25), 'realocate' : false, 'date_realocate' : [], 'tags' : ['java','android','aplicativo'], 'activities' : [{'activity_id' : act[2]._id, 'name' : act[2].name},{'activity_id' : act[3]._id, 'name' : act[3].name}]}], 'tags' : ['java','aplicativo','android'], members : [{'user_id' : users[9]._id, 'type_name' : 'Programador', 'notify' : '', 'name' : users[9].name},{'user_id' : users[5]._id, 'type_name' : 'Programador', 'notify' : '', 'name' : users[5].name},{'user_id' : users[6]._id, 'type_name' : 'Programador', 'notify' : '', 'name' : users[6].name},{'user_id' : users[7]._id, 'type_name' : 'Programador', 'notify' : '', 'name' : users[7].name},{'user_id' : users[8]._id, 'type_name' : 'Programador', 'notify' : '', 'name' : users[8].name}]},

{'name' : 'Desenvolvimento do Aplicativo para iOS', 'description' : 'Desenvolvimento de toda programação para iOS.', 'date_begin' : new Date(2016,0,15), 'date_dream' : new Date(2016,1,25), 'realocate' : false, 'expired' : false, 'goals' : [{'name' : 'Fazer um aplicativo', 'description' : 'Fazer funcionar.', 'date_begin' : new Date(2016,0,15), 'date_end' : new Date(2016,1,25), 'realocate' : false, 'date_realocate' : [], 'tags' : ['ios','swift','aplicativo'], 'activities' : [{'activity_id' : act[4]._id, 'name' : act[4].name},{'activity_id' : act[5]._id, 'name' : act[5].name}]}], 'tags' : ['ios','aplicativo','swift'], members : [{'user_id' : users[1]._id, 'type_name' : 'Programador', 'notify' : '', 'name' : users[1].name},{'user_id' : users[2]._id, 'type_name' : 'Programador', 'notify' : '', 'name' : users[2].name},{'user_id' : users[6]._id, 'type_name' : 'Programador', 'notify' : '', 'name' : users[6].name},{'user_id' : users[4]._id, 'type_name' : 'Programador', 'notify' : '', 'name' : users[4].name},{'user_id' : users[5]._id, 'type_name' : 'Programador', 'notify' : '', 'name' : users[5].name}]},

{'name' : 'Teste dos Aplicativos', 'description' : 'Testar os Aplicativos.', 'date_begin' : new Date(2016,1,15), 'date_dream' : new Date(2016,1,25), 'realocate' : false, 'expired' : false, 'goals' : [{'name' : 'Testar os Aplicativos', 'description' : 'Testar se funciona.', 'date_begin' : new Date(2016,1,15), 'date_end' : new Date(2016,1,25), 'realocate' : false, 'date_realocate' : [], 'tags' : ['ios','swift','aplicativo','teste','java','android'], 'activities' : [{'activity_id' : act[6]._id, 'name' : act[6].name},{'activity_id' : act[7]._id, 'name' : act[7].name}]}], 'tags' : ['ios','swift','aplicativo','teste','java','android'], members : [{'user_id' : users[1]._id, 'type_name' : 'Testador', 'notify' : '', 'name' : users[1].name},{'user_id' : users[7]._id, 'type_name' : 'Programador', 'notify' : '', 'name' : users[7].name},{'user_id' : users[6]._id, 'type_name' : 'Programador', 'notify' : '', 'name' : users[6].name},{'user_id' : users[4]._id, 'type_name' : 'Testador', 'notify' : '', 'name' : users[4].name},{'user_id' : users[9]._id, 'type_name' : 'Programador', 'notify' : '', 'name' : users[9].name}]},

{'name' : 'Entrega dos Aplicativos', 'description' : 'Entregar os Aplicativos.', 'date_begin' : new Date(2016,1,25), 'date_dream' : new Date(2016,1,30), 'realocate' : false, 'expired' : false, 'goals' : [{'name' : 'Entregar os Aplicativos', 'description' : 'Torcer que o cliente aceite.', 'date_begin' : new Date(2016,1,25), 'date_end' : new Date(2016,1,30), 'realocate' : false, 'date_realocate' : [], 'tags' : ['ios','swift','aplicativo','entrega','java','android'], 'activities' : []}], 'tags' : ['ios','swift','aplicativo','teste','java','android'], members : [{'user_id' : users[1]._id, 'type_name' : 'Testador', 'notify' : '', 'name' : users[1].name},{'user_id' : users[3]._id, 'type_name' : 'Programador', 'notify' : '', 'name' : users[3].name},{'user_id' : users[6]._id, 'type_name' : 'Programador', 'notify' : '', 'name' : users[6].name},{'user_id' : users[4]._id, 'type_name' : 'Testador', 'notify' : '', 'name' : users[4].name},{'user_id' : users[9]._id, 'type_name' : 'Programador', 'notify' : '', 'name' : users[9].name}]}]

	> db.project.insert(projects)
BulkWriteResult({
	"writeErrors" : [ ],
	"writeConcernErrors" : [ ],
	"nInserted" : 5,
	"nUpserted" : 0,
	"nMatched" : 0,
	"nModified" : 0,
	"nRemoved" : 0,
	"upserted" : [ ]
})



```
## Retrieve - busca
1 - Liste as informações dos membros de 1 projeto específico que deve ser buscado pelo seu nome de forma a não ligar para maiúsculas e minúsculas.
```
	db.project.findOne({name : /teste dos aplicativos/i}).members

	[
	{
		"user_id" : ObjectId("5691678968213d5e1c422a74"),
		"type_name" : "Testador",
		"notify" : "",
		"name" : "Eduardo"
	},
	{
		"user_id" : ObjectId("5691678968213d5e1c422a7a"),
		"type_name" : "Programador",
		"notify" : "",
		"name" : "Raphael"
	},
	{
		"user_id" : ObjectId("5691678968213d5e1c422a79"),
		"type_name" : "Programador",
		"notify" : "",
		"name" : "Thais"
	},
	{
		"user_id" : ObjectId("5691678968213d5e1c422a77"),
		"type_name" : "Testador",
		"notify" : "",
		"name" : "Guilherme"
	},
	{
		"user_id" : ObjectId("5691678968213d5e1c422a7c"),
		"type_name" : "Programador",
		"notify" : "",
		"name" : "Pedro"
	}
]

```
2 - Liste todos os projetos com a tag que você escolheu para os 3 projetos em comum.

```

 db.project.find({tags : 'java'}).pretty()
{
	"_id" : ObjectId("5691781168213d5e1c422a86"),
	"name" : "Desenvolvimento do Aplicativo para Android",
	"description" : "Desenvolvimento de toda programação para Android.",
	"date_begin" : ISODate("2016-01-15T02:00:00Z"),
	"date_dream" : ISODate("2016-02-25T03:00:00Z"),
	"realocate" : false,
	"expired" : false,
	"goals" : [
		{
			"name" : "Fazer um aplicativo",
			"description" : "Fazer funcionar.",
			"date_begin" : ISODate("2016-01-15T02:00:00Z"),
			"date_end" : ISODate("2016-02-25T03:00:00Z"),
			"realocate" : false,
			"date_realocate" : [ ],
			"tags" : [
				"java",
				"android",
				"aplicativo"
			],
			"activities" : [
				{
					"activity_id" : ObjectId("5691707668213d5e1c422a7f"),
					"name" : "Primeira Atividade"
				},
				{
					"activity_id" : ObjectId("5691707668213d5e1c422a80"),
					"name" : "Segunda Atividade"
				}
			]
		}
	],
	"tags" : [
		"java",
		"aplicativo",
		"android"
	],
	"members" : [
		{
			"user_id" : ObjectId("5691678968213d5e1c422a7c"),
			"type_name" : "Programador",
			"notify" : "",
			"name" : "Pedro"
		},
		{
			"user_id" : ObjectId("5691678968213d5e1c422a78"),
			"type_name" : "Programador",
			"notify" : "",
			"name" : "Alex"
		},
		{
			"user_id" : ObjectId("5691678968213d5e1c422a79"),
			"type_name" : "Programador",
			"notify" : "",
			"name" : "Thais"
		},
		{
			"user_id" : ObjectId("5691678968213d5e1c422a7a"),
			"type_name" : "Programador",
			"notify" : "",
			"name" : "Raphael"
		},
		{
			"user_id" : ObjectId("5691678968213d5e1c422a7b"),
			"type_name" : "Programador",
			"notify" : "",
			"name" : "Fernanda"
		}
	]
}
{
	"_id" : ObjectId("5691781168213d5e1c422a89"),
	"name" : "Entrega dos Aplicativos",
	"description" : "Entregar os Aplicativos.",
	"date_begin" : ISODate("2016-02-25T03:00:00Z"),
	"date_dream" : ISODate("2016-03-01T03:00:00Z"),
	"realocate" : false,
	"expired" : false,
	"goals" : [
		{
			"name" : "Entregar os Aplicativos",
			"description" : "Torcer que o cliente aceite.",
			"date_begin" : ISODate("2016-02-25T03:00:00Z"),
			"date_end" : ISODate("2016-03-01T03:00:00Z"),
			"realocate" : false,
			"date_realocate" : [ ],
			"tags" : [
				"ios",
				"swift",
				"aplicativo",
				"entrega",
				"java",
				"android"
			],
			"activities" : [ ]
		}
	],
	"tags" : [
		"ios",
		"swift",
		"aplicativo",
		"teste",
		"java",
		"android"
	],
	"members" : [
		{
			"user_id" : ObjectId("5691678968213d5e1c422a74"),
			"type_name" : "Testador",
			"notify" : "",
			"name" : "Eduardo"
		},
		{
			"user_id" : ObjectId("5691678968213d5e1c422a76"),
			"type_name" : "Programador",
			"notify" : "",
			"name" : "Jamila"
		},
		{
			"user_id" : ObjectId("5691678968213d5e1c422a79"),
			"type_name" : "Programador",
			"notify" : "",
			"name" : "Thais"
		},
		{
			"user_id" : ObjectId("5691678968213d5e1c422a77"),
			"type_name" : "Testador",
			"notify" : "",
			"name" : "Guilherme"
		},
		{
			"user_id" : ObjectId("5691678968213d5e1c422a7c"),
			"type_name" : "Programador",
			"notify" : "",
			"name" : "Pedro"
		}
	]
}
{
	"_id" : ObjectId("5691781168213d5e1c422a88"),
	"name" : "Teste dos Aplicativos",
	"description" : "Testar os Aplicativos.",
	"date_begin" : ISODate("2016-02-15T02:00:00Z"),
	"date_dream" : ISODate("2016-02-25T03:00:00Z"),
	"realocate" : false,
	"expired" : false,
	"goals" : [
		{
			"name" : "Testar os Aplicativos",
			"description" : "Testar se funciona.",
			"date_begin" : ISODate("2016-02-15T02:00:00Z"),
			"date_end" : ISODate("2016-02-25T03:00:00Z"),
			"realocate" : false,
			"date_realocate" : [ ],
			"tags" : [
				"ios",
				"swift",
				"aplicativo",
				"teste",
				"java",
				"android"
			],
			"activities" : [
				{
					"activity_id" : ObjectId("5691707668213d5e1c422a83"),
					"name" : "Primeira Atividade"
				},
				{
					"activity_id" : ObjectId("5691707668213d5e1c422a84"),
					"name" : "Segunda Atividade"
				}
			]
		}
	],
	"tags" : [
		"ios",
		"swift",
		"aplicativo",
		"teste",
		"java",
		"android"
	],
	"members" : [
		{
			"user_id" : ObjectId("5691678968213d5e1c422a74"),
			"type_name" : "Testador",
			"notify" : "",
			"name" : "Eduardo"
		},
		{
			"user_id" : ObjectId("5691678968213d5e1c422a7a"),
			"type_name" : "Programador",
			"notify" : "",
			"name" : "Raphael"
		},
		{
			"user_id" : ObjectId("5691678968213d5e1c422a79"),
			"type_name" : "Programador",
			"notify" : "",
			"name" : "Thais"
		},
		{
			"user_id" : ObjectId("5691678968213d5e1c422a77"),
			"type_name" : "Testador",
			"notify" : "",
			"name" : "Guilherme"
		},
		{
			"user_id" : ObjectId("5691678968213d5e1c422a7c"),
			"type_name" : "Programador",
			"notify" : "",
			"name" : "Pedro"
		}
	]
}

```

3 - Liste apenas os nomes de todas as atividades para todos os projetos.

```

db.project.find({"goals.activities" : {$ne : [], $exists : true}},{"goals.activities.name":1}).pretty()
{
	"_id" : ObjectId("5691781168213d5e1c422a85"),
	"goals" : [
		{
			"activities" : [
				{
					"name" : "Primeira Atividade"
				},
				{
					"name" : "Segunda Atividade"
				}
			]
		}
	]
}
{
	"_id" : ObjectId("5691781168213d5e1c422a86"),
	"goals" : [
		{
			"activities" : [
				{
					"name" : "Primeira Atividade"
				},
				{
					"name" : "Segunda Atividade"
				}
			]
		}
	]
}
{
	"_id" : ObjectId("56919849a9a04ddd3a077a88"),
	"goals" : [
		{
			"activities" : [
				{
					"name" : "Primeira Atividade"
				},
				{
					"name" : "Segunda Atividade"
				}
			]
		}
	]
}
{
	"_id" : ObjectId("5691781168213d5e1c422a88"),
	"goals" : [
		{
			"activities" : [
				{
					"name" : "Primeira Atividade"
				},
				{
					"name" : "Segunda Atividade"
				}
			]
		}
	]
}

```

4 - Liste todos os projetos que não possuam uma tag.

```
db.project.find({"tags" : {$size : 0}})
```

5 - Liste todos os usuários que não fazem parte do primeiro projeto cadastrado.

```
db.project.find({}, {'members.name' : 1}).skip(1).pretty()
{
	"_id" : ObjectId("5691781168213d5e1c422a86"),
	"members" : [
		{
			"name" : "Pedro"
		},
		{
			"name" : "Alex"
		},
		{
			"name" : "Thais"
		},
		{
			"name" : "Raphael"
		},
		{
			"name" : "Fernanda"
		}
	]
}
{
	"_id" : ObjectId("5691781168213d5e1c422a89"),
	"members" : [
		{
			"name" : "Eduardo"
		},
		{
			"name" : "Jamila"
		},
		{
			"name" : "Thais"
		},
		{
			"name" : "Guilherme"
		},
		{
			"name" : "Pedro"
		}
	]
}
{
	"_id" : ObjectId("56919849a9a04ddd3a077a88"),
	"members" : [
		{
			"name" : "Eduardo"
		},
		{
			"name" : "Leticia"
		},
		{
			"name" : "Thais"
		},
		{
			"name" : "Guilherme"
		},
		{
			"name" : "Alex"
		}
	]
}
{
	"_id" : ObjectId("5691781168213d5e1c422a88"),
	"members" : [
		{
			"name" : "Eduardo"
		},
		{
			"name" : "Raphael"
		},
		{
			"name" : "Thais"
		},
		{
			"name" : "Guilherme"
		},
		{
			"name" : "Pedro"
		}
	]
}
```

## Update - alteração

1 - Adicione para todos os projetos o campo views: 0.

```
var mod = {$set : {"views" : 0}}
db.project.update({},mod,{multi: true})
WriteResult({ "nMatched" : 5, "nUpserted" : 0, "nModified" : 5 })
```
2 - Adicione 1 tag diferente para cada projeto.
```
var projects = db.project.find()

var query = {"_id" : projects[0]._id}
var mod = {$push : {"tags" : "Nova Tag"}}
db.project.update(query,mod)
WriteResult({ "nMatched" : 1, "nUpserted" : 0, "nModified" : 1 })

var mod = {$push : {"tags" : "Nova Tag 2"}}
var query = {"_id" : projects[1]._id}
db.project.update(query,mod)
WriteResult({ "nMatched" : 1, "nUpserted" : 0, "nModified" : 1 })

var mod = {$push : {"tags" : "Nova Tag 3"}}
var query = {"_id" : projects[2]._id}
db.project.update(query,mod)
WriteResult({ "nMatched" : 1, "nUpserted" : 0, "nModified" : 1 })

var mod = {$push : {"tags" : "Nova Tag 4"}}
var query = {"_id" : projects[3]._id}
db.project.update(query,mod)
WriteResult({ "nMatched" : 1, "nUpserted" : 0, "nModified" : 1 })

var mod = {$push : {"tags" : "Nova Tag 5"}}
var query = {"_id" : projects[4]._id}
db.project.update(query,mod)
WriteResult({ "nMatched" : 1, "nUpserted" : 0, "nModified" : 1 })
```
3 - Adicione 2 membros diferentes para cada projeto.
```
var members = db.user.find()
var mod = {$push : {"members" : [{"user_id" : members[0]._id, "type_name" : "Novo Membro", "notify" : "", "name" : members[0].name}, {"user_id" : members[1]._id, "type_name" : "Novo Membro", "notify" : "", "name" : members[1].name}]}}
var query = {"_id" : projects[4]._id}
db.project.update(query,mod)
WriteResult({ "nMatched" : 1, "nUpserted" : 0, "nModified" : 1 })

var query = {"_id" : projects[3]._id}
var mod = {$push : {"members" : [{"user_id" : members[3]._id, "type_name" : "Novo Membro", "notify" : "", "name" : members[3].name}, {"user_id" : members[2]._id, "type_name" : "Novo Membro", "notify" : "", "name" : members[2].name}]}}
db.project.update(query,mod)
WriteResult({ "nMatched" : 1, "nUpserted" : 0, "nModified" : 1 })

var query = {"_id" : projects[2]._id}
var mod = {$push : {"members" : [{"user_id" : members[5]._id, "type_name" : "Novo Membro", "notify" : "", "name" : members[5].name}, {"user_id" : members[4]._id, "type_name" : "Novo Membro", "notify" : "", "name" : members[4].name}]}}
db.project.update(query,mod)
WriteResult({ "nMatched" : 1, "nUpserted" : 0, "nModified" : 1 })

var query = {"_id" : projects[1]._id}
var mod = {$push : {"members" : [{"user_id" : members[7]._id, "type_name" : "Novo Membro", "notify" : "", "name" : members[7].name}, {"user_id" : members[6]._id, "type_name" : "Novo Membro", "notify" : "", "name" : members[6].name}]}}
db.project.update(query,mod)
WriteResult({ "nMatched" : 1, "nUpserted" : 0, "nModified" : 1 })

var query = {"_id" : projects[0]._id}
var mod = {$push : {"members" : [{"user_id" : members[9]._id, "type_name" : "Novo Membro", "notify" : "", "name" : members[9].name}, {"user_id" : members[8]._id, "type_name" : "Novo Membro", "notify" : "", "name" : members[8].name}]}}
db.project.update(query,mod)
WriteResult({ "nMatched" : 1, "nUpserted" : 0, "nModified" : 1 })
```
4 - Adicione 1 comentário em cada atividade, deixe apenas 1 projeto sem.
```
var act = db.activity.find()

var query = {"_id" : act[0]._id}
var mod = {$push : {"comments" : {'text' : 'Novo comentario', date_comment : new Date(2016,0,10)}}}
db.activity.update(query,mod)
WriteResult({ "nMatched" : 1, "nUpserted" : 0, "nModified" : 1 })

var query = {"_id" : act[1]._id}
var mod = {$push : {"comments" : {'text' : 'Novo comentario', date_comment : new Date(2016,0,10)}}}
db.activity.update(query,mod)
WriteResult({ "nMatched" : 1, "nUpserted" : 0, "nModified" : 1 })

var query = {"_id" : act[2]._id}
var mod = {$push : {"comments" : {'text' : 'Novo comentario', date_comment : new Date(2016,0,10)}}}
db.activity.update(query,mod)
WriteResult({ "nMatched" : 1, "nUpserted" : 0, "nModified" : 1 })

var query = {"_id" : act[3]._id}
var mod = {$push : {"comments" : {'text' : 'Novo comentario', date_comment : new Date(2016,0,10)}}}
db.activity.update(query,mod)
WriteResult({ "nMatched" : 1, "nUpserted" : 0, "nModified" : 1 })

var query = {"_id" : act[4]._id}
var mod = {$push : {"comments" : {'text' : 'Novo comentario', date_comment : new Date(2016,0,10)}}}
db.activity.update(query,mod)
WriteResult({ "nMatched" : 1, "nUpserted" : 0, "nModified" : 1 })
```
5 - Adicione 1 projeto inteiro com UPSERT
```
var novo_projeto = {'name' : 'Novo Desenvolvimento do Layout', 'description' : 'Desenvolvimento de todas as paginas dos aplicativos.', 'date_begin' : new Date(2016,0,9), 'date_dream' : new Date(2016,0,25), 'realocate' : false, 'expired' : false, 'goals' : [{'name' : 'Finalizar o layout', 'description' : 'Finalizar esse layout.', 'date_begin' : new Date(2016,0,9), 'date_end' : new Date(2016,0,25), 'realocate' : false, 'date_realocate' : [], 'tags' : ['layout','design','aplicativo'], 'activities' : [{'activity_id' : act[0]._id, 'name' : act[0].name},{'activity_id' : act[1]._id, 'name' : act[1].name}]}], 'tags' : ['layout','design','eh rei'], members : [{'user_id' : users[0]._id, 'type_name' : 'Programador', 'notify' : '', 'name' : users[0].name},{'user_id' : users[1]._id, 'type_name' : 'Programador', 'notify' : '', 'name' : users[1].name},{'user_id' : users[2]._id, 'type_name' : 'Programador', 'notify' : '', 'name' : users[2].name},{'user_id' : users[3]._id, 'type_name' : 'Programador', 'notify' : '', 'name' : users[3].name},{'user_id' : users[4]._id, 'type_name' : 'Programador', 'notify' : '', 'name' : users[4].name}]}
var query = {"name" : novo_projeto.name}
var opt = {upsert : true}

db.project.update(query,novo_projeto,opt)
WriteResult({
	"nMatched" : 0,
	"nUpserted" : 1,
	"nModified" : 0,
	"_id" : ObjectId("5692651ebe52edb4c28ad5bf")
})
```
## Delete - remoção
1 - Apague todos os projetos que não possuam tags.
```
db.project.remove({"tags" : {$size : 0}})
WriteResult({ "nRemoved" : 0 })
```
2 - Apague todos os projetos que não possuam comentários nas atividades.
```
a) Acho as activities sem comentarios

var act = db.activity.find({"comments" : {$size : 0}})

b) Percorro todas as activities sem comentarios entrando nos projetos delas, apos isso eu testo se tem outras activities nesse projeto com comentarios, se tiver eu seto a flag de deletar para 0 e paro o loop, se nao tiver ele termina o for e deleta o projeto.

for(i=0;i<act.count();i++){
	var projects = db.project.find({"goals.activities.activity_id" : act[i]._id});
	for(v=0;v<projects.count();v++){
		var activities = db.project.find({"_id" : projects[v]._id},{"goals.activities":1})
		del = 1
		for(z=0;z<activities.count();z++){
			var act_com_comentarios = db.activity.find({$and : [{"comments" : {$size : 0}}, {"_id" : activities[z]._id}]}).count()
			if(act_com_comentarios==0){
				del = 0;
				break;
			}
		}
		if(del==1){
			db.project.remove("_id",projects[v]._id);
		}
	}
}
```
3 - Apague todos os projetos que não possuam atividades.
```
db.project.remove({"goals.activities" : {$size : 0}})
WriteResult({ "nRemoved" : 2 })
```
4 - Escolha 2 usuário e apague todos os projetos em que os 2 fazem parte.
```
var users = db.user.find()
db.project.remove({$and : [{"members.user_id" : users[5]._id},{"members.user_id" : users[4]._id}]})
WriteResult({ "nRemoved" : 2 })
```
5 - Apague todos os projetos que possuam uma determinada tag em goal.
```
db.project.remove({"goals.tags" : {$eq : "java"}})
WriteResult({ "nRemoved" : 4 })
```

## Gerenciamento de usuários
1 - Crie um usuário com permissões APENAS de Leitura.
```
db.createUser({user : "matheus", pwd: "12345", roles : ["read"]})
Successfully added user: { "user" : "matheus", "roles" : [ "read" ] }
```
2 - Crie um usuário com permissões de Escrita e Leitura.
```
db.createUser({user : "matheus1", pwd : "12345", roles : ["readWrite"]})
Successfully added user: { "user" : "matheus1", "roles" : [ "readWrite" ] }
```
3 - Adicionar o papel grantRolesToUser e revokeRole para o usuário com Escrita e Leitura.
```
db.runCommand({ createRole: "grantRolesToUser",
  privileges: [
    { resource: { db: "admin", collection: "" }, actions: [ "readWrite" ] },
  ],
  roles: [
    { role: "readWrite", db: "be-mean-project" }
  ],
  writeConcern: { w: "majority" , wtimeout: 5000 }
})
{
  "ok": 1
}

db.runCommand({ createRole: "revokeRole",
  privileges: [
    { resource: { db: "admin", collection: "" }, actions: [ "revokeRole" ] },
  ],
  roles: [
      { role: "readWrite", db: "be-mean-project" }
  ]
})
{
  "ok": 1
}

db.grantRolesToUser("matheus",[ "grantRolesToUser" , "revokeRole" ])
```
4 - Remover o papel grantRolesToUser para o usuário com Escrita e Leitura.
```
db.runCommand( { revokeRolesFromUser: "matheus1",
                 roles: [
                          { role: "revokeRole", db: "admin" },
                          "readWrite"
                 ],
                 writeConcern: { w: "majority" }
             } )
{ "ok" : 1 }
```
5 - Listar todos os usuários com seus papéis e ações.
```
db.runCommand({usersInfo: 1})
{
	"users" : [
		{
			"_id" : "admin.matheus",
			"user" : "matheus",
			"db" : "admin",
			"roles" : [
				{
					"role" : "revokeRole",
					"db" : "admin"
				},
				{
					"role" : "grantRolesToUser",
					"db" : "admin"
				},
				{
					"role" : "read",
					"db" : "admin"
				}
			]
		},
		{
			"_id" : "admin.matheus1",
			"user" : "matheus1",
			"db" : "admin",
			"roles" : [ ]
		}
	],
	"ok" : 1
}


```
## Sharding
1 - 3 Replica
```

- Crio e inicio as replicas.

matheus@Math:/data$ mkdir r1
matheus@Math:/data$ mkdir r2
matheus@Math:/data$ mkdir r3
matheus@Math:/data$ sudo mongod --replSet replica_set --port 27030 --dbpath r1
2016-01-23T17:23:21.985-0200 I JOURNAL  [initandlisten] journal dir=r1/journal
2016-01-23T17:23:21.985-0200 I JOURNAL  [initandlisten] recover : no journal files present, no recovery needed
2016-01-23T17:23:22.460-0200 I JOURNAL  [durability] Durability thread started
2016-01-23T17:23:22.460-0200 I JOURNAL  [journal writer] Journal writer thread started
2016-01-23T17:23:22.463-0200 I CONTROL  [initandlisten] MongoDB starting : pid=6201 port=27030 dbpath=r1 64-bit host=Math
2016-01-23T17:23:22.463-0200 I CONTROL  [initandlisten] ** WARNING: You are running this process as the root user, which is not recommended.
2016-01-23T17:23:22.463-0200 I CONTROL  [initandlisten] 
2016-01-23T17:23:22.463-0200 I CONTROL  [initandlisten] 
2016-01-23T17:23:22.463-0200 I CONTROL  [initandlisten] ** WARNING: /sys/kernel/mm/transparent_hugepage/enabled is 'always'.
2016-01-23T17:23:22.463-0200 I CONTROL  [initandlisten] **        We suggest setting it to 'never'
2016-01-23T17:23:22.463-0200 I CONTROL  [initandlisten] 
2016-01-23T17:23:22.463-0200 I CONTROL  [initandlisten] ** WARNING: /sys/kernel/mm/transparent_hugepage/defrag is 'always'.
2016-01-23T17:23:22.463-0200 I CONTROL  [initandlisten] **        We suggest setting it to 'never'
2016-01-23T17:23:22.463-0200 I CONTROL  [initandlisten] 
2016-01-23T17:23:22.463-0200 I CONTROL  [initandlisten] db version v3.0.7
2016-01-23T17:23:22.463-0200 I CONTROL  [initandlisten] git version: 6ce7cbe8c6b899552dadd907604559806aa2e9bd
2016-01-23T17:23:22.463-0200 I CONTROL  [initandlisten] build info: Linux ip-10-229-88-125 3.13.0-24-generic #46-Ubuntu SMP Thu Apr 10 19:11:08 UTC 2014 x86_64 BOOST_LIB_VERSION=1_49
2016-01-23T17:23:22.463-0200 I CONTROL  [initandlisten] allocator: tcmalloc
2016-01-23T17:23:22.463-0200 I CONTROL  [initandlisten] options: { net: { port: 27030 }, replication: { replSet: "replica_set" }, storage: { dbPath: "r1" } }
2016-01-23T17:23:22.464-0200 I INDEX    [initandlisten] allocating new ns file r1/local.ns, filling with zeroes...
2016-01-23T17:23:22.776-0200 I STORAGE  [FileAllocator] allocating new datafile r1/local.0, filling with zeroes...
2016-01-23T17:23:22.776-0200 I STORAGE  [FileAllocator] creating directory r1/_tmp
2016-01-23T17:23:22.781-0200 I STORAGE  [FileAllocator] done allocating datafile r1/local.0, size: 64MB,  took 0.002 secs
2016-01-23T17:23:22.792-0200 I REPL     [initandlisten] Did not find local replica set configuration document at startup;  NoMatchingDocument Did not find replica set configuration document in local.system.replset
2016-01-23T17:23:22.792-0200 I NETWORK  [initandlisten] waiting for connections on port 27030

matheus@Math:/data$ sudo mongod --replSet replica_set --port 27031 --dbpath r2[sudo] password for matheus: 
2016-01-23T17:24:49.587-0200 I JOURNAL  [initandlisten] journal dir=r2/journal
2016-01-23T17:24:49.587-0200 I JOURNAL  [initandlisten] recover : no journal files present, no recovery needed
2016-01-23T17:24:50.117-0200 I JOURNAL  [durability] Durability thread started
2016-01-23T17:24:50.117-0200 I JOURNAL  [journal writer] Journal writer thread started
2016-01-23T17:24:50.120-0200 I CONTROL  [initandlisten] MongoDB starting : pid=6302 port=27031 dbpath=r2 64-bit host=Math
2016-01-23T17:24:50.120-0200 I CONTROL  [initandlisten] ** WARNING: You are running this process as the root user, which is not recommended.
2016-01-23T17:24:50.120-0200 I CONTROL  [initandlisten] 
2016-01-23T17:24:50.120-0200 I CONTROL  [initandlisten] 
2016-01-23T17:24:50.120-0200 I CONTROL  [initandlisten] ** WARNING: /sys/kernel/mm/transparent_hugepage/enabled is 'always'.
2016-01-23T17:24:50.120-0200 I CONTROL  [initandlisten] **        We suggest setting it to 'never'
2016-01-23T17:24:50.120-0200 I CONTROL  [initandlisten] 
2016-01-23T17:24:50.120-0200 I CONTROL  [initandlisten] ** WARNING: /sys/kernel/mm/transparent_hugepage/defrag is 'always'.
2016-01-23T17:24:50.120-0200 I CONTROL  [initandlisten] **        We suggest setting it to 'never'
2016-01-23T17:24:50.120-0200 I CONTROL  [initandlisten] 
2016-01-23T17:24:50.120-0200 I CONTROL  [initandlisten] db version v3.0.7
2016-01-23T17:24:50.120-0200 I CONTROL  [initandlisten] git version: 6ce7cbe8c6b899552dadd907604559806aa2e9bd
2016-01-23T17:24:50.120-0200 I CONTROL  [initandlisten] build info: Linux ip-10-229-88-125 3.13.0-24-generic #46-Ubuntu SMP Thu Apr 10 19:11:08 UTC 2014 x86_64 BOOST_LIB_VERSION=1_49
2016-01-23T17:24:50.120-0200 I CONTROL  [initandlisten] allocator: tcmalloc
2016-01-23T17:24:50.120-0200 I CONTROL  [initandlisten] options: { net: { port: 27031 }, replication: { replSet: "replica_set" }, storage: { dbPath: "r2" } }
2016-01-23T17:24:50.121-0200 I INDEX    [initandlisten] allocating new ns file r2/local.ns, filling with zeroes...
2016-01-23T17:24:50.483-0200 I STORAGE  [FileAllocator] allocating new datafile r2/local.0, filling with zeroes...
2016-01-23T17:24:50.483-0200 I STORAGE  [FileAllocator] creating directory r2/_tmp
2016-01-23T17:24:50.495-0200 I STORAGE  [FileAllocator] done allocating datafile r2/local.0, size: 64MB,  took 0.009 secs
2016-01-23T17:24:50.505-0200 I REPL     [initandlisten] Did not find local replica set configuration document at startup;  NoMatchingDocument Did not find replica set configuration document in local.system.replset
2016-01-23T17:24:50.506-0200 I NETWORK  [initandlisten] waiting for connections on port 27031

matheus@Math:/data$ sudo mongod --replSet replica_set --port 27032 --dbpath r3
[sudo] password for matheus: 
2016-01-23T17:25:18.934-0200 I JOURNAL  [initandlisten] journal dir=r3/journal
2016-01-23T17:25:18.934-0200 I JOURNAL  [initandlisten] recover : no journal files present, no recovery needed
2016-01-23T17:25:19.341-0200 I JOURNAL  [durability] Durability thread started
2016-01-23T17:25:19.342-0200 I JOURNAL  [journal writer] Journal writer thread started
2016-01-23T17:25:19.344-0200 I CONTROL  [initandlisten] MongoDB starting : pid=6338 port=27032 dbpath=r3 64-bit host=Math
2016-01-23T17:25:19.344-0200 I CONTROL  [initandlisten] ** WARNING: You are running this process as the root user, which is not recommended.
2016-01-23T17:25:19.344-0200 I CONTROL  [initandlisten] 
2016-01-23T17:25:19.344-0200 I CONTROL  [initandlisten] 
2016-01-23T17:25:19.344-0200 I CONTROL  [initandlisten] ** WARNING: /sys/kernel/mm/transparent_hugepage/enabled is 'always'.
2016-01-23T17:25:19.344-0200 I CONTROL  [initandlisten] **        We suggest setting it to 'never'
2016-01-23T17:25:19.344-0200 I CONTROL  [initandlisten] 
2016-01-23T17:25:19.344-0200 I CONTROL  [initandlisten] ** WARNING: /sys/kernel/mm/transparent_hugepage/defrag is 'always'.
2016-01-23T17:25:19.344-0200 I CONTROL  [initandlisten] **        We suggest setting it to 'never'
2016-01-23T17:25:19.344-0200 I CONTROL  [initandlisten] 
2016-01-23T17:25:19.344-0200 I CONTROL  [initandlisten] db version v3.0.7
2016-01-23T17:25:19.344-0200 I CONTROL  [initandlisten] git version: 6ce7cbe8c6b899552dadd907604559806aa2e9bd
2016-01-23T17:25:19.344-0200 I CONTROL  [initandlisten] build info: Linux ip-10-229-88-125 3.13.0-24-generic #46-Ubuntu SMP Thu Apr 10 19:11:08 UTC 2014 x86_64 BOOST_LIB_VERSION=1_49
2016-01-23T17:25:19.344-0200 I CONTROL  [initandlisten] allocator: tcmalloc
2016-01-23T17:25:19.344-0200 I CONTROL  [initandlisten] options: { net: { port: 27032 }, replication: { replSet: "replica_set" }, storage: { dbPath: "r3" } }
2016-01-23T17:25:19.344-0200 I INDEX    [initandlisten] allocating new ns file r3/local.ns, filling with zeroes...
2016-01-23T17:25:19.653-0200 I STORAGE  [FileAllocator] allocating new datafile r3/local.0, filling with zeroes...
2016-01-23T17:25:19.653-0200 I STORAGE  [FileAllocator] creating directory r3/_tmp
2016-01-23T17:25:19.672-0200 I STORAGE  [FileAllocator] done allocating datafile r3/local.0, size: 64MB,  took 0.016 secs
2016-01-23T17:25:19.684-0200 I REPL     [initandlisten] Did not find local replica set configuration document at startup;  NoMatchingDocument Did not find replica set configuration document in local.system.replset
2016-01-23T17:25:19.685-0200 I NETWORK  [initandlisten] waiting for connections on port 27032

-- Configuro e inicio a replica set

> rsconf = {    _id: "replica_set",    members: [     {      _id: 0,      host: "127.0.0.1:27030"     }   ] }
{
	"_id" : "replica_set",
	"members" : [
		{
			"_id" : 0,
			"host" : "127.0.0.1:27030"
		}
	]
}
> rs.initiate(rsconf)
{ "ok" : 1 }

-- Adiciona as outras replicas

replica_set:OTHER> rs.add("127.0.0.1:27031")
{ "ok" : 1 }
replica_set:PRIMARY> rs.add("127.0.0.1:27032")
{ "ok" : 1 }


-- Status

replica_set:PRIMARY> rs.status()
{
	"set" : "replica_set",
	"date" : ISODate("2016-01-23T19:30:50.821Z"),
	"myState" : 1,
	"members" : [
		{
			"_id" : 0,
			"name" : "127.0.0.1:27030",
			"health" : 1,
			"state" : 1,
			"stateStr" : "PRIMARY",
			"uptime" : 449,
			"optime" : Timestamp(1453577420, 1),
			"optimeDate" : ISODate("2016-01-23T19:30:20Z"),
			"electionTime" : Timestamp(1453577250, 2),
			"electionDate" : ISODate("2016-01-23T19:27:30Z"),
			"configVersion" : 3,
			"self" : true
		},
		{
			"_id" : 1,
			"name" : "127.0.0.1:27031",
			"health" : 1,
			"state" : 2,
			"stateStr" : "SECONDARY",
			"uptime" : 32,
			"optime" : Timestamp(1453577420, 1),
			"optimeDate" : ISODate("2016-01-23T19:30:20Z"),
			"lastHeartbeat" : ISODate("2016-01-23T19:30:50.788Z"),
			"lastHeartbeatRecv" : ISODate("2016-01-23T19:30:49.841Z"),
			"pingMs" : 0,
			"configVersion" : 3
		},
		{
			"_id" : 2,
			"name" : "127.0.0.1:27032",
			"health" : 1,
			"state" : 2,
			"stateStr" : "SECONDARY",
			"uptime" : 30,
			"optime" : Timestamp(1453577420, 1),
			"optimeDate" : ISODate("2016-01-23T19:30:20Z"),
			"lastHeartbeat" : ISODate("2016-01-23T19:30:50.788Z"),
			"lastHeartbeatRecv" : ISODate("2016-01-23T19:30:50.788Z"),
			"pingMs" : 0,
			"configVersion" : 3
		}
	],
	"ok" : 1
}




```

2 - 1 Config Server
```
matheus@Math:/data$ sudo mkdir configdb
matheus@Math:/data$ mongod --configsvr --port 27010
2016-01-23T16:54:24.694-0200 I STORAGE  [initandlisten] exception in initAndListen: 98 Unable to create/open lock file: /data/configdb/mongod.lock errno:13 Permission denied Is a mongod instance already running?, terminating
2016-01-23T16:54:24.694-0200 I CONTROL  [initandlisten] dbexit:  rc: 100
matheus@Math:/data$ sudo mongod --configsvr --port 27010
2016-01-23T16:54:33.095-0200 I JOURNAL  [initandlisten] journal dir=/data/configdb/journal
2016-01-23T16:54:33.096-0200 I JOURNAL  [initandlisten] recover : no journal files present, no recovery needed
2016-01-23T16:54:33.403-0200 I JOURNAL  [durability] Durability thread started
2016-01-23T16:54:33.404-0200 I JOURNAL  [journal writer] Journal writer thread started
2016-01-23T16:54:33.406-0200 I CONTROL  [initandlisten] MongoDB starting : pid=5509 port=27010 dbpath=/data/configdb master=1 64-bit host=Math
2016-01-23T16:54:33.406-0200 I CONTROL  [initandlisten] ** WARNING: You are running this process as the root user, which is not recommended.
2016-01-23T16:54:33.406-0200 I CONTROL  [initandlisten] 
2016-01-23T16:54:33.406-0200 I CONTROL  [initandlisten] 
2016-01-23T16:54:33.406-0200 I CONTROL  [initandlisten] ** WARNING: /sys/kernel/mm/transparent_hugepage/enabled is 'always'.
2016-01-23T16:54:33.406-0200 I CONTROL  [initandlisten] **        We suggest setting it to 'never'
2016-01-23T16:54:33.407-0200 I CONTROL  [initandlisten] 
2016-01-23T16:54:33.407-0200 I CONTROL  [initandlisten] ** WARNING: /sys/kernel/mm/transparent_hugepage/defrag is 'always'.
2016-01-23T16:54:33.407-0200 I CONTROL  [initandlisten] **        We suggest setting it to 'never'
2016-01-23T16:54:33.407-0200 I CONTROL  [initandlisten] 
2016-01-23T16:54:33.407-0200 I CONTROL  [initandlisten] db version v3.0.7
2016-01-23T16:54:33.407-0200 I CONTROL  [initandlisten] git version: 6ce7cbe8c6b899552dadd907604559806aa2e9bd
2016-01-23T16:54:33.407-0200 I CONTROL  [initandlisten] build info: Linux ip-10-229-88-125 3.13.0-24-generic #46-Ubuntu SMP Thu Apr 10 19:11:08 UTC 2014 x86_64 BOOST_LIB_VERSION=1_49
2016-01-23T16:54:33.407-0200 I CONTROL  [initandlisten] allocator: tcmalloc
2016-01-23T16:54:33.407-0200 I CONTROL  [initandlisten] options: { net: { port: 27010 }, sharding: { clusterRole: "configsvr" } }
2016-01-23T16:54:33.407-0200 I INDEX    [initandlisten] allocating new ns file /data/configdb/local.ns, filling with zeroes...
2016-01-23T16:54:33.717-0200 I STORAGE  [FileAllocator] allocating new datafile /data/configdb/local.0, filling with zeroes...
2016-01-23T16:54:33.718-0200 I STORAGE  [FileAllocator] creating directory /data/configdb/_tmp
2016-01-23T16:54:33.723-0200 I STORAGE  [FileAllocator] done allocating datafile /data/configdb/local.0, size: 16MB,  took 0.002 secs
2016-01-23T16:54:33.733-0200 I REPL     [initandlisten] ******
2016-01-23T16:54:33.734-0200 I REPL     [initandlisten] creating replication oplog of size: 5MB...
2016-01-23T16:54:33.737-0200 I REPL     [initandlisten] ******
2016-01-23T16:54:33.738-0200 I NETWORK  [initandlisten] waiting for connections on port 27010
```

3 - 1 Router
```
matheus@Math:/data$ mongos --configdb localhost:27010 --port 27011
2016-01-23T16:56:09.272-0200 W SHARDING running with 1 config server should be done only for testing purposes and is not recommended for production
2016-01-23T16:56:09.314-0200 I SHARDING [mongosMain] MongoS version 3.0.7 starting: pid=5551 port=27011 64-bit host=Math (--help for usage)
2016-01-23T16:56:09.314-0200 I CONTROL  [mongosMain] db version v3.0.7
2016-01-23T16:56:09.314-0200 I CONTROL  [mongosMain] git version: 6ce7cbe8c6b899552dadd907604559806aa2e9bd
2016-01-23T16:56:09.314-0200 I CONTROL  [mongosMain] build info: Linux ip-10-229-88-125 3.13.0-24-generic #46-Ubuntu SMP Thu Apr 10 19:11:08 UTC 2014 x86_64 BOOST_LIB_VERSION=1_49
2016-01-23T16:56:09.314-0200 I CONTROL  [mongosMain] allocator: tcmalloc
2016-01-23T16:56:09.314-0200 I CONTROL  [mongosMain] options: { net: { port: 27011 }, sharding: { configDB: "localhost:27010" } }
2016-01-23T16:56:09.321-0200 I SHARDING [LockPinger] creating distributed lock ping thread for localhost:27010 and process Math:27011:1453575369:1804289383 (sleeping for 30000ms)
2016-01-23T16:56:09.708-0200 I SHARDING [LockPinger] cluster localhost:27010 pinged successfully at Sat Jan 23 16:56:09 2016 by distributed lock pinger 'localhost:27010/Math:27011:1453575369:1804289383', sleeping for 30000ms
2016-01-23T16:56:09.708-0200 I SHARDING [mongosMain] distributed lock 'configUpgrade/Math:27011:1453575369:1804289383' acquired, ts : 56a3ccc92a2303ba5785ef52
2016-01-23T16:56:09.709-0200 I SHARDING [mongosMain] starting upgrade of config server from v0 to v6
2016-01-23T16:56:09.709-0200 I SHARDING [mongosMain] starting next upgrade step from v0 to v6
2016-01-23T16:56:09.709-0200 I SHARDING [mongosMain] about to log new metadata event: { _id: "Math-2016-01-23T18:56:09-56a3ccc92a2303ba5785ef53", server: "Math", clientAddr: "N/A", time: new Date(1453575369709), what: "starting upgrade of config database", ns: "config.version", details: { from: 0, to: 6 } }
2016-01-23T16:56:09.751-0200 I SHARDING [mongosMain] writing initial config version at v6
2016-01-23T16:56:09.767-0200 I SHARDING [mongosMain] about to log new metadata event: { _id: "Math-2016-01-23T18:56:09-56a3ccc92a2303ba5785ef55", server: "Math", clientAddr: "N/A", time: new Date(1453575369767), what: "finished upgrade of config database", ns: "config.version", details: { from: 0, to: 6 } }
2016-01-23T16:56:09.786-0200 I SHARDING [mongosMain] upgrade of config server to v6 successful
2016-01-23T16:56:09.786-0200 I SHARDING [mongosMain] distributed lock 'configUpgrade/Math:27011:1453575369:1804289383' unlocked. 
2016-01-23T16:56:09.969-0200 I SHARDING [Balancer] about to contact config servers and shards
2016-01-23T16:56:09.970-0200 I SHARDING [Balancer] config servers and shards contacted successfully
2016-01-23T16:56:09.970-0200 I SHARDING [Balancer] balancer id: Math:27011 started at Jan 23 16:56:09
2016-01-23T16:56:09.972-0200 I SHARDING [Balancer] distributed lock 'balancer/Math:27011:1453575369:1804289383' acquired, ts : 56a3ccc92a2303ba5785ef57
2016-01-23T16:56:09.991-0200 I NETWORK  [mongosMain] waiting for connections on port 27011
2016-01-23T16:56:10.001-0200 I SHARDING [Balancer] distributed lock 'balancer/Math:27011:1453575369:1804289383' unlocked. 
```

4 - 3 Shardings
```
matheus@Math:/data$ mkdir shard1
matheus@Math:/data$ mkdir shard2
matheus@Math:/data$ mkdir shard3

matheus@Math:/data$ mongod --port 27023 --dbpath shard1
2016-01-23T17:01:05.039-0200 I JOURNAL  [initandlisten] journal dir=shard1/journal
2016-01-23T17:01:05.039-0200 I JOURNAL  [initandlisten] recover : no journal files present, no recovery needed
2016-01-23T17:01:05.525-0200 I JOURNAL  [durability] Durability thread started
2016-01-23T17:01:05.526-0200 I JOURNAL  [journal writer] Journal writer thread started
2016-01-23T17:01:05.529-0200 I CONTROL  [initandlisten] MongoDB starting : pid=5746 port=27023 dbpath=shard1 64-bit host=Math
2016-01-23T17:01:05.529-0200 I CONTROL  [initandlisten] 
2016-01-23T17:01:05.529-0200 I CONTROL  [initandlisten] ** WARNING: /sys/kernel/mm/transparent_hugepage/enabled is 'always'.
2016-01-23T17:01:05.529-0200 I CONTROL  [initandlisten] **        We suggest setting it to 'never'
2016-01-23T17:01:05.529-0200 I CONTROL  [initandlisten] 
2016-01-23T17:01:05.529-0200 I CONTROL  [initandlisten] ** WARNING: /sys/kernel/mm/transparent_hugepage/defrag is 'always'.
2016-01-23T17:01:05.529-0200 I CONTROL  [initandlisten] **        We suggest setting it to 'never'
2016-01-23T17:01:05.529-0200 I CONTROL  [initandlisten] 
2016-01-23T17:01:05.529-0200 I CONTROL  [initandlisten] db version v3.0.7
2016-01-23T17:01:05.529-0200 I CONTROL  [initandlisten] git version: 6ce7cbe8c6b899552dadd907604559806aa2e9bd
2016-01-23T17:01:05.529-0200 I CONTROL  [initandlisten] build info: Linux ip-10-229-88-125 3.13.0-24-generic #46-Ubuntu SMP Thu Apr 10 19:11:08 UTC 2014 x86_64 BOOST_LIB_VERSION=1_49
2016-01-23T17:01:05.529-0200 I CONTROL  [initandlisten] allocator: tcmalloc
2016-01-23T17:01:05.529-0200 I CONTROL  [initandlisten] options: { net: { port: 27023 }, storage: { dbPath: "shard1" } }
2016-01-23T17:01:05.530-0200 I INDEX    [initandlisten] allocating new ns file shard1/local.ns, filling with zeroes...
2016-01-23T17:01:05.846-0200 I STORAGE  [FileAllocator] allocating new datafile shard1/local.0, filling with zeroes...
2016-01-23T17:01:05.846-0200 I STORAGE  [FileAllocator] creating directory shard1/_tmp
2016-01-23T17:01:05.851-0200 I STORAGE  [FileAllocator] done allocating datafile shard1/local.0, size: 64MB,  took 0.002 secs
2016-01-23T17:01:05.858-0200 I NETWORK  [initandlisten] waiting for connections on port 27023

matheus@Math:/data$ mongod --port 27024 --dbpath shard2
2016-01-23T17:01:37.551-0200 I JOURNAL  [initandlisten] journal dir=shard2/journal
2016-01-23T17:01:37.551-0200 I JOURNAL  [initandlisten] recover : no journal files present, no recovery needed
2016-01-23T17:01:37.861-0200 I JOURNAL  [durability] Durability thread started
2016-01-23T17:01:37.861-0200 I JOURNAL  [journal writer] Journal writer thread started
2016-01-23T17:01:37.864-0200 I CONTROL  [initandlisten] MongoDB starting : pid=5779 port=27024 dbpath=shard2 64-bit host=Math
2016-01-23T17:01:37.864-0200 I CONTROL  [initandlisten] 
2016-01-23T17:01:37.865-0200 I CONTROL  [initandlisten] ** WARNING: /sys/kernel/mm/transparent_hugepage/enabled is 'always'.
2016-01-23T17:01:37.865-0200 I CONTROL  [initandlisten] **        We suggest setting it to 'never'
2016-01-23T17:01:37.865-0200 I CONTROL  [initandlisten] 
2016-01-23T17:01:37.865-0200 I CONTROL  [initandlisten] ** WARNING: /sys/kernel/mm/transparent_hugepage/defrag is 'always'.
2016-01-23T17:01:37.865-0200 I CONTROL  [initandlisten] **        We suggest setting it to 'never'
2016-01-23T17:01:37.865-0200 I CONTROL  [initandlisten] 
2016-01-23T17:01:37.865-0200 I CONTROL  [initandlisten] db version v3.0.7
2016-01-23T17:01:37.865-0200 I CONTROL  [initandlisten] git version: 6ce7cbe8c6b899552dadd907604559806aa2e9bd
2016-01-23T17:01:37.865-0200 I CONTROL  [initandlisten] build info: Linux ip-10-229-88-125 3.13.0-24-generic #46-Ubuntu SMP Thu Apr 10 19:11:08 UTC 2014 x86_64 BOOST_LIB_VERSION=1_49
2016-01-23T17:01:37.865-0200 I CONTROL  [initandlisten] allocator: tcmalloc
2016-01-23T17:01:37.865-0200 I CONTROL  [initandlisten] options: { net: { port: 27024 }, storage: { dbPath: "shard2" } }
2016-01-23T17:01:37.867-0200 I INDEX    [initandlisten] allocating new ns file shard2/local.ns, filling with zeroes...
2016-01-23T17:01:38.210-0200 I STORAGE  [FileAllocator] allocating new datafile shard2/local.0, filling with zeroes...
2016-01-23T17:01:38.210-0200 I STORAGE  [FileAllocator] creating directory shard2/_tmp
2016-01-23T17:01:38.215-0200 I STORAGE  [FileAllocator] done allocating datafile shard2/local.0, size: 64MB,  took 0.002 secs
2016-01-23T17:01:38.221-0200 I NETWORK  [initandlisten] waiting for connections on port 27024

matheus@Math:/data$ mongod --port 27025 --dbpath shard3
2016-01-23T17:02:01.686-0200 I JOURNAL  [initandlisten] journal dir=shard3/journal
2016-01-23T17:02:01.686-0200 I JOURNAL  [initandlisten] recover : no journal files present, no recovery needed
2016-01-23T17:02:01.982-0200 I JOURNAL  [durability] Durability thread started
2016-01-23T17:02:01.982-0200 I JOURNAL  [journal writer] Journal writer thread started
2016-01-23T17:02:01.986-0200 I CONTROL  [initandlisten] MongoDB starting : pid=5811 port=27025 dbpath=shard3 64-bit host=Math
2016-01-23T17:02:01.986-0200 I CONTROL  [initandlisten] 
2016-01-23T17:02:01.986-0200 I CONTROL  [initandlisten] ** WARNING: /sys/kernel/mm/transparent_hugepage/enabled is 'always'.
2016-01-23T17:02:01.986-0200 I CONTROL  [initandlisten] **        We suggest setting it to 'never'
2016-01-23T17:02:01.986-0200 I CONTROL  [initandlisten] 
2016-01-23T17:02:01.986-0200 I CONTROL  [initandlisten] ** WARNING: /sys/kernel/mm/transparent_hugepage/defrag is 'always'.
2016-01-23T17:02:01.986-0200 I CONTROL  [initandlisten] **        We suggest setting it to 'never'
2016-01-23T17:02:01.986-0200 I CONTROL  [initandlisten] 
2016-01-23T17:02:01.986-0200 I CONTROL  [initandlisten] db version v3.0.7
2016-01-23T17:02:01.986-0200 I CONTROL  [initandlisten] git version: 6ce7cbe8c6b899552dadd907604559806aa2e9bd
2016-01-23T17:02:01.986-0200 I CONTROL  [initandlisten] build info: Linux ip-10-229-88-125 3.13.0-24-generic #46-Ubuntu SMP Thu Apr 10 19:11:08 UTC 2014 x86_64 BOOST_LIB_VERSION=1_49
2016-01-23T17:02:01.986-0200 I CONTROL  [initandlisten] allocator: tcmalloc
2016-01-23T17:02:01.986-0200 I CONTROL  [initandlisten] options: { net: { port: 27025 }, storage: { dbPath: "shard3" } }
2016-01-23T17:02:01.986-0200 I INDEX    [initandlisten] allocating new ns file shard3/local.ns, filling with zeroes...
2016-01-23T17:02:02.339-0200 I STORAGE  [FileAllocator] allocating new datafile shard3/local.0, filling with zeroes...
2016-01-23T17:02:02.339-0200 I STORAGE  [FileAllocator] creating directory shard3/_tmp
2016-01-23T17:02:02.344-0200 I STORAGE  [FileAllocator] done allocating datafile shard3/local.0, size: 64MB,  took 0.002 secs
2016-01-23T17:02:02.355-0200 I NETWORK  [initandlisten] waiting for connections on port 27025

matheus@Math:~$ mongo --port 27011 --host localhost
MongoDB shell version: 3.0.7
connecting to: localhost:27011/test
mongos> sh.addShard("localhost:27023")
{ "shardAdded" : "shard0000", "ok" : 1 }
mongos> sh.addShard("localhost:27024")
{ "shardAdded" : "shard0001", "ok" : 1 }
mongos> sh.addShard("localhost:27025")
{ "shardAdded" : "shard0002", "ok" : 1 }

mongos> sh.enableSharding("be-mean-project")
{ "ok" : 1 }

```













