---
title: Apollo Client
taxonomy:
    category: docs
---

Dalam langkah ini kita akan membuat klien untuk GraphQL dengan menggunakan bahasa pengaturcaraan Javascript. Kita akan menggunakan [Apollo](http://www.apollostack.com) client sebagai klien. Apollo client dibangunkan oleh MDG (Meteor Development Group) iaitu pembangun yang sama membangunkan framework [MeteorJS](https://www.meteor.com). Apollo client masih dalam peringkat pembangun dan belum dalam fasa stabil lagi tetapi pihak MDG telah menggunakannya pada produk produksi mereka seperti [Meteor Galaxy](https://www.meteor.com/hosting). Terdapat juga altenatif lain selain Apollo client untuk membuat klien GraphQL seperti [Relay](https://facebook.github.io/relay/) yang dibangunkan oleh pihak Facebook sendiri.

Mari kita mulakan:

###### * Clone git repository projek
Pertama sekali clone git repository ini terlebih dahulu:
```
git clone https://github.com/Akim95/todo-graphql-client.git
```

###### * Pergi ke direktori projek dan pasang dependencies
Selepas itu, pergi ke direktori projek:
```
cd todo-graphql-client
```

kemudian pasang dependencies dengan arahan ini:
```
npm install
```
Ia akan memasang semua dependencies yang tersenarai pada ```package.json``` seperti React iaitu library Javascript untuk membina antaramuka pengguna dan segala yang berkaitan dengan Apollo client.

###### * Buka direktori projek dengan text editor dan sunting fail ```index.js```
Seterusnya, buka fail ```src/index.js``` dan import graphql-tag dan react-apollo pada fail tersebut:
```javascript
// import apollo-client and react-apollo here
import ApolloClient, { createNetworkInterface } from 'apollo-client';
import { ApolloProvider } from 'react-apollo';
```

Selepas itu, kita akan membuat ```new ApolloClient(options)``` untuk membuat sambungan pada pelayan GraphQL dengan menetapkan ```networkInterface``` padanya:
```javascript
// new Apollo Client here
const client = new ApolloClient({
  networkInterface: createNetworkInterface('/graphql', {
    credentials: 'same-origin',
  })
});
```
networkInterface mempunyai ```createNetworkInterface('url endpoint')``` dan terdapat endpoint ```'/graphql'``` padanya.

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
ApolloProvider ialah ```container``` untuk menggunakan Apollo Client dalam paparan pokok React dan terdapat props ```client``` padanya.

###### * Sunting fail App.js
Buka fail ```src/App.js``` dan import ```graphql-tag``` dan ```react-apollo```:

```javascript
// import graphql-tag and react-apollo here
import gql from 'graphql-tag';
import { connect } from 'react-apollo';
```

Selepas itu, dengan menggunakan ```connect``` yang telah diimport daripada react-apollo. connect mempunyai dua ciri iaitu:
1. mapQueriesToProps
2. mapMutationsToProps

ia akan memyambung dua operasi tersebut kepada komponen ```(App)``` dan menggunakan ```graphql-tag``` untuk menghurai GraphQL query. Pertama sekali, kita akan buat untuk operasi query terlebih dahulu:

```javascript
// query and mutation here
const TodoWithData = connect({
  mapQueriesToProps: () => ({
    data: {
      query: gql`
        {
          todos {
            id
            task
            completed
          }
        }
      `,
      forceFetch: true,
    },
  }),
  // mutation here

})(App);

```

Kemudian, buat ```render()``` pada komponen ```App``` untuk memaparkan semua todo pada pengguna:
```javascript
// handleCreateTodo() method here

// your code here
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
dengan menggunakan ES2015 Destructuring ```const { todos } = this.props.data;``` untuk mengakses kepada props data todos bagi query yang telah dijalankan pada ```mapQueriesToProps``` yang telah disambungkan pada komponen App. Kemudian, menggunakan fungsi``` _.map``` lodash untuk menyenaraikan semua nilai yang ada pada todo dalam paparan senarai.

Mari kita membuat untuk operasi mutation pula:
```javascript
// mutation here
mapMutationsToProps: () => ({
  createTodo: (id, todoTask) => ({
    mutation: gql`
      mutation createTodo($id: Int!, $todoTask: String!) {
        createTodo(id: $id, task: $todoTask) {
          id
        }
      }
    `,
    variables: {
      id,
      todoTask,
    },
  }),
}),
//})(App);
```
dengan menambah ```mapMutationsToProps``` pada connect fungsinya sama seperti query sebelum ini tetapi kali ini untuk membuat operasi menambah data baru.


Seterusnya, membuat medan teks untuk kita mengisi task baru yang ingin ditambah:

```javascript
//  <div className="App">
//  <h1>Todo App</h1>
  <form onSubmit={this.handleCreateTodo}>
    <input type="text" name="todoTask" placeholder="Enter task" />
  </form>
```
pada props ```onSubmit``` pada form kita telah istihar ```this.handleCreateTodo``` yang akan memanggil method ini sewaktu pengguna menghantar task baru:

```javascript
// constructor here

// handleCreateTodo() method here
handleCreateTodo(event) {
  event.preventDefault();


  const { mutations } = this.props;
  const { refetch } = this.props.data;

  // input
  const id = Math.floor((Math.random() * 5555) + 1);
  const target = event.target;
  const task = target.todoTask.value;

  // make mutation and refetch data
  return mutations.createTodo(id, task).then(() => {

    // clear text field
    target.todoTask.value = '';

    // refetch data
    if (refetch) {
      return refetch();
    }
  });

}
```
method ini akan mengambil nilai yang telah kita masukkan pada medan teks dan menjalankan operasi mutation.  Setelah selesai melakukan mutation operasi refetch query akan dijalankan untuk memaparkan terus task baru yang telah dihantar kepada pengguna.

Tambahkan juga kod ```constructor``` ini supaya ```handleCreateTodo``` berfungsi:
```javascript
// constructor here
constructor() {
  super();

  this.handleCreateTodo = this.handleCreateTodo.bind(this);
}
```

###### * Nota
Pada projek ini secara lalai telah ditetapkan proxy pada dev server kerana kita mengasingkan folder pelayan GraphQL dan folder klien.
Lihat fail ```scripts/start.js```:
```
proxy: {
  "/graphql": "http://localhost:4000/graphql"
}
```


###### Kod sumber untuk projek ini sebagai rujukan:
```
git clone https://github.com/Akim95/todo-graphql-client-source.git

```
