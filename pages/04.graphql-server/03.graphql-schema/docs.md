---
title: GraphQL Schema
taxonomy:
    category: docs
---

Dalam langkah ini kita akan menulis schema pertama kita untuk operasi query. Pertama sekali kita perlu membuat fail dengan nama ```schema.js``` pada direktori projek. Selepas itu, tulis semua kod berikut.

######  Import GraphQL buildSchema.
Ia akan digunakan untuk membina schema untuk GraphQL.
```javascript
import { buildSchema } from 'graphql';
```

###### Import data Todos.
Kemudian, import data ```Todos``` daripada ```data.js```:  
nota: Kita akan membuat fail data.js kemudian.
```javascript
import { Todos } from './data';
```

###### Membuat schema untuk Todo.
Kita akan membuat schema dengan menggunakan bahasa GraphQL schema. Tulis kod berikut:
```javascript
const schema = buildSchema(`
  type TodoType {
    _id: Int
    task: String
    completed: Boolean
  }
  type Query {
    todos: [TodoType]
  }
  `);
```

###### Membuat resolver bagi setiap endpoint.
Kita akan membuat resolver bagi ```todos``` yang akan mengembalikan senarai Todos dari sumber data. Tulis kod berikut:
```javascript
const root = {
    todos: () => {
      return Todos;
    },
};
```

###### Eksport schema dan root.
```javascript
export { schema, root };
```

###### Membuat fail Data.
Buat fail ```data.js``` pada direktori projek dan tulis kesemua data dalam array berikut termasuk kod ```export``` tersebut:
```javascript
const Todos = [
  { id: 1, task: 'Hello task 1', completed: false },
  { id: 2, task: 'Hello task 2', completed: false },
  { id: 3, task: 'Hello task 3', completed: true },
  { id: 4, task: 'Hello task 4', completed: false },
  { id: 5, task: 'Hello task 5', completed: true },
];

export { Todos };
```

###### Import schema.
Sebelum kita menjalankan pelayan GraphQL ini. Kita perlu mengimport ```schema``` dan ```root``` pada fail ```server.js``` serta menetapkan pada ```graphQLHTTP```.

```javascript
// import express from 'express';
// import graphQLHTTP from 'express-graphql';
import { schema, root } from './schema';
//
// const GraphQLPORT = 4000;
// const app = express();
//
// app.use('/graphql', graphQLHTTP({
//   graphiql: true,
//   pretty: true,
  rootValue: root,
  schema,
// }));
//
// app.listen(GraphQLPORT, () => {
//   console.log(`GraphQL server run on port ${GraphQLPORT}`);
// });
```

###### Jalankan pelayan dan query.
Sekarang mari kita cuba menjalankan pelayan GraphQL berikut:
```
npm start
```
Jika tiada sebarang ralat pada terminal, cuba buka url ini [http://localhost:4000/graphql](http://localhost:4000/graphql) pada pelayar web. Dan cuba jalankan query berikut:

```
{
  todos {
    id
    task
    completed
  }
}
```
