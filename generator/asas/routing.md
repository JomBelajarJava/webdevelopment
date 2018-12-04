---
date: 2018-12-03
description: Routing berasal daripada perkataan route yang bermaksud laluan. Routing dalam aplikasi web bermaksud laluan untuk menggunakan fungsi yang mana.
---

# Routing

Routing berasal daripada perkataan route yang bermaksud laluan. Routing dalam
aplikasi web bermaksud laluan untuk menggunakan fungsi yang mana. Routing
ditentukan dalam controller menggunakan,

* request path, contohnya `/`, `/documentation`
* HTTP method, contohnya `GET` dan `POST`

Dalam tutorial yang lepas, kita ada membuat route `GET` ke path `/` menggunakan
annotation `@GetMapping("/")`. Kita boleh menambah route yang lain pada
controller, contohnya,

```java
@RestController
public class GreetingController {
    @GetMapping("/")
    public String greeting() {
        return "<h1>Hello, World!</h1>";
    }

    @GetMapping("/nama")
    public String name() {
        return "Nama saya ialah...";
    }
}
```

Kita boleh pergi ke `localhost:8080/nama` untuk pergi ke route tersebut.

Untuk membuat route yang menggunakan HTTP method `POST`, kita boleh menggunakan
annotation `@PostMapping`, contohnya,

```java
@RestController
public class GreetingController {
    ...

    @GetMapping("/nama")
    public String name() {
        return "Nama saya ialah...";
    }

    @PostMapping("/nama")
    public String postName() {
        return "Post nama";
    }
}
```

Path tersebut boleh sama dengan path yang lain.

Apabila kita menggunakan browser untuk membuka sesuatu link, kita sebenarnya
membuat request `GET` ke server. Untuk membuat request `POST`, kita boleh
menggunakan mana-mana REST client. Contoh yang saya guna ialah
[RESTClient](https://addons.mozilla.org/en-US/firefox/addon/restclient/). Anda
boleh mencari REST client di internet, kebanyakannya percuma.

Dengan menggunakan REST client, kita boleh mendapatkan response daripada server
bergantung kepada request method yang kita gunakan.
