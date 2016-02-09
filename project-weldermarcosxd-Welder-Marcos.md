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

## Retrieve - busca

1. Liste as informações dos membros de 1 projeto específico que deve ser buscado pelo seu nome de forma a não ligar para maiúsculas e minúsculas.
2. Liste todos os projetos com a tag que você escolheu para os 3 projetos em comum.
3. Liste apenas os nomes de todas as atividades para todos os projetos.
4. Liste todos os projetos que não possuam uma tag.
5. Liste todos os usuários que não fazem parte do primeiro projeto cadastrado.

## Update - alteração

1. Adicione para todos os projetos o campo `views: 0`.
2. Adicione 1 tag diferente para cada projeto.
3. Adicione 2 membros diferentes para cada projeto.
4. Adicione 1 comentário em cada atividade, deixe apenas 1 projeto sem.
5. Adicione 1 projeto inteiro com **UPSERT**.

## Delete - remoção

1. Apague todos os projetos que não possuam *tags*.
2. Apague todos os projetos que não possuam comentários nas atividades.
3. Apague todos os projetos que não possuam atividades.
4. Escolha 2 usuário e apague todos os projetos em que os 2 fazem parte.
5. Apague todos os projetos que possuam uma determinada *tag* em *goal*.

## Sharding

- 1 Router
- 1 Config Server
- 3 Shardings

## Replica

- 3 Replicas
Você deverá escolher qual sua coleção deverá ser *shardeada* para poder aguentar muita carga repentinamente e deverá replicar cada Shard, pode ser feito localmente como em alguma VPS FREE.
