# Setup Projek

Seperti projek Java yang lain, kita boleh menggunakan build tool seperti Maven
atau Gradle untuk Spring Boot. Spring Boot hanyalah sebuah library. Jadi, kita
tambah sahaja dependency untuk Spring Boot menggunakan build tool.

Satu cara yang lebih mudah adalah dengan menggunakan website [Spring
Initializr](https://start.spring.io/) untuk generate projek. Jadi,

* buka website tersebut,
* isikan maklumat mengenai projek,
* pilih dependency 'Web' dan 'Devtools',
* kemudian generate.

Setelah siap download, extract file tersebut, kemudian import projek menggunakan
mana-mana IDE. Dalam tutorial ini, saya akan menggunakan IntelliJ Community
Edition. Berdasarkan pemerhatian saya, IDE Eclipse seperti lebih mudah digunakan
untuk Spring Boot untuk refresh projek. Tetapi tidak mengapalah, terpulang
kepada anda.

Projek yang saya generate mempunyai Group id `com.jombelajarjava` dan Artifact
id `hello` menggunakan Maven.
