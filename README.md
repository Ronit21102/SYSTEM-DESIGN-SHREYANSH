# SYSTEM-DESIGN-SHREYANSH

SOLID
## **Why Do We Need the Single Responsibility Principle (SRP)?**  

The **Single Responsibility Principle (SRP)** is crucial for **maintainability, scalability, and flexibility** in software development. Below are key **reasons** why SRP is needed, along with real-world scenarios.

---

### **🔹 1. Easier Maintenance**  
**Problem without SRP:**  
If a class has multiple responsibilities (e.g., user management + email sending), modifying one feature can unintentionally break another.  

**Scenario:**  
Imagine you need to **change the email notification system** from **SMTP to a third-party API**. If `User` also handles database operations, a change in email logic might introduce **bugs in database management**.

✅ **With SRP**: The `EmailService` class handles emails separately, so changes to email logic **don’t affect user storage**.

---

### **🔹 2. Better Code Reusability**  
**Problem without SRP:**  
If a class does **too many things**, it cannot be reused in different parts of the application.  

**Scenario:**  
Suppose you have a `User` class that **saves users** and **sends notifications**. Later, another part of your app also needs to send notifications for **orders**.  

✅ **With SRP**:  
- We separate `UserRepository` and `EmailService`, so the notification logic (`EmailService`) can be **reused** for orders **without modifying the User class**.

---

### **🔹 3. Easier Debugging and Testing**  
**Problem without SRP:**  
When a class has **multiple responsibilities**, testing and debugging become complex.  

**Scenario:**  
A `User` class that handles **data storage** and **email sending** makes it **hard to write unit tests** because:  
- You need a **database** to test saving users.  
- You need an **email server** to test notifications.  

✅ **With SRP**:  
- `UserRepository` can be tested independently **without an email server**.  
- `EmailService` can be tested separately using **mock emails**.

---

### **🔹 4. Allows Parallel Development**  
**Problem without SRP:**  
If a class does **too much**, multiple developers working on different features **may conflict with each other**.  

**Scenario:**  
In a team:  
- **Developer A** works on user authentication.  
- **Developer B** works on email notifications.  

If both developers modify the `User` class, **merge conflicts and errors** may occur.  

✅ **With SRP**:  
- `UserRepository` and `EmailService` are separate, so **both developers can work independently** without conflicts.

---

### **🔹 5. Supports Open-Closed Principle (OCP)**  
**Problem without SRP:**  
Modifying existing code for **new features** increases the risk of introducing **bugs**.  

**Scenario:**  
Suppose you want to **add SMS notifications** alongside emails.  
- If email sending is inside the `User` class, you must **modify it**, which risks breaking user management.  

✅ **With SRP**:  
- You can **extend** `NotificationService` to include `SMSService` without changing `UserRepository`.  

---

### **📌 Key Takeaways**
| Without SRP ❌ | With SRP ✅ |
|--------------|-----------|
| Hard to maintain 🛠️ | Easy to modify 🔄 |
| Difficult to debug 🐞 | Easier to test ✅ |
| Low reusability ❌ | Highly reusable ♻️ |
| Code conflicts 🚧 | Parallel development 🚀 |
| Modifications introduce bugs 🐛 | Open for extension 🔧 |

---

### **💡 Final Thought**
Following SRP **reduces complexity, improves modularity, and makes code easier to manage**. This principle is especially important in **large-scale applications** where multiple developers work on the same codebase.

### **Single Responsibility Principle (SRP)**
The **Single Responsibility Principle (SRP)** states that **a class or function should have only one reason to change**. In other words, a class should have **only one responsibility**.

---

## **🟢 JavaScript Example**
### ✅ **Before (Violation of SRP)**
Here, the `User` class handles both **user data management** and **email notifications**, which are two different responsibilities.
```js
class User {
  constructor(name, email) {
    this.name = name;
    this.email = email;
  }

  saveToDatabase() {
    console.log(`Saving ${this.name} to the database`);
  }

  sendEmailNotification() {
    console.log(`Sending email to ${this.email}`);
  }
}

// Usage
const user = new User("John Doe", "john@example.com");
user.saveToDatabase();
user.sendEmailNotification();
```
❌ **Problem**:  
- The `User` class has **two responsibilities**:
  - **Managing user data**
  - **Sending email notifications**  
- If we need to change the email system, we must modify the `User` class, violating SRP.

---

### ✅ **After (SRP Applied)**
Now, we separate concerns into **UserRepository** (handles data storage) and **EmailService** (handles notifications).
```js
class User {
  constructor(name, email) {
    this.name = name;
    this.email = email;
  }
}

class UserRepository {
  save(user) {
    console.log(`Saving ${user.name} to the database`);
  }
}

class EmailService {
  sendEmail(user) {
    console.log(`Sending email to ${user.email}`);
  }
}

// Usage
const user = new User("John Doe", "john@example.com");
const userRepository = new UserRepository();
const emailService = new EmailService();

userRepository.save(user);
emailService.sendEmail(user);
```
✅ **Now, if we need to change the email system, we modify only `EmailService`, without affecting `UserRepository`.** 🎯

---

## **🟢 Java Example**
### ✅ **Before (Violation of SRP)**
```java
class User {
    private String name;
    private String email;

    public User(String name, String email) {
        this.name = name;
        this.email = email;
    }

    public void saveToDatabase() {
        System.out.println("Saving " + name + " to database");
    }

    public void sendEmailNotification() {
        System.out.println("Sending email to " + email);
    }
}

// Usage
public class Main {
    public static void main(String[] args) {
        User user = new User("John Doe", "john@example.com");
        user.saveToDatabase();
        user.sendEmailNotification();
    }
}
```
❌ **Problem**:  
- The `User` class is responsible for both **database operations** and **email sending**.  

---

### ✅ **After (SRP Applied)**
```java
class User {
    private String name;
    private String email;

    public User(String name, String email) {
        this.name = name;
        this.email = email;
    }

    public String getName() {
        return name;
    }

    public String getEmail() {
        return email;
    }
}

class UserRepository {
    public void save(User user) {
        System.out.println("Saving " + user.getName() + " to database");
    }
}

class EmailService {
    public void sendEmail(User user) {
        System.out.println("Sending email to " + user.getEmail());
    }
}

// Usage
public class Main {
    public static void main(String[] args) {
        User user = new User("John Doe", "john@example.com");
        UserRepository userRepository = new UserRepository();
        EmailService emailService = new EmailService();

        userRepository.save(user);
        emailService.sendEmail(user);
    }
}
```
✅ **Now, `UserRepository` handles storage, and `EmailService` handles emails. Each class has a single responsibility.** 🚀

---

## **🔹 Key Takeaways**
- **Bad Design** ❌: One class handling multiple responsibilities.
- **Good Design** ✅: Each class has **one reason to change**.
- **Benefits**:
  - Easier maintenance 🛠️
  - Less coupling 🔄
  - More reusable components ♻️

The **Open-Closed Principle (OCP)** states that **a class should be open for extension but closed for modification**. This means we should be able to add new functionality without modifying existing code.  

Below are examples in **JavaScript** and **Java** demonstrating this principle.

---

## **🟢 JavaScript Example (Using Strategy Pattern)**
Let's assume we have a `Discount` class. Instead of modifying the class every time a new discount type is added, we use **polymorphism**.

### ✅ **Before (Violation of OCP)**
Here, we modify the existing class every time a new discount type is added:
```js
class Discount {
  getDiscount(type, price) {
    if (type === "regular") {
      return price * 0.9;  // 10% discount
    } else if (type === "premium") {
      return price * 0.8;  // 20% discount
    }
    return price;
  }
}
```
❌ **Problem**: Every time a new discount type is needed, we have to modify the `getDiscount` method, violating OCP.

---

### ✅ **After (OCP Applied using Strategy Pattern)**
Instead of modifying the `Discount` class, we create separate discount strategies and extend them without changing the core logic.

```js
class DiscountStrategy {
  apply(price) {
    return price;  // Default: no discount
  }
}

class RegularDiscount extends DiscountStrategy {
  apply(price) {
    return price * 0.9; // 10% discount
  }
}

class PremiumDiscount extends DiscountStrategy {
  apply(price) {
    return price * 0.8; // 20% discount
  }
}

// Usage
function applyDiscount(strategy, price) {
  return strategy.apply(price);
}

console.log(applyDiscount(new RegularDiscount(), 100)); // 90
console.log(applyDiscount(new PremiumDiscount(), 100)); // 80
```
✅ **Now, if we want to add a new discount type, we just create a new class without modifying existing code.** 🚀

---

## **🟢 Java Example (Using Polymorphism)**
### ✅ **Before (Violation of OCP)**
```java
class Discount {
    public double applyDiscount(String type, double price) {
        if (type.equals("regular")) {
            return price * 0.9; // 10% discount
        } else if (type.equals("premium")) {
            return price * 0.8; // 20% discount
        }
        return price;
    }
}
```
❌ **Problem**: If we want to add a new discount type, we must modify this class, violating OCP.

---

### ✅ **After (OCP Applied using Interface and Polymorphism)**
```java
interface DiscountStrategy {
    double applyDiscount(double price);
}

class RegularDiscount implements DiscountStrategy {
    public double applyDiscount(double price) {
        return price * 0.9; // 10% discount
    }
}

class PremiumDiscount implements DiscountStrategy {
    public double applyDiscount(double price) {
        return price * 0.8; // 20% discount
    }
}

class DiscountService {
    public double apply(DiscountStrategy strategy, double price) {
        return strategy.applyDiscount(price);
    }
}

public class Main {
    public static void main(String[] args) {
        DiscountService service = new DiscountService();
        
        System.out.println(service.apply(new RegularDiscount(), 100)); // 90.0
        System.out.println(service.apply(new PremiumDiscount(), 100)); // 80.0
    }
}
```
✅ **Now, if we want to add a new discount type, we just implement the `DiscountStrategy` interface in a new class!** 🎯

---

## **🔹 Key Takeaways**
- ✅ **Avoid modifying existing classes when adding new features**  
- ✅ **Use polymorphism (Strategy Pattern) to extend behavior**  
- ✅ **Encapsulate variations into separate classes**  

Both the JavaScript and Java examples follow **OCP** by allowing new functionality (discount types) to be added **without modifying existing code**. 🚀

Yes! **Liskov Substitution Principle (LSP)** basically means:  

> **"Don't add methods in a parent class that cannot be properly inherited by all child classes."**  

If a child class **can't meaningfully use or override** a method from the parent class **without breaking expected behavior**, then **that method should not exist in the parent class.**  

---

### **🔹 Why is This Important?**
If we violate LSP:  
✅ The child class **inherits something it can't support**, leading to unexpected behavior.  
✅ Functions that work with the parent class **might break when given a child class**.  
✅ We end up with **lots of `if` checks** to fix behavior in subclasses.  

---

## **✅ Example of Good vs. Bad LSP Design**  

### **❌ Bad Example (LSP Violation)**
Here, we put a `fly()` method in the `Bird` class, **but not all birds can fly.**  

```js
class Bird {
  fly() {
    console.log("I am flying!");
  }
}

class Sparrow extends Bird {}

class Penguin extends Bird {
  fly() {
    throw new Error("I cannot fly!"); // ❌ LSP Violation
  }
}

function makeBirdFly(bird) {
  bird.fly();  // ❌ This will break for Penguins
}

// Usage
const sparrow = new Sparrow();
makeBirdFly(sparrow);  // ✅ Works fine

const penguin = new Penguin();
makeBirdFly(penguin);  // ❌ Throws an error
```
### **❌ What's Wrong?**
- **Penguin inherits `fly()` but cannot actually fly!**
- The `fly()` method **shouldn't have been in the parent class (Bird) in the first place.**  

---

### **✅ Correct Design (LSP Applied)**
Instead of putting `fly()` in `Bird`, we **separate flying birds from non-flying birds.**  

```js
class Bird {
  eat() {
    console.log("I can eat!");
  }
}

class FlyingBird extends Bird {
  fly() {
    console.log("I am flying!");
  }
}

class Sparrow extends FlyingBird {}

class Penguin extends Bird {
  swim() {
    console.log("I am swimming!");
  }
}

// Function that only takes flying birds
function makeBirdFly(bird) {
  bird.fly();
}

// Usage
const sparrow = new Sparrow();
makeBirdFly(sparrow);  // ✅ Works fine

const penguin = new Penguin();
// makeBirdFly(penguin);  // ❌ Won't compile, prevents errors!
```
### **✅ Why is This Correct?**
- **Flying birds have a separate class (`FlyingBird`)** instead of forcing all birds to fly.  
- **Penguins never inherit `fly()`**, so no errors happen.  
- The program **never needs `if` checks or special conditions.**  

---

## **📌 Summary**
- **Don’t put methods in the parent class if not all child classes can inherit them properly.**  
- Instead, **use separate classes or interfaces** to keep behavior consistent.  
- **Avoid forcing child classes to override methods with "empty" or "throw error" implementations.**  

This makes the code **more predictable, maintainable, and scalable**. 🚀

### **🔹 Interface Segregation Principle (ISP)**
The **Interface Segregation Principle (ISP)** states:  

> **"A class should not be forced to implement interfaces it does not use."**  

In other words, **don’t make large, bloated interfaces** with unrelated methods. Instead, **split them into smaller, more specific interfaces** so that implementing classes only need to use what they actually need.

---

## **❌ Bad Example (ISP Violation)**  
Here, `Worker` is a **fat interface** that forces **all workers** to implement `eat()` and `work()`.  

```java
interface Worker {
    void work();
    void eat();
}

class Developer implements Worker {
    public void work() {
        System.out.println("Coding...");
    }

    public void eat() { 
        System.out.println("Eating lunch...");
    }
}

class Robot implements Worker {
    public void work() {
        System.out.println("Performing tasks...");
    }

    public void eat() { 
        throw new UnsupportedOperationException("Robots don’t eat!"); // ❌ ISP Violation
    }
}
```
### **❌ What's Wrong?**
- `Robot` is **forced to implement `eat()`**, even though **it doesn’t need it.**
- The interface has **unrelated responsibilities** (`work()` vs. `eat()`).
- The solution should be **more flexible** so that `Robot` doesn’t need `eat()`.

---

## **✅ Good Example (ISP Applied)**  
Instead of one big interface, **split it into two smaller interfaces**.

```java
interface Workable {
    void work();
}

interface Eatable {
    void eat();
}

class Developer implements Workable, Eatable {
    public void work() {
        System.out.println("Coding...");
    }

    public void eat() { 
        System.out.println("Eating lunch...");
    }
}

class Robot implements Workable {
    public void work() {
        System.out.println("Performing tasks...");
    }
}
```
### **✅ Why is This Correct?**
✔️ **Now `Robot` only implements `Workable`**, so it’s **not forced to have `eat()`**.  
✔️ `Developer` implements both `Workable` and `Eatable` because **humans need both work and food**.  
✔️ **No unnecessary methods, no `throw new Exception()` hacks.**  

---

## **✅ Example in JavaScript**
Here’s a **bad** example where all payment methods must implement `scanQRCode()` (even though cards don’t need it):

```js
class PaymentMethod {
  pay() {}
  scanQRCode() {} // ❌ Not all payments need QR scanning
}

class CreditCard extends PaymentMethod {
  pay() {
    console.log("Paying with Credit Card");
  }

  scanQRCode() {
    throw new Error("Credit cards don't use QR codes!"); // ❌ ISP Violation
  }
}

class UPI extends PaymentMethod {
  pay() {
    console.log("Paying with UPI");
  }

  scanQRCode() {
    console.log("Scanning QR Code...");
  }
}
```
### **✅ Fix: Use Separate Interfaces**
```js
class Payment {
  pay() {}
}

class QRCodeScanner {
  scanQRCode() {}
}

class CreditCard extends Payment {
  pay() {
    console.log("Paying with Credit Card");
  }
}

class UPI extends Payment {
  pay() {
    console.log("Paying with UPI");
  }
}

class QRCodePayment extends Payment {
  pay() {
    console.log("Paying with QR Code");
  }
}

// If a payment method needs QR scanning, it can use this
Object.assign(QRCodePayment.prototype, new QRCodeScanner());

// Now QRCodePayment has scanQRCode
const qrPayment = new QRCodePayment();
qrPayment.scanQRCode(); // ✅ Works only where needed
```
---
## **📌 Summary**
- **Don't force a class to implement methods it doesn’t use.**
- **Break large interfaces into smaller, more specific ones.**
- **Each class should only depend on the methods it actually needs.**
- Leads to **better flexibility, easier maintenance, and fewer bugs.** 🚀

## **🔹 Dependency Inversion Principle (DIP)**  
The **Dependency Inversion Principle (DIP)** states:  

> **"High-level modules should not depend on low-level modules. Both should depend on abstractions."**  
> **"Abstractions should not depend on details. Details should depend on abstractions."**  

### **🛠 Why Do We Need This?**
- Without DIP, high-level code directly depends on low-level implementations, making it hard to modify or replace dependencies.
- Following DIP makes code **more flexible, maintainable, and testable** by allowing us to change dependencies without breaking the entire system.

---

## **❌ Bad Example (DIP Violation)**
In this example, `OrderService` directly depends on `MySQLDatabase`.  
If we want to change the database (e.g., to MongoDB), we have to modify `OrderService`, which violates DIP.

### **Bad Java Example:**
```java
class MySQLDatabase {
    void saveOrder() {
        System.out.println("Saving order in MySQL");
    }
}

class OrderService {
    private MySQLDatabase database; // ❌ Direct dependency on MySQL

    public OrderService() {
        this.database = new MySQLDatabase();
    }

    void placeOrder() {
        database.saveOrder();
    }
}
```
### **🚨 Problems:**
- `OrderService` is tightly coupled to `MySQLDatabase`.
- If we want to switch databases, we **must modify `OrderService`**.
- This **breaks Open-Closed Principle (OCP)** too, since we need to modify existing code to support new databases.

---

## **✅ Good Example (DIP Applied)**
Instead of depending on a specific database class, we **depend on an abstraction (interface)**.  
Now, `OrderService` works with **any database** that follows the `Database` interface.

### **Correct Java Example:**
```java
// 🔹 High-level module depends on an abstraction
interface Database {
    void saveOrder();
}

// 🔹 Low-level modules implement the abstraction
class MySQLDatabase implements Database {
    public void saveOrder() {
        System.out.println("Saving order in MySQL");
    }
}

class MongoDB implements Database {
    public void saveOrder() {
        System.out.println("Saving order in MongoDB");
    }
}

// 🔹 OrderService now depends on Database abstraction, not a specific class
class OrderService {
    private Database database;

    public OrderService(Database database) { // Inject dependency
        this.database = database;
    }

    void placeOrder() {
        database.saveOrder();
    }
}

// Usage
public class Main {
    public static void main(String[] args) {
        Database db = new MySQLDatabase(); // We can switch to MongoDB easily
        OrderService orderService = new OrderService(db);
        orderService.placeOrder();
    }
}
```
### **✅ Why is This Correct?**
✔️ `OrderService` **does not depend on a specific database implementation**.  
✔️ We can **switch databases (MySQL, MongoDB) without modifying `OrderService`**.  
✔️ **Follows Open-Closed Principle (OCP)** – new databases can be added without modifying existing code.  
✔️ **Easier to test** – we can inject a mock database for unit testing.

---

## **✅ JavaScript Example**
This example follows **Dependency Inversion Principle** using dependency injection.

```js
// Abstraction (Interface)
class Database {
  saveOrder() {
    throw new Error("Method not implemented");
  }
}

// Low-level modules (implementations)
class MySQLDatabase extends Database {
  saveOrder() {
    console.log("Saving order in MySQL");
  }
}

class MongoDB extends Database {
  saveOrder() {
    console.log("Saving order in MongoDB");
  }
}

// High-level module (depends on abstraction, not details)
class OrderService {
  constructor(database) {
    this.database = database; // Dependency Injection
  }

  placeOrder() {
    this.database.saveOrder();
  }
}

// Usage
const db = new MySQLDatabase(); // Can switch to MongoDB
const orderService = new OrderService(db);
orderService.placeOrder();
```

### **✅ Why is This Correct?**
- `OrderService` **depends on an abstraction (Database), not a specific implementation**.
- We can **swap MySQL for MongoDB without changing `OrderService`**.
- **Better testability** – we can inject a mock database for testing.

---

## **📌 Summary**
| ✅ **Good Practice**  | ❌ **Bad Practice**  |
|-----------------|------------------|
| **High-level modules depend on abstractions (interfaces), not concrete implementations.** | High-level modules directly depend on low-level modules. |
| **Low-level modules implement these abstractions.** | Low-level details are tightly coupled with high-level logic. |
| **Flexible, easy to extend and test.** | Hard to change, requires modifying existing code. |

---

## **📢 Key Takeaways**
🔹 **Dependency Inversion Principle (DIP) ensures flexible and maintainable code**.  
🔹 Instead of depending on concrete implementations, **depend on abstractions (interfaces)**.  
🔹 **Easier to replace components (e.g., switching databases, payment gateways, logging systems, etc.)**.  
🔹 Works well with **Dependency Injection (DI) and Inversion of Control (IoC)**.  

###Strategy design pattern
The **Strategy Design Pattern** is used when we need to define a **family of algorithms (or behaviors)**, encapsulate them, and make them interchangeable at runtime. It allows objects to **choose the appropriate behavior dynamically** without modifying their code.

---

### **✅ How Does Strategy Pattern Fit Here?**
If **two child classes need the same property or behavior** but the **parent class does not have it**, you can use the **Strategy Pattern** by **encapsulating the behavior in a separate class** and injecting it dynamically.

---

### **✅ Strategy Pattern Example for This Scenario (Java)**
Let’s say `Electronics` and `Furniture` both need a **discount behavior**, but the `Product` class doesn’t have it.

#### **🔹 Step 1: Create a Strategy Interface**
```java
interface DiscountStrategy {
    double applyDiscount(double price);
}
```

---

#### **🔹 Step 2: Implement Different Strategies**
```java
class ElectronicsDiscount implements DiscountStrategy {
    public double applyDiscount(double price) {
        return price * 0.1; // 10% discount
    }
}

class FurnitureDiscount implements DiscountStrategy {
    public double applyDiscount(double price) {
        return price * 0.2; // 20% discount
    }
}
```

---

#### **🔹 Step 3: Use Strategy in the Context (Product)**
```java
class Product {
    String name;
    double price;
    private DiscountStrategy discountStrategy; // Strategy Pattern

    public Product(String name, double price, DiscountStrategy discountStrategy) {
        this.name = name;
        this.price = price;
        this.discountStrategy = discountStrategy;
    }

    public double getFinalPrice() {
        return price - discountStrategy.applyDiscount(price);
    }
}
```

---

#### **🔹 Step 4: Use Strategy Pattern in Main Method**
```java
public class StrategyPatternExample {
    public static void main(String[] args) {
        Product laptop = new Product("Laptop", 1000, new ElectronicsDiscount());
        Product sofa = new Product("Sofa", 2000, new FurnitureDiscount());

        System.out.println("Laptop Final Price: $" + laptop.getFinalPrice());
        System.out.println("Sofa Final Price: $" + sofa.getFinalPrice());
    }
}
```

**🛠 Output:**  
```
Laptop Final Price: $900.0  
Sofa Final Price: $1600.0  
```

---

### **✅ Strategy Pattern in JavaScript**
```js
// Step 1: Define strategies
const electronicsDiscount = (price) => price * 0.1;
const furnitureDiscount = (price) => price * 0.2;

// Step 2: Product class using Strategy
class Product {
  constructor(name, price, discountStrategy) {
    this.name = name;
    this.price = price;
    this.discountStrategy = discountStrategy;
  }

  getFinalPrice() {
    return this.price - this.discountStrategy(this.price);
  }
}

// Step 3: Using Strategy Pattern
const laptop = new Product("Laptop", 1000, electronicsDiscount);
const sofa = new Product("Sofa", 2000, furnitureDiscount);

console.log("Laptop Final Price:", laptop.getFinalPrice()); // 900
console.log("Sofa Final Price:", sofa.getFinalPrice()); // 1600
```

---

### **✔ Why Use Strategy Pattern?**
| **Scenario** | **Without Strategy** | **With Strategy** |
|-------------|-----------------|----------------|
| Adding new behavior | Modify the base class | Just add a new strategy |
| Code structure | Lots of `if-else` in `Product` class | Clean, reusable, and scalable |
| Flexibility | Hard to extend new discounts | Easy to swap and add behaviors |

Would you like an **example with dynamic strategy switching**? 🚀
