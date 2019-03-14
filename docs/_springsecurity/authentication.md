---
date: 2018-12-09
description: Authentication ialah proses untuk mengesahkan identiti username dan password pengguna. Kita akan menggunakan Spring Security untuk authentication.
---

# Authentication

Authentication ialah proses untuk mengesahkan identiti username dan password
pengguna. Sebelum kita melihat proses authentication, kita lihat maksud cookie
dan session terlebih dahulu.

Cookie ialah data yang dihantar oleh website dan boleh disimpan dalam web
browser pengguna. Website hanyalah seperti aplikasi biasa, jadi website
memerlukan cookie untuk mengenalpasti siapakah pengguna yang sedang
berinteraksi. Cookie yang digunakan untuk mengenalpasti identiti ini disebut
sebagai session.

Jadi, secara teorinya, semasa log in, setelah submit form, website akan mencari
pengguna menggunakan username, kemudian menggunakan cryptography untuk
mengesahkan password. Jika sama, website akan meletakkan session pada web
browser. Untuk semua request yang seterusnya, website hanya perlu melihat
session tersebut untuk mengenali siapa pengguna, maka pengguna tidak perlu lagi
log in berulang kali.

Anda boleh menulis sendiri code untuk membuat authentication jika anda mahu.
Untuk tutorial ini, kita akan terus menggunakan Spring Security untuk membuat
authentication.

Dalam [tutorial CSRF](csrf.md), kita telah membuat class `WebSecurityConfig`.
Kita boleh menambah configuration dalam class tersebut untuk authentication.
Caranya dengan override method `configure()`, seperti berikut:

```java
@Configuration
@EnableWebSecurity
public class WebSecurityConfig extends WebSecurityConfigurerAdapter {
    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http
                .formLogin()
                .loginPage("/login")
                .defaultSuccessUrl("/dashboard")
                .and()
                .logout();
    }
}
```

Method dalam `HttpSecurity` adalah berangkai, sama seperti design pattern
Builder, jadi kita ikut sahaja.

Untuk code di atas, kita configure login dengan memanggil method `formLogin()`
kemudian kita menetapkan url untuk page login dan url untuk redirect setelah
berjaya login. Kemudian, kita memanggil method `and()` untuk configure bahagian
lain dan kita memasang fungsi logout dengan memanggil method `logout()`.

Setelah selesai, kita override satu lagi method, iaitu `userDetailsService()`
dalam class yang sama. Method ini adalah untuk kita beritahu Spring Security,
service mana yang perlu digunakan untuk mencari maklumat pengguna. Disebabkan
kita tidak menggunakan database, kita boleh meletakkan pengguna sebagai demo
terlebih dahulu.

Sebelum itu, kita inject class untuk password hashing terlebih dahulu kerana
kita perlu hash password untuk pengguna demo:

```java
    private PasswordEncoder passwordEncoder;

    @Autowired
    public WebSecurityConfig(PasswordEncoder passwordEncoder) {
        this.passwordEncoder = passwordEncoder;
    }
```

Method `userDetailsService()` adalah seperti berikut:

```java
    @Bean
    @Override
    protected UserDetailsService userDetailsService() {
        UserDetails admin =
                User.withUsername("admin")
                        .password("admin")
                        .roles("ADMIN")
                        .passwordEncoder(passwordEncoder::encode)
                        .build();

        return new InMemoryUserDetailsManager(admin);
    }
```

Ini bermakna user tersebut hanya ada dalam memory.

Code `passwordEncoder::encode` di atas adalah syntax Java 8. Jika anda
menggunakan Java 7, anda boleh menggunakan anonymous class seperti ini:

```java
        ...
        .passwordEncoder(new Function<String, String>() {
            @Override
            public String apply(String rawPassword) {
                return passwordEncoder.encode(rawPassword);
            }
        })
        ...
```

Sekarang sudah selesai, anda boleh test login sekarang. Jika berjaya, anda akan
ke page dashboard, jika tidak anda akan ke page login semula.

Jika anda perhatikan link di web browser, semasa gagal login, link akan bertukar
menjadi [http://localhost:8080/login?error](http://localhost:8080/login?error).
Jadi, anda boleh check parameter `error`. Jika ada nilai, kita boleh memaparkan
message error pada page login.

Anda boleh menambah form untuk logout seperti berikut dalam page dashboard:

{% raw %}
```html
<form action="/logout" method="POST">
  {{> anti_forgery_field}}
  <input type="submit" value="Logout">
</form>
```
{% endraw %}

## Mendapatkan maklumat pengguna

Kita boleh mendapatkan maklumat pengguna dalam controller. Contoh jika kita
ingin memaparkan username dalam page dashboard,

```html
<p>Selamat datang ke Dashboard, {{username}}!</p>
```

Untuk mendapatkan maklumat pengguna, tambah parameter untuk `Principal` pada
method controller, seperti berikut,

```java
    @GetMapping("/dashboard")
    public String dashboard(Principal principal, Model model) {
        model.addAttribute("username", principal.getName());
        return render(model, "dashboard", "Dashboard");
    }
```

Satu lagi penambahbaikan yang kita boleh buat menggunakan `Principal` adalah
redirect pengguna terus ke dashboard jika mereka telah login, seperti berikut,

```java
    @GetMapping("/login")
    public String login(Principal principal, Model model) {
        if (principal == null) {  // jika belum log in
            return render(model, "login", "Log In");
        }
        return "redirect:/dashboard";
    }
```
