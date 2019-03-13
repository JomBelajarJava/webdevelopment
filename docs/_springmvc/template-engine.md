---
date: 2018-12-05
description: Template engine boleh menggunakan syntax-syntax yang istimewa untuk menetapkan bahagian HTML yang boleh diubah, kemudian diubah daripada code Java.
---

# Template Engine

Kita boleh menghantar code HTML ke browser menggunakan String. Namun, apabila
difikirkan, jika code HTML terlalu panjang, tidakkah berserabut code kita nanti.
Jadi, kita boleh memisahkan code HTML ke file yang lain. Apabila code HTML sudah
terpisah daripada code Java, bagaimana pula untuk mengubah kandungan code HTML
tersebut?

Perkenalkan template engine!

Template engine boleh menggunakan syntax-syntax yang istimewa untuk menetapkan
bahagian HTML yang boleh diubah. Bahagian tersebut kemudiannya boleh diubah
daripada code Java.

Syntax-syntax tersebut berbeza mengikut template engine. Anda boleh pergi ke
[Spring Initializr](https://start.spring.io/) (klik pada "*Switch to the full
version*" di bahagian bawah page) untuk melihat senarai template engine yang
boleh digunakan untuk aplikasi Spring Boot, antaranya,

* Thymeleaf
* Freemarker
* Mustache

## Mustache

Untuk tutorial ini, kita akan menggunakan template
[Mustache](https://mustache.github.io/mustache.5.html) kerana Mustache adalah
template yang paling simple. Jika anda mahukan lebih feature, anda boleh lihat
template engine yang lain.

Untuk menggunakan Mustache, tambah dependency berikut pada file `pom.xml`:

```xml
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-mustache</artifactId>
		</dependency>
```

Anda juga boleh menggunakan [Spring Initializr](https://start.spring.io/) untuk
generate projek yang menggunakan Mustache.
