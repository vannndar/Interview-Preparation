# tiket.com SDET Intern — VP Interview Answers
## Bhupesh Mittal (VP - SQA) | 17 Sept 2026, 18:00/18:30 WIB | 45 min

**Fokus dari HR:** positive & negative test cases.

**Cara pakai file ini:** hafal *alurnya*, jangan hafal *katanya*. Kalau dihafal kata per kata, kedengaran seperti membaca.

---

# BAGIAN 1 — PEMBUKAAN

## Tell me about yourself

> "Hi, I'm Thariq, but most people call me Ivan. I'm a Computer Science graduate from ITS.
>
> I've liked programming since high school, and during university most of my work was around AI and data. I was in the UAV team at Bayucaraka, I did an internship as an AI engineer at PLN Nusantara Power, and for my final project I built a RAG chatbot for Indonesian legislation.
>
> From all of those, the part I enjoy most is making sure things actually work — figuring out where they break. That's why I applied for this role."

*(~40 detik. Jangan tambah lagi.)*

---

## Why SDET? Why not just be a developer?

> "Honestly, it came from my own projects. When I built the RAG chatbot, the most useful work wasn't building the pipeline — it was evaluating it. That's when I found out it wasn't working as well as I thought.
>
> I like writing code, and I like questioning whether things really work. SDET is where those two meet. As a developer, you test your own assumptions — you test the happy path you designed for. I'm more interested in the other side, where things break in ways nobody planned."

---

## Why tiket.com?

> "Two things. First, the domain. Travel booking depends on a lot of partners — airlines, hotels, local operators — and every one of those is an integration point that can fail. That's an interesting testing problem, not just UI testing.
>
> Second, the scale. 50 million users means a bug isn't abstract — someone's trip is affected. I find that motivating."

---

## Why should we hire you?

> "I'll be honest: I don't have professional testing experience yet. That's why I'm applying for an internship, not a junior role.
>
> What I do have: I've built systems myself, so I know where software breaks — at integration points, at the edges, when two things happen at once. And I already have some testing experience from university. In the competitive programming committee, my job was writing and checking the test cases for the problems. That's test design work — I had to make sure the tests would catch wrong solutions.
>
> I'm also used to learning things quickly under real constraints, so I don't think the tooling will be a problem."

---

# BAGIAN 2 — POSITIVE & NEGATIVE TEST CASES
### ← ini inti sesinya

## What's the difference between positive and negative test cases?

> "A positive test case gives valid input and expects the system to succeed.
>
> A negative test case gives invalid input and expects the system to handle it properly. And I think the important part is what 'properly' means — not just that it fails, but that it fails cleanly: a clear error message, no crash, and no corrupted data."

*(Kalimat terakhir itu yang bikin jawabanmu beda dari kandidat lain. Jangan dilewat.)*

---

## Give me positive and negative test cases for a login feature

**Positive:**
1. Valid email + valid password → logged in, redirected to homepage
2. Email with different casing (IVAN@mail.com) → still accepted
3. "Remember me" checked → session survives browser restart
4. Login with Google → account linked
5. Space at the start or end of the email → trimmed, login works

**Negative:**
1. Correct email, wrong password → "incorrect password", no login
2. Email not registered → error message, but not one that reveals whether the email exists
3. Empty email or empty password → "this field is required"
4. Wrong email format (ivan@, ivan.com) → format error
5. Password with wrong casing (Pass@123 vs pass@123) → must fail
6. 5 failed attempts in a row → account locked or rate limited
7. SQL injection in the email field → rejected, never executed
8. Double-clicking the Login button → only one login attempt

**Boundary:**
- Email at maximum length → accepted; one character over → rejected
- Password at minimum length → accepted; one below → rejected

*(Kalau ditanya "apa lagi?" — sebut: "I'd also check what happens if the network drops during login, and whether the account gets locked in a way the user can recover from.")*

---

## What is boundary value analysis?

> "It's testing the values right at the edge of a range, because that's where bugs usually hide — usually off-by-one errors.
>
> For example, if a passenger field accepts 1 to 9, I'd test 0, 1, 9, and 10. I wouldn't test just 5, because that would never find a problem with the validation."

---

## What is equivalence partitioning?

> "It's grouping inputs that the system should treat the same way, then testing one from each group instead of testing everything.
>
> Same passenger field: valid is 1–9, too low is below 1, too high is above 9. Testing 5, 0, and 10 covers all three. Boundary testing then checks the edges of those same groups."

---

## What's the difference between a test scenario and a test case?

> "A scenario is what you're testing — like 'verify the login flow.' A test case is how you test it — the input, the steps, and the expected result.
>
> One scenario usually becomes many test cases: valid login, wrong password, empty fields, locked account — all different cases under the same scenario."

---

## How do you decide what to test first?

> "By risk. First question: does it affect revenue or a core user journey? On a travel platform, anything in booking and payment comes first — if the user can't complete a booking, nothing else matters.
>
> Second: how recently did it change, and how many external systems does it touch? Something that just changed and talks to three APIs is riskier than something stable.
>
> And if I run out of time, I'd say what I didn't cover instead of pretending I covered everything."

---

## 30 minutes to test a new feature — what do you do?

> "First couple of minutes to understand what the feature does and what the critical path is.
>
> Then most of the time on that critical path, because if it's broken nothing else matters. Then the risky edge cases — empty input, invalid input, and the failure modes that could corrupt data.
>
> What I'd skip: cross-browser checks and performance. But I'd say that out loud, so whoever asked me knows what's still uncovered."

---

## Payment bug vs profile picture bug — which first?

> "Payment, but the reasoning matters more than the answer.
>
> Payment affects revenue and it can create real financial inconsistency — a user charged without a booking, or a booking without a charge. That's critical.
>
> Profile picture is a small number of users, no financial impact, and it can wait for the next release without blocking anything.
>
> So I'd prioritise by impact on users and on the business, not by how interesting the bug is to fix. Unless the profile picture bug blocked something bigger — like registration."

---

# BAGIAN 3 — BUG REPORTING

## Tell me about a bug you found

> "When I was in the competitive programming committee for Schematics, my job was writing and checking the test cases for the problems.
>
> I found one problem where a brute-force solution was passing, even though it should have timed out. The test cases just weren't big enough to catch it. So the problem looked fine, but it would have accepted wrong solutions and given those contestants full marks.
>
> I fixed it by adding larger test cases designed to break that approach.
>
> It stuck with me, because a test suite passing doesn't mean the software is correct — it means it passes those tests. If the tests are weak, a green result doesn't tell you much."

---

## How do you report a bug?

> "The goal is that the developer can reproduce it without asking me anything. So: what the bug is, the environment, the steps to reproduce, what happened, and what should have happened. Plus evidence — screenshot or the API response. And my view on severity and priority.
>
> I also try not to leave anything implicit. 'Click the button' isn't enough if it matters which state the page was in."

---

## Severity vs priority?

> "Severity is how bad the impact is. Priority is how urgent it is to fix. They're separate things.
>
> A typo in the logo on the homepage is low severity — nothing breaks — but high priority, because everyone sees it and it's a brand issue.
>
> The other way around: a serious calculation error in a feature that a small internal team uses twice a year is high severity but low priority. It can be scheduled."

---

## Developer says "that's not a bug, it's expected behaviour"

> "First I'd re-read the requirement. If the behaviour contradicts what's written there, then I have something objective to discuss instead of just my opinion.
>
> If the requirement is unclear, then the developer might be right — and the real problem is that the requirement wasn't clear, which is worth fixing anyway.
>
> Then I'd show my reproduction steps and the evidence. And if we still disagree, I'd ask the product owner, because they decide what 'correct' means.
>
> I'd try not to make it personal. Usually it just means the requirement was ambiguous."

---

## How do you debug when you can't reproduce a bug?

> "I'd treat 'can't reproduce' as something to investigate, not a reason to close it.
>
> First I'd ask the reporter for more detail — what device, what account, what time, what they saw. Bugs that only affect some users are usually environment or data dependent.
>
> Then I'd check the logs and monitoring around the time it was reported. That's often where the real signal is when you can't reproduce it yourself.
>
> If it looks intermittent, that changes the hypothesis — intermittent usually means timing or concurrency, not a logic bug.
>
> And I'd write down what I tried, so the next person doesn't repeat it."

---

# BAGIAN 4 — AUTOMATION

## Why do we need automation testing?

> "Mostly for things that need to run over and over — like regression checks on every change. Testing everything manually before each release doesn't scale.
>
> But I don't think the value is that automation finds more bugs than a person. It's that it gives people time to do the exploratory testing that automation can't do."

---

## What should be automated and what should stay manual?

> "Automate things that run often and don't change much — regression, data-driven tests with lots of input combinations, API tests, smoke tests.
>
> Keep manual: exploratory testing, usability, one-off checks, and features whose UI is still changing a lot — because then the maintenance cost is higher than the benefit.
>
> It's really a cost decision, not a rule."

---

## What automation tools have you used?

> "I should be accurate here — I haven't used Selenium or Playwright in a real testing context yet.
>
> What I have done: I've written Python validation code for my machine learning pipelines, checking data and output correctness. I've tested my own web apps by sending API requests and checking the responses and status codes. And I've set up Docker services with health checks.
>
> So I understand the concepts, but hands-on framework experience is what I want to build here."

*(Jangan mengklaim lebih. Pertanyaan lanjutan akan langsung ketahuan.)*

---

## UI testing vs API testing?

> "UI testing checks what the user sees, end to end — it catches interface and interaction problems, but it's slow and breaks easily when the UI changes.
>
> API testing checks the service directly — request in, response out. It's faster, more stable, and better for testing business logic and error handling.
>
> I'd put as much as possible at the API level and keep UI tests for the important user journeys. UI tests give you the least confidence per unit of effort."

---

# BAGIAN 5 — SCENARIO (paling mungkin ditanya)

## How would you test flight search?

> "I'd start with the happy path: search Jakarta to Bali, a valid date, one passenger — and check the results are actually correct.
>
> Then the negative cases: same origin and destination, a date in the past, return date before departure, zero passengers, more than the maximum, an airport that doesn't exist. And I'd check those fail with a clear message, not a blank page or a crash.
>
> Then the edges: departure today, maximum passenger count, the furthest date you can book.
>
> And the part I'd pay most attention to: the results come from airline APIs. So I'd also check what happens when one of them times out, returns nothing, or returns bad data. And whether the price shown here is the same price shown at checkout — because that mismatch is a real trust problem."

---

## How would you test the payment flow?

> "I'd be stricter here, because the risk is different.
>
> Happy path is a normal successful payment. Negative side: expired card, wrong CVV, invalid card number, not enough balance, invalid promo code, payment timeout.
>
> But the one I'd really insist on is double submission. If the user clicks Pay twice, or the connection drops and they retry, there should still be only one transaction.
>
> And I'd check the database afterwards, not just the confirmation screen — was the booking created once, is the amount correct, does it match what the payment gateway recorded. The confirmation page can look fine while the database has a duplicate."

---

## Users report booking sometimes fails. How would you investigate?

> "First I'd narrow it down, because 'sometimes' isn't testable. Which payment method, which route, which device, what time. The time matters — it often shows whether it relates to peak load.
>
> Then I'd look at the data first, because it's the fastest answer. Are the failures all on one payment method or one supplier? Do we have bookings marked as failed where the payment actually went through? That version would be the most serious, because it means we took money and didn't deliver.
>
> Then the logs, and try to reproduce it once I have a hypothesis — with concurrent requests if it looks like a timing issue.
>
> Usually 'sometimes' has a pattern. The job is finding what's different between the failures and the successes."

---

## A feature works in Chrome but fails in Safari

> "First I'd pin down exactly what fails and whether it's consistent, because that already tells me something.
>
> The usual causes are JavaScript features Safari handles differently, date and timezone handling — Safari and Chrome really do differ there, and it causes real bugs with date pickers — or CSS differences. The browser console usually shows the mechanism.
>
> Then I'd report it with the exact environment, and check whether Safari is even in our supported browsers. If it is, it's a real bug. If not, it's a known limitation.
>
> Longer term it shouldn't reach users — critical journeys should have cross-browser coverage."

---

## How would you test an application with millions of users?

> "At that scale you can't test everything, so it becomes more about managing risk.
>
> I'd focus on a few things: load and performance testing on the highest-traffic flows, because at that scale the failure mode is usually capacity rather than a logic bug. Different devices and networks, since a travel app gets used in very different conditions. The third-party integrations, because that's where a lot of real incidents come from. And production monitoring and staged rollouts, because you can't pre-test everything — you need to detect problems fast and roll back.
>
> The mindset shifts from 'did we test everything' to 'how do we catch what we missed'."

---

# BAGIAN 6 — SQL & API

## What is SQL used for in testing?

> "Mainly to check things the UI can't show me. After a booking, does the record exist exactly once, are the values right, does the payment status match the booking status.
>
> Also for setting up test data and cleaning it up after, so tests don't depend on each other."

---

## Difference between WHERE and HAVING?

> "WHERE filters rows before grouping, HAVING filters groups after. That's why you can't use an aggregate function in WHERE — the grouping hasn't happened yet.
>
> So to find users with more than three orders, I'd count in HAVING, not WHERE."

---

## Write a query to find duplicate data

```sql
SELECT user_id, offer_id, DATE(created_at) AS booking_date, COUNT(*) AS total
FROM orders
WHERE status = 'confirmed'
GROUP BY user_id, offer_id, DATE(created_at)
HAVING COUNT(*) > 1;
```

> "That last one is what I'd actually run after a payment change. The same user getting two confirmations for the same offer on the same day usually means a double-submit problem. It's a data-level check for something that's very hard to see at the UI."

---

## Users who completed orders — and what else a tester would check

```sql
-- the question itself
SELECT DISTINCT u.user_id, u.name
FROM users u
INNER JOIN orders o ON u.user_id = o.user_id
WHERE o.status = 'completed';

-- orders with no valid user (should never exist)
SELECT o.order_id FROM orders o
LEFT JOIN users u ON o.user_id = u.user_id
WHERE u.user_id IS NULL;

-- duplicate confirmations
SELECT user_id, COUNT(*) FROM orders
WHERE status = 'completed'
GROUP BY user_id HAVING COUNT(*) > 1;
```

> "The first one is a reporting question. The second one is a data integrity question — and that's the kind of thing no UI test would ever show you."

---

## What is API testing and why not just test the UI?

> "API testing sends requests directly to the service and checks the responses — the data, the status codes, the error handling.
>
> The reason it matters: a UI test tells you something wrong appeared on screen. An API test tells you whether the service returned the wrong data or the UI displayed it wrong. Those are different bugs with different fixes. API tests are also faster and break less when the UI changes."

---

## HTTP status codes — which ones matter in testing?

> "200 for success, 201 for created, 400 for a bad request, 401 when you're not logged in, 403 when you are logged in but not allowed, 404 not found, 429 rate limited, and 500 for a server error.
>
> The one I'd check carefully is 401 versus 403, because they mean different things. And I'd check that error responses actually have a clear message, not just the right code."

---

# BAGIAN 7 — BEHAVIORAL (singkat)

## Tell me about a difficult problem

> "On the UAV team, I worked on fire detection using YOLO and I also worked on the cloud side with AWS.
>
> The model itself wasn't the hard part. The hard part was everything around it — the conditions in the field, getting the data off the aircraft, and the deployment limits. Processing everything on board was too heavy, so we moved part of it to the cloud.
>
> What I took from it is that in a real system, the model is usually not the bottleneck. It's the integration and the constraints around it. That changed how I look at problems — I look at the whole system, not the piece I was assigned."

---

## Tell me about working in a team

> "On the UAV team, my work had to fit with people handling other subsystems — hardware, flight control, software. Everything had to work together for the aircraft to fly.
>
> The thing that helped most was making my part's inputs and outputs clear early, so other people could plan around me and we didn't block each other. And I'd share problems in the shared channel, not save them for the meeting — if my part is blocked, the whole team needs to know.
>
> I learned that finishing my part isn't enough. Everyone else needs to know what my part does and when it'll be ready."

---

## Tell me about a failure

> "My PKM project was a two-way sign language translator using a Transformer. We got the funding, which felt like the idea was validated.
>
> What I underestimated was the data. Sign language data is difficult, and building enough good labelled data was much more work than we planned. We spent too long on data and not enough on actually iterating and evaluating.
>
> What I changed after that is how I plan projects. I now put the data work at the front of the plan, because in machine learning the data pipeline is usually the slow part, not the model. And I try to build the smallest working version first.
>
> I actually applied that when I built my RAG chatbot — I got a minimal end-to-end pipeline working first, and I built the evaluation early so I'd find out quickly if it was actually working."

---

## How do you handle pressure and deadlines?

> "I separate what's essential from what's nice to have, and I do the essential path first. When several deadlines overlap, the useful thing is deciding the minimum acceptable outcome for each — trying to do everything at full quality usually means everything is late.
>
> And I'd say early if something is at risk. The people affected need to know while they can still plan around it."

---

# BAGIAN 8 — PROYEK

## Tell me about your most relevant project

> "The most relevant one is my final project — a RAG chatbot that answers questions about Indonesian legislation, specifically UU TNI.
>
> I loaded the documents, split them into chunks, made embeddings with a multilingual model, and stored them in FAISS. At query time I used hybrid retrieval — combining keyword search with semantic search — and then reranked the results with a cross-encoder before sending them to the language model. The app also shows which documents it used, so the user can check the answer.
>
> But the part I'm most glad I did was the evaluation. I fine-tuned several models and compared RAG against fine-tuning, using LLM-as-judge and other metrics.
>
> And the evaluation told me something I didn't want to hear: the results weren't as good as I thought. That was the most useful part of the project, because it made me find out why — the documents were complex and from different sources, and my retrieval needed work. I added the hybrid retrieval and reranking because of that.
>
> The lesson was that evaluation matters more than the pipeline. Without it you don't know your system is failing."

---

## What was your contribution?

> "I built the RAG system itself — the document processing, the retrieval with hybrid search and reranking, and the app with the source display.
>
> I should be precise here because it was a group project: the fine-tuning and evaluation runs were split across the team, with different models handled by different people, and I did my share of that alongside the RAG work.
>
> So the retrieval and the application are fully mine, and the evaluation was shared."

---

## If you could improve it, what would you change?

> "A few things.
>
> Better evaluation metrics — RAGAS, so I could tell whether the problem was in retrieval or in generation. My evaluation measured the final answer but couldn't tell me where it broke.
>
> Metadata filtering, so a legal text and a news article aren't treated as equally reliable for a legal question.
>
> Query rewriting, because user questions are often vague.
>
> And tests. That project has no automated tests at all. I built it and I evaluated its outputs, but I never wrote anything that would run on every change. Looking back, that's the biggest thing missing — and it's part of why I'm here."

---

# BAGIAN 9 — PENUTUP

## What do you expect from this internship?

> "Three things.
>
> I want to see how testing is actually done at scale. I've built things on my own, but I've never seen how quality is managed professionally, with real processes and real automation.
>
> I want to work with people who are better at this than I am. I've been self-taught in a lot of areas, and that's the fastest way to close the gaps.
>
> And I want to own something real, even if it's small. I'm not looking for somewhere to just observe."

---

## Do you have any questions for us?

Pick 2–3:

1. "What would a successful SDET intern look like in their first three months on your team?"
2. "How do the SDETs and developers work together in the release process — same squad, or separate teams?"
3. "What does the automation coverage look like today, and what are you working towards?"
4. "What's the hardest quality problem the team has faced recently?"

**Jangan tanya** soal gaji di tahap ini.

---

# CARA BIAR KEDENGARAN NATURAL

**Yang bikin jawaban terdengar scripted:**
- Kalimat terlalu rapi dan panjang
- Membuka dengan "That's a great question. Let me structure this..."
- Pakai kata seperti "leverage", "robust", "utilize"
- Terlalu banyak poin bernomor

**Yang bikin terdengar natural tapi tetap formal:**
- "Honestly, ..." / "For me, ..." / "I think the important part is ..."
- "So usually what I'd do is ..."
- Berhenti sebentar sebelum menjawab pertanyaan sulit — itu terdengar berpikir, bukan bingung
- Kalau tidak tahu: "I haven't done that directly. Based on what I understand, I'd probably approach it like this — but I'd want to check with someone who's done it."
- Kalau salah ucap: perbaiki saja, jangan minta maaf berlebihan

**Tiga hal yang jangan sampai salah:**
1. Jangan mengklaim pernah pakai Selenium/Playwright kalau belum
2. Jangan bilang "I just want to learn" tanpa menyebut apa yang bisa kamu beri
3. Kalau ditanya test case, **jangan berhenti di positive** — negative-nya yang mereka tunggu

---

# LATIHAN 30 MENIT TERAKHIR

Ambil satu fitur. Ucapkan keras-keras, tanpa baca:
- 5 positive test cases
- 5 negative test cases
- 3 boundary cases

Lakukan untuk **flight search** dan **payment**. Kalau dua-duanya lancar, kamu siap.
