---
title: Setup Pelayan GraphQL
taxonomy:
    category: docs
---

Kita akan membuat pelayan GraphQL HTTP dengan express framework dalam langkah ini. Buat satu fail ```server.js``` dalam direktori projek dan tulis kod berikut pada fail tersebut:
```javascript
import express from 'express';
import graphQLHTTP from 'express-graphql';

const GraphQLPort = 4000;
const app = express();

app.use('/graphql', graphQLHTTP({
  graphiql: true,
  pretty: true,
}));

app.listen(GraphQLPort, () => {
  console.log(`GraphQL server run on port ${GraphQLPort}`);
});
```
Apa yang kita buat:
* Import ```express``` dan ```express-graphql```.
* Istihar port ```4000``` pada pembolehubah ```GraphQLPort```.
* Istihar express pada pembolehubah ```app```.
* Menggunakan express dengan route ```/graphql``` dan menetapkan express-graphql sebagai middleware.
* graphQLHTTP dengan opsyen ```graphiql``` ```true``` yang mana untuk memberi kita akses kepada graphiql dan juga menetapkan ```pretty``` kepada ```true``` untuk mencetak kemas JSON pada query.
* Binds dan listen untuk sambungan mengikut port yang telah ditetapkan.


Sekarang cuba jalankan pelayan dengan arahan berikut pada terminal:
```
npm start
```
Terminal akan menunjuk mesej ```GraphQL server run on port 4000``` jikalau tiada sebarang ralat.
