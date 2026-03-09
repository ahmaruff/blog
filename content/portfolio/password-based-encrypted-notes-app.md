+++
date = '2026-03-09T09:45:02+07:00'
draft = false
title = 'Eksperimen Membangun Aplikasi Catatan Terenkripsi Memanfaatkan Web Crypto API'
featured_image = "/blog/images/portfolio/sikrit-pad.png"
tags = ["portfolio", "javascript", "security"]
+++

SikritPad adalah proyek eksperimen web app untuk membuat catatan terenkripsi hanya dengan satu password,
di mana password berfungsi sebagai identifier sekaligus kunci enkripsi.
Semua proses enkripsi dan dekripsi dilakukan di sisi klien
menggunakan Web Crypto API, tanpa server, dan data disimpan di LocalStorage browser.
Proyek ini menekankan UX sederhana dan modular architecture, dengan konsep unik password-addressed
storage, sekaligus mengeksplorasi trade-off antara deterministik lookup, keamanan kriptografi, dan pengalaman pengguna.

<!--more-->

Belakangan ini saya cukup sering menggunakan website catatan online untuk kebutuhan personal,
salah satunya adalah [ProtectedText.com](https://protectedtext.com) dan [CodedPad.com](https://codedpad.com).

Kedua website tersebut mengklaim data catatan disimpan secara anonim, terenkripsi, dan hanya bisa dibuka oleh pemilik password.  

Sebagai software engineer, saya penasaran bagaimana implementasinya.
Menurut saya, implementasi ProtectedText cukup straight-forward: path URL sebagai identifier, kemudian password untuk enkripsi data.  

Sementara itu, CodedPad menggunakan pendekatan yang menarik: **hanya satu input password saja**, 
password ini berfungsi sebagai **identifier sekaligus kunci akses**

Bagaimana sebuah sistem bisa mencari catatan hanya berdasarkan password tanpa menyimpan password plaintext?
Pertanyaan ini mendorong saya membuat proyek serupa, yang saya sebut **[SikritPad](https://github.com/ahmaruff/sikritpad)**.

---

## Tujuan Proyek

SikritPad adalah web app untuk membuat catatan rahasia hanya dengan password. Password berfungsi **sebagai identifier sekaligus kunci enkripsi**.  

Selain itu, saya menetapkan beberapa **constraint**:

- **Zero backend**: proyek live di Github Pages, tanpa server.
- **Client-side processing**: semua enkripsi dan dekripsi dilakukan di browser.
- **Local storage**: catatan disimpan di LocalStorage browser.
- **Password-only access**: user cukup memasukkan password.
- **Single note per password**: satu password hanya berlaku untuk satu catatan.

> ⚠️ Catatan: SikritPad adalah proyek eksperimen. Semua proses enkripsi dilakukan di sisi klien dan memiliki potensi risiko keamanan jika digunakan untuk data sensitif.

## Requirements

- **Stateless access**: tidak membutuhkan akun atau session.  
- **Deterministic note resolution**: password yang sama selalu menghasilkan note yang sama.  
- **Automatic note creation**: jika tidak ada note dengan password tertentu, sistem membuat note kosong.  
- **Local-only persistence**: data disimpan secara lokal di browser.  
- **Fully encrypted data**: semua catatan terenkripsi menggunakan AES-GCM.  
- **Reload safety**: data tidak hilang saat browser reload.


## Arsitektur Aplikasi

```
User Password
│
├── Key Derivation (PBKDF2) ──► noteId → lookup LocalStorage
│
└── Key Derivation (AES) ─────► AES key → decrypt/encrypt
```

### Modul utama:

- **State Management**: menyimpan state date sebagai single source of truth.  
- **Cryptography Module**: menangani proses enkripsi dan dekripsi data  
- **Storage Module**: wrapper untuk proses menyimpan dan mengambil data dari LocalStorage.  
- **UI Module**: bertugas untuk menangani event dan update DOM, tidak mengandung business logic.  
- **Application Controller**: menghubungkan semua modul dan mengatur alur aplikasi.


### Desain Kriptografi

- **Key Derivation**: PBKDF2 dengan SHA-256 dan iteration tinggi untuk menghasilkan key dari password.  
- **AES-GCM Encryption**: menjamin **confidentiality dan integrity**.  
- **Initialization Vector (IV)**: unik setiap kali menyimpan catatan, mencegah pattern analysis.  
- **Password sebagai identifier**: password di-derive menjadi noteId untuk lookup, sekaligus menghasilkan AES key untuk enkripsi/dekripsi.


## Trade-offs & Pertimbangan

- Untuk menghasilkan deterministic lookup, salt di PBKDF2 (proses pembuatan noteId dari input password) bersifat statis. Hal ini **membatasi fleksibilitas keamanan**, tapi membuat UX sederhana.  
- Karena noteId berasal dari password, attacker bisa melakukan offline password guessing jika sudah memiliki akses LocalStorage.  
- Project fokus pada **sederhana & eksploratif**, bukan sebagai produk security production.

## Pelajaran yang Didapat

- Password-addressed storage adalah pattern unik yang jarang ditemui, tapi menarik untuk eksplorasi.
- Arsitektur modular memudahkan pengembangan, debugging, dan testing
- Desain crypto (enkripsi, decripsi) di sisi klien memiliki **trade-off antara UX dan security**.  

---

## Penutup

SikritPad adalah eksperimen dalam membangun **password-only encrypted notes** sepenuhnya di browser.  

Proyek ini bukan untuk digunakan di production, namun memberikan **insight praktis tentang client-side cryptography**,
deterministic key derivation, dan arsitektur modular untuk aplikasi web modern.

Source code: [SikritPad GitHub](https://github.com/ahmaruff/sikritpad)


