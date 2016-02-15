# MongoDb - Projeto Final
**Autor**: Victor Igor Gomes Martins
**Data**: 1453495523193

## Para qual sistema você usaria o MongoDB (diferente desse)?
![](http://www.gifbin.com/bin/122015/hannibal-boxer-is-watching-you.gif)

- Um sistema onde eu teria uma massa imensa de dados(Gigantesca).
- Onde os dados crescerá e precisaria dividi-los.
- Caso eu queira que os dados sejam baseados por localização.
- Não queira um DBA para meu projeto.
- Qualquer aplicação real time como chats e rede sociais.

Usuaria o noSQL em qualquer caso em que o relacional não daria conta, ou que ficasse com gambiarras. O MongoDB dependendo do que eu queira seria minha opção, ou senão outro banco noSQL.
## Qual a modelagem da sua coleção de `users`?
```json
users:{
  id: ObjectId,
  name: String,
  bio: String,
  date-register: Date,
  avatar-path: String,
  background-path: String,

  auth:[{
    username: String,
    email: String,
    password: String,
    last-acess: Date,
    online: boolean,
    disabled: boolean,
    hash-token: String
  }]
}
```
## Qual a modelagem da sua coleção de `projects`?

```json
project:{
  name: String,
  description: String,
  date_begin: Date,
  date_dream: Date,
  date_end: Date,
  visible: boolean,
  realocate: boolean,
  expired: boolean,
  visualizable_mod: String,

  project_tag:[{}],

  members:[{
    type_member: [{ }],
    user_id: ObjectId,
    notify: boolean
  }],

  goals:[{
    name: String,
    description: String,
    date_begin: Date,
    date_dream: Date,
    date_end: Date,
    realocate: boolean,
    expired: boolean,
    history: [],
    goals_tags:[],

    activity:[{
      id_activity: ObjectId
    }]
  }]
}
```
## Qual a modelagem da sua coleção retirada de `projects`?
```json
activity:{
  id: ObjectId,
  name: String,
  description: String,
  date_begin:Date,
  date_dream:Date,
  date_end:Date,
  realocate: boolean,
  expired: ObjectId,

  tags: [{ }],

  members-activity:[{
    id_user: ObjectId,
    id_project: ObjectId,

    type_member: [{ }]
  }],

  activity-historic: [{
    date_realocate: Date
  }],

  comment:[{
    text: String,
    date_comment: Date,

    file: [{
      path: String,
      weight: int,
      name: String
    }],

    members-comment:[{
      id_user: ObjectId,
      id_project: ObjectId,
      notify: boolean,
      type_member: [{}]
    }]
  }]
}
```
### O porquê dessa modelagem ?

![](http://www.gifbin.com/bin/florida.gif)

Primeiro de tudo, separei o `user` em uma nova collection, assim deixando o banco mais semântico, nele contém apenas campos que tem a ver com usuários, e na collection `project`, tive que separar o campo `activities` e criando uma nova, pois um **project** vai haver muitos objetivos(goals) e cada objetivo vai ter muito mais atividades(activity), separando em uma nova collection, melhora ações e deixa mais conciso, pois não precisa carregar dados desnecessários ao executar os `goals`.

## Create - cadastro
###Cadastre 10 usuários diferentes.

```js
var users_values = {
  name: ["Victor Igor", "José Carlos", "Joao Messias", "Andy Self", "Archer Gly",
         "Arlie Ferreira", "Angela Matos", "Andrew Josias", "Lucas Santos", "Luan Lima"],

  username:["vitus", "jose123", "juau","joaome", "andy321", "gly",
        "Arlie2016", "angel", "dudu","luquinha", "lulu"]
};
  var fill_users = new Array();
  var hash = function (){return Math.random().toString(36).substr(2)}
  for (var i = 0; i < users_values.name.length; i++){
    var user = {
      name:users_values.name[i],
      bio :"place your bio here",
      dateregister: Date.now(),
      avatarpath: "place your avatar here",
      backgroundpath: "place your avatar here",
      auth:{
        username: users_values.username[i],
        email:    users_values.username[i]+"@hotmail.com",
        password: users_values.username[i]+Math.random().toString(36),
        online:   true,
        disabled: false,
        hashtoken: hash()
      }
    }
    fill_users.push(user);
  }
  db.users.insert(fill_users);
```

###Cadastre 5 projetos diferentes.

```js
var project_values = {
 description:[
 "Save all your notes in a file and manipulate.",
 "A clean and simple notification plugin (alert/growl style) for javascript, with no dependencies.",
 "A calculator with nice designer-functional",
 "name, number and search in phonebook",
 "A node module to gen, validate and format Brazilian documents' numbers (aka CPF/CNPJ)."
 ],
 tags:[["javascript","notes","tree"],["growl", "plugin", "grunt"],["calculator","functional","javascript"],["phonebook","nodejs","mathematic"],["bradoc","grunt","javascript"]],
 goalsName:["speed", "style beautiful", "calculator in browser", "study js", "study nodejs"],
 goalsDescription: ["it's better","how to be a good js developer","learn programming functional","use tree-redblack","learning nodejs"],
 goalsTags: [["nodejs", "javascript", "functional"], ["linux","webschool","bemean"], ["calculator","mathematic","calculus"], ["search","phonebook","number"], ["module","brazilian","cpf"]]
}
/****** DATE CONFIG *********/
function endproject(d){
  var someDate = new Date();
  someDate.setDate(someDate.getDate() + d);
  return someDate.getDate() + '/'+((someDate.getMonth()+1) < 10 ? ("0"+(someDate.getMonth()+1)):
            ((someDate.getMonth()+1))) + '/'+someDate.getFullYear()
}
function atual_dt(){
  var dt = new Date();
  return dt.getDate() + '/'+((dt.getMonth()+1) < 10 ? ("0"+(dt.getMonth()+1)):
      ((dt.getMonth()+1))) + '/'+dt.getFullYear()
}
/*******END DATE CONFIG ******/

/*******CREATE ACTIVITY*******/
(function(){
  var fill_activity = new Array();
  for (var i = 0; i < 5; i++){
    var activity = {
      name: "activity "+(i+1),
      description: "description "+(i+1),
      datebegin: atual_dt(),
      datedream: atual_dt(),
      dateend: endproject(Math.floor(Math.random() * (30 - 7) + 7)),
      realocate: false,
      expired: false,
      tags: project_values.tags[i],
      membersActivity: [],
      historic: [],
      comment: []
    }
    fill_activity.push(activity);
  }
  db.activity.insert(fill_activity);
})();
/*******END ACTIVITY*********/
(function(){
  var alltypes = ["admin", "developer", "designer", "analyst", "freelancer"]; /* type members*/
  var id_users = db.users.find({},{id: 1}).toArray(); /* get id of users collection */
  var id_activity = db.activity.find({}, {id:1}).toArray();
  var fill_project = new Array();
  for (var i = 0; i < project_values.description.length; i++){
    var project = {
      name: "project "+(i+1),
      description: project_values.description[i],
      datebegin: atual_dt(),
      datedream: atual_dt(),
      dateend: endproject(Math.floor(Math.random() * (30 - 7) + 7)),
      visible: true,
      realocate: false,
      expired: false,
      visualizablemod: null,
      projecttag: project_values.tags[i],
      members: [{
        type_member: alltypes[i],
        user_id: id_users[i],
        notify: true
      }],
      goals: [{
        name: project_values.goalsName[i],
        description: project_values.goalsDescription[i],
        datebegin: atual_dt(),
        datedream: atual_dt(),
        dateend: endproject(Math.floor(Math.random() * (30 - 7) + 7)),
        realocate: false,
        expired: false,
        history: null,
        goalstag: project_values.goalsTags[i],
        activity:{
          activityID: (i < 4 ? id_activity[i]: null),
          activityID2:(i < 4 ? id_activity[i+1]: null)
        }
      }]
    }
    fill_project.push(project)
  }
  db.project.insert(fill_project);
})();
Inserted 1 record(s) in 4ms

```

## Retrieve - busca

### Liste as informações dos membros de 1 projeto específico que deve ser buscado pelo seu nome de forma a não ligar para maiúsculas e minúsculas.

```js
var query = {name: /victor igor/i}
db.users.find(query)
{
 "_id": ObjectId("569e90075cbee4e1d4a750f7"),
 "name": "Victor Igor",
 "bio": "place your bio here",
 "dateregister": 1453232130811,
 "avatarpath": "place your avatar here",
 "backgroundpath": "place your avatar here",
 "auth": {
   "username": "vitus",
   "email": "vitus@hotmail.com",
   "password": "vitus0.hm6n90qkku9b2o6r",
   "online": true,
   "disabled": false,
   "hashtoken": "rp5dmldxun64j9k9"
 }
```

### Liste todos os projetos com a tag que você escolheu para os 3 projetos em comum.

```js
var query = {projecttag: {$in: [/javascript/i]}}
db.project.find(query, {name: 1, _id: 0})
{
  "name": "project 1"
}
{
  "name": "project 3"
}
{
  "name": "project 5"
}
Fetched 3 record(s) in 1ms
```

###Liste apenas os nomes de todas as atividades para todos os projetos.

```js
db.activity.find({}, {name: 1, _id: 0})
{
  "name": "activity 1"
}
{
  "name": "activity 2"
}
{
  "name": "activity 3"
}
{
  "name": "activity 4"
}
{
  "name": "activity 5"
}
```

###Liste todos os projetos que não possuam uma tag.

```js
db.project.find({projecttag: {$size: 0}})
Fetched 0 record(s) in 0ms
```

###Liste todos os usuários que não fazem parte do primeiro projeto cadastrado.

```js
var firstProject = db.project.findOne();
var members = [];

firstProject.members.forEach(MembersNot);

var MembersNot = function (curr) {
    var user = db.users.findOne({ _id: curr.user_id });
    members.push(user._id);
};

db.users.find({ _id: { $not: { $in: members } } }, { name: 1, _id: 0 }) ///<-- todos que não tem esse id

{"name": "José Carlos"}
{"name": "Joao Messias"}
{"name": "Andy Self"}
{"name": "Archer Gly"}
{"name": "Arlie Ferreira"}
{"name": "Angela Matos"}
{"name": "Andrew Josias"}
{"name": "Lucas Santos"}
{"name": "Luan Lima"}


Fetched 4 record(s) in 5ms

```

## Update - alteração

###Adicione para todos os projetos o campo views: 0.

```js
var query = {}
var mod  = {$set:{views: 0}}
var options = {multi: true}
db.project.update(query, mod, options)
Updated 5 existing record(s) in 69ms
WriteResult({
  "nMatched": 5,
  "nUpserted": 0,
  "nModified": 5
})
```
###Adicione 1 tag diferente para cada projeto.

```js
var tags = ["lua", "emac", "shell", "livescript", "closure"]
for (var i = 0; i < 5; i++){
  var getId = db.project.find().skip(i).limit(1).toArray()
  var query = {_id: getId[0]._id}
  var mod   = {$push: {projecttag: tags[i]}}
  db.project.update(query, mod)
}
Updated 1 existing record(s) in 151ms
Updated 1 existing record(s) in 1ms
Updated 1 existing record(s) in 1ms
Updated 1 existing record(s) in 1ms
Updated 1 existing record(s) in 2ms
WriteResult({
  "nMatched": 1,
  "nUpserted": 0,
  "nModified": 1
})

```
###Adicione 2 membros diferentes para cada projeto.

```js

var id_users = db.users.find({},{id: 1}).toArray(); /* get id of users collection */
var alltypes = ["admin", "developer", "designer", "analyst", "freelancer"];/* type members*/
for (var i = 0; i < 5; i++){ //<-- 5 project
   var newMembers = [{  ///<--- agora é um array pra ser inserido depois
    type_member: alltypes[i],
    user_id: id_users[i],
    notify: true
  },
  {
  type_member: alltypes[Math.floor(Math.random() * (4))],
  user_id: id_users[i+1],
    notify: false
  }];
  var getId = db.project.find().skip(i).limit(1).toArray();
  var query = {_id: getId[0]._id}
  var mod  = {$pushAll: {members: newMembers}} //<--- INSERT TWO MEMBERS
  db.project.update(query, mod)
}

```

###Adicione 1 comentário em cada atividade, deixe apenas 1 projeto sem.

```js
var values = ["primeiro comentario", "segundo comentario", "terceiro comentario", "quarto comentario"];
for (var i = 0; i < 4; i++){
  var getId = db.activity.find().skip(i).limit(1).toArray();
  var query = {_id: getId[0]._id}
  var mod = {$push: {comment: values[i]}}
  db.activity.update(query, mod)
}

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

###Adicione 1 projeto inteiro com UPSERT.

```js

var getProject = {
      name: "project inserted upsert",
      description: "description teste",
      datebegin: atual_dt(),
      datedream: atual_dt(),
      dateend: endproject(Math.floor(Math.random() * (30 - 7) + 7)),
      visible: true,
      realocate: false,
      expired: false,
      visualizablemod: null,
      projecttag: ["js","upsert", "dev"],
      members: {
        type_member: "contribuitor",
        user_id:ObjectId("569e90075cbee4e1d4a750f7"),
        notify: true
      },
      goals: {
        name: "to be very intelligent in js",
        description: "wy yep",
        datebegin: atual_dt(),
        datedream: atual_dt(),
        dateend: endproject(Math.floor(Math.random() * (30 - 7) + 7)),
        realocate: false,
        expired: false,
        history: null,
        goalstag: ["win", "linux","mac"],
        activity:{  
          activityID:ObjectId("569ecbf05cbee4e1d4a75110"),
          activityID2:ObjectId("569ecbf05cbee4e1d4a75114")
        }
      }
    }
var query = {name: "project inserted upsert"}
var options = {upsert: true}
db.project.update(query, getProject, options)

Updated 1 new record(s) in 28ms
WriteResult({
  "nMatched": 0,
  "nUpserted": 1,
  "nModified": 0,
  "_id": ObjectId("56a0112b9b505010b37abf96")
})
```

## Delete - remoção

###Apague todos os projetos que não possuam tags.

```js
db.project.remove({ projecttags: { $eq: [ ] } }, { multi: 1 })
Removed 0 record(s) in 2ms
WriteResult({
  "nRemoved": 0
})
```
###Apague todos os projetos que não possuam comentários nas atividades.

```js
var activity = db.activity.find({ comments: { $eq: [ ] } }).toArray();

for (var i = 0; i < activity.length; i++){
  db.project.remove({ activityID:activity[i]._id}, { multi: 1 });
  db.project.remove({ activityID2:activity[i]._id}, { multi: 1 });
}

Removed 1 record(s) in 2ms
WriteResult({
  "nRemoved": 1
})
```

###Apague todos os projetos que não possuam atividades.

```js
db.project.remove({ activity: { $eq: []} }, { multi: 1 })

Removed 0 record(s) in 2ms
WriteResult({
  "nRemoved": 0
})
```

###Escolha 2 usuário e apague todos os projetos em que os 2 fazem parte.

```js

db.projects.remove({ "members.user_id": { $in: [ObjectId("569e90075cbee4e1d4a750f7"), ObjectId("569e90075cbee4e1d4a750f8") ] } }, { multi: 1 })

Removed 0 record(s) in 6ms
WriteResult({
  "nRemoved": 0
})
```

###Apague todos os projetos que possuam uma determinada tag em goal.

```js
var query = {"goals.goalstag": {$in: [/cpf/i]}}
db.project.remove(query, {multi: 1})

Removed 1 record(s) in 2ms
WriteResult({
  "nRemoved": 1
})

```

##Gerenciamento de Usuários

###Crie um usuário com permissões APENAS de Leitura.

```js
use admin
db.createUser(
  {
    user: "victorvoid",
    pwd: "wanvictor",
    roles: [
        "read"
    ]
  }
)

Successfully added user: {
  "user": "victorvoid",
  "roles": [
    "read"
  ]
}

```

###Crie um usuário com permissões de Escrita e Leitura.

```js

use admin
db.createUser(
  {
    user: "eduardo",
    pwd: "waneduardo",
    roles: ["readWrite"]
  }
)

Successfully added user: {
  "user": "eduardo",
  "roles": [
    "readWrite"
  ]
}

```

###Adicionar o papel grantRolesToUser e revokeRole para o usuário com Escrita e Leitura.

```js
db.createRole(
   {
     role: "grantRolesToUser",
     privileges: [
       { resource: { db: "admin", collection: "" }, actions: [ "grantRole" ] }       
     ],
     roles: []
   },
   { w: "majority" , wtimeout: 5000 }
)

{
  "role": "grantRolesToUser",
  "privileges": [
    {
      "resource": {
        "db": "admin",
        "collection": ""
      },
      "actions": [
        "grantRole"
      ]
    }
  ],
  "roles": []
}

db.createRole(
     {
       role: "revokeRole",
       privileges: [
         { resource: { db: "admin", collection: "" }, actions: [ "revokeRole" ] }       
       ],
       roles: []
     },
     { w: "majority" , wtimeout: 5000 }
  )
{
"role": "revokeRole",
"privileges": [
  {
    "resource": {
      "db": "admin",
      "collection": ""
    },
    "actions": [
      "revokeRole"
    ]
  }
],
"roles": [ ]
}

//add - > user
db.grantRolesToUser(
    "eduardo",
    [ "grantRolesToUser" , "revokeRole" ]
  )
```

###Remover o papel grantRolesToUser para o usuário com Escrita e Leitura.

```js
db.runCommand({
  revokeRolesFromUser: "eduardo",
  roles: [
    "grantRolesToUser"
  ]
})
{
  "ok": 1
}
```

###Listar todos os usuários com seus papéis e ações.

```js
db.runCommand({
  usersInfo: 1
})
{
  "users": [
    {
      "_id": "admin.victorvoid",
      "user": "victorvoid",
      "db": "admin",
      "roles": [
        {
          "role": "read",
          "db": "admin"
        }
      ]
    },
    {
      "_id": "admin.eduardo",
      "user": "eduardo",
      "db": "admin",
      "roles": [
        {
          "role": "readWrite",
          "db": "admin"
        }
      ]
    }
  ],
  "ok": 1
}

```

##Sharding

```js

/* Precisamos criar um Cluster na mão, primeiro criamos um config server */

/** CONFIG SERVER **/
mkdir /data/configdb
mongod --configsvr --port 27010

/** ROUTER **/
mongos --configdb localhost:27010 --port 27011

/** addSHARDING
Criando 3 pastas, para os 3 sharding **/
mkdir /data/shard1 && mkdir /data/shard2 && mkdir /data/shard3

mongod --port 27012 --dbpath /data/shard1
mongod --port 27013 --dbpath /data/shard2
mongod --port 27014 --dbpath /data/shard3

/* Registrando os Shards no Router */
mongo --port 27011 --host localhost
MongoDB shell version: 3.0.8
connecting to: localhost:27011/test

use projFinal

sh.addShard("localhost:27012")  
{
  "shardAdded": "shard0000",
  "ok": 1
}
sh.addShard("localhost:27013")  
{
  "shardAdded": "shard0001",
  "ok": 1
}
sh.addShard("localhost:27014")

{
  "shardAdded": "shard0002",
  "ok": 1
}

/* Especificando qual a database iremos shardear*/

sh.enableSharding("projFinal")
{
  "ok": 1
}
/* especificando qual a coleção */
sh.shardCollection("projFinal.users",{"_id": 1})
{
  "collectionsharded": "projFinal.users",
  "ok": 1
}

```

## Replica

```js
/* criando os 3 diretórios */
mkdir /data/rs1
ᐅ  mkdir /data/rs2
ᐅ  mkdir /data/rs3

/* shell script que cria 3 servidores de replica em portas diferentes, e background*/
sudo mongod --replSet replica_set --port 27017 -dbpath /data/rs1
sudo mongod --replSet replica_set --port 27018 -dbpath /data/rs2
sudo mongod --replSet replica_set --port 27019 -dbpath /data/rs3

/* configurando e iniciando */
mongo --port 27017


var rsconf = {
   _id: "replica_set",
   members: [
    {
     _id: 0,
     host: "127.0.0.1:27017"
    }
  ]
}
rs.initiate(rsconf)
{ "ok" : 1 }

/*Adicionando as outras duas replicas */
rs.add("127.0.0.18")
rs.add("127.0.0.19")
```
