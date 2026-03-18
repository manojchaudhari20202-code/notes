## JVM Internals
## Memory Management
## OOP Fundamentals
## Language
## Data Types & Variables
## Operators & Expressions
## Control Flow
## Exception
## Reflection
## Annotations
## Collections
## Networking
## Multithreading
## Concurrency
## Instrumentation
## Management
## Observability


















## Object Relationships — Inheritance, Association, Aggregation, Composition Explained with Java Examples

This guide explains each concept in simple terms, backed by real-world Java code examples. We'll cover inheritance, association, aggregation, composition, and the best practices around them.

---

### 1. Inheritance

#### What is inheritance? What does `extends` give you?
Inheritance is a mechanism where a class (child/subclass) acquires properties and behavior from another class (parent/superclass). The `extends` keyword gives you code reuse and establishes an `is-a` relationship.

**Example:**
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

// Usage
Dog dog = new Dog();
dog.eat();  // inherited from Animal
dog.bark(); // defined in Dog
```

#### What is the root of the Java class hierarchy (`java.lang.Object`)?
`java.lang.Object` is the superclass of all classes in Java. Every class directly or indirectly inherits from it, providing basic methods like `toString()`, `equals()`, `hashCode()`, etc.

#### What is single inheritance? Why does Java restrict classes to single inheritance?
Single inheritance means a class can extend only one direct superclass. Java restricts this to avoid the **diamond problem** (ambiguity when two superclasses have the same method) and to keep the language simple and less error-prone.

#### What is inherited by a subclass: fields, methods, nested classes — but NOT constructors?
- **Fields** are inherited (but private fields are not directly accessible).
- **Methods** are inherited (and can be overridden).
- **Nested classes** are inherited.
- **Constructors** are **not** inherited. However, a subclass constructor can invoke a superclass constructor using `super()`.

#### What is the difference between `is-a` and `has-a` relationships?
- **`is-a`** (inheritance): A `Dog` **is an** `Animal`.
- **`has-a`** (composition/aggregation): A `Car` **has an** `Engine`.

#### When should you use inheritance vs composition?
- **Inheritance** when you have a genuine, stable `is-a` relationship and you want to reuse code **and** benefit from polymorphism.
- **Composition** when you want to reuse functionality without tying yourself to a specific class hierarchy, or when the relationship is more `has-a` or `uses-a`. Composition is more flexible and less coupled.

#### What is the fragile base class problem?
Modifying a base class can unintentionally break subclasses because they depend on the internal details of the base class. For example, if you add a new method to the base class that collides with a subclass method, or change the implementation that subclasses rely on.

#### What is the Liskov Substitution Principle (LSP)? Give an example of violating it.
LSP states that objects of a superclass should be replaceable with objects of a subclass without affecting the correctness of the program. Violation example: a `Square` class extending `Rectangle` – changing width of a square also changes height, breaking the expected behavior of a rectangle.

```java
class Rectangle {
    private int width, height;
    public void setWidth(int w) { width = w; }
    public void setHeight(int h) { height = h; }
    public int getArea() { return width * height; }
}

class Square extends Rectangle {
    public void setWidth(int w) {
        super.setWidth(w);
        super.setHeight(w); // violates LSP: setting width also changes height
    }
    public void setHeight(int h) {
        super.setWidth(h);
        super.setHeight(h);
    }
}
// Client expecting Rectangle behavior breaks with Square.
```

#### What does `super` refer to? How do you call a superclass method or constructor?
- `super` is a reference to the immediate parent class object.
- To call a superclass method: `super.methodName()`.
- To call a superclass constructor: `super(parameters)` – must be the first statement in the subclass constructor.

#### What are the rules for `super()` constructor call (must be first statement)?
- `super()` must be the first statement in a constructor.
- If you don't call `super()` explicitly, the compiler inserts a no-argument `super()` automatically.

#### What is constructor chaining? What happens if you don't call `super()` explicitly?
Constructor chaining is the process where one constructor calls another constructor (of the same class or superclass) via `this()` or `super()`. If you don't call `super()` explicitly, the compiler inserts `super()` (the no-arg constructor of the parent). If the parent lacks a no-arg constructor, you must explicitly call a parameterized `super(...)`.

#### What is `final` class? Why is `String` final?
A `final` class cannot be subclassed. `String` is final to ensure immutability, security, and performance (e.g., string interning, hash code caching). Allowing subclasses could break immutability or cause unexpected behavior.

#### What is `final` method? Can it be overridden?
A `final` method cannot be overridden by subclasses. It's used to prevent modification of critical behavior.

#### Can a subclass access `private` fields of its superclass? How?
No, private fields are not directly accessible. However, they can be accessed indirectly through public or protected getter/setter methods provided by the superclass.

#### What is the difference between method hiding (static) and method overriding (instance)?
- **Method hiding**: When a subclass defines a static method with the same signature as a static method in the superclass. The method called depends on the reference type (compile-time).
- **Method overriding**: When a subclass defines an instance method with the same signature as a superclass instance method. The method called depends on the object type (runtime polymorphism).

```java
class Parent {
    static void staticMethod() { System.out.println("Parent static"); }
    void instanceMethod() { System.out.println("Parent instance"); }
}
class Child extends Parent {
    static void staticMethod() { System.out.println("Child static"); } // hides
    @Override
    void instanceMethod() { System.out.println("Child instance"); } // overrides
}
// Usage:
Parent p = new Child();
p.staticMethod();   // prints "Parent static" (hiding, based on reference type)
p.instanceMethod(); // prints "Child instance" (overriding, based on object type)
```

#### What is the difference between `extends` for classes and `extends` for interfaces?
- `extends` for classes: single inheritance; a class can extend only one class.
- `extends` for interfaces: multiple inheritance of type; an interface can extend multiple interfaces (e.g., `interface C extends A, B`).

#### What is multi-level inheritance? What are the risks?
Multi-level inheritance is when a class extends a class that extends another class (e.g., `C extends B extends A`). Risks: deep hierarchies become hard to maintain, fragile base class problem increases, and the code becomes tightly coupled.

#### Can a class inherit from multiple classes? What is the diamond problem?
Java does not allow a class to inherit from multiple classes (no multiple inheritance of implementation). The diamond problem arises if two superclasses have methods with the same signature – the compiler wouldn't know which one to use.

#### How does Java solve the diamond problem using interfaces?
Java allows multiple inheritance of type (interfaces). Since Java 8, interfaces can have default methods. If a class implements two interfaces with conflicting default methods, the class must override the method to resolve the conflict. This avoids ambiguity.

```java
interface A {
    default void foo() { System.out.println("A"); }
}
interface B {
    default void foo() { System.out.println("B"); }
}
class C implements A, B {
    @Override
    public void foo() { // must override to resolve conflict
        A.super.foo(); // or B.super.foo() or custom
    }
}
```

---

### 2. Association

#### What is association? What does it mean for two classes to be associated?
Association represents a relationship between two classes where objects of one class know about objects of another. It's a "uses-a" or "has-a" relationship in a broad sense.

#### What is the difference between association, aggregation, and composition?
- **Association**: A general relationship (e.g., a `Teacher` and a `Student` can be associated without ownership).
- **Aggregation**: A special form of association where the part can exist independently of the whole (e.g., `Department` and `Employee`).
- **Composition**: A stronger form where the part cannot exist without the whole (e.g., `House` and `Room`).

#### What is unidirectional vs bidirectional association?
- **Unidirectional**: One class knows about the other, but not vice versa (e.g., `Order` knows `Customer`, but `Customer` does not have a reference to `Order`).
- **Bidirectional**: Both classes know about each other (e.g., `Order` has a reference to `Customer`, and `Customer` has a list of `Order`). Requires careful maintenance to avoid inconsistency.

#### How is association represented in code (one class holds a reference to another)?
By having a field of one class type in another class.

**Example (unidirectional):**
```java
class Customer {
    String name;
    // Customer does not have Order reference
}

class Order {
    int orderId;
    Customer customer; // association: Order knows Customer
}
```

#### What is multiplicity in association (one-to-one, one-to-many, many-to-many)?
Multiplicity defines how many instances are involved.
- **One-to-One**: e.g., `Person` has one `Passport`.
- **One-to-Many**: e.g., `Department` has many `Employee`s.
- **Many-to-Many**: e.g., `Student` enrolls in many `Course`s, and a `Course` has many `Student`s.

**Example of one-to-many:**
```java
class Department {
    List<Employee> employees;
}

class Employee {
    // optionally back-reference to Department for bidirectional
}
```

#### What is a dependency (uses-a) vs association (has-a)?
- **Dependency (`uses-a`)**: A class uses another class temporarily, often as a method parameter or local variable. It's a weaker relationship.
- **Association (`has-a`)**: A longer-term relationship where one class holds a reference to another as a field.

**Dependency example:**
```java
class Driver {
    void drive(Car car) { // uses Car temporarily
        car.start();
    }
}
```

---

### 3. Aggregation

#### What is aggregation? What is the "whole-part" relationship?
Aggregation is a special form of association where one class represents a "whole" that contains "parts," but the parts can exist independently of the whole.

#### What is the key characteristic of aggregation: the part can exist independently of the whole?
Yes. For example, an `Employee` can exist even if the `Department` is dissolved; the employee can move to another department.

#### Give a real-world example of aggregation (e.g., `Department` has `Employees` — employees exist without the department)?
```java
class Employee {
    String name;
}

class Department {
    String name;
    List<Employee> employees; // aggregation

    Department(String name) {
        this.name = name;
        employees = new ArrayList<>();
    }

    void addEmployee(Employee e) {
        employees.add(e);
    }
}
// Employee objects can be created independently
Employee e1 = new Employee();
Department dept = new Department("IT");
dept.addEmployee(e1);
// If dept is destroyed, e1 still exists.
```

#### How is aggregation represented in Java (field reference, usually passed in via constructor or setter)?
Aggregation is represented by a reference (often a collection) to the part class. The part objects are typically created outside and passed in.

#### What is the lifecycle implication of aggregation (destroying the whole does NOT destroy the parts)?
When the whole object is garbage collected, the part objects continue to exist if referenced elsewhere.

#### What is the difference between aggregation and a simple association?
Aggregation implies a whole-part relationship and often a stronger sense of ownership (though not exclusive). Simple association is just a link without any implication of containment.

#### How is aggregation shown in UML (hollow diamond)?
In UML, aggregation is depicted with a hollow diamond on the side of the whole.

---

### 4. Composition

#### What is composition? How is it stronger than aggregation?
Composition is a stricter form of aggregation where the part's lifecycle is tied to the whole. The part cannot exist without the whole, and if the whole is destroyed, the parts are destroyed too.

#### What is the key characteristic of composition: the part cannot exist independently of the whole?
Yes. For example, `Room` cannot exist without a `House`. Rooms are created and destroyed with the house.

#### Give a real-world example of composition (e.g., `House` contains `Rooms` — rooms don't exist without the house)?
```java
class Room {
    String type;
    Room(String type) { this.type = type; }
}

class House {
    List<Room> rooms;
    House() {
        rooms = new ArrayList<>();
        rooms.add(new Room("Bedroom")); // rooms created inside House
        rooms.add(new Room("Kitchen"));
    }
    // No method to get or set rooms externally, rooms are encapsulated.
}
// Outside code cannot create Room objects independently and add them to House.
// When House is destroyed, its rooms are also gone (no external references).
```

#### How is composition represented in Java (part object created inside the owner, not shared)?
The part objects are instantiated inside the whole's constructor or methods and are not exposed via getters that allow external modification. They are private and only used internally.

#### What is the lifecycle implication of composition (destroying the whole destroys the parts)?
Since the whole creates the parts and holds the only references, when the whole becomes unreachable, the parts become unreachable too and are eligible for garbage collection.

#### What is the difference between composition and aggregation in terms of ownership and lifecycle?
- **Composition**: Strong ownership; part's lifecycle depends on whole.
- **Aggregation**: Weak ownership; part can exist independently.

#### How is composition shown in UML (filled diamond)?
UML represents composition with a filled diamond on the side of the whole.

#### How do you enforce composition in code (create the part internally, no external references)?
- Create the part objects inside the whole's constructor.
- Do not provide public getters that return the part objects (or return unmodifiable views).
- Ensure the part objects are not shared outside.

---

### 5. Inheritance vs Composition

#### What is the "favor composition over inheritance" principle?
It suggests that you should prefer composing objects (has-a) to achieve code reuse and flexibility rather than inheriting from a class (is-a), because composition leads to looser coupling and easier modification.

#### What are the problems with deep inheritance hierarchies (tight coupling, fragile base class)?
Deep hierarchies couple subclasses to superclass implementation details, making changes risky. The fragile base class problem becomes more severe.

#### When is inheritance appropriate (`is-a` is genuinely true and stable)?
Inheritance is appropriate when the relationship is a genuine `is-a`, the hierarchy is stable (not likely to change), and you need polymorphism. For example, `Circle` extends `Shape` – a circle truly is a shape, and shape behavior is well-defined.

#### When is composition more appropriate (behavior changes at runtime, avoids coupling)?
Composition is better when you need to change behavior at runtime (e.g., Strategy pattern), when you want to reuse code across unrelated classes, or when the relationship is `has-a` rather than `is-a`.

#### What is the Strategy pattern as an example of composition over inheritance?
The Strategy pattern uses composition to encapsulate interchangeable algorithms. Instead of subclassing to change behavior, you delegate to a strategy object.

```java
interface PaymentStrategy {
    void pay(int amount);
}

class CreditCardPayment implements PaymentStrategy {
    public void pay(int amount) { /* ... */ }
}

class ShoppingCart {
    private PaymentStrategy paymentStrategy; // composition

    void checkout(int amount) {
        paymentStrategy.pay(amount);
    }
}
// Behavior can be changed at runtime by setting a different strategy.
```

#### What is the difference between `is-a` (inheritance), `has-a` (composition/aggregation), and `uses-a` (dependency)?
- `is-a`: inheritance (e.g., `Dog is an Animal`).
- `has-a`: composition or aggregation (e.g., `Car has an Engine`).
- `uses-a`: dependency (e.g., `Driver uses a Car` temporarily).

#### What is the Open/Closed Principle and how does composition help achieve it?
Open/Closed Principle states that classes should be open for extension but closed for modification. Composition helps by allowing you to add new behavior through new composed objects without changing existing code (e.g., injecting different strategies).

#### What is delegation? How does it relate to composition?
Delegation is when an object forwards a request to another object (the delegate). Composition often uses delegation to implement behavior – the whole delegates tasks to its parts.

#### Can you achieve code reuse through composition? How (delegating method calls to the composed object)?
Yes, by composing objects and delegating method calls to them. For example, a `Printer` class could contain a `ConsoleWriter` object and delegate `print()` calls to it, reusing its functionality without inheritance.

---

### 6. Best Practices (Inheritance)

- **Favor composition over inheritance** – only inherit when there is a genuine, stable `is-a` relationship.
- **Design for inheritance or prohibit it** (`final` class) – do not leave classes in an ambiguous state.
- **Always call `super()` explicitly in constructors** when the superclass has meaningful initialization.
- **Do not call overridable methods from constructors** – subclass override runs before the subclass constructor, leaving state uninitialized.
```java
class Parent {
    Parent() { init(); } // BAD: calls overridable method
    void init() { }
}
class Child extends Parent {
    private String data;
    Child() { data = "initialized"; }
    @Override void init() { System.out.println(data.length()); } // data is null here!
}
```
- **Keep inheritance hierarchies shallow** (≤ 2–3 levels) to avoid the fragile base class problem.
- **Always annotate overriding methods with `@Override`** to catch typos at compile time.
- **Make classes `final` unless you explicitly design them to be subclassed.**

### 7. Best Practices (Object Relationships)

- **Model ownership carefully**: Use composition when the part's lifecycle is tied to the owner; use aggregation when parts are shared or independently managed.
- **Prefer composition for code reuse** – swap behavior at runtime without altering the class hierarchy.
- **Avoid circular associations** – they create tight coupling and serialization issues.
- **Use constructor injection** to make dependencies explicit and testable.
```java
class Service {
    private final Repository repo;
    public Service(Repository repo) { // dependency injected
        this.repo = repo;
    }
}
```
- **For many-to-many relationships**, introduce a dedicated relationship/join class rather than holding two lists to avoid duplication and inconsistency.
```java
class Enrollment {
    Student student;
    Course course;
    LocalDate enrollmentDate;
}
```

---













