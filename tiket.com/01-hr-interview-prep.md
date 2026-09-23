# tiket.com SDET Intern — HR Interview Prep Guide
## Tailored for Thariq Ivan Anendar

---

## 📋 Interview Overview

**Position:** SDET Intern (R-3135)
**Company:** tiket.com (Global Tiket Network)
**Location:** Jakarta, Indonesia
**Format:** HR Interview in English (Round 2 of 4)

**Key Focus Areas:**
- Motivation & Career Goals
- Behavioral (STAR Method)
- Cultural Fit & Teamwork
- Technical Communication
- Company Knowledge

---

## 🎯 MOTIVATION QUESTIONS

### Q1: "Tell me about yourself"

**Answer:**
"Hi, I'm Thariq, but my friends and family call me Ivan. I'm a Computer Science graduate from Institut Teknologi Sepuluh Nopember, commonly known as ITS.

Since high school, I've been fascinated by programming — not just writing code, but understanding how software systems solve real human problems. This curiosity led me to build projects like a job tracking application using Next.js and Supabase, and a computer vision system for bird recognition using YOLO and ResNet.

What excites me about SDET is the bridge it creates between development and quality — ensuring that the software we build actually works reliably for millions of users. I'm eager to bring my analytical mindset and testing passion to tiket.com's engineering team."

**Delivery Tips:**
- Speak naturally, not rushed
- Maintain eye contact
- Smile when mentioning your name
- Pause briefly between sections

---

### Q2: "Why do you want to be an SDET?"

**Answer:**
"I choose SDET because it perfectly combines my two strengths: coding and problem-solving.

Throughout my projects, I've realized that building software is only half the battle — ensuring it works correctly across all scenarios is equally important. When I was developing my job tracker app, I spent significant time testing edge cases: what happens when a user has no data, what if the API times out, what if two users edit simultaneously.

SDET lets me turn that analytical mindset into a career. I want to design automated tests that catch issues before they reach users, and I'm excited to apply this at scale with tiket.com's 50 million users."

**Key Points to Hit:**
- Coding + Testing = SDET
- Personal experience with testing challenges
- Understanding of scale impact

---

### Q3: "Why tiket.com?"

**Answer:**
"First, tiket.com is a pioneer — being the first OTA in Indonesia means you've solved problems nobody else has faced, and that's exciting to me as an engineer.

Second, the scale is compelling. Serving 50 million users means every bug has real impact, and every improvement touches millions of travel experiences. That's the kind of environment where testing matters.

Third, I've personally used tiket.com to book flights and hotels, and I've always appreciated how intuitive the UI is despite the complexity behind it. I want to contribute to maintaining that quality — ensuring that when users need to travel, the platform works seamlessly.

Finally, being part of the Blibli Tiket ecosystem gives me exposure to a larger tech organization while still having the agility of a focused product team."

**Research Highlights:**
- Pioneer OTA in Indonesia (2011)
- 50+ million users
- Part of Blibli Tiket ecosystem (IDX: BELI)
- Fastest-growing OTA by Sabre

---

### Q4: "What do you know about the SDET role?"

**Answer:**
"An SDET, or Software Development Engineer in Test, is different from a traditional QA role because it combines development skills with testing expertise. While a QA engineer might focus on manual testing and test case execution, an SDET builds the automation frameworks and tools that make testing scalable.

At tiket.com, I understand the SDET role involves designing test cases for new features, collaborating with developers on quality standards, and contributing to test automation. Given the platform's scale, automated testing is essential — you can't manually test every flow for 50 million users.

My background in full-stack development with Next.js and Python gives me the technical foundation, and my systematic approach to problem-solving aligns with the testing mindset."

---

### Q5: "What are your strengths?"

**Answer:**
"My biggest strength is systematic problem-solving. Before I write any code, I evaluate all possible scenarios — what could go wrong, what edge cases exist, what happens under stress. This analytical approach naturally aligns with testing.

For example, when building my job tracker application, I created a mental model of every user interaction before implementing it. This helped me identify potential issues early — like race conditions when multiple users update the same application status.

I'm also a quick learner. When I needed computer vision for my bird recognition project, I taught myself YOLO and ResNet from scratch within a few weeks. I'm confident I can quickly pick up tiket.com's testing tools and frameworks."

---

### Q6: "What is your weakness?"

**Answer:**
"Honestly, I sometimes spend too much time analyzing before starting. I want to understand every aspect of a problem before diving in, which can slow down my initial progress.

However, I've learned to balance this by setting time limits for research. If I'm stuck analyzing for more than 30 minutes, I start with the simplest approach and iterate. This has actually made me faster overall because I avoid major architectural mistakes early on.

For an SDET role, I think this tendency is actually beneficial — being thorough in test design is better than being fast and missing critical cases."

**Why This Answer Works:**
- Real weakness, but not a dealbreaker
- Shows self-awareness
- Turns weakness into strength for the role
- Demonstrates growth

---

## 🎭 BEHAVIORAL QUESTIONS (STAR Method)

### Q7: "Tell me about a time you solved a difficult problem"

**Situation:**
"While developing my job tracker application, I encountered a critical issue: the real-time synchronization between multiple users was causing data conflicts. When two users updated the same job application simultaneously, one user's changes would overwrite the other's."

**Task:**
"I needed to implement a solution that preserved all updates without data loss, while maintaining the real-time feel that makes the app responsive."

**Action:**
"I researched conflict resolution strategies and implemented an optimistic locking approach with version vectors. Each record now has a version number, and before any update, the system checks if the version matches. If there's a conflict, the system merges changes intelligently rather than overwriting."

**Result:**
"The solution eliminated data conflicts entirely. User testing showed that even with 5 concurrent users on the same record, all changes were preserved. This experience taught me the importance of thinking through edge cases in distributed systems — a skill directly applicable to testing at tiket.com's scale."

---

### Q8: "Describe a time you worked in a team"

**Situation:**
"For my computer vision project on bird recognition, I collaborated with two classmates. We had different skill sets — one was strong in Python, another in machine learning, and I handled the full-stack integration."

**Task:**
"We needed to build a system that could identify individual birds from CCTV footage, with a web interface for managing the dataset. The deadline was 8 weeks."

**Action:**
"I proposed dividing the work by component: my teammate handled the ResNet model training, another focused on the YOLO detection pipeline, and I built the Next.js frontend and FastAPI backend. We used Git with feature branches and held weekly sync meetings to ensure our components integrated properly."

**Result:**
"We delivered on time with a working system that achieved 94% accuracy in identifying individual birds. The experience taught me clear communication and component-based development — skills essential for working with tiket.com's engineering teams."

---

### Q9: "Tell me about a project you're most proud of"

**Answer:**
"I'm most proud of my job tracker application called Huntly. It's a full-stack web app that helps users track their job applications through the entire hiring pipeline — from discovering opportunities to receiving offers.

What makes me proud isn't just the features, but the engineering decisions behind it. I implemented Google OAuth for seamless authentication, Supabase Row Level Security for data isolation, and real-time updates so users see changes instantly. The app handles complex state management across multiple pipeline stages.

The project taught me end-to-end product development — from database schema design to UI/UX decisions to deployment on Vercel. More importantly, it showed me how technology can solve real problems. I've been using it myself to track my own job applications, including my application to tiket.com."

**Connecting to SDET:**
"Building this end-to-end gave me appreciation for testing at every layer — database queries, API endpoints, authentication flows, and UI interactions. That's exactly the perspective an SDET brings."

---

### Q10: "Tell me about a time you failed"

**Situation:**
"In my second year of university, I joined a programming competition. I was confident in my coding skills and didn't prepare adequately for the algorithm portion."

**Task:**
"We had 4 hours to solve 8 algorithmic problems, and the competition was fierce — teams from top universities across Indonesia."

**Action:**
"I struggled with the dynamic programming problems and only solved 3 out of 8. After the competition, instead of being discouraged, I analyzed what went wrong. I realized my weakness was pattern recognition in algorithms, not coding itself."

**Result:**
"I created a systematic study plan, solving 2-3 algorithm problems daily for the next 3 months. When the next competition came, I solved 6 out of 8 problems and our team placed in the top 10. This experience taught me that failure is just feedback — a mindset I bring to testing, where every bug found is an improvement made."

---

### Q11: "How do you handle pressure or tight deadlines?"

**Answer:**
"I break the problem into smaller, manageable pieces and prioritize ruthlessly. When everything feels urgent, I ask: what's the minimum viable deliverable?

For example, during my final semester, I had three project deadlines in the same week. Instead of panicking, I created a priority matrix: which projects had the highest impact, which had dependencies, and which could be simplified without losing quality.

I focused on completing the core features first, then added polish where time allowed. All three projects were submitted on time, and I learned that perfectionism is the enemy of progress — good enough on time is better than perfect late.

For SDET work, this translates to: test the critical paths first, then expand coverage. You can't test everything, but you can prioritize what matters most to users."

---

### Q12: "Describe a time you had to learn something new quickly"

**Situation:**
"For my bird recognition project, I needed to implement computer vision using YOLO for object detection and ResNet for identity recognition. I had zero experience with either technology."

**Task:**
"I had 6 weeks to build a working system that could detect and identify individual birds from video footage."

**Action:**
"I started with the fundamentals — watching tutorial series on computer vision basics. Then I focused on practical implementation: first getting YOLO to detect objects, then training ResNet on our custom dataset. I leveraged pre-trained models and fine-tuned them rather than building from scratch."

**Result:**
"Within 6 weeks, I had a working system with 94% identification accuracy. The key was structured learning: theory first, then small experiments, then full implementation. I applied the same approach when learning Supabase for my job tracker — and I'm ready to apply it to tiket.com's testing frameworks."

---

## 🤝 CULTURAL FIT QUESTIONS

### Q13: "How do you handle working with difficult team members?"

**Answer:**
"I focus on understanding their perspective first. Usually, what seems 'difficult' is actually a communication mismatch or different working styles.

In a group project, one teammate preferred working late at night while I'm a morning person. Instead of forcing synchronization, we established a handoff protocol — he'd commit his code before sleeping, and I'd review and integrate it in the morning. We communicated through detailed code comments and a shared document.

The result was actually more efficient than real-time collaboration. I learned that adapting to different working styles, rather than forcing conformity, leads to better outcomes. At tiket.com, I'd apply the same flexibility — understanding how each team member works best and finding ways to collaborate effectively."

---

### Q14: "How do you handle receiving feedback?"

**Answer:**
"I view feedback as data — it helps me improve. When a professor pointed out that my database schema was inefficient during a code review, my first reaction was defensiveness. But then I asked specific questions: which queries were slow, what would be better?

I restructured the schema, and the performance improved 3x. More importantly, I learned to seek feedback actively rather than waiting for it. Now, after completing a feature, I ask teammates: 'What would you do differently?'

For testing specifically, feedback is everything — every bug report, every test failure, is information that makes the product better. I'm excited to receive feedback from tiket.com's senior engineers to accelerate my growth."

---

### Q15: "Why should we hire you over other candidates?"

**Answer:**
"Three reasons:

First, I bring a full-stack perspective. I've built applications from database to UI, which means I understand how systems break at every layer — exactly the mindset needed for effective testing.

Second, I'm systematic. My approach to problem-solving — evaluating all scenarios before acting — translates directly to test case design. I don't just test the happy path; I think about edge cases, error states, and race conditions.

Third, I'm genuinely excited about tiket.com. This isn't just any internship for me — I've used the product, I understand the scale challenges, and I want to contribute to a platform that 50 million people rely on. That motivation will show in my work quality."

---

### Q16: "Where do you see yourself in 3 years?"

**Answer:**
"In 3 years, I see myself as a skilled SDET who has grown from an intern to a confident contributor. I want to have deep expertise in test automation frameworks, performance testing, and quality engineering practices.

Specifically, I hope to have contributed to significant testing infrastructure improvements at tiket.com — perhaps building automation frameworks that catch issues earlier, or developing testing tools that the whole team uses.

Long-term, I'm interested in quality engineering leadership — not just finding bugs, but building systems that prevent them. The best testing is the kind that's built into the development process from day one."

---

### Q17: "Do you have any questions for us?"

**Yes! Prepare 2-3 questions:**

**Question 1:**
"Can you tell me about the team structure for SDETs at tiket.com? How do SDETs collaborate with developers and product managers?"

**Question 2:**
"What testing tools and frameworks does the team currently use? I'm curious about the tech stack I'd be working with."

**Question 3:**
"What does a successful first month look like for an SDET intern at tiket.com?"

**Question 4 (if talking to HR):**
"What's the typical career progression for SDETs at tiket.com? Are there opportunities to convert to full-time?"

**Never Ask:**
- Salary questions in HR round (save for later)
- "What does your company do?" (shows no research)
- Negative questions about the company

---

## 💡 TECHNICAL COMMUNICATION QUESTIONS

### Q18: "Explain a technical concept to a non-technical person"

**Question: "What is API testing?"**

**Answer:**
"Think of an API like a waiter in a restaurant. When you order food, you don't go directly to the kitchen — you tell the waiter what you want, and the waiter brings it back.

API testing is like testing that waiter: Can they take orders correctly? Do they bring the right food? What happens if the kitchen is closed? What if you order something that doesn't exist?

In software, APIs are how different parts of an application communicate. Testing APIs ensures that when our app asks for data — like flight prices — it gets the correct information back, even under unusual conditions."

---

### Q19: "How would you test a feature like flight search on tiket.com?"

**Answer:**
"I'd approach flight search testing in layers:

**Functional Testing:**
- Search with valid dates, airports, passengers
- Search with edge cases: same-day flights, far-future dates, round-trip vs one-way
- Verify results match actual airline availability

**Boundary Testing:**
- Maximum passengers (9+)
- Search with past dates
- Special characters in airport names
- Very long city names

**Performance Testing:**
- How fast do results load with 100 concurrent users?
- What happens during peak booking hours?

**Usability Testing:**
- Is the date picker intuitive?
- Can users easily modify search parameters?
- Are error messages clear when no flights exist?

Each layer catches different types of bugs. The goal is to ensure that no matter how users interact with the search, they get accurate, fast results."

---

## 📝 QUICK REFERENCE: KEY NUMBERS TO REMEMBER

| Stat | Value |
|------|-------|
| tiket.com users | 50+ million |
| Founded | 2011 |
| Parent company | Blibli (IDX: BELI) |
| Key product | OTA (flights, hotels, attractions) |
| Office location | Jakarta, Indonesia |

---

## 🎤 DELIVERY TIPS

**Body Language:**
- Sit up straight, lean slightly forward
- Maintain eye contact (look at camera if virtual)
- Use hand gestures naturally
- Smile when appropriate

**Voice:**
- Speak clearly and not too fast
- Pause between sentences
- Vary your tone (avoid monotone)
- Project confidence, not arrogance

**Structure:**
- Start answers with a clear statement
- Use examples to support points
- End with a conclusion or lesson learned

**When You Don't Know:**
- "That's a great question. Let me think about that for a moment..."
- It's okay to pause and collect your thoughts
- Never make up an answer

---

## ✅ FINAL CHECKLIST

**Before the Interview:**
- [ ] Research tiket.com recent news
- [ ] Download tiket.com app and explore
- [ ] Prepare 2-3 questions to ask
- [ ] Practice answers out loud (3x minimum)
- [ ] Prepare your environment (quiet, good lighting)

**During the Interview:**
- [ ] Greet with a smile
- [ ] Listen carefully before answering
- [ ] Use STAR method for behavioral questions
- [ ] Connect your experiences to SDET role
- [ ] Ask thoughtful questions at the end

**After the Interview:**
- [ ] Send thank-you email within 24 hours
- [ ] Note down questions you were asked
- [ ] Reflect on areas to improve

---

**Good luck, Ivan! 🍀**

Remember: You have the skills, projects, and passion. The interview is just about communicating that effectively. Be yourself, be confident, and show them why you're the right fit for tiket.com's SDET Intern role.
