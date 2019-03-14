---
date: 2018-12-06
description: Sekarang kita akan melihat dengan lebih mendalam syntax template Mustache.
---

# Mustache

Sekarang kita akan melihat dengan lebih mendalam syntax template Mustache.

## Seksyen

Syntax untuk seksyen bermula dengan variable berserta tanda `#` pada nama,
kemudian diakhiri dengan variable berserta tanda `/`, contohnya,

{% raw %}
```html
{{#title}}
  <h1>Website Hello</h1>
  <p>Website kegemaran anda</p>
{{/title}}
```
{% endraw %}

Code HTML di atas bermakna jika kita ada menghantar apa-apa data ke `title`
tidak kiralah jenis apa, contohnya,

```java
model.addAttribute("title", true);
```

maka bahagian tersebut akan terpapar. Syntax tersebut kita boleh baca seperti
**if title**.

Lawan kepada syntax `#` ialah `^`. Kita boleh baca seperti **if not title**.
Contoh,

{% raw %}
```html
{{^title}}
  <p>Hmmm, tiada tajuk</p>
{{/title}}
```
{% endraw %}

Menggunakan kedua-dua syntax tersebut, kita boleh membuat sesuatu seperti **if
else**. Contoh,

{% raw %}
```html
{{#title}}
  <h1>Website Hello &middot; {{title}}</h1>
{{/title}}
{{^title}}
  <h1>Website Hello</h1>
{{/title}}
```
{% endraw %}

Code di atas bermaksud kita paparkan tajuk website berserta tajuk kecil yang
kita masukkan daripada code Java. Jika kita tidak meletakkan tajuk kecil,
website hanya akan memaparkan tajuk website.

## Loop

Syntax untuk seksyen, `#`, boleh menjadi loop bergantung kepada jenis data
structure. Jika kita menghantar array atau list, Mustache akan mengulangi blok
code tersebut untuk setiap data. Kita boleh menggunakan `this` untuk merujuk
data dalam array atau list.

Contoh,

{% raw %}
```html
<p>
  Kucing saya:
  <ul>
  {{#kucing}}
    <li>{{this}}</li>
  {{/kucing}}
  </ul>
</p>
```
{% endraw %}

Dengan menggunakan code Java seperti ini,

```java
model.addAttribute("kucing", Arrays.asList("Tompok", "Comel", "Maru"));
```

template tersebut akan berkembang menjadi,

```html
<p>
  Kucing saya:
  <ul>
    <li>Tompok</li>
    <li>Comel</li>
    <li>Maru</li>
  </ul>
</p>
```

## Fields object

Contoh di atas menggunakan keyword `this`, yang juga merupakan keyword yang kita
pernah guna dalam object-oriented programming. Ya, kita juga boleh menggunakan
syntax `#` untuk merujuk fields dalam sesuatu object. Katakanlah kita ada
membuat class Kucing,

```java
class Kucing {
    String nama;
    String jenis;

    Kucing(String nama, String jenis) {
        this.nama = nama;
        this.jenis = jenis;
    }
}
```

dan menghantar object Kucing ke model,

```java
model.addAttribute("kucing", new Kucing("Maru", "Parsi"));
```

maka, kita boleh membuat seperti berikut untuk mendapatkan nama dan jenis kucing:

{% raw %}
```html
<p>
  {{#kucing}}
    Nama: {{nama}}, Jenis: {{jenis}}
  {{/kucing}}
</p>
```
{% endraw %}

Kita juga boleh menghantar beberapa object dalam array atau list, dan syntax
yang sama akan sama mengulang untuk semua object sambil mengambil data dalam
fields.

## Partial

Partial bermaksud sebahagian. Dalam Mustache, partial bermaksud syntax untuk
memasukkan sebahagian HTML daripada file yang lain. Syntax tersebut bermula
dengan `>` kemudian nama file yang ingin kita masukkan.

Katakanlah kita ada file `login.mustache`, maka kita boleh menggunakan {% raw
%}`{{> login}}`{% endraw %} untuk memasukkan semua kandungan daripada file
`login.mustache`.

Dengan menggunakan syntax partial dan seksyen, kita boleh memilih kandungan page
tanpa menulis semula bahagian HTML yang lain, contohnya,

File `page.mustache`:

{% raw %}
```html
<!DOCTYPE html>
<html>
  <head>
    {{#title}}<title>{{title}} &middot; Website Hello</title>{{/title}}
    {{^title}}<title>Website Hello</title>{{/title}}
  </head>
  <body>
    {{#title}}<h1>Website Hello &middot; {{title}}</h1>{{/title}}
    {{^title}}<h1>Website Hello</h1>{{/title}}

    {{#home}}{{> home}}{{/home}}
    {{#login}}{{> login}}{{/login}}
  </body>
</html>
```
{% endraw %}

File `home.mustache`:

```html
<p>Selamat datang! Sila <a href="/login">Log In</a>.</p>
```

File `login.mustache`:

```html
<form action="/login" method="POST">
  <p>Username: <input type="text" name="username"></p>
  <p>Password: <input type="password" name="password"></p>
  <p><input type="submit" value="Log In"></p>
</form>
```

Untuk memilih kandungan, kita tetapkan di controller, seperti berikut:

```java
@Controller
public class GreetingController {
    @GetMapping("/")
    public String home(Model model) {
        model.addAttribute("home", true);
        return "page";
    }

    @GetMapping("/login")
    public String login(Model model) {
        model.addAttribute("title", "Log In");
        model.addAttribute("login", true);
        return "page";
    }
}
```

Hasilnya,

![Gambar home website]({{ site.baseurl }}/assets/img/website_home.png)

![Gambar login website]({{ site.baseurl }}/assets/img/website_login.png)
