# MongoDb - Projeto Final
**Autor:** Fábio Reghim Calheiros
**Data** 1450143520249

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

	member : [{
		user_id
	}],

	tags: [],

	goals : [{
		name,
		description,
		created_time,

		tags: []

		activities: [{
				name,
				description,
				created_time,

			comments : [{
				text,
				date_coment,
				members: [],
				file: []
			}],

			historics: []
		}]
	}]
}

## Explicação de porque a modelagem foi feita dessa forma

```

Ao meu ver, separando a coleção de usuários de projetos, e criando um relação entre si, é possivel se obeter eficiencia de tempo de resposta melhor.
Outro ponto positivo é facilidade com que podemos fazer requisições e obter respostas de forma mais simples e organizada.

Exemplo, buscar todos os projetos de um usuário especifico.

db.projects.find({"member.user_id": ObjectId("566231164d3dcdd17f83d298")})

Caso contrário, não fosse feito dessa forma, uma busca como essa poderia não ser não só mais complicada como isso, como sua performance seria inifinitamente inferior

```


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
Inserted 1 record(s) in 2ms
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
Inserted 1 record(s) in 1ms
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
Inserted 1 record(s) in 1ms
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
Inserted 1 record(s) in 2ms
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
Inserted 1 record(s) in 1ms
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
Inserted 1 record(s) in 1ms
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
... name: "Projeto 1",
... description: "projeto Instagram Mongo DB",
... created_time : Date.now(),
... visible: 1,
... 
... member : [{ user_id: ObjectId("566231164d3dcdd17f83d298")}, { user_id: ObjectId("566231234d3dcdd17f83d299")}, { user_id: ObjectId("566231324d3dcdd17f83d29a")}, { user_id: ObjectId("5662313e4d3dcdd17f83d29b")}, { user_id: ObjectId("5662314e4d3dcdd17f83d29c")}],
... 
... tags: ["futebol", "atletico", "galo"],
... 
... goals : [{
... name : "goal inicial",
... description: "goal do primeiro projeto",
... created_time : Date.now(),
... 
... tags: ["brasil", "america do sul", "galodoido", "vaipracimadelesgalo"],
... 
... activities: [{
... name: "Ativiade 1",
... description: "description atividade",
... created_time : Date.now(),
... 
... comments : [{
... text: "Comentário de teste 1",
... created_time : Date.now(),
... members: [{ user_id: ObjectId("566231164d3dcdd17f83d298")}, { user_id: ObjectId("566231234d3dcdd17f83d299")}, { user_id: ObjectId("566231324d3dcdd17f83d29a")}],
... file: []
... }],
... 
... historics: []
... },
... {
... name: "Ativiade 2",
... description: "description atividade 2",
... created_time : Date.now(),
... 
... comments : [{
... text: "Comentário de teste post 2",
... created_time : Date.now(),
... members: [{ user_id: ObjectId("566231164d3dcdd17f83d298")}, { user_id: ObjectId("566231234d3dcdd17f83d299")}, { user_id: ObjectId("566231324d3dcdd17f83d29a")}],
... file: []
... }],
... 
... historics: []
... }
... ]
... }]
... }
fabio-Inspiron-7520(mongod-3.0.7) be-mean-projeto> db.projects.save(project1)
Inserted 1 record(s) in 3ms
WriteResult({
  "nInserted": 1
})
fabio-Inspiron-7520(mongod-3.0.7) be-mean-projeto> var project2 = {
... name: "Projeto 2",
... description: "projeto Instagram Mongo DB 2",
... created_time : Date.now(),
... visible: 1,
... 
... member : [{ user_id: ObjectId("566231594d3dcdd17f83d29d")}, { user_id: ObjectId("566231644d3dcdd17f83d29e")}, { user_id: ObjectId("566231704d3dcdd17f83d29f")}, { user_id: ObjectId("566231794d3dcdd17f83d2a0")}, { user_id: ObjectId("566231844d3dcdd17f83d2a1")}],
... 
... tags: ["futebol", "drink", "galo"],
... 
... goals : [{
... name : "goal 2",
... description: "goal 2 do projeto",
... created_time : Date.now(),
... 
... tags: ["comida", "gostosa", "brasileira"],
... 
... activities: [{
... name: "Ativiade 1 projeto 2",
... description: "description atividade 1 projeto 2",
... created_time : Date.now(),
... 
... comments : [{
... text: "Comentário de teste 1 projeto 2",
... created_time : Date.now(),
... members: [{ user_id: ObjectId("566231594d3dcdd17f83d29d")}, { user_id: ObjectId("566231644d3dcdd17f83d29e")}, { user_id: ObjectId("566231794d3dcdd17f83d2a0")}],
... file: []
... }],
... 
... historics: []
... },
... {
... name: "Ativiade 2",
... description: "description atividade 2",
... created_time : Date.now(),
... 
... comments : [{
... text: "Comentário de teste post 2",
... created_time : Date.now(),
... members: [{ user_id: ObjectId("566231594d3dcdd17f83d29d")}, { user_id: ObjectId("566231644d3dcdd17f83d29e")}, { user_id: ObjectId("566231794d3dcdd17f83d2a0")}],
... file: []
... }],
... 
... historics: []
... }
... ]
... }]
... }
fabio-Inspiron-7520(mongod-3.0.7) be-mean-projeto> db.projects.save(project2)
Inserted 1 record(s) in 3ms
WriteResult({
  "nInserted": 1
})
fabio-Inspiron-7520(mongod-3.0.7) be-mean-projeto> var project3 = {
... name: "Projeto 3",
... description: "projeto Instagram Mongo DB 3",
... created_time : Date.now(),
... visible: 1,
... 
... member : [{ user_id: ObjectId("566231594d3dcdd17f83d29d")}, { user_id: ObjectId("566231324d3dcdd17f83d29a")}, { user_id: ObjectId("566231704d3dcdd17f83d29f")}, { user_id: ObjectId("566231234d3dcdd17f83d299")}, { user_id: ObjectId("566231844d3dcdd17f83d2a1")}],
... 
... tags: ["tag1", "tag2", "futebol"],
... 
... goals : [{
... name : "goal 3",
... description: "goal 3 do projeto",
... created_time : Date.now(),
... 
... tags: ["goal1", "goal2", "goal3"],
... 
... activities: [{
... name: "Ativiade 1 projeto 3",
... description: "description atividade 1 projeto 3",
... created_time : Date.now(),
... 
... comments : [{
... text: "Comentário de teste 1 projeto 3",
... created_time : Date.now(),
... members: [{ user_id: ObjectId("566231594d3dcdd17f83d29d")}, { user_id: ObjectId("566231324d3dcdd17f83d29a")}, { user_id: ObjectId("566231704d3dcdd17f83d29f")}],
... file: []
... }],
... 
... historics: []
... },
... {
... name: "Ativiade 2 do projeto 3",
... description: "description atividade 3",
... created_time : Date.now(),
... 
... comments : [{
... text: "Comentário de teste post 3",
... created_time : Date.now(),
... members: [{ user_id: ObjectId("566231594d3dcdd17f83d29d")}, { user_id: ObjectId("566231324d3dcdd17f83d29a")}, { user_id: ObjectId("566231704d3dcdd17f83d29f")}],
... file: []
... }],
... 
... historics: []
... }
... ]
... }]
... }
fabio-Inspiron-7520(mongod-3.0.7) be-mean-projeto> db.projects.save(project3)
Inserted 1 record(s) in 3ms
WriteResult({
  "nInserted": 1
})
fabio-Inspiron-7520(mongod-3.0.7) be-mean-projeto> var project4 = {
... name: "Projeto 4",
... description: "projeto Instagram Mongo DB 4",
... created_time : Date.now(),
... visible: 1,
... 
... member : [{ user_id: ObjectId("5662313e4d3dcdd17f83d29b")}, { user_id: ObjectId("566231594d3dcdd17f83d29d")}, { user_id: ObjectId("566231644d3dcdd17f83d29e")}, { user_id: ObjectId("566231704d3dcdd17f83d29f")}, { user_id: ObjectId("566231794d3dcdd17f83d2a0")}],
... 
... tags: ["tag4", "tag5", "tag6"],
... 
... goals : [{
... name : "goal 4",
... description: "goal 4 do projeto",
... created_time : Date.now(),
... 
... tags: ["goal4", "goal5", "goal6"],
... 
... activities: [{
... name: "Ativiade 1 projeto 4",
... description: "description atividade 1 projeto 4",
... created_time : Date.now(),
... 
... comments : [{
... text: "Comentário de teste 1 projeto 4",
... created_time : Date.now(),
... members: [{ user_id: ObjectId("5662313e4d3dcdd17f83d29b")}, { user_id: ObjectId("566231594d3dcdd17f83d29d")}, { user_id: ObjectId("566231644d3dcdd17f83d29e")}],
... file: []
... }],
... 
... historics: []
... },
... {
... name: "Ativiade 2 do projeto 3",
... description: "description atividade 3",
... created_time : Date.now(),
... 
... comments : [{
... text: "Comentário de teste post 3",
... created_time : Date.now(),
... members: [{ user_id: ObjectId("5662313e4d3dcdd17f83d29b")}, { user_id: ObjectId("566231594d3dcdd17f83d29d")}, { user_id: ObjectId("566231644d3dcdd17f83d29e")}],
... file: []
... }],
... 
... historics: []
... }
... ]
... }]
... }
fabio-Inspiron-7520(mongod-3.0.7) be-mean-projeto> db.projects.save(project4)
Inserted 1 record(s) in 3ms
WriteResult({
  "nInserted": 1
})
fabio-Inspiron-7520(mongod-3.0.7) be-mean-projeto> var project5 = {
... name: "Projeto 5",
... description: "projeto Instagram Mongo DB 5",
... created_time : Date.now(),
... visible: 1,
... 
... member : [{ user_id: ObjectId("5662313e4d3dcdd17f83d29b")}, { user_id: ObjectId("566231594d3dcdd17f83d29d")}, { user_id: ObjectId("566231644d3dcdd17f83d29e")}, { user_id: ObjectId("566231704d3dcdd17f83d29f")}, { user_id: ObjectId("566231794d3dcdd17f83d2a0")}],
... 
... tags: ["tag7", "tag8", "tag9"],
... 
... goals : [{
... name : "goal 5",
... description: "goal 5 do projeto",
... created_time : Date.now(),
... 
... tags: ["goal7", "goal8", "goal9"]
... }]
... }
fabio-Inspiron-7520(mongod-3.0.7) be-mean-projeto> db.projects.save(project5)
Inserted 1 record(s) in 2ms
WriteResult({
  "nInserted": 1
})



```

## Retrieve - busca

```

### 1 - Liste as informações dos membros de 1 projeto específico que deve ser buscado pelo seu nome de forma a não ligar para maiúsculas e minúsculas.

```

fabio-Inspiron-7520(mongod-3.0.7) be-mean-projeto> var query = {name: /projeto 1/i}
fabio-Inspiron-7520(mongod-3.0.7) be-mean-projeto> var fields = {member: 1, _id:0}
fabio-Inspiron-7520(mongod-3.0.7) be-mean-projeto> db.projects.find(query, fields)
{
  "member": [
    {
      "user_id": ObjectId("566231164d3dcdd17f83d298")
    },
    {
      "user_id": ObjectId("566231234d3dcdd17f83d299")
    },
    {
      "user_id": ObjectId("566231324d3dcdd17f83d29a")
    },
    {
      "user_id": ObjectId("5662313e4d3dcdd17f83d29b")
    },
    {
      "user_id": ObjectId("5662314e4d3dcdd17f83d29c")
    }
  ]
}
Fetched 1 record(s) in 2ms


```

### 2 - Liste todos os projetos com a tag que você escolheu para os 3 projetos em comum.

```

fabio-Inspiron-7520(mongod-3.0.7) be-mean-projeto> var query = {tags: {$in: [/futebol/i]}}
fabio-Inspiron-7520(mongod-3.0.7) be-mean-projeto> db.projects.find(query)
{
  "_id": ObjectId("56660924661f1e8ea0aec016"),
  "name": "Projeto 1",
  "description": "projeto Instagram Mongo DB",
  "created_time": 1449527584078,
  "visible": 1,
  "member": [
    {
      "user_id": ObjectId("566231164d3dcdd17f83d298")
    },
    {
      "user_id": ObjectId("566231234d3dcdd17f83d299")
    },
    {
      "user_id": ObjectId("566231324d3dcdd17f83d29a")
    },
    {
      "user_id": ObjectId("5662313e4d3dcdd17f83d29b")
    },
    {
      "user_id": ObjectId("5662314e4d3dcdd17f83d29c")
    }
  ],
  "tags": [
    "futebol",
    "atletico",
    "galo"
  ],
  "goals": [
    {
      "name": "goal inicial",
      "description": "goal do primeiro projeto",
      "created_time": 1449527584078,
      "tags": [
        "brasil",
        "america do sul",
        "galodoido",
        "vaipracimadelesgalo"
      ],
      "activities": [
        {
          "name": "Ativiade 1",
          "description": "description atividade",
          "created_time": 1449527584078,
          "comments": [
            {
              "text": "Comentário de teste 1",
              "created_time": 1449527584078,
              "members": [
                {
                  "user_id": ObjectId("566231164d3dcdd17f83d298")
                },
                {
                  "user_id": ObjectId("566231234d3dcdd17f83d299")
                },
                {
                  "user_id": ObjectId("566231324d3dcdd17f83d29a")
                }
              ],
              "file": [ ]
            }
          ],
          "historics": [ ]
        },
        {
          "name": "Ativiade 2",
          "description": "description atividade 2",
          "created_time": 1449527584078,
          "comments": [
            {
              "text": "Comentário de teste post 2",
              "created_time": 1449527584078,
              "members": [
                {
                  "user_id": ObjectId("566231164d3dcdd17f83d298")
                },
                {
                  "user_id": ObjectId("566231234d3dcdd17f83d299")
                },
                {
                  "user_id": ObjectId("566231324d3dcdd17f83d29a")
                }
              ],
              "file": [ ]
            }
          ],
          "historics": [ ]
        }
      ]
    }
  ]
}
{
  "_id": ObjectId("5666092f661f1e8ea0aec017"),
  "name": "Projeto 2",
  "description": "projeto Instagram Mongo DB 2",
  "created_time": 1449527596407,
  "visible": 1,
  "member": [
    {
      "user_id": ObjectId("566231594d3dcdd17f83d29d")
    },
    {
      "user_id": ObjectId("566231644d3dcdd17f83d29e")
    },
    {
      "user_id": ObjectId("566231704d3dcdd17f83d29f")
    },
    {
      "user_id": ObjectId("566231794d3dcdd17f83d2a0")
    },
    {
      "user_id": ObjectId("566231844d3dcdd17f83d2a1")
    }
  ],
  "tags": [
    "futebol",
    "drink",
    "galo"
  ],
  "goals": [
    {
      "name": "goal 2",
      "description": "goal 2 do projeto",
      "created_time": 1449527596407,
      "tags": [
        "comida",
        "gostosa",
        "brasileira"
      ],
      "activities": [
        {
          "name": "Ativiade 1 projeto 2",
          "description": "description atividade 1 projeto 2",
          "created_time": 1449527596407,
          "comments": [
            {
              "text": "Comentário de teste 1 projeto 2",
              "created_time": 1449527596407,
              "members": [
                {
                  "user_id": ObjectId("566231594d3dcdd17f83d29d")
                },
                {
                  "user_id": ObjectId("566231644d3dcdd17f83d29e")
                },
                {
                  "user_id": ObjectId("566231794d3dcdd17f83d2a0")
                }
              ],
              "file": [ ]
            }
          ],
          "historics": [ ]
        },
        {
          "name": "Ativiade 2",
          "description": "description atividade 2",
          "created_time": 1449527596407,
          "comments": [
            {
              "text": "Comentário de teste post 2",
              "created_time": 1449527596407,
              "members": [
                {
                  "user_id": ObjectId("566231594d3dcdd17f83d29d")
                },
                {
                  "user_id": ObjectId("566231644d3dcdd17f83d29e")
                },
                {
                  "user_id": ObjectId("566231794d3dcdd17f83d2a0")
                }
              ],
              "file": [ ]
            }
          ],
          "historics": [ ]
        }
      ]
    }
  ]
}
{
  "_id": ObjectId("5666093e661f1e8ea0aec018"),
  "name": "Projeto 3",
  "description": "projeto Instagram Mongo DB 3",
  "created_time": 1449527610573,
  "visible": 1,
  "member": [
    {
      "user_id": ObjectId("566231594d3dcdd17f83d29d")
    },
    {
      "user_id": ObjectId("566231324d3dcdd17f83d29a")
    },
    {
      "user_id": ObjectId("566231704d3dcdd17f83d29f")
    },
    {
      "user_id": ObjectId("566231234d3dcdd17f83d299")
    },
    {
      "user_id": ObjectId("566231844d3dcdd17f83d2a1")
    }
  ],
  "tags": [
    "tag1",
    "tag2",
    "futebol"
  ],
  "goals": [
    {
      "name": "goal 3",
      "description": "goal 3 do projeto",
      "created_time": 1449527610573,
      "tags": [
        "goal1",
        "goal2",
        "goal3"
      ],
      "activities": [
        {
          "name": "Ativiade 1 projeto 3",
          "description": "description atividade 1 projeto 3",
          "created_time": 1449527610573,
          "comments": [
            {
              "text": "Comentário de teste 1 projeto 3",
              "created_time": 1449527610573,
              "members": [
                {
                  "user_id": ObjectId("566231594d3dcdd17f83d29d")
                },
                {
                  "user_id": ObjectId("566231324d3dcdd17f83d29a")
                },
                {
                  "user_id": ObjectId("566231704d3dcdd17f83d29f")
                }
              ],
              "file": [ ]
            }
          ],
          "historics": [ ]
        },
        {
          "name": "Ativiade 2 do projeto 3",
          "description": "description atividade 3",
          "created_time": 1449527610573,
          "comments": [
            {
              "text": "Comentário de teste post 3",
              "created_time": 1449527610573,
              "members": [
                {
                  "user_id": ObjectId("566231594d3dcdd17f83d29d")
                },
                {
                  "user_id": ObjectId("566231324d3dcdd17f83d29a")
                },
                {
                  "user_id": ObjectId("566231704d3dcdd17f83d29f")
                }
              ],
              "file": [ ]
            }
          ],
          "historics": [ ]
        }
      ]
    }
  ]
}
Fetched 3 record(s) in 7ms


```

### 3 - Liste apenas os nomes de todas as atividades para todos os projetos.

```

var query = {}
fabio-Inspiron-7520(mongod-3.0.7) be-mean-projeto> var fields = {"goals.activities.name": "", _id:0}
fabio-Inspiron-7520(mongod-3.0.7) be-mean-projeto> db.projects.find(query, fields)
{
  "goals": [
    {
      "activities": [
        {
          "name": "Ativiade 1"
        },
        {
          "name": "Ativiade 2"
        }
      ]
    }
  ]
}
{
  "goals": [
    {
      "activities": [
        {
          "name": "Ativiade 1 projeto 2"
        },
        {
          "name": "Ativiade 2"
        }
      ]
    }
  ]
}
{
  "goals": [
    {
      "activities": [
        {
          "name": "Ativiade 1 projeto 3"
        },
        {
          "name": "Ativiade 2 do projeto 3"
        }
      ]
    }
  ]
}
{
  "goals": [
    {
      
    }
  ]
}
{
  "goals": [
    {
      "activities": [
        {
          "name": "Ativiade 1 projeto 4"
        },
        {
          "name": "Ativiade 2 do projeto 3"
        }
      ]
    }
  ]
}
Fetched 5 record(s) in 3ms


```

### 4 - Liste todos os projetos que não possuam uma tag.

```

fabio-Inspiron-7520(mongod-3.0.7) be-mean-projeto> db.projects.find( { tags: { $exists: false } } )
Fetched 0 record(s) in 1ms

```

### 5 - Liste todos os usuários que não fazem parte do primeiro projeto cadastrado.

```

fabio-Inspiron-7520(mongod-3.0.7) be-mean-projeto> var query = {}
fabio-Inspiron-7520(mongod-3.0.7) be-mean-projeto> var fields = {_id:1}
fabio-Inspiron-7520(mongod-3.0.7) be-mean-projeto> var idPrimeiro = db.projects.findOne({ }, fields)
fabio-Inspiron-7520(mongod-3.0.7) be-mean-projeto> 
fabio-Inspiron-7520(mongod-3.0.7) be-mean-projeto> var fildsMembers = {member: 1, _id: 0}
fabio-Inspiron-7520(mongod-3.0.7) be-mean-projeto> 
fabio-Inspiron-7520(mongod-3.0.7) be-mean-projeto> var usuariosPrimeiroProjeto = db.projects.find(idPrimeiro, fildsMembers);
fabio-Inspiron-7520(mongod-3.0.7) be-mean-projeto> 
fabio-Inspiron-7520(mongod-3.0.7) be-mean-projeto> var arrFinal = []
fabio-Inspiron-7520(mongod-3.0.7) be-mean-projeto> 
fabio-Inspiron-7520(mongod-3.0.7) be-mean-projeto> for (var member in usuariosPrimeiroProjeto[0].member) {
...    if (usuariosPrimeiroProjeto[0].member.hasOwnProperty(member)) {
...       var obj = usuariosPrimeiroProjeto[0].member[member];
...        
...         for (var user_id in obj) {
...           if(obj.hasOwnProperty(user_id)){
...              arrFinal.push(obj[user_id]);
...           }
...        }
...     }
... }
5
fabio-Inspiron-7520(mongod-3.0.7) be-mean-projeto> 
fabio-Inspiron-7520(mongod-3.0.7) be-mean-projeto> var query = {_id: {$nin: arrFinal}}
fabio-Inspiron-7520(mongod-3.0.7) be-mean-projeto> db.users.find(query)
{
  "_id": ObjectId("566231594d3dcdd17f83d29d"),
  "type": "user",
  "dateRegister": 1449275734435,
  "username": "bambirra",
  "full_name": "Matheus Menezes",
  "email": "menezesdebambirra@gmail.com",
  "password": 6,
  "lastAcess": 1449275734435,
  "disable": 0
}
{
  "_id": ObjectId("566231644d3dcdd17f83d29e"),
  "type": "user",
  "dateRegister": 1449275745216,
  "username": "luizfelipe",
  "full_name": "Luiz Felipe",
  "email": "luiz.felipe@gmail.com",
  "password": 7,
  "lastAcess": 1449275745216,
  "disable": 0
}
{
  "_id": ObjectId("566231704d3dcdd17f83d29f"),
  "type": "user",
  "dateRegister": 1449275757329,
  "username": "lele84",
  "full_name": "Alessandra Negrine",
  "email": "lele.negrine@gmail.com",
  "password": 8,
  "lastAcess": 1449275757329,
  "disable": 0
}
{
  "_id": ObjectId("566231794d3dcdd17f83d2a0"),
  "type": "user",
  "dateRegister": 1449275767219,
  "username": "matheusaraujo",
  "full_name": "Mateus Vireira",
  "email": "ocarala@gmail.com",
  "password": 9,
  "lastAcess": 1449275767219,
  "disable": 0
}
{
  "_id": ObjectId("566231844d3dcdd17f83d2a1"),
  "type": "user",
  "dateRegister": 1449275777145,
  "username": "usermagic",
  "full_name": "Usário Mágico",
  "email": "magic@gmail.com",
  "password": 10,
  "lastAcess": 1449275777145,
  "disable": 0
}
Fetched 5 record(s) in 3ms


```

## Update - alteração

### 1 - Adicione para todos os projetos o campo views: 0.

```

fabio-Inspiron-7520(mongod-3.0.7) be-mean-projeto> var query = {}
fabio-Inspiron-7520(mongod-3.0.7) be-mean-projeto> var mod = {$set: {views: 0}}
fabio-Inspiron-7520(mongod-3.0.7) be-mean-projeto> var options = {multi: true}
fabio-Inspiron-7520(mongod-3.0.7) be-mean-projeto> db.projects.update(query, mod, options)
Updated 5 existing record(s) in 25ms
WriteResult({
  "nMatched": 5,
  "nUpserted": 0,
  "nModified": 5
})
fabio-Inspiron-7520(mongod-3.0.7) be-mean-projeto> db.projects.find()
{
  "_id": ObjectId("56660924661f1e8ea0aec016"),
  "name": "Projeto 1",
  "description": "projeto Instagram Mongo DB",
  "created_time": 1449527584078,
  "visible": 1,
  "member": [
    {
      "user_id": ObjectId("566231164d3dcdd17f83d298")
    },
    {
      "user_id": ObjectId("566231234d3dcdd17f83d299")
    },
    {
      "user_id": ObjectId("566231324d3dcdd17f83d29a")
    },
    {
      "user_id": ObjectId("5662313e4d3dcdd17f83d29b")
    },
    {
      "user_id": ObjectId("5662314e4d3dcdd17f83d29c")
    }
  ],
  "tags": [
    "futebol",
    "atletico",
    "galo"
  ],
  "goals": [
    {
      "name": "goal inicial",
      "description": "goal do primeiro projeto",
      "created_time": 1449527584078,
      "tags": [
        "brasil",
        "america do sul",
        "galodoido",
        "vaipracimadelesgalo"
      ],
      "activities": [
        {
          "name": "Ativiade 1",
          "description": "description atividade",
          "created_time": 1449527584078,
          "comments": [
            {
              "text": "Comentário de teste 1",
              "created_time": 1449527584078,
              "members": [
                {
                  "user_id": ObjectId("566231164d3dcdd17f83d298")
                },
                {
                  "user_id": ObjectId("566231234d3dcdd17f83d299")
                },
                {
                  "user_id": ObjectId("566231324d3dcdd17f83d29a")
                }
              ],
              "file": [ ]
            }
          ],
          "historics": [ ]
        },
        {
          "name": "Ativiade 2",
          "description": "description atividade 2",
          "created_time": 1449527584078,
          "comments": [
            {
              "text": "Comentário de teste post 2",
              "created_time": 1449527584078,
              "members": [
                {
                  "user_id": ObjectId("566231164d3dcdd17f83d298")
                },
                {
                  "user_id": ObjectId("566231234d3dcdd17f83d299")
                },
                {
                  "user_id": ObjectId("566231324d3dcdd17f83d29a")
                }
              ],
              "file": [ ]
            }
          ],
          "historics": [ ]
        }
      ]
    }
  ],
  "views": 0
}
{
  "_id": ObjectId("5666092f661f1e8ea0aec017"),
  "name": "Projeto 2",
  "description": "projeto Instagram Mongo DB 2",
  "created_time": 1449527596407,
  "visible": 1,
  "member": [
    {
      "user_id": ObjectId("566231594d3dcdd17f83d29d")
    },
    {
      "user_id": ObjectId("566231644d3dcdd17f83d29e")
    },
    {
      "user_id": ObjectId("566231704d3dcdd17f83d29f")
    },
    {
      "user_id": ObjectId("566231794d3dcdd17f83d2a0")
    },
    {
      "user_id": ObjectId("566231844d3dcdd17f83d2a1")
    }
  ],
  "tags": [
    "futebol",
    "drink",
    "galo"
  ],
  "goals": [
    {
      "name": "goal 2",
      "description": "goal 2 do projeto",
      "created_time": 1449527596407,
      "tags": [
        "comida",
        "gostosa",
        "brasileira"
      ],
      "activities": [
        {
          "name": "Ativiade 1 projeto 2",
          "description": "description atividade 1 projeto 2",
          "created_time": 1449527596407,
          "comments": [
            {
              "text": "Comentário de teste 1 projeto 2",
              "created_time": 1449527596407,
              "members": [
                {
                  "user_id": ObjectId("566231594d3dcdd17f83d29d")
                },
                {
                  "user_id": ObjectId("566231644d3dcdd17f83d29e")
                },
                {
                  "user_id": ObjectId("566231794d3dcdd17f83d2a0")
                }
              ],
              "file": [ ]
            }
          ],
          "historics": [ ]
        },
        {
          "name": "Ativiade 2",
          "description": "description atividade 2",
          "created_time": 1449527596407,
          "comments": [
            {
              "text": "Comentário de teste post 2",
              "created_time": 1449527596407,
              "members": [
                {
                  "user_id": ObjectId("566231594d3dcdd17f83d29d")
                },
                {
                  "user_id": ObjectId("566231644d3dcdd17f83d29e")
                },
                {
                  "user_id": ObjectId("566231794d3dcdd17f83d2a0")
                }
              ],
              "file": [ ]
            }
          ],
          "historics": [ ]
        }
      ]
    }
  ],
  "views": 0
}
{
  "_id": ObjectId("5666093e661f1e8ea0aec018"),
  "name": "Projeto 3",
  "description": "projeto Instagram Mongo DB 3",
  "created_time": 1449527610573,
  "visible": 1,
  "member": [
    {
      "user_id": ObjectId("566231594d3dcdd17f83d29d")
    },
    {
      "user_id": ObjectId("566231324d3dcdd17f83d29a")
    },
    {
      "user_id": ObjectId("566231704d3dcdd17f83d29f")
    },
    {
      "user_id": ObjectId("566231234d3dcdd17f83d299")
    },
    {
      "user_id": ObjectId("566231844d3dcdd17f83d2a1")
    }
  ],
  "tags": [
    "tag1",
    "tag2",
    "futebol"
  ],
  "goals": [
    {
      "name": "goal 3",
      "description": "goal 3 do projeto",
      "created_time": 1449527610573,
      "tags": [
        "goal1",
        "goal2",
        "goal3"
      ],
      "activities": [
        {
          "name": "Ativiade 1 projeto 3",
          "description": "description atividade 1 projeto 3",
          "created_time": 1449527610573,
          "comments": [
            {
              "text": "Comentário de teste 1 projeto 3",
              "created_time": 1449527610573,
              "members": [
                {
                  "user_id": ObjectId("566231594d3dcdd17f83d29d")
                },
                {
                  "user_id": ObjectId("566231324d3dcdd17f83d29a")
                },
                {
                  "user_id": ObjectId("566231704d3dcdd17f83d29f")
                }
              ],
              "file": [ ]
            }
          ],
          "historics": [ ]
        },
        {
          "name": "Ativiade 2 do projeto 3",
          "description": "description atividade 3",
          "created_time": 1449527610573,
          "comments": [
            {
              "text": "Comentário de teste post 3",
              "created_time": 1449527610573,
              "members": [
                {
                  "user_id": ObjectId("566231594d3dcdd17f83d29d")
                },
                {
                  "user_id": ObjectId("566231324d3dcdd17f83d29a")
                },
                {
                  "user_id": ObjectId("566231704d3dcdd17f83d29f")
                }
              ],
              "file": [ ]
            }
          ],
          "historics": [ ]
        }
      ]
    }
  ],
  "views": 0
}
{
  "_id": ObjectId("56660958661f1e8ea0aec01a"),
  "name": "Projeto 5",
  "description": "projeto Instagram Mongo DB 5",
  "created_time": 1449527637133,
  "visible": 1,
  "member": [
    {
      "user_id": ObjectId("5662313e4d3dcdd17f83d29b")
    },
    {
      "user_id": ObjectId("566231594d3dcdd17f83d29d")
    },
    {
      "user_id": ObjectId("566231644d3dcdd17f83d29e")
    },
    {
      "user_id": ObjectId("566231704d3dcdd17f83d29f")
    },
    {
      "user_id": ObjectId("566231794d3dcdd17f83d2a0")
    }
  ],
  "tags": [
    "tag7",
    "tag8",
    "tag9"
  ],
  "goals": [
    {
      "name": "goal 5",
      "description": "goal 5 do projeto",
      "created_time": 1449527637133,
      "tags": [
        "goal7",
        "goal8",
        "goal9"
      ]
    }
  ],
  "views": 0
}
{
  "_id": ObjectId("5666094e661f1e8ea0aec019"),
  "name": "Projeto 4",
  "description": "projeto Instagram Mongo DB 4",
  "created_time": 1449527623979,
  "visible": 1,
  "member": [
    {
      "user_id": ObjectId("5662313e4d3dcdd17f83d29b")
    },
    {
      "user_id": ObjectId("566231594d3dcdd17f83d29d")
    },
    {
      "user_id": ObjectId("566231644d3dcdd17f83d29e")
    },
    {
      "user_id": ObjectId("566231704d3dcdd17f83d29f")
    },
    {
      "user_id": ObjectId("566231794d3dcdd17f83d2a0")
    }
  ],
  "tags": [
    "tag4",
    "tag5",
    "tag6"
  ],
  "goals": [
    {
      "name": "goal 4",
      "description": "goal 4 do projeto",
      "created_time": 1449527623979,
      "tags": [
        "goal4",
        "goal5",
        "goal6"
      ],
      "activities": [
        {
          "name": "Ativiade 1 projeto 4",
          "description": "description atividade 1 projeto 4",
          "created_time": 1449527623979,
          "comments": [
            {
              "text": "Comentário de teste 1 projeto 4",
              "created_time": 1449527623979,
              "members": [
                {
                  "user_id": ObjectId("5662313e4d3dcdd17f83d29b")
                },
                {
                  "user_id": ObjectId("566231594d3dcdd17f83d29d")
                },
                {
                  "user_id": ObjectId("566231644d3dcdd17f83d29e")
                }
              ],
              "file": [ ]
            }
          ],
          "historics": [ ]
        },
        {
          "name": "Ativiade 2 do projeto 3",
          "description": "description atividade 3",
          "created_time": 1449527623979,
          "comments": [
            {
              "text": "Comentário de teste post 3",
              "created_time": 1449527623979,
              "members": [
                {
                  "user_id": ObjectId("5662313e4d3dcdd17f83d29b")
                },
                {
                  "user_id": ObjectId("566231594d3dcdd17f83d29d")
                },
                {
                  "user_id": ObjectId("566231644d3dcdd17f83d29e")
                }
              ],
              "file": [ ]
            }
          ],
          "historics": [ ]
        }
      ]
    }
  ],
  "views": 0
}
Fetched 5 record(s) in 14ms


```

### 2 - Adicione 1 tag diferente para cada projeto.

```

fabio-Inspiron-7520(mongod-3.0.7) be-mean-projeto> var query = {_id: ObjectId("56660924661f1e8ea0aec016")}
fabio-Inspiron-7520(mongod-3.0.7) be-mean-projeto> var mod = {$push: {tags: 'tagdiferente1'}}
fabio-Inspiron-7520(mongod-3.0.7) be-mean-projeto> db.projects.update(query, mod)
Updated 1 existing record(s) in 2ms
WriteResult({
  "nMatched": 1,
  "nUpserted": 0,
  "nModified": 1
})

fabio-Inspiron-7520(mongod-3.0.7) be-mean-projeto> var query = {_id: ObjectId("5666092f661f1e8ea0aec017")}
fabio-Inspiron-7520(mongod-3.0.7) be-mean-projeto> var mod = {$push: {tags: 'tagdiferente2'}}
fabio-Inspiron-7520(mongod-3.0.7) be-mean-projeto> db.projects.update(query, mod)
Updated 1 existing record(s) in 0ms
WriteResult({
  "nMatched": 1,
  "nUpserted": 0,
  "nModified": 1
})
fabio-Inspiron-7520(mongod-3.0.7) be-mean-projeto> 
fabio-Inspiron-7520(mongod-3.0.7) be-mean-projeto> var query = {_id: ObjectId("5666093e661f1e8ea0aec018")}
fabio-Inspiron-7520(mongod-3.0.7) be-mean-projeto> var mod = {$push: {tags: 'tagdiferente3'}}
fabio-Inspiron-7520(mongod-3.0.7) be-mean-projeto> db.projects.update(query, mod)
Updated 1 existing record(s) in 1ms
WriteResult({
  "nMatched": 1,
  "nUpserted": 0,
  "nModified": 1
})
fabio-Inspiron-7520(mongod-3.0.7) be-mean-projeto> 
fabio-Inspiron-7520(mongod-3.0.7) be-mean-projeto> var query = {_id: ObjectId("56660958661f1e8ea0aec01a")}
fabio-Inspiron-7520(mongod-3.0.7) be-mean-projeto> var mod = {$push: {tags: 'tagdiferente4'}}
fabio-Inspiron-7520(mongod-3.0.7) be-mean-projeto> db.projects.update(query, mod)
Updated 1 existing record(s) in 1ms
WriteResult({
  "nMatched": 1,
  "nUpserted": 0,
  "nModified": 1
})
fabio-Inspiron-7520(mongod-3.0.7) be-mean-projeto> 
fabio-Inspiron-7520(mongod-3.0.7) be-mean-projeto> var query = {_id: ObjectId("5666094e661f1e8ea0aec019")}
fabio-Inspiron-7520(mongod-3.0.7) be-mean-projeto> var mod = {$push: {tags: 'tagdiferente5'}}
fabio-Inspiron-7520(mongod-3.0.7) be-mean-projeto> db.projects.update(query, mod)
Updated 1 existing record(s) in 2ms
WriteResult({
  "nMatched": 1,
  "nUpserted": 0,
  "nModified": 1
})


```
### 3 - Adicione 2 membros diferentes para cada projeto.

```


fabio-Inspiron-7520(mongod-3.0.7) be-mean-projeto> var query = {_id: ObjectId("56660924661f1e8ea0aec016")}
fabio-Inspiron-7520(mongod-3.0.7) be-mean-projeto> 
fabio-Inspiron-7520(mongod-3.0.7) be-mean-projeto> var usuarios = [{"user_id": ObjectId("566231594d3dcdd17f83d29d")}, {"user_id": ObjectId("566231644d3dcdd17f83d29e")}]
fabio-Inspiron-7520(mongod-3.0.7) be-mean-projeto> 
fabio-Inspiron-7520(mongod-3.0.7) be-mean-projeto> var mod = {$pushAll: {member: usuarios}}
fabio-Inspiron-7520(mongod-3.0.7) be-mean-projeto> db.projects.update(query, mod)
Updated 1 existing record(s) in 5ms
WriteResult({
  "nMatched": 1,
  "nUpserted": 0,
  "nModified": 1
})
fabio-Inspiron-7520(mongod-3.0.7) be-mean-projeto> db.projects.find(query)
{
  "_id": ObjectId("56660924661f1e8ea0aec016"),
  "name": "Projeto 1",
  "description": "projeto Instagram Mongo DB",
  "created_time": 1449527584078,
  "visible": 1,
  "member": [
    {
      "user_id": ObjectId("566231164d3dcdd17f83d298")
    },
    {
      "user_id": ObjectId("566231234d3dcdd17f83d299")
    },
    {
      "user_id": ObjectId("566231324d3dcdd17f83d29a")
    },
    {
      "user_id": ObjectId("5662313e4d3dcdd17f83d29b")
    },
    {
      "user_id": ObjectId("5662314e4d3dcdd17f83d29c")
    },
    {
      "user_id": ObjectId("566231594d3dcdd17f83d29d")
    },
    {
      "user_id": ObjectId("566231644d3dcdd17f83d29e")
    }
  ],
  "tags": [
    "futebol",
    "atletico",
    "galo",
    "tagdiferente1"
  ],
  "goals": [
    {
      "name": "goal inicial",
      "description": "goal do primeiro projeto",
      "created_time": 1449527584078,
      "tags": [
        "brasil",
        "america do sul",
        "galodoido",
        "vaipracimadelesgalo"
      ],
      "activities": [
        {
          "name": "Ativiade 1",
          "description": "description atividade",
          "created_time": 1449527584078,
          "comments": [
            {
              "text": "Comentário de teste 1",
              "created_time": 1449527584078,
              "members": [
                {
                  "user_id": ObjectId("566231164d3dcdd17f83d298")
                },
                {
                  "user_id": ObjectId("566231234d3dcdd17f83d299")
                },
                {
                  "user_id": ObjectId("566231324d3dcdd17f83d29a")
                }
              ],
              "file": [ ]
            }
          ],
          "historics": [ ]
        },
        {
          "name": "Ativiade 2",
          "description": "description atividade 2",
          "created_time": 1449527584078,
          "comments": [
            {
              "text": "Comentário de teste post 2",
              "created_time": 1449527584078,
              "members": [
                {
                  "user_id": ObjectId("566231164d3dcdd17f83d298")
                },
                {
                  "user_id": ObjectId("566231234d3dcdd17f83d299")
                },
                {
                  "user_id": ObjectId("566231324d3dcdd17f83d29a")
                }
              ],
              "file": [ ]
            }
          ],
          "historics": [ ]
        }
      ]
    }
  ],
  "views": 0
}
Fetched 1 record(s) in 3ms




fabio-Inspiron-7520(mongod-3.0.7) be-mean-projeto> var query = {_id: ObjectId("5666092f661f1e8ea0aec017")}
fabio-Inspiron-7520(mongod-3.0.7) be-mean-projeto> var usuarios = [{"user_id": ObjectId("566231164d3dcdd17f83d298")}, {"user_id": ObjectId("566231234d3dcdd17f83d299")}]
fabio-Inspiron-7520(mongod-3.0.7) be-mean-projeto> var mod = {$pushAll: {member: usuarios}}
fabio-Inspiron-7520(mongod-3.0.7) be-mean-projeto> db.projects.update(query, mod)
Updated 1 existing record(s) in 2ms
WriteResult({
  "nMatched": 1,
  "nUpserted": 0,
  "nModified": 1
})
fabio-Inspiron-7520(mongod-3.0.7) be-mean-projeto> var query = {_id: ObjectId("5666093e661f1e8ea0aec018")}
fabio-Inspiron-7520(mongod-3.0.7) be-mean-projeto> var usuarios = [{"user_id": ObjectId("566231794d3dcdd17f83d2a0")}, {"user_id": ObjectId("566231644d3dcdd17f83d29e")}]
fabio-Inspiron-7520(mongod-3.0.7) be-mean-projeto> var mod = {$pushAll: {member: usuarios}}
fabio-Inspiron-7520(mongod-3.0.7) be-mean-projeto> db.projects.update(query, mod)
Updated 1 existing record(s) in 1ms
WriteResult({
  "nMatched": 1,
  "nUpserted": 0,
  "nModified": 1
})
fabio-Inspiron-7520(mongod-3.0.7) be-mean-projeto> var query = {_id: ObjectId("56660958661f1e8ea0aec01a")}
fabio-Inspiron-7520(mongod-3.0.7) be-mean-projeto> var usuarios = [{"user_id": ObjectId("566231234d3dcdd17f83d299")}, {"user_id": ObjectId("566231164d3dcdd17f83d298")}]
fabio-Inspiron-7520(mongod-3.0.7) be-mean-projeto> var mod = {$pushAll: {member: usuarios}}
fabio-Inspiron-7520(mongod-3.0.7) be-mean-projeto> db.projects.update(query, mod)
Updated 1 existing record(s) in 2ms
WriteResult({
  "nMatched": 1,
  "nUpserted": 0,
  "nModified": 1
})
fabio-Inspiron-7520(mongod-3.0.7) be-mean-projeto> var query = {_id: ObjectId("5666094e661f1e8ea0aec019")}
fabio-Inspiron-7520(mongod-3.0.7) be-mean-projeto> var usuarios = [{"user_id": ObjectId("566231164d3dcdd17f83d298")}, {"user_id": ObjectId("566231844d3dcdd17f83d2a1")}]
fabio-Inspiron-7520(mongod-3.0.7) be-mean-projeto> var mod = {$pushAll: {member: usuarios}}
fabio-Inspiron-7520(mongod-3.0.7) be-mean-projeto> db.projects.update(query, mod)
Updated 1 existing record(s) in 2ms
WriteResult({
  "nMatched": 1,
  "nUpserted": 0,
  "nModified": 1
})


```
### 4 - Adicione 1 comentário em cada atividade, deixe apenas 1 projeto sem.

```

fabio-Inspiron-7520(mongod-3.0.7) be-mean-projeto> var query = {"_id": ObjectId("56660924661f1e8ea0aec016") }
fabio-Inspiron-7520(mongod-3.0.7) be-mean-projeto> var mod = { $push: 
...   {"goals.0.activities.0.comments": 
...     {
...       "text": "Comentario do projeto 1 inserido depois",
...       "created_time": Date.now(),
...       "members": [{"user_id": ObjectId("5662313e4d3dcdd17f83d29b")}]
...     }
...   }
... }
fabio-Inspiron-7520(mongod-3.0.7) be-mean-projeto> 
fabio-Inspiron-7520(mongod-3.0.7) be-mean-projeto> var options = {upsert: true}
fabio-Inspiron-7520(mongod-3.0.7) be-mean-projeto> db.projects.update(query, mod, options)
Updated 1 existing record(s) in 2ms
WriteResult({
  "nMatched": 1,
  "nUpserted": 0,
  "nModified": 1
})
fabio-Inspiron-7520(mongod-3.0.7) be-mean-projeto> db.projects.find(query)
{
  "_id": ObjectId("56660924661f1e8ea0aec016"),
  "name": "Projeto 1",
  "description": "projeto Instagram Mongo DB",
  "created_time": 1449527584078,
  "visible": 1,
  "member": [
    {
      "user_id": ObjectId("566231164d3dcdd17f83d298")
    },
    {
      "user_id": ObjectId("566231234d3dcdd17f83d299")
    },
    {
      "user_id": ObjectId("566231324d3dcdd17f83d29a")
    },
    {
      "user_id": ObjectId("5662313e4d3dcdd17f83d29b")
    },
    {
      "user_id": ObjectId("5662314e4d3dcdd17f83d29c")
    },
    {
      "user_id": ObjectId("566231594d3dcdd17f83d29d")
    },
    {
      "user_id": ObjectId("566231644d3dcdd17f83d29e")
    }
  ],
  "tags": [
    "futebol",
    "atletico",
    "galo",
    "tagdiferente1"
  ],
  "goals": [
    {
      "name": "goal inicial",
      "description": "goal do primeiro projeto",
      "created_time": 1449527584078,
      "tags": [
        "brasil",
        "america do sul",
        "galodoido",
        "vaipracimadelesgalo"
      ],
      "activities": [
        {
          "name": "Ativiade 1",
          "description": "description atividade",
          "created_time": 1449527584078,
          "comments": [
            {
              "text": "Comentário de teste 1",
              "created_time": 1449527584078,
              "members": [
                {
                  "user_id": ObjectId("566231164d3dcdd17f83d298")
                },
                {
                  "user_id": ObjectId("566231234d3dcdd17f83d299")
                },
                {
                  "user_id": ObjectId("566231324d3dcdd17f83d29a")
                }
              ],
              "file": [ ]
            },
            {
              "text": "Comentario do projeto 1 inserido depois",
              "created_time": 1450049952760,
              "members": [
                {
                  "user_id": ObjectId("5662313e4d3dcdd17f83d29b")
                }
              ]
            }
          ],
          "historics": [ ]
        },
        {
          "name": "Ativiade 2",
          "description": "description atividade 2",
          "created_time": 1449527584078,
          "comments": [
            {
              "text": "Comentário de teste post 2",
              "created_time": 1449527584078,
              "members": [
                {
                  "user_id": ObjectId("566231164d3dcdd17f83d298")
                },
                {
                  "user_id": ObjectId("566231234d3dcdd17f83d299")
                },
                {
                  "user_id": ObjectId("566231324d3dcdd17f83d29a")
                }
              ],
              "file": [ ]
            }
          ],
          "historics": [ ]
        }
      ]
    }
  ],
  "views": 0
}



fabio-Inspiron-7520(mongod-3.0.7) be-mean-projeto> var mod = { $push: 
...   {"goals.0.activities.1.comments": 
...     {
...       "text": "Comentario do projeto 1 inserido depois na atividade 2",
...       "created_time": Date.now(),
...       "members": [{"user_id": ObjectId("5662313e4d3dcdd17f83d29b")}]
...     }
...   }
... }
fabio-Inspiron-7520(mongod-3.0.7) be-mean-projeto> 
fabio-Inspiron-7520(mongod-3.0.7) be-mean-projeto> var options = {upsert: true}
fabio-Inspiron-7520(mongod-3.0.7) be-mean-projeto> db.projects.update(query, mod, options)
Updated 1 existing record(s) in 1ms
WriteResult({
  "nMatched": 1,
  "nUpserted": 0,
  "nModified": 1
})
fabio-Inspiron-7520(mongod-3.0.7) be-mean-projeto> db.projects.find(query)
{
  "_id": ObjectId("56660924661f1e8ea0aec016"),
  "name": "Projeto 1",
  "description": "projeto Instagram Mongo DB",
  "created_time": 1449527584078,
  "visible": 1,
  "member": [
    {
      "user_id": ObjectId("566231164d3dcdd17f83d298")
    },
    {
      "user_id": ObjectId("566231234d3dcdd17f83d299")
    },
    {
      "user_id": ObjectId("566231324d3dcdd17f83d29a")
    },
    {
      "user_id": ObjectId("5662313e4d3dcdd17f83d29b")
    },
    {
      "user_id": ObjectId("5662314e4d3dcdd17f83d29c")
    },
    {
      "user_id": ObjectId("566231594d3dcdd17f83d29d")
    },
    {
      "user_id": ObjectId("566231644d3dcdd17f83d29e")
    }
  ],
  "tags": [
    "futebol",
    "atletico",
    "galo",
    "tagdiferente1"
  ],
  "goals": [
    {
      "name": "goal inicial",
      "description": "goal do primeiro projeto",
      "created_time": 1449527584078,
      "tags": [
        "brasil",
        "america do sul",
        "galodoido",
        "vaipracimadelesgalo"
      ],
      "activities": [
        {
          "name": "Ativiade 1",
          "description": "description atividade",
          "created_time": 1449527584078,
          "comments": [
            {
              "text": "Comentário de teste 1",
              "created_time": 1449527584078,
              "members": [
                {
                  "user_id": ObjectId("566231164d3dcdd17f83d298")
                },
                {
                  "user_id": ObjectId("566231234d3dcdd17f83d299")
                },
                {
                  "user_id": ObjectId("566231324d3dcdd17f83d29a")
                }
              ],
              "file": [ ]
            },
            {
              "text": "Comentario do projeto 1 inserido depois",
              "created_time": 1450049952760,
              "members": [
                {
                  "user_id": ObjectId("5662313e4d3dcdd17f83d29b")
                }
              ]
            }
          ],
          "historics": [ ]
        },
        {
          "name": "Ativiade 2",
          "description": "description atividade 2",
          "created_time": 1449527584078,
          "comments": [
            {
              "text": "Comentário de teste post 2",
              "created_time": 1449527584078,
              "members": [
                {
                  "user_id": ObjectId("566231164d3dcdd17f83d298")
                },
                {
                  "user_id": ObjectId("566231234d3dcdd17f83d299")
                },
                {
                  "user_id": ObjectId("566231324d3dcdd17f83d29a")
                }
              ],
              "file": [ ]
            },
            {
              "text": "Comentario do projeto 1 inserido depois na atividade 2",
              "created_time": 1450050052140,
              "members": [
                {
                  "user_id": ObjectId("5662313e4d3dcdd17f83d29b")
                }
              ]
            }
          ],
          "historics": [ ]
        }
      ]
    }
  ],
  "views": 0
}
Fetched 1 record(s) in 3ms


fabio-Inspiron-7520(mongod-3.0.7) be-mean-projeto> var query = {"_id": ObjectId("5666092f661f1e8ea0aec017")}
fabio-Inspiron-7520(mongod-3.0.7) be-mean-projeto> 
fabio-Inspiron-7520(mongod-3.0.7) be-mean-projeto> var mod = { $push: 
...   {"goals.0.activities.0.comments": 
...     {
...       "text": "Comentario do projeto 2 inserido depois",
...       "created_time": Date.now(),
...       "members": [{"user_id": ObjectId("5662313e4d3dcdd17f83d29b")}]
...     }
...   }
... }
fabio-Inspiron-7520(mongod-3.0.7) be-mean-projeto> 
fabio-Inspiron-7520(mongod-3.0.7) be-mean-projeto> var options = {upsert: true}
fabio-Inspiron-7520(mongod-3.0.7) be-mean-projeto> db.projects.update(query, mod, options)
Updated 1 existing record(s) in 1ms
WriteResult({
  "nMatched": 1,
  "nUpserted": 0,
  "nModified": 1
})


fabio-Inspiron-7520(mongod-3.0.7) be-mean-projeto> var mod = { $push: 
...   {"goals.0.activities.1.comments": 
...     {
...       "text": "Comentario do projeto 2 inserido depois na atividade 2",
...       "created_time": Date.now(),
...       "members": [{"user_id": ObjectId("5662313e4d3dcdd17f83d29b")}]
...     }
...   }
... }
fabio-Inspiron-7520(mongod-3.0.7) be-mean-projeto> var options = {upsert: true}
fabio-Inspiron-7520(mongod-3.0.7) be-mean-projeto> db.projects.update(query, mod, options)
Updated 1 existing record(s) in 2ms
WriteResult({
  "nMatched": 1,
  "nUpserted": 0,
  "nModified": 1
})


fabio-Inspiron-7520(mongod-3.0.7) be-mean-projeto> var query = {"_id": ObjectId("5666093e661f1e8ea0aec018")}
fabio-Inspiron-7520(mongod-3.0.7) be-mean-projeto> var mod = { $push: 
...   {"goals.0.activities.0.comments": 
...     {
...       "text": "Comentario do projeto 3 inserido depois na atividade 1",
...       "created_time": Date.now(),
...       "members": [{"user_id": ObjectId("566231324d3dcdd17f83d29a")}]
...     }
...   }
... }
fabio-Inspiron-7520(mongod-3.0.7) be-mean-projeto> var options = {upsert: true}
fabio-Inspiron-7520(mongod-3.0.7) be-mean-projeto> db.projects.update(query, mod, options)
Updated 1 existing record(s) in 1ms
WriteResult({
  "nMatched": 1,
  "nUpserted": 0,
  "nModified": 1
})
fabio-Inspiron-7520(mongod-3.0.7) be-mean-projeto> var mod = { $push: 
...   {"goals.0.activities.1.comments": 
...     {
...       "text": "Comentario do projeto 3 inserido depois na atividade 2",
...       "created_time": Date.now(),
...       "members": [{"user_id": ObjectId("566231324d3dcdd17f83d29a")}]
...     }
...   }
... }
fabio-Inspiron-7520(mongod-3.0.7) be-mean-projeto> var options = {upsert: true}
fabio-Inspiron-7520(mongod-3.0.7) be-mean-projeto> db.projects.update(query, mod, options)
Updated 1 existing record(s) in 1ms
WriteResult({
  "nMatched": 1,
  "nUpserted": 0,
  "nModified": 1
})



fabio-Inspiron-7520(mongod-3.0.7) be-mean-projeto> var query = {"_id": ObjectId("5666094e661f1e8ea0aec019")}
fabio-Inspiron-7520(mongod-3.0.7) be-mean-projeto> var mod = { $push: 
...   {"goals.0.activities.0.comments": 
...     {
...       "text": "Comentario do projeto 4 inserido depois na atividade 1",
...       "created_time": Date.now(),
...       "members": [{"user_id": ObjectId("566231324d3dcdd17f83d29a")}]
...     }
...   }
... }
fabio-Inspiron-7520(mongod-3.0.7) be-mean-projeto> var options = {upsert: true}
fabio-Inspiron-7520(mongod-3.0.7) be-mean-projeto> db.projects.update(query, mod, options)
Updated 1 existing record(s) in 1ms
WriteResult({
  "nMatched": 1,
  "nUpserted": 0,
  "nModified": 1
})
fabio-Inspiron-7520(mongod-3.0.7) be-mean-projeto> var query = {"_id": ObjectId("5666094e661f1e8ea0aec019")}
fabio-Inspiron-7520(mongod-3.0.7) be-mean-projeto> var mod = { $push: 
...   {"goals.0.activities.1.comments": 
...     {
...       "text": "Comentario do projeto 4 inserido depois na atividade 2",
...       "created_time": Date.now(),
...       "members": [{"user_id": ObjectId("566231324d3dcdd17f83d29a")}]
...     }
...   }
... }
fabio-Inspiron-7520(mongod-3.0.7) be-mean-projeto> var options = {upsert: true}
fabio-Inspiron-7520(mongod-3.0.7) be-mean-projeto> db.projects.update(query, mod, options)
Updated 1 existing record(s) in 2ms
WriteResult({
  "nMatched": 1,
  "nUpserted": 0,
  "nModified": 1
})


```
### 5 - Adicione 1 projeto inteiro com UPSERT.

```

 var query = {name: /Projeto UPSERT/i}
fabio-Inspiron-7520(mongod-3.0.7) be-mean-projeto> var mod = {
...   $set: {visible: 1},
...   $setOnInsert: {
...   "name": "Projeto UPSERT",
...   "description": "Nova inserção usando UPSERT",
...   "created_time": Date.now(),
...   "member": [],
...   "tags": [],
...   "goals": [],
...   "views": 0
...   }
... }
fabio-Inspiron-7520(mongod-3.0.7) be-mean-projeto> var options = {upsert: true}
fabio-Inspiron-7520(mongod-3.0.7) be-mean-projeto> db.projects.update(query, mod, options)
Updated 1 new record(s) in 2ms
WriteResult({
  "nMatched": 0,
  "nUpserted": 1,
  "nModified": 0,
  "_id": ObjectId("566e060314e28f1e78c38346")
})

fabio-Inspiron-7520(mongod-3.0.7) be-mean-projeto> var query = {"_id": ObjectId("566e060314e28f1e78c38346")}
fabio-Inspiron-7520(mongod-3.0.7) be-mean-projeto> db.projects.find(query)
{
  "_id": ObjectId("566e060314e28f1e78c38346"),
  "visible": 1,
  "name": "Projeto UPSERT",
  "description": "Nova inserção usando UPSERT",
  "created_time": 1450051073817,
  "member": [ ],
  "tags": [ ],
  "goals": [ ],
  "views": 0
}


```

## Delete - remoção

```

### 1 - Apague todos os projetos que não possuam tags.

fabio-Inspiron-7520(mongod-3.0.7) be-mean-projeto> var query = {'tags.0': {$exists: false}}
fabio-Inspiron-7520(mongod-3.0.7) be-mean-projeto> db.projects.find(query)
{
  "_id": ObjectId("566e060314e28f1e78c38346"),
  "visible": 1,
  "name": "Projeto UPSERT",
  "description": "Nova inserção usando UPSERT",
  "created_time": 1450051073817,
  "member": [ ],
  "tags": [ ],
  "goals": [ ],
  "views": 0
}
Fetched 1 record(s) in 1ms
fabio-Inspiron-7520(mongod-3.0.7) be-mean-projeto> db.projects.remove(query)
Removed 1 record(s) in 1ms
WriteResult({
  "nRemoved": 1
})

```


### 2 - Apague todos os projetos que não possuam comentários nas atividades.

```

fabio-Inspiron-7520(mongod-3.0.7) be-mean-projeto> var query = {'goals.activities.comments.0': {$exists: false}}
fabio-Inspiron-7520(mongod-3.0.7) be-mean-projeto> var fields = {goals: 1, name: 1}
fabio-Inspiron-7520(mongod-3.0.7) be-mean-projeto> db.projects.find(query, fields)
{
  "_id": ObjectId("56660958661f1e8ea0aec01a"),
  "name": "Projeto 5",
  "goals": [
    {
      "name": "goal 5",
      "description": "goal 5 do projeto",
      "created_time": 1449527637133,
      "tags": [
        "goal7",
        "goal8",
        "goal9"
      ]
    }
  ]
}
Fetched 1 record(s) in 1ms
fabio-Inspiron-7520(mongod-3.0.7) be-mean-projeto> db.projects.remove(query)
Removed 1 record(s) in 1ms
WriteResult({
  "nRemoved": 1
})

```


### 3 - Apague todos os projetos que não possuam atividades.

```

fabio-Inspiron-7520(mongod-3.0.7) be-mean-projeto> var query = {'goals.activities': {$exists: false}}
fabio-Inspiron-7520(mongod-3.0.7) be-mean-projeto> db.projects.find(query)
{
  "_id": ObjectId("566e10b214e28f1e78c38348"),
  "visible": 1,
  "name": "Projeto NOVO 2",
  "description": "Nova inserção para testes 2",
  "created_time": 1450053808646,
  "member": [
    {
      "user_id": ObjectId("566231844d3dcdd17f83d2a1")
    },
    {
      "user_id": ObjectId("566e0f7ffdfa76611e9da1f5")
    }
  ],
  "tags": [ ],
  "goals": [ ],
  "views": 0
}
{
  "_id": ObjectId("566e111914e28f1e78c38349"),
  "visible": 1,
  "name": "Projeto NOVO 2",
  "description": "Nova inserção para testes 3",
  "created_time": 1450053912556,
  "member": [
    {
      "user_id": ObjectId("566231844d3dcdd17f83d2a1")
    },
    {
      "user_id": ObjectId("566231794d3dcdd17f83d2a0")
    }
  ],
  "tags": [
    "tag1"
  ],
  "goals": [ ],
  "views": 0
}
Fetched 2 record(s) in 2ms
fabio-Inspiron-7520(mongod-3.0.7) be-mean-projeto> db.projects.remove(query)
Removed 2 record(s) in 1ms
WriteResult({
  "nRemoved": 2
})


```

### 4 - Escolha 2 usuários e apague todos os projetos em que os 2 fazem parte.

```

fabio-Inspiron-7520(mongod-3.0.7) be-mean-projeto> var usuarios = [{"user_id": ObjectId("566e0f69fdfa76611e9da1f4")},{"user_id": ObjectId("566e0f7ffdfa76611e9da1f5")}]
fabio-Inspiron-7520(mongod-3.0.7) be-mean-projeto> var fields = {member:1, name: 1}
fabio-Inspiron-7520(mongod-3.0.7) be-mean-projeto> var query = {member: {$all: usuarios}}
fabio-Inspiron-7520(mongod-3.0.7) be-mean-projeto> db.projects.find(query, fields)
{
  "_id": ObjectId("566e108c14e28f1e78c38347"),
  "name": "Projeto NOVO",
  "member": [
    {
      "user_id": ObjectId("566e0f69fdfa76611e9da1f4")
    },
    {
      "user_id": ObjectId("566e0f7ffdfa76611e9da1f5")
    }
  ]
}
Fetched 1 record(s) in 1ms
fabio-Inspiron-7520(mongod-3.0.7) be-mean-projeto> db.projects.remove(query)
Removed 1 record(s) in 1ms
WriteResult({
  "nRemoved": 1
})


```

### 5 - Apague todos os projetos que possuam uma determinada tag em goal.

```

 var query = {"goals.tags": {$in: ["goal4"]}}
fabio-Inspiron-7520(mongod-3.0.7) be-mean-projeto> db.projects.find(query)
{
  "_id": ObjectId("5666094e661f1e8ea0aec019"),
  "name": "Projeto 4",
  "description": "projeto Instagram Mongo DB 4",
  "created_time": 1449527623979,
  "visible": 1,
  "member": [
    {
      "user_id": ObjectId("5662313e4d3dcdd17f83d29b")
    },
    {
      "user_id": ObjectId("566231594d3dcdd17f83d29d")
    },
    {
      "user_id": ObjectId("566231644d3dcdd17f83d29e")
    },
    {
      "user_id": ObjectId("566231704d3dcdd17f83d29f")
    },
    {
      "user_id": ObjectId("566231794d3dcdd17f83d2a0")
    },
    {
      "user_id": ObjectId("566231164d3dcdd17f83d298")
    },
    {
      "user_id": ObjectId("566231844d3dcdd17f83d2a1")
    }
  ],
  "tags": [
    "tag4",
    "tag5",
    "tag6",
    "tagdiferente5"
  ],
  "goals": [
    {
      "name": "goal 4",
      "description": "goal 4 do projeto",
      "created_time": 1449527623979,
      "tags": [
        "goal4",
        "goal5",
        "goal6"
      ],
      "activities": [
        {
          "name": "Ativiade 1 projeto 4",
          "description": "description atividade 1 projeto 4",
          "created_time": 1449527623979,
          "comments": [
            {
              "text": "Comentário de teste 1 projeto 4",
              "created_time": 1449527623979,
              "members": [
                {
                  "user_id": ObjectId("5662313e4d3dcdd17f83d29b")
                },
                {
                  "user_id": ObjectId("566231594d3dcdd17f83d29d")
                },
                {
                  "user_id": ObjectId("566231644d3dcdd17f83d29e")
                }
              ],
              "file": [ ]
            },
            {
              "text": "Comentario do projeto 4 inserido depois na atividade 1",
              "created_time": 1450050497413,
              "members": [
                {
                  "user_id": ObjectId("566231324d3dcdd17f83d29a")
                }
              ]
            }
          ],
          "historics": [ ]
        },
        {
          "name": "Ativiade 2 do projeto 3",
          "description": "description atividade 3",
          "created_time": 1449527623979,
          "comments": [
            {
              "text": "Comentário de teste post 3",
              "created_time": 1449527623979,
              "members": [
                {
                  "user_id": ObjectId("5662313e4d3dcdd17f83d29b")
                },
                {
                  "user_id": ObjectId("566231594d3dcdd17f83d29d")
                },
                {
                  "user_id": ObjectId("566231644d3dcdd17f83d29e")
                }
              ],
              "file": [ ]
            },
            {
              "text": "Comentario do projeto 4 inserido depois na atividade 2",
              "created_time": 1450050518865,
              "members": [
                {
                  "user_id": ObjectId("566231324d3dcdd17f83d29a")
                }
              ]
            }
          ],
          "historics": [ ]
        }
      ]
    }
  ],
  "views": 0
}
Fetched 1 record(s) in 4ms
fabio-Inspiron-7520(mongod-3.0.7) be-mean-projeto> db.projects.remove(query)
Removed 1 record(s) in 1ms
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
fabio-Inspiron-7520(mongod-3.0.7) be-mean-projeto> db.createUser(
...   {
...     user: "projectuser2",
...     pwd: "user123",
...     roles: [ { role: "readWrite", db: "admin"} ]
...   }
... )
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

fabio-Inspiron-7520(mongod-3.0.7) be-mean-projeto> db.runCommand( { grantRolesToUser: "projectuser2",
...   roles: [
...     { role: "userAdmin", db: "be-mean-projeto"}
...   ],
...   writeConcern: {}
...  })
{
  "ok": 1
}

```

### 4 - Remover o papel grantRolesToUser para o usuário com Escrita e Leitura.

```
 be-mean-projeto> db.runCommand( { revokeRolesFromUser: "projectuser2",
...   roles: [
...     { role: "userAdmin", db: "be-mean-projeto"}
...   ],
...   writeConcern: {}
... })
{
  "ok": 1
}


```

### 5 - Listar todos os usuários com seus papéis e ações.

```

fabio-Inspiron-7520(mongod-3.0.7) admin> db.runCommand( { usersInfo: [ { user: "projectuser", db: "be-mean-projeto" }, { user: "projectuser2", db: "be-mean-projeto" } ],
...   showCredentials: true,
...   showPrivileges: true
... } )
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


## Sharding

```

fabio@fabio-Inspiron-7520:/data$ mkdir configdb

fabio@fabio-Inspiron-7520:/data$ mongod --configsvr --port 27010

fabio@fabio-Inspiron-7520:/data$ mongos --configdb localhost:27010 --port 27011

fabio@fabio-Inspiron-7520:/data$ mkdir /data/shard1 && mkdir /data/shard2 && mkdir /data/shard3

fabio@fabio-Inspiron-7520:/data$ mongod --port 27013 --dbpath /data/shard2

fabio@fabio-Inspiron-7520:/data$ mongod --port 27014 --dbpath /data/shard3

fabio@fabio-Inspiron-7520:/data$ mongo --port 27011 --host localhost
MongoDB shell version: 3.0.7
connecting to: localhost:27011/test
Mongo-Hacker 0.0.8
mongos> use be-mean-projeto
switched to db be-mean-projeto

mongos> sh.addShard("localhost:27012")
{
  "shardAdded": "shard0000",
  "ok": 1
}
fabio-Inspiron-7520:27011(mongos-3.0.7)[mongos] be-mean-projeto> sh.addShard("localhost:27013")
{
  "shardAdded": "shard0001",
  "ok": 1
}
fabio-Inspiron-7520:27011(mongos-3.0.7)[mongos] be-mean-projeto> sh.addShard("localhost:27014")
{
  "shardAdded": "shard0002",
  "ok": 1
}

fabio-Inspiron-7520:27011(mongos-3.0.7)[mongos] be-mean-projeto> sh.enableSharding("be-mean-projeto")
{
  "ok": 1
}

fabio-Inspiron-7520:27011(mongos-3.0.7)[mongos] be-mean-projeto> sh.shardCollection("be-mean-projeto.projects", {"_id": 1})
{
  "collectionsharded": "be-mean-projeto.projects",
  "ok": 1
}

```


## Replica

```

fabio@fabio-Inspiron-7520:/data$ mkdir /data/rs1

fabio@fabio-Inspiron-7520:/data$ mkdir /data/rs2

fabio@fabio-Inspiron-7520:/data$ mkdir /data/rs3

mongod --replSet replica_set --port 27117 --dbpath /data/rs1
mongod --replSet replica_set --port 27118 --dbpath /data/rs2
mongod --replSet replica_set --port 27119 --dbpath /data/rs3

fabio@fabio-Inspiron-7520:~$ mongo --port 27117

fabio-Inspiron-7520:27117(mongod-3.0.7) test> rsconf = {
...    _id: "replica_set",
...    members: [
...     {
...      _id: 0,
...      host: "127.0.0.1:27117"
...     }
...   ]
... }
{
  "_id": "replica_set",
  "members": [
    {
      "_id": 0,
      "host": "127.0.0.1:27117"
    }
  ]
}
fabio-Inspiron-7520:27117(mongod-3.0.7) test> rs.initiate(rsconf)
{
  "ok": 1
}

fabio-Inspiron-7520:27117(mongod-3.0.7)[PRIMARY:replica_set] test> rs.add("127.0.0.1:27118")
{
  "ok": 1
}

fabio-Inspiron-7520:27117(mongod-3.0.7)[PRIMARY:replica_set] test> rs.add("127.0.0.1:27119")
{
  "ok": 1
}

fabio-Inspiron-7520:27117(mongod-3.0.7)[PRIMARY:replica_set] test> rs.status()
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