---
layout: docsv2
title: Pengenalan
---

Bono adalah sebuah framework aplikasi web yang cepat dan tepat guna untuk pengembangan aplikasi web.

### Kenapa Bono?
<hr/>

- Membangun aplikasi berbasis web lebih cepat dari biasanya,
- Menambahkan modul baru dengan cepat dengan fitur scaffold,
- Menggunakan standar template untuk kebutuhan SCRUD (search, create, update, dan delete),
- Tampilan yang dinamis dan fungsi-fungsi bantuan untuk membuat API dan halaman web.

### Konsep
<hr/>

Beberapa hal di bawah ini adalah konsep-konsep yang mendukung dan menyusun Bono.

#### Middleware

Middleware dapat menambahkan kemampuan Request Handling. Contoh: session, autentikasi, dll.

Sejak versi 2.0, Bono menghilangkan Provider. Fungsionalitas yang biasanya diimplementasi di Bono 1.0 di Provider dialihkan ke implementasi menggunakan Middleware.

#### Bundle

Pada versi 2.0, Bono memperkenalkan konsep Bundle. Bundle berlaku seperti layaknya sub aplikasi. Bahkan aplikasi utama pada dasarnya di-compose dalam wujud Bundle. 

Bundle juga menggantikan keberadaan Controller pada Bono 1.0.

Bundle dapat menambahkan kemampuan pada aplikasi berbasis Bono. Contoh: modul chat, mesin CMS dan blog, forum dan komunitas, dll.

#### Routing

Routing adalah kemampuan pada Bono untuk mendefinisikan pengalihan Request untuk ditangani oleh spesifik fungsi berdasarkan pola URI.

