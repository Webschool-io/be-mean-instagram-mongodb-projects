# MongoDb - Projeto Final
**Autor:** Fábio Reghim Calheiros
**Data** 1450837082262

## Para qual sistema você usaria o MogoDB (diferente desse)?

Pra qualquer sistema, o mongoDB por ser um banco de dados de alta performance e com a facilidade de se obter respostas e inserir dados de forma rápida e simples, serve para pqeuenos e grandes projetos.
Utilizando o MongoDB de forma correta, utilizando replicas e shards para garantir a performance e estabilidade, qualquer aplicação estaria segura, o que nos proporciona mais tranquilidade no futuro

## Qual a modelagem da sua coleção de `users`?

users: {
	id,
	type,
	dateRegister,
	username,
	email,
	bio,
	avatarPath,
	password,
	lastAcess,
	disable
}	

## Qual a modelagem da sua coleção de `projects`?

projects: {
  id,
  name,
  description,
  created_time,
  visible,

  members : [{
    id
  }],

  tags: [],

  goals : [{
    name,
    description,
    created_time,

    tags: []

    activities: [{
      id
    }]
  }]
}

## Qual a modelagem da sua coleção retirada de 'projects'?

activities: {
  id,
  name,
  description,
  created_time,

  comments : [{
    text,
    date_coment,
    members: [],
    files: []
  }],

  historics: []
}

## Explicação de porque a modelagem foi feita dessa forma

Ao meu ver, separando a coleção de usuários de projetos, e criando um relação entre si, é possivel se obeter eficiencia de tempo de resposta melhor.
Outro ponto positivo é facilidade com que podemos fazer requisições e obter respostas de forma mais simples e organizada.

Exemplo, buscar todos os projetos de um usuário especifico.

db.projects.find({"member.user_id": ObjectId("566231164d3dcdd17f83d298")})

Caso contrário, não fosse feito dessa forma, uma busca como essa poderia não ser não só mais complicada como isso, como sua performance seria inifinitamente inferior.

Isso vale também para a coleção de atividades, que foi separada devido ao grande numero de inserções que pode ocorrer em um unico projeto.
Para que a consulta obtesse uma quantidade muito grande de informações ao se requisitar informações do projeto, é aconselhavel separa-la da coleção de projects.

## Create - cadastro

### 1 - Cadastre 10 usuários diferentes.

```

 var user1 = {
... type: "user",
... dateRegister: Date.now(),
... username: "fabiocalheiros",
... full_name: "Fábio Calheiros",
... email: "fabiocalheiros@gmail.com",
... bio: "Breve descrição",
... avatarPath: "https://scontent-mia1-1.xx.fbcdn.net/hphotos-xpa1/v/t1.0-9/1381642_608172905891134_1368571562_n.jpg?oh=72d97e473608fa7907bf4bb4befa9b79&oe=56E46FB5",
... password: 1,
... lastAcess: Date.now(),
... disable: 0
... }
fabio-Inspiron-7520(mongod-3.0.7) be-mean-projeto> db.users.save(user1)
Inserted 1 record(s) in 4ms
WriteResult({
  "nInserted": 1
})
fabio-Inspiron-7520(mongod-3.0.7) be-mean-projeto> var user2 = {
... type: "user",
... dateRegister: Date.now(),
... username: "chicao",
... full_name: "Chico Cézar",
... email: "todechico@gmail.com",
... password: 2,
... lastAcess: Date.now(),
... disable: 0
... }
fabio-Inspiron-7520(mongod-3.0.7) be-mean-projeto> db.users.save(user2)
Inserted 1 record(s) in 2ms
WriteResult({
  "nInserted": 1
})
fabio-Inspiron-7520(mongod-3.0.7) be-mean-projeto> var user3 = {
... type: "user",
... dateRegister: Date.now(),
... username: "simsim",
... full_name: "Simone Costa",
... email: "simsim@ig.com.br",
... password: 3,
... lastAcess: Date.now(),
... disable: 0
... }
fabio-Inspiron-7520(mongod-3.0.7) be-mean-projeto> db.users.save(user3)
Inserted 1 record(s) in 2ms
WriteResult({
  "nInserted": 1
})
fabio-Inspiron-7520(mongod-3.0.7) be-mean-projeto> var user4 = {
... type: "user",
... dateRegister: Date.now(),
... username: "skyzito",
... full_name: "Diego Bolina",
... email: "diego.bolina@gmail.com",
... password: 4,
... lastAcess: Date.now(),
... disable: 0
... }
fabio-Inspiron-7520(mongod-3.0.7) be-mean-projeto> db.users.save(user4)
Inserted 1 record(s) in 2ms
WriteResult({
  "nInserted": 1
})
fabio-Inspiron-7520(mongod-3.0.7) be-mean-projeto> var user5 = {
... type: "user",
... dateRegister: Date.now(),
... username: "marcosenjoy",
... full_name: "Marcos Enjoy",
... email: "marcos.enjoy@gmail.com",
... password: 5,
... lastAcess: Date.now(),
... disable: 0
... }
fabio-Inspiron-7520(mongod-3.0.7) be-mean-projeto> db.users.save(user5)
Inserted 1 record(s) in 1ms
WriteResult({
  "nInserted": 1
})
fabio-Inspiron-7520(mongod-3.0.7) be-mean-projeto> var user6 = {
... type: "user",
... dateRegister: Date.now(),
... username: "bambirra",
... full_name: "Matheus Menezes",
... email: "menezesdebambirra@gmail.com",
... password: 6,
... lastAcess: Date.now(),
... disable: 0
... }
fabio-Inspiron-7520(mongod-3.0.7) be-mean-projeto> db.users.save(user6)
Inserted 1 record(s) in 1ms
WriteResult({
  "nInserted": 1
})
fabio-Inspiron-7520(mongod-3.0.7) be-mean-projeto> var user7 = {
... type: "user",
... dateRegister: Date.now(),
... username: "luizfelipe",
... full_name: "Luiz Felipe",
... email: "luiz.felipe@gmail.com",
... password: 7,
... lastAcess: Date.now(),
... disable: 0
... }
fabio-Inspiron-7520(mongod-3.0.7) be-mean-projeto> db.users.save(user7)
Inserted 1 record(s) in 4ms
WriteResult({
  "nInserted": 1
})
fabio-Inspiron-7520(mongod-3.0.7) be-mean-projeto> var user8 = {
... type: "user",
... dateRegister: Date.now(),
... username: "lele84",
... full_name: "Alessandra Negrine",
... email: "lele.negrine@gmail.com",
... password: 8,
... lastAcess: Date.now(),
... disable: 0
... }
fabio-Inspiron-7520(mongod-3.0.7) be-mean-projeto> db.users.save(user8)
Inserted 1 record(s) in 2ms
WriteResult({
  "nInserted": 1
})
fabio-Inspiron-7520(mongod-3.0.7) be-mean-projeto> var user9 = {
... type: "user",
... dateRegister: Date.now(),
... username: "matheusaraujo",
... full_name: "Mateus Vireira",
... email: "ocarala@gmail.com",
... password: 9,
... lastAcess: Date.now(),
... disable: 0
... }
fabio-Inspiron-7520(mongod-3.0.7) be-mean-projeto> db.users.save(user9)
Inserted 1 record(s) in 2ms
WriteResult({
  "nInserted": 1
})
fabio-Inspiron-7520(mongod-3.0.7) be-mean-projeto> var user10 = {
... type: "user",
... dateRegister: Date.now(),
... username: "usermagic",
... full_name: "Usário Mágico",
... email: "magic@gmail.com",
... password: 10,
... lastAcess: Date.now(),
... disable: 0
... }
fabio-Inspiron-7520(mongod-3.0.7) be-mean-projeto> db.users.save(user10)
Inserted 1 record(s) in 2ms
WriteResult({
  "nInserted": 1
})


```

### 2 - Cadastre 5 projetos diferentes.

```

var project1 = {
  
  name: "Projeto 1",
  description: "Descrição do projeto 1",
  created_time : Date.now(),
  visible: 1,

  members : [
    {
      user_id: ObjectId("5678546da5959e5d4718e58b")
    },
    {
      user_id: ObjectId("56785475a5959e5d4718e58c")
    },
    {
      user_id: ObjectId("5678547da5959e5d4718e58d")
    },
    {
      user_id: ObjectId("56785484a5959e5d4718e58e")
    },
    {
      user_id: ObjectId("5678548ba5959e5d4718e58f")
    }
  ],

  tags: ["futebol", "atletico", "galo", "brasil"],

  goals : [{
    name : "goal inicial",
    description: "goal do primeiro projeto",
    created_time : Date.now(),

    tags: ["brasil", "america do sul", "galodoido", "vaipracimadelesgalo"],

    activities: [
      {
        "activities_id" : ObjectId("5678749d3c5410e0b73e2825")
      },
      {
        "activities_id" : ObjectId("567874fd3c5410e0b73e2826")
      }
    ],
    historics : []
  }]
}


var project2 = {
  
  name: "Projeto 2",
  description: "Descrição do projeto 2",
  created_time : Date.now(),
  visible: 1,

  members : [
    {
      user_id: ObjectId("567854b0a5959e5d4718e594")
    },
    {
      user_id: ObjectId("567854a9a5959e5d4718e593")
    },
    {
      user_id: ObjectId("567854a2a5959e5d4718e592")
    },
    {
      user_id: ObjectId("5678549aa5959e5d4718e591")
    },
    {
      user_id: ObjectId("56785493a5959e5d4718e590")
    }
  ],

  tags: ["food", "drink", "truck", "brasil"],

  goals : [{
    name : "goal food track",
    description: "goal do segundo projeto",
    created_time : Date.now(),

    tags: ["goal1", "goal2", "goal3"],

    activities: [
      {
        "activities_id" : ObjectId("567875803c5410e0b73e2829")
      },
      {
        "activities_id" : ObjectId("5678749d3c5410e0b73e2825")
      }
    ],
    historics : []
  }]
}


var project3 = {
  
  name: "Projeto 3",
  description: "Descrição do projeto 3",
  created_time : Date.now(),
  visible: 1,

  members : [
    {
      user_id: ObjectId("56785475a5959e5d4718e58c")
    },
    {
      user_id: ObjectId("56785484a5959e5d4718e58e")
    },
    {
      user_id: ObjectId("56785493a5959e5d4718e590")
    },
    {
      user_id: ObjectId("567854a2a5959e5d4718e592")
    },
    {
      user_id: ObjectId("567854b0a5959e5d4718e594")
    }
  ],

  tags: ["nodejs", "angular", "express", "workshop"],

  goals : [{
    name : "Be mean",
    description: "goal do segundo projeto",
    created_time : Date.now(),

    tags: ["foda", "mongo", "noSql"],

    activities: [
      {
        "activities_id" : ObjectId("567875393c5410e0b73e2827")
      },
      {
        "activities_id" : ObjectId("5678755e3c5410e0b73e2828")
      }
    ],
    historics : []
  }]
}


var project4 = {
  
  name: "Projeto 4",
  description: "Descrição do projeto 4",
  created_time : Date.now(),
  visible: 1,

  members : [
    {
      user_id: ObjectId("567854a2a5959e5d4718e592")
    },
    {
      user_id: ObjectId("56785493a5959e5d4718e590")
    },
    {
      user_id: ObjectId("56785484a5959e5d4718e58e")
    },
    {
      user_id: ObjectId("5678546da5959e5d4718e58b")
    },
    {
      user_id: ObjectId("56785475a5959e5d4718e58c")
    }
  ],

  tags: ["terra", "fogo", "agua", "workshop"],

  goals : [{
    name : "Pokemons",
    description: "Pokemon do capeta",
    created_time : Date.now(),

    tags: ["golpe1", "golpe2", "golpe3"],

    activities: [
      {
        "activities_id" : ObjectId("567874fd3c5410e0b73e2826")
      },
      {
        "activities_id" : ObjectId("5678755e3c5410e0b73e2828")
      }
    ],
    historics : []
  }]
}


var project5 = {
  
  name: "Projeto 5",
  description: "Descrição do projeto 5",
  created_time : Date.now(),
  visible: 1,

  members : [
    {
      user_id: ObjectId("56785475a5959e5d4718e58c")
    },
    {
      user_id: ObjectId("5678547da5959e5d4718e58d")
    },
    {
      user_id: ObjectId("5678548ba5959e5d4718e58f")
    },
    {
      user_id: ObjectId("5678549aa5959e5d4718e591")
    },
    {
      user_id: ObjectId("567854a2a5959e5d4718e592")
    }
  ],

  tags: ["chuva", "sol", "vento", "workshop"],

  goals : [{
    name : "Goal do tempo",
    description: "Atmosfera Urbana",
    created_time : Date.now(),

    tags: ["BH", "RJ", "SP"],

    historics : []
  }]
}


db.projects.insert(project1)
db.projects.insert(project2)
db.projects.insert(project3)
db.projects.insert(project4)
db.projects.insert(project5)


```


## Retrieve - busca

```

### 1 - Liste as informações dos membros de 1 projeto específico que deve ser buscado pelo seu nome de forma a não ligar para maiúsculas e minúsculas.

```

var project = {name: /projeto 1/i}
var membros = [];
var getUser = function (usuario) {
  membros.push(db.users.findOne({ _id: usuario.user_id }))
};
var invt = db.projects.findOne();
invt.members.forEach(getUser);
membros
[
  {
    "_id": ObjectId("5678546da5959e5d4718e58b"),
    "type": "user",
    "dateRegister": 1450726508098,
    "username": "fabiocalheiros",
    "full_name": "Fábio Calheiros",
    "email": "fabiocalheiros@gmail.com",
    "bio": "Breve descrição",
    "avatarPath": "https://scontent-mia1-1.xx.fbcdn.net/hphotos-xpa1/v/t1.0-9/1381642_608172905891134_1368571562_n.jpg?oh=72d97e473608fa7907bf4bb4befa9b79&oe=56E46FB5",
    "password": 1,
    "lastAcess": 1450726508098,
    "disable": 0
  },
  {
    "_id": ObjectId("56785475a5959e5d4718e58c"),
    "type": "user",
    "dateRegister": 1450726516928,
    "username": "chicao",
    "full_name": "Chico Cézar",
    "email": "todechico@gmail.com",
    "password": 2,
    "lastAcess": 1450726516928,
    "disable": 0
  },
  {
    "_id": ObjectId("5678547da5959e5d4718e58d"),
    "type": "user",
    "dateRegister": 1450726524786,
    "username": "simsim",
    "full_name": "Simone Costa",
    "email": "simsim@ig.com.br",
    "password": 3,
    "lastAcess": 1450726524786,
    "disable": 0
  },
  {
    "_id": ObjectId("56785484a5959e5d4718e58e"),
    "type": "user",
    "dateRegister": 1450726531801,
    "username": "skyzito",
    "full_name": "Diego Bolina",
    "email": "diego.bolina@gmail.com",
    "password": 4,
    "lastAcess": 1450726531801,
    "disable": 0
  },
  {
    "_id": ObjectId("5678548ba5959e5d4718e58f"),
    "type": "user",
    "dateRegister": 1450726538923,
    "username": "marcosenjoy",
    "full_name": "Marcos Enjoy",
    "email": "marcos.enjoy@gmail.com",
    "password": 5,
    "lastAcess": 1450726538923,
    "disable": 0
  }
]



```

### 2 - Liste todos os projetos com a tag que você escolheu para os 3 projetos em comum.

```

var query = {tags: {$in: [/workshop/i]}}
var fields = {name: 1, tags: 1}
db.projects.find(query, fields)
{
  "_id": ObjectId("56787c433c5410e0b73e282c"),
  "name": "Projeto 3",
  "tags": [
    "nodejs",
    "angular",
    "express",
    "workshop"
  ]
}
{
  "_id": ObjectId("56787c453c5410e0b73e282d"),
  "name": "Projeto 4",
  "tags": [
    "terra",
    "fogo",
    "agua",
    "workshop"
  ]
}
{
  "_id": ObjectId("56787c473c5410e0b73e282e"),
  "name": "Projeto 5",
  "tags": [
    "chuva",
    "sol",
    "vento",
    "workshop"
  ]
}
Fetched 3 record(s) in 2ms

```

### 3 - Liste apenas os nomes de todas as atividades para todos os projetos.

```

var arrayAtividades = [];

var qtd = db.projects.find().length();

var getAtividades = function (atividade) {
    var invt = db.activities.findOne({ _id: atividade.activities_id });
    arrayAtividades.push(invt.name);
};

for (var i = 0; i < qtd; i++) {
    var project = db.projects.find().skip(i).limit(1).toArray();
    if (Array.isArray(project[0].goals[0].activities)) {
        project[0].goals[0].activities.forEach(getAtividades);
    }
}


fabio-Inspiron-7520(mongod-3.0.7) be-mean-projeto> arrayAtividades
[
  "Ativiade 1",
  "Ativiade 2",
  "Ativiade 5",
  "Ativiade 1",
  "Ativiade 3",
  "Ativiade 4",
  "Ativiade 2",
  "Ativiade 4"
]


```

### 4 - Liste todos os projetos que não possuam uma tag.

```

var query = {tags: {$nin: ['galo']}}
var fields = {tags: 1, name: 1}
db.projects.find(query, fields)
{
  "_id": ObjectId("56787c403c5410e0b73e282b"),
  "name": "Projeto 2",
  "tags": [
    "food",
    "drink",
    "truck",
    "brasil"
  ]
}
{
  "_id": ObjectId("56787c433c5410e0b73e282c"),
  "name": "Projeto 3",
  "tags": [
    "nodejs",
    "angular",
    "express",
    "workshop"
  ]
}
{
  "_id": ObjectId("56787c453c5410e0b73e282d"),
  "name": "Projeto 4",
  "tags": [
    "terra",
    "fogo",
    "agua",
    "workshop"
  ]
}
{
  "_id": ObjectId("56787c473c5410e0b73e282e"),
  "name": "Projeto 5",
  "tags": [
    "chuva",
    "sol",
    "vento",
    "workshop"
  ]
}
Fetched 4 record(s) in 2ms


```

### 5 - Liste todos os usuários que não fazem parte do primeiro projeto cadastrado.

```

var primeiro = db.projects.findOne();
var membros = [];

var getMembro = function (member) {
    var invt = db.users.findOne({ _id: member.user_id });
    membros.push(invt._id);
};

primeiro.members.forEach(getMembro);

db.users.find({_id: { $not: { $in: membros }}})

{
  "_id": ObjectId("56785493a5959e5d4718e590"),
  "type": "user",
  "dateRegister": 1450726547025,
  "username": "bambirra",
  "full_name": "Matheus Menezes",
  "email": "menezesdebambirra@gmail.com",
  "password": 6,
  "lastAcess": 1450726547025,
  "disable": 0
}
{
  "_id": ObjectId("5678549aa5959e5d4718e591"),
  "type": "user",
  "dateRegister": 1450726553707,
  "username": "luizfelipe",
  "full_name": "Luiz Felipe",
  "email": "luiz.felipe@gmail.com",
  "password": 7,
  "lastAcess": 1450726553707,
  "disable": 0
}
{
  "_id": ObjectId("567854a2a5959e5d4718e592"),
  "type": "user",
  "dateRegister": 1450726561360,
  "username": "lele84",
  "full_name": "Alessandra Negrine",
  "email": "lele.negrine@gmail.com",
  "password": 8,
  "lastAcess": 1450726561360,
  "disable": 0
}
{
  "_id": ObjectId("567854a9a5959e5d4718e593"),
  "type": "user",
  "dateRegister": 1450726568154,
  "username": "matheusaraujo",
  "full_name": "Mateus Vireira",
  "email": "ocarala@gmail.com",
  "password": 9,
  "lastAcess": 1450726568154,
  "disable": 0
}
{
  "_id": ObjectId("567854b0a5959e5d4718e594"),
  "type": "user",
  "dateRegister": 1450726575632,
  "username": "usermagic",
  "full_name": "Usário Mágico",
  "email": "magic@gmail.com",
  "password": 10,
  "lastAcess": 1450726575632,
  "disable": 0
}
Fetched 5 record(s) in 2ms


```

## Update - alteração

### 1 - Adicione para todos os projetos o campo views: 0.

```

var query = {}
var mod = {$set: {views: 0}}
var options = {multi: true}
db.projects.update(query, mod, options)


```

### 2 - Adicione 1 tag diferente para cada projeto.

```

for (var i = 0; i < 5; i++) {
    var project = db.projects.find().skip(i).limit(1).toArray();
    var newTag = "Tag "+ i;
    db.projects.update({_id: project[0]._id}, {$push: { tags: newTag } })
}

 db.projects.find({}, {name:1, tags:1})
{
  "_id": ObjectId("56787c3d3c5410e0b73e282a"),
  "name": "Projeto 1",
  "tags": [
    "futebol",
    "atletico",
    "galo",
    "brasil",
    "Tag 0"
  ]
}
{
  "_id": ObjectId("56787c403c5410e0b73e282b"),
  "name": "Projeto 2",
  "tags": [
    "food",
    "drink",
    "truck",
    "brasil",
    "Tag 1"
  ]
}
{
  "_id": ObjectId("56787c433c5410e0b73e282c"),
  "name": "Projeto 3",
  "tags": [
    "nodejs",
    "angular",
    "express",
    "workshop",
    "Tag 2"
  ]
}
{
  "_id": ObjectId("56787c453c5410e0b73e282d"),
  "name": "Projeto 4",
  "tags": [
    "terra",
    "fogo",
    "agua",
    "workshop",
    "Tag 3"
  ]
}
{
  "_id": ObjectId("56787c473c5410e0b73e282e"),
  "name": "Projeto 5",
  "tags": [
    "chuva",
    "sol",
    "vento",
    "workshop",
    "Tag 4"
  ]
}
Fetched 5 record(s) in 2ms


```
### 3 - Adicione 2 membros diferentes para cada projeto.

```

var query = {_id: ObjectId("56787c3d3c5410e0b73e282a")}
var usuarios = [{"user_id": ObjectId("56789d723c5410e0b73e282f")}, {"user_id": ObjectId("56789d833c5410e0b73e2830")}]
var mod = {$pushAll: {members: usuarios}}
db.projects.update(query, mod)


var query = {_id: ObjectId("56787c403c5410e0b73e282b")}
var usuarios = [{"user_id": ObjectId("56789d913c5410e0b73e2831")}, {"user_id": ObjectId("56789d9d3c5410e0b73e2832")}]
var mod = {$pushAll: {members: usuarios}}
db.projects.update(query, mod)


var query = {_id: ObjectId("56787c433c5410e0b73e282c")}
var usuarios = [{"user_id": ObjectId("56789da93c5410e0b73e2833")}, {"user_id": ObjectId("56789db73c5410e0b73e2834")}]
var mod = {$pushAll: {members: usuarios}}
db.projects.update(query, mod)


var query = {_id: ObjectId("56787c453c5410e0b73e282d")}
var usuarios = [{"user_id": ObjectId("56789dc53c5410e0b73e2835")}, {"user_id": ObjectId("56789dd23c5410e0b73e2836")}]
var mod = {$pushAll: {members: usuarios}}
db.projects.update(query, mod)


var query = {_id: ObjectId("56787c473c5410e0b73e282e")}
var usuarios = [{"user_id": ObjectId("56789de13c5410e0b73e2837")}, {"user_id": ObjectId("56789df03c5410e0b73e2838")}]
var mod = {$pushAll: {members: usuarios}}
db.projects.update(query, mod)


```
### 4 - Adicione 1 comentário em cada atividade, deixe apenas 1 projeto sem.

```

var users = db.users.find().toArray()
var atividades = db.activities.find().toArray()
var qtdAtividades = db.activities.find().length()
var quantidadeTotal = qtdAtividades - 1;
var comentario = []

function insertComment(index){

  comentario.push({
      "text": "Comentário inserido depois " + index,
      "date_coment": Date.now(),

      "members": [
        {
          "user_id": users[index]._id
        }
      ],
      "files": [ ]
  })
    
  db.activities.update({ _id: atividades[index]._id }, { $push: { comments: comentario } });
}

for (var i=0; i<quantidadeTotal; i++){
  insertComment(i)
}

Updated 1 existing record(s) in 2ms
Updated 1 existing record(s) in 2ms
Updated 1 existing record(s) in 1ms
Updated 1 existing record(s) in 1ms


```
### 5 - Adicione 1 projeto inteiro com UPSERT.

```

var query = {name: /Projeto UPSERT/i}
var mod = {
  $set: {visible: 1},
    $setOnInsert: {
      "name": "Projeto UPSERT",
      "description": "Nova inserção usando UPSERT",
      "created_time": Date.now(),
      "members" :
      [
        {
          user_id: ObjectId("567854b0a5959e5d4718e594")
        },
        {
          user_id: ObjectId("567854a9a5959e5d4718e593")
        },
        {
          user_id: ObjectId("567854a2a5959e5d4718e592")
        },
        {
          user_id: ObjectId("5678549aa5959e5d4718e591")
        },
        {
          user_id: ObjectId("56785493a5959e5d4718e590")
        }
      ],
      "tags": [],
      "goals": [
          {
            "activities":
            [
              {
                "activities_id": ObjectId("5678a90d3c5410e0b73e2839")
              }
            ]
          }
        ],
      "views": 0
    }
}

var options = {upsert: true}
db.projects.update(query, mod, options)

Updated 1 new record(s) in 2ms
WriteResult({
  "nMatched": 0,
  "nUpserted": 1,
  "nModified": 0,
  "_id": ObjectId("5678a6623e07b0ff3a5ad172")
})


```

## Delete - remoção

```

### 1 - Apague todos os projetos que não possuam tags.

var query = {'tags.0': {$exists: false}}
db.projects.remove(query)

Removed 1 record(s) in 1ms
WriteResult({
  "nRemoved": 1
})


```


### 2 - Apague todos os projetos que não possuam comentários nas atividades.

```

function item(recebeId){
  db.projects.remove({"goals.activities.activities_id":  recebeId._id});
}

db.activities.find({'comments.0': {$exists: false}}).forEach(item)


### 3 - Apague todos os projetos que não possuam atividades.

```

var query = {'goals.activities': {$exists: false}}
db.projects.remove(query)

Removed 1 record(s) in 34ms
WriteResult({
  "nRemoved": 1
})


```

### 4 - Escolha 2 usuários e apague todos os projetos em que os 2 fazem parte.

```

var usuarios = [
  {
    "user_id": ObjectId("567854b0a5959e5d4718e594")
  },
  {
    "user_id": ObjectId("5678549aa5959e5d4718e591")
  }
]

var query = {members: {$all: usuarios}}
db.projects.remove(query)

Removed 1 record(s) in 2ms
WriteResult({
  "nRemoved": 1
})


```

### 5 - Apague todos os projetos que possuam uma determinada tag em goal.

```

var query = {"goals.tags": {$in: ["golpe1"]}}
db.projects.remove(query)

Removed 1 record(s) in 2ms
WriteResult({
  "nRemoved": 1
})


```

## Gerenciamento de usuários

### 1 - Crie um usuário com permissões APENAS de Leitura.

```

fabio-Inspiron-7520(mongod-3.0.7) be-mean-projeto> db.createUser(
...   {
...     user: "projectuser",
...     pwd: "user123",
...     roles: [ { role: "read", db: "admin"} ]
...   }
... )
Successfully added user: {
  "user": "projectuser",
  "roles": [
    {
      "role": "read",
      "db": "admin"
    }
  ]
}


```

### 2 - Crie um usuário com permissões de Escrita e Leitura.

```
db.createUser(
  {
  user: "projectuser2",
    pwd: "user123",
    roles: [ { role: "readWrite", db: "admin"} ]
  }
)
Successfully added user: {
  "user": "projectuser2",
  "roles": [
    {
      "role": "readWrite",
      "db": "admin"
    }
  ]
}


```

### 3 - Adicionar o papel grantRolesToUser e revokeRole para o usuário com Escrita e Leitura.

```

db.runCommand( { grantRolesToUser: "projectuser2",
  roles: [
    { role: "userAdmin", db: "be-mean-projeto"}
  ],
  writeConcern: {}
  })
{
  "ok": 1
}

```

### 4 - Remover o papel grantRolesToUser para o usuário com Escrita e Leitura.

```
db.runCommand(
  {
    revokeRolesFromUser: "projectuser2",
    roles: [
      { role: "userAdmin", db: "be-mean-projeto"}
    ],
    writeConcern: {}
  }
)
{
  "ok": 1
}


```

### 5 - Listar todos os usuários com seus papéis e ações.

```

db.runCommand( { usersInfo: [ { user: "projectuser", db: "be-mean-projeto" }, { user: "projectuser2", db: "be-mean-projeto" } ],
showCredentials: true,
showPrivileges: true
} )

{
  "users": [
    {
      "_id": "be-mean-projeto.projectuser",
      "user": "projectuser",
      "db": "be-mean-projeto",
      "credentials": {
        "SCRAM-SHA-1": {
          "iterationCount": 10000,
          "salt": "WyEpAfzGTaxNzF5+xc6HBQ==",
          "storedKey": "NDynVGBKrsFIVYn/a6uKNDnqwNE=",
          "serverKey": "FeEkJlr2vg/nRDvsFVmuGNI2gSc="
        }
      },
      "roles": [
        {
          "role": "read",
          "db": "admin"
        }
      ],
      "inheritedRoles": [
        {
          "role": "read",
          "db": "admin"
        }
      ],
      "inheritedPrivileges": [
        {
          "resource": {
            "db": "admin",
            "collection": ""
          },
          "actions": [
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
          "resource": {
            "anyResource": true
          },
          "actions": [
            "listCollections"
          ]
        },
        {
          "resource": {
            "db": "admin",
            "collection": "system.indexes"
          },
          "actions": [
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
          "resource": {
            "db": "admin",
            "collection": "system.js"
          },
          "actions": [
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
          "resource": {
            "db": "admin",
            "collection": "system.namespaces"
          },
          "actions": [
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
    },
    {
      "_id": "be-mean-projeto.projectuser2",
      "user": "projectuser2",
      "db": "be-mean-projeto",
      "credentials": {
        "SCRAM-SHA-1": {
          "iterationCount": 10000,
          "salt": "VID1770oR4XyziYfDyySvA==",
          "storedKey": "x113H5/tk2mRCKF63wN/nCPcaGI=",
          "serverKey": "I0YaP9A/PVLQc4tsbYCgeXSm6Yc="
        }
      },
      "roles": [
        {
          "role": "readWrite",
          "db": "admin"
        }
      ],
      "inheritedRoles": [
        {
          "role": "readWrite",
          "db": "admin"
        }
      ],
      "inheritedPrivileges": [
        {
          "resource": {
            "db": "admin",
            "collection": ""
          },
          "actions": [
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
          "resource": {
            "anyResource": true
          },
          "actions": [
            "listCollections"
          ]
        },
        {
          "resource": {
            "db": "admin",
            "collection": "system.indexes"
          },
          "actions": [
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
          "resource": {
            "db": "admin",
            "collection": "system.js"
          },
          "actions": [
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
          "resource": {
            "db": "admin",
            "collection": "system.namespaces"
          },
          "actions": [
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
  "ok": 1
}


```


## Cluster
Depois de criada toda sua base você deverá criar um cluster utilizando:

### (1) - 1 Router

### (2) - 1 Config Server

### (3) - 3 Shardings

### (4) - 3 Replicas

```

// Cria a pasta de configuração
mkdir configdb

// Inicia o confiServer
mongod --configsvr --port 27010

// faz o router em cima do server criado
mongos --configdb localhost:27010 --port 27011

// cria os diretorios de shards
mkdir /data/shard1 && mkdir /data/shard2 && mkdir /data/shard3

// inicia as guardas cada qual em sua porrta
mongod --port 27012 --dbpath /data/shard1
mongod --port 27013 --dbpath /data/shard2
mongod --port 27014 --dbpath /data/shard3

// conectamos ao router e adicionamos as shards
mongo --port 27011 --host localhost

// adiciona shard1
sh.addShard("localhost:27012")
{
  "shardAdded": "shard0000",
  "ok": 1
}

// adiciona shard2
sh.addShard("localhost:27013")
{
  "shardAdded": "shard0001",
  "ok": 1
}

// adiciona shard3
sh.addShard("localhost:27014")
{
  "shardAdded": "shard0002",
  "ok": 1
}

// Habilita Shardind em um determinado banco de dados (db)
sh.enableSharding("be-mean-projeto")
{
  "ok": 1
}

// aponta qual a base será shardeada
sh.shardCollection("be-mean-projeto.projects", {"_id": 1})
{
  "collectionsharded": "be-mean-projeto.projects",
  "ok": 1
}

```

## Replica

```

// Cria os diretorios para as réplicas
mkdir /data/rs1
mkdir /data/rs2
mkdir /data/rs3

// cria as replicas apontando para a porta e a pasta
mongod --replSet replica_set --port 27117 --dbpath /data/rs1
mongod --replSet replica_set --port 27118 --dbpath /data/rs2
mongod --replSet replica_set --port 27119 --dbpath /data/rs3

// conectamos com o mongo na replica primaria
mongo --port 27117

// adiciona as configurações da replicaset
rsconf = {
  _id: "replica_set",
      members: [
      {
        _id: 0,
        host: "127.0.0.1:27117"
      }
    ]
}
{
  "_id": "replica_set",
  "members": [
    {
      "_id": 0,
      "host": "127.0.0.1:27117"
    }
  ]
}

// inicia as configurações da replica set
rs.initiate(rsconf)
{
  "ok": 1
}

//adiciona a replica 2 (secundária)
rs.add("127.0.0.1:27118")
{
  "ok": 1
}

//adiciona a replica 3 (secundária)
rs.add("127.0.0.1:27119")
{
  "ok": 1
}

//Exibe status da replica
rs.status()
{
  "set": "replica_set",
  "date": ISODate("2015-12-14T23:34:57.196Z"),
  "myState": 1,
  "members": [
    {
      "_id": 0,
      "name": "127.0.0.1:27117",
      "health": 1,
      "state": 1,
      "stateStr": "PRIMARY",
      "uptime": 613,
      "optime": Timestamp(1450135833, 1),
      "optimeDate": ISODate("2015-12-14T23:30:33Z"),
      "electionTime": Timestamp(1450135576, 2),
      "electionDate": ISODate("2015-12-14T23:26:16Z"),
      "configVersion": 3,
      "self": true
    },
    {
      "_id": 1,
      "name": "127.0.0.1:27118",
      "health": 1,
      "state": 2,
      "stateStr": "SECONDARY",
      "uptime": 280,
      "optime": Timestamp(1450135833, 1),
      "optimeDate": ISODate("2015-12-14T23:30:33Z"),
      "lastHeartbeat": ISODate("2015-12-14T23:34:57.099Z"),
      "lastHeartbeatRecv": ISODate("2015-12-14T23:34:56.421Z"),
      "pingMs": 0,
      "syncingTo": "127.0.0.1:27117",
      "configVersion": 3
    },
    {
      "_id": 2,
      "name": "127.0.0.1:27119",
      "health": 1,
      "state": 2,
      "stateStr": "SECONDARY",
      "uptime": 264,
      "optime": Timestamp(1450135833, 1),
      "optimeDate": ISODate("2015-12-14T23:30:33Z"),
      "lastHeartbeat": ISODate("2015-12-14T23:34:57.099Z"),
      "lastHeartbeatRecv": ISODate("2015-12-14T23:34:57.102Z"),
      "pingMs": 0,
      "lastHeartbeatMessage": "could not find member to sync from",
      "configVersion": 3
    }
  ],
  "ok": 1
}

```