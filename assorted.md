### Abstract Document
- **Definition**: A structural design pattern that allows handling of non-typed properties in a flexible way, using a key-value store and dynamic accessors.
- **Intent**: Enables adding new attributes to objects without changing their structure, useful for configurations or dynamic data models.
- **Structure**: Consists of an interface defining get/put methods, an abstract document class implementing these methods, and concrete document classes that provide type-safe accessors.
- **Pros**: Flexibility, easy extension, promotes separation of concerns (data storage vs. behavior).
- **Cons**: Type safety is partially lost; requires runtime checks; can lead to complex hierarchies.
---
### Abstract Factory
- **Definition**: A creational pattern that provides an interface for creating families of related or dependent objects without specifying their concrete classes.
- **Intent**: Encapsulates object creation logic, ensuring that products from the same family are used together.
- **Structure**: Abstract Factory interface with methods for each product type; Concrete Factories implement these to create specific product variants.
- **Pros**: Promotes consistency among products; isolates concrete classes; easy to swap product families.
- **Cons**: Adding new products requires extending the factory interface and all implementations, which can be cumbersome.
---
### Active Object
- **Definition**: A concurrency pattern that decouples method execution from method invocation to simplify multithreaded programming.
- **Intent**: Allows methods to be invoked asynchronously by placing requests in a queue and processing them in a separate thread.
- **Structure**: Proxy (interface), method requests, activation queue, scheduler, servant (actual implementation), and future for results.
- **Pros**: Simplifies concurrency by handling synchronization internally; improves responsiveness.
- **Cons**: Overhead of queue and threading; potential for increased complexity in error handling.
---
### Actor Model
- **Definition**: A conceptual model for concurrent computation where "actors" are the fundamental units of processing.
- **Intent**: Avoid shared state and locks by having actors communicate via asynchronous messages.
- **Structure**: Each actor has a mailbox, behavior, and state; it processes messages sequentially and can create new actors.
- **Pros**: Highly scalable; fault-tolerant (supervision); natural fit for distributed systems.
- **Cons**: Requires paradigm shift; debugging can be challenging; message ordering not guaranteed.
---
### Acyclic Visitor
- **Definition**: A variation of the Visitor pattern that eliminates cyclic dependencies by using dynamic_cast or type checks.
- **Intent**: Allow new operations to be added to existing class hierarchies without modifying them, while avoiding circular dependencies.
- **Structure**: Visitor interface declares visit methods for each type; elements accept visitors; concrete visitors implement operations.
- **Pros**: No cycles; easier to extend with new visitors; works with existing hierarchies.
- **Cons**: Still requires changes if new element types are added; type safety may rely on RTTI.
---
### Adapter
- **Definition**: A structural pattern that allows objects with incompatible interfaces to collaborate.
- **Intent**: Convert the interface of a class into another interface clients expect.
- **Structure**: Adapter implements the target interface and holds a reference to the adaptee, delegating calls.
- **Pros**: Reuse existing classes without modification; increases flexibility.
- **Cons**: Can add complexity; may introduce performance overhead if many adapters.
---
### Ambassador
- **Definition**: A pattern that provides a helper service instance on a client side to offload common functionalities (e.g., networking, logging, retries).
- **Intent**: Encapsulate cross-cutting concerns in a separate object to keep the main client simple.
- **Structure**: Ambassador acts as a local proxy to a remote service, handling connectivity, retries, caching, etc.
- **Pros**: Simplifies client code; centralizes remote communication logic; can add resilience features.
- **Cons**: Adds another layer; may increase latency if not implemented efficiently.
---
### Anti-Corruption Layer
- **Definition**: A pattern that protects a domain model from the complexities of external systems by translating between them.
- **Intent**: Isolate the core system from changes in legacy or third-party systems.
- **Structure**: Consists of translators, adapters, and facades that convert external models to internal ones.
- **Pros**: Preserves domain integrity; reduces coupling; facilitates testing.
- **Cons**: Adds complexity; requires maintenance of mapping logic.
---
### Arrange/Act/Assert
- **Definition**: A pattern for structuring unit tests into three distinct sections.
- **Intent**: Improve test readability and maintainability by clearly separating setup, execution, and verification.
- **Structure**: Arrange (set up test data and conditions), Act (invoke the method under test), Assert (check outcomes).
- **Pros**: Clear structure; easier to understand and debug; encourages consistent testing style.
- **Cons**: May lead to duplication if not reused; not suitable for very simple tests.
---
### Async Method Invocation
- **Definition**: A pattern for invoking methods asynchronously, often using callbacks or futures.
- **Intent**: Allow non-blocking calls and handle results when ready.
- **Structure**: Typically involves a method that returns a Future or accepts a callback; the invocation returns immediately.
- **Pros**: Improves responsiveness; enables parallelism.
- **Cons**: Complexity in error handling; risk of callback hell.
---
### Backpressure
- **Definition**: A mechanism to handle flow control in systems where producers outpace consumers.
- **Intent**: Prevent overwhelming consumers by signaling producers to slow down.
- **Structure**: Can be implemented via bounded queues, reactive streams (e.g., with request(N)), or feedback loops.
- **Pros**: Preceeds resource exhaustion; improves system stability.
- **Cons**: Adds complexity; may reduce throughput if not tuned.
---
### Balking
- **Definition**: A concurrency pattern where an object refuses to perform an action if it is in an inappropriate state.
- **Intent**: Avoid performing operations when the object is not ready or already processing.
- **Structure**: Method checks a condition (e.g., a flag) and returns immediately or throws an exception if condition not met.
- **Pros**: Simple to implement; prevents invalid operations.
- **Cons**: May lead to silent failures if not handled properly.
---
### Bloc
- **Definition**: A pattern used in UI development (especially Flutter) to manage state and separate business logic from presentation.
- **Intent**: Provide a predictable state container that reacts to events and emits states.
- **Structure**: Bloc receives events, processes them (possibly using async operations), and outputs new states; UI listens to state changes.
- **Pros**: Clear separation; testable; reactive.
- **Cons**: Boilerplate; learning curve.
---
### Bridge
- **Definition**: A structural pattern that decouples an abstraction from its implementation so they can vary independently.
- **Intent**: Avoid a permanent binding between abstraction and implementation; allows switching implementations at runtime.
- **Structure**: Abstraction holds a reference to Implementor; refined abstractions and concrete implementors.
- **Pros**: Improves extensibility; follows Open/Closed principle; hides implementation details.
- **Cons**: Increases complexity; more interfaces.
---
### Builder
- **Definition**: A creational pattern that separates the construction of a complex object from its representation.
- **Intent**: Allow the same construction process to create different representations.
- **Structure**: Director orchestrates the building process using a Builder interface; concrete builders construct parts.
- **Pros**: Fine control over construction; reusable construction code; avoids telescoping constructors.
- **Cons**: Requires additional classes; may be overkill for simple objects.
---
### Business Delegate
- **Definition**: A pattern that reduces coupling between presentation tier and business services by providing a facade.
- **Intent**: Hide complexity of service lookup and communication.
- **Structure**: Business delegate encapsulates access to business services, possibly using service locator or lookup.
- **Pros**: Simplifies client; centralizes service access logic.
- **Cons**: Adds indirection; may become a bottleneck.
---
### Bytecode
- **Definition**: A pattern where behavior is encoded in a sequence of instructions interpreted by a virtual machine.
- **Intent**: Achieve flexibility and portability by defining operations as bytecode.
- **Structure**: Includes an interpreter, a bytecode array, and a context (stack, variables).
- **Pros**: Platform independence; can be dynamically modified; compact representation.
- **Cons**: Slower than native code; complexity of interpreter.
---
### Caching
- **Definition**: A pattern that stores frequently accessed data in a fast-access storage to improve performance.
- **Intent**: Reduce latency and load on underlying systems.
- **Structure**: Cache (in-memory store) with eviction policies (LRU, TTL); cache aside, read-through, write-through strategies.
- **Pros**: Faster data access; reduces resource usage.
- **Cons**: Cache invalidation complexity; memory overhead; stale data risk.
---
### Callback
- **Definition**: A mechanism where a function is passed as an argument to another function, to be executed after an operation completes.
- **Intent**: Allow asynchronous notification or customization of behavior.
- **Structure**: Caller accepts a callback interface or function pointer; invokes it at appropriate time.
- **Pros**: Decouples caller from callee; enables asynchronous processing.
- **Cons**: Can lead to callback hell; inversion of control.
---
### Chain of Responsibility
- **Definition**: A behavioral pattern that passes a request along a chain of handlers until one handles it.
- **Intent**: Avoid coupling the sender of a request to its receiver by giving multiple objects a chance to handle it.
- **Structure**: Handler interface with successor reference; concrete handlers either handle or pass to next.
- **Pros**: Decouples sender and receiver; dynamic chain; promotes flexibility.
- **Cons**: No guarantee request will be handled; debugging chain flow can be hard.
---
### Circuit Breaker
- **Definition**: A pattern that prevents repeated attempts to invoke a failing operation, allowing time for recovery.
- **Intent**: Protect system from cascading failures and provide graceful degradation.
- **Structure**: Circuit breaker states: closed (normal), open (failing), half-open (testing recovery); trips after failures.
- **Pros**: Improves resilience; reduces load on failing services; fast failure.
- **Cons**: Complexity in tuning thresholds; may mask underlying issues.
---
### Clean Architecture
- **Definition**: An architectural pattern that emphasizes separation of concerns through concentric layers.
- **Intent**: Create systems that are independent of frameworks, UI, databases, and external agencies.
- **Structure**: Entities (core), use cases, interface adapters, frameworks/drivers; dependencies point inward.
- **Pros**: Testable; maintainable; independent of external details.
- **Cons**: Complexity; steep learning curve; may be overkill for simple apps.
---
### Client Session
- **Definition**: A pattern for managing user session state on the client side rather than the server.
- **Intent**: Reduce server memory footprint and improve scalability.
- **Structure**: Session data stored in cookies, local storage, or client-side database; sent with each request.
- **Pros**: Scalable; reduces server-side state management.
- **Cons**: Security concerns; size limitations; must be validated on server.
---
### Collecting Parameter
- **Definition**: A pattern where a parameter passed to methods accumulates results from multiple method calls.
- **Intent**: Avoid creating separate result containers; pass a single collection that gets populated.
- **Structure**: Method receives a collection (e.g., list) and adds results to it; caller then uses the collection.
- **Pros**: Simplifies result aggregation; reduces return complexity.
- **Cons**: Mutates parameter; may be less intuitive.
---
### Collection Pipeline
- **Definition**: A pattern that chains operations (filter, map, reduce) on collections in a declarative style.
- **Intent**: Process data by composing transformations, similar to Unix pipes.
- **Structure**: Series of operations where each step takes a collection and produces a new collection.
- **Pros**: Readable; composable; often parallelizable.
- **Cons**: Can be inefficient if not lazily evaluated; overuse may hide intent.
---
### Combinator
- **Definition**: A pattern that combines functions or objects to build complex behaviors from simpler ones.
- **Intent**: Enable composition of operations in a declarative way.
- **Structure**: Combinators are higher-order functions that take functions/values and return new ones.
- **Pros**: Highly reusable; expressive; promotes immutability.
- **Cons**: Can be hard to debug; requires functional programming mindset.
---
### Command
- **Definition**: A behavioral pattern that encapsulates a request as an object, allowing parameterization and queuing.
- **Intent**: Decouple sender and receiver; support undo/redo, logging, transactions.
- **Structure**: Command interface with execute(); concrete commands hold receiver; invoker stores commands.
- **Pros**: Extensible; supports undoable operations; queues commands.
- **Cons**: Adds class per command; may increase complexity.
---
### Commander
- **Definition**: A pattern used in distributed systems to orchestrate and manage distributed transactions (e.g., Saga).
- **Intent**: Coordinate multiple steps across services with compensating actions for failure.
- **Structure**: Commander process initiates steps, tracks state, and triggers compensating actions if needed.
- **Pros**: Ensures eventual consistency; handles failures gracefully.
- **Cons**: Complexity in state management; requires idempotency.
---
### Command Query Responsibility Segregation (CQRS)
- **Definition**: A pattern that separates read and write operations into different models.
- **Intent**: Optimize for different scalability and consistency needs of commands and queries.
- **Structure**: Command side handles updates (using domain model); query side handles reads (using denormalized views); possibly separate databases.
- **Pros**: Scalability; flexibility; independent optimization.
- **Cons**: Complexity; eventual consistency; duplication of data.
---
### Component
- **Definition**: A structural pattern that allows objects to be composed hierarchically, often used in game development.
- **Intent**: Enable dynamic composition of behavior by attaching components to entities.
- **Structure**: Entity holds a collection of components; each component provides specific functionality.
- **Pros**: Flexible; reusable; avoids deep inheritance.
- **Cons**: Communication between components can be tricky; performance overhead.
---
### Composite
- **Definition**: A structural pattern that composes objects into tree structures to represent part-whole hierarchies.
- **Intent**: Allow clients to treat individual objects and compositions uniformly.
- **Structure**: Component interface with common operations; Leaf and Composite (which contains children) implement it.
- **Pros**: Uniform treatment; simplifies client; easy to add new leaf types.
- **Cons**: Overly general design may make restrictions difficult.
---
### Composite Entity
- **Definition**: A pattern in enterprise applications where a coarse-grained entity is composed of multiple fine-grained objects.
- **Intent**: Manage related objects as a single unit to simplify client interactions.
- **Structure**: Composite entity manages dependent objects; often used with EJBs.
- **Pros**: Simplifies client; encapsulates relationships; reduces network calls.
- **Cons**: May hide necessary details; complex to implement.
---
### Composite View
- **Definition**: A presentation pattern that builds a view from smaller, reusable subviews.
- **Intent**: Promote reusability and modularity in UI development.
- **Structure**: A master view includes child views; each child view can be a composite itself.
- **Pros**: Reusable components; easier maintenance; separation of concerns.
- **Cons**: Overhead of managing multiple views; communication between them.
---
### Context Object
- **Definition**: A pattern that encapsulates state and behavior shared across multiple components in a request.
- **Intent**: Avoid passing many parameters; provide a single object with contextual data.
- **Structure**: Context object holds key-value pairs or specific attributes; passed down the call chain.
- **Pros**: Reduces method clutter; centralizes context; easy to add new data.
- **Cons**: Can become a god object; hidden dependencies.
---
### Converter
- **Definition**: A pattern that converts one data type to another, often used in mapping between layers.
- **Intent**: Provide a centralized place for conversion logic to avoid duplication.
- **Structure**: Converter class with methods to convert between types (e.g., DTO to entity).
- **Pros**: Reusable; testable; keeps conversion logic isolated.
- **Cons**: Might need many converters; performance overhead if heavy.
---
### Curiously Recurring Template Pattern (CRTP)
- **Definition**: A C++ idiom where a class inherits from a template instantiated with itself.
- **Intent**: Achieve static polymorphism, avoiding virtual function overhead.
- **Structure**: `class Derived : public Base<Derived> { ... }`; base class uses derived type to call methods.
- **Pros**: Compile-time polymorphism; efficient; enables mixins.
- **Cons**: Only works in C++; can lead to code bloat; harder to understand.
---
### Currying
- **Definition**: A functional programming technique that transforms a function taking multiple arguments into a sequence of functions each taking a single argument.
- **Intent**: Enable partial application and function composition.
- **Structure**: `f(a,b,c)` becomes `f(a)(b)(c)`.
- **Pros**: Increases reusability; allows specialized functions; aids composition.
- **Cons**: May obscure intent; not idiomatic in all languages.
---
### DAO Factory
- **Definition**: A pattern that combines Data Access Object (DAO) with Abstract Factory to provide DAO creation.
- **Intent**: Encapsulate the creation of DAOs for different data sources.
- **Structure**: Abstract factory declares methods for creating DAOs; concrete factories produce DAOs for specific databases.
- **Pros**: Centralizes DAO instantiation; easy to switch data sources.
- **Cons**: Adds abstraction layers; may be overkill if only one data source.
---
### Data Access Object (DAO)
- **Definition**: A pattern that abstracts and encapsulates access to a data source, providing a uniform interface.
- **Intent**: Separate data access logic from business logic.
- **Structure**: DAO interface with CRUD methods; concrete DAO implements for specific persistence mechanism.
- **Pros**: Decouples business layer from persistence; easier testing; portable.
- **Cons**: Leakage of persistence details if not careful; boilerplate.
---
### Data Bus
- **Definition**: A pattern that provides a shared communication channel for components to publish and subscribe to events.
- **Intent**: Decouple event producers and consumers.
- **Structure**: Central bus (event dispatcher) with subscribe/publish methods; components post events.
- **Pros**: Loose coupling; dynamic subscriptions; scales well.
- **Cons**: Debugging becomes harder; potential for event storms.
---
### Data Locality
- **Definition**: A pattern that organizes data in memory to take advantage of CPU caches.
- **Intent**: Improve performance by reducing cache misses.
- **Structure**: Store related data together in contiguous memory (e.g., arrays of structs vs. struct of arrays).
- **Pros**: Significant performance gains; essential for high-performance computing.
- **Cons**: May complicate code; harder to maintain.
---
### Data Mapper
- **Definition**: A pattern that transfers data between objects and a database while keeping them independent.
- **Intent**: Isolate domain objects from persistence details.
- **Structure**: Mapper class handles loading and storing; domain objects have no knowledge of database.
- **Pros**: Clean domain model; flexibility; supports complex mappings.
- **Cons**: Complexity; overhead of mapping.
---
### Data Transfer Object (DTO)
- **Definition**: A pattern that carries data between processes, often to reduce network calls.
- **Intent**: Aggregate data from multiple sources into a single object for transfer.
- **Structure**: Simple object with fields and getters/setters; no business logic.
- **Pros**: Reduces network traffic; decouples layers; can be serialized easily.
- **Cons**: Anemic objects; may lead to duplication.
---
### Decorator
- **Definition**: A structural pattern that attaches additional responsibilities to an object dynamically.
- **Intent**: Provide a flexible alternative to subclassing for extending functionality.
- **Structure**: Decorator implements the same interface as the component and holds a reference to it; adds behavior before/after delegating.
- **Pros**: More flexible than inheritance; follows Open/Closed; can combine multiple decorators.
- **Cons**: Complexity from many small classes; ordering matters.
---
### Delegation
- **Definition**: A pattern where an object hands off a request to another object (the delegate) instead of handling it itself.
- **Intent**: Achieve composition over inheritance; reuse functionality.
- **Structure**: Object has a reference to a delegate and forwards calls to it.
- **Pros**: Promotes loose coupling; dynamic behavior change; simpler than inheritance.
- **Cons**: May obscure flow; extra indirection.
---
### Dependency Injection
- **Definition**: A pattern where objects receive their dependencies from an external source rather than creating them internally.
- **Intent**: Invert control of dependency creation to improve testability and flexibility.
- **Structure**: Dependencies passed via constructor, setter, or interface; container manages wiring.
- **Pros**: Decouples classes; easier unit testing; promotes single responsibility.
- **Cons**: Complexity; can make configuration hard to trace.
---
### Dirty Flag
- **Definition**: A pattern that tracks whether an object's state has changed (is dirty) to avoid unnecessary operations.
- **Intent**: Optimize performance by only performing expensive updates when needed.
- **Structure**: Boolean flag set when data changes; cleared after update; operations check flag.
- **Pros**: Reduces work; simple to implement.
- **Cons**: Flag can become stale; concurrency issues if not synchronized.
---
### Domain Model
- **Definition**: A pattern that creates a conceptual model of the problem domain with behavior and data.
- **Intent**: Represent business concepts and rules in a rich object model.
- **Structure**: Classes with attributes and methods reflecting domain logic; relationships like inheritance, composition.
- **Pros**: Captures business complexity; reusable; testable.
- **Cons**: Complexity; may need mapping to persistence.
---
### Double Buffer
- **Definition**: A pattern that uses two buffers to allow reading and writing concurrently without interference.
- **Intent**: Avoid tearing or inconsistent state when updating and displaying data simultaneously.
- **Structure**: Two buffers: one for writing, one for reading; swap after writing completes.
- **Pros**: Smooth updates; thread-safe without locks.
- **Cons**: Memory overhead; complexity of swapping.
---
### Double-Checked Locking
- **Definition**: A concurrency pattern that reduces lock overhead by checking a condition twice before acquiring a lock.
- **Intent**: Lazy initialization with minimal locking.
- **Structure**: Check if instance is null (without lock), then lock and check again before creating.
- **Pros**: Improves performance in multithreaded lazy initialization.
- **Cons**: Fragile; can be broken by compiler optimizations or memory models (requires volatile).
---
### Double Dispatch
- **Definition**: A technique where the operation executed depends on the runtime types of two objects.
- **Intent**: Simulate dynamic dispatch for multiple objects (like Visitor pattern).
- **Structure**: First object calls a method on second, passing itself; second then calls back a method on first with its type.
- **Pros**: Enables polymorphic behavior on two objects; extensible.
- **Cons**: Requires collaboration; can be complex.
---
### Dynamic Proxy
- **Definition**: A mechanism to create a proxy class dynamically at runtime that implements a set of interfaces.
- **Intent**: Intercept method calls to add behavior (e.g., logging, transactions) without modifying original code.
- **Structure**: Proxy class generated via reflection; invocation handler handles method calls.
- **Pros**: Reduces boilerplate; flexible; supports AOP.
- **Cons**: Performance overhead; limited to interfaces in some languages.
---
### Event Aggregator
- **Definition**: A pattern that centralizes event handling by aggregating multiple event sources into a single object.
- **Intent**: Simplify event subscription and publishing for many components.
- **Structure**: Event aggregator maintains list of subscribers; components publish events to aggregator, which forwards.
- **Pros**: Reduces coupling; simplifies wiring; single point for event management.
- **Cons**: Can become a bottleneck; hides event flow.
---
### Event-Based Asynchronous
- **Definition**: A pattern where components communicate via events asynchronously, often using a message queue.
- **Intent**: Decouple event producers and consumers, allowing non-blocking interactions.
- **Structure**: Event sources raise events; event handlers subscribe; events processed asynchronously.
- **Pros**: Loose coupling; scalability; responsiveness.
- **Cons**: Complexity in ordering and error handling; eventual consistency.
---
### Event-Driven Architecture
- **Definition**: An architectural style where system components react to events.
- **Intent**: Build systems that are highly responsive, scalable, and decoupled.
- **Structure**: Event producers, event channels, event processors; often using pub/sub or event streaming.
- **Pros**: Loose coupling; scalability; real-time capabilities.
- **Cons**: Complexity in testing and debugging; eventual consistency.
---
### Event Queue
- **Definition**: A pattern that decouples event production from event processing using a queue.
- **Intent**: Manage asynchronous processing and smooth out peaks.
- **Structure**: Events are placed in a queue; one or more consumers process them in order.
- **Pros**: Buffering; load leveling; reliable processing.
- **Cons**: Potential for queue overflow; latency.
---
### Event Sourcing
- **Definition**: A pattern where state changes are stored as a sequence of events, rather than just the current state.
- **Intent**: Reconstruct past states, enable auditing, and support complex business logic.
- **Structure**: Events are immutable; aggregate applies events to rebuild state; event store persists events.
- **Pros**: Audit trail; temporal queries; scalability; enables CQRS.
- **Cons**: Complexity; event versioning; eventual consistency.
---
### Execute Around
- **Definition**: A pattern that centralizes common setup and cleanup code around a method call.
- **Intent**: Ensure that necessary actions (e.g., resource acquisition/release) are always performed.
- **Structure**: A method takes a callback; it performs setup, calls the callback, then performs cleanup.
- **Pros**: Reduces duplication; guarantees resource handling; improves safety.
- **Cons**: Requires lambda or inner class; may obscure flow.
---
### Extension Objects
- **Definition**: A pattern that allows adding new functionality to objects without modifying them, by attaching extension objects.
- **Intent**: Provide a flexible way to extend object behavior dynamically.
- **Structure**: Each object has a method to retrieve extension by type; extensions implement specific interfaces.
- **Pros**: Open/Closed; dynamic extensions; avoids inheritance explosion.
- **Cons**: Lookup overhead; type safety issues.
---
### Facade
- **Definition**: A structural pattern that provides a simplified interface to a complex subsystem.
- **Intent**: Reduce complexity and coupling between clients and subsystems.
- **Structure**: Facade class delegates client requests to appropriate subsystem classes.
- **Pros**: Simplifies usage; decouples clients; promotes layering.
- **Cons**: May become a god object; hides functionality.
---
### Factory
- **Definition**: A creational pattern that defines an interface for creating objects, but lets subclasses decide which class to instantiate.
- **Intent**: Encapsulate object creation to promote loose coupling.
- **Structure**: Creator (abstract) with factory method; concrete creators override to produce specific products.
- **Pros**: Flexible; supports open/closed; isolates concrete classes.
- **Cons**: May lead to many subclasses; complexity.
---
### Factory Kit
- **Definition**: A pattern that combines Factory and Builder to create families of objects with a fluent interface.
- **Intent**: Provide a flexible way to configure and create objects with different characteristics.
- **Structure**: Kit registers builders for each type; client uses kit to select and build.
- **Pros**: Configurable; fluent; separates creation logic.
- **Cons**: More complex than simple factory.
---
### Factory Method
- **Definition**: A creational pattern that defines an interface for creating an object, but lets subclasses alter the type of objects that will be created.
- **Intent**: Defer instantiation to subclasses.
- **Structure**: Creator declares factory method; concrete creators implement it.
- **Pros**: Promotes loose coupling; easy extension.
- **Cons**: Requires subclassing; may not be needed for simple creation.
---
### Fan-Out/Fan-In
- **Definition**: A pattern in concurrent programming where tasks are split (fanned out) to multiple workers and then results are combined (fanned in).
- **Intent**: Achieve parallelism by dividing work.
- **Structure**: Fan-out distributes tasks; fan-in aggregates results, often using a wait group or join.
- **Pros**: Maximizes concurrency; scalable.
- **Cons**: Overhead of coordination; potential for contention.
---
### Feature Toggle
- **Definition**: A technique that allows enabling/disabling features at runtime without deploying new code.
- **Intent**: Facilitate continuous delivery, A/B testing, and gradual rollouts.
- **Structure**: Configuration flags checked in code; toggles can be static or dynamic.
- **Pros**: Reduces branching; enables experimentation; quick rollback.
- **Cons**: Code complexity; toggle debt; testing overhead.
---
### Filterer
- **Definition**: A pattern that provides a way to apply filters to a collection or stream, often with chaining.
- **Intent**: Allow flexible and composable filtering criteria.
- **Structure**: Filter interface with `apply` method; filters can be combined via AND/OR.
- **Pros**: Reusable; composable; decouples filtering logic.
- **Cons**: May introduce many small classes; performance overhead.
---
### Fluent Interface
- **Definition**: A design approach that allows method chaining to create readable, domain-specific language-like code.
- **Intent**: Improve code readability and expressiveness.
- **Structure**: Methods return `this` to allow chaining; often used in builders and DSLs.
- **Pros**: Readable; expressive; reduces verbosity.
- **Cons**: Can hide side effects; debugging chained calls may be harder.
---
### Flux
- **Definition**: An architectural pattern for client-side applications (especially React) with unidirectional data flow.
- **Intent**: Manage state in a predictable way, avoiding two-way data binding issues.
- **Structure**: Actions -> Dispatcher -> Stores -> Views; actions trigger dispatcher, stores update, views re-render.
- **Pros**: Predictable; easy to debug; unidirectional flow.
- **Cons**: Boilerplate; steep learning curve.
---
### Flyweight
- **Definition**: A structural pattern that minimizes memory usage by sharing common parts of object state.
- **Intent**: Support large numbers of fine-grained objects efficiently.
- **Structure**: Flyweight interface; concrete flyweights store intrinsic state (shared); extrinsic state passed in.
- **Pros**: Reduces memory footprint; improves performance.
- **Cons**: Complexity in separating state; may increase CPU if extrinsic state is large.
---
### Front Controller
- **Definition**: A pattern that centralizes request handling for a web application, routing requests to appropriate handlers.
- **Intent**: Provide a single entry point to manage common concerns (security, logging, etc.).
- **Structure**: Front controller servlet/handler processes all requests, delegates to commands or actions.
- **Pros**: Centralized control; reduces duplication; easier to add cross-cutting concerns.
- **Cons**: Single point of failure; may become monolithic.
---
### Function Composition
- **Definition**: A functional programming technique where the output of one function becomes the input of another.
- **Intent**: Build complex operations by combining simpler functions.
- **Structure**: `(f ∘ g)(x) = f(g(x))`; often using `compose` or `pipe` utilities.
- **Pros**: Reusable; declarative; easy to test.
- **Cons**: Can be less intuitive for non-functional programmers; debugging may be tricky.
---
### Game Loop
- **Definition**: A pattern that controls the timing and processing of a game, updating state and rendering continuously.
- **Intent**: Maintain consistent game speed across different hardware.
- **Structure**: Loop processes input, updates game logic, and renders; may use fixed time steps or variable steps.
- **Pros**: Smooth animation; predictable updates.
- **Cons**: Complexity in handling varying frame rates; potential for performance issues.
---
### Gateway
- **Definition**: A pattern that encapsulates access to an external system or resource, providing a simple interface.
- **Intent**: Isolate client from external system details, such as APIs or databases.
- **Structure**: Gateway class with methods that perform external calls, handle mapping, and errors.
- **Pros**: Decouples client; centralizes external access; easier testing.
- **Cons**: May become a bottleneck; adds abstraction.
---
### Guarded Suspension
- **Definition**: A concurrency pattern where a thread waits until a condition is satisfied before proceeding.
- **Intent**: Coordinate threads when a condition must hold before action.
- **Structure**: Loop checks condition; if false, thread waits (using wait/notify or condition variables) until notified.
- **Pros**: Prevents busy waiting; safe coordination.
- **Cons**: Complexity; risk of deadlock if notify missed.
---
### Half-Sync/Half-Async
- **Definition**: A concurrency pattern that separates synchronous and asynchronous processing layers.
- **Intent**: Combine the benefits of synchronous and asynchronous I/O.
- **Structure**: Async layer handles I/O, queues tasks; sync layer processes tasks in threads.
- **Pros**: Efficient I/O; simplified sync processing.
- **Cons**: Queue overhead; two layers may complicate design.
---
### Health Check
- **Definition**: A pattern where services expose endpoints to report their health status.
- **Intent**: Monitor system health and facilitate automated recovery.
- **Structure**: Health check endpoints return status (OK, degraded, down) and details; used by orchestrators.
- **Pros**: Improves observability; enables self-healing.
- **Cons**: Extra overhead; must be kept simple.
---
### Hexagonal Architecture
- **Definition**: An architectural pattern (Ports and Adapters) that isolates the core application from external agents.
- **Intent**: Create a system that can be driven by multiple inputs (UI, tests) and outputs (DB, APIs) without changing core.
- **Structure**: Core contains domain logic; ports define interfaces; adapters implement ports for external systems.
- **Pros**: High testability; flexibility; independent of technology.
- **Cons**: Complexity; many interfaces.
---
### Identity Map
- **Definition**: A pattern that ensures each object is loaded only once per transaction, caching objects by ID.
- **Intent**: Avoid inconsistencies and improve performance by reusing objects.
- **Structure**: Map from ID to object; before loading, check map; if present, return same instance.
- **Pros**: Prevents duplicate objects; reduces database hits.
- **Cons**: Memory overhead; must manage scope (transaction).
---
### Intercepting Filter
- **Definition**: A pattern that processes requests and responses through a chain of filters before reaching the target.
- **Intent**: Handle common pre/post-processing like authentication, logging, compression.
- **Structure**: Filter chain manages filters; each filter performs its task and calls next.
- **Pros**: Reusable; declarative; flexible.
- **Cons**: Ordering matters; may impact performance.
---
### Interpreter
- **Definition**: A behavioral pattern that defines a grammar for a language and provides an interpreter to evaluate sentences.
- **Intent**: Solve problems that can be expressed in a simple language.
- **Structure**: Abstract expression, terminal and nonterminal expressions; context stores variables.
- **Pros**: Easy to extend grammar; good for small languages.
- **Cons**: Complexity for large grammars; performance may be poor.
---
### Iterator
- **Definition**: A behavioral pattern that provides a way to access elements of a collection sequentially without exposing its underlying representation.
- **Intent**: Provide a uniform interface for traversing different data structures.
- **Structure**: Iterator interface with `hasNext()`, `next()`; concrete iterators for each collection.
- **Pros**: Encapsulates traversal; supports multiple traversals; simplifies collection interface.
- **Cons**: May not be efficient for some structures; extra object.
---
### Layered Architecture
- **Definition**: A common architectural style where components are organized into horizontal layers (e.g., presentation, business, persistence).
- **Intent**: Separate concerns and promote maintainability.
- **Structure**: Each layer depends only on the layer below; communication flows top-down.
- **Pros**: Separation of concerns; easier testing; familiarity.
- **Cons**: May lead to monolithic design; performance overhead from layer jumps.
---
### Lazy Loading
- **Definition**: A pattern that delays the initialization of an object until it is actually needed.
- **Intent**: Improve performance and memory usage by loading only required data.
- **Structure**: Implemented via proxy, ghost, or value holder that loads on first access.
- **Pros**: Saves resources; faster startup.
- **Cons**: Complexity; may cause latency spikes at access time.
---
### Leader Election
- **Definition**: A pattern used in distributed systems to elect a single node as coordinator.
- **Intent**: Ensure a single leader to manage tasks, avoiding conflicts.
- **Structure**: Algorithms like Bully, Raft, Paxos; nodes elect leader, others monitor.
- **Pros**: Fault tolerance; coordination.
- **Cons**: Complexity; network overhead; split-brain risk.
---
### Leader-Followers
- **Definition**: A concurrency pattern where one thread (leader) waits for events and then passes leadership to another.
- **Intent**: Efficiently handle multiple events with a thread pool, reducing context switching.
- **Structure**: Threads take turns being leader; leader waits, processes event, then promotes follower.
- **Pros**: Low latency; efficient thread usage.
- **Cons**: Complexity; may not scale with many cores.
---
### Lockable Object
- **Definition**: A pattern that provides a mechanism to lock an object for exclusive access.
- **Intent**: Control concurrent access to a shared resource.
- **Structure**: Object exposes lock/unlock methods, possibly with timeout; uses mutex internally.
- **Pros**: Simple synchronization; encapsulated locking.
- **Cons**: Risk of deadlock; may not be composable.
---
### MapReduce
- **Definition**: A programming model for processing large data sets in parallel across a cluster.
- **Intent**: Simplify distributed computations by abstracting map and reduce steps.
- **Structure**: Map processes input key-value pairs to intermediate; Reduce aggregates intermediate by key.
- **Pros**: Scalable; fault-tolerant; hides distribution details.
- **Cons**: Not suitable for all problems; overhead for small data.
---
### Marker Interface
- **Definition**: An interface with no methods, used to convey metadata about a class.
- **Intent**: Signal that a class possesses a certain property (e.g., Serializable, Cloneable).
- **Structure**: Empty interface; classes implement it; runtime checks via `instanceof`.
- **Pros**: Simple; type-safe at compile time.
- **Cons**: Limited; can be replaced by annotations.
---
### Master-Worker
- **Definition**: A pattern where a master process distributes tasks to worker processes and collects results.
- **Intent**: Parallelize work across multiple nodes/threads.
- **Structure**: Master divides task, assigns to workers; workers process and return; master aggregates.
- **Pros**: Scalable; fault tolerance possible.
- **Cons**: Master may become bottleneck; communication overhead.
---
### Mediator
- **Definition**: A behavioral pattern that reduces coupling between communicating objects by centralizing interactions.
- **Intent**: Define an object that encapsulates how a set of objects interact.
- **Structure**: Mediator interface; concrete mediator coordinates colleagues; colleagues communicate via mediator.
- **Pros**: Reduces dependencies; simplifies object protocols; centralizes control.
- **Cons**: Mediator can become complex; single point of failure.
---
### Memento
- **Definition**: A behavioral pattern that captures and externalizes an object's internal state so it can be restored later.
- **Intent**: Provide undo/rollback capabilities without violating encapsulation.
- **Structure**: Originator creates memento; caretaker stores memento; memento is opaque.
- **Pros**: Preserves encapsulation; simple undo.
- **Cons**: May consume memory; overhead of copying state.
---
### Metadata Mapping
- **Definition**: A pattern that maps between database tables and objects using metadata (e.g., XML, annotations).
- **Intent**: Simplify persistence mapping by centralizing mapping rules.
- **Structure**: Metadata defines how fields map to columns; mapping engine uses it to load/store.
- **Pros**: Flexible; reduces code; easier to change mappings.
- **Cons**: Runtime overhead; complexity of metadata management.
---
### Microservices Aggregator
- **Definition**: A pattern where a service aggregates data from multiple microservices and returns a combined response.
- **Intent**: Simplify client interactions by providing a unified API.
- **Structure**: Aggregator service calls downstream services, combines results, and sends to client.
- **Pros**: Reduces client complexity; hides internal composition.
- **Cons**: Adds latency; aggregator may become bottleneck.
---
### Microservices API Gateway
- **Definition**: A pattern where a single entry point handles all client requests, routing to appropriate microservices.
- **Intent**: Provide a unified API for clients and handle cross-cutting concerns.
- **Structure**: API gateway routes requests, may perform authentication, logging, rate limiting, and aggregation.
- **Pros**: Simplifies clients; centralizes cross-cutting concerns; enables protocol translation.
- **Cons**: Single point of failure; adds latency; potential for becoming monolith.
---
### Microservices Client-Side UI Composition
- **Definition**: A pattern where the UI is composed by fragments from different microservices, assembled on the client.
- **Intent**: Allow each microservice to own a part of the UI, improving autonomy.
- **Structure**: Client fetches fragments (e.g., via JavaScript) from multiple services and composes them.
- **Pros**: Decouples UI deployment; each team owns its UI pieces.
- **Cons**: Complexity in coordination; performance may suffer.
---
### Microservices Distributed Tracing
- **Definition**: A technique to track requests across multiple microservices to understand flow and debug issues.
- **Intent**: Provide visibility into distributed transactions.
- **Structure**: Trace ID propagates through services; spans record timing and metadata; collectors aggregate.
- **Pros**: Debugging; performance analysis; dependency mapping.
- **Cons**: Overhead; requires instrumentation.
---
### Microservices Idempotent Consumer
- **Definition**: A pattern ensuring that processing the same message multiple times has the same effect as once.
- **Intent**: Handle duplicate messages safely in distributed systems.
- **Structure**: Consumer checks a deduplication store (e.g., DB) before processing; if already processed, skip.
- **Pros**: Fault tolerance; simplifies retry logic.
- **Cons**: Storage overhead; requires idempotency keys.
---
### Microservices Log Aggregation
- **Definition**: A pattern that collects logs from all microservices into a central system for searching and analysis.
- **Intent**: Centralize logging for debugging and monitoring.
- **Structure**: Agents ship logs to central store (e.g., ELK stack); logs tagged with service and instance.
- **Pros**: Unified view; facilitates troubleshooting.
- **Cons**: Storage costs; potential for log overload.
---
### Microservices Pattern - Self-Registration
- **Definition**: A pattern where a microservice registers itself with a service registry on startup.
- **Intent**: Enable dynamic discovery of services without manual configuration.
- **Structure**: Service registers its location (host, port) with registry; clients query registry.
- **Pros**: Decentralized; reduces manual configuration; supports dynamic scaling.
- **Cons**: Registry becomes critical; need health checks to deregister.
---
### Model-View-Controller (MVC)
- **Definition**: An architectural pattern that separates application into Model (data), View (UI), and Controller (input handling).
- **Intent**: Separate concerns to improve maintainability and testability.
- **Structure**: Model notifies View of changes; Controller updates Model and selects View.
- **Pros**: Separation of concerns; multiple views for same model; testable.
- **Cons**: Complexity; tight coupling between View and Controller in some implementations.
---
### Model-View-Intent (MVI)
- **Definition**: A reactive pattern for UI, especially in Android, where user intents trigger state updates.
- **Intent**: Create unidirectional data flow for predictable UI state management.
- **Structure**: View emits intents; Model processes them and produces new state; View renders state.
- **Pros**: Unidirectional; easy to test; state is immutable.
- **Cons**: Boilerplate; learning curve.
---
### Model-View-Presenter (MVP)
- **Definition**: A UI pattern where the Presenter handles logic and updates the View, which is passive.
- **Intent**: Improve testability by isolating view logic.
- **Structure**: View interface; Presenter interacts with Model and updates View; View delegates events to Presenter.
- **Pros**: Testable (Presenter can be unit tested); separation.
- **Cons**: View-Presenter mapping can be verbose; Presenter may become large.
---
### Model-View-ViewModel (MVVM)
- **Definition**: A UI pattern that leverages data binding to automatically synchronize View and ViewModel.
- **Intent**: Reduce boilerplate by having the ViewModel expose observable data.
- **Structure**: View binds to ViewModel properties; ViewModel contains commands and state; Model provides data.
- **Pros**: Less code; easier two-way binding; testable.
- **Cons**: Debugging data binding can be hard; may introduce complexity.
---
### Monad
- **Definition**: A functional programming construct that allows structuring programs with composable operations, handling side effects.
- **Intent**: Encapsulate computations in a context (e.g., Maybe, Either, List) and chain them.
- **Structure**: Monad implements `flatMap` (bind) and `unit` (return) following monadic laws.
- **Pros**: Enables pure functional composition; handles null, errors, etc., elegantly.
- **Cons**: Steep learning curve; can be overused.
---
### Money
- **Definition**: A pattern for representing monetary values to avoid floating-point inaccuracies.
- **Intent**: Handle currency and arithmetic safely.
- **Structure**: Money class with amount (using integer or decimal) and currency; operations like add, subtract.
- **Pros**: Precision; encapsulates currency rules.
- **Cons**: May be heavier than primitive; needs careful handling of rounding.
---
### Monitor
- **Definition**: A concurrency construct that provides mutual exclusion and condition variables.
- **Intent**: Simplify thread synchronization by encapsulating locks and waiting.
- **Structure**: Object with synchronized methods; internally uses a lock; `wait()` and `notify()` for conditions.
- **Pros**: High-level abstraction; reduces errors.
- **Cons**: Can lead to deadlock if not careful; limited to single JVM.
---
### Monolithic Architecture
- **Definition**: A traditional software model where all components are packaged together as a single unit.
- **Intent**: Simplicity in development and deployment for small applications.
- **Structure**: Single codebase, single deployable unit; all modules interconnected.
- **Pros**: Simple to develop, test, and deploy; low latency.
- **Cons**: Scaling is coarse; hard to maintain as it grows; technology lock-in.
---
### Monostate
- **Definition**: A variation of Singleton where all instances share the same state via static fields.
- **Intent**: Achieve Singleton-like behavior without enforcing a single instance.
- **Structure**: All instance methods operate on static data.
- **Pros**: Transparency (no special getInstance); multiple objects share same state.
- **Cons**: Hidden dependencies; can be confusing.
---
### Multiton
- **Definition**: A creational pattern that ensures a class has a named instances, each identified by a key.
- **Intent**: Control the number of instances per key, similar to a registry of singletons.
- **Structure**: Map from key to instance; getInstance(key) returns or creates.
- **Pros**: Manages a fixed set of instances; easy access.
- **Cons**: Global state; may lead to memory leaks if keys not cleaned.
---
### Mute Idiom
- **Definition**: A pattern that suppresses exceptions by swallowing them, often used when exceptions are impossible or should be ignored.
- **Intent**: Avoid clutter when an exception cannot occur or is irrelevant.
- **Structure**: try-catch block with empty catch or logging at debug level.
- **Pros**: Simplicity; avoids noise.
- **Cons**: Can hide critical errors; misuse leads to silent failures.
---
### Naked Objects
- **Definition**: A pattern where domain objects are directly exposed as the UI, with automatic generation of views.
- **Intent**: Reduce development time by having UI derived from domain model.
- **Structure**: Framework generates UI from domain objects; user interacts directly with objects.
- **Pros**: Rapid development; consistency; focuses on domain.
- **Cons**: Limited UI customization; not suitable for complex interactions.
---
### Notification
- **Definition**: A pattern where an object notifies others of state changes, often via events or callbacks.
- **Intent**: Decouple event source from observers.
- **Structure**: Subject maintains list of observers; observers register and are notified.
- **Pros**: Loose coupling; dynamic subscriptions.
- **Cons**: Notification storms; may lead to performance issues.
---
### Null Object
- **Definition**: A pattern that provides a default object with neutral behavior instead of null references.
- **Intent**: Avoid null checks and null pointer exceptions.
- **Structure**: Null object implements the same interface as real object but does nothing or returns defaults.
- **Pros**: Simplifies code; eliminates conditionals.
- **Cons**: May hide errors; need to ensure null object behavior is appropriate.
---
### Object Mother
- **Definition**: A pattern for creating test data, centralizing the creation of objects for tests.
- **Intent**: Simplify test setup by providing factory methods for common test objects.
- **Structure**: ObjectMother class with static methods returning pre-configured instances.
- **Pros**: Reduces duplication; makes tests more readable.
- **Cons**: May become cluttered; can hide test-specific variations.
---
### Object Pool
- **Definition**: A creational pattern that reuses objects from a fixed pool to avoid expensive creation/destruction.
- **Intent**: Improve performance for objects that are expensive to create (e.g., database connections).
- **Structure**: Pool manages a set of reusable objects; clients borrow and return.
- **Pros**: Reduces overhead; controls resource usage.
- **Cons**: Complexity in managing pool state; potential for leaks.
---
### Observer
- **Definition**: A behavioral pattern where an object (subject) maintains a list of dependents (observers) and notifies them of state changes.
- **Intent**: Establish a one-to-many dependency without tight coupling.
- **Structure**: Subject interface with attach/detach/notify; observers implement update.
- **Pros**: Loose coupling; supports broadcast communication.
- **Cons**: Unexpected updates; memory leaks if observers not detached.
---
### Optimistic Offline Lock
- **Definition**: A concurrency control pattern for databases where conflicts are detected at commit time.
- **Intent**: Avoid long-held locks by assuming conflicts are rare.
- **Structure**: Each record has a version number; update checks that version hasn't changed.
- **Pros**: High concurrency; simple.
- **Cons**: Rollback cost on conflict; requires retry logic.
---
### Page Controller
- **Definition**: A pattern in web applications where each page has its own controller handling requests.
- **Intent**: Keep logic for a specific page in one place.
- **Structure**: Controller per page; processes input, invokes model, selects view.
- **Pros**: Simple; easy to understand; good for small sites.
- **Cons**: Duplication across pages; not DRY.
---
### Page Object
- **Definition**: A pattern in UI testing where each page is represented by a class encapsulating its elements and behaviors.
- **Intent**: Reduce duplication and improve maintainability of tests.
- **Structure**: Page object exposes methods for interactions; tests use these methods.
- **Pros**: Centralizes page details; makes tests robust to UI changes.
- **Cons**: Overhead of maintaining page objects.
---
### Parameter Object
- **Definition**: A pattern that groups multiple related parameters into an object to pass to a method.
- **Intent**: Improve readability and reduce method signature clutter.
- **Structure**: Parameter class with fields; method accepts an instance.
- **Pros**: Simplifies method signatures; easier to add parameters.
- **Cons**: May create many small classes; if overused, hides required parameters.
---
### Partial Response
- **Definition**: A pattern where an API allows clients to specify which fields to return, reducing payload.
- **Intent**: Improve performance and bandwidth usage.
- **Structure**: Query parameters (e.g., `fields`) or GraphQL-like syntax; server filters response.
- **Pros**: Flexible; efficient; reduces over-fetching.
- **Cons**: Complexity in parsing; caching may be harder.
---
### Pipeline
- **Definition**: A pattern where data flows through a series of processing stages, each performing a transformation.
- **Intent**: Compose operations in a linear fashion, similar to assembly line.
- **Structure**: Pipeline with stages; each stage receives input, processes, passes to next.
- **Pros**: Decouples stages; reusable; easy to test.
- **Cons**: May introduce latency; error handling across stages.
---
### Poison Pill
- **Definition**: A special message placed in a queue to signal consumers to shut down.
- **Intent**: Gracefully stop processing in a producer-consumer setup.
- **Structure**: When consumer receives poison pill, it stops processing and possibly exits.
- **Pros**: Controlled shutdown; avoids lost messages.
- **Cons**: Must ensure all consumers see it; may need multiple pills.
---
### Presentation Model
- **Definition**: A pattern where a model (Presentation Model) represents the state and behavior of a UI independently of the views.
- **Intent**: Separate UI logic from view for testability.
- **Structure**: Presentation Model contains data and commands; view binds to it; updates reflect automatically.
- **Pros**: Testable; view-agnostic; supports multiple views.
- **Cons**: Complexity; may duplicate domain model.
---
### Private Class Data
- **Definition**: A pattern that restricts access to class data by encapsulating it in a separate object.
- **Intent**: Protect class internals and reduce exposure.
- **Structure**: Data class holds private fields with getters/setters; main class holds an instance.
- **Pros**: Encapsulation; reduces coupling; easier to change data representation.
- **Cons**: Indirection; may increase number of classes.
---
### Producer-Consumer
- **Definition**: A concurrency pattern where producers generate data and place it in a buffer, while consumers take and process it.
- **Intent**: Decouple production and consumption, allowing different rates.
- **Structure**: Shared queue with synchronization; producers put, consumers take.
- **Pros**: Smooths peaks; decoupling.
- **Cons**: Complexity of synchronization; buffer overflow risk.
---
### Promise
- **Definition**: A placeholder for a future result of an asynchronous operation.
- **Intent**: Simplify async programming by providing a composable abstraction.
- **Structure**: Promise object that can be resolved or rejected; then/catch for chaining.
- **Pros**: Avoids callback hell; composable; better error handling.
- **Cons**: May introduce complexity; learning curve.
---
### Property
- **Definition**: A pattern that provides a key-value store for objects, allowing dynamic attributes.
- **Intent**: Enable flexible addition of properties without changing class structure.
- **Structure**: Class has a map of property names to values; get/set methods.
- **Pros**: Dynamic; extensible; useful for configurations.
- **Cons**: Type safety lost; performance overhead.
---
### Prototype
- **Definition**: A creational pattern that creates new objects by cloning an existing instance (prototype).
- **Intent**: Avoid costly creation by copying pre-existing objects.
- **Structure**: Prototype interface with `clone()` method; concrete prototypes implement cloning.
- **Pros**: Hides complexities of object creation; reduces subclassing.
- **Cons**: Cloning may be complex (deep/shallow); circular references.
---
### Proxy
- **Definition**: A structural pattern that provides a surrogate or placeholder for another object to control access.
- **Intent**: Add indirection for lazy loading, access control, logging, etc.
- **Structure**: Proxy implements same interface as real subject; holds reference to subject.
- **Pros**: Controls access; can add behavior without modifying subject.
- **Cons**: Adds indirection; may degrade performance.
---
### Publish-Subscribe
- **Definition**: A messaging pattern where publishers send messages without knowing subscribers, and subscribers receive messages of interest.
- **Intent**: Decouple senders and receivers.
- **Structure**: Message broker or event bus manages subscriptions; publishers emit events; subscribers receive.
- **Pros**: Loose coupling; scalability; dynamic.
- **Cons**: Complexity; message delivery guarantees may be weak.
---
### Queue-Based Load Leveling
- **Definition**: A pattern that uses a queue to buffer requests and smooth out spikes in load.
- **Intent**: Protect backend services from sudden bursts.
- **Structure**: Requests placed in queue; workers process at controlled rate.
- **Pros**: Improves stability; prevents overload.
- **Cons**: Adds latency; queue management overhead.
---
### Reactor
- **Definition**: An event handling pattern that demultiplexes events and dispatches them to corresponding handlers.
- **Intent**: Efficiently handle multiple I/O events in a single thread.
- **Structure**: Reactor waits for events, then calls registered handlers for each event.
- **Pros**: High throughput; low overhead.
- **Cons**: Single-threaded may limit CPU-bound tasks; complex.
---
### Registry
- **Definition**: A pattern that provides a global lookup for objects or services.
- **Intent**: Centralize access to commonly used objects.
- **Structure**: Registry class with static methods to register and retrieve instances.
- **Pros**: Convenient; reduces lookup overhead.
- **Cons**: Global state; can lead to hidden dependencies.
---
### Repository
- **Definition**: A pattern that mediates between domain and data mapping layers, acting like an in-memory collection.
- **Intent**: Provide a collection-like interface for accessing domain objects.
- **Structure**: Repository interface with methods like `find`, `save`; concrete implementation uses data mapper.
- **Pros**: Decouples domain from persistence; easy to test; centralizes queries.
- **Cons**: May add complexity; can become bloated with many methods.
---
### Resource Acquisition Is Initialization (RAII)
- **Definition**: A C++ idiom where resource acquisition is tied to object lifetime, released automatically in destructor.
- **Intent**: Ensure proper release of resources (memory, locks, files) even in exceptions.
- **Structure**: Resource acquired in constructor; released in destructor.
- **Pros**: Exception-safe; deterministic cleanup; reduces leaks.
- **Cons**: Only in languages with deterministic destruction; requires careful copy semantics.
---
### Retry
- **Definition**: A pattern that automatically retries a failed operation with possible backoff.
- **Intent**: Handle transient failures in distributed systems.
- **Structure**: Retry logic with configurable attempts, delay, and backoff strategy.
- **Pros**: Improves resilience; simple to implement.
- **Cons**: May amplify load if not careful; indefinite retries can cause problems.
---
### Role Object
- **Definition**: A pattern where an object can dynamically acquire and remove roles (interfaces) at runtime.
- **Intent**: Allow objects to change behavior dynamically by attaching/detaching roles.
- **Structure**: Core object maintains a set of role objects; client accesses roles via specific interfaces.
- **Pros**: Flexible; supports dynamic behavior; avoids inheritance explosion.
- **Cons**: Complexity; role management overhead.
---
### Saga
- **Definition**: A pattern for managing distributed transactions using a sequence of local transactions with compensating actions.
- **Intent**: Ensure data consistency across microservices without two-phase commit.
- **Structure**: Saga orchestration (central coordinator) or choreography (events); each step has compensating action.
- **Pros**: Scalable; eventual consistency; avoids locks.
- **Cons**: Complexity; compensating logic may be hard; no isolation.
---
### Separated Interface
- **Definition**: A pattern where an interface is defined in a separate package from its implementation.
- **Intent**: Reduce coupling and allow multiple implementations.
- **Structure**: Interface in one module; implementation in another; client depends only on interface.
- **Pros**: Decouples; easier to swap implementations; facilitates testing.
- **Cons**: More modules; may be overkill for simple cases.
---
### Serialized Entity
- **Definition**: A pattern where an entity is serialized and stored as a blob, often used in NoSQL or caching.
- **Intent**: Simplify storage by avoiding complex relational mappings.
- **Structure**: Entity implements Serializable; stored as bytes in key-value store.
- **Pros**: Fast; simple; flexible schema.
- **Cons**: No querying by fields; versioning issues.
---
### Serialized LOB
- **Definition**: A pattern where large objects (LOBs) are serialized and stored in a database column.
- **Intent**: Store complex data structures without normalizing.
- **Structure**: Object serialized to bytes or XML and stored in a BLOB/CLOB column.
- **Pros**: Simple; good for object graphs.
- **Cons**: No SQL access to internal fields; versioning.
---
### Servant
- **Definition**: A pattern where a servant class provides behavior to a group of classes without being their superclass.
- **Intent**: Implement common operations for multiple unrelated classes.
- **Structure**: Servant has methods that operate on objects implementing a specific interface.
- **Pros**: Reuse across hierarchies; avoids duplication.
- **Cons**: May require those classes to expose necessary data.
---
### Server Session
- **Definition**: A pattern where session state is stored on the server (e.g., in memory or database).
- **Intent**: Maintain user state across requests.
- **Structure**: Session ID stored in cookie; server associates data with ID.
- **Pros**: Simple; secure (data not exposed).
- **Cons**: Memory overhead; scalability issues in distributed environments.
---
### Service Layer
- **Definition**: A pattern that defines an application's boundary with a layer of services that encapsulate business logic.
- **Intent**: Provide a clear API for the client and coordinate use cases.
- **Structure**: Service classes with methods corresponding to use cases; may use domain model.
- **Pros**: Encapsulates business logic; provides transaction boundaries; reusable.
- **Cons**: May become anemic if not careful; can grow large.
---
### Service Locator
- **Definition**: A pattern that provides a central registry for obtaining services.
- **Intent**: Decouple clients from service implementation classes.
- **Structure**: ServiceLocator class with methods to get services; it manages instances or lookup.
- **Pros**: Simplifies client code; centralizes service lookup.
- **Cons**: Hides dependencies; makes testing harder (global state).
---
### Service Stub
- **Definition**: A pattern where a stub implementation of a service is used for testing or prototyping.
- **Intent**: Simulate a service's behavior without real dependencies.
- **Structure**: Stub implements the service interface with fixed responses.
- **Pros**: Enables testing; fast; isolates from external systems.
- **Cons**: May not reflect real behavior; maintenance.
---
### Service to Worker
- **Definition**: A pattern that combines a Front Controller and a dispatcher with views and helpers.
- **Intent**: Centralize request handling and dispatch to appropriate views.
- **Structure**: Front Controller receives request, uses dispatcher to select view and helper, which may invoke model.
- **Pros**: Centralized control; reusable helpers.
- **Cons**: Complexity; may be overkill for simple apps.
---
### Session Facade
- **Definition**: A pattern that provides a coarse-grained facade over fine-grained business objects, typically in EJB.
- **Intent**: Reduce network calls by exposing a few high-level methods.
- **Structure**: Session bean that orchestrates multiple entity beans.
- **Pros**: Reduces remote calls; simplifies client; encapsulates workflow.
- **Cons**: May become bloated; ties client to specific workflow.
---
### Sharding
- **Definition**: A pattern where data is partitioned across multiple databases (shards) based on a key.
- **Intent**: Scale horizontally by distributing load.
- **Structure**: Sharding logic determines which shard holds each record; queries may need to fan out.
- **Pros**: Scalability; high throughput.
- **Cons**: Complexity in queries and transactions; rebalancing.
---
### Single Table Inheritance
- **Definition**: A pattern where an inheritance hierarchy is mapped to a single database table.
- **Intent**: Simplify persistence by storing all classes in one table.
- **Structure**: Table has columns for all attributes of the hierarchy, plus a discriminator column.
- **Pros**: Simple; no joins; good performance.
- **Cons**: Wasted space; table becomes wide; limited by row size.
---
### Singleton
- **Definition**: A creational pattern that ensures a class has only one instance and provides a global access point.
- **Intent**: Control access to a shared resource.
- **Structure**: Private constructor, static method returning the instance; thread-safe implementation.
- **Pros**: Controlled access; reduces namespace clutter.
- **Cons**: Global state; hard to test; can hide dependencies.
---
### Spatial Partition
- **Definition**: A pattern that organizes objects in space to quickly find nearby objects (e.g., quadtree, grid).
- **Intent**: Optimize spatial queries in games or simulations.
- **Structure**: Data structure partitions space; objects stored in cells; queries check relevant cells.
- **Pros**: Faster collision detection; scalable.
- **Cons**: Complexity; dynamic objects require updates.
---
### Special Case
- **Definition**: A pattern that handles exceptional cases by returning a special object instead of null or throwing.
- **Intent**: Simplify client code by providing consistent behavior for special cases.
- **Structure**: Subclass of the return type that implements specific behavior (e.g., MissingCustomer).
- **Pros**: Avoids null checks; encapsulates special-case logic.
- **Cons**: May lead to many small classes.
---
### Specification
- **Definition**: A pattern that represents business rules as combinable objects.
- **Intent**: Encapsulate business logic in reusable and composable specifications.
- **Structure**: Specification interface with `isSatisfiedBy` method; concrete specifications; can combine with AND/OR.
- **Pros**: Reusable; flexible; expressive.
- **Cons**: May increase complexity; performance overhead.
---
### State
- **Definition**: A behavioral pattern that allows an object to alter its behavior when its internal state changes.
- **Intent**: Encapsulate state-specific behavior into separate classes.
- **Structure**: Context holds a reference to a State object; State interface defines behavior; concrete states implement.
- **Pros**: Eliminates conditional statements; makes state transitions explicit.
- **Cons**: Many classes; state explosion.
---
### Step Builder
- **Definition**: A variation of Builder that forces a step-by-step construction process, often using interfaces.
- **Intent**: Ensure objects are built in a valid state by guiding the client through steps.
- **Structure**: Interfaces for each step; final build method returns object.
- **Pros**: Compile-time safety; readable; ensures completeness.
- **Cons**: Complex to implement; may be overkill.
---
### Strangler
- **Definition**: A pattern for incrementally replacing a legacy system by gradually building a new one around it.
- **Intent**: Mitigate risk by migrating functionality piece by piece.
- **Structure**: New system takes over parts of the old; requests are routed accordingly; eventually old system is strangled.
- **Pros**: Low risk; continuous delivery; allows rollback.
- **Cons**: Complexity of coexistence; routing logic.
---
### Strategy
- **Definition**: A behavioral pattern that defines a family of algorithms, encapsulates each, and makes them interchangeable.
- **Intent**: Let the algorithm vary independently from clients.
- **Structure**: Strategy interface; concrete strategies; context uses a strategy.
- **Pros**: Open/Closed; eliminates conditionals; runtime switching.
- **Cons**: Clients must know different strategies; communication overhead.
---
### Subclass Sandbox
- **Definition**: A pattern where base class defines a set of operations for subclasses to use, controlling how they interact with the game world.
- **Intent**: Provide a safe and controlled environment for subclasses to implement behavior.
- **Structure**: Base class provides protected methods; subclasses override a template method and use base operations.
- **Pros**: Reduces code duplication; enforces safety; simplifies subclassing.
- **Cons**: Base class may become bloated; limited flexibility.
---
### Table Inheritance
- **General term covering Single Table, Class Table, and Concrete Table Inheritance.**
- **Intent**: Map object inheritance to relational tables.
- **Single Table**: One table for whole hierarchy.
- **Class Table**: One table per class (includes inherited fields via joins).
- **Concrete Table**: One table per concrete class, with all fields.
- **Pros/Cons**: Single: simple but wasteful; Class: normalized but many joins; Concrete: no joins but duplicate columns.
---
### Table Module
- **Definition**: A pattern where a single class handles database access for a table, providing methods for queries and updates.
- **Intent**: Organize data access logic per table, similar to a record set.
- **Structure**: TableModule class with methods like `find`, `insert`, etc.; works on a record set (e.g., ADO.NET DataTable).
- **Pros**: Simple; matches table structure; good for simple apps.
- **Cons**: Not object-oriented; business logic may leak.
---
### Template Method
- **Definition**: A behavioral pattern that defines the skeleton of an algorithm in a method, deferring some steps to subclasses.
- **Intent**: Let subclasses redefine certain steps without changing the algorithm's structure.
- **Structure**: Abstract class with template method calling abstract or hook methods.
- **Pros**: Code reuse; controls extension points.
- **Cons**: Restricts flexibility; can be hard to follow.
---
### Template View
- **Definition**: A pattern where a view is built by embedding processing instructions (like JSP, ERB) within a template.
- **Intent**: Separate presentation from logic by using templates.
- **Structure**: Template file with placeholders and tags; processed by engine to generate output.
- **Pros**: Simple; designer-friendly; clear separation.
- **Cons**: Logic may creep into templates; hard to test.
---
### Thread-Pool Executor
- **Definition**: A pattern that manages a pool of worker threads to execute tasks concurrently.
- **Intent**: Avoid thread creation overhead and control resource usage.
- **Structure**: Thread pool with queue; tasks submitted; idle threads pick tasks.
- **Pros**: Efficient; limits concurrency; separates task submission from execution.
- **Cons**: Complexity; tuning pool size; task cancellation.
---
### Throttling
- **Definition**: A pattern that controls the rate of requests to a service to prevent overload.
- **Intent**: Protect system resources and ensure fair usage.
- **Structure**: Rate limiter (e.g., token bucket, leaky bucket) that rejects or delays requests over limit.
- **Pros**: Prevents abuse; maintains stability.
- **Cons**: May reject legitimate requests; adds latency.
---
### Tolerant Reader
- **Definition**: A pattern where a reader (consumer) is designed to be tolerant of changes in the message structure.
- **Intent**: Handle evolution of message formats without breaking.
- **Structure**: Reader extracts only the fields it needs, ignoring unknown fields.
- **Pros**: Robust to changes; easier to evolve contracts.
- **Cons**: May miss important new fields; requires careful design.
---
### Trampoline
- **Definition**: A technique to avoid stack overflow in recursive functions by using a loop that iteratively invokes thunks.
- **Intent**: Convert recursion into iteration to prevent deep call stacks.
- **Structure**: Function returns a thunk (or result); trampoline loop calls thunks until result.
- **Pros**: Enables tail recursion in languages without TCO; safe for deep recursion.
- **Cons**: Performance overhead; less intuitive.
---
### Transaction Script
- **Definition**: A pattern where business logic is organized as a single procedure per use case, directly manipulating data.
- **Intent**: Simple approach for small applications with simple logic.
- **Structure**: Class with methods for each transaction; each method contains all steps.
- **Pros**: Simple; easy to understand; fast development.
- **Cons**: Duplication; not scalable for complex logic; hard to maintain.
---
### Twin
- **Definition**: A pattern that allows multiple inheritance in languages that don't support it by using two classes that mirror each other.
- **Intent**: Simulate multiple inheritance by having a pair of objects that share a common interface.
- **Structure**: Two classes each handle part of the behavior; they hold references to each other.
- **Pros**: Provides multiple inheritance-like behavior.
- **Cons**: Complex; hard to maintain; requires synchronization.
---
### Type Object
- **Definition**: A pattern where a class defines a type, and instances of that class represent different types of objects.
- **Intent**: Allow new types to be created at runtime without modifying code.
- **Structure**: Type class with attributes; instances of the main object reference a type.
- **Pros**: Flexible; reduces subclassing; data-driven.
- **Cons**: Complexity; type checking at runtime.
---
### Unit of Work
- **Definition**: A pattern that maintains a list of objects affected by a transaction and coordinates their persistence.
- **Intent**: Ensure consistency and performance by batching changes.
- **Structure**: Unit of Work tracks new, dirty, removed objects; on commit, flushes to database.
- **Pros**: Optimizes database calls; ensures transactional consistency.
- **Cons**: Complexity; may hide individual operations.
---
### Update Method
- **Definition**: A pattern where objects have an `update()` method called once per frame to simulate continuous behavior.
- **Intent**: Handle game logic over time without threads.
- **Structure**: Game objects implement `update(float delta)`, called by game loop.
- **Pros**: Simple; deterministic; easy to manage.
- **Cons**: May become bottleneck if many objects; delta time issues.
---
### Value Object
- **Definition**: A small object whose equality is based on its values, not identity.
- **Intent**: Represent immutable concepts like money, dates.
- **Structure**: Immutable; fields set at construction; equals/hashCode overridden; no setters.
- **Pros**: Thread-safe; side-effect free; easier reasoning.
- **Cons**: May cause many small objects; performance overhead.
---
### Version Number
- **Definition**: A pattern for optimistic locking where each entity has a version field.
- **Intent**: Detect concurrent updates.
- **Structure**: Version number incremented on each update; update checks that version hasn't changed.
- **Pros**: Simple; effective for optimistic concurrency.
- **Cons**: Requires version column; retry logic needed.
---
### View Helper
- **Definition**: A pattern where a helper object encapsulates view logic and data retrieval, separate from the view template.
- **Intent**: Keep views clean by moving logic to helper classes.
- **Structure**: View uses helper to get data and format; helper may call model.
- **Pros**: Separates concerns; reusable helpers; testable.
- **Cons**: Extra classes; may over-abstract.
---
### Virtual Proxy
- **Definition**: A proxy that delays the creation or loading of an expensive object until it's actually needed.
- **Intent**: Improve performance by lazy loading.
- **Structure**: Proxy stands in for real object; on first request, loads/creates real object then forwards.
- **Pros**: Saves resources; transparent to client.
- **Cons**: Complexity; first access may be slow.
---
### Visitor
- **Definition**: A behavioral pattern that allows adding new operations to existing class hierarchies without modifying them.
- **Intent**: Separate algorithms from the objects they operate on.
- **Structure**: Visitor interface with visit methods for each element type; elements accept visitor and call back.
- **Pros**: Open/Closed; gathers related operations; double dispatch.
- **Cons**: Adding new elements requires changes to all visitors; may break encapsulation.
---
https://github.com/iluwatar/java-design-patterns/tree/master/abstract-document
---


