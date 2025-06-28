+++
date = '2025-06-28T06:58:16+07:00'
draft = false
title = "Domain-Driven Design di Laravel: Mengapa Kita Butuh Modularitas? (Part 1)"
+++

Laravel merupakan _web framework_ PHP yang terkenal dengan pendekatan MVC-nya (Model-View-Controller).
Pendekatan/arsitektur MVC ini singkatnya ialah memecah sebuah sistem aplikasi menjadi tiga bagian secara horizontal.
Sistem dipecah berdasarkan fungsionalitasnya: **Model** bertugas mengelola data (operasi CRUD, accessor/mutator, dsb.),
**View** mengelola presentasi/tampilan (HTML templating, styling, interactivity, dsb.),
dan **Controller** mengontrol alur _request_ dan _response_.

Namun, bagi banyak _developer_ Laravel, pendekatan MVC ini seringkali membawa tantangan tersendiri seiring berkembangnya sebuah aplikasi.
Karena aplikasi dipecah secara horizontal, komponen-komponen didalamnya cenderung **_tightly coupled_** satu sama lain.
Kita mungkin sering menemukan **_controller_ yang terlalu gemuk (_fat controller_)** atau **_model_ yang terlalu pintar (_God Model_)**,
yang mana satu bagian kode melakukan terlalu banyak hal atau terlalu bergantung pada bagian lain.

Tentu, ini tidak menjadi masalah apabila skala aplikasi kita kecil, atau sistem kita tidak menuntut modularitas.
Akan tetapi, di iklim industri saat ini yang menuntut **_agility_** dan perubahan yang cepat,
arsitektur yang **modular** menjadi pertimbangan penting bagi tim.
Kita membutuhkan teknik agar perubahan di satu bagian aplikasi tidak merusak bagian lain,
dan itulah mengapa kita perlu memikirkan kembali bagaimana kita menentukan rancangan aplikasi, termasuk di ekosistem Laravel yang sudah _mature_ ini.


## By the Way, Memang Apa Sih Aplikasi Modular Itu?

Sederhananya, **aplikasi yang modular itu dibangun menggunakan beberapa komponen independen yang dapat kita sebut sebagai modul.**
Setiap modul dapat bekerja secara mandiri tanpa bergantung pada modul lain secara berlebihan.

Analoginya mirip **sebuah kendaraan**. Sebuah mobil terdiri dari berbagai komponen, seperti mesin, sistem kelistrikan, rem, dan bodi.
Setiap komponen ini dirancang untuk bekerja secara independen. Apabila rem blong, mesin mobil tidak serta merta menjadi rusak,
karena keduanya adalah komponen yang berbeda. Tentu, mobil tidak akan aman dikendarai, tapi kegagalan di satu bagian
bukan berarti meruntuhkan seluruh sistem.

Sama halnya dalam _software_, jika **modul pembayaran** mengalami masalah, kita tidak ingin **modul user**
ikut terpengaruh atau bahkan membuat seluruh aplikasi _crash_.

Kemudian, mengapa kita membutuhkan aplikasi yang modular? Tujuan utamanya adalah **_maintainability_** atau kemudahan dalam pemeliharaan sistem.
Kita tentu tidak ingin mengubah satu blok kode justru membuat keseluruhan sistem tidak stabil dan menghasilkan _bug_ di mana-mana.
Dengan memecah aplikasi menjadi modul-modul yang lebih kecil dan fokus pada satu tanggung jawab, harapannya kita bisa:
* Mempermudah proses **pengembangan** fitur baru.
* Membuat **_testing_** jadi lebih terisolasi.
* Mempermudah proses **_debugging_**.
* Meningkatkan **produktivitas tim** secara keseluruhan karena beberapa _developer_ bisa bekerja paralel pada modul yang berbeda tanpa banyak konflik.


## Domain Driven Design

Jadi, bagaimana cara kita mengembangkan aplikasi yang modular? Nah, di sinilah **Domain-Driven Design (DDD)** hadir.

Domain Driven Design adalah sekumpulan prinsip dalam merancang aplikasi yang menjadikan **"domain" atau "business logic"** sebagai titik pusat
atau inti dari seluruh sistem. Segala hal lain di luar inti domain tersebut dianggap sebagai **supporting system**, bukan penggerak utama.
Karena domain inilah yang "menggerakkan" (_drive_) keseluruhan sistem, maka model pengembangan ini disebut "_domain driven_,"
atau secara harfiah berarti digerakkan oleh domain.

> _Yang simple-simple aja bang jelasinnya :<_

Bayangkan kita punya aplikasi **_todo list_**. Kita bisa mencoba memecahnya menjadi beberapa komponen atau **domain**,
misalnya: **User Management**, **_Todo_**, dan **Notifikasi**.

Setiap domain ini memiliki **keahlian khusus** dan hanya fokus pada satu hal. Contohnya, domain **User Management**
adalah pihak yang _**PALING AHLI**_ dalam mengelola segala sesuatu yang berhubungan dengan pengguna, seperti _authentication_ dan _authorization_.
Oleh karena itu, hanya domain inilah yang berhak mengatur _business logic_ terkait pengguna.
Domain lain seperti _Todo_ atau Notifikasi tidak boleh "menyentuh" detail implementasi dari domain User Management.
Begitu pula sebaliknya, domain User Management juga tidak boleh "lompat pagar" dengan mengurusi domain lain.

Masing-masing domain ini akan mengelola **_core business logic_** mereka sendiri, termasuk bagaimana mereka menyimpan data,
hingga _presentation layer_ mereka (misalnya, Web API atau Service API).

Jika divisualisasikan, ini seperti kita **memotong sebuah aplikasi secara vertikal**.
Setiap "potongan vertikal" ini adalah sebuah domain yang mengelola infrastruktur hingga _presentation layer_ mereka sendiri.

Ini kontras dengan arsitektur MVC yang memotong aplikasi secara horizontal.
Dalam DDD, sistem akan dipecah secara vertikal berdasarkan domain-domain spesifik.
Setelah itu, di dalam masing-masing domain barulah kita bisa memecahnya lagi secara horizontal
sesuai dengan _layer_ fungsionalitasnya (misalnya, _application layer_, _domain layer_, _infrastructure layer_, dll.).

![Horizontal slicing vs Vertical slicing](/blog/images/horizontal-vs-vertical-slicing.svg)


## Komunikasi Antar Domain

> "Bang, bang... Kalo saya mau bikin _todo list_, kan tetap butuh informasi _user_ yang membuat _todo_ itu,
> lah terus gimana caranya komunikasi antar domain?"

Nah, pertanyaan ini mejadi **kunci keberhasilan Domain-Driven Design**.
Tujuan utama DDD adalah menghasilkan sistem yang **_Low Coupling, High Cohesion_**.
Artinya, sebuah domain seharusnya tidak terlalu bergantung (atau _tightly coupled_) terhadap detail implementasi domain lain.
Namun, ketika domain-domain itu bekerja sama, mereka harus menghasilkan perpaduan yang _✨epic✨_
sehingga seluruh sistem dapat berjalan dengan baik dan harmonis.

Seperti yang sudah disebutkan sebelumnya, setiap domain memiliki _presentation layer_-nya sendiri.
_Layer_ ini bisa berupa apa saja, misalnya HTML _form_, REST API, atau _service API_.
Ketika kita berbicara tentang domain yang berada dalam satu aplikasi yang sama,
tentu kita tidak memerlukan REST API untuk komunikasi internal, 'kan?  

Di sinilah kita perlu ingat bahwa **API bukan hanya REST API**. API adalah singkatan dari _Application Programming Interface_,
yang pada intinya adalah "antar muka" yang kita gunakan untuk berinteraksi dengan fungsionalitas di dalamnya.

Setiap domain akan **mengeluarkan (_expose_) _service API_** mereka, misal dalam bentuk sebuah _function_ dengan berbagai parameter.
Melalui _function_ atau API inilah, domain-domain lain dapat menggunakan fungsionalitas yang disediakan
tanpa perlu tahu detail implementasi di dalamnya.

Misalnya, pada domain _Todo_, kita membutuhkan informasi _user_ untuk mengambil data _todo_ mereka.
Kita cukup memanggil API yang diberikan oleh domain **User Management** untuk memperoleh _User ID_.
Bagaimana proses _User ID_ itu diperoleh (misal dari database atau _cache_), domain _Todo_ tidak perlu tahu.
Yang domain _Todo_ ketahui hanyalah: **"Untuk memperoleh _User ID_, panggil _function_ ini,"** cukup.

Kemudian, umumnya, domain **tidak akan mengonsumsi langsung API dari domain lain**.
Sebagai gantinya, setiap domain akan membuat sebuah **"adapter"** sebagai _middle man_.
Ini adalah praktik krusial untuk menjaga _low coupling_!

> Bayangkan kita pergi ke luar negeri yang memiliki jenis stop kontak berbeda.
> Tentu kita tidak mau memotong kabel perangkat kita dan mengganti kepalanya sesuai dengan stop kontak negara itu setiap kali berpindah negara.
> Kita cukup membeli **adapter** yang menyesuaikan kepala kabel kita dengan stop kontak tersebut.
> Apabila kita pergi ke negara lain lagi dengan jenis stop kontak berbeda, kita cukup mengganti _adapter_-nya saja, bukan kabelnya.

Melalui teknik _adapter_ inilah, _low coupling_ dapat benar-benar tercapai. Perubahan pada _service API_ di satu domain
hanya perlu disesuaikan pada _adapter_ di domain yang mengonsumsi API tersebut, bukan pada semua kode yang menggunakan API tersebut.
Ini membuat sistem jauh lebih tangguh dan mudah diubah.


## Praktek!!!

> _"Yapping_ doang mah gampang, kodenya mana bruhh?"

Saya akan mencoba menerapkan prinsip Domain-Driven Design ini dalam sebuah proyek, yaitu sebuah **Ticketing System** sederhana.
_Website_ ini nantinya memungkinkan _customer_ untuk mengajukan laporan/tiket, yang kemudian dapat ditindaklanjuti oleh admin.

Dari sekilas, kita sudah bisa mengidentifikasi beberapa domain yang ada didalamnya: misal domain **User Management** dan 
domain **Ticketing** itu sendiri. Namun, disamping itu, ada beberapa detail yang menarik.
Seperti bagaimana kita akan memisahkan **_admin dashboard_** dengan **_customer portal?_**,
atau bagaimana kita menangani **_user authentication_** di antara domain-domain tersebut?.

Aplikasi ini saya kembangkan _di atas_ Laravel. Saya tidak ingin melenceng terlalu jauh dari "kultur" atau ekosistem Laravel.
Oleh karena itu, saya memilih untuk tidak menggunakan _3rd party library_ 
seperti [nWidart/laravel-modules](https://github.com/nWidart/laravel-modules) yang bagi saya cukup kompleks, dan saya kesulitan untuk memahaminya. 
Demikian pula dengan _library_ [InterNACHI/modular](https://github.com/InterNACHI/modular) yang menurut saya
belum sepenuhnya menjawab kebutuhan kita dalam menghasilkan sistem _low coupling, high cohesion_ antar domain.

Artikel ini adalah **Part 1**, sebuah pengantar dasar tentang prinsip dan mengapa kita membutuhkan Domain Driven Design.
Tentu, perjalanan proyek eksperimen ini akan terus saya perbarui pada part-part selanjutnya,
di mana kita akan mulai menyelam ke dalam implementasi dan detail arsitektur.

Terima kasih sudah membaca, dan sampai jumpa di artikel berikutnya! 👋

