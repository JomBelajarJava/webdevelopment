---
date: 2018-12-11
description: Uberjar ialah pakej aplikasi yang mengandungi keseluruhan aplikasi tersebut. Maka, bina uberjar, upload file uberjar ke server, dan run.
---

# Uberjar

Uberjar ialah pakej aplikasi yang mengandungi keseluruhan aplikasi tersebut.
Maka, jika kita ada menyewa satu server, kita boleh install Java, kemudian
membina uberjar, kemudian upload file uberjar tersebut ke server, dan run.

Untuk membina uberjar, kita boleh menggunakan command Maven, iaitu

```
./mvnw clean package
```

Setelah selesai, file uberjar akan ada dalam folder `target` dengan nama bermula
nama projek dan version, contohnya `hello-0.0.1-SNAPSHOT.jar`. File `.jar`
itulah yang dinamakan uberjar. Kita hanya perlu satu file itu sahaja untuk
menjalankan website.

Untuk run uberjar, kita boleh menggunakan command Java berserta flag `-jar`,
contohnya,

```
java -jar hello-0.0.1-SNAPSHOT.jar
```

Jika anda cuba command tersebut di komputer anda, website akan berjalan sama
sahaja seperti semasa kita membuat development.

## Server hosting

Untuk menjadikan website tersebut boleh diakses oleh orang ramai, kita perlu
menghubungi *internet service provider*(ISP), seperti UniFi. Cara yang lain
ialah menyewa server di mana-mana server hosting.

Antara server hosting yang popular ialah:

- [DigitalOcean](https://www.digitalocean.com/)
- [Linode](https://www.linode.com/)
- [OVH](https://www.ovh.com/world/) (EU)

Dedicated server bermaksud satu mesin dimiliki oleh kita sepenuhnya. Virtual
private server bermaksud aplikasi kita nanti berjalan dalam virtual machine.
Pilih virtual private server kerana murah.

Cara-cara untuk setup server biasanya disediakan oleh website server hosting
tersebut. Jika anda ada masalah, anda boleh menghubungi mereka untuk support.
