## Creational Patterns

### Singleton
**Explanation:** Ensures a class has only one instance and provides a global point of access to it. Useful for shared resources like configuration managers, thread pools, or database connections.

**Real-world Example:** A configuration manager that loads settings from a file once and provides them to all parts of an application.

```java
public class ConfigurationManager {
    private static ConfigurationManager instance;
    private Properties config;

    private ConfigurationManager() {
        config = new Properties();
        try (InputStream input = new FileInputStream("config.properties")) {
            config.load(input);
        } catch (IOException e) {
            e.printStackTrace();
        }
    }

    public static ConfigurationManager getInstance() {
        if (instance == null) {
            instance = new ConfigurationManager();
        }
        return instance;
    }

    public String getConfigValue(String key) {
        return config.getProperty(key);
    }
}
```

### Factory Method
**Explanation:** Defines an interface for creating an object, but lets subclasses decide which class to instantiate. The creation logic is deferred to subclasses.

**Real-world Example:** A logistics application where different types of transport (truck, ship) can be created by subclasses of a `Logistics` class.

```java
interface Transport {
    void deliver();
}

class Truck implements Transport {
    public void deliver() { System.out.println("Deliver by land in a box."); }
}

class Ship implements Transport {
    public void deliver() { System.out.println("Deliver by sea in a container."); }
}

abstract class Logistics {
    abstract Transport createTransport();
    
    public void planDelivery() {
        Transport t = createTransport();
        t.deliver();
    }
}

class RoadLogistics extends Logistics {
    Transport createTransport() { return new Truck(); }
}

class SeaLogistics extends Logistics {
    Transport createTransport() { return new Ship(); }
}
```

### Abstract Factory
**Explanation:** Provides an interface for creating families of related or dependent objects without specifying their concrete classes. It’s like a factory of factories.

**Real-world Example:** A UI library that creates different styles of buttons, checkboxes, and scroll bars for Windows, Mac, or Linux.

```java
interface Button { void paint(); }
interface Checkbox { void paint(); }

class WinButton implements Button { public void paint() { System.out.println("Windows button"); } }
class MacButton implements Button { public void paint() { System.out.println("Mac button"); } }
class WinCheckbox implements Checkbox { public void paint() { System.out.println("Windows checkbox"); } }
class MacCheckbox implements Checkbox { public void paint() { System.out.println("Mac checkbox"); } }

interface GUIFactory {
    Button createButton();
    Checkbox createCheckbox();
}

class WinFactory implements GUIFactory {
    public Button createButton() { return new WinButton(); }
    public Checkbox createCheckbox() { return new WinCheckbox(); }
}

class MacFactory implements GUIFactory {
    public Button createButton() { return new MacButton(); }
    public Checkbox createCheckbox() { return new MacCheckbox(); }
}

class Application {
    private Button button;
    private Checkbox checkbox;

    public Application(GUIFactory factory) {
        button = factory.createButton();
        checkbox = factory.createCheckbox();
    }

    public void paint() {
        button.paint();
        checkbox.paint();
    }
}
```

### Builder
**Explanation:** Separates the construction of a complex object from its representation so that the same construction process can create different representations. Useful when an object requires many optional parameters.

**Real-world Example:** Building a custom computer with optional components (RAM, HDD, graphics card).

```java
class Computer {
    private String CPU;
    private String RAM;
    private String storage;
    private boolean graphicsCardEnabled;
    private boolean bluetoothEnabled;

    private Computer(Builder builder) {
        this.CPU = builder.CPU;
        this.RAM = builder.RAM;
        this.storage = builder.storage;
        this.graphicsCardEnabled = builder.graphicsCardEnabled;
        this.bluetoothEnabled = builder.bluetoothEnabled;
    }

    public static class Builder {
        private String CPU;
        private String RAM;
        private String storage;
        private boolean graphicsCardEnabled;
        private boolean bluetoothEnabled;

        public Builder(String CPU, String RAM) {
            this.CPU = CPU;
            this.RAM = RAM;
        }

        public Builder setStorage(String storage) { this.storage = storage; return this; }
        public Builder setGraphicsCardEnabled(boolean value) { this.graphicsCardEnabled = value; return this; }
        public Builder setBluetoothEnabled(boolean value) { this.bluetoothEnabled = value; return this; }
        public Computer build() { return new Computer(this); }
    }
}
```

### Prototype
**Explanation:** Creates new objects by copying an existing object (prototype) rather than calling a constructor. Useful when object creation is expensive or complex.

**Real-world Example:** Cloning a graphical shape (like a circle or rectangle) with all its properties to create variations.

```java
interface Shape extends Cloneable {
    Shape clone();
    void draw();
}

class Circle implements Shape {
    private int radius;
    private String color;

    public Circle(int radius, String color) {
        this.radius = radius;
        this.color = color;
    }

    public Shape clone() {
        try {
            return (Circle) super.clone();
        } catch (CloneNotSupportedException e) {
            return null;
        }
    }

    public void draw() { System.out.println("Drawing a " + color + " circle with radius " + radius); }
}

// Usage
Circle original = new Circle(10, "red");
Circle copy = (Circle) original.clone();
copy.draw();
```

---

## Structural Patterns

### Adapter
**Explanation:** Converts the interface of a class into another interface that clients expect. Allows classes with incompatible interfaces to work together.

**Real-world Example:** Adapting a legacy XML-based library to work with a new JSON-based interface.

```java
// Target interface expected by client
interface JsonProcessor {
    void processJson(String json);
}

// Adaptee (legacy class)
class XmlProcessor {
    void processXml(String xml) {
        System.out.println("Processing XML: " + xml);
    }
}

// Adapter
class XmlToJsonAdapter implements JsonProcessor {
    private XmlProcessor xmlProcessor;

    public XmlToJsonAdapter(XmlProcessor xmlProcessor) {
        this.xmlProcessor = xmlProcessor;
    }

    public void processJson(String json) {
        // Convert JSON to XML (simplified)
        String xml = convertJsonToXml(json);
        xmlProcessor.processXml(xml);
    }

    private String convertJsonToXml(String json) {
        return "<converted>" + json + "</converted>"; // dummy conversion
    }
}
```

### Bridge
**Explanation:** Decouples an abstraction from its implementation so that both can vary independently. Useful when you have multiple dimensions of variation.

**Real-world Example:** Separating a remote control (abstraction) from the device it controls (implementation). You can have different remotes (basic, advanced) and different devices (TV, radio).

```java
// Implementor
interface Device {
    void turnOn();
    void turnOff();
    void setVolume(int percent);
}

// Concrete Implementors
class TV implements Device {
    public void turnOn() { System.out.println("TV on"); }
    public void turnOff() { System.out.println("TV off"); }
    public void setVolume(int percent) { System.out.println("TV volume set to " + percent); }
}

class Radio implements Device {
    public void turnOn() { System.out.println("Radio on"); }
    public void turnOff() { System.out.println("Radio off"); }
    public void setVolume(int percent) { System.out.println("Radio volume set to " + percent); }
}

// Abstraction
abstract class RemoteControl {
    protected Device device;
    public RemoteControl(Device device) { this.device = device; }
    abstract void togglePower();
}

// Refined Abstraction
class BasicRemote extends RemoteControl {
    private boolean isOn = false;
    public BasicRemote(Device device) { super(device); }
    void togglePower() {
        if (isOn) { device.turnOff(); isOn = false; }
        else { device.turnOn(); isOn = true; }
    }
}
```

### Composite
**Explanation:** Composes objects into tree structures to represent part-whole hierarchies. Clients can treat individual objects and compositions uniformly.

**Real-world Example:** A graphics application where you can draw simple shapes (circle, square) or groups of shapes (which are also shapes).

```java
interface Graphic {
    void draw();
}

class Circle implements Graphic {
    public void draw() { System.out.println("Drawing a circle"); }
}

class Square implements Graphic {
    public void draw() { System.out.println("Drawing a square"); }
}

class CompositeGraphic implements Graphic {
    private List<Graphic> children = new ArrayList<>();

    public void add(Graphic graphic) { children.add(graphic); }
    public void remove(Graphic graphic) { children.remove(graphic); }

    public void draw() {
        for (Graphic child : children) {
            child.draw();
        }
    }
}
```

### Decorator
**Explanation:** Attaches additional responsibilities to an object dynamically. Provides a flexible alternative to subclassing for extending functionality.

**Real-world Example:** Adding features like scroll bars or borders to a text view in a GUI.

```java
interface TextView {
    void draw();
}

class SimpleTextView implements TextView {
    public void draw() { System.out.println("Drawing text"); }
}

abstract class TextViewDecorator implements TextView {
    protected TextView decoratedView;
    public TextViewDecorator(TextView view) { this.decoratedView = view; }
    public void draw() { decoratedView.draw(); }
}

class ScrollBarDecorator extends TextViewDecorator {
    public ScrollBarDecorator(TextView view) { super(view); }
    public void draw() {
        super.draw();
        addScrollBar();
    }
    private void addScrollBar() { System.out.println("Adding scroll bar"); }
}

class BorderDecorator extends TextViewDecorator {
    public BorderDecorator(TextView view) { super(view); }
    public void draw() {
        super.draw();
        addBorder();
    }
    private void addBorder() { System.out.println("Adding border"); }
}
```

### Facade
**Explanation:** Provides a simplified, unified interface to a set of interfaces in a subsystem. Hides the complexity of the subsystem.

**Real-world Example:** A computer startup facade that hides the complex interactions between CPU, memory, and hard drive.

```java
class CPU {
    void start() { System.out.println("CPU starting"); }
    void execute() { System.out.println("CPU executing"); }
}

class Memory {
    void load() { System.out.println("Loading data into memory"); }
}

class HardDrive {
    void read() { System.out.println("Reading data from hard drive"); }
}

class ComputerFacade {
    private CPU cpu;
    private Memory memory;
    private HardDrive hardDrive;

    public ComputerFacade() {
        this.cpu = new CPU();
        this.memory = new Memory();
        this.hardDrive = new HardDrive();
    }

    public void start() {
        cpu.start();
        memory.load();
        hardDrive.read();
        cpu.execute();
    }
}
```

### Flyweight
**Explanation:** Uses sharing to support large numbers of fine-grained objects efficiently. Reduces memory usage by sharing common parts of object state.

**Real-world Example:** Representing characters in a word processor – each character object can be shared if it has the same formatting, with only the position stored externally.

```java
// Flyweight
class Character {
    private char symbol; // intrinsic state (shared)
    private String fontFamily;
    private int fontSize;

    public Character(char symbol, String fontFamily, int fontSize) {
        this.symbol = symbol;
        this.fontFamily = fontFamily;
        this.fontSize = fontSize;
    }

    public void display(int position) { // extrinsic state (position) passed
        System.out.println(symbol + " at position " + position + " [" + fontFamily + ", " + fontSize + "]");
    }
}

class CharacterFactory {
    private Map<String, Character> cache = new HashMap<>();

    public Character getCharacter(char symbol, String fontFamily, int fontSize) {
        String key = symbol + fontFamily + fontSize;
        if (!cache.containsKey(key)) {
            cache.put(key, new Character(symbol, fontFamily, fontSize));
        }
        return cache.get(key);
    }
}
```

### Proxy
**Explanation:** Provides a surrogate or placeholder for another object to control access to it. Common types: virtual proxy (lazy loading), protection proxy (access control), remote proxy.

**Real-world Example:** A virtual proxy for loading large images on demand in an image viewer.

```java
interface Image {
    void display();
}

class RealImage implements Image {
    private String filename;

    public RealImage(String filename) {
        this.filename = filename;
        loadFromDisk();
    }

    private void loadFromDisk() {
        System.out.println("Loading " + filename);
    }

    public void display() {
        System.out.println("Displaying " + filename);
    }
}

class ProxyImage implements Image {
    private RealImage realImage;
    private String filename;

    public ProxyImage(String filename) { this.filename = filename; }

    public void display() {
        if (realImage == null) {
            realImage = new RealImage(filename);
        }
        realImage.display();
    }
}
```

---

## Behavioral Patterns

### Chain of Responsibility
**Explanation:** Avoids coupling the sender of a request to its receiver by giving multiple objects a chance to handle the request. The request is passed along a chain until handled.

**Real-world Example:** A logging system with multiple loggers (info, debug, error) forming a chain.

```java
abstract class Logger {
    public static int INFO = 1;
    public static int DEBUG = 2;
    public static int ERROR = 3;

    protected int level;
    protected Logger nextLogger;

    public void setNextLogger(Logger next) { this.nextLogger = next; }

    public void logMessage(int level, String message) {
        if (this.level <= level) {
            write(message);
        }
        if (nextLogger != null) {
            nextLogger.logMessage(level, message);
        }
    }

    abstract protected void write(String message);
}

class InfoLogger extends Logger {
    public InfoLogger() { this.level = INFO; }
    protected void write(String message) { System.out.println("INFO: " + message); }
}

class ErrorLogger extends Logger {
    public ErrorLogger() { this.level = ERROR; }
    protected void write(String message) { System.out.println("ERROR: " + message); }
}
```

### Command
**Explanation:** Encapsulates a request as an object, thereby allowing parameterization, queuing, logging, and undo/redo operations.

**Real-world Example:** A remote control with buttons that execute commands (turn on light, open garage door).

```java
interface Command {
    void execute();
    void undo();
}

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
    private Command slot;
    public void setCommand(Command cmd) { slot = cmd; }
    public void pressButton() { slot.execute(); }
}
```

### Interpreter
**Explanation:** Defines a grammar for a language and provides an interpreter to evaluate sentences in that language. Useful for simple domain-specific languages.

**Real-world Example:** An arithmetic expression evaluator that parses and computes expressions like "5 + 3 - 2".

```java
interface Expression {
    int interpret();
}

class Number implements Expression {
    private int number;
    public Number(int number) { this.number = number; }
    public int interpret() { return number; }
}

class Add implements Expression {
    private Expression left, right;
    public Add(Expression left, Expression right) { this.left = left; this.right = right; }
    public int interpret() { return left.interpret() + right.interpret(); }
}

class Subtract implements Expression {
    private Expression left, right;
    public Subtract(Expression left, Expression right) { this.left = left; this.right = right; }
    public int interpret() { return left.interpret() - right.interpret(); }
}

// Usage: (5 + 3) - 2
Expression expr = new Subtract(new Add(new Number(5), new Number(3)), new Number(2));
System.out.println(expr.interpret()); // 6
```

### Iterator
**Explanation:** Provides a way to access elements of an aggregate object sequentially without exposing its underlying representation.

**Real-world Example:** Iterating over a collection of books in a library.

```java
class Book {
    private String title;
    public Book(String title) { this.title = title; }
    public String getTitle() { return title; }
}

interface Iterator<T> {
    boolean hasNext();
    T next();
}

class BookIterator implements Iterator<Book> {
    private List<Book> books;
    private int index = 0;
    public BookIterator(List<Book> books) { this.books = books; }
    public boolean hasNext() { return index < books.size(); }
    public Book next() { return books.get(index++); }
}

class Library implements Iterable<Book> {
    private List<Book> books = new ArrayList<>();
    public void addBook(Book book) { books.add(book); }
    public Iterator<Book> iterator() { return new BookIterator(books); }
}
```

### Mediator
**Explanation:** Defines an object that encapsulates how a set of objects interact. Promotes loose coupling by keeping objects from referring to each other explicitly.

**Real-world Example:** A chat room where users communicate via a mediator instead of directly.

```java
interface ChatMediator {
    void sendMessage(String message, User user);
    void addUser(User user);
}

abstract class User {
    protected ChatMediator mediator;
    protected String name;
    public User(ChatMediator mediator, String name) { this.mediator = mediator; this.name = name; }
    public abstract void send(String message);
    public abstract void receive(String message);
}

class ChatRoom implements ChatMediator {
    private List<User> users = new ArrayList<>();
    public void addUser(User user) { users.add(user); }
    public void sendMessage(String message, User sender) {
        for (User user : users) {
            if (user != sender) {
                user.receive(message);
            }
        }
    }
}

class ConcreteUser extends User {
    public ConcreteUser(ChatMediator mediator, String name) { super(mediator, name); }
    public void send(String message) { System.out.println(name + " sends: " + message); mediator.sendMessage(message, this); }
    public void receive(String message) { System.out.println(name + " receives: " + message); }
}
```

### Memento
**Explanation:** Captures and externalizes an object's internal state so that the object can be restored to this state later, without violating encapsulation.

**Real-world Example:** Saving and restoring the state of a text editor (undo feature).

```java
class Editor {
    private String content;

    public void type(String words) { content = words; }
    public String getContent() { return content; }

    public Memento save() { return new Memento(content); }
    public void restore(Memento memento) { content = memento.getContent(); }

    // Memento
    public static class Memento {
        private final String content;
        private Memento(String content) { this.content = content; }
        private String getContent() { return content; }
    }
}

class History {
    private Stack<Editor.Memento> history = new Stack<>();
    public void push(Editor.Memento memento) { history.push(memento); }
    public Editor.Memento pop() { return history.pop(); }
}
```

### Observer
**Explanation:** Defines a one-to-many dependency between objects so that when one object changes state, all its dependents are notified and updated automatically.

**Real-world Example:** A stock market where investors are notified of price changes.

```java
interface Observer {
    void update(float price);
}

class Stock {
    private List<Observer> observers = new ArrayList<>();
    private float price;

    public void addObserver(Observer o) { observers.add(o); }
    public void removeObserver(Observer o) { observers.remove(o); }
    public void setPrice(float price) {
        this.price = price;
        notifyObservers();
    }
    private void notifyObservers() {
        for (Observer o : observers) o.update(price);
    }
}

class Investor implements Observer {
    private String name;
    public Investor(String name) { this.name = name; }
    public void update(float price) { System.out.println(name + " notified of price change: " + price); }
}
```

### State
**Explanation:** Allows an object to alter its behavior when its internal state changes. The object will appear to change its class.

**Real-world Example:** A vending machine with states like "NoCoin", "HasCoin", "Sold", "SoldOut".

```java
interface VendingMachineState {
    void insertCoin();
    void selectProduct();
    void dispense();
}

class NoCoinState implements VendingMachineState {
    private VendingMachine machine;
    public NoCoinState(VendingMachine machine) { this.machine = machine; }
    public void insertCoin() { machine.setState(machine.getHasCoinState()); }
    public void selectProduct() { System.out.println("Insert coin first"); }
    public void dispense() { System.out.println("Insert coin first"); }
}

class HasCoinState implements VendingMachineState {
    private VendingMachine machine;
    public HasCoinState(VendingMachine machine) { this.machine = machine; }
    public void insertCoin() { System.out.println("Coin already inserted"); }
    public void selectProduct() { machine.setState(machine.getSoldState()); }
    public void dispense() { System.out.println("Select product first"); }
}

class VendingMachine {
    private VendingMachineState noCoinState, hasCoinState, soldState, soldOutState;
    private VendingMachineState currentState;

    public VendingMachine() {
        noCoinState = new NoCoinState(this);
        hasCoinState = new HasCoinState(this);
        // ... initialize others
        currentState = noCoinState;
    }

    public void setState(VendingMachineState state) { this.currentState = state; }
    public VendingMachineState getHasCoinState() { return hasCoinState; }
    public VendingMachineState getSoldState() { return soldState; }

    public void insertCoin() { currentState.insertCoin(); }
    public void selectProduct() { currentState.selectProduct(); }
}
```

### Strategy
**Explanation:** Defines a family of algorithms, encapsulates each one, and makes them interchangeable. The algorithm varies independently from clients that use it.

**Real-world Example:** Different sorting algorithms (bubble, quick) that can be swapped at runtime.

```java
interface SortingStrategy {
    void sort(int[] numbers);
}

class BubbleSort implements SortingStrategy {
    public void sort(int[] numbers) { System.out.println("Sorting using bubble sort"); }
}

class QuickSort implements SortingStrategy {
    public void sort(int[] numbers) { System.out.println("Sorting using quick sort"); }
}

class Sorter {
    private SortingStrategy strategy;
    public Sorter(SortingStrategy strategy) { this.strategy = strategy; }
    public void setStrategy(SortingStrategy strategy) { this.strategy = strategy; }
    public void sort(int[] numbers) { strategy.sort(numbers); }
}
```

### Template Method
**Explanation:** Defines the skeleton of an algorithm in a method, deferring some steps to subclasses. Subclasses can redefine certain steps without changing the algorithm's structure.

**Real-world Example:** A data mining class that defines steps for extracting data; subclasses implement for different formats (CSV, PDF).

```java
abstract class DataMiner {
    public final void mine() {
        openFile();
        extractData();
        parseData();
        closeFile();
    }
    abstract void openFile();
    abstract void extractData();
    abstract void parseData();
    void closeFile() { System.out.println("Close file"); } // common step
}

class CsvDataMiner extends DataMiner {
    void openFile() { System.out.println("Open CSV file"); }
    void extractData() { System.out.println("Extract CSV data"); }
    void parseData() { System.out.println("Parse CSV data"); }
}
```

### Visitor
**Explanation:** Represents an operation to be performed on elements of an object structure. Lets you define a new operation without changing the classes of the elements.

**Real-world Example:** A document structure with different elements (paragraph, image) and a visitor that exports to HTML.

```java
interface DocumentElement {
    void accept(Visitor visitor);
}

class Paragraph implements DocumentElement {
    private String text;
    public Paragraph(String text) { this.text = text; }
    public String getText() { return text; }
    public void accept(Visitor visitor) { visitor.visit(this); }
}

class Image implements DocumentElement {
    private String src;
    public Image(String src) { this.src = src; }
    public String getSrc() { return src; }
    public void accept(Visitor visitor) { visitor.visit(this); }
}

interface Visitor {
    void visit(Paragraph paragraph);
    void visit(Image image);
}

class HtmlExportVisitor implements Visitor {
    public void visit(Paragraph p) { System.out.println("<p>" + p.getText() + "</p>"); }
    public void visit(Image i) { System.out.println("<img src='" + i.getSrc() + "' />"); }
}
```
