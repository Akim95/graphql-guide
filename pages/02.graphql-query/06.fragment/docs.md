---
title: Fragment
taxonomy:
    category: docs
---

```Fragment``` ialah mengumpulkan ```field``` yang biasa diguna dalam satu kumpulan dan menggunakannya semula. Seperti biasa kita akan menulis query seperti ini:

```
{
  First: singlePost(id: "1") {
    title
    content
    author {
      firstName
      lastName
    }
  },
  Second: singlePost(id: "2") {
    title
    content
    author {
      firstName
      lastName
    }
  }
}
```
untuk meminta dua data yang mempunyai field yang sama kita perlu mengulang menulis field yang sama untuk kedua-dua data yang diminta ianya tiada masalah jika hendak meminta data yang tidak begitu banyak, jika banyak kita perlu mengulang menulis field yang sama berkali-kali. Jadi fragment menyelesaikan masalah ini.

Kita menulis fragment seperti berikut:
```
{
  First: singlePost(id: "1") {
    ...PostFragment
    author {
      firstName
      lastName
    }
  }
  Second: singlePost(id: "2") {
    ...PostFragment
    author {
      firstName
      lastName
    }
  }
}

fragment PostFragment on Post {
  title
  content
}
```

nampak apa perbezaan? Ya, kita tidak lagi mengulang menulis field ```title``` dan ```content``` yang mana kita telah kumpulan ia pada fragment dan menggunakan ```spread operator(...)``` untuk menggunakannya pada operasi query yang ingin dijalankan.
