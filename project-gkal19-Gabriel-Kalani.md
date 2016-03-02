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
//Rand
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
    
//generateMembers
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
                                "date_begin" : ISODate("2006-05-04T18:04:49.831Z"),
                                "date_dream" : ISODate("2008-08-25T19:12:45.143Z"),
                                "date_end" : ISODate("2004-06-10T22:27:40.481Z"),
                                "date_relocate" : ISODate("1986-09-16T17:37:36.575Z"),
                                "expired" : ISODate("2009-02-15T17:32:32.494Z"),
                                "realocate" : false
                        }
                ],
                "date_begin" : ISODate("2001-11-28T12:41:49.483Z"),
                "date_dream" : ISODate("1985-03-24T05:13:37.837Z"),
                "date_end" : ISODate("2010-10-21T17:22:30.037Z"),
                "expired" : ISODate("2014-04-10T08:10:10.988Z"),
                "visible" : true,
                "members" : [
                        {
                                "user_id" : "56d7278dc5b14d2a55bb2ff4",
                                "type_member" : "aluno",
                                "notify" : false
                        },
                        {
                                "user_id" : "56d7278dc5b14d2a55bb2ff5",
                                "type_member" : "aluno",
                                "notify" : true
                        },
                        {
                                "user_id" : "56d7278dc5b14d2a55bb2ff7",
                                "type_member" : "aluno",
                                "notify" : false
                        },
                        {
                                "user_id" : "56d7278dc5b14d2a55bb2ff6",
                                "type_member" : "aluno",
                                "notify" : false
                        },
                        {
                                "user_id" : "56d7278dc5b14d2a55bb2ff3",
                                "type_member" : "aluno",
                                "notify" : false
                        }
                ],
                "realocate" : false,
                "visualizable_mod" : "all"
        },
        {
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
                                "date_begin" : ISODate("1982-05-30T12:37:15.734Z"),
                                "date_dream" : ISODate("1995-04-30T15:28:16.393Z"),
                                "date_end" : ISODate("2010-05-12T23:56:31.725Z"),
                                "date_relocate" : ISODate("1989-11-18T09:39:55.370Z"),
                                "expired" : ISODate("1999-05-16T07:24:02.471Z"),
                                "realocate" : false
                        }
                ],
                "date_begin" : ISODate("1988-01-24T07:39:08.585Z"),
                "date_dream" : ISODate("2015-06-12T11:15:46.415Z"),
                "date_end" : ISODate("2011-03-03T12:17:42.894Z"),
                "expired" : ISODate("2013-08-15T18:37:10.905Z"),
                "visible" : true,
                "members" : [
                        {
                                "user_id" : "56d7278dc5b14d2a55bb2ff6",
                                "type_member" : "aluno",
                                "notify" : true
                        },
                        {
                                "user_id" : "56d7278dc5b14d2a55bb2ff7",
                                "type_member" : "aluno",
                                "notify" : false
                        },
                        {
                                "user_id" : "56d7278dc5b14d2a55bb2ff4",
                                "type_member" : "aluno",
                                "notify" : true
                        },
                        {
                                "user_id" : "56d7278dc5b14d2a55bb2ff5",
                                "type_member" : "aluno",
                                "notify" : true
                        },
                        {
                                "user_id" : "56d7278dc5b14d2a55bb2ff8",
                                "type_member" : "aluno",
                                "notify" : true
                        }
                ],
                "realocate" : false,
                "visualizable_mod" : "all"
        },
        {
                "name" : "Patreon",
                "description" : "Sistema igual ao Patreon porém apenas os professores da Webschool.io poderão usar.",
                "tags" : [
                        "teacher",
                        "patreon",
                        "help",
                        "MEAN"
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
                                "date_begin" : ISODate("1989-07-28T04:07:17.151Z"),
                                "date_dream" : ISODate("2007-01-21T18:07:10.765Z"),
                                "date_end" : ISODate("1986-10-12T22:06:06.144Z"),
                                "date_relocate" : ISODate("1987-03-22T03:52:34.085Z"),
                                "expired" : ISODate("2003-06-14T12:31:35.751Z"),
                                "realocate" : false
                        }
                ],
                "date_begin" : ISODate("2013-11-11T03:12:03.612Z"),
                "date_dream" : ISODate("1993-08-30T16:11:38.498Z"),
                "date_end" : ISODate("2015-11-27T15:43:59.920Z"),
                "expired" : ISODate("1989-03-12T22:36:32.785Z"),
                "visible" : true,
                "members" : [
                        {
                                "user_id" : "56d7278dc5b14d2a55bb2ff5",
                                "type_member" : "aluno",
                                "notify" : true
                        },
                        {
                                "user_id" : "56d7278dc5b14d2a55bb2ff6",
                                "type_member" : "aluno",
                                "notify" : false
                        },
                        {
                                "user_id" : "56d7278dc5b14d2a55bb2ff3",
                                "type_member" : "aluno",
                                "notify" : true
                        },
                        {
                                "user_id" : "56d7278dc5b14d2a55bb2ff2",
                                "type_member" : "aluno",
                                "notify" : false
                        },
                        {
                                "user_id" : "56d7278dc5b14d2a55bb2ff4",
                                "type_member" : "aluno",
                                "notify" : true
                        }
                ],
                "realocate" : false,
                "visualizable_mod" : "all"
        },
        {
                "name" : "Estante Virtual",
                "description" : "Quem sabe começar com os alunos de faculdade que desejam vender, trocar ou doar seus livros que ele não precisará mais, porém os calouros precisam.",
                "tags" : [
                        "leia",
                        "webschool",
                        "virtual",
                        "MEAN"
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
                                "date_begin" : ISODate("1997-11-10T15:03:30.170Z"),
                                "date_dream" : ISODate("1989-12-27T18:50:13.051Z"),
                                "date_end" : ISODate("1993-01-22T18:31:14.773Z"),
                                "date_relocate" : ISODate("1995-01-13T07:49:53.083Z"),
                                "expired" : ISODate("1994-06-14T05:37:08.865Z"),
                                "realocate" : false
                        }
                ],
                "date_begin" : ISODate("1999-12-10T14:13:37.723Z"),
                "date_dream" : ISODate("1997-09-21T18:37:55.378Z"),
                "date_end" : ISODate("1994-11-17T19:59:05.597Z"),
                "expired" : ISODate("1979-12-30T18:19:02.723Z"),
                "visible" : true,
                "members" : [
                        {
                                "user_id" : "56d7278dc5b14d2a55bb2ff9",
                                "type_member" : "aluno",
                                "notify" : false
                        },
                        {
                                "user_id" : "56d7278dc5b14d2a55bb2ff6",
                                "type_member" : "aluno",
                                "notify" : true
                        },
                        {
                                "user_id" : "56d7278dc5b14d2a55bb2ff7",
                                "type_member" : "aluno",
                                "notify" : true
                        },
                        {
                                "user_id" : "56d7278dc5b14d2a55bb2ff5",
                                "type_member" : "aluno",
                                "notify" : true
                        },
                        {
                                "user_id" : "56d7278dc5b14d2a55bb2ff8",
                                "type_member" : "aluno",
                                "notify" : false
                        }
                ],
                "realocate" : false,
                "visualizable_mod" : "all"
        },
        {
                "name" : "Gerenciador de Doações",
                "description" : "O sistema servirá para qualquer tipo de doações, utilizando o nosso meio de pagamento.",
                "tags" : [
                        "payment",
                        "donation",
                        "basic",
                        "help",
                        "MEAN"
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
                                "date_begin" : ISODate("2004-04-02T00:34:37.165Z"),
                                "date_dream" : ISODate("2001-02-15T10:11:07.616Z"),
                                "date_end" : ISODate("2009-12-22T14:55:27.645Z"),
                                "date_relocate" : ISODate("1997-09-21T04:48:53.034Z"),
                                "expired" : ISODate("2011-08-30T17:21:05.400Z"),
                                "realocate" : false
                        }
                ],
                "date_begin" : ISODate("1983-12-23T04:33:32.543Z"),
                "date_dream" : ISODate("1996-09-06T01:21:53.226Z"),
                "date_end" : ISODate("1982-07-08T01:25:52.851Z"),
                "expired" : ISODate("1994-01-01T10:41:44.234Z"),
                "visible" : true,
                "members" : [
                        {
                                "user_id" : "56d7278dc5b14d2a55bb2ff7",
                                "type_member" : "aluno",
                                "notify" : true
                        },
                        {
                                "user_id" : "56d7278dc5b14d2a55bb2ff9",
                                "type_member" : "aluno",
                                "notify" : true
                        },
                        {
                                "user_id" : "56d7278dc5b14d2a55bb2ff8",
                                "type_member" : "aluno",
                                "notify" : false
                        },
                        {
                                "user_id" : "56d7278dc5b14d2a55bb2ffa",
                                "type_member" : "aluno",
                                "notify" : true
                        },
                        {
                                "user_id" : "56d7278dc5b14d2a55bb2ffb",
                                "type_member" : "aluno",
                                "notify" : true
                        }
                ],
                "realocate" : false,
                "visualizable_mod" : "all"
        }
]

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
        "_id" : ObjectId("56d72f7cc5b14d2a55bb3018"),
        "members" : [
                {
                        "user_id" : "56d7278dc5b14d2a55bb2ff4",
                        "type_member" : "aluno",
                        "notify" : false
                },
                {
                        "user_id" : "56d7278dc5b14d2a55bb2ff5",
                        "type_member" : "aluno",
                        "notify" : true
                },
                {
                        "user_id" : "56d7278dc5b14d2a55bb2ff7",
                        "type_member" : "aluno",
                        "notify" : false
                },
                {
                        "user_id" : "56d7278dc5b14d2a55bb2ff6",
                        "type_member" : "aluno",
                        "notify" : false
                },
                {
                        "user_id" : "56d7278dc5b14d2a55bb2ff3",
                        "type_member" : "aluno",
                        "notify" : false
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
                "_id" : ObjectId("56d7278dc5b14d2a55bb2ff4"),
                "name" : "Douglas",
                "bio" : "Projeto Final - MongoDB",
                "avatar_path" : "imgs/avatares/bxaopl78.jpg",
                "background_path" : "imgs/bgs/mqoxi833.gif",
                "date_register" : ISODate("1998-07-19T22:00:50.230Z"),
                "auth" : {
                        "username" : "uvusd",
                        "email" : "Douglas@be-mean.com",
                        "password" : 5865245687309,
                        "hash_token" : "zeloq521yisqy236",
                        "online" : true,
                        "disabled" : false,
                        "last_access" : ISODate("1998-07-20T00:09:42.923Z")
                }
        },
        {
                "_id" : ObjectId("56d7278dc5b14d2a55bb2ff5"),
                "name" : "Ednilson",
                "bio" : "Projeto Final - MongoDB",
                "avatar_path" : "imgs/avatares/cwkxfz731.jpg",
                "background_path" : "imgs/bgs/wcswe475.gif",
                "date_register" : ISODate("2007-04-18T11:09:44.552Z"),
                "auth" : {
                        "username" : "tdwxs",
                        "email" : "Ednilson@be-mean.com",
                        "password" : 2361156207043,
                        "hash_token" : "ntklb965ibfae913",
                        "online" : true,
                        "disabled" : false,
                        "last_access" : ISODate("2007-04-18T13:40:56.986Z")
                }
        },
        {
                "_id" : ObjectId("56d7278dc5b14d2a55bb2ff7"),
                "name" : "Suissa",
                "bio" : "Projeto Final - MongoDB",
                "avatar_path" : "imgs/avatares/kwuubb897.jpg",
                "background_path" : "imgs/bgs/apxub82.gif",
                "date_register" : ISODate("1985-03-09T01:48:26.520Z"),
                "auth" : {
                        "username" : "onome",
                        "email" : "Suissa@be-mean.com",
                        "password" : 8902459193486,
                        "hash_token" : "wrken101ttohc707",
                        "online" : true,
                        "disabled" : false,
                        "last_access" : ISODate("1985-03-09T03:25:01.341Z")
                }
        },
        {
                "_id" : ObjectId("56d7278dc5b14d2a55bb2ff6"),
                "name" : "Jô Soares",
                "bio" : "Projeto Final - MongoDB",
                "avatar_path" : "imgs/avatares/ikxsev416.jpg",
                "background_path" : "imgs/bgs/lsmge821.gif",
                "date_register" : ISODate("2005-03-24T11:08:12.440Z"),
                "auth" : {
                        "username" : "qfxtc",
                        "email" : "Jô Soares@be-mean.com",
                        "password" : 8966049144509,
                        "hash_token" : "viscd10vhoxk924",
                        "online" : true,
                        "disabled" : false,
                        "last_access" : ISODate("2005-03-24T12:17:19.315Z")
                }
        },
        {
                "_id" : ObjectId("56d7278dc5b14d2a55bb2ff3"),
                "name" : "Gabriel",
                "bio" : "Projeto Final - MongoDB",
                "avatar_path" : "imgs/avatares/hqdtfs913.jpg",
                "background_path" : "imgs/bgs/tknqd270.gif",
                "date_register" : ISODate("1977-01-18T04:31:01.317Z"),
                "auth" : {
                        "username" : "ckjvf",
                        "email" : "Gabriel@be-mean.com",
                        "password" : 1094670130405,
                        "hash_token" : "hmzcm5xrjld493",
                        "online" : true,
                        "disabled" : false,
                        "last_access" : ISODate("1977-01-18T06:27:56.213Z")
                }
        }
]
        
```

#### Liste todos os projetos com a tag que você escolheu para os 3 projetos em comum.

```js

> db.projects.find({tags:{$in:['MEAN']}}).pretty()
{
        "_id" : ObjectId("56d72f7cc5b14d2a55bb3018"),
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
                        "date_begin" : ISODate("2006-05-04T18:04:49.831Z"),
                        "date_dream" : ISODate("2008-08-25T19:12:45.143Z"),
                        "date_end" : ISODate("2004-06-10T22:27:40.481Z"),
                        "date_relocate" : ISODate("1986-09-16T17:37:36.575Z"),
                        "expired" : ISODate("2009-02-15T17:32:32.494Z"),
                        "realocate" : false
                }
        ],
        "date_begin" : ISODate("2001-11-28T12:41:49.483Z"),
        "date_dream" : ISODate("1985-03-24T05:13:37.837Z"),
        "date_end" : ISODate("2010-10-21T17:22:30.037Z"),
        "expired" : ISODate("2014-04-10T08:10:10.988Z"),
        "visible" : true,
        "members" : [
                {
                        "user_id" : "56d7278dc5b14d2a55bb2ff4",
                        "type_member" : "aluno",
                        "notify" : false
                },
                {
                        "user_id" : "56d7278dc5b14d2a55bb2ff5",
                        "type_member" : "aluno",
                        "notify" : true
                },
                {
                        "user_id" : "56d7278dc5b14d2a55bb2ff7",
                        "type_member" : "aluno",
                        "notify" : false
                },
                {
                        "user_id" : "56d7278dc5b14d2a55bb2ff6",
                        "type_member" : "aluno",
                        "notify" : false
                },
                {
                        "user_id" : "56d7278dc5b14d2a55bb2ff3",
                        "type_member" : "aluno",
                        "notify" : false
                }
        ],
        "realocate" : false,
        "visualizable_mod" : "all"
}
{
        "_id" : ObjectId("56d72f7cc5b14d2a55bb301a"),
        "name" : "Patreon",
        "description" : "Sistema igual ao Patreon porém apenas os professores da Webschool.io poderão usar.",
        "tags" : [
                "teacher",
                "patreon",
                "help",
                "MEAN"
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
                        "date_begin" : ISODate("1989-07-28T04:07:17.151Z"),
                        "date_dream" : ISODate("2007-01-21T18:07:10.765Z"),
                        "date_end" : ISODate("1986-10-12T22:06:06.144Z"),
                        "date_relocate" : ISODate("1987-03-22T03:52:34.085Z"),
                        "expired" : ISODate("2003-06-14T12:31:35.751Z"),
                        "realocate" : false
                }
        ],
        "date_begin" : ISODate("2013-11-11T03:12:03.612Z"),
        "date_dream" : ISODate("1993-08-30T16:11:38.498Z"),
        "date_end" : ISODate("2015-11-27T15:43:59.920Z"),
        "expired" : ISODate("1989-03-12T22:36:32.785Z"),
        "visible" : true,
        "members" : [
                {
                        "user_id" : "56d7278dc5b14d2a55bb2ff5",
                        "type_member" : "aluno",
                        "notify" : true
                },
                {
                        "user_id" : "56d7278dc5b14d2a55bb2ff6",
                        "type_member" : "aluno",
                        "notify" : false
                },
                {
                        "user_id" : "56d7278dc5b14d2a55bb2ff3",
                        "type_member" : "aluno",
                        "notify" : true
                },
                {
                        "user_id" : "56d7278dc5b14d2a55bb2ff2",
                        "type_member" : "aluno",
                        "notify" : false
                },
                {
                        "user_id" : "56d7278dc5b14d2a55bb2ff4",
                        "type_member" : "aluno",
                        "notify" : true
                }
        ],
        "realocate" : false,
        "visualizable_mod" : "all"
}
{
        "_id" : ObjectId("56d72f7cc5b14d2a55bb301b"),
        "name" : "Estante Virtual",
        "description" : "Quem sabe começar com os alunos de faculdade que desejam vender, trocar ou doar seus livros que ele não precisará mais, porém os calouros precisam.",
        "tags" : [
                "leia",
                "webschool",
                "virtual",
                "MEAN"
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
                        "date_begin" : ISODate("1997-11-10T15:03:30.170Z"),
                        "date_dream" : ISODate("1989-12-27T18:50:13.051Z"),
                        "date_end" : ISODate("1993-01-22T18:31:14.773Z"),
                        "date_relocate" : ISODate("1995-01-13T07:49:53.083Z"),
                        "expired" : ISODate("1994-06-14T05:37:08.865Z"),
                        "realocate" : false
                }
        ],
        "date_begin" : ISODate("1999-12-10T14:13:37.723Z"),
        "date_dream" : ISODate("1997-09-21T18:37:55.378Z"),
        "date_end" : ISODate("1994-11-17T19:59:05.597Z"),
        "expired" : ISODate("1979-12-30T18:19:02.723Z"),
        "visible" : true,
        "members" : [
                {
                        "user_id" : "56d7278dc5b14d2a55bb2ff9",
                        "type_member" : "aluno",
                        "notify" : false
                },
                {
                        "user_id" : "56d7278dc5b14d2a55bb2ff6",
                        "type_member" : "aluno",
                        "notify" : true
                },
                {
                        "user_id" : "56d7278dc5b14d2a55bb2ff7",
                        "type_member" : "aluno",
                        "notify" : true
                },
                {
                        "user_id" : "56d7278dc5b14d2a55bb2ff5",
                        "type_member" : "aluno",
                        "notify" : true
                },
                {
                        "user_id" : "56d7278dc5b14d2a55bb2ff8",
                        "type_member" : "aluno",
                        "notify" : false
                }
        ],
        "realocate" : false,
        "visualizable_mod" : "all"
}
{
        "_id" : ObjectId("56d72f7cc5b14d2a55bb301c"),
        "name" : "Gerenciador de Doações",
        "description" : "O sistema servirá para qualquer tipo de doações, utilizando o nosso meio de pagamento.",
        "tags" : [
                "payment",
                "donation",
                "basic",
                "help",
                "MEAN"
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
                        "date_begin" : ISODate("2004-04-02T00:34:37.165Z"),
                        "date_dream" : ISODate("2001-02-15T10:11:07.616Z"),
                        "date_end" : ISODate("2009-12-22T14:55:27.645Z"),
                        "date_relocate" : ISODate("1997-09-21T04:48:53.034Z"),
                        "expired" : ISODate("2011-08-30T17:21:05.400Z"),
                        "realocate" : false
                }
        ],
        "date_begin" : ISODate("1983-12-23T04:33:32.543Z"),
        "date_dream" : ISODate("1996-09-06T01:21:53.226Z"),
        "date_end" : ISODate("1982-07-08T01:25:52.851Z"),
        "expired" : ISODate("1994-01-01T10:41:44.234Z"),
        "visible" : true,
        "members" : [
                {
                        "user_id" : "56d7278dc5b14d2a55bb2ff7",
                        "type_member" : "aluno",
                        "notify" : true
                },
                {
                        "user_id" : "56d7278dc5b14d2a55bb2ff9",
                        "type_member" : "aluno",
                        "notify" : true
                },
                {
                        "user_id" : "56d7278dc5b14d2a55bb2ff8",
                        "type_member" : "aluno",
                        "notify" : false
                },
                {
                        "user_id" : "56d7278dc5b14d2a55bb2ffa",
                        "type_member" : "aluno",
                        "notify" : true
                },
                {
                        "user_id" : "56d7278dc5b14d2a55bb2ffb",
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

	> db.projects.find({'tags':{$nin:['curso']}}).pretty()
{
        "_id" : ObjectId("56d72f7cc5b14d2a55bb3018"),
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
                        "date_begin" : ISODate("2006-05-04T18:04:49.831Z"),
                        "date_dream" : ISODate("2008-08-25T19:12:45.143Z"),
                        "date_end" : ISODate("2004-06-10T22:27:40.481Z"),
                        "date_relocate" : ISODate("1986-09-16T17:37:36.575Z"),
                        "expired" : ISODate("2009-02-15T17:32:32.494Z"),
                        "realocate" : false
                }
        ],
        "date_begin" : ISODate("2001-11-28T12:41:49.483Z"),
        "date_dream" : ISODate("1985-03-24T05:13:37.837Z"),
        "date_end" : ISODate("2010-10-21T17:22:30.037Z"),
        "expired" : ISODate("2014-04-10T08:10:10.988Z"),
        "visible" : true,
        "members" : [
                {
                        "user_id" : "56d7278dc5b14d2a55bb2ff4",
                        "type_member" : "aluno",
                        "notify" : false
                },
                {
                        "user_id" : "56d7278dc5b14d2a55bb2ff5",
                        "type_member" : "aluno",
                        "notify" : true
                },
                {
                        "user_id" : "56d7278dc5b14d2a55bb2ff7",
                        "type_member" : "aluno",
                        "notify" : false
                },
                {
                        "user_id" : "56d7278dc5b14d2a55bb2ff6",
                        "type_member" : "aluno",
                        "notify" : false
                },
                {
                        "user_id" : "56d7278dc5b14d2a55bb2ff3",
                        "type_member" : "aluno",
                        "notify" : false
                }
        ],
        "realocate" : false,
        "visualizable_mod" : "all"
}
{
        "_id" : ObjectId("56d72f7cc5b14d2a55bb3019"),
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
                        "date_begin" : ISODate("1982-05-30T12:37:15.734Z"),
                        "date_dream" : ISODate("1995-04-30T15:28:16.393Z"),
                        "date_end" : ISODate("2010-05-12T23:56:31.725Z"),
                        "date_relocate" : ISODate("1989-11-18T09:39:55.370Z"),
                        "expired" : ISODate("1999-05-16T07:24:02.471Z"),
                        "realocate" : false
                }
        ],
        "date_begin" : ISODate("1988-01-24T07:39:08.585Z"),
        "date_dream" : ISODate("2015-06-12T11:15:46.415Z"),
        "date_end" : ISODate("2011-03-03T12:17:42.894Z"),
        "expired" : ISODate("2013-08-15T18:37:10.905Z"),
        "visible" : true,
        "members" : [
                {
                        "user_id" : "56d7278dc5b14d2a55bb2ff6",
                        "type_member" : "aluno",
                        "notify" : true
                },
                {
                        "user_id" : "56d7278dc5b14d2a55bb2ff7",
                        "type_member" : "aluno",
                        "notify" : false
                },
                {
                        "user_id" : "56d7278dc5b14d2a55bb2ff4",
                        "type_member" : "aluno",
                        "notify" : true
                },
                {
                        "user_id" : "56d7278dc5b14d2a55bb2ff5",
                        "type_member" : "aluno",
                        "notify" : true
                },
                {
                        "user_id" : "56d7278dc5b14d2a55bb2ff8",
                        "type_member" : "aluno",
                        "notify" : true
                }
        ],
        "realocate" : false,
        "visualizable_mod" : "all"
}
{
        "_id" : ObjectId("56d72f7cc5b14d2a55bb301a"),
        "name" : "Patreon",
        "description" : "Sistema igual ao Patreon porém apenas os professores da Webschool.io poderão usar.",
        "tags" : [
                "teacher",
                "patreon",
                "help",
                "MEAN"
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
                        "date_begin" : ISODate("1989-07-28T04:07:17.151Z"),
                        "date_dream" : ISODate("2007-01-21T18:07:10.765Z"),
                        "date_end" : ISODate("1986-10-12T22:06:06.144Z"),
                        "date_relocate" : ISODate("1987-03-22T03:52:34.085Z"),
                        "expired" : ISODate("2003-06-14T12:31:35.751Z"),
                        "realocate" : false
                }
        ],
        "date_begin" : ISODate("2013-11-11T03:12:03.612Z"),
        "date_dream" : ISODate("1993-08-30T16:11:38.498Z"),
        "date_end" : ISODate("2015-11-27T15:43:59.920Z"),
        "expired" : ISODate("1989-03-12T22:36:32.785Z"),
        "visible" : true,
        "members" : [
                {
                        "user_id" : "56d7278dc5b14d2a55bb2ff5",
                        "type_member" : "aluno",
                        "notify" : true
                },
                {
                        "user_id" : "56d7278dc5b14d2a55bb2ff6",
                        "type_member" : "aluno",
                        "notify" : false
                },
                {
                        "user_id" : "56d7278dc5b14d2a55bb2ff3",
                        "type_member" : "aluno",
                        "notify" : true
                },
                {
                        "user_id" : "56d7278dc5b14d2a55bb2ff2",
                        "type_member" : "aluno",
                        "notify" : false
                },
                {
                        "user_id" : "56d7278dc5b14d2a55bb2ff4",
                        "type_member" : "aluno",
                        "notify" : true
                }
        ],
        "realocate" : false,
        "visualizable_mod" : "all"
}
{
        "_id" : ObjectId("56d72f7cc5b14d2a55bb301b"),
        "name" : "Estante Virtual",
        "description" : "Quem sabe começar com os alunos de faculdade que desejam vender, trocar ou doar seus livros que ele não precisará mais, porém os calouros precisam.",
        "tags" : [
                "leia",
                "webschool",
                "virtual",
                "MEAN"
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
                        "date_begin" : ISODate("1997-11-10T15:03:30.170Z"),
                        "date_dream" : ISODate("1989-12-27T18:50:13.051Z"),
                        "date_end" : ISODate("1993-01-22T18:31:14.773Z"),
                        "date_relocate" : ISODate("1995-01-13T07:49:53.083Z"),
                        "expired" : ISODate("1994-06-14T05:37:08.865Z"),
                        "realocate" : false
                }
        ],
        "date_begin" : ISODate("1999-12-10T14:13:37.723Z"),
        "date_dream" : ISODate("1997-09-21T18:37:55.378Z"),
        "date_end" : ISODate("1994-11-17T19:59:05.597Z"),
        "expired" : ISODate("1979-12-30T18:19:02.723Z"),
        "visible" : true,
        "members" : [
                {
                        "user_id" : "56d7278dc5b14d2a55bb2ff9",
                        "type_member" : "aluno",
                        "notify" : false
                },
                {
                        "user_id" : "56d7278dc5b14d2a55bb2ff6",
                        "type_member" : "aluno",
                        "notify" : true
                },
                {
                        "user_id" : "56d7278dc5b14d2a55bb2ff7",
                        "type_member" : "aluno",
                        "notify" : true
                },
                {
                        "user_id" : "56d7278dc5b14d2a55bb2ff5",
                        "type_member" : "aluno",
                        "notify" : true
                },
                {
                        "user_id" : "56d7278dc5b14d2a55bb2ff8",
                        "type_member" : "aluno",
                        "notify" : false
                }
        ],
        "realocate" : false,
        "visualizable_mod" : "all"
}
{
        "_id" : ObjectId("56d72f7cc5b14d2a55bb301c"),
        "name" : "Gerenciador de Doações",
        "description" : "O sistema servirá para qualquer tipo de doações, utilizando o nosso meio de pagamento.",
        "tags" : [
                "payment",
                "donation",
                "basic",
                "help",
                "MEAN"
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
                        "date_begin" : ISODate("2004-04-02T00:34:37.165Z"),
                        "date_dream" : ISODate("2001-02-15T10:11:07.616Z"),
                        "date_end" : ISODate("2009-12-22T14:55:27.645Z"),
                        "date_relocate" : ISODate("1997-09-21T04:48:53.034Z"),
                        "expired" : ISODate("2011-08-30T17:21:05.400Z"),
                        "realocate" : false
                }
        ],
        "date_begin" : ISODate("1983-12-23T04:33:32.543Z"),
        "date_dream" : ISODate("1996-09-06T01:21:53.226Z"),
        "date_end" : ISODate("1982-07-08T01:25:52.851Z"),
        "expired" : ISODate("1994-01-01T10:41:44.234Z"),
        "visible" : true,
        "members" : [
                {
                        "user_id" : "56d7278dc5b14d2a55bb2ff7",
                        "type_member" : "aluno",
                        "notify" : true
                },
                {
                        "user_id" : "56d7278dc5b14d2a55bb2ff9",
                        "type_member" : "aluno",
                        "notify" : true
                },
                {
                        "user_id" : "56d7278dc5b14d2a55bb2ff8",
                        "type_member" : "aluno",
                        "notify" : false
                },
                {
                        "user_id" : "56d7278dc5b14d2a55bb2ffa",
                        "type_member" : "aluno",
                        "notify" : true
                },
                {
                        "user_id" : "56d7278dc5b14d2a55bb2ffb",
                        "type_member" : "aluno",
                        "notify" : true
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
        "_id" : ObjectId("56d7278dc5b14d2a55bb2ff2"),
        "name" : "Jean",
        "bio" : "Projeto Final - MongoDB",
        "avatar_path" : "imgs/avatares/mbfyyb893.jpg",
        "background_path" : "imgs/bgs/byaou398.gif",
        "date_register" : ISODate("2012-06-16T15:43:27.302Z"),
        "auth" : {
                "username" : "ighkc",
                "email" : "Jean@be-mean.com",
                "password" : 9563597280066,
                "hash_token" : "cpjse860eoqjt719",
                "online" : true,
                "disabled" : false,
                "last_access" : ISODate("2012-06-16T17:34:01.533Z")
        }
}
{
        "_id" : ObjectId("56d7278dc5b14d2a55bb2ff8"),
        "name" : "Mateus",
        "bio" : "Projeto Final - MongoDB",
        "avatar_path" : "imgs/avatares/btuzta768.jpg",
        "background_path" : "imgs/bgs/otmui873.gif",
        "date_register" : ISODate("2004-08-17T03:27:47.076Z"),
        "auth" : {
                "username" : "lsorc",
                "email" : "Mateus@be-mean.com",
                "password" : 2702322800178,
                "hash_token" : "rkhsu459mgojv216",
                "online" : true,
                "disabled" : false,
                "last_access" : ISODate("2004-08-17T04:48:16.717Z")
        }
}
{
        "_id" : ObjectId("56d7278dc5b14d2a55bb2ff9"),
        "name" : "Emerson",
        "bio" : "Projeto Final - MongoDB",
        "avatar_path" : "imgs/avatares/iwbkgo173.jpg",
        "background_path" : "imgs/bgs/rsfwn377.gif",
        "date_register" : ISODate("1976-08-22T23:39:30.201Z"),
        "auth" : {
                "username" : "crnvu",
                "email" : "Emerson@be-mean.com",
                "password" : 5329252888914,
                "hash_token" : "eweke736sxhfy741",
                "online" : true,
                "disabled" : false,
                "last_access" : ISODate("1976-08-22T23:46:18.844Z")
        }
}
{
        "_id" : ObjectId("56d7278dc5b14d2a55bb2ffa"),
        "name" : "Marcos",
        "bio" : "Projeto Final - MongoDB",
        "avatar_path" : "imgs/avatares/rwafvd476.jpg",
        "background_path" : "imgs/bgs/dndxe65.gif",
        "date_register" : ISODate("2009-11-28T23:43:33.008Z"),
        "auth" : {
                "username" : "imwwk",
                "email" : "Marcos@be-mean.com",
                "password" : 2816404292825,
                "hash_token" : "rgcut740togeb111",
                "online" : true,
                "disabled" : false,
                "last_access" : ISODate("2009-11-29T01:17:38.624Z")
        }
}
{
        "_id" : ObjectId("56d7278dc5b14d2a55bb2ffb"),
        "name" : "Roberto",
        "bio" : "Projeto Final - MongoDB",
        "avatar_path" : "imgs/avatares/bhpzhw811.jpg",
        "background_path" : "imgs/bgs/extom165.gif",
        "date_register" : ISODate("1971-01-14T17:10:28.355Z"),
        "auth" : {
                "username" : "svsjx",
                "email" : "Roberto@be-mean.com",
                "password" : 3247531019151,
                "hash_token" : "aqsmj704bxgso161",
                "online" : true,
                "disabled" : false,
                "last_access" : ISODate("1971-01-14T17:49:39.181Z")
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
        "_id" : ObjectId("56d7366990fbb75678808659")
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
                "_id" : ObjectId("56d7278dc5b14d2a55bb2ff2")
        },
        {
                "_id" : ObjectId("56d7278dc5b14d2a55bb2ff3")
        }
]
gkal19-aula-bemean-2468380(mongod-2.6.11) db> users.forEach(function(user){
   id_users.push(user._id.valueOf());
   });
gkal19-aula-bemean-2468380(mongod-2.6.11) db> db.projects.remove({'members.user_id':{$all:id_users}});
WriteResult({ "nRemoved" : 1 })

})
  
```
#### Apague todos os projetos que possuam uma determinada tag em goal.
```js
  gkal19-aula-bemean-2468380(mongod-2.6.11) db> db.projects.remove({
   'goals.tags': {$nin:['computacao']}
   });
        WriteResult({ "nRemoved" : 3 })
})
```

## Gerenciamento de usuários
#### Crie um usuário com permissões APENAS de Leitura.

```js

  gkal19-aula-bemean-2468380(mongod-2.6.11) db> db.runCommand({
     createUser:"Gabriel",
     pwd:"123",
     roles:[{role:"read", db:"be-mean"}]
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
    roles:[{role:"readWrite", db:"be-mean"}]
    });
{
  "ok": 1
}
```

#### Adicionar o papel grantRolesToUser e revokeRole para o usuário com Escrita e Leitura.
Mudei de banco da dados a partir do `grantRolesToUser`.

```js

  db.runCommand({ createRole: "grantRolesToUser",
    privileges: [{resource: { db: "be-mean", collection: "" }, actions: ["grantRole"]}],
    roles: [],
    writeConcern: { w: "majority" , wtimeout: 5000 }
  });
  
  db.runCommand({ createRole: "revokeRole",
    privileges: [{resource: { db: "be-mean", collection: "" }, actions: ["revokeRole"]},],
    roles: [],
    writeConcern: { w: "majority" , wtimeout: 5000 }
  });
  
  db.runCommand({
    grantRolesToUser: 'Arroz',
    roles:[
        {role:"grantRolesToUser", db:"be-mean"},
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
        {role:"grantRolesToUser", db:"be-mean"},
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
         {user: 'Gabriel', db:'be-mean'},
         {user: 'Arroz', db:'be-mean'}
     ],
     showCredentials:true,
     showPrivileges:true
   });
{
        "users" : [
                {
                        "_id" : "be-mean.Gabriel",
                        "user" : "Gabriel",
                        "db" : "be-mean",
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
                        "_id" : "be-mean.Arroz",
                        "user" : "Arroz",
                        "db" : "be-mean",
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
