---
date: 2018-12-09
description: Authorization ialah proses untuk memberi akses kepada ahli website. Kita akan menggunakan fungsi authorization daripada Spring Security.
---

# Authorization

Dalam [tutorial Authentication](authentication.md), walaupun tidak log in,
pengguna masih boleh melayari page dashboard. Untuk menyekat akses pengguna yang
tidak log in, kita boleh menggunakan fungsi authorization daripada Spring
Security. Authorization ialah proses untuk memberi akses kepada ahli website.

Kita boleh configure authorization dalam method `configure()` sama seperti
authentication. Kita tambah seperti berikut:

```java
    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http
                .formLogin()
                .loginPage("/login")
                .defaultSuccessUrl("/dashboard")
                .and()
                .logout()
                .and()
                .authorizeRequests()
                .antMatchers("/dashboard/**").authenticated()
                .antMatchers("/admin/**").hasRole("ADMIN");
    }
```

Selepas configure authentication, kita panggil method `and()` untuk configure
bahagian lain dan pilih `authorizeRequests()` untuk configure authorization.
Kemudian kita menggunakan `antMatchers()` untuk memilih path mana yang perlu
disekat, jadi kita tulis `"/dashboard/**"` yang bermaksud mana-mana path yang
bermula dengan `/dashboard`. Kemudian kita panggil method `authenticated()`
untuk path tersebut menandakan hanya pengguna yang login sahaja yang boleh
akses.

Setelah selesai, anda boleh cuba logout, kemudian layari
`localhost:8080/dashboard` dan website akan redirect anda ke page login.

## Role

Jika anda perhatikan dalam [tutorial Authentication](authentication.md), semasa
kita membuat pengguna untuk demo, kita ada meletakkan role. Semasa menggunakan
website, bukan semua ahli boleh akses ke semua page. Ada ahli yang berperanan
sebagai admin, ada yang ahli berbayar, dan ada ahli biasa. Maka kita boleh
menggunakan role untuk membezakan kumpulan-kumpulan ini.

Sekarang kita cuba membuat page yang khas untuk admin sahaja. Kandungan page
terpulang kepada anda. Controller untuk page adalah seperti berikut:

```java
    @GetMapping("/admin")
    public String admin(Model model) {
        return render(model, "admin", "Admin");
    }
```

Kita juga akan membuat satu lagi pengguna biasa untuk melihat sama ada dia boleh
mengakses page admin.

```java
        UserDetails admin = ...

        UserDetails pleb =
                User.withUsername("user")
                        .password("password")
                        .roles("USER")
                        .passwordEncoder(passwordEncoder::encode)
                        .build();

        return new InMemoryUserDetailsManager(admin, pleb);
```

Perhatikan role untuk pengguna tersebut berbeza dengan admin.

Untuk menyekat akses untuk page admin, kita boleh menambah path yang lain
menggunakan `antMatchers()` kemudian guna method `hasRole()`, seperti berikut:

```java
                ...
                .and()
                .authorizeRequests()
                .antMatchers("/dashboard/**").authenticated()
                .antMatchers("/admin/**").hasRole("ADMIN");
                ...
```

Setelah selesai, cuba login sebagai pengguna biasa, dan layari
`localhost:8080/admin` dan page akan memaparkan '*forbidden*'.

Pengguna juga boleh memiliki lebih daripada satu role.
