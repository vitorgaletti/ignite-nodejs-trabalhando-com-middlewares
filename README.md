# Desafio 02 - Trabalhando com middlewares

## :computer: Sobre o desafio

<hr>

Nesse desafio você irá trabalhar mais a fundo com middlewares no Express. Dessa forma você será capaz de fixar mais ainda os conhecimentos obtidos até agora.

Para facilitar um pouco mais do conhecimento da regra de negócio, você irá trabalhar com a mesma aplicação do desafio anterior: uma aplicação para gerenciar tarefas (ou _todos_) mas com algumas mudanças.

Será permitida a criação de um usuário com `name` e `username`, bem como fazer o CRUD de *todos*:

- Criar um novo _todo_;
- Listar todos os _todos_;
- Alterar o `title` e `deadline` de um _todo_ existente;
- Marcar um _todo_ como feito;
- Excluir um _todo_;

Tudo isso para cada usuário em específico. Além disso, dessa vez teremos um plano grátis onde o usuário só pode criar até dez _todos_ e um plano Pro que irá permitir criar _todos_ ilimitados, isso tudo usando middlewares para fazer as validações necessárias. 🚀

## :rocket: Techs

<ul>
  <li> Javascript </li>
  <li> Node.js </li>
  <li> Express </li>
  <li> uuid </li>
  <li> cors </li>
</ul>

## Desenvolvimento

---

### Pré-requisitos

- Instalar [Node.js](https://nodejs.org)

- Instalar [Yarn](https://yarnpkg.com/)

### Clone o repositório

```bash
$ git clone https://github.com/vitorgaletti/ignite-nodejs-trabalhando-com-middlewares.git
```

### Executar Projeto

```bash
# Mudar para directório
$ cd ignite-nodejs-trabalhando-com-middlewares/
```

- Instalar dependências

```bash
$ yarn install
```

- Execute

```bash
$ yarn dev
```

- Executar scripts

|        Ação        | Utilização  |
| :----------------: | :---------: |
| Iniciar o servidor | `yarn dev`  |
|  Executar testes   | `yarn test` |

URL da API = http://localhost:3333

<br>

## API Reference

#### Cadastrar novo usuário

```http
POST /users
```

| Request Field | Type     |
| :------------ | :------- |
| `name`        | `string` |
| `username`    | `string` |

| Status code | Description                             |
| :---------- | :-------------------------------------- |
| `201`       | Conta criada com sucesso                |
| `400`       | Já possui um usuário com mesmo username |

```json
{
  "name": "vitor",
  "username": "vitor1997"
}
```

#### Visualizar um usuário específico

```http
GET /users/:id
```

| Parameter | Type     | Description              |
| :-------- | :------- | :----------------------- |
| `id`      | `number` | Identificador do usuário |

| Response Field | Type      | Description                    |
| :------------- | :-------- | :----------------------------- |
| `id`           | `number`  | Identificador do usuário       |
| `name`         | `string`  | Nome do usuário                |
| `pro`          | `boolean` | Se o usuário possui versão pro |
| `todos`        | `array`   | Conjunto de tarefas do usuário |

| Status code | Description            |
| :---------- | :--------------------- |
| `200`       | Visualiza um usuário   |
| `404`       | Usuário não encontrado |

```json
{
  "id": "679ea0a3-1a71-4885-a22f-32fafef47947",
  "name": "vitor",
  "username": "vitor1997",
  "pro": false,
  "todos": []
}
```

#### Alterar o plano do usuário para pro

```http
PATCH /users/:id/pro
```

| Parameter | Type     | Description              |
| :-------- | :------- | :----------------------- |
| `id`      | `number` | Identificador do usuário |

| Response Field | Type      | Description                    |
| :------------- | :-------- | :----------------------------- |
| `id`           | `number`  | Identificador do usuário       |
| `name`         | `string`  | Nome do usuário                |
| `pro`          | `boolean` | Se o usuário possui versão pro |
| `todos`        | `array`   | Conjunto de tarefas do usuário |

| Status code | Description                |
| :---------- | :------------------------- |
| `200`       | Plano alterado com sucesso |
| `404`       | Usuário não encontrado     |
| `400`       | Plano pro já ativado       |

```json
{
  "id": "5fe2487a-81f4-4b16-9f28-a9df44c1106f",
  "name": "vitor",
  "username": "vitor1997",
  "pro": true,
  "todos": []
}
```

#### Visualizar tarefas a fazer

```http
GET /todos
```

| Header Field |   Type   | Description                                         |
| :----------- | :------: | --------------------------------------------------- |
| `username`   | `string` | Informar o username do usuário cadastrado no header |

| Response Field | Type      | Description                      |
| :------------- | :-------- | :------------------------------- |
| `id`           | `number`  | Identificador da tarefa          |
| `title`        | `string`  | Título da tarefa                 |
| `done`         | `boolean` | A tarefa foi feita ou não        |
| `deadline`     | `string`  | Data limite da entrega da tarefa |
| `created_at`   | `string`  | Data criada da tarefa            |

| Status code | Description                                      |
| :---------- | :----------------------------------------------- |
| `200`       | Visualizar todas as tarefa do username informado |
| `404`       | Usuário não encontrado                           |

```json
[
  {
    "id": "b73cae2b-7c0b-4de0-8285-8db0a609327e",
    "title": "Estudar Matematica",
    "done": false,
    "deadline": "2022-06-10T00:00:00.000Z",
    "created_at": "2022-06-15T21:17:44.881Z"
  }
]
```

#### Criar uma tarefa

```http
POST /todos
```

| Request Field | Type     | Description                      |
| :------------ | :------- | :------------------------------- |
| `title`       | `string` | Título da tarefa                 |
| `deadline`    | `string` | Data limite da entrega da tarefa |

| Header Field | Type     | Description                                         |
| :----------- | :------- | :-------------------------------------------------- |
| `username`   | `string` | Informar o username do usuário cadastrado no header |

| Status code | Description                             |
| :---------- | :-------------------------------------- |
| `201`       | Tarefa criada com sucesso               |
| `404`       | Usuário não encontrado                  |
| `403`       | Usuário alcançou o limite de 10 tarefas |

```json
{
  "title": "Estudar Matematica",
  "deadline": "2022-06-10"
}
```

#### Alterar uma tarefa

```http
PUT /todos/:id
```

| Parameter | Type     | Description             |
| :-------- | :------- | :---------------------- |
| `id`      | `number` | Identificador da tarefa |

| Request Field | Type     | Description                      |
| :------------ | :------- | :------------------------------- |
| `title`       | `string` | Título da tarefa                 |
| `deadline`    | `string` | Data limite da entrega da tarefa |

| Header Field |   Type   | Description                                         |
| :----------- | :------: | --------------------------------------------------- |
| `username`   | `string` | Informar o username do usuário cadastrado no header |

| Status code | Description                 |
| :---------- | :-------------------------- |
| `200`       | Tarefa alterada com sucesso |
| `400`       | Id não é um UUID            |
| `404`       | Usuário não encontrado      |
| `404`       | Tarefa não encontrada       |

```json
{
  "title": "Tarefa Portugues",
  "deadline": "2022-06-26"
}
```

#### Alterar status da tarefa para feita

```http
PATCH /todos/:id/done
```

| Parameter | Type     | Description             |
| :-------- | :------- | :---------------------- |
| `id`      | `number` | Identificador da tarefa |

| Header Field |   Type   | Description                                         |
| :----------- | :------: | --------------------------------------------------- |
| `username`   | `string` | Informar o username do usuário cadastrado no header |

| Status code | Description                 |
| :---------- | :-------------------------- |
| `200`       | Tarefa alterada com sucesso |
| `400`       | Id não é um UUID            |
| `404`       | Usuário não encontrado      |
| `404`       | Tarefa não encontrada       |

```json
{
  "id": "8d7b647f-118f-487a-be3f-3c680f82a68d",
  "title": "Estudar Matematica",
  "done": true,
  "deadline": "2022-06-10T00:00:00.000Z",
  "created_at": "2022-06-15T21:31:31.925Z"
}
```

#### Excluir uma tarefa

```http
DELETE /todos/:id
```

| Parameter | Type     | Description             |
| :-------- | :------- | :---------------------- |
| `id`      | `number` | Identificador da tarefa |

| Header Field |   Type   | Description                                         |
| :----------- | :------: | --------------------------------------------------- |
| `username`   | `string` | Informar o username do usuário cadastrado no header |

| Status code | Description                 |
| :---------- | :-------------------------- |
| `204`       | Tarefa excluida com sucesso |
| `400`       | Id não é um UUID            |
| `404`       | Usuário não encontrado      |
| `404`       | Tarefa não encontrada       |

## Autor

- [@vitorgaletti](https://github.com/vitorgaletti)
