## Reflection
### 1. What are the key differences between unary, server streaming, and bi-directional streaming RPC (Remote Procedure Call) methods, and in what scenarios would each be most suitable?
Untuk unary, server dan client sama sama mengirim single request (client) and response (server). Untuk server streaming, client mengirim single request sedangkan server mengirim stream of responses. Untuk bi-directional streaming, client dan server sama sama mengirim stream untuk request ataupun response.  
Unary biasa dipakai untuk data yang tidak terlalu besar dan cukup satu kali pengiriman (seperti autentikasi), server streaming dipakai untuk mengirim data yang besar agar tidak sekaligus mengirim semuanya (seperti transaction history), dan bi directional dipakai ketika client dan server sama2 ingin mengirim data banyak secara bersamaan, seperti kasus ChatService.

### 2. What are the potential security considerations involved in implementing a gRPC service in Rust, particularly regarding authentication, authorization, and data encryption?
Validasi input, rate limiting, serta transport security.

### 3. What are the potential challenges or issues that may arise when handling bidirectional streaming in Rust gRPC, especially in scenarios like chat applications?
Mungkin karna dia sifatnya asinkronus, jadi untuk manajemen siapa saja yang sedang memakai thread, siapa saja yang ingin membaca data, harus lebih diperhatikan lagi agar tidak terjadi deadlock dan semacamnya. 

### 4. What are the advantages and disadvantages of using the `tokio_stream::wrappers::ReceiverStream` for streaming responses in Rust gRPC services?
Keuntungannya dia sangat praktis untuk mengubah receiver dari mpsc menjadi sebuah Stream, tapi justru karna itu juga dia mempunyai kekurangan yaitu tidak bisa fleksibel menggonta ganti logika streamnya karena dia bergantung pada mpsc.

### 5. In what ways could the Rust gRPC code be structured to facilitate code reuse and modularity, promoting maintainability and extensibility over time?
Bisa dalam bentuk pemisahan modul modul, dan logika. Seperti pemisahan kode gRPC, kode logika, data, dsb. agar programmer yang melihatnya tidak pusing mencari cari kode.

### 6. In the MyPaymentService implementation, what additional steps might be necessary to handle more complex payment processing logic?
Bisa dengan validasi amount, kemudian tambahkan implementasi interaksi dengan database untuk mengupdate hasil bayar. Bisa juga nambahin gateway ke pihak luar seperti gopay dll.

### 7. What impact does the adoption of gRPC as a communication protocol have on the overall architecture and design of distributed systems, particularly in terms of interoperability with other technologies and platforms?
gRPC sangat berdampak baik dikarenakan dia mengirim data dalam bentuk protobuffer, yang jelas jauh lebih kecil dibanding dengan REST yang mengirim dengan JSON. Kemudian, dia menggunakan HTTP/2 dibanding REST/yg lain yang masih menggunakan HTTP/1.1. Overall, gRPC jauh lebih efisien dibandingkan metode2 lawas.

### 8. What are the advantages and disadvantages of using HTTP/2, the underlying protocol for gRPC, compared to HTTP/1.1 or HTTP/1.1 with WebSocket for REST APIs?
Untuk HTTP/2, dia itu multiplexing yaitu bisa ngirim banyak request/response secara sekaligus. Dia juga bisa kompresi header agar pengiriman data lebih efisien.

### 9. How does the request-response model of REST APIs contrast with the bidirectional streaming capabilities of gRPC in terms of real-time communication and responsiveness?
Pada REST API, dia tidak akan memproses request selanjutnya apabila request yang sekarang belum selesai, makanya untuk real-time communication terasa jauh lebih lambat. Selain itu, dia bersifat satu arah yang berarti client harus mengirim request agar mendapat response dari server, sementara kalau di gRPC, client dan server bisa mengirim data tanpa harus ada request dulu/tanpa menerima dulu.

### 10. What are the implications of the schema-based approach of gRPC, using Protocol Buffers, compared to the more flexible, schema-less nature of JSON in REST API payloads?
Karna dia menggunakan semacam template, maka dia jauh lebih strict dalam menerima data, sehingga seharusnya error dalam transfer data mengurang.