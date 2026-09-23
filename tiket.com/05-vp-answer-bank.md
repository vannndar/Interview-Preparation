# tiket.com SDET Intern — User Interview (VP Level) — Answer Bank
## Interviewer: Bhupesh Mittal (VP – SQA) | 17 Sept 2026, 18:00/18:30 WIB | 45 min

**INTEL DARI HR:** "Vp round focusing on positive negative test cases"

**Sumber jawaban:** CV (Thariq Ivan Anendar) + repositori GitHub (vannndar). Semua klaim berbasis bukti nyata.

> ⚠️ **PERLU DIKONFIRMASI SEBELUM INTERVIEW:** CV menyebut "Sarjana di Teknik Informatika 2022 – Sekarang" (sedang berkuliah). Pastikan statusmu saat ini (mahasiswa tingkat akhir / sudah lulus) dan pakai framing yang konsisten sepanjang interview.

---

# SECTION 1 — Introduction & Background

## ⭐ Q1. Tell me about yourself. (45–60 detik)

> "Hi, I'm Thariq, but my friends call me Ivan. I'm a Computer Science student at Institut Teknologi Sepuluh Nopember, and I've been programming since high school — what drew me in was the idea that software can solve real problems for real people.
>
> Since then I've been building end-to-end: I worked on a UAV research team where I implemented a fire detection system using YOLO and deployed the mission system on AWS; I did an AI engineering internship at PLN Nusantara Power working on time series models and zero-shot prediction for power plant data; and I built a RAG chatbot called LawBot that answers questions about Indonesian legislation, including fine-tuning several LLMs and evaluating them.
>
> What I realized across all of those projects is that the part I enjoy most is thinking about how systems fail — edge cases, unexpected inputs, integration points. That's exactly what SDET work is about, and it's why I'm here."

**Kenapa ini kuat untuk VP:** menunjukkan journey, bukan daftar skill; menyebutkan bukti konkret (UAV/AWS, PLN, LawBot); dan berakhir dengan *insight* tentang motif memilih SDET — bukan "because I like testing".

---

## Q2. Can you walk me through your educational background and experience?

> "I'm studying Computer Science at ITS, focusing my interest on AI and data. Outside coursework, most of my learning happened through three things:
>
> First, competitive and research organizations — I was on the Schematics NPC-ITS competitive programming committee, where I wrote and validated programming problems. Second, the Bayucaraka UAV team, where I worked on the airframe division: implementing autonomous systems with PX4, building a fire detection feature using YOLO, and deploying our mission system to AWS. Third, an AI engineering internship at PT PLN Nusantara Power, where I explored large time series models and implemented zero-shot prediction on power plant data.
>
> Alongside that I've built personal projects — the most substantial being LawBot, a RAG-based chatbot for Indonesian legislation."

---

## ⭐ Q3. Why are you interested in SDET? (high-probability question)

> "Two reasons.
>
> The first is that SDET combines the two things I'm actually good at — writing code and thinking analytically about failure. In every project I've built, the hardest and most interesting part was never the happy path; it was asking 'what happens if the input is empty, if the API times out, if two users act at the same time.' When I built LawBot, the most valuable work I did wasn't building the pipeline — it was building the evaluation. That's what told me my system wasn't working as well as I thought, and that's when it actually improved.
>
> The second reason is scope. A manual tester verifies behavior; an SDET builds the infrastructure that lets an entire team verify behavior repeatedly and reliably. I want to build things, not just check things — and SDET is the role where engineering and quality meet."

---

## Q4. Why do you want to join tiket.com?

> "Three things stood out to me.
>
> First, the domain complexity. A travel platform isn't one product — it's flights, hotels, ground transport, events, each with different partners, different pricing rules, and different failure modes. Booking a single trip touches dozens of third-party APIs. That's a genuinely hard quality problem, and that's the kind of problem I want to work on.
>
> Second, the scale. 50+ million users means a bug isn't an abstract concept — it's people missing flights. I find that accountability motivating.
>
> Third, the T-visa program itself. I'm at the point where I've built things on my own and I've learned a lot from doing that, but I haven't learned how quality engineering is done properly at scale, with real processes and real standards. That's the gap I want to close, and tiket.com is a place where that knowledge exists."

---

## Q5. What do you know about tiket.com and our product?

> "tiket.com started in 2011 as the first online travel agent in Indonesia. Today it's part of the Blibli Tiket ecosystem — Blibli is listed on the IDX — and it serves more than 50 million users across flights, hotels, ground transport, attractions, events, and travel essentials.
>
> What's relevant from a quality perspective is the architecture: it's a platform that aggregates supply from many partners — airlines, hotel chains, local operators — and any of those integrations can fail independently. That makes integration testing, contract testing, and production monitoring central to quality here, not just UI testing.
>
> On the product side, I've used tiket.com to book travel myself, so I know the customer experience firsthand — including how frustrating it is when something in the booking flow doesn't behave as expected."

*(Catatan: hanya klaim pernah memakai tiket.com jika benar — jangan mengarang.)*

---

## Q6. Why should we hire you as an SDET intern?

> "I'll be honest about where I am: I don't have professional testing experience yet, and that's why I'm applying for an internship rather than a mid-level role.
>
> What I do bring is three things. First, I understand systems from the inside — I've built backends, ML pipelines, and deployments, so I know where software actually breaks: at integration boundaries, under concurrency, and at edge cases. Second, I have a testing instinct that's already shown up in my work: on the competitive programming committee I wrote and validated problems for other people, which means I've already had to think about 'does this test case actually catch the wrong solution?' Third, I learn fast under real constraints — I picked up YOLO and autonomous systems for the UAV team and shipped working features, and I taught myself the full RAG stack to build LawBot.
>
> So what you get is someone with a strong engineering foundation, a genuine testing mindset, and no ego about learning the parts I don't know yet."

---

# SECTION 2 — Understanding of SDET & Testing

## Q7. What is the role of an SDET?

> "An SDET is an engineer who owns quality from a technical side. The core difference from a QA engineer is that the deliverable is code: automation frameworks, test infrastructure, and tools — not just test results.
>
> In practice that means designing test strategy, building and maintaining automation, integrating tests into CI/CD so feedback happens on every change, and working with developers early — reviewing requirements and designs to catch problems before they're built. And an SDET still does exploratory and manual testing where automation doesn't make sense."

## Q8. Difference between QA Engineer, Software Tester, and SDET?

> "A software tester primarily executes tests and reports defects. A QA engineer focuses on process — preventing defects through standards, reviews, and planning. An SDET does all of that but brings engineering: they write the automation, build the frameworks, and treat test code as production code. The easiest way to think about it: the tester verifies the product, the QA engineer improves the process, and the SDET builds the systems that make verification scalable."

## Q9. Why is software testing important?

> "Because the cost of a defect increases the later it's found. A defect caught in code review costs minutes; the same defect in production costs engineering time, customer trust, and sometimes money. At tiket.com's scale, a bug in the payment flow isn't a ticket — it's failed transactions and lost revenue.
>
> There's also a second purpose people underestimate: testing provides the confidence a team needs to ship quickly. Good test coverage is what makes fast releases safe."

## Q10. What is the software testing lifecycle (STLC)?

> "Requirement analysis, test planning, test case development, environment setup, test execution, defect reporting and retesting, and test closure. What matters more than memorizing the phases is that a tester is involved from the first phase — in requirement analysis, the tester's job is to find ambiguity and gaps before anyone writes code. Ambiguous requirements are one of the biggest sources of defects."

## Q11. Where does testing happen in SDLC?

> "Ideally at every stage, and this is what shift-left means. Requirement reviews catch ambiguity. Design reviews catch architectural risk. Unit tests catch logic errors. Integration tests catch interface problems. System and acceptance testing validate end-to-end behavior. And after release, monitoring and production testing close the loop.
>
> The point is that testing isn't a phase at the end of the SDLC — it's a thread that runs through it. The earlier defects are found, the cheaper they are."

## Q12. What makes a good test engineer?

> "Curiosity and skepticism first — a good tester genuinely enjoys asking 'what if this goes wrong?'. Attention to detail, because bugs hide in the gap between what the requirement says and what it means. Communication, because a bug report that a developer can't reproduce is useless. And discipline — the ability to be systematic rather than random, and to document things reproducibly.
>
> There's also a quality I've learned matters: intellectual honesty. It's easy to assume your tests are good; it's much harder to ask whether they'd actually fail if the code were broken."

---

# SECTION 3 — Manual Testing Fundamentals ← **FOKUS VP ROUND**

## Q13. Difference between test scenario and test case?

> "A test scenario is what to test — a functional area, like 'verify the login flow.' A test case is how to test it — the specific input, steps, and expected result. One scenario usually expands into many test cases: valid login, invalid password, empty fields, locked account, all belong to the same scenario.
>
> The mental model I use: scenario is the coverage checklist, test case is the executable unit."

## Q14. How do you create a good test case?

> "It needs to be specific, independent, and verifiable. That means a clear precondition, explicit steps, and an explicit expected result — not 'verify it works' but 'the system responds with HTTP 200 and redirects to the dashboard.'
>
> I also make sure each test case is independent: it should not depend on a previous test having run. And I deliberately cover positive, negative, and boundary cases for each requirement, because tests that only cover the happy path give false confidence."

## ⭐ Q15. What are positive and negative test cases?

> "A positive test case uses valid input and expects the system to succeed. A negative test case uses invalid or unexpected input and expects the system to handle it correctly.
>
> The important nuance — and I think this is where people often get it wrong — is what 'handle it correctly' means in a negative case. The expected result isn't just 'it fails.' It's that the system fails **gracefully**: it returns a clear error message, it doesn't crash, and it doesn't corrupt data or leave the system in an inconsistent state. That's the standard I'd hold negative test cases to."

## ⭐ Q16. Give examples of positive and negative test cases for a login feature.

**Positive test cases:**
1. Valid email and valid password → user logged in and redirected to homepage
2. Login via Google/Apple OAuth → account linked and session created
3. Email entered with different casing (IVAN@mail.com) → still accepted, since emails are case-insensitive
4. "Remember me" enabled → session persists after browser restart
5. Leading or trailing whitespace in email → trimmed automatically and login succeeds
6. Login after password reset using the new password → succeeds

**Negative test cases:**
1. Valid email with wrong password → "incorrect password" message, no login, no hint about which field is wrong
2. Unregistered email → generic error (not "email not found", which would enable user enumeration)
3. Empty email → inline validation "email is required"
4. Empty password → inline validation "password is required"
5. Malformed email format (ivan@, ivan.com, @mail.com) → format validation error
6. Password case sensitivity: `Pass@123` vs `pass@123` → must fail
7. Five consecutive failed attempts → account temporarily locked or rate-limited
8. SQL injection in the email field (`' OR 1=1--`) → rejected, never executed
9. XSS payload in input (`<script>alert(1)</script>`) → escaped, never executed
10. Double-clicking the login button → only one authentication request processed
11. Accessing the dashboard URL directly without a session → redirected to login
12. Network failure during login → informative error, not a hanging spinner

**Boundary cases:**
- Email at maximum allowed length (e.g. 254 characters) → accepted
- Email one character over the limit → rejected with a clear message
- Password at minimum length → accepted; one character below → rejected

## Q17. What is boundary value analysis? Give an example.

> "Boundary value analysis tests the values at the edges of an input range, because defects cluster there — off-by-one errors, incorrect comparison operators, and rounding problems all show up at boundaries.
>
> Example: a passenger count field that accepts 1 to 9. Boundary testing means testing 0 (below minimum), 1 (minimum), 2 (just above minimum), 8 (just below maximum), 9 (maximum), and 10 (above maximum). If you only test 5, you'd never find an off-by-one error in the validation."

## Q18. What is equivalence partitioning?

> "It's dividing input data into groups that the system should treat identically, then testing one representative value from each group rather than every possible value. It's how you get coverage without testing infinitely many inputs.
>
> For example, the same passenger count field: valid group is 1–9, invalid-low is anything below 1, invalid-high is anything above 9. Testing 5, 0, and 10 covers all three partitions. Boundary value analysis complements it by testing the edges of those same partitions."

## ⭐ Q19. How do you decide what should be tested first? (high-probability)

> "Risk-based prioritization. I score on two axes: impact if it fails, and likelihood of failing.
>
> **Impact** means: does this block revenue, corrupt data, or affect a large number of users? On a travel platform, anything in the booking and payment path is maximum impact — if a user can't complete a booking, nothing else matters.
>
> **Likelihood** means: how recently did this change, how complex is it, how many integrations does it touch? A feature with a recent code change and three external API dependencies is riskier than a stable, self-contained feature.
>
> So my ordering is: critical revenue path first, then recently-changed or complex areas, then high-likelihood edge cases, then everything else. And I communicate that trade-off explicitly — if someone asks why I didn't test something, I'd rather explain a deliberate prioritization decision than admit I ran out of time by accident."

---

# SECTION 4 — Test Strategy & Prioritization

## Q20. You have only 30 minutes to test a new feature. What do you do?

> "First, two minutes on scope: what does this feature do, what's the critical path, and what changed.
>
> Then I'd spend most of the time on the critical path — the end-to-end flow a real user takes — because if that's broken, nothing else matters. Then the highest-risk edge cases: invalid inputs, empty states, and the failure modes most likely to cause data problems.
>
> What I would deliberately skip is exhaustive boundary testing and cross-browser checks, and I'd say so explicitly to whoever assigned the task: 'I covered the critical path and these edge cases; I have not covered browser compatibility or performance — those remain open risks.' A tester who communicates what they didn't test is more useful than one who claims everything passed."

## Q21. How do you prioritize your testing?

> "By risk, then by dependency order. Risk = impact × likelihood. Dependency matters because some tests can't run until others pass — there's no point testing the checkout flow if login is broken.
>
> In a sprint context I'd also align with what's changing: the areas most affected by this release get the deepest coverage, and stable areas get covered by the regression suite rather than fresh manual effort."

## Q22. How do you decide which features have higher risk?

> "Four signals. Business criticality — does this touch money or core user journeys? Change frequency and recency — recently modified code is where defects are. Complexity and integration count — the more external systems involved, the more places it can fail. And historical defect density — if a feature has had bugs before, it will again.
>
> Applied here: booking and payment would score highest on all four at tiket.com."

## Q23. Payment bug vs profile picture upload bug — which first?

> "Payment, clearly — but let me be precise about why, because the reasoning matters more than the answer.
>
> Payment: critical business impact, affects revenue directly, and a bug there can create financial inconsistency — a user charged without a booking, or a booking without a charge. That's severity critical and priority urgent.
>
> Profile picture upload: affects a small subset of users, has no financial impact, and can be deferred to the next release without blocking anything. Severity low to medium, priority low.
>
> The general principle is that I prioritize by business impact and user harm, not by how interesting the bug is to fix. And if the picture upload bug were blocking something else — like it prevented users from completing registration — I'd re-evaluate."

## Q24. How do you ensure your testing coverage is enough?

> "Coverage should be measured against requirements, not against lines of code. My approach has four checks:
>
> One, requirement traceability — every acceptance criterion maps to at least one test case, and I verify that mapping explicitly rather than assuming.
>
> Two, scenario categories — for each feature, have I covered positive, negative, boundary, and non-functional cases? Missing a whole category is the most common coverage gap.
>
> Three, layer coverage — did I test at the UI, the API, and the data layer? Bugs that UI testing can't see are often visible at the API or database layer.
>
> Four, and this is the one I find most valuable: would my tests actually fail if the code were broken? Code coverage tells you what executed, not what was verified. A test with weak assertions can have 100% coverage and catch nothing."

---

# SECTION 5 — Bug Reporting & Debugging

## ⭐ Q25. Tell me about a bug you found. (high-probability)

> "The most instructive one came from the competitive programming committee I was on at Schematics. My job was to write and validate problems for the contest — which meant creating the test cases that determine whether a solution is correct.
>
> I found a case where a problem's test data was too weak. A naive brute-force solution — the kind that would time out on real constraints — was actually passing, because none of the test cases were large enough to expose it. The problem was technically 'working' but it wasn't discriminating: it would have accepted incorrect solutions and given those participants full marks.
>
> I fixed it by generating larger and adversarial test cases specifically designed to break the naive approach.
>
> I remember it because it reframed how I think about testing. A test suite passing doesn't mean the software is correct — it means the software satisfies *those* tests. If the tests are weak, a green suite is just false confidence. That's why I care about whether test cases actually discriminate, not just whether they exist."

**Kenapa jawaban ini kuat untuk VP:** bukan bug kosmetik; menunjukkan pemahaman tentang *kualitas test case itu sendiri*; dan menyimpulkan prinsip yang berlaku langsung ke automation di tiket.com.

## Q26. How do you report a bug to developers?

> "The goal is that a developer can reproduce it in five minutes without asking me anything. So: a clear title with the module and symptom, the environment, the precondition, exact reproduction steps, the actual result, and the expected result. Plus evidence — screenshots, video, or the API request/response. And my severity and priority assessment.
>
> I also try to include my hypothesis about the cause if I have one, but I'm careful to label it as a hypothesis, not a conclusion. And I make sure my steps don't leave anything implicit — 'click the button' isn't enough if it matters which button or what state the page was in."

## Q27. What information should be included in a bug report?

> "Title, environment, precondition, steps to reproduce, actual result, expected result, severity, priority, evidence, and the test data used. And a link to the requirement or acceptance criteria it violates — that's what closes any debate about whether it's really a bug."

## Q28. Difference between severity and priority?

> "Severity is the technical impact of the defect on the system. Priority is how urgently it should be fixed. They're independent, and the interesting cases are where they diverge.
>
> Example: a typo in the site logo on the homepage is severity low — nothing breaks — but priority high, because it's the first thing every user and every stakeholder sees, and it's a brand issue.
>
> The reverse: a feature that has a serious calculation error is severity high, but if it's used by a small internal team once a year, its priority could be low — it can be scheduled rather than hotfixed."

## ⭐ Q29. Developer says "this is not a bug, it's expected behavior." (high-probability)

> "My first move is never to argue — it's to go back to the source of truth. I re-read the requirement and the acceptance criteria. If the behavior contradicts what's written there, I have an objective basis for the discussion rather than one person's interpretation.
>
> If the requirement is genuinely ambiguous or silent, then the developer might be right, and I treat the ambiguity itself as the finding — it means the requirement needs to be clarified.
>
> Then I present evidence: steps to reproduce, environment, and the actual versus expected result. I also ask what context I might be missing — maybe there's a configuration or a business rule I don't know about.
>
> If we still disagree after that, I escalate to the Product Owner, because they own the definition of correct behavior. And whatever the outcome, I document the decision so the same question doesn't come up twice.
>
> The thing I try to avoid is making it personal. The developer isn't wrong for defending their code, and I'm not wrong for raising the concern. The disagreement is usually a sign that the requirement was unclear, and the fix is to make it clear."

## Q30. How do you debug when you cannot reproduce a bug?

> "First, I treat 'cannot reproduce' as a finding to investigate, not a conclusion to close with.
>
> I go back to the reporter and gather everything: exact steps, environment, browser and version, account used, time of occurrence, and whatever they saw. Bugs that only appear for some users are usually environment-dependent or data-dependent.
>
> Then I test systematically: different browsers and devices, different accounts with different data states, different network conditions. I check the logs and monitoring for errors around the reported time — that's often where the real signal is when reproduction fails.
>
> If the report suggests it's intermittent, that reframes it: intermittent failures point to timing, concurrency, or a race condition rather than a logic bug, and I'd approach it with that hypothesis.
>
> And I document everything I tried and what I found. Even a failed reproduction is useful evidence — it tells the next person what's already been ruled out."

---

# SECTION 6 — Automation Testing

## ⭐ Q31. Why do we need automation testing? (high-probability)

> "Three reasons. Repeatability — a regression suite that runs on every commit catches breakage immediately, which is impossible to do manually at that frequency. Consistency — automated tests don't have bad days or skip a step because they're tired. And coverage at scale — once the framework exists, running 500 more tests costs almost nothing, whereas manual testing scales linearly with headcount.
>
> But I'd add a caveat that I think matters: automation's real value isn't that it finds more bugs than manual testing — it's that it lets humans spend their time on the bugs automation *can't* find. Exploratory testing, usability judgement, and complex scenario reasoning are where humans add the most value. Automation buys the time for that."

## Q32. What should be automated vs remain manual?

> "Automate: regression tests, data-driven tests with many input combinations, smoke and sanity suites, API tests, and anything that runs repeatedly in CI. The rule of thumb is high frequency plus high stability.
>
> Keep manual: exploratory testing, usability and UX evaluation, one-off checks, tests that require human judgement, and — importantly — features whose UI is still changing rapidly, because the automation maintenance cost outweighs the benefit.
>
> It's an ROI decision, not a principle. Automating the wrong test gives you a suite that's expensive to maintain and always red."

## Q33. What automation tools have you used?

> "I want to be accurate here rather than impressive. I haven't used Selenium or Playwright in a professional testing context yet.
>
> What I do have is automation-adjacent experience from building systems: I've written Python test and validation code for my machine learning pipelines, where I had to verify data integrity and model output correctness. I've also worked with browser automation and API debugging — sending requests, validating JSON responses, and checking status codes — while developing and testing my own web applications. And I've set up Dockerized services with health checks, which is infrastructure for automated verification.
>
> So I understand the concepts and the tooling landscape, and I'd be honest that hands-on framework experience is what I'm here to build."

*(JANGAN mengklaim pengalaman Selenium/Playwright profesional. Kejujuran di sini justru menaikkan kredibilitas.)*

## Q34. Explain your experience with Selenium.

> "I haven't used Selenium directly yet, so let me be straight about that and tell you what I do understand about it.
>
> Selenium is a browser automation library — it drives a real browser through the WebDriver protocol, so it can interact with a page the way a user does: finding elements, clicking, typing, and reading the resulting DOM. It's the foundation of most Java-based UI automation frameworks.
>
> Where I've done comparable work is that I understand the parts around it from building web applications: I know how the DOM works, how locators can be fragile when the UI changes, and why waits and synchronization are the hardest part of UI automation — timing issues are the main source of flaky tests.
>
> What I'd want to learn hands-on is the framework structure — page objects, driver management, and CI integration."

## Q35. How does Selenium work internally?

> "At a high level: your test code uses the Selenium client library, which sends commands over the W3C WebDriver protocol — typically HTTP — to a browser driver like chromedriver. That driver translates the commands into the browser's own automation interface and executes them, then returns the result.
>
> So the flow is: test code → Selenium client → WebDriver protocol → browser driver → real browser. That's why there's no magic: it's a remote-control protocol, which also explains why synchronization is the hard part — your test runs at a different speed than the browser and network."

## Q36. What are the challenges of automation testing?

> "Four main ones. Flakiness — tests that pass and fail without code changes, usually from timing, test interdependency, or environment instability, and they erode trust in the whole suite. Maintenance — UI automation breaks whenever the UI changes, so locator strategy and page objects matter a lot. Initial investment — a framework takes real time before it produces any value. And knowing what not to automate — a suite that tests everything is a suite that's expensive and slow.
>
> The one I'd emphasize is flakiness, because it's a trust problem as much as a technical one: once a team starts ignoring red builds, the automation has stopped doing its job."

## Q37. How do you maintain automation scripts when the application changes?

> "Structurally, so that changes are localized. Page Object Model is the main technique — all locators and page interactions live in one place per page, so when the UI changes, I update one file rather than fifty tests. Beyond that: prefer stable, semantic locators — data-testid attributes or accessibility roles — over fragile CSS chains tied to styling. Keep test and application code separate so application refactors don't break test structure. And keep tests independent, so one broken test doesn't cascade.
>
> The underlying principle is that test code deserves the same design discipline as production code — it's a long-lived asset, and the team pays its maintenance cost every sprint."

## Q38. Difference between UI testing and API testing?

> "UI testing verifies the user-facing interface — it validates the full stack end to end and catches presentation and interaction issues, but it's slow, brittle, and expensive to maintain.
>
> API testing verifies the service layer directly — request in, response out — and it's much faster, more stable, and better at validating business logic, edge cases, and error handling. It also lets you test at a layer where most real defects actually live, without waiting for a UI.
>
> The strategy I'd favor is to push as much validation as possible down to the API and unit level and keep UI tests focused on the critical user journeys. It's the classic testing pyramid, and it's driven by cost: UI tests are the most expensive per unit of confidence they provide."

---

# SECTION 7 — Programming & Technical

## Q39. What programming languages are you comfortable with?

> "Python is my strongest — most of my work has been in it: machine learning pipelines, data processing, backend services, and the RAG system I built. Java and C/C++ I've used in coursework, which means I'm comfortable with OOP, and I know enough to read and write it productively — I've been sharpening my Java specifically because I know it's central to this role. SQL I use regularly for data work. And I've built web applications in TypeScript.
>
> I'd rather be honest about depth than claim fluency in everything."

## Q40. Explain OOP concepts.

> "Four pillars. Encapsulation — bundling data with the methods that operate on it and restricting direct access, so an object's internal state can't be corrupted from outside; in practice, private fields with controlled access. Inheritance — a class extending another to reuse and specialize behavior, which is useful but easy to overuse because it creates tight coupling. Polymorphism — the same interface behaving differently depending on the actual type, which is what lets you write code against an abstraction instead of concrete classes. And abstraction — exposing only the essential behavior and hiding the implementation detail behind an interface.
>
> In a testing context these matter concretely: polymorphism is how a page object or a strategy pattern for different test environments works, and encapsulation is why well-designed test utilities have clear interfaces.
>
> For SDET work specifically, I'd also mention composition over inheritance — a framework built by composing small utilities tends to be far more maintainable than a deep inheritance hierarchy."

## Q41. Difference between array and list?

> "An array is fixed-size and, in Java, holds a single type. A list is a collection interface with dynamic sizing — typically ArrayList, which grows automatically, and LinkedList, which is efficient for insertion in the middle.
>
> Practically: I use arrays when I know the size in advance and want minimal overhead — like the character counting approach for a frequency problem, where a fixed 26-element int array is faster than a hash map. I use lists when the size is dynamic or I need collection operations."

## Q42. How would you handle exceptions in your code?

> "Catch only what I can actually handle, and let everything else propagate. A broad `catch (Exception e)` that swallows errors is one of the worst things you can do — it hides failures and makes debugging much harder.
>
> The pattern I follow: catch specific exception types, log with enough context to diagnose the issue, and either recover meaningfully or rethrow wrapped in application-specific exception. Use try-with-resources for anything that needs closing, so resources are released even on failure. And keep exception handling out of business logic where possible — it clutters the code. In test code specifically, I make failures explicit and readable, because a test that fails without telling you why isn't much use."

## Q43. How do you write clean and maintainable test code?

> "The same way I'd write production code, because that's what test code is. Clear naming that describes the scenario and the expectation, so a failing test name tells you what broke. One logical assertion focus per test, so failures point to a single cause. No dependencies between tests. Test data in fixtures or a shared factory rather than duplicated inline. And no logic — no conditionals or loops that make a test's behavior vary, because then a passing test tells you less.
>
> The measure I care about is: when this test fails six months from now, will the person reading it immediately understand what broke and why? If not, it needs rewriting."

## Q44. Explain your approach when solving programming problems.

> "I try to understand the problem and the constraints before writing anything — what's the input size, because that determines whether an O(n²) approach is acceptable. Then I work through concrete examples by hand, including edge cases: empty input, single element, duplicates, maximum size.
>
> I start with the straightforward correct solution even if it's not optimal, because getting something correct first is higher value than getting something clever that doesn't work. Then I optimize if the constraints require it — which usually means asking whether I can trade space for time, or whether there's a pattern like two pointers or sliding window.
>
> Before I finish, I dry-run the code mentally on the edge cases rather than assuming it works. I've learned to place more emphasis on this — I've seen myself and others lose more time to avoidable edge-case bugs than to algorithmic ones."

---

# SECTION 8 — SQL (PENTING untuk tiket.com)

## ⭐ Q45. What is SQL used for in testing? (high-probability)

> "SQL is how I verify what the UI and API can't tell me — the actual state of the data. Three main uses.
>
> First, data verification: after a booking is made, does the record exist exactly once, with the right values at every level? Is the payment status consistent with the booking status? Second, test data setup and cleanup: creating preconditions and removing test data afterward so tests stay independent. Third, defect investigation: checking whether bad data in the database explains a bug I'm seeing at the UI, or tracing the history of a record that shouldn't exist.
>
> The broader point is that UI-level verification can't catch data problems. You can see a booking confirmation page and still have a duplicate row or an incorrect total in the database."

## Q46. Difference between WHERE and HAVING?

> "WHERE filters rows before aggregation; HAVING filters groups after aggregation. Because of that, WHERE can't reference aggregate functions — that's the classic error people hit.
>
> Example: to find users with more than three orders, I'd filter individual rows with WHERE if needed, group by user, and then use HAVING COUNT(*) > 3. Writing COUNT(*) in a WHERE clause fails, because the grouping hasn't happened yet."

## Q47. Explain INNER JOIN, LEFT JOIN, RIGHT JOIN.

> "INNER JOIN returns only rows where the join condition matches on both sides. LEFT JOIN returns all rows from the left table plus matches from the right — with NULLs where there's no match. RIGHT JOIN is the mirror: all rows from the right, matches from the left.
>
> The practical one for testing is LEFT JOIN, because it's how you find missing relationships — for example, finding users who have never placed an order, or bookings with no corresponding payment record. That's a data integrity check.

## Q48. Write a query to find duplicate data.

```sql
-- Duplicate emails
SELECT email, COUNT(*) AS occurrences
FROM users
GROUP BY email
HAVING COUNT(*) > 1;

-- Full duplicate rows, where the whole record exists twice
SELECT order_id, user_id, status, amount, COUNT(*) AS occurrences
FROM orders
GROUP BY order_id, user_id, status, amount
HAVING COUNT(*) > 1;

-- Find duplicate bookings (same user, same offer, created twice - indicates a double-submit bug)
SELECT user_id, offer_id, DATE(created_at) AS booking_date, COUNT(*) AS bookings
FROM orders
WHERE status = 'confirmed'
GROUP BY user_id, offer_id, DATE(created_at)
HAVING COUNT(*) > 1;
```

> "That last one is the query I'd actually run after a payment-flow change — duplicate confirmations for the same user and offer on the same day is the signature of an idempotency bug. It's a data-level test for a failure mode that's very hard to catch at the UI."

## Q49. Write a query to find the second highest value.

```sql
-- Approach 1: subquery with MAX (no window functions needed)
SELECT MAX(amount) AS second_highest
FROM orders
WHERE amount < (SELECT MAX(amount) FROM orders);

-- Approach 2: window function — more flexible for Nth
SELECT amount
FROM (
    SELECT amount, DENSE_RANK() OVER (ORDER BY amount DESC) AS rnk
    FROM orders
) ranked
WHERE rnk = 2;

-- Approach 3: DISTINCT with LIMIT/OFFSET
SELECT DISTINCT amount
FROM orders
ORDER BY amount DESC
LIMIT 1 OFFSET 1;
```

> "I'd use DENSE_RANK over ROW_NUMBER here, because with duplicate amounts, ROW_NUMBER would give the same logical rank two different numbers. DENSE_RANK handles ties correctly."

## ⭐ Q50. How would you verify booking/payment data using SQL?

Given:
```
Users:  user_id, name
Orders: order_id, user_id, status, amount
```

**The asked query — users who have completed orders:**
```sql
SELECT DISTINCT u.user_id, u.name
FROM users u
INNER JOIN orders o ON u.user_id = o.user_id
WHERE o.status = 'completed';
```

**But as a tester, I'd verify much more than that.** The queries I'd actually run:

```sql
-- 1. Users with NO orders (potential orphaned accounts / data issue)
SELECT u.user_id, u.name
FROM users u
LEFT JOIN orders o ON u.user_id = o.user_id
WHERE o.order_id IS NULL;

-- 2. Orders with NO valid user (referential integrity violation - critical)
SELECT o.order_id, o.user_id
FROM orders o
LEFT JOIN users u ON o.user_id = u.user_id
WHERE u.user_id IS NULL;

-- 3. Duplicate orders for the same user (double-submit / idempotency bug)
SELECT user_id, COUNT(*) AS order_count
FROM orders
WHERE status = 'completed'
GROUP BY user_id
HAVING COUNT(*) > 1;

-- 4. Order status distribution - sanity check after a release
SELECT status, COUNT(*) AS total, SUM(amount) AS total_amount
FROM orders
GROUP BY status
ORDER BY total DESC;

-- 5. Orders with invalid amounts (negative or zero)
SELECT * FROM orders WHERE amount <= 0;

-- 6. Orphaned status: completed but amount is zero
SELECT * FROM orders WHERE status = 'completed' AND amount = 0;
```

> "The distinction I'd draw is that 'users who completed orders' is a reporting question, while the checks below it are integrity questions — and integration questions are usually where the real defects are. Query number two, orders with no valid user, is the kind of thing that should never exist and that no UI test would ever surface."

---

# SECTION 9 — API Testing

## Q51. What is API testing?

> "API testing validates the service layer directly — sending requests and verifying the responses — without going through the UI. It covers correctness of the response data, status codes, error handling, authentication and authorization, and behavior under invalid input.
>
> It's the layer where business logic actually lives, so it's the most efficient place to find defects: faster than UI tests, more stable, and it can catch problems the UI hides."

## Q52. Why do we test APIs instead of only UI?

> "Because the UI is a thin layer over behavior that's mostly determined elsewhere, and many defects are invisible or ambiguous at the UI level. A UI test can tell you that a wrong number appeared on screen; an API test tells you whether the service returned the wrong number or the UI displayed it wrong — which is a completely different bug with a different fix.
>
> Practically: API tests run faster, break less often when the UI changes, cover error handling and edge cases the UI may not even expose, and can be run in CI on every commit. The UI still gets tested — but for user journeys, not for validating business rules."

## Q53. What HTTP methods do you know?

> "GET retrieves data and shouldn't change state. POST creates a resource or submits data. PUT replaces a resource entirely, and it's idempotent — sending it twice produces the same result. PATCH partially updates a resource. DELETE removes a resource.
>
> The distinction that matters most for testing is idempotency: GET, PUT, and DELETE should be safe to retry, while POST typically is not — which is exactly why payment flows need an idempotency key. If a payment POST is retried after a network timeout without one, you get a double charge. That's a case I'd test explicitly."

## Q54. What HTTP status codes do you know?

> "2xx success — 200 OK, 201 Created, 204 No Content. 3xx redirects. 4xx client errors — 400 bad request, 401 unauthenticated, 403 authenticated but not authorized, 404 not found, 409 conflict, 422 validation error, 429 rate limited. 5xx server errors — 500 internal error, 502 bad gateway, 503 unavailable, 504 timeout.
>
> The distinction I pay most attention to in testing is 401 versus 403 — a test that expects 'access denied' should verify which one, because they mean different things: 401 means you're not authenticated, 403 means you are but you're not allowed. And I verify that error responses are actually structured correctly, not just that the code is right."

## Q55. How do you validate an API response?

> "Several layers. Status code first — does it match the expected outcome for this request. Then the response body: correct structure and schema, correct values, and that calculated fields are actually correct rather than just present. Then headers where relevant — content type, caching, rate-limit headers.
>
> Then the negative side, which I think matters more: does an invalid request return the right error code with a clear message, and does it fail without creating partial data? And does authorization hold — can I access another user's resource by changing an ID in the request? That last check is one of the highest-value tests you can write, and it's invisible at the UI level.
>
> Finally, I'd verify at the database layer that the API's claimed effect actually happened."

---

# SECTION 10 — Mobile/Web Testing Scenario ← **INTI SEKSI VP**

## ⭐ Q56. How would you test tiket.com flight search? (high-probability)

**Struktur jawaban (sebutkan dulu, baru detail):**

> "Let me structure this. I'd verify the requirement and scope first, then cover positive cases for the happy path, negative cases for error handling, boundary conditions, and then the non-functional and integration layers. Should I go deep on any particular area?"

**Positive test cases:**
1. Search CGK → DPS, future date, 1 adult → results display with correct routes and prices
2. Round-trip search → both legs shown correctly, return date must be ≥ departure
3. Filters (direct only, airline, departure time) → results filtered correctly
4. Sort by cheapest / fastest → ordering correct, including tie-breaking
5. Passenger composition (adults + children + infants) → pricing rules applied correctly
6. Changing origin/destination → results refresh for the new route
7. Selecting a date from the date picker → correct date applied
8. Result card shows all required info: price, duration, stops, baggage allowance

**Negative test cases:**
1. Same origin and destination → rejected with a clear message
2. Departure date in the past → rejected; date picker disables past dates
3. Return date before departure date → validation error
4. Passenger count of 0 → rejected
5. Passenger count above maximum (10) → rejected
6. Infants exceeding adults → rejected per airline rules
7. Empty origin/destination field → "required" validation
8. Non-existent airport name → "no results found", not a 500 error
9. Special characters in the city field → handled gracefully, no crash
10. Route with no available flights → informative empty state, not a blank page
11. Double-clicking Search → only one request sent
12. Airline API timeout → informative error with retry option, not an indefinite spinner
13. Very broad search (all airlines) → performance remains acceptable, UI doesn't freeze
14. Navigating back after a search → state consistent, filters and dates preserved correctly

**Boundary cases:**
- Departure date = today → must work
- Departure date = tomorrow → must work
- Maximum passenger count (9) → works; 10 → rejected
- Maximum advance booking window → works; one day beyond → rejected
- Minimum search field length → handled

**Non-functional:**
- Performance: results load within acceptable time; verify under concurrent load
- Compatibility: Chrome, Safari, Firefox, mobile web, and the app
- Accessibility: keyboard navigation through filters, screen-reader labels on the date picker

**Integration layer (di mana bug sebenarnya hidup):**
> "And the most important part: the search results come from third-party airline APIs. So I'd test the integration boundary — what happens when a supplier returns malformed data, a partial response, or nothing at all? Whether results from multiple suppliers merge correctly, and whether prices shown are consistent with what the booking flow later confirms. That last one is a real risk: a price displayed in search that doesn't match the price at checkout is a serious trust problem."

---

## ⭐ Q57. How would you test the payment flow? (paling kritikal untuk bisnis)

**Positive test cases:**
1. Successful payment with a valid card → booking confirmed, confirmation email sent
2. Each payment method (card, virtual account, e-wallet) → correct flow and final status
3. Valid promo code → discount applied, final total correct
4. Correct total calculation: base + tax + service fee − discount
5. E-ticket/voucher delivered after successful payment
6. Booking record created with correct values at the database level
7. Cancellation within policy → refund initiated

**Negative test cases:**
1. Expired card → declined with a clear message, no booking created
2. Wrong CVV → declined
3. Invalid card number (fails Luhn check) → rejected before submission
4. Insufficient balance in e-wallet → clear error, no booking
5. Invalid or expired promo code → rejected, price unchanged
6. Already-used promo code → rejected
7. Payment timeout → booking must NOT be marked confirmed; status must be pending or failed
8. User closes browser after clicking pay but before the callback → status must remain consistent
9. **Double-clicking Pay → exactly one transaction created (idempotency — the highest-risk case)**
10. Tampering with the price in the client payload → server rejects and uses server-side pricing
11. Inventory sold out mid-flow → prevented, no overselling
12. Missing or invalid passenger data → per-field validation
13. Network failure during payment → user can retry without being double-charged
14. Refund amount exceeding the original payment → rejected

**Boundary cases:**
- Payment amount at minimum and maximum allowed
- Promo discount equal to the full amount → total must not go negative
- Maximum allowable transaction value

**Concurrency (yang paling sering terlewat):**
> "The case I'd insist on testing is concurrency. Two users attempting to book the last available seat or room at the same moment — exactly one should succeed and the other should get a clear failure, never both. Testing this requires actual concurrent requests, not UI clicks, and it's one of the failure modes that causes the most damage in production."

**Data integrity verification:**
> "And then I'd verify at the database level: was the booking created exactly once? Did inventory decrement by exactly one? Does the amount in our records match what the payment gateway reports? UI confirmation can look perfectly correct while the database contains a duplicate row or a mismatched total."

---

## Q58. Users report that booking sometimes fails. How would you investigate?

> "I'd start by narrowing the report, because 'sometimes' isn't testable. I need to know: how often, which payment method, which route or hotel, which device, and at what time — because timing often reveals whether it correlates with load.
>
> Then I'd check the data first, because it's the fastest source of truth: look at failed bookings, their status values, and the payment gateway records. Are failures concentrated on one payment method or one supplier? Do we have bookings marked failed where the payment actually succeeded — which would mean we're taking money without delivering, the most serious version of this bug.
>
> Then the logs and monitoring for errors around the failure times, and the pattern of whether it correlates with peak traffic, which would point to a timeout or capacity issue rather than a logic bug.
>
> And I'd try to reproduce it deterministically once I have a hypothesis — with concurrent requests if it looks like a race condition. The key thing is that 'sometimes fails' almost always has a pattern, and the job is to find the variable that separates the failures from the successes."

## Q59. A feature works on Chrome but fails on Safari. What do you do?

> "First I'd characterize the difference precisely rather than treating it as 'browser bug' — what exactly fails, and does it fail consistently on Safari or intermittently? Consistency tells me a lot.
>
> Then I'd look for the common reasons: JavaScript features Safari doesn't support or implements differently, date and timezone handling — Safari and Chrome differ here and it causes real bugs with date pickers — CSS differences, and third-party library compatibility. The network tab and console in Safari usually show the mechanism.
>
> Then I'd log it as a compatibility defect with the exact environment details and a clear statement of browser support expectations, because the priority depends on whether Safari is in the supported browser matrix. If it is, it's a real defect; if not, it's a known limitation.
>
> Structurally, the fix is to have cross-browser coverage in the automation suite for critical journeys, so this class of issue is caught before release rather than by users."

## Q60. How would you test an application with millions of users?

> "That scale changes what testing means. You can't test every path, so it becomes a risk management exercise plus heavy reliance on production observability.
>
> I'd focus on four things. First, critical paths at scale — performance and load testing of the highest-traffic flows, because the failure mode at millions of users is usually capacity and concurrency, not logic. Second, location and device diversity — different networks, devices, and regions, since a travel platform is used across very different conditions. Third, third-party dependencies — at this scale, the external APIs and payment gateways are where a large share of incidents originate, so contract testing and graceful degradation matter enormously. Fourth, production monitoring and canary releases — at scale you cannot pre-test everything, so you need the ability to detect problems fast, release gradually, and roll back.
>
> The mental shift is from 'did we test everything?' to 'how do we detect and contain what we missed?'"

---

# SECTION 11 — Behavioral

## ⭐ Q61. Tell me about a difficult problem you solved.

**Situation:** "On the Bayucaraka UAV team, I worked on the Technology Development airframe division, and one of my responsibilities was implementing fire detection using YOLO, running on the aircraft."

**Task:** "The challenge was that the existing model approach wasn't giving us the reliability we needed for a competition context, where an incorrect detection means a failed mission. I had to make the detection work reliably and also get our compute off the aircraft — processing everything onboard was limiting what we could do."

**Action:** "I approached it in two parts. First, on the detection side, I worked through the model and the data — understanding where the failure modes were rather than just tuning parameters, and validating on conditions that matched the real operating environment. Second, on the deployment side, I worked on implementing the cloud component with AWS so that part of the processing could happen off-platform, which is where the freemission-AWS project came from. That meant handling the data flow from the aircraft, the upload, and the processing server-side."

**Result:** "The work contributed to our team's results — we placed third nationally in the Technology Development Airframe division in 2023, first regionally in 2024, and received Best Method nationally in 2024."

**Lesson:** "What I took from it is that in a real system, the hard part is rarely the model itself. It's everything around it — the data conditions, the integration, the deployment constraints. That's changed how I approach problems: I look at the whole system rather than just the piece I'm assigned."

---

## Q62. Tell me about a time you worked with a team.

**Situation:** "On the Bayucaraka UAV team, I was part of the airframe division alongside teammates handling different subsystems — and our hardware, flight control, and software all had to work together for the aircraft to fly."

**Task:** "My work on autonomous systems and fire detection had to integrate with decisions made by people working on other parts of the aircraft, with a fixed deadline for the competition."

**Action:** "The most important thing I did was make my interfaces explicit early — what data my component needed, what it produced, and what conditions it assumed. That way people working on other subsystems could plan around it without us blocking each other. I also made sure to communicate progress and problems in the shared channel rather than only at meetings, because a blocked component is a problem for the whole team, not just me."

**Result:** "We competed successfully across multiple events with the subsystems working together — including the national Best Method award in 2024."

**Lesson:** "I learned that in a team, the most valuable thing I can do isn't just finish my part — it's to make sure everyone else knows what my part does and when it will be ready."

---

## Q63. Tell me about a disagreement with someone. How did you solve it?

**Situation:** "During my work on the UAV team, there was a difference of opinion about approach — I wanted to spend time on something differently than how it had been done before, and there was a reasonable concern about whether that was worth the risk given the competition deadline."

**Task:** "I needed to either make my case properly or accept the alternative — what I couldn't do was just insist and create friction."

**Action:** "I tried to move the discussion from preference to criteria — what are we actually optimizing for here, reliability or time? Once we framed it that way, it became a clearer decision rather than two people defending positions. I also made sure to acknowledge the concern was legitimate, because it was — a deadline is a real constraint, not an obstacle to argue away."

**Result:** "We settled on an approach that satisfied both concerns, and it worked in competition."

**Lesson:** "What I took from it is that most technical disagreements are actually disagreement about priorities rather than about facts. If you can surface the real trade-off, the disagreement usually resolves itself — and the other person is more likely to be right than you expect."

---

## Q64. Tell me about a failure and what you learned.

> "For my PKM project, our team proposed a two-way BISINDO sign language translator using a Transformer-based approach. We received funding in 2023, which felt like validation that the idea was sound.
>
> What I learned was the difference between an idea being good and a project being well-executed. We underestimated how much work the data side would be — sign language datasets are difficult, and building enough quality labelled data was a much larger undertaking than we'd planned for. We spent more time than expected on data collection and preprocessing, and less on iteration and evaluation than the project deserved.
>
> In terms of the outcome, we got the funding and moved the project forward, but I know the results were limited by how we allocated our effort — and that was our planning failure, not a technical one.
>
> What I changed afterwards is how I estimate projects. I now front-load the data work in my planning, because in machine learning, the data pipeline is usually the long pole, not the model. And I try to validate early with the smallest working version rather than assuming the whole plan will hold. I applied that when building LawBot — I built the minimal end-to-end pipeline first and made it work before expanding, and I built the evaluation early specifically so I'd find out quickly whether it was actually working."

---

## Q65. How do you handle pressure and deadlines?

> "I break the problem into what's essential versus what's nice to have, and I prioritize the essential path first. When I had multiple academic deadlines and competition work overlapping, the thing that helped was deciding explicitly what the minimum acceptable outcome was for each — because trying to do everything at full quality under time pressure usually means doing everything late.
>
> The other half is communicating early. If I'm going to miss something or a dependency is at risk, the people affected need to know while they can still plan around it, not after the deadline passes.
>
> And I try to be honest with myself about what I actually can't do in the time available, rather than assuming I'll somehow make it work. That's the failure mode that costs trust."

---

## Q66. How do you communicate with developers and product managers?

> "With developers, I focus on precision and evidence — exact reproduction steps, the environment, the actual versus expected result, and the requirement being violated. I state hypotheses about the cause clearly labelled as hypotheses, because I'm not there to tell them how to write their code.
>
> With product managers, I focus on impact and trade-offs — how many users are affected, what the business risk is, and what the options are. They're making prioritization decisions, so they need impact, not implementation detail.
>
> And with both, I try to be explicit about uncertainty: 'I've verified this' versus 'I suspect this but haven't confirmed it.' Being clear about the difference is what makes your findings trustworthy over time."

---

# SECTION 12 — Project Experience

## Q67. Explain your previous project.

**Gunakan LawBot (paling relevan & solid):**

> "My most substantial project is LawBot — a retrieval-augmented generation chatbot that answers questions about Indonesian legislation, specifically UU TNI No. 34 of 2004 and its revisions.
>
> The pipeline: I loaded source documents in multiple formats — PDF, structured JSON, and web articles — split them into chunks with controlled overlap, generated embeddings using a multilingual sentence-transformer model, and stored them in a FAISS vector index. At query time, I used hybrid retrieval combining keyword-based BM25 search with semantic search, merged the results, and then applied a cross-encoder to rerank them. The top results are passed as context to the language model, which generates an answer grounded in those documents. The interface is a Streamlit app that also displays the source documents used, so the user can verify the answer.
>
> The part I'm most proud of isn't the pipeline — it's the evaluation. I fine-tuned several open-source models using LoRA with 4-bit quantization, then compared RAG against fine-tuning across them, using LLM-as-judge, ROUGE, and BERTScore, tracking everything in Weights & Biases.
>
> And that evaluation told me something uncomfortable: the results were not as good as I'd hoped. Precision was very low for several configurations. That was the most valuable output of the project, because it forced me to understand why — the data was complex and multi-source, and my retrieval needed proper tuning. I added the hybrid retrieval and reranking as a direct response. The lesson is that evaluation is more important than the pipeline, because without it you don't know your system is failing."

*(Alasan proyek ini: paling relevan dengan SDET — menunjukkan kamu membangun sesuatu, mengukurnya, menemukan itu tidak bekerja, dan memperbaikinya. Itu siklus engineering.)*

## Q68. What was your contribution?

> "I built the RAG system — the document processing pipeline, the retrieval layer including the hybrid search and reranking, and the deployment application with multi-model support and source display.
>
> I want to be precise about scope here since it was a collaborative academic project: the model fine-tuning and evaluation runs were spread across the team, with different models handled by different people, and I was responsible for my portion of that evaluation work alongside the RAG implementation.
>
> So the parts that are entirely mine are the retrieval and application side, and the evaluation contributions are shared."

*(Kejujuran tentang pembagian kontribusi akan terdeteksi kalau dibohongi — dan VP-level interviewer akan menghargai ketelitian ini.)*

## Q69. What technical challenge did you face?

> "Retrieval quality — specifically, semantic search alone wasn't finding the right passages for legal questions.
>
> The reason is domain-specific: legal questions often hinge on specific terminology and article numbers. A user asking about a particular article expects documents containing that exact reference. Pure embedding-based search retrieves passages that are semantically similar in topic but may not contain the specific article being asked about — it's good at meaning, but it can miss exact terms.
>
> My solution was hybrid retrieval: BM25 for exact keyword matching, semantic search for meaning, then merging both result sets, and applying a cross-encoder to rerank the merged list. The cross-encoder evaluates query and document together rather than separately, which is more accurate, at the cost of speed — which is why it's a second-stage step over a small candidate set rather than the primary search.
>
> What I learned is that retrieval is a two-stage problem: recall first, then precision. Cast a wide net cheaply, then filter and rank accurately."

## Q70. If you could improve that project, what would you change?

> "Several things, in priority order.
>
> First, proper RAG evaluation metrics — specifically RAGAS, which measures faithfulness, answer relevance, and context precision and recall separately. My evaluation measured the final answer but couldn't tell me *where* the pipeline was failing — was it retrieval or generation? Separating those metrics would have told me faster.
>
> Second, metadata filtering. Right now all documents are treated equally, but a statutory text and a web article about a protest are not equally authoritative for a legal question. Tagging sources and filtering by type would improve answer quality meaningfully.
>
> Third, query rewriting — user questions are often vague, and rewriting them into better search queries before retrieval is a cheap improvement.
>
> Fourth, moving from FAISS to a production vector database like Qdrant or Milvus — FAISS is excellent locally but doesn't handle concurrent access and metadata filtering the way a proper vector database does.
>
> And finally, tests. That project has no automated test suite, which is exactly the gap I'm aware of and the reason I'm here. I built it, I evaluated its outputs, but I never wrote tests that would run on every change. Looking back, that's the most significant thing missing."

*(Jawaban ini sangat kuat untuk VP: menunjukkan self-awareness tentang gap testing-mu sendiri, sekaligus menunjukkan kamu tahu arah perbaikan teknis.)*

---

# SECTION 13 — Closing

## Q71. What are your career goals?

> "I want to become a genuinely strong engineer in test — someone who builds test infrastructure rather than just executing tests. My near-term goal is depth: automation frameworks, API and integration testing, and CI/CD integration. Longer term I'm drawn to the quality engineering side — designing the systems and standards that make quality a property of the whole development process rather than a gate at the end.
>
> My honest position is that I've built software and I've trained and evaluated models, and I've learned that I'm most interested in the part where you prove something actually works. I want to build a career around that."

## Q72. Where do you see yourself in 3–5 years?

> "In three to five years I'd like to be an SDET with real depth — someone who can design a test strategy for a complex system and build the automation to support it. I'd want to have worked on a system where I understand the integration and reliability side deeply, not just the UI.
>
> Beyond that, I'm interested in the direction of quality engineering as a discipline — the standards, tooling, and practices that make a whole team's output more reliable. But I'm realistic: that's a direction, not a plan, and the near-term priority is genuinely becoming good at the fundamentals first."

## Q73. What do you expect from this internship?

> "Three things, and I'd rather be specific than say 'learning experience.'
>
> First, I want to learn what testing actually looks like at scale — in a company serving 50 million users, with real release processes, real automation infrastructure, and real consequences when something fails. I've built things alone; I haven't seen how quality is managed professionally.
>
> Second, I want to work under people who are better at this than I am. I've been self-taught in a lot of areas, and the fastest way to close the gaps is to be somewhere with real standards and honest feedback.
>
> Third, I want to contribute — not just observe. I'm not looking for a placement where I watch; I want to own something real, even if it's a small piece, and be accountable for it."

## ⭐ Q74. Do you have any questions for us? (SELALU ditanya — siapkan 3)

**Pertanyaan terbaik untuk VP SQA:**

1. **"How do you see the SQA team at tiket.com evolving over the next one to two years — particularly around test automation and shift-left practices?"**
   *(Menunjukkan kamu berpikir strategis, bukan hanya operasional.)*

2. **"What are the biggest quality engineering challenges specific to tiket.com's scale — particularly given how many third-party supplier and payment integrations the platform depends on?"**
   *(Menunjukkan kamu sudah menganalisis sistem mereka, bukan hanya membaca website.)*

3. **"What would a successful SDET intern look like in their first three months on your team?"**
   *(Menunjukkan ownership dan keinginan berkontribusi cepat.)*

**Cadangan:**
- "How do the SDET and development teams collaborate in the release process — is testing integrated into the same squads, or separate?"
- "What does the current automation landscape look like at tiket.com — what's covered and what are you working toward?"

**JANGAN tanya:** gaji (simpan untuk tahap offer), hal yang jawabannya ada di website, atau pertanyaan yang menunjukkan kamu tidak riset.

---

# LAMPIRAN — PRINSIP JAWABAN UNTUK LEVEL VP

**VP tidak menilai hafalan. Yang dinilai: cara berpikir.**

1. **Selalu struktur jawaban sebelum masuk detail.** "Let me structure this..." lalu sebutkan kategori.
2. **Selalu sebut layer.** UI → API → database → integration. Kebanyakan kandidat berhenti di UI.
3. **Sebut trade-off, bukan jawaban absolut.** Pertanyaan "mana yang dulu" biasanya mencari penalaran, bukan pilihan.
4. **Jujur saat tidak tahu — lalu tunjukkan cara berpikirmu.** "I haven't done that directly, but here's how I'd approach it."
5. **Tunjukkan intellectual honesty.** "My tests passed but that doesn't prove it works" adalah jawaban senior.
6. **Hubungkan ke bisnis.** Bug = user gagal booking, bukan sekadar "test failed".
7. **Jangan mengklaim pengalaman yang tidak ada.** Pertanyaan lanjutan akan menjebakmu, dan VP sudah mendengar ribuan jawaban.

---

# CHECKLIST FINAL (cek 30 menit sebelum)

- [ ] Konfirmasi status pendidikan (mahasiswa tingkat akhir / lulus) — konsisten sepanjang sesi
- [ ] Hafal struktur jawaban test case: POSITIVE → NEGATIVE → BOUNDARY → SECURITY/DATA → PERFORMANCE
- [ ] Latih dengan suara keras: 5 positive + 5 negative + 3 boundary untuk **flight search** dan **payment** tanpa baca catatan
- [ ] Hafal Q1 (Tell me about yourself, 45-60 detik), Q3 (Why SDET), Q19 (prioritization)
- [ ] Siapkan 3 pertanyaan untuk VP
- [ ] Test kamera, mikrofon, koneksi Google Meet
- [ ] Buka link 5 menit lebih awal: https://meet.google.com/suq-fbcj-uht
- [ ] Siapkan catatan poin kunci di samping layar (bukan script penuh)
