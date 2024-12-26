## Index
1. Create UI
2. Create Controller
3. Create Model
4. Create Api Service Class

Keep in mind controller always should be on root widget. To use controller:  
**Dependency injection**: `CrudController controller = Get.put(CrudController());`

First thing first, How we can get data from server? look data can be in different different format. Well I'm talking about `Json` data.  
**Ex:**

1. **Nested Json**:
```json
{
    "status": "OK",
    "data": {
        "id": 13,
        "title": "Java",
        "description": "Lets do this....",
        "isCompleted": true
    }
}
```
2. **Single Json**:
```json
 {
     "id": 13,
     "title": "Java",
     "description": "Lets do this....",
     "isCompleted": true
 }
```

#### How we can handle Nested Json:
```dart
var decodedValues = jsonDecode(response.body); //You can use Map<String, dynamic> also its fine

for(var items in decodedValues['data']){
  allTodos.add(Data.fromJson(items));
}
```

#### How we can handle Single Json:
```dart
var decodedValues = jsonDecode(response.body);

for(var items in decodedValues){
  allTodos.add(Data.fromJson(items));
}
```

#### There is also two way to create model
1. Simple approach: This approach pretty simple but you have to write additonal code for get, create, and update.
```dart
class Todos{
  Long id;
  String title;
  String description;
  Boolean isCompleted;
}
```
2. Json to Dart (Convert it no need to do it manually)
```dart
class Item {
  String? status;
  List<Data>? data;

  Item({this.status, this.data});

  Item.fromJson(Map<String, dynamic> json) {
    status = json['status'];
    if (json['data'] != null) {
      data = <Data>[];
      json['data'].forEach((v) {
        data!.add(new Data.fromJson(v));
      });
    }
  }

  Map<String, dynamic> toJson() {
    final Map<String, dynamic> data = new Map<String, dynamic>();
    data['status'] = this.status;
    if (this.data != null) {
      data['data'] = this.data!.map((v) => v.toJson()).toList();
    }
    return data;
  }
}

class Data {
  int? id;
  String? title;
  String? description;
  bool? isCompleted;

  Data({this.id, this.title, this.description, this.isCompleted});

  Data.fromJson(Map<String, dynamic> json) {
    id = json['id'];
    title = json['title'];
    description = json['description'];
    isCompleted = json['isCompleted'];
  }

  Map<String, dynamic> toJson() {
    final Map<String, dynamic> data = new Map<String, dynamic>();
    data['id'] = this.id;
    data['title'] = this.title;
    data['description'] = this.description;
    data['isCompleted'] = this.isCompleted;
    return data;
  }
}
```
