# MongoDb - Projeto Final
**Autor:** Mateus Cerezini Gomes
**Data** 1456110734

## Para qual sistema você usaria o MogoDB (diferente desse)?
- Jogos
- E-commerce
- Chat
- Grandes sistemas comerciais
- Qualquer sistema robusto que demanda alta escalabilidade e alta disponibilidade.

## Qual a modelagem da sua coleção de `users`?
```js
user = {
	_id: <ObjectId>,
	name: <String>,
	bio: <String>,
	date_register: <Date>,
	username: <String>,
	email: <String>,
	password: <String>,
	last_access: <Date>,
	online: <Boolean>,
	disabled: <Boolean>,
	hash_token: <String>,
	avatar_path: <Binary>,
	background_path: <Binary>
}
```

## Qual a modelagem da sua coleção de `projects`?
```js
project = {
	_id: <ObjectId>,
	name: <String>,
	description: <String>,
	date_begin: <Date>,
	date_dream: <Date>,
	date_end: <Date>,
	visible: <Boolean>,
	realocate: <Boolean>,
	expired: <Boolean>,
	visualizable_mod: <Value>

	members: [{
		user_id: <ObjectId>,
		types: [<String>],
		notify: <Boolean>
	}]

	tags: [<String>],

	goals: [{
		name: <String>,
		description: <String>,
		date_begin: <Date>,
		date_dream: <Date>,
		date_end: <Date>,
		realocate: <Boolean>,
		expired: <Boolean>

		realocate_historic: [<Date>],

		tags: [<String>]

		activities: [<ObjectId>]
	}]
}
```

## Qual a modelagem da sua coleção retirada de `projects`?
```js
activities = {
	_id: <ObjectId>,
	name: <String>,
	description: <String>,
	date_begin: <Date>,
	date_dream: <Date>,
	date_end: <Date>,
	realocate: <Boolean>,
	expired: <Boolean>,

	members: [{
		user_id: <ObjectId>,
		types: [<String>],
		notify: <Boolean>
	}],	

	tags: [<String>],

	realocate_historic: [<Date>],

	comments: [{
		text: <String>,
		data_comment: <Date>,
		
		members: [{
			user_id: <ObjectId>,
			types: [<String>],
			notify: <Boolean>
		}],	
		
		files: [{
			name: <String>,
			path: <String>,
			weight: <String>
		}]
	}]
}
```
A coleção `activities` foi criada porque ela tende a crescer muito, pois, um `project` possuirá algumas `goals` e cada uma pode conter muitas `activities`. Além disso a suas propriedades `comments` e `files` tornariam o tamanho de uma activity grande, logo um documento `project` teria seu tamanho máximo ultrapassado.

## Create - cadastro

####1. Cadastre 10 usuários diferentes.
```js
var users = [
	{name: "Marcos Jagunsso", username: "marcosjagunsso", hash_token: "H77icu8bzJCLizgO5cXW"},
	{name: "Pedro Pedreira", username: "pedropedreira", hash_token: "uSTf8TMQG050U1Tl1HAQ"},
	{name: "Raquel Ratita", username: "raquelRatita", hash_token: "d7VdzXD0cZ6GCrnv8zAG"},
	{name: "Jozislene Coelho", username: "jozislenecoelho", hash_token: "5i5rzSfYrRnI58MIjaOm"},
	{name: "Creudeci Da Costa", username: "creudecidacosta", hash_token: "GpIqt8heMyncmE30typh"},
	{name: "Jaiminho da Cruz", username: "jaiminhodacruz", hash_token: "E62LUZ4ph5FixhwFf2Zd"},
	{name: "Pedro Pão de Batata", username: "pedropaodebatata", hash_token: "pMDYbUJ84WehWt1HRkLL"},
	{name: "Clóvis Couve Flor", username: "cloviscouveflor", hash_token: "drNCPlKhamouUgu5iF4i"},
	{name: "Verônica Silva", username: "veronicasilva", hash_token: "IiswCwPFm9piHvtdWfDC"},
	{name: "Patrícia Pinto", username: "patriciapinto", hash_token: "DWzGPPb4LIjxLgwmnAja"}
];

var usersInsert = []; 
users.forEach(function (user) {
	usersInsert.push({
		name: user.name,
		bio: "bla bla bla",
		date_register: new Date(),
		username: user.username,
		email: user.username + "@bol.com.br",
		password: user.username + "123",
		last_access: null,
		online: false,
		disabled: false,
		hash_token: user.hash_token,
		avatar_path: null,
		background_path: null
	});
});

db.users.insert(usersInsert);

Inserted 1 record(s) in 429ms
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

####2. Cadastre 5 projetos diferentes.
- Cada um com 5 membros, sempre diferentes dentro dos projetos;
- Cada um com pelo menos 3 tags diferentes;
	- Escolha 1 tag onde deva ficar em 2 projetos;
	- Escolha 1 tag onde deva ficar em 3 projetos;
- Cada projeto com pelo menos 1 goal;
	- Cada goal com pelo menos 3 tags;
	- Cada goal com pelo menos 2 atividades, deixe 1 projeto sem.
```js
//Members
var usersIds =  db.users.find({},{_id: 1}).toArray();
var members = [
	{
		user_id: usersIds[0]._id,
		types: ["manager"],
		notify: false
	}, {
		user_id: usersIds[1]._id,
		types: ["admin"],
		notify: true
	}, {
		user_id: usersIds[2]._id,
		types: ["worker"],
		notify: false
	}, {
		user_id: usersIds[3]._id,
		types: ["design"],
		notify: true
	}, {
		user_id: usersIds[4]._id,
		types: ["worker"],
		notify: false
	}, {
		user_id: usersIds[5]._id,
		types: ["admin"],
		notify: false
	}, {
		user_id: usersIds[6]._id,
		types: ["worker"],
		notify: false
	}, {
		user_id: usersIds[7]._id,
		types: ["engineer"],
		notify: false
	}, {
		user_id: usersIds[8]._id,
		types: ["worker"],
		notify: false
	}, {
		user_id: usersIds[9]._id,
		types: ["worker"],
		notify: false
	}
];

//Function that creates/insert an activity and returns its ObjectId
var createActivity = function(name, description, members, tags) {
	
	var id = new ObjectId();

	db.activities.insert({
		_id: id,
		name: name,
		description: description,
		date_begin: new Date(),
		date_dream: new Date(),
		date_end: new Date(),
		realocate: false,
		expired: false,

		members: members,	

		tags: tags,

		realocate_historic: [],

		comments: []
	});

	return id;
};

//Function that inserts 5 projects
var createProjects = function(members) {

	var projects = [
		{
			name: "Projeto 1",
			description: "Projeto 1 description",
			date_begin: new Date(),
			date_dream: new Date(),
			date_end: new Date(),
			visible: true,
			realocate: false,
			expired: true,
			visualizable_mod: null,

			members: members.slice(0,5),

			tags: ["bemean", "programming", "potato"],

			goals: [{
				name: "First goal",
				description: "really nice",
				date_begin: new Date(),
				date_dream: new Date(),
				date_end: new Date(),
				realocate: true,
				expired: true,

				realocate_historic: [],

				tags: ["bemean","giripoca","pinga"],

				activities: [
					createActivity("Activity 1","Bla Bla dsf", members.slice(0,2), ["potato", "project"]), 
					createActivity("Activity 2","Bla Bla dsf dfs", members.slice(1,4), ["bemean", "foguete"])
				]
			}]
		},
		{
			name: "Projeto 2",
			description: "Projeto 2 description",
			date_begin: new Date(),
			date_dream: new Date(),
			date_end: new Date(),
			visible: true,
			realocate: true,
			expired: false,
			visualizable_mod: null,

			members: members.slice(1,6),

			tags: ["bemean", "react", "js"],

			goals: [{
				name: "Go bananas",
				description: "shfhsdbf",
				date_begin: new Date(),
				date_dream: new Date(),
				date_end: new Date(),
				realocate: false,
				expired: false,

				realocate_historic: [],

				tags: ["bemean", "react", "rocket"],

				activities: [
					createActivity("Activity 1 project 2","Bla Bla dsf", members.slice(1,3), ["potato", "react"]), 
					createActivity("Activity 2 project 2","Bla Bla dsf dfs", members.slice(2,5), ["bemean", "react"])
				]
			}]
		},
		{
			name: "Projeto 3",
			description: "Projeto 3 description",
			date_begin: new Date(),
			date_dream: new Date(),
			date_end: new Date(),
			visible: true,
			realocate: true,
			expired: false,
			visualizable_mod: null,

			members: members.slice(2,7),

			tags: ["security", "react", "js"],

			goals: [{
				name: "Go Buffalo",
				description: "shfhsdbf",
				date_begin: new Date(),
				date_dream: new Date(),
				date_end: new Date(),
				realocate: false,
				expired: false,

				realocate_historic: [],

				tags: ["security", "react", "locoloco"],

				activities: [
					createActivity("Activity 1 project 3","Bla Bla dsf", members.slice(2,4), ["security", "react"]), 
					createActivity("Activity 2 project 3","Bla Bla dsf dfs", members.slice(3,6), ["angularjs", "react"])
				]
			}]
		},
		{
			name: "Projeto 4",
			description: "Projeto 4 description",
			date_begin: new Date(),
			date_dream: new Date(),
			date_end: new Date(),
			visible: true,
			realocate: false,
			expired: false,
			visualizable_mod: null,

			members: members.slice(3,8),

			tags: ["security", "react", "js"],

			goals: [{
				name: "Go bananas",
				description: "shfhsdbf",
				date_begin: new Date(),
				date_dream: new Date(),
				date_end: new Date(),
				realocate: false,
				expired: false,

				realocate_historic: [],

				tags: ["security", "react", "banana"],

				activities: [
					createActivity("Activity 1 project 4","Bla Bla dsf", members.slice(3,5), ["potato", "react"]), 
					createActivity("Activity 2 project 4","Bla Bla dsf dfs", members.slice(4,7), ["bemean", "react"])
				]
			}]
		},
		{
			name: "Projeto 5",
			description: "Projeto 5 description",
			date_begin: new Date(),
			date_dream: new Date(),
			date_end: new Date(),
			visible: true,
			realocate: true,
			expired: false,
			visualizable_mod: null,

			members: members.slice(4,9),

			tags: ["security", "potato", "js"],

			goals: [{
				name: "Go bananas",
				description: "shfhsdbf",
				date_begin: new Date(),
				date_dream: new Date(),
				date_end: new Date(),
				realocate: false,
				expired: false,

				realocate_historic: [],

				tags: ["security", "react", "rocket"],

				activities: []
			}]
		}
	];

	db.projects.insert(projects);
}

projeto> createProjects()
Inserted 1 record(s) in 5ms
Inserted 1 record(s) in 2ms
Inserted 1 record(s) in 2ms
Inserted 1 record(s) in 2ms
Inserted 1 record(s) in 2ms
Inserted 1 record(s) in 3ms
Inserted 1 record(s) in 6ms
Inserted 1 record(s) in 2ms
Inserted 1 record(s) in 9ms
```

## Retrieve - busca

####1. Liste as informações dos membros de 1 projeto específico que deve ser buscado pelo seu nome de forma a não ligar para maiúsculas e minúsculas.
```js
var members = (db.projects.findOne(/projeto 1/i, {members: 1})).members;
var usersIds = [];
members.forEach(function (member) {
	usersIds.push(member.user_id);
});

db.users.find({_id: {$in: usersIds}}); 
{
  "_id": ObjectId("56b173210cc505f686aac57a"),
  "name": "Marcos Jagunsso",
  "bio": "bla bla bla",
  "date_register": ISODate("2016-02-03T03:24:15.005Z"),
  "username": "marcosjagunsso",
  "email": "marcosjagunsso@bol.com.br",
  "password": "marcosjagunsso123",
  "last_access": null,
  "online": false,
  "disabled": false,
  "hash_token": "H77icu8bzJCLizgO5cXW",
  "avatar_path": null,
  "background_path": null
}
{
  "_id": ObjectId("56b173210cc505f686aac57b"),
  "name": "Pedro Pedreira",
  "bio": "bla bla bla",
  "date_register": ISODate("2016-02-03T03:24:15.005Z"),
  "username": "pedropedreira",
  "email": "pedropedreira@bol.com.br",
  "password": "pedropedreira123",
  "last_access": null,
  "online": false,
  "disabled": false,
  "hash_token": "uSTf8TMQG050U1Tl1HAQ",
  "avatar_path": null,
  "background_path": null
}
{
  "_id": ObjectId("56b173210cc505f686aac57c"),
  "name": "Raquel Ratita",
  "bio": "bla bla bla",
  "date_register": ISODate("2016-02-03T03:24:15.005Z"),
  "username": "raquelRatita",
  "email": "raquelRatita@bol.com.br",
  "password": "raquelRatita123",
  "last_access": null,
  "online": false,
  "disabled": false,
  "hash_token": "d7VdzXD0cZ6GCrnv8zAG",
  "avatar_path": null,
  "background_path": null
}
{
  "_id": ObjectId("56b173210cc505f686aac57d"),
  "name": "Jozislene Coelho",
  "bio": "bla bla bla",
  "date_register": ISODate("2016-02-03T03:24:15.005Z"),
  "username": "jozislenecoelho",
  "email": "jozislenecoelho@bol.com.br",
  "password": "jozislenecoelho123",
  "last_access": null,
  "online": false,
  "disabled": false,
  "hash_token": "5i5rzSfYrRnI58MIjaOm",
  "avatar_path": null,
  "background_path": null
}
{
  "_id": ObjectId("56b173210cc505f686aac57e"),
  "name": "Creudeci Da Costa",
  "bio": "bla bla bla",
  "date_register": ISODate("2016-02-03T03:24:15.005Z"),
  "username": "creudecidacosta",
  "email": "creudecidacosta@bol.com.br",
  "password": "creudecidacosta123",
  "last_access": null,
  "online": false,
  "disabled": false,
  "hash_token": "GpIqt8heMyncmE30typh",
  "avatar_path": null,
  "background_path": null
}
Fetched 5 record(s) in 27ms
```

####2. Liste todos os projetos com a tag que você escolheu para os 3 projetos em comum.
```js
db.projects.find({tags: {$in: [/security/i]}}, {name: 1})
{
  "_id": ObjectId("56b42ce174a991a9bf77550f"),
  "name": "Projeto 3"
}
{
  "_id": ObjectId("56b42ce174a991a9bf775510"),
  "name": "Projeto 4"
}
{
  "_id": ObjectId("56b42ce174a991a9bf775511"),
  "name": "Projeto 5"
}
Fetched 3 record(s) in 3ms
```

####3. Liste apenas os nomes de todas as atividades para todos os projetos.
```js
var projects = db.projects.find().toArray();
var result = {};

projects.forEach( function (p) {
	p.goals.forEach( function (g) {
		g.activities.forEach( function (a) {
			var name = db.activities.findOne({_id: a}, {name: 1}).name;
			
			if (!(p.name in result)) {
				result[p.name] = {};
			}

			if (!('activities' in result[p.name])) {
				result[p.name]['activities'] = [];
			}
			result[p.name]['activities'].push(name);	
		});
	});
});

result = {
  "Projeto 1": {
    "activities": [
      "Activity 1",
      "Activity 2"
    ]
  },
  "Projeto 2": {
    "activities": [
      "Activity 1 project 2",
      "Activity 2 project 2"
    ]
  },
  "Projeto 3": {
    "activities": [
      "Activity 1 project 3",
      "Activity 2 project 3"
    ]
  },
  "Projeto 4": {
    "activities": [
      "Activity 1 project 4",
      "Activity 2 project 4"
    ]
  }
}
```

####4. Liste todos os projetos que não possuam uma tag.
```js
db.projects.find({tags: []},{name: 1});
Fetched 0 record(s) in 1ms

db.projects.find({tags: {$size: 0}},{name: 1});
Fetched 0 record(s) in 1ms
```

####5. Liste todos os usuários que não fazem parte do primeiro projeto cadastrado.
```js
var members = (db.projects.findOne({}, {members: 1})).members;
var membersIds = [];

(db.projects.findOne({}, {members: 1})).members.forEach(function (m) {
	membersIds.push(m.user_id);
});

db.users.find({_id: {$nin: membersIds}}, {name: 1}); 
{
  "_id": ObjectId("56b173210cc505f686aac57f"),
  "name": "Jaiminho da Cruz"
}
{
  "_id": ObjectId("56b173210cc505f686aac580"),
  "name": "Pedro Pão de Batata"
}
{
  "_id": ObjectId("56b173210cc505f686aac581"),
  "name": "Clóvis Couve Flor"
}
{
  "_id": ObjectId("56b173210cc505f686aac582"),
  "name": "Verônica Silva"
}
{
  "_id": ObjectId("56b173210cc505f686aac583"),
  "name": "Patrícia Pinto"
}
Fetched 5 record(s) in 16ms
```

## Update - alteração

####1. Adicione para todos os projetos o campo `views: 0`.
```js
db.projects.update({}, {$set: {views: 0}}, {multi: true});
Updated 5 existing record(s) in 3ms
```

####2. Adicione 1 tag diferente para cada projeto.
```js
var tags = ['a', 'b', 'c', 'd', 'e']
var projects = db.projects.find().toArray();

projects.forEach( function (p, i) {
	p.tags.push(tags[i]);
	db.projects.save(p);	
});

Updated 1 existing record(s) in 4ms
Updated 1 existing record(s) in 4ms
Updated 1 existing record(s) in 3ms
Updated 1 existing record(s) in 5ms
Updated 1 existing record(s) in 5ms
```

####3. Adicione 2 membros diferentes para cada projeto.
```js
//Same members vector used in the Create section
var usersIds =  db.users.find({},{_id: 1}).toArray();
var members = [
	{
		user_id: usersIds[0]._id,
		types: ["manager"],
		notify: false
	}, {
		user_id: usersIds[1]._id,
		types: ["admin"],
		notify: true
	}, {
		user_id: usersIds[2]._id,
		types: ["worker"],
		notify: false
	}, {
		user_id: usersIds[3]._id,
		types: ["design"],
		notify: true
	}, {
		user_id: usersIds[4]._id,
		types: ["worker"],
		notify: false
	}, {
		user_id: usersIds[5]._id,
		types: ["admin"],
		notify: false
	}, {
		user_id: usersIds[6]._id,
		types: ["worker"],
		notify: false
	}, {
		user_id: usersIds[7]._id,
		types: ["engineer"],
		notify: false
	}, {
		user_id: usersIds[8]._id,
		types: ["worker"],
		notify: false
	}, {
		user_id: usersIds[9]._id,
		types: ["worker"],
		notify: false
	}
];
members = members.slice(5,10).concat(members.slice(0,5));
var projects = db.projects.find().toArray();

projects.forEach( function (p, i) {
	p.members.push(members[i], members[i+1]);
	db.projects.save(p);	
});

Updated 1 existing record(s) in 8ms
Updated 1 existing record(s) in 4ms
Updated 1 existing record(s) in 5ms
Updated 1 existing record(s) in 4ms
Updated 1 existing record(s) in 3ms
```

####4. Adicione 1 comentário em cada atividade, deixe apenas 1 projeto sem.
```js
var comment = {text: "New comment bla bla bla", data_comment: new Date(), members: [], file: []};
var activitiesIds = [];

db.activities.find({name: {$not: /Activity \d project 5/i}},{_id: 1}).toArray().forEach( function (a) {
	activitiesIds.push(a._id);
});

db.activities.update({_id: {$in : activitiesIds}},{$push: {comments: comment}},{multi: true});

Updated 8 existing record(s) in 4ms
WriteResult({
  "nMatched": 8,
  "nUpserted": 0,
  "nModified": 8
})
```

####5. Adicione 1 projeto inteiro com UPSERT.
```js
//Using createActivity and members from Create section
var project = {
	name: "Project 6",
	description: "Projeto 6 description",
	date_begin: new Date(),
	date_dream: new Date(),
	date_end: new Date(),
	visible: true,
	realocate: false,
	expired: true,
	visualizable_mod: null,

	members: members.slice(5,10),

	tags: ["bemean", "programming", "potato"],

	goals: [{
		name: "First goal",
		description: "really nice",
		date_begin: new Date(),
		date_dream: new Date(),
		date_end: new Date(),
		realocate: true,
		expired: true,

		realocate_historic: [],

		tags: ["bemean","giripoca","pinga"],

		activities: [
			createActivity("Activity 1","Bla Bla dsf", members.slice(5,7), ["potato", "project"]), 
			createActivity("Activity 2","Bla Bla dsf dfs", members.slice(6,8), ["bemean", "foguete"])
		]
	}]
};

db.projects.update({name: /Project 6/i}, {$setOnInsert: project}, {upsert:true});

Updated 1 new record(s) in 3ms
WriteResult({
  "nMatched": 0,
  "nUpserted": 1,
  "nModified": 0,
  "_id": ObjectId("56c154994581f02b2fb68f7d")
})
```

## Delete - remoção

####1. Apague todos os projetos que não possuam *tags*.
```js
var projectsIds = [];
var activitiesIds = [];

db.projects.find({tags: []},{goals: 1}).toArray().forEach( function (p) {
	projectsIds.push(p._id);
	
	p.goals.forEach( function (g) {
		activitiesIds = activitiesIds.concat(g.activities);
	});
});

db.activities.remove({_id: {$in: activitiesIds}});
Removed 0 record(s) in 2ms
WriteResult({
  "nRemoved": 0
})

db.projects.remove({_id: {$in: projectsIds}});
Removed 0 record(s) in 2ms
WriteResult({
  "nRemoved": 0
})
```

####2. Apague todos os projetos que não possuam comentários nas atividades.
```js
var activitiesIds = [];

db.activities.find({comments: []}).toArray().forEach( function (a) {
	activitiesIds.push(a._id);
});

db.activities.remove({_id: {$in: activitiesIds}});
Removed 2 record(s) in 1ms
WriteResult({
  "nRemoved": 2
})

db.projects.remove({goals : {$elemMatch: {activities: {$in: activitiesIds}}}});
Removed 1 record(s) in 12ms
WriteResult({
  "nRemoved": 1
})
```

####3. Apague todos os projetos que não possuam atividades.
```js
db.projects.remove({goals : {$elemMatch: {activities: []}}});

Removed 1 record(s) in 3ms
WriteResult({
  "nRemoved": 1
})
```

####4. Escolha 2 usuário e apague todos os projetos em que os 2 fazem parte.
```js
var usersIds = [];
var projectsIds = [];
var activitiesIds = [];

db.users.find({}, {_id: 1}).limit(2).toArray().forEach( function (u) {
	usersIds.push(u._id);
});

db.projects.find({members: {$elemMatch: {user_id: {$in: usersIds}}}}, {goals: 1}).toArray().forEach( function (p) {
	projectsIds.push(p._id);
	
	p.goals.forEach( function (g) {
		activitiesIds = activitiesIds.concat(g.activities);
	});
});

db.activities.remove({_id: {$in: activitiesIds}});
Removed 4 record(s) in 2ms
WriteResult({
  "nRemoved": 4
})

db.projects.remove({_id: {$in: projectsIds}});
Removed 2 record(s) in 2ms
WriteResult({
  "nRemoved": 2
})
```

####5. Apague todos os projetos que possuam uma determinada *tag* em *goal*.
```js
var projectsIds = [];
var activitiesIds = [];

db.projects.find({goals: {$elemMatch: {tags: {$in: [/banana/i]}}}}, {goals: 1}).toArray().forEach( function (p) {
	projectsIds.push(p._id);
	
	p.goals.forEach( function (g) {
		activitiesIds = activitiesIds.concat(g.activities);
	});
});

db.activities.remove({_id: {$in: activitiesIds}});
Removed 2 record(s) in 2ms
WriteResult({
  "nRemoved": 2
})

db.projects.remove({_id: {$in: projectsIds}});
Removed 1 record(s) in 2ms
WriteResult({
  "nRemoved": 1
})
```

## Gerenciamento de usuários

####1. Crie um usuário com permissões APENAS de Leitura.
```js
use admin
db.createUser(
	{
      user: "user1",
      pwd: "12345678",
      roles: [{ role: "read", db: "admin" }]
    }
);
```

####2. Crie um usuário com permissões de Escrita e Leitura.
```js
db.createUser(
    {
      user: "user2",
      pwd: "12345678",
      roles: [{ role: "readWrite", db: "admin" }]
    }
);
```

####3. Adicionar o papel `grantRolesToUser` e `revokeRole` para o usuário com Escrita e Leitura.
Como estes roles não são nativos do mongodb foi necessário criá-los.
```js
db.createRole(
   {
     role: "grantRolesToUser",
     privileges: [
       { resource: { db: "", collection: "" }, actions: [ "grantRole" ] }
     ],
     roles: []
   }
);

db.createRole(
   {
     role: "revokeRole",
     privileges: [
       { resource: { db: "", collection: "" }, actions: [ "revokeRole" ] }
     ],
     roles: []
   }
);

db.grantRolesToUser(
	"user2",
	[
		{role: 'grantRolesToUser', db: 'admin'},
		{role: 'revokeRole', db: 'admin'}
	]
);
```

####4. Remover o papel `grantRolesToUser` para o usuário com Escrita e Leitura.
```js
db.revokeRolesFromUser(
	"user2",
	[
		{role: 'grantRolesToUser', db: 'admin'}
	]
);
```

####5. Listar todos os usuários com seus papéis e ações.
```js
db.runCommand({usersInfo: [{user: 'user1', db: 'admin'}, {user: 'user2', db: 'admin'}], showCredentials: true, showPrivileges: true});

{
  "users": [
    {
      "_id": "admin.user1",
      "user": "user1",
      "db": "admin",
      "credentials": {
        "SCRAM-SHA-1": {
          "iterationCount": 10000,
          "salt": "mn/JcbozTybnZZKIEFMXKQ==",
          "storedKey": "rMkCGPXSuKGfltUxj2VdAi4pMxY=",
          "serverKey": "dVs7i2hgTxLYDcv2IIs/rLQEWHU="
        }
      },
      "roles": [
        {
          "role": "read",
          "db": "admin"
        }
      ],
      "inheritedRoles": [
        {
          "role": "read",
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
            "db": "admin",
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
            "db": "admin",
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
            "db": "admin",
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
    },
    {
      "_id": "admin.user2",
      "user": "user2",
      "db": "admin",
      "credentials": {
        "SCRAM-SHA-1": {
          "iterationCount": 10000,
          "salt": "2L8VCAGNgI1EKOc1AUkn3A==",
          "storedKey": "Q4cy6SFYnPu7MmXptPdzXz2wVHc=",
          "serverKey": "lvEYLyVYoW893Lxh8gXUf/DsyoI="
        }
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
      ],
      "inheritedRoles": [
        {
          "role": "readWrite",
          "db": "admin"
        },
        {
          "role": "revokeRole",
          "db": "admin"
        }
      ],
      "inheritedPrivileges": [
        {
          "resource": {
            "db": "",
            "collection": ""
          },
          "actions": [
            "revokeRole"
          ]
        },
        {
          "resource": {
            "db": "admin",
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
            "db": "admin",
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
            "db": "admin",
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
            "db": "admin",
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
  ],
  "ok": 1
}
```

## Cluster

O cluster será feito localmente contendo:
- 1 `Config Server`
- 1 `Router`
- 3 `Shards`
- 3 `Réplicas` = cada `Shard` possuirá uma 1 `Réplica Secundária`

Foi criada uma pasta específica para armazenar todos os servers.
```js
mkdir projeto-mongodb
cd projeto-mongodb
```

###Config Server 
```js
mkdir configdb
mongod --configsvr --port 27010 --dbpath configdb

2016-02-21T22:09:38.905-0300 I CONTROL  [initandlisten] MongoDB starting : pid=6777 port=27010 dbpath=configdb master=1 64-bit host=matrix
2016-02-21T22:09:38.906-0300 I CONTROL  [initandlisten] db version v3.0.9
2016-02-21T22:09:38.906-0300 I CONTROL  [initandlisten] git version: 20d60d3491908f1ae252fe452300de3978a040c7
2016-02-21T22:09:38.906-0300 I CONTROL  [initandlisten] build info: Linux ip-10-237-6-122 3.13.0-24-generic #46-Ubuntu SMP Thu Apr 10 19:11:08 UTC 2014 x86_64 BOOST_LIB_VERSION=1_49
2016-02-21T22:09:38.906-0300 I CONTROL  [initandlisten] allocator: tcmalloc
2016-02-21T22:09:38.906-0300 I CONTROL  [initandlisten] options: { net: { port: 27010 }, sharding: { clusterRole: "configsvr" }, storage: { dbPath: "configdb" } }
2016-02-21T22:09:39.010-0300 I JOURNAL  [initandlisten] journal dir=configdb/journal
2016-02-21T22:09:39.011-0300 I JOURNAL  [initandlisten] recover : no journal files present, no recovery needed
2016-02-21T22:09:39.151-0300 I JOURNAL  [durability] Durability thread started
2016-02-21T22:09:39.151-0300 I JOURNAL  [journal writer] Journal writer thread started
2016-02-21T22:09:39.196-0300 I CONTROL  [initandlisten] 
2016-02-21T22:09:39.197-0300 I CONTROL  [initandlisten] ** WARNING: /sys/kernel/mm/transparent_hugepage/enabled is 'always'.
2016-02-21T22:09:39.197-0300 I CONTROL  [initandlisten] **        We suggest setting it to 'never'
2016-02-21T22:09:39.197-0300 I CONTROL  [initandlisten] 
2016-02-21T22:09:39.197-0300 I CONTROL  [initandlisten] ** WARNING: /sys/kernel/mm/transparent_hugepage/defrag is 'always'.
2016-02-21T22:09:39.197-0300 I CONTROL  [initandlisten] **        We suggest setting it to 'never'
2016-02-21T22:09:39.197-0300 I CONTROL  [initandlisten] 
2016-02-21T22:09:40.437-0300 I NETWORK  [initandlisten] waiting for connections on port 27010
```

###Router
```js
mongos --configdb localhost:27010 --port 27011

2016-02-21T22:27:27.678-0300 W SHARDING running with 1 config server should be done only for testing purposes and is not recommended for production
2016-02-21T22:27:27.685-0300 I SHARDING [mongosMain] MongoS version 3.0.9 starting: pid=7162 port=27011 64-bit host=matrix (--help for usage)
2016-02-21T22:27:27.685-0300 I CONTROL  [mongosMain] db version v3.0.9
2016-02-21T22:27:27.685-0300 I CONTROL  [mongosMain] git version: 20d60d3491908f1ae252fe452300de3978a040c7
2016-02-21T22:27:27.685-0300 I CONTROL  [mongosMain] build info: Linux ip-10-237-6-122 3.13.0-24-generic #46-Ubuntu SMP Thu Apr 10 19:11:08 UTC 2014 x86_64 BOOST_LIB_VERSION=1_49
2016-02-21T22:27:27.685-0300 I CONTROL  [mongosMain] allocator: tcmalloc
2016-02-21T22:27:27.685-0300 I CONTROL  [mongosMain] options: { net: { port: 27011 }, sharding: { configDB: "localhost:27010" } }
2016-02-21T22:27:27.794-0300 I SHARDING [LockPinger] creating distributed lock ping thread for localhost:27010 and process matrix:27011:1456104447:1804289383 (sleeping for 30000ms)
2016-02-21T22:27:28.123-0300 I SHARDING [LockPinger] cluster localhost:27010 pinged successfully at Sun Feb 21 22:27:27 2016 by distributed lock pinger 'localhost:27010/matrix:27011:1456104447:1804289383', sleeping for 30000ms
2016-02-21T22:27:28.124-0300 I SHARDING [mongosMain] distributed lock 'configUpgrade/matrix:27011:1456104447:1804289383' acquired, ts : 56ca63ffc54d87a2169aa70a
2016-02-21T22:27:28.125-0300 I SHARDING [mongosMain] starting upgrade of config server from v0 to v6
2016-02-21T22:27:28.126-0300 I SHARDING [mongosMain] starting next upgrade step from v0 to v6
2016-02-21T22:27:28.126-0300 I SHARDING [mongosMain] about to log new metadata event: { _id: "matrix-2016-02-22T01:27:28-56ca6400c54d87a2169aa70b", server: "matrix", clientAddr: "N/A", time: new Date(1456104448126), what: "starting upgrade of config database", ns: "config.version", details: { from: 0, to: 6 } }
2016-02-21T22:27:28.263-0300 I SHARDING [mongosMain] writing initial config version at v6
2016-02-21T22:27:28.350-0300 I SHARDING [mongosMain] about to log new metadata event: { _id: "matrix-2016-02-22T01:27:28-56ca6400c54d87a2169aa70d", server: "matrix", clientAddr: "N/A", time: new Date(1456104448350), what: "finished upgrade of config database", ns: "config.version", details: { from: 0, to: 6 } }
2016-02-21T22:27:28.480-0300 I SHARDING [mongosMain] upgrade of config server to v6 successful
2016-02-21T22:27:28.481-0300 I SHARDING [mongosMain] distributed lock 'configUpgrade/matrix:27011:1456104447:1804289383' unlocked. 
2016-02-21T22:27:29.544-0300 I SHARDING [Balancer] about to contact config servers and shards
```

###Shards
####Pastas onde serão criados os servidores shards
```js
mkdir shard1 && mkdir shard2 && mkdir shard3
```

####Shard 1 - replicaSet rs1
```js
mongod --replSet rs1 --port 27012 --dbpath shard1

2016-02-21T22:42:13.483-0300 I CONTROL  [initandlisten] MongoDB starting : pid=7229 port=27012 dbpath=shard1 64-bit host=matrix
2016-02-21T22:42:13.483-0300 I CONTROL  [initandlisten] db version v3.0.9
2016-02-21T22:42:13.483-0300 I CONTROL  [initandlisten] git version: 20d60d3491908f1ae252fe452300de3978a040c7
2016-02-21T22:42:13.483-0300 I CONTROL  [initandlisten] build info: Linux ip-10-237-6-122 3.13.0-24-generic #46-Ubuntu SMP Thu Apr 10 19:11:08 UTC 2014 x86_64 BOOST_LIB_VERSION=1_49
2016-02-21T22:42:13.483-0300 I CONTROL  [initandlisten] allocator: tcmalloc
2016-02-21T22:42:13.483-0300 I CONTROL  [initandlisten] options: { net: { port: 27012 }, replication: { replSet: "rs1" }, storage: { dbPath: "shard1" } }
2016-02-21T22:42:13.519-0300 I JOURNAL  [initandlisten] journal dir=shard1/journal
2016-02-21T22:42:13.519-0300 I JOURNAL  [initandlisten] recover : no journal files present, no recovery needed
2016-02-21T22:42:16.060-0300 I JOURNAL  [initandlisten] preallocateIsFaster=true 32.62
2016-02-21T22:42:19.156-0300 I JOURNAL  [initandlisten] preallocateIsFaster=true 26.04
2016-02-21T22:42:22.780-0300 I JOURNAL  [initandlisten] preallocateIsFaster=true 34.9
2016-02-21T22:42:22.780-0300 I JOURNAL  [initandlisten] preallocateIsFaster check took 9.26 secs
2016-02-21T22:42:22.780-0300 I JOURNAL  [initandlisten] preallocating a journal file shard1/journal/prealloc.0
2016-02-21T22:42:25.090-0300 I -        [initandlisten]   File Preallocator Progress: 587202560/1073741824 54%
2016-02-21T22:42:28.020-0300 I -        [initandlisten]   File Preallocator Progress: 828375040/1073741824 77%
2016-02-21T22:42:31.124-0300 I -        [initandlisten]   File Preallocator Progress: 1027604480/1073741824 95%
2016-02-21T22:42:37.779-0300 I JOURNAL  [initandlisten] preallocating a journal file shard1/journal/prealloc.1
2016-02-21T22:42:40.108-0300 I -        [initandlisten]   File Preallocator Progress: 513802240/1073741824 47%
2016-02-21T22:42:43.073-0300 I -        [initandlisten]   File Preallocator Progress: 723517440/1073741824 67%
2016-02-21T22:42:47.454-0300 I -        [initandlisten]   File Preallocator Progress: 754974720/1073741824 70%
2016-02-21T22:42:53.192-0300 I JOURNAL  [initandlisten] preallocating a journal file shard1/journal/prealloc.2
2016-02-21T22:42:56.068-0300 I -        [initandlisten]   File Preallocator Progress: 471859200/1073741824 43%
2016-02-21T22:42:59.941-0300 I -        [initandlisten]   File Preallocator Progress: 734003200/1073741824 68%
2016-02-21T22:43:03.729-0300 I -        [initandlisten]   File Preallocator Progress: 933232640/1073741824 86%
2016-02-21T22:43:09.047-0300 I JOURNAL  [durability] Durability thread started
2016-02-21T22:43:09.048-0300 I JOURNAL  [journal writer] Journal writer thread started
2016-02-21T22:43:09.164-0300 I CONTROL  [initandlisten] 
2016-02-21T22:43:09.164-0300 I CONTROL  [initandlisten] ** WARNING: /sys/kernel/mm/transparent_hugepage/enabled is 'always'.
2016-02-21T22:43:09.164-0300 I CONTROL  [initandlisten] **        We suggest setting it to 'never'
2016-02-21T22:43:09.164-0300 I CONTROL  [initandlisten] 
2016-02-21T22:43:09.164-0300 I CONTROL  [initandlisten] ** WARNING: /sys/kernel/mm/transparent_hugepage/defrag is 'always'.
2016-02-21T22:43:09.164-0300 I CONTROL  [initandlisten] **        We suggest setting it to 'never'
2016-02-21T22:43:09.164-0300 I CONTROL  [initandlisten] 
2016-02-21T22:43:09.165-0300 I INDEX    [initandlisten] allocating new ns file shard1/local.ns, filling with zeroes...
2016-02-21T22:43:09.508-0300 I STORAGE  [FileAllocator] allocating new datafile shard1/local.0, filling with zeroes...
2016-02-21T22:43:09.508-0300 I STORAGE  [FileAllocator] creating directory shard1/_tmp
2016-02-21T22:43:09.607-0300 I STORAGE  [FileAllocator] done allocating datafile shard1/local.0, size: 64MB,  took 0.052 secs
2016-02-21T22:43:09.713-0300 I REPL     [initandlisten] Did not find local replica set configuration document at startup;  NoMatchingDocument Did not find replica set configuration document in local.system.replset
2016-02-21T22:43:09.714-0300 I NETWORK  [initandlisten] waiting for connections on port 27012
2016-02-21T22:54:26.526-0300 I NETWORK  [initandlisten] connection accepted from 127.0.0.1:48552 #1 (1 connection now open)
2016-02-21T22:57:41.369-0300 I NETWORK  [initandlisten] connection accepted from 127.0.0.1:50719 #2 (2 connections now open)
2016-02-21T23:02:58.008-0300 I REPL     [conn2] replSetInitiate admin command received from client
2016-02-21T23:02:58.025-0300 I REPL     [conn2] replSet replSetInitiate config object with 1 members parses ok
2016-02-21T23:02:58.027-0300 I REPL     [ReplicationExecutor] New replica set config in use: { _id: "rs1", version: 1, members: [ { _id: 0, host: "127.0.0.1:27012", arbiterOnly: false, buildIndexes: true, hidden: false, priority: 1.0, tags: {}, slaveDelay: 0, votes: 1 } ], settings: { chainingAllowed: true, heartbeatTimeoutSecs: 10, getLastErrorModes: {}, getLastErrorDefaults: { w: 1, wtimeout: 0 } } }
2016-02-21T23:02:58.027-0300 I REPL     [ReplicationExecutor] This node is 127.0.0.1:27012 in the config
2016-02-21T23:02:58.027-0300 I REPL     [ReplicationExecutor] transition to STARTUP2
2016-02-21T23:02:58.027-0300 I REPL     [conn2] ******
2016-02-21T23:02:58.027-0300 I REPL     [conn2] creating replication oplog of size: 4078MB...
2016-02-21T23:02:58.028-0300 I STORAGE  [FileAllocator] allocating new datafile shard1/local.1, filling with zeroes...
2016-02-21T23:02:58.071-0300 I STORAGE  [FileAllocator] done allocating datafile shard1/local.1, size: 2047MB,  took 0.042 secs
2016-02-21T23:02:58.071-0300 I STORAGE  [FileAllocator] allocating new datafile shard1/local.2, filling with zeroes...
2016-02-21T23:02:58.096-0300 I STORAGE  [FileAllocator] done allocating datafile shard1/local.2, size: 2047MB,  took 0.024 secs
2016-02-21T23:02:58.098-0300 I REPL     [conn2] ******
2016-02-21T23:02:58.099-0300 I REPL     [conn2] Starting replication applier threads
2016-02-21T23:02:58.099-0300 I REPL     [ReplicationExecutor] transition to RECOVERING
2016-02-21T23:02:58.101-0300 I REPL     [ReplicationExecutor] transition to SECONDARY
2016-02-21T23:02:58.101-0300 I REPL     [ReplicationExecutor] transition to PRIMARY
2016-02-21T23:02:59.100-0300 I REPL     [rsSync] transition to primary complete; database writes are now permitted
2016-02-21T23:03:55.908-0300 I NETWORK  [conn2] end connection 127.0.0.1:50719 (1 connection now open)
```

####Shard2 - replicaSet rs2
```js
mongod --replSet rs2 --port 27013 --dbpath shard2

2016-02-21T22:42:30.343-0300 I CONTROL  [initandlisten] MongoDB starting : pid=7234 port=27013 dbpath=shard2 64-bit host=matrix
2016-02-21T22:42:30.343-0300 I CONTROL  [initandlisten] db version v3.0.9
2016-02-21T22:42:30.343-0300 I CONTROL  [initandlisten] git version: 20d60d3491908f1ae252fe452300de3978a040c7
2016-02-21T22:42:30.343-0300 I CONTROL  [initandlisten] build info: Linux ip-10-237-6-122 3.13.0-24-generic #46-Ubuntu SMP Thu Apr 10 19:11:08 UTC 2014 x86_64 BOOST_LIB_VERSION=1_49
2016-02-21T22:42:30.343-0300 I CONTROL  [initandlisten] allocator: tcmalloc
2016-02-21T22:42:30.343-0300 I CONTROL  [initandlisten] options: { net: { port: 27013 }, replication: { replSet: "rs2" }, storage: { dbPath: "shard2" } }
2016-02-21T22:42:30.453-0300 I JOURNAL  [initandlisten] journal dir=shard2/journal
2016-02-21T22:42:30.454-0300 I JOURNAL  [initandlisten] recover : no journal files present, no recovery needed
2016-02-21T22:43:12.235-0300 I JOURNAL  [initandlisten] preallocateIsFaster=true 659.02
2016-02-21T22:43:16.916-0300 I JOURNAL  [initandlisten] preallocateIsFaster=true 53.92
2016-02-21T22:43:22.787-0300 I JOURNAL  [initandlisten] preallocateIsFaster=true 52.22
2016-02-21T22:43:22.788-0300 I JOURNAL  [initandlisten] preallocateIsFaster check took 52.334 secs
2016-02-21T22:43:22.788-0300 I JOURNAL  [initandlisten] preallocating a journal file shard2/journal/prealloc.0
2016-02-21T22:43:25.225-0300 I -        [initandlisten]   File Preallocator Progress: 283115520/1073741824 26%
2016-02-21T22:43:28.157-0300 I -        [initandlisten]   File Preallocator Progress: 387973120/1073741824 36%
2016-02-21T22:43:31.032-0300 I -        [initandlisten]   File Preallocator Progress: 482344960/1073741824 44%
2016-02-21T22:43:34.120-0300 I -        [initandlisten]   File Preallocator Progress: 566231040/1073741824 52%
2016-02-21T22:43:39.553-0300 I -        [initandlisten]   File Preallocator Progress: 671088640/1073741824 62%
2016-02-21T22:43:42.536-0300 I -        [initandlisten]   File Preallocator Progress: 775946240/1073741824 72%
2016-02-21T22:43:45.064-0300 I -        [initandlisten]   File Preallocator Progress: 954204160/1073741824 88%
2016-02-21T22:43:48.176-0300 I -        [initandlisten]   File Preallocator Progress: 1048576000/1073741824 97%
2016-02-21T22:43:52.815-0300 I JOURNAL  [initandlisten] preallocating a journal file shard2/journal/prealloc.1
2016-02-21T22:43:55.060-0300 I -        [initandlisten]   File Preallocator Progress: 272629760/1073741824 25%
2016-02-21T22:43:58.288-0300 I -        [initandlisten]   File Preallocator Progress: 377487360/1073741824 35%
2016-02-21T22:44:01.328-0300 I -        [initandlisten]   File Preallocator Progress: 482344960/1073741824 44%
2016-02-21T22:44:05.632-0300 I -        [initandlisten]   File Preallocator Progress: 534773760/1073741824 49%
2016-02-21T22:44:08.188-0300 I -        [initandlisten]   File Preallocator Progress: 754974720/1073741824 70%
2016-02-21T22:44:13.753-0300 I -        [initandlisten]   File Preallocator Progress: 849346560/1073741824 79%
2016-02-21T22:44:16.906-0300 I -        [initandlisten]   File Preallocator Progress: 943718400/1073741824 87%
2016-02-21T22:44:23.602-0300 I JOURNAL  [initandlisten] preallocating a journal file shard2/journal/prealloc.2
2016-02-21T22:44:26.241-0300 I -        [initandlisten]   File Preallocator Progress: 293601280/1073741824 27%
2016-02-21T22:44:29.028-0300 I -        [initandlisten]   File Preallocator Progress: 408944640/1073741824 38%
2016-02-21T22:44:32.225-0300 I -        [initandlisten]   File Preallocator Progress: 566231040/1073741824 52%
2016-02-21T22:44:35.252-0300 I -        [initandlisten]   File Preallocator Progress: 629145600/1073741824 58%
2016-02-21T22:44:38.108-0300 I -        [initandlisten]   File Preallocator Progress: 723517440/1073741824 67%
2016-02-21T22:44:42.133-0300 I -        [initandlisten]   File Preallocator Progress: 838860800/1073741824 78%
2016-02-21T22:44:45.016-0300 I -        [initandlisten]   File Preallocator Progress: 975175680/1073741824 90%
2016-02-21T22:44:53.843-0300 I JOURNAL  [durability] Durability thread started
2016-02-21T22:44:53.843-0300 I JOURNAL  [journal writer] Journal writer thread started
2016-02-21T22:44:53.969-0300 I CONTROL  [initandlisten] 
2016-02-21T22:44:53.969-0300 I CONTROL  [initandlisten] ** WARNING: /sys/kernel/mm/transparent_hugepage/enabled is 'always'.
2016-02-21T22:44:53.969-0300 I CONTROL  [initandlisten] **        We suggest setting it to 'never'
2016-02-21T22:44:53.969-0300 I CONTROL  [initandlisten] 
2016-02-21T22:44:53.969-0300 I CONTROL  [initandlisten] ** WARNING: /sys/kernel/mm/transparent_hugepage/defrag is 'always'.
2016-02-21T22:44:53.969-0300 I CONTROL  [initandlisten] **        We suggest setting it to 'never'
2016-02-21T22:44:53.969-0300 I CONTROL  [initandlisten] 
2016-02-21T22:44:53.970-0300 I INDEX    [initandlisten] allocating new ns file shard2/local.ns, filling with zeroes...
2016-02-21T22:44:54.512-0300 I STORAGE  [FileAllocator] allocating new datafile shard2/local.0, filling with zeroes...
2016-02-21T22:44:54.512-0300 I STORAGE  [FileAllocator] creating directory shard2/_tmp
2016-02-21T22:44:54.601-0300 I STORAGE  [FileAllocator] done allocating datafile shard2/local.0, size: 64MB,  took 0.053 secs
2016-02-21T22:44:54.708-0300 I REPL     [initandlisten] Did not find local replica set configuration document at startup;  NoMatchingDocument Did not find replica set configuration document in local.system.replset
2016-02-21T22:44:54.721-0300 I NETWORK  [initandlisten] waiting for connections on port 27013
2016-02-21T23:06:21.441-0300 I NETWORK  [initandlisten] connection accepted from 127.0.0.1:56199 #1 (1 connection now open)
2016-02-21T23:06:33.227-0300 I REPL     [conn1] replSetInitiate admin command received from client
2016-02-21T23:06:33.227-0300 I REPL     [conn1] replSet replSetInitiate config object with 1 members parses ok
2016-02-21T23:06:33.229-0300 I REPL     [ReplicationExecutor] New replica set config in use: { _id: "rs2", version: 1, members: [ { _id: 0, host: "127.0.0.1:27013", arbiterOnly: false, buildIndexes: true, hidden: false, priority: 1.0, tags: {}, slaveDelay: 0, votes: 1 } ], settings: { chainingAllowed: true, heartbeatTimeoutSecs: 10, getLastErrorModes: {}, getLastErrorDefaults: { w: 1, wtimeout: 0 } } }
2016-02-21T23:06:33.229-0300 I REPL     [ReplicationExecutor] This node is 127.0.0.1:27013 in the config
2016-02-21T23:06:33.229-0300 I REPL     [ReplicationExecutor] transition to STARTUP2
2016-02-21T23:06:33.229-0300 I REPL     [conn1] ******
2016-02-21T23:06:33.229-0300 I REPL     [conn1] creating replication oplog of size: 3873MB...
2016-02-21T23:06:33.230-0300 I STORAGE  [FileAllocator] allocating new datafile shard2/local.1, filling with zeroes...
2016-02-21T23:06:33.258-0300 I STORAGE  [FileAllocator] done allocating datafile shard2/local.1, size: 2047MB,  took 0.028 secs
2016-02-21T23:06:33.259-0300 I STORAGE  [FileAllocator] allocating new datafile shard2/local.2, filling with zeroes...
2016-02-21T23:06:33.284-0300 I STORAGE  [FileAllocator] done allocating datafile shard2/local.2, size: 2047MB,  took 0.025 secs
2016-02-21T23:06:33.294-0300 I REPL     [conn1] ******
2016-02-21T23:06:33.295-0300 I REPL     [conn1] Starting replication applier threads
2016-02-21T23:06:33.295-0300 I REPL     [ReplicationExecutor] transition to RECOVERING
2016-02-21T23:06:33.297-0300 I REPL     [ReplicationExecutor] transition to SECONDARY
2016-02-21T23:06:33.297-0300 I REPL     [ReplicationExecutor] transition to PRIMARY
2016-02-21T23:06:34.296-0300 I REPL     [rsSync] transition to primary complete; database writes are now permitted
2016-02-21T23:06:39.174-0300 I NETWORK  [conn1] end connection 127.0.0.1:56199 (0 connections now open)
```

####Shard3 - replicaSet rs3
```js
mongod --replSet rs3 --port 27014 --dbpath shard3

2016-02-21T22:42:51.227-0300 I CONTROL  [initandlisten] MongoDB starting : pid=7251 port=27014 dbpath=shard3 64-bit host=matrix
2016-02-21T22:42:51.227-0300 I CONTROL  [initandlisten] db version v3.0.9
2016-02-21T22:42:51.227-0300 I CONTROL  [initandlisten] git version: 20d60d3491908f1ae252fe452300de3978a040c7
2016-02-21T22:42:51.227-0300 I CONTROL  [initandlisten] build info: Linux ip-10-237-6-122 3.13.0-24-generic #46-Ubuntu SMP Thu Apr 10 19:11:08 UTC 2014 x86_64 BOOST_LIB_VERSION=1_49
2016-02-21T22:42:51.227-0300 I CONTROL  [initandlisten] allocator: tcmalloc
2016-02-21T22:42:51.227-0300 I CONTROL  [initandlisten] options: { net: { port: 27014 }, replication: { replSet: "rs3" }, storage: { dbPath: "shard3" } }
2016-02-21T22:42:53.180-0300 I JOURNAL  [initandlisten] journal dir=shard3/journal
2016-02-21T22:42:53.181-0300 I JOURNAL  [initandlisten] recover : no journal files present, no recovery needed
2016-02-21T22:43:12.472-0300 I JOURNAL  [initandlisten] preallocateIsFaster=true 351.68
2016-02-21T22:43:16.990-0300 I JOURNAL  [initandlisten] preallocateIsFaster=true 58.56
2016-02-21T22:43:22.841-0300 I JOURNAL  [initandlisten] preallocateIsFaster=true 54.98
2016-02-21T22:43:22.841-0300 I JOURNAL  [initandlisten] preallocateIsFaster check took 29.66 secs
2016-02-21T22:43:22.841-0300 I JOURNAL  [initandlisten] preallocating a journal file shard3/journal/prealloc.0
2016-02-21T22:43:25.017-0300 I -        [initandlisten]   File Preallocator Progress: 251658240/1073741824 23%
2016-02-21T22:43:28.137-0300 I -        [initandlisten]   File Preallocator Progress: 367001600/1073741824 34%
2016-02-21T22:43:31.300-0300 I -        [initandlisten]   File Preallocator Progress: 471859200/1073741824 43%
2016-02-21T22:43:34.188-0300 I -        [initandlisten]   File Preallocator Progress: 597688320/1073741824 55%
2016-02-21T22:43:39.557-0300 I -        [initandlisten]   File Preallocator Progress: 702545920/1073741824 65%
2016-02-21T22:43:42.549-0300 I -        [initandlisten]   File Preallocator Progress: 807403520/1073741824 75%
2016-02-21T22:43:45.224-0300 I -        [initandlisten]   File Preallocator Progress: 996147200/1073741824 92%
2016-02-21T22:43:52.787-0300 I JOURNAL  [initandlisten] preallocating a journal file shard3/journal/prealloc.1
2016-02-21T22:43:55.245-0300 I -        [initandlisten]   File Preallocator Progress: 251658240/1073741824 23%
2016-02-21T22:43:58.173-0300 I -        [initandlisten]   File Preallocator Progress: 346030080/1073741824 32%
2016-02-21T22:44:01.236-0300 I -        [initandlisten]   File Preallocator Progress: 450887680/1073741824 41%
2016-02-21T22:44:05.629-0300 I -        [initandlisten]   File Preallocator Progress: 503316480/1073741824 46%
2016-02-21T22:44:08.096-0300 I -        [initandlisten]   File Preallocator Progress: 639631360/1073741824 59%
2016-02-21T22:44:11.209-0300 I -        [initandlisten]   File Preallocator Progress: 754974720/1073741824 70%
2016-02-21T22:44:14.737-0300 I -        [initandlisten]   File Preallocator Progress: 922746880/1073741824 85%
2016-02-21T22:44:18.784-0300 I -        [initandlisten]   File Preallocator Progress: 1038090240/1073741824 96%
2016-02-21T22:44:23.602-0300 I JOURNAL  [initandlisten] preallocating a journal file shard3/journal/prealloc.2
2016-02-21T22:44:26.213-0300 I -        [initandlisten]   File Preallocator Progress: 241172480/1073741824 22%
2016-02-21T22:44:30.140-0300 I -        [initandlisten]   File Preallocator Progress: 304087040/1073741824 28%
2016-02-21T22:44:33.117-0300 I -        [initandlisten]   File Preallocator Progress: 440401920/1073741824 41%
2016-02-21T22:44:36.553-0300 I -        [initandlisten]   File Preallocator Progress: 566231040/1073741824 52%
2016-02-21T22:44:39.244-0300 I -        [initandlisten]   File Preallocator Progress: 692060160/1073741824 64%
2016-02-21T22:44:42.035-0300 I -        [initandlisten]   File Preallocator Progress: 754974720/1073741824 70%
2016-02-21T22:44:46.607-0300 I -        [initandlisten]   File Preallocator Progress: 880803840/1073741824 82%
2016-02-21T22:44:49.081-0300 I -        [initandlisten]   File Preallocator Progress: 1048576000/1073741824 97%
2016-02-21T22:44:53.867-0300 I JOURNAL  [durability] Durability thread started
2016-02-21T22:44:53.867-0300 I JOURNAL  [journal writer] Journal writer thread started
2016-02-21T22:44:53.969-0300 I CONTROL  [initandlisten] 
2016-02-21T22:44:53.969-0300 I CONTROL  [initandlisten] ** WARNING: /sys/kernel/mm/transparent_hugepage/enabled is 'always'.
2016-02-21T22:44:53.969-0300 I CONTROL  [initandlisten] **        We suggest setting it to 'never'
2016-02-21T22:44:53.969-0300 I CONTROL  [initandlisten] 
2016-02-21T22:44:53.969-0300 I CONTROL  [initandlisten] ** WARNING: /sys/kernel/mm/transparent_hugepage/defrag is 'always'.
2016-02-21T22:44:53.969-0300 I CONTROL  [initandlisten] **        We suggest setting it to 'never'
2016-02-21T22:44:53.969-0300 I CONTROL  [initandlisten] 
2016-02-21T22:44:53.970-0300 I INDEX    [initandlisten] allocating new ns file shard3/local.ns, filling with zeroes...
2016-02-21T22:44:54.512-0300 I STORAGE  [FileAllocator] allocating new datafile shard3/local.0, filling with zeroes...
2016-02-21T22:44:54.512-0300 I STORAGE  [FileAllocator] creating directory shard3/_tmp
2016-02-21T22:44:54.564-0300 I STORAGE  [FileAllocator] done allocating datafile shard3/local.0, size: 64MB,  took 0.033 secs
2016-02-21T22:44:54.604-0300 I REPL     [initandlisten] Did not find local replica set configuration document at startup;  NoMatchingDocument Did not find replica set configuration document in local.system.replset
2016-02-21T22:44:54.605-0300 I NETWORK  [initandlisten] waiting for connections on port 27014
2016-02-21T23:06:44.149-0300 I NETWORK  [initandlisten] connection accepted from 127.0.0.1:59483 #1 (1 connection now open)
2016-02-21T23:06:58.097-0300 I REPL     [conn1] replSetInitiate admin command received from client
2016-02-21T23:06:58.097-0300 I REPL     [conn1] replSet replSetInitiate config object with 1 members parses ok
2016-02-21T23:06:58.098-0300 I REPL     [ReplicationExecutor] New replica set config in use: { _id: "rs3", version: 1, members: [ { _id: 0, host: "127.0.0.1:27014", arbiterOnly: false, buildIndexes: true, hidden: false, priority: 1.0, tags: {}, slaveDelay: 0, votes: 1 } ], settings: { chainingAllowed: true, heartbeatTimeoutSecs: 10, getLastErrorModes: {}, getLastErrorDefaults: { w: 1, wtimeout: 0 } } }
2016-02-21T23:06:58.098-0300 I REPL     [ReplicationExecutor] This node is 127.0.0.1:27014 in the config
2016-02-21T23:06:58.098-0300 I REPL     [ReplicationExecutor] transition to STARTUP2
2016-02-21T23:06:58.098-0300 I REPL     [conn1] ******
2016-02-21T23:06:58.098-0300 I REPL     [conn1] creating replication oplog of size: 3669MB...
2016-02-21T23:06:58.098-0300 I STORAGE  [FileAllocator] allocating new datafile shard3/local.1, filling with zeroes...
2016-02-21T23:06:58.129-0300 I STORAGE  [FileAllocator] done allocating datafile shard3/local.1, size: 2047MB,  took 0.03 secs
2016-02-21T23:06:58.129-0300 I STORAGE  [FileAllocator] allocating new datafile shard3/local.2, filling with zeroes...
2016-02-21T23:06:58.172-0300 I STORAGE  [FileAllocator] done allocating datafile shard3/local.2, size: 2047MB,  took 0.042 secs
2016-02-21T23:06:58.617-0300 I REPL     [conn1] ******
2016-02-21T23:06:58.618-0300 I REPL     [conn1] Starting replication applier threads
2016-02-21T23:06:58.618-0300 I COMMAND  [conn1] command admin.$cmd command: replSetInitiate { replSetInitiate: { _id: "rs3", members: [ { _id: 0.0, host: "127.0.0.1:27014" } ] } } keyUpdates:0 writeConflicts:0 numYields:0 reslen:37 locks:{ Global: { acquireCount: { r: 5, w: 3, W: 2 } }, MMAPV1Journal: { acquireCount: { w: 7 }, acquireWaitCount: { w: 1 }, timeAcquiringMicros: { w: 190 } }, Database: { acquireCount: { w: 1, W: 2 } }, Metadata: { acquireCount: { W: 4 } }, oplog: { acquireCount: { w: 1 } } } 521ms
2016-02-21T23:06:58.618-0300 I REPL     [ReplicationExecutor] transition to RECOVERING
2016-02-21T23:06:58.621-0300 I REPL     [ReplicationExecutor] transition to SECONDARY
2016-02-21T23:06:58.621-0300 I REPL     [ReplicationExecutor] transition to PRIMARY
2016-02-21T23:06:59.619-0300 I REPL     [rsSync] transition to primary complete; database writes are now permitted
2016-02-21T23:07:01.005-0300 I NETWORK  [conn1] end connection 127.0.0.1:59483 (0 connections now open)
```

####Configuração das replicaSet em cada shard criado
```js
mongo --port 27012
var rsconf = 
{
  "_id": "rs1",
  "members": [
    {
      "_id": 0,
      "host": "127.0.0.1:27012"
    }
  ]
}
rs.initiate(rsconf)
{
  "ok": 1
}
```

```js
mongo --port 27013
var rsconf = 
{
  "_id": "rs2",
  "members": [
    {
      "_id": 0,
      "host": "127.0.0.1:27013"
    }
  ]
}
rs.initiate(rsconf)
{
  "ok": 1
}
```

```js
mongo --port 27014
var rsconf = 
{
  "_id": "rs3",
  "members": [
    {
      "_id": 0,
      "host": "127.0.0.1:27014"
    }
  ]
}
rs.initiate(rsconf)
{
  "ok": 1
}
```

####Configuração do cluster, conectar ao `Router`, adicionar os servidores `Shards` e configurar qual `collection` será shardeada
```js
mongo --port 27011

mongos> sh.addShard('rs1/127.0.0.1:27012')
{
  "shardAdded": "rs1",
  "ok": 1
}

mongos> sh.addShard('rs2/127.0.0.1:27013')
{
  "shardAdded": "rs2",
  "ok": 1
}

mongos> sh.addShard('rs3/127.0.0.1:27014')
{
  "shardAdded": "rs3",
  "ok": 1
}
```

A coleção escolhida para sharding foi a `activities`, porque ela tem forte tendência de crescimento, pelos motivos já comentados na etapa de Criação e a inserção de comentários e files.
```js
sh.enableSharding('projeto');
sh.shardCollection('projeto.activities', {'_id': 1});
{
  "collectionsharded": "projeto.activities",
  "ok": 1
}
```


####Teste
```js
for(var i = 0; i < 100000; i++) {
	db.activities.insert({
		name: 'Activity ' + i,
		description: 'bla',
		date_begin: new Date(),
		date_dream: new Date(),
		date_end: new Date(),
		realocate: false,
		expired: false,

		members: [],	

		tags: [],

		realocate_historic: [],

		comments: []
	});
}

```

###Replicas
####Criação das pastas onde serão armazenadas as réplicas
```js
mkdir rs1 && mkdir rs2 && mkdir rs3
```

####Inicialização das réplicas e configuração nas suas respectivas replicaSets
```js
mongod --replSet rs1 --port 27018 --dbpath rs1
mongod --replSet rs2 --port 27019 --dbpath rs2
mongod --replSet rs3 --port 27020 --dbpath rs3
```
```js
mongo --port 27012
rs.add("127.0.0.1:27018");

{
  "ok": 1,
  "$gleStats": {
    "lastOpTime": Timestamp(1456108609, 1),
    "electionId": ObjectId("56ca6c52ef80ed727dd601da")
  }
}
```

```js
mongo --port 27013
rs.add("127.0.0.1:27019");

{
  "ok": 1,
  "$gleStats": {
    "lastOpTime": Timestamp(1456108887, 1),
    "electionId": ObjectId("56ca6d293cf173d55a5cdbaf")
  }
}
```

```js
mongo --port 27014
rs.add("127.0.0.1:27020");

{
  "ok": 1,
  "$gleStats": {
    "lastOpTime": Timestamp(1456108934, 1),
    "electionId": ObjectId("56ca6d42a5faa52a3df1baf1")
  }
}
```

####Status do sharding
```js
mongo --port 27011
db.printShardingStatus()
--- Sharding Status --- 
  sharding version: {
    "_id": 1,
    "minCompatibleVersion": 5,
    "currentVersion": 6,
    "clusterId": ObjectId("56ca6400c54d87a2169aa70c")
  }
  shards:
    {  "_id": "rs1",  "host": "rs1/127.0.0.1:27012,127.0.0.1:27018" }
    {  "_id": "rs2",  "host": "rs2/127.0.0.1:27013,127.0.0.1:27019" }
    {  "_id": "rs3",  "host": "rs3/127.0.0.1:27014,127.0.0.1:27020" }
  balancer:
	Currently enabled:  yes
	Currently running:  no
	Failed balancer rounds in last 5 attempts:  0
	Migration Results for the last 24 hours: 
		3 : Success
		1 : Failed with error 'migration already in progress', from rs2 to rs1
  databases:
    {  "_id": "admin",  "partitioned": false,  "primary": "config" }
    {  "_id": "projeto",  "partitioned": true,  "primary": "rs1" }
    projeto.activities
      shard key: { "_id": 1 }
      chunks:
        rs1: 2
        rs2: 2
        rs3: 1
        { "_id": { "$minKey" : 1 } } -> { "_id": ObjectId("56b42ce174a991a9bf77550a") } on: rs3 Timestamp(3, 0) 
        { "_id": ObjectId("56b42ce174a991a9bf77550a") } -> { "_id": ObjectId("56ca717eb287173100cc17d6") } on: rs1 Timestamp(3, 1) 
        { "_id": ObjectId("56ca717eb287173100cc17d6") } -> { "_id": ObjectId("56ca71a9b287173100cca05e") } on: rs2 Timestamp(4, 1) 
        { "_id": ObjectId("56ca71a9b287173100cca05e") } -> { "_id": ObjectId("56ca71e1b287173100cd53af") } on: rs2 Timestamp(3, 3) 
        { "_id": ObjectId("56ca71e1b287173100cd53af") } -> { "_id": { "$maxKey" : 1 } } on: rs1 Timestamp(4, 0) 
    {  "_id": "test",  "partitioned": false,  "primary": "rs1" }
```

####Status de cada replicaSet
```js
mongo --port 27012
(mongod-3.0.9)[PRIMARY:rs1] test> db.printReplicationInfo()
configured oplog size:   4078.550765991211MB
log length start to end: 2031secs (0.56hrs)
oplog first event time:  Sun Feb 21 2016 23:02:58 GMT-0300 (BRT)
oplog last event time:   Sun Feb 21 2016 23:36:49 GMT-0300 (BRT)
now:                     Sun Feb 21 2016 23:47:24 GMT-0300 (BRT)
```

```js
mongo --port 27013
(mongod-3.0.9)[PRIMARY:rs2] test> db.printReplicationInfo()
configured oplog size:   3873.847640991211MB
log length start to end: 2094secs (0.58hrs)
oplog first event time:  Sun Feb 21 2016 23:06:33 GMT-0300 (BRT)
oplog last event time:   Sun Feb 21 2016 23:41:27 GMT-0300 (BRT)
now:                     Sun Feb 21 2016 23:47:42 GMT-0300 (BRT)
```

```js
mongo --port 27014
(mongod-3.0.9)[PRIMARY:rs3] test> db.printReplicationInfo()
configured oplog size:   3669.144515991211MB
log length start to end: 2116secs (0.59hrs)
oplog first event time:  Sun Feb 21 2016 23:06:58 GMT-0300 (BRT)
oplog last event time:   Sun Feb 21 2016 23:42:14 GMT-0300 (BRT)
now:                     Sun Feb 21 2016 23:46:22 GMT-0300 (BRT)
```
