# PSN — Skenario Troubleshooting AI Engineer

**Kandidat:** Thariq Ivan Anendar

**Cara menjawab:** Jangan langsung menyalahkan model. Tentukan gejalanya, pisahkan setiap tahap, cari titik pertama yang salah, perbaiki penyebabnya, lalu verifikasi dengan kasus yang sama dan regression test.

---

# 1. Jawaban chatbot RAG salah

## Pertanyaan

> Chatbot RAG memberikan jawaban yang salah, padahal dokumen yang benar sebenarnya tersedia. Apa yang kamu periksa?

## Urutan analisis

1. **Periksa input**
   - Apakah pertanyaan user jelas?
   - Apakah ada typo, singkatan, atau istilah domain yang tidak dikenali?
   - Apakah pertanyaan membutuhkan data yang memang tersedia?

2. **Periksa ingestion**
   - Apakah dokumen yang benar sudah masuk ke sistem?
   - Apakah proses parsing berhasil?
   - Apakah isi tabel, heading, atau halaman PDF hilang?

3. **Periksa chunking**
   - Apakah informasi penting terpotong di batas chunk?
   - Apakah chunk terlalu kecil sehingga konteks tidak lengkap?
   - Apakah chunk terlalu besar sehingga satu vector memuat terlalu banyak topik?

4. **Periksa embedding dan vector store**
   - Apakah query dan dokumen menggunakan embedding model yang sama?
   - Apakah dimensinya sesuai?
   - Apakah index sudah diperbarui setelah dokumen berubah?

5. **Periksa retrieval**
   - Dokumen apa yang muncul pada top-k?
   - Apakah dokumen yang benar muncul?
   - Kalau tidak muncul, masalahnya ada sebelum generation.

6. **Periksa reranking**
   - Apakah dokumen yang benar ditemukan retriever tetapi turun setelah reranking?
   - Apakah reranker cocok dengan bahasa dan domain dokumennya?

7. **Periksa prompt dan generation**
   - Kalau konteksnya sudah benar tetapi jawaban masih salah, baru periksa prompt dan model.
   - Apakah model diperintahkan menjawab hanya dari konteks?
   - Apakah model diminta mengatakan tidak tahu ketika bukti tidak tersedia?

## Kondisi sehat yang diharapkan

- Dokumen yang benar masuk ke top-k
- Reranker menempatkan bukti utama di posisi atas
- Jawaban mengikuti konteks
- Sumber atau sitasi sesuai dengan klaim
- Kalau bukti tidak tersedia, chatbot mengatakan tidak menemukan jawabannya

## Jawaban singkat

> Saya tidak langsung mengganti model. Saya ambil satu pertanyaan yang gagal, lalu saya telusuri dari ingestion, chunking, embedding, retrieval, reranking, sampai generation. Kalau dokumen yang benar tidak masuk top-k, masalahnya ada di retrieval. Kalau konteksnya sudah benar tetapi jawabannya salah, baru saya periksa prompt dan model. Hasil yang saya harapkan bukan hanya jawaban benar, tetapi bukti yang benar muncul dan dapat diperiksa.

---

# 2. Chatbot sering menjawab “tidak ditemukan”

## Pertanyaan

> Setelah guardrail diperketat, chatbot terlalu sering menjawab bahwa informasi tidak ditemukan. Apa yang kamu periksa?

## Urutan analisis

1. Ukur persentase jawaban kosong sebelum dan sesudah perubahan.
2. Periksa apakah dokumen memang tidak tersedia atau retrieval gagal.
3. Cek threshold similarity. Threshold mungkin terlalu tinggi.
4. Cek nilai top-k. Nilainya mungkin terlalu kecil.
5. Cek apakah query perlu rewriting atau expansion.
6. Cek filter metadata, tenant, user, tanggal, dan tipe dokumen.
7. Cek prompt. Model mungkin terlalu cepat memilih untuk menolak menjawab.

## Kondisi sehat yang diharapkan

- Pertanyaan yang mempunyai bukti tetap dijawab
- Pertanyaan tanpa bukti ditolak dengan benar
- False rejection rendah
- Access-control filter tetap tidak bocor

## Jawaban singkat

> Saya pisahkan apakah masalahnya benar-benar tidak ada dokumen atau dokumennya gagal ditemukan. Saya cek threshold, top-k, query rewriting, dan metadata filter. Targetnya bukan membuat model selalu menjawab, tetapi menyeimbangkan coverage dan grounding: pertanyaan yang punya bukti dijawab, yang tidak punya bukti ditolak.

---

# 3. Retrieval memberikan dokumen yang tidak relevan

## Pertanyaan

> Semantic search sering mengambil dokumen dengan topik mirip tetapi bukan jawaban yang benar. Bagaimana memperbaikinya?

## Urutan analisis

1. Buat kumpulan query dan dokumen relevan sebagai evaluation set.
2. Ukur Recall@k dan Precision@k, jangan menilai dari satu contoh.
3. Periksa kualitas serta bahasa embedding model.
4. Evaluasi ukuran chunk dan metadata.
5. Tambahkan BM25 untuk istilah persis.
6. Gunakan hybrid retrieval.
7. Ambil kandidat lebih lebar, lalu gunakan cross-encoder reranking.
8. Tuning index hanya setelah kualitas data dan pipeline diperiksa.

## Kondisi sehat yang diharapkan

- Dokumen relevan muncul pada top-k
- Istilah persis dan kesamaan makna sama-sama tertangkap
- Reranker menempatkan dokumen terbaik di urutan atas
- Perbaikan terlihat pada evaluation set, bukan satu pertanyaan

## Jawaban singkat

> Saya ukur Recall@k dan Precision@k terlebih dahulu. Kalau vector search melewatkan istilah persis, saya tambahkan BM25 dan menggunakan hybrid retrieval. Saya ambil kandidat lebih banyak lalu rerank dengan cross-encoder. Keberhasilannya diukur pada sekumpulan query, bukan dari satu contoh yang kebetulan benar.

---

# 4. Jawaban memiliki sitasi, tetapi sitasinya tidak mendukung klaim

## Pertanyaan

> Chatbot menampilkan sumber, tetapi isi sumbernya tidak mendukung jawaban. Apa penyebabnya?

## Urutan analisis

1. Pastikan sitasi diambil dari chunk yang benar-benar diberikan ke model.
2. Simpan hubungan antara jawaban, chunk ID, document ID, dan halaman.
3. Periksa apakah model membuat sitasi sendiri sebagai teks.
4. Periksa apakah beberapa dokumen yang bertentangan tercampur.
5. Validasi setiap klaim utama terhadap konteks.
6. Jika perlu, gunakan tahap citation verification setelah generation.

## Kondisi sehat yang diharapkan

- Setiap sitasi menunjuk chunk yang benar-benar digunakan
- Halaman dan document ID dapat ditelusuri
- Klaim utama didukung kalimat pada sumber
- Model tidak boleh menciptakan nomor sumber sendiri

## Jawaban singkat

> Sitasi tidak boleh hanya berupa teks yang dibuat model. Saya pastikan sitasi berasal dari metadata chunk yang masuk ke prompt. Setelah generation, klaim utama dapat diperiksa kembali terhadap chunk tersebut. Hasil yang baik adalah setiap klaim dapat ditelusuri ke dokumen, halaman, dan potongan teks yang benar.

---

# 5. Dua dokumen memberikan informasi yang bertentangan

## Pertanyaan

> RAG menemukan SOP lama dan SOP baru dengan jawaban berbeda. Apa yang kamu lakukan?

## Urutan analisis

1. Pastikan dokumen memiliki metadata versi, tanggal berlaku, dan status aktif.
2. Filter dokumen yang sudah tidak berlaku saat retrieval.
3. Jika keduanya tetap relevan, tampilkan perbedaannya secara eksplisit.
4. Jangan meminta LLM menebak mana yang benar.
5. Definisikan source priority dari pemilik proses.
6. Tambahkan validasi saat dokumen baru masuk.

## Kondisi sehat yang diharapkan

- Dokumen aktif diprioritaskan
- Dokumen lama tidak diam-diam menjadi sumber jawaban
- Konflik yang belum terselesaikan disampaikan kepada user
- Ada jejak versi dan tanggal berlaku

## Jawaban singkat

> Ini tidak saya selesaikan dengan prompt saja. Dokumen harus memiliki metadata versi dan status berlaku. Retrieval memprioritaskan dokumen aktif. Kalau konflik belum dapat ditentukan, sistem menyampaikan kedua sumber dan meminta konfirmasi, bukan memilih sendiri.

---

# 6. RAG menjadi lambat setelah jumlah dokumen bertambah

## Pertanyaan

> Latency chatbot naik dari dua detik menjadi lima belas detik setelah jumlah dokumen meningkat. Bagaimana menganalisisnya?

## Urutan analisis

1. Pecah latency per tahap: query embedding, vector search, BM25, reranking, dan generation.
2. Periksa p50, p95, dan p99, bukan hanya rata-rata.
3. Periksa ukuran index dan jenis index yang digunakan.
4. Periksa nilai top-k sebelum reranking.
5. Periksa jumlah token konteks dan output.
6. Periksa queue, concurrent request, CPU, RAM, VRAM, dan network.
7. Optimalkan bagian yang memang menjadi bottleneck.

## Kondisi sehat yang diharapkan

- Bottleneck dapat ditunjuk dengan metrik
- Retrieval tetap cepat saat corpus tumbuh
- Context tidak membawa terlalu banyak dokumen
- p95 sesuai target layanan
- Kualitas retrieval tidak turun setelah optimasi

## Jawaban singkat

> Saya ukur latency tiap tahap terlebih dahulu. Kalau lambatnya di retrieval, saya periksa index dan kandidat yang diambil. Kalau di reranking, saya kurangi kandidat atau gunakan model yang lebih ringan. Kalau di generation, saya cek panjang konteks, output, queue, dan utilisasi GPU. Targetnya menurunkan p95 tanpa mengorbankan kualitas jawaban.

---

# 7. Setelah mengganti embedding model, hasil retrieval rusak

## Pertanyaan

> Embedding model diganti, tetapi hasil pencarian menjadi buruk atau insert gagal. Mengapa?

## Urutan analisis

1. Periksa dimensi model lama dan baru.
2. Pastikan query dan seluruh dokumen menggunakan model yang sama.
3. Jangan mencampur vector space lama dan baru.
4. Bangun index baru sebagai candidate.
5. Backfill seluruh dokumen.
6. Jalankan evaluation set pada index baru.
7. Aktifkan secara atomik hanya setelah lulus.
8. Pertahankan index lama untuk rollback.

## Kondisi sehat yang diharapkan

- Semua vector memakai model dan dimensi yang sama
- Tidak ada index campuran
- Candidate index tervalidasi sebelum cutover
- Rollback tersedia

## Jawaban singkat

> Embedding dari model berbeda berada pada vector space berbeda, walaupun dimensinya sama. Jadi saya tidak mengubahnya secara bertahap dalam index aktif. Saya bangun candidate index, backfill semua dokumen, evaluasi, lalu melakukan atomic cutover. Index lama tetap tersedia untuk rollback.

---

# 8. Data confidential muncul pada jawaban user yang salah

## Pertanyaan

> User dapat melihat potongan dokumen yang bukan miliknya. Apa yang kamu lakukan?

## Tindakan awal

1. Anggap sebagai security incident.
2. Nonaktifkan endpoint atau retrieval yang terdampak bila perlu.
3. Simpan audit log dan tentukan ruang lingkup kebocoran.
4. Jangan hanya memperbaiki prompt.

## Analisis akar masalah

- Apakah tenant dan owner filter diterapkan pada query database?
- Apakah filtering hanya dilakukan setelah retrieval?
- Apakah cache digunakan lintas user?
- Apakah vector store mencampur namespace?
- Apakah prompt, output, atau context tersimpan pada log?

## Kondisi sehat yang diharapkan

- Access control ditegakkan sebelum retrieval atau di database
- Setiap tenant mempunyai filter atau namespace yang jelas
- Cache key memasukkan identitas tenant
- Log tidak menyimpan context confidential secara utuh
- Ada negative test lintas user

## Jawaban singkat

> Ini saya perlakukan sebagai insiden keamanan, bukan sekadar masalah kualitas. Saya hentikan jalur yang bocor, periksa audit log, lalu cari apakah filter tenant diterapkan di database sebelum retrieval. Hasil yang benar adalah dokumen user lain tidak pernah masuk ke kandidat retrieval, bukan hanya disembunyikan setelah generation.

---

# 9. Agent memilih tool yang salah

## Pertanyaan

> Agent diminta melihat histori telemetri, tetapi malah memanggil tool lain. Apa yang kamu periksa?

## Urutan analisis

1. Lihat tool list yang diterima model.
2. Periksa nama, deskripsi, parameter, dan contoh masing-masing tool.
3. Cari tools yang fungsi dan deskripsinya tumpang tindih.
4. Periksa prompt dan konteks percakapan.
5. Tambahkan tracing keputusan tool.
6. Batasi tools yang tersedia sesuai konteks.
7. Buat evaluation set untuk tool selection.

## Kondisi sehat yang diharapkan

- Tool yang benar dipilih secara konsisten
- Parameter sesuai schema
- Tool berisiko membutuhkan approval
- Kesalahan tool dapat ditelusuri dari log

## Jawaban singkat

> Saya periksa kontrak tool sebelum menyalahkan model. Nama, deskripsi, dan schema yang tumpang tindih membuat model sulit memilih. Saya batasi tool berdasarkan konteks, perjelas deskripsi, tambahkan tracing, dan menguji tool selection dengan kumpulan pertanyaan tetap.

---

# 10. Agent masuk loop dan terus memanggil tool

## Pertanyaan

> Agent terus memanggil tool yang sama tanpa menyelesaikan tugas. Bagaimana mengatasinya?

## Urutan analisis

1. Periksa observation yang dikembalikan tool.
2. Pastikan error dan empty result dibedakan.
3. Simpan jumlah percobaan dalam state.
4. Tambahkan max steps, timeout, dan token budget.
5. Deteksi tool call dengan argumen yang sama.
6. Tentukan kondisi berhenti dan fallback ke manusia.
7. Tambahkan circuit breaker untuk tool yang terus gagal.

## Kondisi sehat yang diharapkan

- Agent berhenti pada batas yang jelas
- Tidak mengulang call identik tanpa perubahan
- Error tool dilaporkan, bukan disamarkan
- Ada fallback atau human escalation

## Jawaban singkat

> Agent tidak boleh dibiarkan berhenti berdasarkan keputusan model saja. Saya pasang max steps, timeout, token budget, dan deteksi call identik. Kalau tool terus gagal, circuit breaker dibuka dan tugas dialihkan ke fallback atau operator.

---

# 11. MCP tool tidak muncul atau gagal dipanggil

## Pertanyaan

> MCP server aktif, tetapi tool tidak terlihat oleh agent atau selalu gagal. Apa yang dicek?

## Urutan analisis

1. Periksa apakah client berhasil connect dan menjalankan tool discovery.
2. Periksa transport, command, URL, dan timeout.
3. Periksa schema tool dan compatibility SDK.
4. Untuk stdio, pastikan stdout tidak tercampur log biasa.
5. Periksa authentication dan permission.
6. Panggil tool secara langsung tanpa agent.
7. Periksa bentuk structured response.

## Kondisi sehat yang diharapkan

- Server terhubung dan tool terdaftar
- Schema dapat dibaca client
- Direct tool call berhasil
- Error terstruktur dan credential tidak bocor
- Reconnection bekerja ketika koneksi terputus

## Jawaban singkat

> Saya pisahkan masalah MCP dari keputusan agent. Pertama saya pastikan discovery berhasil, lalu saya panggil tool secara langsung. Kalau transport-nya stdio, saya pastikan log tidak ditulis ke stdout karena dapat merusak JSON-RPC. Setelah direct call sehat, baru saya uji pemilihan tool oleh model.

---

# 12. Model serving mengalami Out of Memory

## Pertanyaan

> Service LLM atau embedding mengalami CUDA out of memory saat traffic naik. Apa yang kamu lakukan?

## Tindakan awal

1. Hentikan retry tanpa batas.
2. Kurangi concurrency atau hentikan penerimaan request baru sementara.
3. Catat batch size, token length, model, dan penggunaan VRAM saat gagal.

## Analisis

- Apakah input terlalu panjang?
- Apakah batch token terlalu besar?
- Apakah concurrent request bertambah?
- Apakah ada memory leak atau model dimuat lebih dari sekali?
- Apakah precision dan quantization sesuai?
- Apakah workload perlu queue atau model lebih kecil?

## Kondisi sehat yang diharapkan

- Memory usage stabil
- Request berlebih mengantre, bukan membuat service crash
- Ada batas input dan batch token
- p95 latency dan throughput terukur
- Tidak ada restart loop

## Jawaban singkat

> Saya cek apakah OOM dipicu panjang input, batch size, atau concurrency. Tindakan cepatnya menurunkan concurrency dan membatasi token. Setelah itu saya periksa apakah model termuat ganda, gunakan quantization atau model lebih kecil bila perlu, lalu uji load kembali. Sistem yang sehat mengantrekan request berlebih, bukan crash.

---

# 13. Service model hidup, tetapi latency tiba-tiba naik

## Pertanyaan

> Healthcheck masih hijau, tetapi pengguna merasa inference jauh lebih lambat. Apa yang diperiksa?

## Urutan analisis

1. Bandingkan p50, p95, dan p99 sebelum serta sesudah kejadian.
2. Pisahkan queue time dari execution time.
3. Periksa jumlah concurrent request.
4. Periksa CPU, RAM, VRAM, GPU utilization, dan temperature.
5. Periksa panjang input dan output.
6. Periksa network serta dependency eksternal.
7. Periksa apakah model atau image berubah.

## Kondisi sehat yang diharapkan

- Queue time dan inference time terlihat terpisah
- Tidak ada resource saturation
- Versi model konsisten
- Alert berbasis latency, bukan hanya uptime

## Jawaban singkat

> Healthcheck hanya membuktikan process hidup. Saya pisahkan waktu antre dan waktu inference, lalu cek concurrency, panjang token, resource, network, dan perubahan versi. Sistem sehat harus mempunyai latency metric dan alert, bukan hanya endpoint health.

---

# 14. Provider LLM eksternal gagal atau rate limited

## Pertanyaan

> Provider membalas 429 atau timeout. Bagaimana sistem tetap aman?

## Urutan analisis

1. Klasifikasikan error retryable dan non-retryable.
2. Hormati `Retry-After`.
3. Gunakan exponential backoff dengan jitter.
4. Batasi jumlah retry.
5. Simpan job ke queue atau dead-letter queue.
6. Gunakan idempotency key untuk operasi yang dapat terduplikasi.
7. Pertimbangkan fallback provider atau model lokal.

## Kondisi sehat yang diharapkan

- Tidak terjadi retry storm
- Job tidak hilang atau diproses dua kali
- Provider yang bermasalah tidak menjatuhkan seluruh aplikasi
- Error dapat dipulihkan atau ditinjau manual

## Jawaban singkat

> Untuk 429 saya menghormati `Retry-After`, lalu menggunakan backoff dan jitter dengan batas retry. Kalau tetap gagal, job masuk dead-letter queue, bukan dihapus. Untuk operasi yang bisa diulang saya gunakan idempotency. Kalau kebutuhan bisnis mengizinkan, ada fallback provider atau model lokal.

---

# 15. Versi model baru lebih buruk daripada versi lama

## Pertanyaan

> Model baru lebih bagus pada benchmark offline, tetapi pengguna mengeluh hasil production menurun. Apa yang dilakukan?

## Urutan analisis

1. Jangan langsung menggantikan versi lama sepenuhnya.
2. Bandingkan distribution data offline dan production.
3. Periksa metrik per kelompok kasus, bukan hanya aggregate score.
4. Jalankan shadow test atau canary.
5. Bandingkan kualitas, latency, error rate, dan biaya.
6. Kumpulkan contoh kegagalan pengguna.
7. Rollback jika acceptance criteria tidak terpenuhi.

## Kondisi sehat yang diharapkan

- Deployment dapat di-rollback
- Model baru diuji pada traffic nyata secara terbatas
- Keputusan memakai beberapa metrik
- Tidak hanya bergantung pada satu benchmark

## Jawaban singkat

> Benchmark offline tidak selalu mewakili production. Saya gunakan canary atau shadow test, lalu membandingkan kualitas, latency, error, dan biaya. Saya juga melihat metrik per kategori use case. Kalau acceptance criteria gagal, saya rollback sambil menganalisis distribution shift.

---

# 16. Alert anomaly telemetri terlalu banyak

## Pertanyaan

> Sistem anomaly detection mengirim terlalu banyak alert sampai operator mulai mengabaikannya. Apa yang kamu lakukan?

## Urutan analisis

1. Ukur precision alert dan jumlah alert per periode.
2. Periksa threshold per kanal, bukan satu threshold global.
3. Perhitungkan seasonality dan operating mode.
4. Gunakan persistence rule, misalnya anomali harus bertahan beberapa interval.
5. Gabungkan signal yang berkaitan.
6. Kelompokkan alert berulang menjadi satu incident.
7. Minta feedback operator untuk true dan false alert.

## Kondisi sehat yang diharapkan

- Alert actionable
- False positive terkendali
- Severity jelas
- Satu kejadian tidak menghasilkan puluhan notifikasi
- Operator tetap percaya pada sistem

## Jawaban singkat

> Targetnya bukan mendeteksi semua penyimpangan, tetapi memberikan alert yang dapat ditindaklanjuti. Saya ukur precision, sesuaikan threshold per kanal, memperhitungkan seasonality, dan menggunakan persistence rule. Alert berulang digabung menjadi satu incident, lalu feedback operator digunakan untuk memperbaiki threshold.

---

# 17. Data telemetri berhenti masuk

## Pertanyaan

> Dashboard tidak menunjukkan anomali, tetapi ternyata data satu ground station berhenti masuk. Mengapa model gagal mendeteksi?

## Analisis

Model anomaly detection biasanya memeriksa nilai yang masuk. Kalau tidak ada data, model tidak mempunyai nilai untuk dinilai. Ini adalah masalah data availability, bukan anomaly pada nilai.

## Yang diperiksa

1. Last-seen timestamp per source.
2. Expected reporting interval.
3. Heartbeat device dan pipeline.
4. Queue lag dan consumer status.
5. Network, authentication, schema, dan storage error.
6. Apakah data terlambat atau benar-benar hilang.

## Kondisi sehat yang diharapkan

- Missing data mempunyai alert terpisah
- Setiap source memiliki freshness SLA
- Dashboard membedakan nilai normal dari data yang tidak tersedia
- Recovery dan backfill dapat dilakukan

## Jawaban singkat

> Tidak adanya anomali belum berarti sistem sehat. Kalau datanya berhenti masuk, model tidak punya nilai untuk dianalisis. Karena itu saya pisahkan data-quality monitoring dari model monitoring, menggunakan last-seen timestamp, heartbeat, queue lag, dan freshness SLA.

---

# 18. Computer vision menghasilkan terlalu banyak false positive

## Pertanyaan

> Detektor CCTV sering menganggap objek lain sebagai target. Bagaimana menganalisisnya?

## Urutan analisis

1. Kumpulkan false positive dan kelompokkan berdasarkan kondisi.
2. Periksa label dan class imbalance.
3. Periksa lighting, angle, blur, resolution, dan background.
4. Bandingkan confidence threshold serta IoU threshold.
5. Tambahkan hard-negative examples ke dataset.
6. Evaluasi per lokasi dan kondisi, bukan accuracy aggregate saja.
7. Jika video, gunakan temporal consistency atau tracking.

## Kondisi sehat yang diharapkan

- Precision naik tanpa recall jatuh terlalu jauh
- Dataset mencakup kondisi production
- Threshold ditentukan dari validation set
- Deteksi tidak berubah liar antarfame

## Jawaban singkat

> Saya tidak langsung menaikkan confidence threshold karena recall bisa jatuh. Saya kelompokkan false positive, memeriksa kualitas label dan kondisi visual, lalu menambahkan hard-negative examples. Untuk video, saya gunakan temporal consistency agar satu frame salah tidak langsung menjadi event.

---

# 19. Fine-tuning meningkatkan ROUGE tetapi halusinasi tetap tinggi

## Pertanyaan

> Setelah fine-tuning, ROUGE naik tetapi jawaban masih banyak mengarang. Mengapa?

## Analisis

Fine-tuning dapat memperbaiki gaya, format, dan pola jawaban, tetapi tidak menjamin model mempunyai fakta terbaru atau selalu menggunakan sumber yang benar.

## Langkah perbaikan

1. Pisahkan evaluasi similarity dan factuality.
2. Periksa kualitas dataset fine-tuning.
3. Tambahkan RAG untuk fakta yang berubah atau spesifik.
4. Gunakan grounding prompt dan source citation.
5. Evaluasi hallucination serta factual accuracy secara terpisah.
6. Tambahkan refusal ketika bukti tidak tersedia.

## Kondisi sehat yang diharapkan

- Jawaban bukan hanya mirip referensi, tetapi faktual
- Klaim dapat ditelusuri ke sumber
- Model menolak ketika bukti tidak ada
- Metrik tidak bergantung pada ROUGE saja

## Jawaban singkat

> ROUGE mengukur kemiripan teks, bukan kebenaran fakta. Fine-tuning dapat membuat gaya jawaban lebih sesuai tetapi tetap mengarang. Saya pisahkan factual evaluation, menambahkan RAG dan citation, lalu mengukur hallucination serta refusal ketika bukti tidak tersedia.

---

# 20. Hasil evaluasi offline bagus, tetapi sistem tidak dipakai

## Pertanyaan

> Akurasi model bagus, tetapi user tetap memakai proses manual. Apa yang salah?

## Urutan analisis

1. Tanyakan keputusan apa yang sebenarnya perlu dibantu.
2. Periksa apakah output masuk ke workflow user.
3. Ukur waktu, langkah, dan pekerjaan manual yang tersisa.
4. Periksa apakah hasil mudah dipahami dan dipercaya.
5. Periksa latency dan availability.
6. Minta feedback langsung dari operator.
7. Ubah metrik sukses dari model score menjadi outcome.

## Kondisi sehat yang diharapkan

- Waktu atau langkah kerja berkurang
- Output muncul pada sistem yang sudah digunakan
- User memahami alasan rekomendasi
- Ada ownership ketika model salah
- Penggunaan dapat diukur

## Jawaban singkat

> Model yang akurat belum tentu berguna. Saya cek apakah output masuk ke workflow pengguna, apakah waktunya cukup cepat, dan apakah hasilnya dapat dipercaya. Selain model metric, saya ukur outcome seperti waktu yang dihemat, jumlah langkah yang berkurang, dan tingkat penggunaan.

---

# Pola Jawaban Umum

Kalau mendapat masalah yang belum pernah ditemui, gunakan alur berikut:

1. **Definisikan gejala** dengan contoh dan metrik.
2. **Reproduksi** menggunakan input yang gagal.
3. **Pisahkan pipeline** menjadi beberapa tahap.
4. Temukan **titik pertama yang salah**.
5. Perbaiki **akar masalah**, bukan hanya gejalanya.
6. Verifikasi pada kasus gagal yang sama.
7. Jalankan **regression test** pada kasus lain.
8. Tambahkan monitoring agar masalah terdeteksi lebih awal.

## Jawaban universal

> Saya mulai dengan satu contoh yang gagal dan mengubahnya menjadi kasus yang dapat direproduksi. Setelah itu saya pecah sistem per tahap dan mencari titik pertama yang hasilnya sudah salah. Saya memperbaiki bagian tersebut, menguji kembali kasus awal, lalu menjalankan regression test agar perbaikannya tidak merusak kasus lain. Terakhir, saya tambahkan metrik atau alert supaya masalah yang sama dapat ditemukan lebih cepat.
