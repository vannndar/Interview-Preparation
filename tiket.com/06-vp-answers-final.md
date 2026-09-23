# SDET Intern Interview Preparation - tiket.com
## Interviewer: Bhupesh Mittal (VP - SQA) | 17 Sept 2026, 18:00 WIB | 45 min

Fokus dari HR: positive dan negative test cases.

---

# 1. Introduction & Motivation

### Q1. Tell me about yourself.

**Answer:**

My name is Thariq Ivan Anendar. I recently graduated from Institut Teknologi Sepuluh Nopember with a degree in Computer Science.

During my studies, most of my work was around AI and data. I was in the UAV team at Bayucaraka, I did an internship as an AI engineer at PLN Nusantara Power, and my final project was a RAG chatbot for Indonesian legislation.

Across those projects, I found that the work I enjoyed most was not building the system, but verifying that it actually worked. That is why I am applying for this role.

---

### Q2. Can you explain your background and experience?

**Answer:**

My background is Computer Science. I studied programming, software engineering, databases, and system development.

The most relevant projects:

1. **UAV team (Bayucaraka)** - I worked on autonomous systems using PX4, implemented fire detection using YOLO, and worked on the cloud component where images and videos were transferred to AWS S3 and delivered to the ground control station.

2. **AI Engineer internship (PT PLN Nusantara Power)** - I processed time series data and explored large time series models for zero-shot prediction on power plant data.

3. **Final project (RAG chatbot)** - I built a retrieval-augmented generation system for Indonesian legislation, including document processing, hybrid retrieval, reranking, and model evaluation.

4. **Competitive programming committee (Schematics)** - I wrote and validated the test cases for contest problems. This is where I first did testing work seriously.

From these, I learned how systems fail at integration points, how to debug problems with limited information, and how important it is to verify data rather than only checking the interface.

---

### Q3. Why are you interested in SDET?

**Answer:**

I am interested in SDET because it combines the two things I am good at: writing code and finding problems.

When I built my RAG chatbot, the most useful work was not building the pipeline. It was building the evaluation. That is when I found out the system was not working as well as I thought, and that is when it actually improved.

I also understand that a good SDET is not only someone who finds bugs. It is someone who understands the product, thinks about how it can fail, and builds systems that let the whole team verify quality repeatedly.

I want to work in that direction rather than only executing test cases.

---

### Q4. Why do you want to join tiket.com?

**Answer:**

Two reasons.

First, the domain. Travel booking depends on many partners - airlines, hotels, local operators - and each one is a separate integration point that can fail independently. That makes testing more interesting than checking a UI.

Second, the scale. With 50 million users, a defect is not abstract. Someone's trip is affected. I find that responsibility motivating.

I also want to learn how quality is managed in a real company, because all my experience so far is from building things myself.

---

### Q5. Why should we hire you as an SDET intern?

**Answer:**

I do not have professional testing experience yet. That is why I am applying for an internship rather than a junior role.

What I do have:

1. I have built systems end to end, so I understand where software breaks - at integration boundaries, at edge cases, and under concurrent access.

2. I already have some testing experience from university. In the competitive programming committee, my job was writing and checking the test cases for the problems. The test cases had to actually distinguish a correct solution from an incorrect one, which is the same problem as designing a good test.

3. I learn new tools quickly. I picked up YOLO and autonomous systems for the UAV team, and I taught myself the full RAG stack for my final project.

---

# 2. SDET & Testing Fundamentals

### Q6. What is an SDET?

**Answer:**

SDET stands for Software Development Engineer in Test.

An SDET is an engineer who combines development skills with testing knowledge. The main difference from a QA engineer is that the deliverable is code: test automation, frameworks, and test infrastructure.

Responsibilities include designing test strategy, building and maintaining automation, integrating tests into CI/CD, and working with developers early to catch problems before they are built.

An SDET also still does exploratory and manual testing where automation does not make sense.

---

### Q7. What is the difference between QA Engineer, Tester, and SDET?

**Answer:**

**Software Tester:** executes test cases, finds defects, verifies behavior.

**QA Engineer:** focuses on the process - standards, planning, reviews - to prevent defects from being created.

**SDET:** has stronger programming skills. Builds automation systems, writes test code, integrates testing into the development pipeline, and treats test code as production code.

The easiest way to separate them: the tester verifies the product, the QA engineer improves the process, and the SDET builds the systems that make verification scale.

---

### Q8. Why is software testing important?

**Answer:**

Testing is important because the cost of a defect increases the later it is found. A defect found in code review costs minutes. The same defect in production costs engineering time and user trust.

Testing helps:
- Find defects before users encounter them
- Reduce business and financial risk
- Improve reliability
- Confirm that new changes do not break existing features
- Give the team confidence to release quickly

For applications with many users, quality is not optional. A bug in the payment flow means failed transactions and lost revenue.

---

### Q9. Explain SDLC and STLC.

**Answer:**

**SDLC (Software Development Life Cycle)** describes the process of building software:
1. Requirement analysis
2. Design
3. Development
4. Testing
5. Deployment
6. Maintenance

**STLC (Software Testing Life Cycle)** focuses on testing:
1. Requirement analysis
2. Test planning
3. Test case design
4. Test environment setup
5. Test execution
6. Defect reporting and retesting
7. Test closure

Testing should be involved from the first phase. In requirement analysis, the tester's job is to find ambiguity before anyone writes code, because unclear requirements are one of the biggest sources of defects.

---

# 3. Manual Testing Fundamentals

### Q10. What is the difference between test scenario and test case?

**Answer:**

A test scenario describes what needs to be tested, at a high level.

Example: "Verify user login functionality."

A test case describes how to test it, with specific steps and expected result.

Example:
1. Open login page
2. Enter valid email
3. Enter valid password
4. Click Login
5. Verify user is redirected to homepage

One scenario usually becomes many test cases: valid login, wrong password, empty fields, locked account - all under the same scenario.

---

### Q11. What makes a good test case?

**Answer:**

A good test case should be:
- Clear and easy to understand
- Reproducible by anyone on the team
- Have defined input and expected output
- Independent - it should not depend on a previous test having run
- Cover normal cases, edge cases, and failure cases

The expected result must be explicit. "Verify it works" is not a test case. "The system returns HTTP 200 and redirects to the dashboard" is.

---

### Q12. What are positive and negative test cases?

**Answer:**

A **positive test case** uses valid input and expects the system to succeed.

Example: login with correct email and password, expect the user to be logged in.

A **negative test case** uses invalid input and expects the system to handle it properly.

Example: login with an incorrect password, expect an error message and no login.

The important detail is what "handle it properly" means. The expected result is not only that it fails. It is that it fails cleanly:
- A clear error message
- No crash
- No corrupted or inconsistent data

That is the standard I would hold negative test cases to.

---

### Q13. Give login test cases.

**Answer:**

**Positive cases:**
1. Valid email and valid password → logged in, redirected to homepage
2. Email with different casing (IVAN@mail.com) → still accepted
3. "Remember me" checked → session persists after browser restart
4. Login with Google → account linked, session created
5. Extra space before or after the email → trimmed, login succeeds

**Negative cases:**
1. Correct email, wrong password → "incorrect password", not logged in
2. Email not registered → error message, but not one that reveals whether the email exists
3. Empty email → "email is required"
4. Empty password → "password is required"
5. Wrong email format (ivan@, ivan.com, @mail.com) → format error
6. Password with wrong casing (Pass@123 vs pass@123) → must fail
7. Five failed attempts in a row → account locked or rate limited
8. SQL injection in the email field → rejected, never executed
9. XSS payload in the input → escaped, never executed
10. Double-clicking the Login button → only one authentication request

**Boundary cases:**
- Email at maximum allowed length → accepted; one character more → rejected
- Password at minimum length → accepted; one character less → rejected

**Non-functional:**
- Network failure during login → informative error, not an indefinite spinner
- Accessing the dashboard URL directly without a session → redirected to login

---

### Q14. Explain boundary value analysis.

**Answer:**

Boundary value analysis tests the values at the edges of an input range, because defects cluster there. Most of these are off-by-one errors and incorrect comparison operators.

Example: if a password must be 8 to 20 characters:

Test:
- 7 characters → invalid
- 8 characters → valid
- 9 characters → valid
- 19 characters → valid
- 20 characters → valid
- 21 characters → invalid

Testing only 15 would never find a problem with the validation.

---

### Q15. Explain equivalence partitioning.

**Answer:**

Equivalence partitioning divides input data into groups that the system should treat the same way, then tests one representative value from each group instead of testing every possible value.

Example: age requirement of 18 to 60:

- Below 18 → invalid group
- 18 to 60 → valid group
- Above 60 → invalid group

Testing 15, 30, and 70 covers all three partitions.

Boundary value analysis then checks the edges of those same partitions. The two techniques are usually used together.

---

### Q16. How do you decide what to test first?

**Answer:**

By risk. I score on two things: impact if it fails, and likelihood of failing.

**Impact:** does it affect revenue, corrupt data, or affect many users? On a travel platform, anything in the booking and payment path comes first. If the user cannot complete a booking, nothing else matters.

**Likelihood:** how recently did it change, how complex is it, and how many external systems does it touch? A feature that just changed and depends on three APIs is riskier than a stable, self-contained feature.

My order: critical revenue path first, then recently changed or complex areas, then high-likelihood edge cases, then everything else.

If I run out of time, I would state what I did not cover rather than implying everything was tested.

---

### Q17. Payment feature bug or profile picture bug - which do you fix or test first?

**Answer:**

Payment.

Payment affects revenue directly, and it can create financial inconsistency: a user charged without a booking, or a booking without a charge. That is severity critical and priority urgent.

Profile picture upload affects a small number of users, has no financial impact, and can be deferred to the next release without blocking anything. Severity low, priority low.

I would decide by impact on users and the business, not by which bug is more interesting to fix.

The exception is if the profile picture bug blocks something bigger, like preventing registration from completing. Then I would re-evaluate.

---

### Q18. You have 30 minutes to test a new feature. What do you do?

**Answer:**

First, two minutes to understand scope: what the feature does, what the critical path is, and what changed.

Then most of the time on the critical path, because if that is broken, nothing else matters.

Then the highest-risk edge cases: invalid input, empty states, and failure modes that could corrupt data.

What I would skip: exhaustive boundary testing, cross-browser checks, and performance testing.

Then I would report what I did not cover. A tester who says "I covered the critical path and these edge cases, but not browser compatibility" is more useful than one who claims everything passed.

---

### Q19. How do you ensure your testing coverage is enough?

**Answer:**

I measure coverage against requirements, not against lines of code. Four checks:

1. **Requirement traceability** - every acceptance criterion maps to at least one test case.

2. **Scenario categories** - for each feature, have I covered positive, negative, boundary, and non-functional cases? Missing an entire category is the most common coverage gap.

3. **Layer coverage** - did I test at the UI, the API, and the data layer? Some defects are not visible at the UI.

4. **Would my tests fail if the code were broken?** Code coverage tells you what executed, not what was verified. A test with weak assertions can have full coverage and catch nothing.

---

# 4. Bug Reporting

### Q20. Tell me about a bug you found.

**Answer:**

In the competitive programming committee for Schematics, my job was writing and validating the test cases for the contest problems.

I found one problem where a brute-force solution was passing, even though it should have timed out under the stated constraints. The test cases were not large enough to catch it. The problem looked correct, but it would have accepted wrong solutions and given those participants full marks.

I fixed it by generating larger and adversarial test cases designed to break that approach.

The lesson stayed with me: a test suite passing does not mean the software is correct. It means the software passes those tests. If the tests are weak, a green result is false confidence.

That is why I care about whether test cases actually discriminate, not just whether they exist.

---

### Q21. How do you report a bug?

**Answer:**

The goal is that the developer can reproduce it without asking me anything.

A bug report should include:
- Title with module and symptom
- Environment (browser, OS, device, version)
- Precondition
- Steps to reproduce
- Actual result
- Expected result
- Severity and priority
- Evidence (screenshot, video, API request/response)
- Test data used

I also link the requirement or acceptance criteria it violates. That removes any debate about whether it is a bug.

If I have a hypothesis about the cause, I label it clearly as a hypothesis, not a conclusion.

---

### Q22. What is the difference between severity and priority?

**Answer:**

Severity is how bad the impact of the defect is on the system. Priority is how urgently it needs to be fixed. They are independent.

Example - severity low, priority high: a typo in the logo on the homepage. Nothing breaks technically, but every user and stakeholder sees it, so it is a brand issue and gets fixed immediately.

Example - severity high, priority low: a serious calculation error in a feature that a small internal team uses twice a year. The impact is real, but it can be scheduled rather than hotfixed.

---

### Q23. A developer says "this is not a bug, it is expected behavior." What do you do?

**Answer:**

First, I re-read the requirement and the acceptance criteria. If the behavior contradicts what is written there, I have an objective basis for discussion rather than one person's opinion.

If the requirement is unclear or silent, the developer may be right, and the real finding is that the requirement needs clarification.

Then I present evidence: steps to reproduce, environment, actual versus expected result. I also ask what context I might be missing, such as a configuration or a business rule I do not know about.

If we still disagree, I escalate to the Product Owner, because they own the definition of correct behavior. Whatever the outcome, I document the decision so the same question does not come up again.

I try not to make it personal. Most of the time, the disagreement means the requirement was ambiguous.

---

### Q24. How do you debug when you cannot reproduce a bug?

**Answer:**

I treat "cannot reproduce" as something to investigate, not a reason to close it.

Steps:
1. Ask the reporter for detail: exact steps, environment, browser, account used, time of occurrence, and what they saw. Bugs that affect only some users are usually environment or data dependent.
2. Test systematically with different accounts and data states, browsers, devices, and network conditions.
3. Check logs and monitoring around the reported time. That is often where the real signal is when reproduction fails.
4. If the report suggests it is intermittent, change the hypothesis. Intermittent failures usually point to timing or concurrency, not a logic bug.
5. Document everything tried and found, so the next person does not repeat it.

Even a failed reproduction is useful evidence, because it tells the next person what has been ruled out.

---

# 5. Automation Testing

### Q25. Why do we need automation testing?

**Answer:**

Three reasons:

1. **Repeatability** - a regression suite that runs on every commit catches breakage immediately. That frequency is not possible manually.
2. **Consistency** - automated tests do not skip steps or vary between runs.
3. **Coverage at scale** - once the framework exists, running more tests costs almost nothing.

One clarification: I do not think the main value is that automation finds more bugs than manual testing. The value is that it frees people to do the exploratory testing and usability judgment that automation cannot do.

---

### Q26. What should be automated and what should remain manual?

**Answer:**

Automate:
- Regression tests
- Data-driven tests with many input combinations
- API tests
- Smoke and sanity suites
- Anything that runs repeatedly in CI

Keep manual:
- Exploratory testing
- Usability and UX evaluation
- One-off checks
- Tests requiring human judgment
- Features whose UI is still changing rapidly, because the maintenance cost exceeds the benefit

It is an ROI decision, not a rule. Automating the wrong test gives a suite that is expensive to maintain and always red.

---

### Q27. What automation tools have you used?

**Answer:**

I should be accurate here rather than impressive. I have not used Selenium or Playwright in a professional testing context yet.

What I do have:

1. I have written Python validation code for my machine learning pipelines, checking data integrity and model output correctness.
2. I have tested my own web applications by sending API requests and verifying responses and status codes.
3. I have set up Dockerized services with health checks, which is infrastructure for automated verification.
4. In the competitive programming committee, I automated test case validation for contest problems.

So I understand the concepts and the tooling landscape, but hands-on framework experience is what I want to build here.

---

### Q28. How does Selenium work internally?

**Answer:**

I have not used Selenium directly, so I will explain my understanding of it.

The flow is: test code uses the Selenium client library, which sends commands over the W3C WebDriver protocol - usually HTTP - to a browser driver such as chromedriver. The driver translates those commands into the browser's own automation interface, executes them, and returns the result.

So there is no magic: it is a remote control protocol. That also explains why synchronization is the hard part, because the test runs at a different speed than the browser and the network.

---

### Q29. What are the challenges of automation testing?

**Answer:**

Four main challenges:

1. **Flakiness** - tests that pass and fail without code changes, usually from timing, test interdependency, or environment instability. This is also a trust problem: once the team starts ignoring red builds, the automation has stopped working.
2. **Maintenance** - UI automation breaks whenever the UI changes.
3. **Initial investment** - a framework takes real time before it produces value.
4. **Scope decisions** - a suite that tests everything is expensive and slow.

The one I would emphasize is flakiness, because it erodes trust in the whole suite.

---

### Q30. How do you maintain automation scripts when the application changes?

**Answer:**

Structurally, so that changes are localized.

1. **Page Object Model** - all locators and page interactions for a page live in one file. When the UI changes, one file is updated instead of fifty tests.
2. **Stable locators** - prefer semantic attributes such as data-testid or accessibility roles over CSS chains tied to styling.
3. **Independent tests** - so one broken test does not cascade.
4. **Separate test and application code** - so application refactors do not break test structure.

The principle is that test code deserves the same design discipline as production code, because the team pays its maintenance cost every sprint.

---

### Q31. What is the difference between UI testing and API testing?

**Answer:**

**UI testing** verifies the interface end to end. It validates the full stack and catches presentation and interaction issues, but it is slow, brittle, and expensive to maintain.

**API testing** verifies the service layer directly - request in, response out. It is faster, more stable, and better for validating business logic, edge cases, and error handling.

The distinction that matters: a UI test tells you that a wrong value appeared on screen. An API test tells you whether the service returned the wrong value or the UI displayed it wrong. Those are different bugs with different fixes.

I would push as much validation as possible to the API and unit level and keep UI tests for critical user journeys.

---

# 6. Scenario - Test Case Design

### Q32. How would you test tiket.com flight search?

**Answer:**

**Positive cases:**
1. Search CGK to DPS, valid future date, one adult → results display with correct route and price
2. Round-trip search → both legs displayed correctly
3. Filters (direct only, airline, departure time) → results filtered correctly
4. Sort by cheapest and by fastest → ordering correct, including ties
5. Passenger composition (adults, children, infants) → pricing rules applied correctly
6. Changing origin or destination → results refresh for the new route
7. Selecting a date from the date picker → correct date applied
8. Result card shows all required information: price, duration, stops, baggage

**Negative cases:**
1. Same origin and destination → rejected with a clear message
2. Departure date in the past → rejected, date picker disables past dates
3. Return date before departure date → validation error
4. Passenger count of 0 → rejected
5. Passenger count above maximum → rejected
6. Infants exceeding adults → rejected per airline rules
7. Empty origin or destination → required field validation
8. Non-existent airport name → "no results found", not a 500 error
9. Special characters in the city field → handled, no crash
10. Route with no available flights → informative empty state, not a blank page
11. Double-clicking Search → only one request sent
12. Airline API timeout → informative error with retry option, not an indefinite spinner
13. Navigating back after a search → state consistent

**Boundary cases:**
- Departure date today → must work
- Departure date tomorrow → must work
- Maximum passenger count → works; one above → rejected
- Furthest bookable date → works; one day beyond → rejected

**Non-functional:**
- Performance: results load within acceptable time, including under concurrent load
- Compatibility: Chrome, Safari, Firefox, mobile web, and the app
- Accessibility: keyboard navigation through filters, labels on the date picker

**Integration layer:**
The results come from third-party airline APIs, so I would test what happens when a supplier times out, returns nothing, or returns malformed data. And I would verify that the price shown in search matches the price shown at checkout, because that mismatch is a real trust problem.

---

### Q33. How would you test the payment flow?

**Answer:**

**Positive cases:**
1. Successful payment with a valid card → booking confirmed, confirmation email sent
2. Each payment method (card, virtual account, e-wallet) → correct flow and final status
3. Valid promo code → discount applied, total correct
4. Total calculation: base price + tax + service fee - discount
5. E-ticket or voucher delivered after successful payment
6. Booking record created with correct values in the database

**Negative cases:**
1. Expired card → declined with a clear message, no booking created
2. Wrong CVV → declined
3. Invalid card number (fails Luhn check) → rejected before submission
4. Insufficient e-wallet balance → clear error, no booking
5. Invalid or expired promo code → rejected, price unchanged
6. Already-used promo code → rejected
7. Payment timeout → booking must NOT be marked confirmed; status must be pending or failed
8. Browser closed after clicking pay but before the callback → status must remain consistent
9. Double-clicking Pay → exactly one transaction created
10. Price tampered in the client payload → server rejects and uses server-side pricing
11. Inventory sold out mid-flow → prevented, no overselling
12. Missing passenger data → per-field validation
13. Network failure during payment → user can retry without being double charged

**Boundary cases:**
- Payment amount at minimum and maximum allowed
- Promo discount equal to the full amount → total must not go negative

**Concurrency:**
Two users attempting to book the last available seat or room at the same moment. Exactly one should succeed and the other should receive a clear failure. Never both. This requires concurrent requests, not UI clicks.

**Data integrity:**
After the flow, I would verify in the database: was the booking created exactly once, did inventory decrement by exactly one, and does the recorded amount match what the payment gateway reports. The confirmation page can look correct while the database contains a duplicate row.

---

### Q34. Users report that booking sometimes fails. How would you investigate?

**Answer:**

First, I would narrow the report, because "sometimes" is not testable. I need to know: which payment method, which route or hotel, which device, and at what time. The time matters because it often shows whether failures correlate with peak load.

Then I would check the data first, since it is the fastest source of truth:
- Are failures concentrated on one payment method or one supplier?
- Are there bookings marked as failed where the payment actually succeeded? That version is the most serious, because it means we took money and did not deliver.
- Is there a pattern in the failure status values?

Then logs and monitoring around the failure times.

Then I would try to reproduce it deterministically once I have a hypothesis, using concurrent requests if it looks like a race condition.

The key point is that "sometimes fails" almost always has a pattern. The job is finding the variable that separates failures from successes.

---

### Q35. A feature works on Chrome but fails on Safari. What do you do?

**Answer:**

First, characterize the difference precisely: what exactly fails, and does it fail consistently on Safari or intermittently? Consistency tells me a lot.

Then check the common causes:
- JavaScript features Safari handles differently or does not support
- Date and timezone handling, which differs between Safari and Chrome and causes real bugs with date pickers
- CSS differences
- Third-party library compatibility

The browser console and network tab in Safari usually show the mechanism.

Then report it as a compatibility defect with exact environment details, and confirm whether Safari is in the supported browser matrix. If it is, it is a real defect. If not, it is a known limitation.

Longer term, critical journeys should have cross-browser coverage in the automation suite so this class of issue is caught before release.

---

### Q36. How would you test an application with millions of users?

**Answer:**

At that scale you cannot test everything, so it becomes risk management plus production observability.

Four focus areas:

1. **Critical paths at scale** - performance and load testing of the highest-traffic flows, because at millions of users the failure mode is usually capacity and concurrency, not logic.
2. **Device and network diversity** - different devices, networks, and regions, since a travel platform is used in very different conditions.
3. **Third-party dependencies** - at this scale, external APIs and payment gateways are where a large share of incidents originate, so contract testing and graceful degradation matter enormously.
4. **Production monitoring and staged rollouts** - canary releases and fast rollback, because you cannot pre-test everything.

The mental shift is from "did we test everything" to "how do we detect and contain what we missed".

---

# 7. SQL and API

### Q37. What is SQL used for in testing?

**Answer:**

SQL is how I verify what the UI and API cannot tell me: the actual state of the data.

Three main uses:
1. **Data verification** - after a booking, does the record exist exactly once, are the values correct, does the payment status match the booking status?
2. **Test data setup and cleanup** - creating preconditions and removing test data afterward so tests stay independent.
3. **Defect investigation** - checking whether bad data explains a bug seen at the UI.

The broader point is that UI-level verification cannot catch data problems. A booking confirmation page can look correct while the database contains a duplicate row.

---

### Q38. Difference between WHERE and HAVING?

**Answer:**

WHERE filters rows before aggregation. HAVING filters groups after aggregation.

That is why WHERE cannot reference aggregate functions. Writing COUNT(*) in a WHERE clause fails, because the grouping has not happened yet.

Example: to find users with more than three orders, group by user and use HAVING COUNT(*) > 3.

---

### Q39. Explain INNER JOIN and LEFT JOIN.

**Answer:**

**INNER JOIN** returns only rows where the condition matches on both sides.

**LEFT JOIN** returns all rows from the left table plus matches from the right, with NULLs where there is no match.

The practical one for testing is LEFT JOIN, because it is how you find missing relationships - for example, users who have never placed an order, or bookings with no corresponding payment record. That is a data integrity check.

---

### Q40. Write a query to find duplicate data.

**Answer:**

```sql
-- Duplicate emails
SELECT email, COUNT(*) AS occurrences
FROM users
GROUP BY email
HAVING COUNT(*) > 1;

-- Duplicate bookings: same user, same offer, same day
SELECT user_id, offer_id, DATE(created_at) AS booking_date, COUNT(*) AS total
FROM orders
WHERE status = 'confirmed'
GROUP BY user_id, offer_id, DATE(created_at)
HAVING COUNT(*) > 1;
```

The second query is what I would run after a payment-related change. The same user getting two confirmations for the same offer on the same day is the signature of a double-submit problem. It is a data-level check for a failure mode that is very hard to catch at the UI.

---

### Q41. Users table and Orders table - find all users who have completed orders.

**Answer:**

```sql
SELECT DISTINCT u.user_id, u.name
FROM users u
INNER JOIN orders o ON u.user_id = o.user_id
WHERE o.status = 'completed';
```

But as a tester, I would also run checks that verify integrity rather than report data:

```sql
-- Orders with no valid user (referential integrity violation)
SELECT o.order_id, o.user_id
FROM orders o
LEFT JOIN users u ON o.user_id = u.user_id
WHERE u.user_id IS NULL;

-- Duplicate completed orders for the same user
SELECT user_id, COUNT(*) AS total
FROM orders
WHERE status = 'completed'
GROUP BY user_id
HAVING COUNT(*) > 1;

-- Invalid amounts
SELECT * FROM orders WHERE amount <= 0;

-- Completed orders with zero amount
SELECT * FROM orders WHERE status = 'completed' AND amount = 0;
```

The first query is a reporting question. The others are integrity questions, and that is usually where the real defects are. An order with no valid user should never exist, and no UI test would surface it.

---

### Q42. What is API testing?

**Answer:**

API testing validates the service layer directly by sending requests and verifying responses, without going through the UI.

It covers:
- Correctness of response data
- Status codes
- Error handling
- Authentication and authorization
- Behavior with invalid input

It is the layer where business logic lives, so it is the most efficient place to find defects: faster than UI tests, more stable, and it can catch problems the UI hides.

---

### Q43. Why do we test APIs and not only the UI?

**Answer:**

Because many defects are invisible or ambiguous at the UI level.

A UI test can tell you that a wrong number appeared on screen. An API test tells you whether the service returned the wrong number or the UI displayed it wrong - a different bug with a different fix.

Practically: API tests run faster, break less often when the UI changes, can cover error handling the UI does not expose, and can run in CI on every commit.

The UI still gets tested, but for user journeys rather than for validating business rules.

---

### Q44. What HTTP methods do you know?

**Answer:**

- **GET** - retrieves data, should not change state
- **POST** - creates a resource or submits data
- **PUT** - replaces a resource entirely, idempotent
- **PATCH** - partially updates a resource
- **DELETE** - removes a resource

The distinction that matters most for testing is idempotency. GET, PUT, and DELETE should be safe to retry. POST typically is not, which is exactly why payment flows need an idempotency key. If a payment POST is retried after a network timeout without one, the result is a double charge. That is a case I would test explicitly.

---

### Q45. What HTTP status codes do you know?

**Answer:**

- 2xx: 200 OK, 201 Created, 204 No Content
- 3xx: redirects
- 4xx: 400 Bad Request, 401 Unauthenticated, 403 Forbidden, 404 Not Found, 409 Conflict, 422 Validation Error, 429 Too Many Requests
- 5xx: 500 Internal Server Error, 502 Bad Gateway, 503 Service Unavailable, 504 Gateway Timeout

The distinction I check carefully is 401 versus 403, because they mean different things. 401 means not authenticated. 403 means authenticated but not authorized. A test expecting "access denied" should verify which one.

---

### Q46. How do you validate an API response?

**Answer:**

Several layers:
1. **Status code** - does it match the expected outcome for this request
2. **Body structure** - correct schema and required fields present
3. **Body values** - correct data, and calculated fields actually correct rather than merely present
4. **Headers** - content type, caching, rate limit headers where relevant
5. **Negative behavior** - invalid requests return the right error code with a clear message, and do not create partial data
6. **Authorization** - can I access another user's resource by changing an ID in the request? That check is high value and invisible at the UI level
7. **Database consistency** - did the API's claimed effect actually happen

---

# 8. Behavioral

### Q47. Tell me about a difficult problem you solved.

**Answer:**

On the Bayucaraka UAV team, I worked on autonomous systems and implemented fire detection using YOLO. I also worked on the cloud component where images and videos were transferred to AWS S3 and delivered to the ground control station.

The model itself was not the hardest part. The hard parts were everything around it: the conditions in the field, getting the data off the aircraft reliably, and the processing limits on board. Processing everything on the aircraft was too heavy, so part of it was moved to the cloud.

What I learned is that in a real system, the model is usually not the bottleneck. It is the integration and the constraints around it. That changed how I approach problems - I look at the whole system, not just the piece I was assigned.

---

### Q48. Tell me about a time you worked with a team.

**Answer:**

On the UAV team, my work had to integrate with people handling different subsystems - hardware, flight control, and software. Everything had to work together for the aircraft to fly.

The most useful thing I did was make my component's inputs, outputs, and assumptions explicit early, so other people could plan around it without blocking.

I also shared problems in the shared channel rather than saving them for meetings, because a blocked component is a problem for the whole team.

What I learned is that finishing my part is not enough. Everyone else needs to know what my part does and when it will be ready.

---

### Q49. Tell me about a disagreement and how you solved it.

**Answer:**

On the UAV team, there was a difference of opinion about approach - I wanted to spend time differently than how it had been done before, and there was a reasonable concern about whether that was worth the risk given the competition deadline.

I tried to move the discussion from preference to criteria: what are we actually optimizing for, reliability or time? Once framed that way, it became a clearer decision rather than two people defending positions. I also acknowledged that the concern about the deadline was legitimate.

We settled on an approach that satisfied both concerns, and it worked in competition.

What I took from it is that most technical disagreements are really disagreements about priorities rather than facts. If you surface the real trade-off, the disagreement usually resolves itself.

---

### Q50. Tell me about a failure and what you learned.

**Answer:**

For my PKM project, our team proposed a two-way BISINDO sign language translator using a Transformer-based approach. We received funding, which felt like the idea was validated.

What I underestimated was the data. Sign language datasets are difficult, and building enough quality labelled data was a much larger undertaking than we planned. We spent more time than expected on data collection and preprocessing, and less on iteration and evaluation than the project deserved.

What I changed afterward is how I estimate projects. I now put the data work at the front of the plan, because in machine learning the data pipeline is usually the long pole, not the model. I also try to validate with the smallest working version rather than assuming the whole plan will hold.

I applied that when building my RAG chatbot - I built a minimal end-to-end pipeline first, and I built the evaluation early so I would find out quickly whether it was actually working.

---

### Q51. How do you handle pressure and deadlines?

**Answer:**

I separate what is essential from what is nice to have, and I complete the essential path first.

When I had several deadlines overlapping, the approach that worked was deciding the minimum acceptable outcome for each. Trying to do everything at full quality under time pressure usually means everything is late.

The other part is communicating early. If something is at risk, the people affected need to know while they can still plan around it.

And I try to be honest about what I cannot finish in the time available, rather than assuming I will make it work.

---

### Q52. Explain your previous project.

**Answer:**

My final project was a RAG chatbot that answers questions about Indonesian legislation, specifically UU TNI No. 34 of 2004 and its revisions.

**How it works:**
1. Documents are loaded from PDF, JSON, and web sources
2. Split into chunks with controlled overlap
3. Embeddings generated using a multilingual sentence-transformer model
4. Stored in a FAISS vector index
5. At query time, hybrid retrieval combining BM25 keyword search with semantic search
6. Results merged and reranked using a cross-encoder
7. Top results passed to the language model as context
8. The app also displays the source documents used, so the user can verify the answer

**Evaluation:**
I fine-tuned several open-source models using LoRA with 4-bit quantization, and compared RAG against fine-tuning across them, using LLM-as-judge, ROUGE, and BERTScore, with experiments tracked in Weights & Biases.

**What I learned:**
The evaluation showed the results were not as good as I expected - precision was low for several configurations. That was the most valuable output of the project, because it forced me to find out why. The documents were complex and multi-source, and my retrieval needed tuning. I added the hybrid retrieval and reranking in response.

The lesson is that evaluation matters more than the pipeline, because without it you do not know your system is failing.

---

### Q53. What was your contribution to that project?

**Answer:**

I built the RAG system - the document processing, the retrieval layer including hybrid search and reranking, and the application with multi-model support and source display.

To be precise, since it was a collaborative academic project: the model fine-tuning and evaluation runs were split across the team, with different models handled by different people. I was responsible for the RAG implementation and my portion of the evaluation work.

So the retrieval and application are entirely mine, and the evaluation contributions are shared.

---

### Q54. If you could improve that project, what would you change?

**Answer:**

In priority order:

1. **Better evaluation metrics** - RAGAS, to measure faithfulness, answer relevance, context precision, and context recall separately. My evaluation measured the final answer but could not tell me whether the failure was in retrieval or generation.
2. **Metadata filtering** - tag document types so a legal text and a news article are not treated as equally reliable for a legal question.
3. **Query rewriting** - user questions are often vague, and rewriting them before retrieval is a cheap improvement.
4. **Production vector database** - moving from FAISS to Qdrant or Milvus, since FAISS does not handle concurrent access and metadata filtering the way a production vector database does.
5. **Tests** - that project has no automated test suite. I built it and evaluated its outputs, but I never wrote anything that runs on every change. Looking back, that is the most significant thing missing, and it is part of why I am here.

---

# 9. Closing

### Q55. What do you expect from this internship?

**Answer:**

Three things.

1. I want to see how testing is actually done at scale. I have built things on my own, but I have not seen how quality is managed professionally, with real release processes and real automation infrastructure.
2. I want to work with people who are better at this than I am. I have been self-taught in many areas, and that is the fastest way to close the gaps.
3. I want to own something real, even if it is small. I am not looking for a placement where I only observe.

---

### Q56. Do you have any questions for us?

**Answer:**

Choose two or three:

1. "What would a successful SDET intern look like in their first three months on your team?"
2. "How do the SDETs and developers work together in the release process - same squad, or separate teams?"
3. "What does the automation coverage look like today, and what are you working towards?"
4. "What is the hardest quality problem the team has faced recently?"

Do not ask about salary at this stage.

---

# NOTES

1. If asked about test cases, do not stop at positive cases. The negative cases are what they are waiting for.
2. Do not claim experience with Selenium or Playwright. Answer honestly and explain what you do understand.
3. Keep answers between 30 and 60 seconds. Longer answers lose attention.
4. If you do not know: "I have not done that directly. Based on what I understand, I would probably approach it this way, but I would want to check with someone who has."
5. Pause briefly before answering difficult questions. That reads as thinking, not confusion.

---

# FINAL PRACTICE - 30 MINUTES

Take one feature. Say out loud, without reading:
- 5 positive test cases
- 5 negative test cases
- 3 boundary cases

Do this for **flight search** and **payment**.
