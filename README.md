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
1. In this tutorial, we used <b>RwLock<></b> to synchronise the use of <b>Vec</b> of <b>Notifications</b>. Explain why it is necessary for this case, and explain why we do not use <b>Mutex<></b> instead?

   - Penggunaan `RwLock` untuk penyinkronan penggunaan `Vec` dari `Notifications` agar memungkinkan beberapa Thread membaca data secara bersamaan, selama tidak ada thread yang sedang menulis. Hal ini sangat penting untuk peningkatan performa ketika terdapat banyak operasi baca dibanding operasi tulis. Sebaliknya, `Mutex` hanya menggunakan satu thread untuk mengakses data, baik membaca maupun menulis, sehingga dapat menyebabkan bottleneck pada operasi baca yang sering terjadi. Oleh karena itu, pada kasus ini, `RwLock` lebih efisien karena pola akses data yang lebih sering membutuhkan baca daripada tulis sehingga kinerja aplikasi lebih optimal.

2. In this tutorial, we used <b>lazy_static</b> external library to define <b>Vec</b> and <b>DashMap</b> as a <b>“static”</b> variable. Compared to Java where we can mutate the content of a <b>static</b> variable via a <b>static</b> function, why did not Rust allow us to do so?

   - Rust tidak mengizinkan mutasi langsung pada variabel `static` karena Rust mengutamakan keamanan memori dan mencegah `race conditions`. Dalam Rust, semua akses ke data bersama harus disinkronkan secara eksplisit menggunakan alat seperti `Mutex` atau `RwLock`. Sebaliknya, di Java, mutasi pada variabel `static` diperbolehkan karena Java mengandalkan garbage collector dan runtime checks untuk menangani masalah seperti ini. Dengan menggunakan pustaka `lazy_static`, Rust memungkinkan inisialisasi variabel statis yang aman dan mendukung mutasi melalui mekanisme sinkronisasi. Pendekatan ini memastikan bahwa Rust tetap aman tanpa mengorbankan fleksibilitas.

#### Reflection Subscriber-2
1. Have you explored things outside of the steps in the tutorial, for example: src/lib.rs? If not, explain why you did not do so. If yes, explain things that you have learned from those other parts of code.

   - Saya telah mengeksplorasi `APP_CONFIG` dan `REQWEST_CLIENT`, yang merupakan bagian penting dari proyek ini. `APP_CONFIG` digunakan untuk mengintegrasikan konfigurasi environment dengan sistem aplikasi, memungkinkan pengaturan seperti URL dan port dapat dikelola secara fleksibel melalui file `.env`. Sementara itu, `REQWEST_CLIENT` berfungsi sebagai `HTTP client` yang reusable, mempermudah pengiriman permintaan HTTP secara efisien di berbagai bagian aplikasi. Dari eksplorasi ini, saya memahami bagaimana framework Rocket memanfaatkan konfigurasi dan komponen reusable untuk mendukung pengembangan aplikasi yang modular. Hal ini memberikan wawasan tentang bagaimana struktur dasar aplikasi dirancang untuk mendukung fleksibilitas dan skalabilitas.

2. Since you have completed the tutorial by now and have tried to test your notification system by spawning multiple instances of Receiver, explain how Observer pattern eases you to plug in more subscribers. How about spawning more than one instance of Main app, will it still be easy enough to add to the system?

   - Observer pattern mempermudah penambahan subscriber baru karena setiap subscriber hanya perlu mendaftarkan dirinya ke publisher tanpa memengaruhi sistem yang sudah ada. Pola ini memungkinkan notifikasi dikirim secara otomatis ke semua subscriber yang terdaftar, sehingga skalabilitas sistem terjaga. Dengan menambahkan lebih banyak instance Receiver, sistem tetap berjalan lancar karena setiap instance dapat berfungsi secara independen. Namun, jika lebih dari satu instance Main app ditambahkan, kompleksitas pengelolaan sinkronisasi antar-publisher dapat meningkat, sehingga memerlukan mekanisme tambahan untuk memastikan konsistensi data. Meskipun demikian, Observer pattern tetap memberikan fleksibilitas tinggi dalam mengelola hubungan antara publisher dan subscriber.

3. Have you tried to make your own Tests, or enhance documentation on your <b>Postman collection</b>? If you have tried those features, tell us whether it is useful for your work (it can be your tutorial work or your Group Project).

   - Membuat tes sendiri atau meningkatkan dokumentasi pada `Postman Collection` sangat bermanfaat untuk memastikan API bekerja sesuai dengan spesifikasi. Dengan Postman, pengujian endpoint dapat dilakukan secara efisien, termasuk pengujian otomatis untuk memvalidasi respons dan perilaku API. Dokumentasi yang ditingkatkan membantu tim memahami cara kerja API dan mempermudah integrasi dengan sistem lain. Selain itu, fitur seperti skrip pengujian otomatis di Postman dapat menghemat waktu dalam mendeteksi bug atau masalah pada API. Secara keseluruhan, Postman Collection meningkatkan kualitas pengembangan dan pengujian API secara signifikan.
