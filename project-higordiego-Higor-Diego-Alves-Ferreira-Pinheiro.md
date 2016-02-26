# MongoDb - Projeto Final
**Autor:** Higor Diego Alves Ferreira Pinheiro
**Data** 2016-01-21T18:46:10.786Z 

## Para qual sistema você usaria o MogoDB (diferente desse)?

Qualquer sistema que não use transaction, e que orçamento para infraestrutura não seja baixo e que o numero dos usuários seja alto.

## Qual a modelagem da sua coleção de `users`?

```js
user: {
	name: String,
	bio: String,
	date-register: Date,
	avatar-path: String,
	backgroud-path: String,
	auth:{
		username: String,
		email: String,
		password: String,
		last-access: Date,
		online: Boolean,
		disable: Boolean,
		hash-token: String

	}
}

```

## Qual a modelagem da sua coleção de `projects`?

```js
project:{
	name: String,
	description: String,
	date_begin: Date,
	date_dream: Date,
	date_end: Date,
	visible: Boolean,
	realocate: String,
	expired: Date,
	visualizable_mod: Number,

	members: [{
		member_id: ObjectID(),
		type: String,
		notify: Boolean,
	}],
	goal:[
	{
		name: String,
		description: String,
		date_begin: Date,
		date_dream: Date,
		date_end: Date,
		realocate: Number,
		expired: Date,
		activity:[
			act_id: ObjectID()
		],
		tag: String
	}
	],


}


```
## Qual a modelagem da sua coleção retirada de `projects`?
Activity. Complexidade do relacionamento e a questão de campos repetidos, assim podendo estourar o tamanho do limite do documento,
comprometendo aplicação.
```js

activity:{
	name: String,
	description: String,
	date_begin: Date,
	date_dream: Date,
	date_relocate: Date,
	expired: Date,
	date_end: Date,
	realocate: Number,
	members: [
	{
		type_member: String, 
		user_id: ObjectID(''), 
		notify: Boolean
	}
	],
	tags: [],
	comments:[
	{
		text: String, 
		Date_comment: Date, 
		files:[{path: String, weight: int, name: String}],
		members: [
		{
			type_member: String, 
			user_id: ObjectID(''), 
			notify: Boolean
		}
		]
	}
	]


}

```

## Create - cadastro


#1)
```js
var usuario = ['João', 'Maria', 'Pedro', 'Sampaio', 'Paulo', 'Francisco', 'Paulo', 'Emerson', 'Marcos', 'Roberto'];

var contador = 0;
usuario.forEach( function(usuario) {
	var user = {
		name: usuario,
		bio: contador,
		date_register: new Date(),
		avatar_path: 'Testando avatar'+contador,
		background_path: 'fundo de tela'+contador,
		auth:{
			username: usuario,
			email: usuario+'@gmail.com',
			password: usuario+'$@!'+contador,
			last_access: new Date(),
			online: false,
			disable: false,
			has_token: '90a9c6ef1ce477b68ecff635c0849a82'+contador
		}
	};
	db.user.insert(user);
	contador++;
});

```

#2)

Cadastrando activity.
```js
var ativo = ['Teste 1','teste 2'];
var contador =0;
ativo.forEach( function(index) {
	var activo = {
		name: index,
		description: 'Be-mean',
		date_begin: new Date(),
		date_dream: new Date(),
		date_relocate: new Date(),
		expired: new Date(),
		date_end: new Date(),
		realocate: contador,
		member:[],
		tags:[],
		comments:[]
	}
	db.activity.insert(activo);
	contador++;
});
```
Cadastrando o projeto.

1 Tag em dois projetos
```js
var name = ['Projeto 01','Projeto 02']
var contador = 0;
var user = db.user.find({},{id:1}).limit(4).toArray();
var ativo = db.activity.find({},{id:1}).limit(2).toArray();
name.forEach(function(name) {

	var projeto = {
		name: name,
		description: 'Descrição'+contador,
		date_betin: new Date(),
		date_dream: new Date(),
		date_end: new Date(),
		visible: false,
		realocate: contador,
		expired: new Date(),
		visualizable_mod: contador,
		members: [{
			usuario_id: user[0]._id,
			type: 'padrao',
			notify: false
		},{
			usuario_id: user[1]._id,
			type: 'padrao',
			notify: true
		},{
			usuario_id: user[2]._id,
			type: 'admin',
			notify: false
		},{
			usuario_id: user[3]._id,
			type: 'admin',
			notify: true
		}],

		tag : ['Be-mean','mongo'+contador,'nodejs'+contador],
		goal: [{
			name: 'Be-mean',
			description: 'Curso Online',
			date_begin: new Date(),
			date_dream: new Date(),
			date_end: new Date(),
			realocate: contador,
			expired: new Date(),	
			tag : ['Contar Plin','Contar Plin'+contador,'Contar Plu'+contador],
			activities: [{
				act_id: ativo[0]._id
			}, {
				act_id_id: ativo[1]._id
			}]
		}]
	}
	contador++;
	db.projeto.insert(projeto);
});
```
1 tag em 3 projetos

```js
var user = db.user.find({},{id:1}).limit(4).toArray();
var ativo = db.activity.find({},{id:1}).limit(2).toArray();
var contador = 0;
var user;
var name = ['Projeto 03','Projeto 04']
name.forEach(function(name) {

	var projeto = {
		name: name,
		description: 'Descrição'+contador,
		date_betin: new Date(),
		date_dream: new Date(),
		date_end: new Date(),
		visible: false,
		realocate: contador,
		expired: new Date(),
		visualizable_mod: contador,
		members: [{
			usuario_id: user[0]._id,
			type: 'padrao',
			notify: false
		},{
			usuario_id: user[1]._id,
			type: 'padrao',
			notify: true
		},{
			usuario_id: user[2]._id,
			type: 'admin',
			notify: false
		},{
			usuario_id: user[3]._id,
			type: 'admin',
			notify: true
		}],
		tag : ['teste','mongo','pegar'+contador],
		goal: [{
			name: 'Be-mean',
			description: 'Curso Online',
			date_begin: new Date(),
			date_dream: new Date(),
			date_end: new Date(),
			realocate: contador,
			expired: new Date(),	
			tag : ['Contar Plu','Contar Plug'+contador,'Contar Pla'+contador],
			activities: [{
				act_id: ativo[0]._id
			}, {
				act_id_id: ativo[1]._id
			}]
		}]
	}
	contador++;
	db.projeto.insert(projeto);
});

```
Sem goal

```js
var name = ['Projeto 05']
var contador = 0;
var user;
var user = db.user.find({},{id:1}).limit(4).toArray();
var ativo = db.activity.find({},{id:1}).limit(2).toArray();
name.forEach(function(name) {

	var projeto = {
		name: name,
		description: 'Descrição'+contador,
		date_betin: new Date(),
		date_dream: new Date(),
		date_end: new Date(),
		visible: false,
		realocate: contador,
		expired: new Date(),
		visualizable_mod: contador,
		members: [{
			usuario_id: user[0]._id,
			type: 'padrao',
			notify: false
		},{
			usuario_id: user[1]._id,
			type: 'padrao',
			notify: true
		},{
			usuario_id: user[2]._id,
			type: 'admin',
			notify: false
		},{
			usuario_id: user[3]._id,
			type: 'admin',
			notify: true
		}],
		tag : ['teste','mongo','pegar'+contador],
		goal: [{
			name: 'Be-mean',
			description: 'Curso Online',
			date_begin: new Date(),
			date_dream: new Date(),
			date_end: new Date(),
			realocate: contador,
			expired: new Date(),	
			tag : ['Contar Plu','Contar Plug'+contador,'Contar Pla'+contador],
			activity:[]
		}]
	}
	contador++;
	db.projeto.insert(projeto);
});

```

## Retrieve - busca

#1)

```js
var usuarios = []
var getUsuario = function(usuario){ 
	usuarios.push(db.user.findOne({_id: usuario.usuario_id})) 
}
var lista = db.projeto.findOne(query,find);
lista.members.forEach(getUsuario)
lista
{
	"members": [
	{
		"usuario_id": ObjectId("569ec0ad52f2b0caa0a42ce3"),
		"type": "padrao",
		"notify": false
	},
	{
		"usuario_id": ObjectId("569ec0ad52f2b0caa0a42ce4"),
		"type": "padrao",
		"notify": true
	},
	{
		"usuario_id": ObjectId("569ec0ad52f2b0caa0a42ce5"),
		"type": "admin",
		"notify": false
	},
	{
		"usuario_id": ObjectId("569ec0ad52f2b0caa0a42ce6"),
		"type": "admin",
		"notify": true
	}
	]
}

```
#2)
```js
var query = {tag: {$in: ['mongo']}};
db.projeto.find(query);

{
	"_id": ObjectId("56a00a42f399030fbe8effaf"),
	"name": "Projeto 03",
	"description": "Descrição0",
	"date_betin": ISODate("2016-01-20T22:29:22.447Z"),
	"date_dream": ISODate("2016-01-20T22:29:22.447Z"),
	"date_end": ISODate("2016-01-20T22:29:22.447Z"),
	"visible": false,
	"realocate": 0,
	"expired": ISODate("2016-01-20T22:29:22.447Z"),
	"visualizable_mod": 0,
	"members": [
	{
		"usuario_id": ObjectId("569ec0ad52f2b0caa0a42ce3"),
		"type": "padrao",
		"notify": false
	},
	{
		"usuario_id": ObjectId("569ec0ad52f2b0caa0a42ce4"),
		"type": "padrao",
		"notify": true
	},
	{
		"usuario_id": ObjectId("569ec0ad52f2b0caa0a42ce5"),
		"type": "admin",
		"notify": false
	},
	{
		"usuario_id": ObjectId("569ec0ad52f2b0caa0a42ce6"),
		"type": "admin",
		"notify": true
	}
	],
	"tag": [
	"teste",
	"mongo",
	"pegar0"
	],
	"goal": [
	{
		"name": "Be-mean",
		"description": "Curso Online",
		"date_begin": ISODate("2016-01-20T22:29:22.448Z"),
		"date_dream": ISODate("2016-01-20T22:29:22.448Z"),
		"date_end": ISODate("2016-01-20T22:29:22.448Z"),
		"realocate": 0,
		"expired": ISODate("2016-01-20T22:29:22.448Z"),
		"tag": [
		"Contar Plu",
		"Contar Plug0",
		"Contar Pla0"
		],
		"activity": [
		ObjectId("569fc14a0125aaf90daab141"),
		ObjectId("569fc14a0125aaf90daab142")
		]
	}
	]
}
{
	"_id": ObjectId("56a00a42f399030fbe8effb0"),
	"name": "Projeto 04",
	"description": "Descrição1",
	"date_betin": ISODate("2016-01-20T22:29:22.699Z"),
	"date_dream": ISODate("2016-01-20T22:29:22.699Z"),
	"date_end": ISODate("2016-01-20T22:29:22.699Z"),
	"visible": false,
	"realocate": 1,
	"expired": ISODate("2016-01-20T22:29:22.699Z"),
	"visualizable_mod": 1,
	"members": [
	{
		"usuario_id": ObjectId("569ec0ad52f2b0caa0a42ce3"),
		"type": "padrao",
		"notify": false
	},
	{
		"usuario_id": ObjectId("569ec0ad52f2b0caa0a42ce4"),
		"type": "padrao",
		"notify": true
	},
	{
		"usuario_id": ObjectId("569ec0ad52f2b0caa0a42ce5"),
		"type": "admin",
		"notify": false
	},
	{
		"usuario_id": ObjectId("569ec0ad52f2b0caa0a42ce6"),
		"type": "admin",
		"notify": true
	}
	],
	"tag": [
	"teste",
	"mongo",
	"pegar1"
	],
	"goal": [
	{
		"name": "Be-mean",
		"description": "Curso Online",
		"date_begin": ISODate("2016-01-20T22:29:22.699Z"),
		"date_dream": ISODate("2016-01-20T22:29:22.699Z"),
		"date_end": ISODate("2016-01-20T22:29:22.699Z"),
		"realocate": 1,
		"expired": ISODate("2016-01-20T22:29:22.699Z"),
		"tag": [
		"Contar Plu",
		"Contar Plug1",
		"Contar Pla1"
		],
		"activity": [
		ObjectId("569fc14a0125aaf90daab141"),
		ObjectId("569fc14a0125aaf90daab142")
		]
	}
	]
}
{
	"_id": ObjectId("56a00a48f399030fbe8effb1"),
	"name": "Projeto 05",
	"description": "Descrição0",
	"date_betin": ISODate("2016-01-20T22:29:28.738Z"),
	"date_dream": ISODate("2016-01-20T22:29:28.738Z"),
	"date_end": ISODate("2016-01-20T22:29:28.738Z"),
	"visible": false,
	"realocate": 0,
	"expired": ISODate("2016-01-20T22:29:28.738Z"),
	"visualizable_mod": 0,
	"members": [
	{
		"usuario_id": ObjectId("569ec0ad52f2b0caa0a42ce3"),
		"type": "padrao",
		"notify": false
	},
	{
		"usuario_id": ObjectId("569ec0ad52f2b0caa0a42ce4"),
		"type": "padrao",
		"notify": true
	},
	{
		"usuario_id": ObjectId("569ec0ad52f2b0caa0a42ce5"),
		"type": "admin",
		"notify": false
	},
	{
		"usuario_id": ObjectId("569ec0ad52f2b0caa0a42ce6"),
		"type": "admin",
		"notify": true
	}
	],
	"tag": [
	"teste",
	"mongo",
	"pegar0"
	],
	"goal": [
	{
		"name": "Be-mean",
		"description": "Curso Online",
		"date_begin": ISODate("2016-01-20T22:29:28.739Z"),
		"date_dream": ISODate("2016-01-20T22:29:28.739Z"),
		"date_end": ISODate("2016-01-20T22:29:28.739Z"),
		"realocate": 0,
		"expired": ISODate("2016-01-20T22:29:28.739Z"),
		"tag": [
		"Contar Plu",
		"Contar Plug0",
		"Contar Pla0"
		],
		"activity": [ ]
	}
	]
}


```

#3)
```js
var query  = { 
	$and : [
	{ "goal.activities": { $exists : true         } },
	{ "goal.activities": { $not    : { $eq : [] } } }
	]
};
var find = {"goal.activities.name":1};
db.projeto.find(query, find);

{
	"_id": ObjectId("56a01962f399030fbe8effc0"),
	"goal": [
	{
		"activities": [
		{

		},
		{

		}
		]
	}
	]
}
{
	"_id": ObjectId("56a01962f399030fbe8effc1"),
	"goal": [
	{
		"activities": [
		{

		},
		{

		}
		]
	}
	]
}
{
	"_id": ObjectId("56a01969f399030fbe8effc2"),
	"goal": [
	{
		"activities": [
		{

		},
		{

		}
		]
	}
	]
}
{
	"_id": ObjectId("56a01969f399030fbe8effc3"),
	"goal": [
	{
		"activities": [
		{

		},
		{

		}
		]
	}
	]
}


```

#4)

```js
var query = {tag: 'black'}
db.projeto.find(query);
```

#5)

```js
var members = [];

function getId(member) {
members.push(member.usuario_id);
};

var query = {name: /projeto/i};
var projeto = db.projeto.findOne(query).members.forEach(getId);

db.user.find({_id: {$not: {$in: members}}},{name:1})

{
	"_id": ObjectId("569ec0ad52f2b0caa0a42ce7"),
	"name": "Paulo"
}
{
	"_id": ObjectId("569ec0ad52f2b0caa0a42ce8"),
	"name": "Francisco"
}
{
	"_id": ObjectId("569ec0ad52f2b0caa0a42ce9"),
	"name": "Paulo"
}
{
	"_id": ObjectId("569ec0ad52f2b0caa0a42cea"),
	"name": "Emerson"
}
{
	"_id": ObjectId("569ec0ad52f2b0caa0a42ceb"),
	"name": "Marcos"
}
{
	"_id": ObjectId("569ec0ad52f2b0caa0a42cec"),
	"name": "Roberto"
}

```
## Update - alteração

#1)

```js
var query  = {};
var find   = {$set: {view: 0}};
var opc    = {multi: true}
db.projeto.update(query,find,opc)

Updated 5 existing record(s) in 4ms
WriteResult({
	"nMatched": 5,
	"nUpserted": 0,
	"nModified": 5
})

```
#2)

```js
var recebe = db.projeto.find({},{_id:1}).toArray();
var contador = 0;
recebe.forEach( function(recebe) {
	var find   = {$push : {tag : "testando"+contador}};
	var query  = {_id: recebe._id};
	db.projeto.update(query,find);   
	contador++;
});

```
#3)

```js
var members = [];
function getId(member) {
	members.push(member.usuario_id);
};
var query = {name: /projeto 01/i};
var projeto = db.projeto.findOne(query).members.forEach(getId);
var user = db.user.find({_id: {$not: {$in: members}}},{_id:1}).sort({_id:1}).toArray();
var recebe = db.projeto.find({},{_id:1}).toArray();

for (var i = 0; i < 6; i++) {
	if(i ==2){
		var query = {_id: recebe[1]._id};
		var find  = {$push: {members: user[i]._id}};
		db.projeto.update(query,find);    
	}else if(i ==4){
		var query = {_id: recebe[2]._id};
		var find  = {$push: {members: user[i]._id}};
		db.projeto.update(query,find);    
	}else{
		var query = {_id: recebe[3]._id};
		var find  = {$push: {members: user[i]._id}};
		db.projeto.update(query,find);    
	} 

}
```

#4)
```js
var comment = {comments: 'Comentario 01' };
db.activity.update({},{$set: {comment: comment}});
Updated 1 existing record(s) in 4ms
WriteResult({
	"nMatched": 1,
	"nUpserted": 0,
	"nModified": 1
})
```
#5)
```js
var contador = 0;
var user = db.user.find({},{_id:1}).toArray();


var query = {name: /projeto01/i};
var inserir = {
	$set: {active: true},
	$setOnInsert: {
		name: 'Testando',
		description: 'Descrição',
		date_betin: new Date(),
		date_dream: new Date(),
		date_end: new Date(),
		visible: false,
		realocate: contador,
		expired: new Date(),
		visualizable_mod: contador,
		members: [{
			usuario_id: user[0]._id,
			type: 'padrao',
			notify: false
		},{
			usuario_id: user[1]._id,
			type: 'padrao',
			notify: true
		},{
			usuario_id: user[2]._id,
			type: 'admin',
			notify: false
		},{
			usuario_id: user[3]._id,
			type: 'admin',
			notify: true
		}],
		tags: ["mongo", "Nodejs", "Angularjs"],
		goal: [{
			name: 'Be-mean',
			description: 'Curso Online',
			date_begin: new Date(),
			date_dream: new Date(),
			date_end: new Date(),
			realocate: contador,
			expired: new Date(),    
			tag : ['Contar Plin','Contar Plin','Contar Plu'],
			activities: []
		}]

	}
}
var opcao = {upsert: true};

db.projects.update(query, inserir, opcao);
```

## Delete - remoção

#1)

```js
db.projects.remove({tags: {$size: 0}})
```

#2)
```js
var query = var query = {$or: [{comment: {$exists: 0}}, {comment: {$eq: [ ]}}]};
var activity = db.activity.find(query, {_id: 1})

var id_act = []

activity.forEach(function(cur){
id_act.push(cur._id)    
})

db.projeto.remove({'goals.activity.act_id' : { $in : id_act } }, {multi:1})

db.activity.remove({'_id' : { $in : id_act}}, {multi:1})
```

#3)


```js
var query = {'goal.activity':{$size:0}}
db.projeto.remove(query);

```

#4)

```js
var userId = []

db.user.find({},{_id:1}).limit(2).forEach(function(user){
    userId.push(user._id);
});

db.projeto.remove({'members.usuario_id': {$in: userId}},{multi:1});
```

#5)

```js

db.projeto.remove({'goal.tag': /contar plin/i});

```


## Gerencia de Usuário

#1)

```js

db.createUser({user: "Higor",pwd: "abc",roles: [ "read" ]})

```

#2)

```js

db.createUser({user: "Higor",pwd: "abc",roles: [ "readWrite" ]})

```

#3)

```js
db.runCommand({ createRole: "grantRolesToUser",privileges: [
    { resource: { db: "higor", collection: "" }, actions: [ "grantRole" ] },
  ],
  roles: [
      "readWrite"
  ]
})

db.runCommand({ createRole: "revokeRole",
  privileges: [
    { resource: { db: "higor", collection: "" }, actions: [ "revokeRole" ] },
  ],
  roles: [
      "readWrite"
  ]
})




```

#4)
```JS
db.grantRolesToUser(
  "higor",
  [
      "grantRolesToUser",
      "revokeRole"
  ]
)

```
#5)
```JS

db.runCommand({usersInfo: 1})

```

## Sharding

```js
mkdir /data/configdb
mongod --configsvr --port 27010
mongos --configdb localhost:27010 --port 27011
mkdir /data/shard1
mkdir /data/shard2
mkdir /data/shard3

mongod --port 27012 --dbpath /data/shard1 
mongod --port 27013 --dbpath /data/shard2
mongod --port 27014 --dbpath /data/shard3

mongo --port 27011 --host localhost

sh.addShard("localhost:27012")
sh.addShard("localhost:27013")
sh.addShard("localhost:27014")

 sh.enableSharding("projeto");

```js

## Replica
```js
mkdir /data/rs1
mkdir /data/rs2
mkdir /data/rs3

#!/bin/bash
mongod --replSet projeto_final --port 27018 --dbpath /data/rs1 --logpath /data/rs1/log.txt --fork
mongod --replSet projeto_final --port 27019 --dbpath /data/rs2 --logpath /data/rs2/log.txt --fork
mongod --replSet projeto_final --port 27020 --dbpath /data/rs3 --logpath /data/rs3/log.txt --fork

mongo --port 27018
rsconf = {
    _id: "projeto_final",
    members:[
    {
        _id:0,
        host:"127.0.0.1:27018"
    }
    ]
}

rs.initiate(rsconf);

rs.add("127.0.0.1:27019");
rs.add("127.0.0.1:27020");


```

##Detalhando o activity/Projeto

```js
activity:{
	name: String,
	description: String,
	date_begin: Date,
	date_dream: Date,
	date_relocate: Date,
	expired: Date,
	date_end: Date,
	realocate: Number,
	members: [
	{
		type_member: String, 
		user_id: ObjectID(''), 
		notify: Boolean
	}
	],
	tags: [],
	comments:[
	{
		text: String, 
		Date_comment: Date, 
		files:[{path: String, weight: int, name: String}],
		members: [
		{
			type_member: String, 
			user_id: ObjectID(''), 
			notify: Boolean
		}
		]
	}
	]


}

```
##Projeto

```js
project:{
	name: String,
	description: String,
	date_begin: Date,
	date_dream: Date,
	date_end: Date,
	visible: Boolean,
	realocate: String,
	expired: Date,
	visualizable_mod: Number,

	members: [{
		member_id: ObjectID(),
		type: String,
		notify: Boolean,
	}],
	goal:[
	{
		name: String,
		description: String,
		date_begin: Date,
		date_dream: Date,
		date_end: Date,
		realocate: Number,
		expired: Date,
		//Activity aqui o id dele está nomeado como act_id
		activity:[
			act_id: ObjectID()
		],
		tag: String
	}
	],


}

```
