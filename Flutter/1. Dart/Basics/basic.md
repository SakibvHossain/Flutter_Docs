# Dart Basics Practice Note

This note covers essential Dart programming concepts to build a strong foundation. Use this as a guide to practice Dart's fundamental features.

---

## 1. **Variables and Data Types**

### Declaring Variables
```dart
void main() {
  var name = 'Alice'; // Automatically infers String
  int age = 25;
  double height = 5.9;
  bool isMarried = false;
  
  print(name); // Output: Alice
}
```

### Constants and Final
```dart
void main() {
  const pi = 3.14; // Compile-time constant
  final currentTime = DateTime.now(); // Run-time constant
}
```

---

## 2. **Control Flow Statements**

### If-Else
```dart
void main() {
  int number = 10;

  if (number > 0) {
    print("Positive");
  } else {
    print("Non-Positive");
  }
}
```

### Loops
```dart
void main() {
  // For loop
  for (int i = 0; i < 5; i++) {
    print(i);
  }

  // While loop
  int j = 0;
  while (j < 5) {
    print(j);
    j++;
  }

  // Do-While loop
  int k = 0;
  do {
    print(k);
    k++;
  } while (k < 5);
}
```

### Switch-Case
```dart
void main() {
  String grade = 'A';

  switch (grade) {
    case 'A':
      print("Excellent"); //  Output: Excellent
      break;
    case 'B':
      print("Good");
      break;
    default:
      print("Invalid grade");
  }
}
```

---

## 3. **Collections**

### List
```dart
void main() {
  List<int> numbers = [1, 2, 3];
  numbers.add(4);

  print(numbers); // Output: [1, 2, 3, 4]
}
```

### Set
```dart
void main() {
  Set<String> cities = {'Paris', 'London', 'New York'};
  cities.add('Tokyo');

  print(cities); // Output: {Paris, London, New York, Tokyo}
}
```

### Map
```dart
void main() {
  Map<String, int> scores = {'Alice': 90, 'Bob': 85};
  scores['Charlie'] = 88;

  print(scores); // Output: {Alice: 90, Bob: 85, Charlie: 88}
}
```

---

## 4. **Functions**

### Declaring Functions
```dart
void greet(String name) {
  print("Hello, $name");
}

void main() {
  greet("Alice"); // Output: Hello, Alice
}
```

### Anonymous Functions
```dart
void main() {
  var numbers = [1, 2, 3];
  numbers.forEach((number) {
    print(number * 2);
  });
}
```

---

## 5. **Null Safety**

### Nullable and Non-Nullable
```dart
void main() {
  String? name = null; // Nullable
  name = "Alice";
  
  print(name); // Output: Alice
}
```

### Null-aware Operators
```dart
void main() {
  String? name;
  print(name ?? "Guest"); // Output: Guest
}
```

---

## 6. **Classes and Objects**

### Create a Class
```dart
class Person {
  String name;
  int age;

  Person(this.name, this.age);
}

void main() {
  Person person = Person("Alice", 25);
  print(person.name); // Output: Alice
}
```

---

## 7. **Error Handling**

### Try-Catch
```dart
void main() {
  try {
    int result = 10 ~/ 0;
    print(result);
  } catch (e) {
    print("Error: $e");
  }
}
```

---

## 8. **Asynchronous Programming**

### Future
```dart
Future<String> fetchData() async {
  return "Data fetched";
}

void main() async {
  String data = await fetchData();
  print(data);
}
```

### Stream
```dart
void main() async {
  Stream<int> numberStream = Stream.fromIterable([1, 2, 3, 4]);

  await for (int number in numberStream) {
    print(number);
  }
}
```
#### Example (without AP)
```dart
void main()  {
  Stream<int> stream = Stream.fromIterable([1, 2, 3, 4, 5]);

  stream.forEach((values) => print(values * 2));

  Map<String, String> map = {'name': 'Ichigo', 'city': 'Japan'};

  map.forEach((key, value) {
    print(value);
  });
} 
```

#### Example (with AP)
```dart
void main() async {
  Stream<int> stream = Stream.fromIterable([1, 2, 3, 4, 5]);

 await stream.forEach((values) => print(values * 2));

  Map<String, String> map = {'name': 'Ichigo', 'city': 'Japan'};

  map.forEach((key, value) {
    print(value);
  });
}
```


---

## 9. **Practice Exercises**

1. **List Operations:** Write a program to add, remove, and iterate over a list of fruits.
2. **Set Operations:** Create a set of unique numbers and find the intersection with another set.
3. **Map Usage:** Build a map to store employee names and their salaries. Update a salary and print the map.
4. **Null Safety:** Write a program to safely handle a nullable string and print a default value if null.
5. **Async Programming:** Simulate a delay in fetching data using a `Future` and print the result.

---

## Additional Resources

- [Dart Language Tour](https://dart.dev/guides/language/language-tour)
- [Effective Dart](https://dart.dev/guides/language/effective-dart)

Happy Coding! 🚀
