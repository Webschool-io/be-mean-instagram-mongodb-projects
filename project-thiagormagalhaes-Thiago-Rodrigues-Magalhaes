# MongoDb - Projeto Final
**Autor:** Thiago Rodrigues Magalhães
**Data** 1457961660047

## Para qual sistema você usaria o MongoDB (diferente desse)?
Acredito que o Mongo possa ser usado para qualquer tipo sistemas, entretanto, em sistemas com grande volume de dados o Mongo pode ter um destaque maior, devido a facilidade no manuseio dos dados e ao seu desempenho em relação ao tempo.

## Qual a modelagem da sua coleção de `users`?
```js
User: {
  name: String,
  bio: String,
  date-register: Date,
  avatar-path: String,
  
  auth:
  [
    username: String,
    email: String,
    password: String,
    last-acess: Date,
    online: Boolean,
    disable: Boolean,
    hash-token: String
  ],

  background-path: String
}

```

## Qual a modelagem da sua coleção de `projects`?
``` js
Project: {
  name: String,
  description: String,
  date_begin: Date,
  date_dream: Date,
  date_end: Date,
  visible: Boolean,
  realocate: Boolean,
  expired: Boolean,
  visualizable_mod: Boolean,

  tag: [],

  members:
  [{
    user_id: String,
    type-member: String
  }],

  goal:
  [
    name: String,
    description: String,
    date_begin: Date,
    date_dream: Date,
    date_end: Date,
    realocate: Boolean,
    expired: Date,

    historic: [],

    activity:
    [{
      activity_id: String
    }]
  ]
}
``` 

## Qual a modelagem da sua coleção retirada de `projects`?
```js
activity {
  name: String,
  description: String,
  date_begin: Date,
  date_dream: Date,
  date_end: Date,
  realocate: Boolean,
  expired: Boolean,

  historic: [],

  members:
  [{
    user_id: String,
    notify: Boolean,
    type_member: String
  }],

  historic: [],

  comment:
  [{
    text: String,
    date_comment: Date,

    file:
    [{
      path: String,
      weight: Number,
      name: String
    }],

    members_comment:
    [{
      user_id: String,
      notify: Boolean,
      type_member: String
    }]

  }]
}
```

## Create - cadastro

### 1. Cadastre 10 usuários diferentes.
```js
// 1
var user = {
 name: 'Clark Joseph Kent',
 bio: 'Bio of Clark',
 data_register: new Date(),
 avatar_path: 'img/avatares/61ae0d965d185e6befea1d213e0730a7.jpg',
 auth: {
     username: 'superman',
     email: 'superman@email.com',
     password: 'ecb1d4ce2c43c60fbdf74a70648cf020',
     last_access: new Date(),
     online: true,
     disabled: false,
     hash_token: '61ae0d965d185e6befea1d213e0730a7',
 },
 background_path: 'img/backgrounds/61ae0d965d185e6befea1d213e0730a7.jpg',
};
db.users.save(user);

Inserted 1 record(s) in 9ms
WriteResult({
  "nInserted": 1
})


// 2
var user = {
 name: 'Bruce Wayne',
 bio: 'Bio of Bruce',
 data_register: new Date(),
 avatar_path: 'img/avatares/66b2f5c3bc465b507dbe10e11442775d.jpg',
 auth: {
     username: 'batman',
     email: 'batman@email.com',
     password: 'b51a4ad33cbd2c22f8196ae4857337b2',
     last_access: new Date(),
     online: true,
     disabled: false,
     hash_token: '66b2f5c3bc465b507dbe10e11442775d',
 },
 background_path: 'img/backgrounds/66b2f5c3bc465b507dbe10e11442775d.jpg',
};
db.users.save(user);

Inserted 1 record(s) in 5ms
WriteResult({
  "nInserted": 1
})


// 3
var user = {
 name: 'Peter Benjamin Parker',
 bio: 'Bio of Peter',
 data_register: new Date(),
 avatar_path: 'img/avatares/f3743bbb098c026b3aa3d014f1e565ff.jpg',
 auth: {
     username: 'homemaranha',
     email: 'homemaranha@email.com',
     password: '72bcdb21fc05b1d14e87023bc230a9ad',
     last_access: new Date(),
     online: true,
     disabled: false,
     hash_token: 'f3743bbb098c026b3aa3d014f1e565ff',
 },
 background_path: 'img/backgrounds/f3743bbb098c026b3aa3d014f1e565ff.jpg',
};
db.users.save(user);

Inserted 1 record(s) in 3ms
WriteResult({
  "nInserted": 1
})


// 4
var user = {
 name: 'James Logan Howlett',
 bio: 'Bio of Logan',
 data_register: new Date(),
 avatar_path: 'img/avatares/f9b4dcaace4e860601abc7286f0306d8.jpg',
 auth: {
     username: 'wolverine',
     email: 'wolverine@email.com',
     password: '0a444c5b89257fccec677cb390f59690',
     last_access: new Date(),
     online: true,
     disabled: false,
     hash_token: 'f9b4dcaace4e860601abc7286f0306d8',
 },
 background_path: 'img/backgrounds/f9b4dcaace4e860601abc7286f0306d8.jpg',
};
db.users.save(user);

Inserted 1 record(s) in 2ms
WriteResult({
  "nInserted": 1
})


// 5
var user = {
 name: 'Princesa Diana de Themyscira',
 bio: 'Bio of Diana',
 data_register: new Date(),
 avatar_path: 'img/avatares/d6b71d6b09d368db0ceb722e09ccbeab.jpg',
 auth: {
     username: 'mulhermaravilha',
     email: 'mulhermaravilha@email.com',
     password: '40683c1f95e8f5313ab764f57f7f526c',
     last_access: new Date(),
     online: true,
     disabled: false,
     hash_token: 'd6b71d6b09d368db0ceb722e09ccbeab',
 },
 background_path: 'img/backgrounds/d6b71d6b09d368db0ceb722e09ccbeab.jpg',
};
db.users.save(user);

Inserted 1 record(s) in 3ms
WriteResult({
  "nInserted": 1
})

// 6
var user = {
 name: 'Steve Rogers',
 bio: 'Bio of Rogers',
 data_register: new Date(),
 avatar_path: 'img/avatares/fa2e3942176c4e7c4c687a5b915fbcaa.jpg',
 auth: {
     username: 'capitaoamerica',
     email: 'capitaoamerica@email.com',
     password: '35d1321e6854115cf82034f7b7d0efae',
     last_access: new Date(),
     online: true,
     disabled: false,
     hash_token: 'fa2e3942176c4e7c4c687a5b915fbcaa',
 },
 background_path: 'img/backgrounds/fa2e3942176c4e7c4c687a5b915fbcaa.jpg',
};
db.users.save(user);

Inserted 1 record(s) in 5ms
WriteResult({
  "nInserted": 1
})

// 7
var user = {
 name: 'John Stewart',
 bio: 'Bio of John',
 data_register: new Date(),
 avatar_path: 'img/avatares/42cfd85a880905fa8b680d0b53fdf0c1.jpg',
 auth: {
     username: 'lanternaverde',
     email: 'lanternaverde@email.com',
     password: '39e06a1bf214e2ab696488c8d296019f',
     last_access: new Date(),
     online: true,
     disabled: false,
     hash_token: '42cfd85a880905fa8b680d0b53fdf0c1',
 },
 background_path: 'img/backgrounds/42cfd85a880905fa8b680d0b53fdf0c1.jpg',
};
db.users.save(user);

Inserted 1 record(s) in 4ms
WriteResult({
  "nInserted": 1
})

// 8
var user = {
 name: 'Barry Allen',
 bio: 'Bio of Barry',
 data_register: new Date(),
 avatar_path: 'img/avatares/dc9305e4445660632fdfdc5ec9e3490f.jpg',
 auth: {
     username: 'flash',
     email: 'flash@email.com',
     password: 'a9c04403aed6d8f1a1602d4f2e4d05bc',
     last_access: new Date(),
     online: true,
     disabled: false,
     hash_token: 'dc9305e4445660632fdfdc5ec9e3490f',
 },
 background_path: 'img/backgrounds/dc9305e4445660632fdfdc5ec9e3490f.jpg',
};
db.users.save(user);

Inserted 1 record(s) in 3ms
WriteResult({
  "nInserted": 1
})

// 9
var user = {
 name: 'Robert Bruce Banner',
 bio: 'Bio of Banner',
 data_register: new Date(),
 avatar_path: 'img/avatares/ac20d9140e6f12975dfe021875ef1322.jpg',
 auth: {
     username: 'hulk',
     email: 'hulk@email.com',
     password: '9ddbc04c5ac2ca10ce94f59e270a7212',
     last_access: new Date(),
     online: true,
     disabled: false,
     hash_token: 'ac20d9140e6f12975dfe021875ef1322',
 },
 background_path: 'img/backgrounds/ac20d9140e6f12975dfe021875ef1322.jpg',
};
db.users.save(user);

Inserted 1 record(s) in 2ms
WriteResult({
  "nInserted": 1
})

// 10
var user = {
 name: 'Matthew Michael Murdock',
 bio: 'Bio of Matt',
 data_register: new Date(),
 avatar_path: 'img/avatares/5c196ce1149ace6d900e4c9ee2205b4f.jpg',
 auth: {
     username: 'demolidor',
     email: 'demolidor@email.com',
     password: '4fa5b823a1646b170a6683ce833bdc1d',
     last_access: new Date(),
     online: true,
     disabled: false,
     hash_token: '5c196ce1149ace6d900e4c9ee2205b4f',
 },
 background_path: 'img/backgrounds/5c196ce1149ace6d900e4c9ee2205b4f.jpg',
};
db.users.save(user);

Inserted 1 record(s) in 2ms
WriteResult({
  "nInserted": 1
})
```
### 2. Cadastre 10 usuários diferentes.
``` js
// Projeto 1
var activities = {
  name: "Limite",
  description: "Técnicas de cálculo de limite",
  date_begin: new Date(),
  date_dream: new Date(),
  date_end: new Date(),
  realocate: false,
  expired: false,
  historic: [],
  members: [
    {"user_id": ObjectId("56e6dd219c0bb73f24b84ae4"), type_member: "Estudante", notify: true},
    {"user_id": ObjectId("56e7450fca76b61efa435c1d"), type_member: "Estudante", notify: true},
    {"user_id": ObjectId("56e7461eca76b61efa435c1e"), type_member: "Estudante", notify: true}
  ],
  tags: ["exemplos", "limite", "matéria"],
  comments : [{
    text: "Exercício 01",
    date_comment: new Date(),
    files :[{
      path: "teste/exemplos01",
      weight: 5,
      name: "Primeira Atividade"
    }],
    members: [
      {"user_id": ObjectId("56e6db209c0bb73f24b84ae3"), notify: true, type_member: "Professor"}
    ]
  }]
};

db.activities.save(activities);

Inserted 1 record(s) in 243ms
WriteResult({
  "nInserted": 1
})

var activities = {
  name: "Derivadas",
  description: "Técnicas de cálculo de derivadas",
  date_begin: new Date(),
  date_dream: new Date(),
  date_end: new Date(),
  realocate: false,
  expired: false,
  historic: [],
  members: [
    {"user_id": ObjectId("56e6dd219c0bb73f24b84ae4"), type_member: "Estudante", notify: true},
    {"user_id": ObjectId("56e7450fca76b61efa435c1d"), type_member: "Estudante", notify: true},
    {"user_id": ObjectId("56e7461eca76b61efa435c1e"), type_member: "Estudante", notify: true}
  ],
  tags: ["exemplos", "derivadas", "matéria"],
  comments : [{
    text: "Exercécio 02",
    date_comment: new Date(),
    files :[{
      path: "teste/exemplos02",
      weight: 5,
      name: "Segunda Atividade"
    }],
    members: [
      {"user_id": ObjectId("56e6db209c0bb73f24b84ae3"), notify: true, type_member: "Professor"}
    ]
  }]
};

db.activities.save(activities);

Inserted 1 record(s) in 4ms
WriteResult({
  "nInserted": 1
})

var project = {
  name : "CÁCULO I",
  description: "Semestre 1",
  date_begin: new Date(),
  date_dream: new Date(),
  date_end: new Date(),
  visible: true,
  realocate: false,
  expired: false,
  vizualizable_mod: false,
  tags: ["matemática", "cálculo","exatas"],
  members: [
    {"user_id": ObjectId("56e6db209c0bb73f24b84ae3"), type_member: "Professor", notify: true},
    {"user_id": ObjectId("56e6dd219c0bb73f24b84ae4"), type_member: "Estudante", notify: true},
    {"user_id": ObjectId("56e7416fca76b61efa435c1c"), type_member: "Monitor", notify: true},
    {"user_id": ObjectId("56e7450fca76b61efa435c1d"), type_member: "Estudante", notify: true},
    {"user_id": ObjectId("56e7461eca76b61efa435c1e"), type_member: "Estudante", notify: true}
  ],
  goals : [{
    name: "Limites e Derivadas",
    description: "Técnicas e Exercícios sobre Limites e Derivadas",
    date_begin: new Date(),
    date_dream: new Date(),
    date_end: new Date(),
    realocate: false,
    expired: false,
    historic: [],
    tags : ["matéria", "limite", "derivada"],
    activities : [{"activity_id": ObjectId("56e7621cca76b61efa435c24")},{"activity_id": ObjectId("56e7627aca76b61efa435c25")}]
  }]
};

db.projects.save(project);

Inserted 1 record(s) in 337ms
WriteResult({
  "nInserted": 1
})

// Projeto 2
var activities = {
  name: "Variáveis",
  description: "Declaração e atribuição de variáveis",
  date_begin: new Date(),
  date_dream: new Date(),
  date_end: new Date(),
  realocate: false,
  expired: false,
  historic: [],
  members: [
    {"user_id": ObjectId("56e7450fca76b61efa435c1d"), type_member: "Estudante", notify: true},
    {"user_id": ObjectId("56e7461eca76b61efa435c1e"), type_member: "Estudante", notify: true},
    {"user_id": ObjectId("56e746e5ca76b61efa435c1f"), type_member: "Estudante", notify: true}
  ],
  tags: ["exercício", "programação", "variáveis"],
  comments : [{
    text: "Exercício 01",
    date_comment: new Date(),
    files :[{
      path: "teste/variaveis01",
      weight: 5,
      name: "Primeira Atividade"
    }],
    members: [
      {"user_id": ObjectId("56e7416fca76b61efa435c1c"), notify: true, type_member: "Monitor"}
    ]
  }]
};

db.activities.save(activities);

Inserted 1 record(s) in 4ms
WriteResult({
  "nInserted": 1
})

var activities = {
  name: "Vetor",
  description: "Declaração e uso de vetores",
  date_begin: new Date(),
  date_dream: new Date(),
  date_end: new Date(),
  realocate: false,
  expired: false,
  historic: [],
  members: [
    {"user_id": ObjectId("56e7450fca76b61efa435c1d"), type_member: "Estudante", notify: true},
    {"user_id": ObjectId("56e7461eca76b61efa435c1e"), type_member: "Estudante", notify: true},
    {"user_id": ObjectId("56e746e5ca76b61efa435c1f"), type_member: "Estudante", notify: true}
  ],
  tags: ["exercício", "programação", "vetores"],
  comments : [{
    text: "Exercício 02",
    date_comment: new Date(),
    files :[{
      path: "teste/vetor02",
      weight: 5,
      name: "Segunda Atividade"
    }],
    members: [
      {"user_id": ObjectId("56e6dd219c0bb73f24b84ae4"), notify: true, type_member: "Professor"}
    ]
  }]
};

db.activities.save(activities);

Inserted 1 record(s) in 4ms
WriteResult({
  "nInserted": 1
})

var project = {
  name : "LÓGICA DE PROGRAMAÇÃO",
  description: "Semestre 1",
  date_begin: new Date(),
  date_dream: new Date(),
  date_end: new Date(),
  visible: true,
  realocate: false,
  expired: false,
  vizualizable_mod: false,
  tags: ["computação", "lógica", "programação"],
  members: [
    {"user_id": ObjectId("56e6dd219c0bb73f24b84ae4"), type_member: "Professor", notify: true},
    {"user_id": ObjectId("56e7416fca76b61efa435c1c"), type_member: "Monitor", notify: true},
    {"user_id": ObjectId("56e7450fca76b61efa435c1d"), type_member: "Estudante", notify: true},
    {"user_id": ObjectId("56e7461eca76b61efa435c1e"), type_member: "Estudante", notify: true},
    {"user_id": ObjectId("56e746e5ca76b61efa435c1f"), type_member: "Estudante", notify: true}
  ],
  goals : [{
    name: "Introdução a programação",
    description: "Primeiros passos na programação",
    date_begin: new Date(),
    date_dream: new Date(),
    date_end: new Date(),
    realocate: false,
    expired: false,
    historic: [],
    tags : ["materia", "programação", "requisito"],
    activities : [{"activity_id": ObjectId("56e76e52480ade334f1ef697")},{"activity_id": ObjectId("56e76e96480ade334f1ef698")}]
  }]
};

db.projects.save(project);

Inserted 1 record(s) in 6ms
WriteResult({
  "nInserted": 1
})

// Projeto 3
var activities = {
  name: "MongoDB",
  description: "Aprendendo sobre banco de dados NoSQL",
  date_begin: new Date(),
  date_dream: new Date(),
  date_end: new Date(),
  realocate: false,
  expired: false,
  historic: [],
  members: [
    {"user_id": ObjectId("56e7461eca76b61efa435c1e"), type_member: "Estudante", notify: true},
    {"user_id": ObjectId("56e746e5ca76b61efa435c1f"), type_member: "Estudante", notify: true},
    {"user_id": ObjectId("56e74798ca76b61efa435c20"), type_member: "Estudante", notify: true}
  ],
  tags: ["exercício", "MongoDB", "Banco de Dados"],
  comments : [{
    text: "Exercício 01",
    date_comment: new Date(),
    files :[{
      path: "teste/nosql01",
      weight: 5,
      name: "Primeira Atividade"
    }],
    members: [
      {"user_id": ObjectId("56e7450fca76b61efa435c1d"), notify: true, type_member: "Monitor"}
    ]
  }]
};

db.activities.save(activities);

Inserted 1 record(s) in 4ms
WriteResult({
  "nInserted": 1
})

var activities = {
  name: "MySQL",
  description: "Aprendendo sobre banco de dados relacionais",
  date_begin: new Date(),
  date_dream: new Date(),
  date_end: new Date(),
  realocate: false,
  expired: false,
  historic: [],
  members: [
    {"user_id": ObjectId("56e7461eca76b61efa435c1e"), type_member: "Estudante", notify: true},
    {"user_id": ObjectId("56e746e5ca76b61efa435c1f"), type_member: "Estudante", notify: true},
    {"user_id": ObjectId("56e74798ca76b61efa435c20"), type_member: "Estudante", notify: true}
  ],
  tags: ["exercício", "MySQL", "Banco de Dados"],
  comments : [{
    text: "Exercício 02",
    date_comment: new Date(),
    files :[{
      path: "teste/mysql02",
      weight: 5,
      name: "Segunda Atividade"
    }],
    members: [
      {"user_id": ObjectId("56e7416fca76b61efa435c1c"), notify: true, type_member: "Professor"}
    ]
  }]
};

db.activities.save(activities);

Inserted 1 record(s) in 4ms
WriteResult({
  "nInserted": 1
})

var project = {
  name : "BANCO DE DADOS I",
  description: "Semestre 2",
  date_begin: new Date(),
  date_dream: new Date(),
  date_end: new Date(),
  visible: true,
  realocate: false,
  expired: false,
  vizualizable_mod: false,
  tags: ["computação", "bd", "programação"],
  members: [
    {"user_id": ObjectId("56e7416fca76b61efa435c1c"), type_member: "Professor", notify: true},
    {"user_id": ObjectId("56e7450fca76b61efa435c1d"), type_member: "Monitor", notify: true},
    {"user_id": ObjectId("56e7461eca76b61efa435c1e"), type_member: "Estudante", notify: true},
    {"user_id": ObjectId("56e746e5ca76b61efa435c1f"), type_member: "Estudante", notify: true},
    {"user_id": ObjectId("56e74798ca76b61efa435c20"), type_member: "Estudante", notify: true}
  ],
  goals : [{
    name: "Introdução a Banco de Dados",
    description: "Primeiros passos",
    date_begin: new Date(),
    date_dream: new Date(),
    date_end: new Date(),
    realocate: false,
    expired: false,
    historic: [],
    tags : ["materia", "dados", "requisito"],
    activities : [{"activity_id": ObjectId("56e77464480ade334f1ef69b")},{"activity_id": ObjectId("56e77458480ade334f1ef69a")}]
  }]
};

db.projects.save(project);

Inserted 1 record(s) in 6ms
WriteResult({
  "nInserted": 1
})

// Projeto 4
var project = {
  name : "Estágio Supervisionado",
  description: "Disciplina Optativa",
  date_begin: new Date(),
  date_dream: new Date(),
  date_end: new Date(),
  visible: true,
  realocate: false,
  expired: false,
  vizualizable_mod: false,
  tags: ["computação", "estágio", "optativa"],
  members: [
    {"user_id": ObjectId("56e7461eca76b61efa435c1e"), type_member: "Professor", notify: true},
    {"user_id": ObjectId("56e746e5ca76b61efa435c1f"), type_member: "Monitor", notify: true},
    {"user_id": ObjectId("56e74798ca76b61efa435c20"), type_member: "Estudante", notify: true},
    {"user_id": ObjectId("56e74bc1ca76b61efa435c21"), type_member: "Estudante", notify: true},
    {"user_id": ObjectId("56e74f11ca76b61efa435c22"), type_member: "Estudante", notify: true}
  ],
  goals : [{
    name: "Prática",
    description: "Colocar em prática o que aprendeu no curso",
    date_begin: new Date(),
    date_dream: new Date(),
    date_end: new Date(),
    realocate: false,
    expired: false,
    historic: [],
    tags : ["prática", "estágio", "trabalho"],
    activities : []
  }]
};

db.projects.save(project);

Inserted 1 record(s) in 31ms
WriteResult({
  "nInserted": 1
})

// Projeto 5
var activities = {
  name: "Divisão e Conquista",
  description: "Técnica de Algoritmos",
  date_begin: new Date(),
  date_dream: new Date(),
  date_end: new Date(),
  realocate: false,
  expired: false,
  historic: [],
  members: [
    {"user_id": ObjectId("56e74bc1ca76b61efa435c21"), type_member: "Estudante", notify: true},
    {"user_id": ObjectId("56e74f11ca76b61efa435c22"), type_member: "Estudante", notify: true},
    {"user_id": ObjectId("56e7503cca76b61efa435c23"), type_member: "Estudante", notify: true}
  ],
  tags: ["exercício", "técnica", "divisão e conquista"],
  comments : [{
    text: "Exercício 01",
    date_comment: new Date(),
    files :[{
      path: "teste/divisaoeconquista01",
      weight: 5,
      name: "Primeira Atividade"
    }],
    members: [
      {"user_id": ObjectId("56e74798ca76b61efa435c20"), notify: true, type_member: "Monitor"}
    ]
  }]
};

db.activities.save(activities);

Inserted 1 record(s) in 3ms
WriteResult({
  "nInserted": 1
})

var activities = {
  name: "Complexidade",
  description: "Aprendendo a cálcular complexidade de algoritmos",
  date_begin: new Date(),
  date_dream: new Date(),
  date_end: new Date(),
  realocate: false,
  expired: false,
  historic: [],
  members: [
    {"user_id": ObjectId("56e74bc1ca76b61efa435c21"), type_member: "Estudante", notify: true},
    {"user_id": ObjectId("56e74f11ca76b61efa435c22"), type_member: "Estudante", notify: true},
    {"user_id": ObjectId("56e7503cca76b61efa435c23"), type_member: "Estudante", notify: true}
  ],
  tags: ["exercício", "complexidade", "algoritmos"],
  comments : [{
    text: "Exercício 02",
    date_comment: new Date(),
    files :[{
      path: "teste/complexidade02",
      weight: 5,
      name: "Segunda Atividade"
    }],
    members: [
      {"user_id": ObjectId("56e746e5ca76b61efa435c1f"), notify: true, type_member: "Professor"}
    ]
  }]
};

db.activities.save(activities);

Inserted 1 record(s) in 4ms
WriteResult({
  "nInserted": 1
})

var project = {
  name : "CONSTRUÇÃO E ANÁLISE DE ALGORITMOS",
  description: "Semestre 3",
  date_begin: new Date(),
  date_dream: new Date(),
  date_end: new Date(),
  visible: true,
  realocate: false,
  expired: false,
  vizualizable_mod: false,
  tags: ["computação", "análise", "algoritmos"],
  members: [
    {"user_id": ObjectId("56e746e5ca76b61efa435c1f"), type_member: "Professor", notify: true},
    {"user_id": ObjectId("56e74798ca76b61efa435c20"), type_member: "Monitor", notify: true},
    {"user_id": ObjectId("56e74bc1ca76b61efa435c21"), type_member: "Estudante", notify: true},
    {"user_id": ObjectId("56e74f11ca76b61efa435c22"), type_member: "Estudante", notify: true},
    {"user_id": ObjectId("56e7503cca76b61efa435c23"), type_member: "Estudante", notify: true}
  ],
  goals : [{
    name: "Introdução a Banco de Dados",
    description: "Primeiros passos",
    date_begin: new Date(),
    date_dream: new Date(),
    date_end: new Date(),
    realocate: false,
    expired: false,
    historic: [],
    tags : ["materia", "dados", "requisito"],
    activities : [{"activity_id": ObjectId("56e8b39b01e50323debd7b4f")},{"activity_id": ObjectId("56e8b3ac01e50323debd7b50")}]
  }]
};

db.projects.save(project);

Inserted 1 record(s) in 6ms
WriteResult({
  "nInserted": 1
})
```


## Retrieve - busca
### 1. Liste as informações dos membros de 1 projeto específico que deve ser buscado pelo seu nome de forma a não ligar para maiúsculas e minúsculas.
``` js
var project_info = db.projects.findOne({ name: /construção e análise de algoritmos/i }, { members: 1, _id: 0 } );

var members_info = [];

project_info.members.forEach(function(member) {
  members_info.push(db.users.findOne( { _id: member.user_id } ));
});

members_info;

[
  {
    "_id": ObjectId("56e746e5ca76b61efa435c1f"),
    "name": "Steve Rogers",
    "bio": "Bio of Rogers",
    "data_register": ISODate("2016-03-14T23:18:59.371Z"),
    "avatar_path": "img/avatares/fa2e3942176c4e7c4c687a5b915fbcaa.jpg",
    "auth": {
      "username": "capitaoamerica",
      "email": "capitaoamerica@email.com",
      "password": "35d1321e6854115cf82034f7b7d0efae",
      "last_access": ISODate("2016-03-14T23:18:59.371Z"),
      "online": true,
      "disabled": false,
      "hash_token": "fa2e3942176c4e7c4c687a5b915fbcaa"
    },
    "background_path": "img/backgrounds/fa2e3942176c4e7c4c687a5b915fbcaa.jpg"
  },
  {
    "_id": ObjectId("56e74798ca76b61efa435c20"),
    "name": "John Stewart",
    "bio": "Bio of John",
    "data_register": ISODate("2016-03-14T23:21:57.774Z"),
    "avatar_path": "img/avatares/42cfd85a880905fa8b680d0b53fdf0c1.jpg",
    "auth": {
      "username": "lanternaverde",
      "email": "lanternaverde@email.com",
      "password": "39e06a1bf214e2ab696488c8d296019f",
      "last_access": ISODate("2016-03-14T23:21:57.774Z"),
      "online": true,
      "disabled": false,
      "hash_token": "42cfd85a880905fa8b680d0b53fdf0c1"
    },
    "background_path": "img/backgrounds/42cfd85a880905fa8b680d0b53fdf0c1.jpg"
  },
  {
    "_id": ObjectId("56e74bc1ca76b61efa435c21"),
    "name": "Barry Allen",
    "bio": "Bio of Barry",
    "data_register": ISODate("2016-03-14T23:39:43.891Z"),
    "avatar_path": "img/avatares/dc9305e4445660632fdfdc5ec9e3490f.jpg",
    "auth": {
      "username": "flash",
      "email": "flash@email.com",
      "password": "a9c04403aed6d8f1a1602d4f2e4d05bc",
      "last_access": ISODate("2016-03-14T23:39:43.891Z"),
      "online": true,
      "disabled": false,
      "hash_token": "dc9305e4445660632fdfdc5ec9e3490f"
    },
    "background_path": "img/backgrounds/dc9305e4445660632fdfdc5ec9e3490f.jpg"
  },
  {
    "_id": ObjectId("56e74f11ca76b61efa435c22"),
    "name": "Robert Bruce Banner",
    "bio": "Bio of Banner",
    "data_register": ISODate("2016-03-14T23:53:46.868Z"),
    "avatar_path": "img/avatares/ac20d9140e6f12975dfe021875ef1322.jpg",
    "auth": {
      "username": "hulk",
      "email": "hulk@email.com",
      "password": "9ddbc04c5ac2ca10ce94f59e270a7212",
      "last_access": ISODate("2016-03-14T23:53:46.868Z"),
      "online": true,
      "disabled": false,
      "hash_token": "ac20d9140e6f12975dfe021875ef1322"
    },
    "background_path": "img/backgrounds/ac20d9140e6f12975dfe021875ef1322.jpg"
  },
  {
    "_id": ObjectId("56e7503cca76b61efa435c23"),
    "name": "Matthew Michael Murdock",
    "bio": "Bio of Matt",
    "data_register": ISODate("2016-03-14T23:58:49.969Z"),
    "avatar_path": "img/avatares/5c196ce1149ace6d900e4c9ee2205b4f.jpg",
    "auth": {
      "username": "demolidor",
      "email": "demolidor@email.com",
      "password": "4fa5b823a1646b170a6683ce833bdc1d",
      "last_access": ISODate("2016-03-14T23:58:49.969Z"),
      "online": true,
      "disabled": false,
      "hash_token": "5c196ce1149ace6d900e4c9ee2205b4f"
    },
    "background_path": "img/backgrounds/5c196ce1149ace6d900e4c9ee2205b4f.jpg"
  }
]
```
### 2. Liste todos os projetos com a tag que você escolheu para os 3 projetos em comum.
``` js
aim(mongod-3.2.4) projeto-final> db.projects.find({ tags : { $in : [/computação/i] }  })
{
  "_id": ObjectId("56e774a6480ade334f1ef69c"),
  "name": "BANCO DE DADOS I",
  "description": "Semestre 2",
  "date_begin": ISODate("2016-03-15T02:34:08.599Z"),
  "date_dream": ISODate("2016-03-15T02:34:08.599Z"),
  "date_end": ISODate("2016-03-15T02:34:08.599Z"),
  "visible": true,
  "realocate": false,
  "expired": false,
  "vizualizable_mod": false,
  "tags": [
    "computação",
    "bd",
    "programação"
  ],
  "members": [
    {
      "user_id": ObjectId("56e7416fca76b61efa435c1c"),
      "type_member": "Professor",
      "notify": true
    },
    {
      "user_id": ObjectId("56e7450fca76b61efa435c1d"),
      "type_member": "Monitor",
      "notify": true
    },
    {
      "user_id": ObjectId("56e7461eca76b61efa435c1e"),
      "type_member": "Estudante",
      "notify": true
    },
    {
      "user_id": ObjectId("56e746e5ca76b61efa435c1f"),
      "type_member": "Estudante",
      "notify": true
    },
    {
      "user_id": ObjectId("56e74798ca76b61efa435c20"),
      "type_member": "Estudante",
      "notify": true
    }
  ],
  "goals": [
    {
      "name": "Introdução a Banco de Dados",
      "description": "Primeiros passos",
      "date_begin": ISODate("2016-03-15T02:34:08.599Z"),
      "date_dream": ISODate("2016-03-15T02:34:08.599Z"),
      "date_end": ISODate("2016-03-15T02:34:08.599Z"),
      "realocate": false,
      "expired": false,
      "historic": [ ],
      "tags": [
        "materia",
        "dados",
        "requisito"
      ],
      "activities": [
        {
          "activity_id": ObjectId("56e77464480ade334f1ef69b")
        },
        {
          "activity_id": ObjectId("56e77458480ade334f1ef69a")
        }
      ]
    }
  ]
}
{
  "_id": ObjectId("56e8b1fc01e50323debd7b4e"),
  "name": "Estágio Supervisionado",
  "description": "Disciplina Optativa",
  "date_begin": ISODate("2016-03-16T01:08:08.618Z"),
  "date_dream": ISODate("2016-03-16T01:08:08.618Z"),
  "date_end": ISODate("2016-03-16T01:08:08.618Z"),
  "visible": true,
  "realocate": false,
  "expired": false,
  "vizualizable_mod": false,
  "tags": [
    "computação",
    "estágio",
    "optativa"
  ],
  "members": [
    {
      "user_id": ObjectId("56e7461eca76b61efa435c1e"),
      "type_member": "Professor",
      "notify": true
    },
    {
      "user_id": ObjectId("56e746e5ca76b61efa435c1f"),
      "type_member": "Monitor",
      "notify": true
    },
    {
      "user_id": ObjectId("56e74798ca76b61efa435c20"),
      "type_member": "Estudante",
      "notify": true
    },
    {
      "user_id": ObjectId("56e74bc1ca76b61efa435c21"),
      "type_member": "Estudante",
      "notify": true
    },
    {
      "user_id": ObjectId("56e74f11ca76b61efa435c22"),
      "type_member": "Estudante",
      "notify": true
    }
  ],
  "goals": [
    {
      "name": "Prática",
      "description": "Colocar em prática o que aprendeu no curso",
      "date_begin": ISODate("2016-03-16T01:08:08.618Z"),
      "date_dream": ISODate("2016-03-16T01:08:08.618Z"),
      "date_end": ISODate("2016-03-16T01:08:08.618Z"),
      "realocate": false,
      "expired": false,
      "historic": [ ],
      "tags": [
        "prática",
        "estágio",
        "trabalho"
      ],
      "activities": [ ]
    }
  ]
}
{
  "_id": ObjectId("56e8b42001e50323debd7b51"),
  "name": "CONSTRUÇÃO E ANÁLISE DE ALGORITMOS",
  "description": "Semestre 3",
  "date_begin": ISODate("2016-03-16T01:16:57.464Z"),
  "date_dream": ISODate("2016-03-16T01:16:57.464Z"),
  "date_end": ISODate("2016-03-16T01:16:57.464Z"),
  "visible": true,
  "realocate": false,
  "expired": false,
  "vizualizable_mod": false,
  "tags": [
    "computação",
    "análise",
    "algoritmos"
  ],
  "members": [
    {
      "user_id": ObjectId("56e746e5ca76b61efa435c1f"),
      "type_member": "Professor",
      "notify": true
    },
    {
      "user_id": ObjectId("56e74798ca76b61efa435c20"),
      "type_member": "Monitor",
      "notify": true
    },
    {
      "user_id": ObjectId("56e74bc1ca76b61efa435c21"),
      "type_member": "Estudante",
      "notify": true
    },
    {
      "user_id": ObjectId("56e74f11ca76b61efa435c22"),
      "type_member": "Estudante",
      "notify": true
    },
    {
      "user_id": ObjectId("56e7503cca76b61efa435c23"),
      "type_member": "Estudante",
      "notify": true
    }
  ],
  "goals": [
    {
      "name": "Introdução a Banco de Dados",
      "description": "Primeiros passos",
      "date_begin": ISODate("2016-03-16T01:16:57.464Z"),
      "date_dream": ISODate("2016-03-16T01:16:57.464Z"),
      "date_end": ISODate("2016-03-16T01:16:57.464Z"),
      "realocate": false,
      "expired": false,
      "historic": [ ],
      "tags": [
        "materia",
        "dados",
        "requisito"
      ],
      "activities": [
        {
          "activity_id": ObjectId("56e8b39b01e50323debd7b4f")
        },
        {
          "activity_id": ObjectId("56e8b3ac01e50323debd7b50")
        }
      ]
    }
  ]
}
Fetched 3 record(s) in 22ms
```
### 3. Liste apenas os nomes de todas as atividades para todos os projetos.
``` js
var projects = db.projects.find({}).toArray();

var activities_id = [];

var activities = [];

projects.forEach(function(project) {
  project.goals.forEach(function(goal) {
    goal.activities.forEach(function(activity) {
      activities_id.push(activity.activity_id);
    });
  });
});

activities_id.forEach(function(objectId) {
  activities.push( db.activities.findOne({ _id : objectId }, {_id : 0, name: 1}) );
});

activities;

[
  {
    "name": "Limite"
  },
  {
    "name": "Derivadas"
  },
  {
    "name": "Variaveis"
  },
  {
    "name": "Vetor"
  },
  {
    "name": "MySQL"
  },
  {
    "name": "MongoDB"
  },
  {
    "name": "Divisão e Conquista"
  },
  {
    "name": "Complexidade"
  }
]
```
### 4. Liste todos os projetos que não possuam uma tag.
``` js
db.projects.find({ tags: { $not: { $in: [/computação/i] } } });

{
  "_id": ObjectId("56e766b1480ade334f1ef696"),
  "name": "CALCULO I",
  "description": "Semestre 1",
  "date_begin": ISODate("2016-03-15T01:34:18.906Z"),
  "date_dream": ISODate("2016-03-15T01:34:18.906Z"),
  "date_end": ISODate("2016-03-15T01:34:18.906Z"),
  "visible": true,
  "realocate": false,
  "expired": false,
  "vizualizable_mod": false,
  "tags": [
    "matematica",
    "calculo",
    "projetocad"
  ],
  "members": [
    {
      "user_id": ObjectId("56e6db209c0bb73f24b84ae3"),
      "type_member": "Professor",
      "notify": true
    },
    {
      "user_id": ObjectId("56e6dd219c0bb73f24b84ae4"),
      "type_member": "Estudante",
      "notify": true
    },
    {
      "user_id": ObjectId("56e7416fca76b61efa435c1c"),
      "type_member": "Monitor",
      "notify": true
    },
    {
      "user_id": ObjectId("56e7450fca76b61efa435c1d"),
      "type_member": "Estudante",
      "notify": true
    },
    {
      "user_id": ObjectId("56e7461eca76b61efa435c1e"),
      "type_member": "Estudante",
      "notify": true
    }
  ],
  "goals": [
    {
      "name": "Limites e Derivadas",
      "description": "Tecnicas e Exercicios sobre Limites e Derivadas",
      "date_begin": ISODate("2016-03-15T01:34:18.907Z"),
      "date_dream": ISODate("2016-03-15T01:34:18.907Z"),
      "date_end": ISODate("2016-03-15T01:34:18.907Z"),
      "realocate": false,
      "expired": false,
      "historic": [ ],
      "tags": [
        "materia",
        "limite",
        "derivada"
      ],
      "activities": [
        {
          "activity_id": ObjectId("56e7621cca76b61efa435c24")
        },
        {
          "activity_id": ObjectId("56e7627aca76b61efa435c25")
        }
      ]
    }
  ]
}
{
  "_id": ObjectId("56e76ef0480ade334f1ef699"),
  "name": "LOGICA DE PROGRAMACAO",
  "description": "Semestre 1",
  "date_begin": ISODate("2016-03-15T02:09:47.277Z"),
  "date_dream": ISODate("2016-03-15T02:09:47.277Z"),
  "date_end": ISODate("2016-03-15T02:09:47.277Z"),
  "visible": true,
  "realocate": false,
  "expired": false,
  "vizualizable_mod": false,
  "tags": [
    "computacao",
    "logica",
    "programacao"
  ],
  "members": [
    {
      "user_id": ObjectId("56e6dd219c0bb73f24b84ae4"),
      "type_member": "Professor",
      "notify": true
    },
    {
      "user_id": ObjectId("56e7416fca76b61efa435c1c"),
      "type_member": "Monitor",
      "notify": true
    },
    {
      "user_id": ObjectId("56e7450fca76b61efa435c1d"),
      "type_member": "Estudante",
      "notify": true
    },
    {
      "user_id": ObjectId("56e7461eca76b61efa435c1e"),
      "type_member": "Estudante",
      "notify": true
    },
    {
      "user_id": ObjectId("56e746e5ca76b61efa435c1f"),
      "type_member": "Estudante",
      "notify": true
    }
  ],
  "goals": [
    {
      "name": "Introducao a programacao",
      "description": "Primeiros passos na programacao",
      "date_begin": ISODate("2016-03-15T02:09:47.278Z"),
      "date_dream": ISODate("2016-03-15T02:09:47.278Z"),
      "date_end": ISODate("2016-03-15T02:09:47.278Z"),
      "realocate": false,
      "expired": false,
      "historic": [ ],
      "tags": [
        "materia",
        "programacao",
        "requisito"
      ],
      "activities": [
        {
          "activity_id": ObjectId("56e76e52480ade334f1ef697")
        },
        {
          "activity_id": ObjectId("56e76e96480ade334f1ef698")
        }
      ]
    }
  ]
}
Fetched 2 record(s) in 16ms

```
### 5. Liste todos os usuários que não fazem parte do primeiro projeto cadastrado.
``` js
var project = db.projects.findOne();

var members = [];

project.members.forEach(function(member) {
  members.push(member.user_id);
});

db.users.find({ _id: { $not: { $in: members } } })

{
  "_id": ObjectId("56e746e5ca76b61efa435c1f"),
  "name": "Steve Rogers",
  "bio": "Bio of Rogers",
  "data_register": ISODate("2016-03-14T23:18:59.371Z"),
  "avatar_path": "img/avatares/fa2e3942176c4e7c4c687a5b915fbcaa.jpg",
  "auth": {
    "username": "capitaoamerica",
    "email": "capitaoamerica@email.com",
    "password": "35d1321e6854115cf82034f7b7d0efae",
    "last_access": ISODate("2016-03-14T23:18:59.371Z"),
    "online": true,
    "disabled": false,
    "hash_token": "fa2e3942176c4e7c4c687a5b915fbcaa"
  },
  "background_path": "img/backgrounds/fa2e3942176c4e7c4c687a5b915fbcaa.jpg"
}
{
  "_id": ObjectId("56e74798ca76b61efa435c20"),
  "name": "John Stewart",
  "bio": "Bio of John",
  "data_register": ISODate("2016-03-14T23:21:57.774Z"),
  "avatar_path": "img/avatares/42cfd85a880905fa8b680d0b53fdf0c1.jpg",
  "auth": {
    "username": "lanternaverde",
    "email": "lanternaverde@email.com",
    "password": "39e06a1bf214e2ab696488c8d296019f",
    "last_access": ISODate("2016-03-14T23:21:57.774Z"),
    "online": true,
    "disabled": false,
    "hash_token": "42cfd85a880905fa8b680d0b53fdf0c1"
  },
  "background_path": "img/backgrounds/42cfd85a880905fa8b680d0b53fdf0c1.jpg"
}
{
  "_id": ObjectId("56e74bc1ca76b61efa435c21"),
  "name": "Barry Allen",
  "bio": "Bio of Barry",
  "data_register": ISODate("2016-03-14T23:39:43.891Z"),
  "avatar_path": "img/avatares/dc9305e4445660632fdfdc5ec9e3490f.jpg",
  "auth": {
    "username": "flash",
    "email": "flash@email.com",
    "password": "a9c04403aed6d8f1a1602d4f2e4d05bc",
    "last_access": ISODate("2016-03-14T23:39:43.891Z"),
    "online": true,
    "disabled": false,
    "hash_token": "dc9305e4445660632fdfdc5ec9e3490f"
  },
  "background_path": "img/backgrounds/dc9305e4445660632fdfdc5ec9e3490f.jpg"
}
{
  "_id": ObjectId("56e74f11ca76b61efa435c22"),
  "name": "Robert Bruce Banner",
  "bio": "Bio of Banner",
  "data_register": ISODate("2016-03-14T23:53:46.868Z"),
  "avatar_path": "img/avatares/ac20d9140e6f12975dfe021875ef1322.jpg",
  "auth": {
    "username": "hulk",
    "email": "hulk@email.com",
    "password": "9ddbc04c5ac2ca10ce94f59e270a7212",
    "last_access": ISODate("2016-03-14T23:53:46.868Z"),
    "online": true,
    "disabled": false,
    "hash_token": "ac20d9140e6f12975dfe021875ef1322"
  },
  "background_path": "img/backgrounds/ac20d9140e6f12975dfe021875ef1322.jpg"
}
{
  "_id": ObjectId("56e7503cca76b61efa435c23"),
  "name": "Matthew Michael Murdock",
  "bio": "Bio of Matt",
  "data_register": ISODate("2016-03-14T23:58:49.969Z"),
  "avatar_path": "img/avatares/5c196ce1149ace6d900e4c9ee2205b4f.jpg",
  "auth": {
    "username": "demolidor",
    "email": "demolidor@email.com",
    "password": "4fa5b823a1646b170a6683ce833bdc1d",
    "last_access": ISODate("2016-03-14T23:58:49.969Z"),
    "online": true,
    "disabled": false,
    "hash_token": "5c196ce1149ace6d900e4c9ee2205b4f"
  },
  "background_path": "img/backgrounds/5c196ce1149ace6d900e4c9ee2205b4f.jpg"
}
Fetched 5 record(s) in 17ms
```

## Update - alteração
### 1. Adicione para todos os projetos o campo ```views: 0```.
``` js
db.projects.update( {}, { $set: { views: 0 } }, { multi: true } );

Updated 5 existing record(s) in 24ms
WriteResult({
  "nMatched": 5,
  "nUpserted": 0,
  "nModified": 5
})

db.projects.find();
{
  "_id": ObjectId("56e766b1480ade334f1ef696"),
  "name": "CALCULO I",
  "description": "Semestre 1",
  "date_begin": ISODate("2016-03-15T01:34:18.906Z"),
  "date_dream": ISODate("2016-03-15T01:34:18.906Z"),
  "date_end": ISODate("2016-03-15T01:34:18.906Z"),
  "visible": true,
  "realocate": false,
  "expired": false,
  "vizualizable_mod": false,
  "tags": [
    "matematica",
    "calculo",
    "projetocad"
  ],
  "members": [
    {
      "user_id": ObjectId("56e6db209c0bb73f24b84ae3"),
      "type_member": "Professor",
      "notify": true
    },
    {
      "user_id": ObjectId("56e6dd219c0bb73f24b84ae4"),
      "type_member": "Estudante",
      "notify": true
    },
    {
      "user_id": ObjectId("56e7416fca76b61efa435c1c"),
      "type_member": "Monitor",
      "notify": true
    },
    {
      "user_id": ObjectId("56e7450fca76b61efa435c1d"),
      "type_member": "Estudante",
      "notify": true
    },
    {
      "user_id": ObjectId("56e7461eca76b61efa435c1e"),
      "type_member": "Estudante",
      "notify": true
    }
  ],
  "goals": [
    {
      "name": "Limites e Derivadas",
      "description": "Tecnicas e Exercicios sobre Limites e Derivadas",
      "date_begin": ISODate("2016-03-15T01:34:18.907Z"),
      "date_dream": ISODate("2016-03-15T01:34:18.907Z"),
      "date_end": ISODate("2016-03-15T01:34:18.907Z"),
      "realocate": false,
      "expired": false,
      "historic": [ ],
      "tags": [
        "materia",
        "limite",
        "derivada"
      ],
      "activities": [
        {
          "activity_id": ObjectId("56e7621cca76b61efa435c24")
        },
        {
          "activity_id": ObjectId("56e7627aca76b61efa435c25")
        }
      ]
    }
  ],
  "views": 0
}
{
  "_id": ObjectId("56e76ef0480ade334f1ef699"),
  "name": "LOGICA DE PROGRAMACAO",
  "description": "Semestre 1",
  "date_begin": ISODate("2016-03-15T02:09:47.277Z"),
  "date_dream": ISODate("2016-03-15T02:09:47.277Z"),
  "date_end": ISODate("2016-03-15T02:09:47.277Z"),
  "visible": true,
  "realocate": false,
  "expired": false,
  "vizualizable_mod": false,
  "tags": [
    "computacao",
    "logica",
    "programacao"
  ],
  "members": [
    {
      "user_id": ObjectId("56e6dd219c0bb73f24b84ae4"),
      "type_member": "Professor",
      "notify": true
    },
    {
      "user_id": ObjectId("56e7416fca76b61efa435c1c"),
      "type_member": "Monitor",
      "notify": true
    },
    {
      "user_id": ObjectId("56e7450fca76b61efa435c1d"),
      "type_member": "Estudante",
      "notify": true
    },
    {
      "user_id": ObjectId("56e7461eca76b61efa435c1e"),
      "type_member": "Estudante",
      "notify": true
    },
    {
      "user_id": ObjectId("56e746e5ca76b61efa435c1f"),
      "type_member": "Estudante",
      "notify": true
    }
  ],
  "goals": [
    {
      "name": "Introducao a programacao",
      "description": "Primeiros passos na programacao",
      "date_begin": ISODate("2016-03-15T02:09:47.278Z"),
      "date_dream": ISODate("2016-03-15T02:09:47.278Z"),
      "date_end": ISODate("2016-03-15T02:09:47.278Z"),
      "realocate": false,
      "expired": false,
      "historic": [ ],
      "tags": [
        "materia",
        "programacao",
        "requisito"
      ],
      "activities": [
        {
          "activity_id": ObjectId("56e76e52480ade334f1ef697")
        },
        {
          "activity_id": ObjectId("56e76e96480ade334f1ef698")
        }
      ]
    }
  ],
  "views": 0
}
{
  "_id": ObjectId("56e774a6480ade334f1ef69c"),
  "name": "BANCO DE DADOS I",
  "description": "Semestre 2",
  "date_begin": ISODate("2016-03-15T02:34:08.599Z"),
  "date_dream": ISODate("2016-03-15T02:34:08.599Z"),
  "date_end": ISODate("2016-03-15T02:34:08.599Z"),
  "visible": true,
  "realocate": false,
  "expired": false,
  "vizualizable_mod": false,
  "tags": [
    "computação",
    "bd",
    "programação"
  ],
  "members": [
    {
      "user_id": ObjectId("56e7416fca76b61efa435c1c"),
      "type_member": "Professor",
      "notify": true
    },
    {
      "user_id": ObjectId("56e7450fca76b61efa435c1d"),
      "type_member": "Monitor",
      "notify": true
    },
    {
      "user_id": ObjectId("56e7461eca76b61efa435c1e"),
      "type_member": "Estudante",
      "notify": true
    },
    {
      "user_id": ObjectId("56e746e5ca76b61efa435c1f"),
      "type_member": "Estudante",
      "notify": true
    },
    {
      "user_id": ObjectId("56e74798ca76b61efa435c20"),
      "type_member": "Estudante",
      "notify": true
    }
  ],
  "goals": [
    {
      "name": "Introdução a Banco de Dados",
      "description": "Primeiros passos",
      "date_begin": ISODate("2016-03-15T02:34:08.599Z"),
      "date_dream": ISODate("2016-03-15T02:34:08.599Z"),
      "date_end": ISODate("2016-03-15T02:34:08.599Z"),
      "realocate": false,
      "expired": false,
      "historic": [ ],
      "tags": [
        "materia",
        "dados",
        "requisito"
      ],
      "activities": [
        {
          "activity_id": ObjectId("56e77464480ade334f1ef69b")
        },
        {
          "activity_id": ObjectId("56e77458480ade334f1ef69a")
        }
      ]
    }
  ],
  "views": 0
}
{
  "_id": ObjectId("56e8b1fc01e50323debd7b4e"),
  "name": "Estágio Supervisionado",
  "description": "Disciplina Optativa",
  "date_begin": ISODate("2016-03-16T01:08:08.618Z"),
  "date_dream": ISODate("2016-03-16T01:08:08.618Z"),
  "date_end": ISODate("2016-03-16T01:08:08.618Z"),
  "visible": true,
  "realocate": false,
  "expired": false,
  "vizualizable_mod": false,
  "tags": [
    "computação",
    "estágio",
    "optativa"
  ],
  "members": [
    {
      "user_id": ObjectId("56e7461eca76b61efa435c1e"),
      "type_member": "Professor",
      "notify": true
    },
    {
      "user_id": ObjectId("56e746e5ca76b61efa435c1f"),
      "type_member": "Monitor",
      "notify": true
    },
    {
      "user_id": ObjectId("56e74798ca76b61efa435c20"),
      "type_member": "Estudante",
      "notify": true
    },
    {
      "user_id": ObjectId("56e74bc1ca76b61efa435c21"),
      "type_member": "Estudante",
      "notify": true
    },
    {
      "user_id": ObjectId("56e74f11ca76b61efa435c22"),
      "type_member": "Estudante",
      "notify": true
    }
  ],
  "goals": [
    {
      "name": "Prática",
      "description": "Colocar em prática o que aprendeu no curso",
      "date_begin": ISODate("2016-03-16T01:08:08.618Z"),
      "date_dream": ISODate("2016-03-16T01:08:08.618Z"),
      "date_end": ISODate("2016-03-16T01:08:08.618Z"),
      "realocate": false,
      "expired": false,
      "historic": [ ],
      "tags": [
        "prática",
        "estágio",
        "trabalho"
      ],
      "activities": [ ]
    }
  ],
  "views": 0
}
{
  "_id": ObjectId("56e8b42001e50323debd7b51"),
  "name": "CONSTRUÇÃO E ANÁLISE DE ALGORITMOS",
  "description": "Semestre 3",
  "date_begin": ISODate("2016-03-16T01:16:57.464Z"),
  "date_dream": ISODate("2016-03-16T01:16:57.464Z"),
  "date_end": ISODate("2016-03-16T01:16:57.464Z"),
  "visible": true,
  "realocate": false,
  "expired": false,
  "vizualizable_mod": false,
  "tags": [
    "computação",
    "análise",
    "algoritmos"
  ],
  "members": [
    {
      "user_id": ObjectId("56e746e5ca76b61efa435c1f"),
      "type_member": "Professor",
      "notify": true
    },
    {
      "user_id": ObjectId("56e74798ca76b61efa435c20"),
      "type_member": "Monitor",
      "notify": true
    },
    {
      "user_id": ObjectId("56e74bc1ca76b61efa435c21"),
      "type_member": "Estudante",
      "notify": true
    },
    {
      "user_id": ObjectId("56e74f11ca76b61efa435c22"),
      "type_member": "Estudante",
      "notify": true
    },
    {
      "user_id": ObjectId("56e7503cca76b61efa435c23"),
      "type_member": "Estudante",
      "notify": true
    }
  ],
  "goals": [
    {
      "name": "Introdução a Banco de Dados",
      "description": "Primeiros passos",
      "date_begin": ISODate("2016-03-16T01:16:57.464Z"),
      "date_dream": ISODate("2016-03-16T01:16:57.464Z"),
      "date_end": ISODate("2016-03-16T01:16:57.464Z"),
      "realocate": false,
      "expired": false,
      "historic": [ ],
      "tags": [
        "materia",
        "dados",
        "requisito"
      ],
      "activities": [
        {
          "activity_id": ObjectId("56e8b39b01e50323debd7b4f")
        },
        {
          "activity_id": ObjectId("56e8b3ac01e50323debd7b50")
        }
      ]
    }
  ],
  "views": 0
}
Fetched 5 record(s) in 47ms
```
### 2. Adicione 1 tag diferente para cada projeto.
``` js
for (var x = 0; x < 5; x++) {
  var project = db.projects.find().skip(x).limit(1).toArray();
  db.projects.update( { _id: project[0]._id }, { $push: { tags: 'tag' + (x + 1) } } );
}

db.projects.find()
{
  "_id": ObjectId("56e766b1480ade334f1ef696"),
  "name": "CALCULO I",
  "description": "Semestre 1",
  "date_begin": ISODate("2016-03-15T01:34:18.906Z"),
  "date_dream": ISODate("2016-03-15T01:34:18.906Z"),
  "date_end": ISODate("2016-03-15T01:34:18.906Z"),
  "visible": true,
  "realocate": false,
  "expired": false,
  "vizualizable_mod": false,
  "tags": [
    "matematica",
    "calculo",
    "projetocad",
    "tag1",
    "tag1"
  ],
  "members": [
    {
      "user_id": ObjectId("56e6db209c0bb73f24b84ae3"),
      "type_member": "Professor",
      "notify": true
    },
    {
      "user_id": ObjectId("56e6dd219c0bb73f24b84ae4"),
      "type_member": "Estudante",
      "notify": true
    },
    {
      "user_id": ObjectId("56e7416fca76b61efa435c1c"),
      "type_member": "Monitor",
      "notify": true
    },
    {
      "user_id": ObjectId("56e7450fca76b61efa435c1d"),
      "type_member": "Estudante",
      "notify": true
    },
    {
      "user_id": ObjectId("56e7461eca76b61efa435c1e"),
      "type_member": "Estudante",
      "notify": true
    }
  ],
  "goals": [
    {
      "name": "Limites e Derivadas",
      "description": "Tecnicas e Exercicios sobre Limites e Derivadas",
      "date_begin": ISODate("2016-03-15T01:34:18.907Z"),
      "date_dream": ISODate("2016-03-15T01:34:18.907Z"),
      "date_end": ISODate("2016-03-15T01:34:18.907Z"),
      "realocate": false,
      "expired": false,
      "historic": [ ],
      "tags": [
        "materia",
        "limite",
        "derivada"
      ],
      "activities": [
        {
          "activity_id": ObjectId("56e7621cca76b61efa435c24")
        },
        {
          "activity_id": ObjectId("56e7627aca76b61efa435c25")
        }
      ]
    }
  ],
  "views": 0
}
{
  "_id": ObjectId("56e76ef0480ade334f1ef699"),
  "name": "LOGICA DE PROGRAMACAO",
  "description": "Semestre 1",
  "date_begin": ISODate("2016-03-15T02:09:47.277Z"),
  "date_dream": ISODate("2016-03-15T02:09:47.277Z"),
  "date_end": ISODate("2016-03-15T02:09:47.277Z"),
  "visible": true,
  "realocate": false,
  "expired": false,
  "vizualizable_mod": false,
  "tags": [
    "computacao",
    "logica",
    "programacao",
    "tag2"
  ],
  "members": [
    {
      "user_id": ObjectId("56e6dd219c0bb73f24b84ae4"),
      "type_member": "Professor",
      "notify": true
    },
    {
      "user_id": ObjectId("56e7416fca76b61efa435c1c"),
      "type_member": "Monitor",
      "notify": true
    },
    {
      "user_id": ObjectId("56e7450fca76b61efa435c1d"),
      "type_member": "Estudante",
      "notify": true
    },
    {
      "user_id": ObjectId("56e7461eca76b61efa435c1e"),
      "type_member": "Estudante",
      "notify": true
    },
    {
      "user_id": ObjectId("56e746e5ca76b61efa435c1f"),
      "type_member": "Estudante",
      "notify": true
    }
  ],
  "goals": [
    {
      "name": "Introducao a programacao",
      "description": "Primeiros passos na programacao",
      "date_begin": ISODate("2016-03-15T02:09:47.278Z"),
      "date_dream": ISODate("2016-03-15T02:09:47.278Z"),
      "date_end": ISODate("2016-03-15T02:09:47.278Z"),
      "realocate": false,
      "expired": false,
      "historic": [ ],
      "tags": [
        "materia",
        "programacao",
        "requisito"
      ],
      "activities": [
        {
          "activity_id": ObjectId("56e76e52480ade334f1ef697")
        },
        {
          "activity_id": ObjectId("56e76e96480ade334f1ef698")
        }
      ]
    }
  ],
  "views": 0
}
{
  "_id": ObjectId("56e774a6480ade334f1ef69c"),
  "name": "BANCO DE DADOS I",
  "description": "Semestre 2",
  "date_begin": ISODate("2016-03-15T02:34:08.599Z"),
  "date_dream": ISODate("2016-03-15T02:34:08.599Z"),
  "date_end": ISODate("2016-03-15T02:34:08.599Z"),
  "visible": true,
  "realocate": false,
  "expired": false,
  "vizualizable_mod": false,
  "tags": [
    "computação",
    "bd",
    "programação",
    "tag3"
  ],
  "members": [
    {
      "user_id": ObjectId("56e7416fca76b61efa435c1c"),
      "type_member": "Professor",
      "notify": true
    },
    {
      "user_id": ObjectId("56e7450fca76b61efa435c1d"),
      "type_member": "Monitor",
      "notify": true
    },
    {
      "user_id": ObjectId("56e7461eca76b61efa435c1e"),
      "type_member": "Estudante",
      "notify": true
    },
    {
      "user_id": ObjectId("56e746e5ca76b61efa435c1f"),
      "type_member": "Estudante",
      "notify": true
    },
    {
      "user_id": ObjectId("56e74798ca76b61efa435c20"),
      "type_member": "Estudante",
      "notify": true
    }
  ],
  "goals": [
    {
      "name": "Introdução a Banco de Dados",
      "description": "Primeiros passos",
      "date_begin": ISODate("2016-03-15T02:34:08.599Z"),
      "date_dream": ISODate("2016-03-15T02:34:08.599Z"),
      "date_end": ISODate("2016-03-15T02:34:08.599Z"),
      "realocate": false,
      "expired": false,
      "historic": [ ],
      "tags": [
        "materia",
        "dados",
        "requisito"
      ],
      "activities": [
        {
          "activity_id": ObjectId("56e77464480ade334f1ef69b")
        },
        {
          "activity_id": ObjectId("56e77458480ade334f1ef69a")
        }
      ]
    }
  ],
  "views": 0
}
{
  "_id": ObjectId("56e8b1fc01e50323debd7b4e"),
  "name": "Estágio Supervisionado",
  "description": "Disciplina Optativa",
  "date_begin": ISODate("2016-03-16T01:08:08.618Z"),
  "date_dream": ISODate("2016-03-16T01:08:08.618Z"),
  "date_end": ISODate("2016-03-16T01:08:08.618Z"),
  "visible": true,
  "realocate": false,
  "expired": false,
  "vizualizable_mod": false,
  "tags": [
    "computação",
    "estágio",
    "optativa",
    "tag4"
  ],
  "members": [
    {
      "user_id": ObjectId("56e7461eca76b61efa435c1e"),
      "type_member": "Professor",
      "notify": true
    },
    {
      "user_id": ObjectId("56e746e5ca76b61efa435c1f"),
      "type_member": "Monitor",
      "notify": true
    },
    {
      "user_id": ObjectId("56e74798ca76b61efa435c20"),
      "type_member": "Estudante",
      "notify": true
    },
    {
      "user_id": ObjectId("56e74bc1ca76b61efa435c21"),
      "type_member": "Estudante",
      "notify": true
    },
    {
      "user_id": ObjectId("56e74f11ca76b61efa435c22"),
      "type_member": "Estudante",
      "notify": true
    }
  ],
  "goals": [
    {
      "name": "Prática",
      "description": "Colocar em prática o que aprendeu no curso",
      "date_begin": ISODate("2016-03-16T01:08:08.618Z"),
      "date_dream": ISODate("2016-03-16T01:08:08.618Z"),
      "date_end": ISODate("2016-03-16T01:08:08.618Z"),
      "realocate": false,
      "expired": false,
      "historic": [ ],
      "tags": [
        "prática",
        "estágio",
        "trabalho"
      ],
      "activities": [ ]
    }
  ],
  "views": 0
}
{
  "_id": ObjectId("56e8b42001e50323debd7b51"),
  "name": "CONSTRUÇÃO E ANÁLISE DE ALGORITMOS",
  "description": "Semestre 3",
  "date_begin": ISODate("2016-03-16T01:16:57.464Z"),
  "date_dream": ISODate("2016-03-16T01:16:57.464Z"),
  "date_end": ISODate("2016-03-16T01:16:57.464Z"),
  "visible": true,
  "realocate": false,
  "expired": false,
  "vizualizable_mod": false,
  "tags": [
    "computação",
    "análise",
    "algoritmos",
    "tag5"
  ],
  "members": [
    {
      "user_id": ObjectId("56e746e5ca76b61efa435c1f"),
      "type_member": "Professor",
      "notify": true
    },
    {
      "user_id": ObjectId("56e74798ca76b61efa435c20"),
      "type_member": "Monitor",
      "notify": true
    },
    {
      "user_id": ObjectId("56e74bc1ca76b61efa435c21"),
      "type_member": "Estudante",
      "notify": true
    },
    {
      "user_id": ObjectId("56e74f11ca76b61efa435c22"),
      "type_member": "Estudante",
      "notify": true
    },
    {
      "user_id": ObjectId("56e7503cca76b61efa435c23"),
      "type_member": "Estudante",
      "notify": true
    }
  ],
  "goals": [
    {
      "name": "Introdução a Banco de Dados",
      "description": "Primeiros passos",
      "date_begin": ISODate("2016-03-16T01:16:57.464Z"),
      "date_dream": ISODate("2016-03-16T01:16:57.464Z"),
      "date_end": ISODate("2016-03-16T01:16:57.464Z"),
      "realocate": false,
      "expired": false,
      "historic": [ ],
      "tags": [
        "materia",
        "dados",
        "requisito"
      ],
      "activities": [
        {
          "activity_id": ObjectId("56e8b39b01e50323debd7b4f")
        },
        {
          "activity_id": ObjectId("56e8b3ac01e50323debd7b50")
        }
      ]
    }
  ],
  "views": 0
}
Fetched 5 record(s) in 37ms
```
### 3. Adicione 2 membros diferentes para cada projeto.
``` js
var member = db.users.find().toArray();

var i = 0;
for (var x = 0; x < 5; x++) {
  var project = db.projects.find().limit(1).skip(x).toArray();

  var member1 = {
    type_member : "member",
    user_id : member[9 - i]._id,
    notify : false
  };

  var member2 = {
    type_member : "member",
    user_id : member[8 - i]._id,
    notify : false
  };

  i+= 2;

  db.projects.update({ _id: project[0]._id }, { $push: { members: member1 } });
  db.projects.update({ _id: project[0]._id }, { $push: { members: member2 } });
}

db.projects.find({}, { name: 1, members: 1});
{
  "_id": ObjectId("56e766b1480ade334f1ef696"),
  "name": "CALCULO I",
  "members": [
    {
      "user_id": ObjectId("56e6db209c0bb73f24b84ae3"),
      "type_member": "Professor",
      "notify": true
    },
    {
      "user_id": ObjectId("56e6dd219c0bb73f24b84ae4"),
      "type_member": "Estudante",
      "notify": true
    },
    {
      "user_id": ObjectId("56e7416fca76b61efa435c1c"),
      "type_member": "Monitor",
      "notify": true
    },
    {
      "user_id": ObjectId("56e7450fca76b61efa435c1d"),
      "type_member": "Estudante",
      "notify": true
    },
    {
      "user_id": ObjectId("56e7461eca76b61efa435c1e"),
      "type_member": "Estudante",
      "notify": true
    },
    {
      "type_member": "member",
      "user_id": ObjectId("56e7503cca76b61efa435c23"),
      "notify": false
    },
    {
      "type_member": "member",
      "user_id": ObjectId("56e74f11ca76b61efa435c22"),
      "notify": false
    }
  ]
}
{
  "_id": ObjectId("56e76ef0480ade334f1ef699"),
  "name": "LOGICA DE PROGRAMACAO",
  "members": [
    {
      "user_id": ObjectId("56e6dd219c0bb73f24b84ae4"),
      "type_member": "Professor",
      "notify": true
    },
    {
      "user_id": ObjectId("56e7416fca76b61efa435c1c"),
      "type_member": "Monitor",
      "notify": true
    },
    {
      "user_id": ObjectId("56e7450fca76b61efa435c1d"),
      "type_member": "Estudante",
      "notify": true
    },
    {
      "user_id": ObjectId("56e7461eca76b61efa435c1e"),
      "type_member": "Estudante",
      "notify": true
    },
    {
      "user_id": ObjectId("56e746e5ca76b61efa435c1f"),
      "type_member": "Estudante",
      "notify": true
    },
    {
      "type_member": "member",
      "user_id": ObjectId("56e74bc1ca76b61efa435c21"),
      "notify": false
    },
    {
      "type_member": "member",
      "user_id": ObjectId("56e74798ca76b61efa435c20"),
      "notify": false
    }
  ]
}
{
  "_id": ObjectId("56e774a6480ade334f1ef69c"),
  "name": "BANCO DE DADOS I",
  "members": [
    {
      "user_id": ObjectId("56e7416fca76b61efa435c1c"),
      "type_member": "Professor",
      "notify": true
    },
    {
      "user_id": ObjectId("56e7450fca76b61efa435c1d"),
      "type_member": "Monitor",
      "notify": true
    },
    {
      "user_id": ObjectId("56e7461eca76b61efa435c1e"),
      "type_member": "Estudante",
      "notify": true
    },
    {
      "user_id": ObjectId("56e746e5ca76b61efa435c1f"),
      "type_member": "Estudante",
      "notify": true
    },
    {
      "user_id": ObjectId("56e74798ca76b61efa435c20"),
      "type_member": "Estudante",
      "notify": true
    },
    {
      "type_member": "member",
      "user_id": ObjectId("56e746e5ca76b61efa435c1f"),
      "notify": false
    },
    {
      "type_member": "member",
      "user_id": ObjectId("56e7461eca76b61efa435c1e"),
      "notify": false
    }
  ]
}
{
  "_id": ObjectId("56e8b1fc01e50323debd7b4e"),
  "name": "Estágio Supervisionado",
  "members": [
    {
      "user_id": ObjectId("56e7461eca76b61efa435c1e"),
      "type_member": "Professor",
      "notify": true
    },
    {
      "user_id": ObjectId("56e746e5ca76b61efa435c1f"),
      "type_member": "Monitor",
      "notify": true
    },
    {
      "user_id": ObjectId("56e74798ca76b61efa435c20"),
      "type_member": "Estudante",
      "notify": true
    },
    {
      "user_id": ObjectId("56e74bc1ca76b61efa435c21"),
      "type_member": "Estudante",
      "notify": true
    },
    {
      "user_id": ObjectId("56e74f11ca76b61efa435c22"),
      "type_member": "Estudante",
      "notify": true
    },
    {
      "type_member": "member",
      "user_id": ObjectId("56e7450fca76b61efa435c1d"),
      "notify": false
    },
    {
      "type_member": "member",
      "user_id": ObjectId("56e7416fca76b61efa435c1c"),
      "notify": false
    }
  ]
}
{
  "_id": ObjectId("56e8b42001e50323debd7b51"),
  "name": "CONSTRUÇÃO E ANÁLISE DE ALGORITMOS",
  "members": [
    {
      "user_id": ObjectId("56e746e5ca76b61efa435c1f"),
      "type_member": "Professor",
      "notify": true
    },
    {
      "user_id": ObjectId("56e74798ca76b61efa435c20"),
      "type_member": "Monitor",
      "notify": true
    },
    {
      "user_id": ObjectId("56e74bc1ca76b61efa435c21"),
      "type_member": "Estudante",
      "notify": true
    },
    {
      "user_id": ObjectId("56e74f11ca76b61efa435c22"),
      "type_member": "Estudante",
      "notify": true
    },
    {
      "user_id": ObjectId("56e7503cca76b61efa435c23"),
      "type_member": "Estudante",
      "notify": true
    },
    {
      "type_member": "member",
      "user_id": ObjectId("56e6dd219c0bb73f24b84ae4"),
      "notify": false
    },
    {
      "type_member": "member",
      "user_id": ObjectId("56e6db209c0bb73f24b84ae3"),
      "notify": false
    }
  ]
}
Fetched 5 record(s) in 27ms
```
### 4. Adicione 1 comentário em cada atividade, deixe apenas 1 projeto sem.
``` js
var qnt = var qnt = db.activities.count();

for (x = 0; x < qnt; x++) {
  var aux = db.activities.find().limit(1).skip(x).toArray();

  var comment = {
    text: 'New comment',
    date_comment: new Date(),
    members: [
      { 'user_id': aux[0].members[0].user_id,
        'notify': true,
        'type_member': aux[0].members[0].type_member
      }
    ],
    files: [],
  };

  db.activities.update( { _id: aux[0]._id }, { $push: { comments: comment } } );
}

db.activities.find({}, { comments: 1 });
{
  "_id": ObjectId("56e7621cca76b61efa435c24"),
  "comments": [
    {
      "text": "Exercicio 01",
      "date_comment": ISODate("2016-03-15T01:14:51.227Z"),
      "files": [
        {
          "path": "teste/exemplos01",
          "weight": 5,
          "name": "Primeira Atividade"
        }
      ],
      "members": [
        {
          "user_id": ObjectId("56e6db209c0bb73f24b84ae3"),
          "notify": true,
          "type_member": "Professor"
        }
      ]
    },
    {
      "text": "New comment",
      "date_comment": ISODate("2016-03-17T23:44:28.643Z"),
      "members": [
        {
          "user_id": ObjectId("56e6dd219c0bb73f24b84ae4"),
          "notify": true,
          "type_member": "Estudante"
        }
      ],
      "files": [ ]
    }
  ]
}
{
  "_id": ObjectId("56e7627aca76b61efa435c25"),
  "comments": [
    {
      "text": "Exercicio 02",
      "date_comment": ISODate("2016-03-15T01:16:35.170Z"),
      "files": [
        {
          "path": "teste/exemplos01",
          "weight": 5,
          "name": "Segunda Atividade"
        }
      ],
      "members": [
        {
          "user_id": ObjectId("56e6db209c0bb73f24b84ae3"),
          "notify": true,
          "type_member": "Professor"
        }
      ]
    },
    {
      "text": "New comment",
      "date_comment": ISODate("2016-03-17T23:44:29.024Z"),
      "members": [
        {
          "user_id": ObjectId("56e6dd219c0bb73f24b84ae4"),
          "notify": true,
          "type_member": "Estudante"
        }
      ],
      "files": [ ]
    }
  ]
}
{
  "_id": ObjectId("56e76e52480ade334f1ef697"),
  "comments": [
    {
      "text": "Exercicio 01",
      "date_comment": ISODate("2016-03-15T02:07:09.615Z"),
      "files": [
        {
          "path": "teste/variaveis01",
          "weight": 5,
          "name": "Primeira Atividade"
        }
      ],
      "members": [
        {
          "user_id": ObjectId("56e7416fca76b61efa435c1c"),
          "notify": true,
          "type_member": "Professor"
        }
      ]
    },
    {
      "text": "New comment",
      "date_comment": ISODate("2016-03-17T23:44:29.028Z"),
      "members": [
        {
          "user_id": ObjectId("56e7450fca76b61efa435c1d"),
          "notify": true,
          "type_member": "Estudante"
        }
      ],
      "files": [ ]
    }
  ]
}
{
  "_id": ObjectId("56e76e96480ade334f1ef698"),
  "comments": [
    {
      "text": "Exercicio 02",
      "date_comment": ISODate("2016-03-15T02:08:20.221Z"),
      "files": [
        {
          "path": "teste/vetor02",
          "weight": 5,
          "name": "Segunda Atividade"
        }
      ],
      "members": [
        {
          "user_id": ObjectId("56e6dd219c0bb73f24b84ae4"),
          "notify": true,
          "type_member": "Professor"
        }
      ]
    },
    {
      "text": "New comment",
      "date_comment": ISODate("2016-03-17T23:44:29.032Z"),
      "members": [
        {
          "user_id": ObjectId("56e7450fca76b61efa435c1d"),
          "notify": true,
          "type_member": "Estudante"
        }
      ],
      "files": [ ]
    }
  ]
}
{
  "_id": ObjectId("56e77458480ade334f1ef69a"),
  "comments": [
    {
      "text": "Exercício 01",
      "date_comment": ISODate("2016-03-15T02:32:50.725Z"),
      "files": [
        {
          "path": "teste/nosql01",
          "weight": 5,
          "name": "Primeira Atividade"
        }
      ],
      "members": [
        {
          "user_id": ObjectId("56e7450fca76b61efa435c1d"),
          "notify": true,
          "type_member": "Monitor"
        }
      ]
    },
    {
      "text": "New comment",
      "date_comment": ISODate("2016-03-17T23:44:29.039Z"),
      "members": [
        {
          "user_id": ObjectId("56e7461eca76b61efa435c1e"),
          "notify": true,
          "type_member": "Estudante"
        }
      ],
      "files": [ ]
    }
  ]
}
{
  "_id": ObjectId("56e77464480ade334f1ef69b"),
  "comments": [
    {
      "text": "Exercício 02",
      "date_comment": ISODate("2016-03-15T02:33:05.496Z"),
      "files": [
        {
          "path": "teste/mysql02",
          "weight": 5,
          "name": "Segunda Atividade"
        }
      ],
      "members": [
        {
          "user_id": ObjectId("56e7416fca76b61efa435c1c"),
          "notify": true,
          "type_member": "Professor"
        }
      ]
    },
    {
      "text": "New comment",
      "date_comment": ISODate("2016-03-17T23:44:29.043Z"),
      "members": [
        {
          "user_id": ObjectId("56e7461eca76b61efa435c1e"),
          "notify": true,
          "type_member": "Estudante"
        }
      ],
      "files": [ ]
    }
  ]
}
{
  "_id": ObjectId("56e8b39b01e50323debd7b4f"),
  "comments": [
    {
      "text": "Exercício 01",
      "date_comment": ISODate("2016-03-16T01:15:03.532Z"),
      "files": [
        {
          "path": "teste/divisaoeconquista01",
          "weight": 5,
          "name": "Primeira Atividade"
        }
      ],
      "members": [
        {
          "user_id": ObjectId("56e74798ca76b61efa435c20"),
          "notify": true,
          "type_member": "Monitor"
        }
      ]
    },
    {
      "text": "New comment",
      "date_comment": ISODate("2016-03-17T23:44:29.050Z"),
      "members": [
        {
          "user_id": ObjectId("56e74bc1ca76b61efa435c21"),
          "notify": true,
          "type_member": "Estudante"
        }
      ],
      "files": [ ]
    }
  ]
}
{
  "_id": ObjectId("56e8b3ac01e50323debd7b50"),
  "comments": [
    {
      "text": "Exercício 02",
      "date_comment": ISODate("2016-03-16T01:15:20.650Z"),
      "files": [
        {
          "path": "teste/complexidade02",
          "weight": 5,
          "name": "Segunda Atividade"
        }
      ],
      "members": [
        {
          "user_id": ObjectId("56e746e5ca76b61efa435c1f"),
          "notify": true,
          "type_member": "Professor"
        }
      ]
    },
    {
      "text": "New comment",
      "date_comment": ISODate("2016-03-17T23:44:29.054Z"),
      "members": [
        {
          "user_id": ObjectId("56e74bc1ca76b61efa435c21"),
          "notify": true,
          "type_member": "Estudante"
        }
      ],
      "files": [ ]
    }
  ]
}
Fetched 8 record(s) in 26ms
```
### 5. Adicione 1 projeto inteiro com UPSERT.
``` js
var query = { name: /CALCULO II/i };

var mod = {
  $set: { active: true },
  $setOnInsert: {
    "name": "CALCULO II",
    "description": "Semestre 2",
    "date_begin": ISODate("2016-03-15T01:34:18.906Z"),
    "date_dream": ISODate("2016-03-15T01:34:18.906Z"),
    "date_end": ISODate("2016-03-15T01:34:18.906Z"),
    "visible": true,
    "realocate": false,
    "expired": false,
    "vizualizable_mod": false,
    "tags": [
      "matematica",
      "calculo",
      "projetocad",
      "tag1",
      "tag1"
    ],
    "members": [
      {
        "user_id": ObjectId("56e6db209c0bb73f24b84ae3"),
        "type_member": "Professor",
        "notify": true
      },
      {
        "user_id": ObjectId("56e6dd219c0bb73f24b84ae4"),
        "type_member": "Estudante",
        "notify": true
      },
      {
        "user_id": ObjectId("56e7416fca76b61efa435c1c"),
        "type_member": "Monitor",
        "notify": true
      },
      {
        "user_id": ObjectId("56e7450fca76b61efa435c1d"),
        "type_member": "Estudante",
        "notify": true
      },
      {
        "user_id": ObjectId("56e7461eca76b61efa435c1e"),
        "type_member": "Estudante",
        "notify": true
      },
      {
        "type_member": "member",
        "user_id": ObjectId("56e7503cca76b61efa435c23"),
        "notify": false
      },
      {
        "type_member": "member",
        "user_id": ObjectId("56e74f11ca76b61efa435c22"),
        "notify": false
      }
    ],
    "goals": [
      {
        "name": "Limites e Derivadas",
        "description": "Tecnicas e Exercicios sobre Limites e Derivadas",
        "date_begin": ISODate("2016-03-15T01:34:18.907Z"),
        "date_dream": ISODate("2016-03-15T01:34:18.907Z"),
        "date_end": ISODate("2016-03-15T01:34:18.907Z"),
        "realocate": false,
        "expired": false,
        "historic": [ ],
        "tags": [
          "materia",
          "limite",
          "derivada"
        ],
        "activities": []
      }
    ],
    "views": 0
  }
}

var option = { upsert: true };

db.projects.update(query, mod, option);

Updated 1 new record(s) in 6ms
WriteResult({
  "nMatched": 0,
  "nUpserted": 1,
  "nModified": 0,
  "_id": ObjectId("56eb44a764aad6b3a5ca0757")
})

```

## Delete - remoção
### 1. Apague todos os projetos que não possuam tags.
``` js
db.projects.remove({tags: {$size: 0}}, {multi: 1})

Removed 0 record(s) in 20ms
WriteResult({
  "nRemoved": 0
})

```
### 2. Apague todos os projetos que não possuam comentários nas atividades.
``` js
var activities = db.activities.find({ comments: { $exists: 0 } }).toArray();

activities.forEach(function(activity) {
    db.projects.remove({'goals.activity.activity_id' : { $eq : activity._id } }, { multi: 1 });
});
```
### 3. Apague todos os projetos que não possuam atividades.
``` js
db.projects.remove({ 'goal.activity' : {$size: 0} });

Removed 0 record(s) in 2ms
WriteResult({
  "nRemoved": 0
})
```
### 4. Escolha 2 usuário e apague todos os projetos em que os 2 fazem parte.
``` js
var users = db.users.find({}, { _id: 1}).limit(2).toArray();

var usersDelete = [];
users.forEach(function(user) {
    usersDelete.push(user._id);
});
db.projects.remove({'members.user_id': {$in: usersDelete}}, {multi:1});

Removed 4 record(s) in 3ms
WriteResult({
  "nRemoved": 4
})
```
### 5. Apague todos os projetos que possuam uma determinada tag em goal.
``` js
db.projects.remove({ 'goals.tags': {$eq: 'materia'}  }, {multi: 1})

Removed 1 record(s) in 2ms
WriteResult({
  "nRemoved": 1
})
```

## Gerenciamento de usuários
### 1. Crie um usuário com permissões APENAS de Leitura.
``` js
db.createUser({user: 'userRead', pwd: 'userRead@bemean', roles: ['read'] })

Successfully added user: {
  "user": "userRead",
  "roles": [
    "read"
  ]
}

```
### 2. Crie um usuário com permissões de Escrita e Leitura.
``` js
db.createUser({user: 'userReadWrite', pwd: 'userReadWrite@bemean', roles: ['readWrite'] })

Successfully added user: {
  "user": "userReadWrite",
  "roles": [
    "readWrite"
  ]
}

```
### 3. Adicionar o papel ```grantRolesToUser``` e ```revokeRole``` para o usuário com Escrita e Leitura.
``` js
db.runCommand({ createRole: 'grantRolesToUser',
  privileges: [ { resource: { db: 'projeto-final', collection: '' }, actions: ['grantRole'] } ],
  roles: [ 'readWrite' ]
})

{ "ok": 1 }

db.runCommand({ createRole: 'revokeRole',
  privileges: [ { resource: { db: 'projeto-final', collection: '' }, actions: ['revokeRole'] } ],
  roles: [ 'readWrite' ]
})

{ "ok": 1 }

db.grantRolesToUser( 'userReadWrite', [ 'grantRolesToUser', 'revokeRole' ] )

```
### 4. Remover o papel ```grantRolesToUser``` para o usuário com Escrita e Leitura.
``` js
db.revokeRolesFromUser( 'userReadWrite', [ 'grantRolesToUser' ] )
```
### 5. Listar todos os usuários com seus papéis e ações.
``` js
db.runCommand({usersInfo: 1})

{
  "users": [
    {
      "_id": "projeto-final.userRead",
      "user": "userRead",
      "db": "projeto-final",
      "roles": [
        {
          "role": "read",
          "db": "projeto-final"
        }
      ]
    },
    {
      "_id": "projeto-final.userReadWrite",
      "user": "userReadWrite",
      "db": "projeto-final",
      "roles": [
        {
          "role": "revokeRole",
          "db": "projeto-final"
        },
        {
          "role": "readWrite",
          "db": "projeto-final"
        }
      ]
    }
  ],
  "ok": 1
}

```

## Sharding
### Config Server
``` js
aim@aim:~$ cd /
aim@aim:/$ mkdir data/configdb
aim@aim:/$ mongod --configsvr --port 27010

2016-03-19T22:37:59.362-0300 I CONTROL  [initandlisten] MongoDB starting : pid=6534 port=27010 dbpath=/data/configdb master=1 64-bit host=aim
2016-03-19T22:37:59.363-0300 I CONTROL  [initandlisten] db version v3.2.4
2016-03-19T22:37:59.363-0300 I CONTROL  [initandlisten] git version: e2ee9ffcf9f5a94fad76802e28cc978718bb7a30
2016-03-19T22:37:59.363-0300 I CONTROL  [initandlisten] OpenSSL version: OpenSSL 1.0.1f 6 Jan 2014
2016-03-19T22:37:59.363-0300 I CONTROL  [initandlisten] allocator: tcmalloc
2016-03-19T22:37:59.363-0300 I CONTROL  [initandlisten] modules: none
2016-03-19T22:37:59.363-0300 I CONTROL  [initandlisten] build environment:
2016-03-19T22:37:59.363-0300 I CONTROL  [initandlisten]     distmod: ubuntu1404
2016-03-19T22:37:59.363-0300 I CONTROL  [initandlisten]     distarch: x86_64
2016-03-19T22:37:59.363-0300 I CONTROL  [initandlisten]     target_arch: x86_64
2016-03-19T22:37:59.363-0300 I CONTROL  [initandlisten] options: { net: { port: 27010 }, sharding: { clusterRole: "configsvr" } }
2016-03-19T22:37:59.433-0300 I STORAGE  [initandlisten] wiredtiger_open config: create,cache_size=1G,session_max=20000,eviction=(threads_max=4),config_base=false,statistics=(fast),log=(enabled=true,archive=true,path=journal,compressor=snappy),file_manager=(close_idle_time=100000),checkpoint=(wait=60,log_size=2GB),statistics_log=(wait=0),
2016-03-19T22:38:00.075-0300 I CONTROL  [initandlisten] 
2016-03-19T22:38:00.075-0300 I CONTROL  [initandlisten] ** WARNING: /sys/kernel/mm/transparent_hugepage/enabled is 'always'.
2016-03-19T22:38:00.075-0300 I CONTROL  [initandlisten] **        We suggest setting it to 'never'
2016-03-19T22:38:00.075-0300 I CONTROL  [initandlisten] 
2016-03-19T22:38:00.075-0300 I CONTROL  [initandlisten] ** WARNING: /sys/kernel/mm/transparent_hugepage/defrag is 'always'.
2016-03-19T22:38:00.075-0300 I CONTROL  [initandlisten] **        We suggest setting it to 'never'
2016-03-19T22:38:00.075-0300 I CONTROL  [initandlisten] 
2016-03-19T22:38:00.385-0300 I REPL     [initandlisten] ******
2016-03-19T22:38:00.385-0300 I REPL     [initandlisten] creating replication oplog of size: 5MB...
2016-03-19T22:38:00.473-0300 I STORAGE  [initandlisten] Starting WiredTigerRecordStoreThread local.oplog.$main
2016-03-19T22:38:00.473-0300 I STORAGE  [initandlisten] The size storer reports that the oplog contains 0 records totaling to 0 bytes
2016-03-19T22:38:00.473-0300 I STORAGE  [initandlisten] Scanning the oplog to determine where to place markers for truncation
2016-03-19T22:38:01.014-0300 I REPL     [initandlisten] ******
2016-03-19T22:38:01.014-0300 I FTDC     [initandlisten] Initializing full-time diagnostic data capture with directory '/data/configdb/diagnostic.data'
2016-03-19T22:38:01.014-0300 I NETWORK  [HostnameCanonicalizationWorker] Starting hostname canonicalization worker
2016-03-19T22:38:01.138-0300 I NETWORK  [initandlisten] waiting for connections on port 27010

```
### Router
``` js
aim@aim:/$ mongos -configdb localhost:27010 --port 27011

2016-03-19T22:39:51.502-0300 W SHARDING [main] Running a sharded cluster with fewer than 3 config servers should only be done for testing purposes and is not recommended for production.
2016-03-19T22:39:51.560-0300 I SHARDING [mongosMain] MongoS version 3.2.4 starting: pid=6594 port=27011 64-bit host=aim (--help for usage)
2016-03-19T22:39:51.560-0300 I CONTROL  [mongosMain] db version v3.2.4
2016-03-19T22:39:51.560-0300 I CONTROL  [mongosMain] git version: e2ee9ffcf9f5a94fad76802e28cc978718bb7a30
2016-03-19T22:39:51.560-0300 I CONTROL  [mongosMain] OpenSSL version: OpenSSL 1.0.1f 6 Jan 2014
2016-03-19T22:39:51.560-0300 I CONTROL  [mongosMain] allocator: tcmalloc
2016-03-19T22:39:51.560-0300 I CONTROL  [mongosMain] modules: none
2016-03-19T22:39:51.560-0300 I CONTROL  [mongosMain] build environment:
2016-03-19T22:39:51.560-0300 I CONTROL  [mongosMain]     distmod: ubuntu1404
2016-03-19T22:39:51.560-0300 I CONTROL  [mongosMain]     distarch: x86_64
2016-03-19T22:39:51.560-0300 I CONTROL  [mongosMain]     target_arch: x86_64
2016-03-19T22:39:51.560-0300 I CONTROL  [mongosMain] options: { net: { port: 27011 }, sharding: { configDB: "localhost:27010" } }
2016-03-19T22:39:51.629-0300 I SHARDING [mongosMain] Updating config server connection string to: localhost:27010
2016-03-19T22:39:51.647-0300 I SHARDING [LockPinger] creating distributed lock ping thread for localhost:27010 and process aim:27011:1458437991:-1909948165 (sleeping for 30000ms)
2016-03-19T22:39:51.842-0300 I SHARDING [LockPinger] cluster localhost:27010 pinged successfully at 2016-03-19T22:39:51.649-0300 by distributed lock pinger 'localhost:27010/aim:27011:1458437991:-1909948165', sleeping for 30000ms
2016-03-19T22:39:52.331-0300 I SHARDING [mongosMain] distributed lock 'configUpgrade/aim:27011:1458437991:-1909948165' acquired for 'initializing config database to new format v6', ts : 56edff68946bd9fbfaa06a44
2016-03-19T22:39:52.333-0300 I SHARDING [mongosMain] initializing config server version to 6
2016-03-19T22:39:52.333-0300 I SHARDING [mongosMain] writing initial config version at v6
2016-03-19T22:39:52.551-0300 I SHARDING [mongosMain] initialization of config server to v6 successful
2016-03-19T22:39:52.552-0300 I SHARDING [mongosMain] distributed lock 'configUpgrade/aim:27011:1458437991:-1909948165' unlocked. 
2016-03-19T22:39:54.411-0300 I NETWORK  [HostnameCanonicalizationWorker] Starting hostname canonicalization worker
2016-03-19T22:39:54.411-0300 I SHARDING [Balancer] about to contact config servers and shards
2016-03-19T22:39:54.412-0300 I SHARDING [Balancer] config servers and shards contacted successfully
2016-03-19T22:39:54.412-0300 I SHARDING [Balancer] balancer id: aim:27011 started
2016-03-19T22:39:54.478-0300 I NETWORK  [mongosMain] waiting for connections on port 27011
```
### Shards
``` js
aim@aim:/$ cd /
aim@aim:/$ mkdir /data/shard1 && mkdir /data/shard2 && mkdir /data/shard3
```

#### Shards 1
``` js
aim@aim:/$ mongod --port 27012 --dbpath /data/shard1

2016-03-19T22:44:36.444-0300 I CONTROL  [initandlisten] MongoDB starting : pid=6686 port=27012 dbpath=/data/shard1 64-bit host=aim
2016-03-19T22:44:36.444-0300 I CONTROL  [initandlisten] db version v3.2.4
2016-03-19T22:44:36.444-0300 I CONTROL  [initandlisten] git version: e2ee9ffcf9f5a94fad76802e28cc978718bb7a30
2016-03-19T22:44:36.444-0300 I CONTROL  [initandlisten] OpenSSL version: OpenSSL 1.0.1f 6 Jan 2014
2016-03-19T22:44:36.444-0300 I CONTROL  [initandlisten] allocator: tcmalloc
2016-03-19T22:44:36.444-0300 I CONTROL  [initandlisten] modules: none
2016-03-19T22:44:36.444-0300 I CONTROL  [initandlisten] build environment:
2016-03-19T22:44:36.444-0300 I CONTROL  [initandlisten]     distmod: ubuntu1404
2016-03-19T22:44:36.445-0300 I CONTROL  [initandlisten]     distarch: x86_64
2016-03-19T22:44:36.445-0300 I CONTROL  [initandlisten]     target_arch: x86_64
2016-03-19T22:44:36.445-0300 I CONTROL  [initandlisten] options: { net: { port: 27012 }, storage: { dbPath: "/data/shard1" } }
2016-03-19T22:44:36.536-0300 I STORAGE  [initandlisten] wiredtiger_open config: create,cache_size=1G,session_max=20000,eviction=(threads_max=4),config_base=false,statistics=(fast),log=(enabled=true,archive=true,path=journal,compressor=snappy),file_manager=(close_idle_time=100000),checkpoint=(wait=60,log_size=2GB),statistics_log=(wait=0),
2016-03-19T22:44:37.177-0300 I CONTROL  [initandlisten] 
2016-03-19T22:44:37.177-0300 I CONTROL  [initandlisten] ** WARNING: /sys/kernel/mm/transparent_hugepage/enabled is 'always'.
2016-03-19T22:44:37.177-0300 I CONTROL  [initandlisten] **        We suggest setting it to 'never'
2016-03-19T22:44:37.177-0300 I CONTROL  [initandlisten] 
2016-03-19T22:44:37.177-0300 I CONTROL  [initandlisten] ** WARNING: /sys/kernel/mm/transparent_hugepage/defrag is 'always'.
2016-03-19T22:44:37.177-0300 I CONTROL  [initandlisten] **        We suggest setting it to 'never'
2016-03-19T22:44:37.177-0300 I CONTROL  [initandlisten] 
2016-03-19T22:44:37.178-0300 I FTDC     [initandlisten] Initializing full-time diagnostic data capture with directory '/data/shard1/diagnostic.data'
2016-03-19T22:44:37.178-0300 I NETWORK  [HostnameCanonicalizationWorker] Starting hostname canonicalization worker
2016-03-19T22:44:37.357-0300 I NETWORK  [initandlisten] waiting for connections on port 27012
```

#### Shards 2
``` js
aim@aim:/$ mongod --port 27013 --dbpath /data/shard2

2016-03-19T22:47:54.871-0300 I CONTROL  [initandlisten] MongoDB starting : pid=6720 port=27013 dbpath=/data/shard2 64-bit host=aim
2016-03-19T22:47:54.871-0300 I CONTROL  [initandlisten] db version v3.2.4
2016-03-19T22:47:54.871-0300 I CONTROL  [initandlisten] git version: e2ee9ffcf9f5a94fad76802e28cc978718bb7a30
2016-03-19T22:47:54.871-0300 I CONTROL  [initandlisten] OpenSSL version: OpenSSL 1.0.1f 6 Jan 2014
2016-03-19T22:47:54.871-0300 I CONTROL  [initandlisten] allocator: tcmalloc
2016-03-19T22:47:54.871-0300 I CONTROL  [initandlisten] modules: none
2016-03-19T22:47:54.871-0300 I CONTROL  [initandlisten] build environment:
2016-03-19T22:47:54.871-0300 I CONTROL  [initandlisten]     distmod: ubuntu1404
2016-03-19T22:47:54.871-0300 I CONTROL  [initandlisten]     distarch: x86_64
2016-03-19T22:47:54.871-0300 I CONTROL  [initandlisten]     target_arch: x86_64
2016-03-19T22:47:54.871-0300 I CONTROL  [initandlisten] options: { net: { port: 27013 }, storage: { dbPath: "/data/shard2" } }
2016-03-19T22:47:54.963-0300 I STORAGE  [initandlisten] wiredtiger_open config: create,cache_size=1G,session_max=20000,eviction=(threads_max=4),config_base=false,statistics=(fast),log=(enabled=true,archive=true,path=journal,compressor=snappy),file_manager=(close_idle_time=100000),checkpoint=(wait=60,log_size=2GB),statistics_log=(wait=0),
2016-03-19T22:47:55.609-0300 I CONTROL  [initandlisten] 
2016-03-19T22:47:55.609-0300 I CONTROL  [initandlisten] ** WARNING: /sys/kernel/mm/transparent_hugepage/enabled is 'always'.
2016-03-19T22:47:55.609-0300 I CONTROL  [initandlisten] **        We suggest setting it to 'never'
2016-03-19T22:47:55.609-0300 I CONTROL  [initandlisten] 
2016-03-19T22:47:55.609-0300 I CONTROL  [initandlisten] ** WARNING: /sys/kernel/mm/transparent_hugepage/defrag is 'always'.
2016-03-19T22:47:55.609-0300 I CONTROL  [initandlisten] **        We suggest setting it to 'never'
2016-03-19T22:47:55.609-0300 I CONTROL  [initandlisten] 
2016-03-19T22:47:55.610-0300 I FTDC     [initandlisten] Initializing full-time diagnostic data capture with directory '/data/shard2/diagnostic.data'
2016-03-19T22:47:55.610-0300 I NETWORK  [HostnameCanonicalizationWorker] Starting hostname canonicalization worker
2016-03-19T22:47:55.821-0300 I NETWORK  [initandlisten] waiting for connections on port 27013
```

#### Shards 3
``` js
aim@aim:/$ mongod --port 27014 --dbpath /data/shard3

2016-03-19T22:49:05.229-0300 I CONTROL  [initandlisten] MongoDB starting : pid=6750 port=27014 dbpath=/data/shard3 64-bit host=aim
2016-03-19T22:49:05.229-0300 I CONTROL  [initandlisten] db version v3.2.4
2016-03-19T22:49:05.229-0300 I CONTROL  [initandlisten] git version: e2ee9ffcf9f5a94fad76802e28cc978718bb7a30
2016-03-19T22:49:05.229-0300 I CONTROL  [initandlisten] OpenSSL version: OpenSSL 1.0.1f 6 Jan 2014
2016-03-19T22:49:05.229-0300 I CONTROL  [initandlisten] allocator: tcmalloc
2016-03-19T22:49:05.230-0300 I CONTROL  [initandlisten] modules: none
2016-03-19T22:49:05.230-0300 I CONTROL  [initandlisten] build environment:
2016-03-19T22:49:05.230-0300 I CONTROL  [initandlisten]     distmod: ubuntu1404
2016-03-19T22:49:05.230-0300 I CONTROL  [initandlisten]     distarch: x86_64
2016-03-19T22:49:05.230-0300 I CONTROL  [initandlisten]     target_arch: x86_64
2016-03-19T22:49:05.230-0300 I CONTROL  [initandlisten] options: { net: { port: 27014 }, storage: { dbPath: "/data/shard3" } }
2016-03-19T22:49:05.329-0300 I STORAGE  [initandlisten] wiredtiger_open config: create,cache_size=1G,session_max=20000,eviction=(threads_max=4),config_base=false,statistics=(fast),log=(enabled=true,archive=true,path=journal,compressor=snappy),file_manager=(close_idle_time=100000),checkpoint=(wait=60,log_size=2GB),statistics_log=(wait=0),
2016-03-19T22:49:05.945-0300 I CONTROL  [initandlisten] 
2016-03-19T22:49:05.945-0300 I CONTROL  [initandlisten] ** WARNING: /sys/kernel/mm/transparent_hugepage/enabled is 'always'.
2016-03-19T22:49:05.945-0300 I CONTROL  [initandlisten] **        We suggest setting it to 'never'
2016-03-19T22:49:05.945-0300 I CONTROL  [initandlisten] 
2016-03-19T22:49:05.945-0300 I CONTROL  [initandlisten] ** WARNING: /sys/kernel/mm/transparent_hugepage/defrag is 'always'.
2016-03-19T22:49:05.945-0300 I CONTROL  [initandlisten] **        We suggest setting it to 'never'
2016-03-19T22:49:05.945-0300 I CONTROL  [initandlisten] 
2016-03-19T22:49:05.946-0300 I FTDC     [initandlisten] Initializing full-time diagnostic data capture with directory '/data/shard3/diagnostic.data'
2016-03-19T22:49:05.946-0300 I NETWORK  [HostnameCanonicalizationWorker] Starting hostname canonicalization worker
2016-03-19T22:49:06.202-0300 I NETWORK  [initandlisten] waiting for connections on port 27014
```

#### Registrando shards no router
``` js
aim@aim:/$ mongo --port 27011 --host localhost

MongoDB shell version: 3.2.4
connecting to: localhost:27011/test
Mongo-Hacker 0.0.13
aim:27011(mongos-3.2.4)[mongos] test>

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

```

## Replica
``` js
mkdir /data/rs1 && mkdir /data/rs2 && mkdir /data/rs3
```

### Replica 1
``` js
aim@aim:/$ mongod --replSet replica_set --port 27027 --dbpath /data/rs1

2016-03-19T22:55:31.401-0300 I CONTROL  [initandlisten] MongoDB starting : pid=6857 port=27027 dbpath=/data/rs1 64-bit host=aim
2016-03-19T22:55:31.402-0300 I CONTROL  [initandlisten] db version v3.2.4
2016-03-19T22:55:31.402-0300 I CONTROL  [initandlisten] git version: e2ee9ffcf9f5a94fad76802e28cc978718bb7a30
2016-03-19T22:55:31.402-0300 I CONTROL  [initandlisten] OpenSSL version: OpenSSL 1.0.1f 6 Jan 2014
2016-03-19T22:55:31.402-0300 I CONTROL  [initandlisten] allocator: tcmalloc
2016-03-19T22:55:31.402-0300 I CONTROL  [initandlisten] modules: none
2016-03-19T22:55:31.402-0300 I CONTROL  [initandlisten] build environment:
2016-03-19T22:55:31.402-0300 I CONTROL  [initandlisten]     distmod: ubuntu1404
2016-03-19T22:55:31.402-0300 I CONTROL  [initandlisten]     distarch: x86_64
2016-03-19T22:55:31.402-0300 I CONTROL  [initandlisten]     target_arch: x86_64
2016-03-19T22:55:31.402-0300 I CONTROL  [initandlisten] options: { net: { port: 27027 }, replication: { replSet: "replica_set" }, storage: { dbPath: "/data/rs1" } }
2016-03-19T22:55:31.495-0300 I STORAGE  [initandlisten] wiredtiger_open config: create,cache_size=1G,session_max=20000,eviction=(threads_max=4),config_base=false,statistics=(fast),log=(enabled=true,archive=true,path=journal,compressor=snappy),file_manager=(close_idle_time=100000),checkpoint=(wait=60,log_size=2GB),statistics_log=(wait=0),
2016-03-19T22:55:32.119-0300 I CONTROL  [initandlisten] 
2016-03-19T22:55:32.119-0300 I CONTROL  [initandlisten] ** WARNING: /sys/kernel/mm/transparent_hugepage/enabled is 'always'.
2016-03-19T22:55:32.119-0300 I CONTROL  [initandlisten] **        We suggest setting it to 'never'
2016-03-19T22:55:32.119-0300 I CONTROL  [initandlisten] 
2016-03-19T22:55:32.119-0300 I CONTROL  [initandlisten] ** WARNING: /sys/kernel/mm/transparent_hugepage/defrag is 'always'.
2016-03-19T22:55:32.120-0300 I CONTROL  [initandlisten] **        We suggest setting it to 'never'
2016-03-19T22:55:32.120-0300 I CONTROL  [initandlisten] 
2016-03-19T22:55:32.353-0300 I REPL     [initandlisten] Did not find local voted for document at startup;  NoMatchingDocument: Did not find replica set lastVote document in local.replset.election
2016-03-19T22:55:32.353-0300 I REPL     [initandlisten] Did not find local replica set configuration document at startup;  NoMatchingDocument: Did not find replica set configuration document in local.system.replset
2016-03-19T22:55:32.354-0300 I FTDC     [initandlisten] Initializing full-time diagnostic data capture with directory '/data/rs1/diagnostic.data'
2016-03-19T22:55:32.354-0300 I NETWORK  [HostnameCanonicalizationWorker] Starting hostname canonicalization worker
2016-03-19T22:55:32.569-0300 I NETWORK  [initandlisten] waiting for connections on port 27027

```

### Replica 2
``` js
aim@aim:/$ mongod --replSet replica_set --port 27028 --dbpath /data/rs2

2016-03-19T22:56:47.832-0300 I CONTROL  [initandlisten] MongoDB starting : pid=6890 port=27028 dbpath=/data/rs2 64-bit host=aim
2016-03-19T22:56:47.832-0300 I CONTROL  [initandlisten] db version v3.2.4
2016-03-19T22:56:47.832-0300 I CONTROL  [initandlisten] git version: e2ee9ffcf9f5a94fad76802e28cc978718bb7a30
2016-03-19T22:56:47.832-0300 I CONTROL  [initandlisten] OpenSSL version: OpenSSL 1.0.1f 6 Jan 2014
2016-03-19T22:56:47.832-0300 I CONTROL  [initandlisten] allocator: tcmalloc
2016-03-19T22:56:47.832-0300 I CONTROL  [initandlisten] modules: none
2016-03-19T22:56:47.832-0300 I CONTROL  [initandlisten] build environment:
2016-03-19T22:56:47.832-0300 I CONTROL  [initandlisten]     distmod: ubuntu1404
2016-03-19T22:56:47.832-0300 I CONTROL  [initandlisten]     distarch: x86_64
2016-03-19T22:56:47.832-0300 I CONTROL  [initandlisten]     target_arch: x86_64
2016-03-19T22:56:47.833-0300 I CONTROL  [initandlisten] options: { net: { port: 27028 }, replication: { replSet: "replica_set" }, storage: { dbPath: "/data/rs2" } }
2016-03-19T22:56:47.925-0300 I STORAGE  [initandlisten] wiredtiger_open config: create,cache_size=1G,session_max=20000,eviction=(threads_max=4),config_base=false,statistics=(fast),log=(enabled=true,archive=true,path=journal,compressor=snappy),file_manager=(close_idle_time=100000),checkpoint=(wait=60,log_size=2GB),statistics_log=(wait=0),
2016-03-19T22:56:48.552-0300 I CONTROL  [initandlisten] 
2016-03-19T22:56:48.552-0300 I CONTROL  [initandlisten] ** WARNING: /sys/kernel/mm/transparent_hugepage/enabled is 'always'.
2016-03-19T22:56:48.552-0300 I CONTROL  [initandlisten] **        We suggest setting it to 'never'
2016-03-19T22:56:48.553-0300 I CONTROL  [initandlisten] 
2016-03-19T22:56:48.553-0300 I CONTROL  [initandlisten] ** WARNING: /sys/kernel/mm/transparent_hugepage/defrag is 'always'.
2016-03-19T22:56:48.553-0300 I CONTROL  [initandlisten] **        We suggest setting it to 'never'
2016-03-19T22:56:48.553-0300 I CONTROL  [initandlisten] 
2016-03-19T22:56:48.810-0300 I REPL     [initandlisten] Did not find local voted for document at startup;  NoMatchingDocument: Did not find replica set lastVote document in local.replset.election
2016-03-19T22:56:48.810-0300 I REPL     [initandlisten] Did not find local replica set configuration document at startup;  NoMatchingDocument: Did not find replica set configuration document in local.system.replset
2016-03-19T22:56:48.811-0300 I FTDC     [initandlisten] Initializing full-time diagnostic data capture with directory '/data/rs2/diagnostic.data'
2016-03-19T22:56:48.811-0300 I NETWORK  [HostnameCanonicalizationWorker] Starting hostname canonicalization worker
2016-03-19T22:56:49.084-0300 I NETWORK  [initandlisten] waiting for connections on port 27028
```

### Replica 3
``` js
aim@aim:/$ mongod --replSet replica_set --port 27029 --dbpath /data/rs3

2016-03-19T22:57:44.507-0300 I CONTROL  [initandlisten] MongoDB starting : pid=6926 port=27029 dbpath=/data/rs3 64-bit host=aim
2016-03-19T22:57:44.508-0300 I CONTROL  [initandlisten] db version v3.2.4
2016-03-19T22:57:44.508-0300 I CONTROL  [initandlisten] git version: e2ee9ffcf9f5a94fad76802e28cc978718bb7a30
2016-03-19T22:57:44.508-0300 I CONTROL  [initandlisten] OpenSSL version: OpenSSL 1.0.1f 6 Jan 2014
2016-03-19T22:57:44.508-0300 I CONTROL  [initandlisten] allocator: tcmalloc
2016-03-19T22:57:44.508-0300 I CONTROL  [initandlisten] modules: none
2016-03-19T22:57:44.508-0300 I CONTROL  [initandlisten] build environment:
2016-03-19T22:57:44.508-0300 I CONTROL  [initandlisten]     distmod: ubuntu1404
2016-03-19T22:57:44.508-0300 I CONTROL  [initandlisten]     distarch: x86_64
2016-03-19T22:57:44.508-0300 I CONTROL  [initandlisten]     target_arch: x86_64
2016-03-19T22:57:44.508-0300 I CONTROL  [initandlisten] options: { net: { port: 27029 }, replication: { replSet: "replica_set" }, storage: { dbPath: "/data/rs3" } }
2016-03-19T22:57:44.615-0300 I STORAGE  [initandlisten] wiredtiger_open config: create,cache_size=1G,session_max=20000,eviction=(threads_max=4),config_base=false,statistics=(fast),log=(enabled=true,archive=true,path=journal,compressor=snappy),file_manager=(close_idle_time=100000),checkpoint=(wait=60,log_size=2GB),statistics_log=(wait=0),
2016-03-19T22:57:45.261-0300 I CONTROL  [initandlisten] 
2016-03-19T22:57:45.261-0300 I CONTROL  [initandlisten] ** WARNING: /sys/kernel/mm/transparent_hugepage/enabled is 'always'.
2016-03-19T22:57:45.261-0300 I CONTROL  [initandlisten] **        We suggest setting it to 'never'
2016-03-19T22:57:45.261-0300 I CONTROL  [initandlisten] 
2016-03-19T22:57:45.261-0300 I CONTROL  [initandlisten] ** WARNING: /sys/kernel/mm/transparent_hugepage/defrag is 'always'.
2016-03-19T22:57:45.262-0300 I CONTROL  [initandlisten] **        We suggest setting it to 'never'
2016-03-19T22:57:45.262-0300 I CONTROL  [initandlisten] 
2016-03-19T22:57:45.465-0300 I REPL     [initandlisten] Did not find local voted for document at startup;  NoMatchingDocument: Did not find replica set lastVote document in local.replset.election
2016-03-19T22:57:45.465-0300 I REPL     [initandlisten] Did not find local replica set configuration document at startup;  NoMatchingDocument: Did not find replica set configuration document in local.system.replset
2016-03-19T22:57:45.466-0300 I FTDC     [initandlisten] Initializing full-time diagnostic data capture with directory '/data/rs3/diagnostic.data'
2016-03-19T22:57:45.466-0300 I NETWORK  [HostnameCanonicalizationWorker] Starting hostname canonicalization worker
2016-03-19T22:57:45.655-0300 I NETWORK  [initandlisten] waiting for connections on port 27029
```

### Config
``` js
aim@aim:/$ mongo --port 27027

MongoDB shell version: 3.2.4
connecting to: 127.0.0.1:27027/test
Mongo-Hacker 0.0.13
Server has startup warnings: 
2016-03-19T22:55:32.119-0300 I CONTROL  [initandlisten] 
2016-03-19T22:55:32.119-0300 I CONTROL  [initandlisten] ** WARNING: /sys/kernel/mm/transparent_hugepage/enabled is 'always'.
2016-03-19T22:55:32.119-0300 I CONTROL  [initandlisten] **        We suggest setting it to 'never'
2016-03-19T22:55:32.119-0300 I CONTROL  [initandlisten] 
2016-03-19T22:55:32.119-0300 I CONTROL  [initandlisten] ** WARNING: /sys/kernel/mm/transparent_hugepage/defrag is 'always'.
2016-03-19T22:55:32.120-0300 I CONTROL  [initandlisten] **        We suggest setting it to 'never'
2016-03-19T22:55:32.120-0300 I CONTROL  [initandlisten] 

rsconf = {
   _id: "replica_set",
   members: [
    {
     _id: 0,
     host: "127.0.0.1:27027"
    }
  ]
}

aim:27027(mongod-3.2.4) test> rs.initiate(rsconf)
{
  "ok": 1
}

aim:27027(mongod-3.2.4)[RECOVERING:replica_set] test> rs.add("127.0.0.1:27028")
{
  "ok": 1
}

aim:27027(mongod-3.2.4)[PRIMARY:replica_set] test> rs.add("127.0.0.1:27029")
{
  "ok": 1
}
```