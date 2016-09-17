---
title: Sumber Data Sebenar
taxonomy:
    category: docs
---

Dalam langkah ini kita akan menggunakan sumber data yang sebenar pada projek todo yang sama. Kita akan menggunakan pangkalan data MySQL/MariaDB dengan menggunakan sequelize. Pastikan anda telah memasang MariaDB atau MySQL pada komputer anda.

###### Pasang Dependencies.
Pertama, pasang ```sequelize``` dan ```mysql```:
```
npm install --save sequelize mysql
```
Sequelize ialah promise-based ORM untuk pangkalan data SQL bagi Node aplikasi.

###### Membuat fail connectors.
Kemudian, buat fail dengan nama ```connectors.js``` pada direktori projek dan tulis semua kod untuk sequelize berikut:
```javascript
import Sequelize from 'sequelize';

const db = new Sequelize('Todos', 'username', 'password', {
  host: 'localhost',
  dialect: 'mariadb',
});

const TodoModel = db.define('todos', {
  id: {
    type: Sequelize.INTEGER,
    primaryKey: true,
  },
  task: {
    type: Sequelize.STRING,
  },
  completed: {
    type: Sequelize.BOOLEAN,
  },
});

const Todos = db.models.todos;

export { Todos };
```

###### Gantikan data dari JSON kepada pangkalan data SQL.
Selepas itu, padam ```import { Todos } from './data';``` dan gantikan dengan ```import { Todos } from './connectors';```.
```javascript
// import { Todos } from './data';
import { Todos } from './connectors';
```

###### Ubah resolver pada fail schema.
Gantikan ```return Todos;``` kepada ```return Todos.findAll({});```.
```javascript
// const root = {
    // todos: () => {
      return Todos.findAll({ order: 'createdAt DESC' });
    // },
```

Gantikan ```Todos.push(todo);``` kepada ```Todos.create(todo);```.
```javascript
// const root = {
//     todos: () => {
//       return Todos.findAll({ order: 'createdAt DESC' });
//     },
    // createTodo: (args) => {
      // const todo = {
      //   id: args.id,
      //   task: args.task,
      //   completed: false,
      // };
      Todos.create(todo);
      // return todo;
    // },
// };
```
