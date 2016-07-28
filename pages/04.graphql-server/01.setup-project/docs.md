---
title: Setup Projek
taxonomy:
    category: docs
---

Kita akan membuat pelayan GraphQL untuk aplikasi Todo. Pertama sekali pastikan anda telah memasang ```Node.js``` pada komputer anda jika belum anda boleh memuat turun dilaman web rasmi [Node.js](https://nodejs.org/en/). Jika selesai memasang Node.js kita perlu membuat satu folder baru dengan nama ```todo-graphql-server``` atau terpulang kepada anda. Dalam tutorial ini kita menggunakan Node.js untuk membangunkan pelayan GraphQL.

###### Membuat folder projek.
```
mkdir todo-graphql-server
```

dan pergi ke direktori tersebut:
```
cd todo-graphql-server
```

###### Membuat fail package.json.
Jalankan arahan berikut:
```
npm init -f
```
ia akan membuat ```package.json``` yang mana memberi tahu kepada [npm](https://www.npmjs.com/) apakah nama projek dan maklumat lain mengenai projek.

###### Memasang babel.
Kemudian pasang Babel:
```bash
npm install --save babel babel-core babel-preset-es2015
```
[Babel](https://babeljs.io/) ialah Javascript compiler. Ia membolehkan anda menggunakan kod ```ES2015``` iaitu versi terkini untuk Javascript (ECMAScript) dan membuatkannya berfungsi pada semua pelayar web walaupun pelayar web tidak menyokong ES2015. Babel akan compile dari kod ES2015 ke kod ```ES5```.

###### Memasang Node.js.
Seterusnya memasang Express iaitu framework untuk Node.js:
```bash
npm install --save express
```

###### Memasang graphql dan express-graphql.
Akhir sekali pasang [graphql-express](https://github.com/graphql/express-graphql) dan [graphql.js](https://github.com/graphql/graphql-js):
```bash
npm install --save express-graphql graphql
```

```graphql-express``` digunakan untuk membuat pelayan GraphQL HTTP dengan express.  
```graphql.js``` digunakan untuk membuat pelaksanaan GraphQL dengan Javascript.

###### Semak dependencies
Pastikan semua ```dependencies``` telah terpasang dengan arahan berikut:
```
cat package.json
```

keputusan akan seperti berikut:
```
{
  "name": "todo-graphql-server",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "dependencies": {
    "babel": "^6.5.2",
    "babel-core": "^6.10.4",
    "babel-preset-es2015": "^6.9.0",
    "express": "^4.14.0",
    "express-graphql": "^0.5.3",
    "graphql": "^0.6.0"
  }
}
```

###### Membuat fail .bashrc.
 Buka folder projek menggunakan text editor kegemaran anda dan buat satu fail dengan nama ```.bashrc``` dan tambah baris berikut ke fail .babelrc anda:
```
{
  "presets": ["es2015"]
}
```

###### Tambah start script.
Kemudian buka pula fail ```package.json``` dan pada objek ```scripts``` tambah baris berikut pada scripts:
```json
"scripts": {
  "start": "node index.js",
  // more...
},
// more...
```

###### Membuat fail index.js.
Akhir sekali buat fail dengan nama ```index.js``` dan tulis kod berikut:
```
require('babel-core/register');
require('./server.js');
```

###### Kod sumber untuk projek ini sebagai rujukan:
```
git clone https://github.com/Akim95/todo-graphql-server

// untuk yang menggunakan sumber data sebenar:
git checkout data-source
```
