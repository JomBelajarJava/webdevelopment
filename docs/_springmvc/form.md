---
date: 2018-12-07
description: Dalam tutorial yang lepas, kita ada membuat form untuk login. Sekarang kita akan melihat bagaimana untuk mengambil input tersebut.
---

# Form

Dalam [tutorial Mustache](mustache.md), kita ada membuat form untuk login.
Sekarang kita akan melihat bagaimana untuk mengambil input tersebut.

Semasa menghantar form, konsepnya sama seperti menghantar request body, cuma
dalam format yang berbeza. Untuk mengambil input daripada form, kita perlu
menggunakan annotation `@ModelAttribute`.

Sebagai contoh, kita bina class `LoginForm`, seperti berikut,

```java
public class LoginForm {
    private String username;
    private String password;

    public LoginForm() {}

    public LoginForm(String username, String password) {
        this.username = username;
        this.password = password;
    }

    // dan method-method lain seperti setter dan getter
}
```

Kemudian tambah method pada controller untuk mengambil input, seperti berikut,

```java
    @PostMapping("/login")
    public String authenticate(@ModelAttribute LoginForm loginForm) {
        return "page";
    }
```

Path dan HTTP method perlu sama dengan code HTML untuk form tersebut, iaitu,

```html
<form action="/login" method="POST">
```

Untuk test controller, kita bina satu page untuk memaparkan username.

Edit `page.mustache` untuk page dashboard:

{% raw %}
```html
    ...
    {{#home}}{{> home}}{{/home}}
    {{#login}}{{> login}}{{/login}}
    {{#dashboard}}{{> dashboard}}{{/dashboard}}
    ...
```
{% endraw %}

kemudian, bina file `dashboard.mustache`:

{% raw %}
```html
{{#success}}
  <p>Selamat datang, {{username}}!</p>
{{/success}}
{{^success}}
  <p>Maaf, password tak betul.</p>
{{/success}}
```
{% endraw %}

Akhir sekali, edit method `authenticate()` untuk menghantar data yang
diperlukan:

```java
    @PostMapping("/login")
    public String authenticate(@ModelAttribute LoginForm loginForm, Model model) {
        boolean success = loginForm.getUsername().equals("admin") &&
                loginForm.getPassword().equals("admin");

        model.addAttribute("title", "Dashboard");
        model.addAttribute("dashboard", true);
        model.addAttribute("success", success);
        model.addAttribute("username", loginForm.getUsername());
        return "page";
    }
```

> **Amaran keras!** Dalam website sebenar, jangan sesekali membandingkan
> password seperti code di atas, sebaliknya, gunakan cryptography.

Untuk test, kita boleh login menggunakan username `admin` dan password `admin`.

## Data transfer object (DTO)

Ini adalah maklumat tambahan. Kita telah melihat sebelum-sebelum ini kita
membuat class untuk menempatkan data input daripada pengguna seperti class
`LoginForm` di atas. Ini adalah salah satu daripada *design pattern* yang
dinamakan *data transfer object*(DTO).
