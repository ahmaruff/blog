+++
date = '2025-09-11T01:21:25+07:00'
draft = false
title = 'Integrasi Whatsapp & AI Chatbot'
featured_image = "/blog/images/portfolio/chat-ui-superapp.png"
tags = ["portfolio", "backend", "nodejs", "javascript", "ai"]
+++

Sebagai bagian dari ekosistem Waktoo Super App, kami mengembangkan fitur layanan pelanggan
dengan menghubungkan pesan WhatsApp ke platform chatbot AI kami (**Kazee.ai**).

Backend layanan ini dikembangkan menggunakan Node.js dan Firebase,
yang memungkinkan komunikasi dua arah yang lancar dan dukungan pelanggan secara _real-time_.

<!--more-->

## Gambaran Umum

![Tampilan Chat di Waktoo Super App](/blog/images/portfolio/chat-ui-superapp.png)

Sebagai bagian dari ekosistem Waktoo Super App, kami mengembangkan fitur layanan pelanggan
dengan menghubungkan pesan WhatsApp ke platform chatbot AI kami (**Kazee.ai**).

Backend layanan ini dikembangkan menggunakan Node.js dan Firebase,
yang memungkinkan komunikasi dua arah yang lancar dan dukungan pelanggan secara _real-time_.

## Permasalahan:

- Tim CS sering kesulitan menangani pesan WhatsApp dalam jumlah besar:
- Pelanggan berharap mendapatkan balasan instan, tetapi CS tidak selalu bisa merespons dengan cepat.
- Pelanggan berharap pembaruan yang hampir _real-time_, tetapi metode _polling_ server tradisional dapat menambah latensi dan kompleksitas.

## Solusi:

Kami membangun layanan _backend_ yang mengintegrasikan WhatsApp dengan Kazee AI untuk memberikan layanan pelanggan _hybrid_:
- **Respons Awal oleh AI** - Pesan yang masuk dijawab secara instan oleh AI, mengurangi waktu tunggu dan meningkatkan tingkat respons.
- **Pengambilalihan oleh Agen CS** - Agen CS dapat mengaktifkan atau menonaktifkan mode AI kapan saja, memungkinkan mereka untuk mengambil alih percakapan secara langsung.
- **Penanganan Webhook** - Memproses pesan masuk, tanda terima pengiriman, dan pembaruan status _template_ dari **WhatsApp**.
- **API Pesan (_Messaging APIs_)** - Memungkinkan pengiriman pesan, pembuatan _template_ pesan, dan penggunaan _template_ yang sudah disetujui.
- **Penyimpanan Permanen (_Persistent Storage_)** - Setiap pesan masuk dan keluar disimpan di **Firebase** untuk audit dan riwayat.
- **Pembaruan _Real-time_** - _Frontend_ mengandalkan SDK **Firebase** untuk sinkronisasi percakapan secara instan.

## Dampak

![Tampilan Chat di Whatsapp](/blog/images/portfolio/chat-ui-wa.png)

Dengan mengintegrasikan **WhatsApp** dengan **Kazee.ai**, kami mencapai peningkatan signifikan dalam operasi dukungan pelanggan:
- **Waktu Respons Lebih Cepat** - Pelanggan menerima balasan instan berbasis AI, yang mengurangi rata-rata waktu tunggu.
- **Peningkatan Efisiensi Agen** - Agen dapat berfokus pada pertanyaan yang kompleks, sementara AI menangani pesan-pesan rutin.
- **Pengambilalihan yang Lancar** - Agen dapat mengambil alih percakapan kapan saja, memastikan pengawasan manusia saat dibutuhkan.
- **Percakapan _Real-time_** - _Frontend_ diperbarui secara instan melalui **Firebase**, memberikan pengalaman _chat_ yang mulus dan responsif.
- **Pelacakan Pesan yang Andal** - Semua pesan dan status pengiriman disimpan secara permanen untuk audit dan referensi riwayat.

Sistem ini membantu memberikan dukungan pelanggan yang lebih cepat, lebih cerdas, dan lebih andal,
meningkatkan kepuasan pengguna secara keseluruhan sekaligus efisiensi biaya operasional.
