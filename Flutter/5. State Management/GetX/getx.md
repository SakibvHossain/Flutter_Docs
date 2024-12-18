# GetX Practice Notes

GetX is a lightweight, fast, and powerful state management solution for Flutter. It also provides routing and dependency injection capabilities. Follow this guide to start practicing GetX effectively.

### Index
1.  [Setting Up GetX](getx.md#1-setting-up-getx)
2.  [State Management Basics](getx.md#2-state-management-basics)
3.  [Navigation with GetX](getx.md#3-navigation-with-getx)
4.  [Dependency Injection](getx.md#4-dependency-injection)
5.  [Snackbar, Dialog, and BottomSheet](getx.md#5-snackbar-dialog-and-bottomsheet)

---

## 1. **Setting Up GetX**

1. Add GetX to your `pubspec.yaml` file:

   ```yaml
   dependencies:
     get: ^4.6.5
   ```

2. Run the command to get the dependencies:

   ```bash
   flutter pub get
   ```

3. Import GetX into your Dart files:

   ```dart
   import 'package:get/get.dart';
   ```

---

## 2. **State Management Basics**

### Using `Obx` for Reactive State

1. Create a `Controller` class:

   ```dart
   import 'package:get/get.dart';

   class CounterController extends GetxController {
     var count = 0.obs; // Reactive variable

     void increment() {
       count++;
     }
   }
   ```

2. Use the controller in your widget:

   ```dart
   import 'package:flutter/material.dart';
   import 'package:get/get.dart';
   import 'counter_controller.dart';

   class HomePage extends StatelessWidget {
     final CounterController counterController = Get.put(CounterController());

     @override
     Widget build(BuildContext context) {
       return Scaffold(
         appBar: AppBar(title: Text("GetX Counter")),
         body: Center(
           child: Obx(() => Text(
                 "Count: ${counterController.count}",
                 style: TextStyle(fontSize: 25),
               )),
         ),
         floatingActionButton: FloatingActionButton(
           onPressed: counterController.increment,
           child: Icon(Icons.add),
         ),
       );
     }
   }
   ```

---

## 3. **Navigation with GetX**

### Basic Navigation

1. Navigate to a new page:

   ```dart
   Get.to(NextPage());
   ```

2. Go back to the previous page:

   ```dart
   Get.back();
   ```

3. Navigate to a page and remove all previous pages:

   ```dart
   Get.offAll(HomePage());
   ```

### Example:

1. Create two pages: `HomePage` and `NextPage`.

   ```dart
   class NextPage extends StatelessWidget {
     @override
     Widget build(BuildContext context) {
       return Scaffold(
         appBar: AppBar(title: Text("Next Page")),
         body: Center(
           child: ElevatedButton(
             onPressed: () => Get.back(),
             child: Text("Go Back"),
           ),
         ),
       );
     }
   }
   ```

2. Add navigation in `HomePage`:

   ```dart
   ElevatedButton(
     onPressed: () => Get.to(NextPage()),
     child: Text("Go to Next Page"),
   )
   ```

---

## 4. **Dependency Injection**

1. Create a service or controller:

   ```dart
   class MyService extends GetxService {
     Future<void> init() async {
       print("Service Initialized");
     }
   }
   ```

2. Inject the service:

   ```dart
   void main() {
     Get.put(MyService());
     runApp(MyApp());
   }
   ```

3. Access it anywhere:

   ```dart
   final MyService myService = Get.find();
   ```

---

## 5. **Snackbar, Dialog, and BottomSheet**

### Show a Snackbar:

```dart
Get.snackbar("Title", "This is a GetX Snackbar");
```

### Show a Dialog:

```dart
Get.defaultDialog(
  title: "Alert",
  middleText: "This is a GetX Dialog",
);
```

### Show a BottomSheet:

```dart
Get.bottomSheet(
  Container(
    color: Colors.white,
    child: Wrap(
      children: [
        ListTile(
          leading: Icon(Icons.wb_sunny_outlined),
          title: Text("Light Theme"),
          onTap: () {
            Get.changeTheme(ThemeData.light());
          },
        ),
        ListTile(
          leading: Icon(Icons.wb_sunny),
          title: Text("Dark Theme"),
          onTap: () {
            Get.changeTheme(ThemeData.dark());
          },
        ),
      ],
    ),
  ),
);
```

---

## 6. **Practice Exercises**

1. **Counter App:** Build a simple counter app using GetX state management.
2. **Todo App:** Create a CRUD app for managing todos with GetX.
3. **Themed App:** Add light/dark theme switching using GetX.
4. **Multi-Screen Navigation:** Implement an app with multiple screens and smooth navigation using GetX.
5. **Login Form:** Create a login form and manage its state using GetX.

---

### Additional Resources
- [GetX Documentation](https://pub.dev/packages/get)
- [GetX GitHub Repository](https://github.com/jonataslaw/getx)

Happy Coding! 🚀
