# MongoDb - Projeto Final
**Autor:** Lucas Duarte Anício
**Data** 1456491787525

## Para qual sistema você usaria o MogoDB (diferente desse)?
Para sistemas que possuem um grande fluxo de usuários e que necessitam de escalabilidade.
## Qual a modelagem da sua coleção de `users`?
```js
users = {
	"name": String,
	"bio": String,
	"date-register": Date,
	"avatar-path": String,
	"background-path": String,
	"auth": {
		"username": String,
		"email": String,
		"password": String,
		"last-access": Date,
		"online": Boolean,
		"disabled": Boolean,
		"hash-token": String
	}	
}
```
## Qual a modelagem da sua coleção de `projects`?
```js
projects = {
	"name": String,
	"description": String,
	"date_begin": Date,
	"date-dream": Date,
	"date-end": Date,
	"visible": Boolean,
	"realocate": Boolean,
	"expired": Date,
	"visualizable_mod": String,
	"tags": [],
	"members": [
		{
			"member_id": ObjectID(),
			"type_member": String,
			"notify": Boolean
		}
	]
	"goals": [
		{
			"name": String,
			"description": String,
			"date_begin": Date,
			"date_dream": Date,
			"date_end": Date,
			"realocate": Boolean,
			"expired": Date,
			"tags": [],
			"activities": [
				{
					"activity_id": ObjectID()
				}
			],
			"historic": [
				"date-realocate": Date
			]
		}
	]
}
```

## Qual a modelagem da sua coleção retirada de `projects`?
```
activity = {
	"name": String,
	"description": String,
	"date-begin": Date,
	"date-dream": Date,
	"date-end": Date,
	"realocate": Boolean,
	"expired": Date,
	"tags": [],
	"members": [
		{
			"user_id": ObjectID(),
			"notify": Boolean,
			"type-name": String"
		}
	],
	"historic": [
		{
			"date_realocate": Date
		}
	],
	"comments": [
		{
			"text": String,
			"date_comment": Date,
			"members": [
				{
					"member_id": ObjectID(),
					"notify": Boolean
				}
			],
			"files": [
				{
					"path": String,
					"weight": String,
					"name": String
				}
			]
		}
	]
}
```

## Create - cadastro
**1. Cadastre 10 usuários diferentes**
```js
for(i = 0; i<10; i++) {
	var user = {
		"name":	"User " + i,
		"bio": "Bio",
		"date-register": new Date(),
		"avatar-path": i + ".png",
		"background-path": i + "-bg.png",
	}
	
	db.users.insert(user);
}

Inserted 1 record(s) in 23ms
Inserted 1 record(s) in 1ms
Inserted 1 record(s) in 0ms
Inserted 1 record(s) in 1ms
Inserted 1 record(s) in 1ms
Inserted 1 record(s) in 1ms
Inserted 1 record(s) in 1ms
Inserted 1 record(s) in 1ms
Inserted 1 record(s) in 1ms
Inserted 1 record(s) in 1ms

```
**2. Cadastre 5 projetos diferentes**
```js
//Cadastrando primeiramente as activities
for(i = 0; i<2; i++) {
	var activity = {
	    "name": "Activity " + i,
	    "description": "Description",
	    "date-begin": new Date(),
	    "date-dream": new Date() + 10,
	    "date-end": new Date() + 20,
	    "realocate": false,
	    "expired": new Date() + 50,
	    "tags": [],
	    "members": [],
	    "historic": [],
	    "comments": []
	}
	
	db.activities.insert(activity);
}

Inserted 1 record(s) in 17ms
Inserted 1 record(s) in 1ms



//1 Tag em dois projetos
for(var i=0; i<2; i++){
    var project = {
    	"name": "Project" + i,
    	"description": "Description",
    	"date-begin": new Date(),
    	"date-dream": new Date(),
    	"date-end": new Date(),
    	"visible": true,
    	"realocate": false,
    	"expired": new Date(),
    	"visualizable_mod": "Teste",
    	"tags": ["Tag1", "random" + i, "anothertag" + i],
    	"members": [ db.users.aggregate([{ $sample: { size:5 }}]) ],
    	"historic": [ ],
    	"goals": [{
			"name": "goal",
			"description": "description",
			"date_begin": new Date(),
			"date_dream": new Date(),
			"date_end": new Date(),
			"realocate": false,
			"expired": new Date(),
			"tags":["tag1", "tag2", "tag3"],
			"activities": [ db.activities.aggregate([{ $sample: { size:2 }}]) ]	
    	}]
    }
    
    db.projects.insert(project);
}
Inserted 1 record(s) in 5ms
Inserted 1 record(s) in 1ms

//1 tag em 3 projetos e um projeto sem activity
for(var i=2; i<4; i++){
    var project = {
    	"name": "Project" + i,
    	"description": "Description",
    	"date-begin": new Date(),
    	"date-dream": new Date(),
    	"date-end": new Date(),
    	"visible": true,
    	"realocate": false,
    	"expired": new Date(),
    	"visualizable_mod": "Teste",
    	"tags": ["Tag2", "randomtag" + i, "anothertagrandom" + i],
    	"members": [ db.users.aggregate([{ $sample: { size:5 }}]) ],
    	"historic": [ ],
    	"goals": [{
			"name": "goal",
			"description": "description",
			"date_begin": new Date(),
			"date_dream": new Date(),
			"date_end": new Date(),
			"realocate": false,
			"expired": new Date(),
			"tags":["tag1", "tag2", "tag3"],
			"activities": [ 
			        db.activities.aggregate([{ $sample: { size:2 }}]) 
			 ]	
    	}]
    }
    
    db.projects.insert(project);
}
Inserted 1 record(s) in 1ms
Inserted 1 record(s) in 3ms

var project = {
   	"name": "Project5",
   	"description": "Description",
   	"date-begin": new Date(),
   	"date-dream": new Date(),
   	"date-end": new Date(),
   	"visible": true,
   	"realocate": false,
   	"expired": new Date(),
   	"visualizable_mod": "Teste",
   	"tags": ["Tag2", "randomtag5", "anothertagrandom5"],
   	"members": [ db.users.aggregate([{ $sample: { size:5 }}]) ],
   	"historic": [ ],
   	"goals": [{
		"name": "goal",
		"description": "description",
		"date_begin": new Date(),
		"date_dream": new Date(),
		"date_end": new Date(),
		"realocate": false,
		"expired": new Date(),
		"tags":["tag1", "tag2", "tag3"],
		"activities": [ 
		        
		 ]	
   	}]
}
db.projects.insert(project);
Inserted 1 record(s) in 1ms
```

## Retrieve - busca
**1. Liste as informações dos membros de 1 projeto específico que deve ser buscado pelo seu nome de forma a não ligar para maiúsculas e minúsculas.**
```js
be-mean> db.projects.find({name: /project0/i}, {members: 1} );
{
  "_id": ObjectId("56d8cc1ff8b6cd6dd0bf7ea2"),
  "members": [
    {
      "waitedMS": NumberLong("0"),
      "result": [
        {
          "_id": ObjectId("56d60c198609f803e485e5f7"),
          "name": "User 6",
          "bio": "Bio",
          "date-register": ISODate("2016-03-01T21:39:37.013Z"),
          "avatar-path": "6.png",
          "background-path": "6-bg.png"
        },
        {
          "_id": ObjectId("56d60c198609f803e485e5f3"),
          "name": "User 2",
          "bio": "Bio",
          "date-register": ISODate("2016-03-01T21:39:37.010Z"),
          "avatar-path": "2.png",
          "background-path": "2-bg.png"
        },
        {
          "_id": ObjectId("56d60c198609f803e485e5f4"),
          "name": "User 3",
          "bio": "Bio",
          "date-register": ISODate("2016-03-01T21:39:37.010Z"),
          "avatar-path": "3.png",
          "background-path": "3-bg.png"
        },
        {
          "_id": ObjectId("56d60c188609f803e485e5f1"),
          "name": "User 0",
          "bio": "Bio",
          "date-register": ISODate("2016-03-01T21:39:36.986Z"),
          "avatar-path": "0.png",
          "background-path": "0-bg.png"
        },
        {
          "_id": ObjectId("56d60c198609f803e485e5f5"),
          "name": "User 4",
          "bio": "Bio",
          "date-register": ISODate("2016-03-01T21:39:37.011Z"),
          "avatar-path": "4.png",
          "background-path": "4-bg.png"
        }
      ],
      "ok": 1
    }
  ]
}
Fetched 1 record(s) in 2ms

```
**2. Liste todos os projetos com a tag que você escolheu para os 3 projetos em comum.**
```js
be-mean> db.projects.find({tags: { $eq: 'Tag2' } })
{
  "_id": ObjectId("56d8cd1af8b6cd6dd0bf7ea4"),
  "name": "Project2",
  "description": "Description",
  "date-begin": ISODate("2016-03-03T23:47:38.794Z"),
  "date-dream": ISODate("2016-03-03T23:47:38.794Z"),
  "date-end": ISODate("2016-03-03T23:47:38.794Z"),
  "visible": true,
  "realocate": false,
  "expired": ISODate("2016-03-03T23:47:38.794Z"),
  "visualizable_mod": "Teste",
  "tags": [
    "Tag2",
    "randomtag2",
    "anothertagrandom2"
  ],
  "members": [
    {
      "waitedMS": NumberLong("0"),
      "result": [
        {
          "_id": ObjectId("56d60c198609f803e485e5f9"),
          "name": "User 8",
          "bio": "Bio",
          "date-register": ISODate("2016-03-01T21:39:37.015Z"),
          "avatar-path": "8.png",
          "background-path": "8-bg.png"
        },
        {
          "_id": ObjectId("56d60c198609f803e485e5f3"),
          "name": "User 2",
          "bio": "Bio",
          "date-register": ISODate("2016-03-01T21:39:37.010Z"),
          "avatar-path": "2.png",
          "background-path": "2-bg.png"
        },
        {
          "_id": ObjectId("56d60c198609f803e485e5f6"),
          "name": "User 5",
          "bio": "Bio",
          "date-register": ISODate("2016-03-01T21:39:37.012Z"),
          "avatar-path": "5.png",
          "background-path": "5-bg.png"
        },
        {
          "_id": ObjectId("56d60c198609f803e485e5f5"),
          "name": "User 4",
          "bio": "Bio",
          "date-register": ISODate("2016-03-01T21:39:37.011Z"),
          "avatar-path": "4.png",
          "background-path": "4-bg.png"
        },
        {
          "_id": ObjectId("56d60c198609f803e485e5f7"),
          "name": "User 6",
          "bio": "Bio",
          "date-register": ISODate("2016-03-01T21:39:37.013Z"),
          "avatar-path": "6.png",
          "background-path": "6-bg.png"
        }
      ],
      "ok": 1
    }
  ],
  "historic": [ ],
  "goals": [
    {
      "name": "goal",
      "description": "description",
      "date_begin": ISODate("2016-03-03T23:47:38.795Z"),
      "date_dream": ISODate("2016-03-03T23:47:38.795Z"),
      "date_end": ISODate("2016-03-03T23:47:38.795Z"),
      "realocate": false,
      "expired": ISODate("2016-03-03T23:47:38.795Z"),
      "tags": [
        "tag1",
        "tag2",
        "tag3"
      ],
      "activities": [
        {
          "waitedMS": NumberLong("0"),
          "result": [
            {
              "_id": ObjectId("56d613b98609f803e485e5fc"),
              "name": "Activity 1",
              "description": "Description",
              "date-begin": ISODate("2016-03-01T22:12:09.667Z"),
              "date-dream": "Tue Mar 01 2016 19:12:09 GMT-0300 (BRT)10",
              "date-end": "Tue Mar 01 2016 19:12:09 GMT-0300 (BRT)20",
              "realocate": false,
              "expired": "Tue Mar 01 2016 19:12:09 GMT-0300 (BRT)50",
              "tags": [ ],
              "members": [ ],
              "historic": [ ],
              "comments": [ ]
            },
            {
              "_id": ObjectId("56d613b98609f803e485e5fb"),
              "name": "Activity 0",
              "description": "Description",
              "date-begin": ISODate("2016-03-01T22:12:09.650Z"),
              "date-dream": "Tue Mar 01 2016 19:12:09 GMT-0300 (BRT)10",
              "date-end": "Tue Mar 01 2016 19:12:09 GMT-0300 (BRT)20",
              "realocate": false,
              "expired": "Tue Mar 01 2016 19:12:09 GMT-0300 (BRT)50",
              "tags": [ ],
              "members": [ ],
              "historic": [ ],
              "comments": [ ]
            }
          ],
          "ok": 1
        }
      ]
    }
  ]
}
{
  "_id": ObjectId("56d8cd1af8b6cd6dd0bf7ea5"),
  "name": "Project3",
  "description": "Description",
  "date-begin": ISODate("2016-03-03T23:47:38.797Z"),
  "date-dream": ISODate("2016-03-03T23:47:38.797Z"),
  "date-end": ISODate("2016-03-03T23:47:38.797Z"),
  "visible": true,
  "realocate": false,
  "expired": ISODate("2016-03-03T23:47:38.797Z"),
  "visualizable_mod": "Teste",
  "tags": [
    "Tag2",
    "randomtag3",
    "anothertagrandom3"
  ],
  "members": [
    {
      "waitedMS": NumberLong("0"),
      "result": [
        {
          "_id": ObjectId("56d60c198609f803e485e5f9"),
          "name": "User 8",
          "bio": "Bio",
          "date-register": ISODate("2016-03-01T21:39:37.015Z"),
          "avatar-path": "8.png",
          "background-path": "8-bg.png"
        },
        {
          "_id": ObjectId("56d60c198609f803e485e5f2"),
          "name": "User 1",
          "bio": "Bio",
          "date-register": ISODate("2016-03-01T21:39:37.009Z"),
          "avatar-path": "1.png",
          "background-path": "1-bg.png"
        },
        {
          "_id": ObjectId("56d60c198609f803e485e5fa"),
          "name": "User 9",
          "bio": "Bio",
          "date-register": ISODate("2016-03-01T21:39:37.016Z"),
          "avatar-path": "9.png",
          "background-path": "9-bg.png"
        },
        {
          "_id": ObjectId("56d60c198609f803e485e5f7"),
          "name": "User 6",
          "bio": "Bio",
          "date-register": ISODate("2016-03-01T21:39:37.013Z"),
          "avatar-path": "6.png",
          "background-path": "6-bg.png"
        },
        {
          "_id": ObjectId("56d60c198609f803e485e5f4"),
          "name": "User 3",
          "bio": "Bio",
          "date-register": ISODate("2016-03-01T21:39:37.010Z"),
          "avatar-path": "3.png",
          "background-path": "3-bg.png"
        }
      ],
      "ok": 1
    }
  ],
  "historic": [ ],
  "goals": [
    {
      "name": "goal",
      "description": "description",
      "date_begin": ISODate("2016-03-03T23:47:38.798Z"),
      "date_dream": ISODate("2016-03-03T23:47:38.798Z"),
      "date_end": ISODate("2016-03-03T23:47:38.798Z"),
      "realocate": false,
      "expired": ISODate("2016-03-03T23:47:38.798Z"),
      "tags": [
        "tag1",
        "tag2",
        "tag3"
      ],
      "activities": [
        {
          "waitedMS": NumberLong("0"),
          "result": [
            {
              "_id": ObjectId("56d613b98609f803e485e5fb"),
              "name": "Activity 0",
              "description": "Description",
              "date-begin": ISODate("2016-03-01T22:12:09.650Z"),
              "date-dream": "Tue Mar 01 2016 19:12:09 GMT-0300 (BRT)10",
              "date-end": "Tue Mar 01 2016 19:12:09 GMT-0300 (BRT)20",
              "realocate": false,
              "expired": "Tue Mar 01 2016 19:12:09 GMT-0300 (BRT)50",
              "tags": [ ],
              "members": [ ],
              "historic": [ ],
              "comments": [ ]
            },
            {
              "_id": ObjectId("56d613b98609f803e485e5fc"),
              "name": "Activity 1",
              "description": "Description",
              "date-begin": ISODate("2016-03-01T22:12:09.667Z"),
              "date-dream": "Tue Mar 01 2016 19:12:09 GMT-0300 (BRT)10",
              "date-end": "Tue Mar 01 2016 19:12:09 GMT-0300 (BRT)20",
              "realocate": false,
              "expired": "Tue Mar 01 2016 19:12:09 GMT-0300 (BRT)50",
              "tags": [ ],
              "members": [ ],
              "historic": [ ],
              "comments": [ ]
            }
          ],
          "ok": 1
        }
      ]
    }
  ]
}
{
  "_id": ObjectId("56d8cd48f8b6cd6dd0bf7ea6"),
  "name": "Project5",
  "description": "Description",
  "date-begin": ISODate("2016-03-03T23:48:23.372Z"),
  "date-dream": ISODate("2016-03-03T23:48:23.372Z"),
  "date-end": ISODate("2016-03-03T23:48:23.372Z"),
  "visible": true,
  "realocate": false,
  "expired": ISODate("2016-03-03T23:48:23.372Z"),
  "visualizable_mod": "Teste",
  "tags": [
    "Tag2",
    "randomtag5",
    "anothertagrandom5"
  ],
  "members": [
    {
      "waitedMS": NumberLong("0"),
      "result": [
        {
          "_id": ObjectId("56d60c198609f803e485e5fa"),
          "name": "User 9",
          "bio": "Bio",
          "date-register": ISODate("2016-03-01T21:39:37.016Z"),
          "avatar-path": "9.png",
          "background-path": "9-bg.png"
        },
        {
          "_id": ObjectId("56d60c198609f803e485e5f4"),
          "name": "User 3",
          "bio": "Bio",
          "date-register": ISODate("2016-03-01T21:39:37.010Z"),
          "avatar-path": "3.png",
          "background-path": "3-bg.png"
        },
        {
          "_id": ObjectId("56d60c198609f803e485e5f5"),
          "name": "User 4",
          "bio": "Bio",
          "date-register": ISODate("2016-03-01T21:39:37.011Z"),
          "avatar-path": "4.png",
          "background-path": "4-bg.png"
        },
        {
          "_id": ObjectId("56d60c198609f803e485e5f2"),
          "name": "User 1",
          "bio": "Bio",
          "date-register": ISODate("2016-03-01T21:39:37.009Z"),
          "avatar-path": "1.png",
          "background-path": "1-bg.png"
        },
        {
          "_id": ObjectId("56d60c188609f803e485e5f1"),
          "name": "User 0",
          "bio": "Bio",
          "date-register": ISODate("2016-03-01T21:39:36.986Z"),
          "avatar-path": "0.png",
          "background-path": "0-bg.png"
        }
      ],
      "ok": 1
    }
  ],
  "historic": [ ],
  "goals": [
    {
      "name": "goal",
      "description": "description",
      "date_begin": ISODate("2016-03-03T23:48:23.372Z"),
      "date_dream": ISODate("2016-03-03T23:48:23.372Z"),
      "date_end": ISODate("2016-03-03T23:48:23.372Z"),
      "realocate": false,
      "expired": ISODate("2016-03-03T23:48:23.372Z"),
      "tags": [
        "tag1",
        "tag2",
        "tag3"
      ],
      "activities": [ ]
    }
  ]
}
Fetched 3 record(s) in 6ms
```

**3. Liste apenas os nomes de todas as atividades para todos os projetos.**
```js
be-mean> db.projects.find({}, {"goals.activities": 1})
{
  "_id": ObjectId("56d8cc1ff8b6cd6dd0bf7ea2"),
  "goals": [
    {
      "activities": [
        {
          "waitedMS": NumberLong("0"),
          "result": [
            {
              "_id": ObjectId("56d613b98609f803e485e5fb"),
              "name": "Activity 0",
              "description": "Description",
              "date-begin": ISODate("2016-03-01T22:12:09.650Z"),
              "date-dream": "Tue Mar 01 2016 19:12:09 GMT-0300 (BRT)10",
              "date-end": "Tue Mar 01 2016 19:12:09 GMT-0300 (BRT)20",
              "realocate": false,
              "expired": "Tue Mar 01 2016 19:12:09 GMT-0300 (BRT)50",
              "tags": [ ],
              "members": [ ],
              "historic": [ ],
              "comments": [ ]
            },
            {
              "_id": ObjectId("56d613b98609f803e485e5fc"),
              "name": "Activity 1",
              "description": "Description",
              "date-begin": ISODate("2016-03-01T22:12:09.667Z"),
              "date-dream": "Tue Mar 01 2016 19:12:09 GMT-0300 (BRT)10",
              "date-end": "Tue Mar 01 2016 19:12:09 GMT-0300 (BRT)20",
              "realocate": false,
              "expired": "Tue Mar 01 2016 19:12:09 GMT-0300 (BRT)50",
              "tags": [ ],
              "members": [ ],
              "historic": [ ],
              "comments": [ ]
            }
          ],
          "ok": 1
        }
      ]
    }
  ]
}
{
  "_id": ObjectId("56d8cc1ff8b6cd6dd0bf7ea3"),
  "goals": [
    {
      "activities": [
        {
          "waitedMS": NumberLong("0"),
          "result": [
            {
              "_id": ObjectId("56d613b98609f803e485e5fc"),
              "name": "Activity 1",
              "description": "Description",
              "date-begin": ISODate("2016-03-01T22:12:09.667Z"),
              "date-dream": "Tue Mar 01 2016 19:12:09 GMT-0300 (BRT)10",
              "date-end": "Tue Mar 01 2016 19:12:09 GMT-0300 (BRT)20",
              "realocate": false,
              "expired": "Tue Mar 01 2016 19:12:09 GMT-0300 (BRT)50",
              "tags": [ ],
              "members": [ ],
              "historic": [ ],
              "comments": [ ]
            },
            {
              "_id": ObjectId("56d613b98609f803e485e5fb"),
              "name": "Activity 0",
              "description": "Description",
              "date-begin": ISODate("2016-03-01T22:12:09.650Z"),
              "date-dream": "Tue Mar 01 2016 19:12:09 GMT-0300 (BRT)10",
              "date-end": "Tue Mar 01 2016 19:12:09 GMT-0300 (BRT)20",
              "realocate": false,
              "expired": "Tue Mar 01 2016 19:12:09 GMT-0300 (BRT)50",
              "tags": [ ],
              "members": [ ],
              "historic": [ ],
              "comments": [ ]
            }
          ],
          "ok": 1
        }
      ]
    }
  ]
}
{
  "_id": ObjectId("56d8cd1af8b6cd6dd0bf7ea4"),
  "goals": [
    {
      "activities": [
        {
          "waitedMS": NumberLong("0"),
          "result": [
            {
              "_id": ObjectId("56d613b98609f803e485e5fc"),
              "name": "Activity 1",
              "description": "Description",
              "date-begin": ISODate("2016-03-01T22:12:09.667Z"),
              "date-dream": "Tue Mar 01 2016 19:12:09 GMT-0300 (BRT)10",
              "date-end": "Tue Mar 01 2016 19:12:09 GMT-0300 (BRT)20",
              "realocate": false,
              "expired": "Tue Mar 01 2016 19:12:09 GMT-0300 (BRT)50",
              "tags": [ ],
              "members": [ ],
              "historic": [ ],
              "comments": [ ]
            },
            {
              "_id": ObjectId("56d613b98609f803e485e5fb"),
              "name": "Activity 0",
              "description": "Description",
              "date-begin": ISODate("2016-03-01T22:12:09.650Z"),
              "date-dream": "Tue Mar 01 2016 19:12:09 GMT-0300 (BRT)10",
              "date-end": "Tue Mar 01 2016 19:12:09 GMT-0300 (BRT)20",
              "realocate": false,
              "expired": "Tue Mar 01 2016 19:12:09 GMT-0300 (BRT)50",
              "tags": [ ],
              "members": [ ],
              "historic": [ ],
              "comments": [ ]
            }
          ],
          "ok": 1
        }
      ]
    }
  ]
}
{
  "_id": ObjectId("56d8cd1af8b6cd6dd0bf7ea5"),
  "goals": [
    {
      "activities": [
        {
          "waitedMS": NumberLong("0"),
          "result": [
            {
              "_id": ObjectId("56d613b98609f803e485e5fb"),
              "name": "Activity 0",
              "description": "Description",
              "date-begin": ISODate("2016-03-01T22:12:09.650Z"),
              "date-dream": "Tue Mar 01 2016 19:12:09 GMT-0300 (BRT)10",
              "date-end": "Tue Mar 01 2016 19:12:09 GMT-0300 (BRT)20",
              "realocate": false,
              "expired": "Tue Mar 01 2016 19:12:09 GMT-0300 (BRT)50",
              "tags": [ ],
              "members": [ ],
              "historic": [ ],
              "comments": [ ]
            },
            {
              "_id": ObjectId("56d613b98609f803e485e5fc"),
              "name": "Activity 1",
              "description": "Description",
              "date-begin": ISODate("2016-03-01T22:12:09.667Z"),
              "date-dream": "Tue Mar 01 2016 19:12:09 GMT-0300 (BRT)10",
              "date-end": "Tue Mar 01 2016 19:12:09 GMT-0300 (BRT)20",
              "realocate": false,
              "expired": "Tue Mar 01 2016 19:12:09 GMT-0300 (BRT)50",
              "tags": [ ],
              "members": [ ],
              "historic": [ ],
              "comments": [ ]
            }
          ],
          "ok": 1
        }
      ]
    }
  ]
}
{
  "_id": ObjectId("56d8cd48f8b6cd6dd0bf7ea6"),
  "goals": [
    {
      "activities": [ ]
    }
  ]
}
Fetched 5 record(s) in 4ms
```

**4. Liste todos os projetos que não possuam uma tag.**
```
be-mean> db.projects.find({ tags: [] })
```

**5. Liste todos os usuários que não fazem parte do primeiro projeto cadastrado.**
```js
be-mean> var users = db.projects.find({name: /Project0/}, {"members._id": 1, _id: 0})
be-mean> db.users.find({ _id: { $not: { $in: users } } })
{
  "_id": ObjectId("56d60c198609f803e485e5f2"),
  "name": "User 1",
  "bio": "Bio",
  "date-register": ISODate("2016-03-01T21:39:37.009Z"),
  "avatar-path": "1.png",
  "background-path": "1-bg.png"
}
{
  "_id": ObjectId("56d60c198609f803e485e5f6"),
  "name": "User 5",
  "bio": "Bio",
  "date-register": ISODate("2016-03-01T21:39:37.012Z"),
  "avatar-path": "5.png",
  "background-path": "5-bg.png"
}
{
  "_id": ObjectId("56d60c198609f803e485e5f8"),
  "name": "User 7",
  "bio": "Bio",
  "date-register": ISODate("2016-03-01T21:39:37.014Z"),
  "avatar-path": "7.png",
  "background-path": "7-bg.png"
}
{
  "_id": ObjectId("56d60c198609f803e485e5f9"),
  "name": "User 8",
  "bio": "Bio",
  "date-register": ISODate("2016-03-01T21:39:37.015Z"),
  "avatar-path": "8.png",
  "background-path": "8-bg.png"
}
{
  "_id": ObjectId("56d60c198609f803e485e5fa"),
  "name": "User 9",
  "bio": "Bio",
  "date-register": ISODate("2016-03-01T21:39:37.016Z"),
  "avatar-path": "9.png",
  "background-path": "9-bg.png"
}
Fetched 5 record(s) in 1ms

```

## Update - alteração

**1. Adicione para todos os projetos o campo views: 0.**
```js
be-mean> db.projects.update({}, { $set: {views: 0} }, { multi: true} )
Updated 5 existing record(s) in 1ms
WriteResult({
  "nMatched": 5,
  "nUpserted": 0,
  "nModified": 5
})

```
**2. Adicione 1 tag diferente para cada projeto.**
```js
var number = 0;
var projects = db.projects.find();
projects.forEach(function(project){
    var query = { "_id": project._id} ;
    var mod = { $push: { "tags": "TagNova" + ++number } }
    db.projects.update(query, mod);
})
Updated 1 existing record(s) in 6ms
Updated 1 existing record(s) in 0ms
Updated 1 existing record(s) in 0ms
Updated 1 existing record(s) in 1ms
Updated 1 existing record(s) in 1ms
```
**3. Adicione 2 membros diferentes para cada projeto.**
```js
var projects = db.projects.find();
projects.forEach(function(project){
    var query = { "_id": project._id} ;
    var members = db.users.aggregate([{$sample: {size: 2}}]).result
    var mod = { $push: { "members": members } }
    db.projects.update(query, mod);
})
Updated 1 existing record(s) in 1ms
Updated 1 existing record(s) in 1ms
Updated 1 existing record(s) in 1ms
Updated 1 existing record(s) in 2ms
Updated 1 existing record(s) in 1ms

```
**4. Adicione 1 comentário em cada atividade, deixe apenas 1 projeto sem.**
```js
var activities = db.activities.find()

activities.forEach(function(activity){
    var query = activity._id;
    var mod = {$push : {"comments" : {'text' : 'Comment', date_comment : new Date()}}}
    
    db.activities.update(query,mod);
})
```
**5. Adicione 1 projeto inteiro com UPSERT.**
```js
var project = {
    	"name": "New Project",
    	"description": "Description",
    	"date-begin": new Date(),
    	"date-dream": new Date(),
    	"date-end": new Date(),
    	"visible": true,
    	"realocate": false,
    	"expired": new Date(),
    	"visualizable_mod": "Teste",
    	"tags": [],
    	"members": [ ],
    	"historic": [ ],
    	"goals": [{
			"name": "goal",
			"description": "description",
			"date_begin": new Date(),
			"date_dream": new Date(),
			"date_end": new Date(),
			"realocate": false,
			"expired": new Date(),
			"tags":["tag1", "tag2", "tag3"],
			"activities": []	
    	}]
}

var query = {}
var opt = {upsert: true}
be-mean> db.projects.update(project,query,opt)
Updated 1 new record(s) in 3ms
WriteResult({
  "nMatched": 0,
  "nUpserted": 1,
  "nModified": 0,
  "_id": ObjectId("56dae0552a0c75d7ed9d04fd")
})

```

## Delete - remoção

**1. Apague todos os projetos que não possuam tags.**
```js
db.project.remove({"tags" : {$size : 0}})
```
**2. Apague todos os projetos que não possuam comentários nas atividades.**
```js
var query = {
        $or: [{
            comments: { $eq: [ ] }
        }]
    };

var ids = [];
db.activities.find(query, { _id: 1 }).forEach(function(activity) {
    ids.push(activity._id);
});

db.activities.remove({ _id: { $in: ids } });

db.projects.remove({ "goals.activities.activity_id": { $in: ids } });
```
**3. Apague todos os projetos que não possuam atividades.**
```js
be-mean> var query = { "goals.activities": { $size: 0 } } 
be-mean> db.projects.remove(query)
Removed 1 record(s) in 0ms
WriteResult({
  "nRemoved": 1
})

```
**4. Escolha 2 usuários e apague todos os projetos em que os 2 fazem parte.**
```js
var users = db.user.find()
db.project.remove({$and : [{"members.user_id" : users[0]._id},{"members.user_id" : users[1]._id}]})
```
**5. Apague todos os projetos que possuam uma determinada tag em goal.**
```js
db.project.remove({"goals.tags" : {$eq : "Tag1"}})
```

## Gerenciamento de usuários

**1. Crie um usuário com permissões APENAS de Leitura.**
```js
be-mean> db.createUser({user: "lucas", pwd: "secret", roles: ["read"]})
Successfully added user: {
  "user": "lucas",
  "roles": [
    "read"
  ]
}

```
**2. Crie um usuário com permissões de Escrita e Leitura.**
```js
be-mean> db.createUser({user: "lucasWrite", pwd: "secret", roles: ["readWrite"]}) 
Successfully added user: {
  "user": "lucasWrite",
  "roles": [
    "readWrite"
  ]
}

```
**3. Adicionar o papel grantRolesToUser e revokeRole para o usuário com Escrita e Leitura.**
```js
db.createRole({
    role: "grantRolesToUser",
    privileges: [{
        resource: { db: "admin", collection: "" },
        actions: [ "grantRole" ]
    }],
    roles: []
})

db.createRole({
    role: "revokeRole",
    privileges: [{
        resource: { db: "admin", collection: "" },
        actions: [ "grantRole" ]
    }],
    roles: []
})

db.grantRolesToUser("lucasWrite", [ "grantRolesToUser", "revokeRole" ]);
```
**4. Remover o papel grantRolesToUser para o usuário com Escrita e Leitura.**
```js
db.runCommand({
     revokeRolesFromUser: "lucasWrite",
     roles: [ "grantRolesToUser" ]
 });
{
  "ok": 1
}

```
**5. Listar todos os usuários com seus papéis e ações.**
```js
db.runCommand({
     usersInfo: [
         { user: "lucas", db: "admin" },
         { user: "lucasWrite", db: "admin" }
     ],
     showCredentials: true,
     showPrivileges: true
 });
```

## **Cluster**

###**1 Config Server**
```js
lucas@lucas-pc:~$ sudo mongod --configsvr --port 27011
2016-03-05T12:06:10.204-0300 I CONTROL  [initandlisten] MongoDB starting : pid=31284 port=27011 dbpath=/data/configdb master=1 64-bit host=lucas-pc
2016-03-05T12:06:10.204-0300 I CONTROL  [initandlisten] db version v3.2.3
2016-03-05T12:06:10.204-0300 I CONTROL  [initandlisten] git version: b326ba837cf6f49d65c2f85e1b70f6f31ece7937
2016-03-05T12:06:10.204-0300 I CONTROL  [initandlisten] OpenSSL version: OpenSSL 1.0.2d 9 Jul 2015
2016-03-05T12:06:10.204-0300 I CONTROL  [initandlisten] allocator: tcmalloc
2016-03-05T12:06:10.204-0300 I CONTROL  [initandlisten] modules: none
2016-03-05T12:06:10.204-0300 I CONTROL  [initandlisten] build environment:
2016-03-05T12:06:10.204-0300 I CONTROL  [initandlisten]     distmod: ubuntu1404
2016-03-05T12:06:10.204-0300 I CONTROL  [initandlisten]     distarch: x86_64
2016-03-05T12:06:10.204-0300 I CONTROL  [initandlisten]     target_arch: x86_64
2016-03-05T12:06:10.204-0300 I CONTROL  [initandlisten] options: { net: { port: 27011 }, sharding: { clusterRole: "configsvr" } }
2016-03-05T12:06:10.230-0300 I -        [initandlisten] Detected data files in /data/configdb created by the 'wiredTiger' storage engine, so setting the active storage engine to 'wiredTiger'.
2016-03-05T12:06:10.230-0300 I STORAGE  [initandlisten] wiredtiger_open config: create,cache_size=4G,session_max=20000,eviction=(threads_max=4),config_base=false,statistics=(fast),log=(enabled=true,archive=true,path=journal,compressor=snappy),file_manager=(close_idle_time=100000),checkpoint=(wait=60,log_size=2GB),statistics_log=(wait=0),
2016-03-05T12:06:10.495-0300 I STORAGE  [initandlisten] Starting WiredTigerRecordStoreThread local.oplog.$main
2016-03-05T12:06:10.495-0300 I STORAGE  [initandlisten] The size storer reports that the oplog contains 63 records totaling to 9757 bytes
2016-03-05T12:06:10.495-0300 I STORAGE  [initandlisten] Scanning the oplog to determine where to place markers for truncation
2016-03-05T12:06:10.515-0300 I CONTROL  [initandlisten] ** WARNING: You are running this process as the root user, which is not recommended.
2016-03-05T12:06:10.515-0300 I CONTROL  [initandlisten] 
2016-03-05T12:06:10.515-0300 I CONTROL  [initandlisten] 
2016-03-05T12:06:10.515-0300 I CONTROL  [initandlisten] ** WARNING: /sys/kernel/mm/transparent_hugepage/enabled is 'always'.
2016-03-05T12:06:10.515-0300 I CONTROL  [initandlisten] **        We suggest setting it to 'never'
2016-03-05T12:06:10.516-0300 I CONTROL  [initandlisten] 
2016-03-05T12:06:10.516-0300 I CONTROL  [initandlisten] ** WARNING: /sys/kernel/mm/transparent_hugepage/defrag is 'always'.
2016-03-05T12:06:10.516-0300 I CONTROL  [initandlisten] **        We suggest setting it to 'never'
2016-03-05T12:06:10.516-0300 I CONTROL  [initandlisten] 
2016-03-05T12:06:10.520-0300 I FTDC     [initandlisten] Initializing full-time diagnostic data capture with directory '/data/configdb/diagnostic.data'
2016-03-05T12:06:10.520-0300 I NETWORK  [HostnameCanonicalizationWorker] Starting hostname canonicalization worker
2016-03-05T12:06:10.520-0300 I NETWORK  [initandlisten] waiting for connections on port 27011

```
###**1 Router**
```js
lucas@lucas-pc:~$ mongos --configdb localhost:27011 --port 27012
2016-03-05T12:06:36.584-0300 W SHARDING [main] Running a sharded cluster with fewer than 3 config servers should only be done for testing purposes and is not recommended for production.
2016-03-05T12:06:36.588-0300 I SHARDING [mongosMain] MongoS version 3.2.3 starting: pid=31356 port=27012 64-bit host=lucas-pc (--help for usage)
2016-03-05T12:06:36.588-0300 I CONTROL  [mongosMain] db version v3.2.3
2016-03-05T12:06:36.588-0300 I CONTROL  [mongosMain] git version: b326ba837cf6f49d65c2f85e1b70f6f31ece7937
2016-03-05T12:06:36.588-0300 I CONTROL  [mongosMain] OpenSSL version: OpenSSL 1.0.2d 9 Jul 2015
2016-03-05T12:06:36.588-0300 I CONTROL  [mongosMain] allocator: tcmalloc
2016-03-05T12:06:36.588-0300 I CONTROL  [mongosMain] modules: none
2016-03-05T12:06:36.588-0300 I CONTROL  [mongosMain] build environment:
2016-03-05T12:06:36.588-0300 I CONTROL  [mongosMain]     distmod: ubuntu1404
2016-03-05T12:06:36.588-0300 I CONTROL  [mongosMain]     distarch: x86_64
2016-03-05T12:06:36.588-0300 I CONTROL  [mongosMain]     target_arch: x86_64
2016-03-05T12:06:36.588-0300 I CONTROL  [mongosMain] options: { net: { port: 27012 }, sharding: { configDB: "localhost:27011" } }
2016-03-05T12:06:36.589-0300 I SHARDING [mongosMain] Updating config server connection string to: localhost:27011
2016-03-05T12:06:36.592-0300 I SHARDING [LockPinger] creating distributed lock ping thread for localhost:27011 and process lucas-pc:27012:1457190396:-699966356 (sleeping for 30000ms)
2016-03-05T12:06:36.593-0300 I SHARDING [LockPinger] cluster localhost:27011 pinged successfully at 2016-03-05T12:06:36.592-0300 by distributed lock pinger 'localhost:27011/lucas-pc:27012:1457190396:-699966356', sleeping for 30000ms
2016-03-05T12:06:36.599-0300 I NETWORK  [HostnameCanonicalizationWorker] Starting hostname canonicalization worker
2016-03-05T12:06:36.599-0300 I SHARDING [Balancer] about to contact config servers and shards
2016-03-05T12:06:36.599-0300 I SHARDING [Balancer] config servers and shards contacted successfully
2016-03-05T12:06:36.599-0300 I SHARDING [Balancer] balancer id: lucas-pc:27012 started
2016-03-05T12:06:36.603-0300 I SHARDING [Balancer] distributed lock 'balancer/lucas-pc:27012:1457190396:-699966356' acquired for 'doing balance round', ts : 56daf5fcbc6cfb9eca13c877
2016-03-05T12:06:36.604-0300 I SHARDING [Balancer] about to log metadata event into actionlog: { _id: "lucas-pc-2016-03-05T12:06:36.604-0300-56daf5fcbc6cfb9eca13c878", server: "lucas-pc", clientAddr: "", time: new Date(1457190396604), what: "balancer.round", ns: "", details: { executionTimeMillis: 4, errorOccured: false, candidateChunks: 0, chunksMoved: 0 } }
2016-03-05T12:06:36.607-0300 I SHARDING [Balancer] distributed lock 'balancer/lucas-pc:27012:1457190396:-699966356' unlocked. 
2016-03-05T12:06:36.626-0300 I NETWORK  [mongosMain] waiting for connections on port 27012

```
##**3 Shardings**
```
lucas@lucas-pc:~$ mkdir /data/shard1 && mkdir /data/shard2 && mkdir /data/shard3
```
###**Shard1**
```js
lucas@lucas-pc:~$ sudo mongod --port 27013 --dbpath /data/shard1
2016-03-05T12:07:44.604-0300 I CONTROL  [initandlisten] MongoDB starting : pid=31524 port=27013 dbpath=/data/shard1 64-bit host=lucas-pc
2016-03-05T12:07:44.604-0300 I CONTROL  [initandlisten] db version v3.2.3
2016-03-05T12:07:44.604-0300 I CONTROL  [initandlisten] git version: b326ba837cf6f49d65c2f85e1b70f6f31ece7937
2016-03-05T12:07:44.604-0300 I CONTROL  [initandlisten] OpenSSL version: OpenSSL 1.0.2d 9 Jul 2015
2016-03-05T12:07:44.604-0300 I CONTROL  [initandlisten] allocator: tcmalloc
2016-03-05T12:07:44.604-0300 I CONTROL  [initandlisten] modules: none
2016-03-05T12:07:44.604-0300 I CONTROL  [initandlisten] build environment:
2016-03-05T12:07:44.604-0300 I CONTROL  [initandlisten]     distmod: ubuntu1404
2016-03-05T12:07:44.604-0300 I CONTROL  [initandlisten]     distarch: x86_64
2016-03-05T12:07:44.604-0300 I CONTROL  [initandlisten]     target_arch: x86_64
2016-03-05T12:07:44.604-0300 I CONTROL  [initandlisten] options: { net: { port: 27013 }, storage: { dbPath: "/data/shard1" } }
2016-03-05T12:07:44.630-0300 I STORAGE  [initandlisten] wiredtiger_open config: create,cache_size=4G,session_max=20000,eviction=(threads_max=4),config_base=false,statistics=(fast),log=(enabled=true,archive=true,path=journal,compressor=snappy),file_manager=(close_idle_time=100000),checkpoint=(wait=60,log_size=2GB),statistics_log=(wait=0),
2016-03-05T12:07:44.701-0300 I CONTROL  [initandlisten] ** WARNING: You are running this process as the root user, which is not recommended.
2016-03-05T12:07:44.701-0300 I CONTROL  [initandlisten] 
2016-03-05T12:07:44.701-0300 I CONTROL  [initandlisten] 
2016-03-05T12:07:44.701-0300 I CONTROL  [initandlisten] ** WARNING: /sys/kernel/mm/transparent_hugepage/enabled is 'always'.
2016-03-05T12:07:44.701-0300 I CONTROL  [initandlisten] **        We suggest setting it to 'never'
2016-03-05T12:07:44.701-0300 I CONTROL  [initandlisten] 
2016-03-05T12:07:44.701-0300 I CONTROL  [initandlisten] ** WARNING: /sys/kernel/mm/transparent_hugepage/defrag is 'always'.
2016-03-05T12:07:44.701-0300 I CONTROL  [initandlisten] **        We suggest setting it to 'never'
2016-03-05T12:07:44.701-0300 I CONTROL  [initandlisten] 
2016-03-05T12:07:44.702-0300 I FTDC     [initandlisten] Initializing full-time diagnostic data capture with directory '/data/shard1/diagnostic.data'
2016-03-05T12:07:44.702-0300 I NETWORK  [HostnameCanonicalizationWorker] Starting hostname canonicalization worker
2016-03-05T12:07:44.716-0300 I NETWORK  [initandlisten] waiting for connections on port 27013
```
####**Shard2**
```js
lucas@lucas-pc:~$ sudo mongod --port 27014 --dbpath /data/shard2
[sudo] password for lucas: 
2016-03-05T12:08:29.809-0300 I CONTROL  [initandlisten] MongoDB starting : pid=31992 port=27014 dbpath=/data/shard2 64-bit host=lucas-pc
2016-03-05T12:08:29.809-0300 I CONTROL  [initandlisten] db version v3.2.3
2016-03-05T12:08:29.809-0300 I CONTROL  [initandlisten] git version: b326ba837cf6f49d65c2f85e1b70f6f31ece7937
2016-03-05T12:08:29.809-0300 I CONTROL  [initandlisten] OpenSSL version: OpenSSL 1.0.2d 9 Jul 2015
2016-03-05T12:08:29.809-0300 I CONTROL  [initandlisten] allocator: tcmalloc
2016-03-05T12:08:29.809-0300 I CONTROL  [initandlisten] modules: none
2016-03-05T12:08:29.809-0300 I CONTROL  [initandlisten] build environment:
2016-03-05T12:08:29.809-0300 I CONTROL  [initandlisten]     distmod: ubuntu1404
2016-03-05T12:08:29.809-0300 I CONTROL  [initandlisten]     distarch: x86_64
2016-03-05T12:08:29.809-0300 I CONTROL  [initandlisten]     target_arch: x86_64
2016-03-05T12:08:29.809-0300 I CONTROL  [initandlisten] options: { net: { port: 27014 }, storage: { dbPath: "/data/shard2" } }
2016-03-05T12:08:29.835-0300 I STORAGE  [initandlisten] wiredtiger_open config: create,cache_size=4G,session_max=20000,eviction=(threads_max=4),config_base=false,statistics=(fast),log=(enabled=true,archive=true,path=journal,compressor=snappy),file_manager=(close_idle_time=100000),checkpoint=(wait=60,log_size=2GB),statistics_log=(wait=0),
2016-03-05T12:08:29.906-0300 I CONTROL  [initandlisten] ** WARNING: You are running this process as the root user, which is not recommended.
2016-03-05T12:08:29.906-0300 I CONTROL  [initandlisten] 
2016-03-05T12:08:29.906-0300 I CONTROL  [initandlisten] 
2016-03-05T12:08:29.906-0300 I CONTROL  [initandlisten] ** WARNING: /sys/kernel/mm/transparent_hugepage/enabled is 'always'.
2016-03-05T12:08:29.907-0300 I CONTROL  [initandlisten] **        We suggest setting it to 'never'
2016-03-05T12:08:29.907-0300 I CONTROL  [initandlisten] 
2016-03-05T12:08:29.907-0300 I CONTROL  [initandlisten] ** WARNING: /sys/kernel/mm/transparent_hugepage/defrag is 'always'.
2016-03-05T12:08:29.907-0300 I CONTROL  [initandlisten] **        We suggest setting it to 'never'
2016-03-05T12:08:29.907-0300 I CONTROL  [initandlisten] 
2016-03-05T12:08:29.907-0300 I FTDC     [initandlisten] Initializing full-time diagnostic data capture with directory '/data/shard2/diagnostic.data'
2016-03-05T12:08:29.907-0300 I NETWORK  [HostnameCanonicalizationWorker] Starting hostname canonicalization worker
2016-03-05T12:08:29.921-0300 I NETWORK  [initandlisten] waiting for connections on port 27014


```
####**Shard3**
```js
lucas@lucas-pc:~$ sudo mongod --port 27015 --dbpath /data/shard3
[sudo] password for lucas: 
2016-03-05T12:08:59.824-0300 I CONTROL  [initandlisten] MongoDB starting : pid=32405 port=27015 dbpath=/data/shard3 64-bit host=lucas-pc
2016-03-05T12:08:59.824-0300 I CONTROL  [initandlisten] db version v3.2.3
2016-03-05T12:08:59.824-0300 I CONTROL  [initandlisten] git version: b326ba837cf6f49d65c2f85e1b70f6f31ece7937
2016-03-05T12:08:59.824-0300 I CONTROL  [initandlisten] OpenSSL version: OpenSSL 1.0.2d 9 Jul 2015
2016-03-05T12:08:59.824-0300 I CONTROL  [initandlisten] allocator: tcmalloc
2016-03-05T12:08:59.824-0300 I CONTROL  [initandlisten] modules: none
2016-03-05T12:08:59.824-0300 I CONTROL  [initandlisten] build environment:
2016-03-05T12:08:59.824-0300 I CONTROL  [initandlisten]     distmod: ubuntu1404
2016-03-05T12:08:59.824-0300 I CONTROL  [initandlisten]     distarch: x86_64
2016-03-05T12:08:59.824-0300 I CONTROL  [initandlisten]     target_arch: x86_64
2016-03-05T12:08:59.824-0300 I CONTROL  [initandlisten] options: { net: { port: 27015 }, storage: { dbPath: "/data/shard3" } }
2016-03-05T12:08:59.850-0300 I STORAGE  [initandlisten] wiredtiger_open config: create,cache_size=4G,session_max=20000,eviction=(threads_max=4),config_base=false,statistics=(fast),log=(enabled=true,archive=true,path=journal,compressor=snappy),file_manager=(close_idle_time=100000),checkpoint=(wait=60,log_size=2GB),statistics_log=(wait=0),
2016-03-05T12:08:59.923-0300 I CONTROL  [initandlisten] ** WARNING: You are running this process as the root user, which is not recommended.
2016-03-05T12:08:59.923-0300 I CONTROL  [initandlisten] 
2016-03-05T12:08:59.923-0300 I CONTROL  [initandlisten] 
2016-03-05T12:08:59.923-0300 I CONTROL  [initandlisten] ** WARNING: /sys/kernel/mm/transparent_hugepage/enabled is 'always'.
2016-03-05T12:08:59.923-0300 I CONTROL  [initandlisten] **        We suggest setting it to 'never'
2016-03-05T12:08:59.923-0300 I CONTROL  [initandlisten] 
2016-03-05T12:08:59.923-0300 I CONTROL  [initandlisten] ** WARNING: /sys/kernel/mm/transparent_hugepage/defrag is 'always'.
2016-03-05T12:08:59.923-0300 I CONTROL  [initandlisten] **        We suggest setting it to 'never'
2016-03-05T12:08:59.923-0300 I CONTROL  [initandlisten] 
2016-03-05T12:08:59.923-0300 I FTDC     [initandlisten] Initializing full-time diagnostic data capture with directory '/data/shard3/diagnostic.data'
2016-03-05T12:08:59.923-0300 I NETWORK  [HostnameCanonicalizationWorker] Starting hostname canonicalization worker
2016-03-05T12:08:59.937-0300 I NETWORK  [initandlisten] waiting for connections on port 27015

```
**Registrar Shards no Router**
```js
lucas@lucas-pc:~$ mongo --port 27012 --host localhost
MongoDB shell version: 3.2.3
connecting to: localhost:27012/test
Mongo-Hacker 0.0.12
lucas-pc:27012(mongos-3.2.3)[mongos] test> sh.addShard("localhost:27013")
{
  "shardAdded": "shard0000",
  "ok": 1
}
lucas-pc:27012(mongos-3.2.3)[mongos] test> sh.addShard("localhost:27014")
{
  "shardAdded": "shard0001",
  "ok": 1
}
lucas-pc:27012(mongos-3.2.3)[mongos] test> sh.addShard("localhost:27015")
{
  "shardAdded": "shard0002",
  "ok": 1
}


test> sh.enableSharding("be-mean")
{
  "ok": 1
}

lucas-pc:27012(mongos-3.2.3)[mongos] test> sh.shardCollection("be-mean.projects", { "_id": 1 })
{
  "collectionsharded": "be-mean.projects",
  "ok": 1
}

```

## **Replica**
```
lucas@lucas-pc:~$ mkdir /data/rs1 && mkdir /data/rs2 && mkdir /data/rs3
```

### **Replica 1**
```js
lucas@lucas-pc:~$ sudo mongod --replSet replica_set --port 27030 --dbpath /data/rs1
[sudo] password for lucas: 
2016-03-05T12:13:56.813-0300 I CONTROL  [initandlisten] MongoDB starting : pid=1121 port=27030 dbpath=/data/rs1 64-bit host=lucas-pc
2016-03-05T12:13:56.813-0300 I CONTROL  [initandlisten] db version v3.2.3
2016-03-05T12:13:56.813-0300 I CONTROL  [initandlisten] git version: b326ba837cf6f49d65c2f85e1b70f6f31ece7937
2016-03-05T12:13:56.813-0300 I CONTROL  [initandlisten] OpenSSL version: OpenSSL 1.0.2d 9 Jul 2015
2016-03-05T12:13:56.813-0300 I CONTROL  [initandlisten] allocator: tcmalloc
2016-03-05T12:13:56.813-0300 I CONTROL  [initandlisten] modules: none
2016-03-05T12:13:56.813-0300 I CONTROL  [initandlisten] build environment:
2016-03-05T12:13:56.813-0300 I CONTROL  [initandlisten]     distmod: ubuntu1404
2016-03-05T12:13:56.813-0300 I CONTROL  [initandlisten]     distarch: x86_64
2016-03-05T12:13:56.813-0300 I CONTROL  [initandlisten]     target_arch: x86_64
2016-03-05T12:13:56.813-0300 I CONTROL  [initandlisten] options: { net: { port: 27030 }, replication: { replSet: "replica_set" }, storage: { dbPath: "/data/rs1" } }
2016-03-05T12:13:56.840-0300 I STORAGE  [initandlisten] wiredtiger_open config: create,cache_size=4G,session_max=20000,eviction=(threads_max=4),config_base=false,statistics=(fast),log=(enabled=true,archive=true,path=journal,compressor=snappy),file_manager=(close_idle_time=100000),checkpoint=(wait=60,log_size=2GB),statistics_log=(wait=0),
2016-03-05T12:13:56.912-0300 I CONTROL  [initandlisten] ** WARNING: You are running this process as the root user, which is not recommended.
2016-03-05T12:13:56.912-0300 I CONTROL  [initandlisten] 
2016-03-05T12:13:56.912-0300 I CONTROL  [initandlisten] 
2016-03-05T12:13:56.912-0300 I CONTROL  [initandlisten] ** WARNING: /sys/kernel/mm/transparent_hugepage/enabled is 'always'.
2016-03-05T12:13:56.912-0300 I CONTROL  [initandlisten] **        We suggest setting it to 'never'
2016-03-05T12:13:56.912-0300 I CONTROL  [initandlisten] 
2016-03-05T12:13:56.912-0300 I CONTROL  [initandlisten] ** WARNING: /sys/kernel/mm/transparent_hugepage/defrag is 'always'.
2016-03-05T12:13:56.912-0300 I CONTROL  [initandlisten] **        We suggest setting it to 'never'
2016-03-05T12:13:56.912-0300 I CONTROL  [initandlisten] 
2016-03-05T12:13:56.926-0300 I REPL     [initandlisten] Did not find local voted for document at startup;  NoMatchingDocument: Did not find replica set lastVote document in local.replset.election
2016-03-05T12:13:56.926-0300 I REPL     [initandlisten] Did not find local replica set configuration document at startup;  NoMatchingDocument: Did not find replica set configuration document in local.system.replset
2016-03-05T12:13:56.926-0300 I NETWORK  [HostnameCanonicalizationWorker] Starting hostname canonicalization worker
2016-03-05T12:13:56.926-0300 I FTDC     [initandlisten] Initializing full-time diagnostic data capture with directory '/data/rs1/diagnostic.data'
2016-03-05T12:13:56.943-0300 I NETWORK  [initandlisten] waiting for connections on port 27030

```

### **Replica 2**
```js
lucas@lucas-pc:~$ sudo mongod --replSet replica_set --port 27031 --dbpath /data/rs2
[sudo] password for lucas: 
2016-03-05T12:15:27.678-0300 I CONTROL  [initandlisten] MongoDB starting : pid=1761 port=27031 dbpath=/data/rs2 64-bit host=lucas-pc
2016-03-05T12:15:27.678-0300 I CONTROL  [initandlisten] db version v3.2.3
2016-03-05T12:15:27.678-0300 I CONTROL  [initandlisten] git version: b326ba837cf6f49d65c2f85e1b70f6f31ece7937
2016-03-05T12:15:27.678-0300 I CONTROL  [initandlisten] OpenSSL version: OpenSSL 1.0.2d 9 Jul 2015
2016-03-05T12:15:27.678-0300 I CONTROL  [initandlisten] allocator: tcmalloc
2016-03-05T12:15:27.678-0300 I CONTROL  [initandlisten] modules: none
2016-03-05T12:15:27.678-0300 I CONTROL  [initandlisten] build environment:
2016-03-05T12:15:27.678-0300 I CONTROL  [initandlisten]     distmod: ubuntu1404
2016-03-05T12:15:27.678-0300 I CONTROL  [initandlisten]     distarch: x86_64
2016-03-05T12:15:27.678-0300 I CONTROL  [initandlisten]     target_arch: x86_64
2016-03-05T12:15:27.678-0300 I CONTROL  [initandlisten] options: { net: { port: 27031 }, replication: { replSet: "replica_set" }, storage: { dbPath: "/data/rs2" } }
2016-03-05T12:15:27.704-0300 I STORAGE  [initandlisten] wiredtiger_open config: create,cache_size=4G,session_max=20000,eviction=(threads_max=4),config_base=false,statistics=(fast),log=(enabled=true,archive=true,path=journal,compressor=snappy),file_manager=(close_idle_time=100000),checkpoint=(wait=60,log_size=2GB),statistics_log=(wait=0),
2016-03-05T12:15:27.780-0300 I CONTROL  [initandlisten] ** WARNING: You are running this process as the root user, which is not recommended.
2016-03-05T12:15:27.780-0300 I CONTROL  [initandlisten] 
2016-03-05T12:15:27.780-0300 I CONTROL  [initandlisten] 
2016-03-05T12:15:27.780-0300 I CONTROL  [initandlisten] ** WARNING: /sys/kernel/mm/transparent_hugepage/enabled is 'always'.
2016-03-05T12:15:27.780-0300 I CONTROL  [initandlisten] **        We suggest setting it to 'never'
2016-03-05T12:15:27.780-0300 I CONTROL  [initandlisten] 
2016-03-05T12:15:27.780-0300 I CONTROL  [initandlisten] ** WARNING: /sys/kernel/mm/transparent_hugepage/defrag is 'always'.
2016-03-05T12:15:27.780-0300 I CONTROL  [initandlisten] **        We suggest setting it to 'never'
2016-03-05T12:15:27.780-0300 I CONTROL  [initandlisten] 
2016-03-05T12:15:27.794-0300 I REPL     [initandlisten] Did not find local voted for document at startup;  NoMatchingDocument: Did not find replica set lastVote document in local.replset.election
2016-03-05T12:15:27.794-0300 I REPL     [initandlisten] Did not find local replica set configuration document at startup;  NoMatchingDocument: Did not find replica set configuration document in local.system.replset
2016-03-05T12:15:27.794-0300 I NETWORK  [HostnameCanonicalizationWorker] Starting hostname canonicalization worker
2016-03-05T12:15:27.794-0300 I FTDC     [initandlisten] Initializing full-time diagnostic data capture with directory '/data/rs2/diagnostic.data'
2016-03-05T12:15:27.808-0300 I NETWORK  [initandlisten] waiting for connections on port 27031

```

### **Replica 3**
```js
lucas@lucas-pc:~$ sudo mongod --replSet replica_set --port 27032 --dbpath /data/rs3
[sudo] password for lucas: 
2016-03-05T12:15:47.150-0300 I CONTROL  [initandlisten] MongoDB starting : pid=2326 port=27032 dbpath=/data/rs3 64-bit host=lucas-pc
2016-03-05T12:15:47.150-0300 I CONTROL  [initandlisten] db version v3.2.3
2016-03-05T12:15:47.150-0300 I CONTROL  [initandlisten] git version: b326ba837cf6f49d65c2f85e1b70f6f31ece7937
2016-03-05T12:15:47.150-0300 I CONTROL  [initandlisten] OpenSSL version: OpenSSL 1.0.2d 9 Jul 2015
2016-03-05T12:15:47.150-0300 I CONTROL  [initandlisten] allocator: tcmalloc
2016-03-05T12:15:47.150-0300 I CONTROL  [initandlisten] modules: none
2016-03-05T12:15:47.150-0300 I CONTROL  [initandlisten] build environment:
2016-03-05T12:15:47.150-0300 I CONTROL  [initandlisten]     distmod: ubuntu1404
2016-03-05T12:15:47.150-0300 I CONTROL  [initandlisten]     distarch: x86_64
2016-03-05T12:15:47.150-0300 I CONTROL  [initandlisten]     target_arch: x86_64
2016-03-05T12:15:47.150-0300 I CONTROL  [initandlisten] options: { net: { port: 27032 }, replication: { replSet: "replica_set" }, storage: { dbPath: "/data/rs3" } }
2016-03-05T12:15:47.176-0300 I STORAGE  [initandlisten] wiredtiger_open config: create,cache_size=4G,session_max=20000,eviction=(threads_max=4),config_base=false,statistics=(fast),log=(enabled=true,archive=true,path=journal,compressor=snappy),file_manager=(close_idle_time=100000),checkpoint=(wait=60,log_size=2GB),statistics_log=(wait=0),
2016-03-05T12:15:47.254-0300 I CONTROL  [initandlisten] ** WARNING: You are running this process as the root user, which is not recommended.
2016-03-05T12:15:47.254-0300 I CONTROL  [initandlisten] 
2016-03-05T12:15:47.254-0300 I CONTROL  [initandlisten] 
2016-03-05T12:15:47.254-0300 I CONTROL  [initandlisten] ** WARNING: /sys/kernel/mm/transparent_hugepage/enabled is 'always'.
2016-03-05T12:15:47.254-0300 I CONTROL  [initandlisten] **        We suggest setting it to 'never'
2016-03-05T12:15:47.254-0300 I CONTROL  [initandlisten] 
2016-03-05T12:15:47.254-0300 I CONTROL  [initandlisten] ** WARNING: /sys/kernel/mm/transparent_hugepage/defrag is 'always'.
2016-03-05T12:15:47.254-0300 I CONTROL  [initandlisten] **        We suggest setting it to 'never'
2016-03-05T12:15:47.254-0300 I CONTROL  [initandlisten] 
2016-03-05T12:15:47.268-0300 I REPL     [initandlisten] Did not find local voted for document at startup;  NoMatchingDocument: Did not find replica set lastVote document in local.replset.election
2016-03-05T12:15:47.268-0300 I REPL     [initandlisten] Did not find local replica set configuration document at startup;  NoMatchingDocument: Did not find replica set configuration document in local.system.replset
2016-03-05T12:15:47.268-0300 I FTDC     [initandlisten] Initializing full-time diagnostic data capture with directory '/data/rs3/diagnostic.data'
2016-03-05T12:15:47.268-0300 I NETWORK  [HostnameCanonicalizationWorker] Starting hostname canonicalization worker
2016-03-05T12:15:47.289-0300 I NETWORK  [initandlisten] waiting for connections on port 27032

```

### **Config**
```js
lucas@lucas-pc:~$ mongo --port 27030

 var rsconf = {
      _id: "replica_set",
      members: [
          {
              _id: 0,
              host: "127.0.0.1:27030"
          }
      ]
  }
lucas-pc:27030(mongod-3.2.3) test> rs.initiate(rsconf)
{
  "ok": 1
}

lucas-pc:27030(mongod-3.2.3)[SECONDARY:replica_set] test> rs.add("127.0.0.1:27031")
{
  "ok": 1
}
lucas-pc:27030(mongod-3.2.3)[PRIMARY:replica_set] test> rs.add("127.0.0.1:27032")
{
  "ok": 1
}

```
