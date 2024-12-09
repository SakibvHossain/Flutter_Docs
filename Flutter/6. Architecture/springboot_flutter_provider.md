# Spring Boot API into Flutter App using Clean Architecture with Provider State Management

### Folder Structure
Clean Architecture in Flutter typically involves four main layers: **Presentation**, **Application**, **Domain**, and **Data**.

```
lib/
├── core/
│   └── utils/                           # Shared utility functions/constants
├── data/                                # Data Layer
│   ├── data_sources/
│   │   └── task_remote_data_source.dart # Manages API calls
│   └── repositories/
│       └── task_repository_impl.dart    # Implements Domain repository interface
├── domain/                              # Domain Layer
│   ├── entities/
│   │   └── task.dart                    # Task entity
│   ├── repositories/
│   │   └── task_repository.dart         # Abstract repository interface
│   └── usecases/
│       └── get_tasks.dart               # Use case for fetching tasks
├── application/                         # Application Layer
│   └── providers/                       # State management with Provider
│       └── task_provider.dart           # Task provider class
└── presentation/                        # Presentation Layer (UI)
    ├── pages/
    │   └── task_page.dart               # UI page for tasks
    └── widgets/
        └── task_list_item.dart          # Reusable widget for task display
```

## Step by Step Implementation
#### Step 1: Domain Layer (Business Logic)
1. **Define Entities**: Define a `Task` entity representing your core business object.
2. **Define Repository Interface**: Define `TaskRepository` to abstract data access.
3. **Define Use Cases**: Create `GetTasks` to handle business logic for fetching tasks.
#### Step 2: Data Layer (API Implementation)
1. **Remote Data Source**: Define `TaskRemoteDataSource` to interact with the Spring Boot API.
2. **Repository Implementation**: Implement the `TaskRepository` interface.
#### Step 3: Application Layer (Provider for State Management)
1. **Task Provider**: Implement `TaskProvider` using Provider for state management.
#### Step 4: Presentation Layer (UI)
1. **Create Task Page**: Use Consumer to observe changes from `TaskProvider`.
2. **Task List Item Widget**: Display each task item.
#### Step 5: Dependency Injection and Provider Setup
1. **Main App Setup**: Register TaskProvider at the root of the widget tree.

### Explanation of the Workflow
* Task Page (UI) -> TaskProvider (Provider for State Management) -> GetTasks Use Case -> TaskRepository Interface -> TaskRepositoryImpl -> TaskRemoteDataSource -> Spring Boot API

This setup follows Clean Architecture principles, with each layer fulfilling a single responsibility. The TaskProvider uses ChangeNotifier to manage state, and the Consumer widget in TaskPage observes the TaskProvider for changes, allowing for a clean, reactive UI.
