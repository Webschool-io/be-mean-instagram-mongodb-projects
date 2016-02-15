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

## Justificativa

Para o ambiente sugerido pela modelagem o maior número de documentos serão os referentes as atividades, pois cada projeto pode
possuir inúmeros goals e dentro destes goals inúmeras atividades por isso está categoria de documentos merece uma coleção, ao
meu ver essa netodologia de modelagem também se justifica pelo fato de deixar os documentos de projetos mais "leves".

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
var projects = db.projects.find({},{name:1, "members._id": 1}).toArray()

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
Welder-Mint(mongod-3.2.1) test> db.createUser({
... user: "leitor",
... pwd: "leitura",
... roles: [{role: "read", db: "test"}]
... })
Successfully added user: {
  "user": "leitor",
  "roles": [
    {
      "role": "read",
      "db": "test"
    }
  ]
}
```

2. Crie um usuário com permissões de Escrita e Leitura.
```js
Welder-Mint(mongod-3.2.1) test> db.createUser({
... user: "escritor",
... pwd: "escrita",
... roles: [{role: "readWrite", db: "test"}]
... })
Successfully added user: {
  "user": "escritor",
  "roles": [
    {
      "role": "readWrite",
      "db": "test"
    }
  ]
}
```

3. Adicionar o papel grantRole e revokeRole para o usuário com Escrita e Leitura.
```js
Welder-Mint(mongod-3.2.1) test> db.createRole(
...    {
...      role: "grantRolesToUser",
...      privileges: [
...        { resource: { db: "test", collection: "" }, actions: [ "grantRole" ] }
...      ],
...      roles: []
...    },
...    { w: "majority" , wtimeout: 5000 }
... )
{
  "role": "grantRolesToUser",
  "privileges": [
    {
      "resource": {
        "db": "test",
        "collection": ""
      },
      "actions": [
        "grantRole"
      ]
    }
  ],
  "roles": [ ]
}

Welder-Mint(mongod-3.2.1) test> db.createRole(
...    {
...      role: "revokeRole",
...      privileges: [
...        { resource: { db: "test", collection: "" }, actions: [ "revokeRole" ] }
...      ],
...      roles: []
...    },
...    { w: "majority" , wtimeout: 5000 }
... )
{
  "role": "revokeRole",
  "privileges": [
    {
      "resource": {
        "db": "test",
        "collection": ""
      },
      "actions": [
        "revokeRole"
      ]
    }
  ],
  "roles": [ ]
}

Welder-Mint(mongod-3.2.1) test> db.grantRolesToUser(
...   "escritor",
...   [
...     { role: "grantRolesToUser", db: "test" } ,
...     { role: "revokeRole", db:"test" }
...   ]
... )

```

4. Remover o papel grantRolesToUser para o usuário com Escrita e Leitura.
```js
Welder-Mint(mongod-3.2.1) test> db.runCommand({
... revokeRolesFromUser: "escritor",
...   roles: [
...   { role: "grantRolesToUser", db: "test" }
...   ],
...   writeConcern: { w: "majority" }
... })
{
  "ok": 1
}
```

5. Listar todos os usuários com seus papéis e ações.
```js
Welder-Mint(mongod-3.2.1) admin> use admin
switched to db admin
Welder-Mint(mongod-3.2.1) admin>
Welder-Mint(mongod-3.2.1) admin> var urs = db.system.users.find({},{user:1}).toArray()
Welder-Mint(mongod-3.2.1) admin>
Welder-Mint(mongod-3.2.1) admin> var log = []
Welder-Mint(mongod-3.2.1) admin>
Welder-Mint(mongod-3.2.1) admin> urs.forEach(function(u){
... log.push(db.runCommand({
... usersInfo: {user: u.user, db: "test"},
... showCredentials: true,
... showPrivileges: true
... }))
... })
Welder-Mint(mongod-3.2.1) admin>
Welder-Mint(mongod-3.2.1) admin> log
[
  {
    "users": [
      {
        "_id": "test.leitor",
        "user": "leitor",
        "db": "test",
        "credentials": {
          "SCRAM-SHA-1": {
            "iterationCount": 10000,
            "salt": "OSUvGzJlz/oKR0fBKLW0zw==",
            "storedKey": "fwFqna9fCuyb3juKINoQZb+e/ts=",
            "serverKey": "vNzqN1e9rGJf6VmkZtZsKVXf8EM="
          }
        },
        "roles": [
          {
            "role": "read",
            "db": "test"
          }
        ],
        "inheritedRoles": [
          {
            "role": "read",
            "db": "test"
          }
        ],
        "inheritedPrivileges": [
          {
            "resource": {
              "db": "test",
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
              "db": "test",
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
              "db": "test",
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
              "db": "test",
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
  },
  {
    "users": [
      {
        "_id": "test.escritor",
        "user": "escritor",
        "db": "test",
        "credentials": {
          "SCRAM-SHA-1": {
            "iterationCount": 10000,
            "salt": "1skOyiyLozHYnObvCRHKHQ==",
            "storedKey": "4bZAH15vJAOP3jQgcmIcjzeACiA=",
            "serverKey": "mEl8UvDZ8zAUklk0k31DfCHx/t0="
          }
        },
        "roles": [
          {
            "role": "revokeRole",
            "db": "test"
          },
          {
            "role": "readWrite",
            "db": "test"
          }
        ],
        "inheritedRoles": [
          {
            "role": "readWrite",
            "db": "test"
          },
          {
            "role": "revokeRole",
            "db": "test"
          }
        ],
        "inheritedPrivileges": [
          {
            "resource": {
              "db": "test",
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
              "revokeRole",
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
              "db": "test",
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
              "db": "test",
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
              "db": "test",
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
]
```

## Cluster

### 1 Router
```js

//iniciando o router informando o config server a ser utilizado
welder@Welder-Mint ~ $ mongos --configdb localhost:27010 --port 27011
2016-02-14T11:56:15.979-0200 W SHARDING [main] Running a sharded cluster with fewer than 3 config servers should only be done for testing purposes and is not recommended for production.
2016-02-14T11:56:16.051-0200 I SHARDING [mongosMain] MongoS version 3.2.1 starting: pid=7072 port=27011 64-bit host=Welder-Mint (--help for usage)
2016-02-14T11:56:16.051-0200 I CONTROL  [mongosMain] db version v3.2.1
2016-02-14T11:56:16.051-0200 I CONTROL  [mongosMain] git version: a14d55980c2cdc565d4704a7e3ad37e4e535c1b2
2016-02-14T11:56:16.051-0200 I CONTROL  [mongosMain] OpenSSL version: OpenSSL 1.0.1f 6 Jan 2014
2016-02-14T11:56:16.051-0200 I CONTROL  [mongosMain] allocator: tcmalloc
2016-02-14T11:56:16.051-0200 I CONTROL  [mongosMain] modules: none
2016-02-14T11:56:16.051-0200 I CONTROL  [mongosMain] build environment:
2016-02-14T11:56:16.051-0200 I CONTROL  [mongosMain]     distmod: ubuntu1404
2016-02-14T11:56:16.051-0200 I CONTROL  [mongosMain]     distarch: x86_64
2016-02-14T11:56:16.051-0200 I CONTROL  [mongosMain]     target_arch: x86_64
2016-02-14T11:56:16.051-0200 I CONTROL  [mongosMain] options: { net: { port: 27011 }, sharding: { configDB: "localhost:27010" } }
2016-02-14T11:56:16.080-0200 I SHARDING [mongosMain] Updating config server connection string to: localhost:27010
2016-02-14T11:56:16.382-0200 I SHARDING [LockPinger] creating distributed lock ping thread for localhost:27010 and process Welder-Mint:27011:1455458176:1804289383 (sleeping for 30000ms)
2016-02-14T11:56:16.942-0200 I SHARDING [LockPinger] cluster localhost:27010 pinged successfully at 2016-02-14T11:56:16.382-0200 by distributed lock pinger 'localhost:27010/Welder-Mint:27011:1455458176:1804289383', sleeping for 30000ms
2016-02-14T11:56:16.943-0200 I SHARDING [mongosMain] distributed lock 'configUpgrade/Welder-Mint:27011:1455458176:1804289383' acquired for 'initializing config database to new format v6', ts : 56c08780f3e11b85c6a8e5fa
2016-02-14T11:56:16.943-0200 I SHARDING [mongosMain] initializing config server version to 6
2016-02-14T11:56:16.943-0200 I SHARDING [mongosMain] writing initial config version at v6
2016-02-14T11:56:17.317-0200 I SHARDING [mongosMain] initialization of config server to v6 successful
2016-02-14T11:56:17.318-0200 I SHARDING [mongosMain] distributed lock 'configUpgrade/Welder-Mint:27011:1455458176:1804289383' unlocked.
2016-02-14T11:56:19.652-0200 I NETWORK  [HostnameCanonicalizationWorker] Starting hostname canonicalization worker
2016-02-14T11:56:19.652-0200 I SHARDING [Balancer] about to contact config servers and shards
2016-02-14T11:56:19.652-0200 I SHARDING [Balancer] config servers and shards contacted successfully
2016-02-14T11:56:19.652-0200 I SHARDING [Balancer] balancer id: Welder-Mint:27011 started
2016-02-14T11:56:19.697-0200 I NETWORK  [mongosMain] waiting for connections on port 27011
2016-02-14T11:56:19.890-0200 I SHARDING [Balancer] distributed lock 'balancer/Welder-Mint:27011:1455458176:1804289383' acquired for 'doing balance round', ts : 56c08783f3e11b85c6a8e5fd
2016-02-14T11:56:20.062-0200 I SHARDING [Balancer] about to log metadata event into actionlog: { _id: "Welder-Mint-2016-02-14T11:56:20.062-0200-56c08784f3e11b85c6a8e5fe", server: "Welder-Mint", clientAddr: "", time: new Date(1455458180062), what: "balancer.round", ns: "", details: { executionTimeMillis: 238, errorOccured: false, candidateChunks: 0, chunksMoved: 0 } }
2016-02-14T11:56:20.074-0200 I SHARDING [Balancer] distributed lock 'balancer/Welder-Mint:27011:1455458176:1804289383' unlocked.
2016-02-14T11:56:30.109-0200 I SHARDING [Balancer] distributed lock 'balancer/Welder-Mint:27011:1455458176:1804289383' acquired for 'doing balance round', ts : 56c0878ef3e11b85c6a8e5ff
2016-02-14T11:56:30.109-0200 I SHARDING [Balancer] about to log metadata event into actionlog: { _id: "Welder-Mint-2016-02-14T11:56:30.109-0200-56c0878ef3e11b85c6a8e600", server: "Welder-Mint", clientAddr: "", time: new Date(1455458190109), what: "balancer.round", ns: "", details: { executionTimeMillis: 2, errorOccured: false, candidateChunks: 0, chunksMoved: 0 } }
2016-02-14T11:56:30.119-0200 I SHARDING [Balancer] distributed lock 'balancer/Welder-Mint:27011:1455458176:1804289383' unlocked.
```

### 1 Config Server
```js
//criando a pasta do config server
welder@Welder-Mint ~ $ sudo mkdir /data/configdb

//iniciando o servidor de configuração
welder@Welder-Mint ~ $ sudo mongod --configsvr --port 27010
2016-02-14T11:53:56.946-0200 I CONTROL  [initandlisten] MongoDB starting : pid=7014 port=27010 dbpath=/data/configdb master=1 64-bit host=Welder-Mint
2016-02-14T11:53:56.946-0200 I CONTROL  [initandlisten] db version v3.2.1
2016-02-14T11:53:56.946-0200 I CONTROL  [initandlisten] git version: a14d55980c2cdc565d4704a7e3ad37e4e535c1b2
2016-02-14T11:53:56.946-0200 I CONTROL  [initandlisten] OpenSSL version: OpenSSL 1.0.1f 6 Jan 2014
2016-02-14T11:53:56.946-0200 I CONTROL  [initandlisten] allocator: tcmalloc
2016-02-14T11:53:56.946-0200 I CONTROL  [initandlisten] modules: none
2016-02-14T11:53:56.946-0200 I CONTROL  [initandlisten] build environment:
2016-02-14T11:53:56.946-0200 I CONTROL  [initandlisten]     distmod: ubuntu1404
2016-02-14T11:53:56.946-0200 I CONTROL  [initandlisten]     distarch: x86_64
2016-02-14T11:53:56.946-0200 I CONTROL  [initandlisten]     target_arch: x86_64
2016-02-14T11:53:56.946-0200 I CONTROL  [initandlisten] options: { net: { port: 27010 }, sharding: { clusterRole: "configsvr" } }
2016-02-14T11:53:56.970-0200 I -        [initandlisten] Detected data files in /data/configdb created by the 'wiredTiger' storage engine, so setting the active storage engine to 'wiredTiger'.
2016-02-14T11:53:56.970-0200 I STORAGE  [initandlisten] wiredtiger_open config: create,cache_size=1G,session_max=20000,eviction=(threads_max=4),config_base=false,statistics=(fast),log=(enabled=true,archive=true,path=journal,compressor=snappy),file_manager=(close_idle_time=100000),checkpoint=(wait=60,log_size=2GB),statistics_log=(wait=0),
2016-02-14T11:53:57.812-0200 I STORAGE  [initandlisten] Starting WiredTigerRecordStoreThread local.oplog.$main
2016-02-14T11:53:57.812-0200 I STORAGE  [initandlisten] The size storer reports that the oplog contains 7 records totaling to 434 bytes
2016-02-14T11:53:57.812-0200 I STORAGE  [initandlisten] Scanning the oplog to determine where to place markers for truncation
2016-02-14T11:53:57.867-0200 I CONTROL  [initandlisten] ** WARNING: You are running this process as the root user, which is not recommended.
2016-02-14T11:53:57.867-0200 I CONTROL  [initandlisten]
2016-02-14T11:53:57.934-0200 I FTDC     [initandlisten] Initializing full-time diagnostic data capture with directory '/data/configdb/diagnostic.data'
2016-02-14T11:53:57.934-0200 I NETWORK  [HostnameCanonicalizationWorker] Starting hostname canonicalization worker
2016-02-14T11:53:57.935-0200 I NETWORK  [initandlisten] waiting for connections on port 27010

```

### 3 Shardings
```js

//criando os diretórios de cada shard
welder@Welder-Mint ~ $ sudo mkdir /data/shard1 && sudo mkdir /data/shard2 && sudo mkdir /data/shard3

//adicionando o primeiro shard
welder@Welder-Mint ~ $ sudo mongod --port 27012 --dbpath /data/shard1
2016-02-14T21:41:57.448-0200 I CONTROL  [initandlisten] MongoDB starting : pid=10752 port=27012 dbpath=/data/shard1 64-bit host=Welder-Mint
2016-02-14T21:41:57.448-0200 I CONTROL  [initandlisten] db version v3.2.1
2016-02-14T21:41:57.448-0200 I CONTROL  [initandlisten] git version: a14d55980c2cdc565d4704a7e3ad37e4e535c1b2
2016-02-14T21:41:57.448-0200 I CONTROL  [initandlisten] OpenSSL version: OpenSSL 1.0.1f 6 Jan 2014
2016-02-14T21:41:57.448-0200 I CONTROL  [initandlisten] allocator: tcmalloc
2016-02-14T21:41:57.448-0200 I CONTROL  [initandlisten] modules: none
2016-02-14T21:41:57.448-0200 I CONTROL  [initandlisten] build environment:
2016-02-14T21:41:57.448-0200 I CONTROL  [initandlisten]     distmod: ubuntu1404
2016-02-14T21:41:57.448-0200 I CONTROL  [initandlisten]     distarch: x86_64
2016-02-14T21:41:57.448-0200 I CONTROL  [initandlisten]     target_arch: x86_64
2016-02-14T21:41:57.448-0200 I CONTROL  [initandlisten] options: { net: { port: 27012 }, storage: { dbPath: "/data/shard1" } }
2016-02-14T21:41:57.471-0200 I STORAGE  [initandlisten] wiredtiger_open config: create,cache_size=1G,session_max=20000,eviction=(threads_max=4),config_base=false,statistics=(fast),log=(enabled=true,archive=true,path=journal,compressor=snappy),file_manager=(close_idle_time=100000),checkpoint=(wait=60,log_size=2GB),statistics_log=(wait=0),
2016-02-14T21:41:58.072-0200 I CONTROL  [initandlisten] ** WARNING: You are running this process as the root user, which is not recommended.
2016-02-14T21:41:58.072-0200 I CONTROL  [initandlisten]
2016-02-14T21:41:58.072-0200 I FTDC     [initandlisten] Initializing full-time diagnostic data capture with directory '/data/shard1/diagnostic.data'
2016-02-14T21:41:58.072-0200 I NETWORK  [HostnameCanonicalizationWorker] Starting hostname canonicalization worker
2016-02-14T21:41:58.251-0200 I NETWORK  [initandlisten] waiting for connections on port 27012


//adicionando o segundo shard
welder@Welder-Mint ~ $ sudo mongod --port 27013 --dbpath /data/shard2
[sudo] password for welder:
2016-02-14T21:42:29.597-0200 I CONTROL  [initandlisten] MongoDB starting : pid=10793 port=27013 dbpath=/data/shard2 64-bit host=Welder-Mint
2016-02-14T21:42:29.597-0200 I CONTROL  [initandlisten] db version v3.2.1
2016-02-14T21:42:29.597-0200 I CONTROL  [initandlisten] git version: a14d55980c2cdc565d4704a7e3ad37e4e535c1b2
2016-02-14T21:42:29.597-0200 I CONTROL  [initandlisten] OpenSSL version: OpenSSL 1.0.1f 6 Jan 2014
2016-02-14T21:42:29.597-0200 I CONTROL  [initandlisten] allocator: tcmalloc
2016-02-14T21:42:29.597-0200 I CONTROL  [initandlisten] modules: none
2016-02-14T21:42:29.597-0200 I CONTROL  [initandlisten] build environment:
2016-02-14T21:42:29.597-0200 I CONTROL  [initandlisten]     distmod: ubuntu1404
2016-02-14T21:42:29.597-0200 I CONTROL  [initandlisten]     distarch: x86_64
2016-02-14T21:42:29.597-0200 I CONTROL  [initandlisten]     target_arch: x86_64
2016-02-14T21:42:29.597-0200 I CONTROL  [initandlisten] options: { net: { port: 27013 }, storage: { dbPath: "/data/shard2" } }
2016-02-14T21:42:29.621-0200 I STORAGE  [initandlisten] wiredtiger_open config: create,cache_size=1G,session_max=20000,eviction=(threads_max=4),config_base=false,statistics=(fast),log=(enabled=true,archive=true,path=journal,compressor=snappy),file_manager=(close_idle_time=100000),checkpoint=(wait=60,log_size=2GB),statistics_log=(wait=0),
2016-02-14T21:42:30.518-0200 I CONTROL  [initandlisten] ** WARNING: You are running this process as the root user, which is not recommended.
2016-02-14T21:42:30.518-0200 I CONTROL  [initandlisten]
2016-02-14T21:42:30.518-0200 I FTDC     [initandlisten] Initializing full-time diagnostic data capture with directory '/data/shard2/diagnostic.data'
2016-02-14T21:42:30.518-0200 I NETWORK  [HostnameCanonicalizationWorker] Starting hostname canonicalization worker
2016-02-14T21:42:30.722-0200 I NETWORK  [initandlisten] waiting for connections on port 27013


//adicionando o terceiro shard
welder@Welder-Mint ~ $ sudo mongod --port 27014 --dbpath /data/shard3
[sudo] password for welder:
2016-02-14T21:42:48.566-0200 I CONTROL  [initandlisten] MongoDB starting : pid=10812 port=27014 dbpath=/data/shard3 64-bit host=Welder-Mint
2016-02-14T21:42:48.566-0200 I CONTROL  [initandlisten] db version v3.2.1
2016-02-14T21:42:48.566-0200 I CONTROL  [initandlisten] git version: a14d55980c2cdc565d4704a7e3ad37e4e535c1b2
2016-02-14T21:42:48.566-0200 I CONTROL  [initandlisten] OpenSSL version: OpenSSL 1.0.1f 6 Jan 2014
2016-02-14T21:42:48.566-0200 I CONTROL  [initandlisten] allocator: tcmalloc
2016-02-14T21:42:48.566-0200 I CONTROL  [initandlisten] modules: none
2016-02-14T21:42:48.566-0200 I CONTROL  [initandlisten] build environment:
2016-02-14T21:42:48.566-0200 I CONTROL  [initandlisten]     distmod: ubuntu1404
2016-02-14T21:42:48.566-0200 I CONTROL  [initandlisten]     distarch: x86_64
2016-02-14T21:42:48.566-0200 I CONTROL  [initandlisten]     target_arch: x86_64
2016-02-14T21:42:48.566-0200 I CONTROL  [initandlisten] options: { net: { port: 27014 }, storage: { dbPath: "/data/shard3" } }
2016-02-14T21:42:48.589-0200 I STORAGE  [initandlisten] wiredtiger_open config: create,cache_size=1G,session_max=20000,eviction=(threads_max=4),config_base=false,statistics=(fast),log=(enabled=true,archive=true,path=journal,compressor=snappy),file_manager=(close_idle_time=100000),checkpoint=(wait=60,log_size=2GB),statistics_log=(wait=0),
2016-02-14T21:42:49.184-0200 I CONTROL  [initandlisten] ** WARNING: You are running this process as the root user, which is not recommended.
2016-02-14T21:42:49.184-0200 I CONTROL  [initandlisten]
2016-02-14T21:42:49.184-0200 I FTDC     [initandlisten] Initializing full-time diagnostic data capture with directory '/data/shard3/diagnostic.data'
2016-02-14T21:42:49.184-0200 I NETWORK  [HostnameCanonicalizationWorker] Starting hostname canonicalization worker
2016-02-14T21:42:49.432-0200 I NETWORK  [initandlisten] waiting for connections on port 27014



//informando os shards ao router
Welder-Mint:27011(mongos-3.2.1)[mongos] test> sh.addShard("localhost:27012")
{
  "shardAdded": "shard0000",
  "ok": 1
}
Welder-Mint:27011(mongos-3.2.1)[mongos] test> sh.addShard("localhost:27013")
{
  "shardAdded": "shard0001",
  "ok": 1
}
Welder-Mint:27011(mongos-3.2.1)[mongos] test> sh.addShard("localhost:27014")
{
  "shardAdded": "shard0002",
  "ok": 1
}


//habilitando sharding
Welder-Mint:27011(mongos-3.2.1)[mongos] test> sh.enableSharding("test")
{
  "ok": 1
}


//A coleção shardeada será de activities pois provavelmente será a com maior número de documentos
Welder-Mint:27011(mongos-3.2.1)[mongos] test> sh.shardCollection("test.activities", {"_id" : 1})
{
  "collectionsharded": "test.activities",
  "ok": 1
}
```

### 3 Replicas
```js

//diretórios
welder@Welder-Mint ~ $ sudo mkdir /data/rs1 && sudo mkdir /data/rs2 && sudo mkdir /data/rs3

//réplica 1
sudo mongod --replSet replica_set --port 27027 --dbpath /data/rs1
2016-02-14T22:10:52.837-0200 I CONTROL  [initandlisten] MongoDB starting : pid=16546 port=27027 dbpath=/data/rs1 64-bit host=Welder-Mint
2016-02-14T22:10:52.838-0200 I CONTROL  [initandlisten] db version v3.2.1
2016-02-14T22:10:52.838-0200 I CONTROL  [initandlisten] git version: a14d55980c2cdc565d4704a7e3ad37e4e535c1b2
2016-02-14T22:10:52.838-0200 I CONTROL  [initandlisten] OpenSSL version: OpenSSL 1.0.1f 6 Jan 2014
2016-02-14T22:10:52.838-0200 I CONTROL  [initandlisten] allocator: tcmalloc
2016-02-14T22:10:52.838-0200 I CONTROL  [initandlisten] modules: none
2016-02-14T22:10:52.838-0200 I CONTROL  [initandlisten] build environment:
2016-02-14T22:10:52.838-0200 I CONTROL  [initandlisten]     distmod: ubuntu1404
2016-02-14T22:10:52.838-0200 I CONTROL  [initandlisten]     distarch: x86_64
2016-02-14T22:10:52.838-0200 I CONTROL  [initandlisten]     target_arch: x86_64
2016-02-14T22:10:52.838-0200 I CONTROL  [initandlisten] options: { net: { port: 27027 }, replication: { replSet: "replica_set" }, storage: { dbPath: "/data/rs1" } }
2016-02-14T22:10:52.863-0200 I STORAGE  [initandlisten] wiredtiger_open config: create,cache_size=1G,session_max=20000,eviction=(threads_max=4),config_base=false,statistics=(fast),log=(enabled=true,archive=true,path=journal,compressor=snappy),file_manager=(close_idle_time=100000),checkpoint=(wait=60,log_size=2GB),statistics_log=(wait=0),
2016-02-14T22:10:53.501-0200 I CONTROL  [initandlisten] ** WARNING: You are running this process as the root user, which is not recommended.
2016-02-14T22:10:53.501-0200 I CONTROL  [initandlisten]
2016-02-14T22:10:53.786-0200 I REPL     [initandlisten] Did not find local voted for document at startup;  NoMatchingDocument Did not find replica set lastVote document in local.replset.election
2016-02-14T22:10:53.786-0200 I REPL     [initandlisten] Did not find local replica set configuration document at startup;  NoMatchingDocument Did not find replica set configuration document in local.system.replset
2016-02-14T22:10:53.786-0200 I FTDC     [initandlisten] Initializing full-time diagnostic data capture with directory '/data/rs1/diagnostic.data'
2016-02-14T22:10:53.786-0200 I NETWORK  [HostnameCanonicalizationWorker] Starting hostname canonicalization worker
2016-02-14T22:10:54.003-0200 I NETWORK  [initandlisten] waiting for connections on port 27027

//réplica 2
welder@Welder-Mint ~ $ sudo mongod --replSet replica_set --port 27028 --dbpath /data/rs2
[sudo] password for welder:
2016-02-14T22:12:07.144-0200 I CONTROL  [initandlisten] MongoDB starting : pid=16859 port=27028 dbpath=/data/rs2 64-bit host=Welder-Mint
2016-02-14T22:12:07.144-0200 I CONTROL  [initandlisten] db version v3.2.1
2016-02-14T22:12:07.144-0200 I CONTROL  [initandlisten] git version: a14d55980c2cdc565d4704a7e3ad37e4e535c1b2
2016-02-14T22:12:07.144-0200 I CONTROL  [initandlisten] OpenSSL version: OpenSSL 1.0.1f 6 Jan 2014
2016-02-14T22:12:07.144-0200 I CONTROL  [initandlisten] allocator: tcmalloc
2016-02-14T22:12:07.144-0200 I CONTROL  [initandlisten] modules: none
2016-02-14T22:12:07.144-0200 I CONTROL  [initandlisten] build environment:
2016-02-14T22:12:07.144-0200 I CONTROL  [initandlisten]     distmod: ubuntu1404
2016-02-14T22:12:07.144-0200 I CONTROL  [initandlisten]     distarch: x86_64
2016-02-14T22:12:07.144-0200 I CONTROL  [initandlisten]     target_arch: x86_64
2016-02-14T22:12:07.144-0200 I CONTROL  [initandlisten] options: { net: { port: 27028 }, replication: { replSet: "replica_set" }, storage: { dbPath: "/data/rs2" } }
2016-02-14T22:12:07.170-0200 I STORAGE  [initandlisten] wiredtiger_open config: create,cache_size=1G,session_max=20000,eviction=(threads_max=4),config_base=false,statistics=(fast),log=(enabled=true,archive=true,path=journal,compressor=snappy),file_manager=(close_idle_time=100000),checkpoint=(wait=60,log_size=2GB),statistics_log=(wait=0),
2016-02-14T22:12:07.744-0200 I CONTROL  [initandlisten] ** WARNING: You are running this process as the root user, which is not recommended.
2016-02-14T22:12:07.744-0200 I CONTROL  [initandlisten]
2016-02-14T22:12:07.959-0200 I REPL     [initandlisten] Did not find local voted for document at startup;  NoMatchingDocument Did not find replica set lastVote document in local.replset.election
2016-02-14T22:12:07.959-0200 I REPL     [initandlisten] Did not find local replica set configuration document at startup;  NoMatchingDocument Did not find replica set configuration document in local.system.replset
2016-02-14T22:12:07.959-0200 I NETWORK  [HostnameCanonicalizationWorker] Starting hostname canonicalization worker
2016-02-14T22:12:07.959-0200 I FTDC     [initandlisten] Initializing full-time diagnostic data capture with directory '/data/rs2/diagnostic.data'
2016-02-14T22:12:08.171-0200 I NETWORK  [initandlisten] waiting for connections on port 27028

//réplica 3
welder@Welder-Mint ~ $ sudo mongod --replSet replica_set --port 27029 --dbpath /data/rs3
2016-02-14T22:13:04.095-0200 I CONTROL  [initandlisten] MongoDB starting : pid=17072 port=27029 dbpath=/data/rs3 64-bit host=Welder-Mint
2016-02-14T22:13:04.095-0200 I CONTROL  [initandlisten] db version v3.2.1
2016-02-14T22:13:04.095-0200 I CONTROL  [initandlisten] git version: a14d55980c2cdc565d4704a7e3ad37e4e535c1b2
2016-02-14T22:13:04.095-0200 I CONTROL  [initandlisten] OpenSSL version: OpenSSL 1.0.1f 6 Jan 2014
2016-02-14T22:13:04.095-0200 I CONTROL  [initandlisten] allocator: tcmalloc
2016-02-14T22:13:04.095-0200 I CONTROL  [initandlisten] modules: none
2016-02-14T22:13:04.095-0200 I CONTROL  [initandlisten] build environment:
2016-02-14T22:13:04.095-0200 I CONTROL  [initandlisten]     distmod: ubuntu1404
2016-02-14T22:13:04.095-0200 I CONTROL  [initandlisten]     distarch: x86_64
2016-02-14T22:13:04.095-0200 I CONTROL  [initandlisten]     target_arch: x86_64
2016-02-14T22:13:04.095-0200 I CONTROL  [initandlisten] options: { net: { port: 27029 }, replication: { replSet: "replica_set" }, storage: { dbPath: "/data/rs3" } }
2016-02-14T22:13:04.120-0200 I STORAGE  [initandlisten] wiredtiger_open config: create,cache_size=1G,session_max=20000,eviction=(threads_max=4),config_base=false,statistics=(fast),log=(enabled=true,archive=true,path=journal,compressor=snappy),file_manager=(close_idle_time=100000),checkpoint=(wait=60,log_size=2GB),statistics_log=(wait=0),
2016-02-14T22:13:04.647-0200 I CONTROL  [initandlisten] ** WARNING: You are running this process as the root user, which is not recommended.
2016-02-14T22:13:04.647-0200 I CONTROL  [initandlisten]
2016-02-14T22:13:04.918-0200 I REPL     [initandlisten] Did not find local voted for document at startup;  NoMatchingDocument Did not find replica set lastVote document in local.replset.election
2016-02-14T22:13:04.918-0200 I REPL     [initandlisten] Did not find local replica set configuration document at startup;  NoMatchingDocument Did not find replica set configuration document in local.system.replset
2016-02-14T22:13:04.918-0200 I FTDC     [initandlisten] Initializing full-time diagnostic data capture with directory '/data/rs3/diagnostic.data'
2016-02-14T22:13:04.918-0200 I NETWORK  [HostnameCanonicalizationWorker] Starting hostname canonicalization worker
2016-02-14T22:13:05.116-0200 I NETWORK  [initandlisten] waiting for connections on port 27029


//iniciando replicação
Welder-Mint:27027(mongod-3.2.1) test> rsconf = {    _id: "replica_set",    members: [     {      _id: 0,      host: "127.0.0.1:27027"     }   ] }
{
  "_id": "replica_set",
  "members": [
    {
      "_id": 0,
      "host": "127.0.0.1:27027"
    }
  ]
}
Welder-Mint:27027(mongod-3.2.1) test> rs.initiate(rsconf)
{
  "ok": 1
}

//log rs 1
2016-02-14T22:43:53.527-0200 I REPL     [ReplicationExecutor] New replica set config in use: { _id: "replica_set", version: 1, protocolVersion: 1, members: [ { _id: 0, host: "127.0.0.1:27027", arbiterOnly: false, buildIndexes: true, hidden: false, priority: 1.0, tags: {}, slaveDelay: 0, votes: 1 } ], settings: { chainingAllowed: true, heartbeatIntervalMillis: 2000, heartbeatTimeoutSecs: 10, electionTimeoutMillis: 10000, getLastErrorModes: {}, getLastErrorDefaults: { w: 1, wtimeout: 0 } } }
2016-02-14T22:43:53.527-0200 I REPL     [ReplicationExecutor] This node is 127.0.0.1:27027 in the config
2016-02-14T22:43:53.527-0200 I REPL     [ReplicationExecutor] transition to STARTUP2
2016-02-14T22:43:53.527-0200 I REPL     [conn1] Starting replication applier threads
2016-02-14T22:43:53.527-0200 I REPL     [ReplicationExecutor] transition to RECOVERING
2016-02-14T22:43:53.528-0200 I REPL     [ReplicationExecutor] transition to SECONDARY
2016-02-14T22:43:53.528-0200 I REPL     [ReplicationExecutor] conducting a dry run election to see if we could be elected
2016-02-14T22:43:53.528-0200 I REPL     [ReplicationExecutor] dry election run succeeded, running for election
2016-02-14T22:43:53.533-0200 I COMMAND  [conn1] command local.oplog.rs command: replSetInitiate { replSetInitiate: { _id: "replica_set", members: [ { _id: 0.0, host: "127.0.0.1:27027" } ] } } keyUpdates:0 writeConflicts:0 numYields:0 reslen:22 locks:{ Global: { acquireCount: { r: 5, w: 3, W: 2 }, acquireWaitCount: { W: 1 }, timeAcquiringMicros: { W: 19918 } }, Database: { acquireCount: { w: 2, W: 1 } }, Metadata: { acquireCount: { w: 1 } }, oplog: { acquireCount: { w: 2 } } } protocol:op_command 821ms
2016-02-14T22:43:53.761-0200 I REPL     [ReplicationExecutor] election succeeded, assuming primary role in term 1
2016-02-14T22:43:53.761-0200 I REPL     [ReplicationExecutor] transition to PRIMARY
2016-02-14T22:43:54.541-0200 I REPL     [rsSync] transition to primary complete; database writes are now permitted


//adicionando demais réplicas
Welder-Mint:27027(mongod-3.2.1)[SECONDARY:replica_set] test> rs.add("127.0.0.1:27028")
{
  "ok": 1
}
Welder-Mint:27027(mongod-3.2.1)[PRIMARY:replica_set] test> rs.add("127.0.0.1:27029")
{
  "ok": 1
}

//rs2 log
016-02-14T22:46:24.779-0200 I REPL     [ReplicationExecutor] New replica set config in use: { _id: "replica_set", version: 3, protocolVersion: 1, members: [ { _id: 0, host: "127.0.0.1:27027", arbiterOnly: false, buildIndexes: true, hidden: false, priority: 1.0, tags: {}, slaveDelay: 0, votes: 1 }, { _id: 1, host: "127.0.0.1:27028", arbiterOnly: false, buildIndexes: true, hidden: false, priority: 1.0, tags: {}, slaveDelay: 0, votes: 1 }, { _id: 2, host: "127.0.0.1:27029", arbiterOnly: false, buildIndexes: true, hidden: false, priority: 1.0, tags: {}, slaveDelay: 0, votes: 1 } ], settings: { chainingAllowed: true, heartbeatIntervalMillis: 2000, heartbeatTimeoutSecs: 10, electionTimeoutMillis: 10000, getLastErrorModes: {}, getLastErrorDefaults: { w: 1, wtimeout: 0 } } }
2016-02-14T22:46:24.779-0200 I REPL     [ReplicationExecutor] This node is 127.0.0.1:27028 in the config
2016-02-14T22:46:24.779-0200 W REPL     [ReplicationExecutor] The liveness timeout does not match callback handle, so not resetting it.
2016-02-14T22:46:24.779-0200 I NETWORK  [conn4] end connection 127.0.0.1:52739 (1 connection now open)
2016-02-14T22:46:24.780-0200 I ASIO     [NetworkInterfaceASIO-Replication-0] Successfully connected to 127.0.0.1:27029
2016-02-14T22:46:24.780-0200 I REPL     [ReplicationExecutor] Member 127.0.0.1:27029 is now in state STARTUP
2016-02-14T22:46:24.780-0200 I NETWORK  [initandlisten] connection accepted from 127.0.0.1:52741 #5 (2 connections now open)
2016-02-14T22:46:24.885-0200 I REPL     [ReplicationExecutor] syncing from: 127.0.0.1:27027
2016-02-14T22:46:24.886-0200 I REPL     [SyncSourceFeedback] setting syncSourceFeedback to 127.0.0.1:27027
2016-02-14T22:46:24.887-0200 I ASIO     [NetworkInterfaceASIO-BGSync-0] Successfully connected to 127.0.0.1:27027
2016-02-14T22:46:26.780-0200 I REPL     [ReplicationExecutor] Member 127.0.0.1:27029 is now in state STARTUP2
2016-02-14T22:46:26.962-0200 I NETWORK  [initandlisten] connection accepted from 127.0.0.1:52746 #6 (3 connections now open)
2016-02-14T22:46:26.985-0200 I NETWORK  [conn6] end connection 127.0.0.1:52746 (2 connections now open)
2016-02-14T22:46:28.780-0200 I REPL     [ReplicationExecutor] Member 127.0.0.1:27029 is now in state SECONDARY

//rs3 log
2016-02-14T22:51:20.832-0200 I REPL     [ReplicationExecutor] New replica set config in use: { _id: "replica_set", version: 3, protocolVersion: 1, members: [ { _id: 0, host: "127.0.0.1:27027", arbiterOnly: false, buildIndexes: true, hidden: false, priority: 1.0, tags: {}, slaveDelay: 0, votes: 1 }, { _id: 1, host: "127.0.0.1:27028", arbiterOnly: false, buildIndexes: true, hidden: false, priority: 1.0, tags: {}, slaveDelay: 0, votes: 1 }, { _id: 2, host: "127.0.0.1:27029", arbiterOnly: false, buildIndexes: true, hidden: false, priority: 1.0, tags: {}, slaveDelay: 0, votes: 1 } ], settings: { chainingAllowed: true, heartbeatIntervalMillis: 2000, heartbeatTimeoutSecs: 10, electionTimeoutMillis: 10000, getLastErrorModes: {}, getLastErrorDefaults: { w: 1, wtimeout: 0 } } }
2016-02-14T22:51:20.832-0200 I REPL     [ReplicationExecutor] This node is 127.0.0.1:27029 in the config
2016-02-14T22:51:20.832-0200 I REPL     [ReplicationExecutor] transition to STARTUP2
2016-02-14T22:51:20.832-0200 I REPL     [ReplicationExecutor] Starting replication applier threads
2016-02-14T22:51:20.832-0200 I REPL     [ReplicationExecutor] transition to RECOVERING
2016-02-14T22:51:20.833-0200 I ASIO     [NetworkInterfaceASIO-Replication-0] Successfully connected to 127.0.0.1:27027
2016-02-14T22:51:20.833-0200 I ASIO     [NetworkInterfaceASIO-Replication-0] Successfully connected to 127.0.0.1:27028
2016-02-14T22:51:20.833-0200 I REPL     [ReplicationExecutor] Member 127.0.0.1:27027 is now in state PRIMARY
2016-02-14T22:51:20.833-0200 I REPL     [ReplicationExecutor] Member 127.0.0.1:27028 is now in state SECONDARY
2016-02-14T22:51:20.833-0200 I REPL     [ReplicationExecutor] transition to SECONDARY

```
