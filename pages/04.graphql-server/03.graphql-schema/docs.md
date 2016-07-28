---
title: GraphQL Schema
taxonomy:
    category: docs
---

Dalam langkah ini kita akan menulis schema pertama kita untuk operasi query. Pertama sekali kita perlu membuat fail ```schema.js``` pada direktori projek. Selepas itu, tulis semua kod dibawah.

######  Import modul graphql/type.
import jenis graphql yang diperlukan:
```javascript
import {
  GraphQLSchema,
  GraphQLObjectType,
  GraphQLList,
  GraphQLInt,
  GraphQLString,
  GraphQLBoolean,
} from 'graphql/type';
```
###### Import data Todos.
Kemudian, import data ```Todos``` daripada ```data.js```:  
nota: Kita akan membuat fail data.js kemudian.
```javascript
import { Todos } from './data';
```

###### Membuat jenis todo (Todo Type).
Istihar jenis todo pada pembolehubah ```TodoType```. Membuat jenis todo dengan ```new GraphQLObjectType``` yang terkandung dengan beberapa ```medan``` padanya. Kita menentukan 3 jenis medan iaitu ```id``` dengan jenis data ```Integer```, ```task``` dengan jenis data ```String``` dan ```completed``` dengan jenis data ```Boolean```.
```javascript
const TodoType = new GraphQLObjectType({
  name: 'Todo',
  fields: () => ({
    id: {
      type: GraphQLInt,
    },
    task: {
      type: GraphQLString,
    },
    completed: {
      type: GraphQLBoolean,
    },
  }),
});
```

###### Membuat root query.
Pada langkah ini kita perlu menggunakan ```method resolve``` yang mana digunakan oleh GraphQL apabila query dijalankan (akan kembalikan data daripada sumber data). Sama seperti membuat ```TodoType``` dengan menggunakan ```new GraphQLObjectType``` disini kita mengistihar 1 medan iaitu ```todos``` yang mana untuk kita query kesemua data yang ada pada Todos.

Medan:
* ```todos``` kita menentukan ```new GraphQLList(TodoType)``` sebagai jenis todos yang mana akan mengembalikan kesemua objek ```TodoType``` dalam senarai. Resolve akan mengembalikan kesemua data pada todo.
```javascript
    const RootQuery = new GraphQLObjectType({
      name: 'RootQuery',
      fields: () => ({
        todos: {  
          type: new GraphQLList(TodoType),
          resolve() {
            return Todos;
          },
        },
      }),
    });
```

###### Mengeksport GraphQL schema.
Sekarang kita akan mengeksport ```GraphQLSchema```, sebelum itu kita tentukan operasi ```query``` dengan ```RootQuery``` pada ```new GraphQLSchema```.
```javascript
export default new GraphQLSchema({
  query: RootQuery,
});
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
Sebelum kita menjalankan pelayan GraphQL ini. Kita perlu mengimport ```schema``` pada fail ```server.js``` serta istiharkan ia pada opsyen graphQLHTTP.

```javascript
// import express from 'express';
// import graphQLHTTP from 'express-graphql';

// import schema
import schema from './schema';

//  const GraphQLPORT = 4000;
//  const app = express();

// app.use('/graphql', graphQLHTTP({
//  graphiql: true,
//  pretty: true,
    schema, // es6 object-shorthand
// }));

//  app.listen(GraphQLPORT, () => {
//  console.log(`GraphQL server run on port ${GraphQLPORT}`);
//  });
```

###### Jalankan pelayan dan query.
Ok, mari kita cuba menjalankan pelayan GraphQL berikut:
```
npm start
```
Jika tiada sebarang ralat pada terminal, cuba buka url ini [http://localhost:4000/graphql/](http://localhost/graphql/) pada pelayar web. Dan cuba jalankan query berikut:

```
{
  todos {
    id
    task
    completed
  }
}
```
