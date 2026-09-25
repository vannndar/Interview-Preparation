# PSN — AI Engineering Competency Map

**Kandidat:** Thariq Ivan Anendar  
**Tujuan:** Ringkasan pengalaman dan konsep untuk menjawab interview AI Engineer DevOps  
**Audit:** project lokal, GitHub `vannndar`, Hugging Face Hub, notebook, Docker, dan konfigurasi cloud

## Penanda

- ✅ **Pernah dilakukan:** ada implementasi atau artefak yang bisa diperiksa
- 🟡 **Pernah dipelajari atau dicoba sebagian:** memahami konsep atau pernah mengerjakan bagian terdekat, tetapi belum implementasi penuh
- ❌ **Belum ditemukan bukti:** jangan mengaku pernah memakai

---

# 1. AI Agent

- ❌ **LangGraph agent:** belum ditemukan implementasi nyata
- ❌ **Agentic loop:** belum ada sistem tempat LLM memilih langkah dan mengulang proses secara mandiri
- 🟡 **Pengalaman terdekat:** pipeline RAG bertahap dan worker queue dengan retry, dead-letter queue, lease, serta atomic activation
- 🟡 **Pemahaman:** agent memiliki state, tool selection, conditional flow, dan batas jumlah langkah

**Inti jawaban:**

> Saya belum pernah membangun AI agent secara penuh. Pengalaman terdekat saya adalah pipeline RAG bertahap dan worker asynchronous yang memiliki state, retry, serta dead-letter queue. Perbedaannya, pada sistem saya langkahnya masih ditentukan oleh kode, sedangkan pada agent model dapat memilih tool dan langkah berikutnya.

---

# 2. MCP

- ❌ **MCP server:** belum ditemukan implementasi nyata
- ❌ **MCP client:** belum ditemukan implementasi nyata
- 🟡 **Pernah dipelajari:** tools, resources, prompts, JSON Schema, serta transport stdio dan Streamable HTTP
- 🟡 **Pengalaman terdekat:** REST API dan service internal yang mengekspos fungsi melalui endpoint

**Inti jawaban:**

> Saya belum pernah mengimplementasikan MCP. Saya memahami bahwa MCP menstandarkan akses model ke tools dan resources. Pengalaman saya yang paling dekat adalah membangun dan mengintegrasikan REST API, tetapi pemilihan endpoint-nya masih ditentukan aplikasi, bukan model melalui protokol MCP.

---

# 3. Tool Calling dan API Integration

- ❌ **LLM function calling:** belum ditemukan implementasi nyata tempat model memilih fungsi
- ✅ **REST API:** FastAPI, Next.js API routes, dan endpoint model
- ✅ **External API integration:** Gemini, DeepSeek, EZVIZ Open Platform, Google Drive OAuth, dan Telegram webhook
- ✅ **Model API:** client LawBot memanggil endpoint `/predict`
- ✅ **Hosted prediction API:** Hugging Face Space `vannndar/kurang_sks` memakai FastAPI dan Docker dengan endpoint `/predict`

**Inti jawaban:**

> Saya sudah banyak mengintegrasikan API, tetapi belum function calling. Pada API integration biasa, saya menentukan fungsi yang dipanggil melalui kode. Pada function calling, model memilih fungsi berdasarkan nama, deskripsi, dan schema parameter, lalu aplikasi tetap bertanggung jawab memvalidasi dan menjalankannya.

---

# 4. Model Serving

## Pernah dilakukan

- ✅ **Docker + GPU lokal/self-hosted:** Hugging Face Text Embeddings Inference untuk `BAAI/bge-m3`, dimensi 1024, berjalan di RTX 3050 8 GB
- ✅ **Container hardening:** image dan revisi model dipin, GPU reservation, healthcheck, API key, resource limit, serta bind ke `127.0.0.1`
- ✅ **Reverse proxy:** akses melalui Cloudflare Tunnel dan Nginx, bukan membuka port model langsung
- ✅ **Hugging Face Spaces:**
  - `vannndar/walet-inference`, Gradio, computer vision, CPU Basic
  - `vannndar/kurang_sks`, Docker + FastAPI, endpoint `/predict`
- ✅ **Local inference:** Hugging Face Transformers, sentence-transformers, Faster Whisper, dan InsightFace
- ✅ **Model API deployment:** LawBot memiliki service model dan client terpisah

## Pernah dipelajari

- 🟡 **vLLM:** memahami OpenAI-compatible endpoint, PagedAttention, dan continuous batching
- 🟡 **SGLang:** mengetahui sebagai opsi serving LLM, belum pernah menjalankan

## Belum dilakukan

- ❌ **vLLM production deployment**
- ❌ **SGLang deployment**
- ❌ **Azure ML managed endpoint**
- ❌ **AWS SageMaker endpoint**
- ❌ **Vertex AI endpoint**

**Inti jawaban:**

> Saya pernah melakukan model serving dengan dua cara. Pertama, self-host Hugging Face TEI di Docker menggunakan GPU untuk embedding. Kedua, deploy aplikasi inference ke Hugging Face Spaces, satu menggunakan Gradio dan satu menggunakan Docker serta FastAPI. Saya belum pernah menjalankan vLLM, SGLang, atau managed endpoint seperti Azure ML dan SageMaker.

---

# 5. Model Training dan Fine-Tuning

- ✅ **QLoRA 4-bit:** fine-tuning LLM memakai Unsloth, PEFT, TRL `SFTTrainer`, dan bitsandbytes
- ✅ **Enam model LLM:** Llama 3.1, Qwen, Mistral, Gemma, SEA-LION, dan SahabatAI
- ✅ **LoRA configuration:** rank 16, target `q_proj`, `k_proj`, dan `v_proj`
- ✅ **Training experiment:** 5 epoch, effective batch 32, sekitar 2.332 sampel
- ✅ **Adapter output:** menyimpan LoRA adapter per epoch
- ✅ **Experiment tracking:** W&B digunakan dalam project tim LawBot. Jangan mengklaim akun atau run tersebut sepenuhnya milik pribadi
- ✅ **Computer vision training/evaluation:** YOLO dan embedding model untuk identifikasi walet
- ✅ **Time-series model usage:** Sundial, Timer, dan eksperimen foundation model untuk prediksi remaining useful life
- ❌ **Full pretraining dari nol:** belum pernah
- ❌ **Distributed multi-GPU training:** belum pernah

**Inti jawaban:**

> Saya pernah fine-tuning enam LLM dengan QLoRA 4-bit. Base model tetap dibekukan dan yang dilatih adalah adapter LoRA, sehingga model 7 sampai 8 miliar parameter tetap dapat diproses pada GPU terbatas. Saya belum pernah melakukan pretraining dari nol atau distributed training.

---

# 6. RAG

- ✅ **Document ingestion:** PDF, JSON, CSV, dan hasil web scraping
- ✅ **Chunking:** `RecursiveCharacterTextSplitter`; beberapa eksperimen memakai ukuran dan overlap berbeda sesuai dokumen
- ✅ **Embedding lokal:** `paraphrase-multilingual-MiniLM-L12-v2`
- ✅ **Embedding self-hosted:** `BAAI/bge-m3` melalui Hugging Face TEI
- ✅ **Embedding API:** Gemini embedding melalui gateway OpenAI-compatible
- ✅ **Vector store:** FAISS, pgvector, dan terdapat bukti eksperimen Chroma pada notebook LawBot
- ✅ **Hybrid retrieval:** BM25 + semantic search
- ✅ **Reranking:** cross-encoder
- ✅ **Grounded generation:** jawaban disertai konteks atau sitasi sumber
- ✅ **Production indexing:** queue, retry, dead-letter queue, candidate generation, dan atomic activation
- ❌ **Milvus:** belum ditemukan bukti penggunaan
- ❌ **Weaviate:** belum ditemukan bukti penggunaan

**Inti jawaban:**

> Saya sudah membangun RAG end-to-end, mulai dari ingestion, chunking, embedding, vector search, hybrid retrieval, reranking, sampai generation dengan sumber. Untuk prototyping saya memakai FAISS dan Chroma, sedangkan pada aplikasi berbasis Postgres saya memakai pgvector. Saya belum pernah memakai Milvus atau Weaviate.

---

# 7. Evaluation

- ✅ **LLM evaluation:** hallucination rate, factual accuracy, ROUGE, BERTScore, dan semantic F1
- ✅ **LLM-as-judge:** DeepSeek API dengan proses evaluasi paralel
- ✅ **Human evaluation:** digunakan sebagai pembanding pada project LawBot
- ✅ **Retrieval evaluation:** precision dan recall at k
- ✅ **Model comparison:** membandingkan enam LLM pada baseline, fine-tuning, dan fine-tuning + RAG
- ✅ **Experiment tracking:** W&B pada project tim
- ✅ **Computer vision evaluation:** accuracy dan laporan evaluasi model walet
- ✅ **Operational evaluation:** latency, healthcheck, heartbeat, failure rate, dan kondisi output kosong

**Inti jawaban:**

> Saya memisahkan evaluasi menjadi kualitas model dan kualitas sistem. Untuk model saya memakai hallucination rate, factual accuracy, ROUGE, BERTScore, human judge, dan LLM-as-judge. Untuk sistem saya melihat latency, failure, retrieval kosong, serta apakah hasilnya benar-benar mengurangi waktu kerja pengguna.

---

# 8. MLOps dan AI Operations

- ✅ **Docker Compose:** memisahkan API, worker, web, database, dan model service
- ✅ **Linux deployment:** menjalankan service pada server Linux
- ✅ **Healthcheck:** endpoint kesehatan dan start period yang sesuai waktu loading model
- ✅ **Monitoring:** structured JSON log, worker heartbeat, frame heartbeat, no-segment watchdog, disk watermark, dan reconciliation
- ✅ **Queue reliability:** retry dengan backoff, `Retry-After`, dead-letter queue, dan lease extension
- ✅ **Safe rollout:** candidate generation lalu atomic activation agar index lama tetap tersedia ketika proses baru gagal
- ✅ **Version pinning:** Docker image digest dan model revision
- ✅ **Security:** localhost binding, reverse proxy, API key, `cap_drop`, dan `no-new-privileges`
- ✅ **CI/CD:** terdapat workflow pada beberapa project, tetapi jangan menyamakan CI/CD aplikasi dengan model registry penuh
- ❌ **Kubernetes:** belum ditemukan pengalaman implementasi
- ❌ **MLflow/model registry:** belum ditemukan bukti
- ❌ **Kubeflow/Airflow:** belum ditemukan bukti

**Inti jawaban:**

> Pengalaman MLOps saya lebih kuat pada deployment dan reliability. Saya memisahkan API dan worker, menggunakan healthcheck, structured logging, retry, dead-letter queue, watchdog, dan atomic activation. Saya belum pernah memakai Kubernetes, Kubeflow, atau model registry seperti MLflow.

---

# 9. Hugging Face

## Pernah dilakukan

- ✅ **Akun Hugging Face:** `vannndar`
- ✅ **Hugging Face Spaces:** dua Space publik
- ✅ **Transformers dan Hub:** download serta penggunaan model melalui Transformers dan `huggingface_hub`
- ✅ **Hugging Face TEI:** self-hosted embedding service di Docker + GPU
- ✅ **Hugging Face models:** Unsloth Llama, sentence-transformers, Sundial, Timer, IndoBERT, PEGASUS, dan Faster Whisper
- ✅ **Model artifacts di Space:** checkpoint computer vision terdapat pada Space Walet

## Batas klaim

- ❌ Tidak ada repository **Model** khusus pada akun HF
- ❌ Tidak ada repository **Dataset** khusus pada akun HF
- ❌ Tidak ditemukan `push_to_hub` untuk model LLM hasil fine-tuning

**Inti jawaban:**

> Saya pernah memakai Hugging Face pada tiga level: memakai model dari Hub, self-host TEI untuk embedding, dan deploy dua aplikasi melalui Hugging Face Spaces. Namun saya belum mempublikasikan LLM hasil fine-tuning sebagai repository model terpisah di Hub.

---

# 10. Cloud

## AWS

- ✅ **EC2:** digunakan sebagai relay server untuk proyek UAV
- ✅ **Linux server setup:** instalasi dependency, UDP server, dan pengujian jaringan
- 🟡 **CodeWhisperer/Kiro:** pernah ada profil autentikasi, tetapi bukan bukti pengalaman AWS infrastructure
- ❌ **S3:** belum ditemukan bukti implementasi dalam audit ini
- ❌ **SageMaker, Lambda, ECS, dan EKS:** belum ditemukan bukti

**Jawaban singkat:**

> Pengalaman AWS saya ada pada EC2 sebagai relay server untuk proyek UAV. Saya belum pernah memakai SageMaker atau managed ML service AWS.

## Azure

- 🟡 **Azure for AI and Machine Learning:** ada bukti pembelajaran atau credential
- ❌ **Azure ML SDK:** belum ditemukan
- ❌ **Azure ML workspace, job, model registry, dan endpoint:** belum ditemukan
- ❌ **Azure OpenAI deployment:** belum ditemukan

**Jawaban singkat:**

> Saya pernah mempelajari Azure untuk AI dan Machine Learning, tetapi belum pernah menjalankan workspace, training job, registry, atau endpoint di Azure ML. Jadi saya tidak mengklaim pengalaman Azure ML production.

## Google Cloud

- ✅ **Firebase:** Auth, Crashlytics, dan konfigurasi aplikasi Flutter
- ✅ **Gemini API:** digunakan dalam project learningwithus dan project lain
- ❌ **Vertex AI:** belum ditemukan bukti penggunaan
- ❌ **GCP ML endpoint atau training job:** belum ditemukan

**Jawaban singkat:**

> Pengalaman Google Cloud saya ada pada Firebase dan integrasi Gemini API. Saya belum pernah menggunakan Vertex AI untuk training atau managed endpoint.

## Hugging Face Cloud

- ✅ **Spaces:** Gradio Space dan Docker Space
- ✅ **Hosted application endpoint:** FastAPI `/predict` melalui Space
- ❌ **Dedicated Inference Endpoint:** belum ditemukan

---

# 11. Computer Vision

- ✅ **YOLO:** object detection pada UAV dan walet
- ✅ **InsightFace/ArcFace dan ResNet:** embedding untuk identifikasi individual walet
- ✅ **Similarity search:** pencocokan embedding
- ✅ **Dataset pipeline:** cropping, labeling tool, penyimpanan metadata, dan evaluasi
- ✅ **Hugging Face Space:** aplikasi Walet Insight
- ✅ **Real-time video pipeline:** CCTV, FFmpeg, HLS, dan arsip MP4

**Inti jawaban:**

> Saya pernah mengerjakan computer vision pada dua konteks. Di UAV saya mengintegrasikan deteksi api dan asap. Pada project walet saya memakai YOLO untuk detection, lalu InsightFace dan ResNet untuk menghasilkan embedding dan melakukan identifikasi melalui similarity search.

---

# 12. Time Series dan Predictive Maintenance

- ✅ **PLN Nusantara Power:** prediksi remaining useful life dari data sensor pembangkit
- ✅ **Foundation model:** eksperimen Sundial, Timer, dan pendekatan zero-shot
- ✅ **Data pipeline:** preprocessing time series dan penyimpanan hasil ke PostgreSQL
- ✅ **Domain study:** mempelajari penyebab kerusakan komponen, bukan hanya membangun model
- 🟡 **Anomaly detection:** memahami baseline, threshold, supervised, dan unsupervised; bukti implementasi utamanya lebih kuat pada RUL daripada sistem anomaly detection production

**Inti jawaban:**

> Pada PLN Nusantara Power saya mengerjakan remaining useful life dari data sensor dan mengeksplorasi foundation model time series untuk zero-shot prediction. Pengalaman ini paling dekat dengan telemetri satelit karena sama-sama berupa data perangkat yang berubah terhadap waktu.

---

# 13. Backend dan Data Engineering

- ✅ **Python dan FastAPI**
- ✅ **Next.js API routes**
- ✅ **PostgreSQL dan Supabase**
- ✅ **pgvector dan Row Level Security**
- ✅ **Async worker dan queue**
- ✅ **OAuth dan external API integration**
- ✅ **Nginx dan Cloudflare Tunnel**
- 🟡 **Go:** pernah digunakan pada project tingkat dasar
- ❌ **Rust:** belum ditemukan pengalaman implementasi

---

# Ringkasan Cepat

## Kekuatan utama

- ✅ RAG end-to-end
- ✅ QLoRA fine-tuning enam LLM
- ✅ Hugging Face Transformers, TEI, dan Spaces
- ✅ Docker/Linux deployment
- ✅ Evaluation dan experiment comparison
- ✅ Monitoring dan reliability worker
- ✅ Computer vision
- ✅ Time-series prediction
- ✅ FastAPI, PostgreSQL, dan API integration

## Sedang dipelajari atau punya pengalaman terdekat

- 🟡 AI Agent dan LangGraph
- 🟡 MCP
- 🟡 LLM function calling
- 🟡 vLLM dan SGLang
- 🟡 Azure untuk AI/ML
- 🟡 Anomaly detection production

## Jangan diklaim sebagai pengalaman

- ❌ vLLM atau SGLang production
- ❌ LangGraph agent production
- ❌ MCP server/client
- ❌ Azure ML workspace, job, registry, atau endpoint
- ❌ AWS SageMaker
- ❌ Vertex AI
- ❌ Kubernetes, Kubeflow, Airflow, atau MLflow
- ❌ Milvus atau Weaviate
- ❌ Pretraining LLM dari nol
- ❌ Distributed multi-GPU training

---

# Jawaban Gabungan untuk Interview

> Pengalaman saya paling kuat pada RAG, fine-tuning QLoRA, evaluation, dan deployment. Saya pernah membandingkan enam LLM, membangun retrieval hybrid dengan reranking, menjalankan embedding service Hugging Face TEI di Docker dan GPU, serta deploy dua aplikasi melalui Hugging Face Spaces. Untuk operasional, saya menggunakan healthcheck, worker queue, retry, dead-letter queue, watchdog, dan atomic activation. Saya belum pernah membangun agent, MCP, atau menjalankan vLLM di production. Saya memahami konsepnya dan punya fondasi terdekat melalui RAG, API integration, serta sistem asynchronous, tetapi saya tetap membedakan hal yang sudah saya jalankan dari yang baru saya pelajari.

---

# Bukti Utama

- `vannndar/chatbot-uu-tni` dan `D:/ivan/Data Project/LawBot`
- `D:/ivan/Portofolio/server-infrastructure/learningwithus-embedding/compose.yaml`
- `vannndar/learningwithus`
- Hugging Face Space `vannndar/walet-inference`
- Hugging Face Space `vannndar/kurang_sks`
- `D:/ivan/Portofolio/Walet/walet-insightface`
- `D:/ivan/Data Project/PLN`
- `D:/ivan/Portofolio/Bayucaraka-UAV-Teknofest/freemission-AWS`

**Catatan:** Status ditentukan dari artefak yang ditemukan pada audit 25 September 2026. Tidak ditemukannya bukti berarti jangan mengklaim pengalaman tersebut, bukan berarti teknologinya sama sekali belum pernah dilihat.