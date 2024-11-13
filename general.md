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