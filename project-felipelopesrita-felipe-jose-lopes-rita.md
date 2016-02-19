# MongoDb - Projeto Final
**Autor:** Felipe José Lopes Rita
**Data** 1455850109228

##1. Para qual sistema você usaria o MongoDB (diferente desse)?
Antigamente não cogitava a utilização de bancos NoSQL, por não conhecer e julgar que a curva de aprendizado seria lenta e não produtiva. Agora que conheço, posso dizer que fiquei fascinado com a simplicidade e praticidade que o banco proporciona. Sendo assim, eu usaria MongoDB em quase todas as minhas aplicações web, devido as suas vantagens que apontei antes. Obviamente, talvez fossem necessários outros bancos para casos mais específicos (grafos e tudo mais), mas então eu poderia utilizá-los em conjunto com o mongo, tendo uma aplicação com múltiplos bancos de dados, como o Suissa comentou que existiam, na primeira aula de mongo.

##2. Qual a modelagem da sua coleção de `users`?
```js
{
	name: String,
    bio: String,
    date_register: Date,
    avatar_path: String,
    background_path: String,
    auth: {
    	username: String,
        email: String,
        password: String,
        last_access: Date,
        status: String, //online, offline, disable, etc
        hash_token: String
    }
}
```

##3. Qual a modelagem da sua coleção de `projects`?
```js
{
	name: String,
    description: String,
    date_begin: Date,
    date_dream: Date,
    date_end: Date,
    visible: Boolean,
    realocate: Boolean,
    expired: Boolean
    visualizable_mod: Boolean,
    tags: [],
    members: [{
    	user_id: String,
        type_member: String,
        notify: String
    }]
    goals: [{
    	name: String,
        description: String,
        date_begin: Date,
        date_dream: Date,
        date_end: Date,
        realocate: Boolean,
        date_realocate: Date,
        expired: Boolean,
        tags: [],
        "activities" : [{
                "activity_id" : String
        }]
    }]
}
```

##4. Qual a modelagem da sua coleção retirada de `projects`?
```js
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
Retirei essa coleção de projects. O principal é que tendo em mente o tipo de busca realizada pelo sistema, as atividades não seriam buscadas em conjunto. Isso é, tendo em mente que a busca do usuário seria algo por projects e goals, faz sentido manter estes campos na mesma coleção, para que toda informação seja trazida de uma vez na busca. Porém, atividades provavelmente serão buscadas apenas após você ter informações sobre os projects e goals, então ela poderia ficar numa coleção separada.

##5. Create - cadastro
#####5.1 Cadastro de usuários
```js
var users = [];
var usernames = [ "Felipe", "Jorge", "Lucas", "Matheus", "Robson", "Adiana", "Bianca", "Luciana", "Bruna", "Heloise" ]

for( var i=0; i< 10; i++ ) {

	var name = usernames[i].toLowerCase();
	var x = {
    	name: usernames[i],
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
	users.push(x);
}
StarKiller(mongod-3.2.0) be-mean-projeto> db.users.insert(users)
Inserted 1 record(s) in 201ms
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

#####5.1 Cadastro de projetos
```js
/* Ids dos usuarios */
var resul = db.users.find({}, {_id:1})
var ids = [];
for(var i=0; i< 10;i++) { ids.push(resul[i]._id); }

/* Cadastrando as activities */
var activities = [
        {name: 'MEAN - MongoDB'},
        {name: 'MEAN - Express'},
        {name: 'MEAN - Angular'},
        {name: 'MEAN - NodeJS'},
        {name: 'PHP - MongoDB'},
        {name: 'PHP - Frameworks'},
        {name: 'Frontend - HTML5&CSS3'},
        {name: 'Editores de texto - Sublime Text'}
];
db.activity.insert(activities)
var resul = db.activity.find({}, {_id:1})
var idsActiv = [];
for(var i=0; i< activities.length;i++) { idsActiv.push(resul[i]._id); }

/* Jsons com os documentos a serem inseridos (projects) */
var insert = [
	{
        name: "MongoDB",
        description: "Projeto de estudo do MongoDB ",
        tags: [ 'noSQL', 'Be MEAN', 'Projeto final' ],
        members: [
        	{ user_id: ids[0], type_member: "normal", notify: null },
        	{ user_id: ids[1], type_member: "normal", notify: null },
        	{ user_id: ids[2], type_member: "normal", notify: null },
        	{ user_id: ids[3], type_member: "normal", notify: null },
        	{ user_id: ids[4], type_member: "normal", notify: null }
		],
        goals: [
        	{
            	name:"Aprendizado",
            	description: "Aprender na prática sobre bancos NoSQL",
                tags:['Aprendizado', 'Webschool', 'Youtube'],
                activities: [ idsActiv[0], idsActiv[1] ]
            }
        ]
    },
    {
        name: "Express",
        description: "Projeto de estudo do Express ",
        tags: [ 'Framework', 'Be MEAN', 'Javascript' ],
        members: [
        	{ user_id: ids[5], type_member: "normal", notify: null },
        	{ user_id: ids[6], type_member: "normal", notify: null },
        	{ user_id: ids[7], type_member: "normal", notify: null },
        	{ user_id: ids[8], type_member: "normal", notify: null },
        	{ user_id: ids[9], type_member: "normal", notify: null }
		],
        goals: [
        	{
            	name:"Aprendizado",
            	description: "Aprender na prática sobre Express",
                tags:['Aprendizado', 'Webschool', 'Youtube'],
                activities: [ idsActiv[2], idsActiv[3] ]
            }
        ]
    },
    {
        name: "Angular",
        description: "Projeto de estudo do Angular ",
        tags: [ 'angularJs', 'Be MEAN', 'CodeForLive' ],
        members: [
        	{ user_id: ids[0], type_member: "normal", notify: null },
        	{ user_id: ids[5], type_member: "normal", notify: null },
        	{ user_id: ids[1], type_member: "normal", notify: null },
        	{ user_id: ids[6], type_member: "normal", notify: null },
        	{ user_id: ids[2], type_member: "normal", notify: null }
		],
        goals: [
        	{
            	name:"Aprendizado",
            	description: "Aprender na prática sobre AngularJS",
                tags:['Aprendizado', 'Webschool', 'Youtube'],
                activities: [ idsActiv[4], idsActiv[5] ]
            }
        ]
    },
    {
        name: "NodeJS",
        description: "Projeto de estudo do NodeJS ",
        tags: [ 'JSnoComando', 'Server', 'CodeForLive' ],
        members: [
        	{ user_id: ids[0], type_member: "normal", notify: null },
        	{ user_id: ids[6], type_member: "normal", notify: null },
        	{ user_id: ids[2], type_member: "normal", notify: null },
        	{ user_id: ids[9], type_member: "normal", notify: null },
        	{ user_id: ids[4], type_member: "normal", notify: null }
		],
        goals: [
        	{
            	name:"Aprendizado",
            	description: "Aprender na prática sobre bancos NoSQL",
                tags:['Aprendizado', 'Webschool', 'Youtube'],
                activities: [ idsActiv[6], idsActiv[7] ]
            }
        ]
    },
    {
        name: "PHP",
        description: "Projeto de estudo do PHP ",
        tags: [ 'PHPé-de-pano', 'PHPica', 'PHPinga' ],
        members: [
        	{ user_id: ids[9], type_member: "normal", notify: null },
        	{ user_id: ids[1], type_member: "normal", notify: null },
        	{ user_id: ids[3], type_member: "normal", notify: null },
        	{ user_id: ids[7], type_member: "normal", notify: null },
        	{ user_id: ids[5], type_member: "normal", notify: null }
		],
        goals: [
        	{
            	name:"Aprendizado",
            	description: "Aprender na prática sobre PHP",
                tags:['Aprendizado', 'Webschool', 'Youtube'],
                activities: []
            }
        ]
    }
]

/* Insere os projects */
StarKiller(mongod-3.2.0) be-mean-projeto> db.project.insert(insert)
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

##6. Retrieve - busca
#####6.1 Usuários de um projeto
```js
function getInfoUsers( arr ){
	var info = [];
    arr.forEach(function(user){
    	info.push(db.users.findOne({_id: user.user_id}));
    });
    return info;
}
var query = { name: /express/i }
var members = getInfoUsers(db.project.findOne(query).members)
StarKiller(mongod-3.2.0) be-mean-projeto> members
[
  {
    "_id": ObjectId("56c47f5b5586eacfc2e6054a"),
    "name": "Adiana",
    "bio": null,
    "date_register": "2016-02-17",
    "avatar_path": null,
    "background_path": null,
    "auth": {
      "username": "adiana",
      "email": "adiana@gmail.com",
      "password": "qwe123",
      "last_access": "2016-02-17",
      "status": "offline"
    }
  },
  {
    "_id": ObjectId("56c47f5b5586eacfc2e6054b"),
    "name": "Bianca",
    "bio": null,
    "date_register": "2016-02-17",
    "avatar_path": null,
    "background_path": null,
    "auth": {
      "username": "bianca",
      "email": "bianca@gmail.com",
      "password": "qwe123",
      "last_access": "2016-02-17",
      "status": "offline"
    }
  },
  {
    "_id": ObjectId("56c47f5b5586eacfc2e6054c"),
    "name": "Luciana",
    "bio": null,
    "date_register": "2016-02-17",
    "avatar_path": null,
    "background_path": null,
    "auth": {
      "username": "luciana",
      "email": "luciana@gmail.com",
      "password": "qwe123",
      "last_access": "2016-02-17",
      "status": "offline"
    }
  },
  {
    "_id": ObjectId("56c47f5b5586eacfc2e6054d"),
    "name": "Bruna",
    "bio": null,
    "date_register": "2016-02-17",
    "avatar_path": null,
    "background_path": null,
    "auth": {
      "username": "bruna",
      "email": "bruna@gmail.com",
      "password": "qwe123",
      "last_access": "2016-02-17",
      "status": "offline"
    }
  },
  {
    "_id": ObjectId("56c47f5b5586eacfc2e6054e"),
    "name": "Heloise",
    "bio": null,
    "date_register": "2016-02-17",
    "avatar_path": null,
    "background_path": null,
    "auth": {
      "username": "heloise",
      "email": "heloise@gmail.com",
      "password": "qwe123",
      "last_access": "2016-02-17",
      "status": "offline"
    }
  }
]
```
#####6.2 Projeto com a tag presente em 3 projetos
```js
var query = { tags: { $in: [/Be Mean/i] } }
StarKiller(mongod-3.2.0) be-mean-projeto> db.project.find(query)
{
  "_id": ObjectId("56c49ad25ca97b410b38fbfc"),
  "name": "MongoDB",
  "description": "Projeto de estudo do MongoDB ",
  "tags": [
    "noSQL",
    "Be MEAN",
    "Projeto final"
  ],
  "members": [
    {
      "user_id": ObjectId("56c47f5b5586eacfc2e60545"),
      "type_member": "normal",
      "notify": null
    },
    {
      "user_id": ObjectId("56c47f5b5586eacfc2e60546"),
      "type_member": "normal",
      "notify": null
    },
    {
      "user_id": ObjectId("56c47f5b5586eacfc2e60547"),
      "type_member": "normal",
      "notify": null
    },
    {
      "user_id": ObjectId("56c47f5b5586eacfc2e60548"),
      "type_member": "normal",
      "notify": null
    },
    {
      "user_id": ObjectId("56c47f5b5586eacfc2e60549"),
      "type_member": "normal",
      "notify": null
    }
  ],
  "goals": [
    {
      "name": "Aprendizado",
      "description": "Aprender na prática sobre bancos NoSQL",
      "tags": [
        "Aprendizado",
        "Webschool",
        "Youtube"
      ],
      "activities": [
        ObjectId("56c49aaa5ca97b410b38fbf4"),
        ObjectId("56c49aaa5ca97b410b38fbf5")
      ]
    }
  ]
}
{
  "_id": ObjectId("56c49ad25ca97b410b38fbfd"),
  "name": "Express",
  "description": "Projeto de estudo do Express ",
  "tags": [
    "Framework",
    "Be MEAN",
    "Javascript"
  ],
  "members": [
    {
      "user_id": ObjectId("56c47f5b5586eacfc2e6054a"),
      "type_member": "normal",
      "notify": null
    },
    {
      "user_id": ObjectId("56c47f5b5586eacfc2e6054b"),
      "type_member": "normal",
      "notify": null
    },
    {
      "user_id": ObjectId("56c47f5b5586eacfc2e6054c"),
      "type_member": "normal",
      "notify": null
    },
    {
      "user_id": ObjectId("56c47f5b5586eacfc2e6054d"),
      "type_member": "normal",
      "notify": null
    },
    {
      "user_id": ObjectId("56c47f5b5586eacfc2e6054e"),
      "type_member": "normal",
      "notify": null
    }
  ],
  "goals": [
    {
      "name": "Aprendizado",
      "description": "Aprender na prática sobre Express",
      "tags": [
        "Aprendizado",
        "Webschool",
        "Youtube"
      ],
      "activities": [
        ObjectId("56c49aaa5ca97b410b38fbf6"),
        ObjectId("56c49aaa5ca97b410b38fbf7")
      ]
    }
  ]
}
{
  "_id": ObjectId("56c49ad25ca97b410b38fbfe"),
  "name": "Angular",
  "description": "Projeto de estudo do Angular ",
  "tags": [
    "angularJs",
    "Be MEAN",
    "CodeForLive"
  ],
  "members": [
    {
      "user_id": ObjectId("56c47f5b5586eacfc2e60545"),
      "type_member": "normal",
      "notify": null
    },
    {
      "user_id": ObjectId("56c47f5b5586eacfc2e6054a"),
      "type_member": "normal",
      "notify": null
    },
    {
      "user_id": ObjectId("56c47f5b5586eacfc2e60546"),
      "type_member": "normal",
      "notify": null
    },
    {
      "user_id": ObjectId("56c47f5b5586eacfc2e6054b"),
      "type_member": "normal",
      "notify": null
    },
    {
      "user_id": ObjectId("56c47f5b5586eacfc2e60547"),
      "type_member": "normal",
      "notify": null
    }
  ],
  "goals": [
    {
      "name": "Aprendizado",
      "description": "Aprender na prática sobre AngularJS",
      "tags": [
        "Aprendizado",
        "Webschool",
        "Youtube"
      ],
      "activities": [
        ObjectId("56c49aaa5ca97b410b38fbf8"),
        ObjectId("56c49aaa5ca97b410b38fbf9")
      ]
    }
  ]
}
Fetched 3 record(s) in 4ms
```
#####6.3 Lista nome das atividades
```js
function getNameActivity( obj ) { return db.activity.findOne( {_id: obj}, {name:1, _id:0} ) }
var resul = db.project.find({})
var cont = db.project.count()
var namesActivities = []
for( var i=0; i< cont;i++ ) {
	resul[i].goals.forEach(function(goal){
    	goal.activities.forEach(function(idEl){
            namesActivities.push( getNameActivity(idEl) );
        });
    });
}
StarKiller(mongod-3.2.0) be-mean-projeto> namesActivities
[
  {
    "name": "MEAN - MongoDB"
  },
  {
    "name": "MEAN - Express"
  },
  {
    "name": "MEAN - Angular"
  },
  {
    "name": "MEAN - NodeJS"
  },
  {
    "name": "PHP - MongoDB"
  },
  {
    "name": "PHP - Frameworks"
  },
  {
    "name": "Frontend - HTML5&CSS3"
  },
  {
    "name": "Editores de texto - Sublime Text"
  }
]
```
#####6.4  Lista de projetos que não possuam uma tag
```js
var query = { tags: { $not: {$in:[/'Be MEAN/i]} } }
StarKiller(mongod-3.2.0) be-mean-projeto> db.project.find(query, {name:1})
{
  "_id": ObjectId("56c49ad25ca97b410b38fbfc"),
  "name": "MongoDB"
}
{
  "_id": ObjectId("56c49ad25ca97b410b38fbfd"),
  "name": "Express"
}
{
  "_id": ObjectId("56c49ad25ca97b410b38fbfe"),
  "name": "Angular"
}
{
  "_id": ObjectId("56c49ad25ca97b410b38fbff"),
  "name": "NodeJS"
}
{
  "_id": ObjectId("56c49ad25ca97b410b38fc00"),
  "name": "PHP"
}
Fetched 5 record(s) in 14ms
```

#####6.5 Lista de usuários que não estão na primeira pesquisa
```js
var query = { _id: ObjectId("56c49ad25ca97b410b38fbfc") }
var members = [];
db.project.findOne(query).members.forEach(function(ObjId){
	members.push(ObjId.user_id)
})
db.users.find({ _id: { $not: { $in: members } } }, {name:1})
```
##7. Update - alteração
#####7.1 Adição de campo a todos os projetos
```js
var mod = { $set: { views: 0 } }
var opt = { multi: true }
db.project.update({}, mod, opt)
Updated 5 existing record(s) in 186ms
WriteResult({
  "nMatched": 5,
  "nUpserted": 0,
  "nModified": 5
})
```
#####7.2 Adicionar uma tag diferente para cada projeto
```js
var idProj = db.project.find({}, {_id: 1})
var cont = db.project.count()
for(var i=0; i< cont; i++) {
	var query = { _id: idProj[i]._id }
	var mod = { $push: { tags: 'Tag '+(i+1) } }
    db.project.update(query, mod)
}
Updated 1 existing record(s) in 41ms
Updated 1 existing record(s) in 1ms
Updated 1 existing record(s) in 1ms
Updated 1 existing record(s) in 1ms
Updated 1 existing record(s) in 0ms
WriteResult({
  "nMatched": 1,
  "nUpserted": 0,
  "nModified": 1
})
```
#####7.3 Adicionar dois usuarios diferentes a todos os projetos
```js

/* Ids dos usuarios */
var resul = db.users.find({}, {_id:1})
var ids = [];
for(var i=0; i< 10;i++) { ids.push(resul[i]._id); }

var idProj = db.project.find({}, {_id: 1})
var cont = db.project.count()

var modUsers = []
for(var i=0; i< 10; i++) {
	modUsers.push([ {user_id: ids[i]}, {user_id: ids[i+1]} ]);
}
for(var i=0; i< cont; i++) {
	var query = { _id: idProj[i]._id }
	var mod = { $pushAll: { members: modUsers[i] } }
    db.project.update(query, mod)
}
Updated 1 existing record(s) in 1ms
Updated 1 existing record(s) in 2ms
Updated 1 existing record(s) in 2ms
Updated 1 existing record(s) in 1ms
Updated 1 existing record(s) in 1ms
WriteResult({
  "nMatched": 1,
  "nUpserted": 0,
  "nModified": 1
})
```
#####7.4 Adicione um comentário em cada atividade (um sem)
```js
/* Ids dos usuarios */
var resul = db.users.find({}, {_id:1})
var ids = [];
for(var i=0; i< 10;i++) { ids.push(resul[i]._id); }

var idAct = db.activity.find({}, {_id: 1})
var cont = db.activity.count()
for(var i=0; i< cont-1; i++) {
	var query = { _id: idAct[i]._id }
	var mod = { $push: { comments: { user_id:ids[i], text: 'Comentário número '+(i+1) } } }
    db.activity.update(query, mod)
}
Updated 1 existing record(s) in 2ms
Updated 1 existing record(s) in 0ms
Updated 1 existing record(s) in 0ms
Updated 1 existing record(s) in 1ms
Updated 1 existing record(s) in 1ms
Updated 1 existing record(s) in 1ms
Updated 1 existing record(s) in 1ms
WriteResult({
  "nMatched": 1,
  "nUpserted": 0,
  "nModified": 1
})
```
#####7.5 Adicionar projeto com Upsert
```js
var query = {name: "Bootstrap+CSS3"}
var opt = {upsert: true}
mod = {
	$setOnInsert:{
        name: "Bootstrap+CSS3",
        description: "Projeto de estudo Frontend ",
        members: [
        	{ user_id: ObjectId("56c47f5b5586eacfc2e60545"), type_member: "normal", notify: null },
        	{ user_id: ObjectId("56c47f5b5586eacfc2e6054e"), type_member: "normal", notify: null },
		],
        goals: [
        	{
            	name:"Aprendizado",
            	description: "Aprender na prática sobre bancos FrontEnd",
                tags:['Aprendizado', 'Webschool', 'Youtube']
            }
        ]
    }
}
db.project.update(query, mod, opt)
Updated 1 new record(s) in 24ms
WriteResult({
  "nMatched": 0,
  "nUpserted": 1,
  "nModified": 0,
  "_id": ObjectId("56c4ec11c01f2ab6fced0f9e")
})
```

##8. Delete - remoção
#####8.1 Deletar projetos que não possuam tags
```js
db.project.remove({ tags : { $exists : 0 } })
Removed 1 record(s) in 3ms
WriteResult({
  "nRemoved": 1
})
```
#####8.2 Excluir projetos sem comentários nas atividades
```js
function removeProjct(objId){
  db.projects.remove({"goals.activities.activity_id":  objId._id});
}
db.activity.find({'comments.0': {$exists: false}}).forEach(removeProjct)
```
#####8.3 Excluir projetos sem atividades
```js
var query = { 'goals.activities': [ ] }
db.project.remove(query)
Removed 1 record(s) in 34ms
WriteResult({
  "nRemoved": 1
})
```
#####8.4 Apagar projetos que dois usuários fazem parte
```js
var users = [
	{"_id": ObjectId("56c47f5b5586eacfc2e60545")},
	{"_id": ObjectId("56c47f5b5586eacfc2e60546")}
]
users.forEach(function(x){ db.project.remove({'members.user_id': x._id}) })
Removed 3 record(s) in 2ms
Removed 1 record(s) in 2ms
```
#####8.5 Apagar projeto com uma tag em goal
```js
var query = { 'goals.tags': 'Youtube' }
db.project.remove(query)
```

##9. Sharding
```js
mkdir /data/configdb
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

##10. Replica
```js
mkdir /data/rs1
mkdir /data/rs2
mkdir /data/rs3
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

##11. Gerenciamento de usuários
##### Usuário com permissões APENAS de Leitura
```js
db.createUser(
    {
      user: "usuario-leitura",
      pwd: "12345678",
      roles: [ "read" ]
    }
)
```
#####Crie um usuário com permissões de Escrita e Leitura.
```js
db.createUser(
    {
      user: "usuario-leitura-escrita",
      pwd: "12345678",
      roles: [ "readWrite" ]
    }
)
```
#####Adicionar o papel grantRolesToUser e revokeRole para o usuário com Escrita e Leitura
```js
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
```js
db.revokeRolesFromUser(
    "usuario-leitura-escrita",
    [ "grantRolesToUser" ]
)
```
#####Listar todos os usuários com seus papéis e ações.
```js
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


