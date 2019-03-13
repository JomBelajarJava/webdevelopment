---
date: 2018-12-07
description: Redirect ialah cara browser menghantar pengguna ke page yang lain. Dengan menggunakan Spring MVC, kita ada dua pilihan untuk membuat redirect.
---

# Redirect

Dalam [tutorial Form](form.md), selepas login, cuba klik Refresh pada browser.
Browser kemudian akan bertanya sama ada ingin menghantar form semula ataupun
tidak. Ada sesetengah masa, kita tidak mahu menghantar form semula. Untuk itu,
kita boleh menggunakan *redirect*.

Redirect ialah cara browser menghantar pengguna ke page yang lain. Dengan
menggunakan Spring MVC, kita ada dua pilihan untuk membuat redirect. Kita akan
melihat kedua-dua cara tersebut.

Sebelum itu, kita edit dahulu page Dashboard kerana kita masih tiada akses ke
username.

File `dashboard.mustache`:

```html
<p>Selamat datang ke Dashboard!</p>
```

dan tambah controller untuk melihat page tersebut:

```java
    @GetMapping("/dashboard")
    public String dashboard(Model model) {
        model.addAttribute("title", "Dashboard");
        model.addAttribute("dashboard", true);
        return "page";
    }
```

Sekarang kita akan melihat cara untuk redirect ke page Dashboard setelah login.

## Meta refresh

Cara yang pertama adalah dengan menambah meta pada HTML, iaitu,

```html
<meta http-equiv="refresh" content="3; url=/dashboard">
```

Meta di atas bermaksud redirect ke `/dashboard` selepas 3 saat. Kita boleh
menambah message untuk memberitahu pengguna bahawa mereka akan redirect ke page
Dashboard. Sebagai contoh, kita bina file `login_success.mustache`:

```html
<!DOCTYPE html>
<html>
  <head>
    <title>Website Hello</title>
    <meta http-equiv="refresh" content="3; url=/dashboard">
  </head>
  <body>
    <h1>Website Hello</h1>

    <p>Anda telah berjaya login.</p>
    <p>Anda akan dihantar ke Dashboard dalam masa 3 saat.</p>
  </body>
</html>
```

Untuk controller, kita paparkan page `login_success.mustache` setelah berjaya
login, jika gagal kita paparkan login form semula. Contohnya,

```java
    @PostMapping("/login")
    public String authenticate(@ModelAttribute LoginForm loginForm, Model model) {
        boolean success = loginForm.getUsername().equals("admin") &&
                loginForm.getPassword().equals("admin");

        if (success) {
            return "login_success";
        }

        return login(model);
    }
```

## redirect:

Cara yang kedua adalah dengan menggunakan cara redirect daripada Spring MVC,
iaitu dengan memulangkan string bermula `"redirect:"` kemudian url page. Contohnya,

```java
    @PostMapping("/login")
    public String authenticate(@ModelAttribute LoginForm loginForm, Model model) {
        boolean success = loginForm.getUsername().equals("admin") &&
                loginForm.getPassword().equals("admin");

        if (success) {
            return "redirect:/dashboard";
        }

        return login(model);
    }
```

Dengan cara ini, pengguna akan terus ke page dashboard. Cuba refresh browser
setelah berjaya login.
