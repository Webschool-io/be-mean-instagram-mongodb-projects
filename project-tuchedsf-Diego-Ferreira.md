# MongoDb - Projeto Final
**Autor:** Diego Ferreira
**Data** Início: 08/12/2015 - 11:23  / Fim:

## Para qual sistema você usaria o MogoDB (diferente desse)?

Usaria o MongoDb em sistemas onde tivessem altas cargas de leitura, sendo necessário dividi-los ou que tenham uma alta taxa de crescimento, e os dados necessitem de ser recuperados rapidamente, facilitando a escalabilidade horizontal e com isso mantendo boa performance.

Sobre o projeto atual:
A escolha da modelagem abaixo se deu devido aos conteudos passados pelo Suissa em aula, principalmente as dicas de modelagem na aula 7. Uma das dicas dadas por ele foi que o mongo preza pela velocidade, e, detrerimento a duplicidade de informações, quanto mais fácil de se encontrar o dado que esta pesquisando melhor. A partir desta informação consegui detectar 3 blocos príncipais, na modelagem apresentada, ou seja 3 assuntos bem destintos (usuario / projeto / atividades ) na modelagem os quais decidi que deveriam ser as coleções existentes no sistema. Ou seja, uma modelagem em como o sistema faz as requisiçoes de informações para o banco de dados.

## Qual a modelagem da sua coleção de `users`?
```
user : {
  name ,
  bio,
  data_register,
  avatar_path,
  auth : {
    username,
    email,
    password,
    last_access,
    online,
    disable,
    hash_token
  },
  background-path
}
```
## Qual a modelagem da sua coleção de `projects`?
```
project : {
  name,
  description,
  date_begin,
  date_dream,
  date_end,
  visible,
  realocate,
  expired,
  vizualizable_mod,
  tags: [],
  members: [{
    user_id,
    type_member,
    notify
    }],
  goals : [{
    name,
    description,
    date_begin,
    date_dream,
    date_end,
    realocate,
    expired,
    date_realocate,
    tags : [],
    activities : []    
  }]
}
```

## Outras modelagens feitas:
```
activity : {
  name,
  description,
  date_begin,
  date_dream,
  date_end,
  realocate,
  expired,
  date_realocate,
  members: [{
    user_id,
    type_member,
    notify
    }],
  tags : [],
    comments : [{
    text,
    date_comment,
    files :[{
      path,
      weight,
      name
    }],
    members: [{
      user_id,
      type_member,
      notify
      }]
    }]
}
```
## Create - cadastro

##### 1. Cadastre 10 usuários diferentes:
```
db.users.insert({name: 'user1',
  bio: "Analista",
  data_register: "08/12/1025",
  avatar_path: "http://github.com/avatar/user1",
  auth: {
    username: "user1",
     email: "user1@project.com",
     password: "user1admin",
     last_access: "",
     online: false,
     disable: false,
     hash_token: "isdoiauoiuwoiu83793274ksjd893342"
   },
   background_path: "http://github.com/background-path/user1"})

Inserted 1 record(s) in 317ms
WriteResult({
  "nInserted": 1
})

db.users.insert({name: 'user2',
  bio: "tester",
  data_register: "07/12/1025",
  avatar_path: "http://github.com/avatar/user2",
  auth: {
    username: "user2",
    email: "user2@project.com",
    password: "user222222",
    last_access: "",
    online: false,
    disable: false,
    hash_token: "isdosdf34543iauoiuwoiu83793274ksjd893342"
  },
  background_path: "http://github.com/background-path/user2"})

Inserted 1 record(s) in 4ms
WriteResult({
  "nInserted": 1
})

db.users.insert([
  {name: 'Usuario 03',
  bio: "usuario 03 bio",
  data_register: "07/12/1025",
  avatar_path: "http://github.com/avatar/user3",
  auth: {
    username: "user3",
    email: "user3@project.com",
    password: "user33333",
    last_access: "",
    online: false,
    disable: false,
    hash_token: "isdosdf34543iauoiuwoiu83333793274ksjd33333"},
  background_path: "http://github.com/background-path/user3"},

  {name: 'Usuario 04',
  bio: "usuario 04 bio",
    data_register: "06/12/1025",
    avatar_path: "http://github.com/avatar/user4",
    auth: {
      username: "user4",
      email: "user4@project.com",
      password: "user44444",
      last_access: "",
      online: false,
      disable: false,
      hash_token: "isdosdf34543iauoiuwoiu83793274ksjd44444"},
    background_path: "http://github.com/background-path/user4"},

  {name: 'Usuario 05',
    bio: "usuario 05 bio",
    data_register: "07/12/1025",
    {name: 'Usuario 03',
    bio: "usuario 03 bio",
    data_register: "07/12/1025",
    avatar_path: "http://github.com/avatar/user3",
    auth: {
      username: "user3",
      email: "user3@project.com",
      password: "user33333",
      last_access: "",
      online: false,
      disable: false,
      hash_token: "isdosdf34543iauoiuwoiu83333793274ksjd33333"},
    background_path: "http://github.com/background-path/user3"}
])

Inserted 1 record(s) in 4ms
BulkWriteResult({
  "writeErrors": [ ],
  "writeConcernErrors": [ ],
  "nInserted": 3,
  "nUpserted": 0,
  "nMatched": 0,
  "nModified": 0,
  "nRemoved": 0,
  "upserted": [ ]
})

db.users.insert([
  {name: 'Usuario 06',
    bio: "usuario 06 bio",
    data_register: "09/12/1025",
    avatar_path: "http://github.com/avatar/user6",
    auth: {
      username: "user6",
      email: "user6@project.com",
      password: "user66666",
      last_access: "",
      online: false,
      disable: false,
      hash_token: "isdosdf34543iauoiuwoiu83793274ksjd666666"},
    background_path: "http://github.com/background-path/user6"},
  
  {name: 'Usuario 07',
    bio: "usuario 07 bio",
    data_register: "07/12/1025",
    avatar_path: "http://github.com/avatar/user7",
    auth: {
      username: "user7",
      email: "user7@project.com",
      password: "user77777",
      last_access: "",
      online: false,
      disable: false,
      hash_token: "isdosdf34543iauoiuwoiu83793274ksjd877777"},
      background_path: "http://github.com/background-path/user7"},

  {name: 'Usuario 08',
    bio: "usuario 08 bio",
    data_register: "07/12/1025",
    avatar_path: "http://github.com/avatar/user8",
    auth: {
      username: "user8",
      email: "user8@project.com",
      password: "user88888",
      last_access: "",
      online: false,
      disable: false,
      hash_token: "isdosdf34543iauoiuwoiu83793274ksjd89334288888"},
    background_path: "http://github.com/background-path/user8"
  }      
])

Inserted 1 record(s) in 4ms
BulkWriteResult({
  "writeErrors": [ ],
  "writeConcernErrors": [ ],
  "nInserted": 3,
  "nUpserted": 0,
  "nMatched": 0,
  "nModified": 0,
  "nRemoved": 0,
  "upserted": [ ]
})

db.users.insert([
  {name: 'Usuario 09',
   bio: "usuario 09 bio",
   data_register: "07/12/1025",
   avatar_path: "http://github.com/avatar/user9",
   auth: {
     username: "user9",
     email: "user9@project.com",
     password: "user99999",
     last_access: "",
     online: false,
     disable: false,
     hash_token: "isdosdf34543iauoiuwoiu83793274ksjd89334299999"},
   background_path: "http://github.com/background-path/user9"},
  
  {name: 'Usuario 10',
    bio: "usuario 10 bio",
    data_register: "07/12/1025",
    avatar_path: "http://github.com/avatar/user10",
    auth: {
      username: "user10",
      email: "user10@project.com",
      password: "user1010101010",
      last_access: "",
      online: false,
      disable: false,
      hash_token: "isdosdf34543iauoiuwoiu83793274ksjd893342101010101010"},
    background_path: "http://github.com/background-path/user10"}
])

Inserted 1 record(s) in 2ms
BulkWriteResult({
  "writeErrors": [ ],
  "writeConcernErrors": [ ],
  "nInserted": 2,
  "nUpserted": 0,
  "nMatched": 0,
  "nModified": 0,
  "nRemoved": 0,
  "upserted": [ ]
})
```
##### 2. Cadastre 5 projetos diferentes:

###### Projeto 1:
```
Air-de-Diego(mongod-3.0.7) be-mean-final> db.activities.insert([
{
  name: "execucao projeto",
  description: "execucao projeto cad",
  date_begin: "10/12/2015",
  date_dream: "10/12/2015",
  date_end: "10/12/2015",
  realocate: false,
  expired: false,
  date_realocate: "",
  members: [
    {"user_id": ObjectId("5666e2033aa0ba636df3f85f"),type_member: "Engenheiro tecnico",notify: true},
    {"user_id": ObjectId("5666e2033aa0ba636df3f860"),type_member: "Projetista",notify: true},
    {"user_id": ObjectId("5666e2033aa0ba636df3f861"),type_member: "Tecnico",notify: true}
  ],
  tags: ["engenharia", "civil","projetocad"],
  comments : [{
    text: "desenho planta inferior",
    date_comment: "09/12/2015",
    files :[{
      path: "teste/projetoCad1",
      weight: "22",
      name: "Projeto CAD 1"
    }],
    members: [
      {"user_id": ObjectId("5666e2033aa0ba636df3f861"),type_member: "Tecnico",notify: true}
    ]
  }]
},

{
  name: "Impressao 3d projeto",
  description: "impressao 3d projeto cad",
  date_begin: "10/12/2015",
  date_dream: "10/12/2015",
  date_end: "10/12/2015",
  realocate: false,
  expired: false,
  date_realocate: "",
  members: [
    {"user_id": ObjectId("5666e2033aa0ba636df3f860"),type_member: "Projetista",notify: true}
  ],
  tags: ["engenharia", "civil","projetocad"],
  comments : [{
    text: "envio projeto impressao",
    date_comment: "09/12/2015",
    files :[{
      path: "teste/projetoCad1",
      weight: "22",
      name: "Projeto CAD 1"
    }],
    members: [
      {"user_id": ObjectId("5666e2033aa0ba636df3f861"),type_member: "Tecnico",notify: true}
    ]
  }]
}])

Inserted 1 record(s) in 54ms
BulkWriteResult({
  "writeErrors": [ ],
  "writeConcernErrors": [ ],
  "nInserted": 2,
  "nUpserted": 0,
  "nMatched": 0,
  "nModified": 0,
  "nRemoved": 0,
  "upserted": [ ]
})

Air-de-Diego(mongod-3.0.7) be-mean-final> db.projects.insert(
{
  name : "PROJECT 1 - ENGENHARIA CIVIL",
  description: "Projeto 1 ",
  date_begin: "09/12/2015",
  date_dream: "01/02/2016" ,
  date_end: "",
  visible: true,
  realocate: false,
  expired: false,
  vizualizable_mod: false,
  tags: ["engenharia", "civil","projetocad"],
  members: [
    {"user_id": ObjectId("5666de303aa0ba636df3f85d"),type_member: "Gerente",notify: true},
    {"user_id": ObjectId("5666df133aa0ba636df3f85e"),type_member: "Engenheiro chefe",notify: true},
    {"user_id": ObjectId("5666e2033aa0ba636df3f85f"),type_member: "Engenheiro tecnico",notify: true},
    {"user_id": ObjectId("5666e2033aa0ba636df3f860"),type_member: "Projetista",notify: true},
    {"user_id": ObjectId("5666e2033aa0ba636df3f861"),type_member: "Tecnico",notify: true}
  ],
  goals : [{
    name: "Projeto CAD Edificio",
    description: "Construcao projeto cad edificio",
    date_begin: "10/12/2015",
    date_dream: "01/01/2016" ,
    date_end: "",
    realocate: false,
    expired: false,
    date_realocate : "",
    tags : ["planta", "planta3d", "cad"],
    activities : [{"activity_id": ObjectId("56681e8cd515fc7740146431")},{"activity_id": ObjectId("56681e8cd515fc7740146431")}]
  }]
})

Inserted 1 record(s) in 22ms
WriteResult({
  "nInserted": 1
})

```

###### Projeto 2:
```
Air-de-Diego(mongod-3.0.7) be-mean-final> db.projects.insert(
{
  name : "PROJECT 2 - ENGENHARIA Eletrica",
  description: "Projeto Engenharia Eletrica",
  date_begin: "09/12/2015",
  date_dream: "01/02/2016" ,
  date_end: "",
  visible: true,
  realocate: false,
  expired: false,
  vizualizable_mod: false,
  tags: ["engenharia", "energia","iluminacao"],
  members: [
    {"user_id": ObjectId("5666df133aa0ba636df3f85e"),type_member: "Engenheiro chefe",notify: true},
    {"user_id": ObjectId("5666e2033aa0ba636df3f85f"),type_member: "Engenheiro tecnico",notify: true},
    {"user_id": ObjectId("5666e2033aa0ba636df3f860"),type_member: "Projetista",notify: true},
    {"user_id": ObjectId("5666e2033aa0ba636df3f861"),type_member: "Tecnico",notify: true},
    {"user_id": ObjectId("5666e2f83aa0ba636df3f862"),type_member: "Engenheiro Eletrico",notify: true}
  ],
  goals : [{
    name: "Implantacao iluminacao urbana",
    description: "Iluminacao rua x",
    date_begin: "10/12/2015",
    date_dream: "01/01/2016" ,
    date_end: "",
    realocate: false,
    expired: false,
    date_realocate : "",
    tags : ["energia", "postes", "iluminacao"],
    activities : []
  }]
})

Inserted 1 record(s) in 2ms
WriteResult({
  "nInserted": 1
})
```
###### Projeto 3:
```
Air-de-Diego(mongod-3.0.7) be-mean-final> db.activities.insert([
{
  name: "Desmanche motor",
  description: "Desmonte do cabecote do motor",
  date_begin: "10/12/2015",
  date_dream: "10/12/2015",
  date_end: "10/12/2015",
  realocate: false,
  expired: false,
  date_realocate: "",
  members: [
    {"user_id": ObjectId("5666e2033aa0ba636df3f861"),type_member: "Tecnico",notify: true},
    {"user_id": ObjectId("5666e2033aa0ba636df3f85f"),type_member: "Engenheiro tecnico",notify: true}
  ],
  tags: ["desmanche", "motor","cabecote"],
  comments : [{
    text: "cuidado pressao interna ao desmontar",
    date_comment: "09/12/2015",
    files :[{
      path: "teste/projetoMotorCaminhado",
      weight: "22",
      name: "Motor Caminhao Fora Estrada"
    }],
    members: [
      {"user_id": ObjectId("5666e2033aa0ba636df3f85f"),type_member: "Engenheiro tecnico",notify: true}
    ]
  }]
},

{
  name: "Projeto Retifica Cabecote",
  description: "Realizar projeto de retifica do cabecote",
  date_begin: "10/12/2015",
  date_dream: "10/12/2015",
  date_end: "10/12/2015",
  realocate: false,
  expired: false,
  date_realocate: "",
  members: [
    {"user_id": ObjectId("5666e2033aa0ba636df3f860"),type_member: "Projetista",notify: true}
  ],
  tags: ["retifica", "motor","cabecote"],
  comments : []
}])     
Inserted 1 record(s) in 12ms
BulkWriteResult({
  "writeErrors": [ ],
  "writeConcernErrors": [ ],
  "nInserted": 2,
  "nUpserted": 0,
  "nMatched": 0,
  "nModified": 0,
  "nRemoved": 0,
  "upserted": [ ]
})

Air-de-Diego(mongod-3.0.7) be-mean-final> db.projects.insert(
{
  name : "PROJECT 2 - ENGENHARIA Mecanica",
  description: "Projeto Engenharia Mecanica",
  date_begin: "09/12/2015",
  date_dream: "01/02/2016" ,
  date_end: "",
  visible: true,
  realocate: false,
  expired: false,
  vizualizable_mod: false,
  tags: ["engenharia", "mecanica","motor"],
  members: [
    {"user_id": ObjectId("5666e2033aa0ba636df3f85f"),type_member: "Engenheiro tecnico",notify: true},
    {"user_id": ObjectId("5666e2033aa0ba636df3f860"),type_member: "Projetista",notify: true},
    {"user_id": ObjectId("5666e2033aa0ba636df3f861"),type_member: "Tecnico",notify: true},
    {"user_id": ObjectId("5666e2f83aa0ba636df3f862"),type_member: "Engenheiro Eletrico",notify: true},
    {"user_id": ObjectId("5666e2f83aa0ba636df3f863"),type_member: "Analista",notify: true}
  ],
  goals : [{
    name: "Retificacao",
    description: "Retifica motor caminhao fora de estrada",
    date_begin: "10/12/2015",
    date_dream: "01/01/2016" ,
    date_end: "",
    realocate: false,
    expired: false,
    date_realocate : "",
    tags : ["cabecote", "retifica", "pistao"],
    activities : [{"activity_id": ObjectId("5668231cd515fc7740146435")},{"activity_id": ObjectId("5668231cd515fc7740146436"),}]
  }]
})

Inserted 1 record(s) in 2ms
WriteResult({
  "nInserted": 1
})
```

###### Projeto 4:
```
Air-de-Diego(mongod-3.0.7) be-mean-final> db.activities.insert([
{
  name: "Cadastro Inicial",
  description: "Cadastro inicial dados popular base dados",
  date_begin: "10/12/2015",
  date_dream: "10/12/2015",
  date_end: "10/12/2015",
  realocate: false,
  expired: false,
  date_realocate: "",
  members: [
    {"user_id": ObjectId("5666e38a3aa0ba636df3f866"),type_member: "Desenvolvedor",notify: true},
    {"user_id": ObjectId("5666e2f83aa0ba636df3f864"),type_member: "Programador",notify: true}
  ],
  tags: ["mongodb", "insert","update"],
  comments : [{
    text: "exercicio 1 projeto final",
    date_comment: "09/12/2015",
    files :[{
      path: "github/projetoFinal",
      weight: "234",
      name: "projeto final mongodb"
    }],
    members: [
      {"user_id": ObjectId("5666e38a3aa0ba636df3f866"),type_member: "Desenvolvedor",notify: true}
    ]
  }]
},
{
  name: "Upate e retrive",
  description: "exercicio update e retrive",
  date_begin: "10/12/2015",
  date_dream: "10/12/2015",
  date_end: "10/12/2015",
  realocate: false,
  expired: false,
  date_realocate: "",
  members: [
    {"user_id": ObjectId("5666e38a3aa0ba636df3f866"),type_member: "Desenvolvedor",notify: true},
    {"user_id": ObjectId("5666e2f83aa0ba636df3f864"),type_member: "Programador",notify: true}
  ],
  tags: ["mongodb", "retrieve","update"],
  comments : [{
    text: "exercicio 3 projeto final",
    date_comment: "09/12/2015",
    files :[{
      path: "github/projetoFinal",
      weight: "234",
      name: "projeto final mongodb"
    }],
    members: [
      {"user_id": ObjectId("5666e38a3aa0ba636df3f866"),type_member: "Desenvolvedor",notify: true}
    ]
  }]
}])

Inserted 1 record(s) in 8ms
BulkWriteResult({
  "writeErrors": [ ],
  "writeConcernErrors": [ ],
  "nInserted": 2,
  "nUpserted": 0,
  "nMatched": 0,
  "nModified": 0,
  "nRemoved": 0,
  "upserted": [ ]
})

Air-de-Diego(mongod-3.0.7) be-mean-final> db.projects.insert(
{
  name : "PROJECT4 - BE-MEAN-Instagram",
  description: "Projeto Curso Be MEan Instagram",
  date_begin: "09/12/2015",
  date_dream: "01/02/2016" ,
  date_end: "",
  visible: true,
  realocate: false,
  expired: false,
  vizualizable_mod: false,
  tags: ["computacao", "javascript","mean"],
  members: [
    {"user_id": ObjectId("5666e2f83aa0ba636df3f863"),type_member: "Analista",notify: true},
    {"user_id": ObjectId("5666e2033aa0ba636df3f861"),type_member: "Tecnico",notify: true},
    {"user_id": ObjectId("5666e2033aa0ba636df3f860"),type_member: "Projetista",notify: true},
    {"user_id": ObjectId("5666e38a3aa0ba636df3f866"),type_member: "Desenvolvedor",notify: true},
    {"user_id": ObjectId("5666e2f83aa0ba636df3f864"),type_member: "Programador",notify: true}
  ],
  goals : [{
    name: "Projeto Final",
    description: "Projeto Final mongoDB",
    date_begin: "10/12/2015",
    date_dream: "01/01/2016" ,
    date_end: "",
    realocate: false,
    expired: false,
    date_realocate : "",
    tags : ["mongodb", "retrive", "sharding"],
    activities : [{"activity_id": ObjectId("566829d7d515fc7740146439")},{"activity_id": ObjectId("566829d7d515fc7740146438")}]
  }]
})

Inserted 1 record(s) in 12ms
WriteResult({
  "nInserted": 1
})
```

###### Projeto 5:
```
Air-de-Diego(mongod-3.0.7) be-mean-final> db.activities.insert([
{
  name: "TDD",
  description: "Projeto TDD",
  date_begin: "10/12/2015",
  date_dream: "10/12/2015",
  date_end: "10/12/2015",
  realocate: false,
  expired: false,
  date_realocate: "",
  members: [
    {"user_id": ObjectId("5666e2f83aa0ba636df3f863"),type_member: "Analista",notify: true}
  ],
  tags: ["ruby", "rails","tdd"],
  comments : [{
    text: "Documentacao",
    date_comment: "09/12/2015",
    files :[{
      path: "github/projetorails",
      weight: "234",
      name: "projeto Documentacao TDD"
    }],
    members: [
      {"user_id": ObjectId("5666e2f83aa0ba636df3f863"),type_member: "Analista",notify: true}
    ]
  }]
},
{
  name: "Design",
  description: "realizar design",
  date_begin: "10/12/2015",
  date_dream: "10/12/2015",
  date_end: "10/12/2015",
  realocate: false,
  expired: false,
  date_realocate: "",
  members: [
    {"user_id": ObjectId("5666e38a3aa0ba636df3f866"),type_member: "Desenvolvedor",notify: true}
  ],
  tags: ["design", "rails","template"],
  comments : []
}])

Inserted 1 record(s) in 2ms
BulkWriteResult({
  "writeErrors": [ ],
  "writeConcernErrors": [ ],
  "nInserted": 2,
  "nUpserted": 0,
  "nMatched": 0,
  "nModified": 0,
  "nRemoved": 0,
  "upserted": [ ]
})

Air-de-Diego(mongod-3.0.7) be-mean-final> db.projects.insert(
{
  name : "PROJECT 5 - RUBY ON RAILS",
  description: "Projeto Ruby on Rails",
  date_begin: "09/12/2015",
  date_dream: "01/02/2016" ,
  date_end: "",
  visible: true,
  realocate: false,
  expired: false,
  vizualizable_mod: false,
  tags: ["computacao", "ruby","rubyonrails"],
  members: [
    {"user_id": ObjectId("5666e2f83aa0ba636df3f863"),type_member: "Analista",notify: true},
    {"user_id": ObjectId("5666e2033aa0ba636df3f861"),type_member: "Tecnico",notify: true},
    {"user_id": ObjectId("5666e38a3aa0ba636df3f866"),type_member: "Desenvolvedor",notify: true},
    {"user_id": ObjectId("5666e2f83aa0ba636df3f864"),type_member: "Programador",notify: true},
    {"user_id": ObjectId("5666e38a3aa0ba636df3f865"),type_member: "Estagiario",notify: true}
  ],
  goals : [{
    name: "Projeto Rails",
    description: "Projeto ROR",
    date_begin: "10/12/2015",
    date_dream: "01/01/2016" ,
    date_end: "",
    realocate: false,
    expired: false,
    date_realocate : "",
    tags : ["ruby", "rails", "tdd"],
    activities : [{"activity_id": ObjectId("56682bb1d515fc774014643c")},{"activity_id": ObjectId("56682bb1d515fc774014643b")}]
  }]
})

Inserted 1 record(s) in 2ms
WriteResult({
  "nInserted": 1
})
```
## Retrieve - busca
###### 1 - Liste as informações dos membros de 1 projeto específico que deve ser buscado pelo seu nome de forma a não ligar para maiúsculas e minúsculas

```
Air-de-Diego(mongod-3.0.7) be-mean-final> var users = [];
Air-de-Diego(mongod-3.0.7) be-mean-final> var getUsers = function(id){ users.push(db.users.findOne()) }
Air-de-Diego(mongod-3.0.7) be-mean-final> var projects = db.projects.findOne({name:/projeto 1/i})
Air-de-Diego(mongod-3.0.7) be-mean-final> projects.members.forEach(getUsers)
Air-de-Diego(mongod-3.0.7) be-mean-final> projects
{
  "_id": ObjectId("56681fc9d515fc7740146433"),
  "name": "PROJECT 1 - ENGENHARIA CIVIL",
  "description": "Projeto 1 ",
  "date_begin": "09/12/2015",
  "date_dream": "01/02/2016",
  "date_end": "",
  "visible": true,
  "realocate": false,
  "expired": false,
  "vizualizable_mod": false,
  "tags": [
    "engenharia",
    "civil",
    "projetocad"
  ],
  "members": [
    {
      "_id": ObjectId("5666de303aa0ba636df3f85d"),
      "type_member": "Gerente",
      "notify": true
    },
    {
      "_id": ObjectId("5666df133aa0ba636df3f85e"),
      "type_member": "Engenheiro chefe",
      "notify": true
    },
    {
      "_id": ObjectId("5666e2033aa0ba636df3f85f"),
      "type_member": "Engenheiro tecnico",
      "notify": true
    },
    {
      "_id": ObjectId("5666e2033aa0ba636df3f860"),
      "type_member": "Projetista",
      "notify": true
    },
    {
      "_id": ObjectId("5666e2033aa0ba636df3f861"),
      "type_member": "Tecnico",
      "notify": true
    }
  ],
  "goals": [
    {
      "name": "Projeto CAD Edificio",
      "description": "Construcao projeto cad edificio",
      "date_begin": "10/12/2015",
      "date_dream": "01/01/2016",
      "date_end": "",
      "realocate": false,
      "expired": false,
      "date_realocate": "",
      "tags": [
        "planta",
        "planta3d",
        "cad"
      ],
      "activities": [
        {
          "_id": ObjectId("56681e8cd515fc7740146431")
        },
        {
          "_id": ObjectId("56681e8cd515fc7740146431")
        }
      ]
    }
  ]
}
Air-de-Diego(mongod-3.0.7) be-mean-final> users
[
  {
    "_id": ObjectId("5666de303aa0ba636df3f85d"),
    "name": "user1",
    "bio": "Analista",
    "data_register": "08/12/1025",
    "avatar_path": "http://github.com/avatar/user1",
    "auth": {
      "username": "user1",
      "email": "user1@project.com",
      "password": "user1admin",
      "last_access": "",
      "online": false,
      "disable": false,
      "hash_token": "isdoiauoiuwoiu83793274ksjd893342"
    },
    "background_path": "http://github.com/background-path/user1"
  },
  {
    "_id": ObjectId("5666de303aa0ba636df3f85d"),
    "name": "user1",
    "bio": "Analista",
    "data_register": "08/12/1025",
    "avatar_path": "http://github.com/avatar/user1",
    "auth": {
      "username": "user1",
      "email": "user1@project.com",
      "password": "user1admin",
      "last_access": "",
      "online": false,
      "disable": false,
      "hash_token": "isdoiauoiuwoiu83793274ksjd893342"
    },
    "background_path": "http://github.com/background-path/user1"
  },
  {
    "_id": ObjectId("5666de303aa0ba636df3f85d"),
    "name": "user1",
    "bio": "Analista",
    "data_register": "08/12/1025",
    "avatar_path": "http://github.com/avatar/user1",
    "auth": {
      "username": "user1",
      "email": "user1@project.com",
      "password": "user1admin",
      "last_access": "",
      "online": false,
      "disable": false,
      "hash_token": "isdoiauoiuwoiu83793274ksjd893342"
    },
    "background_path": "http://github.com/background-path/user1"
  },
  {
    "_id": ObjectId("5666de303aa0ba636df3f85d"),
    "name": "user1",
    "bio": "Analista",
    "data_register": "08/12/1025",
    "avatar_path": "http://github.com/avatar/user1",
    "auth": {
      "username": "user1",
      "email": "user1@project.com",
      "password": "user1admin",
      "last_access": "",
      "online": false,
      "disable": false,
      "hash_token": "isdoiauoiuwoiu83793274ksjd893342"
    },
    "background_path": "http://github.com/background-path/user1"
  },
  {
    "_id": ObjectId("5666de303aa0ba636df3f85d"),
    "name": "user1",
    "bio": "Analista",
    "data_register": "08/12/1025",
    "avatar_path": "http://github.com/avatar/user1",
    "auth": {
      "username": "user1",
      "email": "user1@project.com",
      "password": "user1admin",
      "last_access": "",
      "online": false,
      "disable": false,
      "hash_token": "isdoiauoiuwoiu83793274ksjd893342"
    },
    "background_path": "http://github.com/background-path/user1"
  }
]
```


###### 2 - Liste todos os projetos com a tag que você escolheu para os 3 projetos em comum.
```
Air-de-Diego(mongod-3.0.7) be-mean-final> var query = {tags: /engenharia/i}
Air-de-Diego(mongod-3.0.7) be-mean-final> db.projects.find(query)
{
  "_id": ObjectId("56681fc9d515fc7740146433"),
  "name": "PROJECT 1 - ENGENHARIA CIVIL",
  "description": "Projeto 1 ",
  "date_begin": "09/12/2015",
  "date_dream": "01/02/2016",
  "date_end": "",
  "visible": true,
  "realocate": false,
  "expired": false,
  "vizualizable_mod": false,
  "tags": [
    "engenharia",
    "civil",
    "projetocad"
  ],
  "members": [
    {
      "user_id": ObjectId("5666de303aa0ba636df3f85d"),
      "type_member": "Gerente",
      "notify": true
    },
    {
      "user_id": ObjectId("5666df133aa0ba636df3f85e"),
      "type_member": "Engenheiro chefe",
      "notify": true
    },
    {
      "user_id": ObjectId("5666e2033aa0ba636df3f85f"),
      "type_member": "Engenheiro tecnico",
      "notify": true
    },
    {
      "user_id": ObjectId("5666e2033aa0ba636df3f860"),
      "type_member": "Projetista",
      "notify": true
    },
    {
      "user_id": ObjectId("5666e2033aa0ba636df3f861"),
      "type_member": "Tecnico",
      "notify": true
    }
  ],
  "goals": [
    {
      "name": "Projeto CAD Edificio",
      "description": "Construcao projeto cad edificio",
      "date_begin": "10/12/2015",
      "date_dream": "01/01/2016",
      "date_end": "",
      "realocate": false,
      "expired": false,
      "date_realocate": "",
      "tags": [
        "planta",
        "planta3d",
        "cad"
      ],
      "activities": [
        {
          "activity_id": ObjectId("56681e8cd515fc7740146431")
        },
        {
          "activity_id": ObjectId("56681e8cd515fc7740146431")
        }
      ]
    }
  ]
}
{
  "_id": ObjectId("566820e2d515fc7740146434"),
  "name": "PROJECT 2 - ENGENHARIA Eletrica",
  "description": "Projeto Engenharia Eletrica",
  "date_begin": "09/12/2015",
  "date_dream": "01/02/2016",
  "date_end": "",
  "visible": true,
  "realocate": false,
  "expired": false,
  "vizualizable_mod": false,
  "tags": [
    "engenharia",
    "energia",
    "iluminacao"
  ],
  "members": [
    {
      "user_id": ObjectId("5666df133aa0ba636df3f85e"),
      "type_member": "Engenheiro chefe",
      "notify": true
    },
    {
      "user_id": ObjectId("5666e2033aa0ba636df3f85f"),
      "type_member": "Engenheiro tecnico",
      "notify": true
    },
    {
      "user_id": ObjectId("5666e2033aa0ba636df3f860"),
      "type_member": "Projetista",
      "notify": true
    },
    {
      "user_id": ObjectId("5666e2033aa0ba636df3f861"),
      "type_member": "Tecnico",
      "notify": true
    },
    {
      "user_id": ObjectId("5666e2f83aa0ba636df3f862"),
      "type_member": "Engenheiro Eletrico",
      "notify": true
    }
  ],
  "goals": [
    {
      "name": "Implantacao iluminacao urbana",
      "description": "Iluminacao rua x",
      "date_begin": "10/12/2015",
      "date_dream": "01/01/2016",
      "date_end": "",
      "realocate": false,
      "expired": false,
      "date_realocate": "",
      "tags": [
        "energia",
        "postes",
        "iluminacao"
      ],
      "activities": [ ]
    }
  ]
}
{
  "_id": ObjectId("5668238ed515fc7740146437"),
  "name": "PROJECT 2 - ENGENHARIA Mecanica",
  "description": "Projeto Engenharia Mecanica",
  "date_begin": "09/12/2015",
  "date_dream": "01/02/2016",
  "date_end": "",
  "visible": true,
  "realocate": false,
  "expired": false,
  "vizualizable_mod": false,
  "tags": [
    "engenharia",
    "mecanica",
    "motor"
  ],
  "members": [
    {
      "user_id": ObjectId("5666e2033aa0ba636df3f85f"),
      "type_member": "Engenheiro tecnico",
      "notify": true
    },
    {
      "user_id": ObjectId("5666e2033aa0ba636df3f860"),
      "type_member": "Projetista",
      "notify": true
    },
    {
      "user_id": ObjectId("5666e2033aa0ba636df3f861"),
      "type_member": "Tecnico",
      "notify": true
    },
    {
      "user_id": ObjectId("5666e2f83aa0ba636df3f862"),
      "type_member": "Engenheiro Eletrico",
      "notify": true
    },
    {
      "user_id": ObjectId("5666e2f83aa0ba636df3f863"),
      "type_member": "Analista",
      "notify": true
    }
  ],
  "goals": [
    {
      "name": "Retificacao",
      "description": "Retifica motor caminhao fora de estrada",
      "date_begin": "10/12/2015",
      "date_dream": "01/01/2016",
      "date_end": "",
      "realocate": false,
      "expired": false,
      "date_realocate": "",
      "tags": [
        "cabecote",
        "retifica",
        "pistao"
      ],
      "activities": [
        {
          "activity_id": ObjectId("5668231cd515fc7740146435")
        },
        {
          "activity_id": ObjectId("5668231cd515fc7740146436")
        }
      ]
    }
  ]
}
Fetched 3 record(s) in 43ms
```
###### 3 - Liste apenas os nomes de todas as atividades para todos os projetos.
```
Air-de-Diego(mongod-3.0.7) be-mean-final> db.activities.find({},{"_id" : 0, name : 1})
{
  "name": "Projeto Retifica Cabecote"
}
{
  "name": "execucao projeto"
}
{
  "name": "Desmanche motor"
}
{
  "name": "Impressao 3d projeto"
}
{
  "name": "Cadastro Inicial"
}
{
  "name": "Design"
}
{
  "name": "Upate e retrive"
}
{
  "name": "TDD"
}
Fetched 8 record(s) in 3ms
```

###### 4 - Liste todos os projetos que não possuam uma tag.
```
Air-de-Diego(mongod-3.0.7) be-mean-final> db.projects.find({ tags: { $not: { $in: ['engenharia'] } } }, { name: 1, tags: 1 })
{
  "_id": ObjectId("56682bebd515fc774014643d"),
  "name": "PROJECT 5 - RUBY ON RAILS",
  "tags": [
    "computacao",
    "ruby",
    "rubyonrails"
  ]
}
{
  "_id": ObjectId("56682a31d515fc774014643a"),
  "name": "PROJECT4 - BE-MEAN-Instagram",
  "tags": [
    "computacao",
    "javascript",
    "mean"
  ]
}
Fetched 2 record(s) in 1ms
```

###### 5 - Liste todos os usuários que não fazem parte do primeiro projeto cadastrado

```
MacBook-Air-Diego(mongod-3.0.7) be-mean-final> var projeto = db.projects.findOne()
MacBook-Air-Diego(mongod-3.0.7) be-mean-final> var membros = []
MacBook-Air-Diego(mongod-3.0.7) be-mean-final> 
var getMembros = function(member){
  var user = db.users.findOne({ _id: member._id },{_id: 1});
  membros.push(user._id)
}
MacBook-Air-Diego(mongod-3.0.7) be-mean-final> projeto.members.forEach(getMembros)
MacBook-Air-Diego(mongod-3.0.7) be-mean-final> membros
[
  ObjectId("5666de303aa0ba636df3f85d"),
  ObjectId("5666df133aa0ba636df3f85e"),
  ObjectId("5666e2033aa0ba636df3f85f"),
  ObjectId("5666e2033aa0ba636df3f860"),
  ObjectId("5666e2033aa0ba636df3f861")
]
MacBook-Air-Diego(mongod-3.0.7) be-mean-final> db.users.find({ _id: { $not: { $in: membros } } })
{
  "_id": ObjectId("5666e2f83aa0ba636df3f862"),
  "name": "Usuario 06",
  "bio": "usuario 06 bio",
  "data_register": "09/12/1025",
  "avatar_path": "http://github.com/avatar/user6",
  "auth": {
    "username": "user6",
    "email": "user6@project.com",
    "password": "user66666",
    "last_access": "",
    "online": false,
    "disable": false,
    "hash_token": "isdosdf34543iauoiuwoiu83793274ksjd666666"
  },
  "background_path": "http://github.com/background-path/user6"
}
{
  "_id": ObjectId("5666e2f83aa0ba636df3f863"),
  "name": "Usuario 07",
  "bio": "usuario 07 bio",
  "data_register": "07/12/1025",
  "avatar_path": "http://github.com/avatar/user7",
  "auth": {
    "username": "user7",
    "email": "user7@project.com",
    "password": "user77777",
    "last_access": "",
    "online": false,
    "disable": false,
    "hash_token": "isdosdf34543iauoiuwoiu83793274ksjd877777"
  },
  "background_path": "http://github.com/background-path/user7"
}
{
  "_id": ObjectId("5666e2f83aa0ba636df3f864"),
  "name": "Usuario 08",
  "bio": "usuario 08 bio",
  "data_register": "07/12/1025",
  "avatar_path": "http://github.com/avatar/user8",
  "auth": {
    "username": "user8",
    "email": "user8@project.com",
    "password": "user88888",
    "last_access": "",
    "online": false,
    "disable": false,
    "hash_token": "isdosdf34543iauoiuwoiu83793274ksjd89334288888"
  },
  "background_path": "http://github.com/background-path/user8"
}
{
  "_id": ObjectId("5666e38a3aa0ba636df3f865"),
  "name": "Usuario 09",
  "bio": "usuario 09 bio",
  "data_register": "07/12/1025",
  "avatar_path": "http://github.com/avatar/user9",
  "auth": {
    "username": "user9",
    "email": "user9@project.com",
    "password": "user99999",
    "last_access": "",
    "online": false,
    "disable": false,
    "hash_token": "isdosdf34543iauoiuwoiu83793274ksjd89334299999"
  },
  "background_path": "http://github.com/background-path/user9"
}
{
  "_id": ObjectId("5666e38a3aa0ba636df3f866"),
  "name": "Usuario 10",
  "bio": "usuario 10 bio",
  "data_register": "07/12/1025",
  "avatar_path": "http://github.com/avatar/user10",
  "auth": {
    "username": "user10",
    "email": "user10@project.com",
    "password": "user1010101010",
    "last_access": "",
    "online": false,
    "disable": false,
    "hash_token": "isdosdf34543iauoiuwoiu83793274ksjd893342101010101010"
  },
  "background_path": "http://github.com/background-path/user10"
}
Fetched 5 record(s) in 4ms

```

## Update - alteração
###### Adicione para todos os projetos o campo views: 0.
```
MacBook-Air-Diego(mongod-3.0.7) be-mean-final> var query = {}
MacBook-Air-Diego(mongod-3.0.7) be-mean-final> var mod = {$set: {views: 0}}
MacBook-Air-Diego(mongod-3.0.7) be-mean-final> var options = {multi: true}
MacBook-Air-Diego(mongod-3.0.7) be-mean-final> db.projects.update(query,mod,options)
Updated 5 existing record(s) in 2ms
WriteResult({
  "nMatched": 5,
  "nUpserted": 0,
  "nModified": 5
})
MacBook-Air-Diego(mongod-3.0.7) be-mean-final> db.projects.find(query,{views : 1})
{
  "_id": ObjectId("56681fc9d515fc7740146433"),
  "views": 0
}
{
  "_id": ObjectId("566820e2d515fc7740146434"),
  "views": 0
}
{
  "_id": ObjectId("5668238ed515fc7740146437"),
  "views": 0
}
{
  "_id": ObjectId("56682bebd515fc774014643d"),
  "views": 0
}
{
  "_id": ObjectId("56682a31d515fc774014643a"),
  "views": 0
}
Fetched 5 record(s) in 4ms
```
###### Adicione 1 tag diferente para cada projeto.
```
MacBook-Air-Diego(mongod-3.0.7) be-mean-final> var tags = ["tag1","tag2","tag3","tag4","tag5"]
MacBook-Air-Diego(mongod-3.0.7) be-mean-final> var verify = []
MacBook-Air-Diego(mongod-3.0.7) be-mean-final> 
for (var i = 0; i < 5; i++) {
  var project = db.projects.find().skip(i).limit(1).toArray();
  verify.push(project[0]._id, tags[i]);
  db.projects.update({ _id: project[0]._id }, { $push: { tags: tags[i]}});
}
Updated 1 existing record(s) in 2ms
Updated 1 existing record(s) in 1ms
Updated 1 existing record(s) in 1ms
Updated 1 existing record(s) in 1ms
Updated 1 existing record(s) in 2ms

WriteResult({
  "nMatched": 1,
  "nUpserted": 0,
  "nModified": 1
})

MacBook-Air-Diego(mongod-3.0.7) be-mean-final> verify
[
  ObjectId("56681fc9d515fc7740146433"),
    "tag1",
  ObjectId("566820e2d515fc7740146434"),
    "tag2",
  ObjectId("5668238ed515fc7740146437"),
    "tag3",
  ObjectId("56682bebd515fc774014643d"),
    "tag4",
  ObjectId("56682a31d515fc774014643a"),
    "tag5"
]

MacBook-Air-Diego(mongod-3.0.7) be-mean-final> db.projects.find({},{"_id": 1, tags: 1})
{
  "_id": ObjectId("56681fc9d515fc7740146433"),
  "tags": [
    "engenharia",
    "civil",
    "projetocad",
    "tag1"
  ]
}
{
  "_id": ObjectId("566820e2d515fc7740146434"),
  "tags": [
    "engenharia",
    "energia",
    "iluminacao",
    "tag2"
  ]
}
{
  "_id": ObjectId("5668238ed515fc7740146437"),
  "tags": [
    "engenharia",
    "mecanica",
    "motor",
    "tag3"
  ]
}
{
  "_id": ObjectId("56682bebd515fc774014643d"),
  "tags": [
    "computacao",
    "ruby",
    "rubyonrails",
    "tag4"
  ]
}
{
  "_id": ObjectId("56682a31d515fc774014643a"),
  "tags": [
    "computacao",
    "javascript",
    "mean",
    "tag5"
  ]
}
Fetched 5 record(s) in 2ms
```

###### Adicione 2 membros diferentes para cada projeto.

```
MacBook-Air-Diego(mongod-3.0.7) be-mean-final> var x = 5;
MacBook-Air-Diego(mongod-3.0.7) be-mean-final> var y = 0;
MacBook-Air-Diego(mongod-3.0.7) be-mean-final> 
for (var i = 0; i < 5; i++) {
  x --;
  var project = db.projects.find().skip(x).limit(1).toArray();
  var user1 = db.users.find().skip(y).limit(1).toArray();
  y++;
  var user2 = db.users.find().skip(y).limit(1).toArray();
  y++;
  var membro = {
    user_id : user1[0]._id,
    type_member : "adicional 1",
    notify : true
  };
  db.projects.update({ _id: project[0]._id }, { $push: { members: membro1 } });

  var membro2 = {
    user_id : user2[0]._id,
    type_member : "adicional 2",
    notify : true
  };
  db.projects.update({ _id: project[0]._id }, { $push: { members: membro2 } });
}

Updated 1 existing record(s) in 49ms
Updated 1 existing record(s) in 1ms
Updated 1 existing record(s) in 1ms
Updated 1 existing record(s) in 3ms
Updated 1 existing record(s) in 3ms
Updated 1 existing record(s) in 1ms
Updated 1 existing record(s) in 1ms
Updated 1 existing record(s) in 0ms
Updated 1 existing record(s) in 1ms
Updated 1 existing record(s) in 1ms
WriteResult({
  "nMatched": 1,
  "nUpserted": 0,
  "nModified": 1
})
```

###### Adicione 1 comentário em cada atividade, deixe apenas 1 projeto sem.
```
MacBook-Air-Diego(mongod-3.0.7) be-mean-final> var activities = []
MacBook-Air-Diego(mongod-3.0.7) be-mean-final> 
var getActivities = function(activity){
  activities.push(activity)
}
MacBook-Air-Diego(mongod-3.0.7) be-mean-final> db.activities.find({}).toArray().forEach(getActivities)
MacBook-Air-Diego(mongod-3.0.7) be-mean-final> activities.length
8
MacBook-Air-Diego(mongod-3.0.7) be-mean-final> 
var comment = {
  "text": "adicaocomentario",
  "date_comment": "02/01/2016",
  "files": [],
  "members": [
    {
      "user_id": ObjectId("5666e2033aa0ba636df3f861"),
      "type_member": "Tecnico",
      "notify": true
    }
  ]
}

MacBook-Air-Diego(mongod-3.0.7) be-mean-final> 
for (var i = 0; i < activities.length; i++) {
  if (activities[i].comments.length > 0) {
    db.activities.update({_id: activities[i]._id}, {$push: comment})
  }

}
Updated 1 existing record(s) in 4ms
Updated 1 existing record(s) in 2ms
Updated 1 existing record(s) in 2ms
Updated 1 existing record(s) in 0ms
Updated 1 existing record(s) in 1ms
Updated 1 existing record(s) in 1ms
WriteResult({
  "nMatched": 1,
  "nUpserted": 0,
  "nModified": 1
})
```

###### Adicione 1 projeto inteiro com UPSERT.
```
MacBook-Air-Diego(mongod-3.0.7) be-mean-final> var query = {name: /projetoArea51/i}
MacBook-Air-Diego(mongod-3.0.7) be-mean-final> var mod = 
{
  $set: {active : true},
  $setOnInsert:{
    name : "PROJECT AREA 51",
    description: "Projeto ETS",
    date_begin: "01/01/2016",
    date_dream: "01/01/2016" ,
    date_end: "",
    visible: true,
    realocate: false,
    expired: false,
    vizualizable_mod: false,
    tags: [],
    members: [
      {"user_id": ObjectId("5666df133aa0ba636df3f85e"),type_member: "Engenheiro chefe",notify: true}
    ],
    goals : [{
      name: "Caca ET varginha",
      description: "investigar capsula contencao",
      date_begin: "01/01/2016",
      date_dream: "01/01/2016" ,
      date_end: "",
      realocate: false,
      expired: false,
      date_realocate : "",
      tags : ["et", "varginha"],
      activities : []
    }],
    views : 0
  }
}

MacBook-Air-Diego(mongod-3.0.7) be-mean-final> var options = {upsert: true}
MacBook-Air-Diego(mongod-3.0.7) be-mean-final> db.projects.update(query,mod,options)
Updated 1 new record(s) in 13ms
WriteResult({
  "nMatched": 0,
  "nUpserted": 1,
  "nModified": 0,
  "_id": ObjectId("568800e38fe8ff7673cbeb4d")
})
```


## Delete - remoção

###### Apague todos os projetos que não possuam tags.
```
MacBook-Air-Diego(mongod-3.0.7) be-mean-final> var query = {tags: {$size: 0}}
MacBook-Air-Diego(mongod-3.0.7) be-mean-final> db.projects.find(query)
{
  "_id": ObjectId("568800e38fe8ff7673cbeb4d"),
  "active": true,
  "name": "PROJECT AREA 51",
  "description": "Projeto ETS",
  "date_begin": "01/01/2016",
  "date_dream": "01/01/2016",
  "date_end": "",
  "visible": true,
  "realocate": false,
  "expired": false,
  "vizualizable_mod": false,
  "tags": [ ],
  "members": [
    {
      "user_id": ObjectId("5666df133aa0ba636df3f85e"),
      "type_member": "Engenheiro chefe",
      "notify": true
    }
  ],
  "goals": [
    {
      "name": "Caca ET varginha",
      "description": "investigar capsula contencao",
      "date_begin": "01/01/2016",
      "date_dream": "01/01/2016",
      "date_end": "",
      "realocate": false,
      "expired": false,
      "date_realocate": "",
      "tags": [
        "et",
        "varginha"
      ],
      "activities": [ ]
    }
  ],
  "views": 0
}
Fetched 1 record(s) in 7ms
MacBook-Air-Diego(mongod-3.0.7) be-mean-final> db.projects.remove(query)
Removed 1 record(s) in 17ms
WriteResult({
  "nRemoved": 1
})
```
###### Apague todos os projetos que não possuam comentários nas atividades.
```
MacBook-Air-Diego(mongod-3.0.7) be-mean-final> var activities = []
MacBook-Air-Diego(mongod-3.0.7) be-mean-final> var getActivities = function(act){ activities.push(act._id)}
MacBook-Air-Diego(mongod-3.0.7) be-mean-final> var query = {comments : {$size : 0}}
MacBook-Air-Diego(mongod-3.0.7) be-mean-final> db.activities.find(query).toArray().forEach(getActivities)
MacBook-Air-Diego(mongod-3.0.7) be-mean-final> activities
[
  ObjectId("5668231cd515fc7740146436"),
  ObjectId("56682bb1d515fc774014643c")
]
MacBook-Air-Diego(mongod-3.0.7) be-mean-final> var query = {"goals.activities._id" : {$in :activities}}
MacBook-Air-Diego(mongod-3.0.7) be-mean-final> db.projects.find(query)
{
  "_id": ObjectId("56682bebd515fc774014643d"),
  "name": "PROJECT 5 - RUBY ON RAILS",
  "description": "Projeto Ruby on Rails",
  "date_begin": "09/12/2015",
  "date_dream": "01/02/2016",
  "date_end": "",
  "visible": true,
  "realocate": false,
  "expired": false,
  "vizualizable_mod": false,
  "tags": [
    "computacao",
    "ruby",
    "rubyonrails",
    "tag4"
  ],
  "members": [
    {
      "_id": ObjectId("5666e2f83aa0ba636df3f863"),
      "type_member": "Analista",
      "notify": true
    },
    {
      "_id": ObjectId("5666e2033aa0ba636df3f861"),
      "type_member": "Tecnico",
      "notify": true
    },
    {
      "_id": ObjectId("5666e38a3aa0ba636df3f866"),
      "type_member": "Desenvolvedor",
      "notify": true
    },
    {
      "_id": ObjectId("5666e2f83aa0ba636df3f864"),
      "type_member": "Programador",
      "notify": true
    },
    {
      "_id": ObjectId("5666e38a3aa0ba636df3f865"),
      "type_member": "Estagiario",
      "notify": true
    },
    {
      "user_id": ObjectId("5666e2033aa0ba636df3f85f"),
      "type_member": "adicional 1",
      "notify": true
    },
    {
      "user_id": ObjectId("5666e2033aa0ba636df3f860"),
      "type_member": "adicional 2",
      "notify": true
    }
  ],
  "goals": [
    {
      "name": "Projeto Rails",
      "description": "Projeto ROR",
      "date_begin": "10/12/2015",
      "date_dream": "01/01/2016",
      "date_end": "",
      "realocate": false,
      "expired": false,
      "date_realocate": "",
      "tags": [
        "ruby",
        "rails",
        "tdd"
      ],
      "activities": [
        {
          "_id": ObjectId("56682bb1d515fc774014643c")
        },
        {
          "_id": ObjectId("56682bb1d515fc774014643b")
        }
      ]
    }
  ],
  "views": 0
}
{
  "_id": ObjectId("5668238ed515fc7740146437"),
  "name": "PROJECT 2 - ENGENHARIA Mecanica",
  "description": "Projeto Engenharia Mecanica",
  "date_begin": "09/12/2015",
  "date_dream": "01/02/2016",
  "date_end": "",
  "visible": true,
  "realocate": false,
  "expired": false,
  "vizualizable_mod": false,
  "tags": [
    "engenharia",
    "mecanica",
    "motor",
    "tag3"
  ],
  "members": [
    {
      "_id": ObjectId("5666e2033aa0ba636df3f85f"),
      "type_member": "Engenheiro tecnico",
      "notify": true
    },
    {
      "_id": ObjectId("5666e2033aa0ba636df3f860"),
      "type_member": "Projetista",
      "notify": true
    },
    {
      "_id": ObjectId("5666e2033aa0ba636df3f861"),
      "type_member": "Tecnico",
      "notify": true
    },
    {
      "_id": ObjectId("5666e2f83aa0ba636df3f862"),
      "type_member": "Engenheiro Eletrico",
      "notify": true
    },
    {
      "_id": ObjectId("5666e2f83aa0ba636df3f863"),
      "type_member": "Analista",
      "notify": true
    },
    {
      "user_id": ObjectId("5666e2033aa0ba636df3f861"),
      "type_member": "adicional 1",
      "notify": true
    },
    {
      "user_id": ObjectId("5666e2f83aa0ba636df3f862"),
      "type_member": "adicional 2",
      "notify": true
    }
  ],
  "goals": [
    {
      "name": "Retificacao",
      "description": "Retifica motor caminhao fora de estrada",
      "date_begin": "10/12/2015",
      "date_dream": "01/01/2016",
      "date_end": "",
      "realocate": false,
      "expired": false,
      "date_realocate": "",
      "tags": [
        "cabecote",
        "retifica",
        "pistao"
      ],
      "activities": [
        {
          "_id": ObjectId("5668231cd515fc7740146435")
        },
        {
          "_id": ObjectId("5668231cd515fc7740146436")
        }
      ]
    }
  ],
  "views": 0
}
Fetched 2 record(s) in 6ms
MacBook-Air-Diego(mongod-3.0.7) be-mean-final> var options = {multi: true}
MacBook-Air-Diego(mongod-3.0.7) be-mean-final> db.projects.remove(query,options)
Removed 2 record(s) in 2ms
WriteResult({
  "nRemoved": 2
})
```


###### Apague todos os projetos que não possuam atividades.
```
MacBook-Air-Diego(mongod-3.0.7) be-mean-final> var query = {'goals.activities': {$size: 0}}
MacBook-Air-Diego(mongod-3.0.7) be-mean-final> db.projects.find(query)
{
  "_id": ObjectId("566820e2d515fc7740146434"),
  "name": "PROJECT 2 - ENGENHARIA Eletrica",
  "description": "Projeto Engenharia Eletrica",
  "date_begin": "09/12/2015",
  "date_dream": "01/02/2016",
  "date_end": "",
  "visible": true,
  "realocate": false,
  "expired": false,
  "vizualizable_mod": false,
  "tags": [
    "engenharia",
    "energia",
    "iluminacao",
    "tag2"
  ],
  "members": [
    {
      "_id": ObjectId("5666df133aa0ba636df3f85e"),
      "type_member": "Engenheiro chefe",
      "notify": true
    },
    {
      "_id": ObjectId("5666e2033aa0ba636df3f85f"),
      "type_member": "Engenheiro tecnico",
      "notify": true
    },
    {
      "_id": ObjectId("5666e2033aa0ba636df3f860"),
      "type_member": "Projetista",
      "notify": true
    },
    {
      "_id": ObjectId("5666e2033aa0ba636df3f861"),
      "type_member": "Tecnico",
      "notify": true
    },
    {
      "_id": ObjectId("5666e2f83aa0ba636df3f862"),
      "type_member": "Engenheiro Eletrico",
      "notify": true
    },
    {
      "user_id": ObjectId("5666e2f83aa0ba636df3f863"),
      "type_member": "adicional 1",
      "notify": true
    },
    {
      "user_id": ObjectId("5666e2f83aa0ba636df3f864"),
      "type_member": "adicional 2",
      "notify": true
    }
  ],
  "goals": [
    {
      "name": "Implantacao iluminacao urbana",
      "description": "Iluminacao rua x",
      "date_begin": "10/12/2015",
      "date_dream": "01/01/2016",
      "date_end": "",
      "realocate": false,
      "expired": false,
      "date_realocate": "",
      "tags": [
        "energia",
        "postes",
        "iluminacao"
      ],
      "activities": [ ]
    }
  ],
  "views": 0
}
Fetched 1 record(s) in 3ms
MacBook-Air-Diego(mongod-3.0.7) be-mean-final> db.projects.remove(query)
Removed 1 record(s) in 2ms
WriteResult({
  "nRemoved": 1
})
```
###### Escolha 2 usuário e apague todos os projetos em que os 2 fazem parte.
```
MacBook-Air-Diego(mongod-3.0.7) be-mean-final> var users = []
MacBook-Air-Diego(mongod-3.0.7) be-mean-final> var getUser = function(user){users.push(user._id)}
MacBook-Air-Diego(mongod-3.0.7) be-mean-final> db.users.find().skip(2).limit(2).toArray().forEach(getUser)
MacBook-Air-Diego(mongod-3.0.7) be-mean-final> users
[
  ObjectId("5666e2033aa0ba636df3f85f"),
  ObjectId("5666e2033aa0ba636df3f860")
]
MacBook-Air-Diego(mongod-3.0.7) be-mean-final> var options = {multi : true}
MacBook-Air-Diego(mongod-3.0.7) be-mean-final> var query = {"members._id": {$in : users}}
MacBook-Air-Diego(mongod-3.0.7) be-mean-final> db.projects.remove(query,options)
Removed 2 record(s) in 5ms
WriteResult({
  "nRemoved": 2
})
```
###### Apague todos os projetos que possuam uma determinada tag em goal.
```
MacBook-Air-Diego(mongod-3.0.7) be-mean-final> var options = {multi : true}
MacBook-Air-Diego(mongod-3.0.7) be-mean-final> var query = { "goals.tags": { $eq: 'engenharia' } }
MacBook-Air-Diego(mongod-3.0.7) be-mean-final> db.projects.remove(query,options)
MacBook-Air-Diego(mongod-3.0.7) be-mean-final> var options = {multi : true}
MacBook-Air-Diego(mongod-3.0.7) be-mean-final> db.projects.remove(query,options)
Removed 0 record(s) in 2ms
WriteResult({
  "nRemoved": 0
})
```
obs.: Esta query nao removeu nenhum projeto pois todos os projetos já haviam sido excluidos nos exercicios anteriores.

##Gerenciamento de usuários

###### Crie um usuário com permissões APENAS de Leitura.
```
MacBook-Air-Diego(mongod-3.0.7) admin> db.createUser({user : "userRead", pwd: "read", roles: [{role: "read", db: "be-mean-final"}]})
Successfully added user: {
  "user": "userRead",
  "roles": [
    {
      "role": "read",
      "db": "be-mean-final"
    }
  ]
}
```
###### Crie um usuário com permissões de Escrita e Leitura.
```
MacBook-Air-Diego(mongod-3.0.7) admin> db.createUser({user : "userReadWrite", pwd: "readwrite", roles: [{role: "readWrite", db: "be-mean-final"}]})
Successfully added user: {
  "user": "userReadWrite",
  "roles": [
    {
      "role": "readWrite",
      "db": "be-mean-final"
    }
  ]
}
```
###### Adicionar o papel grantRolesToUser e revokeRole para o usuário com Escrita e Leitura.
```
MacBook-Air-Diego(mongod-3.0.7) admin> db.createRole(
{
  role: "grantRolesToUser",
  privileges: [
    { resource: { db: "admin", collection: "" }, actions: [ "grantRole" ] }
  ],
  roles: []
})

MacBook-Air-Diego(mongod-3.0.7) admin> db.createRole(
{
  role: "revokeRole",
  privileges: [
    { resource: { db: "admin", collection: "" }, actions: [ "grantRole" ] }
  ],
  roles: []
})

MacBook-Air-Diego(mongod-3.0.7) admin> db.grantRolesToUser(
"userReadWrite",
  [
    "grantRolesToUser","revokeRole"
  ]
)
```

###### Remover o papel grantRolesToUser para o usuário com Escrita e Leitura.
```
MacBook-Air-Diego(mongod-3.0.7) admin> db.revokeRolesFromUser("userReadWrite",["grantRolesToUser"])
```

###### Listar todos os usuários com seus papéis e ações.
```
MacBook-Air-Diego(mongod-3.0.7) admin> db.runCommand({usersInfo :1})
{
  "users": [
    {
      "_id": "admin.diegoAdmin",
      "user": "diegoAdmin",
      "db": "admin",
      "roles": [
        {
          "role": "userAdminAnyDatabase",
          "db": "admin"
        }
      ]
    },
    {
      "_id": "admin.teste",
      "user": "teste",
      "db": "admin",
      "roles": [
        {
          "role": "read",
          "db": "admin"
        }
      ]
    },
    {
      "_id": "admin.diego",
      "user": "diego",
      "db": "admin",
      "roles": [
        {
          "role": "userAdminAnyDatabase",
          "db": "admin"
        }
      ]
    },
    {
      "_id": "admin.userRead",
      "user": "userRead",
      "db": "admin",
      "roles": [
        {
          "role": "read",
          "db": "be-mean-final"
        }
      ]
    },
    {
      "_id": "admin.userReadWrite",
      "user": "userReadWrite",
      "db": "admin",
      "roles": [
        {
          "role": "revokeRole",
          "db": "admin"
        },
        {
          "role": "readWrite",
          "db": "be-mean-final"
        }
      ]
    }
  ],
  "ok": 1
}
```

##Cluster

#####Depois de criada toda sua base você deverá criar um cluster utilizando:
##### 1 Router
```
MacBook-Air-Diego:~ diego$ mongos --configdb localhost:27010 --port 27011
2016-01-11T21:28:50.403-0200 W SHARDING running with 1 config server should be done only for testing purposes and is not recommended for production
2016-01-11T21:28:50.421-0200 I SHARDING [mongosMain] MongoS version 3.0.7 starting: pid=1396 port=27011 64-bit host=MacBook-Air-Diego (--help for usage)
2016-01-11T21:28:50.421-0200 I CONTROL  [mongosMain] db version v3.0.7
2016-01-11T21:28:50.421-0200 I CONTROL  [mongosMain] git version: nogitversion
2016-01-11T21:28:50.421-0200 I CONTROL  [mongosMain] build info: Darwin elcapitanvm.local 15.0.0 Darwin Kernel Version 15.0.0: Wed Aug 26 16:57:32 PDT 2015; root:xnu-3247.1.106~1/RELEASE_X86_64 x86_64 BOOST_LIB_VERSION=1_49
2016-01-11T21:28:50.421-0200 I CONTROL  [mongosMain] allocator: system
2016-01-11T21:28:50.421-0200 I CONTROL  [mongosMain] options: { net: { port: 27011 }, sharding: { configDB: "localhost:27010" } }
2016-01-11T21:28:50.431-0200 I SHARDING [LockPinger] creating distributed lock ping thread for localhost:27010 and process MacBook-Air-Diego:27011:1452554930:16807 (sleeping for 30000ms)
2016-01-11T21:28:50.551-0200 I SHARDING [LockPinger] cluster localhost:27010 pinged successfully at Mon Jan 11 21:28:50 2016 by distributed lock pinger 'localhost:27010/MacBook-Air-Diego:27011:1452554930:16807', sleeping for 30000ms
2016-01-11T21:28:50.551-0200 I SHARDING [mongosMain] distributed lock 'configUpgrade/MacBook-Air-Diego:27011:1452554930:16807' acquired, ts : 56943ab208785de266c09b6c
2016-01-11T21:28:50.552-0200 I SHARDING [mongosMain] starting upgrade of config server from v0 to v6
2016-01-11T21:28:50.552-0200 I SHARDING [mongosMain] starting next upgrade step from v0 to v6
2016-01-11T21:28:50.553-0200 I SHARDING [mongosMain] about to log new metadata event: { _id: "MacBook-Air-Diego-2016-01-11T23:28:50-56943ab208785de266c09b6d", server: "MacBook-Air-Diego", clientAddr: "N/A", time: new Date(1452554930553), what: "starting upgrade of config database", ns: "config.version", details: { from: 0, to: 6 } }
2016-01-11T21:28:50.566-0200 I SHARDING [mongosMain] writing initial config version at v6
2016-01-11T21:28:50.574-0200 I SHARDING [mongosMain] about to log new metadata event: { _id: "MacBook-Air-Diego-2016-01-11T23:28:50-56943ab208785de266c09b6f", server: "MacBook-Air-Diego", clientAddr: "N/A", time: new Date(1452554930574), what: "finished upgrade of config database", ns: "config.version", details: { from: 0, to: 6 } }
2016-01-11T21:28:50.581-0200 I SHARDING [mongosMain] upgrade of config server to v6 successful
2016-01-11T21:28:50.581-0200 I SHARDING [mongosMain] distributed lock 'configUpgrade/MacBook-Air-Diego:27011:1452554930:16807' unlocked.
2016-01-11T21:28:50.674-0200 I SHARDING [Balancer] about to contact config servers and shards
2016-01-11T21:28:50.674-0200 I SHARDING [Balancer] config servers and shards contacted successfully
2016-01-11T21:28:50.674-0200 I SHARDING [Balancer] balancer id: MacBook-Air-Diego:27011 started at Jan 11 21:28:50
2016-01-11T21:28:50.675-0200 I NETWORK  [mongosMain] waiting for connections on port 27011
```
##### 1 Config Server
```
MacBook-Air-Diego:~ diego$ sudo mongod --configsvr --port 27010
Password:
2016-01-11T21:27:11.138-0200 I JOURNAL  [initandlisten] journal dir=/data/configdb/journal
2016-01-11T21:27:11.139-0200 I JOURNAL  [initandlisten] recover : no journal files present, no recovery needed
2016-01-11T21:27:11.162-0200 I JOURNAL  [durability] Durability thread started
2016-01-11T21:27:11.162-0200 I JOURNAL  [journal writer] Journal writer thread started
2016-01-11T21:27:11.163-0200 I CONTROL  [initandlisten] MongoDB starting : pid=1327 port=27010 dbpath=/data/configdb master=1 64-bit host=MacBook-Air-Diego
2016-01-11T21:27:11.163-0200 I CONTROL  [initandlisten] ** WARNING: You are running this process as the root user, which is not recommended.
2016-01-11T21:27:11.163-0200 I CONTROL  [initandlisten]
2016-01-11T21:27:11.163-0200 I CONTROL  [initandlisten]
2016-01-11T21:27:11.163-0200 I CONTROL  [initandlisten] ** WARNING: soft rlimits too low. Number of files is 256, should be at least 1000
2016-01-11T21:27:11.163-0200 I CONTROL  [initandlisten] db version v3.0.7
2016-01-11T21:27:11.163-0200 I CONTROL  [initandlisten] git version: nogitversion
2016-01-11T21:27:11.163-0200 I CONTROL  [initandlisten] build info: Darwin elcapitanvm.local 15.0.0 Darwin Kernel Version 15.0.0: Wed Aug 26 16:57:32 PDT 2015; root:xnu-3247.1.106~1/RELEASE_X86_64 x86_64 BOOST_LIB_VERSION=1_49
2016-01-11T21:27:11.163-0200 I CONTROL  [initandlisten] allocator: system
2016-01-11T21:27:11.163-0200 I CONTROL  [initandlisten] options: { net: { port: 27010 }, sharding: { clusterRole: "configsvr" } }
2016-01-11T21:27:11.166-0200 I INDEX    [initandlisten] allocating new ns file /data/configdb/local.ns, filling with zeroes...
2016-01-11T21:27:11.211-0200 I STORAGE  [FileAllocator] allocating new datafile /data/configdb/local.0, filling with zeroes...
2016-01-11T21:27:11.211-0200 I STORAGE  [FileAllocator] creating directory /data/configdb/_tmp
2016-01-11T21:27:11.233-0200 I STORAGE  [FileAllocator] done allocating datafile /data/configdb/local.0, size: 16MB,  took 0.021 secs
2016-01-11T21:27:11.272-0200 I REPL     [initandlisten] ******
2016-01-11T21:27:11.273-0200 I REPL     [initandlisten] creating replication oplog of size: 5MB...
2016-01-11T21:27:11.275-0200 I REPL     [initandlisten] ******
2016-01-11T21:27:11.277-0200 I NETWORK  [initandlisten] waiting for connections on port 27010
```
##### 3 Shardings

###### criando Pastas para cada sharding
```
MacBook-Air-Diego:~ diego$ sudo mkdir /data/shard1
MacBook-Air-Diego:~ diego$ sudo mkdir /data/shard2
MacBook-Air-Diego:~ diego$ sudo mkdir /data/shard3
```

###### definindo diretório para cada shardind
```
//shard 1
MacBook-Air-Diego:~ diego$ sudo mongod --port 27012 --dbpath /data/shard1
2016-01-11T21:33:23.085-0200 I JOURNAL  [initandlisten] journal dir=/data/shard1/journal
2016-01-11T21:33:23.086-0200 I JOURNAL  [initandlisten] recover : no journal files present, no recovery needed
2016-01-11T21:33:23.110-0200 I JOURNAL  [durability] Durability thread started
2016-01-11T21:33:23.111-0200 I JOURNAL  [journal writer] Journal writer thread started
2016-01-11T21:33:23.111-0200 I CONTROL  [initandlisten] MongoDB starting : pid=1620 port=27012 dbpath=/data/shard1 64-bit host=MacBook-Air-Diego
2016-01-11T21:33:23.111-0200 I CONTROL  [initandlisten] ** WARNING: You are running this process as the root user, which is not recommended.
2016-01-11T21:33:23.111-0200 I CONTROL  [initandlisten]
2016-01-11T21:33:23.111-0200 I CONTROL  [initandlisten]
2016-01-11T21:33:23.111-0200 I CONTROL  [initandlisten] ** WARNING: soft rlimits too low. Number of files is 256, should be at least 1000
2016-01-11T21:33:23.111-0200 I CONTROL  [initandlisten] db version v3.0.7
2016-01-11T21:33:23.111-0200 I CONTROL  [initandlisten] git version: nogitversion
2016-01-11T21:33:23.111-0200 I CONTROL  [initandlisten] build info: Darwin elcapitanvm.local 15.0.0 Darwin Kernel Version 15.0.0: Wed Aug 26 16:57:32 PDT 2015; root:xnu-3247.1.106~1/RELEASE_X86_64 x86_64 BOOST_LIB_VERSION=1_49
2016-01-11T21:33:23.111-0200 I CONTROL  [initandlisten] allocator: system
2016-01-11T21:33:23.111-0200 I CONTROL  [initandlisten] options: { net: { port: 27012 }, storage: { dbPath: "/data/shard1" } }
2016-01-11T21:33:23.112-0200 I INDEX    [initandlisten] allocating new ns file /data/shard1/local.ns, filling with zeroes...
2016-01-11T21:33:23.156-0200 I STORAGE  [FileAllocator] allocating new datafile /data/shard1/local.0, filling with zeroes...
2016-01-11T21:33:23.156-0200 I STORAGE  [FileAllocator] creating directory /data/shard1/_tmp
2016-01-11T21:33:23.306-0200 I STORAGE  [FileAllocator] done allocating datafile /data/shard1/local.0, size: 64MB,  took 0.15 secs
2016-01-11T21:33:23.341-0200 I NETWORK  [initandlisten] waiting for connections on port 27012

//shard 2
MacBook-Air-Diego:~ diego$ sudo mongod --port 27013 --dbpath /data/shard2
2016-01-11T21:33:41.427-0200 I JOURNAL  [initandlisten] journal dir=/data/shard2/journal
2016-01-11T21:33:41.428-0200 I JOURNAL  [initandlisten] recover : no journal files present, no recovery needed
2016-01-11T21:33:41.454-0200 I JOURNAL  [durability] Durability thread started
2016-01-11T21:33:41.455-0200 I JOURNAL  [journal writer] Journal writer thread started
2016-01-11T21:33:41.455-0200 I CONTROL  [initandlisten] MongoDB starting : pid=1622 port=27013 dbpath=/data/shard2 64-bit host=MacBook-Air-Diego
2016-01-11T21:33:41.455-0200 I CONTROL  [initandlisten] ** WARNING: You are running this process as the root user, which is not recommended.
2016-01-11T21:33:41.455-0200 I CONTROL  [initandlisten]
2016-01-11T21:33:41.455-0200 I CONTROL  [initandlisten]
2016-01-11T21:33:41.455-0200 I CONTROL  [initandlisten] ** WARNING: soft rlimits too low. Number of files is 256, should be at least 1000
2016-01-11T21:33:41.455-0200 I CONTROL  [initandlisten] db version v3.0.7
2016-01-11T21:33:41.455-0200 I CONTROL  [initandlisten] git version: nogitversion
2016-01-11T21:33:41.455-0200 I CONTROL  [initandlisten] build info: Darwin elcapitanvm.local 15.0.0 Darwin Kernel Version 15.0.0: Wed Aug 26 16:57:32 PDT 2015; root:xnu-3247.1.106~1/RELEASE_X86_64 x86_64 BOOST_LIB_VERSION=1_49
2016-01-11T21:33:41.455-0200 I CONTROL  [initandlisten] allocator: system
2016-01-11T21:33:41.455-0200 I CONTROL  [initandlisten] options: { net: { port: 27013 }, storage: { dbPath: "/data/shard2" } }
2016-01-11T21:33:41.456-0200 I INDEX    [initandlisten] allocating new ns file /data/shard2/local.ns, filling with zeroes...
2016-01-11T21:33:41.501-0200 I STORAGE  [FileAllocator] allocating new datafile /data/shard2/local.0, filling with zeroes...
2016-01-11T21:33:41.501-0200 I STORAGE  [FileAllocator] creating directory /data/shard2/_tmp
2016-01-11T21:33:41.639-0200 I STORAGE  [FileAllocator] done allocating datafile /data/shard2/local.0, size: 64MB,  took 0.137 secs
2016-01-11T21:33:41.674-0200 I NETWORK  [initandlisten] waiting for connections on port 27013

//shard 3
MacBook-Air-Diego:~ diego$ sudo mongod --port 27014 --dbpath /data/shard3
2016-01-11T21:34:03.304-0200 I JOURNAL  [initandlisten] journal dir=/data/shard3/journal
2016-01-11T21:34:03.304-0200 I JOURNAL  [initandlisten] recover : no journal files present, no recovery needed
2016-01-11T21:34:03.328-0200 I JOURNAL  [durability] Durability thread started
2016-01-11T21:34:03.329-0200 I JOURNAL  [journal writer] Journal writer thread started
2016-01-11T21:34:03.329-0200 I CONTROL  [initandlisten] MongoDB starting : pid=1626 port=27014 dbpath=/data/shard3 64-bit host=MacBook-Air-Diego
2016-01-11T21:34:03.329-0200 I CONTROL  [initandlisten] ** WARNING: You are running this process as the root user, which is not recommended.
2016-01-11T21:34:03.329-0200 I CONTROL  [initandlisten]
2016-01-11T21:34:03.329-0200 I CONTROL  [initandlisten]
2016-01-11T21:34:03.329-0200 I CONTROL  [initandlisten] ** WARNING: soft rlimits too low. Number of files is 256, should be at least 1000
2016-01-11T21:34:03.329-0200 I CONTROL  [initandlisten] db version v3.0.7
2016-01-11T21:34:03.329-0200 I CONTROL  [initandlisten] git version: nogitversion
2016-01-11T21:34:03.329-0200 I CONTROL  [initandlisten] build info: Darwin elcapitanvm.local 15.0.0 Darwin Kernel Version 15.0.0: Wed Aug 26 16:57:32 PDT 2015; root:xnu-3247.1.106~1/RELEASE_X86_64 x86_64 BOOST_LIB_VERSION=1_49
2016-01-11T21:34:03.329-0200 I CONTROL  [initandlisten] allocator: system
2016-01-11T21:34:03.329-0200 I CONTROL  [initandlisten] options: { net: { port: 27014 }, storage: { dbPath: "/data/shard3" } }
2016-01-11T21:34:03.330-0200 I INDEX    [initandlisten] allocating new ns file /data/shard3/local.ns, filling with zeroes...
2016-01-11T21:34:03.374-0200 I STORAGE  [FileAllocator] allocating new datafile /data/shard3/local.0, filling with zeroes...
2016-01-11T21:34:03.374-0200 I STORAGE  [FileAllocator] creating directory /data/shard3/_tmp
2016-01-11T21:34:03.509-0200 I STORAGE  [FileAllocator] done allocating datafile /data/shard3/local.0, size: 64MB,  took 0.135 secs
2016-01-11T21:34:03.543-0200 I NETWORK  [initandlisten] waiting for connections on port 27014
```

###### Registrando shards no Router
```
MacBook-Air-Diego:~ diego$ mongo --port 27011 --host localhost
MongoDB shell version: 3.0.7
connecting to: localhost:27011/test
Mongo-Hacker 0.0.8
mongos> sh.addShard("localhost:27012")
{
  "shardAdded": "shard0000",
  "ok": 1
}
MacBook-Air-Diego:27011(mongos-3.0.7)[mongos] test> sh.addShard("localhost:27013")
{
  "shardAdded": "shard0001",
  "ok": 1
}
MacBook-Air-Diego:27011(mongos-3.0.7)[mongos] test> sh.addShard("localhost:27014")
{
  "shardAdded": "shard0002",
  "ok": 1
}
```

###### Definir qual database será shardeada
```
MacBook-Air-Diego:27011(mongos-3.0.7)[mongos] test> sh.enableSharding("be-mean-final")
{
  "ok": 1
}

//definindo a coleção que será shardeada
MacBook-Air-Diego:27011(mongos-3.0.7)[mongos] test> sh.shardCollection("be-mean-final.activities",{"_id": 1})
{
  "collectionsharded": "be-mean-final.activities",
  "ok": 1
}
```
##### 3 Replicas

###### Criacao das pastas que receberao os dados das Replicas
```
MacBook-Air-Diego:~ diego$ sudo mkdir /data/rs1
Password:
MacBook-Air-Diego:~ diego$ sudo mkdir /data/rs2
MacBook-Air-Diego:~ diego$ sudo mkdir /data/rs3
```
###### iniciar os processos de inicializacao das réplicas
```
//rs1
MacBook-Air-Diego:~ diego$ sudo mongod --replSet replica_set --port 27017 --dbpath /data/rs1
2016-01-11T21:50:04.574-0200 I JOURNAL  [initandlisten] journal dir=/data/rs1/journal
2016-01-11T21:50:04.575-0200 I JOURNAL  [initandlisten] recover : no journal files present, no recovery needed
2016-01-11T21:50:04.597-0200 I JOURNAL  [durability] Durability thread started
2016-01-11T21:50:04.598-0200 I JOURNAL  [journal writer] Journal writer thread started
2016-01-11T21:50:04.598-0200 I CONTROL  [initandlisten] MongoDB starting : pid=1790 port=27017 dbpath=/data/rs1 64-bit host=MacBook-Air-Diego
2016-01-11T21:50:04.598-0200 I CONTROL  [initandlisten] ** WARNING: You are running this process as the root user, which is not recommended.
2016-01-11T21:50:04.598-0200 I CONTROL  [initandlisten]
2016-01-11T21:50:04.598-0200 I CONTROL  [initandlisten]
2016-01-11T21:50:04.598-0200 I CONTROL  [initandlisten] ** WARNING: soft rlimits too low. Number of files is 256, should be at least 1000
2016-01-11T21:50:04.598-0200 I CONTROL  [initandlisten] db version v3.0.7
2016-01-11T21:50:04.598-0200 I CONTROL  [initandlisten] git version: nogitversion
2016-01-11T21:50:04.598-0200 I CONTROL  [initandlisten] build info: Darwin elcapitanvm.local 15.0.0 Darwin Kernel Version 15.0.0: Wed Aug 26 16:57:32 PDT 2015; root:xnu-3247.1.106~1/RELEASE_X86_64 x86_64 BOOST_LIB_VERSION=1_49
2016-01-11T21:50:04.598-0200 I CONTROL  [initandlisten] allocator: system
2016-01-11T21:50:04.598-0200 I CONTROL  [initandlisten] options: { net: { port: 27017 }, replication: { replSet: "replica_set" }, storage: { dbPath: "/data/rs1" } }
2016-01-11T21:50:04.599-0200 I INDEX    [initandlisten] allocating new ns file /data/rs1/local.ns, filling with zeroes...
2016-01-11T21:50:04.645-0200 I STORAGE  [FileAllocator] allocating new datafile /data/rs1/local.0, filling with zeroes...
2016-01-11T21:50:04.645-0200 I STORAGE  [FileAllocator] creating directory /data/rs1/_tmp
2016-01-11T21:50:04.782-0200 I STORAGE  [FileAllocator] done allocating datafile /data/rs1/local.0, size: 64MB,  took 0.137 secs
2016-01-11T21:50:04.817-0200 I REPL     [initandlisten] Did not find local replica set configuration document at startup;  NoMatchingDocument Did not find replica set configuration document in local.system.replset
2016-01-11T21:50:04.819-0200 I NETWORK  [initandlisten] waiting for connections on port 27017

//rs2
MacBook-Air-Diego:~ diego$ sudo mongod --replSet replica_set --port 27018 --dbpath /data/rs2
2016-01-11T21:50:23.857-0200 I JOURNAL  [initandlisten] journal dir=/data/rs2/journal
2016-01-11T21:50:23.858-0200 I JOURNAL  [initandlisten] recover : no journal files present, no recovery needed
2016-01-11T21:50:23.881-0200 I JOURNAL  [durability] Durability thread started
2016-01-11T21:50:23.881-0200 I JOURNAL  [journal writer] Journal writer thread started
2016-01-11T21:50:23.881-0200 I CONTROL  [initandlisten] MongoDB starting : pid=1860 port=27018 dbpath=/data/rs2 64-bit host=MacBook-Air-Diego
2016-01-11T21:50:23.881-0200 I CONTROL  [initandlisten] ** WARNING: You are running this process as the root user, which is not recommended.
2016-01-11T21:50:23.881-0200 I CONTROL  [initandlisten]
2016-01-11T21:50:23.881-0200 I CONTROL  [initandlisten]
2016-01-11T21:50:23.881-0200 I CONTROL  [initandlisten] ** WARNING: soft rlimits too low. Number of files is 256, should be at least 1000
2016-01-11T21:50:23.881-0200 I CONTROL  [initandlisten] db version v3.0.7
2016-01-11T21:50:23.881-0200 I CONTROL  [initandlisten] git version: nogitversion
2016-01-11T21:50:23.882-0200 I CONTROL  [initandlisten] build info: Darwin elcapitanvm.local 15.0.0 Darwin Kernel Version 15.0.0: Wed Aug 26 16:57:32 PDT 2015; root:xnu-3247.1.106~1/RELEASE_X86_64 x86_64 BOOST_LIB_VERSION=1_49
2016-01-11T21:50:23.882-0200 I CONTROL  [initandlisten] allocator: system
2016-01-11T21:50:23.882-0200 I CONTROL  [initandlisten] options: { net: { port: 27018 }, replication: { replSet: "replica_set" }, storage: { dbPath: "/data/rs2" } }
2016-01-11T21:50:23.882-0200 I INDEX    [initandlisten] allocating new ns file /data/rs2/local.ns, filling with zeroes...
2016-01-11T21:50:23.928-0200 I STORAGE  [FileAllocator] allocating new datafile /data/rs2/local.0, filling with zeroes...
2016-01-11T21:50:23.928-0200 I STORAGE  [FileAllocator] creating directory /data/rs2/_tmp
2016-01-11T21:50:24.067-0200 I STORAGE  [FileAllocator] done allocating datafile /data/rs2/local.0, size: 64MB,  took 0.139 secs
2016-01-11T21:50:24.104-0200 I REPL     [initandlisten] Did not find local replica set configuration document at startup;  NoMatchingDocument Did not find replica set configuration document in local.system.replset
2016-01-11T21:50:24.106-0200 I NETWORK  [initandlisten] waiting for connections on port 27018

//rs3
MacBook-Air-Diego:~ diego$ sudo mongod --replSet replica_set --port 27019 --dbpath /data/rs3
2016-01-11T21:50:36.253-0200 I JOURNAL  [initandlisten] journal dir=/data/rs3/journal
2016-01-11T21:50:36.254-0200 I JOURNAL  [initandlisten] recover : no journal files present, no recovery needed
2016-01-11T21:50:36.277-0200 I JOURNAL  [durability] Durability thread started
2016-01-11T21:50:36.277-0200 I JOURNAL  [journal writer] Journal writer thread started
2016-01-11T21:50:36.277-0200 I CONTROL  [initandlisten] MongoDB starting : pid=1930 port=27019 dbpath=/data/rs3 64-bit host=MacBook-Air-Diego
2016-01-11T21:50:36.277-0200 I CONTROL  [initandlisten] ** WARNING: You are running this process as the root user, which is not recommended.
2016-01-11T21:50:36.277-0200 I CONTROL  [initandlisten]
2016-01-11T21:50:36.277-0200 I CONTROL  [initandlisten]
2016-01-11T21:50:36.277-0200 I CONTROL  [initandlisten] ** WARNING: soft rlimits too low. Number of files is 256, should be at least 1000
2016-01-11T21:50:36.277-0200 I CONTROL  [initandlisten] db version v3.0.7
2016-01-11T21:50:36.277-0200 I CONTROL  [initandlisten] git version: nogitversion
2016-01-11T21:50:36.277-0200 I CONTROL  [initandlisten] build info: Darwin elcapitanvm.local 15.0.0 Darwin Kernel Version 15.0.0: Wed Aug 26 16:57:32 PDT 2015; root:xnu-3247.1.106~1/RELEASE_X86_64 x86_64 BOOST_LIB_VERSION=1_49
2016-01-11T21:50:36.277-0200 I CONTROL  [initandlisten] allocator: system
2016-01-11T21:50:36.277-0200 I CONTROL  [initandlisten] options: { net: { port: 27019 }, replication: { replSet: "replica_set" }, storage: { dbPath: "/data/rs3" } }
2016-01-11T21:50:36.278-0200 I INDEX    [initandlisten] allocating new ns file /data/rs3/local.ns, filling with zeroes...
2016-01-11T21:50:36.324-0200 I STORAGE  [FileAllocator] allocating new datafile /data/rs3/local.0, filling with zeroes...
2016-01-11T21:50:36.324-0200 I STORAGE  [FileAllocator] creating directory /data/rs3/_tmp
2016-01-11T21:50:36.466-0200 I STORAGE  [FileAllocator] done allocating datafile /data/rs3/local.0, size: 64MB,  took 0.142 secs
2016-01-11T21:50:36.501-0200 I REPL     [initandlisten] Did not find local replica set configuration document at startup;  NoMatchingDocument Did not find replica set configuration document in local.system.replset
2016-01-11T21:50:36.502-0200 I NETWORK  [initandlisten] waiting for connections on port 27019
```

###### configurando a replica e iniciando
```
Last login: Mon Jan 11 21:50:28 on ttys001
MacBook-Air-Diego:~ diego$ mongo --port 27017
MongoDB shell version: 3.0.7
connecting to: 127.0.0.1:27017/test
Mongo-Hacker 0.0.8
Server has startup warnings:
2016-01-11T21:50:04.598-0200 I CONTROL  [initandlisten]
2016-01-11T21:50:04.598-0200 I CONTROL  [initandlisten] ** WARNING: soft rlimits too low. Number of files is 256, should be at least 1000
MacBook-Air-Diego(mongod-3.0.7) test> rsconf = 
{
  id: "replica_set",
  members: [
    {_id: 0,host: "127.0.0.1:27017"}
  ]
}
{
  "_id": "replica_set",
  "members": [
    {
      "_id": 0,
      "host": "127.0.0.1:27017"
    }
  ]
}
MacBook-Air-Diego(mongod-3.0.7) test> rs.initiate(rsconf)
{
  "ok": 1
}

//terminal rs1
016-01-11T21:52:57.094-0200 I NETWORK  [initandlisten] connection accepted from 127.0.0.1:49653 #1 (1 connection now open)
2016-01-11T21:54:56.051-0200 I REPL     [conn1] replSetInitiate admin command received from client
2016-01-11T21:54:56.053-0200 I REPL     [conn1] replSet replSetInitiate config object with 1 members parses ok
2016-01-11T21:54:56.056-0200 I REPL     [ReplicationExecutor] New replica set config in use: { _id: "replica_set", version: 1, members: [ { _id: 0, host: "127.0.0.1:27017", arbiterOnly: false, buildIndexes: true, hidden: false, priority: 1.0, tags: {}, slaveDelay: 0, votes: 1 } ], settings: { chainingAllowed: true, heartbeatTimeoutSecs: 10, getLastErrorModes: {}, getLastErrorDefaults: { w: 1, wtimeout: 0 } } }
2016-01-11T21:54:56.056-0200 I REPL     [ReplicationExecutor] This node is 127.0.0.1:27017 in the config
2016-01-11T21:54:56.057-0200 I REPL     [ReplicationExecutor] transition to STARTUP2
2016-01-11T21:54:56.057-0200 I REPL     [conn1] ******
2016-01-11T21:54:56.057-0200 I REPL     [conn1] creating replication oplog of size: 192MB...
2016-01-11T21:54:56.057-0200 I STORAGE  [FileAllocator] allocating new datafile /data/rs1/local.1, filling with zeroes...
2016-01-11T21:54:56.690-0200 I STORAGE  [FileAllocator] done allocating datafile /data/rs1/local.1, size: 256MB,  took 0.632 secs
2016-01-11T21:54:56.719-0200 I REPL     [conn1] ******
2016-01-11T21:54:56.719-0200 I REPL     [conn1] Starting replication applier threads
2016-01-11T21:54:56.719-0200 I COMMAND  [conn1] command admin.$cmd command: replSetInitiate { replSetInitiate: { _id: "replica_set", members: [ { _id: 0.0, host: "127.0.0.1:27017" } ] } } keyUpdates:0 writeConflicts:0 numYields:0 reslen:37 locks:{ Global: { acquireCount: { r: 5, w: 3, W: 2 } }, MMAPV1Journal: { acquireCount: { w: 7 }, acquireWaitCount: { w: 1 }, timeAcquiringMicros: { w: 141 } }, Database: { acquireCount: { w: 1, W: 2 } }, Metadata: { acquireCount: { W: 3 } }, oplog: { acquireCount: { w: 1 } } } 671ms
2016-01-11T21:54:56.719-0200 I REPL     [ReplicationExecutor] transition to RECOVERING
2016-01-11T21:54:56.721-0200 I REPL     [ReplicationExecutor] transition to SECONDARY
2016-01-11T21:54:56.721-0200 I REPL     [ReplicationExecutor] transition to PRIMARY
2016-01-11T21:54:57.726-0200 I REPL     [rsSync] transition to primary complete; database writes are now permitted
```

###### Adicionando réplicas
```
MacBook-Air-Diego(mongod-3.0.7)[PRIMARY:replica_set] test> rs.add("127.0.0.1:27018")
{
  "ok": 1
}
MacBook-Air-Diego(mongod-3.0.7)[PRIMARY:replica_set] test> rs.add("127.0.0.1:27019")
{
  "ok": 1
}
```

###### conectando as outras réplicas
```
//rs2
MacBook-Air-Diego:~ diego$ mongo --port 27018
MongoDB shell version: 3.0.7
connecting to: 127.0.0.1:27018/test
Mongo-Hacker 0.0.8
Server has startup warnings:
2016-01-11T21:50:23.881-0200 I CONTROL  [initandlisten] ** WARNING: You are running this process as the root user, which is not recommended.
2016-01-11T21:50:23.881-0200 I CONTROL  [initandlisten]
2016-01-11T21:50:23.881-0200 I CONTROL  [initandlisten]
2016-01-11T21:50:23.881-0200 I CONTROL  [initandlisten] ** WARNING: soft rlimits too low. Number of files is 256, should be at least 1000
MacBook-Air-Diego:27018(mongod-3.0.7)[SECONDARY:replica_set] test>

//rs3
MacBook-Air-Diego:~ diego$ mongo --port 27019
MongoDB shell version: 3.0.7
connecting to: 127.0.0.1:27019/test
Mongo-Hacker 0.0.8
Server has startup warnings:
2016-01-11T21:50:36.277-0200 I CONTROL  [initandlisten] ** WARNING: You are running this process as the root user, which is not recommended.
2016-01-11T21:50:36.277-0200 I CONTROL  [initandlisten]
2016-01-11T21:50:36.277-0200 I CONTROL  [initandlisten]
2016-01-11T21:50:36.277-0200 I CONTROL  [initandlisten] ** WARNING: soft rlimits too low. Number of files is 256, should be at least 1000
MacBook-Air-Diego:27019(mongod-3.0.7)[SECONDARY:replica_set] test>
```

###### status das réplicas
```
MacBook-Air-Diego(mongod-3.0.7)[PRIMARY:replica_set] test> rs.status()
{
  "set": "replica_set",
  "date": ISODate("2016-01-12T00:02:06.550Z"),
  "myState": 1,
  "members": [
    {
      "_id": 0,
      "name": "127.0.0.1:27017",
      "health": 1,
      "state": 1,
      "stateStr": "PRIMARY",
      "uptime": 722,
      "optime": Timestamp(1452556760, 1),
      "optimeDate": ISODate("2016-01-11T23:59:20Z"),
      "electionTime": Timestamp(1452556496, 2),
      "electionDate": ISODate("2016-01-11T23:54:56Z"),
      "configVersion": 3,
      "self": true
    },
    {
      "_id": 1,
      "name": "127.0.0.1:27018",
      "health": 1,
      "state": 2,
      "stateStr": "SECONDARY",
      "uptime": 171,
      "optime": Timestamp(1452556760, 1),
      "optimeDate": ISODate("2016-01-11T23:59:20Z"),
      "lastHeartbeat": ISODate("2016-01-12T00:02:04.780Z"),
      "lastHeartbeatRecv": ISODate("2016-01-12T00:02:04.856Z"),
      "pingMs": 0,
      "configVersion": 3
    },
    {
      "_id": 2,
      "name": "127.0.0.1:27019",
      "health": 1,
      "state": 2,
      "stateStr": "SECONDARY",
      "uptime": 165,
      "optime": Timestamp(1452556760, 1),
      "optimeDate": ISODate("2016-01-11T23:59:20Z"),
      "lastHeartbeat": ISODate("2016-01-12T00:02:04.780Z"),
      "lastHeartbeatRecv": ISODate("2016-01-12T00:02:04.780Z"),
      "pingMs": 0,
      "configVersion": 3
    }
  ],
  "ok": 1
}
```

###### definindo o árbitro
```
// criando pasta do arbitro
MacBook-Air-Diego:~ diego$ sudo mkdir /data/arb
```

###### levantando/inicializando o árbitro
```
acBook-Air-Diego:~ diego$ sudo mongod --port 30000 --dbpath /data/arb --replSet replica_set
2016-01-11T22:12:23.067-0200 I JOURNAL  [initandlisten] journal dir=/data/arb/journal
2016-01-11T22:12:23.068-0200 I JOURNAL  [initandlisten] recover : no journal files present, no recovery needed
2016-01-11T22:12:23.090-0200 I JOURNAL  [durability] Durability thread started
2016-01-11T22:12:23.090-0200 I JOURNAL  [journal writer] Journal writer thread started
2016-01-11T22:12:23.090-0200 I CONTROL  [initandlisten] MongoDB starting : pid=2271 port=30000 dbpath=/data/arb 64-bit host=MacBook-Air-Diego
2016-01-11T22:12:23.090-0200 I CONTROL  [initandlisten] ** WARNING: You are running this process as the root user, which is not recommended.
2016-01-11T22:12:23.091-0200 I CONTROL  [initandlisten]
2016-01-11T22:12:23.091-0200 I CONTROL  [initandlisten]
2016-01-11T22:12:23.091-0200 I CONTROL  [initandlisten] ** WARNING: soft rlimits too low. Number of files is 256, should be at least 1000
2016-01-11T22:12:23.091-0200 I CONTROL  [initandlisten] db version v3.0.7
2016-01-11T22:12:23.091-0200 I CONTROL  [initandlisten] git version: nogitversion
2016-01-11T22:12:23.091-0200 I CONTROL  [initandlisten] build info: Darwin elcapitanvm.local 15.0.0 Darwin Kernel Version 15.0.0: Wed Aug 26 16:57:32 PDT 2015; root:xnu-3247.1.106~1/RELEASE_X86_64 x86_64 BOOST_LIB_VERSION=1_49
2016-01-11T22:12:23.091-0200 I CONTROL  [initandlisten] allocator: system
2016-01-11T22:12:23.091-0200 I CONTROL  [initandlisten] options: { net: { port: 30000 }, replication: { replSet: "replica_set" }, storage: { dbPath: "/data/arb" } }
2016-01-11T22:12:23.091-0200 I INDEX    [initandlisten] allocating new ns file /data/arb/local.ns, filling with zeroes...
2016-01-11T22:12:23.137-0200 I STORAGE  [FileAllocator] allocating new datafile /data/arb/local.0, filling with zeroes...
2016-01-11T22:12:23.137-0200 I STORAGE  [FileAllocator] creating directory /data/arb/_tmp
2016-01-11T22:12:23.276-0200 I STORAGE  [FileAllocator] done allocating datafile /data/arb/local.0, size: 64MB,  took 0.139 secs
2016-01-11T22:12:23.311-0200 I REPL     [initandlisten] Did not find local replica set configuration document at startup;  NoMatchingDocument Did not find replica set configuration document in local.system.replset
2016-01-11T22:12:23.313-0200 I NETWORK  [initandlisten] waiting for connections on port 30000
```

###### adicionando o árbitro a réplica primária
```
MacBook-Air-Diego(mongod-3.0.7)[PRIMARY:replica_set] test> rs.addArb("127.0.0.1:30000")
{
  "ok": 1
}
```
