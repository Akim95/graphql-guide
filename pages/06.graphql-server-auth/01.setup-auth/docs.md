---
title: Setup Pengesahan
taxonomy:
    category: docs
---

Kita akan membuat pengesahan untuk akses kepada pelayan GraphQL. Tiada sebarang pengesahan yang kursus terbina untuk pelayan GraphQL kita bebas untuk menggunakan apa sahaja kaedah untuk pengesahan. Kita akan menggunakan kaedah ```HTTP Bearer Auth``` dengan menggunakan [passport.js](http://passportjs.org).

###### Clone direktori projek.
Pertama sekali clone projek pelayan graphql todo berikut dan tukar ke branch ```data-source```.
```
git clone https://github.com/Akim95/todo-graphql-server.git
git checkout data-source
```

###### Pasang ```passport``` dan ```passport-http-bearer```.
Selepas itu, pasang npm dependency ```passport``` dan ```passport-http-bearer``` berikut.
```bash
npm install --save passport passport-http-bearer
```

##### Membuat Sequelize schema pada fail ```connectors.js```.

###### Tambah schema token.
Kemudian, kita perlu membuat schema baru untuk menyimpan token pengguna pada pangkalan data SQL.
```javascript
const TokenModel = db.define('tokens', {
  token: Sequelize.STRING,
});

// create initial data and remove all data after restart server
// db.sync({ force: true }).then(() => {
//   TodoModel.create({
//     id: 1,
//     task: 'demo data',
//     completed: false,
//   });
  TokenModel.create({
    token: '402bd0d6-59ac-447f-af4c-f9bc61691dae',
  });
// });

// const Todos = db.models.todos;
const Tokens = db.models.tokens;

// export Sequelize model
export { ..., Tokens };
```

##### Menambah konfigurasi Pengesahan Bearer pada fail ```server.js```.

###### Import ```passport``` dan ```passport-http-bearer``` serta ```Tokens``` model`.
Import kesemua dependency yang diperlukan berikut.
```javascript
import passport from 'passport';
import Strategy from 'passport-http-bearer';
import { Tokens } from './connectors';
```

###### Membuat konfigurasi Strategi Bearer.
Strategi Bearer memerlukan ```token bearer``` untuk memberi akses kepada pengguna. Ia akan mencari token bearer yang wujud pada pangkalan data SQL yang telah dibuat dan akan membuat callback sama ada pengesahan berjaya atau tidak.
```javascript
passport.use(new Strategy(
  function (token, cb) {
    Tokens.findOne({
      token,
    }).then(function (user) {
      return cb(null, user);
    }).catch(function (err) {
      return cb(err);
    });
  }
));
```

###### Menggunakan ```passport.authenticate()```.
Tambah ```passport.authenticate()``` dengan ditetapkan strategi 'bearer' untuk pengesahan. Pengesahan menggunakan token bearer tidak memerlukan session jadi kita tetapkan ```session``` kepada ```false```.
```javascript
// app.use('/graphql',
  passport.authenticate('bearer', { session: false }),
  // graphQLHTTP({
  //   graphiql: true,
  //   pretty: true,
  //   rootValue: root,
  //   schema,
  // }));
  ```
