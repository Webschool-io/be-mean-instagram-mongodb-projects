# MongoDb - Projeto Final  
**Autor:** Ednilson Amaral  
**Data:** 1450493323653  


## Para qual sistema você usaria o MongoDB (diferente desse)?  
Estou começando a me aventurar agora com NoSQL, conhecendo tudo agora, MongoDB, etc. E, após as aulas do Suissa, pude perceber, ainda com um pouco de mentalidade relacional, que banco de dados não relacionais podem ser utilizados para qualquer sistema. Especificamente para aplicações real-time, como plataformas do Twitter, Facebook, Instagram. De uma forma resumida, acredito que qualquer aplicação de real-time pode ser utilizado MongoDB, tanto pela sua praticidade, como suas inúmeras vantagens quando falamos de performance, etc.


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

A coleção retirada de `projects` foi `activities`. Sua modelagem ficou da seguinte maneira:  

```  
activities{  
  name_project: name_project,  
  goal_name: goal_name,  
  name: String,  
  description: String,  
  date_begin: Date,  
  date_dream: Date,  
  date_end: Date,  
  realocate: boolean,  
  expired: boolean  
}  
```


## Create - cadastro  

### Usuários  

Primeiramente, fiz um único cadastro para teste:  

```  
be-mean-mongodb> var json = {  
  "name": "Ednilson Amaral",  
  "bio": "Desenvolvedor Frontend",  
  "email": "ednilsonamaral.ti@gmail.com",  
  "auth": [  
    {  
      "username": "ednilson",  
      "password": "123456",  
      "last_access": Date.now(),  
      "online": false,   
      "disable": false,   
      "hash_token": null  
    }  
  ],  
  "date_register": Date.now(),  
  "avatar_path": "oi"  
}

EDNILSON-NB(mongod-3.2.0) be-mean-mongodb> json  
{  
  "name": "Ednilson Amaral",  
  "bio": "Desenvolvedor Frontend",  
  "email": "ednilsonamaral.ti@gmail.com",  
  "auth": [  
    {  
      "username": "ednilson",  
      "password": "123456",  
      "last_access": 1450486386448,  
      "online": false,  
      "disable": false,  
      "hash_token": null  
    }  
  ],  
  "date_register": 1450486386448,  
  "avatar_path": "oi"  
}  

be-mean-mongodb> db.users.save(json)  
Inserted 1 record(s) in 145ms  
WriteResult({  
  "nInserted": 1  
})  

be-mean-mongodb> db.users.find()  
{  
  "_id": ObjectId("5674aade9fda77e81be555da"),  
  "name": "Ednilson Amaral",  
  "bio": "Desenvolvedor Frontend",  
  "email": "ednilsonamaral.ti@gmail.com",  
  "auth": [  
    {  
      "username": "ednilson",  
      "password": "123456",  
      "last_access": 1450486386448,  
      "online": false,  
      "disable": false,  
      "hash_token": null  
    }  
  ],  
  "date_register": 1450486386448,  
  "avatar_path": "oi"  
}  
Fetched 1 record(s) in 3ms  
```  

Após essa inserção de teste, inserir os 9 usuários restantes através de um `mongoimport`:  

```  
ednilson@EDNILSON-NB:/var/www/be-mean-instagram-mongodb$ mongoimport --db be-mean-mongodb --collection users --file users.json  
2015-12-18T22:56:51.030-0200  connected to: localhost  
2015-12-18T22:56:51.222-0200  imported 9 documents  
```  

Eu não utilizei o parâmetro `--drop` junto do `mongoimport` devido que o registro que já tinha na coleção `users` também será utilizado posteriormente.


### Projetos  

Resolvi utilizar o modo manual para fazer o relacionamento necessário no `array` de `comments` e também de `members`. Então, criei uma variável utilizando Regex para buscar um nome de usuário.  

```  
be-mean-mongodb> var nome = {"name": /jean/i}  
be-mean-mongodb> nome  
{  
  "name": /jean/i  
}  

be-mean-mongodb> db.users.find(nome)  
{  
  "_id": ObjectId("5674abfcc1519c3ff37103f3"),  
  "name": "Jean Carlo",  
  "bio": "Carinha mais ou menos fodinha",  
  "email": "admin@admin.com",  
  "auth": [  
    {  
      "username": "suissa",  
      "password": "123456",  
      "last_access": null,  
      "online": false,  
      "disable": false,  
      "hash_token": null  
    }  
  ],  
  "date_register": null,  
  "avatar_path": null  
}  
Fetched 1 record(s) in 3ms  

be-mean-mongodb> var comment_name = "Jean Carlo"  
be-mean-mongodb> comment_name  
Jean Carlo  

be-mean-mongodb> var nome = {"name": /ednilson/i}  
be-mean-mongodb> nome  
{  
  "name": /ednilson/i  
}  

be-mean-mongodb> db.users.find(nome)  
{  
  "_id": ObjectId("5674aade9fda77e81be555da"),  
  "name": "Ednilson Amaral",  
  "bio": "Desenvolvedor Frontend",  
  "email": "ednilsonamaral.ti@gmail.com",  
  "auth": [  
    {  
      "username": "ednilson",  
      "password": "123456",  
      "last_access": 1450486386448,  
      "online": false,  
      "disable": false,  
      "hash_token": null  
    }  
  ],  
  "date_register": 1450486386448,  
  "avatar_path": "oi"  
}  
Fetched 1 record(s) in 4ms  

be-mean-mongodb> var membro_one = "Ednilson Amaral"  
be-mean-mongodb> membro_one  
Ednilson Amaral  
```  

Repeti esses passos tanto para os comentários como para membros. No caso da variável para membros, eu criei:  

* `membro_one`;  
* `membro_two`;  
* `membro_three`;  
* `membro_four`;  
* `membro_five`;  

Criei essas variáveis seguindo o mesmo processo, buscando um usuário utilizando Regex e então armazenando o nome dele na variável desejada. Em seguida, criei uma variável `projeto_one` para inserir os projetos, um a um.  

```  
var projeto = {  
  "name": "Projeto 1",  
  "description": "Lorem ipsum dolor sit amet, consectetur adipisicing elit",  
  "date_begin": 1450490066921,  
  "date_dream": null,  
  "date_end": null,  
  "visible": true,  
  "realocate": false,  
  "expired": false,  
  "visualizable_mod": null,  

  "tags": ["vaca", "meia lua", "gremio"],  

  "members": [  
    {  
      "name": "Ednilson Amaral",  
      "type_member": "Tipo A",  
      "notify": false  
    },  

    {  
      "name": "José da Silva",  
      "type_member": "Tipo B",  
      "notify": false  
    },  

    {  
      "name": "Carlos Almeida",  
      "type_member": "Tipo X",  
      "notify": false  
    },  

    {  
      "name": "Laís Ramos",  
      "type_member": "Tipo B",  
      "notify": false  
    },  

    {  
      "name": "Jéssica Bueno",  
      "type_member": "Tipo Z",  
      "notify": false  
    }  
    ],  

    "goals": [{  
      "name": "Ronaldinho Gaúcho",  
      "description": "Lorem ipsum dolor sit amet, consectetur adipisicing elit",  
      "date_begin": 1450490066921,  
      "date_dream": null,  
      "date_end": null,  
      "realocate": false,  
      "expired": false,  

      "tags": ["meia lua", "drible da vaca", "tchau"],  

      "comments": [{  
        "user_name": "Jean Carlo",  
        "text": "Lorem ipsum dolor sit amet, consectetur adipisicing elit",  
        "date_comment": 1450490066921  
      }]  
    }]  
}  

be-mean-mongodb> db.projects.save(projeto)
```


## Retrieve - busca  

### Liste as informações dos membros de 1 projeto específico que deve ser buscado pelo seu nome de forma a não ligar para maiúsculas e minúsculas.  

```  
be-mean-mongodb> var busca = {"name": /projeto 1/i}  
be-mean-mongodb> busca  
{  
  "name": /projeto 1/i  
}  

be-mean-mongodb> db.projects.find(busca)  
{  
  "_id": ObjectId("5674bc17c1519c3ff37103fd"),  
  "name": "Projeto 1",  
  "description": "Lorem ipsum dolor sit amet, consectetur adipisicing elit",  
  "date_begin": 1450490066921,  
  "date_dream": null,  
  "date_end": null,  
  "visible": true,  
  "realocate": false,  
  "expired": false,  
  "visualizable_mod": null,  
  "tags": [  
    "vaca",  
    "craque",  
    "gremio"  
  ],  
  "members": [  
    {  
      "name": "Ednilson Amaral",  
      "type_member": "Tipo A",  
      "notify": false  
    },  
    {  
      "name": "José da Silva",  
      "type_member": "Tipo B",  
      "notify": false  
    },  
    {  
      "name": "Carlos Almeida",  
      "type_member": "Tipo X",  
      "notify": false  
    },  
    {  
      "name": "Laís Ramos",  
      "type_member": "Tipo B",  
      "notify": false  
    },  
    {  
      "name": "Jéssica Bueno",  
      "type_member": "Tipo Z",  
      "notify": false  
    }  
  ],  
  "goals": [  
    {  
      "name": "Ronaldinho Gaúcho",  
      "description": "Lorem ipsum dolor sit amet, consectetur adipisicing elit",  
      "date_begin": 1450490066921,  
      "date_dream": null,  
      "date_end": null,  
      "realocate": false,  
      "expired": false,  
      "tags": [  
        "meia lua",  
        "drible da vaca",  
        "tchau"  
      ],  
      "comments": [  
        {  
          "user_name": "Jean Carlo",  
          "text": "Lorem ipsum dolor sit amet, consectetur adipisicing elit",  
          "date_comment": 1450490066921  
        }  
      ]  
    }  
  ]  
}  
Fetched 1 record(s) in 3ms    
```


### Liste todos os projetos com a tag que você escolheu para os 3 projetos em comum.  

```  
be-mean-mongodb> var query = {tags: {$in: [/vaca/i]}}  
be-mean-mongodb> query  
{  
  "tags": {  
    "$in": [  
      /vaca/i  
    ]  
  }  
}  

db.projects.find(query, {name: 1})  
{  
  "_id": ObjectId("5674bc17c1519c3ff37103fd"),  
  "name": "Projeto 1"  
}  
{  
  "_id": ObjectId("5674bc17c1519c3ff37103fe"),  
  "name": "Projeto 3"  
}  
{  
  "_id": ObjectId("5674bc17c1519c3ff37103ff"),  
  "name": "Projeto 2"  
}  
Fetched 3 record(s) in 5ms  
```


### Liste apenas os nomes de todas as atividades para todos os projetos.  

```  
be-mean-mongodb> db.activities.find({}, {name_project: 1, name: 1})  
{  
  "_id": ObjectId("5674c077c1519c3ff3710403"),  
  "name_project": "Projeto 1",  
  "name": "Nome da Atividade 1"  
}  

{  
  "_id": ObjectId("5674c077c1519c3ff3710404"),  
  "name_project": "Projeto 1",  
  "name": "Nome da Atividade 2"  
}  

{  
  "_id": ObjectId("5674c077c1519c3ff3710405"),  
  "name_project": "Projeto 2",  
  "name": "Nome da Atividade 1"  
}  

{  
  "_id": ObjectId("5674c077c1519c3ff3710406"),  
  "name_project": "Projeto 2",  
  "name": "Nome da Atividade 2"  
}  

{  
  "_id": ObjectId("5674c077c1519c3ff3710407"),  
  "name_project": "Projeto 3",  
  "name": "Nome da Atividade 1"  
}  

{  
  "_id": ObjectId("5674c077c1519c3ff3710408"),  
  "name_project": "Projeto 3",  
  "name": "Nome da Atividade 2"  
}  

{  
  "_id": ObjectId("5674c077c1519c3ff3710409"),  
  "name_project": "Projeto 4",  
  "name": "Nome da Atividade 1"  
}  

{  
  "_id": ObjectId("5674c077c1519c3ff371040a"),  
  "name_project": "Projeto 4",  
  "name": "Nome da Atividade 2"  
}  
Fetched 8 record(s) in 5ms  
```


### Liste todos os projetos que não possuam uma tag.  

```  
be-mean-mongodb> db.projects.find({tags: {$size: 0}})  
Fetched 0 record(s) in 1ms  
```  

Pelo o que li na documentação oficial do MongoDB, na parte que explica o `$size`, ele vai retornar a quantidade de argumentos que estejam dentro de um array. Tipo se coloco `$size: 1` ele vai mostrar os objetos que possuam um argumento no campo especificado. Se eu entendi corretamente, então, `$size: 0` vai retornar os projetos que não possua nenhuma `tag`. Caso eu tenha entendido errado, sorry. :s


### Liste todos os usuários que não fazem parte do primeiro projeto cadastrado.  

```  
be-mean-mongodb> db.projects.find({}, {'members.name': 1}).skip(1)  
{  
  "_id": ObjectId("5674bc17c1519c3ff37103fe"),  
  "members": [  
    {  
      "name": "Ednilson Amaral"  
    },  
    {  
      "name": "José da Silva"  
    },  
    {  
      "name": "Jean Carlo"  
    },  
    {  
      "name": "Admin"  
    },  
    {  
      "name": "Érica dos Santos"  
    }  
  ]  
}  
{  
  "_id": ObjectId("5674bc17c1519c3ff37103ff"),  
  "members": [  
    {  
      "name": "Rodrigo Amaral"  
    },  
    {  
      "name": "Marcos de la Rua"  
    },  
    {  
      "name": "Jean Carlo"  
    },  
    {  
      "name": "Admin"  
    },  
    {  
      "name": "Érica dos Santos"  
    }  
  ]  
}  
{  
  "_id": ObjectId("5674bc17c1519c3ff3710400"),  
  "members": [  
    {  
      "name": "Laís Ramos"  
    },  
    {  
      "name": "Jéssica Bueno"  
    },  
    {  
      "name": "Rodrigo Amaral"  
    },  
    {  
      "name": "Marcos de la Rua"  
    },  
    {  
      "name": "Jean Carlo"  
    }  
  ]  
}  
{  
  "_id": ObjectId("5674bc17c1519c3ff3710401"),  
  "members": [  
    {  
      "name": "Admin"  
    },  
    {  
      "name": "Érica dos Santos"  
    },  
    {  
      "name": "Ednilson Amaral"  
    },  
    {  
      "name": "Carlos Almeida"  
    },  
    {  
      "name": "Laís Ramos"  
    }  
  ]  
}  
Fetched 4 record(s) in 7ms  
```


## Update - alteração  

### Adicione para todos os projetos o campo views: 0.  

```  
be-mean-mongodb> var query = {"name": /projeto 1/i}  
be-mean-mongodb> var mod = {$set: {views: 0}}  
be-mean-mongodb> var opt = {upsert: true}  
be-mean-mongodb> db.projects.update(query, mod, opt)  
Updated 1 existing record(s) in 4ms  
WriteResult({  
  "nMatched": 1,  
  "nUpserted": 0,  
  "nModified": 1  
})  

be-mean-mongodb> var query = {"name": /projeto 2/i}  
be-mean-mongodb> var mod = {$set: {views: 0}}  
be-mean-mongodb> var opt = {upsert: true}  
be-mean-mongodb> db.projects.update(query, mod, opt)  
Updated 1 existing record(s) in 4ms  
WriteResult({  
  "nMatched": 1,  
  "nUpserted": 0,  
  "nModified": 1  
})  

be-mean-mongodb> var query = {"name": /projeto 3/i}  
be-mean-mongodb> var mod = {$set: {views: 0}}  
be-mean-mongodb> var opt = {upsert: true}  
be-mean-mongodb> db.projects.update(query, mod, opt)  
Updated 1 existing record(s) in 4ms  
WriteResult({  
  "nMatched": 1,  
  "nUpserted": 0,  
  "nModified": 1  
})  

be-mean-mongodb> var query = {"name": /projeto 4/i}  
be-mean-mongodb> var mod = {$set: {views: 0}}  
be-mean-mongodb> var opt = {upsert: true}  
be-mean-mongodb> db.projects.update(query, mod, opt)  
Updated 1 existing record(s) in 4ms  
WriteResult({  
  "nMatched": 1,  
  "nUpserted": 0,  
  "nModified": 1  
})  

be-mean-mongodb> var query = {"name": /projeto 5/i}  
be-mean-mongodb> var mod = {$set: {views: 0}}  
be-mean-mongodb> var opt = {upsert: true}  
be-mean-mongodb> db.projects.update(query, mod, opt)  
Updated 1 existing record(s) in 4ms  
WriteResult({  
  "nMatched": 1,  
  "nUpserted": 0,  
  "nModified": 1  
})  
```  


### Adicione 1 tag diferente para cada projeto.  

```  
be-mean-mongodb> for(var i=0; i<=5; i++){  
  var query = {"name": "Projeto " + i};  
 
  var mod = {$push: {"tags": "tag"+i}};  

  db.projects.update(query, mod);  
 
  i++  
}

Updated 0 record(s) in 3ms  
Updated 1 existing record(s) in 3ms  
Updated 1 existing record(s) in 2ms  
4  
```


### Adicione 2 membros diferentes para cada projeto.  

```  
be-mean-mongodb> var query = {"name": "Projeto 1"}  
be-mean-mongodb> var mod = {$push: {"members": [{"name": "Admin", "type_member": "Tipo A", "notify": false}]}}  
be-mean-mongodb> db.projects.update(query, mod)  

Updated 1 existing record(s) in 3ms  
WriteResult({  
  "nMatched": 1,  
  "nUpserted": 0,  
  "nModified": 1  
})  

be-mean-mongodb> var mod = {$push: {"members": [{"name": "Jean Carlo", "type_member": "Tipo A", "notify": false}]}}  
be-mean-mongodb> db.projects.update(query, mod)  

Updated 1 existing record(s) in 6ms  
WriteResult({  
  "nMatched": 1,  
  "nUpserted": 0,  
  "nModified": 1  
})  

be-mean-mongodb> var query = {"name": "Projeto 2"}  
be-mean-mongodb> var mod = {$push: {"members": [{"name": "Ednilson Amaral", "type_member": "Tipo A", "notify": false}]}}  
be-mean-mongodb> db.projects.update(query, mod)  

Updated 1 existing record(s) in 9ms  
WriteResult({  
  "nMatched": 1,  
  "nUpserted": 0,  
  "nModified": 1  
})  

be-mean-mongodb> var mod = {$push: {"members": [{"name": "Jéssica Bueno", "type_member": "Tipo A", "notify": false}]}}  
be-mean-mongodb> db.projects.update(query, mod)  

Updated 1 existing record(s) in 5ms  
WriteResult({  
  "nMatched": 1,  
  "nUpserted": 0,  
  "nModified": 1  
})  

be-mean-mongodb> var query = {"name": "Projeto 3"}  
be-mean-mongodb> var mod = {$push: {"members": [{"name": "Rodrigo Amaral", "type_member": "Tipo A", "notify": false}]}}  
be-mean-mongodb> db.projects.update(query, mod)  

Updated 1 existing record(s) in 3ms  
WriteResult({  
  "nMatched": 1,  
  "nUpserted": 0,  
  "nModified": 1  
})  

be-mean-mongodb> var mod = {$push: {"members": [{"name": "Laís Ramos", "type_member": "Tipo A", "notify": false}]}}  
be-mean-mongodb> db.projects.update(query, mod)  

Updated 1 existing record(s) in 2ms  
WriteResult({  
  "nMatched": 1,  
  "nUpserted": 0,  
  "nModified": 1  
})  

be-mean-mongodb> var query = {"name": "Projeto 4"}  
be-mean-mongodb> var mod = {$push: {"members": [{"name": "Admin", "type_member": "Tipo A", "notify": false}]}}  
be-mean-mongodb> db.projects.update(query, mod)  

Updated 1 existing record(s) in 8ms  
WriteResult({  
  "nMatched": 1,  
  "nUpserted": 0,  
  "nModified": 1  
})  

be-mean-mongodb> var mod = {$push: {"members": [{"name": "Ednilson Amaral", "type_member": "Tipo A", "notify": false}]}}  
be-mean-mongodb> db.projects.update(query, mod)  

Updated 1 existing record(s) in 7ms  
WriteResult({  
  "nMatched": 1,  
  "nUpserted": 0,  
  "nModified": 1  
})  

be-mean-mongodb> var query = {"name": "Projeto 5"}  
be-mean-mongodb> var mod = {$push: {"members": [{"name": "Jean Carlo", "type_member": "Tipo A", "notify": false}]}}  
be-mean-mongodb> db.projects.update(query, mod)  

Updated 1 existing record(s) in 3ms  
WriteResult({  
  "nMatched": 1,  
  "nUpserted": 0,  
  "nModified": 1  
})  

be-mean-mongodb> var mod = {$push: {"members": [{"name": "Carlos Almeida", "type_member": "Tipo A", "notify": false}]}}  
be-mean-mongodb> db.projects.update(query, mod)  

Updated 1 existing record(s) in 6ms  
WriteResult({  
  "nMatched": 1,  
  "nUpserted": 0,  
  "nModified": 1  
})  
```


### Adicione 1 comentário em cada atividade, deixe apenas 1 projeto sem.  

```  
be-mean-mongodb> for(var i=0; i<=4; i++){  
  var query = {"name": "Projeto " + i};  
  var mod = {$push: {"comments": [{"user_name": "Ednilson Amaral", "text": "Apenas mais um comentário qualquer aqui, parça!", "date_comment": null}]}};  
 
  db.projects.update(query, mod);  
 
  i++  
}  

Updated 0 record(s) in 3ms  
Updated 1 existing record(s) in 3ms  
Updated 1 existing record(s) in 2ms  
4  
```


### Adicione 1 projeto inteiro com UPSERT.  

```  
be-mean-mongodb> var query = {"name": "Novo Projeto Qualquer"}  

be-mean-mongodb> var mod = {  
  $set: {"views": 1},  

  $setOnInsert: {  
    "name": "Novo Projeto Qualquer",  
    "description": "Lorem ipsum dolor sit amet, consectetur adipisicing elit",  
    "date_begin": null,  
    "date_dream": null,  
    "date_end": null,  
    "visible": false,  
    "realocate": false,  
    "expired": false,  
    "visualizable_mod": null,  
     
    "tags": [],  
     
    "members": [],  

    "goals": [{  
    "name": "Lionel Messi",  
    "description": "Lorem ipsum dolor sit amet, consectetur adipisicing elit",  
    "date_begin": null,  
    "date_dream": null,  
    "date_end": null,  
    "realocate": false,  
    "expired": false,  

    "tags": [] 
    }],  

    "comments": []  
  }  
}  

be-mean-mongodb> var opt = {upsert: true}  
be-mean-mongodb> db.projects.update(query, mod, opt)  
Updated 1 new record(s) in 4ms  
WriteResult({  
  "nMatched": 0,  
  "nUpserted": 1,  
  "nModified": 0,  
  "_id": ObjectId("566f7daf6a9e204090ad1b70")  
})  

be-mean-mongodb> var query = {"_id": ObjectId("566f7daf6a9e204090ad1b70")}  
be-mean-mongodb> query  
{  
  "_id": ObjectId("566f7daf6a9e204090ad1b70")  
}  

be-mean-mongodb> db.projects.find(query)  
{  
  "_id": ObjectId("566f7daf6a9e204090ad1b70"),  
  "name": "Novo Projeto Qualquer",  
  "views": 1,  
  "description": "Lorem ipsum dolor sit amet, consectetur adipisicing elit",  
  "date_begin": null,  
  "date_dream": null,  
  "date_end": null,  
  "visible": false,  
  "realocate": false,  
  "expired": false,  
  "visualizable_mod": null,  
  "tags": [ ],  
  "members": [ ],  
  "goals": [  
    {  
      "name": "Lionel Messi",  
      "description": "Lorem ipsum dolor sit amet, consectetur adipisicing elit",  
      "date_begin": null,  
      "date_dream": null,  
      "date_end": null,  
      "realocate": false,  
      "expired": false,  
      "tags": [ ],   
    }  
  ],  
  "comments": [ ]  
}  
Fetched 1 record(s) in 6ms  
```


## Delete - remoção  

### Apague todos os projetos que não possuam tags.  

```  
be-mean-mongodb> db.projects.remove({"tags": []})  
Removed 1 record(s) in 3ms  
WriteResult({  
  "nRemoved": 1  
})  
```


### Apague todos os projetos que não possuam comentários nas atividades.  

```  
be-mean-mongodb> db.projects.remove({"comments": []})  
Removed 0 record(s) in 3ms  
WriteResult({  
  "nRemoved": 0  
})  
```


### Apague todos os projetos que não possuam atividades.  

```  
be-mean-mongodb> db.activities.find({}, {name_project: 1})  
{  
  "_id": ObjectId("5674c077c1519c3ff3710403"),  
  "name_project": "Projeto 1"  
}  
{  
  "_id": ObjectId("5674c077c1519c3ff3710404"),  
  "name_project": "Projeto 1"  
}  
{  
  "_id": ObjectId("5674c077c1519c3ff3710405"),  
  "name_project": "Projeto 2"  
}  
{  
  "_id": ObjectId("5674c077c1519c3ff3710406"),  
  "name_project": "Projeto 2"  
}  
{  
  "_id": ObjectId("5674c077c1519c3ff3710407"),  
  "name_project": "Projeto 3"  
}  
{  
  "_id": ObjectId("5674c077c1519c3ff3710408"),  
  "name_project": "Projeto 3"  
}  
{  
  "_id": ObjectId("5674c077c1519c3ff3710409"),  
  "name_project": "Projeto 4"  
}  
{  
  "_id": ObjectId("5674c077c1519c3ff371040a"),  
  "name_project": "Projeto 4"  
}  
Fetched 8 record(s) in 6ms  

be-mean-mongodb> var remover = {"name": /projeto 5/i}  

be-mean-mongodb> db.projects.remove(remover)  
Removed 1 record(s) in 6ms  
WriteResult({  
  "nRemoved": 1  
})  
```


### Escolha 2 usuários e apague todos os projetos em que os 2 fazem parte.  

```  
be-mean-mongodb> var query = {"comments.user_name": "Ednilson Amaral"}  

be-mean-mongodb> query  
{  
  "comments.user_name": "Ednilson Amaral"  
}  

be-mean-mongodb> db.projects.remove(query)  
Removed 1 record(s) in 10ms  
WriteResult({  
  "nRemoved": 1  
})  

be-mean-mongodb> var query = {"comments.user_name": "Laís Ramos"}  

be-mean-mongodb> db.projects.remove(query)  
Removed 1 record(s) in 3ms  
WriteResult({  
  "nRemoved": 1  
})  
```


### Apague todos os projetos que possuam uma determinada tag em goal.  

```  
be-mean-mongodb> var query = {"goals.tags": {$in: [/tchau/i]}}  

be-mean-mongodb> query  
{  
  "goals.tags": {  
    "$in": [  
      /tchau/i  
    ]  
  }  
}  

be-mean-mongodb> db.projects.remove(query)  
Removed 1 record(s) in 4ms  
WriteResult({  
  "nRemoved": 1  
})  
```


## Gerenciamento de usuários  

### Crie um usuário com permissões APENAS de Leitura.  

```  
be-mean-mongodb> db.createUser(  
  {  
    user: "ednilson",  
    pwd: "123456",  
    roles: [{role: "read", db: "be-mean-mongodb"}]  
  }  
)  

Successfully added user: {  
  "user": "ednilson",  
  "roles": [  
    {  
      "role": "read",  
      "db": "be-mean-mongodb"  
    }  
  ]  
}  
```


### Crie um usuário com permissões de Escrita e Leitura.  

```  
be-mean-mongodb> db.createUser(  
  {  
    user: "admin",  
    pwd: "admin123",  
    roles: [{role: "readWrite", db: "be-mean-mongodb"}]  
  }  
)  

Successfully added user: {  
  "user": "admin",  
  "roles": [  
    {  
      "role": "readWrite",  
      "db": "be-mean-mongodb"  
    }  
  ]  
}  
```


### Adicionar o papel grantRolesToUser e revokeRole para o usuário com Escrita e Leitura.  

Primeiramente o `grantRolesToUser`:  

```  
be-mean-mongodb> db.runCommand({  
    grantRolesToUser: "admin",  
    roles: [  
        {  
          role: "read", db: "be-mean-mongodb"  
        },  
        "readWrite"  
    ],  
    writeConcern: {w: "majority", wtimeout: 2000}  
  }  
)  

{  
  "ok": 1  
}  
```  

Então, o `revokeRolesFromUser`:  

```  
db.runCommand({  
  revokeRolesFromUser: "admin",  
  roles: [  
    {  
      role: "read", db: "be-mean-mongodb"  
    },  
    "readWrite"  
  ],  
  writeConcern: {w: "majority"}  
  }  
)  

{  
  "ok": 1  
}  
```


### Listar todos os usuários com seus papéis e ações.  

```  
be-mean-mongodb> db.runCommand({usersInfo: 1})  
{  
  "users": [  
    {  
      "_id": "be-mean-mongodb.ednilson",  
      "user": "ednilson",  
      "db": "be-mean-mongodb",  
      "roles": [  
        {  
          "role": "read",  
          "db": "be-mean-mongodb"  
        }  
      ]  
    },  
    {  
      "_id": "be-mean-mongodb.admin",  
      "user": "admin",  
      "db": "be-mean-mongodb",  
      "roles": [ ]  
    }  
  ],  
  "ok": 1  
}  
```


## Cluster  

### Servidor  

```  
ednilson@EDNILSON-NB:/var/www/be-mean-instagram-mongodb$ sudo mongod --dbpath /var/lib/mongodb --configsvr --port 27050  
```

### Router  

```  
ednilson@EDNILSON-NB:/var/www/be-mean-instagram-mongodb$ mongos --configdb localhost:27050 --port 27060  
```

### Shardings  

#### Criando os shardings  

```  
ednilson@EDNILSON-NB:/var/lib/mongodb$ sudo mkdir /var/lib/mongodb/shard1  
ednilson@EDNILSON-NB:/var/lib/mongodb$ sudo mkdir /var/lib/mongodb/shard2  
ednilson@EDNILSON-NB:/var/lib/mongodb$ sudo mkdir /var/lib/mongodb/shard3  
```  

#### Iniciando os shards  

```  
ednilson@EDNILSON-NB:~$ sudo mongod --port 27070 --dbpath /var/lib/mongodb/shard1  
ednilson@EDNILSON-NB:~$ sudo mongod --port 27080 --dbpath /var/lib/mongodb/shard2  
ednilson@EDNILSON-NB:~$ sudo mongod --port 27090 --dbpath /var/lib/mongodb/shard3  
```  

#### Registrando os shards no router  

```  
ednilson@EDNILSON-NB:~$ mongo --port 27060 --host localhost  
MongoDB shell version: 3.2.0  
connecting to: localhost:27060/test  
Mongo-Hacker 0.0.8  
EDNILSON-NB:27060(mongos-3.2.0)[mongos] test>  

[mongos] test> sh.addShard("localhost:27070")  
{  
  "shardAdded": "shard0000",  
  "ok": 1  
}  

[mongos] test> sh.addShard("localhost:27080")  
{  
  "shardAdded": "shard0001",  
  "ok": 1  
}  

[mongos] test> sh.addShard("localhost:27090")  
{  
  "shardAdded": "shard0002",  
  "ok": 1  
}  
```  

#### Especificando qual *database* e qual coleção iremos *shardear*  

```  
[mongos] test> sh.enableSharding("be-mean-mongodb")  
{  
  "ok": 1  
}  

[mongos] test> sh.shardCollection("be-mean-mongodb.projects", {"_id": 1})  
{  
  "collectionsharded": "be-mean-mongodb.projects",  
  "ok": 1  
}  
```

### Replicas  

#### Criando as replicas  

```  
ednilson@EDNILSON-NB:/var/lib/mongodb$ sudo mkdir /var/lib/mongodb/rs1  
ednilson@EDNILSON-NB:/var/lib/mongodb$ sudo mkdir /var/lib/mongodb/rs2  
ednilson@EDNILSON-NB:/var/lib/mongodb$ sudo mkdir /var/lib/mongodb/rs3  

ednilson@EDNILSON-NB:/var/www/be-mean-instagram-mongodb$ sudo mongod --replSet replica_set --port 27100 --dbpath /var/lib/mongodb/rs1  

ednilson@EDNILSON-NB:/var/www/be-mean-instagram-mongodb$ sudo mongod --replSet replica_set --port 27200 --dbpath /var/lib/mongodb/rs2  

ednilson@EDNILSON-NB:/var/www/be-mean-instagram-mongodb$ sudo mongod --replSet replica_set --port 27300 --dbpath /var/lib/mongodb/rs3  
```  

#### Iniciando uma replica **primary**  

```  
ednilson@EDNILSON-NB:~$ mongo --port 27100  
MongoDB shell version: 3.2.0  
connecting to: 127.0.0.1:27100/test  
Mongo-Hacker 0.0.8  
Server has startup warnings:  
2015-12-19T00:43:36.680-0200 I CONTROL  [initandlisten] ** WARNING: You are running this process as the root user, which is not recommended.  
2015-12-19T00:43:36.680-0200 I CONTROL  [initandlisten]  
2015-12-19T00:43:36.681-0200 I CONTROL  [initandlisten]  
2015-12-19T00:43:36.681-0200 I CONTROL  [initandlisten] ** WARNING: /sys/kernel/mm/transparent_hugepage/defrag is 'always'.  
2015-12-19T00:43:36.681-0200 I CONTROL  [initandlisten] **        We suggest setting it to 'never'  
2015-12-19T00:43:36.681-0200 I CONTROL  [initandlisten]  
2015-12-19T00:43:36.681-0200 I CONTROL  [initandlisten] ** WARNING: soft rlimits too low. rlimits set to 22582 processes, 65536 files. Number of processes should be at least 32768 : 0.5 times number of files.  
2015-12-19T00:43:36.681-0200 I CONTROL  [initandlisten]  
EDNILSON-NB:27100(mongod-3.2.0) test> use be-mean-mongodb
switched to db be-mean-mongodb
EDNILSON-NB:27100(mongod-3.2.0) [PRIMARY] be-mean-mongodb>  
```  

#### Iniciando uma replica **secondary**  

```  
ednilson@EDNILSON-NB:~$ mongo --port 27200  
MongoDB shell version: 3.2.0  
connecting to: 127.0.0.1:27200/test  
Mongo-Hacker 0.0.8  
Server has startup warnings:  
2015-12-19T00:43:36.680-0200 I CONTROL  [initandlisten] ** WARNING: You are running this process as the root user, which is not recommended.  
2015-12-19T00:43:36.680-0200 I CONTROL  [initandlisten]  
2015-12-19T00:43:36.681-0200 I CONTROL  [initandlisten]  
2015-12-19T00:43:36.681-0200 I CONTROL  [initandlisten] ** WARNING: /sys/kernel/mm/transparent_hugepage/defrag is 'always'.  
2015-12-19T00:43:36.681-0200 I CONTROL  [initandlisten] **        We suggest setting it to 'never'  
2015-12-19T00:43:36.681-0200 I CONTROL  [initandlisten]  
2015-12-19T00:43:36.681-0200 I CONTROL  [initandlisten] ** WARNING: soft rlimits too low. rlimits set to 22582 processes, 65536 files. Number of processes should be at least 32768 : 0.5 times number of files.  
2015-12-19T00:43:36.681-0200 I CONTROL  [initandlisten]  
EDNILSON-NB:27100(mongod-3.2.0) test> use be-mean-mongodb
switched to db be-mean-mongodb
EDNILSON-NB:27100(mongod-3.2.0) [SECONDARY] be-mean-mongodb>  
```