
#MongoDb - Projeto Final
**Autor:** Sóstenes Freitas de Andrade
**Data** : 14/Jun/2016

## Para qual sistema você usaria o MogoDB (diferente desse)?
```
Usuaria em um sistema generico de gerenciamento de projetos, organizaria muito a minha vida na faculdade.
```
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
```
activity: {
	name: String,
    description: String,
    date_begin: Date,
    date_dream: Date,
    date_end: Date,
    realocate: Boolean,
    date_realocate: Date,
    expired: Boolean,
    tags: [],
	members: [{
    	user_id: String,
        notify: String
    
	}],
comments: [{
    	user_id: String, //member
    	text: String,
        date_comment: Date,
		files: [{
        	path: String,
            weight: String,
            name: String
        
		}]
    
		  }]

}
```

####A coleção `activities` foi retirada da coleção `projects` porque sempre sera inserido novas atividades ao projetos, fazendo com que a coleção  fique muito grande, podendo estorar o limite do documento(16MB).

## Create - cadastro
#### Usuario
```
var usuarios = ["sostenes","ash","brok","misty","down","kukui","carvalho","oak","gray","luffy"];

usuarios.forEach(function(name){
		var user = {
        name: name,
        bio: null,
        date_register: "2015-05-11",
        avatar_path: null,
        background_path: null,
		auth: {
            username: name+'pokemon',
            email: name+"@pokemons.com",
            password:"123456789",
            last_access: "2016-06-12",
            status: "offline", //online, offline, disable, etc
        }
    };
   db.users.insert(user);
});

```
#### Cadastrando activities para os goals.

```
var activities = [
        {name: 'Completar a Pokedex', description: 'Temos que ajuda o professor Carvalho nos estudos dos pokemons'},
        {name: 'Ser o campeão da liga', description: 'Derrotar todos os lideres de ginasio'},
        {name: 'Vencer a Elite 4', description: 'Conquistar A liga mais forte de todas'},
        {name: 'Evoluir os pokemons', description: 'Tendo o maior poder'},
        {name: 'Capturar os Lendarios', description: 'Tornando-se o maior mestre pokemon'},
        {name: 'Fazer amigos ao longo da jornada', description: 'Não tem graça seguir viagem sozinho'},  
    
];

```

#### Criando 5 Projetos
```
var inserir = [
    {
        name: "Friends",
        description: "Amigos da jornada",
        tags: [ 'Best Friend Forever', 'BFF', 'Rumo a elite 4'  ],
		members: [
            { "_id": ObjectId("575d73d608e0e1db5cbafdf1"), type_member: "Principal",   notify: null  },
            { "_id": ObjectId("575d73d708e0e1db5cbafdf2"), type_member: "coadjuvante", notify: null  },
            { "_id": ObjectId("575d73d708e0e1db5cbafdf3"), type_member: "coadjuvante", notify: null  },
            { "_id": ObjectId("575d73d708e0e1db5cbafdf4"), type_member: "coadjuvante", notify: null  },
            { "_id": ObjectId("575d73d708e0e1db5cbafdf5"), type_member: "coadjuvante", notify: null  }
        
		],
		goals: [
		{
                name:"Eterna amizade",
                description: "nois é top",
                tags:['Catch All', 'Shyni', 'Legends'],
                activities: [ {"_id": ObjectId("575d8ea408e0e1db5cbafe00")}, {"_id": ObjectId("575d8ea408e0e1db5cbafdfe")}  ]
            
		}
        
		]
    
},
    
    
{
        name: "Lideres de Ginasio",
        description: "Os cara que tem uns poke massa",
        tags: [ 'Rumo a elite 4', 'Master', 'Lider'  ],
		members: [
            {"_id": ObjectId("575d73d708e0e1db5cbafdf6"), type_member: "normal", notify: null },
            {"_id": ObjectId("575d73d708e0e1db5cbafdf7"), type_member: "normal", notify: null },
            {"_id": ObjectId("575d73d708e0e1db5cbafdf8"), type_member: "normal", notify: null },
            {"_id": ObjectId("575d73d708e0e1db5cbafdf9"), type_member: "normal", notify: null },
            {"_id": ObjectId("575d73d708e0e1db5cbafdfa"), type_member: "normal", notify: null }  
        
		],
		goals: [
		{
                name:"Ganhar de todos",
                description: "Temos que derrota todos",
                tags:['Catch All', 'Webschool', 'Suissamon'],
                activities: [ { "_id": ObjectId("575d8ea408e0e1db5cbafdfd") }, {"_id": ObjectId("575d8ea408e0e1db5cbafdfc")}  ]
            
		}
        
		]
    
},
    
{
        name: "Pega os Lendarios",
        description: "Capturar todos os lendarios",
        tags: [ 'Rumo a elite 4', 'Ser o mais fore', 'The Best'  ],
		members: [
            {"_id": ObjectId("575d73d608e0e1db5cbafdf1"), type_member: "normal", notify: null },
            {"_id": ObjectId("575d73d708e0e1db5cbafdf9"), type_member: "normal", notify: null },
            {"_id": ObjectId("575d73d708e0e1db5cbafdf2"), type_member: "normal", notify: null },
            {"_id": ObjectId("575d73d708e0e1db5cbafdf3"), type_member: "normal", notify: null },
            {"_id": ObjectId("575d73d708e0e1db5cbafdf4"), type_member: "normal", notify: null }
        
		],
		goals: [
		{
                name:"Master Ball",
                description: "Com master Ball eh mais facil",
                tags:['Catch ALl', 'WebPokemon', 'suisamon'],
                activities: [ {"_id": ObjectId("575d8ea408e0e1db5cbafdff")},{"_id": ObjectId("575d8ea408e0e1db5cbafdfb")}  ]
            
		}
        
		]
    
},
    
{
        name: "Vencer a elite 4",
        description: " ganhar dos melhors mestres pokemons ",
        tags: [ 'Vencer', 'Best', 'Melhor'  ],
		members: [
             {"_id": ObjectId("575d73d708e0e1db5cbafdf4"), type_member: "normal", notify: null },
             {"_id": ObjectId("575d73d708e0e1db5cbafdf5"), type_member: "normal", notify: null },
             {"_id": ObjectId("575d73d708e0e1db5cbafdf6"), type_member: "normal", notify: null },
             {"_id": ObjectId("575d73d708e0e1db5cbafdf7"), type_member: "normal", notify: null },
             {"_id": ObjectId("575d73d708e0e1db5cbafdf8"), type_member: "normal", notify: null }
        
		],
		goals: [
		{
                name:"Elite 4 winner",
                description: "Ultimo desafio",
                tags:['Winner', 'Campeao', 'monster'],
                activities: [ {"_id": ObjectId("575d8ea408e0e1db5cbafdfd")}, { "_id": ObjectId("575d8ea408e0e1db5cbafdfe") }  ]
            
		}
        
		]
    
},
    
{
        name: "Fim de jogo",
        description: "Sem mais desafios",
        tags: [ 'Vencer', 'finish', 'end'  ],
		members: [
            {"_id": ObjectId("575d73d708e0e1db5cbafdf2"), type_member: "normal", notify: null },
            {"_id": ObjectId("575d73d708e0e1db5cbafdf3"), type_member: "normal", notify: null },
            {"_id": ObjectId("575d73d708e0e1db5cbafdf4"), type_member: "normal", notify: null },
            {"_id": ObjectId("575d73d708e0e1db5cbafdf9"), type_member: "normal", notify: null },
            {"_id": ObjectId("575d73d708e0e1db5cbafdfa"), type_member: "normal", notify: null }
        
		],
		goals: [
		{
                name:"End game",
                description: "Nao tem mais nada pra fazer",
                tags:['finish', 'game end', 'game over'],
                activities: []
            
		}
        
		]
    
}

]
    

db.project.insert(inserir)
Inserted 1 record(s) in 27ms
BulkWriteResult({
  "writeErrors": [  ],
  "writeConcernErrors": [  ],
  "nInserted": 5,
  "nUpserted": 0,
  "nMatched": 0,
  "nModified": 0,
  "nRemoved": 0,
  "upserted": [  ]

		})
```
## Retrieve - busca
#### Listar as informações dos membros de um projeto  
```
var projeto = db.projects.findOne({},{members:1})

var membros_info = []

projeto.members.forEach(function(member){membros_info.push(db.users.findOne(member._id))})

corsair(mongod-3.2.7) projeto-final> projeto_membros
[
{
    "_id": ObjectId("575d73d608e0e1db5cbafdf1"),
    "name": "sostenes",
    "bio": null,
    "date_register": "2015-05-11",
    "avatar_path": null,
    "background_path": null,
	"auth": {
      "username": "sostenespokemon",
      "email": "sostenes@pokemons.com",
      "password": "123456789",
      "last_access": "2016-06-12",
      "status": "offline"
    
	}
  
},
{
    "_id": ObjectId("575d73d708e0e1db5cbafdf2"),
    "name": "ash",
    "bio": null,
    "date_register": "2015-05-11",
    "avatar_path": null,
    "background_path": null,
	"auth": {
      "username": "ashpokemon",
      "email": "ash@pokemons.com",
      "password": "123456789",
      "last_access": "2016-06-12",
      "status": "offline"
    
	}
  
},
{
    "_id": ObjectId("575d73d708e0e1db5cbafdf3"),
    "name": "brok",
    "bio": null,
    "date_register": "2015-05-11",
    "avatar_path": null,
    "background_path": null,
	"auth": {
      "username": "brokpokemon",
      "email": "brok@pokemons.com",
      "password": "123456789",
      "last_access": "2016-06-12",
      "status": "offline"
    
	}
  
},
{
    "_id": ObjectId("575d73d708e0e1db5cbafdf4"),
    "name": "misty",
    "bio": null,
    "date_register": "2015-05-11",
    "avatar_path": null,
    "background_path": null,
	"auth": {
      "username": "mistypokemon",
      "email": "misty@pokemons.com",
      "password": "123456789",
      "last_access": "2016-06-12",
      "status": "offline"
    
	}
  
},
{
    "_id": ObjectId("575d73d708e0e1db5cbafdf5"),
    "name": "down",
    "bio": null,
    "date_register": "2015-05-11",
    "avatar_path": null,
    "background_path": null,
	"auth": {
      "username": "downpokemon",
      "email": "down@pokemons.com",
      "password": "123456789",
      "last_access": "2016-06-12",
      "status": "offline"
    
	}
  
}

]


```
#####Liste todos os projetos com a tag que você escolheu para os 3 projetos em comum 
```
corsair(mongod-3.2.7) projeto-final> db.project.find({tags:{$in:['Rumo a elite 4']}}).pretty()
{
  "_id": ObjectId("575d93ea08e0e1db5cbafe01"),
  "name": "Friends",
  "description": "Amigos da jornada",
  "tags": [
    "Best Friend Forever",
    "BFF",
    "Rumo a elite 4"
  
  ],
  "members": [
  {
      "_id": ObjectId("575d73d608e0e1db5cbafdf1"),
      "type_member": "Principal",
      "notify": null
    
  },
  {
      "_id": ObjectId("575d73d708e0e1db5cbafdf2"),
      "type_member": "coadjuvante",
      "notify": null
    
  },
  {
      "_id": ObjectId("575d73d708e0e1db5cbafdf3"),
      "type_member": "coadjuvante",
      "notify": null
    
  },
  {
      "_id": ObjectId("575d73d708e0e1db5cbafdf4"),
      "type_member": "coadjuvante",
      "notify": null
    
  },
  {
      "_id": ObjectId("575d73d708e0e1db5cbafdf5"),
      "type_member": "coadjuvante",
      "notify": null
    
  }
  
  ],
  "goals": [
  {
      "name": "Eterna amizade",
      "description": "nois é top",
	  "tags": [
        "Catch All",
        "Shyni",
        "Legends"
      
	  ],
	  "activities": [
	  {
          "_id": ObjectId("575d8ea408e0e1db5cbafe00")
        
	  },
	  {
          "_id": ObjectId("575d8ea408e0e1db5cbafdfe")
        
	  }
      
	  ]
    
  }
  
  ]

}
{
  "_id": ObjectId("575d93ea08e0e1db5cbafe02"),
  "name": "Lideres de Ginasio",
  "description": "Os cara que tem uns poke massa",
  "tags": [
    "Rumo a elite 4",
    "Master",
    "Lider"
  
  ],
  "members": [
  {
      "_id": ObjectId("575d73d708e0e1db5cbafdf6"),
      "type_member": "normal",
      "notify": null
    
  },
  {
      "_id": ObjectId("575d73d708e0e1db5cbafdf7"),
      "type_member": "normal",
      "notify": null
    
  },
  {
      "_id": ObjectId("575d73d708e0e1db5cbafdf8"),
      "type_member": "normal",
      "notify": null
    
  },
  {
      "_id": ObjectId("575d73d708e0e1db5cbafdf9"),
      "type_member": "normal",
      "notify": null
    
  },
  {
      "_id": ObjectId("575d73d708e0e1db5cbafdfa"),
      "type_member": "normal",
      "notify": null
    
  }
  
  ],
  "goals": [
  {
      "name": "Ganhar de todos",
      "description": "Temos que derrota todos",
	  "tags": [
        "Catch All",
        "Webschool",
        "Suissamon"
      
	  ],
	  "activities": [
	  {
          "_id": ObjectId("575d8ea408e0e1db5cbafdfd")
        
	  },
	  {
          "_id": ObjectId("575d8ea408e0e1db5cbafdfc")
        
	  }
      
	  ]
    
  }
  
  ]

}
{
  "_id": ObjectId("575d93ea08e0e1db5cbafe03"),
  "name": "Pega os Lendarios",
  "description": "Capturar todos os lendarios",
  "tags": [
    "Rumo a elite 4",
    "Ser o mais fore",
    "The Best"
  
  ],
  "members": [
  {
      "_id": ObjectId("575d73d608e0e1db5cbafdf1"),
      "type_member": "normal",
      "notify": null
    
  },
  {
      "_id": ObjectId("575d73d708e0e1db5cbafdf9"),
      "type_member": "normal",
      "notify": null
    
  },
  {
      "_id": ObjectId("575d73d708e0e1db5cbafdf2"),
      "type_member": "normal",
      "notify": null
    
  },
  {
      "_id": ObjectId("575d73d708e0e1db5cbafdf3"),
      "type_member": "normal",
      "notify": null
    
  },
  {
      "_id": ObjectId("575d73d708e0e1db5cbafdf4"),
      "type_member": "normal",
      "notify": null
    
  }
  
  ],
  "goals": [
  {
      "name": "Master Ball",
      "description": "Com master Ball eh mais facil",
	  "tags": [
        "Catch ALl",
        "WebPokemon",
        "suisamon"
      
	  ],
	  "activities": [
	  {
          "_id": ObjectId("575d8ea408e0e1db5cbafdff")
        
	  },
	  {
          "_id": ObjectId("575d8ea408e0e1db5cbafdfb")
        
	  }
      
	  ]
    
  }
  
  ]

}
Fetched 3 record(s) in 5ms


```
####Liste apenas os nomes das  atividades para todos os projetos
```
corsair(mongod-3.2.7) projeto-final> db.activity.find({}, { name: 1})
{
  "_id": ObjectId("575d8ea408e0e1db5cbafdfb"),
  "name": "Completar a Pokedex"

}
{
  "_id": ObjectId("575d8ea408e0e1db5cbafdfc"),
  "name": "Ser o campeão da liga"

}
{
  "_id": ObjectId("575d8ea408e0e1db5cbafdfd"),
  "name": "Vencer a Elite 4"

}
{
  "_id": ObjectId("575d8ea408e0e1db5cbafdfe"),
  "name": "Evoluir os pokemons"

}
{
  "_id": ObjectId("575d8ea408e0e1db5cbafdff"),
  "name": "Capturar os Lendarios"

}
{
  "_id": ObjectId("575d8ea408e0e1db5cbafe00"),
  "name": "Fazer amigos ao longo da jornada"

}
Fetched 6 record(s) in 2ms

```
#### Liste todos os projetos que não possuam uma tag.
```
corsair(mongod-3.2.7) projeto-final> db.project.find({'tags':{$nin:['Rumo a elite 4']}})
{
  "_id": ObjectId("575d93ea08e0e1db5cbafe04"),
  "name": "Vencer a elite 4",
  "description": " ganhar dos melhors mestres pokemons ",
  "tags": [
    "Vencer",
    "Best",
    "Melhor"
  
  ],
  "members": [
  {
      "_id": ObjectId("575d73d708e0e1db5cbafdf4"),
      "type_member": "normal",
      "notify": null
    
  },
  {
      "_id": ObjectId("575d73d708e0e1db5cbafdf5"),
      "type_member": "normal",
      "notify": null
    
  },
  {
      "_id": ObjectId("575d73d708e0e1db5cbafdf6"),
      "type_member": "normal",
      "notify": null
    
  },
  {
      "_id": ObjectId("575d73d708e0e1db5cbafdf7"),
      "type_member": "normal",
      "notify": null
    
  },
  {
      "_id": ObjectId("575d73d708e0e1db5cbafdf8"),
      "type_member": "normal",
      "notify": null
    
  }
  
  ],
  "goals": [
  {
      "name": "Elite 4 winner",
      "description": "Ultimo desafio",
	  "tags": [
        "Winner",
        "Campeao",
        "monster"
      
	  ],
	  "activities": [
	  {
          "_id": ObjectId("575d8ea408e0e1db5cbafdfd")
        
	  },
	  {
          "_id": ObjectId("575d8ea408e0e1db5cbafdfe")
        
	  }
      
	  ]
    
  }
  
  ]

}
{
  "_id": ObjectId("575d93ea08e0e1db5cbafe05"),
  "name": "Fim de jogo",
  "description": "Sem mais desafios",
  "tags": [
    "Vencer",
    "finish",
    "end"
  
  ],
  "members": [
  {
      "_id": ObjectId("575d73d708e0e1db5cbafdf2"),
      "type_member": "normal",
      "notify": null
    
  },
  {
      "_id": ObjectId("575d73d708e0e1db5cbafdf3"),
      "type_member": "normal",
      "notify": null
    
  },
  {
      "_id": ObjectId("575d73d708e0e1db5cbafdf4"),
      "type_member": "normal",
      "notify": null
    
  },
  {
      "_id": ObjectId("575d73d708e0e1db5cbafdf9"),
      "type_member": "normal",
      "notify": null
    
  },
  {
      "_id": ObjectId("575d73d708e0e1db5cbafdfa"),
      "type_member": "normal",
      "notify": null
    
  }
  
  ],
  "goals": [
  {
      "name": "End game",
      "description": "Nao tem mais nada pra fazer",
	  "tags": [
        "finish",
        "game end",
        "game over"
      
	  ],
      "activities": [  ]
    
  }
  
  ]

}
Fetched 2 record(s) in 2ms
corsair(mongod-3.2.7) projeto-final> 
corsair(mongod-3.2.7) projeto-final> db.project.find({'tags':{$nin:['Rumo a elite 4']}})
{
  "_id": ObjectId("575d93ea08e0e1db5cbafe04"),
  "name": "Vencer a elite 4",
  "description": " ganhar dos melhors mestres pokemons ",
  "tags": [
    "Vencer",
    "Best",
    "Melhor"
  
  ],
  "members": [
  {
      "_id": ObjectId("575d73d708e0e1db5cbafdf4"),
      "type_member": "normal",
      "notify": null
    
  },
  {
      "_id": ObjectId("575d73d708e0e1db5cbafdf5"),
      "type_member": "normal",
      "notify": null
    
  },
  {
      "_id": ObjectId("575d73d708e0e1db5cbafdf6"),
      "type_member": "normal",
      "notify": null
    
  },
  {
      "_id": ObjectId("575d73d708e0e1db5cbafdf7"),
      "type_member": "normal",
      "notify": null
    
  },
  {
      "_id": ObjectId("575d73d708e0e1db5cbafdf8"),
      "type_member": "normal",
      "notify": null
    
  }
  
  ],
  "goals": [
  {
      "name": "Elite 4 winner",
      "description": "Ultimo desafio",
	  "tags": [
        "Winner",
        "Campeao",
        "monster"
      
	  ],
	  "activities": [
	  {
          "_id": ObjectId("575d8ea408e0e1db5cbafdfd")
        
	  },
	  {
          "_id": ObjectId("575d8ea408e0e1db5cbafdfe")
        
	  }
      
	  ]
    
  }
  
  ]

}
{
  "_id": ObjectId("575d93ea08e0e1db5cbafe05"),
  "name": "Fim de jogo",
  "description": "Sem mais desafios",
  "tags": [
    "Vencer",
    "finish",
    "end"
  
  ],
  "members": [
  {
      "_id": ObjectId("575d73d708e0e1db5cbafdf2"),
      "type_member": "normal",
      "notify": null
    
  },
  {
      "_id": ObjectId("575d73d708e0e1db5cbafdf3"),
      "type_member": "normal",
      "notify": null
    
  },
  {
      "_id": ObjectId("575d73d708e0e1db5cbafdf4"),
      "type_member": "normal",
      "notify": null
    
  },
  {
      "_id": ObjectId("575d73d708e0e1db5cbafdf9"),
      "type_member": "normal",
      "notify": null
    
  },
  {
      "_id": ObjectId("575d73d708e0e1db5cbafdfa"),
      "type_member": "normal",
      "notify": null
    
  }
  
  ],
  "goals": [
  {
      "name": "End game",
      "description": "Nao tem mais nada pra fazer",
	  "tags": [
        "finish",
        "game end",
        "game over"
      
	  ],
      "activities": [  ]
    
  }
  
  ]

}
Fetched 2 record(s) in 40ms


```
#### Liste todos os usuários que não fazem parte de um projeto cadastrado.
```
ir(mongod-3.2.7) projeto-final> var membros = db.project.findOne({},{members:1})
corsair(mongod-3.2.7) projeto-final> var remove = []
corsair(mongod-3.2.7) projeto-final> membros.members.forEach(function(member){remove.push({"_id":ObjectId(member._id)});});
corsair(mongod-3.2.7) projeto-final> db.users.find({$nor:remove})
{
  "_id": ObjectId("575d73d708e0e1db5cbafdf6"),
  "name": "kukui",
  "bio": null,
  "date_register": "2015-05-11",
  "avatar_path": null,
  "background_path": null,
  "auth": {
    "username": "kukuipokemon",
    "email": "kukui@pokemons.com",
    "password": "123456789",
    "last_access": "2016-06-12",
    "status": "offline"
  
  }

}
{
  "_id": ObjectId("575d73d708e0e1db5cbafdf7"),
  "name": "carvalho",
  "bio": null,
  "date_register": "2015-05-11",
  "avatar_path": null,
  "background_path": null,
  "auth": {
    "username": "carvalhopokemon",
    "email": "carvalho@pokemons.com",
    "password": "123456789",
    "last_access": "2016-06-12",
    "status": "offline"
  
  }

}
{
  "_id": ObjectId("575d73d708e0e1db5cbafdf8"),
  "name": "oak",
  "bio": null,
  "date_register": "2015-05-11",
  "avatar_path": null,
  "background_path": null,
  "auth": {
    "username": "oakpokemon",
    "email": "oak@pokemons.com",
    "password": "123456789",
    "last_access": "2016-06-12",
    "status": "offline"
  
  }

}
{
  "_id": ObjectId("575d73d708e0e1db5cbafdf9"),
  "name": "gray",
  "bio": null,
  "date_register": "2015-05-11",
  "avatar_path": null,
  "background_path": null,
  "auth": {
    "username": "graypokemon",
    "email": "gray@pokemons.com",
    "password": "123456789",
    "last_access": "2016-06-12",
    "status": "offline"
  
  }

}
{
  "_id": ObjectId("575d73d708e0e1db5cbafdfa"),
  "name": "luffy",
  "bio": null,
  "date_register": "2015-05-11",
  "avatar_path": null,
  "background_path": null,
  "auth": {
    "username": "luffypokemon",
    "email": "luffy@pokemons.com",
    "password": "123456789",
    "last_access": "2016-06-12",
    "status": "offline"
  
  }

}
Fetched 5 record(s) in 14ms

```
## Update - alteração
##### Adicione para todos os projetos o campo views: 0
```
var mod = { $set: { views: 0  }  }
var option = { multi: true  }
db.project.update({}, mod, option)
WriteResult({ "nMatched" : 17, "nUpserted" : 0, "nModified" : 17  })
```
##### Adicione 1 tag diferente para cada projeto
```
corsair(mongod-3.2.7) projeto-final>  var projeto = db.project.find({}, {_id: 1})
corsair(mongod-3.2.7) projeto-final>  var contador = db.project.count()
	corsair(mongod-3.2.7) projeto-final>  for(var i=0; i< contador; i++) {
...  var query = { _id: projeto[i]._id  }
...  var mod = { $push: { tags: 'Tag '+(i+1)  }  }
...      db.project.update(query, mod)
...  
	}
Updated 1 existing record(s) in 106ms
Updated 1 existing record(s) in 1ms
Updated 1 existing record(s) in 7ms
Updated 1 existing record(s) in 0ms
Updated 1 existing record(s) in 1ms
WriteResult({
  "nMatched": 1,
  "nUpserted": 0,
  "nModified": 1

		})
)

```
##### Adicione 2 membros diferentes para cada projeto
```
var userId = db.users.find({}, {_id:1})
var ids = [];
userId.forEach(function(id){ ids.push(db.users.findOne(id._id)) }) 

var idProj = db.project.find({}, {_id: 1})
var cont = db.project.count()

var mod = []
for(var i=0; i< 10; i++) {
    mod.push([ {user_id: ids[i]}, {user_id: ids[i+1]}  ]);

}
for(var i=0; i< cont; i++) {
    var query = { _id: idProj[i]._id  }
    var mod = { $pushAll: { members: mod[i]  }  }
    db.project.update(query, mod)

}))

```
##### Adicione 1 comentário em cada atividade, deixe apenas 1 projeto sem
```
var userId = db.users.find({}, {_id:1})
var ids = [];
userId.forEach(function(id){ ids.push(db.users.findOne(id,{_id:1})).toArray }) 
var idActivity = db.activity.find({}, {_id: 1})
var cont = db.activity.count()
	for(var i=0; i< cont-1; i++) {
    var query = { _id: idActivity[i]._id  }
    var mod = { $push: { comments: { user_id:ids[i], text: 'Pokemon '+(i+1)  }  }  }
    db.activity.remove(query, mod)

	})

```
##### Adicione 1 projeto inteiro com UPSERT
```
var query = {name: "Pokemons moon"}
var option = {upsert: true}
mod = {
	$setOnInsert:{
        name: "Na região de alola",
        description: "Baseado no havai ",
		members: [
            { user_id: ObjectId("575d73d708e0e1db5cbafdf6"), type_member: "Professor da região", notify: null  },
            { user_id: ObjectId("575d73d708e0e1db5cbafdf2"), type_member: "Personagem principal", notify: null  },
        
		],
		goals: [
		{
                name:"Nova jornada",
                description: "Onde tudo começa",
                tags:['Rumo ao topo', 'New island', 'Gratidão']
            
		}
        
		]
    
	}

}
db.project.update(query, mod, option)

```
## Delete - remoção
##### Apague todos os projetos que não possuam tags
```
db.projects.remove({tags:[]})

```
##### Apague todos os projetos que não possuam comentários nas atividades
```
  var activities = db.activity.find({comments:[]}, {"_id":1}).toArray();
  var values = [];

  activities.forEach(function(act){
    db.activity.remove({"_id":act._id});
    values.push(act._id.valueOf());
  
		  });

db.project.remove({
    'goals.activities': {$in:values}
  
		});
```
##### Apague todos os projetos que não possuam atividades
```
corsair(mongod-3.2.7) projeto-final> db.project.remove({     'goals.activities': []    });
Removed 1 record(s) in 2ms
WriteResult({
  "nRemoved": 1
})

```
##### Escolha 2 usuário e apague todos os projetos em que os 2 fazem parte
```
corsair(mongod-3.2.7) projeto-final> var users = [
...     {"_id": ObjectId("575d73d708e0e1db5cbafdf9")},
...     {"_id": ObjectId("575d73d708e0e1db5cbafdf3")}
... 
]
corsair(mongod-3.2.7) projeto-final> users.forEach(function(x){ db.project.remove({'members.user_id': x._id})  })

```
##### Apague todos os projetos que possuam uma determinada tag em goal
```
corsair(mongod-3.2.7) projeto-final> db.project.remove({
...     'goals.tags': {$nin:['Rumo a elite 4']}
...   
		});
Removed 5 record(s) in 24ms
WriteResult({
  "nRemoved": 5

		})

```

## Gerenciamento de usuários
##### Crie um usuário com permissões **APENAS** de Leitura
```
db.runCommand({
    createUser:"Miojo",
    pwd:"miojo",
    roles:[{role:"read", db:"projeto-final"}]
  
		});
```
##### Crie um usuário com permissões de Escrita e Leitura
```
db.runCommand({
    createUser:"MiojoRoot",
    pwd:"root",
    roles:[{role:"readWrite", db:"projeto-final"}]
  
		});

```
##### Adicionar o papel grantRolesToUser e revokeRole para o usuário com Escrita e Leitura
```
  db.runCommand({ createRole: "grantRolesToUser",
    privileges: [{resource: { db: "projeto-final", collection: ""  }, actions: ["grantRole"]}],
    roles: [],
    writeConcern: { w: "majority" , wtimeout: 5000  }
   });

  db.runCommand({ createRole: "revokeRole",
    privileges: [{resource: { db: "projeto-final", collection: ""  }, actions: ["revokeRole"]},],
    roles: [],
    writeConcern: { w: "majority" , wtimeout: 5000  }
   });

db.runCommand({
    grantRolesToUser: 'MiojoRoot',
	roles:[
        {role:"grantRolesToUser", db:"projeto-final"},
        {role:"revokeRole", db:"projeto-final"}
    
	],
    writeConcern:{w:"majority", wtimeout:5000}
  
		});  

```
##### Remover o papel grantRolesToUser para o usuário com Escrita e Leitura
```
db.revokeRolesFromUser(
    "MiojoRoot",
    [ "grantRolesToUser"  ]

		)
```
##### Listar todos os usuários com seus papéis e ações
```
corsair(mongod-3.2.7) projeto-final>   db.runCommand({
		...     usersInfo:[
...         {user: 'Miojo', db:'projeto-final'},
...         {user: 'MiojoRoot', db:'projeto-final'}
...     
		],
...     showCredentials:true,
...     showPrivileges:true
...   
		});
{
	"users": [
	{
      "_id": "projeto-final.Miojo",
      "user": "Miojo",
      "db": "projeto-final",
	  "credentials": {
		  "SCRAM-SHA-1": {
          "iterationCount": 10000,
          "salt": "iaXA66NOsgCW9cYoWdWqcQ==",
          "storedKey": "R0rwXsR07WfNmBAmV1VYNTOPYdw=",
          "serverKey": "LrjT8TF6kwFSQInk/utJBC9ckJ8="
        
		  }
      
	  },
	  "roles": [
	  {
          "role": "read",
          "db": "projeto-final"
        
	  }
      
	  ],
	  "inheritedRoles": [
	  {
          "role": "read",
          "db": "projeto-final"
        
	  }
      
	  ],
	  "inheritedPrivileges": [
	  {
		  "resource": {
            "db": "projeto-final",
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
            "db": "projeto-final",
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
            "db": "projeto-final",
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
            "db": "projeto-final",
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
      "_id": "projeto-final.MiojoRoot",
      "user": "MiojoRoot",
      "db": "projeto-final",
	  "credentials": {
		  "SCRAM-SHA-1": {
          "iterationCount": 10000,
          "salt": "cg90a2LXYQ/9RdhLE8176w==",
          "storedKey": "QhzhbbxhnFqxF1LpgqqzKysf3qs=",
          "serverKey": "Ajpha41itI3TQLC4iVjXq8Zod/Q="
        
		  }
      
	  },
	  "roles": [
	  {
          "role": "revokeRole",
          "db": "projeto-final"
        
	  },
	  {
          "role": "readWrite",
          "db": "projeto-final"
        
	  }
      
	  ],
	  "inheritedRoles": [
	  {
          "role": "readWrite",
          "db": "projeto-final"
        
	  },
	  {
          "role": "revokeRole",
          "db": "projeto-final"
        
	  }
      
	  ],
	  "inheritedPrivileges": [
	  {
		  "resource": {
            "db": "projeto-final",
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
            "revokeRole",
            "update"
          
		  ]
        
	  },
	  {
		  "resource": {
            "db": "projeto-final",
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
            "db": "projeto-final",
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
            "db": "projeto-final",
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
corsair(mongod-3.2.7) projeto-final> 

```
## Sharding
```
**Server**

mkdir /data/config 
mongod --configsvr --port 27010

**Router**

mongos --configdb localhost:27010 --port 27011

**Shardings**

mongod --port 27012 --dbpath /data/shard1
mongod --port 27013 --dbpath /data/shard2
mongod --port 27014 --dbpath /data/shard3


**Conectando e adicionando**

mongo --port 27011 --host localhost

mongos> sh.addShard("localhost:27012")
mongos> sh.addShard("localhost:27013")
mongos> sh.addShard("localhost:27014")

mongos> sh.enableSharding("projeto-final")
mongos> sh.shardCollection("projeto-final.project", {"_id" : 1})
mongos> sh.shardCollection("projeto-final.users", {"_id" : 1})
mongos> sh.shardCollection("projeto-final.activity", {"_id" : 1})
```
## Réplica
```
**Subindo as replicas**

mongod --replSet replica_set --port 27017 --dbpath /data/rs1
mongod --replSet replica_set --port 27018 --dbpath /data/rs2
mongod --replSet replica_set --port 27019 --dbpath /data/rs3

**Conectando na réplica primaria**

mongo --port 27017

mongo> rsconf =
{
  "_id": "replica_set",
  "members": [
  {
      "_id": 0,
      "host": "127.0.0.1:27017"
    
  }
  
  ]

}
rs.initiate(rsconf)
rs.add("127.0.0.1:27018")
rs.add("127.0.0.1:27019")
```
