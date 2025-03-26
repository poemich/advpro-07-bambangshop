# BambangShop Publisher App
Tutorial and Example for Advanced Programming 2024 - Faculty of Computer Science, Universitas Indonesia

---

## About this Project
In this repository, we have provided you a REST (REpresentational State Transfer) API project using Rocket web framework.

This project consists of four modules:
1.  `controller`: this module contains handler functions used to receive request and send responses.
    In Model-View-Controller (MVC) pattern, this is the Controller part.
2.  `model`: this module contains structs that serve as data containers.
    In MVC pattern, this is the Model part.
3.  `service`: this module contains structs with business logic methods.
    In MVC pattern, this is also the Model part.
4.  `repository`: this module contains structs that serve as databases and methods to access the databases.
    You can use methods of the struct to get list of objects, or operating an object (create, read, update, delete).

This repository provides a basic functionality that makes BambangShop work: ability to create, read, and delete `Product`s.
This repository already contains a functioning `Product` model, repository, service, and controllers that you can try right away.

As this is an Observer Design Pattern tutorial repository, you need to implement another feature: `Notification`.
This feature will notify creation, promotion, and deletion of a product, to external subscribers that are interested of a certain product type.
The subscribers are another Rocket instances, so the notification will be sent using HTTP POST request to each subscriber's `receive notification` address.

## API Documentations

You can download the Postman Collection JSON here: https://ristek.link/AdvProgWeek7Postman

After you download the Postman Collection, you can try the endpoints inside "BambangShop Publisher" folder.
This Postman collection also contains endpoints that you need to implement later on (the `Notification` feature).

Postman is an installable client that you can use to test web endpoints using HTTP request.
You can also make automated functional testing scripts for REST API projects using this client.
You can install Postman via this website: https://www.postman.com/downloads/

## How to Run in Development Environment
1.  Set up environment variables first by creating `.env` file.
    Here is the example of `.env` file:
    ```bash
    APP_INSTANCE_ROOT_URL="http://localhost:8000"
    ```
    Here are the details of each environment variable:
    | variable              | type   | description                                                |
    |-----------------------|--------|------------------------------------------------------------|
    | APP_INSTANCE_ROOT_URL | string | URL address where this publisher instance can be accessed. |
2.  Use `cargo run` to run this app.
    (You might want to use `cargo check` if you only need to verify your work without running the app.)

## Mandatory Checklists (Publisher)
-   [x] Clone https://gitlab.com/ichlaffterlalu/bambangshop to a new repository.
-   **STAGE 1: Implement models and repositories**
    -   [x] Commit: `Create Subscriber model struct.`
    -   [x] Commit: `Create Notification model struct.`
    -   [x] Commit: `Create Subscriber database and Subscriber repository struct skeleton.`
    -   [x] Commit: `Implement add function in Subscriber repository.`
    -   [x] Commit: `Implement list_all function in Subscriber repository.`
    -   [x] Commit: `Implement delete function in Subscriber repository.`
    -   [x] Write answers of your learning module's "Reflection Publisher-1" questions in this README.
-   **STAGE 2: Implement services and controllers**
    -   [ ] Commit: `Create Notification service struct skeleton.`
    -   [ ] Commit: `Implement subscribe function in Notification service.`
    -   [ ] Commit: `Implement subscribe function in Notification controller.`
    -   [ ] Commit: `Implement unsubscribe function in Notification service.`
    -   [ ] Commit: `Implement unsubscribe function in Notification controller.`
    -   [ ] Write answers of your learning module's "Reflection Publisher-2" questions in this README.
-   **STAGE 3: Implement notification mechanism**
    -   [ ] Commit: `Implement update method in Subscriber model to send notification HTTP requests.`
    -   [ ] Commit: `Implement notify function in Notification service to notify each Subscriber.`
    -   [ ] Commit: `Implement publish function in Program service and Program controller.`
    -   [ ] Commit: `Edit Product service methods to call notify after create/delete.`
    -   [ ] Write answers of your learning module's "Reflection Publisher-3" questions in this README.

## Your Reflections
This is the place for you to write reflections:

### Mandatory (Publisher) Reflections

#### Reflection Publisher-1

1. Q: In the Observer Pattern diagram from the Head First Design Patterns book, Subscriber is depicted as an interface. Based on your understanding of observer design patterns, is it necessary to define a subscriber using an interface (or trait in Rust) in BambangShop, or is a simple struct sufficient?  
   A: For BambangShop, a simple Subscriber struct is sufficient because we currently only have one kind of subscriber. This approach keeps the design simple and easy to understand. It avoids unnecessary complexity that would come from managing multiple types through a trait. However, if the system evolves and introduces more diverse subscriber behaviors, a trait would then provide a clear contract to implement those differences. This design choice ensures that the solution is both practical for the current scenario and flexible enough for future changes. Overall, simplicity is maintained without sacrificing potential expansion.

2. Q: Considering that Program.id and Subscriber.url are intended to be unique, is using a Vec (list) adequate, or is employing a DashMap (map/dictionary) necessary?  
   A: Using a DashMap is preferable because it helps to achieve constant time O(1) lookups, which is crucial as the system scales. With a Vec, searching for an element is an O(n) operation, which can become inefficient when handling a large list of subscribers. The DashMap’s design supports uniqueness by allowing direct key-based access, providing both speed and reliability. In addition, DashMap handles concurrent access, which is important in a multi-threaded environment like Rust. This ensures that as the number of subscribers grows, performance remains optimal. Therefore, DashMap is the more suitable choice for maintaining uniqueness and high performance.

3. Q: Rust enforces strict thread-safety rules. In managing the static Subscribers variable, is DashMap necessary, or could a Singleton pattern with a standard HashMap suffice?  
   A: The combination of a singleton (implemented via lazy_static) with DashMap is ideal because it guarantees thread safety without adding extra complexity. Lazy_static ensures that only a single instance of the data store is created, while DashMap provides safe concurrent access to that instance. A standard HashMap would require additional synchronization mechanisms such as mutexes, which could complicate the code and potentially degrade performance. DashMap inherently manages concurrent read and write operations efficiently. The design leverages Rust’s strengths in safe concurrency while maintaining clarity in the code. Overall, this approach offers simplicity, efficiency, and robust thread safety for the application.

#### Reflection Publisher-2

#### Reflection Publisher-3