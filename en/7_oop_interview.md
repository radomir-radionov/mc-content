# OOP — Interview

Answers are filled for every question in this note.

[← Back to hub](./0_system_design.md)

### Related docs

- [OOP & SOLID](./7_special_engineering.md) · [OOP — Interview](./7_oop_interview.md) · [System design — Interview](./0_system_design_interview.md)

### Sections

- [Interview Practice](#interview-practice)

---

## Interview Practice

Questions from [Devinterview.io — OOP](https://devinterview.io/questions/web-and-mobile-development/oop-interview-questions/).

### OOP Fundamentals

<details>
<summary>What is Object-Oriented Programming (OOP)?</summary>

### What is Object-Oriented Programming (OOP)?

**Definition:**

Object-Oriented Programming is a programming paradigm that organizes code into self-contained objects.

Each object combines data and the methods that operate on that data, resulting in more modular, flexible, and reusable code.

**How it works:**

Classes and Objects. A class serves as a blueprint for creating objects, defining their attributes and methods. An object represents an instance of a class. It contains both data (attributes of the class) and the methods designed to manipulate that data.

Encapsulation refers to the bundling of data and related methods within a class, keeping them hidden to protect their integrity. External code accesses them through public methods, often referred to as getters (to read the attributes) and setters (to change them).

Inheritance allows for the creation of new classes that inherit properties and methods of existing classes. It promotes code reuse and establishes a hierarchical relationship. The class being inherited from is the base class or superclass, while the class inheriting is the derived class or subclass.

Polymorphism allows objects of derived classes (subclasses) to be treated as objects of their base class (the superclass) but still maintain their unique behaviors. This enables objects to respond uniquely to the same method call, especially when various classes share a method name but have differing implementations.

- Compile-Time Polymorphism: Determined before the code runs, typically using method overloading.
- Run-Time Polymorphism: Determined while the code is running, typically through method overriding.

Abstraction is all about creating a simple interface that exposes only relevant and necessary functionalities.

- Abstract Classes: Generalized classes, therefore not instantiated directly. They serve as base classes for derived ones.
- Interfaces: Set guidelines for classes, specifying which methods they must implement.

Associations. Objects often have relationships with other objects, such as one object using or being composed of others. Such relationships are defined as associations.

- Aggregation: One object owns or contains the other, indicated by a "has-a" relationship.
- Composition: A stronger form of aggregation, where the child cannot exist without the parent.

**Pluses and minuses:**

- Modularity: Objects are organized into standalone entities and communicate through public interfaces, promoting code separation and easier maintenance.
- Reusability: Objects and class hierarchies can be reused in different projects. Additionally, inheritance allows classes to acquire attributes and behaviors from parent classes.
- Extensibility: New features can be incorporated through class inheritance and interface implementation.
- Flexibility: Polymorphism enables objects to adapt their behavior based on context.

**Examples:**

```java
// Abstract base class
abstract class Animal {
    private String name;

    public Animal(String name) {
        this.name = name;
    }

    public String getName() {
        return name;
    }

    // Abstract method
    public abstract void makeSound();

    public void sleep() {
        System.out.println(name + " is sleeping.");
    }
}

// Derived class
class Lion extends Animal {
    public Lion(String name) {
        super(name);
    }

    @Override
    public void makeSound() {
        System.out.println("Roar!");
    }
}

// Derived class
class Parrot extends Animal {
    private String color;

    public Parrot(String name, String color) {
        super(name);
        this.color = color;
    }

    public String getColor() {
        return color;
    }

    @Override
    public void makeSound() {
        System.out.println("Squawk!");
    }
}

public class Zoo {
    public static void main(String[] args) {
        Lion simba = new Lion("Simba");
        Parrot rio = new Parrot("Rio", "Blue");

        System.out.println(simba.getName() + " is a Lion.");
        simba.makeSound();
        simba.sleep();

        System.out.println(rio.getName() + " is a " + rio.getColor() + " Parrot.");
        rio.makeSound();
        rio.sleep();
    }
}
```

In the example:

- Classes and Objects: `Animal` is a class, while `simba` and `rio` are objects.
- Encapsulation: `Animal`'s `name` attribute and `Parrot`'s `color` attribute are encapsulated with private visibility, and public getter methods are provided.
- Inheritance: Both `Lion` and `Parrot` classes inherit from the `Animal` class.
- Polymorphism: Both `Lion` and `Parrot` provide their own implementation of the `makeSound()` method, even though they are treated as `Animal` objects.
- Abstraction: The `Animal` class contains the abstract method `makeSound()`, ensuring derived classes provide their own implementation.

**Sources:**

- [Devinterview.io — OOP](https://devinterview.io/questions/web-and-mobile-development/oop-interview-questions/)

</details>

<details>
<summary>What is the difference between procedural and Object-Oriented programming?</summary>

### What is the difference between procedural and Object-Oriented programming?

**Definition:**

Procedural and Object-Oriented Programming (OOP) are distinct programming paradigms. While procedural programming is linear and task-centric, OOP emphasizes an organic model, where data is encapsulated in objects.

**How it works:**

Data and function management.

- Procedural: Functions act on data. Data is often global, leading to potential conflicts.
- OOP: Data and functions are bundled within objects. Objects interact through methods, ensuring data integrity and encapsulation.

Code abstraction and reusability.

- Procedural: Code is often segmented into functions. Global data can negatively affect reusability.
- OOP: Abstraction is achieved through classes and objects. Encapsulation helps create discrete, self-contained modules.

Inheritance and polymorphism.

- Procedural: Inheritance and polymorphism are absent.
- OOP: Inheritance fosters code reusability while polymorphism allows objects of different classes to be treated as instances of a shared superclass or interface.

OOP vs. procedural in different languages.

- Procedural: C is a prominent example. It's centered around the procedural approach where code is organized as a series of tasks or procedures. While C can emulate certain OOP features, like using structs to group related data, it doesn't inherently support the full spectrum of OOP.
- OOP: Java is intrinsically designed around OOP. It fully supports classes, inheritance, encapsulation, and other OOP principles.

In practice, many modern languages like Python, JavaScript, and C# support multiple paradigms, offering flexibility in choosing the right approach for a given task.

**Examples:**

Procedural approach:

```python
class Animal:
    def __init__(self, sound):
        self.sound = sound

def make_sound(animal):
    print(animal.sound)

dog = Animal("Woof")
cat = Animal("Meow")

make_sound(dog)
make_sound(cat)
```

Object-oriented approach:

```java
import java.util.ArrayList;
import java.util.List;

abstract class Animal {
    public abstract void makeSound();
}

class Dog extends Animal {
    public void makeSound() {
        System.out.println("Woof");
    }
}

class Cat extends Animal {
    public void makeSound() {
        System.out.println("Meow");
    }
}

public class Main {
    public static void main(String[] args) {
        List<Animal> animals = new ArrayList<>();
        animals.add(new Dog());
        animals.add(new Cat());

        for (Animal animal : animals) {
            animal.makeSound();
        }
    }
}
```

**Sources:**

- [Devinterview.io — OOP](https://devinterview.io/questions/web-and-mobile-development/oop-interview-questions/)

</details>

<details>
<summary>What is encapsulation?</summary>

### What is encapsulation?

**Definition:**

Encapsulation is an object-oriented programming principle that combines data and methods that act on that data within a single unit, known as an object.

**How it works:**

- Information Hiding: Encapsulation hides internal state and implementation details.
- Access Control: Data is made available to external code only through defined methods: getters for reading and setters for writing.
- Constraining Behavior: Access methods can ensure that data adheres to specific rules (for example, range checks or formatting standards).

**Pluses and minuses:**

- Security: Offers controlled access to object data, reducing the risk of data corruption or unauthorized modifications.
- Simplicity: Objects abstract complex systems, presenting a simple interface for interaction.
- Flexibility: Encapsulation promotes loose coupling, making it easier to modify or replace object internals without impacting the external code.

**When to use:**

- Class Construction: Modern programming languages like Java and C# follow an "encapsulation first" approach, utilizing access specifiers like `public`, `private`, and `protected`.
- API Design: Encapsulating classes and modules helps create intuitive and focused APIs. It allows hiding internal strategies while exposing the desired functionality.
- Testing: Data hiding helps prevent direct access to object internals during testing, ensuring proper evaluation of object behavior through its public interface.

**Examples:**

```java
public class Car {
    private int fuel;  // private ensures that fuel can't be accessed directly from outside the class

    public Car() {
        this.fuel = 100;  // Initialize with 100 units of fuel
    }

    // Getter method for fuel
    public int getFuel() {
        return fuel;
    }

    // Setter method for fuel with encapsulation enforcing constraints
    public void setFuel(int fuel) {
        if (fuel >= 0 && fuel <= 100) {
            this.fuel = fuel;
        } else {
            System.out.println("Invalid fuel amount.");
        }
    }

    public void drive() {
        if (fuel > 0) {
            fuel--;
            System.out.println("Vroom!");
        } else {
            System.out.println("Out of fuel!");
        }
    }

    public static void main(String[] args) {
        Car myCar = new Car();
        myCar.drive();
        System.out.println("Fuel remaining: " + myCar.getFuel());
        myCar.setFuel(120);  // This will print "Invalid fuel amount."
    }
}
```

**Sources:**

- [Devinterview.io — OOP](https://devinterview.io/questions/web-and-mobile-development/oop-interview-questions/)

</details>

<details>
<summary>What is polymorphism? Explain overriding and overloading.</summary>

### What is polymorphism? Explain overriding and overloading.

**Definition:**

Polymorphism in object-oriented programming allows objects of different types to be treated as if they belong to the same type through a shared interface. It abstracts method details, promoting code flexibility and reusability.

**How it works:**

Overloading (compile-time polymorphism). Multiple methods in the same class can have the same name but different parameters, allowing them to coexist. The compiler selects the appropriate method based on the method signature.

Overriding (runtime polymorphism). A subclass provides a specific implementation of a method that is already defined in its parent class, effectively replacing the parent's version. The method to be called is determined during the program's execution.

Virtual methods (in languages like C++). Methods marked `virtual` in the base class can be overridden in derived classes. They enable dynamic dispatch, ensuring the correct method is called, even through base class references.

Polymorphism often leverages dynamic dispatch, a mechanism that determines at runtime which specific method to invoke. This is also known as late binding. This feature enables generic code that can handle multiple object types, enriching flexibility and adaptability.

**Examples:**

Overloading:

```java
public class Sum {
    public int add(int a, int b) {
        return a + b;
    }

    public double add(double a, double b) {
        return a + b;
    }
}
```

Overriding:

```python
class Animal:
    def speak(self):
        return "Animal speaks"

class Cat(Animal):
    def speak(self):
        return "Cat meows"

obj_cat = Cat()
print(obj_cat.speak())  # Output: Cat meows
```

Virtual methods:

```cpp
#include <iostream>
using namespace std;

class Animal {
public:
    virtual void speak() {
        cout << "Animal speaks";
    }
};

class Cat : public Animal {
public:
    void speak() override {
        cout << "Cat meows";
    }
};
```

**Sources:**

- [Devinterview.io — OOP](https://devinterview.io/questions/web-and-mobile-development/oop-interview-questions/)

</details>

<details>
<summary>What is inheritance? Name some types of inheritance.</summary>

### What is inheritance? Name some types of inheritance.

**Definition:**

Inheritance is a fundamental concept in object-oriented programming that allows for the creation of new classes based on existing ones.

Inheritance establishes an "is-a" relationship, where the derived class (also called the subclass or child class) inherits properties and behaviors from its parent, or base class.

**How it works:**

- Single Inheritance: A class inherits from only one base class. Common in languages like Java. Example: Class B inherits from class A.
- Multiple Inheritance: A class inherits from multiple base classes. While C++ supports it, languages like Java use interfaces to achieve a similar effect without the complexities associated with the "diamond problem." Example: Class C inherits from both classes A and B.
- Multilevel Inheritance: A class inherits from another class, which itself is a derived class. Example: Class C inherits from class B, which inherits from class A.
- Hierarchical Inheritance: One base class is inherited by multiple subclasses. Example: Both classes B and C inherit from class A.
- Hybrid Inheritance: A combination of two or more of the above inheritance types. Its use can lead to complexities. Example: In C++, a class can be involved in both multiple and multilevel inheritances.

**Examples:**

```java
// Single Inheritance
class A {
    void funcA() {
        System.out.println("Function of class A");
    }
}

class B extends A { }  // Class B inherits from class A


// Multiple Inheritance using interfaces
interface X {
    void funcX();
}

interface Y {
    void funcY();
}

class Z implements X, Y {  // Class Z implements both interface X and interface Y
    public void funcX() {
        System.out.println("Function of interface X");
    }

    public void funcY() {
        System.out.println("Function of interface Y");
    }
}


// Multilevel Inheritance
class A {
    void funcA() {
        System.out.println("Function of class A");
    }
}

class B extends A {
    void funcB() {
        System.out.println("Function of class B");
    }
}

class C extends B { }  // Class C inherits from class B, which inherits from class A


// Hierarchical Inheritance
class H {
    void funcH() {
        System.out.println("Function of class H");
    }
}

class I extends H { }  // Class I inherits from class H

class J extends H { }  // Class J also inherits from class H


// Java does not directly support hybrid inheritance through classes.
// However, you can achieve something similar using interfaces, as shown with multiple inheritance.
```

**Sources:**

- [Devinterview.io — OOP](https://devinterview.io/questions/web-and-mobile-development/oop-interview-questions/)

</details>

<details>
<summary>What is an abstraction? Name some abstraction techniques.</summary>

### What is an abstraction? Name some abstraction techniques.

**Definition:**

Abstraction in object-oriented programming (OOP) is the concept of focusing on essential characteristics and hiding unnecessary details.

It enables clear separation between high-level concepts and their specific implementations, promoting code reusability, maintainability, and security.

**How it works:**

Abstract classes.

- Definition: An abstract class is a class that cannot be instantiated on its own. It serves as a foundation for other classes.
- Purpose: Abstract classes allow you to define base behaviors that can be overridden by derived classes, while also allowing you to provide some default implementations. This means that while you define certain methods as abstract (no implementation), others can have actual code.
- Use Case: If you have a set of functionalities where some methods have a default behavior but can be overridden, and others must be defined by any child class, you'd use an abstract class.

Interfaces.

- Definition: An interface is a contract or blueprint that classes adhere to. Unlike abstract classes, interfaces have no implementation details; they only declare method and property signatures.
- Purpose: Interfaces ensure a certain set of methods or properties exist in the classes that implement them. They guarantee a specific structure or behavior without dictating how that behavior is achieved.
- Use Case: When you want multiple classes to adhere to a specific contract, ensuring they all have the same methods (but possibly different implementations), you'd use an interface.

**Examples:**

```java
// Abstract Class
abstract class Vehicle {
    // Abstract method (no body)
    public abstract String start();

    // Regular method
    public String stop() {
        return "Vehicle stopped!";
    }
}

// Implementing the abstract class
class Car extends Vehicle {
    @Override
    public String start() {
        return "Car started!";
    }
}

// Interface
interface Drivable {
    // All methods in an interface are implicitly abstract
    void drive();
}

// Implementing both the abstract class and the interface
class Bike extends Vehicle implements Drivable {
    @Override
    public String start() {
        return "Bike started!";
    }

    @Override
    public void drive() {
        System.out.println("Bike is being driven!");
    }
}

public class Main {
    public static void main(String[] args) {
        Car car = new Car();
        System.out.println(car.start()); // Output: Car started!
        System.out.println(car.stop());  // Output: Vehicle stopped!

        Bike bike = new Bike();
        System.out.println(bike.start()); // Output: Bike started!
        bike.drive();                     // Output: Bike is being driven!
    }
}
```

**Sources:**

- [Devinterview.io — OOP](https://devinterview.io/questions/web-and-mobile-development/oop-interview-questions/)

</details>

<details>
<summary>What is a class in OOP?</summary>

### What is a class in OOP?

**Definition:**

Class in Object-Oriented Programming represents a blueprint for creating objects (instances). It encapsulates attributes (data) and behaviors (methods) under a unified structure.

When instantiated, each object can carry individual data, adhering to the blueprint provided by its class.

**How it works:**

- Attributes: They store the state of an object and can have varying data types.
- Methods: These are functions defined within the class, designed to operate on attributes or perform specific behaviors.

OOP principles in classes:

- Inheritance: Allows a subclass (child class) to inherit attributes and methods from a superclass (parent class).
- Encapsulation: Attributes and methods are bundled within a class, limiting direct access to ensure data safety.
- Polymorphism: Allows objects of different classes that implement the same interface to be used in a consistent manner.
- Abstraction: Hides internal details, providing a simplified interface. Achieved in classes by exposing only essential methods.

**Examples:**

```python
class Car:
    """A class to represent a car."""

    def __init__(self, make, model, year):
        """Initialize car attributes."""
        self.make = make
        self.model = model
        self.year = year
        self.fuel = 0

    def fill_tank(self, gallons):
        """Add fuel to the tank. Ensure the amount is positive."""
        if gallons > 0:
            self.fuel += gallons
        else:
            print("Invalid fuel amount.")

    def drive(self, distance=1):
        """Drive the car, consuming fuel based on distance."""
        if self.fuel >= distance:
            self.fuel -= distance
            print(f"Car drove {distance} unit(s). Remaining fuel: {self.fuel} units.")
        else:
            print("Insufficient fuel.")

# Create a Car object
my_car = Car("Honda", "Civic", 2022)

# Access its attributes and methods
my_car.fill_tank(10)

for _ in range(15):
    my_car.drive()
```

**Sources:**

- [Devinterview.io — OOP](https://devinterview.io/questions/web-and-mobile-development/oop-interview-questions/)

</details>

<details>
<summary>What is an object in OOP?</summary>

### What is an object in OOP?

**Definition:**

An object represents a specific instance of a class. It encapsulates both data (attributes) and behavior (methods) within a single unit.

**How it works:**

- Identity: Each object has a unique identity that distinguishes it from others.
- State: Defined by its attributes, an object's state can change throughout its existence.
- Behavior: The methods associated with the object describe its possible actions or operations.

Lifecycle of an object:

- Creation: Objects are created from a class through a process called instantiation.
- Manipulation: They may undergo state changes as attributes are modified, and methods are invoked.
- Destruction: Terminates the object's existence, often handled automatically by the programming language ("garbage collection") or explicitly through code.

**Examples:**

```python
class Dog:
    def __init__(self, name, breed):
        self.name = name
        self.breed = breed

    def bark(self):
        print("Woof!")

myDog = Dog("Buddy", "Golden Retriever")
```

**Sources:**

- [Devinterview.io — OOP](https://devinterview.io/questions/web-and-mobile-development/oop-interview-questions/)

</details>

<details>
<summary>How do access specifiers work and what are they typically?</summary>

### How do access specifiers work and what are they typically?

**Definition:**

In Object-Oriented Programming (OOP), access specifiers define the level of visibility and accessibility of class members.

**How it works:**

- Private: Members are only accessible within the defining class, ensuring data encapsulation.
- Protected: Members are accessible within the defining class and its subclasses.
- Public: Members are globally accessible to all classes.

Data Encapsulation: It ensures data integrity by hiding the class's internal state and exposing it through methods. This enables better control over the data and its access.

Inheritance and Abstraction: Access levels regulate visibility in the context of inheritance, allowing classes to interact while preserving encapsulation.

**Pluses and minuses:**

Common mistakes:

- Excessive Use of Getter/Setter Pairs: While they are useful for protecting data, avoid introducing methods that do not provide added functionality.
- Needless Use of Public Members: If a member does not require global access, consider using more restrictive access levels for better encapsulation.

Best practices:

- Favor Composition Over Inheritance: If classes are loosely related, hiding details and components ensures better code maintenance.
- Private by Default: Encapsulate whenever possible. By default, members should be private, and their visibility be broadened only if necessary.
- Minimal Use of Public Members: Use them sparingly for elements that genuinely need global access.
- Consistent Visibility: Aim for uniform visibility within a class to avoid confusion.

**Examples:**

```java
public class Car {
    private String make;  // Accessible only within class
    protected int year;   // Accessible within class and subclasses
    double price;         // Default visibility: package-private

    public void setMake(String make) {
        this.make = make;
    }

    public String getMake() {
        return make;
    }

    protected void startEngine() {
       System.out.println("Engine started!");
    }
}
```

**Sources:**

- [Devinterview.io — OOP](https://devinterview.io/questions/web-and-mobile-development/oop-interview-questions/)

</details>

<details>
<summary>Name some ways to overload a method.</summary>

### Name some ways to overload a method.

**Definition:**

Three common techniques overload a method.

**How it works:**

1. Changing Parameter Number. This method involves altering the number of parameters in different method signatures.
2. Adapting Parameter Order. Another overloading technique involves rearranging the order of parameters in methods to create unique signatures.
3. Varying Parameter Type. You can define methods with different types of parameters to enable overloading.

**Examples:**

Changing the number of parameters:

```java
public int calculateSum(int a, int b) { // Two parameters
    return a + b;
}

public int calculateSum(int a, int b, int c) { // Three parameters
    return a + b + c;
}
```

Adapting parameter order:

```java
public double calculateArea(double length, double width) { // Length, then width
    return length * width;
}

public double calculateArea(double radius) { // Just radius for a circle
    return Math.PI * radius * radius;
}
```

Varying parameter type:

```java
public void printDetails(String name, int age) {
    System.out.println("Name: " + name + " , Age: " + age);
}

public void printDetails(int id) {
    System.out.println("ID: " + id);
}

public void printDetails(double salary) {
    System.out.println("Salary: " + salary);
}
```

**Sources:**

- [Devinterview.io — OOP](https://devinterview.io/questions/web-and-mobile-development/oop-interview-questions/)

</details>

<details>
<summary>What is cohesion in OOP?</summary>

### What is cohesion in OOP?

**Definition:**

Cohesion in OOP refers to how closely the methods and data within a single class are related to one another. A highly cohesive class is focused on a specific task or responsibility, making it easier to maintain, understand, and ensure reliability.

High cohesion is a desired attribute because it means that methods and properties within a class work together in a unified manner. In contrast, low cohesion indicates that a class has multiple, often unrelated responsibilities, making it harder to understand and maintain.

**How it works:**

- Coincidental: Methods and properties within the class have no meaningful relationship.
- Logical: Methods are grouped based on some logic but lack a clear theme.
- Temporal: Methods are related by when they are executed, for example initialization methods.
- Procedural: Methods are executed in a specific sequence.
- Communicational: Methods work on the same set of data.
- Sequential: The output of one method serves as the input for another.
- Functional: All methods in the class contribute to a single well-defined task.

Of these, functional cohesion is the most desirable, as it closely aligns with the Single Responsibility Principle.

**Examples:**

```java
public class FileUtility {

    public String readFile(String fileName) {
        // Read a file
        return content;
    }

    public void writeToDatabase(String data) {
        // Write content to a database
    }

    public void clearCache() {
        // Clear application cache
    }

    public List<String> parseFile(String content) {
        // Parse file content
        return parsedData;
    }
}
```

This `FileUtility` class exhibits low cohesion as it mixes file operations, database writing, and cache management.

**Thoughts:**

- Single Responsibility Principle (SRP): Each class should have only one reason to change. This principle suggests that a class should focus on one task or responsibility.
- Encapsulation: Encourage data hiding, and expose data only through focused and related methods.

**Sources:**

- [Devinterview.io — OOP](https://devinterview.io/questions/web-and-mobile-development/oop-interview-questions/)

</details>

<details>
<summary>What is coupling in OOP?</summary>

### What is coupling in OOP?

**Definition:**

Coupling in OOP describes the degree of interdependence between classes or modules. It determines how closely different modules or classes are linked to each other, impacting the system's flexibility, maintainability, and testability.

Loose coupling is generally preferred in software design.

**How it works:**

- Content Coupling: This is the strongest form of coupling where one module directly accesses or modifies another module's internal data.
- Common Coupling: Multiple modules share access to common global data. Any change to this shared resource can affect all the modules that depend on it.
- Control Coupling: One module controls the flow of another by passing it control information, such as using flags.
- External Coupling: Classes or modules are linked by external factors, such as configuration files or data schemas.
- Stamp (or Data) Coupling: Modules share data structures and use only parts of them, requiring knowledge about the structure of the data being passed.
- Message Coupling: The lowest form of coupling where modules communicate only through standard interfaces, such as method calls or messages.

Relationship with SOLID principles:

- Single Responsibility Principle (SRP): Adhering to SRP typically results in low coupling since classes have a singular focus, thereby minimizing dependencies.
- Open-Closed Principle (OCP): Emphasizing extensibility without modification, OCP reduces the risk of tight coupling as extensions are typically made through interfaces or abstract classes.
- Liskov Substitution Principle (LSP): When derived classes can replace their base classes without side effects, there's often a reduction in coupling, ensuring modules can operate independently of the specific derived class in use.
- Interface Segregation Principle (ISP): By endorsing focused interfaces rather than "one-size-fits-all" ones, ISP naturally leads to decreased coupling as classes aren't forced to depend on methods they don't use.
- Dependency Inversion Principle (DIP): By relying on abstractions rather than concrete implementations, DIP promotes low coupling, making systems more modular and adaptable.

**Sources:**

- [Devinterview.io — OOP](https://devinterview.io/questions/web-and-mobile-development/oop-interview-questions/)

</details>


### Class Relationships and Design

<details>
<summary>What is a constructor and how is it used?</summary>

### What is a constructor and how is it used?

**Definition:**

A constructor is a special method in object-oriented programming used for initializing objects. It ensures that newly created objects are set up with appropriate initial state and any required resources.

**How it works:**

Purpose.

- A constructor ensures that an object is in a consistent state when created.
- It initializes and configures the object's attributes or fields.

Characteristics.

- Matches Object Definitions: Each class defines one or more constructors for its objects.
- Has a Class Name: Constructors are named after the class, making them easy to identify.
- No Return Type: They don't return a value, not even `void`.
- Automatic Invocation: They are invoked or called when an object is created.

Types of constructors.

- Default Constructors: No parameters. Automatically provided by the language if no constructor is defined.
- Parameterized Constructors: Accept parameters for custom initialization. Often used to provide initial values to internal attributes or fields.
- Copy Constructors: Accept another object of the same type and initialize the current object using values from it. Commonly used for deep copying in languages like C++.
- Static Constructors: Used to initialize static member variables or for other class-level actions. They don't take part in the creation of objects and can be absent in many OOP languages.

**Examples:**

```java
class Vehicle {
    private String vehicleType;
    private int wheels;

    // Default Constructor
    public Vehicle() {
        vehicleType = "Car";
        wheels = 4;
    }

    // Parameterized Constructor
    public Vehicle(String type, int wheelCount) {
        vehicleType = type;
        wheels = wheelCount;
    }

    public void showDetails() {
        System.out.println("Type of Vehicle: " + vehicleType);
        System.out.println("Number of Wheels: " + wheels);
    }

    public static void main(String[] args) {
        // Calling Default Constructor
        Vehicle car = new Vehicle();
        car.showDetails();

        // Calling Parameterized Constructor
        Vehicle bike = new Vehicle("Bike", 2);
        bike.showDetails();
    }
}
```

```cpp
#include <iostream>
using namespace std;

class Person {
    private:
        string name;
        int age;

    public:
        // Parameterized Constructor
        Person(string n, int a) {
            name = n;
            age = a;
        }

        void displayInfo() {
            cout << "Name: " << name << ", Age: " << age << endl;
        }
};

int main() {
    // Calling Parameterized Constructor
    Person p1 = Person("Alice", 25);
    p1.displayInfo();

    return 0;
}
```

```csharp
using System;

class Car {
    private string model;

    // Parameterized Constructor
    public Car(string m) {
        model = m;
    }

    static void Main() {
        // Calling Parameterized Constructor
        Car c = new Car("Toyota");
        Console.WriteLine("Car Model: " + c.model);
    }
}
```

**Sources:**

- [Devinterview.io — OOP](https://devinterview.io/questions/web-and-mobile-development/oop-interview-questions/)

</details>

<details>
<summary>Describe the concept of destructor or finalizer in OOP.</summary>

### Describe the concept of destructor or finalizer in OOP.

**Definition:**

Destructors, or class finalizers, are used in object-oriented programming to perform cleanup actions before an object is destroyed.

**How it works:**

Objects can be explicitly or implicitly destroyed:

- Explicit: The programmer calls a destructing function or method. Languages like C++ use a destructor.
- Implicit: The language or environment manages object destruction. Garbage collectors in Java, C#, and Python are examples.

A destructor is a special method that is automatically called right before an object goes out of scope or is explicitly destroyed. Its primary purpose is to release resources or perform other cleanup tasks, like closing files or releasing memory.

Use cases:

- Resource Management: Ensuring timely release of system resources.
- Memory Management: Helpful in non-garbage collected languages to prevent memory leaks.
- Logging Exit or Cleanup Actions: Commonly used to log an object's destruction or clean-up steps.

For memory management, garbage collection is a mechanism that specifically targets automatic memory reclamation. In languages like Java and C#, destructors may not be necessary due to reliable garbage collectors.

Java introduced the `finalize()` method, which is similar to a destructor. It's often discouraged in favor of using the `AutoCloseable` interface and the try-with-resources statement for enhanced resource management.

Destructors can manage a variety of resources in addition to memory, such as:

- Files: Ensuring files are closed to avoid data loss.
- Network Connections: Closing sockets to prevent resource leaks.
- Database Connections: Helpful in releasing database locks or sessions.
- Hardware Devices: For devices like cameras or sensors, freeing resources ensures they are available for other applications.

**Examples:**

```cpp
class Resource {
public:
    Resource() { std::cout << "Resource Acquired\n"; }
    ~Resource() { std::cout << "Resource Destroyed\n"; }
};

int main() {
    Resource r;
}  // Destructs r here
```

```cpp
#include <iostream>
#include <fstream>

class FileHandler {
public:
    std::ofstream file;
    FileHandler(std::string fileName) {
        file.open(fileName);
        if (file.is_open()) {
            std::cout << "File Opened\n";
        }
    }
    ~FileHandler() {
        if (file.is_open()) {
            file.close();
            std::cout << "File Closed\n";
        }
    }
};

int main() {
    FileHandler fh("sample.txt");
}  // Destructor called on fh, closes the file
```

**Sources:**

- [Devinterview.io — OOP](https://devinterview.io/questions/web-and-mobile-development/oop-interview-questions/)

</details>

<details>
<summary>Compare inheritance vs. mixin vs. composition.</summary>

### Compare inheritance vs. mixin vs. composition.

**Definition:**

Inheritance, mixins, and composition are all techniques in object-oriented programming that deal with code reuse and the relationship between objects or classes.

**How it works:**

Inheritance.

- Definition: A class (subclass) inherits properties and behaviors from another class (superclass).
- Relationship: "is-a" (for example, a Car is a Vehicle).
- Pros: Direct way to reuse code; establishes intuitive relationships.
- Cons: Can lead to complex hierarchies; potential for over-generalization.

Mixin.

- Definition: A class that offers methods to other classes without being a standalone entity. Common in languages without multiple inheritance, such as Python.
- Relationship: "kind-of-a" or "can-do-this" (for example, a Helicopter can fly like a Bird).
- Pros: Reuses code across classes; avoids deep inheritance issues.
- Cons: Method source can be unclear; potential name clashes.

Composition.

- Definition: Building objects by combining simpler ones. Emphasizes a "has-a" relationship.
- Relationship: "has-a" (for example, a Car has an Engine).
- Pros: Encourages modular design; components can be easily swapped.
- Cons: May need more design upfront; can require more boilerplate code.

**Thoughts:**

Many modern OOP design guidelines, like the composition over inheritance principle, suggest that unless there's a clear "is-a" relationship, it's often better to use composition as it's more flexible and leads to a more modular and maintainable design.

**Sources:**

- [Devinterview.io — OOP](https://devinterview.io/questions/web-and-mobile-development/oop-interview-questions/)

</details>

<details>
<summary>Explain the concept of an interface and how it differs from an abstract class.</summary>

### Explain the concept of an interface and how it differs from an abstract class.

**Definition:**

An interface is a contract of behavior: a type that names methods a class must provide. An abstract class is a class that cannot be instantiated and can mix unimplemented methods with shared fields and concrete methods.

**How it works:**

In Java, a class `implements` any number of interfaces and `extends` only one class, abstract or not. Interface fields are public, static, and final. Methods you declare on an interface are public; methods that are not `default` or `static` are implicitly abstract. An abstract class may declare non-static, non-final fields and public, protected, or private concrete methods. From Java 8, interfaces may also have `default` and `static` methods, but they still cannot hold per-instance state the way an abstract class can.

**When to use:**

Use an abstract class when closely related types share code, fields, or non-public methods. Use an interface when unrelated types should share a behavior (`Comparable`, `Cloneable`, `Map`) or when you need multiple inheritance of type.

**Examples:**

```java
interface Drawable {
    void draw();
}

abstract class Shape {
    int x, y;
    void moveTo(int newX, int newY) { x = newX; y = newY; }
    abstract double area();
}
```

**Sources:**

- [Abstract Methods and Classes](https://docs.oracle.com/javase/tutorial/java/IandI/abstract.html)
- [Difference Between Abstract Class and Interface in Java](https://www.geeksforgeeks.org/java/difference-between-abstract-class-and-interface-in-java/)
</details>

<details>
<summary>Can a class have multiple parents in a single-inheritance system?</summary>

### Can a class have multiple parents in a single-inheritance system?

**Definition:**

No. In a single-inheritance class system a class has one direct superclass. It can still satisfy several contracts by implementing multiple interfaces.

**How it works:**

Java allows single, multilevel, and hierarchical class inheritance, and it does not allow a class to extend more than one class. That rule avoids ambiguous method resolution. A class can implement several interfaces, and an interface can extend several interfaces. The implementing class must supply every abstract method from those interfaces. GeeksforGeeks describes class inheritance as an IS-A reuse of implementation and an interface as a CAN-DO contract.

**When to use:**

Keep one parent class when types share structure. Add interfaces when the same object must be usable through several unrelated contracts.

**Examples:**

```java
class Circle extends Shape implements Drawable, Colorable {
    public void draw() { /* ... */ }
    public void setColor(String color) { /* ... */ }
}
```

**Sources:**

- [Interfaces and Inheritance in Java](https://www.geeksforgeeks.org/java/interfaces-and-inheritance-in-java/)
- [Diamond Problem in Java](https://www.geeksforgeeks.org/java/diamond-problem-in-java/)
</details>

<details>
<summary>How would you design a class to prevent it from being subclassed?</summary>

### How would you design a class to prevent it from being subclassed?

**Definition:**

Declare the class `final`. A final class cannot be subclassed. Individual methods can be `final` when only those methods must stay fixed.

**How it works:**

Oracle uses `final` on a method to say subclasses cannot override it, and `final` on a class to say nobody can extend it. `String` is the usual example of a final class used for an immutable type. Methods called from constructors should generally be final: if a constructor calls a non-final method, a subclass can redefine that method with surprising results.

**When to use:**

Seal a class when its invariants must not change, especially for immutable values. Seal a method when one step must stay consistent even if the rest of the class is extended.

**Examples:**

```java
public final class Money {
    private final long cents;
    public Money(long cents) { this.cents = cents; }
    public long cents() { return cents; }
}
```

**Sources:**

- [Writing Final Classes and Methods](https://docs.oracle.com/javase/tutorial/java/IandI/final.html)
</details>

<details>
<summary>Explain the 'is-a' vs 'has-a' relationship in OOP.</summary>

### Explain the 'is-a' vs 'has-a' relationship in OOP.

**Definition:**

IS-A means a type is a subtype of another and is expressed with class inheritance. HAS-A means one object uses or contains another and is expressed with a field that refers to that object.

**How it works:**

GeeksforGeeks maps class inheritance to IS-A and an interface to CAN-DO. Association is a uses-a link between objects that can exist on their own. Aggregation is a weaker has-a: the part can outlive the whole. Composition is a stronger part-of: the part’s life cycle depends on the container. Inheritance couples the child to the parent, so a change in the parent ripples down. Composition delegates to another object and can swap that object, for example a `Car` that holds an `Engine` instead of extending `Engine`.

**When to use:**

Use IS-A only for a true subtype. Use HAS-A when you need reuse or a replaceable part without claiming the container is that part.

**Examples:**

```java
class Car {          // has-a
    Engine engine;
}
class ElectricCar extends Car { } // is-a Car
```

**Sources:**

- [Interfaces and Inheritance in Java](https://www.geeksforgeeks.org/java/interfaces-and-inheritance-in-java/)
- [Association, Composition and Aggregation in Java](https://www.geeksforgeeks.org/java/association-composition-aggregation-java/)
- [Favoring Composition Over Inheritance In Java](https://www.geeksforgeeks.org/java/favoring-composition-over-inheritance-in-java-with-examples/)
</details>

<details>
<summary>Explain how aggregation relationship is represented in OOP.</summary>

### Explain how aggregation relationship is represented in OOP.

**Definition:**

Aggregation is a has-a association with weak ownership. The contained object can exist independently of the container, and the two have independent life cycles.

**How it works:**

The container holds a reference, often a collection, that is supplied from outside rather than created as a private part. GeeksforGeeks uses a `Department` that holds a `List` of `Student` objects: students are created first and then passed in, so destroying the department does not imply destroying the students. In a UML class diagram this whole-part link is drawn as a solid line with a hollow diamond on the whole. Shared ownership is allowed. Composition is the stronger case: a filled diamond, exclusive ownership, and a part that cannot exist without the container.

**When to use:**

Model aggregation when the part is shared or survives the whole, such as students in a department or books in a library. Use composition when the part is created and destroyed with the owner, such as rooms owned by a house in the composition example.

**Examples:**

```java
class Department {
    private List<Student> students; // students exist on their own
    Department(String name, List<Student> students) {
        this.students = students;
    }
}
```

**Sources:**

- [Association, Composition and Aggregation in Java](https://www.geeksforgeeks.org/java/association-composition-aggregation-java/)
- [UML Class Diagram](https://www.geeksforgeeks.org/system-design/unified-modeling-language-uml-class-diagrams/)
</details>

<details>
<summary>What is method overriding, and what rules apply to it?</summary>

### What is method overriding, and what rules apply to it?

**Definition:**

Method overriding replaces an inherited instance method with a subclass method that has the same signature and a compatible return type, so the call uses the subclass behavior.

**How it works:**

The overriding method has the same name, parameter types, and return type as the superclass method. It may return a subtype (a covariant return). `@Override` asks the compiler to check that a superclass method really exists. The access of the override may be wider, not narrower: a protected method can become public, not private. Changing an instance method to static, or a static method to instance, is a compile-time error. A subclass static method with the same signature hides a superclass static method; which version runs depends on the class you name, not the runtime object. Instance methods are preferred over interface default methods. If two independent default methods conflict, the class must override them and can call `InterfaceName.super.method()`.

**Examples:**

```java
class Animal {
    public void speak() { System.out.println("..."); }
}
class Cat extends Animal {
    @Override
    public void speak() { System.out.println("meow"); }
}
```

**Sources:**

- [Overriding and Hiding Methods](https://docs.oracle.com/javase/tutorial/java/IandI/override.html)
</details>

<details>
<summary>Describe the use of static methods and when they are appropriate.</summary>

### Describe the use of static methods and when they are appropriate.

**Definition:**

A static method belongs to the class, not to an instance. You call it with the class name, without creating an object.

**How it works:**

Class methods can use class variables and other class methods directly. They cannot use instance fields, instance methods, or `this` unless they go through an object reference. A common use is to read or update a static field, such as `Bicycle.getNumberOfBicycles()` around a shared counter. `static` combined with `final` defines a constant such as `PI`. Calling a static method through an instance reference is legal and discouraged, because it hides that the method is a class method. A subclass static method hides, rather than overrides, a superclass static method.

**When to use:**

Use a static method for behavior that needs no per-object state: factories that only use their arguments, counters shared by every instance, and constants. Keep methods that read or change one object’s fields as instance methods.

**Examples:**

```java
public class Bicycle {
    private static int numberOfBicycles = 0;
    public static int getNumberOfBicycles() {
        return numberOfBicycles;
    }
}
```

**Sources:**

- [Understanding Class Members](https://docs.oracle.com/javase/tutorial/java/javaOO/classvars.html)
- [Overriding and Hiding Methods](https://docs.oracle.com/javase/tutorial/java/IandI/override.html)
</details>


### Advanced OOP Concepts

<details>
<summary>What is multiple inheritance and what are some of its disadvantages?</summary>

### What is multiple inheritance and what are some of its disadvantages?

**Definition:**

Multiple inheritance lets one type acquire members from more than one parent type. Java does not allow it for classes. It allows a class to implement multiple interfaces, which is multiple inheritance of type.

**How it works:**

The main disadvantage is ambiguity: if two parents define the same method, the child does not know which body to run. That is the diamond problem. A second disadvantage is a harder method order. Python’s C3 linearization can fail with `TypeError: Cannot create a consistent method resolution order` when the hierarchy is inconsistent or circular, and overlapping constructors can raise `TypeError: got multiple values for keyword argument`. Java avoids class-level ambiguity by allowing only one superclass. Interfaces can still clash once they have `default` methods; the class must override the method and choose `Parent.super.method()`.

**When to use:**

Prefer one superclass plus interfaces, or composition, unless the language’s multiple-inheritance rules (such as Python mixins with `super()`) are explicit and tested.

**Sources:**

- [Diamond Problem in Java](https://www.geeksforgeeks.org/java/diamond-problem-in-java/)
- [Interfaces and Inheritance in Java](https://www.geeksforgeeks.org/java/interfaces-and-inheritance-in-java/)
- [Python multiple inheritance and mixins](https://www.geeksforgeeks.org/python/how-to-fix-python-multiple-inheritance-generates-typeerror-got-multiple-values-for-keyword-argument/)
- [Consistent method resolution order](https://www.geeksforgeeks.org/python/how-to-fix-typeerror-cannot-create-a-consistent-method-resolution-order-mro-in-python/)
</details>

<details>
<summary>Can you explain the 'diamond problem' in multiple inheritance?</summary>

### Can you explain the 'diamond problem' in multiple inheritance?

**Definition:**

The diamond problem is the ambiguity that appears when a type inherits the same method through two parents that both inherit it from one shared ancestor.

**How it works:**

Java does not allow a class to extend two classes, so that class-shaped diamond cannot be written. From Java 8, `default` methods can recreate the ambiguity: two interfaces each provide `default void fun()`, and a class that implements both must override `fun`. It chooses a parent with `Parent1.super.fun()`. Oracle’s rule is the same: if two independently defined default methods conflict, the compiler errors until the class overrides them. Instance methods from a class win over interface default methods, and a more specific overridden default wins over a shared ancestor’s default.

**Examples:**

```java
interface Parent1 {
    default void fun() { System.out.println("Parent1"); }
}
interface Parent2 {
    default void fun() { System.out.println("Parent2"); }
}
class Child implements Parent1, Parent2 {
    @Override public void fun() { Parent1.super.fun(); }
}
```

**Sources:**

- [Diamond Problem in Java](https://www.geeksforgeeks.org/java/diamond-problem-in-java/)
- [Overriding and Hiding Methods](https://docs.oracle.com/javase/tutorial/java/IandI/override.html)
</details>

<details>
<summary>How does OOP languages support polymorphism under the hood?</summary>

### How does OOP languages support polymorphism under the hood?

**Definition:**

Runtime polymorphism selects the overriding instance method from the object’s actual class, not from the type of the variable that points at it.

**How it works:**

Oracle’s `Cat`/`Animal` example stores a `Cat` in an `Animal` variable. `myAnimal.testInstanceMethod()` runs `Cat`’s method. The hidden static method does not: `Animal.testClassMethod()` runs `Animal`’s static method, because hiding is resolved from the named class. The same rule covers interface references: a `Drawable` variable can point at a `Rectangle`, and `draw()` runs `Rectangle`’s method. An overriding method must keep a compatible signature so the call site compiled against the parent type is still valid. This note does not describe a specific virtual-table layout; the observable rule is dynamic dispatch of instance methods by the runtime class.

**Examples:**

```java
Animal myAnimal = new Cat();
myAnimal.testInstanceMethod(); // "The instance method in Cat"
Animal.testClassMethod();      // "The static method in Animal"
```

**Sources:**

- [Overriding and Hiding Methods](https://docs.oracle.com/javase/tutorial/java/IandI/override.html)
- [Difference Between Abstract Class and Interface in Java](https://www.geeksforgeeks.org/java/difference-between-abstract-class-and-interface-in-java/)
</details>

<details>
<summary>What are generics, and how can they be useful in OOP?</summary>

### What are generics, and how can they be useful in OOP?

**Definition:**

Generics let classes, interfaces, and methods take types as parameters, so one piece of code is reused with different input types.

**How it works:**

Formal parameters receive values; type parameters receive types. The Java compiler type-checks generic code and reports violations at compile time. A raw `List` needs a cast on `get`; `List<String>` returns `String` with no cast. The same generic algorithm can run on collections of different types and stay type-safe. In an object model this means a container or algorithm (`List<T>`, `Map<K,V>`) does not hard-code the element class.

**When to use:**

Use generics for collections, repositories, and algorithms that should work for many element types without casts or duplicated classes.

**Examples:**

```java
List<String> list = new ArrayList<String>();
list.add("hello");
String s = list.get(0); // no cast
```

**Sources:**

- [Why Use Generics?](https://docs.oracle.com/javase/tutorial/java/generics/why.html)
</details>

<details>
<summary>Explain the concept of object composition and its benefits.</summary>

### Explain the concept of object composition and its benefits.

**Definition:**

Object composition builds behavior by holding references to other objects and delegating to them. It is a has-a relationship. In the strong form, the part’s life cycle depends on the owner.

**How it works:**

A class contains other objects and calls them instead of inheriting their code. Components can be replaced without a new subclass, including at runtime, and the same component can be reused by many classes. Depending on an interface keeps the owner loosely coupled. GeeksforGeeks lists the practical gains as flexibility, testability (swap in a test double), encapsulation of the part, and avoiding a subclass for every combination of behaviors. Inheritance remains appropriate for a real is-a subtype; composition is the usual reuse tool when that subtype claim is false. A `Car` with an `Engine` field can switch a petrol engine for an electric one without changing `Car`’s inheritance.

**Examples:**

```java
class Car {
    private final Engine engine;
    Car(Engine engine) { this.engine = engine; }
    void drive() { engine.start(); }
}
```

**Sources:**

- [Favoring Composition Over Inheritance In Java](https://www.geeksforgeeks.org/java/favoring-composition-over-inheritance-in-java-with-examples/)
- [Association, Composition and Aggregation in Java](https://www.geeksforgeeks.org/java/association-composition-aggregation-java/)
- [Design Patterns and Best Practices Interview Questions](https://www.geeksforgeeks.org/interview-prep/design-patterns-best-practices-interview-questions-oops/)
</details>

<details>
<summary>What is Liskov Substitution Principle (LSP)? Provide some examples of violation and adherence.</summary>

### What is Liskov Substitution Principle (LSP)? Provide some examples of violation and adherence.

**Definition:**

If S is a subtype of T, objects of type T must be replaceable by objects of type S without changing the correctness of the program.

**How it works:**

The principle fails when a subclass overrides methods in a way that breaks what callers of the parent expect. That usually happens when inheritance is used only to reuse code. The classic case is a `Square` subclass of `Rectangle`: `setWidth` on a rectangle changes only width, but a square that also changes height surprises code written for rectangles. Adherence means a subtype keeps the parent contract. Oracle’s overriding rules support that: same signature, a covariant return at most, and access that is not narrower. Patterns such as Strategy, State, and Decorator use composition so behavior can vary without a false subtype.

**Examples:**

```java
// Violation: Square.setWidth also changes height.
// Adherence: a subtype honors the parent contract.
class Cat extends Animal {
    @Override public void speak() { System.out.println("meow"); }
}
```

**Sources:**

- [Design Patterns and Best Practices Interview Questions](https://www.geeksforgeeks.org/interview-prep/design-patterns-best-practices-interview-questions-oops/)
- [Overriding and Hiding Methods](https://docs.oracle.com/javase/tutorial/java/IandI/override.html)
</details>

<details>
<summary>What is the dependency inversion principle?</summary>

### What is the dependency inversion principle?

**Definition:**

High-level modules should not depend on low-level modules. Both should depend on abstractions. Abstractions should not depend on details; details should depend on abstractions.

**How it works:**

A class should rely on an interface or abstract type rather than a concrete class. A factory can centralize `new`: the caller asks for a `Report` and does not construct `PdfReport`. If the product becomes `ExcelReport`, the factory changes and the high-level code does not. Dependency injection goes further: the object does not construct its dependency at all. A framework or the caller passes a `PaymentProcessor` into `PaymentService`, often through the constructor. Tests can pass a mock instead of the real processor.

**Examples:**

```java
class PaymentService {
    private final PaymentProcessor processor;
    PaymentService(PaymentProcessor processor) {
        this.processor = processor;
    }
}
```

**Sources:**

- [SOLID Principles with Real Life Examples](https://www.geeksforgeeks.org/system-design/solid-principle-in-programming-understand-with-real-life-examples/)
- [Design Patterns and Best Practices Interview Questions](https://www.geeksforgeeks.org/interview-prep/design-patterns-best-practices-interview-questions-oops/)
- [A Pythonic Way for Dependency Injection](https://www.geeksforgeeks.org/python/what-is-a-pythonic-way-for-dependency-injection/)
</details>

<details>
<summary>How can the open/closed principle guide object-oriented design?</summary>

### How can the open/closed principle guide object-oriented design?

**Definition:**

Software entities should be open for extension and closed for modification: you add behavior without editing the entity that already works.

**How it works:**

New cases arrive as new classes that implement an existing abstraction, not as edits inside a method that already handles every case. GeeksforGeeks ties this to Factory, Template Method, and Observer. A factory or strategy lets you add a payment method or document type by adding a class. A template method is a caution: if every new requirement forces a change to the base algorithm and to every subclass, the base is not closed. Hooks with a default, or a Strategy for the varying step, keep the skeleton stable. SOLID’s loose coupling is the point: a change in one class does not force edits across unrelated classes.

**Sources:**

- [SOLID Principles with Real Life Examples](https://www.geeksforgeeks.org/system-design/solid-principle-in-programming-understand-with-real-life-examples/)
- [Design Patterns and Best Practices Interview Questions](https://www.geeksforgeeks.org/interview-prep/design-patterns-best-practices-interview-questions-oops/)
</details>

<details>
<summary>Describe how the Interface Segregation Principle affects system design.</summary>

### Describe how the Interface Segregation Principle affects system design.

**Definition:**

Clients should not be forced to depend on methods they do not use. Many small, client-specific interfaces are better than one fat interface.

**How it works:**

A fat interface couples every implementer to every method. Splitting it lets a class implement only the contracts it needs, which is the same idea as Java allowing many interfaces on one class. GeeksforGeeks connects this to Adapter and Bridge: narrow interfaces are easier to adapt and to bridge. Callers depend on the small type they actually call, so a change to an unrelated method does not recompile or reimplement them.

**Examples:**

```java
interface Printer { void print(Document doc); }
interface Scanner { Document scan(); }
class SimplePrinter implements Printer {
    public void print(Document doc) { /* ... */ }
}
```

**Sources:**

- [SOLID Principles with Real Life Examples](https://www.geeksforgeeks.org/system-design/solid-principle-in-programming-understand-with-real-life-examples/)
- [Design Patterns and Best Practices Interview Questions](https://www.geeksforgeeks.org/interview-prep/design-patterns-best-practices-interview-questions-oops/)
- [Interfaces and Inheritance in Java](https://www.geeksforgeeks.org/java/interfaces-and-inheritance-in-java/)
</details>

<details>
<summary>What is a mixin, and how does it differ from traditional inheritance?</summary>

### What is a mixin, and how does it differ from traditional inheritance?

**Definition:**

A mixin is a class meant to add one piece of behavior to other classes. It is not the primary parent that defines what the object is.

**How it works:**

Traditional inheritance is an is-a link to one main parent and reuses that parent’s structure. A mixin is a small base you combine to add a capability, which GeeksforGeeks suggests as a way to avoid a tangled multiple-inheritance hierarchy. In Python, `class C(MixinA, MixinB)` can take methods from both mixins. Constructors still need care: call each mixin’s initializer explicitly or use `super()` so C3 method resolution stays consistent. If the hierarchy is circular or the base order conflicts, Python raises `TypeError: Cannot create a consistent method resolution order`. Composition is the alternative when even mixins make the MRO hard to follow.

**When to use:**

Use a mixin to share one capability across otherwise unrelated classes. Use a normal superclass when there is a real is-a parent. Prefer composition if the mixin graph becomes ambiguous.

**Examples:**

```python
class JsonMixin:
    def to_json(self):
        return {"id": self.id}

class User(JsonMixin):
    def __init__(self, id):
        self.id = id
```

**Sources:**

- [Python multiple inheritance and mixins](https://www.geeksforgeeks.org/python/how-to-fix-python-multiple-inheritance-generates-typeerror-got-multiple-values-for-keyword-argument/)
- [Consistent method resolution order](https://www.geeksforgeeks.org/python/how-to-fix-typeerror-cannot-create-a-consistent-method-resolution-order-mro-in-python/)
- [Favoring Composition Over Inheritance In Java](https://www.geeksforgeeks.org/java/favoring-composition-over-inheritance-in-java-with-examples/)
</details>


### Practical Use and Patterns

<details>
<summary>How would you refactor a class that has too many responsibilities?</summary>

### How would you refactor a class that has too many responsibilities?

**Definition:**

Split it so each class has one reason to change. The Single Responsibility Principle says a class should have a single job.

**How it works:**

GeeksforGeeks’s bakery example keeps baking, inventory, supply orders, customer service, and cleaning in separate classes. Find the reasons the large class changes, move each reason into its own type, and let the original type delegate. If the pieces are not true subtypes, compose them instead of inheriting. Constructor injection makes the split testable: each collaborator can be replaced with a mock.

**Examples:**

```java
class OrderService {
    private final Inventory inventory;
    private final Payments payments;
    OrderService(Inventory inventory, Payments payments) {
        this.inventory = inventory;
        this.payments = payments;
    }
}
```

**Sources:**

- [SOLID Principles with Real Life Examples](https://www.geeksforgeeks.org/system-design/solid-principle-in-programming-understand-with-real-life-examples/)
- [Favoring Composition Over Inheritance In Java](https://www.geeksforgeeks.org/java/favoring-composition-over-inheritance-in-java-with-examples/)
- [A Pythonic Way for Dependency Injection](https://www.geeksforgeeks.org/python/what-is-a-pythonic-way-for-dependency-injection/)
</details>

<details>
<summary>Describe a singleton pattern and discuss its pros and cons.</summary>

### Describe a singleton pattern and discuss its pros and cons.

**Definition:**

The Singleton pattern keeps a single instance of a class and a global way to reach it.

**How it works:**

It is used for a shared logger, configuration, or cache. GeeksforGeeks also lists why it is often treated as an anti-pattern: the instance is global state, so tests cannot easily replace it; the class manages both its business logic and its own life cycle, which strains single responsibility; a careless implementation races in multiple threads; many callers become tightly coupled to that one object. Lazy creation can avoid building the instance at startup, and it still needs synchronization if several threads can create it. Prefer injecting one shared instance when tests must substitute it.

**When to use:**

Use it for a process-wide resource that truly must be unique, and keep construction thread-safe. Do not use it as a default place to store global data.

**Sources:**

- [Design Patterns and Best Practices Interview Questions](https://www.geeksforgeeks.org/interview-prep/design-patterns-best-practices-interview-questions-oops/)
</details>

<details>
<summary>What is a factory method, and when should it be used?</summary>

### What is a factory method, and when should it be used?

**Definition:**

A factory method defines creation in a method and lets a subclass, or the method’s body, decide which concrete object is returned.

**How it works:**

The caller depends on the product abstraction, not on `new PdfReport()`. GeeksforGeeks contrasts Factory Method with Abstract Factory: Factory Method varies one product (a `DocumentFactory` returns a word or PDF document); Abstract Factory creates a family of related products (button and checkbox together for one platform). Open/closed design follows: a new variant is a new class plus a factory branch or subclass, not a change at every call site. Dependency inversion is the principle; the factory is one way to apply it.

**When to use:**

Use a factory method when one product has several variants and callers should not name the concrete class. Use an abstract factory when several related products must be created as a consistent set.

**Examples:**

```java
interface Document { void open(); }
class PdfDocument implements Document {
    public void open() { /* ... */ }
}
class DocumentFactory {
    Document create(String kind) {
        if ("pdf".equals(kind)) return new PdfDocument();
        throw new IllegalArgumentException(kind);
    }
}
```

**Sources:**

- [Design Patterns and Best Practices Interview Questions](https://www.geeksforgeeks.org/interview-prep/design-patterns-best-practices-interview-questions-oops/)
</details>

<details>
<summary>Explain the builder pattern and where you might apply it.</summary>

### Explain the builder pattern and where you might apply it.

**Definition:**

The Builder pattern separates construction from the finished object so a complex object is assembled step by step through named methods.

**How it works:**

It targets the telescoping constructor problem: `new User("A", "B", null, 0, true, null)` is hard to read and easy to mis-order. A `UserBuilder` sets each property with a named method and `build()` returns the object. GeeksforGeeks notes that the finished object is often immutable, that new fields change the builder rather than a pile of constructors, and that the gains are readability, flexibility across variants, and fewer mistakes with nulls or argument order.

**When to use:**

Use a builder when an object has many optional fields. A short constructor is enough when every argument is required and the call stays readable.

**Examples:**

```java
User user = new UserBuilder()
    .setFirstName("Alice")
    .setAge(30)
    .setAddress("NYC")
    .build();
```

**Sources:**

- [Design Patterns and Best Practices Interview Questions](https://www.geeksforgeeks.org/interview-prep/design-patterns-best-practices-interview-questions-oops/)
</details>

<details>
<summary>What is the prototype pattern, and how does it relate to OOP?</summary>

### What is the prototype pattern, and how does it relate to OOP?

**Definition:**

The Prototype pattern creates a new object by cloning an existing one instead of constructing it from scratch.

**How it works:**

It is a creational alternative to a factory. GeeksforGeeks prefers it over building from scratch when creation is expensive (lookups, network, heavy computation), when the object graph is deep, or when instances differ only by a few fields. A factory still assembles each model step by step; a `SedanPrototype` can be copied and then adjusted, for example its color. In OOP terms the clone is another instance of the same class, with copied state, not a subclass.

**When to use:**

Clone a preconfigured instance when copies are cheaper or clearer than re-running construction. Use a factory when each product is assembled from inputs rather than copied.

**Sources:**

- [Design Patterns and Best Practices Interview Questions](https://www.geeksforgeeks.org/interview-prep/design-patterns-best-practices-interview-questions-oops/)
</details>

<details>
<summary>When would you use the Adapter pattern?</summary>

### When would you use the Adapter pattern?

**Definition:**

Use the Adapter pattern when a client expects one interface and an existing class offers another, and you need a wrapper that translates between them.

**How it works:**

The adapter implements the interface the client already calls and forwards to the adaptee. GeeksforGeeks warns that extra adapter layers add indirection, chained adapters hide a design problem, and an adapter should not replace fixing the underlying types. Interface segregation helps: a small target interface is easier to adapt than a fat one. Use it for real interoperability, such as a third-party type you cannot change.

**Examples:**

```java
class CelsiusSensor {
    double reading() { return 21.5; }
}
class FahrenheitAdapter implements Temperature {
    private final CelsiusSensor sensor;
    FahrenheitAdapter(CelsiusSensor sensor) { this.sensor = sensor; }
    public double fahrenheit() { return sensor.reading() * 9 / 5 + 32; }
}
```

**Sources:**

- [Design Patterns and Best Practices Interview Questions](https://www.geeksforgeeks.org/interview-prep/design-patterns-best-practices-interview-questions-oops/)
- [SOLID Principles with Real Life Examples](https://www.geeksforgeeks.org/system-design/solid-principle-in-programming-understand-with-real-life-examples/)
</details>

<details>
<summary>Can you explain the use of the Decorator pattern?</summary>

### Can you explain the use of the Decorator pattern?

**Definition:**

The Decorator pattern wraps an object that implements the same interface and adds behavior without changing the wrapped class.

**How it works:**

Its goal is augmentation, not access control. GeeksforGeeks contrasts it with Proxy: a proxy stands in to control access (lazy load, remote object, permission check), while a decorator adds behavior such as compression, buffering, or encryption on a stream, or a border on a widget. A logging wrapper around a bank account is a decorator; an authentication check before the account is a proxy. Too many stacked decorators cause the yo-yo problem: readers jump across layers to see what one call does. Add a decorator when the extra behavior must be optional and combinable.

**Examples:**

```java
class LoggingAccount implements Account {
    private final Account inner;
    LoggingAccount(Account inner) { this.inner = inner; }
    public void withdraw(int amount) {
        inner.withdraw(amount);
        System.out.println("withdrew " + amount);
    }
}
```

**Sources:**

- [Design Patterns and Best Practices Interview Questions](https://www.geeksforgeeks.org/interview-prep/design-patterns-best-practices-interview-questions-oops/)
</details>

<details>
<summary>Describe the Observer pattern and a scenario in which you might use it.</summary>

### Describe the Observer pattern and a scenario in which you might use it.

**Definition:**

The Observer pattern lets a subject notify registered dependents when its state changes, without the subject naming every concrete listener.

**How it works:**

A subject keeps references to observers and tells them about an event. That fits open/closed design: a new listener is registered, and the subject is not edited. A typical case is a GUI or a stock ticker: widgets or subscribers update when the model or price changes. GeeksforGeeks notes a leak: if an observer is not unsubscribed, the subject still holds it and garbage collection cannot reclaim it. Weak references, explicit deregistration, or a bus that cleans up addresses that. In a large mesh of notifications, a mediator can be clearer because colleagues talk through one coordinator instead of a web of observers.

**Sources:**

- [Design Patterns and Best Practices Interview Questions](https://www.geeksforgeeks.org/interview-prep/design-patterns-best-practices-interview-questions-oops/)
- [SOLID Principles with Real Life Examples](https://www.geeksforgeeks.org/system-design/solid-principle-in-programming-understand-with-real-life-examples/)
</details>

<details>
<summary>What are the advantages of using the Command pattern?</summary>

### What are the advantages of using the Command pattern?

**Definition:**

The Command pattern turns a request into an object, so the invoker does not call the receiver directly.

**How it works:**

Each operation can expose `execute` and `undo`. Executed commands go on a stack; undo pops the last command and calls `undo`, and redo runs it again or moves it from a redo stack. The command stores the state needed to reverse itself, so an editor, IDE, or drawing tool can undo typing, deletion, or a shape change without the button knowing the domain object. The same object can be queued, logged, or retried because the request is data.

**When to use:**

Use commands when operations must be undone, queued, or invoked by something that should not know the receiver. A direct method call is enough for a one-off action with no history.

**Sources:**

- [Design Patterns and Best Practices Interview Questions](https://www.geeksforgeeks.org/interview-prep/design-patterns-best-practices-interview-questions-oops/)
</details>

<details>
<summary>How does the Strategy pattern provide flexibility in objects?</summary>

### How does the Strategy pattern provide flexibility in objects?

**Definition:**

The Strategy pattern puts interchangeable algorithms in their own types and lets the context hold one of them.

**How it works:**

The context delegates the varying step to a strategy object. Callers swap sorting, payment, or shipping strategies without a subclass for each combination and without editing the context, which is the open/closed idea. GeeksforGeeks distinguishes Strategy from State: strategies are independent and chosen from outside; state objects change behavior because the object’s own mode changed, and transitions stay inside that model. Strategy is composition, so it avoids a subclass lattice and keeps each algorithm testable on its own.

**Examples:**

```java
interface Shipping { int cost(int grams); }
class Order {
    private final Shipping shipping;
    Order(Shipping shipping) { this.shipping = shipping; }
    int cost(int grams) { return shipping.cost(grams); }
}
```

**Sources:**

- [Design Patterns and Best Practices Interview Questions](https://www.geeksforgeeks.org/interview-prep/design-patterns-best-practices-interview-questions-oops/)
- [Favoring Composition Over Inheritance In Java](https://www.geeksforgeeks.org/java/favoring-composition-over-inheritance-in-java-with-examples/)
</details>


### OOP Best Practices

<details>
<summary>What are some common OOP design anti-patterns?</summary>

### What are some common OOP design anti-patterns?

**Definition:**

An OOP anti-pattern is a design that looks structured and still makes the code harder to change: god classes, inheritance used as reuse, global singletons, fat interfaces, and patterns copied in where they do not solve a problem.

**How it works:**

A class with many reasons to change breaks single responsibility. Inheriting to reuse code, such as `Square` extending `Rectangle` or `Car` extending `Engine`, breaks substitution or locks the hierarchy. A singleton that owns its life cycle and is reached globally resists tests. One interface that forces unused methods spreads dependencies. GeeksforGeeks calls out pattern obsession: an abstract factory plus a builder for one configuration object, or a chain of decorators that makes a call a yo-yo across classes. Extra wrappers also add indirection. The corrective rules on the same pages are one reason to change, is-a only for real subtypes, composition for replaceable parts, small interfaces, and a pattern only when the problem recurs.

**Sources:**

- [SOLID Principles with Real Life Examples](https://www.geeksforgeeks.org/system-design/solid-principle-in-programming-understand-with-real-life-examples/)
- [Design Patterns and Best Practices Interview Questions](https://www.geeksforgeeks.org/interview-prep/design-patterns-best-practices-interview-questions-oops/)
- [Favoring Composition Over Inheritance In Java](https://www.geeksforgeeks.org/java/favoring-composition-over-inheritance-in-java-with-examples/)
</details>

<details>
<summary>How do you ensure that your objects are properly encapsulated?</summary>

### How do you ensure that your objects are properly encapsulated?

**Definition:**

Encapsulation keeps an object’s state behind its methods so callers use the behavior you publish, not the fields.

**How it works:**

Oracle’s abstract-class guidance is the practical lever: fields can be non-public, and concrete methods can be private or protected. Keep instance fields private. Change them only in methods that preserve the object’s rules. Expose the smallest interface a client needs (interface segregation) so hidden methods are not part of the contract. A final class or final fields, as with an immutable `String`-style type, stop subclasses and reassignment from bypassing those rules. Composition hides a collaborator’s details inside the owner.

**Examples:**

```java
public final class Money {
    private final long cents;
    public Money(long cents) {
        if (cents < 0) throw new IllegalArgumentException("cents");
        this.cents = cents;
    }
    public long cents() { return cents; }
}
```

**Sources:**

- [Abstract Methods and Classes](https://docs.oracle.com/javase/tutorial/java/IandI/abstract.html)
- [Writing Final Classes and Methods](https://docs.oracle.com/javase/tutorial/java/IandI/final.html)
- [SOLID Principles with Real Life Examples](https://www.geeksforgeeks.org/system-design/solid-principle-in-programming-understand-with-real-life-examples/)
- [Favoring Composition Over Inheritance In Java](https://www.geeksforgeeks.org/java/favoring-composition-over-inheritance-in-java-with-examples/)
</details>

<details>
<summary>Name some techniques for reducing coupling between classes.</summary>

### Name some techniques for reducing coupling between classes.

**Definition:**

Coupling drops when classes depend on small abstractions and delegate to replaceable parts instead of hard-coded concrete types.

**How it works:**

Depend on interfaces (dependency inversion) and inject them through the constructor so the class does not call `new` on its collaborators. Split fat interfaces so a client does not depend on unused methods. Prefer composition over inheritance: a field can be swapped, while a superclass change ripples to every child. Program to a factory when creation is the only concrete detail. Register observers or strategies instead of editing a central class for every new case. GeeksforGeeks summarizes the outcome as loose coupling: a change in one class does not force edits in unrelated classes.

**Sources:**

- [SOLID Principles with Real Life Examples](https://www.geeksforgeeks.org/system-design/solid-principle-in-programming-understand-with-real-life-examples/)
- [Favoring Composition Over Inheritance In Java](https://www.geeksforgeeks.org/java/favoring-composition-over-inheritance-in-java-with-examples/)
- [Design Patterns and Best Practices Interview Questions](https://www.geeksforgeeks.org/interview-prep/design-patterns-best-practices-interview-questions-oops/)
- [A Pythonic Way for Dependency Injection](https://www.geeksforgeeks.org/python/what-is-a-pythonic-way-for-dependency-injection/)
</details>

<details>
<summary>How does immutability help in object-oriented design, and how can it be implemented?</summary>

### How does immutability help in object-oriented design, and how can it be implemented?

**Definition:**

An immutable object does not change after it is constructed, so its state is safe to share and does not surprise other holders of the same reference.

**How it works:**

Oracle cites `String` as a final class used for an immutable type: `final` prevents a subclass from adding setters or overriding methods that would mutate it. Fields are private and final, and methods return new values instead of assigning fields. A builder can assemble those fields and then publish one immutable instance, which GeeksforGeeks notes as a way to get a consistent object without a telescoping constructor. Callers can pass the object across methods without defensive copies of its fields, because those fields are not reassigned.

**Examples:**

```java
public final class Point {
    private final int x, y;
    public Point(int x, int y) { this.x = x; this.y = y; }
    public Point move(int dx, int dy) { return new Point(x + dx, y + dy); }
}
```

**Sources:**

- [Writing Final Classes and Methods](https://docs.oracle.com/javase/tutorial/java/IandI/final.html)
- [Design Patterns and Best Practices Interview Questions](https://www.geeksforgeeks.org/interview-prep/design-patterns-best-practices-interview-questions-oops/)
</details>

<details>
<summary>What tools or techniques would you use to document an object-oriented design?</summary>

### What tools or techniques would you use to document an object-oriented design?

**Definition:**

A UML class diagram documents the structure: classes, attributes, methods, and relationships.

**How it works:**

Each class is a box with the name, attributes, and operations, including visibility. Relationship notation on the GeeksforGeeks page matches the OOP vocabulary: a solid line for association, a hollow diamond for aggregation, a filled diamond for composition, generalization for inheritance (is-a), realization for an interface, and a dependency arrow when one class uses another. The diagram is shared with people who need the structure, and it supports forward design and reverse engineering from code. Keep it next to the code it describes; a diagram that names classes the repository does not have will drift.

**Sources:**

- [UML Class Diagram](https://www.geeksforgeeks.org/system-design/unified-modeling-language-uml-class-diagrams/)
- [Association, Composition and Aggregation in Java](https://www.geeksforgeeks.org/java/association-composition-aggregation-java/)
</details>

<details>
<summary>How do you address circular dependencies in an OOP system?</summary>

### How do you address circular dependencies in an OOP system?

**Definition:**

A circular dependency means types depend on each other in a loop, including a class hierarchy that directly or indirectly inherits from itself. Break the loop so each type depends on an abstraction or on a one-way collaborator.

**How it works:**

Python refuses an inconsistent hierarchy with `TypeError: Cannot create a consistent method resolution order` and tells you to remove the cycle and keep a clear base order. The same shape in design is fixed by dependency inversion: both sides depend on an interface, and a third place wires the concrete objects. Constructor injection makes the direction explicit. If the cycle is only inheritance, composition or a mixin with a linear order replaces the loop. GeeksforGeeks also notes that a mediator can replace a mesh of objects that all observe each other.

**Sources:**

- [Consistent method resolution order](https://www.geeksforgeeks.org/python/how-to-fix-typeerror-cannot-create-a-consistent-method-resolution-order-mro-in-python/)
- [A Pythonic Way for Dependency Injection](https://www.geeksforgeeks.org/python/what-is-a-pythonic-way-for-dependency-injection/)
- [SOLID Principles with Real Life Examples](https://www.geeksforgeeks.org/system-design/solid-principle-in-programming-understand-with-real-life-examples/)
- [Design Patterns and Best Practices Interview Questions](https://www.geeksforgeeks.org/interview-prep/design-patterns-best-practices-interview-questions-oops/)
</details>

<details>
<summary>Explain how to apply unit testing to object-oriented code.</summary>

### Explain how to apply unit testing to object-oriented code.

**Definition:**

Unit-test the public behavior of each class. Put tests for one class in a dedicated test class, and replace collaborators with test doubles.

**How it works:**

GeeksforGeeks’s pytest example uses one test class per production class (`TestSnack`, `TestBeverage`) and one test per function, with `assert` on the result. Overridden parent behavior is tested on the subclass that supplies it. Constructor injection and dependency inversion make that possible: the test passes a fake `PaymentProcessor` instead of the real one. A singleton that constructs itself is harder to replace, which is one reason that pattern fights tests. Keep assertions on observable results, not on private fields.

**Examples:**

```python
class TestOrder:
    def test_cost_uses_shipping(self):
        order = Order(shipping=lambda grams: 5)
        assert order.cost(100) == 5
```

**Sources:**

- [Object oriented testing in Python](https://www.geeksforgeeks.org/python/object-oriented-testing-in-python/)
- [A Pythonic Way for Dependency Injection](https://www.geeksforgeeks.org/python/what-is-a-pythonic-way-for-dependency-injection/)
- [Design Patterns and Best Practices Interview Questions](https://www.geeksforgeeks.org/interview-prep/design-patterns-best-practices-interview-questions-oops/)
</details>

<details>
<summary>What strategies can be used to safely refactor legacy object-oriented code?</summary>

### What strategies can be used to safely refactor legacy object-oriented code?

**Definition:**

Characterize the current behavior with tests, then change structure in small steps that those tests still pass.

**How it works:**

Start with a test class per type and assertions on public results, as in the pytest OOP example. With that net, split a class that has several reasons to change, and introduce interfaces where high-level code names a concrete class. Replace inheritance that exists only for reuse with a composed collaborator. Do not drop in a full pattern set: GeeksforGeeks treats unused factories, decorators, and singletons as obsession that makes the code harder to read. Leave a working path after each step so a failure points at the last edit.

**Sources:**

- [Object oriented testing in Python](https://www.geeksforgeeks.org/python/object-oriented-testing-in-python/)
- [SOLID Principles with Real Life Examples](https://www.geeksforgeeks.org/system-design/solid-principle-in-programming-understand-with-real-life-examples/)
- [Favoring Composition Over Inheritance In Java](https://www.geeksforgeeks.org/java/favoring-composition-over-inheritance-in-java-with-examples/)
- [Design Patterns and Best Practices Interview Questions](https://www.geeksforgeeks.org/interview-prep/design-patterns-best-practices-interview-questions-oops/)
</details>

<details>
<summary>How can the principles of OOP help in achieving a modular and maintainable codebase?</summary>

### How can the principles of OOP help in achieving a modular and maintainable codebase?

**Definition:**

OOP keeps a change local when each type has one job, callers depend on abstractions, and behavior is extended by new types rather than edits to old ones.

**How it works:**

Single responsibility limits why a class changes. Open/closed adds a case as a new class. Liskov substitution keeps a subtype usable wherever the parent was. Interface segregation stops clients from depending on methods they do not call. Dependency inversion, factories, and constructor injection stop high-level code from constructing low-level classes. Composition swaps a part without a new level of inheritance. GeeksforGeeks ties the five principles to loose coupling: classes are less dependent, so a change does not spread. Encapsulation keeps the representation private so callers are not tied to fields.

**Sources:**

- [SOLID Principles with Real Life Examples](https://www.geeksforgeeks.org/system-design/solid-principle-in-programming-understand-with-real-life-examples/)
- [Favoring Composition Over Inheritance In Java](https://www.geeksforgeeks.org/java/favoring-composition-over-inheritance-in-java-with-examples/)
- [Abstract Methods and Classes](https://docs.oracle.com/javase/tutorial/java/IandI/abstract.html)
</details>

<details>
<summary>How do you balance the use of OOP principles with performance considerations in a system design?</summary>

### How do you balance the use of OOP principles with performance considerations in a system design?

**Definition:**

Keep the object boundaries that match the domain, and drop layers that exist only as pattern ceremony. Extra indirection has a cost; a false abstraction has a larger maintenance cost.

**How it works:**

GeeksforGeeks notes that chained adapters and unused wrappers add runtime overhead and that this matters in low-latency systems. An interface is described as slightly more overhead than an abstract class because of the extra abstraction; that is a reason to choose the simpler mechanism when the types are closely related, not a reason to abandon either. Lazy initialization in a singleton or a virtual proxy defers heavy construction until the object is used. Share intrinsic state with a flyweight when many objects repeat the same data, as in a text editor’s font information. Do not collapse a design that has several reasons to change into one class only to avoid a call: measure the hot path, then remove the specific layer that shows up.

**When to use:**

Favor a direct call or one composed object on a measured hot path. Keep interfaces, strategies, and decorators where behavior actually varies.

**Sources:**

- [Design Patterns and Best Practices Interview Questions](https://www.geeksforgeeks.org/interview-prep/design-patterns-best-practices-interview-questions-oops/)
- [Difference Between Abstract Class and Interface in Java](https://www.geeksforgeeks.org/java/difference-between-abstract-class-and-interface-in-java/)
</details>
