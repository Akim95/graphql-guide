---
title: GraphQL Mutation
taxonomy:
    category: docs
---

Kita akan meneruskan dengan membuat operasi mutation pada langkah ini dengan menggunakan projek yang sama.

###### Membuat mutation pada fail schema.
Buka fail ```schema.js``` dan tambah kod mutation berikut:

```javascript
const RootMutation = new GraphQLObjectType({
  name: 'RootMutation',
  fields: {
    createTodo: {
      type: TodoType,
      args: { // here
        id: { type: new GraphQLNonNull(GraphQLInt) },
        task: { type: new GraphQLNonNull(GraphQLString) },
      },
      resolve(root, args) { // and here
        const todo = {
          id: args.id,
          task: args.task,
          completed: false,
        };
        Todos.push(todo);
        return todo;
      },
    },
  },
});
```

Ada beberapa bahagian yang anda sudah tahu pada kod berikut. Benda baru pada kod ini ialah mempunyai 2 ```args (Argument)```:

1. ```id```: Jenis ```new GraphQLNonNull(GraphQLInt)``` iaitu argument wajib ada nilai dan mesti jenis Integer.  
2. ```task```: Jenis ``` new GraphQLNonNull(GraphQLString)``` iaitu argument wajib ada nilai dan mesti jenis String.

Kita juga memberi args kepada ```resolve``` untuk memberi akses kepada argument id dan task.

###### Menambah operasi mutation pada schema.

Kita juga perlu menambah operasi ```mutation``` dengan ```RootMutation``` pada schema:
```javascript
// export default new GraphQLSchema({
    // query: RootQuery,
       mutation: RootMutation,
// });
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
