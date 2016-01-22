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
		activities:[],
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

**Parei aqui**

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

atividade = { name: "Atividade 01", description: "Descrição da atividade 01",	date_begin: new Date(),	date_dream: new Date(),	date_end: new Date(),	realocate: false,	expired: true, tags: [],	members: [], historic: [],	comments: []}

db.activities.insert(atividade)

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

atividade = { name: "Atividade 02", description: "Descrição da atividade 02",	date_begin: new Date(),	date_dream: new Date(),	date_end: new Date(),	realocate: false,	expired: true, tags: [],	members: [], historic: [],	comments: []}

db.activities.insert(atividade)

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

//Crio duas variaveis com os seus _ids

atividade1 = ObjectId("56a180d12530ae6eb812a7f8")
atividade2 = ObjectId("56a180d12530ae6eb812a7f9")

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
	tags: ["Projeto 01", "MEAN"],
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
	tags: ["Projeto 02", "MEAN"],
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
	tags: ["Projeto 03", "MEAN"],
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
	tags: ["Projeto 04", "MEAN"],
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
	tags: ["Projeto 05", "MEAN"],
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

## Update - alteração

## Delete - remoção

## Sharding
// coloque aqui todos os comandos que você executou

## Replica
// coloque aqui todos os comandos que você executou
