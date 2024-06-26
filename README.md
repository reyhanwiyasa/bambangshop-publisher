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
    | variable | type | description |
    |-----------------------|--------|------------------------------------------------------------|
    | APP_INSTANCE_ROOT_URL | string | URL address where this publisher instance can be accessed. |
2.  Use `cargo run` to run this app.
    (You might want to use `cargo check` if you only need to verify your work without running the app.)

## Mandatory Checklists (Publisher)

- [ ] Clone https://gitlab.com/ichlaffterlalu/bambangshop to a new repository.
- **STAGE 1: Implement models and repositories**
  - [ ] Commit: `Create Subscriber model struct.`
  - [ ] Commit: `Create Notification model struct.`
  - [ ] Commit: `Create Subscriber database and Subscriber repository struct skeleton.`
  - [ ] Commit: `Implement add function in Subscriber repository.`
  - [ ] Commit: `Implement list_all function in Subscriber repository.`
  - [ ] Commit: `Implement delete function in Subscriber repository.`
  - [ ] Write answers of your learning module's "Reflection Publisher-1" questions in this README.
- **STAGE 2: Implement services and controllers**
  - [ ] Commit: `Create Notification service struct skeleton.`
  - [ ] Commit: `Implement subscribe function in Notification service.`
  - [ ] Commit: `Implement subscribe function in Notification controller.`
  - [ ] Commit: `Implement unsubscribe function in Notification service.`
  - [ ] Commit: `Implement unsubscribe function in Notification controller.`
  - [ ] Write answers of your learning module's "Reflection Publisher-2" questions in this README.
- **STAGE 3: Implement notification mechanism**
  - [ ] Commit: `Implement update method in Subscriber model to send notification HTTP requests.`
  - [ ] Commit: `Implement notify function in Notification service to notify each Subscriber.`
  - [ ] Commit: `Implement publish function in Program service and Program controller.`
  - [ ] Commit: `Edit Product service methods to call notify after create/delete.`
  - [ ] Write answers of your learning module's "Reflection Publisher-3" questions in this README.

## Your Reflections

This is the place for you to write reflections:

### Mandatory (Publisher) Reflections

#### Reflection Publisher-1

1. Pada kasus BambangShop ini, kita tidak perlu membuat trait karena observernya hanya berupa satu class, yaitu Subscriber. Observer menggunakan trait diperlukan jika observer yang kita miliki terdiri dari berbagai macam class.

2. Pada kasus ini, penggunaan DashMap akan lebih efektif dibandingkan menggunakan Vec. DashMap memiliki kemampuan untuk mendukung keunikan dari id dan url sehingga kita tidak perlu mengiterasi semua data untuk memastikan data baru yang dimasukan unik. Lain halnya dengan Vec dimana kita harus mengiterasi setiap data yang ada untuk memastikan bahwa data yang dimasukan adalah unik.

3. Aplikasi BambangShop adalah aplikasi yang bersifat multithreading dan Map Subscriber akan diakses oleh banyak thread. Oleh karena itu, penggunaan DashMap lebih cocok digunakan karena sudah merupakan data structure yang thread-safe dan cocok untuk multi threading. Singleton pada program ini berguna untuk memastikan kalau subscriber pada produk kita hanya berada pada 1 dashmap sehingga tidak berantakan dalam struktur data.

#### Reflection Publisher-2

1. Memisahkan service dan controller dari model digunakan untuk meningkatkan maintainability, reusability, testability, dan scalibility. Hal ini dimungkinkan karena kita dapat mengubah business logic pada services, data akses pada repository, dan domain pada model dapat dilakukan secara terpisah sehingga mengurangi risiko munculnya bug.

2. Apabila hanya menggunakan model, maka maintanability akan sangat sulit dicapai. Hal ini disebabkan apabila terdapat perubahan pada kode, maka kita harus mengubah semua konsep yang ada, seperti model, data akses, dan logika. Di mana hal tersebut sangat berisiko tinggi dalam menimbulkan bug.

3. Menurut saya Postman sangat berguna karena dapat menampilkan apakah logic dari aplikasi yang kita buat sudah tepat atau belum. Hal ini sangat memudahkan developer dalam melakukan debugging dan menentukan masalah dari aplikasi yang dibuat.

#### Reflection Publisher-3

1. Pada tutorial kali ini, kita memanfaatkan push model. Ketika kita melakukan sesuatu pada modul objek misalnya create, delete, atau update, maka notification service akan memanggil method yang mengintegrasi subscriber untuk mendapatkan update terbarunya.

2. Model Pull memiliki keuntungan dalam mengurangi dependensi antara subscriber dan data yang diawasi, karena subscriber dapat secara independent meminta informasi terbaru sesuai kebutuhan mereka. Namun, model ini juga meningkatkan kompleksitas sistem dan memiliki risiko akan menimbulkan overload jika subscriber terlalu sering melakukan request. Selain itu, metode ini mungkin tidak efisien dalam situasi di mana data berubah secara cepat dan kontinu, karena pembaruan tidak disampaikan secara real-time kecuali diminta oleh subscriber.

3. Apabila proses notifikasi tidak menggunakan multi-threading, setiap subscriber akan diberi tahu secara bergantian, yang bisa menyebabkan keterlambatan dalam pengiriman notifikasi ke masing-masing subscriber. Hal ini mengakibatkan respons dari program secara keseluruhan melambat. Keadaan ini kurang ideal pada aplikasi yang memerlukan response time dengan cepat, misalnya dalam aplikasi real-time atau ketika jumlah subscriber sangat besar.
