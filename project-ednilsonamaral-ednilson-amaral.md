# MongoDb - Projeto Final  
**Autor:** Ednilson Amaral  
**Data:** 15/12/2015 às 01:55


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
		name: String,  
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

		tags: [],  

		activities: [{  
			name: String,  
			description: String,  
			date_begin: Date,  
			date_dream: Date,  
			date_end: Date,  
			realocate: boolean,  
			expired: boolean  
		}],  
	}],  

	comments: [{  
		user_name: users.name,  
		text: String,  
		date_comment: String  
	}]  
}  
```


## Create - cadastro  

### Usuários  

Primeiramente, fiz um único cadastro para teste:  

```  
test> use be-mean-mongodb
switched to db be-mean-mongodb

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
			"hash_token": "oi",  
		}  
	],    
	"date_register": Date.now(),  
	"avatar_path": "oi"}  

be-mean-mongodb> json  
{  
  "name": "Ednilson Amaral",  
  "bio": "Desenvolvedor Frontend",  
  "email": "ednilsonamaral.ti@gmail.com",  
  "auth": [  
    {  
      "username": "ednilson",  
      "password": "123456",  
      "last_access": 1450115832467,  
      "online": false,  
      "disable": false,  
      "hash_token": "oi"  
    }  
  ],  
  "date_register": 1450115832467,  
  "avatar_path": "oi"  
}  

be-mean-mongodb> db.users.save(json)  
Inserted 1 record(s) in 145ms  
WriteResult({  
  "nInserted": 1  
})  

be-mean-mongodb> db.users.find()  
{  
  "_id": ObjectId("566f030c0b5e9fe3f7610e1d"),  
  "name": "Ednilson Amaral",  
  "bio": "Desenvolvedor Frontend",  
  "email": "ednilsonamaral.ti@gmail.com",  
  "auth": [  
    {  
      "username": "ednilson",  
      "password": "123456",  
      "last_access": 1450115832467,  
      "online": false,  
      "disable": false,  
      "hash_token": "oi"  
    }  
  ],  
  "date_register": 1450115832467,  
  "avatar_path": "oi"  
}  
Fetched 1 record(s) in 3ms  
```  

Após essa inserção de teste, inserir os 9 usuários restantes através de um `mongoimport`:  

```  
ednilson@EDNILSON-NB:/var/www/be-mean-instagram-mongodb$ mongoimport --db be-mean-mongodb --collection users --file users.json  
2015-12-14T16:17:03.615-0200	connected to: localhost  
2015-12-14T16:17:03.618-0200	imported 9 documents  
```  

Eu não utilizei o parâmetro `--drop` junto do `mongoimport` devido que o registro que já tinha na coleção `users` também será utilizado posteriormente.


### Projetos  

```  
ednilson@EDNILSON-NB:/var/www/be-mean-instagram-mongodb$ mongoimport --db be-mean-mongodb --collection projects --drop --file projects.json  
2015-12-14T17:13:16.227-0200	connected to: localhost  
2015-12-14T17:13:16.227-0200	dropping: be-mean-mongodb.projects  
2015-12-14T17:13:16.383-0200	imported 5 documents  
```  

O arquivo `projects.json`, contém:  

```  
{"name": "Projeto 1","description": "Lorem ipsum dolor sit amet, consectetur adipisicing elit","date_begin": null,"date_dream": null,"date_end": null,"visible": false,"realocate": false,"expired": false,"visualizable_mod": null,"tags": ["web", "nfl", "be mean"],"members": [{"name": "Edson Arantes do Nascimento","type_member": "Tipo B","notify": false},{"name": "Carlitos Tevez","type_member": "Tipo C","notify": false},{"name": "Stephen Curry","type_member": "Tipo A","notify": false},{"name": "Aaron Rodgers","type_member": "Tipo A","notify": false},{"name": "Tom Brady","type_member": "Tipo A","notify": false}],"goals": [{"name": "Vagner Love","description": "Recebeu passe de Renato Augusto, goleiro adversário se adiantou e fez um golaço de cobertura!","date_begin": null,"date_dream": null,"date_end": null,"realocate": false,"expired": false,"tags": ["cobertura", "chapeuzinho", "por cima"],"activities": [{"name": "Atividade 1","description": "Descrição completa da atividade 1","date_begin": null,"date_dream": null,"date_end": null,"realocate": false,"expired": false},{"name": "Atividade 2","description": "Descrição completa da atividade 2","date_begin": null,"date_dream": null,"date_end": null,"realocate": false,"expired": false}]}],"comments": [{"user_name": "Ednilson Amaral","text": "Belo gol de Vagner Love. Demorou para desencantar, mas depois que desencantou, não pára mais de fazer gol! Isso é ótimo para ele e perfeito para o Corinthians, parça!","date_comment": null}]}  

{"name": "Projeto 2","description": "Lorem ipsum dolor sit amet, consectetur adipisicing elit","date_begin": null,"date_dream": null,"date_end": null,"visible": false,"realocate": false,"expired": false,"visualizable_mod": null,"tags": ["zaga", "escanteio", "fundo"],"members": [{"name": "GPP","type_member": "Tipo B","notify": false},{"name": "Odel Bechkam Jr","type_member": "Tipo C","notify": false},{"name": "Victor Cruz","type_member": "Tipo A","notify": false},{"name": "Bill Bellicheck","type_member": "Tipo A","notify": false},{"name": "Luis Enrique","type_member": "Tipo A","notify": false}],"goals": [{"name": "Vermaleen","description": "Lorem ipsum dolor sit amet, consectetur adipisicing elit","date_begin": null,"date_dream": null,"date_end": null,"realocate": false,"expired": false,"tags": ["jovem", "cabeça", "zagueiro"],"activities": [{"name": "Atividade 1","description": "Descrição completa da atividade 1","date_begin": null,"date_dream": null,"date_end": null,"realocate": false,"expired": false},{"name": "Atividade 2","description": "Descrição completa da atividade 2","date_begin": null,"date_dream": null,"date_end": null,"realocate": false,"expired": false}]}],"comments": [{"user_name": "Érica dos Santos","text": "Lorem ipsum dolor sit amet, consectetur adipisicing elit","date_comment": null}]}  

{"name": "Projeto 3","description": "Lorem ipsum dolor sit amet, consectetur adipisicing elit","date_begin": null,"date_dream": null,"date_end": null,"visible": false,"realocate": false,"expired": false,"visualizable_mod": null,"tags": ["zaga", "ataque", "sozinho"],"members": [{"name": "PVC","type_member": "Tipo B","notify": false},{"name": "Everaldo Marques","type_member": "Tipo C","notify": false},{"name": "Carlão Barreto","type_member": "Tipo A","notify": false},{"name": "Mauro Betting","type_member": "Tipo A","notify": false},{"name": "Nivaldo Pietro","type_member": "Tipo A","notify": false}],"goals": [{"name": "Cristiano Ronaldo","description": "Lorem ipsum dolor sit amet, consectetur adipisicing elit","date_begin": null,"date_dream": null,"date_end": null,"realocate": false,"expired": false,"tags": ["fila", "time", "cade"],"activities": [{"name": "Atividade 1","description": "Descrição completa da atividade 1","date_begin": null,"date_dream": null,"date_end": null,"realocate": false,"expired": false},{"name": "Atividade 2","description": "Descrição completa da atividade 2","date_begin": null,"date_dream": null,"date_end": null,"realocate": false,"expired": false}]}],"comments": [{"user_name": "Laís Ramos","text": "Lorem ipsum dolor sit amet, consectetur adipisicing elit","date_comment": null}]}  

{"name": "Projeto 4","description": "Lorem ipsum dolor sit amet, consectetur adipisicing elit","date_begin": null,"date_dream": null,"date_end": null,"visible": false,"realocate": false,"expired": false,"visualizable_mod": null,"tags": ["vaca", "meia lua", "gremio"],"members": [{"name": "Juninho Pernambucano","type_member": "Tipo B","notify": false},{"name": "William","type_member": "Tipo C","notify": false},{"name": "Junior","type_member": "Tipo A","notify": false},{"name": "Luis Roberto","type_member": "Tipo A","notify": false},{"name": "Cleber Machado","type_member": "Tipo A","notify": false}],"goals": [{"name": "Ronaldinho Gaúcho","description": "Lorem ipsum dolor sit amet, consectetur adipisicing elit","date_begin": null,"date_dream": null,"date_end": null,"realocate": false,"expired": false,"tags": ["meia lua", "drible da vaca", "tchau"],"activities": [{"name": "Atividade 1","description": "Descrição completa da atividade 1","date_begin": null,"date_dream": null,"date_end": null,"realocate": false,"expired": false},{"name": "Atividade 2","description": "Descrição completa da atividade 2","date_begin": null,"date_dream": null,"date_end": null,"realocate": false,"expired": false}]}],	"comments": [{"user_name": "Carlos Almeida","text": "Lorem ipsum dolor sit amet, consectetur adipisicing elit","date_comment": null}]}  

{  
	"name": "Projeto 5",  
	"description": "Lorem ipsum dolor sit amet, consectetur adipisicing elit",  
	"date_begin": null,  
	"date_dream": null,  
	"date_end": null,  
	"visible": false,  
	"realocate": false,  
	"expired": false,  
	"visualizable_mod": null,  
  
	"tags": ["internacional", "barcelona", "espanhol"],  

	"members": [  
		{  
		"name": "Ronaldo",  
		"type_member": "Tipo B",  
		"notify": false  
		},  

		{  
		"name": "Milton Leite",  
		"type_member": "Tipo C",  
		"notify": false  
		},  

		{  
		"name": "Galvão Bueno",  
		"type_member": "Tipo A",  
		"notify": false  
		},  

		{  
		"name": "Eli Manning",  
		"type_member": "Tipo A",  
		"notify": false  
		},  
  
		{  
		"name": "Peyton Manning",  
		"type_member": "Tipo A",  
		"notify": false  
		}	  
	],  

	"goals": [{  
		"name": "Lionel Messi",  
		"description": "Lorem ipsum dolor sit amet, consectetur adipisicing elit",  
		"date_begin": null,  
		"date_dream": null,  
		"date_end": null,  
		"realocate": false,  
		"expired": false,  

		"tags": ["falta", "angulo", "barreira"],  

		"activities": []  
	}],  

	"comments": [  
		{  
		"user_name": "José da Silva",  
		"text": "Lorem ipsum dolor sit amet, consectetur adipisicing elit",  
		"date_comment": null  
		}  
	]  
}   
```


## Retrieve - busca  

### Liste as informações dos membros de 1 projeto específico que deve ser buscado pelo seu nome de forma a não ligar para maiúsculas e minúsculas.  

```  
be-mean-mongodb> db.projects.find({"name": /projeto 1/i})  
{  
  "_id": ObjectId("566f1913d0db00d58dd9aafe"),  
  "name": "Projeto 1",  
  "description": "Lorem ipsum dolor sit amet, consectetur adipisicing elit",  
  "date_begin": null,  
  "date_dream": null,  
  "date_end": null,  
  "visible": false,  
  "realocate": false,  
  "expired": false,  
  "visualizable_mod": null,  
  "tags": [  
    "web",  
    "nfl",  
    "be mean"  
  ],  
  "members": [  
    {  
      "name": "Edson Arantes do Nascimento",  
      "type_member": "Tipo B",  
      "notify": false  
    },  
    {  
      "name": "Carlitos Tevez",  
      "type_member": "Tipo C",  
      "notify": false  
    },  
    {  
      "name": "Stephen Curry",  
      "type_member": "Tipo A",  
      "notify": false  
    },  
    {  
      "name": "Aaron Rodgers",  
      "type_member": "Tipo A",  
      "notify": false  
    },  
    {  
      "name": "Tom Brady",  
      "type_member": "Tipo A",  
      "notify": false  
    }  
  ],  
  "goals": [  
    {  
      "name": "Vagner Love",  
      "description": "Recebeu passe de Renato Augusto, goleiro adversário se adiantou e fez um golaço de cobertura!",  
      "date_begin": null,  
      "date_dream": null,  
      "date_end": null,  
      "realocate": false,  
      "expired": false,  
      "tags": [  
        "cobertura",  
        "chapeuzinho",  
        "por cima"  
      ],  
      "activities": [  
        {  
          "name": "Atividade 1",  
          "description": "Descrição completa da atividade 1",  
          "date_begin": null,  
          "date_dream": null,  
          "date_end": null,  
          "realocate": false,  
          "expired": false  
        },  
        {  
          "name": "Atividade 2",  
          "description": "Descrição completa da atividade 2",  
          "date_begin": null,  
          "date_dream": null,  
          "date_end": null,  
          "realocate": false,  
          "expired": false  
        }  
      ]  
    }  
  ],  
  "comments": [  
    {  
      "user_name": "Ednilson Amaral",  
      "text": "Belo gol de Vagner Love. Demorou para desencantar, mas depois que desencantou, não pára mais de fazer gol! Isso é ótimo para ele e perfeito para o Corinthians, parça!",  
      "date_comment": null  
    }  
  ]  
}  
Fetched 1 record(s) in 10ms  
```


### Liste todos os projetos com a tag que você escolheu para os 3 projetos em comum.  

```  
be-mean-mongodb> var query = {tags: {$in: [/web/i]}}  

be-mean-mongodb> db.projects.find(query, {name: 1})  
{  
  "_id": ObjectId("566f1913d0db00d58dd9aafe"),  
  "name": "Projeto 1"  
}  
{  
  "_id": ObjectId("566f1913d0db00d58dd9aaff"),  
  "name": "Projeto 2"  
}  
{  
  "_id": ObjectId("566f1913d0db00d58dd9ab00"),  
  "name": "Projeto 3"  
}  
Fetched 3 record(s) in 3ms  
```


### Liste apenas os nomes de todas as atividades para todos os projetos.  

```  
be-mean-mongodb> var query = {'name': 1, 'goals.activities.name': 1}  

be-mean-mongodb> query  
{  
  "name": 1,  
  "goals.activities.name": 1  
}  

be-mean-mongodb> db.projects.find({}, query)  
{  
  "_id": ObjectId("566f1913d0db00d58dd9aafe"),  
  "name": "Projeto 1",  
  "goals": [  
    {  
      "activities": [  
        {  
          "name": "Atividade 1"  
        },  
        {  
          "name": "Atividade 2"  
        }  
      ]  
    }  
  ]  
}  
{  
  "_id": ObjectId("566f1913d0db00d58dd9aaff"),  
  "name": "Projeto 2",  
  "goals": [  
    {  
      "activities": [  
        {  
          "name": "Atividade 1"  
        },  
        {  
          "name": "Atividade 2"  
        }  
      ]  
    }  
  ]  
}  
{  
  "_id": ObjectId("566f1913d0db00d58dd9ab00"),  
  "name": "Projeto 3",  
  "goals": [  
    {  
      "activities": [  
        {  
          "name": "Atividade 1"  
        },  
        {  
          "name": "Atividade 2"  
        }  
      ]  
    }  
  ]  
}  
{  
  "_id": ObjectId("566f1913d0db00d58dd9ab01"),  
  "name": "Projeto 5",  
  "goals": [  
    {  
      "activities": [ ]  
    }  
  ]  
}  
{  
  "_id": ObjectId("566f1913d0db00d58dd9ab02"),  
  "name": "Projeto 4",  
  "goals": [  
    {  
      "activities": [  
        {  
          "name": "Atividade 1"  
        },  
        {  
          "name": "Atividade 2"  
        }  
      ]  
    }  
  ]  
}  
Fetched 5 record(s) in 5ms  
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
  "_id": ObjectId("566f1913d0db00d58dd9aaff"),  
  "members": [  
    {  
      "name": "GPP"  
    },  
    {  
      "name": "Odel Bechkam Jr"  
    },  
    {  
      "name": "Victor Cruz"  
    },  
    {  
      "name": "Bill Bellicheck"  
    },  
    {  
      "name": "Luis Enrique"  
    }  
  ]  
}  
{  
  "_id": ObjectId("566f1913d0db00d58dd9ab00"),  
  "members": [  
    {  
      "name": "PVC"  
    },  
    {  
      "name": "Everaldo Marques"  
    },  
    {  
      "name": "Carlão Barreto"  
    },  
    {  
      "name": "Mauro Betting"  
    },  
    {  
      "name": "Nivaldo Pietro"  
    }  
  ]  
}  
{  
  "_id": ObjectId("566f1913d0db00d58dd9ab01"),  
  "members": [  
    {  
      "name": "Ronaldo"  
    },  
    {  
      "name": "Milton Leite"  
    },  
    {  
      "name": "Galvão Bueno"  
    },  
    {  
      "name": "Eli Manning"  
    },  
    {  
      "name": "Peyton Manning"  
    }  
  ]  
}  
{  
  "_id": ObjectId("566f1913d0db00d58dd9ab02"),  
  "members": [  
    {  
      "name": "Juninho Pernambucano"  
    },  
    {  
      "name": "William"  
    },  
    {  
      "name": "Junior"  
    },  
    {  
      "name": "Luis Roberto"  
    },  
    {  
      "name": "Cleber Machado"  
    }  
  ]  
}  
Fetched 4 record(s) in 7ms  
```


## Update - alteração  

### Adicione para todos os projetos o campo views: 0.  

Eu não consegui inserir em todos os 5 projetos de uma só vez com o código abaixo:  

```  
be-mean-mongodb> var query = {}  
  
be-mean-mongodb> var mod = {$set: {views: 0}}  

be-mean-mongodb> mod  
{  
  "$set": {  
    "views": 0  
  }  
}  

be-mean-mongodb> db.projects.update(query, mod)  
Updated 1 existing record(s) in 3ms  
WriteResult({  
  "nMatched": 1,  
  "nUpserted": 0,  
  "nModified": 0  
})  
```  

Tentei passando como parâmetro os {} vazios, para que inserisse em todos os objetos. Mas não foi possível e não entendi o motivo disso não ter dado certo.  

Como alternativa, resolvi fazer uma condição `for` para ir modificando o nome do projeto, assim ir adicionando o campo `views: 0`. Sinceramente, odiei e fiquei com vergonha dessa solução, pior que amadora. :/  

```  
be-mean-mongodb> for(var i=0; i<=5; i++){  
  var query = {"name": "Projeto " + i};  
  var mod = {$set: {views: 0}};  

  db.projects.update(query, mod);  
 
  i++  
}  

Updated 0 record(s) in 2ms  
Updated 1 existing record(s) in 2ms  
Updated 1 existing record(s) in 3ms  
4  
}  
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
Updated 1 existing record(s) in 3ms  
4  
```


### Adicione 2 membros diferentes para cada projeto.  

```  
be-mean-mongodb> var query = {"name": "Projeto 1"}  
be-mean-mongodb> var mod = {$push: {"members": [{"name": "Fulano 1", "type_member": "Tipo A", "notify": false}]}}  
be-mean-mongodb> db.projects.update(query, mod)  

Updated 1 existing record(s) in 3ms  
WriteResult({  
  "nMatched": 1,  
  "nUpserted": 0,  
  "nModified": 1  
})  

be-mean-mongodb> var mod = {$push: {"members": [{"name": "Fulano 2", "type_member": "Tipo A", "notify": false}]}}  
be-mean-mongodb> db.projects.update(query, mod)  

Updated 1 existing record(s) in 6ms  
WriteResult({  
  "nMatched": 1,  
  "nUpserted": 0,  
  "nModified": 1  
})  

be-mean-mongodb> var query = {"name": "Projeto 2"}  
be-mean-mongodb> var mod = {$push: {"members": [{"name": "Fulano 1", "type_member": "Tipo A", "notify": false}]}}  
be-mean-mongodb> db.projects.update(query, mod)  

Updated 1 existing record(s) in 9ms  
WriteResult({  
  "nMatched": 1,  
  "nUpserted": 0,  
  "nModified": 1  
})  

be-mean-mongodb> var mod = {$push: {"members": [{"name": "Fulano 2", "type_member": "Tipo A", "notify": false}]}}  
be-mean-mongodb> db.projects.update(query, mod)  

Updated 1 existing record(s) in 5ms  
WriteResult({  
  "nMatched": 1,  
  "nUpserted": 0,  
  "nModified": 1  
})  

be-mean-mongodb> var query = {"name": "Projeto 3"}  
be-mean-mongodb> var mod = {$push: {"members": [{"name": "Fulano 1", "type_member": "Tipo A", "notify": false}]}}  
be-mean-mongodb> db.projects.update(query, mod)  

Updated 1 existing record(s) in 3ms  
WriteResult({  
  "nMatched": 1,  
  "nUpserted": 0,  
  "nModified": 1  
})  

be-mean-mongodb> var mod = {$push: {"members": [{"name": "Fulano 2", "type_member": "Tipo A", "notify": false}]}}  
be-mean-mongodb> db.projects.update(query, mod)  

Updated 1 existing record(s) in 2ms  
WriteResult({  
  "nMatched": 1,  
  "nUpserted": 0,  
  "nModified": 1  
})  

be-mean-mongodb> var query = {"name": "Projeto 4"}  
be-mean-mongodb> var mod = {$push: {"members": [{"name": "Fulano 1", "type_member": "Tipo A", "notify": false}]}}  
be-mean-mongodb> db.projects.update(query, mod)  

Updated 1 existing record(s) in 8ms  
WriteResult({  
  "nMatched": 1,  
  "nUpserted": 0,  
  "nModified": 1  
})  

be-mean-mongodb> var mod = {$push: {"members": [{"name": "Fulano 2", "type_member": "Tipo A", "notify": false}]}}  
be-mean-mongodb> db.projects.update(query, mod)  

Updated 1 existing record(s) in 7ms  
WriteResult({  
  "nMatched": 1,  
  "nUpserted": 0,  
  "nModified": 1  
})  

be-mean-mongodb> var query = {"name": "Projeto 5"}  
be-mean-mongodb> var mod = {$push: {"members": [{"name": "Fulano 1", "type_member": "Tipo A", "notify": false}]}}  
be-mean-mongodb> db.projects.update(query, mod)  

Updated 1 existing record(s) in 3ms  
WriteResult({  
  "nMatched": 1,  
  "nUpserted": 0,  
  "nModified": 1  
})  

be-mean-mongodb> var mod = {$push: {"members": [{"name": "Fulano 2", "type_member": "Tipo A", "notify": false}]}}  
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

    "tags": [],  

    "activities": []  
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
      "activities": [ ]  
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
be-mean-mongodb> var query = {"goals.activities": []}  

be-mean-mongodb> db.projects.remove(query)  
Removed 1 record(s) in 4ms  
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
2015-12-15T01:43:36.680-0200 I CONTROL  [initandlisten] ** WARNING: You are running this process as the root user, which is not recommended.  
2015-12-15T01:43:36.680-0200 I CONTROL  [initandlisten]  
2015-12-15T01:43:36.681-0200 I CONTROL  [initandlisten]  
2015-12-15T01:43:36.681-0200 I CONTROL  [initandlisten] ** WARNING: /sys/kernel/mm/transparent_hugepage/defrag is 'always'.  
2015-12-15T01:43:36.681-0200 I CONTROL  [initandlisten] **        We suggest setting it to 'never'  
2015-12-15T01:43:36.681-0200 I CONTROL  [initandlisten]  
2015-12-15T01:43:36.681-0200 I CONTROL  [initandlisten] ** WARNING: soft rlimits too low. rlimits set to 22582 processes, 65536 files. Number of processes should be at least 32768 : 0.5 times number of files.  
2015-12-15T01:43:36.681-0200 I CONTROL  [initandlisten]  
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
2015-12-15T01:43:36.680-0200 I CONTROL  [initandlisten] ** WARNING: You are running this process as the root user, which is not recommended.  
2015-12-15T01:43:36.680-0200 I CONTROL  [initandlisten]  
2015-12-15T01:43:36.681-0200 I CONTROL  [initandlisten]  
2015-12-15T01:43:36.681-0200 I CONTROL  [initandlisten] ** WARNING: /sys/kernel/mm/transparent_hugepage/defrag is 'always'.  
2015-12-15T01:43:36.681-0200 I CONTROL  [initandlisten] **        We suggest setting it to 'never'  
2015-12-15T01:43:36.681-0200 I CONTROL  [initandlisten]  
2015-12-15T01:43:36.681-0200 I CONTROL  [initandlisten] ** WARNING: soft rlimits too low. rlimits set to 22582 processes, 65536 files. Number of processes should be at least 32768 : 0.5 times number of files.  
2015-12-15T01:43:36.681-0200 I CONTROL  [initandlisten]  
EDNILSON-NB:27100(mongod-3.2.0) test> use be-mean-mongodb
switched to db be-mean-mongodb
EDNILSON-NB:27100(mongod-3.2.0) [SECONDARY] be-mean-mongodb>  
```