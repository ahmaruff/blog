+++
date = '2026-01-10T13:00:24+07:00'
draft = false
menus = 'writing'
title = 'Mengupas "Sihir" Laravel: Apa yang Sebenarnya Ia Selesaikan?'
tags = ["php", "laravel", "framework", "konsep"]
+++

Saat memulai perjalanan di dunia *web development*, banyak dari kita langsung "melompat" menggunakan *framework*.
Saya sendiri, setelah berkenalan dengan PHP, langsung menggunakan CodeIgniter 4, dan selama lebih dari dua tahun terakhir, saya mengandalkan Laravel secara profesional.

Laravel memang sebuah alat bantu yang luar biasa untuk mempercepat pengembangan.
Namun, kemudahan ini sering kali membuat kita terlena.
Jujur saja, saya jarang berpikir mendalam tentang apa yang sebenarnya terjadi di balik layar.
Kita menerima begitu saja semua kemudahan yang diberikan oleh framework.

Untuk menjawab rasa penasaran itu, saya melakukan sebuah eksperimen sederhana: membuat dua aplikasi CRUD yang identik.

- **Satu versi Laravel.**
- **Satu versi PHP Native** (tanpa *framework* sama sekali).

Keduanya adalah aplikasi manajemen buku sederhana yang menggunakan SQLite dan menerapkan arsitektur MVC, dengan tampilan menggunakan 98.css.

Tujuannya jelas:
> Memahami konsep MVC secara lebih dalam dan melihat secara konkret apa saja masalah-masalah teknis yang sebenarnya diselesaikan oleh Laravel.

### Mengapa Eksperimen Ini Penting?

*Framework* memang menyederhanakan banyak hal. Namun, kemudahan itu sering kali menjadi pedang bermata dua.
Kita jadi lupa alur pemrosesan *request*, tidak lagi paham lifecycle aplikasi, dan membiarkan semuanya berjalan secara magis.

Dengan membangun versi *native*, saya dipaksa untuk:
- Menulis setiap komponen dari nol.
- Merasakan setiap lapisan arsitektur—mulai dari *routing* hingga *view*—secara nyata.
- Bekerja tanpa ada abstraksi yang tersembunyi.

Hasilnya, pemahaman saya tentang cara kerja aplikasi web menjadi jauh lebih jernih. Mari kita bedah satu per satu.

## 1. Routing: Dari _Query String_ ke _Resource Controller_

**PHP Native**  
Di sini, *routing* adalah pekerjaan manual. Saya menggunakan *query string* untuk menentukan halaman mana yang harus ditampilkan.
```php
// URL: ?c=book&a=show&id=3
```
Sebuah *front controller* (`public/index.php`) bertugas membaca parameter `c` (untuk *controller*) dan `a` (untuk *action*), lalu memuat *file* dan memanggil *method* yang sesuai.
Semua proses terlihat jelas, tetapi juga repetitif dan rentan kesalahan.

**Laravel**  
Cukup dengan satu baris elegan di `routes/web.php`:
```php
Route::resource('books', BookController::class);
```
Laravel secara otomatis menangani semuanya: dari mem-parsing URI, mencocokkan *route* dengan *method* HTTP (GET, POST, PUT, DELETE), hingga validasi parameter.

**Apa yang Diselesaikan Laravel?**
- Parsing URI dan ekstraksi parameter.
- Logika pencocokan *route* yang kompleks.
- Dispatching request ke *method controller* yang tepat.

## 2. Controller: Dari _Manual Load_ ke _Dependency Injection_

**PHP Native**  
Setiap *controller* harus saya `require` secara manual. Instansiasi objek juga dilakukan sendiri, lalu *method*-nya dipanggil langsung.
```php
$controller = new BookController();
$controller->show(3);
```

**Laravel**  
Berkat *autoloader* Composer dan *service container*-nya, Laravel menangani seluruh lifecycle objek.
*Dependency* seperti *request object* atau *service class* lain dapat di-*inject* secara otomatis ke dalam *method*.

**Apa yang Diselesaikan Laravel?**
- **IoC Container**: Container yang mengelola pembuatan dan penyediaan objek.
- **Object Lifecycle**: Mengatur kapan objek dibuat dan dihancurkan.
- **Dependency Resolution**: Menyuntikkan dependensi secara otomatis, membuat kode lebih bersih dan modular.

## 3. Data Layer: Dari _PDO Manual_ ke _Eloquent ORM_

**PHP Native**  
Saya harus membuat koneksi PDO, menulis *query* SQL secara manual, dan *mapping* hasilnya ke dalam *array* atau objek secara manual.
Proses ini panjang dan rawan *SQL injection* jika tidak ditangani dengan hati-hati.

**Laravel**  
Dengan Eloquent ORM, operasi database menjadi jauh lebih sederhana dan ekspresif.
```php
$books = Book::all();
$book = Book::find(1);
```
**Apa yang Diselesaikan Laravel?**
- **Query Builder & ORM**: Menyediakan abstraksi yang aman dan mudah dibaca untuk berinteraksi dengan database.
- **Connection Management**: Mengelola koneksi database secara efisien.
- **Object Hydration**: Mengubah hasil *query* menjadi objek PHP secara otomatis.

## 4. View Rendering: Dari _include()_ ke _Blade Engine_

**PHP Native**  
Menampilkan *view* dilakukan dengan `include` *file* PHP biasa. Data harus dikirim secara manual, dan logika untuk *layout templating* harus dibuat sendiri.
Proses *escaping* *output* untuk mencegah XSS juga menjadi tanggung jawab developer sepenuhnya.

**Laravel**  
Blade, *template engine* bawaan Laravel, menyediakan sintaks yang bersih dan fitur-fitur canggih seperti *layout inheritance*, *component system*, dan *automatic output escaping*.
```blade
@extends('layouts.app')

@section('content')
    <h1>{{ $book->title }}</h1>
@endsection
```
**Apa yang Diselesaikan Laravel?**
- Parsing *template* yang efisien.
- Keamanan *output* secara *default*.
- Caching *view* untuk performa lebih cepat.
- Sistem komponen yang dapat digunakan kembali (*reusable*).

## 5. Database Migration: Dari _Script PHP_ ke _Versioning_

**PHP Native**  
Perubahan skema database dikelola dengan *script* PHP atau SQL manual. Tidak ada sistem versioning, sehingga kolaborasi tim dan proses *rollback* menjadi sangat sulit.

**Laravel**  
Cukup satu perintah di terminal:
```bash
php artisan migrate
```
**Apa yang Diselesaikan Laravel?**
- **Version Control untuk Database**: Setiap perubahan skema memiliki versi dan dapat dilacak.
- **Rollback**: Kemampuan untuk kembali ke versi skema sebelumnya dengan mudah.
- **Schema Abstraction**: Menulis definisi skema dalam PHP, yang dapat dijalankan di berbagai sistem database (MySQL, PostgreSQL, dll.).

## Refleksi Akhir

Eksperimen ini membuka mata saya bahwa Laravel bukan sekadar "framework untuk kerja cepat".
Ia adalah hasil dari riset dan implementasi solusi untuk masalah-masalah *engineering* yang nyata dan kompleks.

Namun, ada satu pelajaran penting lainnya: **tanpa memahami fundamental, kita hanya akan menjadi pengguna alat, bukan seorang pengrajin yang menguasainya.**

Laravel tidak menggantikan pemahaman dasar tentang HTTP, MVC, atau OOP.
Sebaliknya, pemahaman dasar itulah yang membuat kita bisa menggunakan Laravel—dan *framework* lainnya—dengan lebih bijak, efisien, dan kreatif.

Eksperimen kecil ini telah mempertajam pemahaman saya tentang lifecycle *request*, memberi saya kepercayaan diri lebih saat melakukan *debugging*,
dan membuat saya lebih menghargai setiap keputusan desain di balik Laravel.

---

### Repository

Bagi Anda yang tertarik untuk melihat perbandingannya secara langsung, berikut adalah *repository* dari kedua aplikasi:

- **Versi Laravel**: [emvici-laravel](https://github.com/ahmaruff/emvici-laravel)
- **Versi PHP Native**: [emvici-php](https://github.com/ahmaruff/emvici-php)
