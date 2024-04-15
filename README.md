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

- [x] Clone https://gitlab.com/ichlaffterlalu/bambangshop to a new repository.
- **STAGE 1: Implement models and repositories**
  - [x] Commit: `Create Subscriber model struct.`
  - [x] Commit: `Create Notification model struct.`
  - [x] Commit: `Create Subscriber database and Subscriber repository struct skeleton.`
  - [x] Commit: `Implement add function in Subscriber repository.`
  - [x] Commit: `Implement list_all function in Subscriber repository.`
  - [x] Commit: `Implement delete function in Subscriber repository.`
  - [x] Write answers of your learning module's "Reflection Publisher-1" questions in this README.
- **STAGE 2: Implement services and controllers**
  - [x] Commit: `Create Notification service struct skeleton.`
  - [x] Commit: `Implement subscribe function in Notification service.`
  - [x] Commit: `Implement subscribe function in Notification controller.`
  - [x] Commit: `Implement unsubscribe function in Notification service.`
  - [x] Commit: `Implement unsubscribe function in Notification controller.`
  - [x] Write answers of your learning module's "Reflection Publisher-2" questions in this README.
- **STAGE 3: Implement notification mechanism**
  - [x] Commit: `Implement update method in Subscriber model to send notification HTTP requests.`
  - [x] Commit: `Implement notify function in Notification service to notify each Subscriber.`
  - [x] Commit: `Implement publish function in Program service and Program controller.`
  - [x] Commit: `Edit Product service methods to call notify after create/delete.`
  - [x] Write answers of your learning module's "Reflection Publisher-3" questions in this README.

## Your Reflections

This is the place for you to write reflections:

### Mandatory (Publisher) Reflections

#### Reflection Publisher-1

1. In the Observer pattern diagram explained by the Head First Design Pattern book, Subscriber is defined as an interface. Explain based on your understanding of Observer design patterns, do we still need an interface (or trait in Rust) in this BambangShop case, or a single Model struct is enough?  
   Jawaban : Dalam pola desain Observer, Subscriber (Observer) sering didefinisikan sebagai _interface_ (trait) karena memungkinkan fleksibilitas. Dalam kasus BambangShop hanya memiliki satu jenis observer yaitu Subscriber saja sehingga jika hanya ada satu jenis objek yang perlu diamati maka satu Model struct mungkin cukup kecuali akan ada penambahan observer baru.
2. **id** in **Program** and **url** in **Subscriber** is intended to be unique. Explain based on your understanding, is using **Vec** (list) sufficient or using **DashMap** (map/dictionary) like we currently use is necessary for this case?  
   Jawaban : Jika **id** dan **url** unik, menggunakan DashMap akan lebih efisien daripada menggunakan Vec. Alasannya jika **id** dan **url** unik maka kita dapat melakukan operasi menggunakan hal tersebut pada DashMap secara O(1) sedangkan jika menggunakan Vec kita harus menelurusi seluruh objek terlebih dahulu O(N).
3. When programming using Rust, we are enforced by rigorous compiler constraints to make a thread-safe program. In the case of the List of Subscribers (SUBSCRIBERS) static variable, we used the DashMap external library for thread safe HashMap. Explain based on your understanding of design patterns, do we still need DashMap or we can implement Singleton pattern instead?  
   Jawaban : The Singleton pattern memastikan bahwa sebuah kelas hanya memiliki satu instance dan menyediakan titik akses global ke kelas tersebut. Di sisi lain, DashMap adalah implementasi HashMap yang aman untuk thread. Menerapkan Singleton pattern untuk variabel SUBSCRIBERS tidak akan memberikan keamanan thread. Pola ini hanya akan memastikan bahwa hanya ada satu instance dari daftar SUBSCRIBERS, tetapi tidak memastikan keamanan thread. Jadi, dalam kasus ini, DashMap diperlukan untuk memastikan keamanan thread, dan Singleton pattern tidak dapat menggantikannya.

#### Reflection Publisher-2

1. In the Model-View Controller (MVC) compound pattern, there is no “Service” and “Repository”. Model in MVC covers both data storage and business logic. Explain based on your understanding of design principles, why we need to separate “Service” and “Repository” from a Model?  
   Jawaban : Memisahkan "Service" dan "Repository" dari Model dalam MVC mengikuti _separation of concerns principle_, memungkinkan tanggung jawab yang berbeda. "Repository" mengelola penyimpanan dan pengambilan data, sementara "Service" mengimplementasikan logika bisnis. Hal ini tidak hanya membuat kode lebih mudah dipahami dan dipelihara, tetapi juga dapat memberikan _testing_ yang lebih baik dan memberikan fleksibilitas yang lebih besar dalam _maintain_ komponen-komponen ini tanpa memengaruhi yang lain.
2. What happens if we only use the Model? Explain your imagination on how the interactions between each model (Program, Subscriber, Notification) affect the code complexity for each model?  
   Jawaban : Jika kita memilih untuk tidak memisahkan masalah ke dalam komponen yang berbeda seperti "Service" dan "Repository", kompleksitas setiap model (Program, Subscriber, Notification) dapat meningkat secara signifikan. Hal ini akan membuat model-model tersebut bertanggung jawab tidak hanya untuk representasi data, tetapi juga untuk mencakup logika bisnis dan penyimpanan/pengambilan data. Akibatnya, perubahan dalam satu aspek, seperti modifikasi pada aturan bisnis. Penggabungan tanggung jawab ini akan memperkuat kerumitan model, menjadikannya lebih rumit untuk dipelihara. Selain itu, model-model tersebut kemungkinan besar akan saling terkait erat (_coupling_), sehingga menimbulkan ketergantungan di antara mereka.
3. Have you explored more about Postman? Tell us how this tool helps you to test your current work. You might want to also list which features in Postman you are interested in or feel like it is helpful to help your Group Project or any of your future software engineering projects.  
   Jawaban : Saya sudah pernah menggunakan Postman untuk melakukan mengirim permintaan HTTP ke endpoint-endpoint API dalam proyek yang saya kerjakan. Dengan menggunakan Postman saya bisa mengetahui response yang dikembalikan sesuai dengan request yang kita berikan, header dan body saat mengirim request sehingga memudahkan untuk melakukan testing. Selain itu juga, saya dapat melakukan documentasi terhadap API yang sedang dibuat.

#### Reflection Publisher-3

1. Observer Pattern has two variations: Push model (publisher pushes data to subscribers) and Pull model (subscribers pull data from publisher). In this tutorial case, which variation of Observer Pattern that we use?  
   Jawaban : Dalam tutorial ini menerapkan variasi model Push dari Observer Pattern. Dalam model Push, setiap Observer atau Subscriber mendapatkan notifikasi untuk setiap create, delete dan publish dari Publisher.
2. What are the advantages and disadvantages of using the other variation of Observer Pattern for this tutorial case? (example: if you answer Q1 with Push, then imagine if we used Pull)  
   Jawaban : Jika kita menggunakan model Pull dalam kasus tutorial ini, salah satu keuntungan utama adalah kontrol yang diberikan kepada pelanggan atas proses pengambilan data. Dengan model ini, pelanggan dapat memutuskan kapan dan data apa yang ingin mereka ambil. Namun, kelemahan dari model Pull termasuk peningkatan kompleksitas karena pelanggan perlu mengetahui bagaimana cara mengambil data dari Publisher. Selain itu, jika pengambilan data dilakukan secara sering, hal tersebut bisa menimbulkan masalah kinerja karena setiap pengambilan data merupakan permintaan yang membutuhkan _resource_.
3. Explain what will happen to the program if we decide to not use multi-threading in the notification process.  
   Jawaban : Tanpa multi-threading, notifikasi dikirim ke pelanggan secara berurutan, menyebabkan program terhenti selama setiap notifikasi hingga selesai, yang berpotensi mengakibatkan penundaan jika tugas notifikasi memerlukan periode pemrosesan yang lama, seperti permintaan jaringan dengan waktu respons yang lama.
