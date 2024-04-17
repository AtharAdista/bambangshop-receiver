# BambangShop Receiver App
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
4.  `repository`: this module contains structs that serve as databases.
    You can use methods of the struct to get list of objects, or operating an object (create, read, update, delete).

This repository provides a Rocket web framework skeleton that you can work with.

As this is an Observer Design Pattern tutorial repository, you need to implement a feature: `Notification`.
This feature will receive notifications of creation, promotion, and deletion of a product, when this receiver instance is subscribed to a certain product type.
The notification will be sent using HTTP POST request, so you need to make the receiver endpoint in this project.

## API Documentations

You can download the Postman Collection JSON here: https://ristek.link/AdvProgWeek7Postman

After you download the Postman Collection, you can try the endpoints inside "BambangShop Receiver" folder.

Postman is an installable client that you can use to test web endpoints using HTTP request.
You can also make automated functional testing scripts for REST API projects using this client.
You can install Postman via this website: https://www.postman.com/downloads/

## How to Run in Development Environment
1.  Set up environment variables first by creating `.env` file.
    Here is the example of `.env` file:
    ```bash
    ROCKET_PORT=8001
    APP_INSTANCE_ROOT_URL=http://localhost:${ROCKET_PORT}
    APP_PUBLISHER_ROOT_URL=http://localhost:8000
    APP_INSTANCE_NAME=Safira Sudrajat
    ```
    Here are the details of each environment variable:
    | variable                | type   | description                                                     |
    |-------------------------|--------|-----------------------------------------------------------------|
    | ROCKET_PORT             | string | Port number that will be listened by this receiver instance.    |
    | APP_INSTANCE_ROOT_URL   | string | URL address where this receiver instance can be accessed.       |
    | APP_PUUBLISHER_ROOT_URL | string | URL address where the publisher instance can be accessed.       |
    | APP_INSTANCE_NAME       | string | Name of this receiver instance, will be shown on notifications. |
2.  Use `cargo run` to run this app.
    (You might want to use `cargo check` if you only need to verify your work without running the app.)
3.  To simulate multiple instances of BambangShop Receiver (as the tutorial mandates you to do so),
    you can open new terminal, then edit `ROCKET_PORT` in `.env` file, then execute another `cargo run`.

    For example, if you want to run 3 (three) instances of BambangShop Receiver at port `8001`, `8002`, and `8003`, you can do these steps:
    -   Edit `ROCKET_PORT` in `.env` to `8001`, then execute `cargo run`.
    -   Open new terminal, edit `ROCKET_PORT` in `.env` to `8002`, then execute `cargo run`.
    -   Open another new terminal, edit `ROCKET_PORT` in `.env` to `8003`, then execute `cargo run`.

## Mandatory Checklists (Subscriber)
-   [x] Clone https://gitlab.com/ichlaffterlalu/bambangshop-receiver to a new repository.
-   **STAGE 1: Implement models and repositories**
    -   [x] Commit: `Create Notification model struct.`
    -   [x] Commit: `Create SubscriberRequest model struct.`
    -   [x] Commit: `Create Notification database and Notification repository struct skeleton.`
    -   [x] Commit: `Implement add function in Notification repository.`
    -   [x] Commit: `Implement list_all_as_string function in Notification repository.`
    -   [x] Write answers of your learning module's "Reflection Subscriber-1" questions in this README.
-   **STAGE 3: Implement services and controllers**
    -   [x] Commit: `Create Notification service struct skeleton.`
    -   [x] Commit: `Implement subscribe function in Notification service.`
    -   [x] Commit: `Implement subscribe function in Notification controller.`
    -   [x] Commit: `Implement unsubscribe function in Notification service.`
    -   [x] Commit: `Implement unsubscribe function in Notification controller.`
    -   [x] Commit: `Implement receive_notification function in Notification service.`
    -   [x] Commit: `Implement receive function in Notification controller.`
    -   [x] Commit: `Implement list_messages function in Notification service.`
    -   [x] Commit: `Implement list function in Notification controller.`
    -   [x] Write answers of your learning module's "Reflection Subscriber-2" questions in this README.

## Your Reflections
This is the place for you to write reflections:

### Mandatory (Subscriber) Reflections

#### Reflection Subscriber-1
1. Kita menggunakan RwLock karena kita ingin banyak thread untuk membaca data notification secara bersamaan dan hanya boleh satu thread untuk melakukan penulisan atau memodifikasi isi vec notifikasi pada suatu waktu tertentu. Dengan menggunakan RwLock, kita dapat memastikan bahwa hanya satu thread yang dapat melakukan write pada suatu waktu tertentu, sehingga menghindari konflik dan kemungkinan terjadinya race conditions. Alasan kita lebih memilih RwLock<> daripada Mutex<> adalah karena pada RwLock<> seperti yang sudah dijelaskan bahwa reader dapat melakukan read data notification secara bersamaan yang mana cocok untuk diterapkan jika kita memiliki banyak operasi read dan sedikit operasi write, tidak seperti mutex<> yang akan membuat seluruh vec terkunci ketika satu thread sedang melakukan suatu operasi, sehingga nantinya akan menghambat akses baca dari thread lainnya. 

2. Dalam Rust, "static" variable memilik sifat yang berbeda dengan static variable dalam bahasa java. Di Rust "static" variable defaultnya bersifat immutable dan thread-safe. Ini diterapkan untuk mencegah masalah konkurensi yang umumnya terjadi, selain itu Rust tidak mengizinkan mutasi langsung terhadap variabel statis karena alasan keamanan dan kestabilan, seperti untuk mencegah kondisi race condition. Dengan membuat "static" variable immutable secara default, Rust mendorong kita untuk menggunakan struktur data yang bersifat thread-safe untuk mengakses data secara bersama secara aman. Oleh karena itu, Rust tidak mengizinkan pengubahan nilai "static" variable secara langsung karena dapat membahayakan keamanan dan kestabilan program, terutama dalam lingkungan konkurensi.

#### Reflection Subscriber-2
1. Saya sudah membaca mengenai src/lib.rs, src/lib.rs merupakan titik awal dari sebuah library rust dan berfungsi sebagai tempat untuk menuliskan kode utama yang ingin dipublikasikan. Isi dari file src/lib.rs akan terdiri dari definisi modul dan fungsi-fungsi yang akan dipublikasikan oleh library. Ketika kita membuat proyek rust baru menggunakan cargo, maka file src/lib.rs akan otomatis dibuat dalam src. Hal lain yang sudah saya pelajari mengenai Rust adalah bagaimana cara melakukan testing di Rust, walaupun saya belum terlalu memahaminya. Selain itu, saya juga baru mempelajari mengenai lazy_static, layz_static berfungsi untuk membuat variabel statik yang diinit secara lazy saat pertama kali diakses, sehingga variabel akan diinit saat pertama kali diakses, bukan saat aplikasi dimulai, sehingga akan menciptakan efisiensi memori dan dapat mencegah deadlock.

2. Observer pattern dapat mempermudah penambahan subscriber dikarenakan pada observer pattern, publisher memberitahukan setiap subscriber yang melakukan subscribe jika terjadi perubahan, sedangkan para subscriber tidak mengetahui satu sama lain. Pemisahan ini membuat sistem semakin fleksibel dan mudah diperluas di mana subscriber baru dapat dengan mudah ditambahkan tanpa memodifikasi struktur utama aplikasi, publisher atau subscriber lainnya. Untuk membuat lebih dari satu instance aplikasi utama bisa dilakukan jika observer pattern diimplementasikan dengan baik. Setiap instance dari aplikasi utama dapat memiliki kumpulan pelanggan sendiri, dan menambahkan pelanggan baru ke setiap instance mengikuti proses yang sama. Namun, jika observer pattern tidak dirancang untuk menangangi beberapa instance dari aplikasi utama, maka akan meningkatkan kompleksitas dan memerlukan mekanisme tambahan sinkronisasi tambahan untuk memastikan bahwa subscriber diberitahu dengan benar di semua instance.

3. Sudah tapi baru beberapa saja, seperti sedikit pengujian dan pembuatan dokumentasi. Pengujian sangat membantu saya ketika ingin melakukan pengujian API dan memastikan bahwa API berfungsi dengan baik, pembuatan dokumentasi membantu saya agar API yang saya buat dapat dipahami oleh orang. Semua fitur yang disediakan postman sangat membantu pekerjaan saya agar lebih efisien daripada sebelumnya.
