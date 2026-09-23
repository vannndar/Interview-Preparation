# SDET Intern Interview Preparation - tiket.com
## Interviewer: Bhupesh Mittal (VP - SQA) | 17 September 2026, 18:00 WIB | 45 menit

Fokus dari HR: positive dan negative test cases.

---

## 1. Introduction & Motivation

### Q1. Tell me about yourself.

**Answer:**
Hi, my name is Thariq Ivan Anendar, but my family and friends call me Ivan. I am a Computer Science graduate from Institut Teknologi Sepuluh Nopember.

I have been programming since senior high school. I started with IoT engineering, then studied Python, C, and C++ for data structures and competitive programming. I use Java for OOP and SQL for database operations.

During college, I had a lot of hands-on work: data science and data analytics projects, web programming, mobile programming using Flutter, and scripting for workflow automation. My main research work was in the Bayucaraka UAV team as a programming and electrical engineer.

From those experiences, I learned that every software can be developed, but not every software can be brought to the production stage. Testing and test automation are what help software become reliable and scalable before it reaches production.

That is why I am interested in this role.

---

### Q2. Can you explain your background and experience?

**Answer:**
My background is Computer Science, where I learned programming, software engineering concepts, databases, and system development.

My most relevant experience:

1. **Bayucaraka UAV research team.** I worked as a programming and electrical engineer. I implemented autonomous systems using PX4, built fire detection using YOLO, and worked on the cloud component where images and videos were transferred to AWS S3 and delivered to the ground control station.

2. **AI Engineer internship at PT PLN Nusantara Power.** I processed time series data and explored large time series models for zero-shot prediction on power plant data.

3. **RAG chatbot for Indonesian legislation.** I built a retrieval-augmented generation system covering document processing, hybrid retrieval, reranking, and model evaluation.

4. **Competitive programming committee at Schematics.** I wrote and validated test cases for contest problems. This was my first experience with testing as an engineering task.

From these projects, I learned how systems fail at integration points, how to debug problems with limited information, and why verifying data is as important as verifying the interface.

---

### Q3. Why are you interested in SDET?

**Answer:**
I am interested in SDET because it combines software development and software testing.

From my hands-on experience, I learned that every software can be developed, but not every software can reach production. The gap between a working system and a reliable system is where most problems occur. Testing is what closes that gap.

When I built my RAG chatbot, the most useful work was not building the pipeline. It was building the evaluation. The evaluation showed that the system was not performing as well as I expected, and that finding led to the main improvement in the project.

I understand that a good SDET is not only someone who finds bugs. A good SDET understands the product, analyzes how it can fail, and builds systems that allow the team to verify quality repeatedly.

I want to develop in that direction rather than only executing test cases.

---

### Q4. Why do you want to join tiket.com?

**Answer:**
There are two reasons.

First, the domain. Travel booking depends on many partners such as airlines, hotels, and local operators. Each partner is a separate integration point that can fail independently. This makes the testing problem more complex than interface testing.

Second, the scale. With more than 50 million users, a defect is not abstract. A single issue affects real users and real trips. I find this responsibility motivating.

I also want to learn how quality is managed in an industry environment, because all of my experience so far comes from building projects independently.

---

### Q5. Why should we hire you as an SDET intern?

**Answer:**
I do not have professional testing experience yet. This is why I am applying for an internship rather than a junior role.

What I have:

1. I have built systems end to end across several areas, so I understand where software breaks: at integration boundaries, at edge cases, and under concurrent access.

2. I already have testing experience from university. In the competitive programming committee, my responsibility was writing and validating the test cases for contest problems. The test cases had to distinguish a correct solution from an incorrect one, which is the same problem as designing a good test.

3. I learn new tools quickly. I learned YOLO and autonomous systems for the UAV team, and I learned the full RAG stack for my chatbot project.

---

## 2. SDET & Testing Fundamentals

### Q6. What is an SDET?

**Answer:**
SDET stands for Software Development Engineer in Test.

An SDET is an engineer who combines software development skills with testing knowledge. The main difference from a QA engineer is that the deliverable is code: test automation, test frameworks, and test infrastructure.

The responsibilities include:
- Designing test strategy
- Building and maintaining automation
- Integrating testing into CI/CD
- Working with developers early to identify problems before implementation

An SDET also performs exploratory and manual testing where automation is not suitable.

---

### Q7. What is the difference between QA Engineer, Tester, and SDET?

**Answer:**

**Software Tester:**
Executes test cases, finds defects, and verifies software behavior.

**QA Engineer:**
Focuses on the quality process, including standards, planning, and reviews, in order to prevent defects from being introduced.

**SDET:**
Has stronger programming skills. Builds automation systems, writes test code, integrates testing into the development pipeline, and treats test code as production code.

The distinction can be summarized as: the tester verifies the product, the QA engineer improves the process, and the SDET builds the systems that make verification scalable.

---

### Q8. Why is software testing important?

**Answer:**
Software testing is important because the cost of a defect increases the later it is found. A defect found in code review costs minutes to fix. The same defect found in production costs engineering time, user trust, and sometimes revenue.

Testing helps:
- Find defects before users encounter them
- Reduce business and financial risk
- Improve software reliability
- Ensure new changes do not break existing features
- Give the team confidence to release frequently

For applications with many users, quality is critical. A defect in the payment flow means failed transactions and lost revenue.

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

**STLC (Software Testing Life Cycle)** describes the testing process:
1. Requirement analysis
2. Test planning
3. Test case design
4. Test environment setup
5. Test execution
6. Defect reporting and retesting
7. Test closure

Testing should be involved from the first phase. During requirement analysis, the tester identifies ambiguity before implementation begins, because unclear requirements are one of the main sources of defects.

---

## 3. Manual Testing Fundamentals

### Q10. What is the difference between test scenario and test case?

**Answer:**

A test scenario describes what needs to be tested at a high level.

Example: "Verify user login functionality."

A test case describes how to test it, with specific steps and expected results.

Example:
1. Open the login page
2. Enter a valid email
3. Enter a valid password
4. Click Login
5. Verify the user is redirected to the homepage

One scenario usually produces many test cases, such as valid login, incorrect password, empty fields, and locked account.

---

### Q11. What makes a good test case?

**Answer:**
A good test case should be:
- Clear and easy to understand
- Reproducible by any member of the team
- Have defined input and explicit expected output
- Independent, without depending on a previous test having run
- Cover normal cases, edge cases, and failure cases

The expected result must be explicit. "Verify it works" is not a valid expected result. "The system returns HTTP 200 and redirects to the dashboard" is.

---

### Q12. What are positive and negative test cases?

**Answer:**

Positive testing verifies that the system works correctly with valid input.

Example: login with a correct email and password, expecting the user to be logged in.

Negative testing verifies that the system handles invalid input properly.

Example: login with an incorrect password, expecting an error message and no login.

The important detail is what "handles properly" means. The expected result is not only that the operation fails. The expected result is that it fails cleanly:
- A clear error message is displayed
- The system does not crash
- No data is corrupted or left in an inconsistent state

---

### Q13. Give login test cases.

**Answer:**

**Positive cases:**
- Valid email and valid password
- User is successfully logged in
- "Remember me" functionality works
- Login with a third-party provider
- Email entered with different casing is still accepted
- Extra space before or after the email is trimmed

**Negative cases:**
- Incorrect password
- Unregistered email
- Empty email or empty password
- Invalid email format
- Password with incorrect casing
- Five consecutive failed attempts
- SQL injection in the email field
- Double-clicking the Login button

**Boundary cases:**
- Email at maximum length
- Email one character above maximum length
- Password at minimum length
- Password one character below minimum length
- Special characters in the input

---

### Q14. Explain boundary value analysis.

**Answer:**
Boundary value analysis is a testing technique where we test values at the limits of an input range, because defects often occur at boundaries. Most of these defects are off-by-one errors and incorrect comparison operators.

Example: if the password requirement is 8 to 20 characters:

Test:
- 7 characters → invalid
- 8 characters → valid
- 9 characters → valid
- 19 characters → valid
- 20 characters → valid
- 21 characters → invalid

Testing only 15 characters would not detect a problem with the validation.

---

### Q15. Explain equivalence partitioning.

**Answer:**
Equivalence partitioning divides input data into groups that are expected to behave similarly. We test one representative value from each group instead of testing every possible value.

Example: if the age requirement is 18 to 60:

- Below 18 → invalid group
- 18 to 60 → valid group
- Above 60 → invalid group

Testing 15, 30, and 70 covers all three groups.

Boundary value analysis then tests the edges of those same groups. Both techniques are usually used together.

---

### Q16. How do you decide what to test first?

**Answer:**
I decide by risk, using two factors: impact if the feature fails, and likelihood of failure.

**Impact.** Does the feature affect revenue, corrupt data, or affect many users? On a travel platform, the booking and payment path comes first. If the user cannot complete a booking, no other function matters.

**Likelihood.** How recently did the code change, how complex is the feature, and how many external systems does it depend on? A feature that recently changed and depends on three APIs carries more risk than a stable, self-contained feature.

The order I use is: critical revenue path first, then recently changed or complex features, then high-likelihood edge cases, then remaining areas.

If time runs out, I report what was not covered instead of implying that everything was tested.

---

### Q17. Payment feature bug or profile picture bug, which do you fix first?

**Answer:**
Payment.

Payment affects revenue directly and can create financial inconsistency, such as a user charged without a booking, or a booking without a charge. This is severity critical and priority urgent.

Profile picture upload affects a small number of users, has no financial impact, and can be deferred without blocking other work. This is severity low and priority low.

The decision is based on impact to users and to the business, not on which defect is more interesting to fix.

The exception is if the profile picture defect blocks a larger flow, such as preventing registration from completing. In that case, I would reassess the priority.

---

### Q18. You have 30 minutes to test a new feature. What do you do?

**Answer:**
I spend the first two minutes understanding the scope: what the feature does, what the critical path is, and what changed.

I spend the majority of the time on the critical path, because if that fails, no other function matters.

I then test the highest-risk edge cases: invalid input, empty states, and failure modes that could corrupt data.

I deliberately skip exhaustive boundary testing, cross-browser verification, and performance testing.

At the end, I report what was not covered. Stating "I covered the critical path and these edge cases, but not browser compatibility" is more useful than claiming that everything passed.

---

### Q19. How do you ensure your testing coverage is enough?

**Answer:**
I measure coverage against requirements, not against lines of code. There are four checks.

1. **Requirement traceability.** Every acceptance criterion maps to at least one test case.

2. **Scenario categories.** For each feature, are positive, negative, boundary, and non-functional cases covered? A missing category is the most common coverage gap.

3. **Layer coverage.** Was the feature tested at the interface layer, the API layer, and the data layer? Some defects are not visible at the interface layer.

4. **Test effectiveness.** Would the tests fail if the code were broken? Code coverage measures what was executed, not what was verified. A test with weak assertions can achieve full coverage and detect nothing.

---

## 4. Bug Reporting

### Q20. Tell me about a bug you found.

**Answer:**
In the competitive programming committee at Schematics, my responsibility was writing and validating the test cases for the contest problems.

I found a problem where a brute-force solution was passing, even though it should have exceeded the time limit under the stated constraints. The test cases were not large enough to detect it. The problem appeared correct, but it would have accepted incorrect solutions and awarded full marks to those participants.

I fixed it by generating larger and adversarial test cases designed to break that approach.

The lesson was that a passing test suite does not mean the software is correct. It means the software passes those tests. If the tests are weak, a passing result provides no real assurance.

This is why I focus on whether test cases actually discriminate, not only on whether they exist.

---

### Q21. How do you report a bug?

**Answer:**
The objective is that the developer can reproduce the defect without asking further questions.

A bug report should include:
- Title with the module and the symptom
- Environment details: browser, operating system, device, version
- Precondition
- Steps to reproduce
- Actual result
- Expected result
- Severity and priority
- Evidence: screenshot, video, or API request and response
- Test data used

I also link the requirement or acceptance criteria that the defect violates. This removes any discussion about whether it is a defect.

If I have a hypothesis about the cause, I label it as a hypothesis rather than a conclusion.

---

### Q22. What is the difference between severity and priority?

**Answer:**
Severity is the impact of the defect on the system. Priority is the urgency of the fix. They are independent.

Example of low severity and high priority: a typo in the logo on the homepage. Nothing breaks functionally, but every user sees it, so it becomes a brand issue and is fixed immediately.

Example of high severity and low priority: a calculation error in a feature used by a small internal team twice a year. The impact is significant, but the fix can be scheduled rather than treated as urgent.

---

### Q23. A developer says "this is not a bug, it is expected behavior." What do you do?

**Answer:**
First, I re-read the requirement and the acceptance criteria. If the behavior contradicts the documented requirement, the discussion is based on the specification rather than on personal opinion.

If the requirement is unclear or does not cover the case, the developer may be correct. In that situation, the finding is that the requirement needs clarification.

I then present the evidence: steps to reproduce, environment, and the actual and expected results. I also ask whether there is context I am missing, such as a configuration or business rule.

If the disagreement continues, I escalate to the Product Owner, because they define the correct behavior. I document the final decision so the question does not recur.

The objective is not to argue. In most cases, the disagreement indicates that the requirement was ambiguous.

---

### Q24. How do you debug when you cannot reproduce a bug?

**Answer:**
I treat "cannot reproduce" as a finding to investigate, not a reason to close the defect.

The steps I follow:

1. Request details from the reporter: exact steps, environment, browser, account used, time of occurrence, and the observed behavior. Defects that affect only some users are usually environment dependent or data dependent.

2. Test systematically with different accounts and data states, browsers, devices, and network conditions.

3. Check logs and monitoring around the reported time. This is often where the signal is found when reproduction fails.

4. If the report suggests the defect is intermittent, change the hypothesis. Intermittent failures usually indicate timing or concurrency issues rather than a logic error.

5. Document everything attempted and observed, so the next person does not repeat the investigation.

A failed reproduction is still useful evidence, because it records what has been ruled out.

---

## 5. Automation Testing

### Q25. Why do we need automation testing?

**Answer:**
There are three reasons.

1. **Repeatability.** A regression suite that runs on every commit detects breakage immediately. This frequency is not achievable manually.

2. **Consistency.** Automated tests do not skip steps or vary between runs.

3. **Coverage at scale.** Once the framework exists, running additional tests costs almost nothing.

One clarification: the main value is not that automation finds more defects than manual testing. The main value is that it frees engineers to perform exploratory testing and usability evaluation, which automation cannot do.

---

### Q26. What should be automated and what should remain manual?

**Answer:**

Automate:
- Regression tests
- Data-driven tests with many input combinations
- API tests
- Smoke and sanity suites
- Any test that runs repeatedly in CI

Keep manual:
- Exploratory testing
- Usability and user experience evaluation
- One-off checks
- Tests that require human judgment
- Features whose interface is still changing rapidly, because maintenance cost exceeds the benefit

This is a cost decision, not a fixed rule. Automating the wrong test produces a suite that is expensive to maintain and frequently fails.

---

### Q27. What automation tools have you used?

**Answer:**
I have not used Selenium or Playwright in a professional testing context yet.

What I have done:

1. I have written Python validation code for machine learning pipelines, checking data integrity and model output correctness.

2. I have tested my own web applications by sending API requests and verifying responses and status codes.

3. I have set up Dockerized services with health checks, which is infrastructure for automated verification.

4. In the competitive programming committee, I validated test cases for contest problems.

I understand the concepts and the tooling landscape. Hands-on framework experience is what I intend to build during this internship.

---

### Q28. How does Selenium work internally?

**Answer:**
I have not used Selenium directly, so I will explain my understanding of the mechanism.

The flow is: the test code uses the Selenium client library, which sends commands over the W3C WebDriver protocol, usually over HTTP, to a browser driver such as chromedriver. The driver translates the commands into the browser's native automation interface, executes them, and returns the result.

There is no additional layer beyond this: it is a remote control protocol. This also explains why synchronization is the most difficult part of interface automation, because the test executes at a different speed than the browser and the network.

---

### Q29. What are the challenges of automation testing?

**Answer:**
There are four main challenges.

1. **Flakiness.** Tests that pass and fail without code changes, usually caused by timing issues, test interdependency, or environment instability. This is also a trust problem: once the team starts ignoring failures, the automation no longer serves its purpose.

2. **Maintenance.** Interface automation breaks whenever the interface changes.

3. **Initial investment.** A framework requires significant time before it produces value.

4. **Scope decisions.** A suite that attempts to test everything becomes expensive and slow.

The challenge I consider most important is flakiness, because it reduces trust in the entire suite.

---

### Q30. How do you maintain automation scripts when the application changes?

**Answer:**
I structure the code so that changes remain localized.

1. **Page Object Model.** All locators and page interactions for a page are stored in one file. When the interface changes, one file is updated instead of many test files.

2. **Stable locators.** Prefer semantic attributes such as data-testid or accessibility roles over CSS selectors tied to styling.

3. **Independent tests.** One failing test should not cause other tests to fail.

4. **Separation between test code and application code.** Application refactoring should not break the test structure.

The principle is that test code requires the same design discipline as production code, because the team pays its maintenance cost every sprint.

---

### Q31. What is the difference between UI testing and API testing?

**Answer:**

**UI testing** verifies the interface end to end. It validates the full stack and detects presentation and interaction issues, but it is slow, brittle, and expensive to maintain.

**API testing** verifies the service layer directly: a request is sent and the response is verified. It is faster, more stable, and better suited for validating business logic, edge cases, and error handling.

The key distinction: a UI test indicates that an incorrect value appeared on the screen. An API test indicates whether the service returned an incorrect value or the interface displayed a correct value incorrectly. These are different defects with different fixes.

My approach would be to push as much validation as possible to the API and unit level, and reserve interface tests for critical user journeys.

---

## 6. Scenario: Test Case Design

### Q32. How would you test tiket.com flight search?

**Answer:**

**Positive cases:**
- Search CGK to DPS with a valid future date and one adult passenger
- Results display with the correct route and price
- Round-trip search returns both legs correctly
- Filters by direct flight, airline, and departure time return correct results
- Sorting by cheapest and by fastest returns the correct order
- Passenger composition of adults, children, and infants applies the correct pricing rules
- Changing the origin or destination refreshes the results
- Selecting a date from the date picker applies the correct date
- Result cards display all required information: price, duration, stops, and baggage allowance

**Negative cases:**
- Origin and destination are the same
- Departure date is in the past
- Return date is earlier than the departure date
- Passenger count is zero
- Passenger count exceeds the maximum
- Infant count exceeds adult count
- Origin or destination field is empty
- Airport name does not exist
- Special characters in the city field
- Route has no available flights
- Search button is double-clicked
- Airline API times out
- Browser back button is used after a search

**Expected behavior for negative cases:**
Each negative case must return an explicit error or an informative empty state. The system must not return a server error, display a blank page, or leave the interface in an inconsistent state.

**Boundary cases:**
- Departure date is today
- Departure date is tomorrow
- Passenger count at the maximum
- Passenger count one above the maximum
- Furthest bookable date
- One day beyond the furthest bookable date

**Non-functional cases:**
- Performance: results load within an acceptable time, including under concurrent load
- Compatibility: Chrome, Safari, Firefox, mobile web, and the mobile application
- Accessibility: keyboard navigation through filters and labels on the date picker

**Integration layer:**
The results come from third-party airline APIs. I would test the behavior when a supplier times out, returns an empty response, or returns malformed data. I would also verify that the price displayed in search matches the price displayed at checkout, because a mismatch at that point affects user trust.

---

### Q33. How would you test the payment flow?

**Answer:**

**Positive cases:**
- Successful payment with a valid card
- Booking is confirmed and a confirmation email is sent
- Each payment method, including card, virtual account, and e-wallet, completes correctly
- Valid promo code applies the correct discount
- Total calculation matches base price plus tax plus service fee minus discount
- E-ticket or voucher is delivered after successful payment
- Booking record is created in the database with correct values

**Negative cases:**
- Card is expired
- CVV is incorrect
- Card number is invalid
- E-wallet balance is insufficient
- Promo code is invalid or expired
- Promo code has already been used
- Payment times out
- Browser is closed after payment is submitted but before the callback
- Pay button is double-clicked
- Price is modified in the client payload
- Inventory sells out during the flow
- Passenger data is incomplete
- Network fails during payment

**Expected behavior for negative cases:**
- Payment is declined with an explicit message
- No booking is created unless payment is confirmed
- Booking status remains pending or failed after a timeout, never confirmed
- Server-side pricing is used, not client-side values
- No partial or duplicated records are created

**Boundary cases:**
- Payment amount at the minimum and maximum allowed values
- Promo discount equal to the full amount, where the total must not become negative

**Concurrency:**
Two users attempting to book the last available seat or room at the same moment. Exactly one must succeed and the other must receive an explicit failure. Both must never succeed. This case requires concurrent requests and cannot be verified through interface interaction.

**Data integrity verification:**
After the flow completes, I would verify in the database:
- The booking record was created exactly once
- Inventory decreased by exactly one
- The recorded amount matches the amount reported by the payment gateway

The confirmation page can display correctly while the database contains a duplicate record or an incorrect amount.

---

### Q34. Users report that booking sometimes fails. How would you investigate?

**Answer:**
First, I would narrow the report, because "sometimes" cannot be tested. I need to establish which payment method, which route, which device, and at what time the failure occurred. The time is important because it indicates whether the failures correlate with peak load.

Second, I would examine the data, because it is the fastest source of information:
- Are failures concentrated on one payment method or one supplier?
- Are there bookings marked as failed where the payment succeeded? This case is the most serious, because it means the payment was collected without delivering the booking.
- Is there a pattern in the failure status values?

Third, I would check logs and monitoring around the failure times.

Fourth, I would attempt to reproduce the defect deterministically once a hypothesis exists, using concurrent requests if the pattern suggests a race condition.

In most cases, an intermittent failure has a pattern. The task is to identify the variable that separates failures from successful transactions.

---

### Q35. A feature works on Chrome but fails on Safari. What do you do?

**Answer:**
First, I would define the failure precisely: which function fails, and whether the failure is consistent or intermittent on Safari. Consistency provides useful information.

Second, I would check the common causes:
- JavaScript features that Safari does not support or implements differently
- Date and timezone handling, which differs between Safari and Chrome and commonly affects date pickers
- CSS rendering differences
- Third-party library compatibility

The browser console and network tab in Safari usually indicate the mechanism.

Third, I would report it as a compatibility defect with the exact environment details, and confirm whether Safari is included in the supported browser matrix. If it is supported, the defect is valid. If it is not supported, it is a known limitation.

To prevent recurrence, critical journeys should have cross-browser coverage in the automation suite.

---

### Q36. How would you test an application with millions of users?

**Answer:**
At that scale, exhaustive testing is not possible, so the approach becomes risk management supported by production observability.

Four focus areas:

1. **Critical paths at scale.** Performance and load testing of the highest-traffic flows, because at millions of users the primary failure mode is capacity and concurrency rather than logic.

2. **Device and network diversity.** Different devices, networks, and regions, because a travel platform is used under widely varying conditions.

3. **Third-party dependencies.** External APIs and payment gateways account for a large share of production incidents, so contract testing and graceful degradation are essential.

4. **Production monitoring and staged rollouts.** Canary releases and fast rollback capability, because not all defects can be detected before release.

The approach changes from "did we test everything" to "how do we detect and contain what we missed".

---

## 7. SQL and API

### Q37. What is SQL used for in testing?

**Answer:**
SQL is used to verify the state of the data, which the interface and API cannot confirm on their own.

Three main uses:

1. **Data verification.** After a booking, does the record exist exactly once, are the values correct, and does the payment status match the booking status?

2. **Test data setup and cleanup.** Creating preconditions and removing test data afterward so that tests remain independent.

3. **Defect investigation.** Checking whether incorrect data explains a defect observed at the interface.

Interface-level verification cannot detect data problems. A booking confirmation page can display correctly while the database contains a duplicate record.

---

### Q38. What is the difference between WHERE and HAVING?

**Answer:**
WHERE filters rows before aggregation. HAVING filters groups after aggregation.

This is why WHERE cannot reference aggregate functions. Using COUNT(*) in a WHERE clause produces an error, because grouping has not occurred at that point.

To find users with more than three orders, the query groups by user and applies HAVING COUNT(*) > 3.

---

### Q39. Explain INNER JOIN and LEFT JOIN.

**Answer:**

**INNER JOIN** returns only the rows where the join condition matches in both tables.

**LEFT JOIN** returns all rows from the left table together with matching rows from the right table, with NULL values where no match exists.

LEFT JOIN is the more useful of the two for testing, because it identifies missing relationships. For example, users with no orders, or bookings with no corresponding payment record. This is a data integrity check.

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

The second query is the one I would run after any change to the payment flow. The same user receiving two confirmations for the same offer on the same day indicates a double submission problem. It is a data-level check for a defect that is difficult to detect at the interface.

---

### Q41. Given Users and Orders tables, find all users who have completed orders.

**Answer:**

```sql
SELECT DISTINCT u.user_id, u.name
FROM users u
INNER JOIN orders o ON u.user_id = o.user_id
WHERE o.status = 'completed';
```

In addition, as a tester I would run integrity checks rather than reporting queries:

```sql
-- Orders with no valid user
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

The first query answers a reporting question. The remaining queries answer integrity questions, and that is where defects are usually found. An order with no valid user should never exist, and no interface test would detect it.

---

### Q42. What is API testing?

**Answer:**
API testing validates the service layer directly by sending requests and verifying the responses, without going through the interface.

It covers:
- Correctness of response data
- Status codes
- Error handling
- Authentication and authorization
- Behavior with invalid input

The service layer contains the business logic, so it is the most efficient layer for detecting defects. API tests are faster than interface tests, more stable, and capable of detecting problems that the interface does not expose.

---

### Q43. Why do we test APIs and not only the UI?

**Answer:**
Because many defects are invisible or ambiguous at the interface level.

An interface test indicates that an incorrect value appeared on the screen. An API test indicates whether the service returned an incorrect value or the interface displayed a correct value incorrectly. These are different defects with different fixes.

In practice, API tests execute faster, break less often when the interface changes, cover error handling that the interface does not expose, and can run in CI on every commit.

The interface is still tested, but for user journeys rather than for validating business logic.

---

### Q44. What HTTP methods do you know?

**Answer:**
- **GET** retrieves data and does not change state
- **POST** creates a resource or submits data
- **PUT** replaces a resource entirely and is idempotent
- **PATCH** partially updates a resource
- **DELETE** removes a resource

The property that matters most for testing is idempotency. GET, PUT, and DELETE can be safely retried. POST cannot, which is why payment flows require an idempotency key. If a payment POST is retried after a network timeout without an idempotency key, the result is a duplicate charge. This is a case I would test explicitly.

---

### Q45. What HTTP status codes do you know?

**Answer:**
- 2xx: 200 OK, 201 Created, 204 No Content
- 3xx: redirects
- 4xx: 400 Bad Request, 401 Unauthenticated, 403 Forbidden, 404 Not Found, 409 Conflict, 422 Validation Error, 429 Too Many Requests
- 5xx: 500 Internal Server Error, 502 Bad Gateway, 503 Service Unavailable, 504 Gateway Timeout

The distinction I verify carefully is 401 versus 403, because they have different meanings. 401 indicates the request is not authenticated. 403 indicates the request is authenticated but not authorized. A test that expects "access denied" should verify which of the two is returned.

---

### Q46. How do you validate an API response?

**Answer:**

1. **Status code.** Verify the response code matches the expected outcome for the request.

2. **Response structure.** Verify the schema and that all required fields are present.

3. **Response values.** Verify the data values are correct, and that calculated fields are correct rather than only present.

4. **Headers.** Verify content type, caching behavior, and rate limit headers where applicable.

5. **Negative behavior.** Verify that invalid requests return the appropriate error code with an explicit message, and do not create partial data.

6. **Authorization.** Verify that a resource belonging to another user cannot be accessed by modifying an identifier in the request. This check is high value and is not visible at the interface level.

7. **Database consistency.** Verify that the effect reported by the API actually occurred in the database.

---

## 8. Behavioral

### Q47. Tell me about a difficult problem you solved.

**Answer:**
In the Bayucaraka UAV team, I worked as a programming and electrical engineer, implemented fire detection using YOLO, and built the cloud component that transferred images and videos to AWS S3 for delivery to the ground control station.

The model was not the difficult part. The difficulty came from the surrounding constraints: the field conditions, transferring data from the aircraft reliably, and the processing limits on board. Processing everything on the aircraft was too resource intensive, so part of the processing was moved to the cloud.

What I learned is that in a real system, the model is usually not the limiting factor. The integration points and the operating constraints are. This changed how I approach problems: I examine the entire system rather than only the component I was assigned.

---

### Q48. Tell me about a time you worked with a team.

**Answer:**
In the UAV team, my work had to integrate with work handled by other people, covering hardware, flight control, and software. All components had to function together for the aircraft to fly.

The most useful action I took was defining my component's inputs, outputs, and assumptions early, so that others could plan around it without being blocked.

I also reported problems in the shared channel rather than saving them for meetings, because a blocked component affects the entire team.

What I learned is that completing my own work is not sufficient. The rest of the team needs to know what my component does and when it will be available.

---

### Q49. Tell me about a disagreement and how you solved it.

**Answer:**
In the UAV team, there was a difference of opinion about the approach. I wanted to allocate time differently than the existing method, and there was a reasonable concern about whether that change was worth the risk given the competition deadline.

I moved the discussion from preference to criteria: what are we optimizing for, reliability or time? Once the question was framed that way, the decision became clearer rather than a disagreement between two positions. I also acknowledged that the concern about the deadline was valid.

We agreed on an approach that addressed both concerns, and it worked in competition.

What I learned is that most technical disagreements concern priorities rather than facts. When the actual trade-off is made explicit, the disagreement usually resolves itself.

---

### Q50. Tell me about a failure and what you learned.

**Answer:**
For my PKM project, our team proposed a two-way BISINDO sign language translator using a Transformer-based approach. The project received funding, which indicated that the proposal was accepted.

What I underestimated was the data. Sign language datasets are difficult to obtain, and building a sufficient volume of correctly labelled data required substantially more work than planned. We spent more time on data collection and preprocessing than expected, and less time on iteration and evaluation than the project required.

What I changed afterward is how I plan projects. I now place the data work at the beginning of the plan, because in machine learning the data pipeline is usually the longest part of the work, not the model. I also validate with the smallest working version rather than assuming the full plan will hold.

I applied this when building my RAG chatbot. I built a minimal end-to-end pipeline first, and I built the evaluation early so that I would know quickly whether the system was working.

---

### Q51. How do you handle pressure and deadlines?

**Answer:**
I separate what is essential from what is optional, and I complete the essential work first.

When several deadlines overlap, the approach that works is deciding the minimum acceptable outcome for each item. Attempting to complete everything at full quality under time pressure usually results in everything being late.

The second part is communicating early. If a deliverable is at risk, the people affected need to know while they can still plan around it.

I also state clearly what cannot be completed within the available time, rather than assuming it will work out.

---

### Q52. Explain your previous project.

**Answer:**
One of my projects is a RAG chatbot that answers questions about Indonesian legislation, specifically UU TNI No. 34 of 2004 and its revisions.

**How it works:**
1. Documents are loaded from PDF, JSON, and web sources
2. Documents are split into chunks with controlled overlap
3. Embeddings are generated using a multilingual sentence-transformer model
4. Embeddings are stored in a FAISS vector index
5. At query time, hybrid retrieval combines BM25 keyword search with semantic search
6. Results are merged and reranked using a cross-encoder
7. The top results are passed to the language model as context
8. The application displays the source documents used, so the user can verify the answer

**Evaluation:**
I fine-tuned several open-source models using LoRA with 4-bit quantization, and compared RAG against fine-tuning across those models, using LLM-as-judge, ROUGE, and BERTScore, with experiments tracked in Weights & Biases.

**What I learned:**
The evaluation showed that the results were lower than expected. Precision was low for several configurations. This was the most valuable result of the project, because it identified a real problem. The documents were complex and came from multiple sources, and the retrieval required improvement. I added hybrid retrieval and reranking in response to that finding.

The lesson is that evaluation is more important than the pipeline, because without evaluation there is no way to know whether the system is failing.

---

### Q53. What was your contribution to that project?

**Answer:**
I built the RAG system: the document processing, the retrieval layer including hybrid search and reranking, and the application with multi-model support and source display.

To be precise, because it was a collaborative academic project: the model fine-tuning and evaluation runs were distributed across the team, with different models handled by different members. I was responsible for the RAG implementation and for my portion of the evaluation work.

The retrieval and application components were entirely my work. The evaluation contributions were shared.

---

### Q54. If you could improve that project, what would you change?

**Answer:**
In priority order:

1. **Better evaluation metrics.** RAGAS, to measure faithfulness, answer relevance, context precision, and context recall separately. The existing evaluation measured the final answer but could not determine whether the failure was in retrieval or in generation.

2. **Metadata filtering.** Tagging document types so that a legal text and a news article are not treated as equally reliable for a legal question.

3. **Query rewriting.** User questions are often vague, and rewriting them before retrieval is a low-cost improvement.

4. **Production vector database.** Moving from FAISS to Qdrant or Milvus, because FAISS does not handle concurrent access and metadata filtering in the way a production vector database does.

5. **Automated tests.** The project has no automated test suite. I built the system and evaluated its outputs, but I did not write anything that runs on every change. This is the most significant gap in the project.

---

## 9. Closing

### Q55. What do you expect from this internship?

**Answer:**
Three things.

1. I want to learn how testing is performed at scale. I have built systems independently, but I have not seen how quality is managed professionally, with defined release processes and production automation infrastructure.

2. I want to work with engineers who are more experienced than I am. I have been self-taught in many areas, and that environment is the fastest way to close the remaining gaps.

3. I want to own a real deliverable, even a small one. I am not looking for a placement where I only observe.

---

### Q56. Do you have any questions for us?

**Answer:**

Select two or three:
1. "What would a successful SDET intern look like in their first three months on your team?"
2. "How do the SDETs and developers work together in the release process?"
3. "What does the automation coverage look like today, and what are you working towards?"
4. "What is the hardest quality problem the team has faced recently?"

Do not ask about salary at this stage.

---

## Notes

1. If asked about test cases, do not stop at positive cases. The negative cases are the main focus of this session.
2. Do not claim experience with Selenium or Playwright. State clearly what you have done instead.
3. Keep answers between 30 and 60 seconds.
4. If you do not know an answer: "I have not worked with that directly. Based on my understanding, I would approach it this way, but I would confirm with someone who has."
5. Pause briefly before answering difficult questions. This indicates consideration rather than uncertainty.

---

## Final practice

Take one feature. State out loud, without reading:
- 5 positive test cases
- 5 negative test cases
- 3 boundary cases

Do this for **flight search** and **payment**.
