---
title: Apollo Client
taxonomy:
    category: docs
---

Dalam langkah ini kita akan membuat klien untuk GraphQL dengan menggunakan bahasa pengaturcaraan Javascript dan dengan menggunakan [Apollo](http://www.apollostack.com) client sebagai klien GraphQL. Apollo client dibangunkan oleh MDG (Meteor Development Group) iaitu pembangun yang sama membangunkan framework [MeteorJS](https://www.meteor.com). Apollo client masih dalam peringkat pembangun dan belum dalam fasa stabil lagi tetapi pihak MDG telah menggunakannya pada beberapa produk produksi mereka seperti [Meteor Galaxy](https://www.meteor.com/hosting). Terdapat juga altenatif lain selain Apollo client untuk membuat klien GraphQL seperti [Relay](https://facebook.github.io/relay/) yang dibangunkan oleh pihak Facebook sendiri.

###### CLONE GIT REPOSITORY PROJEK
Pertama sekali clone git repository ini terlebih dahulu:
```
git clone https://github.com/Akim95/todo-graphql-client.git
```

###### PERGI KE DIREKTORI PROJEK DAN PASANG DEPENDENCIES
Dengan arahan ini untuk pergi ke direktori projek:
```
cd todo-graphql-client
```

kemudian pasang dependencies dengan arahan ini:
```
npm install
```
Ia akan memasang semua dependencies yang tersenarai pada ```package.json``` seperti React iaitu library Javascript untuk membina antaramuka pengguna dan segala yang berkaitan dengan Apollo client.


###### BUKA DIREKTORI PROJEK DENGAN TEXT EDITOR DAN SUNGTING FAIL ```index.js```
Seterusnya, buka fail ```src/index.js``` dan import graphql-tag dan react-apollo pada fail tersebut:
```javascript
// import apollo-client and react-apollo here
import ApolloClient, { createNetworkInterface } from 'apollo-client';
import { ApolloProvider } from 'react-apollo';
```

Selepas itu, kita akan membuat constructor ```new ApolloClient(options)``` untuk membuat sambungan pada pelayan GraphQL dengan menetapkan ```networkInterface``` padanya:
```javascript
// new Apollo Client here
const client = new ApolloClient({
  networkInterface: createNetworkInterface('/graphql', {
    credentials: 'same-origin',
  })
});
```
networkInterface mempunyai opsyen ```createNetworkInterface('url endpoint')``` dan terdapat endpoint ```'/graphql'``` padanya.

Akhir sekali, letak komponen ```<App />``` dalam kurungan ```ApolloProvider```:
```javascript
// ApolloProvider here
ReactDOM.render(
  <ApolloProvider client={client}>
    <App />
  </ApolloProvider>,
  document.getElementById('root')
);
```
ApolloProvider ialah ```container``` untuk menggunakan Apollo Client dalam hierarki paparan React dan ditetapkan props ```client``` padanya.

###### SUNTING FAIL App.js
Buka fail ```src/App.js``` dan import ```graphql-tag``` dan ```react-apollo```:

```javascript
// import graphql-tag and react-apollo here
import gql from 'graphql-tag';
import { graphql } from 'react-apollo';
```

###### MEMBUAT OPERASI QUERY
Tambah kod berikut dibawah ```class App```:
```javascript
const GET_TODO = gql`
  query getTodos {
    todos {
      id
      task
      completed
    }
  }
`;

// mutation here
...
```

```javascript
const TodowithData = graphql(GET_TODO, {
  options: () => ({ pollInterval: 1000 }),
})(App);

// mutation
...
```
Pada kod diatas kita telah membuat ```graphql container``` untuk menjalankan query bagi mengembalikan semua todos pada komponen ```App``` dan telah menetapkan pollInterval 1000ms yang akan refetch query setiap 1000ms.

Kemudian, buat method ```render()``` pada komponen ```App``` untuk memaparkan semua todo kepada pengguna:
```javascript
// handleCreateTodo() method here

// render
render() {
  // data prop
  const { todos } = this.props.data;

  return (
    <div className="App">
    <h1>Todo App</h1>
    // form here

      <ul>
        {
          _.map(todos, (todo) => {
            return (
              <li key={todo.id}>{todo.task}</li>
            )
          })
        }
      </ul>
    </div>
  );
}
```
dengan menggunakan ES2015 Destructuring ```const { todos } = this.props.data;``` untuk mengakses senarai todos yang telah dihantar daripada operasi query kepada komponen App dan menggunakan lodash ```.map``` bagi memaparkan semua todos kepada pengguna.

###### MEMBUAT OPERASI MUTATION
Seterusnya, kita akan membuat operasi mutation pula:
```javascript
// mutation here
const TODO_MUTATION = gql`
  mutation createTodo($id: Int!, $todoTask: String!) {
    createTodo(id: $id, task: $todoTask) {
      id
    }
  }
`;
```

```javascript
// mutation
const TodoWithDataAndMutations = graphql(TODO_MUTATION, {
  props({ mutate }) {
    return {
      submit(id, todoTask) {
        return mutate({ variables: { id, todoTask } });
      }
    };
  },
})(TodowithData);
```
Sama juga seperti membuat operasi query yang menggunakan ```graphql container```, kod mutation diatas akan membuat task baru mengikut input pengguna.

Kemudian, eksport komponen ```App``` termasuk operasi yang telah ditetapkan:
```javascript
// export component
export default TodoWithDataAndMutations;
```

Seterusnya, membuat medan teks untuk kita mengisi task baru yang ingin ditambah:
```javascript
//  <div className="App">
//  <h1>Todo App</h1>
  <form onSubmit={this.handleCreateTodo}>
    <input type="text" name="todoTask" placeholder="Enter task" />
  </form>
```
pada props ```onSubmit``` pada form kita telah istihar ```this.handleCreateTodo``` yang akan memanggil kod method dibawah semasa pengguna menghantar task baru:

```javascript
// constructor here

// handleCreateTodo() method here
handleCreateTodo(event) {
  event.preventDefault();

  const { submit } = this.props;

  // input
  const id = Math.floor((Math.random() * 5555) + 1);
  const target = event.target;
  const task = target.todoTask.value;

  // make mutation and refetch data
  return submit(id, task).then(() => {

    // clear text field
    target.todoTask.value = '';
  });

}
```
method ini akan mengambil nilai yang telah kita masukkan pada medan teks dan menjalankan operasi mutation.

Tambah juga kod berikut pada ```constructor``` supaya method ```handleCreateTodo``` dapat berfungsi:
```javascript
// constructor here
constructor() {
  super();

  this.handleCreateTodo = this.handleCreateTodo.bind(this);
}
```

###### * Nota
Pada projek ini secara lalai telah ditetapkan proxy pada dev server kerana kita mengasingkan folder pelayan GraphQL dan folder klien apollo.
Lihat fail ```package.json```:
```
proxy: "http://localhost:4000"
```


###### Sumber kod untuk projek ini sebagai rujukan:
```
git clone https://github.com/Akim95/todo-graphql-client-source.git

```
