# PSN — Peta Kompetensi AI Engineering

**Kandidat:** Thariq Ivan Anendar

**Tujuan:** Ringkasan konsep dan pengalaman untuk interview AI Engineer DevOps

**Sumber audit:** project lokal, GitHub `vannndar`, Hugging Face Hub, Docker, cloud, serta instalasi Hermes aktif

## Penanda

- ✅ **Pernah dilakukan:** ada penggunaan atau artefak nyata
- 🟡 **Dipelajari atau pengalaman terdekat:** belum membangun implementasi penuh
- ❌ **Belum pernah:** jangan mengklaim pernah mengimplementasikan

Setiap bagian menjelaskan apa teknologinya, masalah yang diselesaikan, keunggulan, contoh, dan bukti pengalaman saya.

---

# 1. AI Agent

## Hermes Agent

**Apa itu:** Framework AI agent yang dapat menerima tujuan, memilih tool, membaca hasilnya, lalu melanjutkan langkah berikutnya sampai tugas selesai.

**Masalah yang diselesaikan:** LLM biasa hanya menghasilkan teks. Agent dapat melakukan tindakan nyata seperti membaca file, menjalankan command, mencari web, menggunakan browser, mengubah kode, menjadwalkan pekerjaan, dan mendelegasikan subtask.

**Keunggulan:**

- Menangani pekerjaan multi-step
- Memilih tool sesuai kebutuhan
- Mendukung memory, skills, sessions, dan scheduled jobs
- Mendukung delegasi dan banyak agent spesialis
- Provider-agnostic

**Contoh:** Agent menerima tugas audit project, mencari repository, membaca konfigurasi, menjalankan test, memperbaiki file, lalu memverifikasi hasilnya.

**Pengalaman saya:** ✅ **Pernah menggunakan dan mengoperasikan**

- Menjalankan Hermes melalui Discord dan local gateway
- Mengelola 37 profile agent untuk research, infrastructure, testing, deployment, dan pekerjaan lain
- Menggunakan subagent paralel untuk audit GitHub, filesystem, dan cloud
- Menjalankan scheduled agent untuk email, briefing, job discovery, memory review, dan system monitoring
- Menggunakan memory, skills, tool calling, cron, dan multi-profile gateway

**Batas klaim:** Saya menggunakan, mengonfigurasi, dan mengorkestrasi Hermes. Saya belum membangun agent runtime seperti Hermes dari nol.

## LangGraph

**Apa itu:** Framework untuk membangun agent sebagai graph. Node adalah langkah, edge adalah transisi, dan state menyimpan konteks workflow.

**Masalah yang diselesaikan:** Agent membutuhkan percabangan, loop, retry, checkpoint, dan state yang sulit dipelihara sebagai rangkaian fungsi linear.

**Keunggulan:**

- Alur dan state eksplisit
- Conditional routing dan loop
- Checkpoint dan human approval
- Lebih mudah ditelusuri dan di-debug

**Contoh:** Agent mengambil telemetri. Kalau data kurang, alur kembali ke tool pengambilan data. Kalau cukup, alur masuk ke analisis dan pembuatan laporan.

**Pengalaman saya:** ❌ Belum mengimplementasikan LangGraph.

**Pengalaman terdekat:** Pipeline RAG bertahap serta worker dengan queue, retry, lease, dan dead-letter queue. Namun alurnya masih ditentukan kode.

## Agentic Loop

**Apa itu:** Siklus model melakukan reasoning, memilih action atau tool, membaca observation, lalu menentukan langkah berikutnya.

**Masalah yang diselesaikan:** Satu respons LLM tidak cukup untuk pekerjaan yang membutuhkan pencarian, pemeriksaan, perbaikan, dan verifikasi berulang.

**Keunggulan:** Dapat menyesuaikan langkah berdasarkan hasil sebelumnya dan mencoba pendekatan lain saat gagal.

**Risiko dan solusi:** Loop dapat tidak berhenti, memilih tool yang salah, atau menghabiskan token. Solusinya adalah step limit, timeout, approval, permission, logging, dan verifikasi hasil.

**Pengalaman saya:** ✅ Pernah menggunakan agentic loop melalui Hermes. Belum menulis loop agent sebagai framework sendiri.

---

# 2. MCP

## MCP Client

**Apa itu:** MCP client terhubung ke MCP server, menemukan tools dan schema-nya, lalu menyediakan tools tersebut kepada agent.

**Masalah yang diselesaikan:** Tanpa standar, setiap aplikasi AI harus membuat integrasi khusus untuk setiap API dan sumber data.

**Keunggulan:**

- Tool discovery otomatis
- Schema input dan output jelas
- Integrasi dapat digunakan kembali
- Mendukung local stdio dan remote HTTP server

**Contoh:** Supabase MCP menyediakan operasi database kepada agent tanpa membuat integrasi baru untuk setiap percakapan.

**Pengalaman saya:** ✅ **Pernah menggunakan dan mengonfigurasi melalui Hermes**

MCP yang aktif:

- Chrome DevTools
- Playwright
- Exa
- Parallel Search
- Supabase
- Zen DevTools

Saya memakainya untuk browser automation, web research, dan operasi database.

**Batas klaim:** Saya sudah menggunakan MCP client dan server pihak lain, tetapi belum menulis MCP server sendiri.

## MCP Server

**Apa itu:** Service yang mengekspos tools, resources, atau prompt kepada MCP client melalui protokol standar.

**Masalah yang diselesaikan:** Fungsi internal cukup dibuat sekali lalu dapat digunakan oleh berbagai agent dan aplikasi AI.

**Keunggulan:**

- Logic bisnis tetap berada di server
- Authentication, permission, dan audit log terpusat
- Tool reusable untuk banyak client
- Kontrak parameter lebih konsisten

**Contoh:** PSN dapat menyediakan `get_telemetry_history`, `detect_anomaly`, dan `render_chart` sebagai MCP tools.

**Pengalaman saya:** ❌ Belum membangun MCP server.

**Pendekatan jika diminta:** Mulai dari satu read-only tool, schema ketat, authentication, audit log, timeout, error handling, dan test. Akses write ditambahkan setelah read flow stabil.

---

# 3. Tool Calling dan API Integration

## Tool Calling

**Apa itu:** Model memilih fungsi berdasarkan nama, deskripsi, dan schema parameter. Aplikasi memvalidasi lalu menjalankan fungsi tersebut.

**Masalah yang diselesaikan:** LLM tidak memiliki akses langsung ke database, browser, file, kalkulator, atau data real-time.

**Keunggulan:**

- Model dapat memakai data terbaru
- Operasi penting tetap dilakukan kode deterministik
- Beberapa tools dapat digabungkan
- Hasil tool dapat dicatat dan diperiksa

**Pengalaman saya:** ✅ Pernah menggunakan tool calling melalui Hermes, misalnya file tools, terminal, browser, Supabase MCP, dan subagent delegation.

**Batas klaim:** Saya belum mengimplementasikan function-calling loop sendiri menggunakan SDK LLM di aplikasi buatan saya.

## API Integration

**Apa itu:** Aplikasi memanggil endpoint yang telah ditentukan developer.

**Perbedaan:** Pada API integration, kode menentukan endpoint dan waktunya. Pada tool calling, model mengusulkan fungsi dan argumen, tetapi aplikasi tetap memegang validasi serta permission.

**Pengalaman saya:** ✅

- Gemini dan DeepSeek API
- EZVIZ Open Platform
- Google Drive OAuth
- Telegram webhook
- FastAPI endpoint `/predict`
- Supabase REST dan PostgreSQL
- OpenAI-compatible gateway

---

# 4. Model Serving

## Docker Self-hosted Serving

**Apa itu:** Model dijalankan sebagai service dalam container dan diakses melalui HTTP.

**Masalah yang diselesaikan:** Model tidak lagi bergantung pada notebook dan dapat dipanggil aplikasi lain dengan environment yang konsisten.

**Keunggulan:** Reproducible, mudah dipindahkan, dapat di-scale terpisah, dan data dapat tetap berada pada infrastructure sendiri.

**Pengalaman saya:** ✅

- Self-host Hugging Face Text Embeddings Inference
- Model `BAAI/bge-m3`, vector 1024 dimensi
- GPU RTX 3050 8 GB
- Image digest dan model revision dipin
- Healthcheck, API key, GPU reservation, dan resource limit
- Port bind ke localhost, diakses melalui Nginx dan Cloudflare Tunnel

## Hugging Face Spaces

**Apa itu:** Platform untuk men-deploy demo atau aplikasi ML melalui Gradio, Streamlit, atau Docker.

**Masalah yang diselesaikan:** Model dapat diakses tanpa mengelola server sendiri.

**Keunggulan:** Deployment cepat, terintegrasi dengan Hub, serta cocok untuk demo dan prototype endpoint.

**Pengalaman saya:** ✅

- `vannndar/walet-inference`: Gradio Space untuk computer vision
- `vannndar/kurang_sks`: Docker Space dengan FastAPI dan `/predict`

**Batas klaim:** Ini hosted application di Spaces, bukan Dedicated Inference Endpoint.

## vLLM

**Apa itu:** Inference engine untuk serving generative LLM dengan throughput tinggi dan OpenAI-compatible API.

**Masalah yang diselesaikan:** Serving Transformers biasa kurang efisien untuk banyak concurrent request dan dapat membuang VRAM pada KV cache.

**Keunggulan:** PagedAttention, continuous batching, penggunaan GPU lebih efisien, dan integrasi API mudah.

**Pengalaman saya:** ❌ Belum menjalankan vLLM.

**Pengalaman terdekat:** TEI self-hosted. Polanya sama-sama service HTTP berbasis model, tetapi TEI saya gunakan untuk embedding, sedangkan vLLM untuk generative LLM.

## SGLang

**Apa itu:** Framework serving dan structured generation dengan runtime optimization serta prefix caching.

**Masalah yang diselesaikan:** Repeated prompt dan workflow generation kompleks dapat menghitung prefix yang sama berkali-kali.

**Keunggulan:** Prefix caching, structured generation, dan throughput tinggi.

**Pengalaman saya:** ❌ Belum menjalankan SGLang.

## Managed Endpoint

**Apa itu:** Provider cloud mengelola deployment, scaling, monitoring, dan availability endpoint model.

**Masalah yang diselesaikan:** Tim tidak perlu mengelola GPU server, patching, dan autoscaling sendiri.

**Keunggulan:** Operasional lebih sederhana dan terintegrasi IAM. Kekurangannya biaya, vendor lock-in, dan kontrol lebih sedikit.

- ❌ Azure ML endpoint
- ❌ AWS SageMaker endpoint
- ❌ Vertex AI endpoint

---

# 5. Model Training dan Fine-Tuning

## QLoRA

**Apa itu:** Base model di-quantize 4-bit lalu hanya adapter LoRA yang dilatih.

**Masalah yang diselesaikan:** Full fine-tuning LLM membutuhkan GPU memory dan biaya besar.

**Keunggulan:** Model 7 sampai 8 miliar parameter dapat diadaptasi pada GPU terbatas, adapter kecil, dan eksperimen lebih murah.

**Pengalaman saya:** ✅

- Fine-tuning Llama 3.1, Qwen, Mistral, Gemma, SEA-LION, dan SahabatAI
- Unsloth, PEFT, TRL `SFTTrainer`, dan bitsandbytes
- LoRA rank 16 pada `q_proj`, `k_proj`, dan `v_proj`
- 5 epoch, effective batch 32, sekitar 2.332 sampel
- Adapter disimpan per epoch

## Quantization

**Apa itu:** Menurunkan presisi bobot, misalnya dari 16-bit ke 4-bit.

**Masalah yang diselesaikan:** Model tidak muat di VRAM atau inference terlalu mahal.

**Keunggulan:** Memory dan biaya compute turun. Trade-off-nya potensi penurunan kualitas.

**Pengalaman saya:** ✅ Menggunakan 4-bit quantization pada QLoRA dan model 7B.

## Full Pretraining dan Distributed Training

**Apa itu:** Pretraining membangun kemampuan dasar model dari corpus besar. Distributed training membagi proses ke banyak GPU.

**Masalah yang diselesaikan:** Dataset dan model terlalu besar untuk satu GPU.

- ❌ Pretraining LLM dari nol
- ❌ Distributed multi-GPU training

---

# 6. RAG

## Ingestion dan Chunking

**Apa itu:** Dokumen dibaca, dibersihkan, dan dibagi menjadi bagian kecil.

**Masalah yang diselesaikan:** Dokumen terlalu panjang untuk embedding dan context window. Satu vector untuk dokumen panjang juga kehilangan detail.

**Keunggulan:** Retrieval lebih spesifik dan konteks lebih relevan.

**Pengalaman saya:** ✅ PDF, JSON, CSV, web content, dan `RecursiveCharacterTextSplitter`.

## Embedding

**Apa itu:** Mengubah teks menjadi vector yang merepresentasikan makna.

**Masalah yang diselesaikan:** Keyword search gagal saat pertanyaan dan dokumen memakai kata berbeda tetapi maknanya sama.

**Pengalaman saya:** ✅

- `paraphrase-multilingual-MiniLM-L12-v2` lokal
- `BAAI/bge-m3` melalui TEI
- Gemini embedding melalui API

## Vector Store

**Apa itu:** Penyimpanan vector yang mendukung similarity search.

**Masalah yang diselesaikan:** Menemukan bagian dokumen paling relevan secara semantik.

- ✅ FAISS untuk local prototype
- ✅ Chroma pada eksperimen LawBot
- ✅ pgvector pada aplikasi PostgreSQL
- ❌ Milvus
- ❌ Weaviate

## Hybrid Retrieval

**Apa itu:** Menggabungkan BM25 keyword search dan semantic vector search.

**Masalah yang diselesaikan:** Vector search dapat melewatkan istilah persis, sedangkan BM25 lemah terhadap parafrase.

**Keunggulan:** Exact match dan semantic match diperoleh bersamaan.

**Pengalaman saya:** ✅ BM25 + semantic search pada LawBot.

## Reranking

**Apa itu:** Cross-encoder menilai ulang kandidat dokumen bersama query.

**Masalah yang diselesaikan:** Retriever awal cepat, tetapi ranking-nya belum cukup presisi.

**Keunggulan:** Kualitas top result naik tanpa menjalankan model mahal pada seluruh corpus.

**Pengalaman saya:** ✅ Cross-encoder reranking.

## Grounded Generation

**Apa itu:** LLM menjawab berdasarkan konteks hasil retrieval dan menyertakan sumber.

**Masalah yang diselesaikan:** Mengurangi jawaban dari memory model yang tidak dapat diverifikasi.

**Pengalaman saya:** ✅ LawBot dengan konteks dan sitasi sumber.

---

# 7. Evaluation

## Hallucination dan Factual Accuracy

**Apa itu:** Mengukur apakah klaim model benar dan didukung sumber.

**Masalah yang diselesaikan:** Jawaban dapat terdengar yakin meskipun salah.

**Pengalaman saya:** ✅ Membandingkan enam LLM pada baseline, fine-tuning, dan RAG.

## ROUGE dan BERTScore

**Apa itu:** ROUGE mengukur overlap kata; BERTScore mengukur kemiripan semantik.

**Masalah yang diselesaikan:** Membandingkan jawaban dengan referensi secara otomatis.

**Keunggulan:** ROUGE sederhana, BERTScore lebih toleran terhadap parafrase. Keduanya tidak menggantikan evaluasi faktual.

**Pengalaman saya:** ✅ LawBot.

## LLM-as-Judge

**Apa itu:** LLM lain menilai jawaban memakai rubric seperti relevance, factuality, dan completeness.

**Masalah yang diselesaikan:** Jawaban terbuka sulit dinilai dengan exact match.

**Keunggulan:** Cepat dan scalable. Risikonya bias terhadap gaya, panjang, atau model tertentu.

**Pengalaman saya:** ✅ DeepSeek API untuk evaluasi paralel dan dibandingkan dengan human evaluation.

## Retrieval Evaluation

**Apa itu:** Mengukur apakah dokumen relevan muncul pada top-k.

**Metrik:** Precision@k, Recall@k, dan kualitas ranking.

**Pengalaman saya:** ✅ LawBot.

## Operational Evaluation

**Apa itu:** Mengukur kualitas sistem setelah deployment.

**Metrik:** Latency, error rate, empty retrieval, queue depth, dan heartbeat.

**Pengalaman saya:** ✅ Monitoring worker serta pengurangan pipeline PLN dari sekitar satu jam menjadi 28 detik.

---

# 8. MLOps dan AI Operations

## Containerization

**Apa itu:** Model, API, worker, dan dependency dikemas dalam container.

**Masalah yang diselesaikan:** Perbedaan environment development dan server.

**Pengalaman saya:** ✅ Docker dan Docker Compose.

## Healthcheck dan Monitoring

**Apa itu:** Memeriksa apakah service hidup dan output tetap dihasilkan dengan benar.

**Masalah yang diselesaikan:** Service dapat tetap hidup tetapi diam-diam berhenti menghasilkan output berguna.

**Pengalaman saya:** ✅ Health endpoint, JSON log, worker heartbeat, frame heartbeat, no-segment watchdog, disk watermark, dan reconciliation.

## Retry dan Dead-letter Queue

**Apa itu:** Kegagalan sementara dicoba ulang dengan batas tertentu; kegagalan permanen dipindahkan untuk investigasi.

**Masalah yang diselesaikan:** Network dan provider dapat gagal sementara, tetapi retry tanpa batas juga berbahaya.

**Pengalaman saya:** ✅ Backoff, jitter, `Retry-After`, dan dead-letter queue.

## Atomic Activation

**Apa itu:** Versi baru dibangun sebagai candidate dan baru diaktifkan setelah proses lengkap berhasil.

**Masalah yang diselesaikan:** Index lama tidak boleh hilang ketika pembangunan versi baru gagal.

**Pengalaman saya:** ✅ Candidate generation dan atomic activation.

## Version Pinning

**Apa itu:** Mengunci image dan model ke digest atau revision tertentu.

**Masalah yang diselesaikan:** Restart tidak mengambil versi baru yang belum diuji.

**Pengalaman saya:** ✅ Docker digest dan model revision.

## CI/CD dan Model Registry

- ✅ CI/CD aplikasi pada beberapa project
- ❌ MLflow atau dedicated model registry
- ❌ Kubernetes, Kubeflow, dan Airflow

---

# 9. Cloud dan Platform

## Hugging Face

**Apa itu:** Hub dan platform untuk model, dataset, Spaces, dan inference tooling.

**Masalah yang diselesaikan:** Distribusi model, penggunaan pretrained model, dan deployment aplikasi ML.

**Pengalaman saya:** ✅ Transformers, model Hub, TEI self-hosted, dan dua Spaces.

**Batas:** Belum mempublikasikan fine-tuned LLM sebagai model repository khusus dan belum memakai Dedicated Inference Endpoint.

## AWS

**Apa itu:** Cloud untuk compute, storage, networking, dan managed ML.

**Pengalaman saya:** ✅ EC2 sebagai relay server proyek UAV, termasuk UDP service dan network testing.

**Belum pernah:** ❌ SageMaker, Lambda, ECS, EKS, dan tidak ditemukan bukti implementasi S3 pada audit ini.

## Azure

**Apa itu:** Cloud Microsoft; Azure ML menyediakan workspace, training jobs, registry, dan endpoints.

**Masalah yang diselesaikan:** Mengelola lifecycle model dan compute tanpa membangun semua infrastructure sendiri.

**Pengalaman saya:** 🟡 Mempelajari Azure for AI and Machine Learning.

**Belum pernah:** ❌ Azure ML SDK, workspace, job, registry, endpoint, dan Azure OpenAI deployment.

## Google Cloud

**Pengalaman saya:** ✅ Firebase Auth, Crashlytics, dan Gemini API.

**Belum pernah:** ❌ Vertex AI training dan managed endpoint.

---

# 10. Computer Vision

## Object Detection

**Apa itu:** Menemukan lokasi dan kelas objek pada gambar atau video.

**Masalah yang diselesaikan:** Sistem perlu mengetahui bukan hanya isi gambar, tetapi posisi objek.

**Pengalaman saya:** ✅ YOLO untuk api, asap, dan walet.

## Embedding Identification

**Apa itu:** Objek diubah menjadi embedding lalu identitas ditentukan berdasarkan similarity.

**Masalah yang diselesaikan:** Identitas baru dapat ditambahkan tanpa selalu melatih ulang classifier.

**Pengalaman saya:** ✅ InsightFace atau ArcFace dan ResNet untuk identifikasi walet.

## Real-time Video Pipeline

**Apa itu:** Video diterima, diproses, ditampilkan, dan diarsipkan terus-menerus.

**Masalah yang diselesaikan:** CCTV tidak cukup diproses sebagai kumpulan gambar terpisah.

**Pengalaman saya:** ✅ FFmpeg, HLS, MP4 archive, watchdog, dan uploader.

---

# 11. Time Series dan Predictive Maintenance

## Remaining Useful Life

**Apa itu:** Memperkirakan waktu atau siklus tersisa sebelum komponen perlu diganti.

**Masalah yang diselesaikan:** Maintenance berdasarkan jadwal dapat terlalu cepat atau terlambat.

**Pengalaman saya:** ✅ PLN Nusantara Power menggunakan data sensor pembangkit.

## Time-series Foundation Model

**Apa itu:** Model yang dilatih pada beragam time series dan dapat diterapkan ke data baru dengan sedikit atau tanpa training tambahan.

**Masalah yang diselesaikan:** Data kegagalan berlabel biasanya terbatas.

**Pengalaman saya:** ✅ Sundial dan Timer untuk zero-shot prediction.

## Anomaly Detection

**Apa itu:** Mendeteksi nilai atau pola yang menyimpang dari kondisi normal.

**Masalah yang diselesaikan:** Gangguan baru mungkin belum memiliki label.

**Keunggulan:** Memberi early warning. Tantangannya false positive dan perubahan pola normal.

**Pengalaman saya:** 🟡 Fondasi time series dan RUL sudah ada, tetapi belum ada sistem anomaly detection production.

---

# 12. Backend AI

## FastAPI

**Apa itu:** Framework Python untuk API dengan type validation dan dokumentasi OpenAPI.

**Masalah yang diselesaikan:** Model perlu kontrak yang jelas agar dapat dipanggil service lain.

**Pengalaman saya:** ✅ Model endpoint dan backend service.

## PostgreSQL, Supabase, dan pgvector

**Apa itu:** PostgreSQL menyimpan data relasional, Supabase menambahkan API dan authentication, sedangkan pgvector menambahkan similarity search.

**Masalah yang diselesaikan:** Data aplikasi dan vector berada di database sama dan dapat mengikuti access control yang sama.

**Pengalaman saya:** ✅ PostgreSQL, Supabase, RLS, dan pgvector.

## Async Worker dan Queue

**Apa itu:** Pekerjaan berat diproses di luar request utama melalui antrean.

**Masalah yang diselesaikan:** Indexing atau inference panjang tidak memblokir API dan dapat di-retry terpisah.

**Pengalaman saya:** ✅ Queue, lease, retry, dead-letter queue, dan graceful shutdown.

---

# Ringkasan Status

## Sudah digunakan atau dilakukan

- ✅ Hermes AI Agent dan agentic loop
- ✅ Multi-agent delegation dan scheduled agent
- ✅ MCP client serta enam MCP server melalui Hermes
- ✅ Tool calling melalui Hermes
- ✅ REST API dan external API integration
- ✅ Docker model serving dan Hugging Face TEI
- ✅ Hugging Face Spaces
- ✅ QLoRA dan quantization
- ✅ RAG, FAISS, Chroma, pgvector, hybrid retrieval, dan reranking
- ✅ LLM evaluation dan retrieval evaluation
- ✅ Monitoring, retry, dead-letter queue, dan atomic activation
- ✅ Computer vision dan time-series prediction
- ✅ AWS EC2, Firebase, dan Gemini API

## Dipelajari atau pengalaman terdekat

- 🟡 LangGraph
- 🟡 Pembuatan MCP server
- 🟡 Function-calling loop pada aplikasi sendiri
- 🟡 vLLM dan SGLang
- 🟡 Azure AI/ML
- 🟡 Anomaly detection production

## Belum pernah dibangun atau digunakan

- ❌ Agent runtime dari nol
- ❌ MCP server sendiri
- ❌ vLLM atau SGLang production
- ❌ Azure ML workspace, job, registry, atau endpoint
- ❌ SageMaker dan Vertex AI
- ❌ Kubernetes, Kubeflow, Airflow, dan MLflow
- ❌ Milvus dan Weaviate
- ❌ LLM pretraining dari nol
- ❌ Distributed multi-GPU training

---

# Jawaban Gabungan

> Pengalaman saya paling kuat pada RAG, fine-tuning QLoRA, evaluation, dan deployment. Saya juga menggunakan Hermes sebagai AI agent untuk tool calling, agentic workflow, multi-agent delegation, scheduled jobs, dan koneksi ke beberapa MCP server. Untuk serving, saya pernah self-host Hugging Face TEI dengan Docker dan GPU serta deploy dua aplikasi di Hugging Face Spaces. Saya membedakan teknologi yang sudah saya gunakan dari yang sudah saya bangun sendiri. Saya sudah menggunakan AI agent dan MCP client melalui Hermes, tetapi belum membangun agent runtime atau MCP server dari nol. Saya juga belum menjalankan vLLM, SGLang, atau managed ML endpoint.