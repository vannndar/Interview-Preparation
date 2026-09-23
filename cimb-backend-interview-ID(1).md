# CIMB Niaga — User Interview: Backend Fundamentals
## Versi lengkap & singkat · Bahasa Indonesia

---

# BAGIAN 1 — API

## 1.1 Anatomi HTTP Request & Response

Setiap komunikasi API punya **struktur tetap**. Ini yang paling dasar dan paling sering ditanya.

### REQUEST — ada 3 bagian

```
POST /v1/transfer HTTP/1.1              ← Request Line
Host: api.cimbniaga.co.id               ┐
Authorization: Bearer eyJhbGci...       │
Content-Type: application/json          ├ Request Headers
X-TIMESTAMP: 2026-09-22T10:00:00+07:00  │
X-SIGNATURE: abc123...                  ┘
                                        ← baris kosong (pemisah)
{                                       ┐
  "from": "1234567890",                 ├ Request Body
  "amount": 100000                      │
}                                       ┘
```

| Bagian | Isi | Wajib? |
|---|---|---|
| **Request Line** | `METHOD` + `path` + `versi HTTP` | ✅ |
| **Headers** | Metadata: auth, content type, timestamp, signature | Sebagian |
| **Baris kosong** | Pemisah antara header dan body | ✅ |
| **Body** | Data yang dikirim (JSON/XML/form) | Tergantung method |

### RESPONSE — ada 3 bagian

```
HTTP/1.1 200 OK                         ← Status Line
Content-Type: application/json          ┐
X-Request-ID: req-abc123                ├ Response Headers
Cache-Control: no-store                 ┘
                                        ← baris kosong
{                                       ┐
  "status": "SUCCESS",                  │
  "data": { ... }                       ├ Response Body
}                                       ┘
```

| Bagian | Isi |
|---|---|
| **Status Line** | `versi HTTP` + `status code` + `reason phrase` |
| **Headers** | Metadata tentang respons |
| **Baris kosong** | Pemisah |
| **Body** | Data hasil (JSON biasanya) |

### Soal "footer" — HTTP Trailer

HTTP **tidak punya footer** dalam arti umum. Yang ada namanya **Trailer** — header yang dikirim **setelah** body selesai, sangat jarang dipakai (biasanya untuk checksum pada streaming/chunked transfer).

Kalau interviewer menyebut "footer", kemungkinan maksudnya:
- **Di JWT** → tidak ada footer. JWT hanya `header.payload.signature`.
- **Di SOAP** → ada `<Envelope>` berisi `<Header>` dan `<Body>`.
- **Di request/response** → yang dimaksud kemungkinan bagian penutup/catatan akhir, bukan konsep HTTP resmi.

**Jawaban aman:** *"Di HTTP tidak ada footer. Strukturnya hanya request/status line, headers, dan body. Ada yang namanya Trailer header, tapi jarang dipakai — biasanya untuk checksum pada streaming."*

---

## 1.2 Header yang penting

### Request header

| Header | Fungsi |
|---|---|
| `Authorization` | Kredensial — biasanya `Bearer <token>` |
| `Content-Type` | Format body yang **dikirim** |
| `Accept` | Format yang **diinginkan** sebagai respons |
| `User-Agent` | Identitas klien |
| `X-TIMESTAMP` | Waktu request (wajib di SNAP) |
| `X-SIGNATURE` | Tanda tangan request (wajib di SNAP) |
| `X-PARTNER-ID` | Client ID partner (wajib di SNAP) |
| `X-EXTERNAL-ID` | Reference unik per request (wajib di SNAP) |
| `CHANNEL-ID` | Channel pengirim (wajib di SNAP) |
| `Idempotency-Key` | Kunci anti-duplikat untuk operasi uang |

### Response header

| Header | Fungsi |
|---|---|
| `Content-Type` | Format body respons |
| `Cache-Control` | Aturan caching — `no-store` untuk data sensitif |
| `Set-Cookie` | Mengirim cookie (mis. refresh token) |
| `X-Request-ID` | ID request untuk tracing/lacak di log |
| `X-RateLimit-Remaining` | Sisa kuota request |
| `Retry-After` | Kapan boleh coba lagi (saat 429/503) |

**Poin yang menunjukkan pemahaman:** `Cache-Control: no-store` wajib di endpoint perbankan — supaya respons berisi saldo tidak tersimpan di cache browser atau proxy perantara.

---

## 1.3 Body request & body respons

### Request body — apa isinya?

Isinya data yang dikirim klien. Formatnya ditentukan `Content-Type`:

| Content-Type | Kapan dipakai |
|---|---|
| `application/json` | Paling umum, standar modern |
| `application/x-www-form-urlencoded` | Form HTML klasik |
| `multipart/form-data` | Upload file |
| `application/xml` | SOAP / sistem legacy perbankan |
| `text/plain` | Sederhana |

**Contoh request body transfer:**
```json
{
  "partnerReferenceNo": "20260922001",
  "sourceAccountNo": "1234567890",
  "beneficiaryAccountNo": "0987654321",
  "amount": { "value": "100000.00", "currency": "IDR" },
  "remark": "Pembayaran"
}
```

**Aturan penting:** nominal dikirim sebagai **string**, bukan number. `"100000.00"` bukan `100000.00`. Alasannya: JSON number itu floating point di banyak parser, dan uang **tidak boleh** pakai floating point.

### Response body — ada apa saja?

**1. Status / hasil operasi**
```json
{ "responseCode": "2001100", "responseMessage": "Successful" }
```
SNAP pakai `responseCode` 7 digit, bukan cuma HTTP status. Jadi ada **dua lapis status**: HTTP status code dan `responseCode` di body. Keduanya bisa berbeda — HTTP 200 tapi `responseCode` menandakan bisnis gagal.

**2. Data hasil**
```json
{
  "data": {
    "referenceNo": "20260922001",
    "amount": { "value": "100000.00", "currency": "IDR" },
    "status": "SUCCESS",
    "transactionDate": "2026-09-22T10:00:05+07:00"
  }
}
```

**3. Metadata**
```json
{ "totalData": 150, "page": 1, "pageSize": 20 }
```

**4. Error detail** (kalau gagal)
```json
{
  "responseCode": "4014701",
  "responseMessage": "Invalid Access Token",
  "errorDetails": [
    { "field": "amount", "reason": "must be greater than zero" }
  ]
}
```

**Struktur standar yang layak dipakai:**
```json
{
  "status": "SUCCESS | FAILED | PENDING",
  "data": { },
  "error": { "code": "", "message": "", "details": [] },
  "meta": { "requestId": "", "timestamp": "" }
}
```

---

## 1.4 Method & Status Code

| Method | Fungsi | Idempotent |
|---|---|---|
| `GET` | Baca | ✅ |
| `POST` | Buat / submit | ❌ |
| `PUT` | Replace seluruhnya | ✅ |
| `PATCH` | Update sebagian | ⚠️ belum tentu |
| `DELETE` | Hapus | ✅ |

**Status code:**

- **2xx** — `200` OK · `201` Created · `202` Accepted (async) · `204` No Content
- **4xx** — `400` Bad Request · `401` belum auth · `403` sudah auth tapi tidak punya izin · `404` · `409` Conflict · `422` validasi gagal · `429` terlalu banyak request
- **5xx** — `500` · `502` Bad Gateway · `503` · `504` Gateway Timeout

**401 vs 403** — pembeda yang sering ditanya:
- **401** = belum terautentikasi. Login ulang **membantu**.
- **403** = sudah terautentikasi tapi tidak berhak. Login ulang **tidak membantu**.

---

# BAGIAN 2 — AUTHENTICATION & AUTHORIZATION

| | Authentication | Authorization |
|---|---|---|
| **Pertanyaan** | "Siapa kamu?" | "Apa yang boleh kamu lakukan?" |
| **Cara** | Password, OTP, biometric, sertifikat | Role, permission, scope, ownership |
| **Urutan** | **Dulu** | **Setelah** authn |

## Session vs Token

| | Session | Token (JWT) |
|---|---|---|
| State | Server simpan session | Server **tidak** simpan |
| Validasi | Lookup ke session store | Verifikasi signature |
| Revoke | Mudah | **Sulit** |
| Scale | Session store harus dibagi | Stateless, mudah di-scale |

**Trade-off kuncinya:** JWT menghilangkan lookup — tapi kamu **kehilangan kemampuan revoke instan**. Ini harus selalu disebut.

---

# BAGIAN 3 — TOKEN & JWT

## 3.1 Struktur JWT — ada 3 bagian

```
eyJhbGciOiJIUzI1NiJ9 . eyJzdWIiOiIxMjMifQ . SflKxwRJSMeKKF2QT4fwp
     HEADER               PAYLOAD            SIGNATURE
```

**1. Header** — algoritma & tipe
```json
{ "alg": "HS256", "typ": "JWT" }
```

**2. Payload** — claims (data)
```json
{
  "sub": "user123",      // siapa
  "iss": "auth.cimb",    // siapa penerbit
  "aud": "api.cimb",     // untuk siapa
  "exp": 1735689600,     // kedaluwarsa
  "iat": 1735686000,     // diterbitkan kapan
  "jti": "a1b2c3",       // ID unik token
  "role": "customer"
}
```

**3. Signature** — `HMAC(secret, base64(header) + "." + base64(payload))`

**Tidak ada bagian keempat.** JWT hanya 3 bagian.

## 3.2 ⚠️ Poin paling penting

> **JWT itu di-ENCODE (Base64URL), BUKAN di-ENKRIPSI.**
> Siapa pun yang punya token bisa **membaca** payload-nya tanpa kunci.
> Signature hanya menjamin **keaslian & integritas**, bukan **kerahasiaan**.

**Jadi:** jangan pernah taruh nomor rekening, NIK, saldo, atau data sensitif di payload.

## 3.3 HS256 vs RS256

| | HS256 | RS256 |
|---|---|---|
| Jenis | **Symmetric** | **Asymmetric** |
| Sign | Secret | Private key |
| Verify | Secret yang sama | **Public key** |
| Masalah | Secret harus dibagi ke semua service | Lebih lambat |
| Cocok untuk | Satu service | **Microservices** |

**Aturan:**
> "Kalau hanya satu service yang menerbitkan dan memverifikasi, HS256 cukup. Dalam arsitektur microservices di mana banyak service perlu memverifikasi token, harus RS256 — karena kamu tidak mau menyebarkan signing secret ke belasan service. Itu bukan soal performa, tapi soal berapa banyak tempat yang bisa bocor."

## 3.4 Serangan JWT

| Serangan | Penjelasan | Pertahanan |
|---|---|---|
| **Algorithm confusion** | Penyerang pakai `alg: HS256` dan sign pakai public key RSA sebagai secret | Kunci algoritma di kode — jangan percaya `alg` dari token |
| **`alg: none`** | Token tanpa signature | Tolak `none` eksplisit |
| **Replay** | Token dicuri lalu dipakai ulang | HTTPS + `exp` pendek + `jti` untuk operasi sekali pakai |
| **Tanpa `exp`** | Token berlaku selamanya | Selalu terbitkan dan validasi `exp` |

## 3.5 Cara revoke JWT

**Tidak bisa.** Token yang sudah ditandatangani valid sampai `exp`.

Solusinya dirancang di sekitar fakta itu:
1. **Access token pendek** (5–15 menit) + refresh token yang dicek ke server
2. **Denylist** `jti` di Redis sampai `exp`-nya
3. **Token version** per user — naikkan versi untuk mencabut semua token user itu

---

# BAGIAN 4 — PENYIMPANAN TOKEN

Ini yang kamu sebut "cara penyimpanannya". **Tidak ada opsi bebas dari serangan** — selalu trade-off.

| Lokasi | XSS | CSRF | Catatan |
|---|---|---|---|
| `localStorage` | ❌ **Rentan** | ✅ Aman | Dibaca JS apa pun. Satu XSS = token dicuri |
| `sessionStorage` | ❌ **Rentan** | ✅ Aman | Sama, hilang saat tab ditutup |
| Cookie tanpa flag | ❌ Rentan | ❌ Rentan | Paling buruk |
| Cookie `httpOnly` | ✅ **Aman** | ❌ **Rentan** | Terkirim otomatis tiap request |
| In-memory (variabel JS) | ✅ Aman | ✅ Aman | Hilang saat refresh |
| Cookie `httpOnly` + `SameSite` + `Secure` | ✅ | ⚠️ sebagian | Praktik modern |

**Pola untuk perbankan:**
- **Access token: in-memory saja** — jangan dipersist
- **Refresh token: cookie `httpOnly` + `Secure` + `SameSite=Strict`**

**Kenapa:** XSS tidak bisa membaca keduanya. Trade-off-nya refresh halaman menghilangkan access token — tapi itu ditangani dengan silent refresh.

**Untuk mobile:** platform keychain / keystore, bukan storage biasa.

**Poin server-side yang sering dilupakan:**
> State session atau revokasi harus di **Redis dengan TTL**, bukan map lokal di memori. Di deployment multi-instance, map lokal berarti revokasi hanya jalan di instance yang menangani login.

---

# BAGIAN 5 — MICROSERVICES

## Monolith vs Microservices

| Aspek | Monolith | Microservices |
|---|---|---|
| Deploy | Satu unit besar | Independen per service |
| Database | Satu bersama | **Per service** |
| Komunikasi | In-process call | Network (HTTP/gRPC/broker) |
| Scaling | Replikasi seluruhnya | Per service |
| Debugging | Mudah | Sulit — butuh tracing |

## Kapan TIDAK tepat pakai microservices

> "Microservices membayar kemandiriannya dengan biaya operasional nyata: kegagalan network, eventual consistency, distributed tracing, dan hilangnya transaksi lintas service. **Modular monolith** biasanya pilihan yang tepat sampai kamu benar-benar merasakan sakitnya."

**Trigger yang membenarkan:** tim saling memblokir saat deploy · satu komponen butuh scaling sangat berbeda · butuh teknologi berbeda untuk alasan nyata.

## Cara menentukan batas service

**Bounded context (DDD)** — batas mengikuti **domain bisnis**, bukan teknis.

✅ **Benar:** Accounts (rekening/saldo) · Payments (transfer) · Cards · Notifications · Auth · Channels/OCTO

❌ **Salah:** service per tabel · service per lapisan · nanoservices

**Aturan:** kalau dua hal harus selalu berubah bersamaan dan konsisten satu sama lain, kemungkinan mereka **satu service**.

---

# BAGIAN 6 — KOMUNIKASI ANTAR SERVICE

## Sync vs Async

| | Synchronous | Asynchronous |
|---|---|---|
| Cara | Kirim → **tunggu** respons | Kirim → lanjut kerja |
| Mekanisme | REST, gRPC, Feign, WebClient | Kafka, RabbitMQ, Redis Pub/Sub |
| Cocok untuk | Butuh hasil sekarang | Boleh terjadi di belakang |
| Risiko | **Cascading failure** | Ordering, duplikat, backpressure |

**Aturan simpel:** sync kalau butuh respons **sekarang**; async untuk apa pun yang boleh terjadi **di belakang**.

## Contoh nyata: alur transfer

```
1. User klik Transfer        → SYNC  ke Payment
2. Validasi saldo            → SYNC  ke Accounts (butuh sekarang)
3. Debit + kredit            → SATU TRANSAKSI DATABASE LOKAL
4. Publish event ke Kafka    → ASYNC
5. Notification consume      → async
6. Reporting consume         → async
7. Fraud detection consume   → async
```

**Kalimat kunci:**
> "Critical path-nya synchronous, karena user sedang menunggu. Semua yang di hilir pergerakan uang itu asynchronous — kegagalan di sana tidak boleh membatalkan transfer yang sudah berhasil."

**Transactional outbox pattern** — kalau transfer commit tapi publish event gagal, notifikasi tidak pernah terkirim. Outbox memastikan keduanya konsisten.

## Circuit breaker

Mencegah **cascading failure**. Kalau service tujuan gagal berulang, breaker **membuka** dan langsung menolak tanpa mencoba.

- **Closed** — normal
- **Open** — langsung tolak
- **Half-open** — coba beberapa request; kalau berhasil, tutup lagi

**Kenapa penting:** tanpa ini, klien terus menunggu timeout, thread pool penuh, dan service yang sehat pun ikut mati.

## Retry, timeout, idempotency

- **Timeout wajib** — call tanpa timeout bisa menggantung dan menghabiskan thread
- **Retry harus bounded** — jumlah terbatas, exponential backoff + **jitter**
- **Retry hanya untuk error layak retry** — timeout/5xx, bukan 400/401
- **Operasi uang wajib idempotent** — idempotency key + **unique constraint di database**

**Poin penting:** kalau timeout, kamu **tidak tahu** apakah operasi di sana berhasil atau gagal. Statusnya ambigu. Itu justru alasan idempotency key ada.

## Saga pattern

Cara menangani transaksi lintas service **tanpa** distributed transaction (2PC).

**Dua model:**
- **Choreography** — tiap service publish event, service lain bereaksi. Tidak ada koordinator.
- **Orchestration** — ada koordinator yang memanggil berurutan dan menangani kegagalan.

**Compensating transaction:**
```
Transfer: debit A → kredit B → notifikasi
Kalau "kredit B" gagal:
  → jalankan operasi KOMPENSASI: refund debit A
  → BUKAN rollback
```

**Kenapa bukan rollback?** Dalam microservices **tidak ada transaksi global**. Operasi yang sudah commit di service lain tidak bisa di-rollback. Yang bisa dilakukan adalah menjalankan operasi **kebalikannya** secara eksplisit.

## Eventual consistency di perbankan

> "Eventual consistency tidak masalah untuk notifikasi dan analytics. **Tapi tidak boleh untuk jalur uang.** Kalau saldo yang dilihat nasabah datang dari replika yang belum tersinkron, kamu bisa mengotorisasi penarikan atas dana yang sebenarnya sudah tidak ada."

**Konsekuensinya:**
- **Jalur uang** → strong consistency, satu database, satu transaksi
- **Jalur non-kritis** (notifikasi, analytics) → eventual consistency boleh
- Ini **alasan kuat** kenapa debit dan kredit biasanya tidak dipisah ke dua service

---

# BAGIAN 7 — ASYNC CALL

## Sync vs Async

**Sync** — pemanggil **menunggu** respons. Thread terblokir.
**Async** — pemanggil **tidak menunggu**. Hasil ditangani lewat callback/future/event.

**Analogi konkret:** telepon (sync) vs kirim surat (async).

## Kapan pakai yang mana

**Sync:** butuh hasil sekarang · user menunggu di layar · operasi cepat dan andal
**Async:** hasil tidak dibutuhkan langsung · operasi lambat · beban bisa dipuncak · ingin decoupling

## Implementasi di Java

| Cara | Kapan |
|---|---|
| `CompletableFuture` | Async dalam satu aplikasi, bisa dikombinasi |
| `@Async` (Spring) | Method di thread pool terpisah |
| `WebClient` | HTTP non-blocking pengganti `RestTemplate` |
| Kafka / RabbitMQ | Komunikasi antar service yang decoupled |

```java
CompletableFuture<Account> acc = CompletableFuture.supplyAsync(() -> accountApi.fetch(id));
CompletableFuture<Limit>  lim = CompletableFuture.supplyAsync(() -> limitApi.fetch(id));
CompletableFuture<Result> combined = acc.thenCombine(lim, (a, l) -> build(a, l));
```

**Poin yang harus disebut:**
> "Async tidak membuat satu operasi lebih cepat — total waktunya bisa sama. Yang berubah: thread tidak terblokir, jadi satu server menangani lebih banyak request bersamaan. Async meningkatkan **throughput**, bukan **latency**."

## Masalah yang muncul di async

- **Error handling** — exception di thread lain tidak otomatis naik
- **Ordering** — pesan bisa datang tidak berurutan
- **Duplikat** — pesan bisa terkirim lebih dari sekali, consumer harus **idempotent**
- **Debugging** — butuh **correlation ID** dan distributed tracing
- **Backpressure** — producer lebih cepat dari consumer
- **Broker mati** — pesan hilang kalau tidak persistent

---

# BAGIAN 8 — SNAP (STANDAR API BANK INDONESIA)

Ini standar **wajib** dari BI. CIMB Niaga memakainya — portal API mereka ada di `api.cimbniaga.co.id`.

## Model keamanan — dua lapis

**Lapis 1 — Access Token (OAuth 2.0)**

```
1. Get Access Token    → partner minta token
2. Return Access Token → CIMB kembalikan token
3. Business request    → inquiry / transfer
4. Return result
```

- Grant type: **`client_credentials`** (partner ke partner, bukan login user)
- Masa berlaku: **900 detik (15 menit)**
- Refresh token harus lebih pendek dari access token

**Lapis 2 — Signature setiap request**

Header wajib:

| Header | Isi |
|---|---|
| `Authorization` | `Bearer <access_token>` |
| `X-TIMESTAMP` | Waktu lokal klien (ISO8601) |
| `X-SIGNATURE` | Signature request |
| `X-PARTNER-ID` | Client ID partner |
| `X-EXTERNAL-ID` | Reference unik per request |
| `CHANNEL-ID` | Channel ID |

**Formula signature:**

```
Symmetric (HMAC_SHA512):
stringToSign = HTTPMethod + ":" + EndpointUrl + ":" + AccessToken + ":" 
             + Lowercase(HexEncode(SHA-256(minify(RequestBody)))) + ":" + TimeStamp

Asymmetric (SHA256withRSA):
stringToSign = HTTPMethod + ":" + EndpointUrl + ":" 
             + Lowercase(HexEncode(SHA-256(minify(RequestBody)))) + ":" + TimeStamp
```

**Bedanya:** symmetric memasukkan **AccessToken** ke stringToSign, asymmetric **tidak** — karena pada asymmetric token diambil di langkah terpisah.

**Kenapa timestamp dan signature wajib:** request tidak bisa di-replay. Signature mengikat method, endpoint, body, dan waktu. Ubah satu karakter di body → signature tidak valid lagi.

---

# BAGIAN 9 — AUTH DI HEADER, BODY, DAN FOOTER

## Di mana auth seharusnya diletakkan?

**Header → ini tempat yang BENAR.**
`Authorization: Bearer <token>` adalah standar, dan inilah yang dipakai SNAP (CIMB Niaga memakainya di API mereka). Token sebagai kredensial selalu di header.

**Body → bisa, tapi BUKAN praktik yang baik untuk kredensial.**
Alasannya:
- Body jauh lebih sering **ter-log** (request logging, debugging, audit trail)
- Body muncul di **pesan error** dan stack trace
- Body bisa **tersimpan di cache proxy** kalau `Cache-Control` tidak di-set benar
- Body lebih besar, jadi token ikut tersalin ke tempat-tempat yang tidak perlu

Kalau ada API yang menaruh token di body, biasanya itu **API legacy** — bukan pola yang layak ditiru.

**Footer → HTTP TIDAK PUNYA FOOTER.**
Yang ada namanya **Trailer** — header yang dikirim setelah body selesai. Dipakai untuk checksum pada chunked/streaming transfer. **Auth tidak pernah di situ.**

## Kalau interviewer bilang ketiganya ada — apa maksudnya?

Ada tiga kemungkinan:

**1. Dia menguji apakah kamu berani mengoreksi.**
Ini yang paling sering. Interviewer senior kadang sengaja menyebut hal yang tidak akurat untuk melihat apakah kandidat ikut saja atau membetulkan. Kandidat yang mengiyakan semuanya justru terlihat tidak paham.

**2. Dia maksud SOAP, bukan REST.**
Di SOAP ada `<Envelope>` yang berisi `<Header>` dan `<Body>`. Keamanan **WS-Security** diletakkan di dalam `<Header>` SOAP — **bukan** di HTTP header.
Jadi di konteks SOAP: ada HTTP header, lalu ada SOAP header (di dalam body), dan SOAP body. Tiga "lapisan" itu bisa terasa seperti header/body/footer.
**Ini relevan di perbankan** karena banyak integrasi legacy masih SOAP.

**3. Dia maksud signature SNAP yang komponennya diambil dari beberapa tempat.**
Signature SNAP dibangun dari **HTTP method + endpoint + access token (header) + hash body + timestamp**. Jadi secara teknis "auth"-nya melibatkan header **dan** body sekaligus.

## Cara menjawab

> "Auth yang standar ada di header — `Authorization: Bearer <token>`, dan itu yang dipakai SNAP. Body bisa membawa data, tapi menaruh kredensial di body dihindari karena body lebih sering ter-log dan muncul di pesan error.
>
> Dan HTTP tidak punya footer. Yang ada Trailer, dan itu untuk checksum pada streaming, bukan untuk auth.
>
> Kalau yang dimaksud SOAP, maka WS-Security memang diletakkan di dalam `<Header>` SOAP, yang posisinya ada di dalam `<Envelope>`. Itu lapisan yang berbeda dari HTTP header."

**Kalau ragu, tanyakan balik:** *"Apakah yang dimaksud SOAP, atau REST?"* — pertanyaan balik yang tepat justru menunjukkan pemahaman.

---

# BAGIAN 10 — THREADING & MULTIPROCESS (5 API + UI)

## Sisi klien (browser / React)

**"Web hanya punya 1 thread" — benar, tapi tidak lengkap.**

**Yang benar:** eksekusi JavaScript di **main thread** itu single-threaded, dan render juga di main thread. Jadi kalau JS-nya blocking, UI freeze.

**Yang perlu dikoreksi:** **fetch tidak memblokir main thread.**

- Network I/O ditangani browser di **proses terpisah** — Chrome punya network service process sendiri
- 5 fetch bersamaan → kelimanya dikirim **paralel** di layer network
- Bisa lewat **satu koneksi HTTP/2 dengan multiplexing** — jadi tidak perlu 5 koneksi terpisah
- Callback-nya nanti dieksekusi **bergantian** di main thread saat respons datang — tapi itu bukan *blocking*, karena CPU tidak menunggu

**Konsekuensinya pada waktu:**
- Pakai `Promise.all` → total waktu = **API terlama**, bukan jumlah kelimanya
- Await satu per satu → total waktu = **jumlah kelimanya**

```javascript
// PARALEL — total ≈ API terlama
const [a, b, c, d, e] = await Promise.all([api1(), api2(), api3(), api4(), api5()]);

// SEKUENSIAL — total ≈ jumlah semuanya
const a = await api1();
const b = await api2();
```

**Browser sebenarnya multi-thread dan multi-process:**
| Komponen | Fungsi |
|---|---|
| Renderer process | Satu per tab (biasanya), tempat JS jalan |
| **Main thread** | Eksekusi JS + render |
| Compositor thread | Menggabungkan layer, mengurus scroll |
| Raster thread | Mengubah elemen jadi pixel |
| **Web Worker** | JS thread asli, tapi **tidak punya akses DOM** |
| Network service process | Menangani semua network I/O |

**Yang harus dijaga:** jangan taruh kerja CPU berat di main thread — itu penyebab UI freeze. Kalau ada (parsing data besar, kalkulasi, image processing), pindahkan ke **Web Worker**.

## Sisi server — bagaimana CIMB melayani request

Modelnya **beda per stack**:

| Stack | Model | Catatan |
|---|---|---|
| **Java / Spring Boot** | **Thread-per-request** | Tomcat thread pool (default ~200). Tiap request pegang satu thread, I/O blocking. Pool habis → request mengantre |
| **Node.js** | **Event loop single-threaded** | I/O non-blocking. Operasi file/DNS di-offload ke **libuv thread pool** (default 4). Network ditangani OS |
| **Python** | **GIL** | Satu thread jalan pada satu waktu untuk CPU. I/O melepas GIL. CPU-bound harus multiprocessing |
| **Go** | **Goroutine** | Ringan, dijadwalkan M:N ke OS thread |

**Java thread-per-request — kenapa penting:**
Spring Boot secara default memakai model ini. Setiap request HTTP dipegang satu thread dari pool sampai selesai. Kalau service kamu memanggil service lain dengan `RestTemplate` (blocking), thread itu **tertahan menunggu** — tidak bisa melayani request lain.

Itu sebabnya:
- **Timeout wajib** — thread yang menggantung tanpa timeout akan menghabiskan pool
- **`WebClient` lebih baik daripada `RestTemplate`** untuk call antar service — non-blocking, tidak menahan thread
- Ukuran thread pool perlu disesuaikan dengan beban, bukan dibiarkan default

## Thread vs Multiprocess — aturan pilihnya

| | Threading | Multiprocessing |
|---|---|---|
| Memori | **Shared** | Terpisah |
| Biaya buat | Murah | Mahal |
| Cocok untuk | **I/O-bound** | **CPU-bound** |
| Kenapa | Thread cuma **menunggu**, jadi bisa banyak | Benar-benar **paralel**, melewati GIL |
| Risiko | Race condition, butuh sinkronisasi | Overhead IPC, memori besar |

**Aturan simpelnya:**
- **I/O-bound** (API call, DB query, baca file, network) → **thread atau async**. Thread-nya cuma menunggu, bukan kerja, jadi bisa ribuan.
- **CPU-bound** (enkripsi, image processing, kompilasi, ML inference) → **multiprocess**. Thread tidak membantu karena CPU-nya memang sibuk.

## Jawaban utuh kalau ditanya

> "Kalau di browser, JavaScript memang single-threaded di main thread, tapi itu bukan hambatan untuk 5 API call. Fetch itu asynchronous — request dikirim lewat network stack browser yang jalan di proses terpisah, jadi main thread tidak menunggu. Kelima request dikirim paralel, dan callback-nya dieksekusi bergantian di main thread saat responsnya datang. Kalau saya pakai `Promise.all`, total waktunya jadi waktu API terlama, bukan jumlah kelimanya.
>
> Yang perlu dijaga adalah jangan menaruh kerja CPU berat di main thread, karena itu yang membuat UI freeze. Kalau ada, pindahkan ke Web Worker.
>
> Di sisi server, modelnya beda per stack. Java pakai thread-per-request — Tomcat punya thread pool dan tiap request memegang satu thread. Itu sebabnya pemanggilan antar service yang blocking berbahaya: thread-nya tertahan menunggu dan tidak bisa melayani request lain, jadi timeout dan pemilihan client yang non-blocking itu penting. Node.js pakai event loop single-threaded dengan I/O non-blocking. Python kena GIL, jadi CPU-bound harus pakai multiprocessing.
>
> Pilihan thread atau process tergantung bebannya: I/O-bound pakai thread atau async karena thread cuma menunggu; CPU-bound pakai multiprocess supaya benar-benar paralel."

---

# BAGIAN 11 — CARA MENJAWAB

**1. Jawab, lalu sebut trade-off-nya.**
Jangan berhenti di definisi. Interviewer senior menilai apakah kamu tahu **kapan tidak memakainya**.

> "JWT itu stateless, jadi bisa dipakai banyak service tanpa lookup. Trade-off-nya kamu kehilangan kemampuan revoke instan — token yang sudah ditandatangani tetap valid sampai `exp`."

**2. Kalau pernah melakukannya — sebut proyekmu.**

> "Sisi penanganan token-nya saya bangun sendiri. Di job tracker saya pakai autentikasi Supabase dengan proteksi route di layer middleware, dan row-level security di database sehingga bug di layer API tidak bisa membuka data user lain. Prinsip defence-in-depth yang sama."

**3. Kalau tidak tahu — akui dan tunjukkan cara berpikir.**

> "Saya belum pernah mengerjakan itu langsung. Berdasarkan yang saya pahami, pendekatan saya akan seperti ini — tapi saya mau konfirmasi dulu ke orang yang sudah melakukannya."

**Jangan mengarang.** Interviewer ini fokus ke dasar, artinya dia menguasai dasar itu.

---

# BAGIAN 12 — TIGA HAL YANG PALING MENENTUKAN

1. **Sebut SNAP** — standar wajib BI, dan CIMB Niaga memakainya. Hampir tidak ada kandidat yang tahu.

2. **Sebut trade-off, bukan cuma definisi** — kalimat *"the trade-off is..."* menunjukkan pemahaman, bukan hafalan.

3. **Jujur soal batas pengetahuan** — pertanyaan lanjutan akan menemukan jawaban yang dikarang.
