---
title: Argument
taxonomy:
    category: docs
---

Untuk menapis keputusan query kita perlu memberi ```argument``` pada query. Seperti query dibawah kita memberi argument ```id: "1"``` yang mana akan mengembalikan pos yang mempunyai id tersebut sahaja.

```
{
  singlePost(id: "1") {
    title
    content
  }
}
```

keputusan:
```
{
  "data": {
    "singlePost": {
      "title": "Tajuk 2",
      "content": "Beberapa perenggan."
    }
  }
}
```
