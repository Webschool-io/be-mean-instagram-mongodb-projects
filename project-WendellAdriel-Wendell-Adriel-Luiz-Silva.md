# MongoDB - Projeto Final

**Autor:** Wendell Adriel Luiz Silva  
**Data:** 1454852286019

## Para qual sistema você usaria o MogoDB (diferente desse)?

- CMS
- E-commerce
- Blog engine
- Rede social
- Jogos

## Qual a modelagem da sua coleção de users?

```js
users : {
  name : String,
  bio : String,
  date_register : Date,
  avatar_path : String,
  background_path : String,
  username : String,
  email : String,
  password : String,
  last_access : Date,
  online : Boolean,
  disabled : Boolean,
  hash_token : String
}
```

## Qual a modelagem da sua coleção de projects?

```js
projects : {
  name : String,
  description : String,
  visible : Boolean,
  date_begin : Date,
  date_dream : Date,
  date_end : Date,
  realocate : Boolean,
  expired : Boolean,
  visualizable_mod : String,
  tags : [],
  members : [{
    user_id : ObjectId(),
    type : String,
    notify : Boolean
  }],
  goal : [{
    name: String,
    description: String,
    date_begin : Date,
    date_dream : Date,
    date_end : Date,
    realocate : Boolean,
    expired : Boolean,
    tags : [],
    historic : [{ date_realocate : Date }],
    activities : [{ activity_id : ObjectId() }]
  }]
}
```

## Qual a modelagem da sua coleção retirada de projects?

```js
activities : {
  name : String,
  description: String,
  date_begin : Date,
  date_dream : Date,
  date_end : Date,
  realocate : Boolean,
  expired : Boolean,
  tags : [],
  historic : [{ date_realocate : Date }],
  members : [{
    user_id : ObjectId(),
    type : String,
    notify : Boolean
  }],
  comments : [{
    text : String,
    date_comment : Date,
    member : {
      user_id : ObjectId(),
      type : String,
      notify : Boolean
    },
    files : [{
      name : String,
      path : String,
      weight : int
    }]
  }]
}
```

## Create - cadastro

### Cadastre 10 usuários diferentes

```js
mongo-project> function insertUsers() {
  for (var i = 0; i < 10; i++) {
    var user = {
      name : 'User' + i,
      bio : 'Lorem ipsum BIO',
      date_register : new Date(),
      avatar_path : 'path-to-avatar',
      background_path : 'path-to-background',
      username : 'user' + i,
      email : 'user' + i + '@gmail.com',
      password : '123456789',
      last_access : new Date(),
      online : false,
      disabled : false,
      hash_token : 'as9d8fskfj230909asdkflajcs'
    };
    db.users.insert(user);
  }
}
mongo-project> insertUsers()
```

### Cadastre 5 projetos diferentes

```js
// Cadastrando activities
mongo-project> function insertActivities() {
  var today = new Date();
  var yesterday = new Date();
  var tomorrow = new Date();
  yesterday = yesterday.setDate(yesterday.getDate() - 1);
  tomorrow = tomorrow.setDate(tomorrow.getDate() + 1);
  for (var i = 0; i < 8; i++) {
    var activity = {
      name : 'Activiy' + i,
      description: 'Boring description',
      date_begin : yesterday,
      date_dream : today,
      date_end : tomorrow,
      realocate : false,
      expired : false,
      tags : [],
      historic : [],
      comments : []
    };
    db.activities.insert(activity);
  }
}
mongo-project> insertActivities()

// Cadastrando projetos
mongo-project> var projects = [
  {
    name : 'First Project',
    description : 'Lorem ipsum description',
    visible : true,
    date_begin : new Date(),
    date_dream : new Date(),
    date_end : new Date(),
    realocate : false,
    expired : false,
    visualizable_mod : 'all',
    tags : ['js', 'mongodb'],
    members : [
      {
        user_id : ObjectId("56b75cfc1e5e9bc994c3db84"),
        type : 'developer',
        notify : true
      },
      {
        user_id : ObjectId("56b75cfc1e5e9bc994c3db85"),
        type : 'developer',
        notify : true
      },
      {
        user_id : ObjectId("56b75cfc1e5e9bc994c3db86"),
        type : 'designer',
        notify : true
      },
      {
        user_id : ObjectId("56b75cfc1e5e9bc994c3db87"),
        type : 'dba',
        notify : true
      },
      {
        user_id : ObjectId("56b75cfc1e5e9bc994c3db88"),
        type : 'manager',
        notify : true
      }
    ],
    goal : [
        {
          name: 'First Goal',
          description: 'Lorem ipsum description',
          date_begin : new Date(),
          date_dream : new Date(),
          date_end : new Date(),
          realocate : false,
          expired : false,
          tags : ['js', 'mongodb', 'webschool'],
          historic : [{ date_realocate : new Date() }],
          activities : [
            { activity_id : ObjectId("56b761fa1e5e9bc994c3db8e") },
            { activity_id : ObjectId("56b761fa1e5e9bc994c3db8f") }
          ]
        }
    ]
  },
  {
    name : 'Second Project',
    description : 'Lorem ipsum description',
    visible : true,
    date_begin : new Date(),
    date_dream : new Date(),
    date_end : new Date(),
    realocate : false,
    expired : false,
    visualizable_mod : 'none',
    tags : ['js', 'angular', 'webschool'],
    members : [
      {
        user_id : ObjectId("56b75cfc1e5e9bc994c3db89"),
        type : 'developer',
        notify : true
      },
      {
        user_id : ObjectId("56b75cfc1e5e9bc994c3db8a"),
        type : 'developer',
        notify : true
      },
      {
        user_id : ObjectId("56b75cfc1e5e9bc994c3db8b"),
        type : 'designer',
        notify : true
      },
      {
        user_id : ObjectId("56b75cfc1e5e9bc994c3db8c"),
        type : 'dba',
        notify : true
      },
      {
        user_id : ObjectId("56b75cfc1e5e9bc994c3db8d"),
        type : 'manager',
        notify : true
      }
    ],
    goal : [
        {
          name: 'First Goal',
          description: 'Lorem ipsum description',
          date_begin : new Date(),
          date_dream : new Date(),
          date_end : new Date(),
          realocate : false,
          expired : false,
          tags : ['js', 'mongodb', 'angular'],
          historic : [{ date_realocate : new Date() }],
          activities : [
            { activity_id : ObjectId("56b761fa1e5e9bc994c3db90") },
            { activity_id : ObjectId("56b761fa1e5e9bc994c3db91") }
          ]
        }
    ]
  },
  {
    name : 'Third Project',
    description : 'Lorem ipsum description',
    visible : true,
    date_begin : new Date(),
    date_dream : new Date(),
    date_end : new Date(),
    realocate : false,
    expired : false,
    visualizable_mod : 'all',
    tags : ['js', 'express'],
    members : [
      {
        user_id : ObjectId("56b75cfc1e5e9bc994c3db84"),
        type : 'developer',
        notify : true
      },
      {
        user_id : ObjectId("56b75cfc1e5e9bc994c3db86"),
        type : 'developer',
        notify : true
      },
      {
        user_id : ObjectId("56b75cfc1e5e9bc994c3db88"),
        type : 'designer',
        notify : true
      },
      {
        user_id : ObjectId("56b75cfc1e5e9bc994c3db8a"),
        type : 'dba',
        notify : true
      },
      {
        user_id : ObjectId("56b75cfc1e5e9bc994c3db8c"),
        type : 'manager',
        notify : true
      }
    ],
    goal : [
        {
          name: 'First Goal',
          description: 'Lorem ipsum description',
          date_begin : new Date(),
          date_dream : new Date(),
          date_end : new Date(),
          realocate : false,
          expired : false,
          tags : ['js', 'angular', 'express'],
          historic : [{ date_realocate : new Date() }],
          activities : [
            { activity_id : ObjectId("56b761fa1e5e9bc994c3db92") },
            { activity_id : ObjectId("56b761fa1e5e9bc994c3db93") }
          ]
        }
    ]
  },
  {
    name : 'Fourth Project',
    description : 'Lorem ipsum description',
    visible : true,
    date_begin : new Date(),
    date_dream : new Date(),
    date_end : new Date(),
    realocate : false,
    expired : false,
    visualizable_mod : 'none',
    tags : ['mongodb', 'node', 'study'],
    members : [
      {
        user_id : ObjectId("56b75cfc1e5e9bc994c3db85"),
        type : 'developer',
        notify : true
      },
      {
        user_id : ObjectId("56b75cfc1e5e9bc994c3db87"),
        type : 'developer',
        notify : true
      },
      {
        user_id : ObjectId("56b75cfc1e5e9bc994c3db89"),
        type : 'designer',
        notify : true
      },
      {
        user_id : ObjectId("56b75cfc1e5e9bc994c3db8b"),
        type : 'dba',
        notify : true
      },
      {
        user_id : ObjectId("56b75cfc1e5e9bc994c3db8d"),
        type : 'manager',
        notify : true
      }
    ],
    goal : [
        {
          name: 'First Goal',
          description: 'Lorem ipsum description',
          date_begin : new Date(),
          date_dream : new Date(),
          date_end : new Date(),
          realocate : false,
          expired : false,
          tags : ['js', 'node', 'angular'],
          historic : [{ date_realocate : new Date() }],
          activities : [
            { activity_id : ObjectId("56b761fa1e5e9bc994c3db94") },
            { activity_id : ObjectId("56b761fa1e5e9bc994c3db95") }
          ]
        }
    ]
  },
  {
    name : 'Fifth Project',
    description : 'Lorem ipsum description',
    visible : true,
    date_begin : new Date(),
    date_dream : new Date(),
    date_end : new Date(),
    realocate : false,
    expired : false,
    visualizable_mod : 'all',
    tags : ['grunt', 'atomic', 'front'],
    members : [
      {
        user_id : ObjectId("56b75cfc1e5e9bc994c3db84"),
        type : 'developer',
        notify : true
      },
      {
        user_id : ObjectId("56b75cfc1e5e9bc994c3db85"),
        type : 'developer',
        notify : true
      },
      {
        user_id : ObjectId("56b75cfc1e5e9bc994c3db89"),
        type : 'designer',
        notify : true
      },
      {
        user_id : ObjectId("56b75cfc1e5e9bc994c3db8c"),
        type : 'dba',
        notify : true
      },
      {
        user_id : ObjectId("56b75cfc1e5e9bc994c3db8d"),
        type : 'manager',
        notify : true
      }
    ],
    goal : [
        {
          name: 'First Goal',
          description: 'Lorem ipsum description',
          date_begin : new Date(),
          date_dream : new Date(),
          date_end : new Date(),
          realocate : false,
          expired : false,
          tags : ['mongodb', 'express', 'angular', 'node'],
          historic : [{ date_realocate : new Date() }],
          activities : []
        }
    ]
  }
]
mongo-project> db.projects.insert(projects)
```

## Retrieve - busca

### Liste as informações dos membros de 1 projeto específico que deve ser buscado pelo seu nome de forma a não ligar para maiúsculas e minúsculas.

```js
mongo-project> var membersToSearch = [];
mongo-project> var query = { name : /second project/i }
mongo-project> var project = db.projects.findOne(query)
mongo-project> function getMembersId(project) {
  for (var i = 0; i < project.members.length; i++) {
    membersToSearch.push(project.members[i].user_id);
  }
}
mongo-project> getMembersId(project)
mongo-project> var query = { _id : { $in : membersToSearch } }
mongo-project> db.users.find(query)
```

### Liste todos os projetos com a tag que você escolheu para os 3 projetos em comum.

```js
mongo-project> var query = { tags : /js/i }
mongo-project> db.projects.find(query)
```

### Liste apenas os nomes de todas as atividades para todos os projetos.

```js
mongo-project> var activitiesId = []
mongo-project> var query = { 'goal.activities' : { $not : { $eq : [] } } }
mongo-project> var fields = { _id : 0, 'goal.activities.activity_id' : 1 }
mongo-project> var projects = db.projects.find(query, fields).toArray()
mongo-project> projects.forEach(function(project) {
  project.goal.forEach(function(goal) {
    goal.activities.forEach(function(activity) {
      activitiesId.push(activity.activity_id);
    })
  })
})
mongo-project> var query = { _id : { $in : activitiesId } }
mongo-project> var fields = { _id : 0, name : 1 }
mongo-project> db.activities.find(query, fields)
```

### Liste todos os projetos que não possuam uma tag.

```js
mongo-project> var query = { tags:  { $not: { $in: ['mongodb'] } } };
mongo-project> db.projects.find(query)
```

### Liste todos os usuários que não fazem parte do primeiro projeto cadastrado.

```js
mongo-project> var membersId = [];
mongo-project> var query = { name : /first project/i }
mongo-project> var project = db.projects.findOne(query)
mongo-project> project.members.forEach(function(member) {
  membersId.push(member.user_id);
})
mongo-project> var query = { _id : { $not : { $in : membersId } } }
mongo-project> db.users.find(query)
```

## Update - alteração

### Adicione para todos os projetos o campo views: 0.

```js
mongo-project> var query = {}
mongo-project> var mod = { $set : { views : 0 } }
mongo-project> var options = { multi : true }
mongo-project> db.projects.update(query, mod, options)
```

### Adicione 1 tag diferente para cada projeto.

```js
mongo-project> var projectsId = db.projects.find({}, {_id: 1}).toArray()
mongo-project> function insertNewTags(projectsId) {
  for (var i = 0; i < projectsId.length; i++) {
    var query = { _id : projectsId[i]._id };
    var mod = { $push : { tags: 'newTag' + i } };
    db.projects.update(query, mod);
  }
}
mongo-project> insertNewTags(projectsId)
```

### Adicione 2 membros diferentes para cada projeto.

```js
mongo-project> var projectsId = db.projects.find({}, {_id: 1}).toArray()
mongo-project> function randomMembers() {
  var newMembers = [];
  for (var i = 0; i < 2; i++) {
    var randomNumber = Math.floor(Math.random() * 50);
    var randUser = {
      name : 'New Random User ' + randomNumber,
      bio : 'Lorem ipsum BIO',
      date_register : new Date(),
      avatar_path : 'path-to-avatar',
      background_path : 'path-to-background',
      username : 'random_user' + randomNumber,
      email : 'random_user' + randomNumber + '@gmail.com',
      password : '123456789',
      last_access : new Date(),
      online : false,
      disabled : false,
      hash_token : 'as9d8fskfj230909asdkflajcs'
    };
    newMembers.push(randUser);
  }
  return newMembers;
}
mongo-project> function insertNewMembers(projectsId) {
  for (var i = 0; i < projectsId.length; i++) {
    var query = { _id : projectsId[i]._id };
    var mod = { $pushAll : { members: randomMembers() } };
    db.projects.update(query, mod);
  }
}
mongo-project> insertNewMembers(projectsId)
```

### Adicione 1 comentário em cada atividade, deixe apenas 1 projeto sem.

```js
mongo-project> var activitiesId = [];
mongo-project> var projects = db.projects.find().limit(3).toArray()
mongo-project> projects.forEach(function(project) {
  project.goal.forEach(function(goal) {
    goal.activities.forEach(function(activity) {
      activitiesId.push(activity.activity_id);
    })
  })
})
mongo-project> function insertNewComments(activitiesId) {
  for (var i = 0; i < activitiesId.length; i++) {
    var number_id = 84;
    var text_id = '56b75cfc1e5e9bc994c3db' + (84 + i);
    var comment = [{
      text : 'New Comment',
      date_comment : new Date(),
      member : {
        user_id : ObjectId(text_id),
        type : 'developer',
        notify : true
      },
      files : [{
        name : 'new_file',
        path : 'path_to_file',
        weight : Math.floor(Math.random() * 1024)
      }]
    }];
    var query = { _id : activitiesId[i] };
    var mod = { $push : { comments : comment } };
    db.activities.update(query, mod);
  }
}
mongo-project> insertNewComments(activitiesId)
```

### Adicione 1 projeto inteiro com UPSERT.

```js
mongo-project> var goals = [{
  name: 'First Goal',
  description: 'Lorem ipsum description',
  date_begin : new Date(),
  date_dream : new Date(),
  date_end : new Date(),
  realocate : false,
  expired : false,
  tags : ['mongodb', 'express', 'angular', 'node', 'webschool'],
  historic : [{ date_realocate : new Date() }],
  activities : []
}]
mongo-project> var members =[{
  name : 'New Project User',
  bio : 'Lorem ipsum BIO',
  date_register : new Date(),
  avatar_path : 'path-to-avatar',
  background_path : 'path-to-background',
  username : 'newprojectuser',
  email : 'newprojectuser@gmail.com',
  password : '123456789',
  last_access : new Date(),
  online : false,
  disabled : false,
  hash_token : 'as9d8fskfj230909asdkflajcs'
}]
mongo-project> var tags = ['mongodb', 'angular', 'js']
mongo-project> var query = { name : /ultra project/i }
mongo-project> var mod = {
  $setOnInsert : {
    name : 'Ultra Project',
    description : 'Lorem ipsum description',
    date_begin : new Date(),
    date_dream : new Date(),
    date_end : new Date(),
    visible : true,
    realocate : false,
    expired : false,
    visualizable_mod : "all",
    members : members,
    goal : goals,
    tags : tags
  }
}
mongo-project> var options = { upsert: true }
mongo-project> db.projects.update(query, mod, options)
```
## Delete - remoção

### Apague todos os projetos que não possuam tags.

```js
// Tira tags de um projeto
mongo-project> var query = { _id : ObjectId("56b78bc413e547e247288cc1") }
mongo-project> var mod = { $set : { tags : [] } }
mongo-project> db.projects.update(query, mod)

// Apagando
mongo-project> var query = { $or : [{ tags : { $size : 0 } }, { tags : { $exists : false } }, { tags : { $eq : [] } }] }
mongo-project> db.projects.remove(query)
```

### Apague todos os projetos que não possuam comentários nas atividades.

```js
mongo-project> var activitiesId = []
mongo-project> var query = { $or : [{ comments : { $size : 0 } }, { comments : { $exists : false } }, { comments : { $eq : [] } }] }
mongo-project> var activities = db.activities.find(query).toArray()
mongo-project> activities.forEach(function(activity) {
  activitiesId.push(activity._id);
})
mongo-project> var query = { 'goal.activities.activity_id' : { $in : activitiesId } }
mongo-project> var options = { multi : true }
mongo-project> db.projects.remove(query, options)
```

### Apague todos os projetos que não possuam atividades.

```js
mongo-project> var query = { $or : [{ 'goal.activities' : { $size : 0 } }, { 'goal.activities' : { $exists : false } }, { 'goal.activities' : { $eq : [] } }] }
mongo-project> db.projects.remove(query)
```

### Escolha 2 usuário e apague todos os projetos em que os 2 fazem parte.

```js
mongo-project> var usersId = [ObjectId("56b75cfc1e5e9bc994c3db84"), ObjectId("56b75cfc1e5e9bc994c3db89")]
mongo-project> var query = { 'members.user_id' : { $in : usersId } }
mongo-project> db.projects.remove(query)
```

### Apague todos os projetos que possuam uma determinada tag em goal.

```js
// Adicionando projetos iniciais novamente com script usado na parte de inserção
mongo-project> load('script.js')
// Removendo projetos
mongo-project> var query = { 'goal.tags' : { $eq : 'mongodb' } }
mongo-project> db.projects.remove(query)
```

## Gerenciamento de Usuários

### Crie um usuário com permissões APENAS de Leitura.

```js
mongo-project> var user = {
  user : 'wendell',
  pwd : '123456',
  roles : [{ role : 'read', db : 'admin' }]
}
mongo-project> db.createUser(user)
```

### Crie um usuário com permissões de Escrita e Leitura.

```js
mongo-project> var user = {
  user : 'wendellrw',
  pwd : '123456',
  roles : [{ role : 'readWrite', db : 'admin' }]
}
mongo-project> db.createUser(user)
```

### Adicionar o papel grantRolesToUser e revokeRole para o usuário com Escrita e Leitura.

```js
// Criando primeira role
mongo-project> var newRole = {
  role : 'grantRolesToUser',
  roles : [{ role : 'readWrite', db : 'admin' }],
  privileges : [{ resource : { db: 'admin', collection : '' }, actions : ['grantRole'] }]
}
mongo-project> db.createRole(newRole)

// Criando segunda role
mongo-project> var newRole = {
  role : 'revokeRole',
  roles : [{ role : 'readWrite', db : 'admin' }],
  privileges : [{ resource : { db: 'admin', collection : '' }, actions : ['revokeRole'] }]
}
mongo-project> db.createRole(newRole)

// Concedendo papéis ao usuário
mongo-project> db.grantRolesToUser('wendellrw', ['grantRolesToUser', 'revokeRole'])
```

### Remover o papel grantRolesToUser para o usuário com Escrita e Leitura.

```js
mongo-project> var revokeCmd = {
  revokeRolesFromUser : 'wendellrw',
  roles : [{ role : 'grantRolesToUser', db : 'admin' }]
}
mongo-project> db.runCommand(revokeCmd)
```

### Listar todos os usuários com seus papéis e ações.

```js
mongo-project> db.runCommand({ usersInfo : 1 })
```

## Sharding

```js
// Criando e iniciando o config server
cd data
mkdir configdb
mongod --configsvr --port 27010

// Iniciando router
mongos --configdb localhost:27010 --port 27011

// Criando shardings
mkdir shard1 && mkdir shard2 && mkdir shard3

// Iniciando shards
mongod --port 27012 --dbpath shard1
mongod --port 27013 --dbpath shard2
mongod --port 27014 --dbpath shard3

// Conectando no router e adicionando os shards
mongo --port 27011
mongo-project> sh.addShard('localhost:27012')
mongo-project> sh.addShard('localhost:27013')
mongo-project> sh.addShard('localhost:27014')

// Definindo qual banco de dados será usado no sharding
mongo-project> sh.enableSharding('mongo-project')

// Definindo qual collection iremos usar no sharding
mongo-project> sh.shardCollection('mongo-project.projects', { _id : 1 })

// Inserindo registros
mongo-project> function massInsert() {
  for (var i = 0; i < 100000; i++) {
    var project = {
      name : 'Secret Project #' + i,
      description : 'Lorem ipsum description',
      date_begin : new Date(),
      date_dream : new Date(),
      date_end : new Date(),
      visible : true,
      realocate : false,
      expired : false,
      visualizable_mod : "none",
      members : [],
      goal : [],
      tags : []
    };
    db.projects.insert(project);
  }
}
mongo-project> massInsert()
```

## Réplica

```js
// Criando réplicas
cd data
mkdir rep1 && mkdir rep2 && mkdir rep3

// Iniciando réplicas
mongod --replSet replica_set --port 27017 --dbpath rep1
mongod --replSet replica_set --port 27018 --dbpath rep2
mongod --replSet replica_set --port 27019 --dbpath rep3

// Conectando na réplica primária e configurando o grupo de réplicas
mongo --port 27017
mongo-project> var config = {
  _id : 'replica_set',
  members : [{ _id : 0, host : '127.0.0.1:27017' }]
}
mongo-project> rs.initiate(config)
mongo-project> rs.add('127.0.0.1:27018')
mongo-project> rs.add('127.0.0.1:27019')

// Verificando status da replica_set configurada
mongo-project> rs.status()
{
  "set": "replica_set",
  "date": ISODate("2016-02-07T19:52:14.384Z"),
  "myState": 1,
  "term": NumberLong("1"),
  "heartbeatIntervalMillis": NumberLong("2000"),
  "members": [
    {
      "_id": 0,
      "name": "127.0.0.1:27017",
      "health": 1,
      "state": 1,
      "stateStr": "PRIMARY",
      "uptime": 411,
      "optime": {
        "ts": Timestamp(1454874644, 1),
        "t": NumberLong("1")
      },
      "optimeDate": ISODate("2016-02-07T19:50:44Z"),
      "electionTime": Timestamp(1454874580, 2),
      "electionDate": ISODate("2016-02-07T19:49:40Z"),
      "configVersion": 3,
      "self": true
    },
    {
      "_id": 1,
      "name": "127.0.0.1:27018",
      "health": 1,
      "state": 2,
      "stateStr": "SECONDARY",
      "uptime": 98,
      "optime": {
        "ts": Timestamp(1454874644, 1),
        "t": NumberLong("1")
      },
      "optimeDate": ISODate("2016-02-07T19:50:44Z"),
      "lastHeartbeat": ISODate("2016-02-07T19:52:14.277Z"),
      "lastHeartbeatRecv": ISODate("2016-02-07T19:52:13.322Z"),
      "pingMs": NumberLong("0"),
      "syncingTo": "127.0.0.1:27017",
      "configVersion": 3
    },
    {
      "_id": 2,
      "name": "127.0.0.1:27019",
      "health": 1,
      "state": 2,
      "stateStr": "SECONDARY",
      "uptime": 90,
      "optime": {
        "ts": Timestamp(1454874644, 1),
        "t": NumberLong("1")
      },
      "optimeDate": ISODate("2016-02-07T19:50:44Z"),
      "lastHeartbeat": ISODate("2016-02-07T19:52:14.277Z"),
      "lastHeartbeatRecv": ISODate("2016-02-07T19:52:11.708Z"),
      "pingMs": NumberLong("0"),
      "configVersion": 3
    }
  ],
  "ok": 1
}
```
