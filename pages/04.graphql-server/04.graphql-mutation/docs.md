---
title: GraphQL Mutation
taxonomy:
    category: docs
---

Dalam langkah ini kita akan membina schema untuk operasi mutation ia akan menambah task baru pada sumber data todo.

###### Membuat mutation pada fail schema.
Buka fail ```schema.js``` dan tambah kod mutation berikut:

```javascript
// const schema = buildSchema(`
//   type TodoType {
//     _id: Int
//     task: String
//     completed: Boolean
//   }
//   type Query {
//     todos: [TodoType]
//   }
  type Mutation {
    createTodo(_id: Int!, task: String!): TodoType
  }
  // `);
```
dalam kod diatas terdapat dua argument ```_id``` dan ```task``` yang diperlukan untuk menjalankan operasi ini.

###### Menambah operasi mutation pada root resolver.

Kita juga perlu menambah operasi ```mutation``` pada root resolver:
```javascript
// const root = {
//     todos: () => {
//       return Todos;
//     },
    createTodo: (args) => {
      const todo = {
        _id: args._id,
        task: args.task,
        completed: false,
      };
      Todos.push(todo);
      return todo;
    },
// };
```

###### Cuba jalankan operasi mutation.

Untuk memcuba operasi mutation yang telah dibuat sila jalankan mutation berikut:
```
mutation {
  createTodo(id: 6, task: "test mutation") {
    id
    task
  }
}
```

dan jalankan query berikut untuk pastikan mutation berikut telah berjaya atau tidak:
```
{
  todos {
    id
    task
    completed
  }
}
```
Anda akan nampak data yang telah anda buat pada keputusan query.
