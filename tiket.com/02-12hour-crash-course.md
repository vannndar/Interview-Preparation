# tiket.com SDET Technical Test — 12-Hour Crash Course
## Your Workflow & Study Plan

---

## TIMELINE OVERVIEW

| Time | Activity | Duration |
|------|----------|----------|
| **Hour 0-1** | Install Java + Setup IDE | 1 hour |
| **Hour 1-4** | Java & OOP Fundamentals | 3 hours |
| **Hour 4-7** | Testing Concepts | 3 hours |
| **Hour 7-10** | Practice Problems | 3 hours |
| **Hour 10-12** | Mock Test + Review | 2 hours |

---

## HOUR 0-1: JAVA SETUP

### Step 1: Download JDK (5 minutes)
1. Go to: https://adoptium.net/temurin/releases/
2. Download **JDK 17** for Windows (x64 .msi)
3. Run installer, click Next through all steps

### Step 2: Verify Installation (2 minutes)
Open **Command Prompt** (not Git Bash) and type:
```
java -version
javac -version
```
You should see version info like `openjdk version "17.0.x"`

### Step 3: Install VS Code Extension (3 minutes)
1. Open VS Code
2. Go to Extensions (Ctrl+Shift+X)
3. Search and install: **"Extension Pack for Java"** by Microsoft
4. Restart VS Code

### Step 4: Create Your First Java Project (5 minutes)
1. Create folder: `C:\Users\WINDOWS\Desktop\java-practice`
2. In VS Code: File > Open Folder > select that folder
3. Create file: `Hello.java`

**Copy this code:**
```java
public class Hello {
    public static void main(String[] args) {
        System.out.println("Hello, tiket.com!");
    }
}
```

4. Run: Click the play button or press F5

**If it works, you are ready!**

---

## HOUR 1-4: JAVA & OOP FUNDAMENTALS

### Hour 1: Java Basics (60 min)

**Topics to cover:**
- Variables (int, String, boolean, double)
- Operators (+, -, *, /, %)
- If/else statements
- For loop, while loop
- Arrays

**Practice code:**
```java
public class Basics {
    public static void main(String[] args) {
        // Variables
        int age = 23;
        String name = "Ivan";
        boolean isStudent = true;
        double gpa = 3.75;
        
        // If/else
        if (age >= 18) {
            System.out.println(name + " is an adult");
        } else {
            System.out.println(name + " is a minor");
        }
        
        // For loop
        for (int i = 1; i <= 5; i++) {
            System.out.println("Count: " + i);
        }
        
        // Array
        String[] languages = {"Java", "Python", "JavaScript"};
        for (String lang : languages) {
            System.out.println("Language: " + lang);
        }
    }
}
```

---

### Hour 2-3: OOP Concepts (90 min)

**4 Pillars of OOP:**

**1. Class and Object**
```java
// Class = Blueprint
class Student {
    String name;
    int age;
    
    // Constructor
    Student(String name, int age) {
        this.name = name;
        this.age = age;
    }
    
    // Method
    void introduce() {
        System.out.println("Hi, I'm " + name + ", age " + age);
    }
}

// Object = Instance
public class Main {
    public static void main(String[] args) {
        Student student1 = new Student("Ivan", 23);
        student1.introduce();
    }
}
```

**2. Encapsulation (Private + Getters/Setters)**
```java
class BankAccount {
    private double balance;  // Private = cannot access directly
    
    public double getBalance() {  // Getter
        return balance;
    }
    
    public void deposit(double amount) {  // Setter with validation
        if (amount > 0) {
            balance += amount;
        }
    }
}
```

**3. Inheritance**
```java
class Animal {
    void eat() {
        System.out.println("Eating...");
    }
}

class Dog extends Animal {
    void bark() {
        System.out.println("Barking...");
    }
}

// Dog inherits eat() from Animal
```

**4. Polymorphism**
```java
class Shape {
    double area() {
        return 0;
    }
}

class Circle extends Shape {
    double radius;
    double area() {
        return Math.PI * radius * radius;
    }
}

class Rectangle extends Shape {
    double width, height;
    double area() {
        return width * height;
    }
}
```

---

### Hour 4: OOP Practice Problems (60 min)

**Problem 1: Create a User class**
```java
class User {
    private String username;
    private String email;
    
    // Constructor
    // Getter for username
    // Setter for email with validation (must contain @)
    // Method: getDisplayName() returns "username (email)"
}
```

**Problem 2: Inheritance - Booking System**
```java
class Booking {
    String bookingId;
    double price;
    
    void confirm() {
        System.out.println("Booking confirmed: " + bookingId);
    }
}

class HotelBooking extends Booking {
    String hotelName;
    int nights;
    // Override confirm() to include hotel details
}

class FlightBooking extends Booking {
    String flightNumber;
    String destination;
    // Override confirm() to include flight details
}
```

---

## HOUR 4-7: TESTING CONCEPTS

### Hour 4: Testing Fundamentals (60 min)

**Key Concepts to Memorize:**

| Concept | Definition | Example |
|---------|------------|---------|
| **Test Case** | Specific input + expected output | "Enter valid email, Accept" |
| **Test Scenario** | What to test | "Login functionality" |
| **Test Plan** | Strategy document | "How we will test the feature" |
| **Bug Life Cycle** | New > Open > Fix > Verify > Close | Developer fixes bug |
| **Severity** | Impact on system (High/Med/Low) | App crashes = High |
| **Priority** | Urgency to fix (P1/P2/P3) | Login broken = P1 |

**Types of Testing:**

| Type | What It Tests |
|------|---------------|
| **Functional** | Does it work? |
| **Non-Functional** | Performance, security, usability |
| **Regression** | Did new code break old features? |
| **Smoke** | Basic functionality works? |
| **Sanity** | Specific fix works? |
| **Black-box** | Test without seeing code |
| **White-box** | Test with code knowledge |

---

### Hour 5-6: Testing Practice (90 min)

**Scenario: Test a Login Page**

Write test cases for: `https://tiket.com/login`

**Positive Test Cases:**
1. Valid email + valid password = Login successful
2. Remember me checked = Session persists
3. Login with Google = Redirects to Google

**Negative Test Cases:**
4. Invalid email format = Error message
5. Empty email = "Email required"
6. Empty password = "Password required"
7. Wrong password = "Invalid credentials"
8. Account locked after 5 attempts

**Boundary Test Cases:**
9. Email with max length (254 chars)
10. Password with 1 char (minimum)
11. Password with 128 chars (maximum)
12. SQL injection attempt: `admin'--`

**UI/UX Test Cases:**
13. Tab key moves to next field
14. Enter key submits form
15. Error messages are visible
16. Password field masks input

---

### Hour 7: SQL Basics (60 min)

**Essential SQL for Testing:**

```sql
-- SELECT (Read data)
SELECT * FROM users WHERE email = 'test@example.com';

-- INSERT (Create data)
INSERT INTO users (name, email) VALUES ('Ivan', 'ivan@test.com');

-- UPDATE (Modify data)
UPDATE users SET status = 'active' WHERE id = 1;

-- DELETE (Remove data)
DELETE FROM users WHERE id = 1;

-- COUNT (Verify)
SELECT COUNT(*) FROM bookings WHERE status = 'pending';

-- JOIN (Relate tables)
SELECT u.name, b.booking_id 
FROM users u 
JOIN bookings b ON u.id = b.user_id;
```

**Why SQL for SDET?**
- Verify data was saved correctly
- Check database state after test
- Create test data
- Debug issues

---

## HOUR 7-10: PRACTICE PROBLEMS

### Hour 7-8: Java Coding Problems (90 min)

**Problem 1: Reverse a String**
```java
public class Reverse {
    public static String reverse(String str) {
        return new StringBuilder(str).reverse().toString();
    }
    
    public static void main(String[] args) {
        System.out.println(reverse("tiket")); // "tekit"
    }
}
```

**Problem 2: Find Largest Number**
```java
public class Largest {
    public static int findLargest(int[] numbers) {
        int max = numbers[0];
        for (int num : numbers) {
            if (num > max) {
                max = num;
            }
        }
        return max;
    }
    
    public static void main(String[] args) {
        int[] nums = {3, 7, 2, 9, 1};
        System.out.println("Largest: " + findLargest(nums)); // 9
    }
}
```

**Problem 3: Check Palindrome**
```java
public class Palindrome {
    public static boolean isPalindrome(String str) {
        String reversed = new StringBuilder(str).reverse().toString();
        return str.equalsIgnoreCase(reversed);
    }
    
    public static void main(String[] args) {
        System.out.println(isPalindrome("racecar")); // true
        System.out.println(isPalindrome("hello"));   // false
    }
}
```

**Problem 4: FizzBuzz (Classic!)**
```java
public class FizzBuzz {
    public static void main(String[] args) {
        for (int i = 1; i <= 100; i++) {
            if (i % 15 == 0) {
                System.out.println("FizzBuzz");
            } else if (i % 3 == 0) {
                System.out.println("Fizz");
            } else if (i % 5 == 0) {
                System.out.println("Buzz");
            } else {
                System.out.println(i);
            }
        }
    }
}
```

---

### Hour 9-10: SDET Scenario Questions (60 min)

**Scenario 1: How would you test flight search?**

Answer Structure:
1. **Functional:** Valid search, date range, passengers
2. **Boundary:** 0 passengers, max passengers, past dates
3. **Error:** Invalid airport, special characters
4. **Performance:** Response time, concurrent users
5. **UI:** Date picker, autocomplete, responsive

**Scenario 2: A bug is reported but you cannot reproduce it. What do you do?**

Steps:
1. Ask for exact steps to reproduce
2. Check environment differences (browser, OS, data)
3. Review logs and error messages
4. Try different test data
5. Check if bug is intermittent (timing issue)
6. Document findings and communicate with reporter

**Scenario 3: How do you decide what to automate?**

Automate if:
- Test is repeated frequently
- Test is data-driven
- Test is time-consuming manually
- Test is critical path

Do not automate if:
- Test is done once
- Test requires human judgment
- Test is exploratory
- ROI is low

---

## HOUR 10-12: MOCK TEST + REVIEW

### Hour 10-11: Practice Test (60 min)

**Questions to Practice:**

**Java Basics:**
1. What is the difference between `==` and `.equals()`?
2. What is a constructor?
3. What is the difference between `ArrayList` and `Array`?
4. What is `try-catch` and when do you use it?

**OOP:**
5. Explain encapsulation with an example
6. What is the difference between inheritance and composition?
7. What is method overriding?
8. Why use interfaces?

**Testing:**
9. What is the difference between verification and validation?
10. When would you use regression testing?
11. What is a test harness?
12. How do you prioritize test cases?

**SQL:**
13. Write a query to find duplicate emails
14. What is the difference between WHERE and HAVING?
15. Explain INNER JOIN vs LEFT JOIN

---

### Hour 12: Final Review (60 min)

**Quick Checklist:**

**Java:**
- [ ] Can write a class with constructor
- [ ] Can use if/else and loops
- [ ] Can create and use objects
- [ ] Understand inheritance basics

**OOP:**
- [ ] Can explain 4 pillars with examples
- [ ] Can write a simple inheritance hierarchy
- [ ] Understand encapsulation (private + getters/setters)

**Testing:**
- [ ] Can write test cases for a login page
- [ ] Know difference between severity and priority
- [ ] Can explain regression vs smoke testing

**SQL:**
- [ ] Can write SELECT with WHERE
- [ ] Can INSERT and UPDATE data
- [ ] Understand JOIN basics

---

## QUICK REFERENCE CARD

### Java Syntax Cheat Sheet
```java
// Class
class MyClass {
    // Field
    int x;
    
    // Constructor
    MyClass(int x) { this.x = x; }
    
    // Method
    void doSomething() { }
}

// Main method
public static void main(String[] args) { }

// Print
System.out.println("text");

// If/else
if (condition) { } else { }

// For loop
for (int i = 0; i < n; i++) { }

// Array
int[] arr = {1, 2, 3};
```

### Testing Terms Cheat Sheet
| Term | Meaning |
|------|---------|
| Test Case | Input + Expected Output |
| Positive Test | Valid input, expect success |
| Negative Test | Invalid input, expect error |
| Boundary Test | Edge values (min, max, empty) |
| Regression | Re-test after changes |
| Smoke Test | Quick sanity check |
| Severity | How bad is the bug |
| Priority | How urgent to fix |

### SQL Cheat Sheet
```sql
SELECT * FROM table WHERE condition;
INSERT INTO table (col) VALUES (val);
UPDATE table SET col = val WHERE condition;
DELETE FROM table WHERE condition;
SELECT COUNT(*) FROM table;
```

---

## POWER TIPS

1. **Do not memorize code** - Understand the logic
2. **Practice typing** - Speed matters in timed tests
3. **Read questions carefully** - Do not miss edge cases
4. **Start easy** - Solve simple problems first
5. **Time management** - Do not spend too long on one question
6. **Test your code** - Run it mentally before submitting
7. **Stay calm** - You have prepared, you have got this!

---

## GOOD LUCK!

You have 12 hours. Follow this plan hour by hour. 

**Remember:**
- Focus on understanding, not memorizing
- Practice typing code, not just reading
- Take short breaks every hour
- Stay hydrated and rested

**You have got this, Ivan!**

---

*Created for tiket.com SDET Intern Technical Test Preparation*
*Last updated: September 2, 2026*
