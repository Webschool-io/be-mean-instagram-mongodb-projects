# Projeto Final
**Autor:** Marcos Antonio Martinez Florentin

**Data:** 1456489239224

## Para qual sistema você usaria o MogoDB (diferente desse)?
* Ecommerce
* Chat
* CMS
* Fórum
* EPR
* Game
* Portais

## Qual a modelagem da sua coleção de `users`?
```JS

user :{
  name: String,
  bio:  String,
  date-register: Date,
  avatar_path:String,
  background_path: String,

  auth :{
    username: String,
    email: String,
    password: Password,
    last-access:Date,
    online:Boolean,
    status : Boolean,
    hash_token: String
  }
}
```

## Qual a modelagem da sua coleção de `projects`?
```JS

project :{
  name: String,
  description:  String,
  date-begin: Date,
  date-dream: Date,
  date-end: Date,
  visible: Boolean,
  realocate:Boolean,
  expired:Boolean,
  visualizable_mod:Boolean,
  tags:[],
  members: [
    {
      type_member: String,
      user_id:ObjectID,
      notify:Boolean
    }
  ],
  goals: [
    {
      name: String,
      description: String,
      date_begin: Date,
      date_dream: Date,
      date_end: Date,
      realocate:Boolean,
      expired:Boolean,
      tags:[],
      historic:[],
      activities: []
    }
  ]
}
```
## Qual a modelagem da sua coleção retirada de `projects`?

```JS
activity:{
  name:String,
  description: String,
  date_begin: Date,
  date_dream: Date,
  date_end: Date,
  realocate: Boolean,
  expired: Boolean,
  tag:[],
  members:[
    {
      user_id:String,
      notify: Boolean,      
    }
  ],
  comments:[{
    text:String,
    date_comment:Date,
    members:[{
      user_id:String,
      notify: Boolean
    }],
    files:[{
      path:String,
      weight:String,
      name:String
    }]

  }],
  historic:[{
    date_realocate: Date
  }]



}

```
## Create - cadastro

#### 1. Cadastre 10 usuários diferentes.

```JS

> var rand = function() {
...     return Math.random().toString(36).substr(2);
... }
> var token = function() {
...     return rand() + rand();
... }
> var usuario = ["Marcos", "jose", "Mauricio", "Santiago", "Suissa", "jhon", "Thiago", "Rodrigo", "Antonio", "Carlos"];
> var newUsuarios = [];
> usuario.forEach(function(user){
...   var datos = {
...     name: user,
...     bio: 'Datos',
...     date: new Date(),
...     avatar: null,
...     background:null,
...     auth:{
...       username: user+'_user',
...       email: user+'@.gmail.com',
...       password:'123',
...       online: Math.random()* 10  >= 2,
...       disabled: Math.random()* 10  >= 2,
...       hash_token: token()
...
...     }
...   }
...   newUsuarios.push(datos)
...
... })
> db.usuarios.insert(newUsuarios)
BulkWriteResult({
	"writeErrors" : [ ],
	"writeConcernErrors" : [ ],
	"nInserted" : 10,
	"nUpserted" : 0,
	"nMatched" : 0,
	"nModified" : 0,
	"nRemoved" : 0,
	"upserted" : [ ]
})

```
```JS
var activity={
  name:'activity3',
  description: 'String',
  date_begin: new Date(),
  date_dream: new Date(),
  date_end: new Date(),
  realocate: true,
  expired: true,
  tag:[],
  members:[
    {
      user_id: {"id" : ObjectId("56a2fff8a5ef125f78c1d20b")},
      notify: true,      
    }
  ],
  comments:[{
    text:'String',
    date_comment:new Date(),
    members:[{
      user_id: {"id" : ObjectId("56a2fff8a5ef125f78c1d20b")},
      notify: true
    }],
    files:[{
      path:'String',
      weight:'3',
      name:'mark'
    }]

  }],
  historic:[{
    date_realocate: new Date()
  }]
}
```
#### 2. Cadastre 5 projetos diferentes.
- cada um com 5 membros, sempre diferentes dentro dos projetos;
- cada um com pelo menos 3 tags diferentes;
    - escolha 1 *tag* onde deva ficar em 2 projetos;
    - escolha 1 *tag* onde deva ficar em 3 projetos;
- cada projeto com pelo menos 1 *goal*;
    - cada *goal* com pelo menos 3 *tags*;
    - cada *goal* com pelo menos 2 atividades, deixe 1 projeto sem.

```JS

var project = db.usuarios.find({},{"_id":true}).toArray()
var activity = db.activity.find({},{"_id":true}).toArray()

db.project.insert([{
    name : 'project_numero_1',
    description : 'project_numero_1',
    date_begin: new Date(),
    date_dream: new Date(),
    date_end: new Date(),
    visible:true,
    realocate:true,
    expired: true,
    visualizable_mod:true,
    tags:['angularjs', 'css', 'html'],
    members: [
      {
        type_member: 'admin',
        user_id:project[0],
        notify: Math.random()* 10  >= 2
      },
      {
        type_member: 'moderador',
        user_id:project[1],
        notify: Math.random()* 10  >= 2
      },
      {
        type_member: 'moderador',
        user_id:project[2],
        notify: Math.random()* 10  >= 2
      },
      {
        type_member: 'moderador',
        user_id:project[3],
        notify: Math.random()* 10  >= 2
      },
      {
        type_member: 'admin',
        user_id:project[4],
        notify: Math.random()* 10  >= 2
      }
    ],
    goals: [
      {
        name: 'Goals',
        description: 'description',
        date_begin: new Date(),
        date_dream: new Date(),
        date_end:  new Date(),
        realocate:Math.random()* 10  >= 2,
        expired:Math.random()* 10  >= 2,
        tags:["Scope", "Function", "JS"],
        historic:[],
        activities: [
          {activity_id: activity[1]},
          {activity_id: activity[0]}
        ],
      }
    ]

  },
  {
      name : 'project_numero_2',
      description : 'project_numero_1',
      date_begin: new Date(),
      date_dream: new Date(),
      date_end: new Date(),
      visible:true,
      realocate:true,
      expired: true,
      visualizable_mod:true,
      tags:['angularjs', 'css', 'react'],
      members: [
        {
          type_member: 'admin',
          user_id:project[0],
          notify: Math.random()* 10  >= 2
        },
        {
          type_member: 'moderador',
          user_id:project[1],
          notify: Math.random()* 10  >= 2
        },
        {
          type_member: 'moderador',
          user_id:project[2],
          notify: Math.random()* 10  >= 2
        },
        {
          type_member: 'moderador',
          user_id:project[3],
          notify: Math.random()* 10  >= 2
        },
        {
          type_member: 'admin',
          user_id:project[4],
          notify: Math.random()* 10  >= 2
        }
      ],
      goals: [
        {
          name: 'Goals',
          description: 'description',
          date_begin: new Date(),
          date_dream: new Date(),
          date_end:  new Date(),
          realocate:Math.random()* 10  >= 2,
          expired:Math.random()* 10  >= 2,
          tags:["Scope", "Function", "JS"],
          historic:[],
          activities: [
            {activity_id: activity[0]},
            {activity_id: activity[2]}
          ],
        }
      ]

    },
    {
        name : 'project_numero_3',
        description : 'project_numero_1',
        date_begin: new Date(),
        date_dream: new Date(),
        date_end: new Date(),
        visible:true,
        realocate:true,
        expired: true,
        visualizable_mod:true,
        tags:['angularjs', 'Socket', 'html'],
        members: [
          {
            type_member: 'admin',
            user_id:project[5],
            notify: Math.random()* 10  >= 2
          },
          {
            type_member: 'moderador',
            user_id:project[6],
            notify: Math.random()* 10  >= 2
          },
          {
            type_member: 'moderador',
            user_id:project[7],
            notify: Math.random()* 10  >= 2
          },
          {
            type_member: 'moderador',
            user_id:project[8],
            notify: Math.random()* 10  >= 2
          },
          {
            type_member: 'admin',
            user_id:project[9],
            notify: Math.random()* 10  >= 2
          }
        ],
        goals: [
          {
            name: 'Goals',
            description: 'description',
            date_begin: new Date(),
            date_dream: new Date(),
            date_end:  new Date(),
            realocate:Math.random()* 10  >= 2,
            expired:Math.random()* 10  >= 2,
            tags:["Scope", "Function", "JS"],
            historic:[],
            activities: [
              {activity_id: activity[1]},
              {activity_id: activity[2]}
            ],
          }
        ]

      },
      {
          name : 'project_numero_4',
          description : 'project_numero_1',
          date_begin: new Date(),
          date_dream: new Date(),
          date_end: new Date(),
          visible:true,
          realocate:true,
          expired: true,
          visualizable_mod:true,
          tags:['Socket', 'mongo', 'react'],
          members: [
            {
              type_member: 'admin',
              user_id:project[0],
              notify: Math.random()* 10  >= 2
            },
            {
              type_member: 'moderador',
              user_id:project[1],
              notify: Math.random()* 10  >= 2
            },
            {
              type_member: 'moderador',
              user_id:project[2],
              notify: Math.random()* 10  >= 2
            },
            {
              type_member: 'moderador',
              user_id:project[3],
              notify: Math.random()* 10  >= 2
            },
            {
              type_member: 'admin',
              user_id:project[4],
              notify: Math.random()* 10  >= 2
            }
          ],
          goals: [
            {
              name: 'Goals',
              description: 'description',
              date_begin: new Date(),
              date_dream: new Date(),
              date_end:  new Date(),
              realocate:Math.random()* 10  >= 2,
              expired:Math.random()* 10  >= 2,
              tags:["Scope", "tutorial", "JS"],
              historic:[],
              activities: [],
            }
          ]

        },
        {
            name : 'project_numero_5',
            description : 'project_numero_1',
            date_begin: new Date(),
            date_dream: new Date(),
            date_end: new Date(),
            visible:true,
            realocate:true,
            expired: true,
            visualizable_mod:true,
            tags:['Socket', 'mongo', 'react'],
            members: [
              {
                type_member: 'admin',
                user_id:project[6],
                notify: Math.random()* 10  >= 2
              },
              {
                type_member: 'moderador',
                user_id:project[7],
                notify: Math.random()* 10  >= 2
              },
              {
                type_member: 'moderador',
                user_id:project[8],
                notify: Math.random()* 10  >= 2
              },
              {
                type_member: 'moderador',
                user_id:project[9],
                notify: Math.random()* 10  >= 2
              },
              {
                type_member: 'admin',
                user_id:project[5],
                notify: Math.random()* 10  >= 2
              }
            ],
            goals: [
              {
                name: 'Goals',
                description: 'description',
                date_begin: new Date(),
                date_dream: new Date(),
                date_end:  new Date(),
                realocate:Math.random()* 10  >= 2,
                expired:Math.random()* 10  >= 2,
                tags:["Scope", "Function", "JS"],
                historic:[],
                activities: [],
              }
            ]

          }
])
BulkWriteResult({
	"writeErrors" : [ ],
	"writeConcernErrors" : [ ],
	"nInserted" : 5,
	"nUpserted" : 0,
	"nMatched" : 0,
	"nModified" : 0,
	"nRemoved" : 0,
	"upserted" : [ ]
})
```
## Retrieve - busca

#### 1. Liste as informações dos membros de 1 projeto específico que deve ser buscado pelo seu nome de forma a não ligar para maiúsculas e minúsculas.
```JS
> var projects = db.project.findOne({name: /project_numero_5/i})
> projects
{
	"_id" : ObjectId("56a95777793a0104e87d743a"),
	"name" : "project_numero_5",
	"description" : "project_numero_1",
	"date_begin" : ISODate("2016-01-27T23:49:11.662Z"),
	"date_dream" : ISODate("2016-01-27T23:49:11.662Z"),
	"date_end" : ISODate("2016-01-27T23:49:11.662Z"),
	"visible" : true,
	"realocate" : true,
	"expired" : true,
	"visualizable_mod" : true,
	"tags" : [
		"Socket",
		"mongo",
		"react"
	],
	"members" : [
		{
			"type_member" : "admin",
			"user_id" : {
				"_id" : ObjectId("56a2fff8a5ef125f78c1d20c")
			},
			"notify" : true
		},
		{
			"type_member" : "moderador",
			"user_id" : {
				"_id" : ObjectId("56a2fff8a5ef125f78c1d20d")
			},
			"notify" : true
		},
		{
			"type_member" : "moderador",
			"user_id" : {
				"_id" : ObjectId("56a2fff8a5ef125f78c1d20e")
			},
			"notify" : true
		},
		{
			"type_member" : "moderador",
			"user_id" : {
				"_id" : ObjectId("56a2fff8a5ef125f78c1d20f")
			},
			"notify" : true
		},
		{
			"type_member" : "admin",
			"user_id" : {
				"_id" : ObjectId("56a2fff8a5ef125f78c1d20b")
			},
			"notify" : false
		}
	],
	"goals" : [
		{
			"name" : "Goals",
			"description" : "description",
			"date_begin" : ISODate("2016-01-27T23:49:11.662Z"),
			"date_dream" : ISODate("2016-01-27T23:49:11.662Z"),
			"date_end" : ISODate("2016-01-27T23:49:11.662Z"),
			"realocate" : true,
			"expired" : false,
			"tags" : [
				"Scope",
				"Function",
				"JS"
			],
			"historic" : [ ],
			"activities" : [ ]
		}
	]
}
```

#### 2 Liste todos os projetos com a tag que você escolheu para os 3 projetos em comum.

```js
db.project.find({tags: {$eq: 'Socket'}}).pretty()
{
 "_id" : ObjectId("56a95777793a0104e87d7438"),
 "name" : "project_numero_3",
 "description" : "project_numero_1",
 "date_begin" : ISODate("2016-01-27T23:49:11.662Z"),
 "date_dream" : ISODate("2016-01-27T23:49:11.662Z"),
 "date_end" : ISODate("2016-01-27T23:49:11.662Z"),
 "visible" : true,
 "realocate" : true,
 "expired" : true,
 "visualizable_mod" : true,
 "tags" : [
   "angularjs",
   "Socket",
   "html"
 ],
 "members" : [
   {
     "type_member" : "admin",
     "user_id" : {
       "_id" : ObjectId("56a2fff8a5ef125f78c1d20b")
     },
     "notify" : true
   },
   {
     "type_member" : "moderador",
     "user_id" : {
       "_id" : ObjectId("56a2fff8a5ef125f78c1d20c")
     },
     "notify" : false
   },
   {
     "type_member" : "moderador",
     "user_id" : {
       "_id" : ObjectId("56a2fff8a5ef125f78c1d20d")
     },
     "notify" : false
   },
   {
     "type_member" : "moderador",
     "user_id" : {
       "_id" : ObjectId("56a2fff8a5ef125f78c1d20e")
     },
     "notify" : true
   },
   {
     "type_member" : "admin",
     "user_id" : {
       "_id" : ObjectId("56a2fff8a5ef125f78c1d20f")
     },
     "notify" : true
   }
 ],
 "goals" : [
   {
     "name" : "Goals",
     "description" : "description",
     "date_begin" : ISODate("2016-01-27T23:49:11.662Z"),
     "date_dream" : ISODate("2016-01-27T23:49:11.662Z"),
     "date_end" : ISODate("2016-01-27T23:49:11.662Z"),
     "realocate" : false,
     "expired" : true,
     "tags" : [
       "Scope",
       "Function",
       "JS"
     ],
     "historic" : [ ],
     "activities" : [
       {
         "activity_id" : {
           "_id" : ObjectId("56a816168505305cb66d7c95")
         }
       },
       {
         "activity_id" : {
           "_id" : ObjectId("56a94193793a0104e87d7435")
         }
       }
     ]
   }
 ]
}
{
 "_id" : ObjectId("56a95777793a0104e87d7439"),
 "name" : "project_numero_4",
 "description" : "project_numero_1",
 "date_begin" : ISODate("2016-01-27T23:49:11.662Z"),
 "date_dream" : ISODate("2016-01-27T23:49:11.662Z"),
 "date_end" : ISODate("2016-01-27T23:49:11.662Z"),
 "visible" : true,
 "realocate" : true,
 "expired" : true,
 "visualizable_mod" : true,
 "tags" : [
   "Socket",
   "mongo",
   "react"
 ],
 "members" : [
   {
     "type_member" : "admin",
     "user_id" : {
       "_id" : ObjectId("56a2fff8a5ef125f78c1d206")
     },
     "notify" : false
   },
   {
     "type_member" : "moderador",
     "user_id" : {
       "_id" : ObjectId("56a2fff8a5ef125f78c1d207")
     },
     "notify" : true
   },
   {
     "type_member" : "moderador",
     "user_id" : {
       "_id" : ObjectId("56a2fff8a5ef125f78c1d208")
     },
     "notify" : true
   },
   {
     "type_member" : "moderador",
     "user_id" : {
       "_id" : ObjectId("56a2fff8a5ef125f78c1d209")
     },
     "notify" : false
   },
   {
     "type_member" : "admin",
     "user_id" : {
       "_id" : ObjectId("56a2fff8a5ef125f78c1d20a")
     },
     "notify" : true
   }
 ],
 "goals" : [
   {
     "name" : "Goals",
     "description" : "description",
     "date_begin" : ISODate("2016-01-27T23:49:11.662Z"),
     "date_dream" : ISODate("2016-01-27T23:49:11.662Z"),
     "date_end" : ISODate("2016-01-27T23:49:11.662Z"),
     "realocate" : true,
     "expired" : true,
     "tags" : [
       "Scope",
       "tutorial",
       "JS"
     ],
     "historic" : [ ],
     "activities" : [ ]
   }
 ]
}
{
 "_id" : ObjectId("56a95777793a0104e87d743a"),
 "name" : "project_numero_5",
 "description" : "project_numero_1",
 "date_begin" : ISODate("2016-01-27T23:49:11.662Z"),
 "date_dream" : ISODate("2016-01-27T23:49:11.662Z"),
 "date_end" : ISODate("2016-01-27T23:49:11.662Z"),
 "visible" : true,
 "realocate" : true,
 "expired" : true,
 "visualizable_mod" : true,
 "tags" : [
   "Socket",
   "mongo",
   "react"
 ],
 "members" : [
   {
     "type_member" : "admin",
     "user_id" : {
       "_id" : ObjectId("56a2fff8a5ef125f78c1d20c")
     },
     "notify" : true
   },
   {
     "type_member" : "moderador",
     "user_id" : {
       "_id" : ObjectId("56a2fff8a5ef125f78c1d20d")
     },
     "notify" : true
   },
   {
     "type_member" : "moderador",
     "user_id" : {
       "_id" : ObjectId("56a2fff8a5ef125f78c1d20e")
     },
     "notify" : true
   },
   {
     "type_member" : "moderador",
     "user_id" : {
       "_id" : ObjectId("56a2fff8a5ef125f78c1d20f")
     },
     "notify" : true
   },
   {
     "type_member" : "admin",
     "user_id" : {
       "_id" : ObjectId("56a2fff8a5ef125f78c1d20b")
     },
     "notify" : false
   }
 ],
 "goals" : [
   {
     "name" : "Goals",
     "description" : "description",
     "date_begin" : ISODate("2016-01-27T23:49:11.662Z"),
     "date_dream" : ISODate("2016-01-27T23:49:11.662Z"),
     "date_end" : ISODate("2016-01-27T23:49:11.662Z"),
     "realocate" : true,
     "expired" : false,
     "tags" : [
       "Scope",
       "Function",
       "JS"
     ],
     "historic" : [ ],
     "activities" : [ ]
   }
 ]
}
>
```

#### 3 Liste apenas os nomes de todas as atividades para todos os projetos.

```js



  db.project.find({},{"goals.activities.activity_id._id":1, "name":1}).toArray()

  [
  	{
  		"_id" : ObjectId("56a95777793a0104e87d7436"),
  		"name" : "project_numero_1",
  		"goals" : [
  			{
  				"activities" : [
  					{
  						"activity_id" : {
  							"_id" : ObjectId("56a816168505305cb66d7c95")
  						}
  					},
  					{
  						"activity_id" : {
  							"_id" : ObjectId("56a815528505305cb66d7c94")
  						}
  					}
  				]
  			}
  		]
  	},
  	{
  		"_id" : ObjectId("56a95777793a0104e87d7437"),
  		"name" : "project_numero_2",
  		"goals" : [
  			{
  				"activities" : [
  					{
  						"activity_id" : {
  							"_id" : ObjectId("56a815528505305cb66d7c94")
  						}
  					},
  					{
  						"activity_id" : {
  							"_id" : ObjectId("56a94193793a0104e87d7435")
  						}
  					}
  				]
  			}
  		]
  	},
  	{
  		"_id" : ObjectId("56a95777793a0104e87d7438"),
  		"name" : "project_numero_3",
  		"goals" : [
  			{
  				"activities" : [
  					{
  						"activity_id" : {
  							"_id" : ObjectId("56a816168505305cb66d7c95")
  						}
  					},
  					{
  						"activity_id" : {
  							"_id" : ObjectId("56a94193793a0104e87d7435")
  						}
  					}
  				]
  			}
  		]
  	},
  	{
  		"_id" : ObjectId("56a95777793a0104e87d7439"),
  		"name" : "project_numero_4",
  		"goals" : [
  			{
  				"activities" : [ ]
  			}
  		]
  	},
  	{
  		"_id" : ObjectId("56a95777793a0104e87d743a"),
  		"name" : "project_numero_5",
  		"goals" : [
  			{
  				"activities" : [ ]
  			}
  		]
  	}
  ]
  >

})

```
#### 4 Liste todos os projetos que não possuam uma tag.

```JS
//todos os projetos tinha que ter tags por isso me retorna array basio
var query = {tags : []}
> var projecto = {name:1}
> db.project.find(query, projecto)
>

```

#### 5 Liste todos os usuários que não fazem parte do primeiro projeto cadastrado.

```JS
>var user = db.project.findOne({"name":"project_numero_1"},{"members.user_id": true})
>var userN = user.members.map(function(obj){ return obj.user_id._id })
> db.usuarios.find({"_id":{$nin:userN}}).pretty()
{
	"_id" : ObjectId("56a2fff8a5ef125f78c1d20b"),
	"name" : "jhon",
	"bio" : "Datos",
	"date" : ISODate("2016-01-23T04:22:11.587Z"),
	"avatar" : null,
	"background" : null,
	"auth" : {
		"username" : "jhon_user",
		"email" : "jhon@.gmail.com",
		"password" : "123",
		"online" : true,
		"disabled" : true,
		"hash_token" : "h4ls7n5lehlg14i5q5n33m2g4gta9k9"
	}
}
{
	"_id" : ObjectId("56a2fff8a5ef125f78c1d20c"),
	"name" : "Thiago",
	"bio" : "Datos",
	"date" : ISODate("2016-01-23T04:22:11.587Z"),
	"avatar" : null,
	"background" : null,
	"auth" : {
		"username" : "Thiago_user",
		"email" : "Thiago@.gmail.com",
		"password" : "123",
		"online" : true,
		"disabled" : true,
		"hash_token" : "5byvql275vpr2j4ip9xqxvrq9tlqh0k9"
	}
}
{
	"_id" : ObjectId("56a2fff8a5ef125f78c1d20d"),
	"name" : "Rodrigo",
	"bio" : "Datos",
	"date" : ISODate("2016-01-23T04:22:11.587Z"),
	"avatar" : null,
	"background" : null,
	"auth" : {
		"username" : "Rodrigo_user",
		"email" : "Rodrigo@.gmail.com",
		"password" : "123",
		"online" : true,
		"disabled" : true,
		"hash_token" : "myxjee735vdndn29v3kfgfvys24kj4i"
	}
}
{
	"_id" : ObjectId("56a2fff8a5ef125f78c1d20e"),
	"name" : "Antonio",
	"bio" : "Datos",
	"date" : ISODate("2016-01-23T04:22:11.587Z"),
	"avatar" : null,
	"background" : null,
	"auth" : {
		"username" : "Antonio_user",
		"email" : "Antonio@.gmail.com",
		"password" : "123",
		"online" : false,
		"disabled" : false,
		"hash_token" : "rzlrfrzabza4te29kt3huces7h55ewmi"
	}
}
{
	"_id" : ObjectId("56a2fff8a5ef125f78c1d20f"),
	"name" : "Carlos",
	"bio" : "Datos",
	"date" : ISODate("2016-01-23T04:22:11.587Z"),
	"avatar" : null,
	"background" : null,
	"auth" : {
		"username" : "Carlos_user",
		"email" : "Carlos@.gmail.com",
		"password" : "123",
		"online" : true,
		"disabled" : true,
		"hash_token" : "xxqmot55j6ecdi8aa53a7plhrrudi"
	}
}
>

```
## Update - alteração
#### 1. Adicione para todos os projetos o campo views: 0.
```js
> var comando = {$set:{ views: 0 }}
> var operacion = {multi : true}
> db.project.update({}, comando, operacion)
WriteResult({ "nMatched" : 5, "nUpserted" : 0, "nModified" : 5 })

```
#### 2. Adicione 1 tag diferente para cada projeto.

```js
var projectos = db.project.find({},{_id:1, name: 1}).toArray()

projectos.forEach(function(tags){
  db.project.update({_id: tags._id}, {$push: {tags:"nuevas tag" + tags.name}})
})

```
#### 3. Adicione 2 membros diferentes para cada projeto.
```JS
var projecto_id = db.project.find({},{'members.user_id':1}).toArray();
for (var i = 0; i < projecto_id.length; i++) {
  var usuarios = projecto_id[i].members.map(function(obj){ return obj.user_id._id});
  var new_user = db.usuarios.find({_id: {$nin: usuarios}},{'_id':true}).limit(2).toArray();
  var new_user_map = new_user.map(function(obj2){
    return {user_id: obj2, type_member:'admin', notify:true};
  });
  db.project.update({ _id : projecto_id[i]._id }, { $pushAll: { members : new_user_map } });
}

WriteResult({ "nMatched" : 1, "nUpserted" : 0, "nModified" : 1 })


```
#### 4. Adicione 1 comentário em cada atividade, deixe apenas 1 projeto sem.
```JS
var activ = db.activity.find({name: {$not: /activity3/i}},{_id:1})
var actividades = [];
activ.forEach(function(obj){
  actividades.push(obj._id)
})
var comen = {
  comments:[{
    text:'nuevo comentario',
    date_comment:new Date(),
    members:[{
      user_id:ObjectId("56a2fff8a5ef125f78c1d20f"),
      notify: Math.random()* 10  >= 2
    }],
    files:[{
      path:'String',
      weight:5,
      name:'juan'
    }]
  }]
}
db.activity.update({_id: {$in: actividades}}, {$push:comen}, {multi:true})
WriteResult({ "nMatched" : 2, "nUpserted" : 0, "nModified" : 2 })


db.activity.update({_id: {$in: actividades}}, {$set:{comments : [] }}, {multi:true})

```

#### 5. Adicione 1 projeto inteiro com **UPSERT**.
```js
var project = db.usuarios.find({},{"_id":true}).toArray()
var activity = db.activity.find({},{"_id":true}).toArray()
var  query = {name: /project_nuevo/i}


var mod ={
    name : 'project_nuevo',
    description : 'project_nuevo',
    date_begin: new Date(),
    date_dream: new Date(),
    date_end: new Date(),
    visible:true,
    realocate:true,
    expired: true,
    visualizable_mod:true,
    tags:['angularjs', 'css', 'html'],
    members: [
      {
        type_member: 'admin',
        user_id:project[0],
        notify: Math.random()* 10  >= 2
      },
      {
        type_member: 'moderador',
        user_id:project[1],
        notify: Math.random()* 10  >= 2
      },
      {
        type_member: 'moderador',
        user_id:project[2],
        notify: Math.random()* 10  >= 2
      },
      {
        type_member: 'moderador',
        user_id:project[3],
        notify: Math.random()* 10  >= 2
      },
      {
        type_member: 'admin',
        user_id:project[4],
        notify: Math.random()* 10  >= 2
      }
    ],
    goals: [
      {
        name: 'Goals',
        description: 'description',
        date_begin: new Date(),
        date_dream: new Date(),
        date_end:  new Date(),
        realocate:Math.random()* 10  >= 2,
        expired:Math.random()* 10  >= 2,
        tags:["Scope", "Function", "JS"],
        historic:[],
        activities: [
          {activity_id: activity[1]},
          {activity_id: activity[0]}
        ],
      }
    ]

  }
  var options = {upsert: true}

  db.project.update(query, mod, options)
  WriteResult({
	"nMatched" : 0,
	"nUpserted" : 1,
	"nModified" : 0,
	"_id" : ObjectId("56b4d72185329a1a663887b7")
})


```
## Delete - remoção
#### 1. Apague todos os projetos que não possuam *tags*.
```js
> db.project.remove({tags: {$size: 0}})
WriteResult({ "nRemoved" : 0 })
```
#### 2. Apague todos os projetos que não possuam comentários nas atividades.
```js
var activ = db.activity.find({comments:{$eq: [ ]}}).toArray()

var remover = function (obj) {
    db.project.remove({ "goals.activities.activity_id": { $eq: obj._id } }, { multi: 1 });
};
activ.forEach(remover);

```
#### 3. Apague todos os projetos que não possuam atividades.

```js
> db.project.remove({ "goals.activities": { $eq: [ ] } }, { multi: 1 })
WriteResult({ "nRemoved" : 2 })


```
#### 4.Escolha 2 usuário e apague todos os projetos em que os 2 fazem parte.

```js
var user = db.usuarios.find({},{'_id':1}).limit(2).toArray();
db.project.remove({'members.user_id':{$in:user}});
WriteResult({ "nRemoved" : 2 })

```
#### 5. Apague todos os projetos que possuam uma determinada *tag* em goal.

```js
> db.project.remove({ "goals.tags": { $eq: 'mongo' } }, { multi: 1 })
WriteResult({ "nRemoved" : 1 })
```
## Gerenciamento de usuários
#### 1. Crie um usuário com permissões **APENAS** de Leitura.
```js


db.createUser(
  {
    user: "MarkMartinez",
    pwd: "Mark123",
    roles: [
        "read"
    ]
  }
)
Successfully added user: { "user" : "MarkMartinez", "roles" : [ "read" ] }

```
#### 2. Crie um usuário com permissões de Escrita e Leitura.

```js


> db.createUser(
...   {
...     user: "MarkMartinez2",
...     pwd: "Mark1234",
...     roles: [
...         "readWrite"
...     ]
...   }
... )
Successfully added user: { "user" : "MarkMartinez2", "roles" : [ "readWrite" ] }
>

```
#### 3. Adicionar o papel `grantRolesToUser` e `revokeRole` para o usuário com Escrita e Leitura.

```js
db.createRole(
  {
    role: "grantRolesToUser",
    privileges: [
      {resource: {db:"admin", collection:""}, actions:["grantRole"]}
    ],
    roles:[]
  })

  db.createRole(
    {
      role: "revokeRole",
      privileges: [
        {resource: {db:"admin", collection:""}, actions:["grantRole"]}
      ],
      roles:[]
    })

    db.grantRolesToUser(
      "MarkMartinez2",
      [
        "grantRolesToUser", "revokeRole"
      ]
    )


```
#### 4. Remover o papel `grantRolesToUser` para o usuário com Escrita e Leitura.
```js
db.runCommand( {
  revokeRolesFromUser: "MarkMartinez2",
  roles: [
    "grantRolesToUser"
  ]
})
{ "ok" : 1 }

```
#### 5. Listar todos os usuários com seus papéis e ações.
```js
> db.runCommand({usersInfo :1})
{
	"users" : [
		{
			"_id" : "admin.MarcosAdmin",
			"user" : "MarcosAdmin",
			"db" : "admin",
			"roles" : [
				{
					"role" : "userAdminAnyDatabase",
					"db" : "admin"
				}
			]
		},
		{
			"_id" : "admin.TesteAdmin",
			"user" : "TesteAdmin",
			"db" : "admin",
			"roles" : [
				{
					"role" : "userAdminAnyDatabase",
					"db" : "admin"
				}
			]
		},
		{
			"_id" : "admin.MarkMartinez",
			"user" : "MarkMartinez",
			"db" : "admin",
			"roles" : [
				{
					"role" : "read",
					"db" : "admin"
				}
			]
		},
		{
			"_id" : "admin.MarkMartinez2",
			"user" : "MarkMartinez2",
			"db" : "admin",
			"roles" : [
				{
					"role" : "revokeRole",
					"db" : "admin"
				},
				{
					"role" : "readWrite",
					"db" : "admin"
				}
			]
		}
	],
	"ok" : 1
}
>

```
####Sharding

```JS
> mkdir /var/lib/mongodb/configdb
> mkdir /var/lib/mongodb/shard1
> mkdir /var/lib/mongodb/shard2
> mkdir /var/lib/mongodb/shard3
```

## Replica

```js
> mkdir /var/lib/mongodb/rs1
> mkdir /var/lib/mongodb/rs2
> mkdir /var/lib/mongodb/rs3

```
##  Config Server
```js
// mongod --configsvr --port 27010

mongod --configsvr --dbpath /var/lib/mongodb/configdb --port 27010
```
##  Router
```js
 mongos -configdb localhost:27010 --port 27011
```
## Shardings

```js
> mongod --replSet rs1 --port 27012 --dbpath  /var/lib/mongodb/shard1

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
{ "ok" : 1 }

```


```js
> mongod --replSet rs2 --port 27013 --dbpath  /var/lib/mongodb/shard2

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
{ "ok" : 1 }

```
```js
> mongod --replSet rs3 --port 27014 --dbpath  /var/lib/mongodb/shard3

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
{ "ok" : 1 }

```
```js
> mongo --port 27011 --host localhost
```
```js
mongos> sh.addShard("rs1/127.0.0.1:27012")
{ "shardAdded" : "rs1", "ok" : 1 }

```

```js
mongos> sh.addShard("rs2/127.0.0.1:27013")
{ "shardAdded" : "rs2", "ok" : 1 }

```

```js
mongos> sh.addShard("rs3/127.0.0.1:27014")
{ "shardAdded" : "rs1", "ok" : 1 }

```
```js
mongos> sh.enableSharding("be-mean-project")
{ "ok" : 1 }

```

```js
mongos> sh.shardCollection("be-mean-project.activities", { "_id": 1 })
{ "collectionsharded" : "be-mean-project.activities", "ok" : 1 }

```
## Replica

```js
mongod --replSet rs1 --port 27015 --dbpath /var/lib/mongodb/rs1
```
```js
mongod --replSet rs2 --port 27016 --dbpath /var/lib/mongodb/rs2
```
```js
mongod --replSet rs3 --port 27019 --dbpath /var/lib/mongodb/rs3
```
```js
> mongo --port 27012
> rs.add("127.0.0.1:27015")
{ "ok" : 1 }
```
```js
> mongo --port 27013
> rs.add("127.0.0.1:27016")
{ "ok" : 1 }
```
```js
> mongo --port 27014
> rs.add("127.0.0.1:27019")
{ "ok" : 1 }
```

```js

db.printShardingStatus()
--- Sharding Status ---
 sharding version: {
 "_id" : 1,
 "minCompatibleVersion" : 5,
 "currentVersion" : 6,
 "clusterId" : ObjectId("569d2859bb88f27d15251ee1")
}
 shards:
 {  "_id" : "rs1",  "host" : "rs1/127.0.0.1:27012" }
 {  "_id" : "rs2",  "host" : "rs2/127.0.0.1:27013" }
 {  "_id" : "rs3",  "host" : "rs2/127.0.0.1:27014" }
 {  "_id" : "shard0000",  "host" : "localhost:27012" }
 {  "_id" : "shard0001",  "host" : "localhost:27013" }
 {  "_id" : "shard0002",  "host" : "localhost:27014" }
 balancer:
 Currently enabled:  yes
 Currently running:  no
 Failed balancer rounds in last 5 attempts:  5
 Last reported error:  ReplicaSetMonitor no master found for set: rs1
 Time of Reported error:  Thu Feb 25 2016 23:19:40 GMT-0300 (PYST)
 Migration Results for the last 24 hours:
   No recent migrations
 databases:
 {  "_id" : "admin",  "partitioned" : false,  "primary" : "config" }
 {  "_id" : "test",  "partitioned" : false,  "primary" : "shard0000" }
 {  "_id" : "be-mean-mongo",  "partitioned" : true,  "primary" : "shard0000" }
   be-mean-mongo.notas
     shard key: { "_id" : 1 }
     chunks:
       shard0000	1
     { "_id" : { "$minKey" : 1 } } -->> { "_id" : { "$maxKey" : 1 } } on : shard0000 Timestamp(1, 0)
 {  "_id" : "be-mean-project",  "partitioned" : true,  "primary" : "rs2" }
   be-mean-project.activities
     shard key: { "_id" : 1 }
     chunks:
       rs2	1
     { "_id" : { "$minKey" : 1 } } -->> { "_id" : { "$maxKey" : 1 } } on : rs2 Timestamp(1, 0)


```
