---
title: Multiple Query
taxonomy:
    category: docs
---

Kita juga boleh menjalankan ```multiple query``` pada GraphQL.

Jalankan query berikut:
```
{
  posts {
    title
    content
  },
  authors {
    firstName
    lastName
  }
}
```

Kita mendapatkan data ```posts``` dan ```authors``` dalam satu operasi query sahaja.

keputusan:
```
{
  "data": {
    "posts": [
      {
        "title": "Tajuk",
        "content": "Beberapa perenggan."
      },
      {
        "title": "Tajuk 2",
        "content": "Beberapa perenggan."
      },
      {
        "title": "Tajuk 3",
        "content": "Beberapa perenggan."
      }
    ],
    "authors": [
      {
        "firstName": "Muhammad",
        "lastName": "Aidil"
      },
      {
        "firstName": "Nurul",
        "lastName": "Jannah"
      },
      {
        "firstName": "Syafiq",
        "lastName": "Rahim"
      }
    ]
  }
}
```

Kita juga boleh menetapkan pembolehubah untuk keputusan query. Seperti berikut:
```
{
  Posts: posts {
    title
    content
  },
  authorName: authors {
    firstName
    lastName
  }
}
```
