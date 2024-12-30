# Hive
Hive is a lightweight, fast, and NoSQL database that allows you to store data on the device in a way that works seamlessly with Flutter.

## Step-by-Step Guide to Using Hive in a Flutter Project

#### Step 1: Add Hive Dependencies
Start by adding the required dependencies in your `pubspec.yaml` file. Hive itself is a local database for storing simple data, and you will also need `hive_flutter` for **Flutter-specific functionality**, `hive_generator` for **code generation**, and `build_runner` for building the necessary **Hive adapter code**.

```dart
dependencies:
  flutter:
    sdk: flutter
  hive: ^2.2.3
  hive_flutter: ^1.1.0
  get: ^4.6.5
  http: ^0.15.0
  cached_network_image: ^3.2.3 # For caching the network image

dev_dependencies:
  hive_generator: ^2.0.0
  build_runner: ^2.4.6
```

**Run the command to install the dependencies:**
```dart
flutter pub get
```

#### Step 2: Define the Hive Model
Hive uses custom classes to store data. You need to create a model class for your data, and Hive will generate code to handle serialization (saving and loading) of this data.

In this case, we're defining a `Product` class that will hold product information.

**File**: `product_model.dart`

```dart
import 'package:hive/hive.dart';
part 'product_model.g.dart'; // The code generator will create this file

@HiveType(typeId: 0) // Type ID to identify the class
class Product {
  @HiveField(0) // This number should be unique within the class
  int id;

  @HiveField(1)
  String title;

  @HiveField(2)
  double price;

  @HiveField(3)
  String description;

  @HiveField(4)
  String category;

  @HiveField(5)
  String image;

  @HiveField(6)
  double ratingRate;

  @HiveField(7)
  int ratingCount;

  Product({
    required this.id,
    required this.title,
    required this.price,
    required this.description,
    required this.category,
    required this.image,
    required this.ratingRate,
    required this.ratingCount,
  });

  // Factory method to create an instance of Product from a JSON object
  factory Product.fromJson(Map<String, dynamic> json) {
    return Product(
      id: json['id'],
      title: json['title'],
      price: json['price'].toDouble(),
      description: json['description'],
      category: json['category'],
      image: json['image'],
      ratingRate: json['rating']['rate'].toDouble(),
      ratingCount: json['rating']['count'],
    );
  }
}
```

In this model:  
*  The `@HiveType` annotation marks the class as a Hive object.  
*  The `@HiveField` annotations mark the fields to be persisted.  
*  The `fromJson` factory method converts a JSON response from an API into a Product instance.  

Now, run the following command to generate the `product_model.g.dart` file:
```dart
flutter pub run build_runner build
```

#### Step 3: Initialize Hive
You need to **initialize Hive** before using it in your app. This is typically done in the main.dart file, before running the app.

**File:** `main.dart`
```dart
void main() async {
  WidgetsFlutterBinding.ensureInitialized();

  await Hive.initFlutter(); //initializes Hive for use with Flutter.

  // Register the Product Adapter (generated code)
  Hive.registerAdapter(ProductAdapter()); 

  // Open the box (database) where products will be stored
  await Hive.openBox<Product>('products'); 

  runApp(const MyApp());
}
```

**In the above code:**
1.  `Hive.initFlutter()` initializes Hive for use with Flutter.
2.  `Hive.registerAdapter(ProductAdapter())` registers the generated Hive adapter for the Product class.
3.  `Hive.openBox<Product>('products')` opens a box (local storage) to store Product objects. A "box" in Hive is like a table in a database.


#### Step 4: Create the Controller
Now, you'll create a controller that manages the state and fetches data from both the API and Hive. 

**File:** `product_controller.dart`
```dart
class ProductController extends GetxController {
  var products = <Product>[].obs; // Observable list of products
  late Box<Product> productBox; // Declare the Hive box

  @override
  void onInit() {
    super.onInit();
    productBox = Hive.box<Product>('products'); // Get the opened box
    if (productBox.isEmpty) {
      fetchProductsFromAPI(); // Fetch from API if no products in Hive
    } else {
      loadProductsFromHive(); // Otherwise, load from Hive
    }
  }
  void loadProductsFromHive() {
    products.value = productBox.values.toList(); // Load data from Hive
  }
  Future<void> fetchProductsFromAPI() async {
    const url = 'https://fakestoreapi.com/products';
    try {
      final response = await http.get(Uri.parse(url));
      if (response.statusCode == 200) {
        final List<dynamic> data = json.decode(response.body);
        final fetchedProducts = data.map((item) => Product.fromJson(item)).toList();
        // Save products to Hive
        for (var product in fetchedProducts) {
          productBox.put(product.id, product); // Store in Hive
        }
        // Update the observable list with the fetched products
        products.value = fetchedProducts;
      } else {
        Get.snackbar('Error', 'Failed to fetch products. Status: ${response.statusCode}');
      }
    } catch (e) {
      Get.snackbar('Error', 'Failed to fetch products. Error: $e');
    }
  }
}
```

**In the controller:**
1.  `products` is an observable list that automatically updates the UI when modified.
2.  `productBox` is the box (database) where products are stored.
3.  In `onInit()`, it checks if the box is empty. If so, it fetches products from the API; otherwise, it loads products from Hive.

The `fetchProductsFromAPI()` function fetches data from an API, converts it into Product objects, and saves them in Hive using `productBox.put(product.id, product)`. The `loadProductsFromHive()` method loads all products from the Hive box into the observable list.


#### Step 5: Display Data in the UI
Now, use `GetX` to display the list of products in the UI. You will use `Obx` to make the UI reactive, meaning it will update whenever the list of products changes.

**File:** `product_screen.dart`
```dart
class ProductScreen extends StatelessWidget {
  const ProductScreen({super.key});

  @override
  Widget build(BuildContext context) {
    final ProductController controller = Get.put(ProductController());

    return Scaffold(
      appBar: AppBar(
        title: const Text('Products'),
        actions: [
          IconButton(
            icon: const Icon(Icons.refresh),
            onPressed: controller.fetchProductsFromAPI, // Refresh data
          ),
        ],
      ),
      body: Obx(() {
        if (controller.products.isEmpty) {
          return const Center(child: Text('No products available.'));
        }
        return ListView.builder(
          itemCount: controller.products.length,
          itemBuilder: (context, index) {
            final product = controller.products[index];
            return ListTile(
              leading: CachedNetworkImage(
                imageUrl: product.image,
                width: 50,
                height: 50,
                placeholder: (context, url) => const CircularProgressIndicator(),
                errorWidget: (context, url, error) => const Icon(Icons.error),
              ),
              title: Text(product.title),
              subtitle: Text("\$${product.price.toStringAsFixed(2)}"),
            );
          },
        );
      }),
    );
  }
}
```

**In this UI:**
1. The `Obx` widget listens to the `products` list and automatically updates the UI when the list changes.
2. The `product` images are displayed using `CachedNetworkImage` for caching the images locally.
3. The `"refresh"` button allows the user to fetch `products` again from the API.
