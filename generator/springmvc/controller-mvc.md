---
date: 2018-12-05
description: Untuk membuat controller menggunakan Spring MVC, gunakan annotation `@Controller`, kemudian pulangkan nama file template yang perlu digunakan.
---

# Controller MVC

Sebelum ini kita menggunakan annotation `@RestController` untuk membuat
controller. Perkataan REST itu merujuk semua yang kita sudah lihat di tutorial
asas. Anda boleh membaca dengan lebih lanjut di internet.

Untuk membuat controller yang menggunakan Spring MVC, kita hanya perlu
menggunakan annotation `@Controller`. String yang dipulangkan bukanlah data
tetapi nama file template yang perlu digunakan.

Contoh,

```java
package com.jombelajarjava.hello;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.GetMapping;

@Controller
public class GreetingController {
    @GetMapping("/")
    public String greeting() {
        return "greeting";
    }
}
```

Maka, `return "greeting"` di atas bermaksud gunakan file template `greeting`
untuk menghantar code HTML.

File template boleh diletakkan di dalam folder `src/main/resources/templates`.
Contoh file template dengan nama `greeting.mustache` (file extension bergantung
kepada template engine yang anda pilih, rujuk documentation):

```html
<!DOCTYPE html>
<html>
  <head>
    <title>Website Hello</title>
  </head>
  <body>
    <h1>Website Hello</h1>
    <p>Selamat datang ke Website Hello!</p>
  </body>
</html>
```

Run program tersebut, kemudian layari `localhost:8080` untuk melihat page
website tersebut.
