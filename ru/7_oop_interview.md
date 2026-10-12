# OOP — Interview

Ответы заполнены для каждого вопроса в этой заметке.

[ ← Вернуться в хаб](./0_system_design.md)

### Related docs

- [SOLID](./7_special_engineering.md) · [OOP — Интервью](./7_oop_interview.md) · [Проектирование системы — Интервью](./0_system_design_interview.md)

### Sections

- [Interview Practice](#interview-practice)

---

## Interview Practice

Вопросы с [Devinterview.io — OOP](https://devinterview.io/questions/web-and-mobile-development/oop-interview-questions/).

### OOP Fundamentals

<details>
<summary>Что такое Object-Oriented Programming (OOP)?</summary>

### Что такое Object-Oriented Programming (OOP)?

**Definition:**

Объектно-ориентированное программирование — парадигма программирования, которая организует код в самостоятельные объекты.

Каждый объект объединяет данные и методы, которые работают с этими данными, и в результате код получается более модульным, гибким и пригодным для повторного использования.

**How it works:**

Классы и объекты. Класс служит чертежом для создания объектов и задаёт их атрибуты и методы. Объект представляет экземпляр класса. Он содержит и данные (атрибуты класса), и методы, предназначенные для работы с этими данными.

Инкапсуляция — это объединение данных и связанных с ними методов внутри класса и их сокрытие, чтобы защитить целостность. Внешний код обращается к ним через публичные методы, которые часто называют геттерами (чтобы читать атрибуты) и сеттерами (чтобы их менять).

Наследование позволяет создавать новые классы, которые наследуют свойства и методы существующих классов. Оно способствует повторному использованию кода и устанавливает иерархическое отношение. Класс, от которого наследуют, — базовый класс, или суперкласс, а класс, который наследует, — производный класс, или подкласс.

Полиморфизм позволяет обращаться с объектами производных классов (подклассов) как с объектами их базового класса (суперкласса), сохраняя при этом их собственное поведение. Благодаря этому объекты по-разному отвечают на один и тот же вызов метода, особенно когда несколько классов делят имя метода, но имеют разные реализации.

- Полиморфизм времени компиляции: определяется до запуска кода, обычно через перегрузку методов.
- Полиморфизм времени выполнения: определяется во время работы кода, обычно через переопределение методов.

Абстракция состоит в том, чтобы создать простой интерфейс, который открывает только уместные и необходимые возможности.

- Абстрактные классы: обобщённые классы, поэтому их не создают напрямую. Они служат базовыми классами для производных.
- Интерфейсы: задают правила для классов и указывают, какие методы те обязаны реализовать.

Ассоциации. У объектов часто есть отношения с другими объектами: один объект использует другие или составлен из них. Такие отношения называют ассоциациями.

- Агрегация: один объект владеет другим или содержит его; на это указывает отношение «has-a».
- Композиция: более сильная форма агрегации, при которой дочерний объект не может существовать без родителя.

**Pluses and minuses:**

- Модульность: объекты организованы в самостоятельные сущности и общаются через публичные интерфейсы, что способствует разделению кода и упрощает сопровождение.
- Повторное использование: объекты и иерархии классов можно переиспользовать в разных проектах. Кроме того, наследование позволяет классам получать атрибуты и поведение от родительских классов.
- Расширяемость: новые возможности можно добавлять через наследование классов и реализацию интерфейсов.
- Гибкость: полиморфизм позволяет объектам менять своё поведение в зависимости от контекста.

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

В примере:

- Классы и объекты: `Animal` — класс, а `simba` и `rio` — объекты.
- Инкапсуляция: атрибут `name` класса `Animal` и атрибут `color` класса `Parrot` инкапсулированы с приватной видимостью, и для них предусмотрены публичные методы-геттеры.
- Наследование: классы `Lion` и `Parrot` наследуют от класса `Animal`.
- Полиморфизм: и `Lion`, и `Parrot` дают собственную реализацию метода `makeSound()`, хотя с ними обращаются как с объектами `Animal`.
- Абстракция: класс `Animal` содержит абстрактный метод `makeSound()`, и производные классы обязаны дать свою реализацию.

**Sources:**

- [Devinterview.io — OOP](https://devinterview.io/questions/web-and-mobile-development/oop-interview-questions/)

</details>

<details>
<summary>Что такое the difference between procedural and Object-Oriented programming?</summary>

### Что такое the difference between procedural and Object-Oriented programming?

**Definition:**

Процедурное и объектно-ориентированное программирование (ООП) — разные парадигмы программирования. Процедурное программирование линейно и сосредоточено на задачах, а ООП делает упор на органичную модель, в которой данные инкапсулированы в объектах.

**How it works:**

Управление данными и функциями.

- Процедурный стиль: функции действуют на данные. Данные часто глобальны, и из-за этого возможны конфликты.
- ООП: данные и функции собраны внутри объектов. Объекты взаимодействуют через методы, и это обеспечивает целостность данных и инкапсуляцию.

Абстракция кода и повторное использование.

- Процедурный стиль: код часто разбивают на функции. Глобальные данные могут мешать повторному использованию.
- ООП: абстракции достигают через классы и объекты. Инкапсуляция помогает создавать отдельные самостоятельные модули.

Наследование и полиморфизм.

- Процедурный стиль: наследования и полиморфизма нет.
- ООП: наследование способствует повторному использованию кода, а полиморфизм позволяет обращаться с объектами разных классов как с экземплярами общего суперкласса или интерфейса.

ООП и процедурный стиль в разных языках.

- Процедурный стиль: яркий пример — C. Он построен вокруг процедурного подхода, где код организован как последовательность задач, или процедур. На C можно имитировать отдельные возможности ООП, например группировать связанные данные в структурах, но язык сам по себе не поддерживает весь спектр ООП.
- ООП: Java изначально спроектирована вокруг ООП. Она полностью поддерживает классы, наследование, инкапсуляцию и другие принципы ООП.

На практике многие современные языки, такие как Python, JavaScript и C#, поддерживают несколько парадигм и дают свободу выбрать подходящий подход для конкретной задачи.

**Examples:**

Процедурный подход:

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

Объектно-ориентированный подход:

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
<summary>Что такое encapsulation?</summary>

### Что такое encapsulation?

**Definition:**

Инкапсуляция — принцип объектно-ориентированного программирования, который объединяет данные и методы, действующие на эти данные, в одной единице — объекте.

**How it works:**

- Сокрытие информации: инкапсуляция скрывает внутреннее состояние и детали реализации.
- Управление доступом: данные доступны внешнему коду только через определённые методы — геттеры для чтения и сеттеры для записи.
- Ограничение поведения: методы доступа могут следить, чтобы данные подчинялись конкретным правилам (например, проверке диапазона или стандартам форматирования).

**Pluses and minuses:**

- Безопасность: даёт управляемый доступ к данным объекта и снижает риск порчи данных или несанкционированных изменений.
- Простота: объекты абстрагируют сложные системы и предъявляют простой интерфейс для взаимодействия.
- Гибкость: инкапсуляция способствует слабому зацеплению, и внутренности объекта проще изменить или заменить, не затронув внешний код.

**When to use:**

- Построение классов: современные языки программирования, такие как Java и C#, следуют подходу «сначала инкапсуляция» и используют спецификаторы доступа вроде `public`, `private` и `protected`.
- Проектирование API: инкапсуляция классов и модулей помогает создавать понятные и сосредоточенные API. Она позволяет скрыть внутренние стратегии и открыть нужную функциональность.
- Тестирование: сокрытие данных не даёт при тестировании обращаться к внутренностям объекта напрямую и заставляет оценивать поведение объекта через его публичный интерфейс.

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
<summary>Что такое polymorphism? Объясните overriding и overloading.</summary>

### Что такое polymorphism? Объясните overriding и overloading.

**Definition:**

Полиморфизм в объектно-ориентированном программировании позволяет обращаться с объектами разных типов так, будто они принадлежат одному типу, через общий интерфейс. Он абстрагирует детали методов и способствует гибкости и повторному использованию кода.

**How it works:**

Перегрузка (полиморфизм времени компиляции). В одном классе может быть несколько методов с одним именем, но разными параметрами, и они сосуществуют. Компилятор выбирает подходящий метод по сигнатуре.

Переопределение (полиморфизм времени выполнения). Подкласс даёт конкретную реализацию метода, который уже определён в родительском классе, и тем самым фактически заменяет версию родителя. Какой метод вызвать, определяется во время выполнения программы.

Виртуальные методы (в языках вроде C++). Методы, помеченные `virtual` в базовом классе, можно переопределить в производных классах. Они включают динамическую диспетчеризацию и гарантируют вызов верного метода, даже через ссылку на базовый класс.

Полиморфизм часто опирается на динамическую диспетчеризацию — механизм, который во время выполнения определяет, какой именно метод вызвать. Это также называют поздним связыванием. Такая возможность позволяет писать общий код, который работает с несколькими типами объектов, и увеличивает гибкость и приспособляемость.

**Examples:**

Перегрузка:

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

Переопределение:

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

Виртуальные методы:

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
<summary>Что такое inheritance? Назовите типы inheritance.</summary>

### Что такое inheritance? Назовите типы inheritance.

**Definition:**

Наследование — фундаментальное понятие объектно-ориентированного программирования, которое позволяет создавать новые классы на основе существующих.

Наследование устанавливает отношение «is-a»: производный класс (его также называют подклассом или дочерним классом) наследует свойства и поведение своего родителя, или базового класса.

**How it works:**

- Одиночное наследование: класс наследует только от одного базового класса. Обычное дело в языках вроде Java. Пример: класс B наследует от класса A.
- Множественное наследование: класс наследует от нескольких базовых классов. C++ его поддерживает, а языки вроде Java используют интерфейсы, чтобы получить похожий эффект без сложностей, связанных с «проблемой ромба» (diamond problem). Пример: класс C наследует и от класса A, и от класса B.
- Многоуровневое наследование: класс наследует от другого класса, который сам является производным. Пример: класс C наследует от класса B, который наследует от класса A.
- Иерархическое наследование: один базовый класс наследуют несколько подклассов. Пример: и класс B, и класс C наследуют от класса A.
- Гибридное наследование: сочетание двух или более перечисленных видов наследования. Его применение может приводить к сложностям. Пример: в C++ класс может участвовать и во множественном, и в многоуровневом наследовании.

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
<summary>Что такое abstraction? Назовите техники abstraction.</summary>

### Что такое abstraction? Назовите техники abstraction.

**Definition:**

Абстракция в объектно-ориентированном программировании (ООП) — это сосредоточение на существенных характеристиках и сокрытие ненужных деталей.

Она даёт ясное разделение между понятиями высокого уровня и их конкретными реализациями и способствует повторному использованию кода, сопровождаемости и безопасности.

**How it works:**

Абстрактные классы.

- Определение: абстрактный класс нельзя создать сам по себе. Он служит основанием для других классов.
- Назначение: абстрактные классы позволяют задать базовое поведение, которое производные классы могут переопределить, и при этом дать часть реализаций по умолчанию. Это значит, что одни методы вы объявляете абстрактными (без реализации), а у других может быть настоящий код.
- Когда применять: если в наборе возможностей у части методов есть поведение по умолчанию, которое можно переопределить, а другие обязан определить любой дочерний класс, используют абстрактный класс.

Интерфейсы.

- Определение: интерфейс — контракт, или чертёж, которому следуют классы. В отличие от абстрактных классов, у интерфейсов нет деталей реализации: они только объявляют сигнатуры методов и свойств.
- Назначение: интерфейсы гарантируют, что в реализующих их классах есть определённый набор методов или свойств. Они гарантируют конкретную структуру или поведение и не диктуют, как это поведение достигается.
- Когда применять: когда нужно, чтобы несколько классов следовали одному контракту и у всех были одни и те же методы (возможно, с разными реализациями), используют интерфейс.

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
<summary>Что такое class in OOP?</summary>

### Что такое class in OOP?

**Definition:**

Класс в объектно-ориентированном программировании представляет чертёж для создания объектов (экземпляров). Он инкапсулирует атрибуты (данные) и поведение (методы) в единой структуре.

При создании экземпляра каждый объект может нести собственные данные, следуя чертежу, который задал его класс.

**How it works:**

- Атрибуты: хранят состояние объекта и могут иметь разные типы данных.
- Методы: это функции, определённые внутри класса; они работают с атрибутами или выполняют конкретное поведение.

Принципы ООП в классах:

- Наследование: позволяет подклассу (дочернему классу) наследовать атрибуты и методы суперкласса (родительского класса).
- Инкапсуляция: атрибуты и методы собраны внутри класса, а прямой доступ ограничен, чтобы данные были в безопасности.
- Полиморфизм: позволяет единообразно использовать объекты разных классов, которые реализуют один и тот же интерфейс.
- Абстракция: скрывает внутренние детали и даёт упрощённый интерфейс. В классах её достигают, открывая только существенные методы.

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
<summary>Что такое object in OOP?</summary>

### Что такое object in OOP?

**Definition:**

Объект представляет конкретный экземпляр класса. Он инкапсулирует и данные (атрибуты), и поведение (методы) в одной единице.

**How it works:**

- Идентичность: у каждого объекта есть уникальная идентичность, которая отличает его от других.
- Состояние: его задают атрибуты; состояние объекта может меняться на протяжении его существования.
- Поведение: методы, связанные с объектом, описывают его возможные действия или операции.

Жизненный цикл объекта:

- Создание: объекты создают из класса в процессе, который называют инстанцированием.
- Использование: они могут менять состояние, когда меняют атрибуты и вызывают методы.
- Уничтожение: прекращает существование объекта; часто это делает сам язык программирования («сборка мусора»), либо это задают явно в коде.

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
<summary>Как работают спецификаторы доступа и каковы они обычно?</summary>

### Как работают спецификаторы доступа и каковы они обычно?

**Definition:**

В объектно-ориентированном программировании (ООП) спецификаторы доступа задают уровень видимости и доступности членов класса.

**How it works:**

- Private: члены доступны только внутри определяющего класса, и это обеспечивает инкапсуляцию данных.
- Protected: члены доступны внутри определяющего класса и его подклассов.
- Public: члены глобально доступны всем классам.

Инкапсуляция данных: она обеспечивает целостность данных, скрывая внутреннее состояние класса и открывая его через методы. Так проще управлять данными и доступом к ним.

Наследование и абстракция: уровни доступа регулируют видимость в контексте наследования и позволяют классам взаимодействовать, сохраняя инкапсуляцию.

**Pluses and minuses:**

Типичные ошибки:

- Чрезмерное использование пар геттер/сеттер: они полезны, чтобы защитить данные, но не стоит вводить методы, которые не добавляют функциональности.
- Ненужные публичные члены: если члену не нужен глобальный доступ, для лучшей инкапсуляции стоит рассмотреть более строгие уровни доступа.

Лучшие практики:

- Предпочитайте композицию наследованию: если классы связаны слабо, сокрытие деталей и составных частей упрощает сопровождение кода.
- По умолчанию — private: инкапсулируйте, где это возможно. По умолчанию члены должны быть приватными, а видимость расширяют только если это необходимо.
- Минимум публичных членов: используйте их экономно, для элементов, которым действительно нужен глобальный доступ.
- Согласованная видимость: стремитесь к единообразной видимости внутри класса, чтобы не было путаницы.

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
<summary>Назовите some ways to overload a method.</summary>

### Назовите some ways to overload a method.

**Definition:**

Метод перегружают тремя распространёнными приёмами.

**How it works:**

1. Изменение числа параметров. Этот приём состоит в том, чтобы менять число параметров в разных сигнатурах метода.
2. Изменение порядка параметров. Другой приём перегрузки — переставлять порядок параметров в методах, чтобы получить уникальные сигнатуры.
3. Различие типов параметров. Можно объявить методы с параметрами разных типов и тем самым включить перегрузку.

**Examples:**

Изменение числа параметров:

```java
public int calculateSum(int a, int b) { // Two parameters
    return a + b;
}

public int calculateSum(int a, int b, int c) { // Three parameters
    return a + b + c;
}
```

Изменение порядка параметров:

```java
public double calculateArea(double length, double width) { // Length, then width
    return length * width;
}

public double calculateArea(double radius) { // Just radius for a circle
    return Math.PI * radius * radius;
}
```

Различие типов параметров:

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
<summary>Что такое cohesion in OOP?</summary>

### Что такое cohesion in OOP?

**Definition:**

Связность в ООП — это то, насколько тесно методы и данные внутри одного класса связаны друг с другом. Высокосвязный класс сосредоточен на конкретной задаче или ответственности, и его проще сопровождать, понимать и делать надёжным.

Высокая связность — желательное свойство, потому что методы и свойства внутри класса работают вместе единообразно. Напротив, низкая связность означает, что у класса несколько, часто несвязанных, ответственностей, и его труднее понимать и сопровождать.

**How it works:**

- Случайная: у методов и свойств внутри класса нет осмысленной связи.
- Логическая: методы сгруппированы по какой-то логике, но ясной темы нет.
- Временная: методы связаны тем, когда их выполняют, например методы инициализации.
- Процедурная: методы выполняют в определённой последовательности.
- Коммуникационная: методы работают с одним и тем же набором данных.
- Последовательная: выход одного метода служит входом другого.
- Функциональная: все методы класса вносят вклад в одну чётко определённую задачу.

Из них наиболее желательна функциональная связность: она ближе всего к принципу единственной ответственности.

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

Класс `FileUtility` показывает низкую связность: он смешивает операции с файлами, запись в базу данных и управление кэшем.

**Thoughts:**

- Принцип единственной ответственности (SRP): у класса должна быть только одна причина для изменения. Этот принцип говорит, что класс должен быть сосредоточен на одной задаче или ответственности.
- Инкапсуляция: поощряйте сокрытие данных и открывайте данные только через сосредоточенные и связанные методы.

**Sources:**

- [Devinterview.io — OOP](https://devinterview.io/questions/web-and-mobile-development/oop-interview-questions/)

</details>

<details>
<summary>Что такое coupling in OOP?</summary>

### Что такое coupling in OOP?

**Definition:**

Зацепление в ООП описывает степень взаимозависимости между классами или модулями. Оно показывает, насколько тесно разные модули или классы связаны друг с другом, и влияет на гибкость, сопровождаемость и тестируемость системы.

В проектировании программ обычно предпочитают слабое зацепление.

**How it works:**

- Зацепление по содержимому: самая сильная форма зацепления, при которой один модуль напрямую обращается к внутренним данным другого модуля или меняет их.
- Зацепление по общей области: несколько модулей вместе обращаются к общим глобальным данным. Любое изменение этого общего ресурса может затронуть все модули, которые от него зависят.
- Зацепление по управлению: один модуль управляет потоком другого, передавая ему управляющую информацию, например флаги.
- Внешнее зацепление: классы или модули связаны внешними факторами, такими как файлы конфигурации или схемы данных.
- Зацепление по образцу (или по данным): модули разделяют структуры данных и используют только их части, и для этого нужно знать структуру передаваемых данных.
- Зацепление по сообщениям: самая низкая форма зацепления, при которой модули общаются только через стандартные интерфейсы, например вызовы методов или сообщения.

Связь с принципами SOLID:

- Принцип единственной ответственности (SRP): следование SRP обычно даёт низкое зацепление, потому что у классов один фокус и зависимостей меньше.
- Принцип открытости/закрытости (OCP): упор на расширяемость без модификации снижает риск тесного зацепления, потому что расширения обычно делают через интерфейсы или абстрактные классы.
- Принцип подстановки Лисков (LSP): когда производные классы могут заменять свои базовые классы без побочных эффектов, зацепление часто снижается, и модули могут работать независимо от того, какой именно производный класс используется.
- Принцип разделения интерфейса (ISP): за счёт узких интерфейсов вместо универсальных «на все случаи» ISP естественно уменьшает зацепление, потому что классы не вынуждены зависеть от методов, которыми не пользуются.
- Принцип инверсии зависимостей (DIP): опора на абстракции, а не на конкретные реализации, способствует низкому зацеплению и делает системы более модульными и приспособляемыми.

**Sources:**

- [Devinterview.io — OOP](https://devinterview.io/questions/web-and-mobile-development/oop-interview-questions/)

</details>


### Class Relationships and Design

<details>
<summary>Что такое constructor and how is it used?</summary>

### Что такое constructor and how is it used?

**Definition:**

Конструктор — особый метод в объектно-ориентированном программировании, которым инициализируют объекты. Он обеспечивает, чтобы только что созданные объекты получили подходящее начальное состояние и все нужные ресурсы.

**How it works:**

Назначение.

- Конструктор обеспечивает, чтобы в момент создания объект был в согласованном состоянии.
- Он инициализирует и настраивает атрибуты, или поля, объекта.

Характеристики.

- Соответствует определениям объектов: каждый класс задаёт один или несколько конструкторов для своих объектов.
- Имя совпадает с именем класса: конструкторы называют в честь класса, и их легко узнать.
- Нет типа возврата: они не возвращают значение, даже `void`.
- Автоматический вызов: их вызывают, когда создают объект.

Виды конструкторов.

- Конструкторы по умолчанию: без параметров. Язык предоставляет их автоматически, если ни один конструктор не определён.
- Параметризованные конструкторы: принимают параметры для собственной инициализации. Их часто используют, чтобы задать начальные значения внутренним атрибутам или полям.
- Конструкторы копирования: принимают другой объект того же типа и инициализируют текущий объект его значениями. В языках вроде C++ их обычно используют для глубокого копирования.
- Статические конструкторы: ими инициализируют статические члены или выполняют другие действия уровня класса. Они не участвуют в создании объектов, и во многих языках ООП их может не быть.

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
<summary>Опишите концепцию деструктора или финализатора в OOP.</summary>

### Опишите концепцию деструктора или финализатора в OOP.

**Definition:**

Деструкторы, или финализаторы класса, используют в объектно-ориентированном программировании, чтобы выполнить очистку перед уничтожением объекта.

**How it works:**

Объекты можно уничтожать явно или неявно:

- Явно: программист вызывает функцию или метод уничтожения. В языках вроде C++ для этого есть деструктор.
- Неявно: уничтожением объектов управляет язык или среда. Примеры — сборщики мусора в Java, C# и Python.

Деструктор — особый метод, который автоматически вызывается непосредственно перед тем, как объект выходит из области видимости или его уничтожают явно. Его главное назначение — освободить ресурсы или выполнить другую очистку, например закрыть файлы или освободить память.

Случаи применения:

- Управление ресурсами: своевременное освобождение системных ресурсов.
- Управление памятью: полезно в языках без сборки мусора, чтобы не было утечек памяти.
- Журналирование выхода или действий очистки: часто используют, чтобы записать уничтожение объекта или шаги очистки.

Для управления памятью сборка мусора — механизм, который специально занимается автоматическим возвратом памяти. В языках вроде Java и C# деструкторы могут быть не нужны благодаря надёжным сборщикам мусора.

В Java появился метод `finalize()`, похожий на деструктор. Его часто не рекомендуют и предпочитают интерфейс `AutoCloseable` и оператор try-with-resources для более надёжного управления ресурсами.

Помимо памяти деструкторы могут управлять разными ресурсами, например:

- Файлы: файлы закрывают, чтобы не потерять данные.
- Сетевые соединения: сокеты закрывают, чтобы не было утечек ресурсов.
- Соединения с базой данных: помогают освободить блокировки или сессии базы.
- Аппаратные устройства: для устройств вроде камер или датчиков освобождение ресурсов делает их доступными другим приложениям.

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
<summary>Сравните inheritance с микшином и составом.</summary>

### Сравните inheritance с микшином и составом.

**Definition:**

Наследование, примеси и композиция — приёмы объектно-ориентированного программирования, которые касаются повторного использования кода и отношений между объектами или классами.

**How it works:**

Наследование.

- Определение: класс (подкласс) наследует свойства и поведение другого класса (суперкласса).
- Отношение: «is-a» (например, Car является Vehicle).
- Плюсы: прямой способ переиспользовать код; устанавливает интуитивные отношения.
- Минусы: может приводить к сложным иерархиям; есть риск чрезмерного обобщения.

Примесь (mixin).

- Определение: класс, который предлагает методы другим классам, не будучи самостоятельной сущностью. Обычное дело в языках без множественного наследования, например в Python.
- Отношение: «kind-of-a» или «can-do-this» (например, вертолёт умеет летать, как птица).
- Плюсы: переиспользует код между классами; избегает проблем глубокого наследования.
- Минусы: источник метода может быть неясен; возможны столкновения имён.

Композиция.

- Определение: объекты собирают из более простых. Делает упор на отношение «has-a».
- Отношение: «has-a» (например, у Car есть Engine).
- Плюсы: поощряет модульное проектирование; составные части легко подменять.
- Минусы: может потребовать больше проектирования заранее; может потребовать больше шаблонного кода.

**Thoughts:**

Многие современные рекомендации по проектированию в ООП, например принцип «композиция вместо наследования», говорят, что если нет ясного отношения «is-a», часто лучше использовать композицию: она гибче и ведёт к более модульному и сопровождаемому проектированию.

**Sources:**

- [Devinterview.io — OOP](https://devinterview.io/questions/web-and-mobile-development/oop-interview-questions/)

</details>

<details>
<summary>Объясните концепцию interface и чем она отличается от abstract class.</summary>

### Объясните концепцию interface и чем она отличается от abstract class.

**Definition:**

Интерфейс — это контракт поведения: тип, который называет методы, обязательные для класса. Абстрактный класс нельзя создать напрямую; в нём можно совместить методы без реализации, общие поля и готовые методы.

**How it works:**

В Java класс `implements` любое число интерфейсов и `extends` только один класс, в том числе абстрактный. Поля интерфейса — public, static и final. Объявленные методы интерфейса публичны; те, что не `default` и не `static`, неявно абстрактны. Абстрактный класс может объявлять нестатические и не-final поля, а также публичные, protected и private методы с телом. С Java 8 у интерфейса тоже бывают `default` и `static` методы, но собственного состояния экземпляра, как у абстрактного класса, у него нет.

**When to use:**

Абстрактный класс уместен, когда близкие типы делят код, поля или непубличные методы. Интерфейс уместен, когда неродственные типы должны делить поведение (`Comparable`, `Cloneable`, `Map`) или нужна множественная реализация типов.

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
<summary>Может ли класс иметь несколько родителей в одной системе inheritance?</summary>

### Может ли класс иметь несколько родителей в одной системе inheritance?

**Definition:**

Нет. В системе с одиночным наследованием классов у класса один прямой суперкласс. Несколько контрактов он всё равно может выполнить, реализовав несколько интерфейсов.

**How it works:**

Java допускает одиночное, многоуровневое и иерархическое наследование классов и не позволяет классу расширять больше одного класса. Это снимает неоднозначность выбора метода. Класс может реализовать несколько интерфейсов, а интерфейс — расширять несколько интерфейсов. Реализующий класс обязан дать все абстрактные методы этих интерфейсов. GeeksforGeeks описывает наследование классов как IS-A и повтор использования реализации, а интерфейс — как контракт CAN-DO.

**When to use:**

Оставляйте один родительский класс, когда типы делят структуру. Добавляйте интерфейсы, когда один объект должен быть доступен через несколько несвязанных контрактов.

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
<summary>Как бы вы design a class to prevent it from being subclassed?</summary>

### Как бы вы design a class to prevent it from being subclassed?

**Definition:**

Объявите класс `final`. Финальный класс нельзя наследовать. Отдельные методы делают `final`, если фиксировать нужно только их.

**How it works:**

Oracle помечает метод `final`, чтобы подкласс не мог его переопределить, и класс `final`, чтобы его нельзя было расширить. `String` — обычный пример финального класса для неизменяемого типа. Методы, которые вызывают из конструктора, обычно тоже делают final: если конструктор вызывает не-final метод, подкласс может переопределить его с неожиданным результатом.

**When to use:**

Закрывайте класс, когда его инварианты нельзя менять, особенно у неизменяемых значений. Закрывайте метод, когда один шаг должен остаться постоянным, даже если остальной класс расширяют.

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
<summary>Объясните соотношение "есть" и "есть" в OOP.</summary>

### Объясните соотношение "есть" и "есть" в OOP.

**Definition:**

IS-A значит, что тип является подтипом другого, и выражается наследованием класса. HAS-A значит, что объект использует или содержит другой, и выражается полем со ссылкой на него.

**How it works:**

GeeksforGeeks сопоставляет наследование классов с IS-A, а интерфейс — с CAN-DO. Ассоциация — это связь uses-a между объектами, которые могут существовать отдельно. Агрегация — более слабое has-a: часть может пережить целое. Композиция — более сильное part-of: жизненный цикл части зависит от контейнера. Наследование связывает потомка с родителем, поэтому изменение родителя расходится вниз. Композиция делегирует другому объекту и может его заменить, например `Car` хранит `Engine`, а не расширяет `Engine`.

**When to use:**

IS-A оставляйте только для настоящего подтипа. HAS-A используйте, когда нужно повторное использование или заменяемая часть без утверждения, что контейнер и есть эта часть.

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
<summary>Объясните, как взаимосвязь aggreGAtion представлена в OOP.</summary>

### Объясните, как взаимосвязь aggreGAtion представлена в OOP.

**Definition:**

Агрегация — это ассоциация has-a со слабым владением. Содержимый объект может существовать независимо от контейнера, и у них независимые жизненные циклы.

**How it works:**

Контейнер хранит ссылку, часто коллекцию, которую передают снаружи, а не создают как закрытую часть. GeeksforGeeks показывает `Department` со списком `Student`: студентов создают заранее и передают внутрь, поэтому уничтожение кафедры не означает уничтожение студентов. На UML-диаграмме классов эта связь целое–часть рисуется сплошной линией с пустым ромбом у целого. Совместное владение допустимо. Композиция сильнее: закрашенный ромб, исключительное владение и часть, которая не живёт без контейнера.

**When to use:**

Агрегацию моделируют, когда часть общая или переживает целое, например студенты кафедры или книги библиотеки. Композицию используют, когда часть создаётся и уничтожается вместе с владельцем, как комнаты дома в примере композиции.

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
<summary>Что такое method overriding, and what rules apply to it?</summary>

### Что такое method overriding, and what rules apply to it?

**Definition:**

Переопределение метода заменяет унаследованный метод экземпляра методом подкласса с той же сигнатурой и совместимым типом возврата, поэтому вызов использует поведение подкласса.

**How it works:**

Переопределяющий метод имеет то же имя, типы параметров и тип возврата, что и метод суперкласса. Он может вернуть подтип (ковариантный возврат). `@Override` просит компилятор проверить, что метод суперкласса действительно есть. Доступ у переопределения может стать шире, но не уже: protected можно сделать public, но не private. Смена метода экземпляра на static или static на метод экземпляра — ошибка компиляции. Статический метод подкласса с той же сигнатурой скрывает статический метод суперкласса; какая версия выполнится, зависит от названного класса, а не от объекта во время выполнения. Методы экземпляра предпочтительнее default-методов интерфейса. Если конфликтуют два независимых default-метода, класс обязан их переопределить и может вызвать `InterfaceName.super.method()`.

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
<summary>Опишите использование статических методов и когда они уместны.</summary>

### Опишите использование статических методов и когда они уместны.

**Definition:**

Статический метод принадлежит классу, а не экземпляру. Его вызывают по имени класса, не создавая объект.

**How it works:**

Методы класса могут напрямую использовать переменные класса и другие методы класса. Они не могут использовать поля экземпляра, методы экземпляра и `this`, пока не обратятся через ссылку на объект. Обычное применение — чтение или обновление статического поля, например `Bicycle.getNumberOfBicycles()` вокруг общего счётчика. `static` вместе с `final` задаёт константу, например `PI`. Вызов статического метода через ссылку на экземпляр допустим и не рекомендуется: так не видно, что метод принадлежит классу. Статический метод подкласса скрывает, а не переопределяет, статический метод суперкласса.

**When to use:**

Статический метод уместен для поведения без состояния одного объекта: фабрики, которым нужны только аргументы, счётчики, общие для всех экземпляров, и константы. Методы, которые читают или меняют поля одного объекта, оставляйте методами экземпляра.

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
<summary>Что такое multiple inheritance and what are some of its disadvantages?</summary>

### Что такое multiple inheritance and what are some of its disadvantages?

**Definition:**

Множественное наследование позволяет одному типу получить члены более чем от одного родителя. Java не разрешает это для классов. Класс может реализовать несколько интерфейсов — это множественное наследование типа.

**How it works:**

Главный недостаток — неоднозначность: если два родителя определяют один и тот же метод, потомок не знает, какое тело выполнить. Это ромбовидная проблема. Второй недостаток — более сложный порядок методов. C3-линеаризация Python может завершиться `TypeError: Cannot create a consistent method resolution order`, если иерархия непоследовательна или циклична, а пересекающиеся конструкторы могут дать `TypeError: got multiple values for keyword argument`. Java убирает неоднозначность на уровне классов, разрешая только один суперкласс. Интерфейсы всё же могут столкнуться, когда у них есть `default`-методы; класс должен переопределить метод и выбрать `Parent.super.method()`.

**When to use:**

Предпочитайте один суперкласс плюс интерфейсы или композицию, если правила множественного наследования языка (например, миксины Python с `super()`) не заданы явно и не проверены.

**Sources:**

- [Diamond Problem in Java](https://www.geeksforgeeks.org/java/diamond-problem-in-java/)
- [Interfaces and Inheritance in Java](https://www.geeksforgeeks.org/java/interfaces-and-inheritance-in-java/)
- [Python multiple inheritance and mixins](https://www.geeksforgeeks.org/python/how-to-fix-python-multiple-inheritance-generates-typeerror-got-multiple-values-for-keyword-argument/)
- [Consistent method resolution order](https://www.geeksforgeeks.org/python/how-to-fix-typeerror-cannot-create-a-consistent-method-resolution-order-mro-in-python/)
</details>

<details>
<summary>Можете ли вы объяснить «проблему с бриллиантами» в нескольких inheritance?</summary>

### Можете ли вы объяснить «проблему с бриллиантами» в нескольких inheritance?

**Definition:**

Ромбовидная проблема — это неоднозначность, когда тип наследует один и тот же метод через двух родителей, а оба получили его от общего предка.

**How it works:**

Java не позволяет классу расширять два класса, поэтому ромб из классов написать нельзя. С Java 8 `default`-методы могут вернуть ту же неоднозначность: два интерфейса дают `default void fun()`, и класс, который реализует оба, обязан переопределить `fun`. Родителя выбирают через `Parent1.super.fun()`. Правило Oracle то же: если конфликтуют два независимо объявленных default-метода, компилятор сообщает об ошибке, пока класс их не переопределит. Методы экземпляра класса побеждают default-методы интерфейса, а более конкретный переопределённый default побеждает default общего предка.

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
<summary>Как языки OOP поддерживают polymorphism под капотом?</summary>

### Как языки OOP поддерживают polymorphism под капотом?

**Definition:**

Полиморфизм времени выполнения выбирает переопределённый метод экземпляра по фактическому классу объекта, а не по типу переменной, которая на него ссылается.

**How it works:**

В примере Oracle `Cat`/`Animal` объект `Cat` хранится в переменной `Animal`. `myAnimal.testInstanceMethod()` выполняет метод `Cat`. Скрытый статический метод — нет: `Animal.testClassMethod()` выполняет статический метод `Animal`, потому что скрытие определяется по названному классу. То же правило покрывает ссылки на интерфейс: переменная `Drawable` может указывать на `Rectangle`, и `draw()` выполнит метод `Rectangle`. У переопределения должна остаться совместимая сигнатура, чтобы вызов, скомпилированный против типа родителя, оставался верным. Эта заметка не описывает конкретную раскладку виртуальной таблицы; наблюдаемое правило — динамическая диспетчеризация методов экземпляра по классу во время выполнения.

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
<summary>Что такое generics, and how can they be useful in OOP?</summary>

### Что такое generics, and how can they be useful in OOP?

**Definition:**

Обобщения позволяют классам, интерфейсам и методам принимать типы как параметры, чтобы один код переиспользовался с разными входными типами.

**How it works:**

Обычные параметры получают значения, параметры-типы получают типы. Компилятор Java проверяет обобщённый код и сообщает о нарушениях на этапе компиляции. У сырого `List` на `get` нужен cast; `List<String>` возвращает `String` без cast. Один и тот же обобщённый алгоритм может работать с коллекциями разных типов и оставаться типобезопасным. В объектной модели это значит, что контейнер или алгоритм (`List<T>`, `Map<K,V>`) не зашивает класс элемента.

**When to use:**

Обобщения уместны для коллекций, репозиториев и алгоритмов, которые должны работать для многих типов элементов без cast и без копий классов.

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
<summary>Объясните концепцию объектной композиции и ее преимущества.</summary>

### Объясните концепцию объектной композиции и ее преимущества.

**Definition:**

Композиция объектов собирает поведение, храня ссылки на другие объекты и делегируя им работу. Это отношение has-a. В сильной форме жизненный цикл части зависит от владельца.

**How it works:**

Класс содержит другие объекты и вызывает их, а не наследует их код. Компоненты можно заменить без нового подкласса, в том числе во время выполнения, и один компонент могут использовать многие классы. Зависимость от интерфейса держит владельца слабо связанным. GeeksforGeeks называет практические выгоды: гибкость, тестируемость (подмена тестовым двойником), инкапсуляция части и отсутствие подкласса на каждую комбинацию поведений. Наследование остаётся уместным для настоящего подтипа is-a; композиция — обычный инструмент повторного использования, когда такого утверждения нет. `Car` с полем `Engine` может сменить бензиновый двигатель на электрический, не меняя наследование `Car`.

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
<summary>Что такое Liskov Substitution Principle (LSP)? Приведите примеры нарушения и соблюдения.</summary>

### Что такое Liskov Substitution Principle (LSP)? Приведите примеры нарушения и соблюдения.

**Definition:**

Если S — подтип T, объекты типа T должны заменяться объектами типа S без изменения корректности программы.

**How it works:**

Принцип нарушается, когда подкласс переопределяет методы так, что ломает ожидания вызывающих код родителя. Обычно так бывает, когда наследование нужно только ради повторного использования кода. Классический случай — `Square` как подкласс `Rectangle`: `setWidth` у прямоугольника меняет только ширину, а квадрат, который меняет ещё и высоту, ломает код, написанный для прямоугольников. Соблюдение значит, что подтип держит контракт родителя. Правила переопределения Oracle это поддерживают: та же сигнатура, возврат не шире ковариантного и доступ не уже. Паттерны Strategy, State и Decorator используют композицию, чтобы поведение менялось без ложного подтипа.

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
<summary>Что такое the dependency inversion principle?</summary>

### Что такое the dependency inversion principle?

**Definition:**

Модули высокого уровня не должны зависеть от модулей низкого уровня. Оба должны зависеть от абстракций. Абстракции не должны зависеть от деталей; детали должны зависеть от абстракций.

**How it works:**

Класс должен опираться на интерфейс или абстрактный тип, а не на конкретный класс. Фабрика может собрать `new` в одном месте: вызывающий просит `Report` и не создаёт `PdfReport`. Если продукт станет `ExcelReport`, меняется фабрика, а не код высокого уровня. Внедрение зависимостей идёт дальше: объект вообще не создаёт свою зависимость. Фреймворк или вызывающий передаёт `PaymentProcessor` в `PaymentService`, часто через конструктор. Тесты могут передать mock вместо настоящего процессора.

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
<summary>Как принцип открытости/закрытости может служить руководством к объектно-ориентированному проектированию?</summary>

### Как принцип открытости/закрытости может служить руководством к объектно-ориентированному проектированию?

**Definition:**

Программные сущности должны быть открыты для расширения и закрыты для изменения: поведение добавляют, не правя сущность, которая уже работает.

**How it works:**

Новые случаи приходят как новые классы, реализующие существующую абстракцию, а не как правки метода, который уже обрабатывает все случаи. GeeksforGeeks связывает это с Factory, Template Method и Observer. Фабрика или стратегия позволяет добавить способ оплаты или тип документа новым классом. Шаблонный метод — предостережение: если каждое новое требование заставляет менять базовый алгоритм и каждый подкласс, база не закрыта. Хуки с поведением по умолчанию или Strategy для меняющегося шага держат каркас стабильным. Смысл слабой связанности SOLID в том, что изменение одного класса не заставляет править несвязанные классы.

**Sources:**

- [SOLID Principles with Real Life Examples](https://www.geeksforgeeks.org/system-design/solid-principle-in-programming-understand-with-real-life-examples/)
- [Design Patterns and Best Practices Interview Questions](https://www.geeksforgeeks.org/interview-prep/design-patterns-best-practices-interview-questions-oops/)
</details>

<details>
<summary>Описать, как принцип interface SegreGAtion влияет на проектирование системы.</summary>

### Описать, как принцип interface SegreGAtion влияет на проектирование системы.

**Definition:**

Клиентов не следует заставлять зависеть от методов, которыми они не пользуются. Много маленьких интерфейсов под клиента лучше одного толстого интерфейса.

**How it works:**

Толстый интерфейс связывает каждую реализацию со всеми методами. Разделение позволяет классу реализовать только нужные контракты — та же идея, что и несколько интерфейсов у одного класса в Java. GeeksforGeeks связывает это с Adapter и Bridge: узкие интерфейсы проще адаптировать и соединять. Вызывающий код зависит от маленького типа, который он реально вызывает, поэтому изменение постороннего метода не заставляет его перекомпилировать или переписывать.

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
<summary>Что такое mixin, and how does it differ from traditional inheritance?</summary>

### Что такое mixin, and how does it differ from traditional inheritance?

**Definition:**

Миксин — это класс, который добавляет другим классам один фрагмент поведения. Он не главный родитель, который определяет, чем объект является.

**How it works:**

Обычное наследование — это связь is-a с одним основным родителем и повторное использование его структуры. Миксин — маленькая база, которую подмешивают ради одной возможности; GeeksforGeeks предлагает это как способ не запутывать иерархию множественного наследования. В Python `class C(MixinA, MixinB)` может взять методы обоих миксинов. С конструкторами всё равно нужна осторожность: вызывайте инициализатор каждого миксина явно или используйте `super()`, чтобы C3-порядок методов оставался согласованным. Если иерархия циклична или порядок баз конфликтует, Python бросает `TypeError: Cannot create a consistent method resolution order`. Композиция — альтернатива, когда даже миксины делают MRO трудным для чтения.

**When to use:**

Миксин уместен, чтобы разделить одну возможность между иначе несвязанными классами. Обычный суперкласс — когда есть настоящий родитель is-a. Композицию предпочитайте, если граф миксинов становится неоднозначным.

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
<summary>Как бы вы refactor a class that has too many responsibilities?</summary>

### Как бы вы refactor a class that has too many responsibilities?

**Definition:**

Разделите класс так, чтобы у каждого была одна причина меняться. Принцип единственной ответственности говорит, что у класса должна быть одна задача.

**How it works:**

В примере пекарни GeeksforGeeks выпечка, склад, заказы поставщикам, обслуживание клиентов и уборка живут в разных классах. Найдите причины, по которым большой класс меняется, вынесите каждую в свой тип и оставьте исходному типу делегирование. Если части не настоящие подтипы, соберите их композицией, а не наследованием. Внедрение через конструктор делает разделение тестируемым: каждого соседа можно заменить mock-объектом.

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
<summary>Опишите рисунок singleton AND обсудите его плюсы AND минусы.</summary>

### Опишите рисунок singleton AND обсудите его плюсы AND минусы.

**Definition:**

Паттерн Singleton хранит один экземпляр класса и глобальный способ его получить.

**How it works:**

Его используют для общего журнала, конфигурации или кэша. GeeksforGeeks также объясняет, почему его часто считают антипаттерном: экземпляр — глобальное состояние, и тесты не могут легко его заменить; класс управляет и своей логикой, и своей жизнью, что давит на единственную ответственность; небрежная реализация гоняется в нескольких потоках; многие вызывающие жёстко связываются с одним объектом. Ленивое создание может не строить экземпляр при старте, и ему всё равно нужна синхронизация, если создать его могут несколько потоков. Предпочитайте внедрение одного общего экземпляра, когда тесты должны его подменить.

**When to use:**

Используйте его для ресурса на весь процесс, который действительно должен быть единственным, и делайте создание потокобезопасным. Не используйте его как место по умолчанию для глобальных данных.

**Sources:**

- [Design Patterns and Best Practices Interview Questions](https://www.geeksforgeeks.org/interview-prep/design-patterns-best-practices-interview-questions-oops/)
</details>

<details>
<summary>Что такое factory method, and when should it be used?</summary>

### Что такое factory method, and when should it be used?

**Definition:**

Фабричный метод задаёт создание в методе и позволяет подклассу или телу метода решить, какой конкретный объект вернуть.

**How it works:**

Вызывающий зависит от абстракции продукта, а не от `new PdfReport()`. GeeksforGeeks противопоставляет Factory Method и Abstract Factory: Factory Method варьирует один продукт (`DocumentFactory` возвращает документ Word или PDF); Abstract Factory создаёт семейство связанных продуктов (кнопка и флажок вместе для одной платформы). Отсюда открытость/закрытость: новый вариант — это новый класс плюс ветка или подкласс фабрики, а не правка каждого места вызова. Инверсия зависимостей — принцип; фабрика — один способ его применить.

**When to use:**

Фабричный метод уместен, когда у одного продукта несколько вариантов и вызывающие не должны называть конкретный класс. Абстрактная фабрика уместна, когда несколько связанных продуктов нужно создавать согласованным набором.

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
<summary>Объясните, что такое шаблон builder и где его можно применить.</summary>

### Объясните, что такое шаблон builder и где его можно применить.

**Definition:**

Паттерн Builder отделяет сборку от готового объекта: сложный объект собирают шаг за шагом именованными методами.

**How it works:**

Он закрывает проблему телескопического конструктора: `new User("A", "B", null, 0, true, null)` трудно читать и легко перепутать порядок. `UserBuilder` задаёт каждое свойство именованным методом, а `build()` возвращает объект. GeeksforGeeks отмечает, что готовый объект часто делают неизменяемым, новые поля меняют builder, а не груду конструкторов, а выгоды — читаемость, гибкость вариантов и меньше ошибок с null и порядком аргументов.

**When to use:**

Builder уместен, когда у объекта много необязательных полей. Короткого конструктора достаточно, когда все аргументы обязательны и вызов остаётся читаемым.

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
<summary>Что такое the prototype pattern, and how does it relate to OOP?</summary>

### Что такое the prototype pattern, and how does it relate to OOP?

**Definition:**

Паттерн Prototype создаёт новый объект, клонируя существующий, а не собирая его с нуля.

**How it works:**

Это порождающая альтернатива фабрике. GeeksforGeeks предпочитает его сборке с нуля, когда создание дорогое (запросы, сеть, тяжёлые вычисления), когда граф объекта глубокий или когда экземпляры отличаются лишь несколькими полями. Фабрика по-прежнему собирает каждую модель по шагам; `SedanPrototype` можно скопировать и поправить, например цвет. В терминах ООП клон — ещё один экземпляр того же класса со скопированным состоянием, а не подкласс.

**When to use:**

Клонируйте заранее настроенный экземпляр, когда копии дешевле или понятнее повторной сборки. Фабрику используйте, когда каждый продукт собирается из входных данных, а не копируется.

**Sources:**

- [Design Patterns and Best Practices Interview Questions](https://www.geeksforgeeks.org/interview-prep/design-patterns-best-practices-interview-questions-oops/)
</details>

<details>
<summary>Когда would you use the adapter pattern?</summary>

### Когда would you use the adapter pattern?

**Definition:**

Adapter нужен, когда клиент ждёт один интерфейс, а готовый класс предлагает другой, и требуется обёртка, которая переводит один в другой.

**How it works:**

Адаптер реализует интерфейс, который клиент уже вызывает, и пересылает вызов адаптируемому объекту. GeeksforGeeks предупреждает, что лишние слои адаптеров добавляют косвенность, цепочки адаптеров прячут проблему дизайна, и адаптер не должен заменять исправление самих типов. Разделение интерфейсов помогает: маленький целевой интерфейс проще адаптировать, чем толстый. Используйте его для настоящей стыковки, например со сторонним типом, который нельзя изменить.

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
<summary>Можете ли вы объяснить использование шаблона decorator?</summary>

### Можете ли вы объяснить использование шаблона decorator?

**Definition:**

Паттерн Decorator оборачивает объект с тем же интерфейсом и добавляет поведение, не меняя обёрнутый класс.

**How it works:**

Его цель — дополнение, а не контроль доступа. GeeksforGeeks противопоставляет его Proxy: прокси стоит вместо объекта и контролирует доступ (ленивая загрузка, удалённый объект, проверка прав), а декоратор добавляет поведение, например сжатие, буферизацию или шифрование потока либо рамку виджета. Обёртка с журналом вокруг банковского счёта — декоратор; проверка входа перед счётом — прокси. Слишком много слоёв декораторов дают проблему йо-йо: чтобы понять один вызов, приходится прыгать по слоям. Декоратор уместен, когда дополнительное поведение должно быть необязательным и сочетаемым.

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
<summary>Опишите шаблон Observer и сценарий, в котором вы можете его использовать.</summary>

### Опишите шаблон Observer и сценарий, в котором вы можете его использовать.

**Definition:**

Паттерн Observer позволяет субъекту уведомлять зарегистрированных зависимых, когда меняется его состояние, не называя каждого конкретного слушателя.

**How it works:**

Субъект хранит ссылки на наблюдателей и сообщает им о событии. Это соответствует открытости/закрытости: нового слушателя регистрируют, субъект не правят. Типичный случай — интерфейс или котировки: виджеты или подписчики обновляются, когда меняется модель или цена. GeeksforGeeks отмечает утечку: если наблюдателя не отписать, субъект всё ещё держит его, и сборщик мусора не может его забрать. Слабые ссылки, явная отписка или шина с очисткой это закрывают. В большой сетке уведомлений медиатор может быть яснее: участники говорят через одного координатора, а не через паутину наблюдателей.

**Sources:**

- [Design Patterns and Best Practices Interview Questions](https://www.geeksforgeeks.org/interview-prep/design-patterns-best-practices-interview-questions-oops/)
- [SOLID Principles with Real Life Examples](https://www.geeksforgeeks.org/system-design/solid-principle-in-programming-understand-with-real-life-examples/)
</details>

<details>
<summary>Что такое the advantages of using the command pattern?</summary>

### Что такое the advantages of using the command pattern?

**Definition:**

Паттерн Command превращает запрос в объект, поэтому инициатор не вызывает получателя напрямую.

**How it works:**

У каждой операции могут быть `execute` и `undo`. Выполненные команды кладут в стек; undo снимает последнюю и вызывает `undo`, а redo выполняет её снова или берёт из стека redo. Команда хранит состояние, нужное для отмены, поэтому редактор, IDE или программа рисования могут отменить ввод, удаление или изменение фигуры, и кнопке не нужно знать доменный объект. Тот же объект можно поставить в очередь, записать в журнал или повторить, потому что запрос — это данные.

**When to use:**

Команды уместны, когда операции нужно отменять, ставить в очередь или запускать из кода, которому не следует знать получателя. Прямого вызова метода достаточно для разового действия без истории.

**Sources:**

- [Design Patterns and Best Practices Interview Questions](https://www.geeksforgeeks.org/interview-prep/design-patterns-best-practices-interview-questions-oops/)
</details>

<details>
<summary>Как the strategy pattern provide flexibility in objects?</summary>

### Как the strategy pattern provide flexibility in objects?

**Definition:**

Паттерн Strategy выносит взаимозаменяемые алгоритмы в собственные типы, а контекст хранит один из них.

**How it works:**

Контекст делегирует меняющийся шаг объекту стратегии. Вызывающие подменяют сортировку, оплату или доставку без подкласса на каждую комбинацию и без правки контекста — это идея открытости/закрытости. GeeksforGeeks отличает Strategy от State: стратегии независимы и выбираются снаружи; объекты состояния меняют поведение, потому что сменился собственный режим объекта, и переходы остаются внутри этой модели. Strategy — это композиция, поэтому она не строит решётку подклассов и позволяет тестировать каждый алгоритм отдельно.

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
<summary>Что такое some common OOP design anti-patterns?</summary>

### Что такое some common OOP design anti-patterns?

**Definition:**

Антипаттерн ООП — это дизайн, который выглядит структурным и всё же затрудняет изменения: божественные классы, наследование ради повторного использования, глобальные одиночки, толстые интерфейсы и паттерны, скопированные туда, где они не решают задачу.

**How it works:**

Класс со многими причинами меняться нарушает единственную ответственность. Наследование ради кода, например `Square` от `Rectangle` или `Car` от `Engine`, ломает подстановку или запирает иерархию. Одиночка, который сам управляет своей жизнью и доступен глобально, сопротивляется тестам. Один интерфейс, который навязывает неиспользуемые методы, размазывает зависимости. GeeksforGeeks называет одержимость паттернами: абстрактная фабрика плюс builder для одного объекта конфигурации или цепочка декораторов, из-за которой вызов превращается в йо-йо по классам. Лишние обёртки также добавляют косвенность. Исправления на тех же страницах: одна причина меняться, is-a только для настоящих подтипов, композиция для заменяемых частей, маленькие интерфейсы и паттерн только когда проблема повторяется.

**Sources:**

- [SOLID Principles with Real Life Examples](https://www.geeksforgeeks.org/system-design/solid-principle-in-programming-understand-with-real-life-examples/)
- [Design Patterns and Best Practices Interview Questions](https://www.geeksforgeeks.org/interview-prep/design-patterns-best-practices-interview-questions-oops/)
- [Favoring Composition Over Inheritance In Java](https://www.geeksforgeeks.org/java/favoring-composition-over-inheritance-in-java-with-examples/)
</details>

<details>
<summary>Как вы гарантируете, что ваши объекты должным образом защищены enCAPsulated?</summary>

### Как вы гарантируете, что ваши объекты должным образом защищены enCAPsulated?

**Definition:**

Инкапсуляция держит состояние объекта за его методами, чтобы вызывающие пользовались опубликованным поведением, а не полями.

**How it works:**

Практический рычаг в руководстве Oracle по абстрактным классам: поля могут быть непубличными, а конкретные методы — private или protected. Держите поля экземпляра private. Меняйте их только в методах, которые сохраняют правила объекта. Публикуйте самый маленький интерфейс, который нужен клиенту (разделение интерфейсов), чтобы скрытые методы не входили в контракт. Финальный класс или финальные поля, как у неизменяемого типа в духе `String`, не дают подклассам и повторному присваиванию обойти эти правила. Композиция прячет детали соседа внутри владельца.

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
<summary>Назовите some techniques for reducing coupling between Classes.</summary>

### Назовите some techniques for reducing coupling between Classes.

**Definition:**

Связанность падает, когда классы зависят от маленьких абстракций и делегируют заменяемым частям, а не зашитым конкретным типам.

**How it works:**

Зависите от интерфейсов (инверсия зависимостей) и внедряйте их через конструктор, чтобы класс не вызывал `new` для своих соседей. Делите толстые интерфейсы, чтобы клиент не зависел от неиспользуемых методов. Предпочитайте композицию наследованию: поле можно заменить, а изменение суперкласса расходится по всем потомкам. Программируйте на фабрику, когда конкретна только деталь создания. Регистрируйте наблюдателей или стратегии вместо правки центрального класса на каждый новый случай. GeeksforGeeks сводит результат к слабой связанности: изменение одного класса не заставляет править несвязанные классы.

**Sources:**

- [SOLID Principles with Real Life Examples](https://www.geeksforgeeks.org/system-design/solid-principle-in-programming-understand-with-real-life-examples/)
- [Favoring Composition Over Inheritance In Java](https://www.geeksforgeeks.org/java/favoring-composition-over-inheritance-in-java-with-examples/)
- [Design Patterns and Best Practices Interview Questions](https://www.geeksforgeeks.org/interview-prep/design-patterns-best-practices-interview-questions-oops/)
- [A Pythonic Way for Dependency Injection](https://www.geeksforgeeks.org/python/what-is-a-pythonic-way-for-dependency-injection/)
</details>

<details>
<summary>Как неизменяемость помогает в объектно-ориентированном проектировании и как ее можно реализовать?</summary>

### Как неизменяемость помогает в объектно-ориентированном проектировании и как ее можно реализовать?

**Definition:**

Неизменяемый объект не меняется после создания, поэтому его состояние безопасно разделять и оно не удивляет других держателей той же ссылки.

**How it works:**

Oracle приводит `String` как финальный класс для неизменяемого типа: `final` не даёт подклассу добавить сеттеры или переопределить методы так, чтобы объект мутировал. Поля делают private и final, а методы возвращают новые значения вместо присваивания полей. Builder может собрать эти поля и затем опубликовать один неизменяемый экземпляр; GeeksforGeeks отмечает это как способ получить согласованный объект без телескопического конструктора. Вызывающие могут передавать объект между методами без защитных копий его полей, потому что эти поля не переприсваиваются.

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
<summary>Какие инструменты или техники вы используете для документирования object-oriented design?</summary>

### Какие инструменты или техники вы используете для документирования object-oriented design?

**Definition:**

Диаграмма классов UML документирует структуру: классы, атрибуты, методы и связи.

**How it works:**

Каждый класс — прямоугольник с именем, атрибутами и операциями, включая видимость. Обозначения связей на странице GeeksforGeeks совпадают со словарём ООП: сплошная линия для ассоциации, пустой ромб для агрегации, закрашенный ромб для композиции, обобщение для наследования (is-a), реализация для интерфейса и стрелка зависимости, когда один класс использует другой. Диаграммой делятся с теми, кому нужна структура, и она поддерживает прямой дизайн и обратное проектирование из кода. Держите её рядом с кодом, который она описывает; диаграмма с классами, которых нет в репозитории, разъедется с ним.

**Sources:**

- [UML Class Diagram](https://www.geeksforgeeks.org/system-design/unified-modeling-language-uml-class-diagrams/)
- [Association, Composition and Aggregation in Java](https://www.geeksforgeeks.org/java/association-composition-aggregation-java/)
</details>

<details>
<summary>Как вы решаете проблему циклических зависимостей в системе OOP?</summary>

### Как вы решаете проблему циклических зависимостей в системе OOP?

**Definition:**

Циклическая зависимость значит, что типы зависят друг от друга по кругу, включая иерархию, в которой класс прямо или косвенно наследует сам себя. Разорвите цикл, чтобы каждый тип зависел от абстракции или от соседа в одну сторону.

**How it works:**

Python отвергает несогласованную иерархию ошибкой `TypeError: Cannot create a consistent method resolution order` и предлагает убрать цикл и сохранить ясный порядок баз. Та же форма в дизайне чинится инверсией зависимостей: обе стороны зависят от интерфейса, а третье место связывает конкретные объекты. Внедрение через конструктор делает направление явным. Если цикл только в наследовании, его заменяют композицией или миксином с линейным порядком. GeeksforGeeks также отмечает, что медиатор может заменить сетку объектов, которые наблюдают друг за другом.

**Sources:**

- [Consistent method resolution order](https://www.geeksforgeeks.org/python/how-to-fix-typeerror-cannot-create-a-consistent-method-resolution-order-mro-in-python/)
- [A Pythonic Way for Dependency Injection](https://www.geeksforgeeks.org/python/what-is-a-pythonic-way-for-dependency-injection/)
- [SOLID Principles with Real Life Examples](https://www.geeksforgeeks.org/system-design/solid-principle-in-programming-understand-with-real-life-examples/)
- [Design Patterns and Best Practices Interview Questions](https://www.geeksforgeeks.org/interview-prep/design-patterns-best-practices-interview-questions-oops/)
</details>

<details>
<summary>Объясните, как применить unit testing к объектно-ориентированному коду.</summary>

### Объясните, как применить unit testing к объектно-ориентированному коду.

**Definition:**

Модульно тестируйте публичное поведение каждого класса. Тесты одного класса кладите в отдельный тестовый класс и подменяйте соседей тестовыми двойниками.

**How it works:**

В примере pytest у GeeksforGeeks на каждый рабочий класс свой тестовый класс (`TestSnack`, `TestBeverage`) и на каждую функцию свой тест с `assert` на результат. Переопределённое поведение родителя проверяют на подклассе, который его даёт. Внедрение через конструктор и инверсия зависимостей это позволяют: тест передаёт поддельный `PaymentProcessor` вместо настоящего. Одиночка, который создаёт себя сам, труднее заменить — одна из причин, почему этот паттерн мешает тестам. Проверяйте наблюдаемый результат, а не приватные поля.

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
<summary>Какие стратегии безопасно рефакторят legacy object-oriented code?</summary>

### Какие стратегии безопасно рефакторят legacy object-oriented code?

**Definition:**

Зафиксируйте текущее поведение тестами, затем меняйте структуру маленькими шагами, которые эти тесты по-прежнему проходят.

**How it works:**

Начните с тестового класса на тип и проверок публичных результатов, как в примере pytest для ООП. С этой сеткой разделите класс, у которого несколько причин меняться, и введите интерфейсы там, где код высокого уровня называет конкретный класс. Наследование, которое нужно только ради повторного использования, замените собранным соседом. Не вставляйте полный набор паттернов: GeeksforGeeks считает лишние фабрики, декораторы и одиночки одержимостью, из-за которой код труднее читать. После каждого шага оставляйте рабочий путь, чтобы сбой указывал на последнюю правку.

**Sources:**

- [Object oriented testing in Python](https://www.geeksforgeeks.org/python/object-oriented-testing-in-python/)
- [SOLID Principles with Real Life Examples](https://www.geeksforgeeks.org/system-design/solid-principle-in-programming-understand-with-real-life-examples/)
- [Favoring Composition Over Inheritance In Java](https://www.geeksforgeeks.org/java/favoring-composition-over-inheritance-in-java-with-examples/)
- [Design Patterns and Best Practices Interview Questions](https://www.geeksforgeeks.org/interview-prep/design-patterns-best-practices-interview-questions-oops/)
</details>

<details>
<summary>Как принципы OOP могут помочь в создании модульной и поддерживаемой кодовой базы?</summary>

### Как принципы OOP могут помочь в создании модульной и поддерживаемой кодовой базы?

**Definition:**

ООП держит изменение локальным, когда у каждого типа одна задача, вызывающие зависят от абстракций, а поведение расширяют новыми типами, а не правками старых.

**How it works:**

Единственная ответственность ограничивает, почему класс меняется. Открытость/закрытость добавляет случай новым классом. Подстановка Лисков сохраняет подтип пригодным везде, где годился родитель. Разделение интерфейсов не даёт клиентам зависеть от методов, которые они не вызывают. Инверсия зависимостей, фабрики и внедрение через конструктор не дают коду высокого уровня создавать классы низкого уровня. Композиция заменяет часть без нового уровня наследования. GeeksforGeeks связывает пять принципов со слабой связанностью: классы меньше зависят друг от друга, поэтому изменение не расползается. Инкапсуляция держит представление закрытым, чтобы вызывающие не были привязаны к полям.

**Sources:**

- [SOLID Principles with Real Life Examples](https://www.geeksforgeeks.org/system-design/solid-principle-in-programming-understand-with-real-life-examples/)
- [Favoring Composition Over Inheritance In Java](https://www.geeksforgeeks.org/java/favoring-composition-over-inheritance-in-java-with-examples/)
- [Abstract Methods and Classes](https://docs.oracle.com/javase/tutorial/java/IandI/abstract.html)
</details>

<details>
<summary>Как вы сочетаете использование принципов OOP с соображениями производительности при проектировании системы?</summary>

### Как вы сочетаете использование принципов OOP с соображениями производительности при проектировании системы?

**Definition:**

Сохраняйте границы объектов, которые совпадают с предметной областью, и убирайте слои, которые существуют только как церемония паттерна. Лишняя косвенность имеет цену; ложная абстракция дороже в сопровождении.

**How it works:**

GeeksforGeeks отмечает, что цепочки адаптеров и ненужные обёртки добавляют накладные расходы во время выполнения и что это важно в системах с низкой задержкой. Интерфейс описан как чуть более дорогой, чем абстрактный класс, из-за дополнительной абстракции; это повод выбрать более простой механизм, когда типы близки, а не повод отказаться от обоих. Ленивая инициализация в одиночке или виртуальном прокси откладывает тяжёлое создание, пока объект не понадобится. Приспособленец делит внутреннее состояние, когда много объектов повторяют одни данные, как сведения о шрифте в текстовом редакторе. Не схлопывайте дизайн с несколькими причинами меняться в один класс только чтобы избежать вызова: измерьте горячий путь и уберите конкретный слой, который на нём виден.

**When to use:**

На измеренном горячем пути предпочитайте прямой вызов или один составной объект. Интерфейсы, стратегии и декораторы оставляйте там, где поведение реально меняется.

**Sources:**

- [Design Patterns and Best Practices Interview Questions](https://www.geeksforgeeks.org/interview-prep/design-patterns-best-practices-interview-questions-oops/)
- [Difference Between Abstract Class and Interface in Java](https://www.geeksforgeeks.org/java/difference-between-abstract-class-and-interface-in-java/)
</details>
