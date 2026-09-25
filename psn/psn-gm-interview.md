# PSN — Persiapan Interview GM

**Posisi:** Artificial Intelligence Engineer DevOps — PT Pasifik Satelit Nusantara
**Kandidat:** Thariq Ivan Anendar
**Disusun:** 25 September 2026

---

## Siapa yang akan kamu hadapi

Jabatan **General Manager of Information Technology** di PSN dipegang oleh **Suhandi**. Di bawahnya ada Yohan Setya Pradana sebagai Manager of Artificial Intelligence, Ardine Pakarti Yuniar sebagai Head of NOC System DevOps, Akbar Rizkoni sebagai Head of Sub Department Implementation and Integration, dan Arif Kurniadi sebagai VSAT MSS Network Manager.

Ada satu nama yang perlu kamu perhatikan, yaitu **Fadholi Mahfudi**, dengan jabatan **Artificial Intelligence DevOps Staff** sejak Juli 2022. Jabatannya persis sama dengan posisi yang kamu lamar. Artinya kamu bukan kandidat pertama untuk peran ini, dan pertanyaan soal keberlanjutan atau penggantian bisa muncul.

Di atas GM ada **Dani Indra W** sebagai EVP, Director, dan CTO. Dia yang membangun jaringan satelit SATRIA.

## Tahap mana ini

Dari catatan kandidat PSN sebelumnya, alur rekrutmen mereka berjalan seperti ini:

Psikotes, lalu Interview HRD dan Interview User pada hari yang sama, kemudian Interview Direksi, lalu Offering.

Psikotes kamu sudah selesai 10 September. Jadi yang berikutnya adalah Interview User bersama tim pemilik posisi, dan GM ada di lapisan ini. Kalau kamu lolos, tahap terakhir adalah Interview Direksi.

## Catatan penting soal GM

Interview dengan GM bukan tes teknis. Interview teknis kamu sudah lewat 9 September dan kamu lolos. Yang dinilai GM ada lima hal:

- Risiko. Apakah merekrut kamu aman untuk dia.
- Retensi. Apakah kamu bertahan, atau pergi enam bulan lagi.
- Komunikasi. Apakah kamu bisa bicara dengan orang non-teknis, seperti tim NOC, klien, atau direksi.
- Pertimbangan. Apakah kamu tahu kapan AI tidak seharusnya dipakai.
- Nilai. Apakah hasil kerjamu sepadan dengan biayanya.

Konsekuensinya, jawablah dalam 30 sampai 60 detik, sebut angka kalau ada, lalu berhenti dan biarkan dia bertanya lanjut. Kalau kamu menjelaskan detail teknis tanpa diminta, dia akan menilai kamu tidak bisa membaca situasi.

---

## Lima belas pertanyaan yang paling mungkin keluar

### 1. Ceritakan tentang diri kamu

- Thariq Ivan Anendar, lulusan Teknik Informatika ITS
- Bekerja di sistem AI dari ujung ke ujung, dari pengolahan dokumen sampai servicenya jalan di Docker
- Punya RAG dokumen hukum, computer vision untuk pengawasan, dan worker yang berjalan di production
- Pernah magang sebagai AI Engineer di PLN Nusantara Power, mengerjakan prediksi dari data sensor
- Sekarang mencari domain yang konsekuensinya nyata, bukan cuma di laporan

### 2. Kenapa PSN

- Datanya. Satelit dioperasikan sendiri, jadi telemetri masuk terus, dan kalau prediksinya salah akibatnya terasa di lapangan
- Pekerjaannya. Tim AI di PSN sudah berjalan pakai LangGraph dan MCP untuk monitoring jaringan
- Dua dari empat pilar kerja mereka sudah pernah saya bangun, yaitu computer vision dan RAG
- Yang belum saya pegang justru agent dan MCP, dan itu yang ingin saya pelajari

### 3. Kamu fresh graduate, kenapa harus kamu

- Yang saya kerjakan bukan proyek latihan, semuanya sudah jalan
- Saya menulis kode dan saya juga yang verifikasi, jadi kalau saya bilang jalan, itu karena sudah dicoba
- Terbiasa bekerja sampai tahap deployment dan monitoring, bukan berhenti di model
- Bisa menyebut angka hasil, bukan cuma klaim

### 4. Apa yang kamu tahu tentang PSN

- Operator satelit swasta pertama di Indonesia, berdiri 1991
- N5 resmi beroperasi 11 Mei 2026, kapasitas 160 Gbps dengan 101 spot beam Ka-band
- Punya tujuh ground station, termasuk Cikarang dan Gresik
- Kapasitasnya mulai dialokasikan ke Filipina dan Malaysia, masing-masing 20 Gbps
- Melayani lebih dari 11.000 titik, terutama di area 3T

### 5. Proyek AI yang paling kamu banggakan

- RAG untuk dokumen UU TNI
- Enam model dibandingkan, bukan memakai satu model saja
- Halusinasi turun dari di atas 60 persen menjadi sekitar 47 persen
- Jawabannya disertai sitasi pasal, jadi bisa diperiksa
- Yang saya banggakan bukan hasilnya bagus, tapi karena hasilnya diukur

### 6. Proyek RAG itu, bagian mana yang kamu pegang sendiri

- Timnya tiga orang
- Saya memegang pipeline dan evaluasinya
- Chunking, embedding, retrieval hybrid, reranking, lalu generate dengan sitasi
- Saya juga yang membangun set evaluasi dan metriknya
- Bagian deployment dan antarmuka juga saya kerjakan

### 7. Dari semua yang kamu kerjakan, mana yang paling sulit

- Bukan modelnya, tapi sekitarnya
- Di proyek UAV, yang sulit adalah kondisi lapangan, mengirim data dari pesawat secara andal, dan batas komputasi onboard
- Pelajarannya, model jarang menjadi penghambat, titik integrasi dan batasan operasional yang menghambat
- Sejak itu saya melihat sistemnya utuh, bukan cuma bagian yang ditugaskan ke saya

### 8. Pernah deploy model ke production

- Ya, tujuh service Docker
- Service API, worker, dan web saya pisahkan
- Ada Nginx, Cloudflare Tunnel, dan healthcheck
- Model embedding di-pin ke satu revisi supaya tidak berubah diam-diam
- Ada worker queue dengan retry dan dead letter untuk job yang gagal

### 9. Kalau model salah menjawab di production, kamu tahu dari mana

- Model yang mulai menjawab buruk tidak akan crash, jadi harus ada pemeriksa sendiri
- Saya memantau heartbeat, kalau berhenti berarti streamnya mati
- Ada watchdog untuk mendeteksi output yang tidak muncul
- Log JSON terstruktur, plus endpoint health dan metrics
- Untuk kualitas, saya pantau berapa persen retrieval yang mengembalikan nol dokumen
- Contoh nyata, kamera yang berhenti merekam tidak error, cuma diam

### 10. Kalau diminta membangun AI untuk monitoring jaringan satelit, mulai dari mana

- Mulai dari menentukan keputusan apa yang mau dibantu, bukan dari modelnya
- Lihat data yang sudah ada dulu, yaitu telemetri dari ground station
- Bangun baseline sederhana dan threshold dulu sebelum masuk ke model
- Deteksi anomali lebih dulu, karena nilainya jelas dan risikonya rendah
- Asisten operasional untuk NOC belakangan, setelah datanya bersih
- Batasnya, AI mengusulkan dan operator yang memutuskan

### 11. Tim meminta sesuatu yang menurutmu tidak butuh AI

- Tanya dulu masalahnya apa dan hasilnya mau dipakai untuk apa
- Kalau cukup dengan aturan atau query biasa, saya bilang cukup
- Saya tunjukkan alternatifnya beserta biayanya
- AI dipakai kalau ada pola yang sulit ditulis manual, atau datanya tidak terstruktur

### 12. Kamu tidak tahu jawaban teknis saat rapat

- Bilang belum tahu, jangan mengarang
- Sebut yang paling dekat yang sudah pernah saya kerjakan
- Sebut apa yang perlu saya cek supaya bisa menjawab
- Kalau saya mengarang, pertanyaan lanjutannya akan menemukan, dan itu merusak seluruh wawancara

### 13. Deadline dua minggu, ternyata butuh sebulan

- Kabari di awal, bukan di hari terakhir
- Pisahkan yang esensial dan yang opsional, selesaikan yang esensial dulu
- Tentukan hasil minimum yang bisa diterima untuk masing-masing item
- Sebut eksplisit apa yang tidak akan selesai dalam waktu itu

### 14. Kamu akan bertahan berapa lama

- Saya mencari kerja untuk bertahan, bukan untuk singgah
- Yang membuat saya bertahan ada dua, masalahnya harus makin sulit dan hasilnya harus dipakai
- Di PSN dua-duanya ada
- Masih banyak yang belum saya kuasai di posisi ini, jadi ruang belajarnya jelas

### 15. Ada pertanyaan untuk kami

- Pilih dua saja, jangan lebih
- Soal prioritas. Monitoring dulu atau asisten operasional untuk NOC
- Soal rencana. Apakah LangGraph dan MCP akan diperluas ke domain lain
- Atau soal harapan. Tiga bulan pertama, apa yang paling berguna kalau saya serahkan
- Jangan tanya gaji, tunjangan, WFH, atau status kontrak di tahap ini

---

## Kalau ditanya hal yang kamu tidak tahu

Polanya begini. Sebut bahwa kamu belum pernah mengerjakannya, lalu sebut yang paling dekat yang sudah pernah kamu kerjakan, lalu sebut apa yang belum kamu lakukan, lalu sebut cara kamu akan mencobanya dan siapa yang akan kamu konfirmasi.

Di depan GM, mengakui batas itu lebih kuat daripada pura-pura tahu, karena GM sudah bertahun-tahun mendengar orang mengarang.

## Tiga gap yang jangan kamu klaim

- **Agent dan function calling.** Sebut pipeline RAG dan worker queue sebagai yang terdekat. Ini gap terbesar.
- **Milvus, Weaviate, Chroma.** Sebut FAISS dan pgvector yang sudah kamu pakai.
- **vLLM dan SGLang.** Sebut Hugging Face sudah kamu pakai, karena di flyer keduanya ada dalam satu poin.

## Tiga angka yang harus kamu hafal

- LawBot, halusinasi turun dari di atas 60 persen ke sekitar 47 persen
- PLN, waktu proses turun dari satu jam ke 28 detik
- Computer vision, akurasi 94 persen dari 666 gambar

## Pertanyaan yang perlu diwaspadai

- **Kamu melamar di perusahaan lain juga?** Jangan bilang tidak, tapi jangan menyebut daftar. Bilang sedang memproses beberapa peluang dan PSN termasuk yang paling diminati, dengan alasan yang jelas.
- **Kami mungkin butuh waktu lama untuk memutuskan.** Ini bukan pertanyaan, ini cara mereka melihat reaksimu. Jawab tenang.
- **Kalau ada lamaran sebelumnya yang tidak berhasil, kenapa?** Jangan menjelekkan perusahaan itu. Bilang posisinya tidak cocok dan kamu ingin fokus ke AI.
- **Berapa ekspektasi gajimu?** Kalau ditanya di tahap ini, jawab dengan rentang dan sebut terbuka untuk didiskusikan.

## Checklist sebelum hari H

- [ ] Kirim WhatsApp ke Eko di 081317901126, karena follow-up sudah lewat jadwal
- [ ] Selesaikan psikotes Blibli sebelum 27 September 23:59 WIB
- [ ] Baca ulang empat pilar kerja tim AI PSN
- [ ] Hafal tiga angka hasil proyek
- [ ] Hafal tiga gap yang jangan diklaim
- [ ] Latihan bicara dua kali penuh, tanpa membaca
- [ ] Cek nvidia-smi dan docker compose ps di server, supaya bisa menjawab dari keadaan sebenarnya
- [ ] Kemeja warna netral, pertahankan level yang sama dengan interview sebelumnya

## Sumber dan status

- Struktur tim IT dan GM Suhandi. Sumber theorg.com, status belum terverifikasi karena data agregasi pihak ketiga
- Jabatan Fadholi Mahfudi. Sumber LinkedIn profil karyawan aktif, terverifikasi per profil
- Alur rekrutmen PSN. Sumber blog kandidat tahun 2016, berlaku sebagai gambaran pola, bukan konfirmasi resmi untuk posisi ini
- Isi flyer posisi dan gap teknis. Sumber flyer rekrutmen PSN dan pemeriksaan langsung ke seluruh repositori proyek
