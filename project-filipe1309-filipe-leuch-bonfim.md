# MongoDb - Projeto Final
**Autor:** Filipe Leuch Bonfim
**Data** 1454879064765

## Para qual sistema você usaria o MogoDB (diferente desse)?
Acredito que o MongoDB seja melhor aproveitado em sistemas que necessitam ser escaláveis, e que possuem uma modelagem com poucos relacionamentos.

## Qual a modelagem da sua coleção de `users`?
Nesta modelagem, as tabelas ``user``, ``settings-system`` e ``auth`` foram unidas em somente uma coleção chamada ``users``.

```javascript
users = {
    name: String,
    bio: String,
    date_register: Date,
    avatar_path: String,
    auth: {
        username: String,
        email: String,
        password: String,
        last_access: Date,
        online: Boolean,
        disabled: Boolean,
        has_token: Boolean
    },
    settings_system: { background_path: String }
}
```

## Qual a modelagem da sua coleção de `projects`?
Nesta modelagem, as tabelas ``project``, ``project-tag``, ``tag``, ``goal``, ``goal-tags`` e ``goal-historic`` foram unidas em somente uma coleção chamada ``projects``.

```javascript
projects = {
    name: String,
    description: String,
    date_begin: Date,
    date_dream: Date,
    date_end: Date,
    visible: Boolean,
    realocate: Boolean,
    expired: Boolean,
    visualizable_mod: String
    tags:  [],
    goals: [
        {
            name: String,
            description: String,
            date_begin: Date,
            date_dream: Date,
            date_end: Date,
            realocate: Boolean,
            expired: Boolean,
            goal_historic: { date_realocate: Date },
            activities: [
                { activity_id: String }
            ],
            tags: []
        }
    ],
    members_project:  [
        {
            user_id: String,
            notify: Boolean,
            type_member: { type_name: String }
        }
    ]
}
```

## Qual a modelagem da sua coleção retirada de `projects`?
A coleção retida de `projects` foi a `activity`, pois pode vir a armazenar uma quantidade considerável de dados. Esta coleção foi formada através das tabelas ``activity``, ``activity-historic``, ``members-activity``, ``activity-tag``, ``comment``, ``members-comment`` e ``file``.

```javascript
activities = {
    name: String,
    description: String,
    date_dream: Date,
    date_end: Date,
    visible: Boolean,
    realocate: Boolean,
    expired: Boolean,
    activity_historic: { date_realocate: Date },
    tags: [],
    members_activities: [
        {
            user_id: String,
            notify: Boolean,
            type_member: { type_name: String }
        }
    ],
    comments: [
        {
            text: String,
            date_comment: Date,
            files: [
                {
                    path: String,
                    weight: Number,
                    name: String,
                }
            ],
            members_comment: [
                {
                    member_project: {
                        user_id: String,
                        notify: Boolean,
                        type_member: { type_name: String }
                    }
                    notify: Boolean,
                }
            ],
        }
    ]
}
```

## Create - cadastro
#### 1. Cadastre 10 usuários diferentes.

```javascript
    // 1
    db.users.save({
        name: "Ozob",
        bio: "replicante",
        date_register: new Date(2119,04,01),
        avatar_path: "/user/ozob",
        auth: {
            username: "ozob",
            email: "ozob@bozo.azaghal",
            password: "babaca",
            last_access: new Date(2119,04,01),
            online: 0,
            disabled: 0,
            has_token: 0
        },
        settings_system: { background_path: "/user/ozob/bg" }
    })

    // 2
    db.users.save({
        name: "James Rexus",
        bio: "humano",
        date_register: new Date(2119,04,01),
        avatar_path: "/user/rexus",
        auth: {
            username: "rexus",
            email: "james@rexus.rex",
            password: "obabaca",
            last_access: new Date(2119,04,01),
            online: 1,
            disabled: 0,
            has_token: 0
        },
        settings_system: { background_path: "/user/rexus/bg" }
    })

    // 3
    db.users.save({
        name: "Android",
        bio: "ndr",
        date_register: new Date(2119,04,01),
        avatar_path: "/user/ndr",
        auth: {
            username: "ndr",
            email: "ndr@lollita.andrew",
            password: "gothicLollita",
            last_access: new Date(2119,04,01),
            online: 1,
            disabled: 0,
            has_token: 0
        },
        settings_system: { background_path: "/user/ndr/bg" }
    })

    // 4
    db.users.save({
        name: "Oleg",
        bio: "russo",
        date_register: new Date(2119,04,01),
        avatar_path: "/user/oleg",
        auth: {
            username: "oleg",
            email: "oleg@urss.tucano",
            password: "urss",
            last_access: new Date(2119,04,01),
            online: 0,
            disabled: 0,
            has_token: 0
        },
        settings_system: { background_path: "/user/oleg/bg" }
    })

    // 5
    db.users.save({
        name: "Steve Durden",
        bio: "humano",
        date_register: new Date(2119,04,01),
        avatar_path: "/user/durden",
        auth: {
            username: "durden",
            email: "durden@steve.carlosvoltor",
            password: "machine",
            last_access: new Date(2119,04,01),
            online: 0,
            disabled: 0,
            has_token: 0
        },
        settings_system: { background_path: "/user/durden/bg" }
    })

    // 6
    db.users.save({
        name: "Dr. Silvana",
        bio: "humano",
        date_register: new Date(2119,04,01),
        avatar_path: "/user/silvana",
        auth: {
            username: "silvana",
            email: "silvana@ironman.jp",
            password: "rica",
            last_access: new Date(2119,04,01),
            online: 1,
            disabled: 0,
            has_token: 0
        },
        settings_system: { background_path: "/user/silvana/bg" }
    })

    // 7
    db.users.save({
        name: "Jovem Nerd",
        bio: "mestre",
        date_register: new Date(2119,04,01),
        avatar_path: "/user/jn",
        auth: {
            username: "jovemnerd",
            email: "jovemnerd@talco.com",
            password: "talcoepomada",
            last_access: new Date(2119,04,01),
            online: 0,
            disabled: 0,
            has_token: 0
        },
        settings_system: { background_path: "/user/jn/bg" }
    })

    // 8
    db.users.save({
        name: "Hirawata",
        bio: "muito rica",
        date_register: new Date(2119,04,01),
        avatar_path: "/user/hirawata",
        auth: {
            username: "hirawata",
            email: "hirawata@hirawata.com",
            password: "hirawata",
            last_access: new Date(2119,04,01),
            online: 0,
            disabled: 0,
            has_token: 0
        },
        settings_system: { background_path: "/user/hirawata/bg" }
    })

    // 9
    db.users.save({
        name: "Ulib",
        bio: "ovni",
        date_register: new Date(2119,04,01),
        avatar_path: "/user/ulib",
        auth: {
            username: "ulib",
            email: "ulib@bilu.3d",
            password: "kenny",
            last_access: new Date(2119,04,01),
            online: 0,
            disabled: 0,
            has_token: 0
        },
        settings_system: { background_path: "/user/ulib/bg" }
    })

    // 10
    db.users.save({
        name: "Cybro",
        bio: "robo",
        date_register: new Date(2252,04,01),
        avatar_path: "/user/cybro",
        auth: {
            username: "cybro",
            email: "cybro@robo.future",
            password: "future",
            last_access: new Date(2252,04,01),
            online: 0,
            disabled: 0,
            has_token: 0
        },
        settings_system: { background_path: "/user/cybro/bg" }
    })

```

#### 2. Cadastre 5 projetos diferentes.
    - cada um com 5 membros, sempre diferentes dentro dos projetos;
    - cada um com pelo menos 3 tags diferentes;
        - escolha 1 *tag* onde deva ficar em 2 projetos;
        ``token mental``
        - escolha 1 *tag* onde deva ficar em 3 projetos;
        ``Cyberpunk``
    - cada projeto com pelo menos 1 *goal*;
        - cada *goal* com pelo menos 3 *tags*;
        - cada *goal* com pelo menos 2 atividades, deixe 1 projeto sem.

```javascript
// Definindo as atividades dos projetos
var activities = [
  {
    name: "infiltrar na festa",
    description: "encontrar um meio para se infiltrar na festa",
    members_activities: [
        {
            user_id: ObjectId("56a50ce624eacf14e8493d56"),
            notify: false,
            type_member: { type_name: "type-0" }
        },
        {
            user_id: ObjectId("56a50cdc24eacf14e8493d54"),
            notify: false,
            type_member: { type_name: "type-0" }
        },
    ],
  },
  {
    name: "encontrar o portador do token mental",
    description: "montar uma estratégia que possibilite a obtenção do token mental",
    members_activities: [
        {
            user_id: ObjectId("56a50cc424eacf14e8493d50"),
            notify: false,
            type_member: { type_name: "type-0" }
        },
        {
            user_id: ObjectId("56a50cd124eacf14e8493d52"),
            notify: false,
            type_member: { type_name: "type-0" }
        },
    ],
  },
  {
    name: "escapar da festa",
    description: "escapar da festa em posse do token mental",
    members_activities: [
        {
            user_id: ObjectId("56a50cd124eacf14e8493d52"),
            notify: false,
            type_member: { type_name: "type-0" }
        },
        {
            user_id: ObjectId("56a50ce624eacf14e8493d56"),
            notify: false,
            type_member: { type_name: "type-0" }
        },
    ],
  },
  {
    name: "encontrar o container",
    description: "se infiltrar nas naves da hirawata, para encontrar o container que contém a carga misteriosa",
    members_activities: [
        {
            user_id: ObjectId("56a503e27d5b9da4738c268f"),
            notify: false,
            type_member: { type_name: "type-0" }
        },
        {
            user_id: ObjectId("56a50cdc24eacf14e8493d54"),
            notify: false,
            type_member: { type_name: "type-0" }
        },
    ],
  },
  {
    name: "obter carga misteriosa",
    description: "obter a carga misteriosa contida no container",
    members_activities: [
        {
            user_id: ObjectId("56a50cd724eacf14e8493d53"),
            notify: false,
            type_member: { type_name: "type-0" }
        },
        {
            user_id: ObjectId("56a50cd124eacf14e8493d52"),
            notify: false,
            type_member: { type_name: "type-0" }
        },
    ],
  },
  {
    name: "escapar das naves da hirawata corporation",
    description: "espacar com a carga misteriosa das naves da hirawata corporation",
    members_activities: [
        {
            user_id: ObjectId("56a50ccb24eacf14e8493d51"),
            notify: false,
            type_member: { type_name: "type-0" }
        },
        {
            user_id: ObjectId("56a50cf024eacf14e8493d58"),
            notify: false,
            type_member: { type_name: "type-0" }
        },
    ],
  },
  {
    name: "fugir para o esconderijo do oleg",
    description: "fugir, sem ser perseguido e com a posse da carga misteriosa, para o esconderijo do oleg",
    members_activities: [
        {
            user_id: ObjectId("56a50ce224eacf14e8493d55"),
            notify: false,
            type_member: { type_name: "type-0" }
        },
        {
            user_id: ObjectId("56a50cd124eacf14e8493d52"),
            notify: false,
            type_member: { type_name: "type-0" }
        },
    ],
  },
  {
    name: "descobrir a funcionalidade da carga misteriosa",
    description: "investigar a procedencia da carga misteriosa",
    members_activities: [
        {
            user_id: ObjectId("56a503e27d5b9da4738c268f"),
            notify: false,
            type_member: { type_name: "type-0" }
        },
        {
            user_id: ObjectId("56a50ceb24eacf14e8493d57"),
            notify: false,
            type_member: { type_name: "type-0" }
        },
    ],
  },
  {
    name: "se encontrar com Ulib",
    description: "se encontrar com Ulib para acessar o conteúdo do robo",
    members_activities: [
        {
            user_id: ObjectId("56a50ccb24eacf14e8493d51"),
            notify: false,
            type_member: { type_name: "type-0" }
        },
        {
            user_id: ObjectId("56a50ceb24eacf14e8493d57"),
            notify: false,
            type_member: { type_name: "type-0" }
        },
    ],
  },
  {
    name: "descobrir o conteúdo do robo",
    description: "utilizar a tecnoloagia de Ulib para entrar dentro da mente do robo",
    members_activities: [
        {
            user_id: ObjectId("56a50ccb24eacf14e8493d51"),
            notify: false,
            type_member: { type_name: "type-0" }
        },
        {
            user_id: ObjectId("56a50cf024eacf14e8493d58"),
            notify: false,
            type_member: { type_name: "type-0" }
        },
    ],
  },
  {
    name: "burlar as defesas do robo",
    description: "através do mecanismo do Ulib, burlar os firewalls internos do robo",
    members_activities: [
        {
            user_id: ObjectId("56a50ce224eacf14e8493d55"),
            notify: false,
            type_member: { type_name: "type-0" }
        },
        {
            user_id: ObjectId("56a503e27d5b9da4738c268f"),
            notify: false,
            type_member: { type_name: "type-0" }
        },
    ],
  },
  {
    name: "encontar o core",
    description: "encontrar um caminho para o core do robo",
    members_activities: [
        {
            user_id: ObjectId("56a50ceb24eacf14e8493d57"),
            notify: false,
            type_member: { type_name: "type-0" }
        },
        {
            user_id: ObjectId("56a503e27d5b9da4738c268f"),
            notify: false,
            type_member: { type_name: "type-0" }
        },
    ],
  },
];

activities.forEach(function(activity) {
  activity.date_dream = new Date(2252,04,01);
  activity.date_end = new Date(2252,04,01);
  activity.visible = true;
  activity.realocate = false;
  activity.expired = false;
  activity.activity_historic = { date_realocate: new Date(2252,04,01) };
  activity.tags = ["Cyberpunk"];
  db.activities.save(activity);
})

// Definindo os projetos
var projects = [
  {
      name: "Projeto - 1",
      description: "descrição - 1",
      date_begin: new Date(2252,04,01),
      date_dream: new Date(2252,04,01),
      date_end: new Date(2252,04,01),
      visible: true,
      realocate: false,
      expired: false,
      visualizable_mod: "1",
      tags:  ["token mental", "hirawata", "invasão", "Cyberpunk"],
      goals: [
          {
              name: "goal 1",
              description: "obter o token mental",
              date_begin: new Date(2252,04,01),
              date_dream: new Date(2252,04,01),
              date_end: new Date(2252,04,01),
              realocate: false,
              expired: false,
              goal_historic: { date_realocate: new Date(2252,04,01) },
              activities: [
                  { activity_id: ObjectId("56a590d06048375df3622426") },
                  { activity_id: ObjectId("56a590d06048375df3622427") },
                  { activity_id: ObjectId("56a590d06048375df3622428") },
              ],
              tags: ["alvo", "blad runner", "festa"]
          }
      ],
      members_project:  [
          {
              user_id: ObjectId("56a50ccb24eacf14e8493d51"),
              notify: true,
              type_member: { type_name: "type-0" }
          },
          {
              user_id: ObjectId("56a50cdc24eacf14e8493d54"),
              notify: true,
              type_member: { type_name: "type-0" }
          },
          {
              user_id: ObjectId("56a50ce624eacf14e8493d56"),
              notify: true,
              type_member: { type_name: "type-0" }
          },
          {
              user_id: ObjectId("56a50cc424eacf14e8493d50"),
              notify: true,
              type_member: { type_name: "type-0" }
          },
          {
              user_id: ObjectId("56a50cd124eacf14e8493d52"),
              notify: true,
              type_member: { type_name: "type-0" }
          },
      ]
  },
  {
      name: "Projeto - 2",
      description: "descrição - 2",
      date_begin: new Date(2252,04,01),
      date_dream: new Date(2252,04,01),
      date_end: new Date(2252,04,01),
      visible: true,
      realocate: false,
      expired: false,
      visualizable_mod: "1",
      tags:  ["token mental", "container", "invasão"],
      goals: [
          {
              name: "goal 1",
              description: "obter a carga valiosa do hirawata corporation",
              date_begin: new Date(2252,04,01),
              date_dream: new Date(2252,04,01),
              date_end: new Date(2252,04,01),
              realocate: false,
              expired: false,
              goal_historic: { date_realocate: new Date(2252,04,01) },
              activities: [
                  { activity_id: ObjectId("56a590d06048375df3622429") },
                  { activity_id: ObjectId("56a590d06048375df362242a") },
              ],
              tags: ["carga", "nave", "invasão"]
          }
      ],
      members_project:  [
          {
              user_id: ObjectId("56a50ce624eacf14e8493d56"),
              notify: true,
              type_member: { type_name: "type-0" }
          },
          {
              user_id: ObjectId("56a50cc424eacf14e8493d50"),
              notify: true,
              type_member: { type_name: "type-0" }
          },
          {
              user_id: ObjectId("56a50cd124eacf14e8493d52"),
              notify: true,
              type_member: { type_name: "type-0" }
          },
          {
              user_id: ObjectId("56a503e27d5b9da4738c268f"),
              notify: true,
              type_member: { type_name: "type-0" }
          },
          {
              user_id: ObjectId("56a50cd724eacf14e8493d53"),
              notify: true,
              type_member: { type_name: "type-0" }
          },
      ]
  },
  {
      name: "Projeto - 3",
      description: "descrição - 3",
      date_begin: new Date(2252,04,01),
      date_dream: new Date(2252,04,01),
      date_end: new Date(2252,04,01),
      visible: true,
      realocate: false,
      expired: false,
      visualizable_mod: "1",
      tags:  ["Cyberpunk", "armas", "robo"],
      goals: [
          {
              name: "goal 1",
              description: "fuga para o esconderijo do oleg",
              date_begin: new Date(2252,04,01),
              date_dream: new Date(2252,04,01),
              date_end: new Date(2252,04,01),
              realocate: false,
              expired: false,
              goal_historic: { date_realocate: new Date(2252,04,01) },
              activities: [
                  { activity_id: ObjectId("56a590d06048375df362242b") },
                  { activity_id: ObjectId("56a590d06048375df362242c") },
              ],
              tags: ["corrida", "armas", "submundo"]
          }
      ],
      members_project:  [
          {
              user_id: ObjectId("56a50ccb24eacf14e8493d51"),
              notify: true,
              type_member: { type_name: "type-0" }
          },
          {
              user_id: ObjectId("56a50cf024eacf14e8493d58"),
              notify: true,
              type_member: { type_name: "type-0" }
          },
          {
              user_id: ObjectId("56a50ce224eacf14e8493d55"),
              notify: true,
              type_member: { type_name: "type-0" }
          },
          {
              user_id: ObjectId("56a50cd124eacf14e8493d52"),
              notify: true,
              type_member: { type_name: "type-0" }
          },
          {
              user_id: ObjectId("56a503e27d5b9da4738c268f"),
              notify: true,
              type_member: { type_name: "type-0" }
          },
      ]
  },
  {
      name: "Projeto - 4",
      description: "descrição - 4",
      date_begin: new Date(2252,04,01),
      date_dream: new Date(2252,04,01),
      date_end: new Date(2252,04,01),
      visible: true,
      realocate: false,
      expired: false,
      visualizable_mod: "1",
      tags:  ["Cyberpunk", "conhecimento", "futuro"],
      goals: [
          {
              name: "goal 1",
              description: "investigar a carga valiosa",
              date_begin: new Date(2252,04,01),
              date_dream: new Date(2252,04,01),
              date_end: new Date(2252,04,01),
              realocate: false,
              expired: false,
              goal_historic: { date_realocate: new Date(2252,04,01) },
              activities: [
                  { activity_id: ObjectId("56a590d06048375df362242d") },
                  { activity_id: ObjectId("56a590d06048375df362242e") },
              ],
              tags: ["mente", "realidade virtual", "futuro"]
          },
          {
              name: "goal 2",
              description: "dentro da mente do robo",
              date_begin: new Date(2252,04,01),
              date_dream: new Date(2252,04,01),
              date_end: new Date(2252,04,01),
              realocate: false,
              expired: false,
              goal_historic: { date_realocate: new Date(2252,04,01) },
              activities: [
                  { activity_id: ObjectId("56a590d06048375df362242f") },
                  { activity_id: ObjectId("56a590d06048375df3622430") },
                  { activity_id: ObjectId("56a590d06048375df3622431") },
              ],
              tags: ["teletransporte", "realidade virtual", "puzzles"]
          },

      ],
      members_project:  [
          {
              user_id: ObjectId("56a50ccb24eacf14e8493d51"),
              notify: true,
              type_member: { type_name: "type-0" }
          },
          {
              user_id: ObjectId("56a50cf024eacf14e8493d58"),
              notify: true,
              type_member: { type_name: "type-0" }
          },
          {
              user_id: ObjectId("56a50ce224eacf14e8493d55"),
              notify: true,
              type_member: { type_name: "type-0" }
          },        
          {
              user_id: ObjectId("56a503e27d5b9da4738c268f"),
              notify: true,
              type_member: { type_name: "type-0" }
          },
          {
              user_id: ObjectId("56a50ceb24eacf14e8493d57"),
              notify: true,
              type_member: { type_name: "type-0" }
          },
      ]
  },
  {
      name: "Projeto - 5",
      description: "descrição - 5",
      date_begin: new Date(2252,04,01),
      date_dream: new Date(2252,04,01),
      date_end: new Date(2252,04,01),
      visible: true,
      realocate: false,
      expired: false,
      visualizable_mod: "1",
      tags:  ["fuga", "bitoca", "explosão"],
      goals: [
          {
              name: "goal 1",
              description: "escapar do cybro",
              date_begin: new Date(2252,04,01),
              date_dream: new Date(2252,04,01),
              date_end: new Date(2252,04,01),
              realocate: false,
              expired: false,
              goal_historic: { date_realocate: new Date(2252,04,01) },
              activities: [],
              tags: ["destruição", "t8000", "robo"]
          }
      ],
      members_project:  [
          {
              user_id: ObjectId("56a50cdc24eacf14e8493d54"),
              notify: true,
              type_member: { type_name: "type-0" }
          },
          {
              user_id: ObjectId("56a50cc424eacf14e8493d50"),
              notify: true,
              type_member: { type_name: "type-0" }
          },
          {
              user_id: ObjectId("56a50cd124eacf14e8493d52"),
              notify: true,
              type_member: { type_name: "type-0" }
          },
          {
              user_id: ObjectId("56a503e27d5b9da4738c268f"),
              notify: true,
              type_member: { type_name: "type-0" }
          },
          {
              user_id: ObjectId("56a50cd724eacf14e8493d53"),
              notify: true,
              type_member: { type_name: "type-0" }
          },
      ]
  },
]

projects.forEach(function(project) {
    db.projects.save(project);
})
```

## Retrieve - busca
#### 1. Liste as informações dos membros de 1 projeto específico que deve ser buscado pelo seu nome de forma a não ligar para maiúsculas e minúsculas.

```javascript
var project = db.projects.findOne(
  {
    name: /projeto - 3/i
  },
  {
    _id: false,
    members_project: true
  })

var members_in_project = []
var getMemberProject = function(member) {
  members_in_project.push(db.users.findOne({_id: member.user_id}))
}  
project.members_project.forEach(getMemberProject)
members_in_project
[
  {
    "_id": ObjectId("56a50ccb24eacf14e8493d51"),
    "name": "Android",
    "bio": "ndr",
    "date_register": ISODate("2119-05-01T03:00:00Z"),
    "avatar_path": "/user/ndr",
    "auth": {
      "username": "ndr",
      "email": "ndr@lollita.andrew",
      "password": "gothicLollita",
      "last_access": ISODate("2119-05-01T03:00:00Z"),
      "online": 1,
      "disabled": 0,
      "has_token": 0
    },
    "settings_system": {
      "background_path": "/user/ndr/bg"
    }
  },
  {
    "_id": ObjectId("56a50cf024eacf14e8493d58"),
    "name": "Cybro",
    "bio": "robo",
    "date_register": ISODate("2252-05-01T03:00:00Z"),
    "avatar_path": "/user/cybro",
    "auth": {
      "username": "cybro",
      "email": "cybro@robo.future",
      "password": "future",
      "last_access": ISODate("2252-05-01T03:00:00Z"),
      "online": 0,
      "disabled": 0,
      "has_token": 0
    },
    "settings_system": {
      "background_path": "/user/cybro/bg"
    }
  },
  {
    "_id": ObjectId("56a50ce224eacf14e8493d55"),
    "name": "Jovem Nerd",
    "bio": "mestre",
    "date_register": ISODate("2119-05-01T03:00:00Z"),
    "avatar_path": "/user/jn",
    "auth": {
      "username": "jovemnerd",
      "email": "jovemnerd@talco.com",
      "password": "talcoepomada",
      "last_access": ISODate("2119-05-01T03:00:00Z"),
      "online": 0,
      "disabled": 0,
      "has_token": 0
    },
    "settings_system": {
      "background_path": "/user/jn/bg"
    }
  },
  {
    "_id": ObjectId("56a50cd124eacf14e8493d52"),
    "name": "Oleg",
    "bio": "russo",
    "date_register": ISODate("2119-05-01T03:00:00Z"),
    "avatar_path": "/user/oleg",
    "auth": {
      "username": "oleg",
      "email": "oleg@urss.tucano",
      "password": "urss",
      "last_access": ISODate("2119-05-01T03:00:00Z"),
      "online": 0,
      "disabled": 0,
      "has_token": 0
    },
    "settings_system": {
      "background_path": "/user/oleg/bg"
    }
  },
  {
    "_id": ObjectId("56a503e27d5b9da4738c268f"),
    "name": "Ozob",
    "bio": "replicante",
    "date_register": ISODate("2119-05-01T03:00:00Z"),
    "avatar_path": "/user/ozob",
    "auth": {
      "username": "ozob",
      "email": "ozob@bozo.azaghal",
      "password": "babaca",
      "last_access": ISODate("2119-05-01T03:00:00Z"),
      "online": 0,
      "disabled": 0,
      "has_token": 0
    },
    "settings_system": {
      "background_path": "/user/ozob/bg"
    }
  }
]
```

#### 2. Liste todos os projetos com a tag que você escolheu para os 3 projetos em comum.

``Cyberpunk``

```javascript
var fields = { _id: false, name: true, tags: true }

var query = { tags: { $in: [ /cyberpunk/i ] } }
//OU
var query = { tags: { $eq: 'Cyberpunk' } }

db.projects.find(query, fields)
{
  "name": "Projeto - 1",
  "tags": [
    "token mental",
    "hirawata",
    "invasão",
    "Cyberpunk"
  ]
}
{
  "name": "Projeto - 3",
  "tags": [
    "Cyberpunk",
    "armas",
    "robo"
  ]
}
{
  "name": "Projeto - 4",
  "tags": [
    "Cyberpunk",
    "conhecimento",
    "futuro"
  ]
}
```

#### 3. Liste apenas os nomes de todas as atividades para todos os projetos.

```javascript
var projects = db.projects.find();
var activities = [];
var getActivity = function(activity) {
    activities.push( db.activities.findOne({ _id: activity.activity_id } ).name );};
projects.forEach(function(project) {
  project.goals.forEach(function(goal) {
    goal.activities.forEach(getActivity)
  })
});
activities
[
  "infiltrar na festa",
  "encontrar o portador do token mental",
  "escapar da festa",
  "encontrar o container",
  "obter carga misteriosa",
  "escapar das naves da hirawata corporation",
  "fugir para o esconderijo do oleg",
  "descobrir a funcionalidade da carga misteriosa",
  "se encontrar com Ulib",
  "descobrir o conteúdo do robo",
  "burlar as defesas do robo",
  "encontar o core"
]
```

#### 4. Liste todos os projetos que não possuam uma tag.

```javascript
var query = {tags: {$exists: true, $ne: []} }
var fields = {name: true}
db.projects.find(query, fields)
{
  "_id": ObjectId("56a599a56048375df3622432"),
  "name": "Projeto - 1"
}
{
  "_id": ObjectId("56a599a66048375df3622433"),
  "name": "Projeto - 2"
}
{
  "_id": ObjectId("56a599a66048375df3622434"),
  "name": "Projeto - 3"
}
{
  "_id": ObjectId("56a599a66048375df3622435"),
  "name": "Projeto - 4"
}
{
  "_id": ObjectId("56a599a66048375df3622436"),
  "name": "Projeto - 5"
}
```

#### 5. Liste todos os usuários que não fazem parte do primeiro projeto cadastrado.

```javascript
var project_1 = db.projects.findOne({name: /projeto - 1/i})
var members_id = []
project_1.members_project.forEach(function(member_project) {
  members_id.push(member_project.user_id)
})
var query = { _id: {$nin: members_id} }
var fields = { name: true }
var users = db.users.find(query, fields)
users
{
  "_id": ObjectId("56a503e27d5b9da4738c268f"),
  "name": "Ozob"
}
{
  "_id": ObjectId("56a50cd724eacf14e8493d53"),
  "name": "Steve Durden"
}
{
  "_id": ObjectId("56a50ce224eacf14e8493d55"),
  "name": "Jovem Nerd"
}
{
  "_id": ObjectId("56a50ceb24eacf14e8493d57"),
  "name": "Ulib"
}
{
  "_id": ObjectId("56a50cf024eacf14e8493d58"),
  "name": "Cybro"
}
```

## Update - alteração
#### 1. Adicione para todos os projetos o campo `views: 0`.

```javascript
var query = {};
var mod = {$set: {views: 0}};
var opt = {multi: true};
db.projects.update(query, mod, opt);
Updated 5 existing record(s) in 15ms
WriteResult({
  "nMatched": 5,
  "nUpserted": 0,
  "nModified": 5
})
db.projects.find({}, {name:true, views: true});
{
  "_id": ObjectId("56a599a56048375df3622432"),
  "name": "Projeto - 1",
  "views": 0
}
{
  "_id": ObjectId("56a599a66048375df3622433"),
  "name": "Projeto - 2",
  "views": 0
}
{
  "_id": ObjectId("56a599a66048375df3622434"),
  "name": "Projeto - 3",
  "views": 0
}
{
  "_id": ObjectId("56a599a66048375df3622435"),
  "name": "Projeto - 4",
  "views": 0
}
{
  "_id": ObjectId("56a599a66048375df3622436"),
  "name": "Projeto - 5",
  "views": 0
}

```

#### 2. Adicione 1 tag diferente para cada projeto.

```javascript
var projects = db.projects.find();
var addTag = function(project) {
  project.tags.push("TAG - "+project.name);
  db.projects.save(project);
};
projects.forEach(addTag);
Updated 1 existing record(s) in 1ms
Updated 1 existing record(s) in 1ms
Updated 1 existing record(s) in 1ms
Updated 1 existing record(s) in 1ms
Updated 1 existing record(s) in 0ms
db.projects.find({}, {name: true, tags: true});
{
  "_id": ObjectId("56a599a56048375df3622432"),
  "name": "Projeto - 1",
  "tags": [
    "token mental",
    "hirawata",
    "invasão",
    "Cyberpunk",
    "TAG - Projeto - 1"
  ]
}
{
  "_id": ObjectId("56a599a66048375df3622433"),
  "name": "Projeto - 2",
  "tags": [
    "token mental",
    "container",
    "invasão",
    "TAG - Projeto - 2"
  ]
}
{
  "_id": ObjectId("56a599a66048375df3622434"),
  "name": "Projeto - 3",
  "tags": [
    "Cyberpunk",
    "armas",
    "robo",
    "TAG - Projeto - 3"
  ]
}
{
  "_id": ObjectId("56a599a66048375df3622435"),
  "name": "Projeto - 4",
  "tags": [
    "Cyberpunk",
    "conhecimento",
    "futuro",
    "TAG - Projeto - 4"
  ]
}
{
  "_id": ObjectId("56a599a66048375df3622436"),
  "name": "Projeto - 5",
  "tags": [
    "fuga",
    "bitoca",
    "explosão",
    "TAG - Projeto - 5"
  ]
}
```

#### 3. Adicione 2 membros diferentes para cada projeto.

```javascript
var projects = db.projects.find();
var addMembers = function(project) {
  var members_id = [];
  project.members_project.forEach(function(member_project) {
    members_id.push(member_project.user_id)
  });
  var query = { _id: {$nin: members_id} };
  var fields = { name: true };
  var users = db.users.find(query, fields).limit(2);
  users.forEach(function(user){
    new_member = { user_id: user._id, notify: true, type_member: {type_name: "type-0"} }
    project.members_project.push(new_member);
    db.projects.save(project);
  })
};
projects.forEach(addMembers);
```

#### 4. Adicione 1 comentário em cada atividade, deixe apenas 1 projeto sem.

```javascript
var activities = db.activities.find();
var addComment = function(activity) {
  var comment = {
      text: "comment - "+activity.name,
      date_comment: Date.now(),
      members_comment: [
          {
              member_project: {
                  user_id: ObjectId("56a503e27d5b9da4738c268f"),
              }
          }
      ],
  }
  var mod = {$set: { comments: [comment]}}
  db.activities.update(activity, mod);
}

activities.forEach(addComment);
Updated 1 existing record(s) in 1ms
Updated 1 existing record(s) in 0ms
Updated 1 existing record(s) in 1ms
Updated 1 existing record(s) in 0ms
Updated 1 existing record(s) in 1ms
Updated 1 existing record(s) in 0ms
Updated 1 existing record(s) in 1ms
Updated 1 existing record(s) in 0ms
Updated 1 existing record(s) in 1ms
Updated 1 existing record(s) in 0ms
Updated 1 existing record(s) in 0ms
Updated 1 existing record(s) in 0ms
```

#### 5. Adicione 1 projeto inteiro com **UPSERT**.

```javascript
var project = {
  "name": "Projeto - 6",
  "description": "descrição - 6",
  "date_begin": new Date(2252,04,01),
  "date_dream": new Date(2252,04,01),
  "date_end": new Date(2252,04,01),
  "visible": true,
  "realocate": false,
  "expired": false,
  "visualizable_mod": "1",
  "tags": [
    "upsert",
    "setOnInsert",
    "shazam"
  ],
  "goals": [
    {
      "name": "goal 1",
      "description": "Deu Zica",
      "date_begin": new Date(2252,04,01),
      "date_dream": new Date(2252,04,01),
      "date_end": new Date(2252,04,01),
      "realocate": false,
      "expired": false,
      "goal_historic": {
        "date_realocate": new Date(2252,04,01)
      },
      "activities": [ ],
      "tags": [
        "Alakazam",
        "tibum",
        "bilu"
      ]
    }
  ],
  "members_project": [
    {
      "user_id": ObjectId("56a50cd124eacf14e8493d52"),
      "notify": true,
      "type_member": {
        "type_name": "type-0"
      }
    },
    {
      "user_id": ObjectId("56a50cd724eacf14e8493d53"),
      "notify": true,
      "type_member": {
        "type_name": "type-0"
      }
    },
    {
      "user_id": ObjectId("56a50ccb24eacf14e8493d51"),
      "notify": true,
      "type_member": {
        "type_name": "type-0"
      }
    }
  ],
  "views": 0
};
var query = {name: /projeto - 06/i};
var mod = {$setOnInsert: project};
var opt = {upsert: true};
db.projects.update(query, mod, opt);
Updated 1 new record(s) in 24ms
WriteResult({
  "nMatched": 0,
  "nUpserted": 1,
  "nModified": 0,
  "_id": ObjectId("56ac2501b3f2facabeaaba3b")
})
```

## Delete - remoção
#### 1. Apague todos os projetos que não possuam *tags*.

```javascript
var query = { $or: [{tags: {$exists: false}}, {tags: {$eq: []}}] }
var opt = { justOne: false }
db.projects.remove(query, opt)
Removed 0 record(s) in 1ms
WriteResult({
  "nRemoved": 0
})
```

#### 2. Apague todos os projetos que não possuam comentários nas atividades.

```javascript
// 1º: Seleciona as activities que não possuem comentários
var no_comments_activities = []
var query = { $or: [{comments: {$exists: false}}, {comments: {$eq: []}}] }
var fields = { _id: true }
db.activities.find(query, fields).toArray().forEach( function(activity) {
  no_comments_activities.push(activity._id);
});

// 2º: Remove os projetos que possuem aquelas activities selecionadas
var query = {'goals.activities.activity_id' : { $in : no_comments_activities } }
var opt = { justOne: false }
db.projects.remove(query, opt)
Removed 0 record(s) in 1ms
WriteResult({
  "nRemoved": 0
})
```

#### 3. Apague todos os projetos que não possuam atividades.

```javascript
var query = {$or: [{"goals.activities": {$exists: false}}, {"goals.activities": {$eq: []}}] }
var opt = { justOne: false }
db.projects.remove(query, opt)
Removed 2 record(s) in 1ms
WriteResult({
  "nRemoved": 2
})
```

#### 4. Escolha 2 usuário e apague todos os projetos em que os 2 fazem parte.

```javascript
var members_project = [
    ObjectId("56a50ce224eacf14e8493d55"),
    ObjectId("56a50ceb24eacf14e8493d57")
  ]
var query = {"members_project.user_id": {$in: members_project}}
var opt = { justOne: false }
db.projects.remove(query, opt)
Removed 2 record(s) in 1ms
WriteResult({
  "nRemoved": 2
})
```

#### 5. Apague todos os projetos que possuam uma determinada *tag* em *goal*.

```javascript
var query = {"goals.tags": {$in: [/blad runner/i]}}
var opt = { justOne: false }
db.projects.remove(query, opt)
Removed 1 record(s) in 0ms
WriteResult({
  "nRemoved": 1
})
```

## Sharding

```javascript
// config server
mkdir /data/configdb

mongod --configsvr --port 27010

// router
mongos --configdb localhost:27010 --port 27011

mkdir /data/shard1 && mkdir /data/shard2 && mkdir /data/shard3
// Shard 1
mongod --port 27012 --dbpath /data/shard1

// Shard 2
mongod --port 27013 --dbpath /data/shard2

// Shard 3
mongod --port 27014 --dbpath /data/shard3

// Adicionando os shards no router
mongo --port 27011 --host localhost

sh.addShard("localhost:27012")
sh.addShard("localhost:27013")
sh.addShard("localhost:27014")

sh.enableSharding("be-mean-modulo-mongodb-pf")

sh.shardCollection("be-mean-modulo-mongodb-pf.activities", {"_id" : 1})
```

## Replica

```javascript
// Criando diretório para as répicas
mkdir /data/rs1 && mkdir /data/rs2 && mkdir /data/rs3

// Inicializando as réplicas em suas respectivas portas
// réplica 1
mongod --replSet replica_set --port 27017 --dbpath /data/rs1

// réplica 2
mongod --replSet replica_set --port 27018 --dbpath /data/rs2

// réplica 3
mongod --replSet replica_set --port 27019 --dbpath /data/rs3

// Conexão na réplica primária para configurar e adicionar as outras réplicas
mongo --port 27017
rsconf = {
   _id: "replica_set",
   members: [
    {
     _id: 0,
     host: "127.0.0.1:27017"
    }
  ]
}
rs.initiate(rsconf)

rs.add("127.0.0.1:27018")
rs.add("127.0.0.1:27019")

// Adicionando o árbitro na réplica primária
rs.addArb("127.0.0.1:30000")

// Adicionando um árbitro
sudo mkdir /data/arb
mongod --port 30000 --dbpath /data/arb --replSet replica_set
```
