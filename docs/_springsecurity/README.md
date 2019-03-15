---
permalink: /:collection/
title: Spring Security
description: Spring Security ialah framework yang memberi kemudahan seperti password encryption, authentication, authorization, dan lain-lain.
---

# Spring Security

Spring Security ialah framework yang memberi kemudahan seperti password
encryption, pengesahan identiti serta password (*authentication*), pemberian
akses untuk ahli website (*authorization*), dan lain-lain. Semua kemudahan
tersebut boleh ditulis sendiri oleh programmer. Namun, bagi mengelakkan
kecuaian, Spring terus menyediakan framework security untuk mengendalikan
kesemuanya.

Jika anda ada mengikuti [tutorial Spring MVC](../springmvc/README.md), saya
telah membuat beberapa perubahan pada `GreetingController` sebagai persediaan
untuk tutorial ini, seperti berikut,

```java
@Controller
public class GreetingController {
    @GetMapping("/")
    public String home(Model model) {
        return render(model, "home", null);
    }

    @GetMapping("/login")
    public String login(Model model) {
        return render(model, "login", "Log In");
    }

    @GetMapping("/dashboard")
    public String dashboard(Model model) {
        return render(model, "dashboard", "Dashboard");
    }

    private String render(Model model, String filename, String title) {
        model.addAttribute(filename, true);
        model.addAttribute("title", title);
        return "page";
    }
}
```

Disebabkan code untuk pemilihan page selalu sangat berulang, maka, saya asingkan
code tersebut ke dalam method `render()`. Anda juga boleh buat cara yang sama
untuk mengemaskan code anda.

## Tambah dependency

Framework Spring Security hanyalah sebuah library. Maka, mudah sahaja. Tambah
dependency tersebut dalam file `pom.xml`:

```xml
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-security</artifactId>
    </dependency>
```

Anda juga boleh menggunakan [Spring Initializr](https://start.spring.io/) untuk
generate projek yang mengandungi Spring Security.
