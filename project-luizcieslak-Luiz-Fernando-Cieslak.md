# MongoDb - Projeto Final
**Autor:** Luiz Fernando Cieslak
**Data** 1476209351602

## Para qual sistema você usaria o MongoDB (diferente desse)?

Eu usaria o MongoDB para prototipagem de sistemas ou sistemas que precisam de estar prontos em pouco tempo, no qual a aplicacão é pensada antes da estrutura de Banco de Dados. O fato de ele ser schemaless e fornecer uma rápida resposta a queries permite-o ser utilizado para tais funcões.

## Qual a modelagem da sua coleção de `users`?

```
users: {
  name: "",
  bio: "",
  auth: [
    ...
  ]
}
```

OBS: o MongoDB já guarda um timestamp quando um documento é criado:
```
db.users.findOne()._id.getTimestamp()
ISODate("2016-09-30T02:09:33Z")
```
Não sendo necessário um campo para date-register.

## Qual a modelagem da sua coleção de `projects`?

```
projects: {
  name: "",
  description: "",

  members: [{
    type-member: "",
    user_id:

  }],

  tags: [],

  goals: [{
    name: "",
    description: "",

    historic: "",

    tags: [],

    activities: [{
      name: "",
      description: "",
      historic: "",

      comments: [{
          text: "",
          date_comment:,
          members: [],
          files: [],
      }]
    }]
  }]
}
```

## Qual a modelagem da sua coleção retirada de `projects`?

Não foi retirada nenhuma parte de projects, mantendo os goals, activities e comments dentro de project. Como a maioria das relacões do projeto são 1-para-(0 ou n), escolhi utilizar os recursos de guardar toda a informacão relacionada dentro de somente um documento. Isso faz com que se tenha uma melhor performance para recuperar os dados, obtendo-os realizando somente um comando.
Como não sabemos se os projetos serão relacionados, não é útil reunir os goals ou activities numa collection separada porque não sabemos se terão goals e activities iguais em diferentes projetos.
Porém deve se tomar cuidado em relacão ao crescimento dos arrays dentro do documento para não ultrapassar o valor máximo do documento que é 16MB. Caso este modelo fosse usado em uma aplicacão real, deve-se estipular um número máximo de goals e activities para respeitar os 16MB.

## Create - cadastro

1. Cadastre 10 usuarios diferentes:

```
be-mean> db.users.find()
{
  "_id": ObjectId("57edc95da1b435c4ab643c93"),
  "name": "Luiz",
  "auth": [ ]
}
{
  "_id": ObjectId("57edc9e8a1b435c4ab643c94"),
  "name": "Ronald",
  "auth": [ ]
}
{
  "_id": ObjectId("57edc9eba1b435c4ab643c95"),
  "name": "Ronaldo",
  "auth": [ ]
}
{
  "_id": ObjectId("57edc9f8a1b435c4ab643c96"),
  "name": "Ronny",
  "auth": [ ]
}
{
  "_id": ObjectId("57edca02a1b435c4ab643c97"),
  "name": "Ronnaldo",
  "auth": [ ]
}
{
  "_id": ObjectId("57edca74a1b435c4ab643c98"),
  "name": "Fernando",
  "auth": [ ]
}
{
  "_id": ObjectId("57edca78a1b435c4ab643c99"),
  "name": "Cieslak",
  "auth": [ ]
}
{
  "_id": ObjectId("57edca8ba1b435c4ab643c9a"),
  "name": "Louis",
  "auth": [ ]
}
{
  "_id": ObjectId("57edca90a1b435c4ab643c9b"),
  "name": "Lui",
  "auth": [ ]
}
{
  "_id": ObjectId("57edca94a1b435c4ab643c9c"),
  "name": "Lu",
  "auth": [ ]
}

```
2. Cadastre 5 projetos diferentes.

Algoritmo para gerar 5 ID's diferentes para serem inseridas no projeto usando o [knuth-shuffle]( https://github.com/coolaj86/knuth-shuffle)

```
function shuffle(array) {
  var currentIndex = array.length, temporaryValue, randomIndex;

  // While there remain elements to shuffle...
  while (0 !== currentIndex) {

    // Pick a remaining element...
    randomIndex = Math.floor(Math.random() * currentIndex);
    currentIndex -= 1;

    // And swap it with the current element.
    temporaryValue = array[currentIndex];
    array[currentIndex] = array[randomIndex];
    array[randomIndex] = temporaryValue;
  }

  return array;
}
```

Escopo:
```
var ids = db.users.find({},{_id:1}).toArray()
shuffle(ids);
var projectIds = ids.slice(0,5)

db.projects.insert({
  name:"Cafe da Manha",
  description:"Realizar o cafe da manha",

  members: [
    {
      "type-member": "manager",
      "user_id": projectIds[0]._id
    },
    {
      "type-member": "employee",
      "user_id": projectIds[1]._id
    },
    {
      "type-member": "employee",
      "user_id": projectIds[2]._id
    },
    {
      "type-member": "intern",
      "user_id": projectIds[3]._id
    },
    {
      "type-member": "intern",
      "user_id": projectIds[4]._id
    }
  ],

  tags: ["comida", "desafio", "breakfast"],

  goals: [{
    name: "Comer a nao se atrasar",
    description: "Preparar tudo a tempo",

    tags: ["alimento", "energia", "colher"],

    activities: [
      {
        name: "Fazer o cafe",
        description: "Confeccionar aquele cafezao monstro pra comecar o dia bem"
      },
      {
        name: "Comer uma fruta",
        description: "Umas vitaminas sao sempre bem-vindas"
      }
    ]
  }]
})
```

Projetos inseridos:
```
be-mean> db.projects.find()
{
  "_id": ObjectId("57f1ad16dd64ab49547ac9bd"),
  "name": "Pokemons",
  "description": "Pegar todos os 610 pokemons da collection",
  "members": [
    {
      "type-member": "manager",
      "user_id": ObjectId("57edc95da1b435c4ab643c93")
    },
    {
      "type-member": "employee",
      "user_id": ObjectId("57edca94a1b435c4ab643c9c")
    },
    {
      "type-member": "employee",
      "user_id": ObjectId("57edca8ba1b435c4ab643c9a")
    },
    {
      "type-member": "intern",
      "user_id": ObjectId("57edca02a1b435c4ab643c97")
    },
    {
      "type-member": "intern",
      "user_id": ObjectId("57edca90a1b435c4ab643c9b")
    }
  ],
  "tags": [
    "pokemons",
    "aventura",
    "exploracao"
  ],
  "goals": [
    {
      "name": "Capturar todos pokemons",
      "description": "Capturar todos os pokemons listados na collection",
      "tags": [
        "pokebola",
        "captura",
        "ashketchum"
      ],
      "activities": [
        {
          "name": "Comprar pokebolas",
          "description": "sem pokebolas nao da pra capturar ne"
        },
        {
          "name": "Arremessar a pokebola",
          "description": "de preferencia um headshot"
        }
      ]
    }
  ]
}
{
  "_id": ObjectId("57f1af9e3acddacc36981625"),
  "name": "Restaurantes",
  "description": "visitar todos os 23k restaurantes na collection",
  "members": [
    {
      "type-member": "manager",
      "user_id": ObjectId("57edca74a1b435c4ab643c98")
    },
    {
      "type-member": "employee",
      "user_id": ObjectId("57edca02a1b435c4ab643c97")
    },
    {
      "type-member": "employee",
      "user_id": ObjectId("57edca78a1b435c4ab643c99")
    },
    {
      "type-member": "intern",
      "user_id": ObjectId("57edc95da1b435c4ab643c93")
    },
    {
      "type-member": "intern",
      "user_id": ObjectId("57edc9e8a1b435c4ab643c94")
    }
  ],
  "tags": [
    "comida",
    "aventura",
    "restaurante"
  ],
  "goals": [
    {
      "name": "Viajar para Nova Iorque",
      "description": "Pegar um aviao com destino a NY, tirar aquela famosa foto na asa do aviao e postar no instagram.",
      "tags": [
        "viagem",
        "aviao",
        "instagram"
      ],
      "activities": [
        {
          "name": "Tirar o visto",
          "description": "Preencher o formulario online e depois comparecer a entrevista em SP."
        },
        {
          "name": "Procurar por passagens baratas",
          "description": "Procurar por promocoes e passagens em conta em sites tipo Google Flights."
        }
      ]
    }
  ]
}
{
  "_id": ObjectId("57f1b1073acddacc36981627"),
  "name": "Be MEAN",
  "description": "Terminar todos os modulos do Be MEAN - Projeto da Webschool",
  "members": [
    {
      "type-member": "manager",
      "user_id": ObjectId("57edc9e8a1b435c4ab643c94")
    },
    {
      "type-member": "employee",
      "user_id": ObjectId("57edca02a1b435c4ab643c97")
    },
    {
      "type-member": "employee",
      "user_id": ObjectId("57edca74a1b435c4ab643c98")
    },
    {
      "type-member": "intern",
      "user_id": ObjectId("57edca78a1b435c4ab643c99")
    },
    {
      "type-member": "intern",
      "user_id": ObjectId("57edc95da1b435c4ab643c93")
    }
  ],
  "tags": [
    "javascript",
    "instagram",
    "fullstack",
    "desafio"
  ],
  "goals": [
    {
      "name": "Construir um app do tipo instagram do zero",
      "description": "Construir o app baseado no que foi ensinado nos modulos.",
      "tags": [
        "bemean",
        "webschool",
        "suissa"
      ],
      "activities": [ ]
    }
  ]
}
{
  "_id": ObjectId("57f1b3ec3acddacc36981628"),
  "name": "Universidade",
  "description": "Me formar na faculdade",
  "members": [
    {
      "type-member": "manager",
      "user_id": ObjectId("57edc9f8a1b435c4ab643c96")
    },
    {
      "type-member": "employee",
      "user_id": ObjectId("57edca90a1b435c4ab643c9b")
    },
    {
      "type-member": "employee",
      "user_id": ObjectId("57edc9e8a1b435c4ab643c94")
    },
    {
      "type-member": "intern",
      "user_id": ObjectId("57edca78a1b435c4ab643c99")
    },
    {
      "type-member": "intern",
      "user_id": ObjectId("57edc9eba1b435c4ab643c95")
    }
  ],
  "tags": [
    "unesp",
    "desafio",
    "fullstack"
  ],
  "goals": [
    {
      "name": "TCC",
      "description": "Pensar num TCC bom e realiza-lo.",
      "tags": [
        "tcc",
        "abnt",
        "projeto"
      ],
      "activities": [
        {
          "name": "Me inscrever na aula de TCC",
          "description": "atividade fundamental para o objetivo almejado"
        },
        {
          "name": "Escrever segundo as normas da ABNT",
          "description": "Um mal necessario."
        }
      ]
    }
  ]
}
{
  "_id": ObjectId("57f1b8b43acddacc3698162a"),
  "name": "Jantar",
  "description": "Realizar o Jantar",
  "members": [
    {
      "type-member": "manager",
      "user_id": ObjectId("57edca74a1b435c4ab643c98")
    },
    {
      "type-member": "employee",
      "user_id": ObjectId("57edca78a1b435c4ab643c99")
    },
    {
      "type-member": "employee",
      "user_id": ObjectId("57edca02a1b435c4ab643c97")
    },
    {
      "type-member": "intern",
      "user_id": ObjectId("57edca94a1b435c4ab643c9c")
    },
    {
      "type-member": "intern",
      "user_id": ObjectId("57edca90a1b435c4ab643c9b")
    }
  ],
  "tags": [
    "comida",
    "desafio",
    "jantar"
  ],
  "goals": [
    {
      "name": "Ficar bem alimentado",
      "description": "Comer nao da xp",
      "tags": [
        "alimento",
        "energia",
        "garfo"
      ],
      "activities": [
        {
          "name": "Cozinhar o frango",
          "description": "Pegue uma panela, ponha o frango e agua nela."
        },
        {
          "name": "Cozinhar a batata",
          "description": "Pegue uma panela, ponha o batata e agua nela."
        }
      ]
    }
  ]
}
Fetched 5 record(s) in 6ms
```

## Retrieve - Busca
1. Liste as informações dos membros de 1 projeto específico que deve ser buscado pelo seu nome de forma a não ligar para maiúsculas e minúsculas.
```
be-mean> db.projects.find({name:/jantar/i},{members:1})
{
  "_id": ObjectId("57f1b8b43acddacc3698162a"),
  "members": [
    {
      "type-member": "manager",
      "user_id": ObjectId("57edca74a1b435c4ab643c98")
    },
    {
      "type-member": "employee",
      "user_id": ObjectId("57edca78a1b435c4ab643c99")
    },
    {
      "type-member": "employee",
      "user_id": ObjectId("57edca02a1b435c4ab643c97")
    },
    {
      "type-member": "intern",
      "user_id": ObjectId("57edca94a1b435c4ab643c9c")
    },
    {
      "type-member": "intern",
      "user_id": ObjectId("57edca90a1b435c4ab643c9b")
    }
  ]
}
Fetched 1 record(s) in 2ms
```

2. Liste todos os projetos com a tag que você escolheu para os 3 projetos em comum.
```
be-mean> var query = {tags: {$in:[/desafio/i]}}
be-mean> db.projects.find(query,{tags:1})
{
  "_id": ObjectId("57f1b1073acddacc36981627"),
  "tags": [
    "javascript",
    "instagram",
    "fullstack",
    "desafio"
  ]
}
{
  "_id": ObjectId("57f1b3ec3acddacc36981628"),
  "tags": [
    "unesp",
    "desafio",
    "fullstack"
  ]
}
{
  "_id": ObjectId("57f1b8b43acddacc3698162a"),
  "tags": [
    "comida",
    "desafio",
    "jantar"
  ]
}
Fetched 3 record(s) in 1ms
```

3. Liste apenas os nomes de todas as atividades para todos os projetos.

```
be-mean> db.projects.find({},{"goals.activities.name":1})
{
  "_id": ObjectId("57f1ad16dd64ab49547ac9bd"),
  "goals": [
    {
      "activities": [
        {
          "name": "Comprar pokebolas"
        },
        {
          "name": "Arremessar a pokebola"
        }
      ]
    }
  ]
}
{
  "_id": ObjectId("57f1af9e3acddacc36981625"),
  "goals": [
    {
      "activities": [
        {
          "name": "Tirar o visto"
        },
        {
          "name": "Procurar por passagens baratas"
        }
      ]
    }
  ]
}
{
  "_id": ObjectId("57f1b1073acddacc36981627"),
  "goals": [
    {
      "activities": [ ]
    }
  ]
}
{
  "_id": ObjectId("57f1b3ec3acddacc36981628"),
  "goals": [
    {
      "activities": [
        {
          "name": "Me inscrever na aula de TCC"
        },
        {
          "name": "Escrever segundo as normas da ABNT"
        }
      ]
    }
  ]
}
{
  "_id": ObjectId("57f1b8b43acddacc3698162a"),
  "goals": [
    {
      "activities": [
        {
          "name": "Cozinhar o frango"
        },
        {
          "name": "Cozinhar a batata"
        }
      ]
    }
  ]
}
Fetched 5 record(s) in 3ms
```

4. Liste todos os projetos que não possuam uma tag.
```
be-mean> db.projects.find({tags: {$nin: [/desafio/i]}}, {name:1})
{
  "_id": ObjectId("57f1ad16dd64ab49547ac9bd"),
  "name": "Pokemons"
}
{
  "_id": ObjectId("57f1af9e3acddacc36981625"),
  "name": "Restaurantes"
}
Fetched 2 record(s) in 1ms
```

5. Liste todos os usuários que não fazem parte do primeiro projeto cadastrado.
```
var project = db.projects.findOne({},{members:1})
var members = []
project.members.forEach(function(element){
      members.push(element.user_id);
  })

be-mean> db.users.find({_id: { $nin: members}})
  {
    "_id": ObjectId("57edc9e8a1b435c4ab643c94"),
    "name": "Ronald",
    "auth": [ ]
  }
  {
    "_id": ObjectId("57edc9eba1b435c4ab643c95"),
    "name": "Ronaldo",
    "auth": [ ]
  }
  {
    "_id": ObjectId("57edc9f8a1b435c4ab643c96"),
    "name": "Ronny",
    "auth": [ ]
  }
  {
    "_id": ObjectId("57edca74a1b435c4ab643c98"),
    "name": "Fernando",
    "auth": [ ]
  }
  {
    "_id": ObjectId("57edca78a1b435c4ab643c99"),
    "name": "Cieslak",
    "auth": [ ]
  }
Fetched 5 record(s) in 2ms
```

## Update - alteração

1. Adicione para todos os projetos o campo views: 0.

```
be-mean> db.projects.update({},{$set: {views:0}},{upsert:false, multi:true})
Updated 5 existing record(s) in 1ms
WriteResult({
  "nMatched": 5,
  "nUpserted": 0,
  "nModified": 5
})
```

2. Adicione 1 tag diferente para cada projeto.
```
be-mean> db.projects.update({name:/be mean/i},{$push: {tags:'bemean'}})
be-mean> db.projects.update({name:/pokemons/i},{$push: {tags:'squirtle'}})
be-mean> db.projects.update({name:/restaurantes/i},{$push: {tags:'newyork'}})
be-mean> db.projects.update({name:/universidade/i},{$push: {tags:'bcc'}})
be-mean> db.projects.update({name:/jantar/i},{$push: {tags:'cozinha'}})
```

3. Adicione 2 membros diferentes para cada projeto.
```
be-mean> var array = db.users.find().toArray()
be-mean> var member = array[Math.floor(_rand()*db.users.count())]

be-mean> db.projects.update({name:/be mean/i},{$push: {members: {type-member: "client", user_id: id}}})
```
4. Adicione 1 comentário em cada atividade, deixe apenas 1 projeto sem.

Obs: temos um problema aqui. O MongoDB ainda não suporta o operador posicional para arrays duplamente aninhados ou mais.

Por exemplo, se eu quiser adicionar uma activity a um goal especifico:

```
db.projects.update({name:/jantar/i, "goals.name":"Ficar bem alimentado"},{$push: {"goals.$.activities": {
    name: "fazer um miojo",
    description: "3 minutos"
}}})
```
Seria realizado desta forma, utilizando o [operador posicional ($)](https://docs.mongodb.com/manual/reference/operator/update/positional/).
Mas como neste exercício é o array comments dentro do array activities dentro do array goals, o mongo não o [suporta ainda](https://jira.mongodb.org/browse/SERVER-831).



Solucão usando este [procedimento](http://stackoverflow.com/questions/18573117/updating-nested-arrays-in-mongodb-via-mongo-shell):
```
be-mean> db.projects.update({name:/jantar/i, "goals.activities.name":"Cozinhar o frango", "goals.activities.name": "Cozinhar o frango" },
  {$push: {"goals.0.activities.$.comments": {     
    "text": "estou com fome",     
    "members": [],     
    "files": []   
}}})
```
Sendo `0` a primeira posicao do array (que e a atividade que eu desejo comentar).

5. Adicione 1 projeto inteiro com UPSERT.
```
var ids = db.users.find({},{_id:1}).toArray()
shuffle(ids);
var projectIds = ids.slice(0,5)
db.projects.update({name:/inexistente/i},{$setOnInsert:{
    name:"Cafe da Manha",
    description:"Realizar o cafe da manha",

    members: [
      {
        "type-member": "manager",
        "user_id": projectIds[0]._id
      },
      {
        "type-member": "employee",
        "user_id": projectIds[1]._id
      },
      {
        "type-member": "employee",
        "user_id": projectIds[2]._id
      },
      {
        "type-member": "intern",
        "user_id": projectIds[3]._id
      },
      {
        "type-member": "intern",
        "user_id": projectIds[4]._id
      }
    ],

    tags: ["comida", "desafio", "breakfast"],

    goals: [{
      name: "Comer a nao se atrasar",
      description: "Preparar tudo a tempo",

      tags: ["alimento", "energia", "colher"],

      activities: [
        {
          name: "Fazer o cafe",
          description: "Confeccionar aquele cafezao monstro pra comecar o dia bem"
        },
        {
          name: "Comer uma fruta",
          description: "Umas vitaminas sao sempre bem-vindas"
        }
      ]
    }]
  }
},{ upsert: true})
```
## Delete - remoção

1. Apague todos os projetos que não possuam tags.
```
db.projects.remove({tags: {$exists: true, $size: 0}})
```
2. Apague todos os projetos que não possuam comentários nas atividades.
```
db.projects.remove({"goals.activities.comments": {$not: {$gt: []}}})
//Remove inclusive os que possuem o array comments vazio
```
3. Apague todos os projetos que não possuam atividades.
```
db.projects.remove({"goals.activities": {$exists: true, $size: 0}})
```

4. Escolha 2 usuário e apague todos os projetos em que os 2 fazem parte.
```
//Se um OU o outro fazem parte
db.projects.remove({"members.user_id": {$in: [ObjectId("57edca94a1b435c4ab643c9c"),ObjectId("57edca90a1b435c4ab643c9b")]}})
//se um E o outro fazem parte
db.projects.remove({"members.user_id": {$all: [ObjectId("57edca94a1b435c4ab643c9c"),ObjectId("57edca90a1b435c4ab643c9b")]}})
```

5. Apague todos os projetos que possuam uma determinada tag em goal.
```
db.projects.remove({"goals.tags": {$in: [/alimento/i]}})
```

## Sharding
```
//Criando os servicos
mongod --configsvr --port 27010
mongos --configdb localhost:27010 --port 27011
mongod --port 27012 --dbpath /data/shard1 --replSet shard1
mongod --port 27013 --dbpath /data/shard2 --replSet shard2
mongod --port 27014 --dbpath /data/shard3 --replSet shard2
```
```
mongo --port 27011 --host localhost
avell:27011(mongos-3.2.10)[mongos] test> sh.addShard("shard1/localhost:27012")
{
  "shardAdded": "shard1",
  "ok": 1
}
avell:27011(mongos-3.2.10)[mongos] test> sh.addShard("shard2/localhost:27013")
{
  "shardAdded": "shard2",
  "ok": 1
}
avell:27011(mongos-3.2.10)[mongos] test> sh.addShard("shard3/localhost:27014")
{
  "shardAdded": "shard3",
  "ok": 1
}
avell:27011(mongos-3.2.10)[mongos] test> sh.enableSharding("be-mean")
{
  "ok": 1
}
avell:27011(mongos-3.2.10)[mongos] test> sh.shardCollection("be-mean.projects",{"_id":1})
{
  "collectionsharded": "be-mean.projects",
  "ok": 1
}
```

## Replica
```
//Criando replicas
mongod --replSet shard1 --port 27017 --dbpath /data/rsShard1
mongod --replSet shard2 --port 27018 --dbpath /data/rsShard2
mongod --replSet shard3 --port 27019 --dbpath /data/rsShard3
```
Shard 1
```
mongo --port 27017

rsconf = {
   _id: "shard1",
   members: [
    {
     _id: 0,
     host: "127.0.0.1:27012"
    },
    {
      id: 0,
      host: "127.0.0.1:27017"
    }
  ]
}
rs.initiate(rsconf)
```
Shard 2
```
mongo --port 27018

rsconf = {
   _id: "shard2",
   members: [
    {
     _id: 0,
     host: "127.0.0.1:27013"
    },
    {
      id: 0,
      host: "127.0.0.1:27018"
    }
  ]
}
rs.initiate(rsconf)
```
Shard 3
```
mongo --port 27019

rsconf = {
   _id: "shard3",
   members: [
    {
     _id: 0,
     host: "127.0.0.1:27014"
    },
    {
      id: 0,
      host: "127.0.0.1:27019"
    }
  ]
}
rs.initiate(rsconf)
```
