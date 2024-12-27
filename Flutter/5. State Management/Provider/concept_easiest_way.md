# Provider
A provider is a third-party library. Here, we need to understand three main concepts to use this library.
1.  ChangeNotifier
2.  ChangeNotifierProvider
3.  Consumer

### 1. ChangeNotifier
It is used for the listener to observe a model for changes. If you extends ChangeNotifier means that your going to use that class as a controller class. Then if you logic done don't forget to add `notifyListener()` method to inform the listeners.
Let's consider the example of **CounterProvider** which has **increment** method to increment the value to see the changes we use `notifyListener()`.
```dart
class CounterProvider extends ChangeNotifier{
  int _count = 0;

  int get count => _count;

  void increment(){
    _count++;
    notifyListeners();
  }
}
```

### 2. ChangeNotifierProvider
**ChangeNotifierProvider** is the widget that provides an instance of a ChangeNotifier to its descendants(). 
```dart
return MaterialApp(
      title: 'Flutter Demo',
      theme: ThemeData(
        colorScheme: ColorScheme.fromSeed(seedColor: Colors.blue),
        useMaterial3: true,
      ),
      debugShowCheckedModeBanner: false,
      home: MultiProvider(
        providers: [
          ChangeNotifierProvider<CounterProvider>(
            create: (_) => CounterProvider(),
          )
        ],
        child: CounterApp(title: 'Counter App'),
   ),
);
```

### 3. Consumer
It is a type of provider that does not do any fancy work. It just calls the provider in a new widget and delegates its build implementation to the builder.  
It just used to tell hey, only this widget going to be changed.
```dart
    Consumer<CounterProvider>(builder: (context, counterProvider, child){
      return Text(
        '${counterProvider.count}',
        style: Theme.of(context).textTheme.headlineMedium,
      );
    })
```
The builder function contains three arguments, which are context, bookingProvider, and child. The first argument, context, contains every build() method. The second argument is the instance of the ChangeNotifier, and the third argument is the child that is used for optimization. It is the best idea to put the consumer widget as deep in the tree as possible.
