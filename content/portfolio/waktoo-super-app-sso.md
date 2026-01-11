+++
date = '2025-09-10T23:11:28+07:00'
draft = false
title = 'Waktoo Super App - Sistem Autentikasi terpadu untuk multi aplikasi'
featured_image = "/blog/images/portfolio/waktoo-superapp-auth-sso-flow.png"
tags = ["portfolio", "backend", "php", "laravel", "auth"]
+++

Mengimplementasikan sistem otentikasi berbasis JWT yang menyatukan tiga aplikasi
yang sebelumnya terpisah menjadi Waktoo Super App. 
Ini memungkinkan Single Sign-On (SSO) yang lancar, dengan tetap mempertahankan alur otentikasi API yang sudah ada.

<!--more-->

## Gambaran Umum

Di Waktoo (bagian dari Kazee), kami mengembangkan tiga produk SASS, yaitu **Waktoo CRM**, **Waktoo HRM**, dan **Waktoo Commerce**.  

Kemudian kami mulai mengembangkan waktoo Super App - Sebuah platform yang menggabungkan ketiga produk diatas menjadi satu kesatuan.

Salah satu tantangan terbesarnya ialah sistem autentikasi. Masing-masing produk sudah memiliki sistem autentikasinya masing masing.
Kami membutuhkan sistem autentikasi terpusat yang bisa bekerja untuk ketiga produk tanpa merusak alur auntentikasi yang sudah ada saat ini.


## Permasalahan

- Pengguna diharuskan log in secara terpisah untuk setiap produk.
- setiap produk menghasilkan JWT token tersendiri, yang tidak valid untuk produk lain.
- Menggantikan alur autentikasi yang sudah berjalan terlalu beresiko.
- Kami membutuhkan Single Sign-on (SSO) tetapi juga yang _backward compatible_

## Solusi

Saya merancang dan mengimplementasikan _Service_ Autentikaso di Waktoo Super App, yang menghadirkan:
- Single Login. Pengguna login sekali melalui portal Super app
- Token terpadu. Sebuah JWT yang bisa dipakai lintas aplikasi.
- _Backward Compatibility_. JWT yang sudah berjalan saat ini pada setiap produk akan tetap valid.
- Integrasi yang _seamless_ atau lancar. Tanpa ada _breaking changes_ untuk client pengguna aplikasi.

Solusi ini menjadi pondasi untuk Super App - Memadukan autentikasi tanpa mengganggu sistem autentikasi yang lama pada masing-masing produk.

## Arsitektur & Alur Autentikasi

![Diagram Alur Autentikasi](/blog/images/portfolio/waktoo-superapp-auth-sso-flow.png)

_Service_ Autentikasi Super App membuat JWT menggunakan algoritma RS256 (enkripsi asimetris). 
Setiap aplikasi akan melakukan validasi token menggunakan _public key_ Super App, sehingga proses autentikasi
dapat berjalan secara independed tanpa bergantung pada pemanggilan api pada Super app.

Berikut gambaran umum alur autentikasi tersebut:
1. **Login** - Pengguna melalukan autentikasi melalui portal Super App
2. **Pembuatan Token** - Ketika login sukses, sistem Super App akan membuat JWT yang berisi identitas dan claims dari pengguna tersebut
3. **Penggunaan Token** - Setiap client menyisipkan token pada setiap _API request_ menggunakan skema Berier
4. **Verifikasi** - Setiap aplikasi melakukan verifikasi terhadap JWT menggunakan public key serta cek validitas token tersebut
5. **Access** - Apabila token valid, maka akses ke API tersebut akan diberikan, selain itu semua _request_ ditolak. 

![Waktoo Super App Token](/blog/images/portfolio/wakto-superapp-auth-token.png)

## Kontribusi

Dengan menggabungkan autentikasi ke SSO berbasis JWT, kami menyelesaikan masalah _session_ yang terpisah dan alur login yang berantakan.

Perbaikan utama:
- Pengalaman Pengguna: login mulus di semua produk.
- Performa: validasi lebih cepat tanpa panggilan jaringan tambahan.
- Keamanan: tanda tangan lebih kuat pakai kunci privat–publik.
- Pemeliharaan: logika autentikasi tidak duplikat lagi.
- Skalabilitas: dasar kuat untuk produk Waktoo berikutnya.



