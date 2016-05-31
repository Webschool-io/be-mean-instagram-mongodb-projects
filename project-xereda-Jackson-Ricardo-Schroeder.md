ga
# MongoDb - Projeto Final
**Autor:** Jackson Ricardo Schroeder
**Usuário:** [xereda](https://github.com/xereda)

> _Esclarecimentos: Como podem observar, estou usando a mesma estrutura criada pelo usuário `"netoabel"` do github, que por sinal, ficara excelente. Embora a estrutura do documento seja similar, seu conteúdo está integralmente exclusivo, apto para avaliação pelo professor._

**Data:** 1464159904637

## Índice

##### [Introdução](#introdução-1)

* [Caso de uso onde seria interessante utilizar o MongoDB](#caso-de-uso-onde-seria-interessante-utilizar-o-mongodb)

##### [Modelagem](#modelagem)

* [Algumas considerações](#algumas-considerações)

##### [Operações CRUD](#operações-crud)

* [Observações referente a modelagem adotada](#observações-referente-a-modelagem-adotada)
* [Inserindo documentos](#inserindo-documentos)
* [Listando dados dos membros de um projeto](#listando-dados-dos-membros-de-um-projeto)
* [Listando projetos com uma tag específica](#listando-projetos-com-uma-tag-específica)
* [Listando apenas os nomes de todas as atividades](#listando-apenas-os-nomes-de-todas-as-atividades)
* [Listando todos os projetos sem tags](#listando-todos-os-projetos-sem-tags)
* [Listando todos os usuários que não fazem parte do primeiro projeto](#listando-todos-os-usuários-que-não-fazem-parte-do-primeiro-projeto)
* [Adicionando o campo views com valor 0 em todos os projetos](#adicionando-o-campo-views-com-valor-0-em-todos-os-projetos)
* [Adicionando uma tag nova a cada projeto](#adicionando-uma-tag-nova-a-cada-projeto)
* [Adicionando 2 novos membros para cada projeto](#adicionando-2-novos-membros-para-cada-projeto)
* [Adicionando 1 comentário em cada atividade (exceto nas atividades 7 e 8)](#adicionando-1-comentário-em-cada-atividade-exceto-nas-atividades-7-e-8)
* [Adicionando um projeto com upsert](#adicionando-um-projeto-com-upsert)
* [Removendo todos os projetos que não possuem tags](#removendo-todos-os-projetos-que-não-possuem-tags)
* [Removendo todos os projetos que não possuem comentários em suas atividades](#removendo-todos-os-projetos-que-não-possuem-comentários-em-suas-atividades)
* [Removendo todos os projetos que não possuem atividades](#removendo-todos-os-projetos-que-não-possuem-atividades)
* [Removendo todos os projetos dos quais dois usuários específicos fazem parte](#removendo-todos-os-projetos-dos-quais-dois-usuários-específicos-fazem-parte)
* [Removendo todos os projetos com uma tag específica em goals](#removendo-todos-os-projetos-com-uma-tag-específica-em-goals)

##### [Controle de acesso](#controle-de-acesso-1)

* [Criando um usuário com permissões de leitura apenas](#criando-um-usuário-com-permissões-de-leitura-apenas)
* [Criando um usuário com permissões de escrita e leitura](#criando-um-usuário-com-permissões-de-escrita-e-leitura)
* [Habilitando as ações grantRole e revokeRole em um usuário através do papel userAdmin](#habilitando-as-ações-grantrole-e-revokerole-em-um-usuário-através-do-papel-useradmin)
* [Removendo o papel userAdmin](#removendo-o-papel-useradmin)
* [Listando todos os usuários e seus papéis](#listando-todos-os-usuários-e-seus-papéis)

##### [Sharding e replica set](#sharding-e-replica-set-1)

* [Iniciando um config server](#iniciando-um-config-server)
* [Iniciando um router](#iniciando-um-router)
* [Iniciando os shards](#iniciando-os-shards)
* [Registrando os shards](#registrando-os-shards)
* [Habilitando sharding para a collection activities na database be-mean-project](#habilitando-sharding-para-a-collection-activities-na-database-be-mean-project)
* [Criando uma réplica para cada shard](#criando-uma-réplica-para-cada-shard)

# Introdução

## Caso de uso onde seria interessante utilizar o MongoDB

Tenho quase 20 anos na área de tecnologia e minha experiência profissional, quando se trata em modelagem de dados, sempre foi baseada em entidade-relacionamento (MER). Antes de mergulhar no universo do `noSQL`, através do curso **Be MEAN**, quando pensava em especificar um software, na minha mente, a normalização era um processo automático.

Há tempos tinha interesse em conhecer um pouco mais detalhadamente sobre um banco de dados do tipo `noSQL`, sendo o que mais me deixava curioso era a diferente forma de pensar a modelagem do banco de dados. Eu me questionava: “ – Por que repetir as informações?”. “ – Como assim, agrupar os atributos num mesmo conjunto de informações? Que coisa estranha!”.

Mas aos poucos fui entendendo o que era proposto pelos bancos de dados orientados a documentos. Fazemos parte da uma era onde a contratação de recursos físicos de um servidor acaba sendo, provavelmente, a menor parte do investimento num projeto de software. Com isso, uma eventual repetição de dados, mesmo desafiando as já conhecidas formas normais de modelagem, não pode ser considerada como um argumento para a não adoção de um banco de dados como o MongoDB, por exemplo.

Entendo que há um banco de dados noSQL possa ter seus contras. E isso é normal em qualquer tipo de tecnologia. Mas devemos considerar em nossos projetos a adoção destes tipos de banco de dados. Lembrando que dentro do noSQL, temos os bancos orientados a documentos, como é o caso do MongoDB, tem também os de esquema chave/valor (Redis, Riak, Berkeley DB, ...), os de coluna, e ainda os orientados a Grafos como o famoso e robusto Neo4J.

A performance na leitura dos dados e a fácil escalabilidade horizontal são umas das principais características do banco de dados MongoDB. Outro grande atrativo é a facilidade de integrá-lo com as necessidades de dados  de um front-end.

Estou usando o **MongoDB** em um projeto para a área da saúde. Este projeto contará com um front-end (portal web), um aplicativo híbrido e um servidor de aplicação que disponibilizará a integração entre eles através de APIs Rest, com NodeJS e Express.


# Modelagem

Conforme modelo relacional abaixo, vamos recriar a estrutura no MongoDB.

>_(A definição das coleções abaixo está adotando tipagem baseada em Mongoose, neste momento apenas com intuito ilustrativo)_

![](https://raw.githubusercontent.com/netoabel/be-mean-instagram-mongodb-projects/master/modeling/relational.jpg)

### Collection users _(coleção de usuários)_

```js

{
    name: String,
    bio: String,
    register_date: { type: Date, default: new Date() },
    username: String,
    email: String,
    password: String,
    last_access: Date,
    online: Boolean,
    disabled: Boolean,
    hash_token: String,
    background_path: String
}

```

### Collection projects _(coleção de projetos)_

```js

{
    name: string,
    description: String,
    date_begin: Date,
    date_dream: Date,
    date_end: Date,
    realocate: Boolean,
    visible: Boolean,
    expired: Boolean,
    visualizable_mod: Number,
    members: [
          {
            user_id: Schema.Types.ObjectId,            
            type_name: String,
            notify: Boolean
          }
    ],
    tags: [String],
    goals: [
        {
            name: String,
            description: String,
            date_begin: Date,
            date_dream: Date,
            date_end: Date,
            realocate: Boolean,
            expired: Boolean,
            tags: [String],
            historic: [
                { date_realocate: Date }
            ],
            activities: [
                { activity_id: Schema.Types.ObjectId}
            ]
        }
    ]
}

```

## Collection activites _(coleção de atividades)_

```js

{
    name: String,
    description: String,
    date_begin: Date,
    date_dream: Date,
    date_end: Date,
    realocate: Boolean,
    expired: Boolean,
    tags: [String],
    historic: [
        { date_realocate: Date }
    ],
    members: [
        {
            user_id: Schema.Types.ObjectId,
            type_name: String,
            notify: Boolean
        }
    ],
    comments: [
        {
            text String,
            date { type: Date, default: new Date() },
            files: [
                {
                  path: String,
                  weight: Number,
                  name: String
                }
            ],
            members: [
                {
                  user_id Schema.Types.ObjectId,
                  type_name: String,
                  notify: Boolean
                }
            ]
        }
    ]
}

```

## Observações referente a modelagem adotada

A fim de evitar que atinjamos o limite de 16mb em um documento na coleção de projetos, isolamos os campos inerentes às atividades numa nova coleção _(activites collection)_. Na coleção users, incorporamos os campos relacionadas em outra tabela, na notação 1 para 1, para dentro da própria coleção. Outras entidades de relacionamentos, como membros de uma atividade e objetivos de um projeto, foram encapsulados na tabelas principais convertendo-as em coleções mais completas.  


# Operações CRUD

## Inserindo usuários

```js

// array de usuários
var usuarios = ["Jackson", "Ricardo", "Anderson", "Marcos", "Romeu", "Gisela", "Mauricio", "Isabeli", "Clara", "Pedro"];

usuarios.forEach(function(usuario) {

  var objeto = {
                  name: usuario,
                  bio: "Aqui entra as informações detalhadas do usuário " + usuario,
                  register_date: new Date(),
                  username: usuario.toLowerCase(),
                  email: usuario.toLowerCase() + "@mail.com",
                  password: usuario,
                  last_access: new Date(),
                  online: true,
                  disabled: false,
                  hash_token: Math.random().toString(16).substring(2) + Math.random().toString(16).substring(2),
                  background_path: "img/background/" + usuario.toLowerCase() + ".jpg"
                };

   db.users.insert(objeto);
 });

```

## Inserindo projetos

### Inserindo atividades que posteriormente serão vinculadas a Goals de um projeto.

```js

for(x = 1; x <= 10; x++) {

  var atividade = {
        name: "Atividade " + x,
        description: "Descrição da atividade " + x,
        date_begin: new Date()
        };

   db.activities.insert(atividade);

}

```

### Inserindo os projetos

```js

// PROJETO 1
var projeto = {
  name: "Curso BE-MEAN",
  description: "Projeto do Curso BE-MEAN",
  date_begin: new Date(),
  date_dream: new Date().setDate(new Date().getDate() + 30),
  date_dream: new Date().setDate(new Date().getDate() + 45),
  realocate: false,
  visible: true,
  expired: false,
  members: [
    {
      user_id: ObjectId("5748900baa7a22d57d1bf43d"),            
      type_name: "comum",
      notify: true
    },
    {
      user_id: ObjectId("5748900caa7a22d57d1bf43e"),            
      type_name: "comum",
      notify: true
    },
    {
      user_id: ObjectId("5748900caa7a22d57d1bf43f"),            
      type_name: "comum",
      notify: true
    },
    {
      user_id: ObjectId("5748900caa7a22d57d1bf440"),            
      type_name: "comum",
      notify: true
    },
    {
      user_id: ObjectId("5748900caa7a22d57d1bf441"),            
      type_name: "comum",
      notify: true
    }
  ],
  tags: ["mongodb", "express", "angular", "nodejs"],
  goals: [
      {
          name: "Avaliação final",
          description: "Avaliação final do curso junto ao professor",
          date_begin: new Date().setDate(new Date().getDate() + 45),
          date_dream: new Date().setDate(new Date().getDate() + 50),
          date_dream: new Date().setDate(new Date().getDate() + 60),
          realocate: false,
          expired: false,
          tags: ["avaliação", "prova", "mean"],
          activities: [
            { activity_id: ObjectId("57489852aa7a22d57d1bf451") },
            { activity_id: ObjectId("57489852aa7a22d57d1bf452") }
          ]
      }
  ]
};

db.projects.insert(projeto);

// PROJETO 2
var projeto = {
  name: "Curso MongoDB",
  description: "Projeto curso MongoDB",
  date_begin: new Date(),
  date_dream: new Date().setDate(new Date().getDate() + 30),
  date_dream: new Date().setDate(new Date().getDate() + 45),
  realocate: false,
  visible: true,
  expired: false,
  members: [
    {
      user_id: ObjectId("5748900caa7a22d57d1bf442"),            
      type_name: "comum",
      notify: true
    },
    {
      user_id: ObjectId("5748900caa7a22d57d1bf443"),            
      type_name: "comum",
      notify: true
    },
    {
      user_id: ObjectId("5748900caa7a22d57d1bf444"),            
      type_name: "comum",
      notify: true
    },
    {
      user_id: ObjectId("5748900caa7a22d57d1bf445"),            
      type_name: "comum",
      notify: true
    },
    {
      user_id: ObjectId("5748900caa7a22d57d1bf446"),            
      type_name: "comum",
      notify: true
    }
  ],
  tags: ["mongodb"],
  goals: [
      {
          name: "Avaliação final",
          description: "Avaliação final do curso junto ao professor",
          date_begin: new Date().setDate(new Date().getDate() + 45),
          date_dream: new Date().setDate(new Date().getDate() + 50),
          date_dream: new Date().setDate(new Date().getDate() + 60),
          realocate: false,
          expired: false,
          tags: ["avaliação", "prova", "mean"],
          activities: [
            { activity_id: ObjectId("57489852aa7a22d57d1bf453") },
            { activity_id: ObjectId("57489852aa7a22d57d1bf454") }
          ]
      }
  ]
};

db.projects.insert(projeto);

// PROJETO 3
var projeto = {
  name: "Curso Rest APIs com Express e NodeJS",
  description: "Projeto do curso Rest APIs com Express e NodeJS",
  date_begin: new Date(),
  date_dream: new Date().setDate(new Date().getDate() + 30),
  date_dream: new Date().setDate(new Date().getDate() + 45),
  realocate: false,
  visible: true,
  expired: false,
  members: [
    {
      user_id: ObjectId("5748900caa7a22d57d1bf43e"),            
      type_name: "comum",
      notify: true
    },
    {
      user_id: ObjectId("5748900caa7a22d57d1bf440"),            
      type_name: "comum",
      notify: true
    },
    {
      user_id: ObjectId("5748900caa7a22d57d1bf442"),            
      type_name: "comum",
      notify: true
    },
    {
      user_id: ObjectId("5748900caa7a22d57d1bf444"),            
      type_name: "comum",
      notify: true
    },
    {
      user_id: ObjectId("5748900caa7a22d57d1bf446"),            
      type_name: "comum",
      notify: true
    }
  ],
  tags: ["mongodb", "express", "angular", "nodejs"],
  goals: [
      {
          name: "Avaliação final",
          description: "Avaliação final do curso junto ao professor",
          date_begin: new Date().setDate(new Date().getDate() + 45),
          date_dream: new Date().setDate(new Date().getDate() + 50),
          date_dream: new Date().setDate(new Date().getDate() + 60),
          realocate: false,
          expired: false,
          tags: ["avaliação", "prova", "mean"],
          activities: [
            { activity_id: ObjectId("57489852aa7a22d57d1bf455") },
            { activity_id: ObjectId("57489852aa7a22d57d1bf456") }
          ]
      }
  ]
};

db.projects.insert(projeto);

// PROJETO 4
var projeto = {
  name: "Curso de AngularJS",
  description: "Projeto do curso AngularJS",
  date_begin: new Date(),
  date_dream: new Date().setDate(new Date().getDate() + 30),
  date_dream: new Date().setDate(new Date().getDate() + 45),
  realocate: false,
  visible: true,
  expired: false,
  members: [
    {
      user_id: ObjectId("5748900caa7a22d57d1bf43e"),            
      type_name: "comum",
      notify: true
    },
    {
      user_id: ObjectId("5748900caa7a22d57d1bf440"),            
      type_name: "comum",
      notify: true
    },
    {
      user_id: ObjectId("5748900caa7a22d57d1bf442"),            
      type_name: "comum",
      notify: true
    },
    {
      user_id: ObjectId("5748900caa7a22d57d1bf444"),            
      type_name: "comum",
      notify: true
    },
    {
      user_id: ObjectId("5748900caa7a22d57d1bf446"),            
      type_name: "comum",
      notify: true
    }
  ],
  tags: ["angular"],
  goals: [
      {
          name: "Avaliação final",
          description: "Avaliação final do curso junto ao professor",
          date_begin: new Date().setDate(new Date().getDate() + 45),
          date_dream: new Date().setDate(new Date().getDate() + 50),
          date_dream: new Date().setDate(new Date().getDate() + 60),
          realocate: false,
          expired: false,
          tags: ["avaliação", "prova", "mean"],
          activities: [
            { activity_id: ObjectId("57489852aa7a22d57d1bf457") },
            { activity_id: ObjectId("57489852aa7a22d57d1bf458") }
          ]
      }
  ]
};

db.projects.insert(projeto);


// PROJETO 5
var projeto = {
  name: "Curso de PHP e MySQL",
  description: "Projeto do curso PHP com Mysql",
  date_begin: new Date(),
  date_dream: new Date().setDate(new Date().getDate() + 30),
  date_dream: new Date().setDate(new Date().getDate() + 45),
  realocate: false,
  visible: true,
  expired: false,
  members: [
    {
      user_id: ObjectId("5748900caa7a22d57d1bf446"),            
      type_name: "comum",
      notify: true
    },
    {
      user_id: ObjectId("5748900caa7a22d57d1bf445"),            
      type_name: "comum",
      notify: true
    },
    {
      user_id: ObjectId("5748900caa7a22d57d1bf444"),            
      type_name: "comum",
      notify: true
    },
    {
      user_id: ObjectId("5748900caa7a22d57d1bf444"),            
      type_name: "comum",
      notify: true
    },
    {
      user_id: ObjectId("5748900caa7a22d57d1bf446"),            
      type_name: "comum",
      notify: true
    }
  ],
  tags: ["php", "mysql"],
  goals: [
      {
          name: "Avaliação final",
          description: "Avaliação final do curso junto ao professor",
          date_begin: new Date().setDate(new Date().getDate() + 45),
          date_dream: new Date().setDate(new Date().getDate() + 50),
          date_dream: new Date().setDate(new Date().getDate() + 60),
          realocate: false,
          expired: false,
          tags: ["avaliação", "prova", "mean"]
      }
  ]
};

db.projects.insert(projeto);

```


## Listando dados dos membros de um projeto

```js

   var usuarios = [];

   var getUser = function(usuario) {

     usuarios.push(db.users.findOne( { _id: usuario.user_id } ));

   }

   var projeto = db.projects.findOne();
   projeto.members.forEach(getUser);

   usuarios;

```

## Liste todos os projetos com a tag que você escolheu para os 3 projetos em comum.

```js

   db.projects.find({ tags: "mongodb" });

```

## Liste apenas os nomes de todas as atividades para todos os projetos.

```js

   var query = {};
   var fields = {name: 1, _id: 0};
   db.projects.find(query, fields);

   {
     "name": "Curso BE-MEAN"
   }
   {
     "name": "Curso MongoDB"
   }
   {
     "name": "Curso Rest APIs com Express e NodeJS"
   }
   {
     "name": "Curso de AngularJS"
   }
   {
     "name": "Curso de PHP e MySQL"
   }
   Fetched 5 record(s) in 2ms

```

## Listando todos os projetos sem tags

```js

   var query = { tags: { $exists:  false } };
   db.projects.find(query);

   Fetched 0 record(s) in 1ms // NAO RETORNA, POIS TODOS OS PROJETOS POSSUEM PELO MENOS UMA TAG

```

## Listando todos os usuários que não fazem parte do primeiro projeto

```js

var usuariosProjeto1 = [];

var getUsersIDs = function(usuario) {

  // adiciona na array o usuario passado como parametro
  usuariosProjeto1.push(usuario.user_id);

}

// recupera o primeiro projeto
var projeto1 = db.projects.findOne({}, { members: 1 });

// percorre todos os usuario relacionados ao primeiro projeto
projeto1.members.forEach(getUsersIDs);

var query = { _id: { $not: { $in: usuariosProjeto1 } } };
db.users.find(query);

{
  "_id": ObjectId("5748900caa7a22d57d1bf442"),
  "name": "Gisela",
  "bio": "Aqui entra as informações detalhadas do usuário Gisela",
  "register_date": ISODate("2016-05-27T18:21:00.419Z"),
  "username": "gisela",
  "email": "gisela@mail.com",
  "password": "Gisela",
  "last_access": ISODate("2016-05-27T18:21:00.419Z"),
  "online": true,
  "disabled": false,
  "hash_token": "a2a05b39a1ac889736c377c9d2a",
  "background_path": "img/background/gisela.jpg"
}
{
  "_id": ObjectId("5748900caa7a22d57d1bf443"),
  "name": "Mauricio",
  "bio": "Aqui entra as informações detalhadas do usuário Mauricio",
  "register_date": ISODate("2016-05-27T18:21:00.420Z"),
  "username": "mauricio",
  "email": "mauricio@mail.com",
  "password": "Mauricio",
  "last_access": ISODate("2016-05-27T18:21:00.420Z"),
  "online": true,
  "disabled": false,
  "hash_token": "c54f7ccdf292582f85a6f0684cc8",
  "background_path": "img/background/mauricio.jpg"
}
{
  "_id": ObjectId("5748900caa7a22d57d1bf444"),
  "name": "Isabeli",
  "bio": "Aqui entra as informações detalhadas do usuário Isabeli",
  "register_date": ISODate("2016-05-27T18:21:00.421Z"),
  "username": "isabeli",
  "email": "isabeli@mail.com",
  "password": "Isabeli",
  "last_access": ISODate("2016-05-27T18:21:00.421Z"),
  "online": true,
  "disabled": false,
  "hash_token": "fb1267a1e8d32329c49e75f8bd",
  "background_path": "img/background/isabeli.jpg"
}
{
  "_id": ObjectId("5748900caa7a22d57d1bf445"),
  "name": "Clara",
  "bio": "Aqui entra as informações detalhadas do usuário Clara",
  "register_date": ISODate("2016-05-27T18:21:00.422Z"),
  "username": "clara",
  "email": "clara@mail.com",
  "password": "Clara",
  "last_access": ISODate("2016-05-27T18:21:00.422Z"),
  "online": true,
  "disabled": false,
  "hash_token": "da4bccd3113178829916170440e",
  "background_path": "img/background/clara.jpg"
}
{
  "_id": ObjectId("5748900caa7a22d57d1bf446"),
  "name": "Pedro",
  "bio": "Aqui entra as informações detalhadas do usuário Pedro",
  "register_date": ISODate("2016-05-27T18:21:00.422Z"),
  "username": "pedro",
  "email": "pedro@mail.com",
  "password": "Pedro",
  "last_access": ISODate("2016-05-27T18:21:00.422Z"),
  "online": true,
  "disabled": false,
  "hash_token": "f4acff8d5d96e894930e9baadd",
  "background_path": "img/background/pedro.jpg"
}
Fetched 5 record(s) in 3ms

```

## Adicionando o campo views com valor 0 em todos os projetos

```js

   var query = {}
   var modifier = { $set: { views: 0 } }
   var options = { multi: true }
   db.projects.update(query, modifier, options)

   Updated 5 existing record(s) in 2ms
   WriteResult({
     "nMatched": 5,
     "nUpserted": 0,
     "nModified": 5
   })

```

## Adicionando uma tag diferente para cada projeto

```js

var x = 0;

var adicionaTag = function(projeto) {

   var query = { _id: projeto._id };
   var mod = { $push: { tags: "nova tag " + x } };
   db.projects.update(query, mod);

   x++;
}

db.projects.find().forEach(adicionaTag);

Updated 1 existing record(s) in 119ms
Updated 1 existing record(s) in 2ms
Updated 1 existing record(s) in 1ms
Updated 1 existing record(s) in 1ms
Updated 1 existing record(s) in 1ms

```

## Adicione 2 membros diferentes para cada projeto.

```js

var usuarios = function(usuario) {

  var query = { $and: [ { $where: "this.members.length < 7" }, { "members.user_id": { $nin: [ usuario._id ] } }  ] };  
  var options = { multi: true };
  var mod = {
     $push: {
       members: {
         $each: [{
           user_id: usuario._id,
           type_name: "comum",
           notify: true }]
         }
       }
     };

    db.projects.update(query, mod, options);

}


  db.users.find().forEach(usuarios);

```

## Adicione 1 comentário em cada atividade, deixe apenas as ATIVIDADES 3 e 5 sem.

```js

var query = { name: { $nin: [ /Atividade 3/ , /Atividade 5/ ] } };
var options = { multi: true };
var dt_comentario = new Date;
var comentario = {$set: { comments: [{ text: "Novo comentário na atividade",
                                       date: dt_comentario,
                                       files: [{ path: "img/comments/" + dt_comentario.getTime() + ".jpg", weight: 3455664, name: "nova imagem"}],
                                       members: [{ user_id: ObjectId("5748900caa7a22d57d1bf446"), type_name: "comum", notify: true }]
                                     }]
                                   }
                                 };

db.activities.update(query, comentario, options);

```

## Adicionando um projeto com upsert

```js

var query = { name: /Curso de Material Design/i };
var projeto = {
  name: "Curso de Material Design",
  description: "Projeto referente ao curso de Material Design",
  date_begin: new Date(),
  date_dream: new Date().setDate(new Date().getDate() + 30),
  date_dream: new Date().setDate(new Date().getDate() + 45),
  realocate: false,
  visible: true,
  expired: false,
  members: [
    {
      user_id: ObjectId("5748900caa7a22d57d1bf445"),            
      type_name: "comum",
      notify: true
    },
    {
      user_id: ObjectId("5748900caa7a22d57d1bf444"),            
      type_name: "comum",
      notify: true
    },
    {
      user_id: ObjectId("5748900caa7a22d57d1bf446"),            
      type_name: "comum",
      notify: true
    },
    {
      user_id: ObjectId("5748900caa7a22d57d1bf444"),            
      type_name: "comum",
      notify: true
    },
    {
      user_id: ObjectId("5748900caa7a22d57d1bf446"),            
      type_name: "comum",
      notify: true
    }
  ],
  goals: [
      {
          name: "Avaliação final",
          description: "Avaliação final do curso junto ao professor",
          date_begin: new Date().setDate(new Date().getDate() + 45),
          date_dream: new Date().setDate(new Date().getDate() + 50),
          date_dream: new Date().setDate(new Date().getDate() + 60),
          realocate: false,
          expired: false,
          tags: ["avaliação", "prova", "final", "design"]
      }
  ]
};
var mod = { $setOnInsert: projeto };
var options = { upsert: true };
db.projects.update(query, mod, options);


```

## Apague todos os projetos que não possuam tags.

```js

var query = { tags: { $exists: false } };
db.projects.remove(query);
Removed 1 record(s) in 40ms
WriteResult({
  "nRemoved": 1
})

```

## Apague todos os projetos que não possuam comentários nas atividades.

```js

var atividades = function(atividade) {

  var query = { "goals.activities.activity_id": atividade._id };
  db.projects.remove(query);

};

var query = {"comments" : { $exists: false }};
db.activities.find(query).forEach(atividades);
Removed 1 record(s) in 1ms
Removed 1 record(s) in 1ms

```

## Apague todos os projetos que não possuam atividades.

```js

var query = { goals: { $elemMatch: { activities: { $exists: false } } } };
db.projects.remove(query);
Removed 1 record(s) in 1ms
WriteResult({
  "nRemoved": 1
})

```

## Escolha 2 usuário e apague todos os projetos em que os 2 fazem parte.

```js

var query = { $and: [ { "members.user_id": ObjectId("5748900caa7a22d57d1bf43e") },
                      { "members.user_id": ObjectId("5748900caa7a22d57d1bf446") }
                    ]
            };
db.projects.remove(query);
Removed 1 record(s) in 2ms
WriteResult({
  "nRemoved": 1
})

```

## Apague todos os projetos que possuam uma determinada tag em goal.

```js

var query = { goals: { $elemMatch: { tags: "prova" } } };
db.projects.remove(query);
Removed 1 record(s) in 1ms
WriteResult({
  "nRemoved": 1
})

```

# Gerenciamento de usuários

## Crie um usuário com permissões APENAS de Leitura.

```js

var createString = { user: "usuario_leitura", pwd: "senha", roles: ['read'] };
db.createUser(createString);
Successfully added user: {
  "user": "usuario_leitura",
  "roles": [
    "read"
  ]
}

```

## Criando um usuário com permissões de escrita e leitura

```js

var createString = { user: "usuario_escrita", pwd: "senha", roles: ['readWrite'] };
db.createUser(createString);
Successfully added user: {
  "user": "usuario_escrita",
  "roles": [
    "readWrite"
  ]
}

```

## Habilitando as ações grantRole e revokeRole em um usuário através do papel userAdmin

```js

db.grantRolesToUser("usuario_escrita", ["userAdmin"]);

```

## Removendo o papel userAdmin

```js

db.revokeRolesFromUser("usuario_escrita", ["userAdmin"]);

```

## Listando todos os usuários e seus papéis

```js

db.runCommand({ usersInfo: 1 }).users // OU `show users`
[
  {
    "_id": "be-mean.xereda",
    "user": "xereda",
    "db": "be-mean",
    "roles": [
      {
        "role": "readWrite",
        "db": "be-mean"
      }
    ]
  },
  {
    "_id": "be-mean.usuario_leitura",
    "user": "usuario_leitura",
    "db": "be-mean",
    "roles": [
      {
        "role": "read",
        "db": "be-mean"
      }
    ]
  },
  {
    "_id": "be-mean.usuario_escrita",
    "user": "usuario_escrita",
    "db": "be-mean",
    "roles": [
      {
        "role": "readWrite",
        "db": "be-mean"
      }
    ]
  }
]

```

## Sharding

```js

> mkdir /usr/local/var/mongodb/data/configdb
> mkdir /usr/local/var/mongodb/data/shard1
> mkdir /usr/local/var/mongodb/data/shard2
> mkdir /usr/local/var/mongodb/data/shard3

```

- 1 Config Server

```js

> mongod --configsvr --port 27010 --dbpath /usr/local/var/mongodb/data/configsvr

```

- 1 Router

```js

> mongos --configdb localhost:27010 --port 27011

```

- 3 Shardings _já com a configuração para adicionar replica_

```js

> mongod --replSet rs1 --port 27012 --dbpath /usr/local/var/mongodb/data/shard1

> mongo --port 27012
> rsconf = {
  "_id": "rs1",
  "members": [
    {
      "_id": 0,
      "host": "127.0.0.1:27012"
    }
  ]
}
> rs.initiate(rsconf)
{
  "ok": 1
}

```

```js

> mongod --replSet rs2 --port 27013 --dbpath /usr/local/var/mongodb/data/shard2

> mongo --port 27013
> rsconf = {
  "_id": "rs2",
  "members": [
    {
      "_id": 0,
      "host": "127.0.0.1:27013"
    }
  ]
}
> rs.initiate(rsconf)
{
  "ok": 1
}

```

```js

> mongod --replSet rs3 --port 27014 --dbpath /usr/local/var/mongodb/data/shard3

> mongo --port 27014
> rsconf = {
  "_id": "rs3",
  "members": [
    {
      "_id": 0,
      "host": "127.0.0.1:27014"
    }
  ]
}
> rs.initiate(rsconf)
{
  "ok": 1
}

```

```js

> mongo --port 27011

```

```js

mongos> sh.addShard("rs1/127.0.0.1:27012")
{
  "shardAdded": "rs1",
  "ok": 1
}

mongos> sh.addShard("rs2/127.0.0.1:27013")
{
  "shardAdded": "rs2",
  "ok": 1
}

mongos> sh.addShard("rs3/127.0.0.1:27014")
{
  "shardAdded": "rs3",
  "ok": 1
}

mongos> sh.enableSharding("be-mean")
{
  "ok": 1
}

```

- Coleção com _shard_ - `(activities)`

```js

mongos> sh.shardCollection("be-mean.activities", { "_id": 1 })
{
  "collectionsharded": "be-mean.activities",
  "ok": 1
}

```

## Replica

```js

> mkdir /usr/local/var/mongodb/data/rs1
> mkdir /usr/local/var/mongodb/data/rs2
> mkdir /usr/local/var/mongodb/data/rs3

```

```js

> mongod --replSet rs1 --port 27015 --dbpath /usr/local/var/mongodb/data/rs1

```

```js

> mongod --replSet rs2 --port 27016 --dbpath /usr/local/var/mongodb/data/rs2

```

```js

> mongod --replSet rs3 --port 27018 --dbpath /usr/local/var/mongodb/data/rs3

```

- Adicionando uma replica para cada *shard*

```js

> mongo --port 27012
> rs.add("127.0.0.1:27015")
{
  "ok": 1
}

```

```js

> mongo --port 27013
> rs.add("127.0.0.1:27016")
{
  "ok": 1
}

```

```js

> mongo --port 27014
> rs.add("127.0.0.1:27018")
{
  "ok": 1
}

```

- Status final do Cluster

```js

macminixereda(mongos-3.2.0)[mongos] be-mean> db.printShardingStatus()
--- Sharding Status ---
  sharding version: {
    "_id": 1,
    "minCompatibleVersion": 5,
    "currentVersion": 6,
    "clusterId": ObjectId("574d4dbcca44633485cdb7e6")
  }
  shards:
    {  "_id": "rs1",  "host": "rs1/127.0.0.1:27012,127.0.0.1:27015" }
    {  "_id": "rs2",  "host": "rs2/127.0.0.1:27013,127.0.0.1:27016" }
    {  "_id": "rs3",  "host": "rs3/127.0.0.1:27014,127.0.0.1:27018" }
  balancer:
	Currently enabled:  yes
	Currently running:  no
	Failed balancer rounds in last 5 attempts:  0
	Migration Results for the last 24 hours:
		No recent migrations
  databases:
    {  "_id": "be-mean",  "primary": "rs1",  "partitioned": true }
    be-mean.activities
      shard key: { "_id": 1 }
      chunks:
        rs1: 1
        { "_id": [object MinKey] } -> { "_id": [object MaxKey] } on: rs1 Timestamp(1, 0)
    {  "_id": "be-meanasdads",  "primary": "rs1",  "partitioned": true }

```
