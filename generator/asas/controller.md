# Controller

Struktur code untuk aplikasi web sama sahaja seperti yang kita lihat di
[tutorial database](/java/database/). Cuma satu yang saya tidak beritahu dalam
tutorial tersebut ialah controller. Dalam tutorial database, controller untuk
code tersebut ialah di bahagian pengambilan input. Maksudnya, controller ialah
bahagian code yang mengawal interaksi dengan pengguna.

Sekarang kita cuba membuat controller untuk memaparkan `Hello, World!` di
browser. Create class baru dengan nama `GreetingController` dengan kandungan
berikut:

```java
package com.jombelajarjava.hello;

import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class GreetingController {
    @GetMapping("/")
    public String greeting() {
        return "Hello, World!";
    }
}
```

Setelah update server, refresh browser dan kita boleh melihat `Hello, World!`
terpapar di skrin.

----

Sekarang kita cuba sesuatu yang lain sedikit. Kita akan menghantar code HTML ke
browser. Tukar baris,

```java
return "Hello, World!";
```

ke

```java
return "<h1>Hello, World!</h1>";
```

Setelah update, refresh browser dan kita boleh nampak tulisan `Hello, World!`
menjadi besar. Jadi, itulah web development secara keseluruhannya, tiada
istimewanya berbanding aplikasi console, cuma perlu memanipulasi string ke
format yang difahami oleh browser.
