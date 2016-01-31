# MongoDb - Projeto Final
**Autor:** Welder Marcos
**Data** 1454201680248

## Para qual sistema você usaria o MogoDB (diferente desse)?

Usaria para um sistema de gerenciamento de notícias, pois acredito que o formato de documento seria o mais adequado para notícias;

## Qual a modelagem da sua coleção de `users`?
```
User: {
	name: String,
	bio: String,
	date-register: Date,
	avatar: Binary,
	background: Binary,
	
	auth:{
		username: String,
		email: String,
		password: String,
		last_acess: Date,
		online: Boolean,
		disabled: Boolean,
		hash-token: String
	}	
}
```

## Qual a modelagem da sua coleção de `projects`?
```
Project:{
	
	name: String,
	description: String,
	date_begin: Date,
	date_dream: Date,
	date_end: Date,
	visible: Boolean,
	realocate: Date,
	expired: Date,
	visualizable_mod: Boolean,
	tags: [String],
	
	members:[{
		type: String,
		_id: ObjectId,
		notify: String
	}],
	
	Goals:{
		name: String,
		tags: [String],
		description: String,
		date_begin: Date,
		date_dream: Date,
		date_end: Date,
		realocate: Date,
		expired: Date,
		historic: [Date],
		
		activity:[{
			_id: ObjectId
		}]
	}	
}
```

## Qual a modelagem da sua coleção retirada de `projects`?
```
Activity:{
	
	name: String,
	tags: [String],
	description: String,
	date_begin: Date,
	date_dream: Date,
	date_end: Date,	
	realocate: Date,
	expired: Date,
	
	comments:[{
		text: String,
		date_comment: Date,
		
		files: [{		
			name: String,
			weight: String,
			path: String
		}]
	}]
}
```

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

1. Liste as informações dos membros de 1 projeto específico que deve ser buscado pelo seu nome de forma a não ligar para maiúsculas e minúsculas.
2. Liste todos os projetos com a tag que você escolheu para os 3 projetos em comum.
3. Liste apenas os nomes de todas as atividades para todos os projetos.
4. Liste todos os projetos que não possuam uma tag.
5. Liste todos os usuários que não fazem parte do primeiro projeto cadastrado.

## Update - alteração

1. Adicione para todos os projetos o campo `views: 0`.
2. Adicione 1 tag diferente para cada projeto.
3. Adicione 2 membros diferentes para cada projeto.
4. Adicione 1 comentário em cada atividade, deixe apenas 1 projeto sem.
5. Adicione 1 projeto inteiro com **UPSERT**.

## Delete - remoção

1. Apague todos os projetos que não possuam *tags*.
2. Apague todos os projetos que não possuam comentários nas atividades.
3. Apague todos os projetos que não possuam atividades.
4. Escolha 2 usuário e apague todos os projetos em que os 2 fazem parte.
5. Apague todos os projetos que possuam uma determinada *tag* em *goal*.

## Sharding

- 1 Router
- 1 Config Server
- 3 Shardings

## Replica

- 3 Replicas
Você deverá escolher qual sua coleção deverá ser *shardeada* para poder aguentar muita carga repentinamente e deverá replicar cada Shard, pode ser feito localmente como em alguma VPS FREE.
