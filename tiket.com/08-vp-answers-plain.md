# tiket.com SDET Intern — VP Interview
## Bhupesh Mittal (VP - SQA) | 17 Sept 2026, 18:00/18:30 WIB

Fokus dari HR: positive & negative test cases.

Hafal alurnya, jangan hafal katanya.

---

## Tell me about yourself

> "Hi, I'm Thariq, but most people call me Ivan. I just graduated in Computer Science from ITS.
>
> During university, most of my work was around AI and data. I was in the UAV team at Bayucaraka, I did an internship as an AI engineer at PLN, and my final project was a RAG chatbot for Indonesian law.
>
> In all of those projects, the part I enjoyed most was checking whether things actually work. That's why I applied for this role."

---

## Why SDET?

> "It started from my own projects. When I built the RAG chatbot, the most useful work wasn't building it — it was testing it. That's when I found out it wasn't working as well as I thought.
>
> I like writing code, and I like finding problems before users do. SDET is where both of those come together."

---

## Why tiket.com?

> "The domain. Travel booking depends on a lot of partners — airlines, hotels, local operators — and all of them can fail separately. That makes testing more interesting than just checking a UI.
>
> And the scale. With 50 million users, a bug isn't abstract. Someone's trip is affected."

---

## Why should we hire you?

> "I don't have professional testing experience yet. That's why I'm applying for an internship.
>
> What I do have — I've built systems myself, so I know where things break. And I already have some testing experience from university: in the competitive programming committee, my job was writing and checking the test cases for the problems. I had to make sure the tests actually caught wrong solutions.
>
> I also learn new tools quickly, so I don't think that will be a problem."

---

# TEST CASES ← ini inti sesinya

## Difference between positive and negative test cases?

> "A positive test case uses valid input and expects it to work.
>
> A negative test case uses invalid input and expects the system to handle it properly. The important part is what 'properly' means — a clear error message, no crash, and no broken data."

---

## Positive and negative test cases for login

**Positive:**
1. Correct email and password → logged in
2. Email in different casing → still works
3. Remember me checked → session stays after closing the browser
4. Login with Google → account linked
5. Extra space before or after the email → trimmed, login works

**Negative:**
1. Correct email, wrong password → error message, not logged in
2. Email not registered → error message
3. Empty email or empty password → "required" message
4. Wrong email format → format error
5. Password with wrong casing → must fail
6. Five failed attempts → account locked or limited
7. SQL injection in the email field → rejected
8. Double-clicking Login → only one attempt

**Boundary:**
- Email at maximum length → accepted; one character more → rejected
- Password at minimum length → accepted; one less → rejected

---

## Boundary value analysis

> "Testing the values at the edge of a range, because that's where bugs usually are — normally off-by-one errors.
>
> If a passenger field accepts 1 to 9, I'd test 0, 1, 9, and 10. Testing 5 wouldn't find a problem with the validation."

---

## Equivalence partitioning

> "Grouping inputs that should behave the same way, then testing one from each group.
>
> Same passenger field: valid is 1 to 9, too low is below 1, too high is above 9. Testing 5, 0, and 10 covers all three."

---

## How do you decide what to test first?

> "By risk. First — does it affect revenue or a main user journey? Anything in booking and payment comes first, because if the user can't complete a booking, nothing else matters.
>
> Then — how recently did it change, and how many external systems does it use? Something that just changed and talks to three APIs is riskier than something stable.
>
> And if I run out of time, I'd say what I didn't cover."

---

## Payment bug or profile picture bug first?

> "Payment. It affects revenue, and it can create real financial problems — a user charged without a booking, or a booking without a charge.
>
> Profile picture affects few users and has no financial impact, so it can wait.
>
> I'd decide by impact, not by which bug is more interesting to fix."

---

# BUG STORY

## Tell me about a bug you found

> "In the competitive programming committee for Schematics, my job was writing and checking the test cases for the problems.
>
> I found one problem where a brute-force solution was passing, even though it should have timed out. The test cases weren't big enough to catch it. So the problem looked fine, but it would have accepted wrong solutions and given those contestants full marks.
>
> I fixed it by adding bigger test cases that broke that approach.
>
> It stayed with me because a test suite passing doesn't mean the software is correct. It means it passes those tests."

---

## How do you report a bug?

> "The goal is that the developer can reproduce it without asking me anything. So — what the bug is, the environment, the steps, what happened, and what should have happened. Plus a screenshot or the API response.
>
> I try not to leave anything unclear. 'Click the button' isn't enough if it matters which state the page was in."

---

## Severity vs priority?

> "Severity is how bad the impact is. Priority is how urgent it is to fix.
>
> A typo in the logo on the homepage is low severity but high priority, because everyone sees it.
>
> The opposite — a serious calculation error in a feature a small internal team uses twice a year is high severity but low priority. It can be scheduled."

---

## Developer says "that's not a bug, it's expected behaviour"

> "First I'd re-read the requirement. If the behaviour contradicts it, then I have something objective to discuss instead of just my opinion.
>
> If the requirement is unclear, the developer might be right — and then the real problem is that the requirement wasn't clear.
>
> Then I'd show my reproduction steps. If we still disagree, I'd ask the product owner, because they decide what correct means.
>
> I'd try not to make it personal. Usually it just means the requirement was ambiguous."

---

## Can't reproduce a bug — what do you do?

> "I'd treat it as something to investigate, not a reason to close it.
>
> First I'd ask for more detail — device, account, time, what they saw. Bugs that only affect some users are usually environment or data dependent.
>
> Then I'd check the logs around that time. That's often where the real answer is.
>
> If it looks intermittent, that changes things — intermittent usually means timing or concurrency, not a logic bug."

---

# SCENARIO

## How would you test flight search?

> "I'd start with the normal case — search Jakarta to Bali, valid date, one passenger — and check the results are correct.
>
> Then the negative cases: same origin and destination, date in the past, return date before departure, zero passengers, more than the maximum, an airport that doesn't exist. And check those show a clear message, not a blank page.
>
> Then the edges — departure today, maximum passengers, the furthest date you can book.
>
> And the part I'd watch most: the results come from airline APIs. So I'd test what happens when one times out or returns bad data. And whether the price here matches the price at checkout, because that mismatch is a real trust problem."

---

## How would you test the payment flow?

> "I'd be stricter here because the risk is different.
>
> Normal case is a successful payment. Negative side — expired card, wrong CVV, invalid card number, not enough balance, invalid promo code, payment timeout.
>
> But the one I'd really check is double submission. If the user clicks Pay twice, or the connection drops and they retry, there should still be only one transaction.
>
> And I'd check the database afterwards, not just the confirmation screen — was the booking created once, is the amount correct, does it match the payment gateway. The confirmation page can look fine while the database has a duplicate."

---

## Booking sometimes fails — how do you investigate?

> "First I'd narrow it down, because 'sometimes' isn't testable. Which payment method, which route, which device, what time. The time matters — it often shows if it relates to peak load.
>
> Then I'd look at the data first, because it's the fastest answer. Are the failures all on one payment method or one supplier? Do we have bookings marked failed where the payment actually went through? That one would be the most serious, because it means we took money and didn't deliver.
>
> Then the logs, and try to reproduce it once I have a guess."

---

## Works in Chrome, fails in Safari

> "First I'd find out exactly what fails and whether it's consistent.
>
> The usual causes are JavaScript features Safari handles differently, date and timezone handling — that's a common one with date pickers — or CSS differences. The browser console usually shows why.
>
> Then I'd report it with the exact environment, and check whether Safari is in our supported browsers. If it is, it's a real bug."

---

# AUTOMATION

## Why automation?

> "Mostly for things that need to run over and over, like regression checks on every change. Testing everything manually before each release doesn't scale.
>
> But I don't think the point is that automation finds more bugs. It's that it gives people time to do the exploratory testing that automation can't do."

---

## What should be automated and what should stay manual?

> "Automate things that run often and don't change much — regression, data-driven tests, API tests, smoke tests.
>
> Keep manual — exploratory testing, usability, one-off checks, and features whose UI is still changing a lot, because the maintenance cost is higher than the benefit.
>
> It's a cost decision, not a rule."

---

## What automation tools have you used?

> "I should be accurate — I haven't used Selenium or Playwright in a real testing context yet.
>
> What I have done: I've written Python validation code for my machine learning pipelines to check data and output correctness. I've tested my own web apps by sending API requests and checking responses and status codes. And I've set up Docker services with health checks.
>
> So I understand the concepts, but hands-on framework experience is what I want to build here."

---

## UI testing vs API testing

> "UI testing checks what the user sees, end to end. It catches interface problems, but it's slow and breaks easily when the UI changes.
>
> API testing checks the service directly — request in, response out. Faster, more stable, and better for testing business logic and error handling.
>
> I'd put as much as possible at the API level and keep UI tests for the important user journeys."

---

# SQL

## What is SQL used for in testing?

> "Mainly to check what the UI can't show me. After a booking — does the record exist once, are the values right, does the payment status match the booking status.
>
> Also for setting up test data and cleaning it up after."

---

## WHERE vs HAVING

> "WHERE filters rows before grouping. HAVING filters groups after. That's why you can't use COUNT in WHERE — the grouping hasn't happened yet."

---

## Find duplicate data

```sql
SELECT user_id, offer_id, DATE(created_at) AS booking_date, COUNT(*) AS total
FROM orders
WHERE status = 'confirmed'
GROUP BY user_id, offer_id, DATE(created_at)
HAVING COUNT(*) > 1;
```

> "I'd run this after a payment change. The same user getting two confirmations for the same offer on the same day usually means a double-submit problem."

---

## Users who completed orders

```sql
SELECT DISTINCT u.user_id, u.name
FROM users u
INNER JOIN orders o ON u.user_id = o.user_id
WHERE o.status = 'completed';
```

> "And as a tester I'd also run this one, which is different in nature:"

```sql
SELECT o.order_id FROM orders o
LEFT JOIN users u ON o.user_id = u.user_id
WHERE u.user_id IS NULL;
```

> "The first is a reporting question. The second is a data integrity question — orders that shouldn't exist. No UI test would show you that."

---

# BEHAVIORAL

## Difficult problem you solved

> "On the UAV team I worked on fire detection using YOLO, and also on the cloud side with AWS.
>
> The model wasn't the hard part. The hard part was everything around it — the conditions in the field, getting data off the aircraft, and processing limits. Running everything on board was too heavy, so we moved part of it to the cloud.
>
> What I learned is that in a real system, the model is usually not the bottleneck."

---

## Working in a team

> "On the UAV team, my work had to fit with people doing hardware, flight control, and software. Everything had to work together for the aircraft to fly.
>
> The most useful thing was making my part's inputs and outputs clear early, so other people could plan around me and we didn't block each other.
>
> I learned that finishing my part isn't enough. Everyone needs to know what it does and when it'll be ready."

---

## A failure

> "My PKM project was a sign language translator using a Transformer. We got the funding, which felt like the idea was validated.
>
> What I underestimated was the data. Sign language data is hard, and building enough good labelled data took much longer than we planned. We spent too long on data and not enough on iterating.
>
> After that I changed how I plan projects. I put the data work at the front, because in machine learning the data is usually the slow part, not the model.
>
> I did that when I built my RAG chatbot — I got a small working version first before expanding."

---

## Your project

> "My final project was a RAG chatbot for Indonesian law, specifically UU TNI.
>
> I split the documents into chunks, made embeddings with a multilingual model, and stored them in FAISS. At query time I used hybrid retrieval — keyword search combined with semantic search — then reranked the results before sending them to the language model. The app also shows which documents it used.
>
> The part I'm most glad I did was the evaluation. I fine-tuned several models and compared RAG against fine-tuning.
>
> And the evaluation told me something I didn't want to hear — the results weren't as good as I thought. That was the most useful part, because it made me find out why. The documents were complex and from different sources, and my retrieval needed work. I added the hybrid retrieval because of that.
>
> The lesson was that testing matters more than building. Without it, you don't know your system is failing."

---

## If you could improve it?

> "Better evaluation metrics, so I could tell if the problem was in retrieval or in generation. Metadata filtering, so a legal text and a news article aren't treated the same. Query rewriting, because user questions are often vague.
>
> And tests. That project has no automated tests at all. I built it and I evaluated the output, but I never wrote anything that runs on every change. That's the biggest thing missing — and it's part of why I'm here."

---

## What do you expect from this internship?

> "I want to see how testing is actually done at scale. I've built things on my own, but I've never seen how quality is managed professionally, with real processes and automation.
>
> I want to work with people better at this than me, because that's the fastest way to close the gaps.
>
> And I want to own something real, even if it's small."

---

## Questions for us

Pick 2–3:

1. "What would a successful SDET intern look like in their first three months?"
2. "How do the SDETs and developers work together in the release process?"
3. "What does the automation coverage look like today, and what are you working towards?"
4. "What's the hardest quality problem the team has faced recently?"

Don't ask about salary at this stage.

---

# HAL YANG PERLU DIINGAT

1. Kalau ditanya test case, **jangan berhenti di positive**. Negative-nya yang mereka tunggu.
2. Jangan bilang pernah pakai Selenium kalau belum.
3. Jawaban bagusnya 30–60 detik. Kalau lebih, itu terlalu panjang.
4. Kalau tidak tahu: "I haven't done that directly. I'd probably approach it like this, but I'd want to check with someone who has."
5. Berhenti sebentar sebelum jawab pertanyaan sulit. Itu terlihat berpikir, bukan bingung.

---

# LATIHAN 30 MENIT TERAKHIR

Ambil satu fitur. Ucapkan keras, tanpa baca:
- 5 positive test cases
- 5 negative test cases
- 3 boundary cases

Lakukan untuk **flight search** dan **payment**.
