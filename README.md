# Flutter Clean Architecture Template

This repository serves as a robust and scalable starter template for new Flutter projects, meticulously structured around the principles of Clean Architecture. It provides a solid foundation for building maintainable and testable applications.

## Architecture Overview

The project is divided into three primary layers: Domain, Data, and Presentation. This separation ensures that the business logic is independent of the UI and data sources.

### Directory Structure

The core logic is organized within the `lib` directory as follows:

```
lib/
├── core/
│   ├── connection/          # Network connectivity checker
│   ├── databases/
│   │   ├── api/             # API consumer (Dio) and endpoint definitions
│   │   └── cache/           # Local data caching (SharedPreferences)
│   ├── errors/            # Custom Failure and Exception classes
│   └── params/            # Parameters for UseCases
│
└── features/
    └── template/            # A reusable template for building new features
        ├── data/
        │   ├── datasources/   # Remote and Local data sources
        │   ├── models/        # Data Transfer Objects (DTOs)
        │   └── repositories/  # Implementation of the Domain repository
        ├── domain/
        │   ├── entities/      # Core business objects
        │   ├── repositories/  # Abstract repository contracts
        │   └── usecases/      # Application-specific business rules
        └── presentation/
            └── screens/       # UI Widgets and Screens
```

### Layers

#### 1. Domain Layer
This is the core of the application, containing the enterprise-wide business logic. It is completely independent of any other layer.
- **Entities**: Core business objects. In this template, `TemplateEntity` is an example.
- **Repositories**: Abstract contracts or interfaces that define the operations required by the domain layer. The Data layer will implement these.
- **Use Cases**: Encapsulate specific business rules and orchestrate the flow of data to and from entities. Example: `GetTemplate`.

#### 2. Data Layer
This layer is responsible for retrieving data from one or more sources (API, local database, etc.). It implements the repository contracts defined in the Domain layer.
- **Models**: Data Transfer Objects (DTOs) that are specific to the data source. They may include parsing logic (e.g., `fromJson`, `toJson`) and can be mapped to and from Domain Entities.
- **Data Sources**: Abstract classes for fetching data (e.g., `TemplateRemoteDataSource`, `TemplateLocalDataSource`).
- **Repositories Implementation**: Concrete implementation of the Domain layer's repository interface. It decides whether to fetch data from a remote source or a local cache.

#### 3. Presentation Layer
This is the outermost layer, responsible for all UI-related logic.
- **Screens**: Contains all the widgets and screens that the user interacts with. This layer interacts with the Domain layer through Use Cases to get data and display it.

## Core Components

The `/lib/core` directory contains shared modules used across different features.

- **API Handling**: `DioConsumer` is a wrapper around the `Dio` package for handling RESTful API calls (`GET`, `POST`, `PATCH`, `DELETE`). API endpoints are centralized in `end_points.dart`.
- **Error Handling**: The template uses `dartz` to handle states and errors gracefully with `Either<Failure, T>`. Custom `Exception` and `Failure` classes provide structured error information from data sources to the UI.
- **Caching**: `CacheHelper` provides a simple abstraction over `shared_preferences` for caching data locally. The template repository (`TemplateRepositoryImpl`) demonstrates a cache-first-then-network strategy.
- **Connection Checking**: `NetworkInfo` utility checks for active internet connectivity before attempting to make a network request.

## Getting Started

To use this template for a new project:

1.  **Clone the repository:**
    ```bash
    git clone https://github.com/AlwaleedKhalidAltrimi/clean_architecture_template.git your_project_name
    ```

2.  **Navigate to the project directory:**
    ```bash
    cd your_project_name
    ```

3.  **Install dependencies:**
    ```bash
    flutter pub get
    ```

4.  **Configure API Endpoints:**
    Modify `lib/core/databases/api/end_points.dart` to set your `baseUrl` and other API routes.

5.  **Create a New Feature:**
    - Duplicate the `lib/features/template` directory and rename it to your new feature's name (e.g., `user`).
    - Rename the classes and files inside from `Template...` to `User...`.
    - Implement your specific logic within the new feature's layers.

## Key Dependencies

- **`dio`**: A powerful HTTP client for Dart, which supports Interceptors, Global configuration, FormData, and more.
- **`dartz`**: Functional programming in Dart, used here for `Either` to handle failures and successes.
- **`shared_preferences`**: Provides a simple way to store key-value data on disk.
- **`data_connection_checker_tv`**: A pure Dart library to check for internet connection.
