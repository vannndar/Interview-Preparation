# Bug Story — versi QA/VP (yang ini dipakai di interview)

Semua dari LearningWithUs (produksi, live). Semua git-verified. Ordered: pakai mana dulu.

Feedback #8 minta "production bug story". Cerita migration/UUID (file versi panjang) di-drop —
itu bug developer, bukan bug yang VP SQA anggap menarik. Yang menarik buat VP:
**bug yang lolos test, kelihatan ke user, dan bisa dicegah oleh automation.**

---

## STORY 1 (PAKAI INI) — Duplicate note saat user retry save

**Versi STAR, natural spoken (~40 detik)**

"So this one is from my own app. It is a study planner with notes, and saving a note is not a single write. It writes the note, then it syncs the tags, then it awards XP.

The problem was that those were separate requests. So if the part after the write failed, the API returned an error but the note was already saved. The user sees an error, presses save again, and now there are two notes. And nothing in the system could tell a retry apart from a new note.

I found it while going through the save flow, not from a user report. My goal after that was simple: make save safe to retry, because a user who sees an error will always try again.

So the fix had two parts. First, I moved the whole save into one database transaction, so it either completes or it does not happen at all. Second, I added an idempotency key, so if the same save is sent twice, the second one returns the original note instead of creating a new one.

After that, retrying a save is safe and duplicate notes from a failed save do not happen. And the failure case that caused it is now a test, not a hope."

**Versi 30 detik (yang ini dipakai)**

"My note app split saving into several requests. If the first request succeeded and a later one failed, the user saw an error, even though the note was already saved. So they pressed save again and got two notes.

I found it while reading the save flow. I fixed it in two ways. The whole save now runs in one transaction, so it either completes or nothing happens. And I added an idempotency key, so a second save returns the original note instead of creating a new one.

Now retrying a save is safe — the user can press save again without creating a duplicate note in the database."

**Kalau ditanya "do you have a test for that?" — jujur (jangan overclaim):**

"Not yet. The fix is enforced at the database level, so it holds even if the client misbehaves, but the retry path itself is not in my automated suite yet. That is the gap I would close next."

Catatan: di repo memang BELUM ada integration test untuk create_note_atomic / idempotency
(yang ada: sanitizer, chunking, prompt-builder, api error contract). Jangan bilang "the failure case is a test".
Hindari juga kalimat meta yang minta validasi ("that failure case is a test") — VP membaca itu sebagai klaim kosong.
Tutup dengan hasil, bukan dengan klaim tentang testing.

**Kalau ditanya tool/evidence:** baca route handler-nya step by step, lihat tiap write commit sendiri, cek schema — tidak ada unique key yang bisa nolak write kedua. Setelah fix: fungsi Postgres transaksional + `api_idempotency_keys`, plus test contract error API.

**Kalau ditanya kenapa itu bug:** user melihat error, jadi user mengulang — dan sistem memberi hasil berbeda (2 catatan) untuk satu niat user. Itu silent data corruption dari sudut pandang user.

**VP-relevance (kalau perlu nyambung):** ini kelas bug yang automation harus punya: bukan "does save work", tapi "what happens when save half-works and the user retries".

---

## STORY 2 — Jadwal bisa overlap walau sistemnya punya conflict check

**~25 detik**

"My planner rejects overlapping study sessions, and it also has a ripple: if you drop a session on a busy slot, the conflicting session moves to the next valid slot.

The bug was that the conflict check and the write were separate requests. Two things could happen: two simultaneous moves could both see the same slot as free and both write it, and the ripple moved events one by one — so if one move failed in the middle, the schedule was left half-moved, with a session in two places.

The database had the time range column but no constraint, so nothing at the data layer rejected an overlap.

I moved the conflict-sensitive writes into one transaction and added a database-level exclusion constraint on the user's time range, so an overlap is rejected even if two requests race. A failed ripple now changes nothing.

The lesson is the one I use for testing: an application check is UX, a database constraint is a guarantee. If something must never be true, it has to be enforced where the data lives."

**Evidence:** `lib/scheduling.ts` (check → recursive move → separate updates), migration `202607110001_data_api_correctness.sql` (`exclude using gist (user_id =, tstzrange(start_at, end_at, '[)') &&)`, deferrable), error-code mapping `23P01 → 409 conflict` + tests.

---

## STORY 3 — Login balik ke halaman yang salah

**~20 detik**

"New users were supposed to land on onboarding after login. The redirect target was passed as a parameter in the login link, and it was being dropped in the round trip — so they landed on the default page instead, before finishing setup.

I caught it by signing in through both paths — Google and the email link — and comparing where each one landed. They didn't match.

The fix: the callback now defaults to onboarding and validates the redirect parameter so it can never point off-site, and the landing page hands a stray auth code to the callback. Both login paths land in the same place now.

What I took from it: I tested that login worked. I hadn't tested where login ended."

**Evidence:** `19da7e0 fix(auth): preserve OAuth callback redirect` — callback default `/dashboard` → `/onboarding`, login stops passing `?next=`, `assert.doesNotMatch(login, /auth\/callback\?next=/)`.

---

## Honesty guard (baca sebelum ngomong)

- Ketiga bug ini ditemukan lewat **review flow + failure-path testing**, bukan dari komplain user atau outage. Kalau ditanya "how did you find it", jawab itu — jangan mengarang ada user yang komplain. Justru poinnya: kamu menulis failure case sebelum user kena.
- Jangan sebut nama file/commit di ruangan. Sebut perilakunya (retry, concurrent move, redirect parameter). Kalau dia minta detail teknis, baru turun ke tabel di atas.
- Jangan pakai cerita migration/UUID di round ini.
