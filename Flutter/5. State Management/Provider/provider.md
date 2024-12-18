# Provider Practice Notes

Provider is a popular state management solution in Flutter that is simple, scalable, and integrates well with Flutter's widget tree. Follow this guide to practice Provider effectively.

---

## 1. **Setting Up Provider**

1. Add Provider to your `pubspec.yaml` file:

   ```yaml
   dependencies:
     provider: ^6.0.5
   ```

2. Run the command to get the dependencies:

   ```bash
   flutter pub get
   ```

3. Import Provider into your Dart files:

   ```dart
   import 'package:provider/provider.dart';
   ```

---

## 2. **State Management Basics**

### Create a State Notifier

1. Define a `ChangeNotifier` class:

   ```dart
   import 'package:flutter/material.dart';

   class CounterProvider extends ChangeNotifier {
     int _count = 0;

     int get count => _count;

     void increment() {
       _count++;
       notifyListeners(); // Notify listeners about state changes
     }
   }
   ```

2. Provide the state to the widget tree:

   ```dart
   void main() {
     runApp(
       MultiProvider(
         providers: [
           ChangeNotifierProvider(create: (_) => CounterProvider()),
         ],
         child: MyApp(),
       ),
     );
   }
   ```

3. Use the state in widgets:

   ```dart
   import 'package:flutter/material.dart';
   import 'package:provider/provider.dart';
   import 'counter_provider.dart';

   class HomePage extends StatelessWidget {
     @override
     Widget build(BuildContext context) {
       return Scaffold(
         appBar: AppBar(title: Text("Provider Counter")),
         body: Center(
           child: Consumer<CounterProvider>(
             builder: (context, counter, child) => Text(
               "Count: ${counter.count}",
               style: TextStyle(fontSize: 25),
             ),
           ),
         ),
         floatingActionButton: FloatingActionButton(
           onPressed: () {
             context.read<CounterProvider>().increment();
           },
           child: Icon(Icons.add),
         ),
       );
     }
   }
   ```

---

## 3. **Using Provider for Complex Applications**

### MultiProvider Setup

If your app has multiple providers, use `MultiProvider`:

```dart
MultiProvider(
  providers: [
    ChangeNotifierProvider(create: (_) => CounterProvider()),
    ChangeNotifierProvider(create: (_) => AnotherProvider()),
  ],
  child: MyApp(),
);
```

### Accessing State

1. **Read Once:**

   ```dart
   int count = context.read<CounterProvider>().count;
   ```

2. **Listen for Changes:**

   ```dart
   int count = context.watch<CounterProvider>().count;
   ```

3. **Consumer Widget:**

   ```dart
   Consumer<CounterProvider>(
     builder: (context, counter, child) => Text("Count: ${counter.count}"),
   );
   ```

---

## 4. **Notifier State Classes**

Use `Notifier` or `ChangeNotifier` for more complex state management.

### Example: Todo List

1. Create a `TodoProvider` class:

   ```dart
   class TodoProvider extends ChangeNotifier {
     List<String> _todos = [];

     List<String> get todos => _todos;

     void addTodo(String todo) {
       _todos.add(todo);
       notifyListeners();
     }
   }
   ```

2. Use it in the widget tree:

   ```dart
   class TodoApp extends StatelessWidget {
     @override
     Widget build(BuildContext context) {
       final todoProvider = context.watch<TodoProvider>();

       return Scaffold(
         appBar: AppBar(title: Text("Todo List")),
         body: ListView.builder(
           itemCount: todoProvider.todos.length,
           itemBuilder: (context, index) => ListTile(
             title: Text(todoProvider.todos[index]),
           ),
         ),
         floatingActionButton: FloatingActionButton(
           onPressed: () {
             todoProvider.addTodo("New Todo ${todoProvider.todos.length + 1}");
           },
           child: Icon(Icons.add),
         ),
       );
     }
   }
   ```

---

## 5. **Practice Exercises**

1. **Counter App:** Build a counter app with increment and decrement functionality using Provider.
2. **Todo App:** Create a CRUD Todo app where users can add, edit, and delete todos.
3. **Shopping Cart:** Implement a shopping cart with Provider to manage cart items.
4. **Theme Switcher:** Build an app with light and dark theme switching using Provider.
5. **Login Form:** Create a login form and manage its validation state using Provider.

---

### Additional Resources
- [Provider Documentation](https://pub.dev/packages/provider)
- [Flutter State Management Guide](https://flutter.dev/docs/development/data-and-backend/state-mgmt/intro)

Happy Coding! 🚀
