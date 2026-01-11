+++
date = '2025-06-23T20:23:24+07:00'
draft = false
menus = 'writing'
title = 'Catatan Tentang PHP: Trait, Interface, dan Class Composition'
tags = ["php", "laravel"]
+++

Hari ini saya belajar tentang beberapa fitur PHP yang tidak pernah saya pikirkan sebelumnya.  
Berkat Laravel, semuanya sudah tersedia, sehingga saya tak pernah memikirkan PHP lebih mendalam sebagai sebuah programming language.

### 1. Interface

Sama seperti di bahasa lain, interface ini umumnya digunakan sebagai _contract agreement_ tentang suatu class
yang memungkinkan memiliki berbagai implementasi. Misalnya `interface UserRepository`,
yang bisa saja diimplementasikan menggunakan MySQL maupun PostgreSQL  
Hal ini memungkinkan _method signature_ keduanya tetap sama,
sehingga "consumer" dari interface tersebut tidak perlu melakukan penyesuaian apabila implementasinya berubah.

Contoh:

```php
interface UserRepository
{
    public function set($params): string;
    public function get(): string;
}

class MysqlUserRepository implements UserRepository
{
    public function set($params): string
    {
        // implementasi dengan MySQL
    }

    public function get(): string
    {
        // implementasi
    }
}

class PgUserRepository implements UserRepository
{
    public function set($params): string
    {
        // implementasi dengan PostgreSQL
    }

    public function get(): string
    {
        // implementasi
    }
}
```

Kedua class tersebut mengimplementasikan interface yang sama, sehingga kode manapun yang menerima
`UserRepository` tetap dapat menggunakan kedua method tanpa mengetahui implementasinya.

### 2. Trait

Trait pada dasarnya adalah sekumpulan method yang dapat dicompose ke dalam berbagai class.
Biasanya digunakan untuk menyisipkan fungsi-fungsi umum ke beberapa class tanpa menggunakan inheritance.

Contoh:

```php
trait HasSlug
{
    public function generateSlug(string $title): string
    {
        return strtolower(str_replace(' ', '-', $title));
    }
}

class Blog
{
    use HasSlug; // otomatis class Blog memiliki method generateSlug()
}
```

### 3. Favor Composition Over Inheritance

Intinya, kita lebih disarankan menggunakan composition (misalnya melalui dependency injection) dibanding inheritance.
Composition lebih fleksibel, mudah di-testing, dan tidak tightly coupled.


Contoh:

```php
class User
{
    protected $name;

    public function name(): string
    {
        return $this->name;
    }
}

class Blog
{
    public function __construct(public User $user) {}

    public function name(): string
    {
        return $this->user->name();
    }
}

```

Dalam contoh di atas, kita bisa mendapatkan nilai User.name tanpa harus melakukan inheritance.
Ini akan sangat bermanfaat jika konstruktor menerima interface alih-alih class,
karena memudahkan saat ingin mengganti implementasi dependency-nya.

Dulu PHP sering dicap jelek dan berantakan—dan memang, dulu nulis PHP bisa bikin pusing sendiri.
Tapi setelah memahami fitur-fitur "bahasa modern" ini, saya jadi sadar kalau PHP juga bisa rapi dan elegan.
Laravel mungkin menyembunyikan banyak detail, tapi ngerti dasar-dasarnya bikin ngoding jadi lebih menyenangkan.
