# PSN — Problem Scenarios Sesuai Job Description AI Engineer DevOps

**Kandidat:** Thariq Ivan Anendar

Dokumen ini mengikuti ruang lingkup posisi PSN: LLM-powered application, RAG, AI agent, tool calling dan MCP, model serving, Hugging Face dan vLLM, vector database, Python API, Linux, Docker, monitoring, performance, dan security.

## Pola jawaban utama

Untuk setiap masalah, gunakan urutan berikut:

1. Tentukan gejala dan dampaknya.
2. Ambil satu contoh yang gagal dan reproduksi.
3. Pecah sistem menjadi beberapa komponen.
4. Cari titik pertama yang menghasilkan output salah.
5. Perbaiki akar masalah.
6. Verifikasi dengan kasus gagal dan regression test.
7. Tambahkan monitoring agar masalah yang sama terdeteksi lebih awal.

---

# A. LLM Application dan RAG

## 1. Chatbot memberikan jawaban salah padahal dokumennya tersedia

### Yang dianalisis

1. Pastikan dokumen sudah berhasil di-ingest dan diparsing.
2. Periksa apakah informasi terpotong akibat chunking.
3. Lihat hasil retrieval untuk pertanyaan yang gagal.
4. Pastikan query dan dokumen memakai embedding model serta versi yang sama.
5. Periksa metadata filter, top-k, similarity threshold, dan reranking.
6. Jika konteks sudah benar, baru periksa prompt dan LLM.

### Harapan hasil yang baik

- Dokumen yang benar muncul pada top-k.
- Bukti utama berada di posisi atas setelah reranking.
- Jawaban mengikuti konteks dan menyertakan sumber yang tepat.
- Model menolak menjawab jika bukti tidak tersedia.

### Jawaban interview

> Saya tidak langsung menyalahkan LLM. Saya ambil satu pertanyaan yang gagal, lalu memeriksa ingestion, chunking, embedding, retrieval, reranking, dan generation. Kalau dokumen yang benar tidak masuk top-k, masalahnya ada sebelum LLM. Kalau konteks sudah benar tetapi jawabannya masih salah, baru saya memeriksa prompt dan model. Hasil yang saya harapkan adalah jawaban yang benar serta bukti yang dapat ditelusuri.

---

## 2. Sistem menampilkan dokumen confidential kepada user yang salah

### Yang dianalisis

1. Perlakukan sebagai security incident.
2. Tentukan user, dokumen, dan periode yang terdampak melalui audit log.
3. Periksa apakah tenant filter dilakukan sebelum retrieval.
4. Periksa vector namespace, metadata filter, dan cache key.
5. Periksa apakah context atau dokumen sensitif tersimpan di log.
6. Tambahkan negative test antartenant.

### Harapan hasil yang baik

- Dokumen yang tidak berizin tidak pernah masuk kandidat retrieval.
- Access control diterapkan pada database atau vector query.
- Cache terisolasi per tenant.
- Semua akses dapat diaudit.

### Jawaban interview

> Ini bukan sekadar masalah jawaban chatbot, tetapi insiden keamanan. Saya hentikan jalur yang bocor, menentukan ruang lingkup melalui audit log, lalu memastikan access control diterapkan sebelum retrieval. Hasil yang benar adalah dokumen user lain tidak pernah masuk ke kandidat, bukan hanya disembunyikan setelah jawaban dibuat.

---

## 3. RAG menjadi lambat ketika jumlah dokumen meningkat

### Yang dianalisis

1. Ukur latency query embedding, vector search, reranking, dan generation secara terpisah.
2. Bandingkan p50, p95, dan p99.
3. Periksa jenis serta konfigurasi vector index.
4. Periksa jumlah kandidat sebelum reranking dan jumlah context token.
5. Periksa CPU, RAM, GPU, network, dan queue time.

### Harapan hasil yang baik

- Bottleneck dapat dibuktikan melalui metrik.
- p95 sesuai service-level objective.
- Optimasi tidak menurunkan retrieval quality.

### Jawaban interview

> Saya memecah latency per tahap agar tidak melakukan optimasi secara acak. Jika bottleneck ada di vector search, saya memeriksa index. Jika ada di reranker, saya mengurangi kandidat atau memakai model lebih ringan. Jika ada di generation, saya memeriksa panjang context, queue, dan utilisasi GPU. Setelah itu saya menguji kembali latency dan kualitasnya.

---

# B. AI Agent, Tool Calling, dan MCP

## 4. Agent memilih tool yang salah

### Yang dianalisis

1. Lihat tools yang tersedia ketika keputusan dibuat.
2. Periksa nama, deskripsi, parameter, dan contoh penggunaan tool.
3. Cari tools yang mempunyai fungsi tumpang tindih.
4. Periksa konteks dan instruction yang diberikan kepada agent.
5. Tambahkan trace untuk tool selection.
6. Buat test set berisi perintah dan expected tool.

### Harapan hasil yang baik

- Tool yang tepat dipilih secara konsisten.
- Argument sesuai schema.
- Tool yang tidak relevan tidak ditawarkan.
- Keputusan dapat ditelusuri.

### Jawaban interview

> Saya memeriksa kontrak tool terlebih dahulu karena nama dan deskripsi yang tumpang tindih dapat membuat model salah memilih. Saya membatasi tools sesuai konteks, memperjelas schema dan contoh, lalu menguji tool selection menggunakan test set tetap. Jadi perbaikannya terukur, bukan hanya mengganti prompt.

---

## 5. Agent terus memanggil tool yang sama dan tidak selesai

### Yang dianalisis

1. Periksa observation yang dikembalikan tool.
2. Bedakan empty result, error retryable, dan error permanen.
3. Deteksi call berulang dengan argument identik.
4. Tambahkan max steps, timeout, token budget, dan retry limit.
5. Gunakan circuit breaker untuk tool yang gagal berulang.
6. Sediakan fallback atau human escalation.

### Harapan hasil yang baik

- Agent berhenti pada batas yang jelas.
- Tidak ada infinite loop.
- Kegagalan dijelaskan kepada user.
- Operasi berisiko tidak dilakukan berulang.

### Jawaban interview

> Saya tidak menyerahkan kondisi berhenti sepenuhnya kepada model. Saya menggunakan max steps, timeout, token budget, dan mendeteksi tool call identik. Kalau tool terus gagal, circuit breaker menghentikan pemanggilan dan tugas dialihkan ke fallback atau manusia.

---

## 6. Agent melakukan tindakan berisiko tanpa persetujuan

### Yang dianalisis

1. Kelompokkan tools menjadi read-only dan state-changing.
2. Tentukan tindakan yang membutuhkan approval.
3. Periksa authorization user, bukan hanya kemampuan agent.
4. Gunakan idempotency untuk operasi yang dapat diulang.
5. Simpan audit log input, approval, tool, dan hasil.

### Harapan hasil yang baik

- Agent boleh membaca data sesuai izin.
- Tindakan seperti menghapus, mengubah konfigurasi, atau mengirim pesan memerlukan persetujuan.
- Semua tindakan mempunyai jejak audit.

### Jawaban interview

> Saya memisahkan tool berdasarkan tingkat risiko. Read-only tool dapat dijalankan sesuai izin, sedangkan tindakan yang mengubah sistem memerlukan approval eksplisit. Saya juga menggunakan idempotency dan audit log supaya retry tidak menghasilkan tindakan ganda dan setiap perubahan dapat ditelusuri.

---

## 7. MCP server aktif tetapi tool tidak muncul

### Yang dianalisis

1. Pastikan MCP client berhasil connect.
2. Periksa proses tool discovery.
3. Periksa transport stdio, HTTP, atau SSE yang digunakan.
4. Periksa schema dan compatibility SDK.
5. Untuk stdio, pastikan stdout tidak tercampur log biasa.
6. Jalankan tool secara langsung tanpa agent.
7. Periksa authentication, permission, timeout, dan structured response.

### Harapan hasil yang baik

- Tool dapat ditemukan dan schema terbaca.
- Direct tool call berhasil.
- Error terstruktur.
- Reconnection bekerja.

### Jawaban interview

> Saya memisahkan masalah MCP dari keputusan agent. Pertama saya memastikan connection dan discovery berhasil, kemudian menjalankan direct tool call. Kalau menggunakan stdio, log tidak boleh ditulis ke stdout karena dapat merusak JSON-RPC. Setelah MCP sehat, baru saya menguji apakah agent memilih tool yang benar.

---

## 8. Hasil tool sangat besar dan membuat agent kehilangan fokus

### Yang dianalisis

1. Ukur ukuran output dan token yang masuk kembali ke model.
2. Periksa apakah tool mengembalikan seluruh data tanpa filter.
3. Tambahkan pagination, field selection, dan batas hasil.
4. Ringkas hasil secara deterministik sebelum diberikan kepada LLM.
5. Simpan data besar sebagai artifact dan berikan reference.

### Harapan hasil yang baik

- Model hanya menerima data yang relevan.
- Context tidak habis oleh raw response.
- Data lengkap tetap dapat ditelusuri melalui artifact.

### Jawaban interview

> Tool tidak seharusnya mengirim seluruh raw data ke model. Saya menambahkan pagination dan filter, lalu melakukan reduksi secara deterministik. Data lengkap disimpan sebagai artifact, sedangkan model menerima ringkasan dan reference. Ini mengurangi biaya serta risiko informasi penting tenggelam dalam context.

---

# C. Model Serving dan Inference Optimization

## 9. vLLM atau model server mengalami CUDA out of memory

### Yang dianalisis

1. Catat panjang input, output, batch token, dan concurrency saat gagal.
2. Periksa apakah model dimuat lebih dari sekali.
3. Periksa penggunaan VRAM dan fragmentasi memory.
4. Turunkan concurrency, batch token, atau context limit.
5. Pertimbangkan quantization atau model yang lebih kecil.
6. Uji kembali dengan load test.

### Harapan hasil yang baik

- Penggunaan VRAM stabil.
- Request berlebih mengantre, bukan menjatuhkan service.
- Batas input dan concurrency jelas.
- Tidak terjadi restart loop.

### Jawaban interview

> Saya menentukan apakah OOM dipicu oleh context length, batch size, atau concurrency. Tindakan awalnya adalah membatasi request agar service stabil. Setelah itu saya memeriksa model ganda, batching, dan penggunaan VRAM. Jika perlu saya memakai quantization atau model lebih kecil, lalu memverifikasinya dengan load test.

---

## 10. Throughput tinggi tetapi latency satu user buruk

### Yang dianalisis

1. Bedakan throughput dan latency.
2. Pisahkan queue time, time to first token, dan total generation time.
3. Periksa batching policy.
4. Periksa request pendek yang tertahan request panjang.
5. Terapkan batas token, scheduling, atau queue terpisah jika diperlukan.

### Harapan hasil yang baik

- Throughput dan p95 latency sama-sama berada dalam target.
- Request pendek tidak selalu tertahan request panjang.
- Time to first token terukur.

### Jawaban interview

> Throughput tinggi tidak otomatis berarti pengalaman user baik. Saya melihat queue time, time to first token, dan total generation time secara terpisah. Batching yang terlalu agresif dapat menaikkan throughput tetapi memperburuk latency. Saya menyesuaikan batch policy dan batas token berdasarkan target layanan.

---

## 11. Model baru bagus di benchmark tetapi buruk di production

### Yang dianalisis

1. Bandingkan distribution data evaluation dan production.
2. Periksa metrik per kategori use case.
3. Jalankan shadow test atau canary deployment.
4. Bandingkan kualitas, latency, error rate, dan biaya.
5. Kumpulkan contoh kegagalan dari pengguna.
6. Rollback jika acceptance criteria tidak terpenuhi.

### Harapan hasil yang baik

- Model baru hanya menerima sebagian traffic saat validasi.
- Deployment dapat di-rollback.
- Keputusan tidak berdasarkan satu benchmark.

### Jawaban interview

> Benchmark offline belum tentu mewakili traffic nyata. Saya menggunakan shadow test atau canary, lalu membandingkan kualitas, latency, error, dan biaya. Saya juga melihat hasil per kategori. Kalau acceptance criteria gagal, model di-rollback sambil dilakukan analisis distribution shift.

---

## 12. Hugging Face model tidak dapat dimuat di server

### Yang dianalisis

1. Periksa model ID dan revision yang digunakan.
2. Pastikan file model sudah lengkap dan formatnya kompatibel.
3. Periksa versi `transformers`, tokenizer, CUDA, dan runtime.
4. Periksa permission atau gated-model token.
5. Periksa disk, RAM, dan VRAM.
6. Pin revision atau image digest yang sudah tervalidasi.

### Harapan hasil yang baik

- Versi model dan dependency reproducible.
- Tidak diam-diam memakai revision terbaru.
- Startup check mendeteksi incompatibility sebelum traffic masuk.

### Jawaban interview

> Saya memeriksa model revision, format weight, tokenizer, dependency, dan kapasitas server. Setelah berhasil, saya pin model revision dan container image agar deployment berikutnya reproducible. Jadi perubahan upstream tidak langsung merusak production.

---

# D. Python API dan Integrasi

## 13. API AI timeout ketika proses inference lama

### Yang dianalisis

1. Pisahkan connection timeout, read timeout, dan processing time.
2. Periksa apakah request memang harus synchronous.
3. Untuk pekerjaan panjang, ubah menjadi job asynchronous.
4. Kembalikan job ID dan sediakan endpoint status.
5. Tambahkan retry hanya pada operasi aman atau idempotent.
6. Gunakan queue dan dead-letter queue.

### Harapan hasil yang baik

- Request singkat tetap synchronous.
- Pekerjaan panjang masuk queue.
- Client dapat memeriksa status.
- Retry tidak menghasilkan job ganda.

### Jawaban interview

> Kalau inference lebih lama daripada batas HTTP, saya tidak sekadar memperbesar timeout. Saya ubah menjadi asynchronous job, mengembalikan job ID, dan menyediakan status endpoint. Queue mengatur beban, sedangkan idempotency mencegah retry membuat pekerjaan ganda.

---

## 14. API menerima lonjakan traffic dan semua request melambat

### Yang dianalisis

1. Periksa request rate, concurrent request, dan queue depth.
2. Temukan dependency yang saturasi: CPU, GPU, database, atau provider.
3. Terapkan rate limit dan bounded queue.
4. Gunakan backpressure dan respons 429 ketika kapasitas penuh.
5. Scale hanya setelah bottleneck diketahui.

### Harapan hasil yang baik

- Beban berlebih ditolak atau diantre secara terkendali.
- Service tidak kehabisan memory.
- Sistem pulih setelah traffic turun.

### Jawaban interview

> Saya menggunakan bounded queue dan rate limit agar lonjakan traffic tidak menghabiskan resource. Jika kapasitas penuh, sistem memberikan 429 atau status job, bukan menerima semua request sampai crash. Setelah bottleneck diketahui, baru saya menentukan apakah perlu scaling atau optimasi.

---

## 15. Respons API berubah dan client lain rusak

### Yang dianalisis

1. Periksa perubahan schema dan backward compatibility.
2. Gunakan contract test.
3. Tambahkan API versioning jika terjadi breaking change.
4. Pisahkan internal model output dari public response schema.
5. Dokumentasikan masa deprecation.

### Harapan hasil yang baik

- Client lama tetap bekerja selama masa transisi.
- Breaking change tidak masuk production tanpa terdeteksi.
- Response mempunyai schema yang stabil.

### Jawaban interview

> Output model dapat berubah, tetapi kontrak API tidak boleh ikut berubah tanpa kontrol. Saya memetakan output ke response schema yang stabil, menggunakan contract test, dan membuat versi baru jika ada breaking change. Client lama diberi masa deprecation yang jelas.

---

# E. Docker, Linux, dan Deployment

## 16. Container berjalan lokal tetapi gagal di server

### Yang dianalisis

1. Bandingkan environment variable dan mounted files.
2. Periksa architecture, GPU runtime, driver, dan CUDA compatibility.
3. Periksa permission, port, network, DNS, dan volume path.
4. Periksa healthcheck dan startup log.
5. Pastikan model artifact tersedia.
6. Pin image digest dan dependency.

### Harapan hasil yang baik

- Environment development dan production reproducible.
- Startup gagal dengan pesan jelas jika dependency tidak tersedia.
- Image yang sama digunakan untuk testing dan deployment.

### Jawaban interview

> Saya membandingkan perbedaan environment, bukan langsung mengubah source code. Saya periksa variable, volume, permission, network, GPU runtime, driver, serta model artifact. Setelah penyebab ditemukan, saya membuat startup validation dan mem-pin image agar hasil deployment konsisten.

---

## 17. Container berulang kali restart

### Yang dianalisis

1. Periksa exit code dan log sebelum restart.
2. Bedakan application crash, OOM kill, dan failed healthcheck.
3. Periksa restart policy agar tidak menyembunyikan penyebab.
4. Periksa dependency saat startup.
5. Tambahkan readiness dan liveness check yang tepat.

### Harapan hasil yang baik

- Penyebab restart dapat dibedakan.
- Readiness tidak mengirim traffic sebelum service siap.
- Liveness tidak membunuh service yang hanya sedang memuat model.
- Tidak ada restart loop tanpa alert.

### Jawaban interview

> Restart policy bukan solusi. Saya melihat exit code, OOM status, dan healthcheck untuk menentukan penyebabnya. Untuk model yang startup-nya lama, readiness dan liveness harus dibedakan agar container tidak dibunuh saat masih memuat model.

---

## 18. Disk server penuh karena model dan log

### Yang dianalisis

1. Cari pertumbuhan disk berdasarkan direktori.
2. Periksa model cache, container layer, log, dan temporary files.
3. Terapkan log rotation dan retention policy.
4. Hapus artifact hanya sesuai policy dan setelah memastikan tidak aktif.
5. Tambahkan disk alert sebelum kritis.

### Harapan hasil yang baik

- Disk usage memiliki warning dan critical threshold.
- Log serta model cache mempunyai retention.
- Deployment tidak gagal karena disk mendadak penuh.

### Jawaban interview

> Saya menentukan sumber pertumbuhan disk terlebih dahulu, biasanya log, model cache, atau container layer. Setelah itu saya menerapkan rotation dan retention yang aman, lalu menambahkan alert sebelum kapasitas kritis. Saya tidak menghapus artifact secara acak karena bisa digunakan oleh deployment aktif.

---

# F. Monitoring, Reliability, dan Evaluation

## 19. Healthcheck hijau tetapi pengguna merasa aplikasi lambat

### Yang dianalisis

1. Healthcheck hanya membuktikan proses hidup.
2. Periksa p50, p95, dan p99 latency.
3. Pisahkan queue, retrieval, model inference, dan external API time.
4. Periksa error rate, timeout, dan saturation.
5. Tambahkan synthetic request end-to-end.

### Harapan hasil yang baik

- Monitoring mencakup latency, traffic, errors, dan saturation.
- Ada end-to-end health signal.
- Alert tidak hanya berdasarkan process uptime.

### Jawaban interview

> Healthcheck hijau belum membuktikan layanan sehat. Saya melihat p95 latency dan memecah waktu antara queue, retrieval, inference, serta dependency eksternal. Saya juga menambahkan synthetic request agar sistem diuji dari sudut pandang pengguna.

---

## 20. Biaya token naik tajam tanpa kenaikan traffic

### Yang dianalisis

1. Ukur input dan output token per request.
2. Periksa perubahan prompt, history, top-k, dan dokumen context.
3. Cari retry atau agent loop yang tidak terlihat.
4. Periksa model routing dan harga provider.
5. Tambahkan token budget dan cost per task.

### Harapan hasil yang baik

- Biaya dapat ditelusuri per fitur atau task.
- Context dan output memiliki batas.
- Agent loop tidak menghasilkan call tersembunyi.

### Jawaban interview

> Saya membandingkan token per request, bukan hanya traffic. Penyebabnya dapat berupa context yang membesar, output terlalu panjang, retry, atau agent loop. Saya menetapkan token budget per task dan memonitor biaya per fitur agar kenaikannya dapat dijelaskan.

---

## 21. Evaluasi model bagus, tetapi pengguna tidak terbantu

### Yang dianalisis

1. Tentukan keputusan atau pekerjaan yang ingin dibantu.
2. Periksa apakah output masuk ke workflow pengguna.
3. Ukur waktu, langkah manual, correction rate, dan adoption.
4. Periksa explainability dan kepercayaan user.
5. Gabungkan model metric dengan business outcome.

### Harapan hasil yang baik

- Pekerjaan user lebih cepat atau lebih akurat.
- Output muncul pada workflow yang benar.
- Penggunaan dan correction rate dapat diukur.

### Jawaban interview

> Model yang akurat belum tentu memberi nilai. Saya mengukur apakah sistem mengurangi waktu, langkah manual, dan correction rate. Saya juga melihat adoption. Dengan begitu keberhasilan tidak hanya ditentukan oleh benchmark model, tetapi oleh dampaknya terhadap pekerjaan pengguna.

---

# G. Skenario Relevan dengan Operasional PSN

## 22. Data telemetri satu lokasi berhenti masuk

### Yang dianalisis

1. Periksa last-seen timestamp dan expected reporting interval.
2. Periksa heartbeat perangkat serta pipeline.
3. Periksa queue lag, consumer, network, authentication, dan schema.
4. Bedakan data terlambat dengan data hilang.
5. Siapkan recovery dan backfill.

### Harapan hasil yang baik

- Missing data mempunyai alert terpisah dari anomaly value.
- Setiap source memiliki freshness target.
- Dashboard tidak menampilkan kondisi normal ketika datanya tidak tersedia.

### Jawaban interview

> Model anomaly hanya dapat menilai data yang masuk. Karena itu saya memisahkan model monitoring dan data-quality monitoring. Saya menggunakan last-seen timestamp, heartbeat, queue lag, dan freshness target agar data yang berhenti tidak dianggap sebagai kondisi normal.

---

## 23. Sistem anomaly detection menghasilkan terlalu banyak alert

### Yang dianalisis

1. Ukur precision alert dan jumlah alert per periode.
2. Periksa threshold per source atau operating mode.
3. Pertimbangkan seasonality.
4. Gunakan persistence rule.
5. Gabungkan alert terkait menjadi satu incident.
6. Gunakan feedback operator.

### Harapan hasil yang baik

- Alert dapat ditindaklanjuti.
- False positive terkendali.
- Satu kejadian tidak menghasilkan puluhan notifikasi.
- Severity mempunyai arti operasional.

### Jawaban interview

> Saya tidak mengejar sensitivitas maksimum karena alert yang terlalu banyak akan diabaikan. Saya mengukur precision, menyesuaikan threshold berdasarkan source dan operating mode, lalu menggunakan persistence rule. Alert terkait digabung menjadi satu incident dan feedback operator dipakai untuk tuning.

---

## 24. Agent diminta menjalankan perubahan konfigurasi jaringan

### Yang dianalisis

1. Pastikan agent hanya memberi rekomendasi atau benar-benar mempunyai write access.
2. Validasi identitas dan kewenangan pengguna.
3. Gunakan allowlist parameter dan batas perubahan.
4. Tampilkan execution plan sebelum tindakan.
5. Minta approval manusia.
6. Simpan before-state, after-state, dan rollback plan.

### Harapan hasil yang baik

- Perubahan kritis tidak dieksekusi hanya karena output LLM.
- Ada human approval.
- Input tervalidasi secara deterministik.
- Rollback tersedia.

### Jawaban interview

> Untuk konfigurasi kritis, LLM tidak boleh mempunyai kebebasan penuh. Agent dapat menyusun rencana, tetapi parameter divalidasi secara deterministik dan operator harus menyetujui sebelum eksekusi. Kondisi sebelum dan sesudah perubahan disimpan agar dapat diaudit dan di-rollback.

---

## 25. Koneksi ke lokasi remote tidak stabil

### Yang dianalisis

1. Bedakan timeout, packet loss, dan service error.
2. Gunakan timeout yang jelas dan retry dengan backoff serta jitter.
3. Hindari retry storm.
4. Gunakan local buffering jika data tidak boleh hilang.
5. Gunakan idempotency saat sinkronisasi ulang.
6. Pantau backlog dan umur data tertua.

### Harapan hasil yang baik

- Data ditahan secara lokal ketika koneksi terputus.
- Sinkronisasi dapat dilanjutkan tanpa duplikasi.
- Gangguan satu lokasi tidak menjatuhkan sistem pusat.

### Jawaban interview

> Untuk koneksi remote, saya mengasumsikan gangguan dapat terjadi. Data dibuffer secara lokal, retry memakai backoff dan jitter, lalu sinkronisasi menggunakan idempotency agar tidak terjadi duplikasi. Saya memonitor backlog serta umur data tertua untuk memastikan pemulihan berlangsung.

---

# Ringkasan Prioritas Belajar

Jika waktu persiapan terbatas, kuasai urutan berikut:

1. Agent memilih tool salah, agent loop, dan approval tindakan berisiko.
2. Model serving OOM, latency, throughput, dan rollback.
3. Python API timeout, queue, retry, dan idempotency.
4. Docker gagal di server, restart loop, dan disk penuh.
5. Monitoring p95, queue depth, GPU, token, cost, dan audit log.
6. RAG retrieval, security filter, serta source verification.
7. Telemetry freshness, anomaly alert, dan koneksi remote.

# Jawaban Universal 45 Detik

> Saya mulai dengan menentukan gejala, dampak, dan satu contoh yang dapat direproduksi. Setelah itu saya memecah sistem menjadi komponen seperti input, API, queue, retrieval, model, tool, dan infrastructure, kemudian mencari titik pertama yang hasilnya salah. Saya memperbaiki akar masalah, menguji kembali kasus awal, lalu menjalankan regression atau load test. Hasil yang saya harapkan harus terukur melalui kualitas, latency, error rate, resource usage, dan dampaknya terhadap pengguna. Terakhir, saya menambahkan monitoring agar masalah yang sama dapat ditemukan lebih cepat.
