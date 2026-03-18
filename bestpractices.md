
## 1. Object-Oriented Design Best Practices

### SOLID Principles Deep Application

#### • SRP violations and refactoring
**Explanation:** The Single Responsibility Principle states that a class should have only one reason to change. Violations occur when a class handles multiple concerns (e.g., business logic + persistence + formatting).  
**Example:** Refactoring a `Report` class that both generates content and saves to a file.

```java
// Before: SRP violation
class Report {
    void generateReport() { /* ... */ }
    void saveToFile(String filename) { /* file I/O code */ }
}

// After: separated responsibilities
class ReportGenerator {
    Report generateReport() { /* ... */ return new Report(...); }
}
class ReportRepository {
    void save(Report report, String filename) { /* file I/O */ }
}
```

#### • OCP with extensibility strategies
**Explanation:** Open/Closed Principle – classes should be open for extension but closed for modification. Achieved via abstractions (interfaces/abstract classes) and polymorphism.  
**Example:** A payment processor that can be extended with new payment methods without changing existing code.

```java
interface PaymentMethod {
    void pay(double amount);
}

class CreditCardPayment implements PaymentMethod {
    public void pay(double amount) { /* ... */ }
}

class PayPalPayment implements PaymentMethod {
    public void pay(double amount) { /* ... */ }
}

class PaymentProcessor {
    void process(PaymentMethod method, double amount) {
        method.pay(amount);
    }
}
```

#### • LSP edge cases (inheritance pitfalls)
**Explanation:** Liskov Substitution Principle – subtypes must be substitutable for their base types. A classic violation is a `Square` extending `Rectangle` if setters break expectations.  
**Example:** Using composition instead of inheritance to avoid LSP violation.

```java
// Violation
class Rectangle {
    void setWidth(int w) { width = w; }
    void setHeight(int h) { height = h; }
}
class Square extends Rectangle {
    void setWidth(int w) { super.setWidth(w); super.setHeight(w); }
    // setHeight similarly breaks
}

// Better: use a common interface
interface Shape { int area(); }
class Rectangle implements Shape { ... }
class Square implements Shape { ... }
```

#### • ISP vs fat interfaces
**Explanation:** Interface Segregation Principle – no client should be forced to depend on methods it does not use. Fat interfaces are split into smaller, more specific ones.  
**Example:** A multi‑function printer interface split into separate roles.

```java
// Fat interface
interface MultiFunctionDevice {
    void print();
    void scan();
    void fax();
}
class OldPrinter implements MultiFunctionDevice {
    public void print() { /* ok */ }
    public void scan() { throw new UnsupportedOperationException(); }
    public void fax() { throw new UnsupportedOperationException(); }
}

// Segregated interfaces
interface Printer { void print(); }
interface Scanner { void scan(); }
interface Fax { void fax(); }
class SimplePrinter implements Printer { public void print() { /* ... */ } }
```

#### • DIP with frameworks (Spring, CDI)
**Explanation:** Dependency Inversion Principle – depend on abstractions, not concretions. Frameworks like Spring implement this via dependency injection.  
**Example:** Injecting a repository interface rather than a concrete class.

```java
interface UserRepository { User findById(Long id); }

@Repository
class JpaUserRepository implements UserRepository { ... }

@Service
class UserService {
    private final UserRepository userRepo;
    
    @Autowired
    public UserService(UserRepository userRepo) { // abstraction injected
        this.userRepo = userRepo;
    }
}
```

### Composition over inheritance
**Explanation:** Prefer composing objects (has‑a) to extending classes (is‑a) to achieve more flexible and reusable designs.  
**Example:** Instead of subclassing `ArrayList` to add logging, compose with a `List` and delegate.

```java
class LoggingList<E> implements List<E> {
    private final List<E> delegate = new ArrayList<>();
    
    public boolean add(E e) {
        System.out.println("Adding " + e);
        return delegate.add(e);
    }
    // other methods delegated similarly
}
```

### Favor immutability
**Explanation:** Immutable objects are simpler, thread‑safe, and side‑effect‑free. Use `final` fields, no setters, and defensive copying.  
**Example:** A `Person` class that cannot be changed after creation.

```java
public final class Person {
    private final String name;
    private final int age;
    
    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }
    // only getters, no setters
}
```

### Encapsulation boundaries
**Explanation:** Hide internal state and implementation details. Expose only what is necessary through a well‑defined public API.  
**Example:** Using package‑private or private access, and providing public methods that guard invariants.

```java
public class BankAccount {
    private BigDecimal balance; // hidden
    
    public void deposit(BigDecimal amount) {
        if (amount.compareTo(BigDecimal.ZERO) <= 0) throw new IllegalArgumentException();
        balance = balance.add(amount);
    }
    public BigDecimal getBalance() { return balance; }
}
```

### Law of Demeter (LoD)
**Explanation:** “Don’t talk to strangers.” An object should only communicate with its immediate dependencies, avoiding method chaining like `a.getB().getC().doSomething()`.  
**Example:** Refactoring to delegate.

```java
// Violation
order.getCustomer().getAddress().getCity();

// Better
class Order {
    private Customer customer;
    public String getCustomerCity() {
        return customer.getCity();
    }
}
class Customer {
    private Address address;
    public String getCity() { return address.getCity(); }
}
```

### Tell Don’t Ask principle
**Explanation:** Instead of asking an object for data and then performing logic on it, tell the object to perform the logic itself.  
**Example:** Calculating tax.

```java
// Tell Don't Ask violation
double price = item.getPrice();
double tax = price * 0.2;
item.setPrice(price + tax);

// Following the principle
item.applyTax(0.2);  // item handles its own logic
```

### Domain modeling best practices
**Explanation:** Model the business domain explicitly, using ubiquitous language and encapsulating rules within domain objects.  
**Example:** A `Money` value object that encapsulates currency and amount, with operations.

```java
public class Money {
    private final BigDecimal amount;
    private final Currency currency;
    
    public Money add(Money other) {
        if (!this.currency.equals(other.currency)) throw new IllegalArgumentException();
        return new Money(this.amount.add(other.amount), this.currency);
    }
    // other business methods
}
```

### Rich vs Anemic domain models
**Explanation:** An anemic domain model has getters/setters but no behavior; a rich model places business logic inside the domain objects.  
**Example:** Anemic: `Order` with getters/setters, service handles total calculation. Rich: `Order` has `calculateTotal()` method.

```java
// Rich model
class Order {
    private List<OrderLine> lines;
    public Money calculateTotal() {
        return lines.stream().map(OrderLine::getSubtotal).reduce(Money.ZERO, Money::add);
    }
}
```

---

## 2. Creational Design Patterns

### Singleton
**Thread‑safe implementations:** Ensure only one instance exists in a concurrent environment.

```java
// Eager initialization (thread‑safe by classloader)
public class EagerSingleton {
    private static final EagerSingleton INSTANCE = new EagerSingleton();
    private EagerSingleton() {}
    public static EagerSingleton getInstance() { return INSTANCE; }
}

// Lazy initialization with double‑checked locking (Java 5+)
public class LazySingleton {
    private static volatile LazySingleton instance;
    private LazySingleton() {}
    public static LazySingleton getInstance() {
        if (instance == null) {
            synchronized (LazySingleton.class) {
                if (instance == null) {
                    instance = new LazySingleton();
                }
            }
        }
        return instance;
    }
}
```

**Enum‑based singleton:** The simplest and most robust way.

```java
public enum EnumSingleton {
    INSTANCE;
    public void doSomething() { ... }
}
```

**Lazy vs eager initialization:** Lazy defers creation until needed; eager creates at class loading.

### Factory Method
**Parameterized factory:** Creates objects based on input parameters.

```java
interface Animal { void speak(); }
class Dog implements Animal { public void speak() { System.out.println("Woof"); } }
class Cat implements Animal { public void speak() { System.out.println("Meow"); } }

class AnimalFactory {
    public static Animal createAnimal(String type) {
        if ("dog".equalsIgnoreCase(type)) return new Dog();
        else if ("cat".equalsIgnoreCase(type)) return new Cat();
        else throw new IllegalArgumentException();
    }
}
```

**Registry‑based factory:** Objects are registered with a key and created via that registry.

```java
class ShapeFactory {
    private static final Map<String, Supplier<Shape>> registry = new HashMap<>();
    static {
        registry.put("circle", Circle::new);
        registry.put("square", Square::new);
    }
    public static Shape createShape(String type) {
        return registry.getOrDefault(type, () -> { throw new IllegalArgumentException(); }).get();
    }
}
```

### Abstract Factory
**Families of related objects:** Provides an interface for creating families of related or dependent objects without specifying concrete classes.

```java
interface GUIFactory {
    Button createButton();
    Checkbox createCheckbox();
}

class WindowsFactory implements GUIFactory {
    public Button createButton() { return new WindowsButton(); }
    public Checkbox createCheckbox() { return new WindowsCheckbox(); }
}

class MacFactory implements GUIFactory {
    public Button createButton() { return new MacButton(); }
    public Checkbox createCheckbox() { return new MacCheckbox(); }
}
```

### Builder
**Immutable object construction:** Builds complex objects step by step, often with a fluent interface.

```java
public class User {
    private final String firstName;
    private final String lastName;
    private final int age;
    // private constructor
    private User(Builder builder) { ... }
    
    public static class Builder {
        private String firstName;
        private String lastName;
        private int age;
        public Builder firstName(String firstName) { this.firstName = firstName; return this; }
        public Builder lastName(String lastName) { this.lastName = lastName; return this; }
        public Builder age(int age) { this.age = age; return this; }
        public User build() { return new User(this); }
    }
}

// Usage: User user = new User.Builder().firstName("John").lastName("Doe").age(30).build();
```

**Fluent builders:** Method chaining that reads like natural language.

### Prototype
**Deep vs shallow copy:** Creating new objects by copying an existing instance. Shallow copy copies references; deep copy duplicates nested objects.

```java
class Address implements Cloneable {
    String city;
    public Address clone() { // shallow
        try { return (Address) super.clone(); } catch (CloneNotSupportedException e) { return null; }
    }
}

class Person implements Cloneable {
    String name;
    Address address;
    public Person shallowCopy() { // shallow
        try { return (Person) super.clone(); } catch (CloneNotSupportedException e) { return null; }
    }
    public Person deepCopy() {
        Person p = shallowCopy();
        p.address = this.address.clone(); // also clone mutable fields
        return p;
    }
}
```

### Object Pool
**Reuse expensive objects:** Manage a pool of reusable objects (e.g., database connections).

```java
class ConnectionPool {
    private final List<Connection> pool = new ArrayList<>();
    private final int maxSize = 10;
    
    public synchronized Connection getConnection() {
        if (pool.isEmpty()) {
            return createNewConnection();
        } else {
            return pool.remove(pool.size() - 1);
        }
    }
    
    public synchronized void releaseConnection(Connection conn) {
        if (pool.size() < maxSize) pool.add(conn);
        else conn.close(); // discard
    }
}
```

### Dependency Injection patterns
**Explanation:** Inversion of control – dependencies are provided from outside rather than created internally. Constructor injection is preferred.

```java
class Service {
    private final Repository repo;
    
    @Inject
    public Service(Repository repo) { // dependency injected
        this.repo = repo;
    }
}
```

---

## 3. Structural Design Patterns

### Adapter
**Class vs object adapter:** Converts the interface of a class into another interface clients expect. Class adapter uses multiple inheritance (not possible in Java), object adapter uses composition.

```java
// Target interface
interface MediaPlayer { void play(String audioType, String filename); }

// Adaptee
class AdvancedMediaPlayer { void playMp4(String filename) { ... } }

// Object adapter
class MediaAdapter implements MediaPlayer {
    private AdvancedMediaPlayer advancedPlayer = new AdvancedMediaPlayer();
    
    public void play(String audioType, String filename) {
        if ("mp4".equals(audioType)) advancedPlayer.playMp4(filename);
        // else handle others
    }
}
```

### Bridge
**Decouple abstraction from implementation:** Allows both to vary independently.

```java
// Implementor
interface Device {
    void turnOn();
    void turnOff();
}

// Abstraction
abstract class RemoteControl {
    protected Device device;
    public RemoteControl(Device device) { this.device = device; }
    abstract void togglePower();
}

class TVRemote extends RemoteControl {
    public TVRemote(Device device) { super(device); }
    void togglePower() { /* use device methods */ }
}

// Concrete implementors
class TV implements Device { public void turnOn() { ... } public void turnOff() { ... } }
class Radio implements Device { ... }
```

### Composite
**Tree structures:** Compose objects into tree structures to represent part‑whole hierarchies. Clients treat individual objects and compositions uniformly.

```java
interface Graphic { void draw(); }

class Circle implements Graphic { public void draw() { System.out.println("Circle"); } }

class CompositeGraphic implements Graphic {
    private List<Graphic> children = new ArrayList<>();
    public void add(Graphic graphic) { children.add(graphic); }
    public void draw() { for (Graphic g : children) g.draw(); }
}
```

### Decorator
**Runtime behavior extension:** Attach additional responsibilities to an object dynamically. Decorators provide a flexible alternative to subclassing.

```java
interface Coffee { double cost(); }

class SimpleCoffee implements Coffee { public double cost() { return 2.0; } }

abstract class CoffeeDecorator implements Coffee {
    protected Coffee decorated;
    public CoffeeDecorator(Coffee coffee) { this.decorated = coffee; }
    public double cost() { return decorated.cost(); }
}

class MilkDecorator extends CoffeeDecorator {
    public MilkDecorator(Coffee coffee) { super(coffee); }
    public double cost() { return super.cost() + 0.5; }
}
```

### Facade
**Simplifying complex subsystems:** Provide a unified interface to a set of interfaces in a subsystem.

```java
class ComputerFacade {
    private CPU cpu = new CPU();
    private Memory memory = new Memory();
    private HardDrive hardDrive = new HardDrive();
    
    public void start() {
        cpu.freeze();
        memory.load();
        cpu.jump();
        cpu.execute();
    }
}
```

### Flyweight
**Memory optimization:** Share common parts of object state among multiple objects instead of storing duplicate data.

```java
// Flyweight
class TreeType {
    private String name, color, texture; // intrinsic state
    public void draw(int x, int y) { ... } // extrinsic state (coordinates) passed as parameters
}

class TreeFactory {
    private static Map<String, TreeType> types = new HashMap<>();
    public static TreeType getTreeType(String name, String color, String texture) {
        String key = name + color + texture;
        return types.computeIfAbsent(key, k -> new TreeType(name, color, texture));
    }
}

class Tree {
    private int x, y;
    private TreeType type; // shared flyweight
    public void draw() { type.draw(x, y); }
}
```

### Proxy
**Virtual proxy:** Delays creation of an expensive object until it is actually needed.

```java
interface Image { void display(); }

class RealImage implements Image {
    private String filename;
    public RealImage(String filename) { loadFromDisk(); } // expensive
    public void display() { System.out.println("Displaying " + filename); }
    private void loadFromDisk() { /* ... */ }
}

class ProxyImage implements Image {
    private RealImage realImage;
    private String filename;
    public ProxyImage(String filename) { this.filename = filename; }
    public void display() {
        if (realImage == null) realImage = new RealImage(filename); // lazy load
        realImage.display();
    }
}
```

**Protection proxy:** Controls access to the real object based on permissions.

```java
interface BankAccount { void withdraw(double amount); }

class RealBankAccount implements BankAccount { ... }

class ProtectionProxy implements BankAccount {
    private RealBankAccount account;
    private String userRole;
    public void withdraw(double amount) {
        if ("admin".equals(userRole)) account.withdraw(amount);
        else throw new SecurityException();
    }
}
```

**Remote proxy:** Represents an object in a different address space (e.g., RMI stubs).

**Dynamic proxy (JDK / CGLIB):** Creates proxy at runtime. JDK dynamic proxy requires an interface.

```java
// JDK dynamic proxy
InvocationHandler handler = (proxy, method, args) -> {
    System.out.println("Before method: " + method.getName());
    Object result = method.invoke(realObject, args);
    System.out.println("After method");
    return result;
};
SomeInterface proxy = (SomeInterface) Proxy.newProxyInstance(
    SomeInterface.class.getClassLoader(),
    new Class<?>[] { SomeInterface.class },
    handler);
```

---

## 4. Behavioral Design Patterns

### Strategy
**Runtime algorithm switching:** Define a family of algorithms, encapsulate each, and make them interchangeable.

```java
interface SortStrategy { void sort(int[] numbers); }

class BubbleSort implements SortStrategy { public void sort(int[] numbers) { ... } }
class QuickSort implements SortStrategy { public void sort(int[] numbers) { ... } }

class Sorter {
    private SortStrategy strategy;
    public void setStrategy(SortStrategy strategy) { this.strategy = strategy; }
    public void sort(int[] numbers) { strategy.sort(numbers); }
}
```

### Observer
**Event‑driven systems:** Define a one‑to‑many dependency between objects so that when one object changes state, all its dependents are notified.

```java
interface Observer { void update(String event); }

class Subject {
    private List<Observer> observers = new ArrayList<>();
    public void attach(Observer o) { observers.add(o); }
    public void notifyObservers(String event) {
        for (Observer o : observers) o.update(event);
    }
}

class ConcreteObserver implements Observer {
    public void update(String event) { System.out.println("Received: " + event); }
}
```

**Reactive streams integration:** Java 9+ `Flow` API.

### Command
**Undo/redo operations:** Encapsulate a request as an object, allowing parameterization, queuing, and undoable operations.

```java
interface Command { void execute(); void undo(); }

class Light {
    public void on() { System.out.println("Light on"); }
    public void off() { System.out.println("Light off"); }
}

class LightOnCommand implements Command {
    private Light light;
    public LightOnCommand(Light light) { this.light = light; }
    public void execute() { light.on(); }
    public void undo() { light.off(); }
}

class RemoteControl {
    private Command lastCommand;
    public void press(Command cmd) { cmd.execute(); lastCommand = cmd; }
    public void undo() { if (lastCommand != null) lastCommand.undo(); }
}
```

### Chain of Responsibility
**Middleware pipelines:** Pass a request along a chain of handlers. Each handler decides either to process the request or pass it to the next.

```java
abstract class Handler {
    private Handler next;
    public Handler setNext(Handler next) { this.next = next; return next; }
    public void handle(String request) {
        if (!process(request) && next != null) next.handle(request);
    }
    protected abstract boolean process(String request);
}

class AuthenticationHandler extends Handler {
    protected boolean process(String request) {
        if (request.contains("auth")) { System.out.println("Authenticated"); return true; }
        return false;
    }
}

class LoggingHandler extends Handler {
    protected boolean process(String request) {
        System.out.println("Logging: " + request);
        return false; // always pass to next
    }
}
```

### State
**State machine modeling:** Allow an object to alter its behavior when its internal state changes. The object will appear to change its class.

```java
interface State { void handle(); }

class StartState implements State {
    public void handle() { System.out.println("Starting"); }
}

class StopState implements State {
    public void handle() { System.out.println("Stopping"); }
}

class Context {
    private State state;
    public void setState(State state) { this.state = state; }
    public void request() { state.handle(); }
}
```

### Template Method
**Define the skeleton of an algorithm in a method, deferring some steps to subclasses.**

```java
abstract class DataProcessor {
    public final void process() {
        loadData();
        processData();
        saveData();
    }
    abstract void loadData();
    abstract void processData();
    void saveData() { System.out.println("Saving to DB"); } // common implementation
}

class CsvProcessor extends DataProcessor {
    void loadData() { System.out.println("Loading CSV"); }
    void processData() { System.out.println("Processing CSV"); }
}
```

### Visitor
**AST processing:** Represent an operation to be performed on elements of an object structure. Visitor lets you define a new operation without changing the classes of the elements.

```java
interface Element { void accept(Visitor v); }

class Book implements Element {
    public void accept(Visitor v) { v.visit(this); }
}
class Fruit implements Element {
    public void accept(Visitor v) { v.visit(this); }
}

interface Visitor {
    void visit(Book book);
    void visit(Fruit fruit);
}

class PriceVisitor implements Visitor {
    public void visit(Book book) { System.out.println("Book price 10"); }
    public void visit(Fruit fruit) { System.out.println("Fruit price 5"); }
}
```

### Mediator
**Centralize complex communications between objects:** Objects communicate through a mediator rather than directly.

```java
interface ChatMediator {
    void sendMessage(String msg, User user);
    void addUser(User user);
}

class ChatRoom implements ChatMediator {
    private List<User> users = new ArrayList<>();
    public void addUser(User user) { users.add(user); }
    public void sendMessage(String msg, User user) {
        for (User u : users) if (u != user) u.receive(msg);
    }
}

class User {
    private ChatMediator mediator;
    private String name;
    public void send(String msg) { mediator.sendMessage(msg, this); }
    public void receive(String msg) { System.out.println(name + " received: " + msg); }
}
```

### Interpreter
**Define a grammar and interpret sentences:** Used in parsing languages, expressions.

```java
interface Expression { int interpret(); }

class Number implements Expression {
    private int number;
    public Number(int number) { this.number = number; }
    public int interpret() { return number; }
}

class Plus implements Expression {
    private Expression left, right;
    public Plus(Expression left, Expression right) { this.left = left; this.right = right; }
    public int interpret() { return left.interpret() + right.interpret(); }
}
// Usage: Expression expr = new Plus(new Number(2), new Number(3));
```

### Iterator
**Provide a way to access elements of a collection sequentially without exposing its underlying representation.** Java’s `Iterator` interface.

```java
class MyCollection<T> implements Iterable<T> {
    private T[] items;
    public Iterator<T> iterator() {
        return new Iterator<T>() {
            private int index = 0;
            public boolean hasNext() { return index < items.length; }
            public T next() { return items[index++]; }
        };
    }
}
```

### Null Object pattern
**Provide an object that does nothing instead of using null:** Avoid null checks.

```java
interface Logger { void log(String msg); }

class ConsoleLogger implements Logger {
    public void log(String msg) { System.out.println(msg); }
}

class NullLogger implements Logger { // does nothing
    public void log(String msg) { /* silent */ }
}

class Service {
    private Logger logger;
    public Service(Logger logger) { this.logger = logger != null ? logger : new NullLogger(); }
    public void doSomething() { logger.log("Doing something"); }
}
```

---

## 5. Concurrency & Multithreading Patterns

### Thread confinement
**Ensure that an object is only accessed by a single thread** to avoid synchronization. Examples: thread‑local variables, stack‑confined objects.

```java
// ThreadLocal example
private static final ThreadLocal<SimpleDateFormat> dateFormat = ThreadLocal.withInitial(
    () -> new SimpleDateFormat("yyyy-MM-dd"));
// Each thread gets its own copy
```

### Thread‑per‑task vs thread pool
**Thread‑per‑task** creates a new thread for each task (simple but overhead). **Thread pool** reuses threads (e.g., `ExecutorService`).

```java
// Thread pool
ExecutorService executor = Executors.newFixedThreadPool(10);
executor.submit(() -> { /* task */ });
```

### Producer-Consumer
**Decouple production and consumption of data using a shared queue.** Java’s `BlockingQueue` simplifies.

```java
BlockingQueue<Integer> queue = new LinkedBlockingQueue<>(10);

// Producer
new Thread(() -> {
    for (int i = 0; i < 100; i++) {
        queue.put(i);
    }
}).start();

// Consumer
new Thread(() -> {
    while (true) {
        Integer item = queue.take();
        // process item
    }
}).start();
```

### Guarded Suspension
**Wait for a condition to become true before proceeding.** Often implemented with `wait()` / `notify()` or higher‑level constructs.

```java
synchronized (lock) {
    while (!condition) {
        lock.wait(); // releases lock and waits
    }
    // condition satisfied, proceed
}
```

### Balking pattern
**Only perform an action if the object is in the correct state; otherwise, return immediately.** Example: a start method that only starts once.

```java
class BackgroundTask {
    private volatile boolean started = false;
    public void start() {
        if (started) return; // balk
        synchronized (this) {
            if (started) return;
            started = true;
            // actually start
        }
    }
}
```

### Double‑checked locking
**Used to lazily initialize a singleton with minimal synchronization.** Requires `volatile` in Java 5+.

```java
private static volatile Singleton instance;
public static Singleton getInstance() {
    if (instance == null) {
        synchronized (Singleton.class) {
            if (instance == null) {
                instance = new Singleton();
            }
        }
    }
    return instance;
}
```

### Immutable objects
**Objects whose state cannot change after construction** – inherently thread‑safe.

```java
public final class ImmutablePoint {
    private final int x, y;
    public ImmutablePoint(int x, int y) { this.x = x; this.y = y; }
    // only getters
}
```

### Read‑write locks
**Allow multiple readers but only one writer.** `ReentrantReadWriteLock`.

```java
ReadWriteLock rwLock = new ReentrantReadWriteLock();
Lock readLock = rwLock.readLock();
Lock writeLock = rwLock.writeLock();

// Reader
readLock.lock();
try { /* read shared data */ } finally { readLock.unlock(); }

// Writer
writeLock.lock();
try { /* modify */ } finally { writeLock.unlock(); }
```

### Fork/Join pattern
**Divide‑and‑conquer parallelism** (Java 7+). Use `ForkJoinPool` and `RecursiveTask`.

```java
class SumTask extends RecursiveTask<Long> {
    private final int[] array; private final int lo, hi;
    protected Long compute() {
        if (hi - lo < THRESHOLD) {
            long sum = 0; for (int i = lo; i < hi; i++) sum += array[i]; return sum;
        } else {
            int mid = (lo + hi) / 2;
            SumTask left = new SumTask(array, lo, mid);
            SumTask right = new SumTask(array, mid, hi);
            left.fork();
            long rightResult = right.compute();
            long leftResult = left.join();
            return leftResult + rightResult;
        }
    }
}
```

### Actor model (Akka)
**Treat “actors” as the universal primitives of concurrency.** Each actor processes messages sequentially, and they communicate asynchronously.

```java
// Akka example
public class MyActor extends AbstractActor {
    public Receive createReceive() {
        return receiveBuilder()
            .match(String.class, msg -> System.out.println("Received: " + msg))
            .build();
    }
}
// actorRef.tell("Hello", ActorRef.noSender());
```

### Reactive concurrency patterns
**Non‑blocking, event‑driven, and backpressure‑aware.** Examples: Project Reactor, RxJava.

```java
Flux.range(1, 100)
    .map(i -> i * 2)
    .subscribe(System.out::println);
```

### Structured concurrency
**Treat multiple tasks running in different threads as a single unit of work.** Java 19+ (preview) with `StructuredTaskScope`.

```java
try (var scope = new StructuredTaskScope.ShutdownOnFailure()) {
    Future<String> user = scope.fork(() -> fetchUser());
    Future<Integer> order = scope.fork(() -> fetchOrder());
    scope.join();            // wait for both
    scope.throwIfFailed();   // propagate errors
    return new Response(user.resultNow(), order.resultNow());
}
```

---

## 6. Functional Programming Patterns (Java & JVM)

### Pure functions
**Functions that always return the same output for the same input and have no side effects.**

```java
// Pure
int add(int a, int b) { return a + b; }

// Impure (uses external state)
int counter = 0;
int nextId() { return counter++; }
```

### Higher‑order functions
**Functions that take other functions as parameters or return a function.** Java 8+ streams and lambdas.

```java
List<Integer> numbers = Arrays.asList(1,2,3);
numbers.stream().map(x -> x * 2).forEach(System.out::println); // map is higher-order
```

### Function composition
**Combine functions to create new functions.** Using `andThen` or `compose`.

```java
Function<Integer, Integer> multiply2 = x -> x * 2;
Function<Integer, Integer> add1 = x -> x + 1;
Function<Integer, Integer> composed = multiply2.andThen(add1); // (x*2)+1
System.out.println(composed.apply(5)); // 11
```

### Currying
**Transform a function of multiple arguments into a sequence of functions each taking a single argument.** Java doesn’t have built‑in currying but can be simulated.

```java
Function<Integer, Function<Integer, Integer>> add = x -> y -> x + y;
Function<Integer, Integer> add5 = add.apply(5);
System.out.println(add5.apply(3)); // 8
```

### Lazy evaluation
**Defer computation until the result is needed.** Java streams are lazily evaluated.

```java
Stream.of(1,2,3,4)
    .filter(x -> { System.out.println("filter " + x); return x % 2 == 0; })
    .map(x -> { System.out.println("map " + x); return x * 2; })
    .findFirst(); // only processes until first match
```

### Streams API patterns
**Map/filter/reduce:** Transform, filter, and aggregate data.

```java
int sum = list.stream()
    .filter(n -> n > 0)
    .mapToInt(n -> n * 2)
    .sum();
```

**Collectors design:** Accumulate stream elements into collections or other results.

```java
Map<Boolean, List<Integer>> partitioned = list.stream()
    .collect(Collectors.partitioningBy(n -> n > 0));
String joined = list.stream().map(String::valueOf).collect(Collectors.joining(", "));
```

### Optional usage patterns
**Avoid null by using `Optional` for optional values.**

```java
Optional<String> name = Optional.ofNullable(getName());
String result = name.orElse("default");
name.ifPresent(System.out::println);
// Avoid using Optional.get() without checking
```

### Monads (Optional, Stream)
**Monads provide a way to chain operations while handling context (like absence, multiple values).** `flatMap` is key.

```java
Optional<Integer> opt = Optional.of(2);
Optional<Integer> result = opt.flatMap(x -> Optional.of(x * 3)); // flatMap avoids nested Optional

Stream.of("a", "b")
    .flatMap(s -> s.chars().mapToObj(c -> (char) c)) // flattens each string into stream of chars
    .forEach(System.out::println);
```

### Immutability in functional design
**Use immutable data structures and avoid side effects.** Example: using `List.copyOf()`.

```java
List<String> original = new ArrayList<>();
List<String> immutable = List.copyOf(original); // immutable view
```

---

## 7. Java API Design Best Practices

### Designing fluent APIs
**Methods return `this` to allow chaining.** Example: builder pattern.

```java
class Mailer {
    public Mailer from(String addr) { ... return this; }
    public Mailer to(String addr) { ... return this; }
    public void send() { ... }
}
// usage: new Mailer().from("a@b.com").to("c@d.com").send();
```

### Method naming conventions
**Consistent, descriptive names.** Use verbs for actions, get/set for accessors, `is` for booleans.

### Overloading vs overriding
**Overloading:** same name, different parameters within the same class. **Overriding:** subclass redefines a superclass method.

### Defensive copying
**Make copies of mutable objects passed to or returned from methods to preserve encapsulation.**

```java
public class Person {
    private final Date birthDate; // Date is mutable!
    public Person(Date birthDate) {
        this.birthDate = new Date(birthDate.getTime()); // defensive copy
    }
    public Date getBirthDate() {
        return new Date(birthDate.getTime()); // defensive copy on return
    }
}
```

### Null handling strategies
**Avoid returning null; use `Optional` or empty collections.** Validate parameters with `Objects.requireNonNull`.

```java
public List<String> getNames() {
    return Collections.emptyList(); // instead of null
}
public void setName(String name) {
    this.name = Objects.requireNonNull(name, "name must not be null");
}
```

### Optional vs exceptions
**Use `Optional` for potentially absent values that are not errors; use exceptions for exceptional conditions.**

### Checked vs unchecked exceptions
**Checked exceptions** should be used for recoverable conditions (caller must handle). **Unchecked** for programming errors (e.g., `IllegalArgumentException`).

### API versioning strategies
**Use semantic versioning.** Backward‑compatible changes increment minor version; breaking changes increment major. Use package renaming or `@deprecated`.

### Backward compatibility
**Ensure new versions can work with clients built against older versions.** Avoid removing public methods; add new ones instead.

### Binary compatibility
**Changes that do not require recompilation of client code.** Adding methods to interfaces (with default methods) is binary compatible in Java.

### SPI (Service Provider Interface)
**Allow third‑party implementations to be plugged in.** Use `ServiceLoader`.

```java
// Define SPI interface
public interface MyService { void execute(); }

// In provider JAR, create file META-INF/services/com.example.MyService containing impl class name
// Client loads:
ServiceLoader<MyService> loader = ServiceLoader.load(MyService.class);
for (MyService service : loader) { service.execute(); }
```

### Builder vs telescoping constructors
**Use builder pattern for many optional parameters instead of multiple constructors.**

### Avoiding side effects
**Methods should not modify input parameters or global state.**

---

## 8. Collections Framework Best Practices

### Choosing correct collection type
- **List:** ordered, allows duplicates. Choose `ArrayList` for random access, `LinkedList` for frequent insert/delete.
- **Set:** no duplicates. `HashSet` for fast lookup, `TreeSet` for sorted, `LinkedHashSet` for insertion order.
- **Map:** key‑value pairs. `HashMap`, `LinkedHashMap`, `TreeMap` similarly.

### HashMap internals and tuning
**HashMap uses an array of buckets, each a linked list or tree (Java 8+).** Initial capacity and load factor affect performance. Default load factor 0.75. For large maps, pre‑size to avoid rehashing.

### Concurrent collections usage
**Use `ConcurrentHashMap` instead of synchronized maps.** `CopyOnWriteArrayList` for read‑heavy lists.

```java
ConcurrentHashMap<String, Integer> map = new ConcurrentHashMap<>();
map.putIfAbsent("key", 1);
map.computeIfAbsent("key2", k -> 2);
```

### Immutable collections
**Java 9+ factory methods: `List.of()`, `Set.of()`, `Map.of()`** produce immutable collections.

```java
List<String> immutable = List.of("a", "b");
// immutable.add("c"); // throws UnsupportedOperationException
```

### Iteration best practices
**Use enhanced for‑loop or `forEach` with lambda.** Avoid modifying a collection while iterating (use `Iterator.remove()` or concurrent collections).

```java
list.removeIf(s -> s.length() == 0); // safe removal
```

### equals() and hashCode() contract
**If you override `equals`, you must override `hashCode` so that equal objects have equal hash codes.** Important for hash‑based collections.

```java
@Override
public boolean equals(Object o) { ... }
@Override
public int hashCode() { return Objects.hash(field1, field2); }
```

### Comparator vs Comparable
**`Comparable` defines natural ordering inside the class. `Comparator` is an external ordering strategy.**

```java
class Person implements Comparable<Person> {
    private String name;
    public int compareTo(Person p) { return name.compareTo(p.name); }
}
Comparator<Person> byAge = Comparator.comparingInt(Person::getAge);
```

### Avoiding ConcurrentModificationException
**Do not modify a collection while iterating over it (unless using iterator’s own remove).** Use concurrent collections or copy.

```java
// Wrong:
for (String s : list) { if (s.isEmpty()) list.remove(s); }
// Correct:
list.removeIf(String::isEmpty);
```

### Memory and performance considerations
**Prefer `ArrayList` over `LinkedList` for most cases. Use primitive collections (e.g., Eclipse Collections) for large data. Be mindful of autoboxing.**

---

## 9. Exception Handling Best Practices

### Exception hierarchy design
**Extend `Exception` for checked, `RuntimeException` for unchecked.** Create custom exceptions for domain‑specific errors.

```java
class InsufficientFundsException extends Exception { ... }
```

### Custom exceptions
**Provide constructors that accept a message and cause.** Include details useful for debugging.

### Fail‑fast vs fail‑safe
**Fail‑fast:** throw exception as soon as an error is detected (e.g., invalid argument). **Fail‑safe:** continue execution but log/recover.

### Logging vs rethrowing
**Log at the boundary where the exception is caught, not at every level.** Rethrow if the current layer cannot handle it.

### Wrapping exceptions
**Wrap low‑level exceptions (like SQLException) into domain‑specific unchecked exceptions to avoid leaking implementation details.**

```java
try { ... } catch (SQLException e) {
    throw new DataAccessException("Failed to save user", e);
}
```

### Avoiding exception swallowing
**Never catch an exception and do nothing.** At least log it.

### Resource cleanup (try‑with‑resources)
**Ensure resources like streams, connections are closed automatically.**

```java
try (FileInputStream fis = new FileInputStream("file.txt")) {
    // use fis
} catch (IOException e) { ... }
```

### Idempotency in error handling
**When retrying operations, ensure the operation is idempotent to avoid duplicate side effects.**

---

## 10. Logging & Observability Best Practices

### Structured logging
**Log in a machine‑parseable format (e.g., JSON) to enable easy searching and analysis.** Use libraries like Logstash Logback Encoder.

### Log levels strategy
- **ERROR:** critical failures that need immediate attention.
- **WARN:** potential issues that don’t stop the app.
- **INFO:** important business events.
- **DEBUG:** detailed information for development.
- **TRACE:** finer‑grained.

### Correlation IDs
**Generate a unique ID per request/transaction and include it in all log entries to trace flow across services.** Use MDC (Mapped Diagnostic Context).

```java
MDC.put("correlationId", UUID.randomUUID().toString());
try { ... } finally { MDC.clear(); }
```

### Distributed tracing integration
**Use tools like Zipkin, Jaeger. Integrate with Spring Cloud Sleuth, OpenTelemetry.**

### Metrics collection patterns
**Counters, gauges, histograms (Micrometer, Dropwizard Metrics).** Expose via endpoints (e.g., Prometheus).

### AOP‑based logging
**Use Aspect‑Oriented Programming to log method entry/exit, performance metrics without cluttering business code.**

```java
@Around("@annotation(LogExecutionTime)")
public Object logExecutionTime(ProceedingJoinPoint joinPoint) throws Throwable { ... }
```

### Avoiding excessive logging
**Do not log in tight loops; use rate limiting or conditional logging.** Be mindful of performance impact.

### Performance impact of logging
**Asynchronous appenders (e.g., Logback’s `AsyncAppender`) can reduce latency.**

---

## 11. JVM Memory & Performance Patterns

### Object allocation patterns
**Avoid creating unnecessary objects.** Reuse mutable objects where possible. Use primitives instead of wrappers.

### Escape analysis
**JVM can allocate objects on the stack instead of heap if they don’t escape the method.** This reduces GC pressure.

### Garbage collection tuning patterns
**Choose the right GC (G1, ZGC) based on application needs.** Tune heap size, young generation ratio.

### Heap vs stack allocation
**Local variables and method calls go on stack; objects on heap.** Stack allocation is faster but limited.

### Object pooling trade‑offs
**Pooling expensive objects (connections) is beneficial; pooling simple objects may hurt due to GC and complexity.**

### Caching strategies
**Use caching for expensive computations. Be mindful of memory limits and eviction policies.**

### Lazy initialization
**Defer object creation until needed.** Can improve startup time but may introduce latency later.

```java
private volatile HeavyObject heavy;
public HeavyObject getHeavy() {
    if (heavy == null) {
        synchronized (this) {
            if (heavy == null) {
                heavy = new HeavyObject();
            }
        }
    }
    return heavy;
}
```

### Memory leaks detection patterns
**Common causes: static collections, unclosed resources, listeners not removed, inner classes holding outer references.** Use profilers and heap dumps.

### Off‑heap memory usage
**Use `ByteBuffer.allocateDirect()` for large buffers to avoid GC overhead.** But manage carefully to avoid native memory leaks.

---

## 12. I/O and Resource Management Patterns

### Stream handling best practices
**Always close streams. Use try‑with‑resources.** Buffer I/O for performance.

```java
try (BufferedReader reader = Files.newBufferedReader(Paths.get("file.txt"))) {
    // read
}
```

### Buffering strategies
**Use buffered streams to reduce system calls.** Choose appropriate buffer size (e.g., 8KB).

### NIO vs IO usage
**NIO (non‑blocking) is better for high concurrency (selectors, channels).** Classic IO is simpler for low‑level file operations.

### Reactive I/O
**Non‑blocking I/O with backpressure (Project Reactor, Akka Streams).** Example: Spring WebFlux.

```java
Mono.fromCallable(() -> readFile())
    .subscribeOn(Schedulers.boundedElastic())
    .subscribe(data -> ...);
```

### Backpressure handling
**In reactive streams, control the rate of data emission to prevent overwhelming consumers.** Use `onBackpressureBuffer`, `onBackpressureDrop`.

### Resource lifecycle management
**Use `@PreDestroy` or `DisposableBean` for cleanup in Spring.** Implement `AutoCloseable`.

### File handling patterns
**Use `Files` utility class for common operations.** Be careful with file permissions and symlinks.

---

## 13. Serialization & Data Transformation Patterns

### Java serialization pitfalls
**Java’s built‑in serialization is slow, insecure, and fragile.** Avoid it; use JSON, Protobuf, etc.

### Externalizable vs Serializable
**`Externalizable` gives control over serialization (custom write/read methods) but is more complex.** Use only if performance critical.

### JSON/XML mapping patterns
**Use Jackson or JAXB.** Annotate POJOs to control mapping.

```java
public class User {
    @JsonProperty("user_name")
    private String name;
    @JsonIgnore
    private String password;
}
```

### DTO vs Entity separation
**DTOs (Data Transfer Objects) are used for API boundaries; entities for persistence.** Never expose entities directly in APIs.

### Schema evolution strategies
**Design formats that support adding fields without breaking compatibility (e.g., Protobuf, Avro).** Use default values.

### Version tolerance
**Include a version field in serialized data to handle different versions.** Use tolerant reader pattern.

### Protobuf/Avro patterns
**Use schema registries, manage compatibility.** Code generation from schema.

---

## 14. Persistence & Data Access Patterns

### DAO pattern
**Data Access Object abstracts the persistence mechanism.** Typically one DAO per entity.

```java
interface UserDao {
    User findById(Long id);
    void save(User user);
}
```

### Repository pattern
**Similar to DAO but operates from a domain perspective, often returning domain objects.** Spring Data repositories.

```java
public interface UserRepository extends JpaRepository<User, Long> {
    List<User> findByLastName(String lastName);
}
```

### Unit of Work
**Tracks changes to objects during a transaction and flushes them at the end.** JPA’s `EntityManager` implements this.

### Identity Map
**Ensures that each database row is loaded only once per transaction, returning the same object instance.** JPA’s first‑level cache.

### Lazy vs eager loading
**Lazy loading defers loading of related data until accessed.** Use eager when you know you’ll need it, but beware of N+1.

### Transaction management
**Declarative with `@Transactional` (Spring).** Understand propagation and isolation levels.

### Optimistic vs pessimistic locking
**Optimistic:** version column, check on update. **Pessimistic:** lock rows during read (`SELECT ... FOR UPDATE`).

### Caching (1st vs 2nd level)
**1st level cache is per‑session (EntityManager).** 2nd level cache is shared across sessions (Hibernate cache).

### ORM pitfalls (N+1 problem)
**Occurs when you fetch a list and then access lazy associations, causing many queries.** Fix with joins (`@EntityGraph`, fetch join).

```java
@Query("SELECT u FROM User u JOIN FETCH u.orders")
List<User> findAllWithOrders();
```

---

## 15. Enterprise & Architectural Patterns

### Layered architecture
**Presentation, Business, Persistence layers.** Each layer has distinct responsibility.

### Hexagonal architecture
**Ports and adapters: core domain isolated from external concerns (UI, DB).** Dependencies point inward.

### Clean architecture
**Similar to hexagonal, with concentric circles: Entities, Use Cases, Interface Adapters, Frameworks.**

### CQRS
**Command Query Responsibility Segregation – separate read and write models.** Often used with Event Sourcing.

### Event sourcing
**Store state changes as a sequence of events, rebuild current state by replaying events.**

### Saga pattern
**Manage distributed transactions via a sequence of local transactions with compensating actions.** Orchestration or choreography.

### Circuit breaker
**Prevent repeated calls to a failing service.** Implementation: Resilience4j, Hystrix.

```java
CircuitBreaker circuitBreaker = CircuitBreaker.ofDefaults("backend");
Supplier<String> decorated = circuitBreaker.decorateSupplier(() -> riskyCall());
```

### Bulkhead pattern
**Isolate failures by limiting resources per service/component (e.g., thread pools).**

### Retry pattern
**Automatically retry transient failures.** Use with exponential backoff.

### API Gateway pattern
**Single entry point for clients, routing requests to appropriate microservices.** Can handle authentication, rate limiting.

### Service discovery
**Clients find service instances dynamically via a registry (Eureka, Consul).**

---

## 16. Reactive & Asynchronous Patterns

### CompletableFuture patterns
**Combine, compose, and handle asynchronous computations.**

```java
CompletableFuture.supplyAsync(() -> fetchData())
    .thenApply(data -> transform(data))
    .thenAccept(System.out::println)
    .exceptionally(ex -> { log.error("Error", ex); return null; });
```

### Reactive streams
**Specification for asynchronous stream processing with non‑blocking backpressure.** Implementations: Project Reactor, RxJava.

### Publisher‑Subscriber model
**Reactive types like `Flux` (many) and `Mono` (0‑1).** Subscribe to receive data.

### Backpressure strategies
**Control data flow: `onBackpressureBuffer()`, `onBackpressureDrop()`, `onBackpressureLatest()`.**

### Non‑blocking I/O
**Threads are not blocked waiting for I/O; instead, callbacks or async APIs are used.**

### Event loop model
**Single thread handles multiple connections (e.g., Netty, Node.js).** Tasks are submitted to the event loop.

### Async error handling
**Use `onErrorResume`, `onErrorReturn` in reactive streams. For `CompletableFuture`, handle with `exceptionally`.**

### Timeout and retry patterns
**Apply timeouts to async operations. Use `timeout()` in Reactor, `orTimeout()` in CompletableFuture. Retry with backoff.**

---

## 17. Security Best Practices & Patterns

### Secure coding practices
**Validate inputs, avoid SQL injection (use prepared statements), escape outputs, use secure libraries.**

### Input validation patterns
**Validate on both client and server. Use whitelisting, length checks, type checks.** Bean Validation (`@NotNull`, `@Size`).

### Authentication vs authorization
**Authentication = who you are. Authorization = what you can do.** Use Spring Security.

### OAuth2 / JWT patterns
**Token‑based authentication. JWT contains claims.** Use libraries like Nimbus, JJWT.

```java
String token = Jwts.builder().setSubject(user).signWith(key).compact();
Jwts.parserBuilder().setSigningKey(key).build().parseClaimsJws(token);
```

### Encryption patterns
**Use strong algorithms (AES, RSA).** Never roll your own crypto. Use key management.

### Secrets management
**Store secrets in environment variables, vaults (Hashicorp Vault), or Kubernetes secrets.** Not in code.

### CSRF/XSS prevention
**CSRF tokens, SameSite cookies, output encoding, Content Security Policy.**

### Secure deserialization
**Avoid deserializing untrusted data. Use safe formats like JSON. If using Java serialization, use a whitelist.**

### Least privilege principle
**Grant minimum necessary permissions to users, processes, and services.**

---

## 18. Testing Patterns & Best Practices

### Unit testing patterns
**AAA (Arrange‑Act‑Assert)** – set up, execute, verify.

```java
@Test
void shouldCalculateTotal() {
    // Arrange
    Cart cart = new Cart();
    cart.addItem(new Item(10));
    // Act
    double total = cart.total();
    // Assert
    assertEquals(10, total);
}
```

### Test data builders
**Simplify creation of test objects with builders, avoiding many constructors.**

### Mocking vs stubbing
**Mocking** verifies interactions; **stubbing** returns predefined data. Use Mockito.

```java
when(repo.findById(1L)).thenReturn(Optional.of(user));
verify(repo, times(1)).save(any());
```

### Integration testing patterns
**Test modules together, using real databases or testcontainers.** SpringBootTest.

### Contract testing
**Verify that services adhere to a contract (consumer‑driven).** Tools: Pact, Spring Cloud Contract.

### Testcontainers usage
**Spin up real Docker containers for databases, message brokers in tests.**

```java
@Testcontainers
class MyTest {
    @Container
    static PostgreSQLContainer<?> postgres = new PostgreSQLContainer<>("postgres:15");
}
```

### Property‑based testing
**Generate many random inputs to test invariants.** Tools: jqwik, QuickCheck.

### Performance testing patterns
**Use JMeter, Gatling to simulate load.** Monitor response times, throughput.

### Test isolation
**Each test should run independently, with no shared state.** Use fresh test data.

---

## 19. Microservices Design Patterns (Java Ecosystem)

### Service decomposition strategies
**Decompose by business capability or subdomain (DDD).**

### Inter‑service communication patterns
**Synchronous (REST, gRPC) or asynchronous (messaging).** Use HTTP/2, circuit breakers.

### Data consistency patterns
**Saga, event sourcing, CQRS.** Avoid distributed transactions.

### API versioning
**URI versioning (`/v1/orders`), content negotiation, or header versioning.**

### Distributed transactions
**Avoid two‑phase commit; use Sagas with compensating actions.**

### Resilience patterns
**Retry, circuit breaker, bulkhead, timeout.** Implement with Resilience4j.

### Observability patterns
**Log aggregation, metrics, distributed tracing.** Use ELK, Prometheus, Jaeger.

### Service mesh patterns
**Sidecar proxies (Envoy) handle service discovery, load balancing, security.** Istio, Linkerd.

---

## 20. Framework‑Specific Best Practices (JVM Ecosystem)

### Spring Framework patterns
**Bean lifecycle:** `@PostConstruct`, `@PreDestroy`, `InitializingBean`, `DisposableBean`.  
**Dependency injection:** Constructor injection preferred.  
**AOP patterns:** Use for cross‑cutting concerns (logging, transactions).

### Spring Boot conventions
**Auto‑configuration, starter dependencies, `application.yml`.** Use `@ConfigurationProperties` for external config.

### Hibernate best practices
**Use fetch joins to avoid N+1, prefer `@ManyToOne(fetch = LAZY)` except when needed, use batch fetching.**

### Akka patterns
**Actor lifecycle, supervision strategies, clustering.** Use for high‑concurrency systems.

### Micronaut / Quarkus optimizations
**GraalVM native image support, compile‑time DI, low memory footprint.**

### Jakarta EE patterns
**CDI, JAX‑RS, EJB.** Use modern specs like Jakarta EE 9+.

---

## 21. Code Quality & Maintainability Practices

### Clean code principles
**Meaningful names, small functions, no duplication, comments only when necessary.**

### Code smells and refactoring
**Long methods, large classes, feature envy, shotgun surgery.** Refactor continuously.

### DRY vs over‑abstraction
**Don’t Repeat Yourself, but avoid premature abstraction.** Balance.

### KISS principle
**Keep It Simple, Stupid.** Prefer simple solutions over complex ones.

### YAGNI principle
**You Ain’t Gonna Need It.** Don’t add functionality until necessary.

### Static analysis usage
**Tools like SonarQube, Checkstyle, PMF, SpotBugs** to catch issues early.

### Code review best practices
**Focus on design, readability, and correctness. Be constructive.**

### Documentation patterns
**Document why, not what. Use Javadoc for public APIs. Keep docs up‑to‑date.**

---

## 22. Distributed Systems Patterns (Java Context)

### CAP theorem implications
**Consistency, Availability, Partition tolerance – pick two.** In distributed systems, partition tolerance is a must, so choose between consistency and availability.

### Consistency models
**Strong, eventual, causal consistency.** Choose based on requirements.

### Leader election
**Use algorithms like Paxos, Raft, or libraries like Apache ZooKeeper, etcd.**

### Gossip protocol
**Efficiently disseminate information in a cluster (e.g., Cassandra, Redis Cluster).**

### Idempotency patterns
**Design operations so that repeating them has the same effect as one.** Use idempotency keys.

### Rate limiting
**Control request rates per client (token bucket, leaky bucket).** Implement with Guava RateLimiter or Redis.

### Load balancing
**Client‑side (Ribbon) or server‑side (NGINX).** Use consistent hashing for sticky sessions if needed.

### Fault tolerance patterns
**Retry, circuit breaker, failover, graceful degradation.**

---

## 23. Caching Patterns

### Cache‑aside
**Application reads from cache; if not present, reads from DB and updates cache.**

```java
public User getUser(String id) {
    User user = cache.get(id);
    if (user == null) {
        user = db.find(id);
        cache.put(id, user);
    }
    return user;
}
```

### Write‑through / write‑behind
**Write‑through:** data written to cache and DB synchronously. **Write‑behind:** write to cache, then asynchronously to DB.

### Read‑through cache
**Cache itself loads from DB on miss (e.g., using `CacheLoader` in Caffeine).**

### Distributed caching
**Use Redis, Hazelcast, Infinispan for shared cache across instances.**

### Cache invalidation strategies
**Time‑to‑live (TTL), time‑to‑idle (TTI), explicit eviction on update.** Use cache events or messaging.

### TTL strategies
**Set appropriate expiration times based on data volatility.**

### Near cache vs remote cache
**Near cache is local to the application (fast) + remote cache (consistent).** Combine for performance.

---

## 24. Messaging & Integration Patterns

### Message queue patterns
**Point‑to‑point (queues) vs publish‑subscribe (topics).** Use JMS, RabbitMQ, Kafka.

### Pub/Sub systems
**Producers publish messages to topics; multiple consumers receive them.**

### Event‑driven architecture
**Components communicate via events.** Loose coupling, scalability.

### Idempotent consumer
**Ensure processing a message twice has the same effect as once.** Use deduplication (e.g., message ID in DB).

### Exactly‑once vs at‑least‑once
**Exactly‑once is hardest; often at‑least‑once with idempotent processing.** Kafka provides exactly‑once semantics with transactions.

### Dead letter queues
**Messages that cannot be processed are moved to a DLQ for later analysis.**

### Message ordering
**Partitioning in Kafka preserves order within a partition.** Use sequence numbers.

### Schema evolution
**Use Avro, Protobuf with schema registry to evolve messages compatibly.**

---

## 25. JVM Language Interoperability Best Practices

### Java‑Kotlin interoperability
**Kotlin compiles to bytecode, can call Java and vice versa.** Be mindful of null‑safety (Kotlin requires annotations for nullability).

### Java‑Scala interoperability
**Scala also runs on JVM; Java can call Scala, but Scala’s features (e.g., implicits) may require adaptation.**

### Null‑safety differences
**Kotlin has built‑in null safety; Java does not.** Use `@Nullable`/`@NotNull` annotations for better interop.

### Functional vs OOP style differences
**Java added lambdas and streams; Scala/Kotlin have richer functional features.** Choose the right style for the team.

### Interop performance considerations
**Calling Kotlin/Scala from Java has negligible overhead.** However, Scala’s collections are different; use Java collections at boundaries.

### Mixed codebase patterns
**Gradually migrate by writing new modules in Kotlin. Use common libraries. Keep a consistent coding style.**

---


