# Advanced Programming - Module 9

## Reflection

1. What are the key differences between unary, server streaming, and bi-directional streaming RPC (Remote Procedure Call) methods, and in what scenarios would each be most suitable?

- Unary RPC: client mengirim satu request ke server dan menunggu satu response. Cocok digunakan ketika mhanya perlu mengirim satu permintaan dan memerlukan hanya satu response juga. Beberapa kasus di mana Unary RPC paling cocok digunakan adalah ketika mengirim notifikasi email dan mengakses detail informasi dari suatu produk dari aplikasi e-commerce.
- Server Streaming RPC: client mengirim saru permintaan ke server dan menerima beberapa response. Cocok digunakan ketika mhanya perlu mengirim satu permintaan, tetapi memerlukan banyak response. Beberapa kasus di mana Server Streaming RPC cocok digunakan adalah ketika mengupdate data secara berkala dan menonton playlist yang terdiri dari beberapa video.
- Bi-directional Streaming RPC: client mengirimkan beberapa request dan menerima beberapa response. Cocok digunakan jika diperlukan interaksi yang saling bergantian antara client dengan server. Contoh kasus di mana cocok menggunakan Bi-directional Streaming RPC adalah aplikasi yang Berjalan secara real-time antara client dengan server (keduanya saling bertukar pesan secara bergantian. Terjadi secara simultaneous dan continuous).

2. What are the potential security considerations involved in implementing a gRPC service in Rust, particularly regarding authentication, authorization, and data encryption?

- Authentication: ada unauthenticated user yang terhubung dengan server grpc -> perlu memastikan bahwa ada autentikasi untuk menjadi guard dan memastikan bahwa yang bisa terhubung ke server grpc hanya authorized user. Selain itu, ada serangan terhadap data -> implement proteksi untuk memastikan tidak ada yang bisa mencuri data dan lain sebagainya. Tambahan: dapat menggunakan memanfaatkan mekanisme autentikasi yang kredibel, seperti JWT.

- Authorization: ada unauthorized user yang mengakses sesuatu yang hanya bisa diakses oleh user dengan role tertentu -> perlu menggunakan role-based access control yang dapapt menentukan hal-hal apa saja yang dapat dilakukan oleh sekelompok user tertentu dan menerapkan pengecekan apakah role user yang ingin menakses memiliki hal terhadap hal tersebut.

- Data encryption: ada pengiriman dari client ke server atau sebaliknya yang tidak dienkripsi terlebih dahulu, sehingga menjadi rentan terhadap pencurian data -> gunakan TLS atau SSL untuk memastikan transit data tersebut melalui proses enkripsi terlebih dahulu. Pencurian data sensitif -> memastikan semua data yang bersifat sensitif telah dienkripsi terlebih dahulu.

3. What are the potential challenges or issues that may arise when handling bidirectional streaming in Rust gRPC, especially in scenarios like chat applications?

Pada proses implementasi streaming bi-directional dalam layanan grpc di Rust, potential challenges or issues nya meliputi manajemen lifetime dari stream, penanganan error dan exception, serta memastikan keamanan dari thread. Dalam skenario aplikasi chatting, kompleksitas tambahan juga dapat muncul dari manajemen beberapa stream yang bersamaan (concurrent stream), memastikan pesan tetap terurut, dan handling pemutusan koneksi atau timeout. Oleh karena itu, perlu memerhatikan beberapa potential challenges atau issues tersebut agar tetap berjalan optimal.

4. What are the advantages and disadvantages of using the tokio_stream::wrappers::ReceiverStream for streaming responses in Rust gRPC services?

Adavantagesnya adalah tentunya karena compatible dengan Tokio, sehingga integrasi antar ReceiverStream degnan Tokio dapat berjalan dengan amksimal dan lancar. Selain itu, ReceiverStream juga memungkinkan sinkronisasi untuk dapat consume data yang diterma asynchronously. Selain itu, dapat mengurangi code complexity dan mempercepat proses develop aplikasi juga, sehingga maintainability aplikasi juga tetap terjaga. Di sisi lain, disadvantagesnya adalah ReceiverStream tidak terintegrasi secara langsung dengan grpc karena sebagaimana telah dijelaskan sebleumnya, dapat dengan mudah terintegrasinya degan Tokio, sehingga cukup suli tuntuk mengintegrasikannya dengan grpc. Selain itu, ada beberapa keterbatasan lain juga mengingat tidak terintegrasi secara langsung dengan grpc (dari sisi fungsionalitas).

5. In what ways could the Rust gRPC code be structured to facilitate code reuse and modularity, promoting maintainability and extensibility over time?

Dapat menerapkan separation of concern pada business logic agar extensibility aplikasi tetap terjaga (mudah ketika perlu melakukan modifikasi pada bagian tertentu), sehingga lbeih maintainble hingga jangka panjang. Selain itu, dapat juga membuat shared modul/library untuk beberapa kode yang cukup mirip (seperti pada modul refactoring), dapat memanfaatkan gengerics agar dapat menerima berbagai tipe data. Selain itu, dapat juga memanfaatkan beberapa jenis design pattern, seperti misal menggunakan design pattern builder untuk build objek yang tergolong kompleks untuk memaksimalkan reusability dan modularity.

6. In the MyPaymentService implementation, what additional steps might be necessary to handle more complex payment processing logic?

Melakukan validasi data untuk semua fields yang diperlukan dan menentukan exception handlingnya untuk memastikan aplikasi tetap robust (dapat terhindar dari berbagai serangan). Beberapa contoh serangan yang dimaksud adalah seperti SQL injection, cross site scripting (XSS), dan lain sebagainya. Selain itu, perlu juga menambahkan program yang dapat mengirimkan notifikasi either payment berhasil atau gagal untuk memastikan aware apa yang telah terjadi (mendapatkan feedback terkait payment apakah berhasil dilakukan karena tentunya berhasil/gagalnya payment menentukan hal-hal yang akan terjadi selanjutnya). 

7. What impact does the adoption of gRPC as a communication protocol have on the overall architecture and design of distributed systems, particularly in terms of interoperability with other technologies and platforms?

grpc menggunakan HTTP/2 untuk transport, sehingga dapat memberikan komunikasi yang efisien dan dapat memanfaatkan multiplexing antara client dengan server. Hal ini dapat memaksimalkan kinerja, latency yang lebih rendah, dan penggunaan sumber daya yang lebih baik (jika dibandingkan dengan HTTP/1 tradisional atau api restful). Selain itu, grpc depends pada protobuf untuk mendifinisikan service dan serialization pada messages. Dan juga, protobuf memfasilitasi definisi api yang independent terhadap bahasa pemrograman tertentu. Maka dari itu, promotes interoperability pada berbagai bahasa pemrograman, platform, dan teknologi.

8. What are the advantages and disadvantages of using HTTP/2, the underlying protocol for gRPC, compared to HTTP/1.1 or HTTP/1.1 with WebSocket for REST APIs?

Advantagesnya adalah sebagaimana telah disinggung sedikit pada poin 7, HTTP/2 supports multiplexing (memungkinkan pengiriman beberapa request dan response dalam satu koneksi TCP), memungkinkan server push (server dapat mengirimkan data walaupun tidak diminta oleh client jika ada pertimbangan bahwa sepertinya client akan memerlukan data tersebut). Maka dari itu, beberapa keuntungannya adalah dapat memaksimalkan penggunaan bandwith, mengurangi latency time untuk memproses request, memaksimalkan efisiensi dari proses transfer data.

Disadvantagesnya belum semua server dan client supports protokol HTTP/2, sehingga dapat menimbulkan issue dari sisi compatibility yang berdampak pada fungsionalitas. Selain itu, tentunya kompleksitas implementaisnya juga bertambah akrena ada beberapa konsep yang sebelumnya tidak ada, seperti misal multiplexing dan server piush yang telah dijelaskan sebelumnya, sehingga memakan waktu lebih lama saat pengembangan aplikasinya.

9. How does the request-response model of REST APIs contrast with the bidirectional streaming capabilities of gRPC in terms of real-time communication and responsiveness?

Model request-response dari REST API cocok digunakan untuk interaksi client-server tradisional, tetapi bisa menyebabkan tidak optimal apabila digunakan utnuk komunikasi yang bersifat real-time (karena lebih cocok untuk hanya  permintaan-respons). Di sisi lain, kemampuan bidirectional streaming grpc bekerja dengan optimal untuk skenario komunikasi real-time karena dapat memberikan pertukaran data instan, latency time yang tergolong rendah, dan responsivitas yang berkelanjutan antara client dengan server (sehingga jadi cocok untuk real-time interaction).

Di sisi lain, REST API menggunakan model request-response yang sederhana, tetapi bisa menjadi kurang optimal untuk interaksi real-time yang berkelanjutan karena terdapat overhead pada HTTP request. Kemudian, grpc mendukung streaming bidirectional yang memungkinkan interaksi secara real-time dan full-duplex, tetapi menjadi lebih kompleks untuk diimplementasikan dan tentunya maintainablity aplikasi juga jadi menurun.

Maka dari itu, pemilihan antara REST API dan grpc perlu mempertimbangkan beberapa faktor yang telah disebutkan di atas, seperti latency time, kompleksitas, dan lain sebagainya. Kalau lebih memprioritaskan interaksi real-time, maka lebih cocok menggunakan grpc dengan streaming bidirectional, tetapi jika cukup sederhana dan memetingkan uniform interface, maka lebih cocok untuk menggunakan REST API.

10. What are the implications of the schema-based approach of gRPC, using Protocol Buffers, compared to the more flexible, schema-less nature of JSON in REST API payloads?

Implikasinya adalah, jika menggunakan grpc dengan protobuf, dapat mendefiniskan skema dan tipe data yang ketat, sehingga dapat meningkatkan keamanan data dan lebih sedikit ada risiko exception mengingat definisinya sudah strict. Selain itu, dapat mengurangi payload dan engoptimalkan runtime saat trasnfer data karena format binernya tidak sekompleks JSON. Di sisi lain, JSON memang lebih fleksibel dari segi struktur data, tetapi kekurangannya seperti tadi, menjadi tidak seaman grpc dengna protobuf, tetapi hal ini menjadi cocok jika tipe datanya bersifat dinamis. Selain itu, untuk redability juga mungkin lebih mudah jika menggunakan JSON karena memang lebih simple dari segi mendefinisikan skema, tipe data mengingat sifatnya yang dinamis. Tambahan juga, karena definisi skemanya sudah jelas, maka interoperabilitas aplikasii juga meningkat untuk berbagai bahasa pemrograman dan platform. Tentunya dalam pemilihannya perlu mempertimbangkan hal-hal yang telah saya sampaikan tadi sesuai dengan hal-hal apa yang menjadi prioritas untuk aplikasinya.