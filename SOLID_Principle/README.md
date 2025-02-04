### 🔹 **SOLID Principles in Java (or OOP)**  
The **SOLID** principles are **five** key design principles that help developers create more **scalable, maintainable, and flexible** software. These principles were introduced by **Robert C. Martin (Uncle Bob)**.

---

## ✅ **S – Single Responsibility Principle (SRP)**
💡 **A class should have only one reason to change.**  
**🔹 Problem:** If a class has **multiple responsibilities**, changes in one responsibility can break another.  
**🔹 Solution:** Separate concerns into **different classes**.

### **❌ Bad Example:**
```java
class Employee {
    public void calculateSalary() { /* Salary calculation logic */ }
    public void printPayslip() { /* Printing logic */ }
    public void saveToDatabase() { /* DB logic */ }
}
```
👉 This class violates SRP because it **handles multiple responsibilities**: **salary calculation, printing, and database operations**.

### **✅ Good Example:**
```java
class SalaryCalculator {
    public void calculateSalary(Employee emp) { /* Salary logic */ }
}

class PayslipPrinter {
    public void printPayslip(Employee emp) { /* Printing logic */ }
}

class EmployeeRepository {
    public void saveToDatabase(Employee emp) { /* DB logic */ }
}
```
✔ Now, each class has a **single responsibility**, making it easier to maintain.

---

## ✅ **O – Open/Closed Principle (OCP)**
💡 **A class should be open for extension but closed for modification.**  
**🔹 Problem:** If we modify existing code to add new features, we risk breaking functionality.  
**🔹 Solution:** Use **abstraction and polymorphism**.

### **❌ Bad Example (Violating OCP):**
```java
class PaymentProcessor {
    public void processPayment(String paymentType) {
        if (paymentType.equals("CreditCard")) {
            // Credit card processing logic
        } else if (paymentType.equals("PayPal")) {
            // PayPal processing logic
        }
    }
}
```
👉 Every time we add a **new payment method**, we **modify** the existing code, which can introduce bugs.

### **✅ Good Example (Following OCP):**
```java
interface PaymentMethod {
    void processPayment();
}

class CreditCardPayment implements PaymentMethod {
    public void processPayment() { /* Credit Card logic */ }
}

class PayPalPayment implements PaymentMethod {
    public void processPayment() { /* PayPal logic */ }
}

class PaymentProcessor {
    public void process(PaymentMethod method) {
        method.processPayment();
    }
}
```
✔ Now, if we add **new payment methods**, we just create **new classes** without modifying `PaymentProcessor`.

---

## ✅ **L – Liskov Substitution Principle (LSP)**
💡 **Subclasses should be substitutable for their base classes without altering program correctness.**  
**🔹 Problem:** If a subclass breaks functionality when used in place of a superclass, it violates LSP.  
**🔹 Solution:** Ensure **child classes behave like their parents**.

### **❌ Bad Example (Violating LSP):**
```java
class Bird {
    public void fly() { /* Flying logic */ }
}

class Penguin extends Bird {
    @Override
    public void fly() { throw new UnsupportedOperationException("Penguins can't fly!"); }
}
```
👉 A `Penguin` **is a** `Bird` but **cannot fly**. This **breaks** the expectations of the base class.

### **✅ Good Example (Following LSP):**
```java
abstract class Bird {
    // Common bird behaviors
}

class FlyingBird extends Bird {
    public void fly() { /* Flying logic */ }
}

class Sparrow extends FlyingBird { }

class Penguin extends Bird { /* Penguins don't fly */ }
```
✔ Now, **only flying birds have `fly()`**, and `Penguin` doesn't break the behavior.

---

## ✅ **I – Interface Segregation Principle (ISP)**
💡 **A class should not be forced to implement interfaces it does not use.**  
**🔹 Problem:** Large interfaces with too many methods force subclasses to implement unnecessary methods.  
**🔹 Solution:** Split large interfaces into **smaller, more specific interfaces**.

### **❌ Bad Example (Violating ISP):**
```java
interface Worker {
    void work();
    void eat();
}

class Robot implements Worker {
    public void work() { /* Working logic */ }
    public void eat() { throw new UnsupportedOperationException("Robots don't eat!"); }
}
```
👉 **Robots don’t eat**, but they **must** implement `eat()` due to the interface.

### **✅ Good Example (Following ISP):**
```java
interface Workable {
    void work();
}

interface Eatable {
    void eat();
}

class HumanWorker implements Workable, Eatable {
    public void work() { /* Working logic */ }
    public void eat() { /* Eating logic */ }
}

class RobotWorker implements Workable {
    public void work() { /* Working logic */ }
}
```
✔ Now, `RobotWorker` only implements the `Workable` interface, avoiding unnecessary methods.

---

## ✅ **D – Dependency Inversion Principle (DIP)**
💡 **High-level modules should not depend on low-level modules. Both should depend on abstractions.**  
**🔹 Problem:** If a high-level module depends directly on a low-level module, changing the low-level module requires modifying the high-level module.  
**🔹 Solution:** Use **interfaces** and **dependency injection**.

### **❌ Bad Example (Violating DIP):**
```java
class MySQLDatabase {
    public void connect() { /* MySQL Connection Logic */ }
}

class UserRepository {
    private MySQLDatabase db = new MySQLDatabase();

    public void saveUser() {
        db.connect();
        // Save user logic
    }
}
```
👉 `UserRepository` is **tightly coupled** to `MySQLDatabase`, making it **hard to switch** databases.

### **✅ Good Example (Following DIP):**
```java
interface Database {
    void connect();
}

class MySQLDatabase implements Database {
    public void connect() { /* MySQL Logic */ }
}

class PostgreSQLDatabase implements Database {
    public void connect() { /* PostgreSQL Logic */ }
}

class UserRepository {
    private final Database db;

    public UserRepository(Database db) {
        this.db = db;
    }

    public void saveUser() {
        db.connect();
        // Save user logic
    }
}
```
✔ Now, `UserRepository` depends on the **Database interface**, making it easy to switch to **PostgreSQL** or other databases.

---

## 🎯 **Summary of SOLID Principles**
| Principle | Meaning | Benefits |
|-----------|---------|----------|
| **S** - Single Responsibility | One class = One reason to change | Easier maintenance |
| **O** - Open/Closed | Open for extension, closed for modification | Flexible & scalable |
| **L** - Liskov Substitution | Subtypes should be replaceable | Avoids unexpected behavior |
| **I** - Interface Segregation | No unnecessary method implementations | Clean & modular code |
| **D** - Dependency Inversion | Depend on abstractions, not concretions | Loosely coupled code |

---

## 🚀 **Final Thoughts**
Following **SOLID principles** helps you write **clean, scalable, and maintainable** code. It encourages **loose coupling** and **high cohesion**, making your software easier to test, modify, and extend.  

Would you like me to provide a **real-world example** of SOLID in Spring Boot? 😊
