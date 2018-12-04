---
date: 2018-12-04
description: Spring pada asalnya adalah merupakan dependency injection framework. Kemudian, semakin lama, semakin berkembang sehingga menjadi web framework.
---

# Autowire

Spring pada asalnya adalah merupakan *dependency injection framework*. Kemudian,
semakin lama, semakin berkembang sehingga menjadi web framework. Jika anda
perasan, selama ini kita hanya menambah annotation `@RestController` untuk
menjadikan class tersebut sebuah controller. Kita tidak membina object
menggunakan **new** pun untuk controller tersebut.

Sekarang kita akan melihat bagaimana untuk membuat *dependency injection*
menggunakan Spring.

## @Repository

Annotation `@Repository` boleh digunakan untuk class DAO, iaitu class yang
mengandungi operasi database. Untuk demo tutorial ini, kita akan membuat class
untuk memilih bahasa berdasarkan pengguna, seperti berikut,

```java
package com.jombelajarjava.hello;

import org.springframework.stereotype.Repository;

@Repository
public class LanguageRepository {
    public String getUserLanguage(String name) {
        switch (name) {
            case "Muhammad Ali": return "ms";
            case "Kamen Rider": return "ja";
            default: return "en";
        }
    }
}
```

Dalam situasi sebenar, class tersebut akan mendapatkan data daripada database.

## @Service

Annotation `@Service`, seperti namanya, boleh digunakan untuk class service.
Untuk demo, kita akan membuat class yang akan memilih greeting mengikut bahasa.

```java
package com.jombelajarjava.hello;

import org.springframework.stereotype.Service;

@Service
public class GreetingService {
    private LanguageRepository languageRepository;

    public String getGreeting(String name) {
        String language = languageRepository.getUserLanguage(name);

        switch (language) {
            case "ms": return "Selamat datang, " + name + "!";
            case "ja": return "Irasshaimase, " + name + "!";
            default: return "Welcome, " + name + "!";
        }
    }
}
```

## @Autowired

`@Autowired` ialah annotation untuk membuat *dependency injection* menggunakan
Spring Boot, sama seperti `@Inject` jika menggunakan framework yang lain.
Disebabkan class `GreetingService` di atas memerlukan `LanguageRepository`, maka
kita akan menggunakan `@Autowired` di situ.

```java
package com.jombelajarjava.hello;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

@Service
public class GreetingService {
    private LanguageRepository languageRepository;

    @Autowired
    public GreetingService(LanguageRepository languageRepository) {
        this.languageRepository = languageRepository;
    }

    ...
}
```

Setelah selesai, kita boleh menggunakan service dalam controller, seperti
berikut,

```java
package com.jombelajarjava.hello;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.*;

@RestController
public class GreetingController {
    private GreetingService greetingService;

    @Autowired
    public GreetingController(GreetingService greetingService) {
        this.greetingService = greetingService;
    }

    @GetMapping("/enter")
    public String greeting(@RequestParam String name) {
        return greetingService.getGreeting(name);
    }
}
```

Sekarang kita sudah boleh test code dengan melayari
`localhost:8080/enter?name=Muhammad Ali` di browser.
