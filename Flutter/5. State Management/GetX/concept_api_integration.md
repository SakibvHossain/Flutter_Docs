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
1. **Simple approach**: This approach pretty simple but you have to write additonal code for get, create, and update.
```dart
class Todos{
  Long id;
  String title;
  String description;
  Boolean isCompleted;
}
```
2. **Json to Dart** (Convert it no need to do it manually)
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


If you want universal solution. Like creating one **ApiService** class which will do rest of the job. Well yap its possible.

I'm backend developer so I love creating abstract class then implement what I actually need for example ApiService:

**ApiService.dart**
```dart
abstract class ApiService{
  Future<void> getTodos() async {} //For getting the todos
  Future<void> createTodos(Map<String, dynamic> todos) async {} //For creating todos
  Future<void> updateTodos(Map<String, dynamic> todos, int id) async{} //For update todos
  Future<void> deleteTodos(int id) async{} //For deleting todos
}
```

ApiServiceImpl.dart
```dart
class HttpServiceImpl implements HttpService {
  final String _baseUrl = 'http://localhost:8083/api/task';

  @override
  Future<http.Response> getTodos() async {
    Uri parseGetTodoURL = Uri.parse('$_baseUrl/all_task');

    http.Response response = await http.get(parseGetTodoURL);
    return response;
  }
  
  @override
  Future<http.Response> createTodos(Map<String, dynamic> todos) async {
    Uri parseCreateTodoURL = Uri.parse('$_baseUrl/create');

   http.Response createTodosResponse =  await http.post(parseCreateTodoURL, headers: {"Content-Type": "application/json"}, body: jsonEncode(todos));

   return createTodosResponse;
  }

  @override
  Future<http.Response> updateTodos(Map<String, dynamic> todos, int id) async{
    Uri parseUpdateTodoURL = Uri.parse('$_baseUrl/update/${id.toString()}');
    http.Response createTodosResponse =  await http.put(parseUpdateTodoURL, headers: {"Content-Type": "application/json"}, body: jsonEncode(todos));

    return createTodosResponse;
  }

  @override
  Future<http.Response> deleteTodos(int id) async{
    Uri parseDeleteUrl = Uri.parse('$_baseUrl/delete/${id.toString()}');
    http.Response deleteTodoResponse = await http.delete(parseDeleteUrl);
    return deleteTodoResponse;
  }
}
```
