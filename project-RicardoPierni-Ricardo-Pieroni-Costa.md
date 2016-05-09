# MongoDb - Projeto Final
**Autor:** Ricardo Pieroni Costa
**Data** :ISODate("2016-04-26T01:06:27.660Z")

## Para qual sistema você usaria o MogoDB (diferente desse)?
```
Usuaria em um sistema de gerenciamentos de questões. Onde se cadastraria questões de acordo com cada disciplina.
Posteriormente o usuario geraria provas sem se preucupar coom repetições de questões. 
Prova de um semestre ia ser diferente da prova do semetre passado.
```
## Qual a modelagem da sua coleção de `users`?
```
users{  
	name: String,  
	bio: String,  
	email: String,  

	auth: [{  
		username: String,  
		password: String,  
		last_access: Date,  
		online: boolean,  
		disable: boolean,  
		hash_token: String  
	}],  

	date_register: Date,  
	avatar_path: String  
}  
```
## Qual a modelagem da sua coleção de `projects`?
```
projects{  
  name: String,  
  description: String,  
  date_begin: Date,  
  date_dream: Date,  
  date_end: Date,  
  visible: boolean,  
  realocate: boolean,  
  expired: boolean,  
  visualizable_mod,  

  tags: [],  

  members: [{  
    name: users_name,  
    type_member: String,  
    notify: boolean  
  }],  

  goals: [{  
    name: String,  
    description: String,  
    date_begin: Date,  
    date_dream: Date,  
    date_end: Date,  
    realocate: boolean,  
    expired: booelan,  

    tags: []  
  }],  

  comments: [{  
    user_name: users.name,  
    text: String,  
    date_comment: String  
  }]  
}   
```
## Qual a modelagem da sua coleção retirada de `projects`?
```
activity: {
	name: String,
    description: String,
    date_begin: Date,
    date_dream: Date,
    date_end: Date,
    realocate: Boolean,
    date_realocate: Date,
    expired: Boolean,
    tags: [],
    members: [{
    	user_id: String,
        notify: String
    }],
    comments: [{
    	user_id: String, //member
    	text: String,
        date_comment: Date,
        files: [{
        	path: String,
            weight: String,
            name: String
        }]
    }]
}
```

####Retirei a coleção `activities` da coleção `projects` porque várias atividades  pode ter um projeto, e  a atividade(coleção) pode se crescer em larga escala, resolvi separar e não carregá-la junta a coleção projects.
 



## Create - cadastro
#### Usuario
```
var usuarios = [];
var nomes = [ "Ricardo", "MAriana", "Lucas", "Pedro", "Valdir", "Adiana", "Bianca", "Luciana", "Bruna", "Priscila" ]
for( var i=0; i< 10; i++ ) {

	var name = nomes[i].toLowerCase();
	var x = {
    	name: nomes[i],
        bio: null,
        date_register: "2016-02-17",
        avatar_path: null,
        background_path: null,
        auth: {
            username: name,
            email: name+"@gmail.com",
            password: "qwe123",
            last_access: "2016-02-17",
            status: "offline", //online, offline, disable, etc
        }
    }
	usuarios.push(x);
}

db.users.insert(usuarios)
```
#### Projetos 5
```
var res = db.users.find({}, {_id:1})
var idsUsers = [];
idsUsers
[ ]
 for(var i=0; i<10;i++){
... idsUsers[i] = res[i]._id;}
ObjectId("571e5152db56cd0859639a5f")

 idsUsers
[
        ObjectId("571e5152db56cd0859639a56"),
        ObjectId("571e5152db56cd0859639a57"),
        ObjectId("571e5152db56cd0859639a58"),
        ObjectId("571e5152db56cd0859639a59"),
        ObjectId("571e5152db56cd0859639a5a"),
        ObjectId("571e5152db56cd0859639a5b"),
        ObjectId("571e5152db56cd0859639a5c"),
        ObjectId("571e5152db56cd0859639a5d"),
        ObjectId("571e5152db56cd0859639a5e"),
        ObjectId("571e5152db56cd0859639a5f")
]

var atividades = [
        {name: 'Atividade - MongoDB'},
        {name: 'Atividade - Express'},
        {name: 'Atividade - Angular'},
        {name: 'Atividade - NodeJS'},
        {name: 'Atividade - HTML5'},
        {name: 'Atividade - NotePad++'}
];
db.activity.insert(atividades)
var resposta = db.activity.find({}, {_id:1})
var idsActividade = [];
for(var i=0; i< atividades.length;i++) { idsActividade.push(resposta[i]._id); }

var inserir = [
	{
        name: "Express",
        description: "Projeto de estudo do Epress ",
        tags: [ 'Framework', 'Be MEAN', 'Projeto final' ],
        members: [
        	{ user_id: idsUsers[0], type_member: "normal", notify: null },
        	{ user_id: idsUsers[1], type_member: "normal", notify: null },
        	{ user_id: idsUsers[2], type_member: "normal", notify: null },
        	{ user_id: idsUsers[3], type_member: "normal", notify: null },
        	{ user_id: idsUsers[4], type_member: "normal", notify: null }
		],
        goals: [
        	{
            	name:"Aprendizado",
            	description: "Aprender o que é top",
                tags:['Aprendizado', 'Webschool', 'Youtube'],
                activities: [ idsActividade[0], idsActividade[1] ]
            }
        ]
    },
    {
        name: "MongoDB",
        description: "Projeto estudando MongoDB ",
        tags: [ 'banco', 'Be MEAN', 'xxxx' ],
        members: [
        	{ user_id: idsUsers[5], type_member: "normal", notify: null },
        	{ user_id: idsUsers[6], type_member: "normal", notify: null },
        	{ user_id: idsUsers[7], type_member: "normal", notify: null },
        	{ user_id: idsUsers[8], type_member: "normal", notify: null },
        	{ user_id: idsUsers[9], type_member: "normal", notify: null }
		],
        goals: [
        	{
            	name:"Aprendizado",
            	description: "Aprender na china é top",
                tags:['Aprendizado', 'Webschool', 'Youtube'],
                activities: [ idsActividade[2], idsActividade[3] ]
            }
        ]
    },
    {
        name: "Angular",
        description: "Projeto de estudo do Angular ",
        tags: [ 'angularJs', 'Be MEAN', 'CodeForLive' ],
        members: [
        	{ user_id: idsUsers[0], type_member: "normal", notify: null },
        	{ user_id: idsUsers[5], type_member: "normal", notify: null },
        	{ user_id: idsUsers[1], type_member: "normal", notify: null },
        	{ user_id: idsUsers[6], type_member: "normal", notify: null },
        	{ user_id: idsUsers[2], type_member: "normal", notify: null }
		],
        goals: [
        	{
            	name:"Aprendizado",
            	description: "Aprender na prática sobre AngularJS",
                tags:['Aprendizado', 'Webschool', 'Youtube'],
                activities: [ idsActividade[4], idsActividade[5] ]
            }
        ]
    },
    {
        name: "NodeJS",
        description: " estudo do NodeJS ",
        tags: [ 'XXXX', 'Server', 'CodeForLive' ],
        members: [
        	{ user_id: idsUsers[0], type_member: "normal", notify: null },
        	{ user_id: idsUsers[1], type_member: "normal", notify: null },
        	{ user_id: idsUsers[1], type_member: "normal", notify: null },
        	{ user_id: idsUsers[9], type_member: "normal", notify: null },
        	{ user_id: idsUsers[4], type_member: "normal", notify: null }
		],
        goals: [
        	{
            	name:"Aprendizado",
            	description: "Aprender xxxxxx",
                tags:['Aprendizado', 'Webschool', 'Youtube'],
                activities: [ idsActividade[6], idsActividade[7] ]
            }
        ]
    },
    {
        name: "JavaEE",
        description: "Projeto de estudo do JavaEE ",
        tags: [ 'Java full', 'nice', 'nice' ],
        members: [
        	{ user_id: idsUsers[9], type_member: "normal", notify: null },
        	{ user_id: idsUsers[1], type_member: "normal", notify: null },
        	{ user_id: idsUsers[3], type_member: "normal", notify: null },
        	{ user_id: idsUsers[7], type_member: "normal", notify: null },
        	{ user_id: idsUsers[5], type_member: "normal", notify: null }
		],
        goals: [
        	{
            	name:"Aprendizado",
            	description: "Aprender java xxx",
                tags:['Aprendizado', 'Webschool', 'Youtube'],
                activities: []
            }
        ]
    }
]

db.project.insert(inserir)
Inserted 1 record(s) in 27ms
BulkWriteResult({
  "writeErrors": [ ],
  "writeConcernErrors": [ ],
  "nInserted": 5,
  "nUpserted": 0,
  "nMatched": 0,
  "nModified": 0,
  "nRemoved": 0,
  "upserted": [ ]
})
```
## Retrieve - busca
#### Listar as informações dos membros de um projeto  
```
function getInfoUsers( arr ){
... var info = [];
...     arr.forEach(function(user){
...     info.push(db.users.findOne({_id: user.user_id}));
...     });
...     return info;
... }
var members = getInfoUsers(db.project.findOne(query).members)
members
[
        {
                "_id" : ObjectId("571e5152db56cd0859639a56"),
                "name" : "Ricardo",
                "bio" : null,
                "date_register" : "2016-02-17",
                "avatar_path" : null,
                "background_path" : null,
                "auth" : {
                        "username" : "ricardo",
                        "email" : "ricardo@gmail.com",
                        "password" : "qwe123",
                        "last_access" : "2016-02-17",
                        "status" : "offline"
                }
        },
        {
                "_id" : ObjectId("571e5152db56cd0859639a57"),
                "name" : "MAriana",
                "bio" : null,
                "date_register" : "2016-02-17",
                "avatar_path" : null,
                "background_path" : null,
                "auth" : {
                        "username" : "mariana",
                        "email" : "mariana@gmail.com",
                        "password" : "qwe123",
                        "last_access" : "2016-02-17",
                        "status" : "offline"
                }
        },
        {
                "_id" : ObjectId("571e5152db56cd0859639a57"),
                "name" : "MAriana",
                "bio" : null,
                "date_register" : "2016-02-17",
                "avatar_path" : null,
                "background_path" : null,
                "auth" : {
                        "username" : "mariana",
                        "email" : "mariana@gmail.com",
                        "password" : "qwe123",
                        "last_access" : "2016-02-17",
                        "status" : "offline"
                }
        },
        {
                "_id" : ObjectId("571e5152db56cd0859639a5f"),
                "name" : "Priscila",
                "bio" : null,
                "date_register" : "2016-02-17",
                "avatar_path" : null,
                "background_path" : null,
                "auth" : {
                        "username" : "priscila",
                        "email" : "priscila@gmail.com",
                        "password" : "qwe123",
                        "last_access" : "2016-02-17",
                        "status" : "offline"
                }
        },
        {
                "_id" : ObjectId("571e5152db56cd0859639a5a"),
                "name" : "Valdir",
                "bio" : null,
                "date_register" : "2016-02-17",
                "avatar_path" : null,
                "background_path" : null,
                "auth" : {
                        "username" : "valdir",
                        "email" : "valdir@gmail.com",
                        "password" : "qwe123",
                        "last_access" : "2016-02-17",
                        "status" : "offline"
                }
        }
]
```
#####Liste todos os projetos com a tag que você escolheu 
```
db.project.find(query).pretty()
{
        "_id" : ObjectId("571e54fedb56cd0859639a6c"),
        "name" : "Express",
        "description" : "Projeto de estudo do Epress ",
        "tags" : [
                "Framework",
                "Be MEAN",
                "Projeto final"
        ],
        "members" : [
                {
                        "user_id" : undefined,
                        "type_member" : "normal",
                        "notify" : null
                },
                {
                        "user_id" : undefined,
                        "type_member" : "normal",
                        "notify" : null
                },
                {
                        "user_id" : undefined,
                        "type_member" : "normal",
                        "notify" : null
                },
                {
                        "user_id" : undefined,
                        "type_member" : "normal",
                        "notify" : null
                },
                {
                        "user_id" : undefined,
                        "type_member" : "normal",
                        "notify" : null
                }
        ],
        "goals" : [
                {
                        "name" : "Aprendizado",
                        "description" : "Aprender na prática sobre bancos NoSQL",
                        "tags" : [
                                "Aprendizado",
                                "Webschool",
                                "Youtube"
                        ],
                        "activities" : [
                                ObjectId("571e52dfdb56cd0859639a60"),
                                ObjectId("571e52dfdb56cd0859639a61")
                        ]
                }
        ]
}
{
        "_id" : ObjectId("571e5fb4db56cd0859639a7e"),
        "name" : "Express",
        "description" : "Projeto de estudo do Epress ",
        "tags" : [
                "Framework",
                "Be MEAN",
                "Projeto final"
        ],
        "members" : [
                {
                        "user_id" : ObjectId("571e5152db56cd0859639a56"),
                        "type_member" : "normal",
                        "notify" : null
                },
                {
                        "user_id" : ObjectId("571e5152db56cd0859639a57"),
                        "type_member" : "normal",
                        "notify" : null
                },
                {
                        "user_id" : ObjectId("571e5152db56cd0859639a58"),
                        "type_member" : "normal",
                        "notify" : null
                },
                {
                        "user_id" : ObjectId("571e5152db56cd0859639a59"),
                        "type_member" : "normal",
                        "notify" : null
                },
                {
                        "user_id" : ObjectId("571e5152db56cd0859639a5a"),
                        "type_member" : "normal",
                        "notify" : null
                }
        ],
        "goals" : [
                {
                        "name" : "Aprendizado",
                        "description" : "Aprender o que é top",
                        "tags" : [
                                "Aprendizado",
                                "Webschool",
                                "Youtube"
                        ],
                        "activities" : [
                                ObjectId("571e52dfdb56cd0859639a60"),
                                ObjectId("571e52dfdb56cd0859639a61")
                        ]
                }
        ]
}
{
        "_id" : ObjectId("571e60fcdb56cd0859639a83"),
        "name" : "Express",
        "description" : "Projeto de estudo do Epress ",
        "tags" : [
                "Framework",
                "Be MEAN",
                "Projeto final"
        ],
        "members" : [
                {
                        "user_id" : ObjectId("571e5152db56cd0859639a56"),
                        "type_member" : "normal",
                        "notify" : null
                },
                {
                        "user_id" : ObjectId("571e5152db56cd0859639a57"),
                        "type_member" : "normal",
                        "notify" : null
                },
                {
                        "user_id" : ObjectId("571e5152db56cd0859639a58"),
                        "type_member" : "normal",
                        "notify" : null
                },
                {
                        "user_id" : ObjectId("571e5152db56cd0859639a59"),
                        "type_member" : "normal",
                        "notify" : null
                },
                {
                        "user_id" : ObjectId("571e5152db56cd0859639a5a"),
                        "type_member" : "normal",
                        "notify" : null
                }
        ],
        "goals" : [
                {
                        "name" : "Aprendizado",
                        "description" : "Aprender o que é top",
                        "tags" : [
                                "Aprendizado",
                                "Webschool",
                                "Youtube"
                        ],
                        "activities" : [
                                ObjectId("571e52dfdb56cd0859639a60"),
                                ObjectId("571e52dfdb56cd0859639a61")
                        ]
                }
        ]
}
{
        "_id" : ObjectId("571e6305db56cd0859639a88"),
        "name" : "Express",
        "description" : "Projeto de estudo do Epress ",
        "tags" : [
                "Framework",
                "Be MEAN",
                "Projeto final"
        ],
        "members" : [
                {
                        "user_id" : ObjectId("571e5152db56cd0859639a56"),
                        "type_member" : "normal",
                        "notify" : null
                },
                {
                        "user_id" : ObjectId("571e5152db56cd0859639a57"),
                        "type_member" : "normal",
                        "notify" : null
                },
                {
                        "user_id" : ObjectId("571e5152db56cd0859639a57"),
                        "type_member" : "normal",
                        "notify" : null
                },
                {
                        "user_id" : ObjectId("571e5152db56cd0859639a5f"),
                        "type_member" : "normal",
                        "notify" : null
                },
                {
                        "user_id" : ObjectId("571e5152db56cd0859639a5a"),
                        "type_member" : "normal",
                        "notify" : null
                }
        ],
        "goals" : [
                {
                        "name" : "Aprendizado",
                        "description" : "Aprender o que é top",
                        "tags" : [
                                "Aprendizado",
                                "Webschool",
                                "Youtube"
                        ],
                        "activities" : [
                                ObjectId("571e52dfdb56cd0859639a60"),
                                ObjectId("571e52dfdb56cd0859639a61")
                        ]
                }
        ]
}
{
        "_id" : ObjectId("571e6430db56cd0859639a89"),
        "name" : "Express",
        "description" : "Projeto de estudo do Epress ",
        "tags" : [
                "Framework",
                "Be MEAN",
                "Projeto final"
        ],
        "members" : [
                {
                        "user_id" : ObjectId("571e5152db56cd0859639a56"),
                        "type_member" : "normal",
                        "notify" : ObjectId("571e5152db56cd0859639a56")
                },
                {
                        "user_id" : ObjectId("571e5152db56cd0859639a57"),
                        "type_member" : "normal",
                        "notify" : ObjectId("571e5152db56cd0859639a56")
                },
                {
                        "user_id" : ObjectId("571e5152db56cd0859639a57"),
                        "type_member" : "normal",
                        "notify" : null
                },
                {
                        "user_id" : ObjectId("571e5152db56cd0859639a5f"),
                        "type_member" : "normal",
                        "notify" : null
                },
                {
                        "user_id" : ObjectId("571e5152db56cd0859639a5a"),
                        "type_member" : "normal",
                        "notify" : null
                }
        ],
        "goals" : [
                {
                        "name" : "Aprendizado",
                        "description" : "Aprender o que é top",
                        "tags" : [
                                "Aprendizado",
                                "Webschool",
                                "Youtube"
                        ],
                        "activities" : [
                                ObjectId("571e52dfdb56cd0859639a60"),
                                ObjectId("571e52dfdb56cd0859639a61")
                        ]
                }
        ]
}
```
####Liste apenas os nomes das  atividades para todos os projetos
```
db.project.find({"goals.activities" : {$ne : [], $exists : true}},{"goals.activities.name":1}).pretty()
{
        "_id" : ObjectId("571e54fedb56cd0859639a6c"),
        "goals" : [
                {
                        "activities" : [ ]
                }
        ]
}
{
        "_id" : ObjectId("571e54fedb56cd0859639a6d"),
        "goals" : [
                {
                        "activities" : [ ]
                }
        ]
}
{
        "_id" : ObjectId("571e54fedb56cd0859639a6e"),
        "goals" : [
                {
                        "activities" : [ ]
                }
        ]
}
{
        "_id" : ObjectId("571e54fedb56cd0859639a6f"),
        "goals" : [
                {
                        "activities" : [ ]
                }
        ]
}
{
        "_id" : ObjectId("571e5fb4db56cd0859639a7e"),
        "goals" : [
                {
                        "activities" : [ ]
                }
        ]
}
{
        "_id" : ObjectId("571e5fb4db56cd0859639a7f"),
        "goals" : [
                {
                        "activities" : [ ]
                }
        ]
}
{
        "_id" : ObjectId("571e5fb4db56cd0859639a80"),
        "goals" : [
                {
                        "activities" : [ ]
                }
        ]
}
{
        "_id" : ObjectId("571e5fb4db56cd0859639a81"),
        "goals" : [
                {
                        "activities" : [ ]
                }
        ]
}
{
        "_id" : ObjectId("571e60fcdb56cd0859639a83"),
        "goals" : [
                {
                        "activities" : [ ]
                }
        ]
}
{
        "_id" : ObjectId("571e60fcdb56cd0859639a84"),
        "goals" : [
                {
                        "activities" : [ ]
                }
        ]
}
{
        "_id" : ObjectId("571e60fcdb56cd0859639a85"),
        "goals" : [
                {
                        "activities" : [ ]
                }
        ]
}
{
        "_id" : ObjectId("571e60fcdb56cd0859639a86"),
        "goals" : [
                {
                        "activities" : [ ]
                }
        ]
}
{
        "_id" : ObjectId("571e6305db56cd0859639a88"),
        "goals" : [
                {
                        "activities" : [ ]
                }
        ]
}
{
        "_id" : ObjectId("571e6430db56cd0859639a89"),
        "goals" : [
                {
                        "activities" : [ ]
                }
        ]
}
```
#### Liste todos os projetos que não possuam uma tag.
```
db.project.find({"tags" : {$size : 0}})
```
#### Liste todos os usuários que não fazem parte de um projeto cadastrado.
```
var project = db.project.findOne({_id:ObjectId("571e6430db56cd0859639a89")});
var members = [];
 
var getMembro = function (user) { var u = db.users.findOne({ _id: user.user_id }); members.push(u._id);  };
project.members.forEach(getMembro);
db.user.find({_id: { $not: { $in: users }}}).pretty()

{
        "_id" : ObjectId("571e5152db56cd0859639a58"),
        "name" : "Lucas",
        "bio" : null,
        "date_register" : "2016-02-17",
        "avatar_path" : null,
        "background_path" : null,
        "auth" : {
                "username" : "lucas",
                "email" : "lucas@gmail.com",
                "password" : "qwe123",
                "last_access" : "2016-02-17",
                "status" : "offline"
        }
}
{
        "_id" : ObjectId("571e5152db56cd0859639a59"),
        "name" : "Pedro",
        "bio" : null,
        "date_register" : "2016-02-17",
        "avatar_path" : null,
        "background_path" : null,
        "auth" : {
                "username" : "pedro",
                "email" : "pedro@gmail.com",
                "password" : "qwe123",
                "last_access" : "2016-02-17",
                "status" : "offline"
        }
}

{
        "_id" : ObjectId("571e5152db56cd0859639a5b"),
        "name" : "Adiana",
        "bio" : null,
        "date_register" : "2016-02-17",
        "avatar_path" : null,
        "background_path" : null,
        "auth" : {
                "username" : "adiana",
                "email" : "adiana@gmail.com",
                "password" : "qwe123",
                "last_access" : "2016-02-17",
                "status" : "offline"
        }
}
{
        "_id" : ObjectId("571e5152db56cd0859639a5c"),
        "name" : "Bianca",
        "bio" : null,
        "date_register" : "2016-02-17",
        "avatar_path" : null,
        "background_path" : null,
        "auth" : {
                "username" : "bianca",
                "email" : "bianca@gmail.com",
                "password" : "qwe123",
                "last_access" : "2016-02-17",
                "status" : "offline"
        }
}
{
        "_id" : ObjectId("571e5152db56cd0859639a5d"),
        "name" : "Luciana",
        "bio" : null,
        "date_register" : "2016-02-17",
        "avatar_path" : null,
        "background_path" : null,
        "auth" : {
                "username" : "luciana",
                "email" : "luciana@gmail.com",
                "password" : "qwe123",
                "last_access" : "2016-02-17",
                "status" : "offline"
        }
}
{
        "_id" : ObjectId("571e5152db56cd0859639a5e"),
        "name" : "Bruna",
        "bio" : null,
        "date_register" : "2016-02-17",
        "avatar_path" : null,
        "background_path" : null,
        "auth" : {
                "username" : "bruna",
                "email" : "bruna@gmail.com",
                "password" : "qwe123",
                "last_access" : "2016-02-17",
                "status" : "offline"
        }
}


```
## Update - alteração
##### Adicione para todos os projetos o campo views: 0
```
var mod = { $set: { views: 0 } }
var option = { multi: true }
db.project.update({}, mod, option)
WriteResult({ "nMatched" : 17, "nUpserted" : 0, "nModified" : 17 })
```
##### Adicione 1 tag diferente para cada projeto
```
 var aux = db.project.find({}, {_id: 1})
 var c = db.project.count()
 for(var i=0; i< c; i++) {
... var query = { _id: aux[i]._id }
... var mod = { $push: { tags: 'Tag '+(i+1) } }
...     db.project.update(query, mod)
... }
WriteResult({ "nMatched" : 1, "nUpserted" : 0, "nModified" : 1 })
```
##### Adicione 2 membros diferentes para cada projeto
```

var aux = db.users.find({}, {_id:1})
var ids = [];
for(var i=0; i< 10;i++) { ids.push(aux[i]._id); }

var idProj = db.project.find({}, {_id: 1})
var cont = db.project.count()

var mod = []
for(var i=0; i< 10; i++) {
	mod.push([ {user_id: ids[i]}, {user_id: ids[i+1]} ]);
}
for(var i=0; i< cont; i++) {
	var query = { _id: idProj[i]._id }
	var mod = { $pushAll: { members: mod[i] } }
    db.project.update(query, mod)
}
WriteResult({
  "nMatched": 17,
  "nUpserted": 0,
  "nModified": 17
})

```
##### Adicione 1 comentário em cada atividade, deixe apenas 1 projeto sem
```
var aux = db.users.find({}, {_id:1})
var ids = [];
for(var i=0; i< 10;i++) { ids.push(aux[i]._id); }
var idAct = db.activity.find({}, {_id: 1})
var cont = db.activity.count()
for(var i=0; i< cont-1; i++) {
	var query = { _id: idAct[i]._id }
	var mod = { $push: { comments: { user_id:ids[i], text: 'Comentário número '+(i+1) } } }
    db.activity.update(query, mod)
}
WriteResult({
  "nMatched": 1,
  "nUpserted": 0,
  "nModified": 1
})
```
##### Adicione 1 projeto inteiro com UPSERT
```
var query = {name: "Bootstrap+CSS3"}
var option = {upsert: true}
mod = {
	$setOnInsert:{
        name: "Inteligencia Artificial",
        description: "Projeto de estudo hardcore ",
        members: [
        	{ user_id: ObjectId("56c47f5b5586eacfc2e60545"), type_member: "normal", notify: null },
        	{ user_id: ObjectId("56c47f5b5586eacfc2e6054e"), type_member: "normal", notify: null },
		],
        goals: [
        	{
            	name:"Aprendizado",
            	description: "Aprender o que é bom pra tosse",
                tags:['Aprendizado', 'Webschool', 'Youtube']
            }
        ]
    }
}
db.project.update(query, mod, option)
Updated 1 new record(s) in 24ms
WriteResult({
  "nMatched": 0,
  "nUpserted": 1,
  "nModified": 0,
  "_id": ObjectId("56c4ec11c01f2ab6fced0f9e")
})
```
## Delete - remoção
##### Apague todos os projetos que não possuam tags
```
db.project.remove({ tags : { $exists : 0 } })
Removed 1 record(s) in 3ms
WriteResult({
  "nRemoved": 0
})
```
##### Apague todos os projetos que não possuam comentários nas atividades
```
function removeProjct(objId){
  db.projects.remove({"goals.activities.activity_id":  objId._id});
}
db.activity.find({'comments.0': {$exists: false}}).forEach(removeProjct)
```
##### Apague todos os projetos que não possuam atividades
```
var query = { 'goals.activities': [ ] }
db.project.remove(query)
Removed 1 record(s) in 34ms
WriteResult({
  "nRemoved": 1
})
```
##### Escolha 2 usuário e apague todos os projetos em que os 2 fazem parte
```
var users = [
	{"_id": ObjectId("571e5152db56cd0859639a56")},
	{"_id": ObjectId("571e5152db56cd0859639a57")}
]
users.forEach(function(x){ db.project.remove({'members.user_id': x._id}) })
```
##### Apague todos os projetos que possuam uma determinada tag em goal
```js
var query = { 'goals.tags': 'Webschool' }
db.project.remove(query)
```

## Gerenciamento de usuários
##### Crie um usuário com permissões **APENAS** de Leitura
```
db.createUser(
    {
      user: "usuario-leitura",
      pwd: "12345678",
      roles: [ "read" ]
    }
)
```
##### Crie um usuário com permissões de Escrita e Leitura
```
db.createUser(
    {
      user: "usuario-leitura-escrita",
      pwd: "12345678",
      roles: [ "readWrite" ]
    }
)
```
##### Adicionar o papel grantRolesToUser e revokeRole para o usuário com Escrita e Leitura
```
db.runCommand({ createRole: "grantRolesToUser",
	privileges: [
		{ resource: { db: "admin", collection: "" }, actions: [ "grantRole" ] },
	],
	roles: [ "readWrite" ]
})
db.runCommand({ createRole: "revokeRole",
	privileges: [
		{ resource: { db: "admin", collection: "" }, actions: [ "revokeRole" ] },
	],
	roles: [ "readWrite" ]
})
db.grantRolesToUser(
  "usuario-leitura-escrita",
  [ "grantRolesToUser", "revokeRole" ]
)
```
##### Remover o papel grantRolesToUser para o usuário com Escrita e Leitura
```
db.revokeRolesFromUser(
    "usuario-leitura-escrita",
    [ "grantRolesToUser" ]
)
```
##### Listar todos os usuários com seus papéis e ações
```
{
  "_id": "admin.usuario-leitura",
  "user": "usuario-leitura",
  "db": "admin",
  "credentials": {
    "SCRAM-SHA-1": {
      "iterationCount": 10000,
      "salt": "RfzpnUnGEvov5XNiTpmH0g==",
      "storedKey": "3oNzKPeJsMvSTRI9Vk055NhpWjY=",
      "serverKey": "TtiMIpe3yuQelqKONVEVh+fu1IE="
    }
  },
  "roles": [
    {
      "role": "read",
      "db": "admin"
    }
  ]
}
{
  "_id": "admin.usuario-leitura-escrita",
  "user": "usuario-leitura-escrita",
  "db": "admin",
  "credentials": {
    "SCRAM-SHA-1": {
      "iterationCount": 10000,
      "salt": "Y39UW7bjohUK1CSIeTlqsQ==",
      "storedKey": "43QIPScQi2G49NuLGk9ibfhiClc=",
      "serverKey": "OEDfwT4r58z5NWAEAmlC1XMMCqQ="
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
  ]
}
Fetched 2 record(s) in 4ms
```
## Sharding
```
mongod --configsvr --port 27010
mongos --configdb localhost:27010 --port 27011
mongod --port 27012 --dbpath /data/shard1
mongod --port 27013 --dbpath /data/shard2
mongod --port 27014 --dbpath /data/shard3
mongo --port 27011 --host localhost
mongos> sh.addShard("localhost:27012")
mongos> sh.addShard("localhost:27013")
mongos> sh.addShard("localhost:27014")
mongos> sh.enableSharding("be-mean-projeto")
mongos> sh.shardCollection("be-mean-projeto.project", {"_id" : 1})
mongos> sh.shardCollection("be-mean-projeto.users", {"_id" : 1})
mongos> sh.shardCollection("be-mean-projeto.activity", {"_id" : 1})
```
## Replica
```
mongod --replSet replica_set --port 27017 --dbpath /data/rs1
mongod --replSet replica_set --port 27018 --dbpath /data/rs2
mongod --replSet replica_set --port 27019 --dbpath /data/rs3
mongo --port 27017
mongo> rsconf =
{
  "_id": "replica_set",
  "members": [
    {
      "_id": 0,
      "host": "127.0.0.1:27017"
    }
  ]
}
rs.initiate(rsconf)
rs.add("127.0.0.1:27018")
rs.add("127.0.0.1:27019")
```