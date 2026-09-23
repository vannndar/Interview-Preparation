# tiket.com Final User Interview — Question Bank
## Interviewer: Bhupesh Mittal (VP - SQA) | 17 Sept 2026, 18:00-18:45 WIB | Google Meet

---

## HIGHEST PROBABILITY (siapkan 100%)

### 1. Tell me about yourself / Walk me through your background
**Yang harus ada:** nama, latar belakang pendidikan (fresh graduate ITS Computer Science), ketertarikan pada software quality, 1-2 pencapaian teknis, alasan ada di interview ini.
**Panjang:** 45-60 detik. Jangan lebih.

### 2. Why SDET? Why not pure developer?
**Yang harus ada:** SDET = kombinasi coding + quality. Kamu suka menulis kode DAN menganalisis bagaimana sistem bisa gagal. Sebutkan bahwa quality engineering adalah disiplin engineering, bukan pekerjaan "mengklik tombol".

### 3. Why tiket.com?
**Yang harus ada:** skala (50+ juta user), kompleksitas domain travel (booking, payment, multi-partner), pioneer OTA Indonesia, bagian ekosistem Blibli Tiket. Sebutkan produk yang pernah kamu pakai.

### 4. What do you know about the SDET role?
**Yang harus ada:** bedanya SDET vs manual QA — SDET membangun automation framework, menulis test code, bekerja bersama developer sejak awal, berkontribusi ke CI/CD.

### 5. Why should we hire you?
**Yang harus ada:** 3 alasan spesifik. Contoh: (1) fondasi programming + pemahaman data/database, (2) track record menyelesaikan proyek end-to-end sendiri, (3) mindset analitis yang cocok untuk test design.

### 6. Where do you see yourself in 3-5 years?
**Yang harus ada:** arah menuju quality engineering / test automation leadership. Tunjukkan kamu berpikir jangka panjang, bukan cari pekerjaan sementara.

---

## QA MINDSET & PHILOSOPHY (paling relevan dengan VP SQA)

### 7. What does "quality" mean to you in software engineering?
**Arah:** quality bukan cuma bebas bug, tapi produk memenuhi kebutuhan user dan requirement secara konsisten. Mention prevention vs detection.

### 8. How do you decide what to test first when time is limited?
**Arah:** risk-based testing. Prioritas: critical path (login, booking, payment), lalu area dengan perubahan terbaru, lalu boundary cases. Beri contoh konkret.

### 9. What is the difference between verification and validation?
**Arah:** verification = "are we building the product right?" (sesuai spec). Validation = "are we building the right product?" (sesuai kebutuhan user).

### 10. When should you NOT automate a test?
**Arah:** test sekali jalan, exploratory testing, UX/usability judgement, dan area yang UI-nya masih sering berubah. Mention ROI: automation butuh maintenance cost.

### 11. How do you handle a flaky test?
**Arah:** jangan cuma re-run. Cari root cause: timing/async issue, dependency antar test, environment tidak stabil, selector rapuh. Isolate masalah, perbaiki sumbernya, jangan tandai sebagai "known issue".

### 12. What is your approach to writing a good test case?
**Arah:** jelas, independent, reproducible, punya expected result eksplisit, mencakup positive/negative/boundary cases. Hindari test yang bergantung pada test lain.

### 13. How would you test a feature like flight/hotel search?
**Arah progresif:** requirement review → test scenario → positive/negative cases → boundary cases (0 penumpang, max penumpang, tanggal lampau, tanggal sama) → API layer → database validation → UI/UX → performance → regression. Sebutkan bahwa kamu definisikan scope dulu sebelum menulis test case.

### 14. What is the difference between severity and priority? Give an example.
**Arah:** severity = dampak teknis; priority = urgensi perbaikan. Contoh: typo pada halaman utama = severity Low, priority High (branding).

### 15. How do you handle disagreement with a developer who says "it works as designed"?
**Arah:** cek requirement/acceptance criteria → tunjukkan langkah reproduksi + evidence (screenshot, log, environment) → diskusi terbuka, mungkin ada konteks yang kamu tidak tahu → eskalasi ke Product Owner/BA kalau masih tidak sepakat → dokumentasikan keputusan akhir.

---

## BEHAVIORAL (STAR METHOD)

### 16. Tell me about a time you found a critical issue / bug.
**Arah:** pakai pengalaman nyata dari proyek. Struktur: Situation → Task → Action → Result → Lesson.

### 17. Tell me about a time you had to learn something new quickly.
**Arah:** contoh kuat: mempelajari teknologi baru dari nol dan menyelesaikannya dalam waktu terbatas.

### 18. Describe a time you worked in a team and had a conflict.
**Arah:** tunjukkan kamu mencari akar masalah komunikasi dulu, bukan menyalahkan. Hasil konkret.

### 19. Tell me about a time you failed or made a mistake.
**Arah:** pilih kegagalan nyata (bukan fake weakness). Fokus ke apa yang kamu pelajari dan ubah setelahnya.

### 20. How do you handle tight deadlines or pressure?
**Arah:** prioritization matrix, minimum viable deliverable, komunikasi proaktif ke stakeholder kalau ada risiko.

### 21. Tell me about a project you're most proud of.
**Arah:** pilih satu, jelaskan kontribusi PERSONAL kamu (bukan "kami"), sebut keputusan teknis yang kamu ambil, dan hasil yang terukur.

### 22. How do you handle feedback or criticism?
**Arah:** feedback = data untuk improvement. Contoh nyata: menerima koreksi, bertanya spesifik, memperbaiki, hasil membaik.

### 23. Are you comfortable working in a fast-paced Agile environment?
**Arah:** sebutkan pemahaman Agile/Scrum: sprint, daily standup, retrospective. Tunjukkan kamu pernah kerja dengan ritme iteratif.

---

## TECHNICAL FUNDAMENTALS (level VP — kemungkinan ringan, tapi bisa muncul)

### 24. What is the difference between QA and QC?
**Arah:** QA = process-oriented, mencegah defect. QC = product-oriented, mendeteksi defect. Testing = aktivitas eksekusinya.

### 25. Explain the bug life cycle.
**Arah:** New → Assigned → Open → Fixed → Verify → Closed (dengan cabang Reopen dan Deferred).

### 26. What are the different levels of testing?
**Arah:** Unit → Integration → System → Acceptance. Jelaskan singkat tiap level.

### 27. What is regression testing and when do you run it?
**Arah:** menjalankan ulang test yang ada setelah perubahan kode, untuk memastikan fitur lama tidak rusak. Dijalankan setiap ada perubahan signifikan atau sebelum release.

### 28. What is smoke testing vs sanity testing?
**Arah:** smoke = cek cepat apakah build layak untuk diuji lebih lanjut. Sanity = verifikasi terfokus bahwa perbaikan bug bekerja.

### 29. Explain boundary value analysis with an example.
**Arah:** menguji nilai di batas dan sekitarnya, karena bug sering tersembunyi di boundary. Contoh: field dengan max 9 → test 8, 9, 10.

### 30. What tools have you used for testing or automation?
**Arah:** sebutkan apa yang benar-benar pernah kamu pakai (misal browser automation dengan Playwright/Cypress, API testing dengan Postman). Jujur soal level pengalaman.

### 31. What is the difference between black-box and white-box testing?
**Arah:** black-box = tanpa melihat kode, fokus input-output. White-box = dengan pengetahuan kode, memeriksa branch dan kondisi.

### 32. Have you written any automated tests before?
**Arah:** jawab jujur. Kalau belum banyak, tunjukkan pemahaman konsep dan antusiasme belajar tool yang tim pakai.

---

## SITUATIONAL / SCENARIO

### 33. Imagine you find a bug right before a major release. What do you do?
**Arah:** nilai severity & priority → assess dampak ke user → komunikasi cepat ke lead/PM → beri rekomendasi (block release, hotfix, atau catat sebagai known issue) → dokumentasikan dengan jelas.

### 34. A bug is reported by a user but you cannot reproduce it. What do you do?
**Arah:** kumpulkan detail dari reporter (langkah, environment, waktu, screenshot) → coba berbagai environment/data → cek log dan monitoring → cek apakah intermittent → dokumentasikan findings → jangan tutup sebagai "cannot reproduce" tanpa investigasi.

### 35. How would you prioritize 20 test cases with only 2 hours available?
**Arah:** risk-based: critical path dulu, lalu area yang baru berubah, lalu test dengan probabilitas kegagalan tinggi. Komunikasikan trade-off ke stakeholder.

### 36. If a developer pushes back on your bug report, what do you do?
**Arah:** sama seperti nomor 15 — requirement check, evidence, diskusi, eskalasi, dokumentasi.

---

## CLOSING

### 37. Do you have any questions for us?

**Pertanyaan yang bagus untuk VP:**
- "How do you see the SQA team at tiket.com evolving over the next 1-2 years, especially regarding test automation?"
- "What are the biggest quality engineering challenges at tiket.com's scale, serving 50+ million users?"
- "What does a successful SDET intern look like in their first three months on your team?"
- "How do the SDET and development teams collaborate in the release process?"
- "What testing practices or infrastructure are you most proud of in the team right now?"

**Hindari:** pertanyaan soal gaji (simpan untuk tahap offer), pertanyaan yang jawabannya ada di website, pertanyaan yang menunjukkan kamu tidak riset.

---

## JAWABAN YANG HARUS DIHINDARI

- Jangan bilang "I just want to learn" tanpa menunjukkan kontribusi yang bisa kamu beri.
- Jangan mengklaim pengalaman yang tidak kamu miliki — follow-up akan menjebak.
- Jangan menyebut competitor secara negatif.
- Jangan bilang kamu belum pernah menulis test sama sekali tanpa menambahkan rencana belajar konkret.

---

## CHECKLIST MALAM INI

- Accept calendar invitation dari Lalita Pathak
- Test Google Meet: kamera, mikrofon, koneksi
- Hafal alur: Tell me about yourself (60 detik), Why SDET, Why tiket.com
- Review: severity vs priority, bug life cycle, test levels, QA vs QC
- Siapkan 3 pertanyaan untuk VP (dari daftar di atas)
- Latihan suara keras minimal 3x untuk jawaban nomor 1-5
