# MongoDb - Projeto Final
**Autor:** Paulo Roberto<br>
**Data:** 1451770779970

## Para qual sistema você usaria o MogoDB (diferente desse)?
  Todo sistema que exige um grau de complexidade maior, o mongo na maioria das vezes é mais performático 
  que os banco de dados relacionais. Posso ver uma perfeita modelagem de quase tudo, seja de um pequeno **CRUD**
  ou até mesmo de algo mais complexo como uma **rede social**, mas em caso de sistema menores um banco de dados 
  relacional talvez se saia melhor, já em maiores não há comparação na escalabilidade, rapidez na procura de dados e 
  inserção isso o mongo está anos luz a frente dos relacionais, essa minha opnião diante do que vi usando ambos.
  
## Qual a modelagem da sua coleção de `users`?
As colunas da tabela `users` foram transformada em atributos, juntamente a coluna do `settings-system`. E as colunas da tabela `auth` foram
transformadas também em atributos de um novo objeto chamado auth, as colunas dessas duas tabelas foram colocadas em user por não
fazer o menor sentido separa-las em outras coleções se tratando de um banco não relacional e todas elas pertecerem diretamente a user.
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
As colunas da tabela `projects` aqui também viraram atributos, juntamente com as tabelas que tinha relação 1:1 assim como na modelagem de
`users`, e as que tinha relação 1:N se tornaram arrays, já todas pertenciam diretamente as projects e não possuiam nenhum campo(s) que podem-se
vim a estorar o limite do documento, como aconteceu entre a relação de `goals` com `activities`.
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
Retirei `activities`, pelo fato que se ela se mante-se dentro dos goals por sua vez dentro de `projects`, ela poderia estorar o limite
do documento(<b>16MB</b>), já que um goal(objetivo) pode ter varios activities(atividades), que pode vim a possuir N comentarios junto
com outras informações e além disso um project pode ter varios goals(objetivos) podendo causar assim um problema pela grande quantidade
de informações caso mantive-se todos juntos.
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
```js
    var rand = {
        number: function(number){
            var number = number || 1000;
            var randNumber = (Math.random()*number);
            return Math.round(randNumber);
        },
        date: function (timestamp){
            var timestamp = timestamp || Date.now();
            var date = new Date(this.number(timestamp));
            return date;
        },
        letter: function(number, alphabet){
            var alphabet = alphabet || 'abcdefghijklmnopqrstuvwxyz';
            var number = number || 1;
            var random_letters = '';
            alphabet = alphabet.split('');
            for(i=0; i<number; i++){
             random_letters += alphabet[this.number(alphabet.length-1)];
            }
            return random_letters;
        },
        array: function (array) {
            var lastKey = array.length - 1;
            for (var i = lastKey; i > 0; i--) {
                var randomKey = Math.floor(Math.random() * (i + 1));
                var temp = array[i];
                array[i] = array[randomKey];
                array[randomKey] = temp;
            }
            return array;
        }
    };

    function generateMembers(numberOfMembers, type_member){
      var notifies = [true, false];
      var numberOfMembers = numberOfMembers || 5;
      var skip = db.users.count()-numberOfMembers;
      var users = db.users.find({},{"_id":1}).limit(numberOfMembers).skip(rand.number(skip)).toArray();
      users.forEach(function(user){
          var notify = notifies[rand.number(1)];
          user.user_id = user._id.valueOf();
          delete user._id;
          user.type_member = type_member;
          user.notify = notify;
      });
            return rand.array(users);
  }
```
#### Cadastre 10 usuários diferentes.
```js
	// 1. Cadastre 10 usuários diferentes.
	var names = ['Paulo', 'Gabriel', 'Luccas', 'John', 'Rafael', 'Roberto', 'Claudio', 'João', 'Matheus', 'Adenildo'];
	names.forEach(function(name){
		var user = {
			name: name,
			bio: 'Projeto final mongoDB',
			avatar_path: 'imgs/avatares/'+rand.letter(6)+rand.number()+'.jpg',
			background_path: 'imgs/bgs/'+rand.letter(5)+rand.number()+'.gif',
			date_register: rand.date(),
			auth:{
				username: rand.letter(5),
				email: name+'@webschool.io',
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
	// Cadastrando activities para os goals
	var activities = [
		{name: 'MEAN - MongoDB', description: 'Falar sobre o banco não relacional mongoDB'},
		{name: 'MEAN - nodeJS', description: 'Explicar sobre javascript no server-side com node'},
		{name: 'MEAN - angularJS', description: 'Falar sobre o framework angular.js que faz parte da stack MEAN'},
		{name: 'PHP - História', description: 'Falar sobre a história do PHP'},
		{name: 'PHP - Instalação', description: 'Instalação e configuração do PHP em diferentes ambientes'},
		{name: 'Matemática - Adição matemática', description: 'Criar uma função para executar uma adição'},
		{name: 'Matemática - Subtração matemática', description: 'Criar uma função para executar uma subtração'},
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
		 text: 'Curso Be MEAN - Projeto final - mongoDB', 
		 Date_comment: rand.date(), 
		 files:[{path: 'imgs/files_comments/'+rand.letter(5)+'.jpg', weight: rand.number(9999999), name: 'arquivo de comentário'}],
		 members: generateMembers(1, 'aluno')
		}
	]
		db.activities.insert(activity);
	});
```
#### Cadastrando projects e goals.
```js
	
	
	// Cadastrando projects e goals
	var projects = [
		{
			name: 'Crie seu instagram com a stack MEAN',
			tags: ['js', 'instagram', 'MEAN', 'angular', 'mongoDB'],
			description: 'Curso que ensinara a criar um instagram usando a stack MEAN',
			goals:[
				{
					name: 'MEAN',
					description: 'Modulo voltado a explicar cada elemento da stack MEAN',
					tags:['mongoDB', 'angular', 'express', 'nodeJS'],
					activities:["56898c30d8c06e00b91437ba", "56898c30d8c06e00b91437bb", "56898c30d8c06e00b91437bc"]
				}
			]
		},
		{
			name: 'Popcorn time + Netflix school',
			description: 'Curso voltado a criar um aplicativo igual ao do Popcorn Time e netflix',
			tags: ['js', 'popcorn', 'netflix', 'school'],
			goals:[
				{
					name: 'Desktop e web',
					description: 'Modulos voltados a desenvolver o aplicativo para Desktop e Web',
					tags:['node-webkit', 'electron', 'webTorrent', 'nodeJS'],
					activities:["56898c30d8c06e00b91437c1", "56898c30d8c06e00b91437c2"]
				}
			]
		},
		{
			name: 'Javascript funcional',
			description: 'Curso voltado a mostrar o uso de JSFuncional',
			tags: ['js', 'funcional', 'programacao', 'webschool'],
			goals:[
				{
					name: 'O que é?',
					description: 'Parte que explicara o que é JS funcional',
					tags:['jsFuncional', 'exemplos'],
					activities:[]
				}
			]
		},
		{
			name: 'Matemática para programadores',
			description: 'Curso que ensinara matemática voltado para programadores',
			tags: ['matematica', 'operacoes', 'logaritimos', 'curso'],
			goals:[
				{
					name: 'Adição e subtração',
					description: 'Adições e subtrações matemáticas com funções',
					tags:['adcicao', 'subtracao', 'matematica'],
					activities:["56898c30d8c06e00b91437bf","56898c30d8c06e00b91437c0"]
				}
			]
		},
		{
			name: 'Curso PHP completo',
			description: 'Curso voltado a ensinar do básico ao avançado sobre PHP',
			tags: ['php', 'linux', 'basico', 'avançado', 'curso'],
			goals:[
				{
					name: 'Historia e instalação do PHP',
					description: 'Modulos que explicaram a história do PHP e sua instalação',
					tags:['mac', 'linux', 'windows', 'php'],
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
#### Liste as informações dos membros de 1 projeto específico.
```js
//1.Liste as informações dos membros de 1 projeto específico que deve ser buscado pelo seu nome de forma a não ligar para maiúsculas e minúsculas.
	var project = db.projects.findOne({name:/Curso PHP completo/i},{members:1});
	> project
  {
        "_id" : ObjectId("568c7aaaad6747f357ce48ca"),
        "members" : [
                {
                        "user_id" : "568987ced8c06e00b91437b4",
                        "type_member" : "aluno",
                        "notify" : false
                },
                {
                        "user_id" : "568987ced8c06e00b91437b2",
                        "type_member" : "aluno",
                        "notify" : true
                },
                {
                        "user_id" : "568987ced8c06e00b91437b6",
                        "type_member" : "aluno",
                        "notify" : true
                },
                {
                        "user_id" : "568987ced8c06e00b91437b3",
                        "type_member" : "aluno",
                        "notify" : true
                },
                {
                        "user_id" : "568987ced8c06e00b91437b5",
                        "type_member" : "aluno",
                        "notify" : false
                }
        ]
}

	var members_project = [];
	project.members.forEach(function(member){
		members_project.push(db.users.findOne({"_id":ObjectId(member.user_id)}));
	});
	
	> members_project
		[
        {
                "_id" : ObjectId("568987ced8c06e00b91437b4"),
                "name" : "Rafael",
                "bio" : "Projeto final mongoDB",
                "avatar_path" : "imgs/avatares/tmkuxp597.jpg",
                "background_path" : "imgs/bgs/cxmns299.gif",
                "date_register" : ISODate("1974-01-17T01:26:57.326Z"),

                "auth" : {
                        "username" : "wdpmr",
                        "email" : "Rafael@webschool.io",
                        "password" : 5313703330406,
                        "hash_token" : "gozrb279zgtjc148",
                        "online" : true,
                        "disabled" : false,
                        "last_access" : ISODate("1974-01-17T02:47:54.550Z")
                }
        },
        {
                "_id" : ObjectId("568987ced8c06e00b91437b2"),
                "name" : "Luccas",
                "bio" : "Projeto final mongoDB",
                "avatar_path" : "imgs/avatares/kimgpu682.jpg",
                "background_path" : "imgs/bgs/rwqlt109.gif",
                "date_register" : ISODate("1990-01-05T05:55:33.734Z"),

                "auth" : {
                        "username" : "uuaze",
                        "email" : "Luccas@webschool.io",
                        "password" : 3973441043780,
                        "hash_token" : "fahyd615uvxgr876",
                        "online" : true,
                        "disabled" : false,
                        "last_access" : ISODate("1990-01-05T07:24:17.517Z")
                }
        },
        {
                "_id" : ObjectId("568987ced8c06e00b91437b6"),
                "name" : "Claudio",
                "bio" : "Projeto final mongoDB",
                "avatar_path" : "imgs/avatares/seqscw957.jpg",
                "background_path" : "imgs/bgs/gvihn538.gif",
                "date_register" : ISODate("2006-12-10T20:30:37.378Z"),

                "auth" : {
                        "username" : "bvvip",
                        "email" : "Claudio@webschool.io",
                        "password" : 4503049910008,
                        "hash_token" : "uizmm331xmhws460",
                        "online" : true,
                        "disabled" : false,
                        "last_access" : ISODate("2006-12-10T23:11:54.223Z")
                }
        },
        {
                "_id" : ObjectId("568987ced8c06e00b91437b3"),
                "name" : "John",
                "bio" : "Projeto final mongoDB",
                "avatar_path" : "imgs/avatares/wlsmih873.jpg",
                "background_path" : "imgs/bgs/djcqi655.gif",
                "date_register" : ISODate("2012-12-05T03:50:34.935Z"),

                "auth" : {
                        "username" : "jimdf",
                        "email" : "John@webschool.io",
                        "password" : 9101906577484,
                        "hash_token" : "bfjtr534pxpzi143",
                        "online" : true,
                        "disabled" : false,
                        "last_access" : ISODate("2012-12-05T04:48:10.621Z")
                }
        },
        {
                "_id" : ObjectId("568987ced8c06e00b91437b5"),
                "name" : "Roberto",
                "bio" : "Projeto final mongoDB",
                "avatar_path" : "imgs/avatares/slwdfa736.jpg",
                "background_path" : "imgs/bgs/ibwjm801.gif",
                "date_register" : ISODate("1989-05-21T09:33:21.648Z"),

                "auth" : {
                        "username" : "kocqe",
                        "email" : "Roberto@webschool.io",
                        "password" : 5745655982894,
                        "hash_token" : "frzoq684wyeud865",
                        "online" : true,
                        "disabled" : false,
                        "last_access" : ISODate("1989-05-21T10:25:38.563Z")
                }
        }
]
```
#### Liste todos os projetos com a tag que você escolheu para os 3 projetos em comum.
```js
	// 2.Liste todos os projetos com a tag que você escolheu para os 3 projetos em comum.
	> db.projects.find({tags:{$in:['js']}}).pretty()
{
        "_id" : ObjectId("568c7aaaad6747f357ce48c6"),
        "name" : "Crie seu instagram com a stack MEAN",
        "tags" : [
                "js",
                "instagram",
                "MEAN",
                "angular",
                "mongoDB"
        ],
        "description" : "Curso que ensinara a criar um instagram usando a stack MEAN",
        "goals" : [
                {
                        "name" : "MEAN",
                        "description" : "Modulo voltado a explicar cada elemento da stack MEAN",
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
                        "date_begin" : ISODate("1976-12-18T04:43:58.051Z"),
                        "date_dream" : ISODate("1972-08-01T20:26:19.268Z"),
                        "date_end" : ISODate("1988-01-22T00:14:40.023Z"),
                        "date_relocate" : ISODate("1995-05-10T05:13:21.813Z"),
                        "expired" : ISODate("2015-10-29T16:15:22.457Z"
),
                        "realocate" : false
                }
        ],
        "date_begin" : ISODate("2009-07-27T14:36:12.896Z"),
        "date_dream" : ISODate("1999-02-13T21:20:02.284Z"),
        "date_end" : ISODate("2015-01-12T14:25:43.728Z"),
        "expired" : ISODate("1991-02-03T16:01:05.241Z"),
        "visible" : true,
        "members" : [
                {
                        "user_id" : "568987ced8c06e00b91437b5",
                        "type_member" : "aluno",
                        "notify" : true
                },
                {
                        "user_id" : "568987ced8c06e00b91437b2",
                        "type_member" : "aluno",
                        "notify" : false
                },
                {
                        "user_id" : "568987ced8c06e00b91437b3",
                        "type_member" : "aluno",
                        "notify" : true
                },
                {
                        "user_id" : "568987ced8c06e00b91437b6",
                        "type_member" : "aluno",
                        "notify" : false
                },
                {
                        "user_id" : "568987ced8c06e00b91437b4",
                        "type_member" : "aluno",
                        "notify" : true
                }
        ],
        "realocate" : false,
        "visualizable_mod" : "all"
}
{
        "_id" : ObjectId("568c7aaaad6747f357ce48c7"),
        "name" : "Popcorn time + Netflix school",
        "description" : "Curso voltado a criar um aplicativo igual ao do Popcorn Time e netflix",
        "tags" : [
                "js",
                "popcorn",
                "netflix",
                "school"
        ],
        "goals" : [
                {
                        "name" : "Desktop e web",
                        "description" : "Modulos voltados a desenvolver o aplicativo para Desktop e Web",
                        "tags" : [
                                "node-webkit",
                                "electron",
                                "webTorrent",
                                "nodeJS"
                        ],
                        "activities" : [
                                "56898c30d8c06e00b91437c1",
                                "56898c30d8c06e00b91437c2"
                        ],
                        "date_begin" : ISODate("2006-07-03T13:08:27.752Z"),
                        "date_dream" : ISODate("1976-07-11T09:42:47.924Z"),
                        "date_end" : ISODate("1990-01-27T20:39:58.954Z"),
                        "date_relocate" : ISODate("1994-08-24T07:06:24.883Z"),
                        "expired" : ISODate("1994-11-23T06:05:07.271Z"
),
                        "realocate" : false
                }
        ],
        "date_begin" : ISODate("1970-02-20T06:33:56.424Z"),
        "date_dream" : ISODate("1973-01-10T13:47:11.922Z"),
        "date_end" : ISODate("1997-06-30T00:51:15.508Z"),
        "expired" : ISODate("1987-12-10T01:57:10.868Z"),
        "visible" : true,
        "members" : [
                {
                        "user_id" : "568987ced8c06e00b91437b5",
                        "type_member" : "aluno",
                        "notify" : true
                },
                {
                        "user_id" : "568987ced8c06e00b91437b7",
                        "type_member" : "aluno",
                        "notify" : false
                },
                {
                        "user_id" : "568987ced8c06e00b91437b4",
                        "type_member" : "aluno",
                        "notify" : false
                },
                {
                        "user_id" : "568987ced8c06e00b91437b6",
                        "type_member" : "aluno",
                        "notify" : true
                },
                {
                        "user_id" : "568987ced8c06e00b91437b3",
                        "type_member" : "aluno",
                        "notify" : false
                }
        ],
        "realocate" : false,
        "visualizable_mod" : "all"
}
{
        "_id" : ObjectId("568c7aaaad6747f357ce48c8"),
        "name" : "Javascript funcional",
        "description" : "Curso voltado a mostrar o uso de JSFuncional",
        "tags" : [
                "js",
                "funcional",
                "programacao",
                "webschool"
        ],
        "goals" : [
                {
                        "name" : "O que é?",
                        "description" : "Parte que explicara o que é JS funcional",
                        "tags" : [
                                "jsFuncional",
                                "exemplos"
                        ],
                        "activities" : [ ],
                        "date_begin" : ISODate("1974-03-04T09:11:02.758Z"),
                        "date_dream" : ISODate("1976-11-11T16:36:42.587Z"),
                        "date_end" : ISODate("1998-07-20T07:50:04.335Z"),
                        "date_relocate" : ISODate("2013-05-01T12:01:21.147Z"),
                        "expired" : ISODate("2015-07-09T10:36:58.671Z"),
                        "realocate" : false
                }
        ],
        "date_begin" : ISODate("1974-05-16T03:35:20Z"),
        "date_dream" : ISODate("1988-07-15T18:15:27.732Z"),
        "date_end" : ISODate("1999-10-22T01:39:33.464Z"),
        "expired" : ISODate("2005-07-15T09:33:03.559Z"),
        "visible" : true,
        "members" : [
                {
                        "user_id" : "568987ced8c06e00b91437b1",
                        "type_member" : "aluno",
                        "notify" : false
                },
                {
                        "user_id" : "568987ced8c06e00b91437b2",
                        "type_member" : "aluno",
                        "notify" : false
                },
                {
                        "user_id" : "568987ced8c06e00b91437b4",
                        "type_member" : "aluno",
                        "notify" : false
                },
                {
                        "user_id" : "568987ced8c06e00b91437b3",
                        "type_member" : "aluno",
                        "notify" : false
                },
                {
                        "user_id" : "568987ced8c06e00b91437b5",
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
// 3.Liste apenas os nomes de todas as atividades para todos os projetos
  var activities = db.activities.find({}, {name:1}).toArray()
  var activities_names = [];
  
  activities.forEach(function(activity){
  	var id = activity._id.valueOf();
  	var project = db.projects.findOne({'goals.activities': {$in:[id]}},{"_id":1});
  	 	if(project)
	     activities_names.push({
		     id_project:project._id,
		     activity: activity.name
	    });
  });

> activities_names
[
        {
                "id_project" : ObjectId("568c7aaaad6747f357ce48c6"),
                "activity" : "MEAN - MongoDB"
        },
        {
                "id_project" : ObjectId("568c7aaaad6747f357ce48c6"),
                "activity" : "MEAN - nodeJS"
        },
        {
                "id_project" : ObjectId("568c7aaaad6747f357ce48c6"),
                "activity" : "MEAN - angularJS"
        },
        {
                "id_project" : ObjectId("568c7aaaad6747f357ce48c7"),
                "activity" : "Popcorn - Desktop"
        },
        {
                "id_project" : ObjectId("568c7aaaad6747f357ce48c7"),
                "activity" : "Popcorn - Web"
        },
        {
                "id_project" : ObjectId("568c7aaaad6747f357ce48c9"),
                "activity" : "Matemática - Adição matemática"
        },
        {
                "id_project" : ObjectId("568c7aaaad6747f357ce48c9"),
                "activity" : "Matemática - Subtração matemática"
        },
        {
                "id_project" : ObjectId("568c7aaaad6747f357ce48ca"),
                "activity" : "PHP - História"
        },
        {
                "id_project" : ObjectId("568c7aaaad6747f357ce48ca"),
                "activity" : "PHP - Instalação"
        }
]

```
#### Liste todos os projetos que não possuam uma tag.
```js
// 4.Liste todos os projetos que não possuam uma tag.
> db.projects.find({'tags':{$nin:['js']}}).pretty()
{
        "_id" : ObjectId("568c7aaaad6747f357ce48c9"),
        "name" : "Matemática para programadores",
        "description" : "Curso que ensinara matemática voltado para programadores",
        "tags" : [
                "matematica",
                "operacoes",
                "logaritimos",
                "curso"
        ],
        "goals" : [
                {
                        "name" : "Adição e subtração",
                        "description" : "Adições e subtrações matemáticas com funções",
                        "tags" : [
                                "adcicao",
                                "subtracao",
                                "matematica"
                        ],
                        "activities" : [
                                "56898c30d8c06e00b91437bf",
                                "56898c30d8c06e00b91437c0"
                        ],
                        "date_begin" : ISODate("1983-12-28T01:17:58.507Z"),
                        "date_dream" : ISODate("1984-10-31T03:06:30.692Z"),
                        "date_end" : ISODate("1984-12-01T08:44:27.803Z"),
                        "date_relocate" : ISODate("2010-12-02T19:41:25.846Z"),
                        "expired" : ISODate("1982-08-07T18:47:05.099Z"),
                        "realocate" : false
                }
        ],
        "date_begin" : ISODate("1984-12-04T05:43:55.397Z"),
        "date_dream" : ISODate("1975-02-15T06:57:04.302Z"),
        "date_end" : ISODate("1990-11-06T04:26:05.743Z"),
        "expired" : ISODate("1997-12-09T05:16:42.070Z"),
        "visible" : true,
        "members" : [
                {
                        "user_id" : "568987ced8c06e00b91437b1",
                        "type_member" : "aluno",
                        "notify" : true
                },
                {
                        "user_id" : "568987ced8c06e00b91437b3",
                        "type_member" : "aluno",
                        "notify" : false
                },
                {
                        "user_id" : "568987ced8c06e00b91437b5",
                        "type_member" : "aluno",
                        "notify" : false
                },
                {
                        "user_id" : "568987ced8c06e00b91437b4",
                        "type_member" : "aluno",
                        "notify" : false
                },
                {
                        "user_id" : "568987ced8c06e00b91437b2",
                        "type_member" : "aluno",
                        "notify" : false
                }
        ],
        "realocate" : false,
        "visualizable_mod" : "all"
}
{
        "_id" : ObjectId("568c7aaaad6747f357ce48ca"),
        "name" : "Curso PHP completo",
        "description" : "Curso voltado a ensinar do básico ao avançado sobre PHP",
        "tags" : [
                "php",
                "linux",
                "basico",
                "avançado",
                "curso"
        ],
        "goals" : [
                {
                        "name" : "Historia e instalação do PHP",
                        "description" : "Modulos que explicaram a história do PHP e sua instalação",
                        "tags" : [
                                "mac",
                                "linux",
                                "windows",
                                "php"
                        ],
                        "activities" : [
                                "56898c30d8c06e00b91437bd",
                                "56898c30d8c06e00b91437be"
                        ],
                        "date_begin" : ISODate("1980-08-01T03:47:34.258Z"),
                        "date_dream" : ISODate("1976-09-28T18:56:44.339Z"),
                        "date_end" : ISODate("2015-02-24T22:19:10.841Z"),
                        "date_relocate" : ISODate("2004-12-13T06:40:08.164Z"),
                        "expired" : ISODate("1972-11-26T17:30:51.877Z"
),
                        "realocate" : false
                }
        ],
        "date_begin" : ISODate("1991-03-16T14:52:05.822Z"),
        "date_dream" : ISODate("2012-04-22T00:38:20.587Z"),
        "date_end" : ISODate("1991-04-18T11:48:36.859Z"),
        "expired" : ISODate("1978-12-15T06:06:51.573Z"),
        "visible" : true,
        "members" : [
                {
                        "user_id" : "568987ced8c06e00b91437b4",
                        "type_member" : "aluno",
                        "notify" : false
                },
                {
                        "user_id" : "568987ced8c06e00b91437b2",
                        "type_member" : "aluno",
                        "notify" : true
                },
                {
                        "user_id" : "568987ced8c06e00b91437b6",
                        "type_member" : "aluno",
                        "notify" : true
                },
                {
                        "user_id" : "568987ced8c06e00b91437b3",
                        "type_member" : "aluno",
                        "notify" : true
                },
                {
                        "user_id" : "568987ced8c06e00b91437b5",
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
// 5.Liste todos os usuários que não fazem parte do primeiro projeto cadastrado
  var members = db.projects.findOne({},{members:1, _id:0});
  var notOr = [];
  members.members.forEach(function(member){
  	notOr.push({"_id":ObjectId(member.user_id)});
  });
  db.users.find({$nor:notOr}).pretty();
  {
        "_id" : ObjectId("568987ced8c06e00b91437b0"),
        "name" : "Paulo",
        "bio" : "Projeto final mongoDB",
        "avatar_path" : "imgs/avatares/hxrtwg814.jpg",
        "background_path" : "imgs/bgs/uwnfc128.gif",
        "date_register" : ISODate("1979-05-31T23:04:04.354Z"),
        "auth" : {
                "username" : "vvvmv",
                "email" : "Paulo@webschool.io",
                "password" : 5835774406809,
                "hash_token" : "xqelo432gthwm923",
                "online" : true,
                "disabled" : false,
                "last_access" : ISODate("1979-06-01T01:31:03.267Z")
        }
}
{
        "_id" : ObjectId("568987ced8c06e00b91437b1"),
        "name" : "Gabriel",
        "bio" : "Projeto final mongoDB",
        "avatar_path" : "imgs/avatares/etcnbx388.jpg",
        "background_path" : "imgs/bgs/wxsiv786.gif",
        "date_register" : ISODate("1976-03-03T13:01:29.234Z"),
        "auth" : {
                "username" : "mtkbu",
                "email" : "Gabriel@webschool.io",
                "password" : 3900077291132,
                "hash_token" : "qkrer620fbslm459",
                "online" : true,
                "disabled" : false,
                "last_access" : ISODate("1976-03-03T15:01:48.246Z")
        }
}
{
        "_id" : ObjectId("568987ced8c06e00b91437b7"),
        "name" : "João",
        "bio" : "Projeto final mongoDB",
        "avatar_path" : "imgs/avatares/ywrejx871.jpg",
        "background_path" : "imgs/bgs/wnngu550.gif",
        "date_register" : ISODate("2011-07-01T15:34:50.998Z"),
        "auth" : {
                "username" : "ialiu",
                "email" : "João@webschool.io",
                "password" : 340242364803,
                "hash_token" : "oykrr213ooqjp971",
                "online" : true,
                "disabled" : false,
                "last_access" : ISODate("2011-07-01T17:50:02.009Z")
        }
}
{
        "_id" : ObjectId("568987ced8c06e00b91437b8"),
        "name" : "Matheus",
        "bio" : "Projeto final mongoDB",
        "avatar_path" : "imgs/avatares/judcbq725.jpg",
        "background_path" : "imgs/bgs/jeygk658.gif",
        "date_register" : ISODate("2008-07-28T15:03:44.944Z"),
        "auth" : {
                "username" : "ahdcw",
                "email" : "Matheus@webschool.io",
                "password" : 6437306780732,
                "hash_token" : "jbsvf272chopl979",
                "online" : true,
                "disabled" : false,
                "last_access" : ISODate("2008-07-28T17:15:25.677Z")
        }
}
{
        "_id" : ObjectId("568987ced8c06e00b91437b9"),
        "name" : "Adenildo",
        "bio" : "Projeto final mongoDB",
        "avatar_path" : "imgs/avatares/lidnkq415.jpg",
        "background_path" : "imgs/bgs/yxyws897.gif",
        "date_register" : ISODate("2009-10-03T13:57:58.930Z"),
        "auth" : {
                "username" : "tsbuv",
                "email" : "Adenildo@webschool.io",
                "password" : 2206570806028,
                "hash_token" : "gqggj514ozdcf977",
                "online" : true,
                "disabled" : false,
                "last_access" : ISODate("2009-10-03T16:31:08.450Z")
        }
}
```
## Update - alteração
#### Adicione para todos os projetos o campo views: 0.
```js
 var query = {}
 var change = {$set:{views:0}}
 var options = {multi:1}
 db.projects.update(query,change,options)
WriteResult({ "nMatched" : 5, "nUpserted" : 0, "nModified" : 0 })
```
#### Adicione 1 tag diferente para cada projeto.
```js
	var tags = ['tag1','tag2','tag3','tag4','tag5'];
	var count = 0;
	var projects = db.projects.find({},{"_id":1}).toArray();
	projects.forEach(function(project){
		var query = {"_id":project._id};
		var change = {$push:{tags:tags[count]}}
		var options = {multi:1}
		db.projects.update(query,change,options);
		count++;
	});
```
#### Adicione 2 membros diferentes para cada projeto.
```js
	var users = db.users.find({}, {"_id":1}).toArray();
	var projects = db.projects.find({},{members:1}).toArray();
	var numberAddRegister = 2;

	projects.forEach(function(project){

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
```
#### Adicione 1 comentário em cada atividade, deixe apenas 1 projeto sem.
```js

// Todas as atividades foram adcionadas com um comentário, então foi apenas retirado os comentários das atividades de um projeto.

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
    name: 'js4girls',
    description: 'Evento free de javascript apenas para mulheres.',
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
            description: 'Trazer mulheres para programação',
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

db.projects.update({name:'js4girls'}, {$setOnInsert:project}, {upsert:1});
```
## Delete - remoção
#### Apague todos os projetos que não possuam tags.
```js
> db.projects.remove({tags:[]})
WriteResult({ "nRemoved" : 1 })
```
#### Apague todos os projetos que não possuam comentários nas atividades.
```js
  var activities = db.activities.find({comments:[]}, {"_id":1}).toArray();
  var values = [];
  
  activities.forEach(function(activity){
  	db.activities.remove({"_id":activity._id});
  	values.push(activity._id.valueOf());
  });
  
  db.projects.remove({
  	'goals.activities': {$in:values}
  });
```
#### Apague todos os projetos que não possuam atividades.
```js
  db.projects.remove({
  	'goals.activities': []
  });
  WriteResult({ "nRemoved" : 1 })
```
#### Escolha 2 usuários e apague todos os projetos em que os 2 fazem parte.
```js
  var id_users = [];
  users = db.users.find({},{"_id":1}).limit(2).toArray();
  users.forEach(function(user){
  	id_users.push(user._id.valueOf());
  });
  db.projects.remove({'members.user_id':{$all:id_users}});
  >WriteResult({ "nRemoved" : 1 });
```
#### Apague todos os projetos que possuam uma determinada tag em goal.
```js
  db.projects.remove({
  	'goals.tags': {$nin:['php']}
  });
  WriteResult({ "nRemoved" : 1 })
```
## Gerenciamento de usuários
#### Crie um usuário com permissões APENAS de Leitura.
```js
  db.runCommand({
  	createUser:"Paulo",
  	pwd:"123",
  	roles:[{role:"read", db:"projeto"}]
  });
```
#### Crie um usuário com permissões de Escrita e Leitura.
```js
  db.runCommand({
  	createUser:"Batata",
  	pwd:"321",
  	roles:[{role:"readWrite", db:"projeto"}]
  });
```
#### Adicionar o papel grantRolesToUser e revokeRole para o usuário com Escrita e Leitura.
```js
  db.runCommand({ createRole: "grantRolesToUser",
    privileges: [{resource: { db: "projeto", collection: "" }, actions: ["grantRole"]}],
    roles: [],
    writeConcern: { w: "majority" , wtimeout: 5000 }
  });
  
  db.runCommand({ createRole: "revokeRole",
    privileges: [{resource: { db: "projeto", collection: "" }, actions: ["revokeRole"]},],
    roles: [],
    writeConcern: { w: "majority" , wtimeout: 5000 }
  });
  
  db.runCommand({
  	grantRolesToUser: 'Batata',
  	roles:[
  		{role:"grantRolesToUser", db:"projeto"},
  		{role:"revokeRole", db:"projeto"}
  	],
  	writeConcern:{w:"majority", wtimeout:5000}
  });  
```
#### Remover o papel grantRolesToUser para o usuário com Escrita e Leitura.
```js
  db.runCommand({
  	revokeRolesFromUser: 'Batata',
  	roles:[
  		{role:"grantRolesToUser", db:"projeto"},
  	],
  	writeConcern:{w:"majority"}
  });
```
#### Listar todos os usuários com seus papéis e ações.
```js
  db.runCommand({
  	usersInfo:[
  		{user: 'Batata', db:'projeto'},
  		{user: 'Paulo', db:'projeto'}
  	],
  	showCredentials:true,
  	showPrivileges:true
  });
  
  // User Paulo  
{
        "users" : [
                {
                        "_id" : "projeto.Paulo",
                        "user" : "Paulo",
                        "db" : "projeto",
                        "credentials" : {
                                "SCRAM-SHA-1" : {
                                        "iterationCount" : 10000,
                                        "salt" : "+DzpZ8RNPe/lS0mj/VwqFA==",
                                        "storedKey" : "AztAdnU7DkfqGcvrrASetRvO4Z8=",
                                        "serverKey" : "lno+I4Vw0+zdCq8p+OEIGAW59cc="
                                }
                        },
                        "roles" : [
                                {
                                        "role" : "read",
                                        "db" : "projeto"
                                }
                        ],
                        "inheritedRoles" : [
                                {
                                        "role" : "read",
                                        "db" : "projeto"
                                }
                        ],
                        "inheritedPrivileges" : [
                                {
                                        "resource" : {
                                                "db" : "projeto",
                                                "collection" : ""
                                        },
                                        "actions" : [
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
                                        "resource" : {
                                                "anyResource" : true
                                        },
                                        "actions" : [
                                                "listCollections"
                                        ]
                                },
                                {
                                        "resource" : {
                                                "db" : "projeto",
                                                "collection" : "system.indexes"
                                        },
                                        "actions" : [
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
                                        "resource" : {
                                                "db" : "projeto",
                                                "collection" : "system.js"
                                        },
                                        "actions" : [
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
                                        "resource" : {
                                                "db" : "projeto",
                                                "collection" : "system.namespaces"
                                        },
                                        "actions" : [
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
        "ok" : 1
}

// User batata
{
        "users" : [
                {
                        "_id" : "projeto.Batata",
                        "user" : "Batata",
                        "db" : "projeto",
                        "credentials" : {
                                "SCRAM-SHA-1" : {
                                        "iterationCount" : 10000,
                                        "salt" : "StlbNkvwhoLm/5USozayNQ==",
                                        "storedKey" : "4Olmo/ZvJioTyUtJUB6gkpm9E/4=",
                                        "serverKey" : "qMBqVvhwMsfq+OmM/psOIwuTjUo="
                                }
                        },
                        "roles" : [
                                {
                                        "role" : "readWrite",
                                        "db" : "projeto"
                                },
                                {
                                        "role" : "revokeRole",
                                        "db" : "projeto"
                                }
                        ],
                        "inheritedRoles" : [
                                {
                                        "role" : "readWrite",
                                        "db" : "projeto"
                                },
                                {
                                        "role" : "revokeRole",
                                        "db" : "projeto"
                                }
                        ],
                        "inheritedPrivileges" : [
                                {
                                        "resource" : {
                                                "db" : "projeto",
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
                                        "resource" : {
                                                "anyResource" : true
                                        },
                                        "actions" : [
                                                "listCollections"
                                        ]
                                },
                                {
                                        "resource" : {
                                                "db" : "projeto",
                                                "collection" : "system.indexes"
                                        },
                                        "actions" : [
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
                                        "resource" : {
                                                "db" : "projeto",
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
                                                "listCollections",
                                                "listIndexes",
                                                "planCacheRead",
                                                "remove",
                                                "renameCollectionSameDB",
                                                "update"
                                        ]
                                },
                                {
                                        "resource" : {
                                                "db" : "projeto",
                                                "collection" : "system.namespaces"
                                        },
                                        "actions" : [
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
        "ok" : 1
}
```
## Sharding
#### 1 Config Server.
```js
mkdir C:/data/configdb
mongod --configsvr --port 27010
  
2016-01-07T18:01:30.419-0200 I CONTROL  [main] Hotfix KB2731284 or later update is not installed, will zero-out data files
2016-01-07T18:01:30.421-0200 I CONTROL  [initandlisten] MongoDB starting : pid=8820 port=27010 dbpath=c:\data\configdb\ master=1 64-bit host=paulo-PC
2016-01-07T18:01:30.421-0200 I CONTROL  [initandlisten] targetMinOS: Windows 7/Windows Server 2008 R2
2016-01-07T18:01:30.422-0200 I CONTROL  [initandlisten] db version v3.2.0
2016-01-07T18:01:30.422-0200 I CONTROL  [initandlisten] git version: 45d947729a0315accb6d4f15a6b06be6d9c19fe7
2016-01-07T18:01:30.423-0200 I CONTROL  [initandlisten] OpenSSL version: OpenSSL 1.0.1p-fips 9 Jul 2015
2016-01-07T18:01:30.423-0200 I CONTROL  [initandlisten] allocator: tcmalloc
2016-01-07T18:01:30.423-0200 I CONTROL  [initandlisten] modules: none
2016-01-07T18:01:30.424-0200 I CONTROL  [initandlisten] build environment:
2016-01-07T18:01:30.424-0200 I CONTROL  [initandlisten]     distmod: 2008plus-ssl
2016-01-07T18:01:30.425-0200 I CONTROL  [initandlisten]     distarch:x86_64
2016-01-07T18:01:30.425-0200 I CONTROL  [initandlisten]     target_arch: x86_64
2016-01-07T18:01:30.425-0200 I CONTROL  [initandlisten] options: { net: { port: 27010 }, sharding: { clusterRole: "configsvr" } }2016-01-07T18:01:30.428-0200 I STORAGE  [initandlisten] wiredtiger_open config: create,cache_size=2G,session_max=20000,eviction=(threads_max=4),config_base=false,statistics=(fast),log=(enabled=true,archive=true,path=journal,compressor=snappy),file_manager=(close_idle_time=100000),checkpoint=(wait=60,log_size=2GB),statistics_log=(wait=0),direct_io=(data),
2016-01-07T18:01:31.744-0200 I REPL     [initandlisten] ******
2016-01-07T18:01:31.744-0200 I REPL     [initandlisten] creating replication oplog of size: 5MB...
2016-01-07T18:01:32.071-0200 I STORAGE  [initandlisten] Starting WiredTigerRecordStoreThread local.oplog.$main
2016-01-07T18:01:32.073-0200 I STORAGE  [initandlisten] The size storer reports that the oplog contains 0 records totaling to 0 bytes
2016-01-07T18:01:32.073-0200 I STORAGE  [initandlisten] Scanning the oplog to determine where to place markers for truncation
2016-01-07T18:01:32.786-0200 I REPL     [initandlisten] ******
2016-01-07T18:01:32.790-0200 I NETWORK  [HostnameCanonicalizationWorker] Starting hostname canonicalization worker
2016-01-07T18:01:32.790-0200 I FTDC     [initandlisten] Initializing full-time diagnostic data capture with directory 'c:/data/configdb/diagnostic.data'2016-01-07T18:01:33.027-0200 I NETWORK  [initandlisten] waiting for connections on port 27010
```
#### 1 Router.
```js
mongos --configdb localhost:27010 --port 27011

2016-01-07T18:09:09.113-0200 W SHARDING [main] Running a sharded cluster with fewer than 3 config servers should only be done for testing purposes and is not recommended for production.
2016-01-07T18:09:09.377-0200 I CONTROL  [main] Hotfix KB2731284 or later update is not installed, will zero-out data files
2016-01-07T18:09:09.378-0200 I SHARDING [mongosMain] MongoS version 3.2.0 starting: pid=4680 port=27011 64-bit host=paulo-PC (--help for usage)
2016-01-07T18:09:09.378-0200 I CONTROL  [mongosMain] db version v3.2.0

2016-01-07T18:09:09.379-0200 I CONTROL  [mongosMain] git version: 45d947729a0315accb6d4f15a6b06be6d9c19fe7
2016-01-07T18:09:09.379-0200 I CONTROL  [mongosMain] OpenSSL version:OpenSSL 1.0.1p-fips 9 Jul 2015
2016-01-07T18:09:09.380-0200 I CONTROL  [mongosMain] allocator: tcmalloc
2016-01-07T18:09:09.380-0200 I CONTROL  [mongosMain] modules: none
2016-01-07T18:09:09.380-0200 I CONTROL  [mongosMain] build environment:
2016-01-07T18:09:09.381-0200 I CONTROL  [mongosMain]     distmod: 2008plus-ssl
2016-01-07T18:09:09.381-0200 I CONTROL  [mongosMain]     distarch: x86_64
2016-01-07T18:09:09.382-0200 I CONTROL  [mongosMain]     target_arch:x86_64
2016-01-07T18:09:09.382-0200 I CONTROL  [mongosMain] options: { net: {port: 27011 }, sharding: { configDB: "localhost:27010" } }
2016-01-07T18:09:09.383-0200 I SHARDING [mongosMain] Updating config server connection string to: localhost:27010
2016-01-07T18:09:09.394-0200 I NETWORK  [HostnameCanonicalizationWorker] Starting hostname canonicalization worker
2016-01-07T18:09:09.394-0200 I SHARDING [Balancer] about to contact config servers and shards
2016-01-07T18:09:09.395-0200 I SHARDING [Balancer] config servers andshards contacted successfully
2016-01-07T18:09:09.396-0200 I SHARDING [Balancer] balancer id: paulo-PC:27011 started
2016-01-07T18:09:09.399-0200 I NETWORK  [mongosMain] waiting for connections on port 27011
2016-01-07T18:09:09.403-0200 I SHARDING [LockPinger] creating distributed lock ping thread for localhost:27010 and process paulo-PC:27011:1452197349:41 (sleeping for 30000ms)
2016-01-07T18:09:09.404-0200 I SHARDING [Balancer] distributed lock 'balancer/paulo-PC:27011:1452197349:41' acquired for 'doing balance round', ts : 568ec5e548abd228b5fcc39b2016-01-07T18:09:09.405-0200 I SHARDING [LockPinger] cluster localhost:27010 pinged successfully at 2016-01-07T18:09:09.404-0200 by distributed lock pinger 'localhost:27010/paulo-PC:27011:1452197349:41', sleeping for 30000ms2016-01-07T18:09:09.405-0200 I SHARDING [Balancer] about to log metadata event into actionlog: { _id: "paulo-PC-2016-01-07T18:09:09.405-0200-568ec5e548abd228b5fcc39c", server: "paulo-PC", clientAddr: "", time:new Date(1452197349405), what: "balancer.round", ns: "", details: { executionTimeMillis: 9, errorOccured: false, candidateChunks: 0, chunksMoved: 0 } }2016-01-07T18:09:09.439-0200 I SHARDING [Balancer] distributed lock 'balancer/paulo-PC:27011:1452197349:41' unlocked.
```

#### 3 Shardings.
```js
mkdir C:/data/shard1
mkdir C:/data/shard2
mkdir C:/data/shard3
mongod --port 27012 --dbpath C:/data/shard1
mongod --port 27013 --dbpath C:/data/shard2
mongod --port 27014 --dbpath C:/data/shard3

// Vou por apenas 1 resultado do terminal
2016-01-07T18:15:20.289-0200 I CONTROL  [main] Hotfix KB2731284 or later update is not installed, will zero-out data files
2016-01-07T18:15:20.291-0200 I CONTROL  [initandlisten] MongoDB starting : pid=9028 port=27014 dbpath=C:/data/shard3 64-bit host=paulo-PC
2016-01-07T18:15:20.291-0200 I CONTROL  [initandlisten] targetMinOS: Windows 7/Windows Server 2008 R2
2016-01-07T18:15:20.291-0200 I CONTROL  [initandlisten] db version v3.2.0
2016-01-07T18:15:20.291-0200 I CONTROL  [initandlisten] git version: 45d947729a0315accb6d4f15a6b06be6d9c19fe7
2016-01-07T18:15:20.291-0200 I CONTROL  [initandlisten] OpenSSL version: OpenSSL 1.0.1p-fips 9 Jul 2015
2016-01-07T18:15:20.291-0200 I CONTROL  [initandlisten] allocator: tcmalloc
2016-01-07T18:15:20.292-0200 I CONTROL  [initandlisten] modules: none
2016-01-07T18:15:20.292-0200 I CONTROL  [initandlisten] build environment:
2016-01-07T18:15:20.292-0200 I CONTROL  [initandlisten]     distmod: 2008plus-ssl
2016-01-07T18:15:20.293-0200 I CONTROL  [initandlisten]     distarch:x86_642016-01-07T18:15:20.293-0200 I CONTROL  [initandlisten]     target_arch: x86_64
2016-01-07T18:15:20.294-0200 I CONTROL  [initandlisten] options: { net: { port: 27014 }, storage: { dbPath: "C:/data/shard3" } }2016-01-07T18:15:20.296-0200 I STORAGE  [initandlisten] wiredtiger_open config: create,cache_size=2G,session_max=20000,eviction=(threads_max=4),config_base=false,statistics=(fast),log=(enabled=true,archive=true,path=journal,compressor=snappy),file_manager=(close_idle_time=100000),checkpoint=(wait=60,log_size=2GB),statistics_log=(wait=0),direct_io=(data),
2016-01-07T18:15:20.973-0200 I NETWORK  [HostnameCanonicalizationWorker] Starting hostname canonicalization worker
2016-01-07T18:15:20.973-0200 I FTDC     [initandlisten] Initializing full-time diagnostic data capture with directory 'C:/data/shard3/diagnostic.data'2016-01-07T18:15:21.152-0200 I NETWORK  [initandlisten] waiting for connections on port 27014
```

#### Conectando no router e adcionando shards
```js
mongo --port 27011 --host localhost
sh.addShard("localhost:27012");
sh.addShard("localhost:27013");
sh.addShard("localhost:27014");
sh.enableSharding("projeto");
sh.enableSharding("projeto");
sh.shardCollection("projeto.projects", {"_id":1});

mongos> sh.addShard("localhost:27012")
{ "shardAdded" : "shard0000", "ok" : 1 }
mongos> sh.addShard("localhost:27013")
{ "shardAdded" : "shard0001", "ok" : 1 }
mongos> sh.addShard("localhost:27014")
{ "shardAdded" : "shard0002", "ok" : 1 }
mongos> sh.enableSharding("projeto");
{ "ok" : 1 }
mongos> sh.shardCollection("projeto.projects", {"_id":1});
{ "collectionsharded" : "projeto.projects", "ok" : 1 }
mongos>
```
#### Adicionando registros.
```js
use projeto
for(var i =1; i<=10000; i++){
	var project = {
		name: rand.letter(5),
		description: rand.letter(10),
		date_begin: rand.date(),
		date_dream: rand.date(),
		date_end: rand.date(),
		expired: rand.date(),
		visible: true,
		realocate: false,
		visualizable_mod: 'all',
		tags:[],
		members: [],
		goals:[
			{
				name: rand.letter(4),
				description: rand.letter(10),
				date_begin: rand.date(),
				date_dream: rand.date(),
				date_end: rand.date(),
				date_relocate: rand.date(),
				expired: rand.date(),
				realocate: true,
				activities: [],
				tags:[]
			}
		]
	}
	db.projects.insert(project);
}
// Adcionei um pouquinho a mais hihi
db.projects.count()
540979
```
## Réplica
#### Levanta as réplicas e criando os diretórios respectivamente
```js
mkdir C:/data/rs1
mkdir C:/data/rs2
mkdir C:/data/rs3
mongod --replSet replica_set --port 27017 --dbpath c:/data/rs1
mongod --replSet replica_set --port 27018 --dbpath c:/data/rs2
mongod --replSet replica_set --port 27019 --dbpath c:/data/rs3

// Resultado apenas da primary 27017
2016-01-07T20:41:13.100-0200 I CONTROL  [main] Hotfix KB2731284 or later update is not installed, will zero-out data files
2016-01-07T20:41:13.102-0200 I CONTROL  [initandlisten] MongoDB starting : pid=10088 port=27017 dbpath=c:/data/rs1 64-bit host=paulo-PC
2016-01-07T20:41:13.102-0200 I CONTROL  [initandlisten] targetMinOS: Windows 7/Windows Server 2008 R2
2016-01-07T20:41:13.102-0200 I CONTROL  [initandlisten] db version v3.2.0
2016-01-07T20:41:13.103-0200 I CONTROL  [initandlisten] git version: 45d947729a0315accb6d4f15a6b06be6d9c19fe7
2016-01-07T20:41:13.103-0200 I CONTROL  [initandlisten] OpenSSL version: OpenSSL 1.0.1p-fips 9 Jul 2015
2016-01-07T20:41:13.104-0200 I CONTROL  [initandlisten] allocator: tcmalloc
2016-01-07T20:41:13.104-0200 I CONTROL  [initandlisten] modules: none
2016-01-07T20:41:13.105-0200 I CONTROL  [initandlisten] build environment:
2016-01-07T20:41:13.105-0200 I CONTROL  [initandlisten]     distmod: 2008plus-ssl
2016-01-07T20:41:13.105-0200 I CONTROL  [initandlisten]     distarch:x86_64
2016-01-07T20:41:13.106-0200 I CONTROL  [initandlisten]     target_arch: x86_64
2016-01-07T20:41:13.107-0200 I CONTROL  [initandlisten] options: { net: { port: 27017 }, replication: { replSet: "replica_set" }, storage: { dbPath: "c:/data/rs1" } }
2016-01-07T20:41:13.109-0200 I STORAGE  [initandlisten] wiredtiger_open config: create,cache_size=2G,session_max=20000,eviction=(threads_max=4),config_base=false,statistics=(fast),log=(enabled=true,archive=true,path=journal,compressor=snappy),file_manager=(close_idle_time=100000),checkpoint=(wait=60,log_size=2GB),statistics_log=(wait=0),direct_io=(data),
2016-01-07T20:41:14.103-0200 I REPL     [initandlisten] Did not find local voted for document at startup;  NoMatchingDocument Did not find replica set lastVote document in local.replset.election
2016-01-07T20:41:14.104-0200 I REPL     [initandlisten] Did not find local replica set configuration document at startup;  NoMatchingDocument Did not find replica set configuration document in local.system.replset
2016-01-07T20:41:14.108-0200 I NETWORK  [HostnameCanonicalizationWorker] Starting hostname canonicalization worker
2016-01-07T20:41:14.108-0200 I FTDC     [initandlisten] Initializing full-time diagnostic data capture with directory 'c:/data/rs1/diagnostic.data'
2016-01-07T20:41:14.375-0200 I NETWORK  [initandlisten] waiting for connections on port 27017
```
#### Conectando na réplica primaria e montando grupo de réplicas
```js
mongo --port 27017
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
rs.add("127.0.0.1:27018")
rs.add("127.0.0.1:27019")

2016-01-07T21:13:37.720-0200 I CONTROL  [main] Hotfix KB2731284 or later update is not installed, will zero-out data filesMongoDB shell version: 3.2.0
connecting to: 127.0.0.1:27017/test
> rsconf = {
...    _id: "replica_set",
...    members: [
...     {
...      _id: 0,
...      host: "127.0.0.1:27017"
...     }
...   ]
... }
{
        "_id" : "replica_set",
        "members" : [
                {
                        "_id" : 0,
                        "host" : "127.0.0.1:27017"
                }
        ]
}
> rs.initiate(rsconf)
{ "ok" : 1 }
replica_set:OTHER> rs.add("127.0.0.1:27018")
{ "ok" : 1 }
replica_set:PRIMARY> rs.add("127.0.0.1:27019")
{ "ok" : 1 }
```
