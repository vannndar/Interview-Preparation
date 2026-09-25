# Glosarium Teknis + Contoh Code — untuk Interview AI Engineer
## Thariq Ivan Anendar | Disusun 18 September 2026

**Cara pakai file ini:**
Tiap entri punya tiga bagian: **apa itu** → **masalah apa yang dia selesaikan** → **contoh code** (kalau perlu code).

Beberapa istilah sengaja **tidak** dikasih code karena tidak ada yang bisa ditulis — itu konsep internal, bukan API. Sudah aku tandai `[KONSEP — tidak ada code]`. Kalau pewawancara tanya, jelaskan dengan angka dan alasan, bukan dengan code.

**Aturan saat ngomong:** istilah teknis tetap English, penjelasannya Bahasa Indonesia. Jangan diterjemahkan.

---

# BAGIAN A — LLM SERVING

## vLLM

**Apa itu:** Engine untuk menjalankan (serve) model LLM open-source sebagai **HTTP endpoint yang kompatibel dengan OpenAI API**. Jadi kamu jalankan model sendiri, tapi aplikasimu tetap memanggilnya dengan cara yang sama seperti memanggil GPT.

**Masalah yang diselesaikan:** Dua hal.

Pertama, **menjaga kerahasiaan data.** Kalau kamu pakai API pihak ketiga (OpenAI, Anthropic), prompt dan dokumenmu keluar dari infrastrukturmu. Untuk dokumen internal perusahaan atau data operasional, itu tidak boleh. vLLM memungkinkan model jalan di server sendiri, jadi data tidak pernah keluar.

Kedua, **biaya dan kontrol.** Untuk volume tinggi, menjalankan model sendiri lebih murah daripada bayar per token. Dan kamu bisa ganti model kapan saja tanpa mengubah aplikasi, karena endpoint-nya sama.

**Contoh menjalankan server:**

```bash
# Jalankan model sebagai server di port 8000
vllm serve NousResearch/Meta-Llama-3-8B-Instruct \
  --dtype auto \
  --api-key token-abc123
```

**Contoh memanggilnya — pakai client OpenAI yang sama:**

```python
from openai import OpenAI

client = OpenAI(
    base_url="http://localhost:8000/v1",   # ← arahkan ke server sendiri
    api_key="token-abc123",
)

completion = client.chat.completions.create(
    model="NousResearch/Meta-Llama-3-8B-Instruct",
    messages=[{"role": "user", "content": "Hello!"}],
)
print(completion.choices[0].message)
```

**Kenapa ini penting untuk jawaban interviewmu:** perhatikan bahwa `base_url`-nya bisa diganti kapan saja. Itu artinya **kode aplikasimu tidak perlu berubah** kalau pindah dari API pihak ketiga ke model self-hosted. Itu jawaban untuk pertanyaan "bagaimana kamu menjaga data tetap di dalam?"

**Yang kamu sudah punya (nyebut ini, jangan klaim vLLM):** kamu menjalankan **HuggingFace Text Embeddings Inference (TEI)** di Docker dengan GPU, image di-pin ke digest, model `BAAI/bge-m3`. Itu pola yang sama — model di dalam container, diakses lewat HTTP. Yang belum kamu lakukan: menjalankannya sebagai server multi-user dengan optimasi throughput.

---

## PagedAttention `[KONSEP — tidak ada code]`

**Apa itu:** Teknik manajemen memori GPU di vLLM. KV cache (cache yang menyimpan konteks percakapan selama generasi) dipotong jadi blok-blok kecil berukuran tetap, lalu blok itu dipetakan lewat tabel — mirip cara sistem operasi memetakan memori virtual ke halaman fisik.

**Masalah yang diselesaikan:** Tanpa ini, KV cache dialokasikan secara berurutan di memori GPU sesuai panjang maksimum yang mungkin. Kalau kamu reservasi untuk 4096 token tapi request-nya cuma pakai 200 token, sisa 3896 token itu terbuang — dan GPU tidak bisa dipakai request lain. Hasilnya: GPU kehabisan VRAM padahal kapasitasnya belum terpakai.

Dengan PagedAttention, memori dialokasikan sesuai yang benar-benar dipakai, dan blok yang sama bisa **dishare** antar request kalau prefix-nya identik (misalnya system prompt yang sama untuk semua user). Efeknya: **throughput jauh lebih tinggi pada VRAM yang sama**, kadang 2–4x.

---

## Continuous Batching `[KONSEP — tidak ada code]`

**Apa itu:** Menjalankan banyak request bersamaan di GPU, bukan satu per satu. Request yang sudah selesai langsung digantikan request baru di slot yang sama, tanpa menunggu batch sebelumnya selesai seluruhnya.

**Masalah yang diselesaikan:** **Static batching** mengumpulkan sekelompok request, memprosesnya, lalu mengembalikan semua hasilnya bersama. Kalau ada satu request panjang dan sembilan pendek, sembilan yang pendek itu menunggu yang panjang selesai — GPU idle di sisa waktunya.

Continuous batching menghilangkan penungguan itu. Request pendek bisa keluar dan digantikan yang baru, sementara yang panjang tetap berjalan.

**Cara ngomong:** "Static batching membuat GPU menunggu request terlama. Continuous batching mengisi slot yang kosong begitu satu request selesai, jadi GPU tidak idle. Itu yang saya belum pernah jalankan sendiri — saya baru sampai tahap satu model jalan di satu container."

---

## Quantization

**Apa itu:** Menurunkan presisi angka bobot model, biasanya dari 16-bit ke 4-bit. Ukuran model dan kebutuhan VRAM turun drastis dengan penurunan kualitas yang terbatas.

**Masalah yang diselesaikan:** Model 7B butuh sekitar 14 GB VRAM di 16-bit. GPU 8 GB tidak cukup. Dengan 4-bit, model itu jalan di sekitar 4 GB — jadi bisa jalan di hardware yang jauh lebih murah.

**Contoh code — ini yang kamu benar-benar pakai di LawBot:**

```python
from transformers import BitsAndBytesConfig

bnb_config = BitsAndBytesConfig(
    load_in_4bit=True,
    bnb_4bit_quant_type="nf4",              # NormalFloat4, presisi terbaik untuk LLM
    bnb_4bit_compute_dtype=torch.float16,    # komputasi tetap 16-bit
    bnb_4bit_use_double_quant=True,          # quantize konstanta quantization-nya juga
)

model = AutoModelForCausalLM.from_pretrained(
    "google/gemma-7b-it",
    quantization_config=bnb_config,
    device_map="auto",
)
```

**Poin untuk interview:** perhatikan `compute_dtype=float16`. Bobotnya 4-bit, tapi komputasinya tetap 16-bit. Kalau tidak set ini, hasilnya lebih cepat tapi kualitasnya jatuh lebih banyak. Detail kecil seperti ini yang menunjukkan kamu benar-benar menjalankannya, bukan cuma baca.

---

## LoRA / QLoRA

**Apa itu:** **LoRA** membekukan bobot model asli, lalu melatih matriks kecil berukuran rendah (low-rank) di beberapa layer. Hasilnya file adapter berukuran puluhan MB, bukan puluhan GB. **QLoRA** adalah LoRA yang base model-nya di-quantize 4-bit — inilah yang membuat fine-tune 7B bisa muat di satu GPU.

**Masalah yang diselesaikan:** Fine-tune penuh model 7B butuh GPU mahal dan menghasilkan file yang tidak praktis dibagikan. QLoRA membuatnya bisa dikerjakan mahasiswa dengan satu GPU.

**Contoh code:**

```python
from peft import LoraConfig, get_peft_model

lora_config = LoraConfig(
    r=16,                      # rank; makin besar makin banyak kapasitas, makin berat
    lora_alpha=32,             # scaling factor
    target_modules=["q_proj", "k_proj", "v_proj"],  # layer attention yang dilatih
    lora_dropout=0.05,
    bias="none",
    task_type="CAUSAL_LM",
)

model = get_peft_model(model, lora_config)
model.print_trainable_parameters()
# trainable params: ~0.12% of total  ← angka ini yang kamu pakai di laporan
```

**Angka kamu:** QLoRA di LawBot melatih **~0.12% parameter**, 5 epoch, batch 32, 360 steps, 2332 sampel. Adapter hasilnya sekitar 38 MB. Sebut angka-angka ini — itu bukti kamu menjalankannya.

---

# BAGIAN B — VECTOR DATABASE

## Vector database

**Apa itu:** Database yang menyimpan vektor (hasil embedding) dan bisa mencari **vektor yang paling mirip** dengan vektor query. Berbeda dari database biasa yang mencari berdasarkan kesamaan nilai, vector DB mencari berdasarkan kedekatan makna.

**Masalah yang diselesaikan:** Kalau kamu punya 100.000 potong dokumen, mencari mana yang relevan dengan pertanyaan user tidak bisa dilakukan dengan `LIKE '%kata%'`. User tidak akan mengetik kata yang sama dengan dokumennya. Vector DB menyelesaikan ini dengan menghitung jarak antar vektor.

**Empat pilihan yang perlu kamu bisa bedakan:**

| Tool | Bentuk | Cocok untuk | Kamu sudah pakai? |
|---|---|---|---|
| **FAISS** | Library, in-process | Prototyping, eksperimen | ✅ LawBot |
| **pgvector** | Extension di Postgres | Production, butuh join dengan data relasional | ✅ learningwithus |
| **Chroma** | Embedded / server, ringan | Prototyping cepat, dataset kecil–menengah | ❌ |
| **Milvus** | Server terdistribusi | Skala besar, miliaran vektor | ❌ |
| **Weaviate** | Server, dengan fitur hybrid & module | Production dengan pencarian hybrid bawaan | ❌ |

## Milvus

**Apa itu:** Vector database open-source untuk skala besar. Bisa jalan embedded (Milvus Lite, di dalam aplikasi Python) untuk demo, atau di Docker/Kubernetes untuk produksi.

**Masalah yang diselesaikan:** Ketika jumlah vektor sudah melewati kapasitas satu database Postgres, atau ketika butuh sharding dan replikasi. pgvector di Postgres masih satu node — kalau index-nya sudah miliaran vektor, kamu butuh sistem yang dirancang terdistribusi dari awal.

**Contoh code — Milvus Lite, bisa kamu jalankan sekarang untuk belajar:**

```python
from pymilvus import MilvusClient

# Milvus Lite: cukup satu nama file, tidak butuh server
client = MilvusClient("milvus_demo.db")

# Collection = tabel di SQL
client.create_collection(
    collection_name="demo_collection",
    dimension=768,          # harus sama dengan dimensi embedding-mu
)
# Default: primary key "id", field vektor "vector", metric COSINE
```

```python
# Insert: teks + vektor + metadata, bisa jadi satu
client.insert(
    collection_name="demo_collection",
    data=[
        {"id": 0, "vector": embedding_0, "text": "Pasal 1 UU TNI", "category": "pasal"},
        {"id": 1, "vector": embedding_1, "text": "Pasal 2 UU TNI", "category": "pasal"},
    ],
)

# Search top-k dengan filter metadata
results = client.search(
    collection_name="demo_collection",
    data=[query_embedding],
    limit=2,
    filter='category == "pasal"',        # ← ini yang pgvector juga bisa, tapi Milvus lebih skalabel
    output_fields=["text"],
)
```

**Jawaban interview kalau ditanya Milvus:** "Belum pernah saya pakai. Yang saya sudah jalankan di production adalah pgvector di Postgres — 1024 dimensi, dengan migrasi dari 384 dimensi yang punya cutover gate dan runbook rollback. Milvus saya pahami sebagai pilihan ketika index-nya sudah melewati kapasitas satu Postgres, atau butuh sharding dan replikasi. Saya belum pernah sampai ke titik skalanya."

**Poin penting:** perhatikan `dimension=768` di Milvus. **Dimensi harus sama dengan model embedding yang kamu pakai.** Kalau embedding-mu 1024 tapi collection-nya 768, insert-nya error. Ini kelas masalah yang sama dengan enforcement 1024 dimensi yang kamu pasang di worker RAG-mu — sebut itu, itu bukti kamu paham konsekuensinya.

## Chroma

**Apa itu:** Vector database yang paling ringan untuk mulai. Bisa jalan sepenuhnya di dalam aplikasi Python, tanpa Docker, tanpa server.

**Masalah yang diselesaikan:** Friksi untuk mulai. FAISS sudah ringan, tapi Chroma lebih tinggi levelnya — dia menyimpan teks dan metadata sekaligus, sedangkan FAISS hanya vektor dan ID. Jadi kamu tidak perlu bikin database terpisah untuk menyimpan isi dokumennya.

**Contoh code:**

```python
import chromadb

client = chromadb.Client()                                  # in-memory
# client = chromadb.PersistentClient(path="./chroma_db")    # atau persist ke disk

collection = client.create_collection(name="uu_tni")

# Chroma bisa generate embedding-nya sendiri, atau kamu kasih vektor manual
collection.add(
    ids=["pasal-1", "pasal-2"],
    documents=["Pasal 1: TNI terdiri atas...", "Pasal 2: Tugas TNI adalah..."],
    metadatas=[{"bab": 1}, {"bab": 1}],
)

results = collection.query(
    query_texts=["apa tugas TNI"],
    n_results=2,
)
```

**Perbedaan kunci dari FAISS:** `query_texts=` — Chroma bisa terima teks langsung dan meng-embed sendiri. FAISS harus kamu embed dulu di luar. Trade-off-nya: Chroma lebih mudah, tapi kamu kurang kontrol atas model embedding-nya.

## Weaviate

**Apa itu:** Vector database dengan **pencarian hybrid bawaan** — menggabungkan pencarian vektor dan keyword (BM25) dalam satu query, plus modul untuk generate embedding sendiri.

**Masalah yang diselesaikan:** Di FAISS, hybrid search harus kamu rakit sendiri — jalankan BM25 terpisah, jalankan vector search terpisah, lalu gabungkan skornya secara manual. Weaviate menyediakan itu dalam satu perintah.

**Contoh code (bentuk query hybrid-nya):**

```python
import weaviate
from weaviate.classes.query import HybridFusion

client = weaviate.connect_to_local()
collection = client.collections.get("UU_TNI")

response = collection.query.hybrid(
    query="tugas TNI dalam operasi militer",
    alpha=0.5,                                # 0 = keyword only, 1 = vector only
    fusion_type=HybridFusion.RELATIVE_SCORE,  # cara menggabungkan skor
    limit=3,
)

for obj in response.objects:
    print(obj.properties)
```

**Kaitan dengan kerjamu:** `alpha=0.5` di Weaviate itu adalah **persis** penggabungan BM25 + semantic yang kamu rakit manual di LawBot. Kalau ditanya soal hybrid retrieval, kamu bisa bilang: "saya sudah mengerjakan penggabungan BM25 dengan semantic search secara manual di proyek saya, jadi saya paham trade-off-nya. Yang belum saya pakai adalah tool yang menyediakan itu bawaan seperti Weaviate."

**Kalau ditanya lebih dalam — kenapa hybrid lebih baik dari vector saja:** dokumen hukum sering dikutip dengan istilah persis ("Pasal 34", "operasi militer selain perang"). Vector search bisa melewatkan itu karena dia mencari makna, bukan kata. BM25 menangkap kecocokan kata persis. Digabung, kamu dapat dua-duanya.

## Index vektor: HNSW vs IVF `[KONSEP — tidak ada code, tapi ada parameter]`

**Apa itu:** Dua cara mengorganisir vektor supaya pencarian tidak perlu membandingkan query dengan **semua** vektor satu per satu.

- **Flat / brute-force** — bandingkan dengan semua. Paling akurat, paling lambat. FAISS default untuk dataset kecil.
- **IVF (Inverted File)** — vektor dikelompokkan jadi klaster. Query hanya mencari di klaster terdekat. Cepat, tapi bisa melewatkan hasil yang ada di klaster lain.
- **HNSW (Hierarchical Navigable Small World)** — graf berlapis. Query menelusuri graf dari lapisan kasar ke halus. Ini yang paling umum dipakai sekarang.

**Masalah yang diselesaikan:** Pada 1 juta vektor, brute-force berarti 1 juta perbandingan per query. Tidak bisa dipakai untuk aplikasi real-time.

**Trade-off yang wajib kamu tahu:** semua index ini **approximate** — mereka bisa melewatkan hasil yang sebenarnya relevan. Parameter seperti `ef_search` (HNSW) atau `nprobe` (IVF) mengatur seberapa keras pencariannya: makin tinggi, makin akurat, makin lambat. Ini disebut **recall vs latency trade-off**.

**Kenapa penting untuk interview:** kalau ditanya "kok hasil retrieval saya kadang meleset padahal dokumennya ada", salah satu jawabannya adalah parameter index terlalu agresif. Ini pertanyaan bagus yang bisa kamu ajukan balik ke mereka.

---

# BAGIAN C — MCP & AGENTS

## MCP (Model Context Protocol)

**Apa itu:** Protokol terbuka yang menstandarkan cara aplikasi LLM terhubung ke sumber data dan fungsi eksternal. Analoginya (tapi bukan analogi — ini deskripsi teknis): **MCP itu seperti USB-C untuk LLM.** Sebelum MCP, setiap tool harus diintegrasikan satu per satu ke setiap aplikasi LLM. Dengan MCP, kamu bikin satu server, dan semua host yang mendukung MCP bisa memakainya.

**Masalah yang diselesaikan:** **Integrasi N×M.** Kalau kamu punya 5 aplikasi LLM dan 10 sumber data, tanpa standar kamu butuh 50 integrasi. Dengan MCP, kamu bikin 10 server dan 5 client — 15 pekerjaan, dan tiap server bisa dipakai semua client.

Untuk PSN ini relevan langsung: tim mereka punya **MCP server untuk monitoring telemetri satelit**. Kalau kamu paham MCP, kamu paham satu dari empat pilar kerja mereka.

**Tiga hal yang bisa diekspos MCP server:**

1. **Tools** — fungsi yang bisa dipanggil LLM (dengan persetujuan user). Contoh: query telemetri, hitung agregat, render chart.
2. **Resources** — data yang bisa dibaca client, seperti isi file atau respons API.
3. **Prompts** — template yang sudah disiapkan untuk tugas tertentu.

## MCP server — contoh membuatnya

**Ini yang kamu minta.** SDK Python MCP versi 2 (spesifikasi 2026-07-28). Perlu Python 3.10+.

**Install:**

```bash
uv add "mcp[cli]"
# atau: pip install "mcp[cli]"
```

**Server minimal — 15 baris:**

```python
# server.py
from mcp.server import MCPServer

mcp = MCPServer("Demo")

@mcp.tool()
def add(a: int, b: int) -> int:
    """Add two numbers."""
    return a + b


if __name__ == "__main__":
    mcp.run()
```

**Perhatikan tiga hal:**

1. `@mcp.tool()` — decorator ini yang mengekspos fungsi ke LLM.
2. **Type hint `(a: int, b: int) -> int` bukan hiasan.** SDK memakai ini untuk menghasilkan JSON Schema yang dikirim ke LLM. LLM membaca schema itu untuk tahu parameter apa yang harus dikirim. **Type hint salah = LLM kirim parameter salah.**
3. **Docstring `"""Add two numbers."""` juga bukan komentar.** Itu yang dibaca LLM untuk memutuskan kapan tool ini dipakai. Docstring kabur = tool tidak pernah dipanggil, atau dipanggil di saat yang salah.

**Server untuk kasus nyata — monitoring telemetri satelit:**

```python
# telemetry_server.py
import logging
from mcp.server import MCPServer

logger = logging.getLogger(__name__)   # WAJIB: jangan pakai print()

mcp = MCPServer("Satellite Telemetry")


@mcp.tool()
def get_channel_history(channel_id: str, hours: int = 24) -> dict:
    """Ambil riwayat telemetri satu kanal dalam N jam terakhir.

    Args:
        channel_id: ID kanal telemetri, contoh "KU-BAND-03".
        hours: Rentang waktu ke belakang dalam jam. Default 24.
    """
    logger.info("query channel=%s hours=%d", channel_id, hours)

    rows = db.query(
        "SELECT ts, value FROM telemetry WHERE channel = %s "
        "AND ts > NOW() - INTERVAL '%s hours' ORDER BY ts",
        (channel_id, hours),
    )
    if not rows:
        return {"channel": channel_id, "points": [], "note": "tidak ada data pada rentang ini"}

    return {
        "channel": channel_id,
        "points": [{"ts": r.ts.isoformat(), "value": r.value} for r in rows],
        "count": len(rows),
    }


@mcp.tool()
def detect_anomaly(channel_id: str, hours: int = 24, z_threshold: float = 3.0) -> dict:
    """Deteksi anomali pada satu kanal menggunakan ambang z-score.

    Args:
        channel_id: ID kanal telemetri.
        hours: Rentang waktu yang diperiksa.
        z_threshold: Ambang z-score; makin kecil makin sensitif.
    """
    history = get_channel_history(channel_id, hours)
    values = [p["value"] for p in history["points"]]
    if len(values) < 30:
        return {"error": "data kurang dari 30 titik, statistik tidak stabil"}

    mean = sum(values) / len(values)
    std = (sum((v - mean) ** 2 for v in values) / len(values)) ** 0.5
    flagged = [
        p for p in history["points"]
        if abs(p["value"] - mean) / std > z_threshold
    ]

    return {
        "channel": channel_id,
        "baseline_mean": round(mean, 3),
        "baseline_std": round(std, 3),
        "anomalies": flagged,
        "count": len(flagged),
    }


if __name__ == "__main__":
    mcp.run()
```

**Jalankan dan test:**

```bash
uv run mcp dev telemetry_server.py    # buka MCP Inspector di browser
```

**Test tanpa server, tanpa port, tanpa subprocess** — SDK v2 bisa connect langsung ke objek server:

```python
# test_telemetry.py
import pytest
from mcp import Client

from telemetry_server import mcp


@pytest.mark.anyio
async def test_detect_anomaly_returns_structure() -> None:
    async with Client(mcp) as client:
        result = await client.call_tool(
            "detect_anomaly",
            {"channel_id": "KU-BAND-03", "hours": 24},
        )
        assert "anomalies" in result.structured_content
```

**Ini bagian yang paling berguna untuk jawaban interviewmu:** kamu bisa bilang "MCP server itu bisa diuji seperti kode biasa — SDK-nya menyediakan client in-memory, jadi tidak perlu subprocess atau port. Itu artinya kualitasnya bisa dijaga dengan test yang sama seperti service lain." Itu jawaban orang yang berpikir soal maintainability, bukan cuma soal demo.

## MCP — jebakan yang wajib kamu tahu

**Kalau transport-nya STDIO, JANGAN pernah pakai `print()`.**

```python
# ❌ SALAH — akan merusak server
print("Processing request")

# ✅ BENAR — logging ke stderr
import logging
logger = logging.getLogger(__name__)
logger.info("Processing request")
```

**Kenapa:** pada transport STDIO, pesan JSON-RPC lewat stdout. `print()` juga menulis ke stdout. Jadi teks debug-mu **tercampur ke dalam protokol** dan pesan JSON-nya rusak. Server-nya mati, dan pesan error-nya tidak jelas.

Kalau transport-nya HTTP, `print()` tidak masalah karena responsnya lewat HTTP, bukan stdout.

**Ini pertanyaan lanjutan yang bagus kamu antisipasi.** Kalau pewawancara sudah pernah bikin MCP server, dia kemungkinan pernah kena ini. Kalau kamu bisa sebut lebih dulu, itu tanda kamu benar-benar membaca dokumentasinya.

## MCP transport: stdio vs Streamable HTTP `[KONSEP + 1 baris config]`

**Apa itu:**
- **stdio** — server jalan sebagai subprocess, komunikasi lewat stdin/stdout. Cocok untuk tool lokal di mesinmu.
- **Streamable HTTP** — server jalan sebagai service jaringan. Cocok untuk server yang dipakai bersama banyak client.

**Contoh konfigurasi (stdio) — bentuk ini familiar karena Hermes pakai pola yang sama:**

```yaml
mcp_servers:
  telemetry:
    command: uv
    args:
      - run
      - --with
      - "mcp[cli]"
      - mcp
      - run
      - /path/to/telemetry_server.py
```

**Pilihan untuk PSN:** kalau MCP server-nya harus diakses banyak operator dari berbagai tempat, HTTP lebih masuk akal. Kalau cuma untuk satu aplikasi di satu server, stdio lebih sederhana. Jawaban interview: "tergantung apakah server-nya dipakai satu aplikasi atau banyak. stdio untuk lokal dan satu consumer, HTTP kalau harus dishare."

## Tool calling `[KONSEP + 1 contoh bentuk]`

**Apa itu:** Mekanisme di mana LLM memutuskan memanggil fungsi, bukan menjawab langsung dari pengetahuannya.

**Alur lengkapnya:**

```text
1. Aplikasi kirim ke LLM: pertanyaan user + daftar tool yang tersedia (nama, deskripsi, schema parameter)
2. LLM balas: bukan teks, tapi permintaan panggil tool  →  {"tool": "detect_anomaly", "args": {"channel_id": "KU-BAND-03"}}
3. Aplikasi yang menjalankan fungsinya (bukan LLM-nya)
4. Hasil fungsi dikirim balik ke LLM sebagai konteks baru
5. LLM menyusun jawaban akhir, atau memanggil tool lain lagi
```

**Masalah yang diselesaikan:** LLM tidak bisa mengakses data real-time dan tidak bisa berhitung dengan andal. Dengan tool calling, dia bisa mendelegasikan ke kode yang benar-benar bisa.

**Poin yang sering salah dipahami:** **LLM tidak menjalankan fungsinya.** LLM hanya mengatakan "tolong jalankan fungsi ini dengan argumen ini". Aplikasimu yang menjalankan. Ini penting untuk keamanan — kamu bisa menolak, validasi, atau minta persetujuan user dulu.

**Kenapa ini penting untuk kerjamu:** di RAG, kamu punya pipeline tetap — ambil dokumen, rerank, generate. Di agent, langkahnya ditentukan model. Itu perbedaan intinya, dan itu yang harus kamu sampaikan dengan jelas.

## LangGraph `[KONSEP + contoh struktur]`

**Apa itu:** Framework untuk membangun alur kerja agent sebagai **graf** — node adalah langkah, edge adalah transisi antar langkah, dan state mengalir di antaranya. Bedanya dari LangChain biasa: LangChain menyusun urutan linear, LangGraph mengizinkan percabangan dan perulangan.

**Masalah yang diselesaikan:** Agent butuh loop dan kondisi. "Kalau tool mengembalikan data kosong, coba tool lain." "Kalau hasilnya belum cukup, ulangi retrieval." Ini sulit ditulis sebagai urutan linear, dan mudah ditulis sebagai graf dengan edge bersyarat.

**Contoh struktur — bukan kode lengkap, ini bentuk alurnya:**

```python
from langgraph.graph import StateGraph, END
from typing import TypedDict

class MonitoringState(TypedDict):
    question: str
    channel_id: str | None
    history: list
    anomalies: list
    answer: str


def extract_channel(state): ...
def fetch_history(state): ...
def analyze(state): ...
def should_refetch(state) -> str:
    # edge bersyarat: kalau data kurang, kembali ambil lagi
    return "fetch_history" if len(state["history"]) < 30 else "analyze"

builder = StateGraph(MonitoringState)
builder.add_node("extract", extract_channel)
builder.add_node("fetch_history", fetch_history)
builder.add_node("analyze", analyze)

builder.set_entry_point("extract")
builder.add_edge("extract", "fetch_history")
builder.add_conditional_edges("fetch_history", should_refetch)
builder.add_edge("analyze", END)

graph = builder.compile()
result = graph.invoke({"question": "Ada anomali di KU-BAND-03?"})
```

**Yang harus kamu sampaikan:** `add_conditional_edges` itu inti perbedaannya. Itu yang bikin agent bisa memutuskan ulang, bukan cuma mengikuti urutan.

**Honest gap:** kamu belum pernah memakai LangGraph. Yang kamu sudah bangun adalah alur multi-tahap linear (retrieve → rerank → generate) plus worker queue dengan retry dan dead-letter. Jadi bagian **reliability dan state management**-nya sudah kamu pegang; yang belum adalah percabangan yang ditentukan model.

---

# BAGIAN D — RAG INTERNALS (pakai kodemu sendiri)

## Chunking

**Apa itu:** Memotong dokumen panjang jadi potongan kecil sebelum di-embed.

**Masalah yang diselesaikan:** Model embedding punya batas panjang input. Dan kalaupun tidak, satu embedding untuk dokumen 36 halaman kehilangan detail — vektornya jadi rata-rata dari terlalu banyak makna.

**Contoh code — ini kodemu di `setup_vectorstore.py` LawBot:**

```python
from langchain.text_splitter import RecursiveCharacterTextSplitter

text_splitter = RecursiveCharacterTextSplitter(
    chunk_size=1000,
    chunk_overlap=100,
)
docs = text_splitter.split_documents(documents)
```

**Kenapa `RecursiveCharacterTextSplitter` dan bukan pemotongan biasa:** dia memotong secara hierarkis — coba pisah di paragraf dulu; kalau masih terlalu panjang, coba di kalimat; baru di karakter. Jadi batas chunk-nya tetap di tempat yang natural, bukan di tengah kata.

**Kenapa 1000 dan 100:** 1000 karakter cukup untuk memuat satu pasal utuh tanpa jadi terlalu umum. Overlap 100 memastikan kalimat yang jatuh tepat di batas chunk tidak hilang dari kedua sisi.

**Pertanyaan lanjutan yang siap kamu jawab:** "Kalau dokumennya berbeda jenis, apakah angkanya tetap?" Jawaban: tidak. Untuk dokumen hukum yang pasalnya panjang, 1000 masuk akal. Untuk chat log atau dokumentasi API yang pendek-pendek, chunk-nya bisa lebih kecil. Dan cara memverifikasi angkanya bukan dengan merasa, tapi dengan mengevaluasi hasil retrieval-nya.

## Embedding

**Apa itu:** Mengubah teks jadi vektor angka yang merepresentasikan maknanya. Teks dengan makna mirip menghasilkan vektor yang berdekatan.

**Contoh code — kodemu:**

```python
from langchain_huggingface import HuggingFaceEmbeddings

embeddings = HuggingFaceEmbeddings(
    model_name="sentence-transformers/paraphrase-multilingual-MiniLM-L12-v2",
    model_kwargs={'device': 'cpu'},
)
```

**Kenapa `paraphrase-multilingual`:** dokumennya Bahasa Indonesia. Model embedding yang hanya dilatih Bahasa Inggris akan menghasilkan vektor yang buruk untuk teks Indonesia — makna yang sebenarnya mirip jadi jauh di ruang vektor.

**Masalah yang diselesaikan:** kamu tidak bisa mencari "mirip" di ruang teks, tapi bisa mencari "dekat" di ruang vektor.

## FAISS

**Apa itu:** Library similarity search dari Meta. Menyimpan vektor di memori dan mencari yang terdekat.

**Masalah yang diselesaikan:** perbandingan brute-force terhadap semua vektor terlalu lambat. FAISS menyediakan index yang jauh lebih cepat dengan memori yang terkendali.

**Contoh code — kodemu di `rag_handler.py`:**

```python
from langchain_community.vectorstores import FAISS

db = FAISS.load_local(
    VECTORSTORE_PATH,
    embeddings,
    allow_dangerous_deserialization=True,
)
return db.as_retriever(search_kwargs={'k': RETRIEVER_SEARCH_K})   # k = 2
```

**Catatan penting soal `allow_dangerous_deserialization=True`:** flag ini wajib di versi LangChain yang lebih baru, karena `load_local` memakai pickle. **Kalau file vectorstore-nya datang dari sumber yang tidak dipercaya, ini celah keamanan** — pickle bisa menjalankan kode. Di proyekmu file-nya kamu bikin sendiri, jadi aman. Tapi kalau ditanya soal security, ini jawaban yang menunjukkan kamu sadar apa yang kamu tulis.

## Top-k

**Apa itu:** Berapa banyak dokumen teratas yang diambil dari retrieval untuk dikirim ke LLM sebagai konteks.

**Contoh code:** itu `k=2` di `RETRIEVER_SEARCH_K` — di file `config.py` kamu.

**Trade-off:**
- **k terlalu kecil** → konteks kurang, LLM tidak punya bahan untuk menjawab benar
- **k terlalu besar** → konteks penuh dokumen tidak relevan, LLM terganggu, dan biaya token naik

**Kenapa kamu pilih k=2:** dokumen UU TNI pembahasannya terpusat per pasal. Dua chunk teratas biasanya sudah memuat pasal yang ditanya. Kalau k terlalu besar, chunk dari pasal lain masuk dan model mulai mencampur.

**Kalau ditanya "kok cuma 2, tidak kekurangan?":** jawabannya adalah ini nilai yang kamu validasi dari hasil evaluasi, bukan angka default. Dan kalau ternyata retrieval-nya sering meleset, **yang diperbaiki dulu adalah retrieval quality-nya, bukan menaikkan k** — menaikkan k hanya menambah konteks sampah.

## Hybrid retrieval (BM25 + semantic)

**Apa itu:** Menggabungkan hasil pencarian keyword (BM25) dengan pencarian makna (semantic), lalu menggabungkan skornya.

**Masalah yang diselesaikan:** semantic search mencari makna, jadi dia bisa melewatkan istilah persis. Di dokumen hukum, user sering mengutip istilah persis — "Pasal 34", "operasi militer selain perang". BM25 menangkap itu, semantic search bisa melewatkannya. Digabung, kamu dapat dua-duanya.

**Contoh code:**

```python
from rank_bm25 import BM25Okapi

# BM25: bangun index dari corpus
tokenized_corpus = [doc.page_content.split() for doc in docs]
bm25 = BM25Okapi(tokenized_corpus)

def hybrid_retrieve(query: str, k: int = 2):
    # 1. keyword: BM25
    bm25_scores = bm25.get_scores(query.split())
    bm25_top = sorted(range(len(bm25_scores)),
                      key=lambda i: bm25_scores[i], reverse=True)[:k*2]

    # 2. semantic: FAISS
    semantic_docs = db.similarity_search(query, k=k*2)

    # 3. gabungkan skor (reciprocal rank fusion)
    combined = {}
    for rank, idx in enumerate(bm25_top):
        combined[idx] = combined.get(idx, 0) + 1 / (60 + rank + 1)
    for rank, doc in enumerate(semantic_docs):
        key = doc.page_content
        combined[key] = combined.get(key, 0) + 1 / (60 + rank + 1)

    return sorted(combined.items(), key=lambda x: x[1], reverse=True)[:k]
```

**Angka 60 di rumus itu:** itu konstanta standard Reciprocal Rank Fusion. Fungsinya melunakkan pengaruh peringkat teratas supaya satu sumber tidak mendominasi hanya karena dia memberi skor lebih besar. Skala skor BM25 dan cosine tidak sama — RRF menyatukannya berdasarkan peringkat, bukan nilai mentah.

## Reranking / Cross-encoder

**Apa itu:** Tahap kedua yang menyusun ulang hasil retrieval memakai model yang lebih akurat.

**Masalah yang diselesaikan:** retrieval tahap pertama cepat tapi hanya kasar akuratnya — dia membandingkan query dan dokumen **secara terpisah** (bi-encoder). Cross-encoder menilai query dan dokumen **bersama-sama** dalam satu input, jadi jauh lebih akurat tapi jauh lebih lambat. Karena lambat, dia tidak bisa dipakai untuk menyaring jutaan dokumen — jadi dipakai untuk menyusun ulang 10–50 kandidat teratas.

**Contoh code:**

```python
from sentence_transformers import CrossEncoder

reranker = CrossEncoder("cross-encoder/ms-marco-MiniLM-L-6-v2")

def rerank(query: str, candidates: list, top_n: int = 2):
    pairs = [(query, doc.page_content) for doc in candidates]
    scores = reranker.predict(pairs)
    ranked = sorted(zip(candidates, scores), key=lambda x: x[1], reverse=True)
    return [doc for doc, _ in ranked[:top_n]]
```

**Pola yang harus kamu sebut:** **retrieve lebar → rerank sempit.** Ambil 20 kandidat dengan retrieval hybrid, lalu cross-encoder pilih 2 terbaik. Itu pola standar, dan kamu sudah menerapkannya di LawBot.

---

# BAGIAN E — DEPLOYMENT & KERAHASIAAN DATA

## Menjaga kerahasiaan data `[KONSEP + code di beberapa titik]`

**Ini pertanyaan yang pasti muncul di PSN** — tim mereka punya LLM untuk dokumen confidential perusahaan, dan satelit itu infrastruktur nasional.

**Empat lapisan yang harus kamu sampaikan:**

### Lapisan 1 — Di mana inferensinya jalan

Kalau dokumennya tidak boleh keluar, modelnya tidak boleh jalan di API pihak ketiga. Ini yang vLLM selesaikan: model jalan di server sendiri, endpoint-nya tetap OpenAI-compatible.

```python
# Ganti base_url dari api.openai.com ke server sendiri — kode lain tidak berubah
client = OpenAI(
    base_url="http://localhost:8000/v1",   # ← model jalan di infrastruktur sendiri
    api_key="token-abc123",
)
```

### Lapisan 2 — Aksesnya difilter di level database, bukan aplikasi

```sql
-- Row Level Security: Postgres memfilter otomatis, tidak bisa dilewati dari aplikasi
ALTER TABLE documents ENABLE ROW LEVEL SECURITY;

CREATE POLICY user_own_documents ON documents
  FOR SELECT
  USING (owner_id = auth.uid());
```

**Masalah yang diselesaikan:** kalau filtering-nya di kode aplikasi, satu endpoint yang lupa memfilter berarti seluruh dokumen bocor. Kalau di level database, filternya jalan untuk **semua** query — termasuk yang lupa.

Ini yang kamu pakai di learningwithus — RLS aktif di Supabase. Sebut itu.

### Lapisan 3 — Logging

Kalau prompt dan output dilog apa adanya, dokumen confidential bisa bocor lewat log meskipun modelnya jalan lokal.

```python
import logging

logger = logging.getLogger(__name__)

# ❌ JANGAN
logger.info("query=%s context=%s", question, full_context)

# ✅ Log metadata-nya, bukan isinya
logger.info("query_len=%d docs=%d doc_ids=%s", len(question), len(docs), doc_ids)
```

### Lapisan 4 — Jangan publish port container ke publik

```yaml
services:
  llm:
    ports:
      - "127.0.0.1:8000:8000"   # ← hanya localhost, bukan 0.0.0.0
```

**Kenapa:** `8000:8000` mengikat ke semua interface, jadi bisa diakses dari jaringan. `127.0.0.1:8000:8000` hanya bisa diakses dari dalam host itu. Akses dari luar lewat reverse proxy yang punya autentikasi.

**Ini pola yang kamu terapkan** — TEI embedding service-mu bind ke `127.0.0.1:8000:80`, dan aksesnya lewat Cloudflare Tunnel → Nginx.

---

## Docker — hal-hal yang menunjukkan kamu benar-benar pernah deploy

**Pin versi immutable, bukan tag `latest`:**

```yaml
# ❌ versi bisa berubah kapan saja saat restart
image: ghcr.io/huggingface/text-embeddings-inference:latest

# ✅ di-pin ke digest — isinya tidak bisa berubah
image: ghcr.io/huggingface/text-embeddings-inference:86-1.9@sha256:a7d82dfe...
```

**Masalah yang diselesaikan:** tag `latest` bisa menunjuk ke versi berbeda minggu depan. Container yang di-restart tiba-tiba memakai kode baru yang belum kamu uji. Digest SHA mengunci isinya.

**Ini persis yang kamu lakukan** di `compose.yaml` learningwithus-embedding. Kalau ditanya, kamu bisa tunjukkan barisnya.

**Healthcheck dengan `start_period`:**

```yaml
healthcheck:
  test: ["CMD-SHELL", "curl -f http://localhost:80/health || exit 1"]
  interval: 30s
  timeout: 10s
  retries: 3
  start_period: 120s     # ← model butuh 2 menit untuk load
```

**Kenapa `start_period` penting:** tanpa ini, healthcheck mulai menghitung gagal sejak detik pertama. Container yang sedang loading model 7B dianggap tidak sehat dan di-restart terus-menerus dalam loop. `start_period` memberi waktu tunggu sebelum penilaian dimulai.

**Hardening container:**

```yaml
    cap_drop:
      - ALL                  # buang semua Linux capability
    security_opt:
      - no-new-privileges:true
    tmpfs:
      - /tmp:size=256m,noexec,nosuid
    mem_limit: 8g
    pids_limit: 512
```

**Kenapa:** container default punya lebih banyak hak dari yang dibutuhkan. `cap_drop: ALL` membuang semuanya; service yang cuma perlu bicara HTTP tidak butuh `NET_ADMIN` atau `SYS_ADMIN`. Kalau container-nya nanti dieksploitasi, penyerang mendapat sesedikit mungkin.

**Catatan jujur:** hardening ini kamu pelajari dan terapkan di service embedding. Kamu belum pernah menghadapi insiden keamanan, jadi jangan klaim pengalaman itu.

**Pola worker terpisah dari API:**

```text
API container   → menerima request, cepat, tidak boleh diblokir kerjaan berat
Worker container → memproses job dari queue, boleh lambat, bisa di-restart independen
```

**Masalah yang diselesaikan:** kalau embedding dijalankan di dalam request handler, satu dokumen besar bisa memblokir seluruh API. Dengan worker terpisah, API tetap responsif dan worker bisa di-scale sendiri.

---

# BAGIAN F — EVALUATION

## Hallucination

**Apa itu:** Model menghasilkan informasi yang salah dengan nada percaya diri.

**Masalah yang diselesaikan:** ini mode kegagalan yang **tidak terlihat** — jawabannya mengalir dan meyakinkan, jadi user percaya. Di domain hukum atau operasional, ini berbahaya.

**Cara mengukurnya:** bandingkan klaim dalam jawaban dengan dokumen sumbernya. Di LawBot, jawaban dinilai apakah dasar hukumnya benar-benar ada di UU yang direferensikan.

**Angka kamu:** baseline semua 6 model halusinasi **di atas 60%**. Setelah fine-tune + RAG, Llama 3.1 turun ke **46,74%**. Sebut angka ini — itu angka yang paling kuat di portofoliomu.

## Grounding

**Apa itu:** Memaksa jawaban bersandar pada konteks yang di-retrieve, bukan pada ingatan model.

**Cara menerapkannya:**

```python
prompt = f"""Jawab HANYA berdasarkan konteks di bawah ini.
Kalau jawabannya tidak ada di konteks, katakan "Tidak ditemukan dalam dokumen".

Konteks:
{context}

Pertanyaan: {question}
"""
```

**Kalau ditanya "kok grounding penting padahal modelnya sudah fine-tune?":** karena fine-tune mengubah **cara** model menjawab, bukan **pengetahuannya tentang fakta spesifik**. Di proyekmu terbukti: fine-tune menaikkan ROUGE (modelnya bicara lebih benar) tapi halusinasi baru turun signifikan setelah RAG ditambahkan.

## ROUGE

**Apa itu:** Metrik yang mengukur tumpang tindih n-gram antara jawaban model dan jawaban referensi.

**Masalah yang diselesaikan:** cara otomatis dan murah untuk mengukur kemiripan teks, tanpa perlu manusia menilai tiap jawaban.

**Kelemahannya yang wajib kamu sebut:** ROUGE menghukum jawaban yang benar tapi disusun dengan kata berbeda. Di proyekmu ini terjadi di tahap akhir — setelah RAG ditambahkan, ROUGE **turun** padahal akurasi faktual **naik**.

**Ini pelajaran paling berharga dari proyekmu.** Kalau ditanya "kenapa ROUGE turun?", jawab: RAG membuat model mengutip dokumen ketimbang menyusun kalimatnya sendiri, jadi kata-katanya berbeda dari referensi. ROUGE menganggap itu penurunan, padahal secara faktual lebih baik. **Metrik otomatis bukan kebenaran** — itu yang membuat kamu harus membaca hasilnya, bukan cuma melihat angkanya.

## LLM-as-judge

**Apa itu:** Memakai satu LLM untuk menilai jawaban LLM lain berdasarkan kriteria yang kamu tentukan.

**Masalah yang diselesaikan:** penilaian manusia mahal dan tidak konsisten. Tapi pertanyaan terbuka tidak bisa dinilai dengan ROUGE. LLM-as-judge mengisi celah itu.

**Contoh bentuk:**

```python
judge_prompt = """Nilai jawaban berikut pada skala 1-5 untuk setiap kriteria.
Output dalam format JSON.

Kriteria:
- factual_accuracy: apakah isinya sesuai dokumen sumber
- grounding: apakah setiap klaim punya dasar di konteks
- completeness: apakah pertanyaan terjawab penuh

Pertanyaan: {question}
Jawaban: {answer}
Dokumen sumber: {context}

Output: {"factual_accuracy": int, "grounding": int, "completeness": int}"""
```

**Kelemahannya yang harus kamu akui:** LLM-as-judge punya bias — cenderung menyukai jawaban panjang, dan cenderung setuju dengan jawaban yang bergaya sama dengan dirinya. Karena itu di LawBot kamu juga menjalankan human judge (penilaian manual) sebagai pembanding, dan hasilnya berbeda di beberapa kasus — SeaLion unggul menurut human judge.

---

# TABEL RINGKAS — mana yang butuh code, mana yang tidak

| Istilah | Butuh contoh code? | Kalau ditanya, jawab dengan |
|---|---|---|
| vLLM | ✅ Ya | `vllm serve` + client OpenAI dengan `base_url` diarahkan |
| PagedAttention | ❌ Tidak, konsep saja | Masalah pemborosan KV cache |
| Continuous batching | ❌ Tidak, konsep saja | GPU idle saat menunggu request terpanjang |
| Quantization | ✅ Ya | `BitsAndBytesConfig` 4-bit, `compute_dtype=float16` |
| LoRA / QLoRA | ✅ Ya | `LoraConfig` target_modules, 0.12% parameter |
| Milvus | ✅ Ya | `MilvusClient`, `create_collection(dimension=...)` |
| Chroma | ✅ Ya | `create_collection`, `query(query_texts=...)` |
| Weaviate | ✅ Ya | `query.hybrid(alpha=0.5)` |
| HNSW / IVF | ❌ Tidak, tapi parameter | recall vs latency trade-off |
| MCP | ✅ Ya | `MCPServer`, `@mcp.tool()`, type hint + docstring |
| Tool calling | ✅ Ya, bentuk alur | 5 langkah; LLM tidak menjalankan fungsinya |
| LangGraph | ✅ Ya, struktur | `add_conditional_edges` |
| Chunking | ✅ Ya | Kodemu LawBot 1000/100 |
| Embedding | ✅ Ya | Kodemu multilingual model |
| FAISS | ✅ Ya | Kodemu + catatan `allow_dangerous_deserialization` |
| Top-k | ✅ Ya | `k=2` di config.py-mu |
| Hybrid retrieval | ✅ Ya | RRF + kenapa 60 |
| Reranking | ✅ Ya | retrieve lebar → rerank sempit |
| Kerahasiaan data | ✅ Ya, 4 lapisan | RLS SQL + base_url + logging + bind 127.0.0.1 |
| Docker hardening | ✅ Ya | digest pin, start_period, cap_drop |
| Hallucination / Grounding | ✅ Ya | prompt grounding + angka 60%→46,74% |
| ROUGE | ⚠️ Angka saja | Kenapa turun padahal faktual naik |
| LLM-as-judge | ✅ Ya | prompt JSON + kelemahan biasnya |

---

# KALAU DITANYA HAL YANG TIDAK ADA DI FILE INI

> "Saya belum tahu itu. Yang paling dekat yang saya sudah kerjakan adalah ..., dan saya paham bedanya di ..."

Jangan improvisasi. Satu klaim yang tidak bisa kamu pertahankan akan membongkar seluruh wawancara, dan pewawancara teknis biasanya bisa menciumnya dalam satu pertanyaan lanjutan.

---

# SUMBER

- vLLM: `docs.vllm.ai` — Quickstart & OpenAI Compatible Server
- MCP: `modelcontextprotocol.io/docs/develop/build-server` + MCP Python SDK v2 (`py.sdk.modelcontextprotocol.io`), spesifikasi 2026-07-28
- Milvus: `milvus.io/docs/quickstart.md`
- Chroma: `docs.trychroma.com/docs/overview/getting-started`
- Kode LawBot: `chatbot-uu-tni/Deployment/` — `setup_vectorstore.py`, `rag_handler.py`, `api_client.py`, `config.py`, `app.py`
- Docker & GPU: `Portofolio/server-infrastructure/learningwithus-embedding/compose.yaml`
