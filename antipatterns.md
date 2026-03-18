## 1. Object-Oriented Design Anti-Patterns

### God Object / Blob  
**Explanation:** A single class that knows too much or does too much, centralizing functionality and becoming hard to maintain.  
**Example:**  
```java
// BAD: God class handling everything
public class GodClass {
    public void processOrder(Order order) { /* ... */ }
    public void calculateInvoice(Invoice inv) { /* ... */ }
    public void sendEmail(User user) { /* ... */ }
    public void generateReport() { /* ... */ }
    // dozens of other unrelated methods
}

// GOOD: Split responsibilities into separate classes
public class OrderProcessor { ... }
public class InvoiceCalculator { ... }
public class EmailService { ... }
public class ReportGenerator { ... }
```

### Anemic Domain Model  
**Explanation:** Domain objects contain only data (getters/setters) with no business logic, which is placed in separate service classes, leading to procedural code.  
**Example:**  
```java
// BAD: Anemic model
public class Order {
    private BigDecimal amount;
    // getters/setters only
}

public class OrderService {
    public void applyDiscount(Order order, BigDecimal discount) {
        order.setAmount(order.getAmount().subtract(discount));
    }
}

// GOOD: Rich domain model
public class Order {
    private BigDecimal amount;
    public void applyDiscount(BigDecimal discount) {
        this.amount = this.amount.subtract(discount);
    }
}
```

### Feature Envy  
**Explanation:** A method that is more interested in the data of another class than its own, indicating misplaced responsibility.  
**Example:**  
```java
public class Phone {
    private String areaCode;
    private String number;
    // getters
}

// BAD: Envying Phone's fields
public class Contact {
    private Phone phone;
    public String getPhoneNumberFormatted() {
        return "(" + phone.getAreaCode() + ") " + phone.getNumber();
    }
}

// GOOD: Move the method to Phone
public class Phone {
    private String areaCode;
    private String number;
    public String getFormattedNumber() {
        return "(" + areaCode + ") " + number;
    }
}
```

### Inappropriate Intimacy  
**Explanation:** One class accesses another’s private internals excessively, increasing coupling.  
**Example:**  
```java
public class A {
    private int secret = 42;
    public int getSecret() { return secret; }
}

// BAD: B uses A's getter to modify internal state
public class B {
    public void manipulate(A a) {
        int val = a.getSecret();
        // ... do something based on val, then set via a.setSecret(val+1)
    }
}
// GOOD: Provide a meaningful method in A to encapsulate the operation.
```

### Shotgun Surgery  
**Explanation:** A single change requires modifying many different classes, indicating poor modularization.  
**Example:**  
```java
// BAD: Changing discount logic affects Order, Invoice, Payment, Report classes
public class Order { public double calculateDiscount() { /* logic */ } }
public class Invoice { public double calculateDiscount() { /* same logic */ } }
public class Payment { public double calculateDiscount() { /* same logic */ } }
// GOOD: Centralize the logic in one class (e.g., DiscountCalculator) and reuse it.
```

### Refused Bequest  
**Explanation:** A subclass inherits from a parent but does not use most of the inherited methods, violating Liskov substitution principle.  
**Example:**  
```java
public class Bird {
    public void fly() { /* flying */ }
}
public class Penguin extends Bird {
    @Override
    public void fly() { throw new UnsupportedOperationException("Can't fly"); }
}
// GOOD: Favor composition or a more specific hierarchy (e.g., FlyingBird extends Bird).
```

### Parallel Inheritance Hierarchies  
**Explanation:** Adding a class to one hierarchy forces adding a corresponding class to another, creating maintenance overhead.  
**Example:**  
```java
// BAD: Vehicle and Mechanic hierarchies grow in parallel
class Car extends Vehicle { ... }
class CarMechanic extends Mechanic { ... }
class Bike extends Vehicle { ... }
class BikeMechanic extends Mechanic { ... }
// GOOD: Use a single hierarchy or a more flexible design (e.g., visitor pattern).
```

### Speculative Generality  
**Explanation:** Designing for future flexibility that never materializes, resulting in unnecessary complexity.  
**Example:**  
```java
// BAD: Abstract class with only one implementation
public abstract class PaymentProcessor {
    public abstract void process();
}
public class CreditCardProcessor extends PaymentProcessor {
    public void process() { /* actual logic */ }
}
// GOOD: Keep it simple unless multiple implementations are certain.
```

### Lava Flow  
**Explanation:** Dead or obsolete code left in the system, often commented out or unused, because developers are afraid to remove it.  
**Example:**  
```java
public class Example {
    // public void oldMethod() { ... } // commented out for years
    public void currentMethod() { ... }
}
// GOOD: Delete dead code; version control will keep history.
```

### Temporary Field Misuse  
**Explanation:** A field that is set only in certain scenarios, making the object’s state confusing.  
**Example:**  
```java
public class Parser {
    private String currentLine; // only used during parse()
    public void parse(File file) {
        currentLine = ...; // set
        // use currentLine
        currentLine = null; // reset
    }
}
// GOOD: Use a local variable instead of a field.
```

---

## 2. Code-Level Anti-Patterns

### Spaghetti Code  
**Explanation:** Code with little structure, using GOTOs, many nested conditionals, and tangled control flow.  
**Example:**  
```java
// BAD: Deeply nested and unstructured
if (condition1) {
    if (condition2) {
        while (something) {
            if (condition3) {
                // ...
            } else {
                // ...
            }
        }
    } else {
        // ...
    }
}
// GOOD: Refactor into smaller methods and use early returns.
```

### Copy-Paste Programming  
**Explanation:** Duplicating code instead of reusing it, leading to maintenance nightmares.  
**Example:**  
```java
public void methodA() {
    System.out.println("Step 1");
    System.out.println("Step 2"); // duplicated in methodB
}
public void methodB() {
    System.out.println("Step 1");
    System.out.println("Step 2");
    System.out.println("Step 3");
}
// GOOD: Extract common steps into a helper method.
```

### Magic Numbers / Strings  
**Explanation:** Using literal values directly in code without named constants, making the code hard to understand and maintain.  
**Example:**  
```java
// BAD
if (temperature > 32) { ... } // 32 what?
// GOOD
private static final int FREEZING_POINT_FAHRENHEIT = 32;
if (temperature > FREEZING_POINT_FAHRENHEIT) { ... }
```

### Long Methods / Large Classes  
**Explanation:** Methods that are too long (e.g., >20-30 lines) or classes that try to do too much, reducing readability and testability.  
**Example:**  
```java
// BAD: One huge method
public void processOrder(Order order) {
    // 100 lines of validation, calculation, persistence, notification...
}
// GOOD: Split into smaller focused methods.
```

### Deeply Nested Logic  
**Explanation:** Excessive nesting of loops and conditionals makes code hard to read.  
**Example:**  
```java
// BAD
for (...) {
    if (...) {
        while (...) {
            if (...) {
                // ...
            }
        }
    }
}
// GOOD: Use early exits, extract methods, or flatten with guard clauses.
```

### Excessive Comments instead of Clean Code  
**Explanation:** Comments are used to explain confusing code instead of making the code self-documenting.  
**Example:**  
```java
// BAD: Comment explains what, not why
// add 1 to i because...
i = i + 1;
// GOOD: Code is clear (e.g., i = i + 1 is fine; but maybe rename variable or extract method)
```

### Dead Code / Unused Code  
**Explanation:** Code that is never executed or used, cluttering the codebase.  
**Example:**  
```java
public void method() {
    int unused = 42;
    return;
}
// GOOD: Remove unused variables, methods, and imports.
```

### Primitive Obsession  
**Explanation:** Using primitives (int, String) to represent domain concepts instead of creating small objects.  
**Example:**  
```java
// BAD
public void setPhone(String phone) { ... } // phone as raw string
// GOOD
public class PhoneNumber { ... }
public void setPhone(PhoneNumber phone) { ... }
```

### Overuse of Static Methods  
**Explanation:** Using static methods for everything, which hinders testing and polymorphism.  
**Example:**  
```java
// BAD
public class Utilities {
    public static int calculateTax(int amount) { ... }
}
// GOOD: Use instance methods or dependency injection for better testability.
```

### Improper Naming Conventions  
**Explanation:** Non-descriptive or misleading names for classes, methods, variables.  
**Example:**  
```java
// BAD
public void d(Order o) { ... }
// GOOD
public void applyDiscount(Order order) { ... }
```

---

## 3. Exception Handling Anti-Patterns

### Catching Generic Exception (`Exception` / `Throwable`)  
**Explanation:** Catching too broad exceptions hides unexpected errors and can mask bugs.  
**Example:**  
```java
try {
    // risky code
} catch (Exception e) { // catches everything, including runtime exceptions
    // handle
}
// GOOD: Catch specific exceptions like IOException.
```

### Swallowing Exceptions  
**Explanation:** Catching an exception and doing nothing, which hides failures.  
**Example:**  
```java
try {
    // ...
} catch (IOException e) {
    // empty block
}
// GOOD: Log the exception or rethrow appropriately.
```

### Logging and Rethrowing  
**Explanation:** Logging the exception and then immediately rethrowing it, causing duplicate logs upstream.  
**Example:**  
```java
catch (IOException e) {
    logger.error("Error", e);
    throw e; // caller will log again
}
// GOOD: Either log or rethrow, not both, unless rethrowing a new exception.
```

### Overusing Checked Exceptions  
**Explanation:** Throwing checked exceptions for recoverable conditions, forcing callers to handle them and cluttering code.  
**Example:**  
```java
public void readFile() throws IOException { ... } // appropriate
// But for business logic failures, unchecked exceptions are often cleaner.
```

### Using Exceptions for Control Flow  
**Explanation:** Using exceptions to control program flow instead of conditionals.  
**Example:**  
```java
try {
    int i = Integer.parseInt(userInput);
} catch (NumberFormatException e) {
    // handle invalid input
}
// GOOD: Use if (userInput.matches("\\d+")) before parsing.
```

### Empty Catch Blocks  
**Explanation:** Catching an exception and doing nothing, which ignores the problem.  
**Example:**  
```java
try {
    // ...
} catch (Exception e) {}
// GOOD: At least log it.
```

### Losing Stack Trace Information  
**Explanation:** Creating a new exception without the cause, losing original stack trace.  
**Example:**  
```java
catch (SQLException e) {
    throw new MyException("DB error"); // loses original cause
}
// GOOD: throw new MyException("DB error", e);
```

### Over-wrapping Exceptions  
**Explanation:** Wrapping an exception multiple times without adding value, making debugging harder.  
**Example:**  
```java
catch (IOException e) {
    throw new RuntimeException(new IOException(e)); // useless wrapping
}
// GOOD: Throw a meaningful custom exception with cause.
```

---

## 4. Concurrency Anti-Patterns

### Synchronizing Everything  
**Explanation:** Using `synchronized` on every method even when not needed, causing unnecessary contention.  
**Example:**  
```java
public synchronized void method1() { ... } // may not need sync
public synchronized void method2() { ... }
// GOOD: Synchronize only critical sections or use concurrent collections.
```

### Overuse of `synchronized`  
**Explanation:** Using `synchronized` at a coarse-grained level (e.g., on entire method) when fine-grained locking would be better.  
**Example:**  
```java
public synchronized void update() {
    // non-critical code
    // critical code
    // more non-critical
}
// GOOD: Synchronize only the critical block.
```

### Ignoring Thread Safety  
**Explanation:** Sharing mutable data across threads without proper synchronization, leading to race conditions.  
**Example:**  
```java
public class Counter {
    private int count = 0;
    public void increment() { count++; } // not thread-safe
}
// GOOD: Use AtomicInteger or synchronized.
```

### Double-Checked Locking (Incorrect Implementation)  
**Explanation:** Attempting to reduce synchronization overhead but failing due to memory visibility issues in pre-Java 5.  
**Example:**  
```java
private static Singleton instance;
public static Singleton getInstance() {
    if (instance == null) {          // check without lock
        synchronized (Singleton.class) {
            if (instance == null) {  // second check
                instance = new Singleton();
            }
        }
    }
    return instance;
}
// In Java 5+, with volatile, it works. But simpler: use static inner class.
```

### Busy Waiting  
**Explanation:** Looping continuously while checking a condition, wasting CPU cycles.  
**Example:**  
```java
while (!flag) {} // spins until flag becomes true
// GOOD: Use wait/notify or higher-level constructs like CountDownLatch.
```

### Thread per Request Model  
**Explanation:** Creating a new thread for each incoming request, which doesn't scale under load.  
**Example:**  
```java
while (true) {
    Socket s = server.accept();
    new Thread(() -> handle(s)).start(); // creates many threads
}
// GOOD: Use a thread pool.
```

### Blocking in Reactive/Async Code  
**Explanation:** Using blocking calls (e.g., Thread.sleep(), JDBC calls) inside reactive pipelines, defeating the purpose.  
**Example:**  
```java
Mono.fromCallable(() -> {
    Thread.sleep(1000); // blocking!
    return "result";
});
// GOOD: Use non-blocking alternatives or schedule on separate scheduler.
```

### Misconfigured Thread Pools  
**Explanation:** Using wrong pool sizes (too small or too large) causing contention or resource exhaustion.  
**Example:**  
```java
Executors.newFixedThreadPool(1000); // may create too many threads
// GOOD: Tune based on CPU cores and task nature.
```

### Shared Mutable State  
**Explanation:** Multiple threads modifying the same variable without synchronization.  
**Example:**  
```java
int counter; // shared
// multiple threads do counter++
// GOOD: Use AtomicInteger or synchronize.
```

### Ignoring Happens-Before Rules  
**Explanation:** Assuming operations in one thread are visible to another in order without proper synchronization.  
**Example:**  
```java
boolean flag = false; // not volatile
// Thread 1: flag = true;
// Thread 2: while (!flag) {} // may never see change
// GOOD: Use volatile or other synchronization.
```

---

## 5. Collections & Data Structure Anti-Patterns

### Using Wrong Collection Type  
**Explanation:** Choosing a collection based on habit rather than requirements (e.g., using ArrayList where frequent insertions in middle are needed).  
**Example:**  
```java
List<String> list = new ArrayList<>();
list.add(0, "element"); // O(n) shift
// GOOD: Use LinkedList if frequent insertions at beginning/middle.
```

### Inefficient Iteration Patterns  
**Explanation:** Iterating in a way that causes performance issues, like nested loops over large collections.  
**Example:**  
```java
for (Product p : products) {
    for (Order o : orders) {
        if (o.getProductId().equals(p.getId())) { ... } // O(n*m)
    }
}
// GOOD: Use maps for lookups.
```

### Overusing `List` instead of `Set`  
**Explanation:** Using a List to store unique elements, then manually checking duplicates.  
**Example:**  
```java
List<String> names = new ArrayList<>();
if (!names.contains(name)) names.add(name); // O(n) check
// GOOD: Use Set<String>.
```

### Misusing `HashMap` Keys (mutable keys)  
**Explanation:** Using mutable objects as keys in HashMap, and then modifying them, breaking lookup.  
**Example:**  
```java
Map<MutableKey, String> map = new HashMap<>();
MutableKey key = new MutableKey(1);
map.put(key, "value");
key.setId(2); // modifies key, now hashcode changes, cannot retrieve
// GOOD: Use immutable keys.
```

### Not Overriding `equals()` and `hashCode()`  
**Explanation:** Using custom objects in hash-based collections without properly overriding these methods.  
**Example:**  
```java
public class Person { String name; }
Set<Person> set = new HashSet<>();
set.add(new Person("John"));
set.contains(new Person("John")); // false, because equals uses Object identity
// GOOD: Override equals and hashCode.
```

### Excessive Object Creation in Loops  
**Explanation:** Creating unnecessary objects inside loops, increasing GC pressure.  
**Example:**  
```java
for (int i = 0; i < 1000; i++) {
    String s = new String("value"); // creates new string each time
}
// GOOD: Use string literal or reuse objects.
```

### Premature Optimization of Collections  
**Explanation:** Optimizing collections without measuring, e.g., setting initial capacities arbitrarily.  
**Example:**  
```java
new ArrayList<>(1000000); // even if list rarely grows that large
// GOOD: Profile first, then optimize if needed.
```

### Using Legacy Collections (`Vector`, `Hashtable`)  
**Explanation:** Using older synchronized collections instead of modern concurrent or unsynchronized ones.  
**Example:**  
```java
Vector<String> v = new Vector<>(); // synchronized, slower
// GOOD: Use ArrayList if no sync needed, or CopyOnWriteArrayList if concurrent.
```

---

## 6. Memory & Resource Management Anti-Patterns

### Memory Leaks via Static References  
**Explanation:** Storing objects in static fields that prevent garbage collection.  
**Example:**  
```java
public class Cache {
    public static List<Data> dataList = new ArrayList<>(); // grows forever
}
// GOOD: Use weak references or clear when not needed.
```

### Misuse of `ThreadLocal`  
**Explanation:** Using ThreadLocal without cleaning up, leading to memory leaks in thread-pooled environments.  
**Example:**  
```java
private static ThreadLocal<Connection> connectionHolder = new ThreadLocal<>();
// after request, thread returns to pool but connection stays
// GOOD: Remove after use: connectionHolder.remove();
```

### Not Closing Resources (Streams, Connections)  
**Explanation:** Failing to close I/O resources, causing leaks.  
**Example:**  
```java
FileInputStream fis = new FileInputStream("file.txt");
// read data, but never close fis
// GOOD: Use try-with-resources.
```

### Overusing Object Creation  
**Explanation:** Creating many short-lived objects unnecessarily, increasing GC overhead.  
**Example:**  
```java
for (int i = 0; i < 1000000; i++) {
    new MyObject().process(); // each iteration creates new object
}
// GOOD: Reuse one object if possible.
```

### Large Object Retention  
**Explanation:** Holding references to large objects longer than needed, preventing GC.  
**Example:**  
```java
public class ReportProcessor {
    private byte[] hugeData;
    public void process() {
        hugeData = loadHugeData();
        // process...
        // hugeData still referenced after processing
    }
}
// GOOD: Set hugeData = null after use.
```

### Improper Caching (Unbounded Cache)  
**Explanation:** Caching without size limits, leading to memory exhaustion.  
**Example:**  
```java
Map<String, Data> cache = new HashMap<>(); // grows indefinitely
// GOOD: Use Guava Cache or Caffeine with eviction policies.
```

### Ignoring GC Behavior  
**Explanation:** Writing code without considering GC impact, e.g., creating many large objects in a loop.  
**Example:**  
```java
while (true) {
    int[] array = new int[1000000]; // allocates big array repeatedly
    // use array
}
// GOOD: Reuse buffers or tune GC.
```

### Holding References Longer Than Needed  
**Explanation:** Keeping references in collections or fields even after they are no longer needed.  
**Example:**  
```java
List<Session> activeSessions = ...;
// when session ends, remove from list, else memory leak.
```

---

## 7. Performance Anti-Patterns

### Premature Optimization  
**Explanation:** Optimizing code before identifying actual bottlenecks, leading to complex and unreadable code.  
**Example:**  
```java
// BAD: Using bit-shifts for multiplication without need
int result = (x << 3) - x; // instead of x * 7
// GOOD: Write clear code first, then profile and optimize.
```

### Ignoring Algorithm Complexity  
**Explanation:** Using algorithms with poor time complexity for large inputs (e.g., O(n²) instead of O(n log n)).  
**Example:**  
```java
// BAD: Bubble sort for large list
for (int i = 0; i < n; i++) {
    for (int j = i+1; j < n; j++) {
        if (arr[i] > arr[j]) swap;
    }
}
// GOOD: Use built-in sort (O(n log n)).
```

### Excessive Logging  
**Explanation:** Logging too much information, especially at debug level in production, causing I/O overhead.  
**Example:**  
```java
logger.debug("Entering method with param: " + param); // even if debug disabled, string concatenation happens
// GOOD: Use guard: if (logger.isDebugEnabled()) logger.debug(...);
```

### Recomputing Instead of Caching  
**Explanation:** Repeating expensive computations instead of caching results.  
**Example:**  
```java
public int fibonacci(int n) {
    if (n <= 1) return n;
    return fibonacci(n-1) + fibonacci(n-2); // exponential
}
// GOOD: Use memoization.
```

### Inefficient String Concatenation  
**Explanation:** Using `+` in loops to concatenate strings, creating many intermediate strings.  
**Example:**  
```java
String s = "";
for (int i = 0; i < 1000; i++) {
    s += i; // creates new String each iteration
}
// GOOD: Use StringBuilder.
```

### Overuse of Reflection  
**Explanation:** Using reflection for operations that could be done directly, causing performance overhead.  
**Example:**  
```java
Method method = obj.getClass().getMethod("getName");
String name = (String) method.invoke(obj);
// GOOD: Just call obj.getName() directly if possible.
```

### Blocking I/O in High-Throughput Systems  
**Explanation:** Using blocking I/O (e.g., reading from disk) in a thread-per-request model under high load, causing thread starvation.  
**Example:**  
```java
// In a servlet, reading a large file synchronously
// GOOD: Use asynchronous I/O or offload to dedicated threads.
```

### Not Using Profiling Tools  
**Explanation:** Guessing performance issues instead of measuring with profilers.  
**Example:**  
```java
// Assuming that method X is slow and optimizing it blindly, while actual bottleneck is elsewhere.
// GOOD: Profile first.
```

---

## 8. API Design Anti-Patterns

### Leaky Abstractions  
**Explanation:** An abstraction that exposes implementation details, forcing clients to know about them.  
**Example:**  
```java
public class FileReader {
    public void read() throws IOException { ... }
    public void close() { ... } // client must remember to close
}
// GOOD: Implement AutoCloseable and encourage try-with-resources.
```

### Inconsistent API Design  
**Explanation:** Similar operations have different naming or parameter orders, confusing users.  
**Example:**  
```java
public void saveUser(User u);
public void deleteById(int id);
public User load(int userId); // not consistent: load vs get, save vs store
// GOOD: Follow consistent naming conventions (e.g., get, save, delete).
```

### Overloaded Methods Abuse  
**Explanation:** Overloading methods with same number of parameters but ambiguous types, causing confusion.  
**Example:**  
```java
public void add(int a, int b);
public void add(Integer a, Integer b); // both have same erasure? Actually allowed but confusing
// GOOD: Use different names or clearer design.
```

### Breaking Backward Compatibility  
**Explanation:** Changing API signatures or behavior without a deprecation period, breaking existing clients.  
**Example:**  
```java
// v1: public void process(Order order)
// v2: public void process(Order order, boolean flag) // added param without default
// GOOD: Use default methods or overload with old signature.
```

### Poor Error Handling Contracts  
**Explanation:** Not specifying what exceptions are thrown or returning null instead of using Optional.  
**Example:**  
```java
public User findUser(String id) {
    if (notFound) return null; // client must check null
}
// GOOD: Return Optional<User> or throw documented exception.
```

### Exposing Internal State  
**Explanation:** Returning references to internal mutable objects, allowing clients to modify internal state.  
**Example:**  
```java
private List<String> items = new ArrayList<>();
public List<String> getItems() {
    return items; // exposes internal list
}
// GOOD: Return unmodifiable view: Collections.unmodifiableList(items);
```

### Fat Interfaces  
**Explanation:** Interfaces with many methods, forcing implementing classes to provide empty implementations.  
**Example:**  
```java
public interface Worker {
    void work();
    void eat();
    void sleep(); // not all workers need sleep
}
// GOOD: Split into smaller, cohesive interfaces.
```

### Inappropriate Use of Optional  
**Explanation:** Using Optional for fields, method parameters, or collections, which is not intended.  
**Example:**  
```java
public void setName(Optional<String> name) { ... } // Optional as param
// GOOD: Use overloading or null-check instead.
```

---

## 9. Spring / Enterprise Anti-Patterns

### Field Injection  
**Explanation:** Using `@Autowired` on fields, making testing harder and hiding dependencies.  
**Example:**  
```java
@Service
public class UserService {
    @Autowired private UserRepository repository; // field injection
}
// GOOD: Use constructor injection for immutability and testability.
```

### God Service Classes  
**Explanation:** A single service class handling many unrelated business processes.  
**Example:**  
```java
@Service
public class GodService {
    public void createUser() { ... }
    public void generateReport() { ... }
    public void sendEmail() { ... }
}
// GOOD: Split into focused services.
```

### Overuse of `@Transactional`  
**Explanation:** Adding `@Transactional` on every method, even read-only ones, causing unnecessary overhead.  
**Example:**  
```java
@Service
public class ProductService {
    @Transactional
    public Product getProduct(Long id) { ... } // read-only transaction
}
// GOOD: Use @Transactional(readOnly = true) for reads.
```

### Improper Transaction Boundaries  
**Explanation:** Transactions spanning multiple service calls or too long, leading to lock contention.  
**Example:**  
```java
@Transactional
public void placeOrder(Order order) {
    saveOrder(order);
    callExternalPaymentService(order); // slow network call within transaction
}
// GOOD: Keep transaction only for database operations.
```

### Circular Dependencies  
**Explanation:** Beans depending on each other, causing Spring to fail or use setter injection to break cycle, leading to confusion.  
**Example:**  
```java
@Service public class A { @Autowired private B b; }
@Service public class B { @Autowired private A a; }
// GOOD: Redesign to remove circular dependency.
```

### Excessive Use of Annotations  
**Explanation:** Cluttering code with annotations that could be configured externally, reducing readability.  
**Example:**  
```java
@RestController
@RequestMapping("/api")
@Slf4j
@RequiredArgsConstructor
@Validated
public class MyController { ... }
// GOOD: Use annotations judiciously.
```

### Misconfigured Bean Scopes  
**Explanation:** Using singleton scope for stateful beans, causing data corruption.  
**Example:**  
```java
@Component
@Scope("singleton")
public class ShoppingCart { // shared across users - wrong
    private List<Item> items = new ArrayList<>();
}
// GOOD: Use session or prototype scope.
```

### Ignoring Lazy Initialization  
**Explanation:** Eagerly loading all beans at startup, increasing startup time unnecessarily.  
**Example:**  
```java
@Component
public class ExpensiveBean {
    public ExpensiveBean() { // heavy initialization
    }
}
// GOOD: Mark with @Lazy if not always needed.
```

### Business Logic in Controllers  
**Explanation:** Putting business rules in controller methods, making them hard to test and reuse.  
**Example:**  
```java
@PostMapping("/order")
public void createOrder(@RequestBody OrderDto dto) {
    // validation
    // business logic
    // persistence
    // email sending
}
// GOOD: Delegate to service layer.
```

### Tight Coupling Between Layers  
**Explanation:** Directly using repository classes in controllers instead of services.  
**Example:**  
```java
@RestController
public class UserController {
    @Autowired private UserRepository repo; // controller accessing repository directly
}
// GOOD: Use service layer.
```

---

## 10. Persistence & Database Anti-Patterns

### N+1 Query Problem  
**Explanation:** Fetching a list of entities and then making additional queries for each entity’s related data.  
**Example:**  
```java
List<Order> orders = orderRepository.findAll(); // 1 query
for (Order o : orders) {
    User u = userRepository.findById(o.getUserId()); // N queries
}
// GOOD: Use JOIN FETCH or batch fetching.
```

### Overusing ORM without Understanding SQL  
**Explanation:** Relying on ORM to generate queries without understanding performance implications.  
**Example:**  
```java
// Hibernate may generate many inefficient queries due to lazy loading
// GOOD: Monitor SQL logs and optimize.
```

### Long Transactions  
**Explanation:** Keeping database transactions open while performing slow operations (e.g., HTTP calls).  
**Example:**  
```java
@Transactional
public void process() {
    db.update();
    restCall(); // minutes
    db.update();
}
// GOOD: Keep transactions short.
```

### Missing Indexes  
**Explanation:** Not adding indexes on columns used in WHERE or JOIN clauses, leading to full table scans.  
**Example:**  
```java
@Column
private String email; // frequently searched, but no index
// GOOD: Add @Index annotation or schema index.
```

### Improper Fetch Strategies  
**Explanation:** Using eager fetching for all relationships, causing huge joins and loading unnecessary data.  
**Example:**  
```java
@OneToMany(fetch = FetchType.EAGER)
private List<Order> orders; // always loads even if not needed
// GOOD: Use lazy fetching and fetch explicitly when needed.
```

### Storing Large Blobs in DB  
**Explanation:** Storing large binary data (e.g., images) in the database, causing performance issues.  
**Example:**  
```java
@Lob
private byte[] image; // large blob
// GOOD: Store files on disk/cloud and store path in DB.
```

### Chatty Database Calls  
**Explanation:** Making many small database calls instead of one batch call.  
**Example:**  
```java
for (Product p : products) {
    productRepository.save(p); // each call separate transaction
}
// GOOD: Use saveAll() or batch inserts.
```

### Ignoring Connection Pool Limits  
**Explanation:** Not configuring connection pool size appropriately, leading to connection exhaustion.  
**Example:**  
```java
// HikariCP default maxPoolSize = 10, but application may need more
// GOOD: Tune based on workload.
```

---

## 11. Microservices Anti-Patterns

### Distributed Monolith  
**Explanation:** Services are deployed separately but are tightly coupled and must be deployed together.  
**Example:**  
```java
// Service A calls Service B directly, and any change in B requires A to change
// GOOD: Define clear interfaces and versioning.
```

### Chatty Services  
**Explanation:** Multiple fine-grained calls between services to complete a single operation, increasing latency.  
**Example:**  
```java
// To place an order: call user service, then product service, then payment service, then inventory service sequentially
// GOOD: Use API composition or aggregate data in one service.
```

### Shared Database Across Services  
**Explanation:** Multiple services accessing the same database, leading to coupling and schema change issues.  
**Example:**  
```java
// OrderService and PaymentService both query orders table directly
// GOOD: Each service owns its data; communicate via APIs.
```

### Missing Service Boundaries  
**Explanation:** Services that are too small or too large, not aligned with business capabilities.  
**Example:**  
```java
// UserService also handles billing, notification, etc.
// GOOD: Apply Domain-Driven Design to define bounded contexts.
```

### Tight Coupling Between Services  
**Explanation:** Services depend on internal details of other services, e.g., sharing domain objects.  
**Example:**  
```java
// Service A uses JPA entity from Service B
// GOOD: Use DTOs and separate models.
```

### Lack of Circuit Breakers  
**Explanation:** Not handling failures gracefully; a failing service causes cascading failures.  
**Example:**  
```java
// Direct HTTP call without timeout or fallback
// GOOD: Use resilience4j or Hystrix.
```

### No Observability  
**Explanation:** Not having centralized logging, metrics, or tracing, making debugging impossible.  
**Example:**  
```java
// Each service logs locally; hard to trace a request across services
// GOOD: Use distributed tracing (Zipkin) and centralized logging (ELK).
```

### Synchronous Chains of Calls  
**Explanation:** Service A calls B, which calls C, etc., synchronously, increasing latency and coupling.  
**Example:**  
```java
// A -> B -> C -> D
// GOOD: Use asynchronous messaging or orchestration.
```

### Improper Retry Mechanisms  
**Explanation:** Retrying immediately without backoff, causing overload on failing services.  
**Example:**  
```java
// Retry 5 times immediately after failure
// GOOD: Use exponential backoff and circuit breaker.
```

---

## 12. Testing Anti-Patterns

### Lack of Tests  
**Explanation:** No automated tests, leading to fear of change and manual testing.  
**Example:**  
```java
// No unit tests for business logic
// GOOD: Write tests for critical paths.
```

### Over-Mocking  
**Explanation:** Mocking everything, even simple collaborators, leading to brittle tests that don't catch real issues.  
**Example:**  
```java
// Mocking a simple utility class that does string manipulation
// GOOD: Use real objects when possible.
```

### Fragile Tests  
**Explanation:** Tests that break with minor code changes, often due to testing implementation details.  
**Example:**  
```java
// Test asserts that a private method was called
// GOOD: Test behavior, not implementation.
```

### Testing Implementation Instead of Behavior  
**Explanation:** Tests that verify how something is done, not what it does.  
**Example:**  
```java
// Test checks that a certain sequence of methods is called
// GOOD: Test the result.
```

### Ignoring Edge Cases  
**Explanation:** Only testing happy paths, missing validation of error conditions.  
**Example:**  
```java
// Test passes valid input only; null, empty, or large values not tested
// GOOD: Include boundary and negative tests.
```

### Slow Test Suites  
**Explanation:** Tests that take too long to run, discouraging frequent execution.  
**Example:**  
```java
// Integration tests that hit real database for every test case
// GOOD: Use in-memory DB or mock for unit tests.
```

### Flaky Tests  
**Explanation:** Tests that sometimes pass, sometimes fail due to timing, concurrency, or environment issues.  
**Example:**  
```java
// Test relies on Thread.sleep
// GOOD: Use proper synchronization or wait conditions.
```

### No Separation Between Test Types  
**Explanation:** Mixing unit, integration, and end-to-end tests, making it hard to run only fast tests.  
**Example:**  
```java
// All tests in same folder, some require external services
// GOOD: Separate into different suites (e.g., unit, integration, e2e).
```

---

## 13. Security Anti-Patterns

### Hardcoded Credentials  
**Explanation:** Storing passwords, API keys, or secrets in source code.  
**Example:**  
```java
String dbPassword = "supersecret"; // in code
// GOOD: Use environment variables or secret management.
```

### Improper Input Validation  
**Explanation:** Trusting user input without validation, leading to injection attacks.  
**Example:**  
```java
String query = "SELECT * FROM users WHERE name = '" + userInput + "'";
// BAD: SQL injection possible
// GOOD: Use prepared statements.
```

### SQL Injection Vulnerabilities  
**Explanation:** Concatenating user input into SQL queries.  
**Example:**  
```java
Statement stmt = conn.createStatement();
stmt.executeQuery("SELECT * FROM products WHERE id = " + id);
// GOOD: Use PreparedStatement.
```

### Insecure Deserialization  
**Explanation:** Deserializing untrusted data without validation, allowing remote code execution.  
**Example:**  
```java
ObjectInputStream ois = new ObjectInputStream(input);
Object obj = ois.readObject(); // could be malicious
// GOOD: Use safe deserialization or validate.
```

### Weak Encryption Practices  
**Explanation:** Using outdated algorithms (MD5, DES) or hardcoded keys.  
**Example:**  
```java
MessageDigest md = MessageDigest.getInstance("MD5");
// GOOD: Use modern algorithms like SHA-256 with salt.
```

### Ignoring HTTPS/TLS  
**Explanation:** Transmitting sensitive data over unencrypted HTTP.  
**Example:**  
```java
// REST API exposed over HTTP
// GOOD: Enforce HTTPS.
```

### Overexposed APIs  
**Explanation:** Exposing internal endpoints without authentication or with excessive data.  
**Example:**  
```java
@GetMapping("/admin/users") // no security annotation
// GOOD: Use Spring Security to secure endpoints.
```

### Poor Secret Management  
**Explanation:** Storing secrets in config files that are committed to version control.  
**Example:**  
```java
// application.properties contains database password
// GOOD: Use externalized configuration and secret vaults.
```

---

## 14. Build & Dependency Anti-Patterns

### Dependency Hell  
**Explanation:** Conflicting transitive dependencies causing version issues.  
**Example:**  
```java
// Two libraries depend on different versions of same library
// GOOD: Use dependency management tools like Maven's dependencyManagement.
```

### Version Conflicts  
**Explanation:** Multiple versions of the same library in classpath, causing `NoSuchMethodError`.  
**Example:**  
```java
// Project includes both guava 18 and guava 19 via different modules
// GOOD: Exclude transitive or align versions.
```

### Overusing Transitive Dependencies  
**Explanation:** Including large dependencies that bring many unnecessary transitive dependencies.  
**Example:**  
```java
// Including spring-boot-starter-data-jpa when only using JDBC
// GOOD: Include only needed starters.
```

### Monolithic Build Pipelines  
**Explanation:** Single build job for all modules, making builds slow and hard to parallelize.  
**Example:**  
```java
// One Jenkins job building entire application with multiple modules
// GOOD: Use multi-module builds with parallel stages.
```

### Ignoring Dependency Updates  
**Explanation:** Using outdated libraries with known vulnerabilities.  
**Example:**  
```java
// Using log4j 1.x with known CVEs
// GOOD: Regularly update dependencies and use vulnerability scanners.
```

### Fat JAR Mismanagement  
**Explanation:** Creating overly large fat JARs including all dependencies, causing slow startup and deployment.  
**Example:**  
```java
// Spring Boot fat JAR includes all libraries; size huge
// GOOD: Use layered JARs or thin JARs where appropriate.
```

---

## 15. Cloud & Deployment Anti-Patterns

### Ignoring Container Limits  
**Explanation:** Running containers without memory/CPU limits, causing resource contention.  
**Example:**  
```java
// Docker run without --memory or --cpus
// GOOD: Set resource limits in orchestration.
```

### Stateful Services in Stateless Environments  
**Explanation:** Storing session state locally in a cloud environment where instances can be restarted or scaled.  
**Example:**  
```java
// HttpSession stored in memory, lost if instance dies
// GOOD: Use external session store (Redis).
```

### Improper Scaling Strategies  
**Explanation:** Scaling based on wrong metrics (e.g., CPU when bottleneck is database connections).  
**Example:**  
```java
// Autoscale based on CPU, but app is I/O bound
// GOOD: Scale based on appropriate metrics.
```

### No Health Checks  
**Explanation:** Not providing health endpoints for orchestrator to know if instance is ready.  
**Example:**  
```java
// Kubernetes liveness probe not configured
// GOOD: Implement /health endpoint.
```

### Configuration Hardcoding  
**Explanation:** Hardcoding environment-specific config (e.g., database URLs) in code.  
**Example:**  
```java
String dbUrl = "jdbc:mysql://localhost:3306/db";
// GOOD: Use environment variables or config server.
```

### Missing Failover Strategies  
**Explanation:** Not planning for instance failures; single point of failure.  
**Example:**  
```java
// Only one replica of a critical service
// GOOD: Run multiple replicas behind load balancer.
```

---

## 16. Observability Anti-Patterns

### No Logging Strategy  
**Explanation:** Logging arbitrarily without structure, making it hard to search and correlate.  
**Example:**  
```java
System.out.println("Error occurred"); // not structured
// GOOD: Use structured logging (e.g., JSON) and log levels.
```

### Excessive or No Logging  
**Explanation:** Logging too much (flood) or too little (blindness).  
**Example:**  
```java
// Logging every line of code vs logging nothing
// GOOD: Log meaningful events at appropriate levels.
```

### Missing Metrics  
**Explanation:** Not collecting metrics (e.g., request rate, latency, errors) to understand system health.  
**Example:**  
```java
// No insight into performance
// GOOD: Use Micrometer or Dropwizard metrics.
```

### Lack of Distributed Tracing  
**Explanation:** Unable to trace a request across multiple microservices, making debugging impossible.  
**Example:**  
```java
// Each service logs independently; cannot follow a transaction
// GOOD: Use tracing (Jaeger, Zipkin) with correlation IDs.
```

### Ignoring Alerts  
**Explanation:** Having alerts but no one responds, or too many false positives.  
**Example:**  
```java
// PagerDuty alerts for every minor error, leading to alert fatigue
// GOOD: Define actionable alerts and tune thresholds.
```

### Logging Sensitive Data  
**Explanation:** Logging passwords, credit card numbers, etc., causing security breaches.  
**Example:**  
```java
logger.info("User login: " + password); // bad!
// GOOD: Mask or omit sensitive data.
```

---

## 17. Architectural Anti-Patterns

### Big Ball of Mud  
**Explanation:** A system with no clear structure, tangled dependencies, and haphazard growth.  
**Example:**  
```java
// Monolithic application where everything depends on everything
// GOOD: Modularize with clear boundaries.
```

### Over-Engineering  
**Explanation:** Adding unnecessary complexity (design patterns, abstractions) for simple problems.  
**Example:**  
```java
// Using factory, builder, visitor for a CRUD app with 2 entities
// GOOD: Keep it simple until needed.
```

### Reinventing the Wheel  
**Explanation:** Building custom solutions for problems already solved by existing libraries.  
**Example:**  
```java
// Writing own JSON parser instead of using Jackson
// GOOD: Use well-tested libraries.
```

### Tight Coupling Across Modules  
**Explanation:** Modules that are highly dependent on each other, making changes difficult.  
**Example:**  
```java
// Module A uses internal classes from Module B
// GOOD: Define stable interfaces.
```

### Lack of Modularity  
**Explanation:** No separation of concerns; code is one big blob.  
**Example:**  
```java
// All classes in one package
// GOOD: Organize by feature or layer.
```

### Violating Separation of Concerns  
**Explanation:** Mixing presentation, business, and data access code.  
**Example:**  
```java
// JSP with SQL queries and business logic
// GOOD: Use MVC pattern.
```

### Ignoring Domain Boundaries  
**Explanation:** Not aligning software structure with business domains, leading to unclear responsibilities.  
**Example:**  
```java
// One service handles both inventory and shipping, which are separate domains
// GOOD: Apply Domain-Driven Design.
```

---
