# MongoDb - Projeto Final
**Autor:** Ronaldo Aragão
**Data** Date.now() //em timestamp

## Para qual sistema você usaria o MongoDB (diferente desse)?

Não vejo sistema que não possa usar o mongo, desde que seja bem modelado.

## Qual a modelagem da sua coleção de `users`?
```js
{
		_id: ObjectId,
		name: String,
		bio: String,
		date_register: Date,
		auth: {
				username: String,
				email: String,
				password: String,
				last_access: Date,
				online: Boolean,
				disabled: Boolean,
				hash_token: String,
		},
		profile_image: String,
		background_image: String
}
```

## Qual a modelagem da sua coleção de `projects`?
```js
{
	_id: Object ID,
	name: String,
	description: String,
	date_begin: Date,
	date_dream: Date,
	date_end: Date,
	visible: Boolean,
	expired: Boolean,
	visualizable: Boolean,
	members: [{
		user_id: Object ID,
		type: String,
		notify: Boolean
	}],
	goals: [{
		name: String,
		description: String,
		date_begin: Date,
		date_dream: Date,
		date_end: Date,
		realocate: Boolean,
		expired: Boolean,
		historic: [Date],
		tags: [String],
		historic: [Date],
		activities:[{
			activity_id: Object ID,
			name: String
		}],
		comments: [{
			id_comment: Object ID,
			text: String,
			date_comment: Date,
			member: Object ID,
			file: {
				path: String,
				weight: String,
				name: String
			}
		}]	
	}],	
	tags: [String],
}
```

## Qual a modelagem da sua coleção retirada de `projects`?

Retirei a coleção `activitiers` porque, de acordo com a minha ideia do sistema, uma mesma activity pode estar em mais de uma `goal`. Como as `activitiers` possuim muitos campos, criei uma nova coleção para reaproveita-las.   

```js
{
	_id: Object ID,
	name: String,
	description: String,
	date_begin: Date,
	date_dream: Date,
	date_end: Date,
	realocate: Boolean,
	expired: Boolean,
	tags: [String],
	members: [{
		user_id: Object ID
	}],
	historic: [Date],
	comments: [{
		id_comment: Object ID,
		text: String,
		date_comment: Date,
		member: Object ID,
		file: {
			path: String,
			weight: String,
			name: String
		}
	}]
}

```

## Create - cadastro

#### 1. Cadastre 10 usuários diferentes.
```js
var usuarios = [
	{
		nome: "Ronaldo Aragão", 
		user: "ronaldoarg" },
	{
		nome: "Raphael Carmo", 
		user: "raphaelcomph" },
	{
		nome: "Rafaell Soler", 
		user: "rafasoler"},
	{
		nome: "Gabriel Leal", 
		user: "gbrlleal"},
  {
		nome: "Zeus Lopes", 
		user: "zeuslps"},
	{
		nome: "Giselle Fonseca", 
		user: "gisellefsc"},
	{
		nome: "Lene Aragão", 
		user: "lenearg"},
	{
		nome: "Marcus Botelho", 
		user: "mvcbotelho"},
	{
		nome: "Lucas de Preto", 
		user: "lucasdepreto"},
	{
		nome: "Mauany Alencar", 
		user: "mauany"}]

var listaUsuarios = []

usuarios.forEach(function(u){
		var query = {
				name: u.nome,
				bio: "Meu nome é "+u.nome,
				date_register: new Date(),
				auth: {
						username: u.user,
						email: u.user+"@gmail.com",
						password: "",
						last_access: new Date(),
						online: true,
						disabled: true,
						hash_token: "04gf65d40g65f4d0654re0t4r5",
				},
				profile_image: null,
				background_image: null
		}
		listaUsuarios.push(query)
})
db.users.insert(listaUsuarios)

Inserted 1 record(s) in 804ms
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

``` js 

//Insiro duas atividades na collection activities

atividade1 = { name: "Atividade 01", description: "Descrição da atividade 01",	date_begin: new Date(),	date_dream: new Date(),	date_end: new Date(),	realocate: false,	expired: true, tags: [],	members: [], historic: [],	comments: []}

db.activities.insert(atividade1)

Inserted 1 record(s) in 204ms
BulkWriteResult({
  "writeErrors": [ ],
  "writeConcernErrors": [ ],
  "nInserted": 2,
  "nUpserted": 0,
  "nMatched": 0,
  "nModified": 0,
  "nRemoved": 0,
  "upserted": [ ]
})

atividade2 = { name: "Atividade 02", description: "Descrição da atividade 02",	date_begin: new Date(),	date_dream: new Date(),	date_end: new Date(),	realocate: false,	expired: true, tags: [],	members: [], historic: [],	comments: []}

db.activities.insert(atividade2)

Inserted 1 record(s) in 204ms
BulkWriteResult({
  "writeErrors": [ ],
  "writeConcernErrors": [ ],
  "nInserted": 2,
  "nUpserted": 0,
  "nMatched": 0,
  "nModified": 0,
  "nRemoved": 0,
  "upserted": [ ]
})

//Crio um array com os _ids dos 10 usuarios

var usuarios = [];

var user =  ObjectId("569eb5f81f52375dd204bd8a")
usuarios.push(user)

var user = ObjectId("569eb5f81f52375dd204bd8b")
usuarios.push(user)

var user = ObjectId("569eb5f81f52375dd204bd8c")
usuarios.push(user)

var user = ObjectId("569eb5f81f52375dd204bd8d")
usuarios.push(user)

var user = ObjectId("569eb5f81f52375dd204bd8e")
usuarios.push(user)

var user = ObjectId("569eb5f81f52375dd204bd8f")
usuarios.push(user)

var user = ObjectId("569eb5f81f52375dd204bd90")
usuarios.push(user)

var user = ObjectId("569eb5f81f52375dd204bd91")
usuarios.push(user)

var user = ObjectId("569eb5f81f52375dd204bd92")
usuarios.push(user)

var user = ObjectId("569eb5f81f52375dd204bd93")
usuarios.push(user)

//Crio duas variaveis com os seus _ids e nomes das atividades

atividade1 = {
activity_id:ObjectId("56a180d12530ae6eb812a7f8"),
name: "Atividade 01" }

atividade2 = {
activity_id:ObjectId("56a180d12530ae6eb812a7f9"),
name: "Atividade 02" }

// Crio uma goal

goal = {
	name: "Goal 01",
	description: "Descrição goal 01",
	date_begin: new Date(),
	date_dream: new Date(),
	date_end: new Date(),
	realocate: false,
	expired: false,
	historic: [],
	tags: ["Mongo", "Angular", "Node"],
	activities: [atividade1, atividade2],
	historic: [],
	comments: []	
}

//Crio cinco projetos e adiciono eles na collection projects

projeto01 = {
	name: "Projeto 01",
	description: "Descrição projeto 01",
	date_begin: new Date(),
	date_dream: new Date(),
	date_end: new Date(),
	visible: true,
	expired: false,
	visualizable: false,

	members: [
		{
			user_id: usuarios[0],
			type: "Basic",
			notify: true,
		},
		{
			user_id: usuarios[1],
			type: "Basic",
			notify: true,
		},
		{
			user_id: usuarios[2],
			type: "Basic",
			notify: true,
		},
		{
			user_id: usuarios[3],
			type: "Basic",
			notify: true,
		},
		{
			user_id: usuarios[4],
			type: "Basic",
			notify: true,
		},
	],

	goals: [goal],	
	tags: ["Projeto 01", "MEAN", "Tag01"],
}

projeto02 = {
	name: "Projeto 02",
	description: "Descrição projeto 02",
	date_begin: new Date(),
	date_dream: new Date(),
	date_end: new Date(),
	visible: true,
	expired: false,
	visualizable: false,

	members: [
		{
			user_id: usuarios[5],
			type: "Basic",
			notify: true,
		},
		{
			user_id: usuarios[1],
			type: "Basic",
			notify: true,
		},
		{
			user_id: usuarios[2],
			type: "Basic",
			notify: true,
		},
		{
			user_id: usuarios[3],
			type: "Basic",
			notify: true,
		},
		{
			user_id: usuarios[4],
			type: "Basic",
			notify: true,
		},
	],

	goals: [goal],	
	tags: ["Projeto 02", "MEAN", "Tag01"],
}

projeto03 = {
	name: "Projeto 03",
	description: "Descrição projeto 03",
	date_begin: new Date(),
	date_dream: new Date(),
	date_end: new Date(),
	visible: true,
	expired: false,
	visualizable: false,

	members: [
		{
			user_id: usuarios[5],
			type: "Basic",
			notify: true,
		},
		{
			user_id: usuarios[6],
			type: "Basic",
			notify: true,
		},
		{
			user_id: usuarios[7],
			type: "Basic",
			notify: true,
		},
		{
			user_id: usuarios[8],
			type: "Basic",
			notify: true,
		},
		{
			user_id: usuarios[9],
			type: "Basic",
			notify: true,
		},
	],

	goals: [goal],	
	tags: ["Projeto 03", "MEAN", "Tag01"],
}


projeto04 = {
	name: "Projeto 04",
	description: "Descrição projeto 04",
	date_begin: new Date(),
	date_dream: new Date(),
	date_end: new Date(),
	visible: true,
	expired: false,
	visualizable: false,

	members: [
		{
			user_id: usuarios[5],
			type: "Basic",
			notify: true,
		},
		{
			user_id: usuarios[6],
			type: "Basic",
			notify: true,
		},
		{
			user_id: usuarios[4],
			type: "Basic",
			notify: true,
		},
		{
			user_id: usuarios[3],
			type: "Basic",
			notify: true,
		},
		{
			user_id: usuarios[2],
			type: "Basic",
			notify: true,
		},
	],

	goals: [goal],	
	tags: ["Projeto 04", "MEAN", "Tag02"],
}

//Tiro as activities da goal para atender o requisito da questão 

goal = {
	name: "Goal 01",
	description: "Descrição goal 01",
	date_begin: new Date(),
	date_dream: new Date(),
	date_end: new Date(),
	realocate: false,
	expired: false,
	historic: [],
	tags: ["Mongo", "Angular", "Node"],
	activities: [],
	historic: [],
	comments: []	
}

projeto05 = {
	name: "Projeto 05",
	description: "Descrição projeto 05",
	date_begin: new Date(),
	date_dream: new Date(),
	date_end: new Date(),
	visible: true,
	expired: false,
	visualizable: false,

	members: [
		{
			user_id: usuarios[3],
			type: "Basic",
			notify: true,
		},
		{
			user_id: usuarios[2],
			type: "Basic",
			notify: true,
		},
		{
			user_id: usuarios[4],
			type: "Basic",
			notify: true,
		},
		{
			user_id: usuarios[7],
			type: "Basic",
			notify: true,
		},
		{
			user_id: usuarios[8],
			type: "Basic",
			notify: true,
		},
	],

	goals: [goal],	
	tags: ["Projeto 05", "MEAN", "Tag02"],
}

db.projects.insert(projeto01)

Inserted 1 record(s) in 3ms
WriteResult({
  "nInserted": 1
})

db.projects.insert(projeto02)

Inserted 1 record(s) in 3ms
WriteResult({
  "nInserted": 1
})

db.projects.insert(projeto03)

Inserted 1 record(s) in 3ms
WriteResult({
  "nInserted": 1
})

db.projects.insert(projeto04)

Inserted 1 record(s) in 3ms
WriteResult({
  "nInserted": 1
})

db.projects.insert(projeto05)

Inserted 1 record(s) in 3ms
WriteResult({
  "nInserted": 1
})

```

## Retrieve - busca

1. Liste as informações dos membros de 1 projeto específico que deve ser buscado pelo seu nome de forma a não ligar para maiúsculas e minúsculas.

```js

var projectMembers = db.projects.findOne({name: /projeto 01/i}, {_id: 0, members: 1})

var informacoes = []

var getInfos = function(e){informacoes.push(db.users.findOne({_id: e.user_id}))}

projectMembers.members.forEach(getInfos)

informacoes

```

2. Liste todos os projetos com a tag que você escolheu para os 3 projetos em comum.
```js

db.projects.find({tags: {$in: [/tag01/i]}}, {name: 1, description: 1})

{
  "_id": ObjectId("56a52c0fdc28e48eaa59d50a"),
  "name": "Projeto 01",
  "description": "Descrição projeto 01"
}
{
  "_id": ObjectId("56a52c2fdc28e48eaa59d50b"),
  "name": "Projeto 02",
  "description": "Descrição projeto 02"
}
{
  "_id": ObjectId("56a52c33dc28e48eaa59d50c"),
  "name": "Projeto 03",
  "description": "Descrição projeto 03"
}


```
3. Liste apenas os nomes de todas as atividades para todos os projetos.
```js

db.projects.find({},{_id: 0,name: 1, 'goals.activities.name':1})

```
4. Liste todos os projetos que não possuam uma tag.

```js

db.projects.find({tags:{$not:{$in:[/tag01/i]}}})

```
5. Liste todos os usuários que não fazem parte do primeiro projeto cadastrado.
```js

var usuarios = []

var membros = db.projects.findOne({name: "Projeto 01"}, {members: 1, _id: 0})

var getUsers = function(u){usuarios.push(db.users.findOne(u.user_id, {name: 1})._id)}

membros.members.forEach(getUsers)

db.users.find({_id: {$not: {$in: usuarios}}}, {name: 1})

```

## Update - alteração

1. Adicione para todos os projetos o campo `views: 0`.
```js
 trabalho-final-mongodb> db.projects.update({},{$set:{views: 0}}, {multi:true})
Updated 5 existing record(s) in 34ms
WriteResult({
  "nMatched": 5,
  "nUpserted": 0,
  "nModified": 5
})
```

2. Adicione 1 tag diferente para cada projeto.
```js

db.projects.update({name:/projeto 01/i},{$push:{tags:"Nova tag pjt1"}})
Updated 1 existing record(s) in 17ms
WriteResult({
  "nMatched": 1,
  "nUpserted": 0,
  "nModified": 1
})

db.projects.update({name:/projeto 02/i},{$push:{tags:"Nova tag pjt2"}})
Updated 1 existing record(s) in 17ms
WriteResult({
  "nMatched": 1,
  "nUpserted": 0,
  "nModified": 1
})

db.projects.update({name:/projeto 03/i},{$push:{tags:"Nova tag pjt3"}})
Updated 1 existing record(s) in 17ms
WriteResult({
  "nMatched": 1,
  "nUpserted": 0,
  "nModified": 1
})

db.projects.update({name:/projeto 04/i},{$push:{tags:"Nova tag pjt4"}})
Updated 1 existing record(s) in 17ms
WriteResult({
  "nMatched": 1,
  "nUpserted": 0,
  "nModified": 1
})

db.projects.update({name:/projeto 05/i},{$push:{tags:"Nova tag pjt5"}})
Updated 1 existing record(s) in 17ms
WriteResult({
  "nMatched": 1,
  "nUpserted": 0,
  "nModified": 1
})


```
3. Adicione 2 membros diferentes para cada projeto.
```js

var usuarios = db.users.find({},{_id:1}).toArray()

db.projects.update({name:/projeto 01/i},{$push:{members:{user_id:usuarios[6]._id, type: "Basic", notify: true}}})
db.projects.update({name:/projeto 01/i},{$push:{members:{user_id:usuarios[5]._id, type: "Basic", notify: true}}})

db.projects.update({name:/projeto 02/i},{$push:{members:{user_id:usuarios[6]._id, type: "Basic", notify: true}}})
db.projects.update({name:/projeto 02/i},{$push:{members:{user_id:usuarios[7]._id, type: "Basic", notify: true}}})

db.projects.update({name:/projeto 03/i},{$push:{members:{user_id:usuarios[0]._id, type: "Basic", notify: true}}})
db.projects.update({name:/projeto 03/i},{$push:{members:{user_id:usuarios[1]._id, type: "Basic", notify: true}}})

db.projects.update({name:/projeto 04/i},{$push:{members:{user_id:usuarios[8]._id, type: "Basic", notify: true}}})
db.projects.update({name:/projeto 04/i},{$push:{members:{user_id:usuarios[9]._id, type: "Basic", notify: true}}})

db.projects.update({name:/projeto 05/i},{$push:{members:{user_id:usuarios[5]._id, type: "Basic", notify: true}}})
db.projects.update({name:/projeto 05/i},{$push:{members:{user_id:usuarios[1]._id, type: "Basic", notify: true}}})

```
4. Adicione 1 comentário em cada atividade, deixe apenas 1 projeto sem.
```js
db.activities.update({name:/atividade 01/i},{$push:{comments:{text:"Novo COmentario", date_comment: new Date(), member:usuarios[5]._id}}})
db.activities.update({name:/atividade 02/i},{$push:{comments:{text:"Novo COmentario", date_comment: new Date(), member:usuarios[6]._id}}})
```
5. Adicione 1 projeto inteiro com **UPSERT**.
```js

var query = {name: "Projeto 06"}
var mod = {
	$set: {active: true},
	$setOnInsert: {
		name: "Projeto 06",
		description: "Descrição Projeto 06",
		date_begin: new Date(),
		date_dream: new Date(),
		date_end: new Date(),
		visible: true,
		realocate: false,
		expired: false,
		visualizable: null,
		members: [{
				user_id: usuarios[0]._id,
				type: "Admin",
				notify: false
		}, {
				user_id: usuarios[2]._id,
				type: "Basic",
				notify: true
		}, {
				user_id: usuarios[4]._id,
				type: "Basic",
				notify: false
		}, {
				user_id: usuarios[6]._id,
				type: "Basic",
				notify: true
		}, {
		user_id: usuarios[8]._id,
				type: "Basic",
				notify: false
		}],
		goals: [{
			name: "Start",
			description: "Começar o projeto",
			date_begin: new Date(),
			date_dream: new Date(),
			date_end: new Date(),
			realocate: false,
			expired: false,
			historic: [],
			tags: ["TAG01", "TAG03", "TAG02"],
			activities: [{
				activity_id: ObjectId("56a180d12530ae6eb812a7f8"),
				name: "Atividade 01"
				},{
				"activity_id": ObjectId("56a180d12530ae6eb812a7f9"),
				"name": "Atividade 02"
				}
			],
			comments: [],
			}
		],
		tags: ["TAG01", "TAG03", "TAG02"]
	}
				
	var options = {upsert: true}

  db.projects.update(query, mod, options)
```
## Delete - remoção

1. Apague todos os projetos que não possuam *tags*.
```js
 db.projects.remove({tags: {$size: 0}})
 Removed 0 record(s) in 20ms
WriteResult({
  "nRemoved": 0
})

 
```
2. Apague todos os projetos que não possuam comentários nas atividades.

Não entendi :(

3. Apague todos os projetos que não possuam atividades.
```js

db.projects.remove({'goals.activities':{$size: 0}}, {multi:1})
Removed 1 record(s) in 3ms
WriteResult({
  "nRemoved": 1
})

```

4. Escolha 2 usuário e apague todos os projetos em que os 2 fazem parte.
```js

db.projects.remove({ "members.user_id": { $in: [ usuarios[0]._id, usuarios[5]._id ]}}, { multi: 1 })
Removed 4 record(s) in 2ms
WriteResult({
  "nRemoved": 4
})


```
5. Apague todos os projetos que possuam uma determinada *tag* em *goal*.

```js

db.projects.remove({ "goals.tags": { $eq: 'Tag02' } }, { multi: 1 })
Removed 0 record(s) in 3ms
WriteResult({
  "nRemoved": 0
})

```

##Gerenciamento de usuários

1.Crie um usuário com permissões APENAS de Leitura.

```js
use admin

db.createUser({user: "ronaldoleitura",pwd: "leitura",roles: ["read"]})
Successfully added user: {
  "user": "ronaldoleitura",
  "roles": [
    "read"
  ]
}

```
2.Crie um usuário com permissões de Escrita e Leitura.
```
db.createUser({user: "ronaldolerescrever",pwd: "lerescrever",roles: ["readWrite"]})
Successfully added user: {
  "user": "ronaldoleitura",
  "roles": [
    "readWrite"
  ]
}

```
3.Adicionar o papel grantRolesToUser e revokeRole para o usuário com Escrita e Leitura.
```js

//criar roles
db.runCommand({ createRole: "grantRolesToUser",
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

db.runCommand({ createRole: "revokeRole",
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

//atribuindo as roles

db.grantRolesToUser(
  "ronaldolerescrever",
  [
      "grantRolesToUser",
      "revokeRole"
  ]
)


```
4.Remover o papel grantRolesToUser para o usuário com Escrita e Leitura.
```js
db.revokeRolesFromUser(
    "ronaldolerescrever",
    [
        "grantRolesToUser"
    ]
)
```


5.Listar todos os usuários com seus papéis e ações.
```js
 admin> db.runCommand({usersInfo: 1})
{
  "users": [
    {
      "_id": "admin.Ronaldo",
      "user": "Ronaldo",
      "db": "admin",
      "roles": [
        {
          "role": "userAdminAnyDatabase",
          "db": "admin"
        }
      ]
    },
    {
      "_id": "admin.ronaldoleitura",
      "user": "ronaldoleitura",
      "db": "admin",
      "roles": [
        {
          "role": "read",
          "db": "admin"
        }
      ]
    },
		{
      "_id": "admin.ronaldolerescrever",
      "user": "ronaldolerescrever",
      "db": "admin",
      "roles": [
        {
          "role": "readWrite",
          "db": "admin"
        }
      ]
    }
  ],
  "ok": 1
}

```


## Sharding
// coloque aqui todos os comandos que você executou

> mkdir /data/configdb
> mkdir /data/shard1
> mkdir /data/shard2
> mkdir /data/shard3

> mongod --configsvr --port 27010

> mongos --configdb localhost:27010 --port 27011

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

> mongo --port 27011 --host localhost
> sh.addShard("rs1/127.0.0.1:27012")
{
  "shardAdded": "rs1",
  "ok": 1
}
> sh.addShard("rs2/127.0.0.1:27013")
{
  "shardAdded": "rs2",
  "ok": 1
}
> sh.addShard("rs3/127.0.0.1:27014")
{
  "shardAdded": "rs3",
  "ok": 1
}
> sh.enableSharding("trabalho-final-mongodb")
{
  "ok": 1
}

> sh.shardCollection("be-mean-project.users", { "_id": 1 })
{
  "collectionsharded": "be-mean-project.users",
  "ok": 1
}

## Replica
// coloque aqui todos os comandos que você executou

> mkdir /data/rs1
> mkdir /data/rs2
> mkdir /data/rs3

> mongod --replSet rs1 --port 27015 --dbpath /data/rs1

> mongod --replSet rs2 --port 27016 --dbpath /data/rs2

> mongod --replSet rs3 --port 27018 --dbpath /data/rs3

> mongo --port 27012
> rs.add("127.0.0.1:27015")
{
  "ok": 1
}

> mongo --port 27013
> rs.add("127.0.0.1:27016")
{
  "ok": 1
}

> mongo --port 27014
> rs.add("127.0.0.1:27018")
{
  "ok": 1
}
