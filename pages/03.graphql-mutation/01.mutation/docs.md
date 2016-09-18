---
title: Mutation
taxonomy:
    category: docs
---

Selain operasi query, GraphQL juga boleh menjalankan operasi mutation yang mana kita boleh membuat operasi ```write``` seperti membuat, mengemaskini dan memadam data. Sintaks mutation juga lebih kurang seperti operasi query. Dengan menggunakan pelayan GraphQL yang sama seperti guide yang sebelum ini [[Link](http://belajargraphql.herokuapp.com)].

Mari kita jalankan mutation berikut:
```
mutation {
  registerAuthor(id: "A4", firstName: "Siti", lastName: "Aishah") {
    id
    firstName
    lastName
  }
}
```

Mutation ini akan mendaftar author baru pada pangkalan data atau pelayan REST.

Kita juga boleh membuat ```multiple mutation``` seperti berikut:
```
mutation {
  Nurul: registerAuthor(id: "A5", firstName: "Nurul", lastName: "Huda") {
    id
    firstName
    lastName
  }
  Ahmad: registerAuthor(id: "A6", firstName: "Ahmad", lastName: "Fadlan") {
    id
    firstName
    lastName
  }
}
```

Jika hendak pastikan data tersebut sudah ditambah atau tidak sila jalankan query berikut:
```
{
  authors {
    firstName
    lastName
  }
}
```
