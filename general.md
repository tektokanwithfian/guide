# General Convention

## Naming

### Singular

- Use singular form for variables, functions, table, or endpoint.

```
/user, instead of /users
db.user, instead of db.users
user(), instead of users()
```
- Whenever possible, use a single word for variables, functions, table, or endpoint.

```
/user, instead of /userList
db.user, instead of db.userList
user(), instead of userList()
```

- If more than one word is needed, then use the following pattern:

```
[grouping][parent][child][SizeAgeShapeColorOriginMaterialPurpose]

i.e.
URL_API
URL_BASE
URL_HOST
```

### Order

- Use alphabetical order for variables, functions, table, or endpoint.

### Comfort

- Use words that are easy to understand and in a language that is more comfortable for you to use.
- if it is decided to use a certain language, then you should pay attention to the language's grammar and syntax.

### Cases

- Use camelCase for variables, functions, table, or endpoint.
- Use PascalCase for class names, and for things that are already standardized to use PascalCase. For example;
  - React Components.
  - Golang Exported functions.
- Use kebab-case for filenames, url and endpoints.
- Use snake_case for languages that exlusively use snake_case.

## API

### Endpoints

- use kebab-case for endpoints.

```
/user-friend
```

- use the following pattern for endpoints:
  - GET    /[entity-name]
  - GET    /[entity-name]/:id
  - POST   /[entity-name]
  - PATCH  /[entity-name]/:id
  - PUT    /[entity-name]/:id
  - DELETE /[entity-name]/:id
- Don't over complicate your endpoints.

```
/[entity#1]/:param/[entity#2]/:param/[entity#3]/:param
```
- Don't add any prefix (`/api`) to your api, to avoid redundancy.

### Versionless

- should not have an explicit version written in the function name.

```
func GetUserV1() // wrong
func GetUser() // right
```

### Separation

Shall there be a major version bump, then the API should be separated into an entirely new service.

```
core api (v1) - alex
/order
/transaction


core api (v2) - bob
/order
/transaction

/v1/transaction -> alex /transaction
/v2/transaction -> bob /transaction

```

## Coding Style

If a certain language has a standard coding style, then dont reinvent the wheel, just use it.
However, for languages that do not have a standard coding style, such as JavaScript (or TypeScript) -Nextjs, in our case- then you should follow the following:
* eslint
* space (2 spaces) over tab
* [airbnb](https://github.com/airbnb/javascript)
* overrides
  * no semicolons
  * Imports order
    * builtin
    * external
    * parent
    * sibling
    * index
    * object
    * type
  * no param reassign (exclusively use const, for immutability)
  * no any
* set the text editor to auto format on save (and or on paste)


## Containarisation

### Docker

Every project should have a
* Dockerfile (for production, multistage)
* Dockerfile.dev (for development, multistage)
* docker-compose.yaml (for development)

#### General structure 

```
# Dockerfile

step #1: install base dependencies
FROM node:20-alpine AS base

[install base dependencies]

# step #2: install project dependencies
FROM base AS dependencies

[install project dependencies: pnpm install]

# step #3: build project
FROM base AS build

[build project]

# step #4: run
FROM base AS runner

// hot reload for development
RUN npm install -g nodemon

[run the project]
```

```
# docker-compose.dev.yaml
version: "3.9"
services:
  [service_name]:
    build:
      context: .
      dockerfile: .dockerfiles/Dockerfile.dev
      target: runner
    container_name: [service_name]
    env_file: .env // for development, git ignored
    volumes:
      - .:/app
    ports:
      - 3000:3000
    networks:
      - default
      - bridge-network
networks:
  default:
    driver: bridge
  bridge-network:
    external: true
```

### Orchestration

There needs to be orchestration related configuration files on every project.
  - .build/build.yaml 
  - .build/config/kubernetes.yaml
  - .build/config/istio.yaml

## Project Structure

for programming languages that have a standard project structure, such as Java, Python, Golang, then you should follow the standard structure.


### General folder structure

```
.
├── .build
│   ├── config
│   │   ├── istio-vs-dr.yaml // virtual service and dest rule config
│   │   ├── istio-vs-gw.yaml // (optional) ingress gateway config
│   │   └── kubernetes.yaml // service and deployment config
│   └── build.yaml // build pipeline
├── .dockerfiles
│   ├── Dockerfile // production dockerfile
│   └── Dockerfile.dev // development dockerfile
.
.
.
├── .env // development env (git ignored)
├── .env.example // example env (tracked)
├── CHANGELOG.md 
├── README.md
├── docker-compose.dev.yaml // docker compose -f docker-compose.dev.yaml up, to run
.
```

### NextJs

For languages that do not have a standard project structure, such as JavaScript (or TypeScript) -Nextjs, in our case- we tend to add extra layering to the project structure, to optimise the automation process of

`Copy a whole folder - paste - find in folder - replace`.

This will not so much a `DRY` principle or not so much a `Clean Code` principle, but it will make the automation process of the project structure much easier.

#### Components

we only put reusable components in the components folder and shadcn's components in the `components/ui` folder.

components/blogSearch.tsx


#### Contructors (feel free to change the name)

This will be the folder that will encapsulate a page. 

Implementation

```
# app/blog/page.tsx
import Page from '@/contructors/blog/home'

export default Page


# app/blog/[id]/page.tsx
import Page from '@/contructors/blog/detail'

export default Page
```

Folder structure

```
.
└── constructors
    └── blog    
        └── home
            ├── actions
            │   └── index.ts // server side logic
            ├── components
            │   ├── content-result // a big enough child component
            │   │   ├── actions
            │   │   │   ├── actions.ts // form related server action
            │   │   │   └── schema.ts // schema or model for the server action
            │   │   ├── content-grand-children.ts // grandchildren component
            │   │   └── content.tsx // 
            │   ├── content-head.tsx // child component
            │   ├── content-body.tsx // child component
            │   ├── content-foot.tsx // child component
            │   └── content.tsx // parent/main component 
            └── index.tsx // main function to pass things from server side logic to
```

#### Services (feel free to change the name)

We put all the things related to business logic, here.

```
.
└── services
   ├── adapter // external abstraction (db connections, sdks, etc)
   ├── hooks // self explanatory
   ├── interface // as an interface for adapter, our constructors code must only communicate with this interface
   └── tools // common functions 
```