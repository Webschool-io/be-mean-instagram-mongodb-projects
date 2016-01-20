# MongoDb - Projeto Final
**Autor:** Ronaldo Aragão
**Data** Date.now() //em timestamp

## Para qual sistema você usaria o MongoDB (diferente desse)?

Não vejo sistema que não possa usar o mongo, desde que seja bem modelado.

## Qual a modelagem da sua coleção de `users`?
```js
{
		_id: ObjectId,
		name: String,
		bio: String,
		date_register: Date,
		auth: {
				username: String,
				email: String,
				password: String,
				last_access: Date,
				online: Boolean,
				disabled: Boolean,
				hash_token: String,
		},
		profile_image: String,
		background_image: String
}
```

## Qual a modelagem da sua coleção de `projects`?
```js
{
	_id: Object ID,
	name: String,
	description: String,
	date_begin: Date,
	date_dream: Date,
	date_end: Date,
	visible: Boolean,
	expired: Boolean,
	visualizable: Boolean,
	members: [{
		user_id: Object ID,
		type: String,
		notify: Boolean
	}],
	goals: [{
		name: String,
		description: String,
		date_begin: Date,
		date_dream: Date,
		date_end: Date,
		realocate: Boolean,
		expired: Boolean,
		historic: [Date],
		tags: [String],
		historic: [Date],
		activities:[],
		comments: [{
			id_comment: Object ID,
			text: String,
			date_comment: Date,
			member: Object ID,
			file: {
				path: String,
				weight: String,
				name: String
			}
		}]	
	}],	
	tags: [String],
}
```

## Qual a modelagem da sua coleção retirada de `projects`?

Retirei a coleção `activitiers` porque, de acordo com a minha ideia do sistema, uma mesma atctivity pode estar em mais de uma `goal`. Como as `activitiers` possuim muitos campos, criei uma nova coleção para reaproveita-las.   

```js
{
	_id: Object ID,
	name: String,
	description: String,
	date_begin: Date,
	date_dream: Date,
	date_end: Date,
	realocate: Boolean,
	expired: Boolean,
	tags: [String],
	members: [{
		user_id: Object ID
	}],
	historic: [Date],
	comments: [{
		id_comment: Object ID,
		text: String,
		date_comment: Date,
		member: Object ID,
		file: {
			path: String,
			weight: String,
			name: String
		}
	}]
}

```

## Create - cadastro

#### 1. Cadastre 10 usuários diferentes.
```js
var usuarios = [
	{
		nome: "Ronaldo Aragão", 
		user: "ronaldoarg" },
	{
		nome: "Raphael Carmo", 
		user: "raphaelcomph" },
	{
		nome: "Rafaell Soler", 
		user: "rafasoler"},
	{
		nome: "Gabriel Leal", 
		user: "gbrlleal"},
  {
		nome: "Zeus Lopes", 
		user: "zeuslps"},
	{
		nome: "Giselle Fonseca", 
		user: "gisellefsc"},
	{
		nome: "Lene Aragão", 
		user: "lenearg"},
	{
		nome: "Marcus Botelho", 
		user: "mvcbotelho"},
	{
		nome: "Lucas de Preto", 
		user: "lucasdepreto"},
	{
		nome: "Mauany Alencar", 
		user: "mauany"}]

var listaUsuarios = []

usuarios.forEach(function(u){
		var query = {
				name: u.nome,
				bio: "Meu nome é "+u.nome,
				date_register: new Date(),
				auth: {
						username: u.user,
						email: u.user+"@gmail.com",
						password: "",
						last_access: new Date(),
						online: true,
						disabled: true,
						hash_token: "04gf65d40g65f4d0654re0t4r5",
				},
				perfil: null,
				capa: null
		}
		listaUsuarios.push(query)
})
db.users.insert(listaUsuarios)

Inserted 1 record(s) in 804ms
BulkWriteResult({
  "writeErrors": [ ],
  "writeConcernErrors": [ ],
  "nInserted": 10,
  "nUpserted": 0,
  "nMatched": 0,
  "nModified": 0,
  "nRemoved": 0,
  "upserted": [ ]
})
```

**Parei aqui**

## Create - cadastro
1. Cadastre 10 usuários diferentes.
2. Cadastre 5 projetos diferentes.
    - cada um com 5 membros, sempre diferentes dentro dos projetos;
    - cada um com pelo menos 3 tags diferentes;
        - escolha 1 *tag* onde deva ficar em 2 projetos;
        - escolha 1 *tag* onde deva ficar em 3 projetos;
    - cada projeto com pelo menos 1 *goal*;
        - cada *goal* com pelo menos 3 *tags*;
        - cada *goal* com pelo menos 2 atividades, deixe 1 projeto sem.


## Retrieve - busca

## Update - alteração

## Delete - remoção

## Sharding
// coloque aqui todos os comandos que você executou

## Replica
// coloque aqui todos os comandos que você executou
