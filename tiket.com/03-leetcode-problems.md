# LeetCode Problems for tiket.com SDET Intern
## Curated List by Topic

---

## PRIORITY 1: String Problems (Must Do)

| # | Problem | Difficulty | Why It Matters |
|---|---------|------------|----------------|
| **344** | [Reverse String](https://leetcode.com/problems/reverse-string) | Easy | Classic, tests array/string manipulation |
| **125** | [Valid Palindrome](https://leetcode.com/problems/valid-palindrome) | Easy | String parsing, two pointers |
| **387** | [First Unique Character in a String](https://leetcode.com/problems/first-unique-character-in-a-string) | Easy | HashMap, character counting |
| **242** | [Valid Anagram](https://leetcode.com/problems/valid-anagram) | Easy | HashMap, string comparison |
| **14** | [Longest Common Prefix](https://leetcode.com/problems/longest-common-prefix) | Easy | String manipulation, edge cases |
| **20** | [Valid Parentheses](https://leetcode.com/problems/valid-parentheses) | Easy | Stack, classic interview question |
| **49** | [Group Anagrams](https://leetcode.com/problems/group-anagrams) | Medium | HashMap, sorting |
| **5** | [Longest Palindromic Substring](https://leetcode.com/problems/longest-palindromic-substring) | Medium | Two pointers, expand around center |

**Time:** 2-3 hours | **Focus:** String manipulation, two pointers, HashMap

---

## PRIORITY 2: Array Problems (Must Do)

| # | Problem | Difficulty | Why It Matters |
|---|---------|------------|----------------|
| **1** | [Two Sum](https://leetcode.com/problems/two-sum) | Easy | HashMap, most asked question |
| **26** | [Remove Duplicates from Sorted Array](https://leetcode.com/problems/remove-duplicates-from-sorted-array) | Easy | Two pointers, in-place modification |
| **27** | [Remove Element](https://leetcode.com/problems/remove-element) | Easy | Array manipulation |
| **35** | [Search Insert Position](https://leetcode.com/problems/search-insert-position) | Easy | Binary search |
| **53** | [Maximum Subarray](https://leetcode.com/problems/maximum-subarray) | Medium | Kadane's algorithm |
| **121** | [Best Time to Buy and Sell Stock](https://leetcode.com/problems/best-time-to-buy-and-sell-stock) | Easy | Greedy, tracking min |
| **217** | [Contains Duplicate](https://leetcode.com/problems/contains-duplicate) | Easy | HashSet |
| **238** | [Product of Array Except Self](https://leetcode.com/problems/product-of-array-except-self) | Medium | Prefix/suffix product |
| **283** | [Move Zeroes](https://leetcode.com/problems/move-zeroes) | Easy | Two pointers |
| **704** | [Binary Search](https://leetcode.com/problems/binary-search) | Easy | Fundamental algorithm |

**Time:** 3-4 hours | **Focus:** Two pointers, binary search, HashMap

---

## PRIORITY 3: OOP Design Problems (Conceptual)

**These are not LeetCode problems but practice OOP design:**

**Problem 1: Booking System**
```
Design classes for a booking system:
- Booking (base class): bookingId, price, status
- HotelBooking (extends Booking): hotelName, nights, checkIn
- FlightBooking (extends Booking): flightNumber, departure, arrival

Requirements:
- Each booking can be confirmed or cancelled
- Calculate total price based on type
- polymorphic behavior for confirm()
```

**Problem 2: User Management**
```
Design a User class with encapsulation:
- Private fields: username, email, password
- Constructor with validation
- Getters for username and email
- Method: getDisplayName() returns "username (email)"
- Method: validateEmail() checks format
```

**Problem 3: Payment System**
```
Design a payment system:
- PaymentProcessor (interface): processPayment(), refund()
- CreditCardPayment (implements PaymentProcessor)
- EWalletPayment (implements PaymentProcessor)

Requirements:
- Each payment type has different processing logic
- Support for payment validation
- Transaction history
```

---

## PRIORITY 4: SQL Problems (Practice on LeetCode)

| # | Problem | Difficulty | Why It Matters |
|---|---------|------------|----------------|
| **175** | [Combine Two Tables](https://leetcode.com/problems/combine-two-tables) | Easy | Basic JOIN |
| **176** | [Second Highest Salary](https://leetcode.com/problems/second-highest-salary) | Easy | Subquery, LIMIT |
| **177** | [Nth Highest Salary](https://leetcode.com/problems/nth-highest-salary) | Medium | Variables, subquery |
| **178** | [Rank Scores](https://leetcode.com/problems/rank-scores) | Medium | Window functions |
| **180** | [Consecutive Numbers](https://leetcode.com/problems/consecutive-numbers) | Medium | Self JOIN |
| **181** | [Employees Earning More Than Their Managers](https://leetcode.com/problems/employees-earning-more-than-their-managers) | Easy | Self JOIN |
| **182** | [Duplicate Emails](https://leetcode.com/problems/duplicate-emails) | Easy | GROUP BY, HAVING |
| **183** | [Customers Who Never Order](https://leetcode.com/problems/customers-who-never-order) | Easy | LEFT JOIN, IS NULL |
| **196** | [Delete Duplicate Emails](https://leetcode.com/problems/delete-duplicate-emails) | Easy | DELETE with subquery |
| **197** | [Rising Temperature](https://leetcode.com/problems/rising-temperature) | Easy | DATE functions, JOIN |

**Time:** 2 hours | **Focus:** JOIN, GROUP BY, subqueries

---

## PRIORITY 5: Hash Map / HashSet Problems

| # | Problem | Difficulty | Why It Matters |
|---|---------|------------|----------------|
| **1** | [Two Sum](https://leetcode.com/problems/two-sum) | Easy | Classic HashMap |
| **217** | [Contains Duplicate](https://leetcode.com/problems/contains-duplicate) | Easy | HashSet basics |
| **242** | [Valid Anagram](https://leetcode.com/problems/valid-anagram) | Easy | Character counting |
| **349** | [Intersection of Two Arrays](https://leetcode.com/problems/intersection-of-two-arrays) | Easy | HashSet operations |
| **387** | [First Unique Character in a String](https://leetcode.com/problems/first-unique-character-in-a-string) | Easy | HashMap + string |

---

## 12-HOUR LEETCODE SCHEDULE

### Hour 0-2: String Problems
- [ ] 344. Reverse String
- [ ] 125. Valid Palindrome
- [ ] 387. First Unique Character
- [ ] 242. Valid Anagram
- [ ] 20. Valid Parentheses

### Hour 2-4: Array Problems
- [ ] 1. Two Sum
- [ ] 26. Remove Duplicates
- [ ] 53. Maximum Subarray
- [ ] 121. Best Time to Buy/Sell
- [ ] 704. Binary Search

### Hour 4-5: HashMap Problems
- [ ] 217. Contains Duplicate
- [ ] 349. Intersection of Two Arrays
- [ ] 49. Group Anagrams (if time)

### Hour 5-6: SQL Problems
- [ ] 175. Combine Two Tables
- [ ] 176. Second Highest Salary
- [ ] 182. Duplicate Emails
- [ ] 183. Customers Who Never Order

### Hour 6-8: OOP Practice
- [ ] Design Booking System class hierarchy
- [ ] Design User class with encapsulation
- [ ] Implement Animal/Dog inheritance

### Hour 8-10: Mixed Practice
- [ ] Review all solved problems
- [ ] Re-solve any that took too long
- [ ] Focus on time complexity

### Hour 10-12: Mock Test
- [ ] Solve 3 random Easy problems in 30 min
- [ ] Solve 1 Medium problem in 30 min
- [ ] Review and optimize solutions

---

## JAVA SYNTAX REFERENCE FOR LEETCODE

**String:**
```java
String s = "hello";
s.length();
s.charAt(0);
s.substring(1, 3);
s.toCharArray();
String.valueOf(charArray);
s.equals("hello");
s.toLowerCase();
```

**Array:**
```java
int[] arr = new int[5];
arr.length;
Arrays.sort(arr);
Arrays.toString(arr);
Arrays.fill(arr, 0);
```

**HashMap:**
```java
HashMap<String, Integer> map = new HashMap<>();
map.put("key", 1);
map.get("key");
map.containsKey("key");
map.getOrDefault("key", 0);
map.values();
map.keySet();
```

**HashSet:**
```java
HashSet<Integer> set = new HashSet<>();
set.add(1);
set.contains(1);
set.remove(1);
set.size();
```

---

## TIPS FOR tiket.com CODING TEST

1. **Read the problem twice** before coding
2. **Start with brute force** solution, then optimize
3. **Think about edge cases**: null, empty, single element
4. **Write clean code** with meaningful variable names
5. **Test your solution** with given examples before submitting
6. **Time complexity matters**: O(n) better than O(n^2)
7. **Don't panic** if stuck — think out loud

---

## KEY PATTERNS TO REMEMBER

| Pattern | When to Use | Example Problem |
|---------|-------------|-----------------|
| **Two Pointers** | Sorted array, palindrome | Valid Palindrome |
| **HashMap** | Counting, lookup | Two Sum, Valid Anagram |
| **Binary Search** | Sorted array, search | Search Insert Position |
| **Sliding Window** | Substring/subarray | Longest Substring |
| **Stack** | Parentheses, nesting | Valid Parentheses |
| **Greedy** | Optimization | Best Time to Buy/Sell |

---

*Created for tiket.com SDET Intern Technical Test*
*Focus on Easy problems first, then Medium*
*Last updated: September 2, 2026*
