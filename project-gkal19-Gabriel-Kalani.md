# MongoDb - Projeto Final
**Autor:** Gabriel Kalani
**Data:** 1454113488

## Para qual sistema você usaria o MongoDB (diferente desse)?
Como estou começando agora minha jornada na stack do MEAN, eu usaria em:
 Rede Social, Chat Real Time e entre outros
  
## Qual a modelagem da sua coleção de `users`?

```js
user:{
	_id: ObjectID(''),
	name: String,
	bio: String,
	avatar_path: String,
	background_path: String,
	date_register: Date,
	auth:{
		username: String,
		email: String,
		password: String,
		hash_token: String,
		last_access: Date,
		online: Boolean,
		disabled:Boolean
	}
}
```
## Qual a modelagem da sua coleção de `projects`?
```js
project:{
	_id: ObjectID(''),
	name: String,
	description: String,
	date_begin: Date,
	date_dream: Date,
	date_end: Date,
	expired: Date,
	visible: Boolean,
	realocate: Boolean,
	visualizable_mod: String,
	tags:[],
	members: [
		{
			type_member: String, 
			user_id: ObjectID(''), 
			notify: Boolean
		}
	],
	goals:[
		{
			name: String,
			description: String,
			date_begin: Date,
			date_dream: Date,
			date_end: Date,
			date_relocate: Date,
			expired: Date,
			realocate: Boolean,
			activities: [ObjectID('')],
			tags:[]
		}
	]
}
```
## Qual a modelagem da sua coleção retirada de `projects`?
```js
activity:{
	_id: ObjectID(''),
	name: String,
	description: String,
	date_begin: Date,
	date_dream: Date,
	date_relocate: Date,
	expired: Date,
	date_end: Date,
	realocate: Boolean,
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

Primeiramente criei uma variável `names` com o valor dos 10 nomes dos usuários.

#### Cadastre 10 usuários diferentes.
```js
	//  Cadastro de 10 usuários
        var names = ['Jean', 'Gabriel', 'Douglas', 'Ednilson', 'Jô Soares', 'Suissa', 'Mateus', 'Emerson', 'Marcos', 'Roberto'];

	names.forEach(function(name){
		var user = {
			name: name,
			bio: 'Projeto Final - MongoDB',
			avatar_path: 'imgs/avatares/'+rand.letter(6)+rand.number()+'.jpg',
			background_path: 'imgs/bgs/'+rand.letter(5)+rand.number()+'.gif',
			date_register: rand.date(),
			auth:{
				username: rand.letter(5),
				email: name+'@be-mean.com',
				password: rand.number(9999999999999),
				hash_token:rand.letter(5)+rand.number()+rand.letter(5)+rand.number(),
				online: true,
				disabled: false
			}
		};
		user.auth.last_access = new Date(user.date_register.getTime()+rand.number(9999999));
		db.users.insert(user);
	});
```
#### Cadastrando activities para os goals.
```js
	// Cadastro de activities para os goals
	var activities = [
		{name: 'MEAN - MongoDB', description: 'O MongoDb é um banco e dados NoSQL open-source e orientado a documentos JSON.'},
		{name: 'MEAN - nodeJS', description: 'Explicar sobre javascript no server-side com node'},
		{name: 'MEAN - Estante Virtual', description: 'Quem sabe começar com os alunos de faculdade que desejam vender, trocar ou doar seus livros que ele não precisará mais, porém os calouros precisam.'},
		{name: 'MEAN - Instagram', description: 'Curso voltado a ensinar o stack conhecido como MEAN(MongoDb, Express, Angular e Node.js) para criarmos um sistema igual do Instagram.'},
		{name: 'MEAN - Missed People', description: 'Sistema para facilitar o cadastro e a busca por pessoas perdidas no Mundo.'},
		{name: 'StudyGuide - JavaScript', description: 'Roteiro de como um programador iniciante pode virar um programador foda.'},
		{name: 'Webschool-magazine-PHP', description: 'A revista dedicada ao PHP.'},
		{name: 'Popcorn - Desktop', description: 'Fazer o sistema com node-webkit ou electron'},
		{name: 'Popcorn - Web', description: 'Fazer o sistema usando Webtorrent'}
	];
	
	activities.forEach(function(activity){
		activity.date_begin =  rand.date(),
		activity.date_dream = rand.date(),
		activity.date_relocate = rand.date(),
		activity.expired = rand.date(),
		activity.date_end = rand.date(),
		activity.realocate = false
		activity.members =  generateMembers(2, 'aluno'),
		activity.tags =  ['Webschool'],
		activity.comments = [
		{
		 text: 'Curso Be MEAN - Projeto Final - MongoDB', 
		 Date_comment: rand.date(), 
		 files:[{path: 'imgs/files_comments/'+rand.letter(5)+'.jpg', weight: rand.number(9999999), name: 'comment qualquer'}],
		 members: generateMembers(1, 'aluno')
		}
	]
		db.activities.insert(activity);
	});
```
#### Cadastrando projects e goals.
```js
	
	
	// Cadastro das projects e goals
	
	var projects = [
		{
		name: 'Crie seu instagram com a MEAN',
		tags: ['js', 'instagram', 'MEAN', 'angular', 'mongoDB'],
		description: 'Curso voltado a ensinar o stack conhecido como MEAN(MongoDb, Express, Angular e Node.js) para criarmos um sistema igual do Instagram.',
		goals:[
				{
				name: 'Projeto Final MongoDb [PRONTO]',
				description: 'Vamos juntos chegar ao projeto final do MongoDb',
				tags:['mongoDB', 'angular', 'express', 'nodeJS'],
				activities:["56898c30d8c06e00b91437ba", "56898c30d8c06e00b91437bb", "56898c30d8c06e00b91437bc"]
				}
			]
		},
		{
		name: 'Jobs',
		description: 'Um sistema que será usado apenas pelos alunos certificados da Webschool.io.',
		tags: ['jobs', 'webschool', 'suissa', 'help'],
		goals:[
				{
				name: 'Entrevista',
				description: 'A entrevista, ou uma pré-entrevista, poderá ser feita via sistema utilizando WebRTC.',
				tags:['entrevista', 'mean', 'webRTC', 'nodeJS'],
				activities:["56898c30d8c06e00b91437c1", "56898c30d8c06e00b91437c2"]
				}
			]
		},
		{
		name: 'Patreon',
		description: 'Sistema igual ao Patreon porém apenas os professores da Webschool.io poderão usar.',
		tags: ['teacher', 'patreon', 'help', 'webschool'],
		goals:[
				{
				name: 'Para que serve?',
				description: 'Esse sistema serve para que seus fãs possam lhe ajudar com um valor pequeno, exemplo R$10, por mês.',
				tags:['money', 'suissa'],
				activities:[]
			}
		]
	},
	{
		name: 'Estante Virtual',
		description: 'Quem sabe começar com os alunos de faculdade que desejam vender, trocar ou doar seus livros que ele não precisará mais, porém os calouros precisam.',
		tags: ['leia', 'webschool', 'virtual', 'curso'],
		goals:[
				{
				name: 'Parceria com as editoras',
				description: 'Fazer parceria com editoras para sortearmos 1 livro por mês, o mais procurado.',
				tags:['parceria', 'book', 'learn'],
				activities:["56898c30d8c06e00b91437bf","56898c30d8c06e00b91437c0"]
			}
		]
	},
	{
		name: 'Gerenciador de Doações',
		description: 'O sistema servirá para qualquer tipo de doações, utilizando o nosso meio de pagamento.',
		tags: ['payment', 'donation', 'basic', 'help', 'curso'],
		goals:[
			{
			name: 'Pagamentos',
			description: 'Para uma ONG ou projeto social poder receber doações ele deverá colocar seu orçamento do mês no sistema, o qual ficará transparente.',
			tags:['paymento', 'webschool', 'windows', 'js'],
			activities:["56898c30d8c06e00b91437bd", "56898c30d8c06e00b91437be"]
			}
		]
	}
];
	
        projects.forEach(function(project){
        project.date_begin = rand.date(),
        project.date_dream = rand.date(),
        project.date_end = rand.date(),
        project.expired = rand.date(),
        project.visible = true,
        project.members = generateMembers(5, 'aluno'),
        project.realocate = false,
        project.visualizable_mod = 'all'
        project.goals.forEach(function(goal){
        goal.date_begin = rand.date(),
        goal.date_dream = rand.date(),
        goal.date_end =  rand.date(),
        goal.date_relocate = rand.date(),
        goal.expired =  rand.date(),
        goal.realocate = false
        });
        db.projects.insert(project);
    });
    
```
## Retrieve - busca

#### Listar as informações dos membros de 1 projeto específico.
```js

gkal19-aula-bemean-2468380(mongod-2.6.11) db> var project = db.projects.findOne({name:/crie seu instagram com a mean/i},{members:1});
gkal19-aula-bemean-2468380(mongod-2.6.11) db> project
{
        "_id" : ObjectId("56abf9f0a4807d7fb6f5c16c"),
        "members" : [
                {
                        "user_id" : "56abf9aaa4807d7fb6f5c15d",
                        "type_member" : "aluno",
                        "notify" : false
                },
                {
                        "user_id" : "56abf9aaa4807d7fb6f5c15f",
                        "type_member" : "aluno",
                        "notify" : false
                },
                {
                        "user_id" : "56abf9aaa4807d7fb6f5c15b",
                        "type_member" : "aluno",
                        "notify" : true
                },
                {
                        "user_id" : "56abf9aaa4807d7fb6f5c15c",
                        "type_member" : "aluno",
                        "notify" : false
                },
                {
                        "user_id" : "56abf9aaa4807d7fb6f5c15e",
                        "type_member" : "aluno",
                        "notify" : true
                }
        ]
}
        
        > var members_project = [];
        > project.members.forEach(function(member){
        members_project.push(db.users.findOne({"_id":ObjectId(member.user_id)}));
	});

	
	> members_project

[
        {
                "_id" : ObjectId("56abf9aaa4807d7fb6f5c15d"),
                "name" : "Jô Soares",
                "bio" : "Projeto Final - MongoDB",
                "avatar_path" : "imgs/avatares/zuctnq377.jpg",
                "background_path" : "imgs/bgs/kswwe705.gif",
                "date_register" : ISODate("1999-12-14T14:29:15.563Z"),
                "auth" : {
                        "username" : "ihgef",
                        "email" : "Jô Soares@be-mean.com",
                        "password" : 2899000504985,
                        "hash_token" : "qyimn637waure157",
                        "online" : true,
                        "disabled" : false,
                        "last_access" : ISODate("1999-12-14T17:12:22.525Z")
                }
        },
        {
                "_id" : ObjectId("56abf9aaa4807d7fb6f5c15f"),
                "name" : "Mateus",
                "bio" : "Projeto Final - MongoDB",
                "avatar_path" : "imgs/avatares/eksxaz106.jpg",
                "background_path" : "imgs/bgs/qjvzu706.gif",
                "date_register" : ISODate("1991-02-13T09:39:10.220Z"),
                "auth" : {
                        "username" : "cjupl",
                        "email" : "Mateus@be-mean.com",
                        "password" : 9314096535089,
                        "hash_token" : "ypvgu647yjvit95",
                        "online" : true,
                        "disabled" : false,
                        "last_access" : ISODate("1991-02-13T11:18:12.595Z")
                }
        },
        {
                "_id" : ObjectId("56abf9aaa4807d7fb6f5c15b"),
                "name" : "Douglas",
                "bio" : "Projeto Final - MongoDB",
                "avatar_path" : "imgs/avatares/cpxipz248.jpg",
                "background_path" : "imgs/bgs/ynpjr565.gif",
                "date_register" : ISODate("2004-07-29T05:42:16.578Z"),
                "auth" : {
                        "username" : "wtsty",
                        "email" : "Douglas@be-mean.com",
                        "password" : 7699388584587,
                        "hash_token" : "zxchi620wqrfx647",
                        "online" : true,
                        "disabled" : false,
                        "last_access" : ISODate("2004-07-29T08:26:01.019Z")
                }
        },
        {
                "_id" : ObjectId("56abf9aaa4807d7fb6f5c15c"),
                "name" : "Ednilson",
                "bio" : "Projeto Final - MongoDB",
                "avatar_path" : "imgs/avatares/kfhqhw447.jpg",
                "background_path" : "imgs/bgs/okrkl487.gif",
                "date_register" : ISODate("1981-02-18T05:10:10.318Z"),
                "auth" : {
                        "username" : "hcrua",
                        "email" : "Ednilson@be-mean.com",
                        "password" : 7664658057037,
                        "hash_token" : "amffy238gvdeq119",
                        "online" : true,
                        "disabled" : false,
                        "last_access" : ISODate("1981-02-18T06:16:44.652Z")
                }
        },
        {
                "_id" : ObjectId("56abf9aaa4807d7fb6f5c15e"),
                "name" : "Suissa",
                "bio" : "Projeto Final - MongoDB",
                "avatar_path" : "imgs/avatares/fvdpjf111.jpg",
                "background_path" : "imgs/bgs/puyxo734.gif",
                "date_register" : ISODate("2001-02-24T21:36:49.714Z"),
                "auth" : {
                        "username" : "hdpjg",
                        "email" : "Suissa@be-mean.com",
                        "password" : 5682312264107,
                        "hash_token" : "husvd251ftpxl552",
                        "online" : true,
                        "disabled" : false,
                        "last_access" : ISODate("2001-02-24T22:58:14.019Z")
                }
        }
]
        
```

#### Liste todos os projetos com a tag que você escolheu para os 3 projetos em comum.

```js

> db.projects.find({tags:{$in:['curso']}}).pretty()
{
        "_id" : ObjectId("56abf9f0a4807d7fb6f5c16f"),
        "name" : "Estante Virtual",
        "description" : "Quem sabe começar com os alunos de faculdade que desejam vender, trocar ou doar seus livros que ele não precisará mais, porém os calouros precisam.",
        "tags" : [
                "leia",
                "webschool",
                "virtual",
                "curso"
        ],
        "goals" : [
                {
                        "name" : "Parceria com as editoras",
                        "description" : "Fazer parceria com editoras para sortearmos 1 livro por mês, o mais procurado.",
                        "tags" : [
                                "parceria",
                                "book",
                                "learn"
                        ],
                        "activities" : [
                                "56898c30d8c06e00b91437bf",
                                "56898c30d8c06e00b91437c0"
                        ],
                        "date_begin" : ISODate("2003-01-19T00:17:16.346Z"),
                        "date_dream" : ISODate("2012-05-24T18:26:40.906Z"),
                        "date_end" : ISODate("2002-12-28T22:39:33.513Z"),
                        "date_relocate" : ISODate("1998-04-18T07:33:46.189Z"),
                        "expired" : ISODate("2005-12-20T07:05:53.044Z"),
                        "realocate" : false
                }
        ],
        "date_begin" : ISODate("1972-11-22T01:21:26.703Z"),
        "date_dream" : ISODate("1990-01-14T20:49:27.721Z"),
        "date_end" : ISODate("1974-12-22T09:15:53.926Z"),
        "expired" : ISODate("1982-04-13T14:39:13.415Z"),
        "visible" : true,
        "members" : [
                {
                        "user_id" : "56abf9aaa4807d7fb6f5c15a",
                        "type_member" : "aluno",
                        "notify" : true
                },
                {
                        "user_id" : "56abf9aaa4807d7fb6f5c159",
                        "type_member" : "aluno",
                        "notify" : true
                },
                {
                        "user_id" : "56abf9aaa4807d7fb6f5c15d",
                        "type_member" : "aluno",
                        "notify" : false
                },
                {
                        "user_id" : "56abf9aaa4807d7fb6f5c15c",
                        "type_member" : "aluno",
                        "notify" : false
                },
                {
                        "user_id" : "56abf9aaa4807d7fb6f5c15b",
                        "type_member" : "aluno",
                        "notify" : true
                }
        ],
        "realocate" : false,
        "visualizable_mod" : "all"
}
{
        "_id" : ObjectId("56abf9f0a4807d7fb6f5c170"),
        "name" : "Gerenciador de Doações",
        "description" : "O sistema servirá para qualquer tipo de doações, utilizando o nosso meio de pagamento.",
        "tags" : [
                "payment",
                "donation",
                "basic",
                "help",
                "curso"
        ],
        "goals" : [
                {
                        "name" : "Pagamentos",
                        "description" : "Para uma ONG ou projeto social poder receber doações ele deverá colocar seu orçamento do mês no sistema, o qual ficará transparente.",
                        "tags" : [
                                "paymento",
                                "webschool",
                                "windows",
                                "js"
                        ],
                        "activities" : [
                                "56898c30d8c06e00b91437bd",
                                "56898c30d8c06e00b91437be"
                        ],
                        "date_begin" : ISODate("2005-12-01T18:08:48.578Z"),
                        "date_dream" : ISODate("2011-05-09T23:40:43.591Z"),
                        "date_end" : ISODate("1979-11-01T09:50:10.216Z"),
                        "date_relocate" : ISODate("1981-09-03T01:56:17.906Z"),
                        "expired" : ISODate("2009-12-17T09:33:34.910Z"),
                        "realocate" : false
                }
        ],
        "date_begin" : ISODate("1990-07-26T07:12:39.946Z"),
        "date_dream" : ISODate("1987-07-12T04:43:08.882Z"),
        "date_end" : ISODate("1989-05-02T08:28:40.585Z"),
        "expired" : ISODate("1972-03-22T06:08:05.535Z"),
        "visible" : true,
        "members" : [
                {
                        "user_id" : "56abf9aaa4807d7fb6f5c15a",
                        "type_member" : "aluno",
                        "notify" : false
                },
                {
                        "user_id" : "56abf9aaa4807d7fb6f5c15b",
                        "type_member" : "aluno",
                        "notify" : true
                },
                {
                        "user_id" : "56abf9aaa4807d7fb6f5c15d",
                        "type_member" : "aluno",
                        "notify" : true
                },
                {
                        "user_id" : "56abf9aaa4807d7fb6f5c159",
                        "type_member" : "aluno",
                        "notify" : true
                },
                {
                        "user_id" : "56abf9aaa4807d7fb6f5c15c",
                        "type_member" : "aluno",
                        "notify" : true
                }
        ],
        "realocate" : false,
        "visualizable_mod" : "all"
}

```

#### Liste apenas os nomes de todas as atividades para todos os projetos.

```js

gkal19-aula-bemean-2468380(mongod-2.6.11) db> var activities = db.activities.find({}, {name:1}).toArray()
gkal19-aula-bemean-2468380(mongod-2.6.11) db> var activities_names = [];
gkal19-aula-bemean-2468380(mongod-2.6.11) db> 
gkal19-aula-bemean-2468380(mongod-2.6.11) db> activities.forEach(function(activity){
         var id = activity._id.valueOf();
         var project = db.projects.findOne({'goals.activities': {$in:[id]}},{"_id":1});
         if(project)
              activities_names.push({
         id_project:project._id,
              activity: activity.name
        });
});

gkal19-aula-bemean-2468380(mongod-2.6.11) db> activities_names
[]

```
Não sei porquê a variável apareceu nula

#### Liste todos os projetos que não possuam uma tag.
```js

gkal19-aula-bemean-2468380(mongod-2.6.11) db> db.projects.find({'tags':{$nin:['curso']}}).pretty()
{
        "_id" : ObjectId("56abf9f0a4807d7fb6f5c16c"),
        "name" : "Crie seu instagram com a MEAN",
        "tags" : [
                "js",
                "instagram",
                "MEAN",
                "angular",
                "mongoDB"
        ],
        "description" : "Curso voltado a ensinar o stack conhecido como MEAN(MongoDb, Express, Angular e Node.js) para criarmos um sistema igual do Instagram.",
        "goals" : [
                {
                        "name" : "Projeto Final MongoDb [PRONTO]",
                        "description" : "Vamos juntos chegar ao projeto final do MongoDb",
                        "tags" : [
                                "mongoDB",
                                "angular",
                                "express",
                                "nodeJS"
                        ],
                        "activities" : [
                                "56898c30d8c06e00b91437ba",
                                "56898c30d8c06e00b91437bb",
                                "56898c30d8c06e00b91437bc"
                        ],
                        "date_begin" : ISODate("1991-11-18T03:48:59.193Z"),
                        "date_dream" : ISODate("1985-05-13T10:18:04.327Z"),
                        "date_end" : ISODate("1995-08-19T20:53:04.142Z"),
                        "date_relocate" : ISODate("1970-01-22T01:08:23.328Z"),
                        "expired" : ISODate("2014-11-02T17:53:37.157Z"),
                        "realocate" : false
                }
        ],
        "date_begin" : ISODate("1975-03-03T14:03:38.775Z"),
        "date_dream" : ISODate("2000-11-25T08:29:44.396Z"),
        "date_end" : ISODate("1989-06-01T05:51:54.513Z"),
        "expired" : ISODate("2014-04-07T07:52:35.912Z"),
        "visible" : true,
        "members" : [
                {
                        "user_id" : "56abf9aaa4807d7fb6f5c15d",
                        "type_member" : "aluno",
                        "notify" : false
                },
                {
                        "user_id" : "56abf9aaa4807d7fb6f5c15f",
                        "type_member" : "aluno",
                        "notify" : false
                },
                {
                        "user_id" : "56abf9aaa4807d7fb6f5c15b",
                        "type_member" : "aluno",
                        "notify" : true
                },
                {
                        "user_id" : "56abf9aaa4807d7fb6f5c15c",
                        "type_member" : "aluno",
                        "notify" : false
                },
                {
                        "user_id" : "56abf9aaa4807d7fb6f5c15e",
                        "type_member" : "aluno",
                        "notify" : true
                }
        ],
        "realocate" : false,
        "visualizable_mod" : "all"
}
{
        "_id" : ObjectId("56abf9f0a4807d7fb6f5c16d"),
        "name" : "Jobs",
        "description" : "Um sistema que será usado apenas pelos alunos certificados da Webschool.io.",
        "tags" : [
                "jobs",
                "webschool",
                "suissa",
                "help"
        ],
        "goals" : [
                {
                        "name" : "Entrevista",
                        "description" : "A entrevista, ou uma pré-entrevista, poderá ser feita via sistema utilizando WebRTC.",
                        "tags" : [
                                "entrevista",
                                "mean",
                                "webRTC",
                                "nodeJS"
                        ],
                        "activities" : [
                                "56898c30d8c06e00b91437c1",
                                "56898c30d8c06e00b91437c2"
                        ],
                        "date_begin" : ISODate("1987-06-05T12:14:41.564Z"),
                        "date_dream" : ISODate("1996-02-17T10:58:06.425Z"),
                        "date_end" : ISODate("1989-05-26T07:57:51.028Z"),
                        "date_relocate" : ISODate("1977-11-05T12:37:00.744Z"),
                        "expired" : ISODate("2010-10-19T14:38:56.568Z"),
                        "realocate" : false
                }
        ],
        "date_begin" : ISODate("2005-11-02T17:44:19.495Z"),
        "date_dream" : ISODate("1982-04-20T17:27:05.723Z"),
        "date_end" : ISODate("2003-11-22T01:09:19.642Z"),
        "expired" : ISODate("1975-05-30T08:08:12.967Z"),
        "visible" : true,
        "members" : [
                {
                        "user_id" : "56abf9aaa4807d7fb6f5c15c",
                        "type_member" : "aluno",
                        "notify" : false
                },
                {
                        "user_id" : "56abf9aaa4807d7fb6f5c15a",
                        "type_member" : "aluno",
                        "notify" : false
                },
                {
                        "user_id" : "56abf9aaa4807d7fb6f5c15d",
                        "type_member" : "aluno",
                        "notify" : false
                },
                {
                        "user_id" : "56abf9aaa4807d7fb6f5c15e",
                        "type_member" : "aluno",
                        "notify" : true
                },
                {
                        "user_id" : "56abf9aaa4807d7fb6f5c15b",
                        "type_member" : "aluno",
                        "notify" : false
                }
        ],
        "realocate" : false,
        "visualizable_mod" : "all"
}
{
        "_id" : ObjectId("56abf9f0a4807d7fb6f5c16e"),
        "name" : "Patreon",
        "description" : "Sistema igual ao Patreon porém apenas os professores da Webschool.io poderão usar.",
        "tags" : [
                "teacher",
                "patreon",
                "help",
                "webschool"
        ],
        "goals" : [
                {
                        "name" : "Para que serve?",
                        "description" : "Esse sistema serve para que seus fãs possam lhe ajudar com um valor pequeno, exemplo R$10, por mês.",
                        "tags" : [
                                "money",
                                "suissa"
                        ],
                        "activities" : [ ],
                        "date_begin" : ISODate("1974-02-02T03:07:38.274Z"),
                        "date_dream" : ISODate("1980-02-14T08:24:51.959Z"),
                        "date_end" : ISODate("1973-01-28T14:31:04.572Z"),
                        "date_relocate" : ISODate("1983-04-14T21:11:21.360Z"),
                        "expired" : ISODate("2001-05-27T10:58:37.827Z"),
                        "realocate" : false
                }
        ],
        "date_begin" : ISODate("1972-05-17T12:52:04.972Z"),
        "date_dream" : ISODate("1982-10-13T17:28:51.734Z"),
        "date_end" : ISODate("1977-02-16T03:57:21.868Z"),
        "expired" : ISODate("2010-11-21T15:58:46.864Z"),
        "visible" : true,
        "members" : [
                {
                        "user_id" : "56abf9aaa4807d7fb6f5c15d",
                        "type_member" : "aluno",
                        "notify" : false
                },
                {
                        "user_id" : "56abf9aaa4807d7fb6f5c15b",
                        "type_member" : "aluno",
                        "notify" : true
                },
                {
                        "user_id" : "56abf9aaa4807d7fb6f5c15e",
                        "type_member" : "aluno",
                        "notify" : true
                },
                {
                        "user_id" : "56abf9aaa4807d7fb6f5c15a",
                        "type_member" : "aluno",
                        "notify" : false
                },
                {
                        "user_id" : "56abf9aaa4807d7fb6f5c15c",
                        "type_member" : "aluno",
                        "notify" : false
                }
        ],
        "realocate" : false,
        "visualizable_mod" : "all"
}

        
```

#### Liste todos os usuários que não fazem parte do primeiro projeto cadastrado.

```js

        gkal19-aula-bemean-2468380(mongod-2.6.11) db> var members = db.projects.findOne({},{members:1, _id:0});
        gkal19-aula-bemean-2468380(mongod-2.6.11) db> var notOr = [];
        gkal19-aula-bemean-2468380(mongod-2.6.11) db> members.members.forEach(function(member){
        notOr.push({"_id":ObjectId(member.user_id)});
   });
        gkal19-aula-bemean-2468380(mongod-2.6.11) db> db.users.find({$nor:notOr}).pretty();
{
        "_id" : ObjectId("56abf9aaa4807d7fb6f5c159"),
        "name" : "Jean",
        "bio" : "Projeto Final - MongoDB",
        "avatar_path" : "imgs/avatares/sjdwwy124.jpg",
        "background_path" : "imgs/bgs/uthwb176.gif",
        "date_register" : ISODate("2012-03-05T05:34:19.800Z"),
        "auth" : {
                "username" : "vghdd",
                "email" : "Jean@be-mean.com",
                "password" : 8682468573096,
                "hash_token" : "rsznd328ntdcp27",
                "online" : true,
                "disabled" : false,
                "last_access" : ISODate("2012-03-05T07:05:20.223Z")
        }
}
{
        "_id" : ObjectId("56abf9aaa4807d7fb6f5c15a"),
        "name" : "Gabriel",
        "bio" : "Projeto Final - MongoDB",
        "avatar_path" : "imgs/avatares/hekevq734.jpg",
        "background_path" : "imgs/bgs/mzyox648.gif",
        "date_register" : ISODate("1995-01-17T14:53:06.816Z"),
        "auth" : {
                "username" : "deakw",
                "email" : "Gabriel@be-mean.com",
                "password" : 9655461492947,
                "hash_token" : "wgzkz448odhjm909",
                "online" : true,
                "disabled" : false,
                "last_access" : ISODate("1995-01-17T17:19:55.888Z")
        }
}
{
        "_id" : ObjectId("56abf9aaa4807d7fb6f5c160"),
        "name" : "Emerson",
        "bio" : "Projeto Final - MongoDB",
        "avatar_path" : "imgs/avatares/xpbgrf412.jpg",
        "background_path" : "imgs/bgs/tmjpr423.gif",
        "date_register" : ISODate("2005-10-05T18:03:44.334Z"),
        "auth" : {
                "username" : "hdofo",
                "email" : "Emerson@be-mean.com",
                "password" : 6372308738063,
                "hash_token" : "xhmcb228fhvet202",
                "online" : true,
                "disabled" : false,
                "last_access" : ISODate("2005-10-05T18:58:38.425Z")
        }
}
{
        "_id" : ObjectId("56abf9aaa4807d7fb6f5c161"),
        "name" : "Marcos",
        "bio" : "Projeto Final - MongoDB",
        "avatar_path" : "imgs/avatares/xabmxx912.jpg",
        "background_path" : "imgs/bgs/iustg712.gif",
        "date_register" : ISODate("2011-03-18T10:31:33.322Z"),
        "auth" : {
                "username" : "nwfvp",
                "email" : "Marcos@be-mean.com",
                "password" : 2319199410267,
                "hash_token" : "blgkk549uqzhx406",
                "online" : true,
                "disabled" : false,
                "last_access" : ISODate("2011-03-18T11:05:44.669Z")
        }
}
{
        "_id" : ObjectId("56abf9aaa4807d7fb6f5c162"),
        "name" : "Roberto",
        "bio" : "Projeto Final - MongoDB",
        "avatar_path" : "imgs/avatares/kxqjnc437.jpg",
        "background_path" : "imgs/bgs/dfbpx350.gif",
        "date_register" : ISODate("1998-03-13T17:30:07.818Z"),
        "auth" : {
                "username" : "cusdl",
                "email" : "Roberto@be-mean.com",
                "password" : 5218298924155,
                "hash_token" : "jbujf840yvios810",
                "online" : true,
                "disabled" : false,
                "last_access" : ISODate("1998-03-13T20:04:46.623Z")
        }
}

```
## Update - alteração
#### Adicione para todos os projetos o campo views: 0.

```js

> var query = {}
> var change = {$set:{views:0}}
> var options = {multi:1}
> db.projects.update(query,change,options)
WriteResult({ "nMatched" : 5, "nUpserted" : 0, "nModified" : 5 })

```
#### Adicione 1 tag diferente para cada projeto.
```js
	
	gkal19-aula-bemean-2468380(mongod-2.6.11) db> var tags = ['tag1','tag2','tag3','tag4','tag5'];
        gkal19-aula-bemean-2468380(mongod-2.6.11) db> var count = 0;
        gkal19-aula-bemean-2468380(mongod-2.6.11) db> var projects = db.projects.find({},{"_id":1}).toArray();
        gkal19-aula-bemean-2468380(mongod-2.6.11) db> projects.forEach(function(project){
          var query = {"_id":project._id};
          var change = {$push:{tags:tags[count]}}
          var options = {multi:1}
          db.projects.update(query, change, options)
         count++;
          });

```

#### Adicione 2 membros diferentes para cada projeto.

```js
gkal19-aula-bemean-2468380(mongod-2.6.11) db> var users = db.users.find({}, {"_id":1}).toArray();
gkal19-aula-bemean-2468380(mongod-2.6.11) db> var projects = db.projects.find({},{members:1}).toArray();
gkal19-aula-bemean-2468380(mongod-2.6.11) db> var numberAddRegister = 2;
gkal19-aula-bemean-2468380(mongod-2.6.11) db> projects.forEach(function(project){
                 var until = project.members.length+numberAddRegister;
                 while(project.members.length < until){
          
                 var valuesOfNotify = [true, false];
                  var user = users[rand.number(users.length-1)];
                  var valueOfNotify = valuesOfNotify[rand.number(valuesOfNotify.length-1)];
 
        function check(member){
                  if(member.user_id != user._id.valueOf()){
                  return true;
         }
 }
 
        if(project.members.every(check)){
                         project.members.push({
                                 "user_id":user._id.valueOf(),
                                 "type_member": 'aluno',
                                 "notify": valueOfNotify
                 });
          }
 }
 db.projects.update({"_id":project._id}, {$set:{members:project.members}});
 });
Updated 1 existing record(s) in 3ms
Updated 1 existing record(s) in 1ms
Updated 1 existing record(s) in 1ms
Updated 1 existing record(s) in 0ms
Updated 1 existing record(s) in 0ms
Updated 1 existing record(s) in 0ms
Updated 1 existing record(s) in 1ms
Updated 1 existing record(s) in 1ms
Updated 1 existing record(s) in 1ms
Updated 1 existing record(s) in 1ms
Updated 1 existing record(s) in 0ms
Updated 1 existing record(s) in 2ms
Updated 1 existing record(s) in 1ms
Updated 1 existing record(s) in 1ms
Updated 1 existing record(s) in 1ms
```

#### Adicione 1 comentário em cada atividade, deixe apenas 1 projeto sem.

```js

	var goals = db.projects.findOne().goals;
	goals.forEach(function(goal){
		goal.activities.forEach(function(activity_id){
			db.activities.update({"_id":ObjectId(activity_id)}, {$set:{comments:[]}});
		});
	});
	
```

#### Adicione 1 projeto inteiro com UPSERT.

```js
        var project = {
            name: 'JS Funcional',
            description: 'Esse curso irá ensinar o paradigma de programação funcional utilizando o nosso JavaScript para isso.',
            date_begin: rand.date(),
            date_dream: rand.date(),
            date_end: rand.date(),
            expired: rand.date(),
            visible: true,
            realocate: false,
            visualizable_mod: 'all',
            tags:[],
            members: generateMembers(5, 'participante'),
            goals:[
                {
                    name: 'Ensinar JS',
                    description: 'Ensinar JS as mulheres de todos os estados.',
                    date_begin: rand.date(),
                    date_dream: rand.date(),
                    date_end: rand.date(),
                    date_relocate: rand.date(),
                    expired: rand.date(),
                    realocate: false,
                    activities: [],
                    tags:[]
                }
            ]
        }

        gkal19-aula-bemean-2468380(mongod-2.6.11) db> db.projects.update({name:'js4girls'}, {$setOnInsert:project}, {upsert:1});
       WriteResult({
        "nMatched" : 0,
        "nUpserted" : 1,
        "nModified" : 0,
        "_id" : ObjectId("56abfc71b216090c84d62478")
})
```

## Delete - remoção
#### Apague todos os projetos que não possuam tags.

```js

        gkal19-aula-bemean-2468380(mongod-2.6.11) db> db.projects.remove({tags:[]})
        WriteResult({ "nRemoved" : 1 })
})

```

#### Apague todos os projetos que não possuam comentários nas atividades.
```js

        gkal19-aula-bemean-2468380(mongod-2.6.11) db> var activities = db.activities.find({comments:[]}, {"_id":1}).toArray();
        gkal19-aula-bemean-2468380(mongod-2.6.11) db> var values = [];
        
        gkal19-aula-bemean-2468380(mongod-2.6.11) db> 
        gkal19-aula-bemean-2468380(mongod-2.6.11) db> activities.forEach(function(activity){
           db.activities.remove({"_id":activity._id});
           values.push(activity._id.valueOf());
           });
        gkal19-aula-bemean-2468380(mongod-2.6.11) db> 
        gkal19-aula-bemean-2468380(mongod-2.6.11) db> db.projects.remove({
           'goals.activities': {$in:values}
           });
        WriteResult({
          "nRemoved": 0
})
```

#### Apague todos os projetos que não possuam atividades.

```js

gkal19-aula-bemean-2468380(mongod-2.6.11) db> db.projects.remove({
   'goals.activities': []
  });
WriteResult({ "nRemoved" : 1 })

})

```

#### Escolha 2 usuários e apague todos os projetos em que os 2 fazem parte.

```js
gkal19-aula-bemean-2468380(mongod-2.6.11) db> var id_users = [];
gkal19-aula-bemean-2468380(mongod-2.6.11) db> users = db.users.find({},{"_id":1}).limit(2).toArray();
[
        {
                "_id" : ObjectId("56abf9aaa4807d7fb6f5c159")
        },
        {
                "_id" : ObjectId("56abf9aaa4807d7fb6f5c15a")
        }
]
gkal19-aula-bemean-2468380(mongod-2.6.11) db> users.forEach(function(user){
   id_users.push(user._id.valueOf());
   });
gkal19-aula-bemean-2468380(mongod-2.6.11) db> db.projects.remove({'members.user_id':{$all:id_users}});
WriteResult({ "nRemoved" : 2 })

})
  
```
#### Apague todos os projetos que possuam uma determinada tag em goal.
```js
  gkal19-aula-bemean-2468380(mongod-2.6.11) db> db.projects.remove({
   'goals.tags': {$nin:['computacao']}
   });
        WriteResult({ "nRemoved" : 2 })
})
```

## Gerenciamento de usuários
#### Crie um usuário com permissões APENAS de Leitura.

```js

  gkal19-aula-bemean-2468380(mongod-2.6.11) db> db.runCommand({
     createUser:"Gabriel",
     pwd:"123",
     roles:[{role:"read", db:"be-mean-mongo"}]
   });
{
  "ok": 1
}
```

#### Crie um usuário com permissões de Escrita e Leitura.
```js

gkal19-aula-bemean-2468380(mongod-2.6.11) db> db.runCommand({
    createUser:"Arroz",
    pwd:"321",
    roles:[{role:"readWrite", db:"be-mean-mongo"}]
    });
{
  "ok": 1
}
```

#### Adicionar o papel grantRolesToUser e revokeRole para o usuário com Escrita e Leitura.
Mudei de banco da dados a partir do `grantRolesToUser`.

```js

  db.runCommand({ createRole: "grantRolesToUser",
    privileges: [{resource: { db: "be-mean-mongo", collection: "" }, actions: ["grantRole"]}],
    roles: [],
    writeConcern: { w: "majority" , wtimeout: 5000 }
  });
  
  db.runCommand({ createRole: "revokeRole",
    privileges: [{resource: { db: "be-mean-mongo", collection: "" }, actions: ["revokeRole"]},],
    roles: [],
    writeConcern: { w: "majority" , wtimeout: 5000 }
  });
  
  db.runCommand({
    grantRolesToUser: 'Arroz',
    roles:[
        {role:"grantRolesToUser", db:"be-mean-mongo"},
        {role:"revokeRole", db:"be-mean-mongo"}
    ],
    writeConcern:{w:"majority", wtimeout:5000}
  });
  
```

#### Remover o papel grantRolesToUser para o usuário com Escrita e Leitura.
```js

  db.runCommand({
    revokeRolesFromUser: 'Arroz',
    roles:[
        {role:"grantRolesToUser", db:"be-mean-mongo"},
    ],
    writeConcern:{w:"majority"}
  });
{
  "ok": 1
}
```

#### Listar todos os usuários com seus papéis e ações.
```js

gkal19-aula-bemean-2468380(mongod-2.6.11) be-mean> db.runCommand({
     usersInfo:[
         {user: 'Gabriel', db:'be-mean-mongo'},
         {user: 'Arroz', db:'be-mean-mongo'}
     ],
     showCredentials:true,
     showPrivileges:true
   });
{
        "users" : [
                {
                        "_id" : "be-mean-mongo.Gabriel",
                        "user" : "Gabriel",
                        "db" : "be-mean-mongo",
                        "credentials" : {
                                "MONGODB-CR" : "61ec3c8fc9c144d2da3fbbd8ec6f8155"
                        },
                        "roles" : [
                                {
                                        "role" : "read",
                                        "db" : "be-mean-mongo"
                                }
                        ],
                        "inheritedRoles" : [
                                {
                                        "role" : "read",
                                        "db" : "be-mean-mongo"
                                }
                        ],
                        "inheritedPrivileges" : [
                                {
                                        "resource" : {
                                                "db" : "be-mean-mongo",
                                                "collection" : ""
                                        },
                                        "actions" : [
                                                "collStats",
                                                "dbHash",
                                                "dbStats",
                                                "find",
                                                "killCursors",
                                                "planCacheRead"
                                        ]
                                },
                                {
                                        "resource" : {
                                                "db" : "be-mean-mongo",
                                                "collection" : "system.indexes"
                                        },
                                        "actions" : [
                                                "collStats",
                                                "dbHash",
                                                "dbStats",
                                                "find",
                                                "killCursors",
                                                "planCacheRead"
                                        ]
                                },
                                {
                                        "resource" : {
                                                "db" : "be-mean-mongo",
                                                "collection" : "system.js"
                                        },
                                        "actions" : [
                                                "collStats",
                                                "dbHash",
                                                "dbStats",
                                                "find",
                                                "killCursors",
                                                "planCacheRead"
                                        ]
                                },
                                {
                                        "resource" : {
                                                "db" : "be-mean-mongo",
                                                "collection" : "system.namespaces"
                                        },
                                        "actions" : [
                                                "collStats",
                                                "dbHash",
                                                "dbStats",
                                                "find",
                                                "killCursors",
                                                "planCacheRead"
                                        ]
                                }
                        ]
                },
                {
                        "_id" : "be-mean-mongo.Arroz",
                        "user" : "Arroz",
                        "db" : "be-mean-mongo",
                        "credentials" : {
                                "MONGODB-CR" : "3e8b58a6ad14939784c6835502656546"
                        },
                        "roles" : [
                                {
                                        "role" : "revokeRole",
                                        "db" : "be-mean-mongo"
                                },
                                {
                                        "role" : "readWrite",
                                        "db" : "be-mean-mongo"
                                }
                        ],
                        "inheritedRoles" : [
                                {
                                        "role" : "revokeRole",
                                        "db" : "be-mean-mongo"
                                },
                                {
                                        "role" : "readWrite",
                                        "db" : "be-mean-mongo"
                                }
                        ],
                        "inheritedPrivileges" : [
                                {
                                        "resource" : {
                                                "db" : "be-mean-mongo",
                                                "collection" : ""
                                        },
                                        "actions" : [
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
                                                "planCacheRead",
                                                "remove",
                                                "renameCollectionSameDB",
                                                "revokeRole",
                                                "update"
                                        ]
                                },
                                {
                                        "resource" : {
                                                "db" : "be-mean-mongo",
                                                "collection" : "system.indexes"
                                        },
                                        "actions" : [
                                                "collStats",
                                                "dbHash",
                                                "dbStats",
                                                "find",
                                                "killCursors",
                                                "planCacheRead"
                                        ]
                                },
                                {
                                        "resource" : {
                                                "db" : "be-mean-mongo",
                                                "collection" : "system.js"
                                        },
                                        "actions" : [
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
                                                "planCacheRead",
                                                "remove",
                                                "renameCollectionSameDB",
                                                "update"
                                        ]
                                },
                                {
                                        "resource" : {
                                                "db" : "be-mean-mongo",
                                                "collection" : "system.namespaces"
                                        },
                                        "actions" : [
                                                "collStats",
                                                "dbHash",
                                                "dbStats",
                                                "find",
                                                "killCursors",
                                                "planCacheRead"
                                        ]
                                }
                        ]
                }
        ],
        "ok" : 1
}
```

## Sharding

```js
//Configurando server
gkal19:~/workspace $ sudo mongod --dbpath /var/lib/mongodb --configsvr --port 27050

//Router
gkal19:~/workspace $ mongos --configdb localhost:27050 --port 27060 

//Shardings
// 1 - Criando shardings
sudo mkdir /var/lib/mongodb/shard1
sudo mkdir /var/lib/mongodb/shard2
sudo mkdir /var/lib/mongodb/shard3

// 2 - Iniciando shardings
sudo mongod --port 27070 --dbpath /var/lib/mongodb/shard1
sudo mongod --port 27080 --dbpath /var/lib/mongodb/shard2
sudo mongod --port 27090 --dbpath /var/lib/mongodb/shard3

// 3 - Registrando shards no router
gkal19:~/workspace $ mongo --port 27060 --host localhost
MongoDB shell version: 2.6.11
connecting to: localhost:27060/test
mongos> sh.addShard("localhost:27070")
{ "shardAdded" : "shard0000", "ok" : 1 }
mongos> sh.addShard("localhost:27080")
{ "shardAdded" : "shard0001", "ok" : 1 }
mongos> sh.addShard("localhost:27090")
{ "shardAdded" : "shard0002", "ok" : 1 }

// 4 - "shardear" coleção e database
mongos> sh.enableSharding("be-mean-mongodb")
{ "ok" : 1 }
mongos> sh.shardCollection("be-mean-mongodb.projects", {"_id": 1})
{ "collectionsharded" : "be-mean-mongodb.projects", "ok" : 1 }
```

## Replicas
```js
// 1 - Criando as réplicas
mkdir /data/rs1
mkdir /data/rs2
mkdir /data/rs3

mongod --replSet replica_set --port 27017 --dbpath /data/rs1
mongod --replSet replica_set --port 27018 --dbpath /data/rs2
mongod --replSet replica_set --port 27019 --dbpath /data/rs3

// 2 - Adicionando shard as réplicas
mongo --port 27018
rsconf = {
    _id: "projeto_final",
    members:[
    {
        _id:0,
        host:"127.0.0.1:27017"
    }
    ]
}

rs.initiate(rsconf);
rs.add("127.0.0.1:27018");
rs.add("127.0.0.1:27029");

```
