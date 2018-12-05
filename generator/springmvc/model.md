---
date: 2018-12-05
description: Sekarang kita akan cuba memasukkan data daripada code Java ke template dengan menambah parameter Model untuk method controller berkenaan.
---

# Model

Sekarang kita akan cuba memasukkan data daripada code Java ke template. Caranya
dengan menambah parameter `Model` untuk method controller berkenaan, kemudian
gunakan method daripada class `Model` untuk memasukkan data. Contohnya,

```java
package com.jombelajarjava.hello;

import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.GetMapping;

@Controller
public class GreetingController {
    @GetMapping("/")
    public String greeting(Model model) {
        model.addAttribute("name", "Mike Tyson");
        return "greeting";
    }
}
```

Kita boleh menggunakan syntax template untuk memaparkan data tersebut. Contoh
untuk template Mustache adalah dengan menggunakan `{{` dan `}}`, seperti
berikut,

```html
<!DOCTYPE html>
<html>
  <head>
    <title>Website Hello</title>
  </head>
  <body>
    <h1>Website Hello</h1>
    <p>Selamat datang, {{name}}!</p>
  </body>
</html>
```

Layari `localhost:8080` untuk test controller.

Parameter yang lain boleh sahaja ditambah pada method tersebut, contohnya jika
kita ingin mengambil parameter, seperti berikut,

```java
    @GetMapping("/")
    public String greeting(@RequestParam String name, Model model) {
        model.addAttribute("name", name);
        return "greeting";
    }
```

Layari `localhost:8080/?name=Mike Tyson` untuk test method tersebut.
