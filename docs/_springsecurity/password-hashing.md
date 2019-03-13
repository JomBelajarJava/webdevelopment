---
date: 2018-12-08
description: Melihat cara untuk membuat password hashing menggunakan Spring Security dengan Spring Boot.
---

# Password Hashing

Sebelum kita melihat maksud *hash*, kita akan menulis code terlebih dahulu.
Kemudian, kita akan melihat penjelasan.

Mula-mula, kita perlu membina class untuk meletakkan *configuration*. Kita
namakan class tersebut `Config`, kemudian tambah annotation `@Configuration`,
seperti berikut,

```java
package com.jombelajarjava.hello;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.security.crypto.bcrypt.BCryptPasswordEncoder;
import org.springframework.security.crypto.password.PasswordEncoder;

@Configuration
public class Config {
}
```

Dalam configuration, kita akan memilih algorithm untuk membuat *hashing*. Kita
akan pilih BCrypt. Jadi, kita tambah method seperti berikut,

```java
@Configuration
public class Config {
    @Bean
    public PasswordEncoder passwordEncoder() {
        return new BCryptPasswordEncoder();
    }
}
```

Annotation `@Bean` tersebut adalah untuk Spring. Nanti apabila kita meletakkan
parameter `PasswordEncoder` menggunakan `@Autowired`, Spring akan membuat
*dependency injection*. Konsep ini sama seperti `@Provider` dalam Google Guice.

Untuk test algorithm BCrypt, kita boleh bina controller tambahan hanya untuk
test. Contoh,

File `test.mustache`:

```html
<p>Result: {{result}}</p>
```

File `TestController.java`:

```java
@Controller
public class TestController {
    @Autowired
    PasswordEncoder passwordEncoder;

    @GetMapping("/test")
    public String test(Model model) {
        String encoded = passwordEncoder.encode("admin");
        model.addAttribute("result", encoded);
        return "test";
    }
}
```

Setelah selesai, layari
[http://localhost:8080/test](http://localhost:8080/test). Hasilnya,

![Gambar result hash]({{ site.baseurl }}/assets/img/hashed.png)

Anda boleh refresh dan lihat nilai tersebut berubah. Jika anda ingin membuat
controller untuk pendaftaran ahli website, anda perlu menyimpan result tersebut
sebagai password.

## Cryptography

Sekarang kita akan melihat penjelasan mengenai code di atas. Kepada sesiapa yang
sudah faham, boleh skip bahagian ini.

Cryptography ialah pembelajaran mengenai teknik untuk menukar sesuatu data ke
dalam bentuk yang lain dengan tujuan supaya orang lain yang tidak berkenaan
tidak dapat memahami data tersebut. Dalam cryptography, ada dua kategori,
iaitu encryption dan hashing.

Encryption ialah menukar data ke bentuk yang lain, dan hasilnya juga boleh
bertukar ke bentuk yang asal (*decryption*). Hashing pula ialah menukar data ke
bentuk yang lain sehingga tidak boleh lagi bertukar ke bentuk yang asal.

Dalam code di atas, kita menggunakan BCrypt, iaitu salah satu algorithm untuk
membuat hashing. Tujuan kita menggunakan hash untuk password adalah supaya
developer website itu sendiri tidak dapat mengakses password pengguna. Jika
developer hanya membuat encryption, mereka dengan mudah mendapatkan bentuk asal
password daripada pengguna, kemudian boleh menggodam akaun pengguna di website
lain.

Sekarang kita sudah faham beza encryption dan hashing. Jadi, jika anda
menggunakan mana-mana website, anda boleh cuba klik 'Forgot password' untuk
melihat sama ada mereka memulangkan password anda atau membuatkan anda membina
password baru. Jika website tersebut boleh memulangkan password anda, maksudnya
penyimpanan password tersebut tidak betul.
