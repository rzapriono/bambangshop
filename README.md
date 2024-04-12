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
-   [ ] Clone https://gitlab.com/ichlaffterlalu/bambangshop to a new repository.
-   **STAGE 1: Implement models and repositories**
    -   [x] Commit: `Create Subscriber model struct.`
    -   [x] Commit: `Create Notification model struct.`
    -   [x] Commit: `Create Subscriber database and Subscriber repository struct skeleton.`
    -   [x] Commit: `Implement add function in Subscriber repository.`
    -   [x] Commit: `Implement list_all function in Subscriber repository.`
    -   [x] Commit: `Implement delete function in Subscriber repository.`
    -   [x] Write answers of your learning module's "Reflection Publisher-1" questions in this README.
-   **STAGE 2: Implement services and controllers**
    -   [x] Commit: `Create Notification service struct skeleton.`
    -   [x] Commit: `Implement subscribe function in Notification service.`
    -   [x] Commit: `Implement subscribe function in Notification controller.`
    -   [x] Commit: `Implement unsubscribe function in Notification service.`
    -   [x] Commit: `Implement unsubscribe function in Notification controller.`
    -   [x] Write answers of your learning module's "Reflection Publisher-2" questions in this README.
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
1. In the Observer pattern diagram explained by the Head First Design Pattern book, Subscriber is defined as an interface. Explain based on your understanding of Observer design patterns, do we still need an interface (or trait in Rust) in this BambangShop case, or a single Model struct is enough?
    
    Pada observer pattern, penggunaan interface (trait pada rust) bergantung pada keragaman dan jenis observer yang terlibat. Lebih tepatnya, penggunaan interface/trait dilakukan jika objek observer terdiri dari beragam tipe, contohnya pada kasus Dog, Cat, dan Mouse. Pada kasus tersebut, interface/trait dibutuhkan karena terdapat kebutuhan untuk menangani berbagai tipe atau jenis subscriber yang masing-masing memiliki behavior yang berbeda. Untuk kasus BambangShop, karena semua subscriber dianggap memiliki behavior yang sama, maka penggunaan interface/trait tidak diperlukan.

2. id in Product and url in Subscriber is intended to be unique. Explain based on your understanding, is using Vec (list) sufficient or using DashMap (map/dictionary) like we currently use is necessary for this case?
    
    DashMap lebih cocok untuk digunakan pada kasus ini dibanding dengan menggunakan Vec (list). Dengan menggunakan DashMap, kita bisa menggunakan id pada product atau url pada subscriber sebagai key karena kedua variabel tersebut dijamin unik. Dengan demikian, kita hanya perlu membuat 1 DashMap dibanding harus menggunakan 2 Vec terpisah masing-masing untuk url dan subscriber. Selain itu, operasi pengaksesan data dengan menggunakan key pada DashMap juga lebih efisien, sehingga tak hanya memberikan kemudahan dalam pengelolaan data tetapi juga memberikan keuntungan dalam hal efisiensi.

3. When programming using Rust, we are enforced by rigorous compiler constraints to make a thread-safe program. In the case of the List of Subscribers (SUBSCRIBERS) static variable, we used the DashMap external library for thread safe HashMap. Explain based on your understanding of design patterns, do we still need DashMap or we can implement Singleton pattern instead?

    DashMap tetap dibutuhkan untuk memastikan bahwa data yang diakses oleh thread yang berbeda tetap konsisten. Hal ini karena DashMap sendiri merupakan HashMap yang bersifat thread-safe dan mendukung operasi concurrent pada pasangan key-value, yang berarti DashMap memungkinkan akses dan modifikasi data secara simultan oleh berbagai thread dengan tetap memperhatikan keamanan data. Walaupun singleton pattern menyediakan cara untuk mengakses satu instance dari kelas, implementasi dasarnya tidak secara otomatis thread-safe. Oleh karena itu, penggunaan singleton pattern dengan penerapan seperti menggunakan lazy initialization bersamaan dengan DashMap dapat menjadi solusi untuk memastikan bahwa data Subscribers tetap aman dalam multithreaded environment dan hanya terdapat satu instance subscribers untuk diakses banyak thread.

#### Reflection Publisher-2
1. In the Model-View Controller (MVC) compound pattern, there is no “Service” and “Repository”. Model in MVC covers both data storage and business logic. Explain based on your understanding of design principles, why we need to separate “Service” and “Repository” from a Model?

     Pemisahan service dengan repository bertujuan untuk meningkatkan modularitas, fleksibilitas, dan keberlanjutan kode. Pemisahan service dan repository juga penting dilakukan untuk
     mengurangi ketergantungan langsung antara business logic dan akses data, karena repository bertanggung jawab atas penyimpanan dan pengambilan data sementara service mengelola business logic. Hal ini sesuai dengan prinsip Separation of Concern, dimana tiap bagian dipisahkan sesuai fungsinya masing-masing untuk mencapai hal-hal yang telah disebutkan sebelumnya. Selain itu, pemisahan service dengan repository berdasarkan Single Responsibility Principle, dimana service mengolah data dari Repository dan Repository yang mengakses database juga penting dilakukan agar kita membuat kode yang mudah dimaintain kedepannya. 

2. What happens if we only use the Model? Explain your imagination on how the interactions between each model (Program, Subscriber, Notification) affect the code complexity for each model?

    Jika hanya menggunakan model, maka business logic dan akses data akan bercampur, sehingga kode akan menjadi sulit dipahami karena kompleksitasnya yang tinggi, sulit dilakukan testing, dan sulit dimaintain. Penggunaan model saja menyebabkan ketergantungan dan terlalu berkaitannya suatu bagian kode dengan bagian-bagian lain, dimana setiap kali kita ingin mengubah bagian dari kode, kita harus memperhatikan bagaimana perubahan tersebut mempengaruhi bagian lain dari kode. Dengan hanya menggunakan model, kita juga harus memperhatikan bagaimana model tersebut berinteraksi dengan model lainnya. Hal ini menyebabkan kode lebih sulit untuk dimaintain, tidak modular, dan menjadi kurang fleksibel. 

3. Have you explored more about Postman? Tell us how this tool helps you to test your current work. Maybe you want to also list which features in Postman you are interested in or feel like it’s helpful to help your Group Project or any of your future software engineering projects.

    Sebelumnya, saya sudah pernah menggunakan Postman pada mata kuliah Pemrograman Berbasis Platform (PBP) untuk mengirimkan http request seperti GET, POST, DELETE dengan suatu data pada body atau parameter ke endpoint tertentu. Menurut saya, Postman sangat membantu dalam melakukan testing API, karena kita bisa mengirimkan request ke server untuk suatu endpoint dan mengecek data yang diterima pada endpoint tersebut serta responsenya tanpa harus membuat frontend terlebih dahulu, sehingga kita bisa fokus pada pengembangan backend dan fungsionalitas program.

#### Reflection Publisher-3
