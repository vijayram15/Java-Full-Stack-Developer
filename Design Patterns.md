**1. Singleton Pattern:**

* **Purpose:** Ensures that a class has only one instance and provides a global point of access to it.
* **Implementation (Thread-Safe, Lazy Initialization):**

**Java**

```java
public class Singleton{

    private static volatile Singleton instance;

    private Singleton(){
        // Private constructor to prevent instantiation
    }

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

    // Example method
    public void doSomething(){
        System.out.println("Singleton doing something.");
    }
}
```

* **Interview Discussion:**
  * Explain the double-checked locking mechanism for thread safety and lazy initialization.
  * Discuss the use of `volatile` to prevent issues with instruction reordering.
  * Mention alternatives like enum singletons (Java 5+) for simplicity.

**2. Strategy Pattern:**

* **Purpose:** Defines a family of algorithms, encapsulates each one, and makes them interchangeable.
* **Implementation:**

**Java**

```java
interface PaymentStrategy{
    void pay(int amount);
}

class CreditCardPayment implements PaymentStrategy{
    @Override
    public void pay(int amount){
        System.out.println("Paid " + amount + " using credit card.");
    }
}

class PaypalPayment implements PaymentStrategy{
    @Override
    public void pay(int amount){
        System.out.println("Paid " + amount + " using PayPal.");
    }
}

class PaymentContext{
    private PaymentStrategy paymentStrategy;

    public void setPaymentStrategy(PaymentStrategy paymentStrategy){
        this.paymentStrategy = paymentStrategy;
    }

    public void processPayment(int amount){
        paymentStrategy.pay(amount);
    }
}

//Usage.
PaymentContext context = new PaymentContext();
context.setPaymentStrategy(new CreditCardPayment());
context.processPayment(100);
context.setPaymentStrategy(new PaypalPayment());
context.processPayment(50);
```

* **Interview Discussion:**
  * Explain how the Strategy pattern decouples the algorithm from the client.
  * Discuss the benefits of flexibility and extensibility.
  * Show how easy it is to add new payment types.

**3. Proxy Pattern:**

* **Purpose:** Provides a surrogate or placeholder for another object to control access to it.
* **Implementation (Virtual Proxy):**

**Java**

```java
interface Image{
    void display();
}

class RealImage implements Image{
    private String filename;

    public RealImage(String filename){
        this.filename = filename;
        loadImageFromDisk();
    }

    private void loadImageFromDisk(){
        System.out.println("Loading image: " + filename);
    }

    @Override
    public void display(){
        System.out.println("Displaying image: " + filename);
    }
}

class ImageProxy implements Image{
    private String filename;
    private RealImage realImage;

    public ImageProxy(String filename){
        this.filename = filename;
    }

    @Override
    public void display(){
        if (realImage == null) {
            realImage = new RealImage(filename);
        }
        realImage.display();
    }
}

//Usage.
Image image1 = new ImageProxy("image1.jpg");
image1.display();
```

* **Interview Discussion:**
  * Explain how the Proxy pattern can be used for lazy loading (virtual proxy), access control (protection proxy), or remote access (remote proxy).
  * Discuss the benefits of improved performance and security.
  * Show how the real image is only created when needed.

**4. Factory Pattern (Simple Factory):**

* **Purpose:** Creates objects without specifying the exact class of object that will be created.
* **Implementation:**

**Java**

```java
interface Shape{
    void draw();
}

class Circle implements Shape{
    @Override
    public void draw(){
        System.out.println("Drawing a circle.");
    }
}

class Square implements Shape{
    @Override
    public void draw(){
        System.out.println("Drawing a square.");
    }
}

class ShapeFactory{
    public static Shape getShape(String shapeType){
        if (shapeType.equalsIgnoreCase("circle")) {
            return new Circle();
        } elseif (shapeType.equalsIgnoreCase("square")) {
            return new Square();
        }
        return null;
    }
}

//Usage.
Shape shape1 = ShapeFactory.getShape("circle");
shape1.draw();
```

* **Interview Discussion:**
  * Explain how the Factory pattern encapsulates object creation logic.
  * Discuss the benefits of loose coupling and improved code organization.
  * Explain the difference between simple factory, factory method, and abstract factory.

**5. **Observer Pattern:****

**
*** **Purpose:** Defines a one-to-many dependency **between objects so that when one object changes state, all its dependents** are notified and updated automatically.

* **Implementation:**

**Java**

```java
import java.util.ArrayList;
import java.util.List;

interface Observer{
    void update(String message);
}

class Subject{
    private List<Observer> observers = new ArrayList<>();
    private String message;

    public void attach(Observer observer){
        observers.add(observer);
    }

    public void detach(Observer observer){
        observers.remove(observer);
    }

    public void setMessage(String message){
        this.message = message;
        notifyObservers();
    }

    private void notifyObservers(){
        for (Observer observer : observers) {
            observer.update(message);
        }
    }
}

class ConcreteObserver implements Observer{
    private String name;

    public ConcreteObserver(String name){
        this.name = name;
    }

    @Override
    public void update(String message){
        System.out.println(name + " received message: " + message);
    }
}

//Usage.
Subject subject = new Subject();
Observer observer1 = new ConcreteObserver("Observer 1");
Observer observer2 = new ConcreteObserver("Observer 2");
subject.attach(observer1);
subject.attach(observer2);
subject.setMessage("Hello, observers!");
```

* **Interview Discussion:**
  * Explain how the Observer pattern facilitates event handling and decoupled communication.
  * Discuss the benefits of loose coupling and flexibility.

**6. Adapter Pattern:**

* **Purpose:** Allows incompatible interfaces to work together. It acts as a wrapper that converts the interface of a class into another interface clients expect.
* **Implementation:**

**Java**

```java
// Target interface (what the client expects)
interface Target {
    void request();
}

// Adaptee (incompatible interface)
class Adaptee {
    public voidspecificRequest(){
        System.out.println("Adaptee's specific request.");
    }
}

// Adapter
class Adapter implements Target{
    private Adaptee adaptee;

    public Adapter(Adaptee adaptee){
        this.adaptee = adaptee;
    }

    @Override
    public void request(){
        adaptee.specificRequest(); // Adapts the request
    }
}

// Usage
Adaptee adaptee = new Adaptee();
Target target = new Adapter(adaptee);
target.request(); // Uses the adapted interface
```

* **Interview Discussion:**
  * Explain how the Adapter pattern bridges the gap between incompatible interfaces.
  * Discuss scenarios where it's used (e.g., integrating legacy code).

**7. Decorator Pattern:**

* **Purpose:** Dynamically adds responsibilities **to an object without altering its structure. It provides a flexible alternative to subclassing for extending functionality.**^1^
* **Implementation:**

**Java**

```java
// Component interface
interface Component{
    void operation();
}

// Concrete component
class ConcreteComponent implements Component{
    @Override
    public void operation(){
        System.out.println("Concrete component operation.");
    }
}

// Decorator abstract class
abstract class Decorator implements Component{
    protected Component component;

    public Decorator(Component component){
        this.component = component;
    }

    @Override
    public void operation(){
        component.operation();
    }
}

// Concrete decorator
class ConcreteDecoratorA extends Decorator{
    public ConcreteDecoratorA(Component component){
        super(component);
    }

    @Override
    public void operation(){
        super.operation();
        System.out.println("Concrete decorator A operation.");
    }
}

// Usage
Component component = new ConcreteComponent();
Component decoratedComponent = new ConcreteDecoratorA(component);
decoratedComponent.operation();
```

* **Interview Discussion:**
  * Explain how the Decorator pattern adds functionality at runtime.
  * Discuss its advantages over inheritance.

**8. Template Method Pattern:**

* **Purpose:** **Defines the skeleton of an algorithm in a base class, allowing subclasses to override specific steps without changing the algorithm's structure.**^2^
* **Implementation:**

**Java**

```java
// Abstract class with template method
abstract class Template {
    public void templateMethod() {
        primitiveOperation1();
        primitiveOperation2();
        hook(); // Optional hook
    }

    protected abstract void primitiveOperation1();

    protected abstract void primitiveOperation2();

    protected void hook() {
        // Optional hook implementation
    }
}

// Concrete class
class ConcreteTemplate extends Template {
    @Override
    protected void primitiveOperation1() {
        System.out.println("Concrete template operation 1.");
    }

    @Override
    protected void primitiveOperation2() {
        System.out.println("Concrete template operation 2.");
    }

    @Override
    protected void hook() {
        System.out.println("Concrete template hook.");
    }
}

// Usage
Template template = new ConcreteTemplate();
template.templateMethod();
```

* **Interview Discussion:**
  * Explain how the Template Method pattern promotes code reuse and defines a common algorithm.
  * Explain the use of the hook method.

**9. Builder Pattern:**

* **Purpose:** **Separates the construction of a complex object from its representation so that the same construction process can create different representations.**^3^
* **Implementation:**

**Java**

```java
// Product class
class Computer{
    private String cpu;
    private String ram;
    private String storage;

    public Computer(String cpu, String ram, String storage){
        this.cpu = cpu;
        this.ram = ram;
        this.storage = storage;
    }

    // Getters
    public String getCpu(){return cpu;}
    public String getRam(){return ram;}
    public String getStorage(){return storage;}
}

// Builder interface
interface ComputerBuilder{
    ComputerBuilder setCpu(String cpu);
    ComputerBuilder setRam(String ram);
    ComputerBuilder setStorage(String storage);
    Computer build();
}

// Concrete builder
class ConcreteComputerBuilder implements ComputerBuilder{
    private String cpu;
    private String ram;
    private String storage;

    @Override
    public ComputerBuilder setCpu(String cpu){
        this.cpu = cpu;
        returnthis;
    }

    @Override
    public ComputerBuilder setRam(String ram){
        this.ram = ram;
        returnthis;
    }

    @Override
    public ComputerBuilder setStorage(String storage){
        this.storage = storage;
        returnthis;
    }

    @Override
    public Computer build(){
        returnnew Computer(cpu, ram, storage);
    }
}

// Usage
ComputerBuilder builder = new ConcreteComputerBuilder();
Computer computer = builder.setCpu("Intel i7").setRam("16GB").setStorage("1TB SSD").build();
System.out.println(computer.getCpu());
```

* **Interview Discussion:**
  * Explain how the Builder pattern simplifies the construction of complex objects.
  * Discuss its advantages over constructors with many parameters.

These patterns, provide a strong foundation for designing robust and maintainable Java applications.
