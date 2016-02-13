# MongoDb - Projeto Final
**Autor:** Welder Marcos
**Data** 1454201680248

## Para qual sistema você usaria o MogoDB (diferente desse)?

Usaria para um sistema de gerenciamento de notícias, pois acredito que o formato de documento seria o mais adequado para notícias;

## Qual a modelagem da sua coleção de `users`?
```js
User: {
	name: String,
	bio: String,
	date-register: Date,
	avatar: Binary,
	background: Binary,

	auth:{
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
```js
Project:{

	name: String,
	description: String,
	date_begin: Date,
	date_dream: Date,
	date_end: Date,
	visible: Boolean,
	realocate: Date,
	expired: Date,
	visualizable_mod: Boolean,
	tags: [String],

	members:[{
		type: [String],
		_id: ObjectId,
		notify: String
	}],

	goals:{
		name: String,
		tags: [String],
		description: String,
		date_begin: Date,
		date_dream: Date,
		date_end: Date,
		realocate: Date,
		expired: Date,
		historic: [Date],

		activity:[{
			_id: ObjectId
		}]
	}
}
```

## Qual a modelagem da sua coleção retirada de `projects`?
```js
Activity:{

	name: String,
	tags: [String],
	description: String,
	date_begin: Date,
	date_dream: Date,
	date_end: Date,
	realocate: Date,
	expired: Date,

	comments:[{
		text: String,
		date_comment: Date,

		files: [{		
			name: String,
			weight: String,
			path: String
		}]

		member: {[
			member_id: ObjectId,
			notify: String
		]}
	}]
}
```

## Create - cadastro

1. Cadastre 10 usuários diferentes.
```js
var saap = "abracadabra";

var users = [
	{"name":"Alpha", bio:"alpha bio","date-register": new Date(),avatar:null, background:null, auth:{username:"alpha",email:"alpha@email",password:saap.split("").reverse().join(""),last_access: new Date(),online: (1 / _rand() % 2) > 1 ? true : false,disabled: (1 / _rand() % 2) > 1 ? true : false,hash_token: "XkqtIWPxrcL03/tl8BTqkN4JE1sAyYWrppufitxGTez"}},
	{"name":"Beta", bio:"beta bio","date-register":new Date(),avatar:null, background:null, auth:{username:"beta",email:"beta@email",password:saap.split("").reverse().join(""),last_access: new Date(),online: (1 / _rand() % 2) > 1 ? true : false,disabled: (1 / _rand() % 2) > 1 ? true : false,hash_token: "XkqtIWPxrcL03/tl8BTqkN4JE1sAyYWrppufitxGTez"}},
	{"name":"Gama", bio:"gama bio","date-register":new Date(),avatar:null, background:null, auth:{username:"gama",email:"gama@email",password:saap.split("").reverse().join(""),last_access: new Date(),online: (1 / _rand() % 2) > 1 ? true : false,disabled: (1 / _rand() % 2) > 1 ? true : false,hash_token: "XkqtIWPxrcL03/tl8BTqkN4JE1sAyYWrppufitxGTez"}},
	{"name":"Delta", bio:"delta bio","date-register":new Date(),avatar:null, background:null, auth:{username:"delta",email:"delta@email",password:saap.split("").reverse().join(""),last_access: new Date(),online: (1 / _rand() % 2) > 1 ? true : false,disabled: (1 / _rand() % 2) > 1 ? true : false,hash_token: "XkqtIWPxrcL03/tl8BTqkN4JE1sAyYWrppufitxGTez"}},
	{"name":"Epsilon", bio:"epsilon bio","date-register":new Date(),avatar:null, background:null, auth:{username:"epsilon",email:"epsilon@email",password:saap.split("").reverse().join(""),last_access: new Date(),online: (1 / _rand() % 2) > 1 ? true : false,disabled: (1 / _rand() % 2) > 1 ? true : false,hash_token: "XkqtIWPxrcL03/tl8BTqkN4JE1sAyYWrppufitxGTez"}},
	{"name":"Zeta", bio:"zeta bio","date-register":new Date(),avatar:null, background:null, auth:{username:"zeta",email:"zeta@email",password:saap.split("").reverse().join(""),last_access: new Date(),online: (1 / _rand() % 2) > 1 ? true : false,disabled: (1 / _rand() % 2) > 1 ? true : false,hash_token: "XkqtIWPxrcL03/tl8BTqkN4JE1sAyYWrppufitxGTez"}},
	{"name":"Eta", bio:"eta bio","date-register":new Date(),avatar:null, background:null, auth:{username:"eta",email:"eta@email",password:saap.split("").reverse().join(""),last_access: new Date(),online: (1 / _rand() % 2) > 1 ? true : false,disabled: (1 / _rand() % 2) > 1 ? true : false,hash_token: "XkqtIWPxrcL03/tl8BTqkN4JE1sAyYWrppufitxGTez"}},
	{"name":"Theta", bio:"theta bio","date-register":new Date(),avatar:null, background:null, auth:{username:"theta",email:"theta@email",password:saap.split("").reverse().join(""),last_access: new Date(),online: (1 / _rand() % 2) > 1 ? true : false,disabled: (1 / _rand() % 2) > 1 ? true : false,hash_token: "XkqtIWPxrcL03/tl8BTqkN4JE1sAyYWrppufitxGTez"}},
	{"name":"Iota", bio:"iota bio","date-register":new Date(),avatar:null, background:null, auth:{username:"iota",email:"iota@email",password:saap.split("").reverse().join(""),last_access: new Date(),online: (1 / _rand() % 2) > 1 ? true : false,disabled: (1 / _rand() % 2) > 1 ? true : false,hash_token: "XkqtIWPxrcL03/tl8BTqkN4JE1sAyYWrppufitxGTez"}},
	{"name":"Kappa", bio:"kappa bio","date-register":new Date(),avatar:null, background:null, auth:{username:"kappa",email:"kappa@email",password:saap.split("").reverse().join(""),last_access: new Date(),online: (1 / _rand() % 2) > 1 ? true : false,disabled: (1 / _rand() % 2) > 1 ? true : false,hash_token: "XkqtIWPxrcL03/tl8BTqkN4JE1sAyYWrppufitxGTez"}}
]

db.users.insert(users);
```
2. Cadastre 5 projetos diferentes.
    - cada um com 5 membros, sempre diferentes dentro dos projetos;
    - cada um com pelo menos 3 tags diferentes;
        - escolha 1 *tag* onde deva ficar em 2 projetos;
        - escolha 1 *tag* onde deva ficar em 3 projetos;
    - cada projeto com pelo menos 1 *goal*;
        - cada *goal* com pelo menos 3 *tags*;
        - cada *goal* com pelo menos 2 atividades, deixe 1 projeto sem.

```js

//Atividades *************************

//inicializa a variável de atividades vazia
activity = [];

//loop de inserção de atividades
for(i = 0; i < 10; i++){

	//armazena um usuário randomico para ser usado como autor do comentário
	var u = db.users.aggregate([ { $sample: { size: 1 } } ])

	//incrementa o array de atividades
	activity.push({
		"name": "Activity " + (i+1),
		"tags": ["Activity " + (i+1)],
		"description": "Activity " + (i+1) + " description",
		"date_begin": new Date(),
		"date_dream": new Date(),
		"date_end": new Date(),
		"realocate": new Date(),
		"expired": new Date(),

		 comments:[{
		 	"text": "Activity "+ (i+1) + " comment",
		 	"date_comment": new Date(),

			files: [{
				"name": "Activity "+ (i+1) + " file",
				"weight": (_rand() * 100) + "kb",
				"path": "/home/Activity"+ (i+1) + "/"
			}],

			member: [{
				"member_id": u.result[0]._id,
				"notify": "Always"
			}]
		 }]
	})
}

//insere o array de atividade na coleção de atividades
db.activities.insert(activity);

//Projetos *************************

//Array de projetos
var project = [];

//Array de ids de todas os usuários cadastrados
var u = db.users.find({},{_id: 1});

//Array de ids de todas as atividades cadastradas
var a = db.activities.find({},{_id: 1});

//loop de criação dos projetos
for(i=0; i<5; i++){

	//Array de membros, precisa estar limpo a cada projeto para que sejam usuários diferentes
	member = []

	//loop de inicialização dos dados de cada membro
	for(j=0; j<5; j++){
			member.push({
				"type": (j == 0) ? "Leader" : "Staff "+j,
				"_id": u[parseInt(_rand() * 10)]._id,
				"notify": (j == 0) ? "Always" : "Never"
			})
	}

	//incrementando o Array de projetos a cada iteração
	project.push({
		"name": "Projeto "+(i+1),
		"description": "Descrição do projeto "+(i+1),
		"date_begin": new Date(new Date().getTime() - 24 * 60 * 60 * 1000),
		"date_dream": new Date(),
		"date_end": new Date(new Date().getTime() + 24 * 60 * 60 * 1000),
		"visible": (1 / _rand() % 2) > 1 ? true : false,disabled: (1 / _rand() % 2) > 1 ? true : false,
		"realocate": new Date(new Date().getTime() - (24 * 10) * 60 * 60 * 1000),
		"expired": new Date(new Date().getTime() + (24 * 10) * 60 * 60 * 1000),
		"visualizable_mod": (1 / _rand() % 2) > 1 ? true : false,"disabled": (1 / _rand() % 2) > 1 ? true : false,
		"tags": [(i < 2) ? "MongoDB" : "Be-Mean", "Modulo " +(i+1)+ " MongoDB", "Aula " + parseInt((_rand() * 10))],

		//Inserindo os objetos de membros previamente inicializados
		members:[
			member[0],
			member[1],
			member[2],
			member[3],
			member[4]
		],

		//inserindo 1 goal a cada projeto
		goals:[{
				"name": "Goal " + parseInt(_rand() * 100) + " do projeto " + (i+1),
				"tags": ["Project " + (i+1), "Project Leader: " + member[0]._id, "Duração: " + parseInt(_rand() * 10) + " Semanas"],
				"description": "Project " +(i+1)+ " goal",
				"date_begin": new Date(new Date().getTime() - 24 * 60 * 60 * 1000),
				"date_dream": new Date,
				"date_end": new Date(new Date().getTime() + 24 * 60 * 60 * 1000),
				"realocate": new Date(new Date().getTime() - (24 * 10) * 60 * 60 * 1000),
				"expired": new Date(new Date().getTime() + (24 * 200) * 60 * 60 * 1000),
				"historic": [new Date(),new Date(new Date().getTime() + 24 * 60 * 60 * 1000)],

				//inserindo 2 atividades da lista de atividades previamente populada e pulando o ultimo projeto
				"activity": (i > 3) ? [] : [{"_id":a[parseInt(_rand() * 10)]._id},{"_id":a[parseInt(_rand() * 10)]._id}]
		}]
	})
}

db.projects.insert(project);
```

## Retrieve - busca

1. Liste as informações dos membros de 1 projeto específico que deve ser buscado pelo seu nome de forma a não ligar para maiúsculas e minúsculas.

```js
Welder-Mint(mongod-3.2.1) test> var usr = db.projects.find({name: /projeto 5/i}, {"members._id": 1, _id: 0}).toArray()
Welder-Mint(mongod-3.2.1) test> var result = usr[0].members.map(function(a) {return a._id;});
Welder-Mint(mongod-3.2.1) test> db.users.find({_id: {$in: result}})
{
  "_id": ObjectId("56b5e9421c02b37364687882"),
  "name": "Alpha",
  "bio": "alpha bio",
  "date-register": ISODate("2016-02-06T12:33:56.346Z"),
  "avatar": null,
  "background": null,
  "auth": {
    "username": "alpha",
    "email": "alpha@email",
    "password": "arbadacarba",
    "last_access": ISODate("2016-02-06T12:33:56.346Z"),
    "online": true,
    "disabled": true,
    "hash_token": "XkqtIWPxrcL03/tl8BTqkN4JE1sAyYWrppufitxGTez"
  }
}
{
  "_id": ObjectId("56b5e9421c02b37364687883"),
  "name": "Beta",
  "bio": "beta bio",
  "date-register": ISODate("2016-02-06T12:33:56.346Z"),
  "avatar": null,
  "background": null,
  "auth": {
    "username": "beta",
    "email": "beta@email",
    "password": "arbadacarba",
    "last_access": ISODate("2016-02-06T12:33:56.346Z"),
    "online": false,
    "disabled": false,
    "hash_token": "XkqtIWPxrcL03/tl8BTqkN4JE1sAyYWrppufitxGTez"
  }
}
{
  "_id": ObjectId("56b5e9421c02b37364687885"),
  "name": "Delta",
  "bio": "delta bio",
  "date-register": ISODate("2016-02-06T12:33:56.346Z"),
  "avatar": null,
  "background": null,
  "auth": {
    "username": "delta",
    "email": "delta@email",
    "password": "arbadacarba",
    "last_access": ISODate("2016-02-06T12:33:56.346Z"),
    "online": true,
    "disabled": true,
    "hash_token": "XkqtIWPxrcL03/tl8BTqkN4JE1sAyYWrppufitxGTez"
  }
}
{
  "_id": ObjectId("56b5e9421c02b37364687886"),
  "name": "Epsilon",
  "bio": "epsilon bio",
  "date-register": ISODate("2016-02-06T12:33:56.346Z"),
  "avatar": null,
  "background": null,
  "auth": {
    "username": "epsilon",
    "email": "epsilon@email",
    "password": "arbadacarba",
    "last_access": ISODate("2016-02-06T12:33:56.346Z"),
    "online": true,
    "disabled": true,
    "hash_token": "XkqtIWPxrcL03/tl8BTqkN4JE1sAyYWrppufitxGTez"
  }
}
{
  "_id": ObjectId("56b5e9421c02b37364687888"),
  "name": "Eta",
  "bio": "eta bio",
  "date-register": ISODate("2016-02-06T12:33:56.346Z"),
  "avatar": null,
  "background": null,
  "auth": {
    "username": "eta",
    "email": "eta@email",
    "password": "arbadacarba",
    "last_access": ISODate("2016-02-06T12:33:56.346Z"),
    "online": true,
    "disabled": true,
    "hash_token": "XkqtIWPxrcL03/tl8BTqkN4JE1sAyYWrppufitxGTez"
  }
}

```

2. Liste todos os projetos com a tag que você escolheu para os 3 projetos em comum.

```js
Welder-Mint(mongod-3.2.1) test> db.projects.find({tags: {$in: [/be-mean/i]}})
{
  "_id": ObjectId("56b9d98966ec2917eadf0e12"),
  "name": "Projeto 3",
  "description": "Descrição do projeto 3",
  "date_begin": ISODate("2016-02-08T12:20:22.061Z"),
  "date_dream": ISODate("2016-02-09T12:20:22.061Z"),
  "date_end": ISODate("2016-02-10T12:20:22.061Z"),
  "visible": false,
  "disabled": true,
  "realocate": ISODate("2016-01-30T12:20:22.061Z"),
  "expired": ISODate("2016-02-19T12:20:22.061Z"),
  "visualizable_mod": false,
  "tags": [
    "Be-Mean",
    "Modulo 3 MongoDB",
    "Aula 9"
  ],
  "members": [
    {
      "type": "Leader",
      "_id": ObjectId("56b5e9421c02b37364687883"),
      "notify": "Always"
    },
    {
      "type": "Staff 1",
      "_id": ObjectId("56b5e9421c02b3736468788a"),
      "notify": "Never"
    },
    {
      "type": "Staff 2",
      "_id": ObjectId("56b5e9421c02b37364687889"),
      "notify": "Never"
    },
    {
      "type": "Staff 3",
      "_id": ObjectId("56b5e9421c02b37364687883"),
      "notify": "Never"
    },
    {
      "type": "Staff 4",
      "_id": ObjectId("56b5e9421c02b37364687887"),
      "notify": "Never"
    }
  ],
  "goals": [
    {
      "name": "Goal 1 do projeto 3",
      "tags": [
        "Project 3",
        "Project Leader: 56b5e9421c02b37364687883",
        "Duração: 8 Semanas"
      ],
      "description": "Project 3 goal",
      "date_begin": ISODate("2016-02-08T12:20:22.061Z"),
      "date_dream": ISODate("2016-02-09T12:20:22.061Z"),
      "date_end": ISODate("2016-02-10T12:20:22.061Z"),
      "realocate": ISODate("2016-01-30T12:20:22.061Z"),
      "expired": ISODate("2016-08-27T12:20:22.061Z"),
      "historic": [
        ISODate("2016-02-09T12:20:22.061Z"),
        ISODate("2016-02-10T12:20:22.061Z")
      ],
      "activity": [
        {
          "_id": ObjectId("56b91bef55a602024aa2cc0f")
        },
        {
          "_id": ObjectId("56b91bef55a602024aa2cc13")
        }
      ]
    }
  ]
}
{
  "_id": ObjectId("56b9d98966ec2917eadf0e13"),
  "name": "Projeto 4",
  "description": "Descrição do projeto 4",
  "date_begin": ISODate("2016-02-08T12:20:22.061Z"),
  "date_dream": ISODate("2016-02-09T12:20:22.061Z"),
  "date_end": ISODate("2016-02-10T12:20:22.061Z"),
  "visible": true,
  "disabled": true,
  "realocate": ISODate("2016-01-30T12:20:22.061Z"),
  "expired": ISODate("2016-02-19T12:20:22.061Z"),
  "visualizable_mod": true,
  "tags": [
    "Be-Mean",
    "Modulo 4 MongoDB",
    "Aula 8"
  ],
  "members": [
    {
      "type": "Leader",
      "_id": ObjectId("56b5e9421c02b37364687886"),
      "notify": "Always"
    },
    {
      "type": "Staff 1",
      "_id": ObjectId("56b5e9421c02b3736468788a"),
      "notify": "Never"
    },
    {
      "type": "Staff 2",
      "_id": ObjectId("56b5e9421c02b3736468788b"),
      "notify": "Never"
    },
    {
      "type": "Staff 3",
      "_id": ObjectId("56b5e9421c02b37364687885"),
      "notify": "Never"
    },
    {
      "type": "Staff 4",
      "_id": ObjectId("56b5e9421c02b37364687885"),
      "notify": "Never"
    }
  ],
  "goals": [
    {
      "name": "Goal 43 do projeto 4",
      "tags": [
        "Project 4",
        "Project Leader: 56b5e9421c02b37364687886",
        "Duração: 4 Semanas"
      ],
      "description": "Project 4 goal",
      "date_begin": ISODate("2016-02-08T12:20:22.061Z"),
      "date_dream": ISODate("2016-02-09T12:20:22.061Z"),
      "date_end": ISODate("2016-02-10T12:20:22.061Z"),
      "realocate": ISODate("2016-01-30T12:20:22.061Z"),
      "expired": ISODate("2016-08-27T12:20:22.061Z"),
      "historic": [
        ISODate("2016-02-09T12:20:22.061Z"),
        ISODate("2016-02-10T12:20:22.061Z")
      ],
      "activity": [
        {
          "_id": ObjectId("56b91bef55a602024aa2cc0f")
        },
        {
          "_id": ObjectId("56b91bef55a602024aa2cc0e")
        }
      ]
    }
  ]
}
{
  "_id": ObjectId("56b9d98966ec2917eadf0e14"),
  "name": "Projeto 5",
  "description": "Descrição do projeto 5",
  "date_begin": ISODate("2016-02-08T12:20:22.061Z"),
  "date_dream": ISODate("2016-02-09T12:20:22.061Z"),
  "date_end": ISODate("2016-02-10T12:20:22.061Z"),
  "visible": true,
  "disabled": true,
  "realocate": ISODate("2016-01-30T12:20:22.061Z"),
  "expired": ISODate("2016-02-19T12:20:22.061Z"),
  "visualizable_mod": true,
  "tags": [
    "Be-Mean",
    "Modulo 5 MongoDB",
    "Aula 0"
  ],
  "members": [
    {
      "type": "Leader",
      "_id": ObjectId("56b5e9421c02b37364687888"),
      "notify": "Always"
    },
    {
      "type": "Staff 1",
      "_id": ObjectId("56b5e9421c02b37364687886"),
      "notify": "Never"
    },
    {
      "type": "Staff 2",
      "_id": ObjectId("56b5e9421c02b37364687883"),
      "notify": "Never"
    },
    {
      "type": "Staff 3",
      "_id": ObjectId("56b5e9421c02b37364687885"),
      "notify": "Never"
    },
    {
      "type": "Staff 4",
      "_id": ObjectId("56b5e9421c02b37364687882"),
      "notify": "Never"
    }
  ],
  "goals": [
    {
      "name": "Goal 65 do projeto 5",
      "tags": [
        "Project 5",
        "Project Leader: 56b5e9421c02b37364687888",
        "Duração: 1 Semanas"
      ],
      "description": "Project 5 goal",
      "date_begin": ISODate("2016-02-08T12:20:22.061Z"),
      "date_dream": ISODate("2016-02-09T12:20:22.061Z"),
      "date_end": ISODate("2016-02-10T12:20:22.061Z"),
      "realocate": ISODate("2016-01-30T12:20:22.061Z"),
      "expired": ISODate("2016-08-27T12:20:22.061Z"),
      "historic": [
        ISODate("2016-02-09T12:20:22.061Z"),
        ISODate("2016-02-10T12:20:22.061Z")
      ],
      "activity": [ ]
    }
  ]
}
```

3. Liste apenas os nomes de todas as atividades para todos os projetos.

```js
//objeto que receberá os nomes das atividades dos projetos
var projects {}

//array com o nomes dos projetos e os _ids das atividades contidas neles
var array = db.projects.find({},{name: 1, "goals.activity._id": 1}).toArray()

//objeto que receberá o nome da atividade
var actName = {}

//pra cada projeto
array.forEach(function(arr){

	//inicializa-se um array de atividades com o nome do projeto
	projects[arr.name] = []

	//pra cada goal no projeto
	arr.goals.forEach(function(goal){

		//pra cada atividade no goal
		goal.activity.forEach(function(act){

			//amazeno o nome da atividade com aquele id
			actName = db.activities.findOne({_id: act._id},{_id:0, name: 1})

			//e insiro no array de atividades com o nome do projeto que estou
			projects[arr.name].push(actName.name)
		})
	})
})

Welder-Mint(mongod-3.2.1) test> projects
{
  "Projeto 1": [
    "Activity 7",
    "Activity 9"
  ],
  "Projeto 2": [
    "Activity 7",
    "Activity 4"
  ],
  "Projeto 3": [
    "Activity 4",
    "Activity 8"
  ],
  "Projeto 4": [
    "Activity 4",
    "Activity 3"
  ],
  "Projeto 5": [ ]
}
```

4. Liste todos os projetos que não possuam uma tag.

```js
Welder-Mint(mongod-3.2.1) test> db.projects.find({tags:{$nin: [/be-mean/i]}})
{
  "_id": ObjectId("56b9d98966ec2917eadf0e10"),
  "name": "Projeto 1",
  "description": "Descrição do projeto 1",
  "date_begin": ISODate("2016-02-08T12:20:22.060Z"),
  "date_dream": ISODate("2016-02-09T12:20:22.060Z"),
  "date_end": ISODate("2016-02-10T12:20:22.060Z"),
  "visible": true,
  "disabled": false,
  "realocate": ISODate("2016-01-30T12:20:22.060Z"),
  "expired": ISODate("2016-02-19T12:20:22.060Z"),
  "visualizable_mod": true,
  "tags": [
    "MongoDB",
    "Modulo 1 MongoDB",
    "Aula 1"
  ],
  "members": [
    {
      "type": "Leader",
      "_id": ObjectId("56b5e9421c02b3736468788b"),
      "notify": "Always"
    },
    {
      "type": "Staff 1",
      "_id": ObjectId("56b5e9421c02b37364687888"),
      "notify": "Never"
    },
    {
      "type": "Staff 2",
      "_id": ObjectId("56b5e9421c02b3736468788a"),
      "notify": "Never"
    },
    {
      "type": "Staff 3",
      "_id": ObjectId("56b5e9421c02b37364687886"),
      "notify": "Never"
    },
    {
      "type": "Staff 4",
      "_id": ObjectId("56b5e9421c02b37364687888"),
      "notify": "Never"
    }
  ],
  "goals": [
    {
      "name": "Goal 36 do projeto 1",
      "tags": [
        "Project 1",
        "Project Leader: 56b5e9421c02b3736468788b",
        "Duração: 6 Semanas"
      ],
      "description": "Project 1 goal",
      "date_begin": ISODate("2016-02-08T12:20:22.060Z"),
      "date_dream": ISODate("2016-02-09T12:20:22.060Z"),
      "date_end": ISODate("2016-02-10T12:20:22.060Z"),
      "realocate": ISODate("2016-01-30T12:20:22.060Z"),
      "expired": ISODate("2016-08-27T12:20:22.060Z"),
      "historic": [
        ISODate("2016-02-09T12:20:22.060Z"),
        ISODate("2016-02-10T12:20:22.060Z")
      ],
      "activity": [
        {
          "_id": ObjectId("56b91bef55a602024aa2cc12")
        },
        {
          "_id": ObjectId("56b91bef55a602024aa2cc14")
        }
      ]
    }
  ]
}
{
  "_id": ObjectId("56b9d98966ec2917eadf0e11"),
  "name": "Projeto 2",
  "description": "Descrição do projeto 2",
  "date_begin": ISODate("2016-02-08T12:20:22.061Z"),
  "date_dream": ISODate("2016-02-09T12:20:22.061Z"),
  "date_end": ISODate("2016-02-10T12:20:22.061Z"),
  "visible": true,
  "disabled": false,
  "realocate": ISODate("2016-01-30T12:20:22.061Z"),
  "expired": ISODate("2016-02-19T12:20:22.061Z"),
  "visualizable_mod": true,
  "tags": [
    "MongoDB",
    "Modulo 2 MongoDB",
    "Aula 6"
  ],
  "members": [
    {
      "type": "Leader",
      "_id": ObjectId("56b5e9421c02b37364687885"),
      "notify": "Always"
    },
    {
      "type": "Staff 1",
      "_id": ObjectId("56b5e9421c02b3736468788a"),
      "notify": "Never"
    },
    {
      "type": "Staff 2",
      "_id": ObjectId("56b5e9421c02b3736468788a"),
      "notify": "Never"
    },
    {
      "type": "Staff 3",
      "_id": ObjectId("56b5e9421c02b37364687886"),
      "notify": "Never"
    },
    {
      "type": "Staff 4",
      "_id": ObjectId("56b5e9421c02b3736468788a"),
      "notify": "Never"
    }
  ],
  "goals": [
    {
      "name": "Goal 66 do projeto 2",
      "tags": [
        "Project 2",
        "Project Leader: 56b5e9421c02b37364687885",
        "Duração: 5 Semanas"
      ],
      "description": "Project 2 goal",
      "date_begin": ISODate("2016-02-08T12:20:22.061Z"),
      "date_dream": ISODate("2016-02-09T12:20:22.061Z"),
      "date_end": ISODate("2016-02-10T12:20:22.061Z"),
      "realocate": ISODate("2016-01-30T12:20:22.061Z"),
      "expired": ISODate("2016-08-27T12:20:22.061Z"),
      "historic": [
        ISODate("2016-02-09T12:20:22.061Z"),
        ISODate("2016-02-10T12:20:22.061Z")
      ],
      "activity": [
        {
          "_id": ObjectId("56b91bef55a602024aa2cc12")
        },
        {
          "_id": ObjectId("56b91bef55a602024aa2cc0f")
        }
      ]
    }
  ]
}
Fetched 2 record(s) in 89ms
```

5. Liste todos os usuários que não fazem parte do primeiro projeto cadastrado.

```js
//armazena todos os usuários de todos os projetos em um array
var usrs = db.projects.find({},{_id:0, "members._id":1}).toArray()

//inicializa o array que armazenará os _ids dos membros do primeiro projeto
var ids = []

//para cada membro do projeto na posição 0 do array de projetos
usrs[0].members.forEach(function(member){
	ids.push(member._id)
})
//armazena o array com o nome dos usuários que não estão no primeiro projeto
var result = db.users.find({_id: {$nin: ids}},{_id:0, name:1})

//inicializa um array de nomes (apenas por estética do resultado)
names = []

//para cada objeto no array de usuários armazena-se somente seu nome no array de nomes
result.forEach(function(name){
	names.push(name.name)
})

Welder-Mint(mongod-3.2.1) test> names
[
  "Alpha",
  "Beta",
  "Gama",
  "Delta",
  "Zeta",
  "Theta"
]
```

## Update - alteração

1. Adicione para todos os projetos o campo `views: 0`.

```js
Welder-Mint(mongod-3.2.1) test> db.projects.update({},{$set: {"views": 0}},{multi: true, upsert: true})
Updated 5 existing record(s) in 2ms
WriteResult({
  "nMatched": 5,
  "nUpserted": 0,
  "nModified": 5
})
Welder-Mint(mongod-3.2.1) test> db.projects.find({},{name: 1, views:1})
{
  "_id": ObjectId("56b9d98966ec2917eadf0e10"),
  "name": "Projeto 1",
  "views": 0
}
{
  "_id": ObjectId("56b9d98966ec2917eadf0e11"),
  "name": "Projeto 2",
  "views": 0
}
{
  "_id": ObjectId("56b9d98966ec2917eadf0e12"),
  "name": "Projeto 3",
  "views": 0
}
{
  "_id": ObjectId("56b9d98966ec2917eadf0e13"),
  "name": "Projeto 4",
  "views": 0
}
{
  "_id": ObjectId("56b9d98966ec2917eadf0e14"),
  "name": "Projeto 5",
  "views": 0
}
Fetched 5 record(s) in 3ms
```

2. Adicione 1 tag diferente para cada projeto.

```js
Welder-Mint(mongod-3.2.1) test> db.projects.update({},{$push: {tags: "NoSQL"}}, {multi: true})
Updated 5 existing record(s) in 1ms
WriteResult({
  "nMatched": 5,
  "nUpserted": 0,
  "nModified": 5
})
Welder-Mint(mongod-3.2.1) test> db.projects.find({},{name:1, tags: 1})
{
  "_id": ObjectId("56b9d98966ec2917eadf0e10"),
  "name": "Projeto 1",
  "tags": [
    "MongoDB",
    "Modulo 1 MongoDB",
    "Aula 1",
    "NoSQL"
  ]
}
{
  "_id": ObjectId("56b9d98966ec2917eadf0e11"),
  "name": "Projeto 2",
  "tags": [
    "MongoDB",
    "Modulo 2 MongoDB",
    "Aula 6",
    "NoSQL"
  ]
}
{
  "_id": ObjectId("56b9d98966ec2917eadf0e12"),
  "name": "Projeto 3",
  "tags": [
    "Be-Mean",
    "Modulo 3 MongoDB",
    "Aula 9",
    "NoSQL"
  ]
}
{
  "_id": ObjectId("56b9d98966ec2917eadf0e13"),
  "name": "Projeto 4",
  "tags": [
    "Be-Mean",
    "Modulo 4 MongoDB",
    "Aula 8",
    "NoSQL"
  ]
}
{
  "_id": ObjectId("56b9d98966ec2917eadf0e14"),
  "name": "Projeto 5",
  "tags": [
    "Be-Mean",
    "Modulo 5 MongoDB",
    "Aula 0",
    "NoSQL"
  ]
}
Fetched 5 record(s) in 2ms
```

3. Adicione 2 membros diferentes para cada projeto.

```js
//array de projetos com seus _ids, nomes e membros
var projects = db.projects.find({},{_id:1, name:1, "members._id": 1}).toArray()

//array de _ids de todos os users
var users = db.users.find({},{_id:1}).toArray()

//variável que para verificação da pré existência do membro no projeto
var teste = 0

//para cada projeto
projects.forEach(function(proj){
	var projUsers = []

	//armazena o array dos membros do projeto atual
	for(i = 0; i < proj.members.length; i++){
		projUsers.push(proj.members[i]._id)
	}

	//booleano que valida se os membros gerados já existem na lista de membros do projeto
	var proceed = false
	var aux = {}
	var aux2 = {}

	while(!proceed){
		//membros extras com _ids randomicos da lista de usuários
		aux.type = "Extra Staff"
		aux._id = users[parseInt(_rand() * 10)]._id
		aux.notify = "Sometimes"

		aux2.type = "Extra Staff"
		aux2._id = users[parseInt(_rand() * 10)]._id
		aux2.notify = "Sometimes"

		projUsers.forEach(function(u){
			if(aux._id.equals(u) || aux2._id.equals(u) || aux2._id == aux._id){
				//se algum dos membros gerados já esxiste no projeto incrementa
				teste++
			}
		})

		//se incrementou alguma vez é porque o teste falhou
		if(teste > 0){
			teste = 0

		//do contrario os membros não fazem parte do projeto e podem ser inseridos
		}else{
			proceed = true

			db.projects.update({_id: proj._id},{$push: {members: aux}}, {upsert: true})
			db.projects.update({_id: proj._id},{$push: {members: aux2}}, {upsert: true})
		}
	}
})

Welder-Mint(mongod-3.2.1) test> db.projects.find({},{name:1, members: 1})
{
  "_id": ObjectId("56b9d98966ec2917eadf0e10"),
  "name": "Projeto 1",
  "members": [
    {
      "type": "Leader",
      "_id": ObjectId("56b5e9421c02b3736468788b"),
      "notify": "Always"
    },
    {
      "type": "Staff 1",
      "_id": ObjectId("56b5e9421c02b37364687888"),
      "notify": "Never"
    },
    {
      "type": "Staff 2",
      "_id": ObjectId("56b5e9421c02b3736468788a"),
      "notify": "Never"
    },
    {
      "type": "Staff 3",
      "_id": ObjectId("56b5e9421c02b37364687886"),
      "notify": "Never"
    },
    {
      "type": "Staff 4",
      "_id": ObjectId("56b5e9421c02b37364687888"),
      "notify": "Never"
    },
    {
      "type": "Extra Staff",
      "_id": ObjectId("56b5e9421c02b37364687885"),
      "notify": "Sometimes"
    },
    {
      "type": "Extra Staff",
      "_id": ObjectId("56b5e9421c02b37364687884"),
      "notify": "Sometimes"
    }
  ]
}
{
  "_id": ObjectId("56b9d98966ec2917eadf0e11"),
  "name": "Projeto 2",
  "members": [
    {
      "type": "Leader",
      "_id": ObjectId("56b5e9421c02b37364687885"),
      "notify": "Always"
    },
    {
      "type": "Staff 1",
      "_id": ObjectId("56b5e9421c02b3736468788a"),
      "notify": "Never"
    },
    {
      "type": "Staff 2",
      "_id": ObjectId("56b5e9421c02b3736468788a"),
      "notify": "Never"
    },
    {
      "type": "Staff 3",
      "_id": ObjectId("56b5e9421c02b37364687886"),
      "notify": "Never"
    },
    {
      "type": "Staff 4",
      "_id": ObjectId("56b5e9421c02b3736468788a"),
      "notify": "Never"
    },
    {
      "type": "Extra Staff",
      "_id": ObjectId("56b5e9421c02b37364687882"),
      "notify": "Sometimes"
    },
    {
      "type": "Extra Staff",
      "_id": ObjectId("56b5e9421c02b3736468788b"),
      "notify": "Sometimes"
    }
  ]
}
{
  "_id": ObjectId("56b9d98966ec2917eadf0e12"),
  "name": "Projeto 3",
  "members": [
    {
      "type": "Leader",
      "_id": ObjectId("56b5e9421c02b37364687883"),
      "notify": "Always"
    },
    {
      "type": "Staff 1",
      "_id": ObjectId("56b5e9421c02b3736468788a"),
      "notify": "Never"
    },
    {
      "type": "Staff 2",
      "_id": ObjectId("56b5e9421c02b37364687889"),
      "notify": "Never"
    },
    {
      "type": "Staff 3",
      "_id": ObjectId("56b5e9421c02b37364687883"),
      "notify": "Never"
    },
    {
      "type": "Staff 4",
      "_id": ObjectId("56b5e9421c02b37364687887"),
      "notify": "Never"
    },
    {
      "type": "Extra Staff",
      "_id": ObjectId("56b5e9421c02b3736468788b"),
      "notify": "Sometimes"
    },
    {
      "type": "Extra Staff",
      "_id": ObjectId("56b5e9421c02b37364687884"),
      "notify": "Sometimes"
    }
  ]
}
{
  "_id": ObjectId("56b9d98966ec2917eadf0e13"),
  "name": "Projeto 4",
  "members": [
    {
      "type": "Leader",
      "_id": ObjectId("56b5e9421c02b37364687886"),
      "notify": "Always"
    },
    {
      "type": "Staff 1",
      "_id": ObjectId("56b5e9421c02b3736468788a"),
      "notify": "Never"
    },
    {
      "type": "Staff 2",
      "_id": ObjectId("56b5e9421c02b3736468788b"),
      "notify": "Never"
    },
    {
      "type": "Staff 3",
      "_id": ObjectId("56b5e9421c02b37364687885"),
      "notify": "Never"
    },
    {
      "type": "Staff 4",
      "_id": ObjectId("56b5e9421c02b37364687885"),
      "notify": "Never"
    },
    {
      "type": "Extra Staff",
      "_id": ObjectId("56b5e9421c02b37364687882"),
      "notify": "Sometimes"
    },
    {
      "type": "Extra Staff",
      "_id": ObjectId("56b5e9421c02b37364687884"),
      "notify": "Sometimes"
    }
  ]
}
{
  "_id": ObjectId("56b9d98966ec2917eadf0e14"),
  "name": "Projeto 5",
  "members": [
    {
      "type": "Leader",
      "_id": ObjectId("56b5e9421c02b37364687888"),
      "notify": "Always"
    },
    {
      "type": "Staff 1",
      "_id": ObjectId("56b5e9421c02b37364687886"),
      "notify": "Never"
    },
    {
      "type": "Staff 2",
      "_id": ObjectId("56b5e9421c02b37364687883"),
      "notify": "Never"
    },
    {
      "type": "Staff 3",
      "_id": ObjectId("56b5e9421c02b37364687885"),
      "notify": "Never"
    },
    {
      "type": "Staff 4",
      "_id": ObjectId("56b5e9421c02b37364687882"),
      "notify": "Never"
    },
    {
      "type": "Extra Staff",
      "_id": ObjectId("56b5e9421c02b37364687889"),
      "notify": "Sometimes"
    },
    {
      "type": "Extra Staff",
      "_id": ObjectId("56b5e9421c02b3736468788b"),
      "notify": "Sometimes"
    }
  ]
}
Fetched 5 record(s) in 9ms
```

4. Adicione 1 comentário em cada atividade, deixe apenas 1 projeto sem.

```js
//array de atividades
var atividades = db.activities.find().toArray()

//inicia o objeto vazio de comentário a ser inserido
var comment = {}

//para cada atividade
atividades.forEach(function(act){
	//array de usuários apenas para escolher um autor randomico do comentário
	var u = db.users.find().toArray()

	//popula o comentário com valores referentes a atividade (sobrscreve o conteúdo do segundo em diante)
	comment = {
		text: act.name + " second comment",
		date_comment: new Date(),

		files: [{
			name: act.name + " second file",
			weight: (_rand() * 100) + "kb",
			path: "/home/Activity"+ (i+1) + "/"
		}],

		member: [{
			"member_id": u[parseInt(_rand() * 10)]._id,
			"notify": "Always"
		}]
	}

	//insere o comentário criado na lista de comentarios coM $PUSH
	db.activities.update({_id: act._id},{$push: {comments: comment}})

})

Welder-Mint(mongod-3.2.1) test> db.activities.findOne({},{comments: 1})
{
  "_id": ObjectId("56b91bef55a602024aa2cc0c"),
  "comments": [
    {
      "text": "Activity 1 comment",
      "date_comment": ISODate("2016-02-08T22:49:44.974Z"),
      "files": [
        {
          "name": "Activity 1 file",
          "weight": "0.04714746028184891kb",
          "path": "/home/Activity1/"
        }
      ],
      "member": [
        {
          "member_id": ObjectId("56b5e9421c02b37364687888"),
          "notify": "Always"
        }
      ]
    },
    {
      "text": "Acitivity Activity 1 second comment",
      "date_comment": ISODate("2016-02-12T23:03:41.512Z"),
      "files": [
        {
          "name": "Acitivity Activity 1 second file",
          "weight": "94.70539512112737kb",
          "path": "/home/Activity6/"
        }
      ],
      "member": [
        {
          "member_id": ObjectId("56b5e9421c02b3736468788b"),
          "notify": "Always"
        }
      ]
    }
  ]
}
```
5. Adicione 1 projeto inteiro com **UPSERT**.

```js
var project  = {
	name: "Null Project",
	description: null,
	date_begin: null,
	date_dream: null,
	date_end: null,
	visible: null,
	realocate: null,
	expired: null,
	visualizable_mod: null,
	tags: [],

	members:[{
		type: [],
		_id: null,
		notify: null
	}],

	goals:{
		name: null,
		tags: [],
		description: null,
		date_begin: null,
		date_dream: null,
		date_end: null,
		realocate: null,
		expired: null,
		historic: [],

		activity:[{
			_id: null
		}]
	}
}

Welder-Mint(mongod-3.2.1) test> db.projects.update({name: "Null Project"},project, {upsert: true})
Updated 1 new record(s) in 1ms
WriteResult({
  "nMatched": 0,
  "nUpserted": 1,
  "nModified": 0,
  "_id": ObjectId("56be6aee7fab87fbbe3bace6")
})

```
## Delete - remoção

1. Apague todos os projetos que não possuam *tags*.

```js
Welder-Mint(mongod-3.2.1) test> db.projects.remove({tags: []})
Removed 1 record(s) in 2ms
WriteResult({
  "nRemoved": 1
})
```
2. Apague todos os projetos que não possuam comentários nas atividades.
```js
//array de projetos
var projects = db.projects.find({},{name: 1, "goals.activity": 1}).toArray()

//array de atividades sem comentários
var noComments = db.activities.find({comments: []},{name:1, comments: 1}).toArray()

//array dos _ids de projetos a serem deletados
var delete = []

//para cada projeto
projects.forEach(function(pro){
	//para cada atividade no goal (só usei a posição 0 de goals pq só cadastrei um por projeto, do contrario usária outro forEach
	pro.goals[0].activity.forEach(function(act) {
		//para cada atividade que não possui comentários (não consegui fazer a função in ou indexOf funcionar)
		noComments.forEach(function(noComment){
			//se a atividade for igual a alguma das atividades sem comentários
			if(act._id == noComment._id){
				//armazena o _id do projeto pra ser deletados
				delete.push(pro._id)
			}
		})
	})
})

//para cada projeto que possui atividades sem comentários
delete.forEach(function(del) {
	db.projects.remove(_id: del)
})
```
3. Apague todos os projetos que não possuam atividades.
```js
Welder-Mint(mongod-3.2.1) test> db.projects.remove({"goals.activity": []},{justOne: false})
Removed 1 record(s) in 3ms
WriteResult({
  "nRemoved": 1
})
```
4. Escolha 2 usuário e apague todos os projetos em que os 2 fazem parte.
```js
Welder-Mint(mongod-3.2.1) test> var usersRmv = db.users.find({name: {$in: ["Gama","Beta"]}},{name:1}).toArray()
Welder-Mint(mongod-3.2.1) test>
Welder-Mint(mongod-3.2.1) test> var projects = db.projects.find({},{name:1, members:1}).toArray()
Welder-Mint(mongod-3.2.1) test>
Welder-Mint(mongod-3.2.1) test> var delet = []
Welder-Mint(mongod-3.2.1) test>
Welder-Mint(mongod-3.2.1) test> projects.forEach(function(pro){
... pro.members.forEach(function(member){
... usersRmv.forEach(function(rmv){
... if(rmv._id.equals(member._id)){
... print(pro.name+ " deve morrer por contem " +rmv.name+ " em seu time")
...
... var i = 0
...
... delet.forEach(function(del){
... if(del.equals(pro._id)){
... i++
... }
... })
...
... if(i == 0){
... delet.push(pro._id)
... }
... }
... })
... })
... })
Projeto 1 deve morrer por contem Gama em seu time
Projeto 3 deve morrer por contem Beta em seu time
Projeto 3 deve morrer por contem Beta em seu time
Projeto 3 deve morrer por contem Gama em seu time
Projeto 4 deve morrer por contem Gama em seu time
Welder-Mint(mongod-3.2.1) test>
Welder-Mint(mongod-3.2.1) test> delet.forEach(function(del){
... db.projects.remove({_id: del})
... })
Removed 1 record(s) in 2ms
Removed 1 record(s) in 2ms
Removed 1 record(s) in 2ms
```
5. Apague todos os projetos que possuam uma determinada *tag* em *goal*.
```js
Welder-Mint(mongod-3.2.1) test> db.projects.remove({"goals.tags": "Project 2"},{justOne: false})
Removed 1 record(s) in 2ms
WriteResult({
  "nRemoved": 1
})
```

## Gerenciamento de usuários

1. Crie um usuário com permissões APENAS de Leitura.
```js
```

2. Crie um usuário com permissões de Escrita e Leitura.
```js
```

3. Adicionar o papel grantRolesToUser e revokeRole para o usuário com Escrita e Leitura.
```js
```

4. Remover o papel grantRolesToUser para o usuário com Escrita e Leitura.
```js
```

5. Listar todos os usuários com seus papéis e ações.
```js
```

## Cluster

- 1 Router
```js
```

- 1 Config Server
```js
```

- 3 Shardings
```js
```

- 3 Replicas
```js
```

Você deverá escolher qual sua coleção deverá ser *shardeada* para poder aguentar muita carga repentinamente e deverá replicar cada Shard, pode ser feito localmente como em alguma VPS FREE.
