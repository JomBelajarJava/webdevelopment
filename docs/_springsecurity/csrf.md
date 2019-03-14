---
date: 2018-12-09
description: CSRF ialah satu bentuk serangan siber di mana pengguna tanpa sedar telah menjalankan sesuatu operasi setelah mereka log in.
---

# Cross-Site Request Forgery (CSRF)

CSRF ialah satu bentuk serangan siber di mana pengguna tanpa sedar telah
menjalankan sesuatu operasi setelah mereka log in. Serangan ini boleh berlaku
disebabkan kebolehan web browser yang boleh melayari mana-mana website.

Sebagai contoh, setelah mangsa log in ke website bank, seorang penjenayah menipu
mangsa menyuruhnya untuk pergi ke satu website. Setelah melayari website
tersebut, satu script telah berjalan untuk menghantar form untuk memindahkan
wang ke akaun penjenayah. Jika website bank tidak mencegah CSRF, website
tersebut tidak boleh tahu sama ada pemindahan wang tersebut adalah kehendak
mangsa ataupun tidak, kerana mangsa telah log in.

Anda boleh baca dengan lebih lanjut di [website OWASP][owasp].

Bagi mengatasi masalah ini, satu cara adalah dengan meletakkan token pada
sesuatu form. Apabila pengguna submit form tersebut, website akan check sama ada
token tersebut wujud dan sama seperti yang diletakkan. Maka, penjenayah tidak
boleh membuat serangan CSRF kerana form tersebut perlu dibuka terlebih dahulu
oleh pengguna.

Kelebihan menggunakan Spring Security ialah pemeriksaan untuk CSRF telahpun
disediakan secara default. Kita hanya perlu meletakkan token pada form.

Sebelum itu, kita tambah config untuk Spring Security terlebih dahulu. Kita bina
class `WebSecurityConfig`, seperti berikut,

```java
@Configuration
@EnableWebSecurity
public class WebSecurityConfig extends WebSecurityConfigurerAdapter {
}
```

dan segala-galanya sudah siap. Anda boleh cuba menghantar form, dan lihat
website akan menulis '*forbidden*' setelah submit. Maknanya, kita tidak boleh
submit form kerana tiada token.

## Anti-forgery token

Jika anda menggunakan template engine yang lain, anda boleh terus mendapatkan
token dalam template. Disebabkan kita menggunakan Mustache untuk tutorial ini,
maka kita perlu menghantar token tersebut menggunakan model.

Berikut ialah template untuk meletakkan token dalam file
`anti_forgery_field.mustache`:

{% raw %}
```html
{{#_csrf}}
  <input type="hidden" name="{{parameterName}}" value="{{token}}">
{{/_csrf}}
```
{% endraw %}

Kemudian kita boleh menggunakan partial untuk memasukkan template tersebut ke
dalam setiap form, seperti berikut:

{% raw %}
```html
<form action="/login" method="POST">
  {{> anti_forgery_field}}
  <p>Username: <input type="text" name="username"></p>
  <p>Password: <input type="password" name="password"></p>
  <p><input type="submit" value="Log In"></p>
</form>
```
{% endraw %}

Untuk mendapatkan token, kita boleh menambah parameter untuk `CsrfToken` pada
method controller kita, seperti berikut:

```java
    @GetMapping("/login")
    public String login(CsrfToken csrfToken, Model model) {
        model.addAttribute("_csrf", csrfToken);
        return render(model, "login", "Log In");
    }
```

Sekarang website sudah boleh menerima form seperti biasa dan dengan lebih
selamat.

## Interceptor

Jika kita melihat kembali method controller di atas, kita mungkin merasakan agak
leceh kerana perlu menulis code untuk setiap page yang mengandungi form. Jadi,
kita boleh menggunakan interceptor untuk memudahkan kita.

Kita tahu bahawa controller mengambil request kemudian memberi response sebagai
output. Interceptor ialah proses tambahan yang boleh berjalan sepanjang proses
controller tersebut.

Untuk membuat interceptor, bina class yang implement `HandlerInterceptor` dan
override salah satu method, seperti berikut,

```java
public class CsrfTokenInterceptor implements HandlerInterceptor {
    @Override
    public void postHandle(HttpServletRequest request, HttpServletResponse response,
                           Object handler, ModelAndView modelAndView) throws Exception {
        CsrfToken csrfToken = (CsrfToken) request.getAttribute("_csrf");
        if (modelAndView != null) {
            modelAndView.addObject("_csrf", csrfToken);
        }
    }
}
```

Dalam code di atas, kita terus memasukkan token ke dalam model, jadi kita tidak
perlu lagi memasukkan token tersebut dalam controller.

Untuk mendaftar interceptor tersebut, bina Bean untuk Spring:

```java
@Configuration
public class Config {
    @Bean
    public CsrfTokenInterceptor csrfTokenInterceptor() {
        return new CsrfTokenInterceptor();
    }
    ...
}
```

Kemudian inject ke dalam class configuration lain yang implement
`WebMvcConfigurer` dan override method `addInterceptors()`, seperti berikut:

```java
@Configuration
public class WebMvcConfig implements WebMvcConfigurer {
    private CsrfTokenInterceptor csrfTokenInterceptor;

    @Autowired
    public WebMvcConfig(CsrfTokenInterceptor csrfTokenInterceptor) {
        this.csrfTokenInterceptor = csrfTokenInterceptor;
    }

    @Override
    public void addInterceptors(InterceptorRegistry registry) {
        registry.addInterceptor(csrfTokenInterceptor);
    }
}
```

Sekarang, kita sudah boleh edit controller ke bentuk asal, seperti berikut:

```java
    @GetMapping("/login")
    public String login(Model model) {
        return render(model, "login", "Log In");
    }
```

[owasp]: https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)
