# Object-Oriented Programming (OOP) in Dart

Object-Oriented Programming (OOP) is a programming paradigm based on the concept of objects that contain data and methods. Dart fully supports OOP principles like classes, objects, inheritance, polymorphism, and encapsulation. Use this guide to practice OOP concepts in Dart.

---

## 1. **Basics of Classes and Objects**

### Create a Class

```dart
class Person {
  String name;
  int age;

  // Constructor
  Person(this.name, this.age);

  // Method
  void introduce() {
    print("Hi, I'm $name, and I'm $age years old.");
  }
}
```

### Create an Object

```dart
void main() {
  Person person = Person("Alice", 25);
  person.introduce(); // Output: Hi, I'm Alice, and I'm 25 years old.
}
```

---

## 2. **Constructors**

### Named Constructors

```dart
class Point {
  double x;
  double y;

  // Named constructor
  Point.origin() : x = 0, y = 0;
}

void main() {
  Point point = Point.origin();
  print("(${point.x}, ${point.y})"); // Output: (0.0, 0.0)
}
```

### Factory Constructors

```dart
class Singleton {
  static final Singleton _instance = Singleton._internal();

  factory Singleton() {
    return _instance;
  }

  Singleton._internal();
}
```

---

## 3. **Encapsulation**

Encapsulation involves hiding internal details using private fields and providing public getters and setters.

```dart
class BankAccount {
  double _balance = 0.0; // Private field

  // Getter
  double get balance => _balance;

  // Setter
  set deposit(double amount) {
    if (amount > 0) _balance += amount;
  }
}

void main() {
  BankAccount account = BankAccount();
  account.deposit = 100.0;
  print(account.balance); // Output: 100.0
}
```

---

## 4. **Inheritance**

Inheritance allows a class to reuse the properties and methods of another class.

```dart
class Animal {
  void eat() {
    print("Eating...");
  }
}

class Dog extends Animal {
  void bark() {
    print("Barking...");
  }
}

void main() {
  Dog dog = Dog();
  dog.eat(); // Output: Eating...
  dog.bark(); // Output: Barking...
}
```

---

## 5. **Polymorphism**

Polymorphism allows objects to be treated as instances of their parent class.

```dart
class Shape {
  void draw() {
    print("Drawing Shape");
  }
}

class Circle extends Shape {
  @override
  void draw() {
    print("Drawing Circle");
  }
}

void main() {
  Shape shape = Circle();
  shape.draw(); // Output: Drawing Circle
}
```

---

## 6. **Abstract Classes and Interfaces**

### Abstract Class

```dart
abstract class Vehicle {
  void start();
}

class Car extends Vehicle {
  @override
  void start() {
    print("Car started");
  }
}

void main() {
  Vehicle vehicle = Car();
  vehicle.start(); // Output: Car started
}
```

### Interface (Implemented via Classes)

```dart
class Printer {
  void printDocument() {
    print("Printing document...");
  }
}

class Scanner {
  void scanDocument() {
    print("Scanning document...");
  }
}

class AllInOne implements Printer, Scanner {}
```

---

## 7. **Mixins**

Mixins are used to share methods across multiple classes.

```dart
mixin Flyable {
  void fly() {
    print("Flying...");
  }
}

class Bird with Flyable {}

void main() {
  Bird bird = Bird();
  bird.fly(); // Output: Flying...
}
```

---

## 8. **Practice Exercises**

1. **Person Class:** Create a `Person` class with fields `name` and `age`, and methods to display the details.
2. **Inheritance:** Build a class hierarchy for `Vehicle`, `Car`, and `Bike` with shared methods and specific behaviors.
3. **Polymorphism:** Create a parent class `Shape` and subclasses `Circle` and `Rectangle`. Use polymorphism to call their respective `draw` methods.
4. **Bank Account:** Implement encapsulation for managing account balance with proper getters and setters.
5. **Mixins:** Use mixins to create shared behaviors like `Swimming` and `Running` for different classes.

---

### ? vs ??
*  Use `?` for declaring nullable types or for null-aware access (?.).
*  Use `??` to provide a fallback value for potentially null expressions. FallBack means **default value** or **behavior**.

## Abstract vs Concrete (Class, Method, Fields)
### Abstract
1. **Class:** A class without implementation.
2. **Method:** A method has no implementation `void method();` is an abstract method.
3. **Fields:** Abstract fileds can be created via getters and setters.

### Concrete
1. **Class:** A concrete class is a class that is fully implemented and can be instantiated.
2. **Method:** A method has implementation or has a body is called a concrete method.
3. **Fields:** A field occupies a memory or space is a concrete field no matter if it holds a null values it is still a concrete field.
  ```
   int count; //Concrete field
   late int count; //Not a Concrete field
  ```


## Additional Resources

- [Dart Language Tour](https://dart.dev/guides/language/language-tour)
- [Effective Dart](https://dart.dev/guides/language/effective-dart)

Happy Learning! 🚀
