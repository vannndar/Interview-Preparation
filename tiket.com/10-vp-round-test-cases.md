# tiket.com VP Round — Positive & Negative Test Cases (FOCUS!)
## Interviewer: Bhupesh Mittal (VP - SQA) | 17 Sept 2026, 18:00/18:30 WIB | 45 min

**INTEL DARI HR (Lalita, WhatsApp):** "Vp round focusing on positive negative test cases"

=> Ini sesi PRAKTIKAL. VP kemungkinan besar memberi satu fitur/scenario, lalu minta kamu menyebutkan positive dan negative test cases-nya. Bukan sesi teori.

---

## CARA MENJAWAB (STRUKTUR WAJIB)

Saat VP memberi scenario, JANGAN langsung menyebut test case. Ikuti alur ini:

1. **Clarify scope** (10 detik) — "Let me confirm my understanding: I'm testing [fitur] for [platform]..."
2. **State assumptions** — "I'll assume [data valid], [environment], [user state]..."
3. **Positive test cases** — happy path dulu, dari yang paling kritikal
4. **Negative test cases** — invalid input, error handling, edge conditions
5. **Boundary cases** — nilai di tepi batas
6. **Layered check** — UI, API, database, security (tunjukkan kamu berpikir lintas layer)
7. **Ask if they want depth on one area**

Kalimat pembuka yang kuat:
> "I'd approach this in layers. Let me start with the positive cases to confirm the happy path works, then negative cases for error handling, then boundary and security cases. Should I go deep on any particular layer?"

---

## ATURAN: APA ITU POSITIVE vs NEGATIVE

**Positive test case** = input/kondisi VALID, sistem diharapkan BERHASIL.
**Negative test case** = input/kondisi INVALID, sistem diharapkan MENANGANI DENGAN BENAR (menolak / menampilkan error / tidak crash).

Kalimat pembeda yang bagus untuk diucapkan:
> "Positive cases verify the system does what it should. Negative cases verify the system doesn't do what it shouldn't — and that it fails gracefully."

**Kesalahan umum kandidat:**
- Negative case bukan "sistem error" — negative case artinya sistem MENANGANI input salah dengan tepat (error message jelas, tidak crash, data tidak rusak)
- Jangan lupa: "tidak crash" itu bagian dari expected result di negative case

---

## CONTOH 1: LOGIN / REGISTRATION (paling sering dipakai interviewer)

### Positive Test Cases
1. Login dengan email dan password valid -> berhasil masuk, redirect ke homepage
2. Login dengan nomor telepon yang terdaftar -> berhasil masuk
3. Login via Google/Apple OAuth -> berhasil, akun terhubung
4. Login dengan email uppercase (IVAN@mail.com) -> tetap berhasil (email case-insensitive)
5. Login dengan "Remember me" dicentang -> session bertahan setelah browser ditutup
6. Login dengan spasi di awal/akhir email -> sistem trim otomatis, berhasil
7. Login setelah reset password dengan password baru -> berhasil

### Negative Test Cases
1. Email valid + password salah -> "Incorrect password", tidak masuk, tidak bocor info
2. Email tidak terdaftar -> "Account not found" (atau pesan generik untuk cegah user enumeration)
3. Email kosong -> validasi "Email is required"
4. Password kosong -> validasi "Password is required"
5. Kedua field kosong -> kedua validasi muncul
6. Format email salah (ivan@, ivan.com, @mail.com) -> "Invalid email format"
7. Password case-sensitive: "Pass@123" vs "pass@123" -> harus gagal
8. Email dengan spasi di tengah (ivan @mail.com) -> ditolak
9. Coba akses halaman dashboard tanpa login -> redirect ke login
10. Login 5x gagal berturut-turut -> akun terkunci sementara / rate limited
11. SQL injection di field email: `' OR 1=1--` -> ditolak, tidak dieksekusi
12. XSS di field: `<script>alert(1)</script>` -> di-escape, tidak dijalankan
13. Field melebihi max length (email 255+ karakter) -> ditolak dengan pesan jelas
14. Klik tombol Login 2x cepat -> hanya 1 request diproses (tidak double submit)
15. Copy-paste password dengan trailing space -> perilaku konsisten (diterima atau ditolak, tapi konsisten)
16. Login saat koneksi internet terputus -> error message jelas, bukan hang
17. Token/session expired -> redirect ke login tanpa data leak
18. Login di 2 device bersamaan -> perilaku session terdefinisi

---

## CONTOH 2: SEARCH FLIGHT (domain tiket.com — PROBABILITAS TINGGI!)

### Positive Test Cases
1. Search rute valid (CGK -> DPS), tanggal future, 1 penumpang -> hasil muncul
2. Search round-trip -> kedua leg tampil dengan benar
3. Search dengan 1 penumpang dewasa -> harga benar
4. Search dengan kombinasi dewasa + anak + infant -> pricing sesuai aturan
5. Filter hasil (langsung/transit, maskapai, waktu) -> hasil terfilter benar
6. Sort by harga termurah -> urutan benar
7. Ganti bandara asal/tujuan -> hasil refresh sesuai
8. Search one-way vs round-trip -> struktur hasil berbeda sesuai ekspektasi
9. Pilih tanggal dari date picker -> tanggal terisi benar
10. Hasil search menampilkan harga, durasi, transit, bagasi dengan lengkap

### Negative Test Cases
1. Bandara asal = bandara tujuan (CGK -> CGK) -> error atau request ditolak
2. Tanggal keberangkatan di masa lalu -> ditolak, date picker disable tanggal lampau
3. Tanggal pulang lebih awal dari keberangkatan -> validasi error
4. Jumlah penumpang 0 -> ditolak
5. Jumlah penumpang melebihi batas (misal 10) -> ditolak
6. Infant lebih banyak dari dewasa -> ditolak (aturan maskapai)
7. Field kota/bandara kosong -> validasi required
8. Ketik nama bandara yang tidak ada ("Bandara Ngawur") -> "No results found", bukan error 500
9. Input special character di field kota (CGK@#$%) -> ditangani, tidak crash
10. Rute tanpa penerbangan tersedia -> pesan "no flights available", bukan halaman kosong
11. Klik Search 2x cepat -> hanya 1 request
12. Search saat API maskapai timeout -> error informatif + opsi retry (bukan blank page)
13. Search hasil besar (semua maskapai) -> performance tetap wajar, tidak freeze
14. Back button setelah search -> state konsisten, tidak reset aneh
15. Kombinasi 0 hasil + apply filter -> tetap konsisten menampilkan "no results"

### Boundary Test Cases
1. Tanggal keberangkatan = HARI INI -> harus jalan
2. Tanggal keberangkatan = besok -> jalan
3. Maksimum penumpang (9) -> jalan
4. Maksimum penumpang + 1 (10) -> ditolak
5. Booking paling jauh di masa depan (batas maskapai, misal 1 tahun) -> jalan
6. Lebih dari batas maksimum tanggal -> ditolak

---

## CONTOH 3: BOOKING / PAYMENT (paling kritikal untuk bisnis)

### Positive Test Cases
1. Booking hotel dengan data valid -> konfirmasi muncul, booking ID terbentuk
2. Payment via kartu kredit valid -> transaksi sukses, status "confirmed"
3. Payment via virtual account -> kode VA terbentuk, status update setelah transfer
4. Payment via e-wallet -> redirect ke app, kembali dengan status sukses
5. Apply promo code valid -> diskon terhitung benar
6. Harga final sesuai perhitungan (harga dasar + pajak + service - diskon)
7. E-ticket/voucher terkirim ke email setelah pembayaran sukses
8. Booking tersimpan di database dengan data yang benar
9. Riwayat booking menampilkan transaksi dengan status benar
10. Cancel booking sesuai kebijakan -> refund diproses

### Negative Test Cases
1. Kartu kredit expired -> payment ditolak dengan pesan jelas
2. CVV salah -> ditolak
3. Nomor kartu invalid (Luhn check gagal) -> ditolak sebelum submit
4. Saldo e-wallet tidak cukup -> pesan jelas, booking tidak dibuat
5. Promo code tidak valid / expired -> error, harga tidak berubah
6. Promo code sudah dipakai -> ditolak
7. Payment timeout -> status booking TIDAK boleh "confirmed" (harus pending/failed)
8. User close browser setelah klik bayar tapi sebelum callback -> status konsisten (tidak double charge)
9. Double-click tombol "Pay" -> hanya 1 transaksi (idempotency) — CASE PALING PENTING
10. Payment amount di-tamper (ubah payload) -> server menolak, pakai harga server-side bukan client-side
11. Booking saat kursi/kamar sudah habis di tengah proses -> dicegah, tidak oversell
12. Data penumpang kosong/tidak lengkap -> validasi per field
13. Nama penumpang dengan karakter special (O'Brien, 田中) -> diterima
14. Booking dengan 2 session/user bersamaan di kursi terakhir -> salah satu gagal dengan jelas
15. Network putus saat proses payment -> user bisa retry tanpa double charge
16. Refund melebihi jumlah yang dibayar -> ditolak

### Data Integrity Check (tunjukkan kamu berpikir database!)
> "Beyond the UI, I'd validate at the database layer: is the booking record created exactly once? Does the seat inventory decrement by exactly 1? Is the transaction amount matching what the payment gateway recorded?"

---

## CONTOH 4: PROMO CODE / VOUCHER

### Positive
1. Promo valid -> diskon diterapkan, harga akhir benar
2. Promo persentase -> perhitungan benar (termasuk cap maksimum kalau ada)
3. Promo nominal -> dikurangi dengan tepat
4. Stacking kalau diizinkan -> total benar

### Negative
1. Promo tidak ada / typo -> "Invalid code"
2. Promo expired -> ditolak dengan alasan jelas
3. Promo belum aktif -> ditolak
4. Promo minimum transaksi tidak terpenuhi -> ditolak + info minimum
5. Promo untuk kategori lain (promo hotel dipakai di flight) -> ditolak
6. Promo dengan kuota habis -> ditolak
7. Promo dipakai 2x oleh user sama -> ditolak
8. Promo dipakai bersamaan dengan promo lain yang tidak boleh stacking -> ditolak
9. Diskon > harga -> harga tidak boleh negatif (harus 0 atau ditolak)
10. Input promo lowercase vs uppercase -> perilaku konsisten

---

## CONTOH 5: USER PROFILE / EDIT DATA

### Positive
1. Update nama valid -> tersimpan, tampil di profil
2. Update nomor telepon valid -> tersimpan
3. Upload foto profil (jpg/png, <2MB) -> berhasil tampil
4. Change password dengan password lama benar -> berhasil, session tetap aman
5. Update data -> data lain tidak berubah (tidak ada side effect)

### Negative
1. Nama kosong -> validasi required
2. Nama melebihi max length -> ditolak
3. Nomor telepon format salah (huruf, kurang digit) -> ditolak
4. Upload file bukan gambar (.pdf/.exe) -> ditolak
5. Upload gambar > max size -> ditolak dengan pesan jelas
6. Upload file 0 byte / corrupt -> ditangani
7. Change password dengan password lama SALAH -> ditolak
8. Password baru = password lama -> ditolak
9. Password baru lemah (123) -> ditolak dengan aturan jelas
10. Konfirmasi password tidak sama -> ditolak
11. Upload nama file dengan karakter aneh (../../etc/passwd) -> ditangani (path traversal)
12. Edit profil user lain via API manipulation (ganti user_id) -> DITOLAK (authorization check)

---

## KALIMAT-KALIMAT KUAT YANG HARUS KELUAR

**Saat memulai:**
> "I'll structure this as positive cases first to verify expected behavior, then negative cases to verify error handling, then boundary conditions."

**Saat menyebut negative case:**
> "For the negative case, the expected result isn't just 'it fails' — it's that the system fails gracefully: clear error message, no crash, and no data corruption."

**Saat bicara security:**
> "I'd also include security-focused negative cases: SQL injection, XSS, and authorization checks — can a user access another user's data by manipulating the request?"

**Saat bicara database:**
> "Beyond the UI layer, I'd verify at the database level that the record was created exactly once and the data is consistent."

**Saat bicara performance:**
> "And I'd add non-functional cases: what happens under concurrent load — for example, two users booking the last available seat at the same time?"

**Saat ditanya "apa lagi?":**
> "I'd also consider what we're NOT testing: the integration with the payment gateway, email delivery, and the third-party APIs we depend on. Those are often where production issues come from."

---

## JEBAKAN YANG HARUS DIHINDARI

1. **Jangan cuma sebut happy path.** VP akan tunggu negative cases — kalau kamu berhenti di positive, itu sinyal buruk.
2. **Jangan bilang "test semua kemungkinan".** Itu mustahil. Tunjukkan prioritisasi: "the highest-risk cases first".
3. **Jangan lupa menyebut expected result.** Test case tanpa expected result bukan test case.
4. **Jangan lupa non-functional.** Performance, security, dan concurrency membedakan kandidat biasa dengan yang kuat.
5. **Jangan mengklaim sudah pernah pakai tool tertentu kalau tidak.** Jawab jujur, alihkan ke konsep.
6. **Jangan diam terlalu lama.** Kalau berpikir, ucapkan: "Let me think about the edge cases here..." — diam panjang terlihat seperti bingung.
7. **Jangan menyebut hanya UI.** Sebut API, database, integration layer.

---

## RUMUS CEPAT (kalau waktu mepet saat menjawab)

```
POSITIVE  = valid input -> expected success
NEGATIVE  = invalid input -> graceful failure + clear error + no data corruption
BOUNDARY  = min-1, min, min+1, max-1, max, max+1
SECURITY  = injection, XSS, authorization, authorization bypass
DATA      = record created once? values correct? no side effects?
PERF      = concurrent access, load, timeout handling
```

---

## LATIHAN 5 MENIT SEBELUM INTERVIEW

Ambil satu fitur. Ucapkan dengan suara keras:
- 5 positive cases
- 5 negative cases
- 3 boundary cases
- 2 security cases
- 1 data integrity check

Kalau bisa lakukan ini lancar tanpa baca catatan, kamu siap.
