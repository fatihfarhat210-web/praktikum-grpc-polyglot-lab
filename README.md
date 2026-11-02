# Bagian 7: Tugas Evaluasi Mandiri

## A. Modifikasi Kontrak Data

Pada tugas evaluasi mandiri ini, dilakukan modifikasi pada file `user.proto` dengan menambahkan field baru yaitu `phone` bertipe data `string` dengan nomor field `3` pada struktur `UserResponse`.

Kode `UserResponse` setelah dimodifikasi:

```protobuf
message UserResponse {
  string name = 1;
  string email = 2;
  string phone = 3;
}
```

Penambahan field `phone` bertujuan agar server dapat mengirimkan informasi nomor telepon pengguna selain nama dan email.

## B. Generate Ulang Kode Protobuf

Setelah file `user.proto` dimodifikasi, dilakukan generate ulang kode protobuf untuk Go dan Python.

### 1. Generate Go

Masuk ke folder `go-server`, kemudian jalankan perintah:

```bash
protoc --proto_path=../ --go_out=. --go-grpc_out=. ../user.proto
```

Perintah tersebut digunakan untuk memperbarui file kode protobuf Go.

### 2. Generate Python

Masuk ke folder `python-client`, kemudian jalankan perintah:

```bash
python -m grpc_tools.protoc --proto_path=../ --python_out=. --grpc_python_out=. ../user.proto
```

Perintah tersebut digunakan untuk memperbarui file `user_pb2.py` dan `user_pb2_grpc.py`.

### 3. Node.js

Node.js menggunakan library `@grpc/proto-loader` untuk membaca file `user.proto` secara langsung ketika program dijalankan. Oleh karena itu, tidak diperlukan proses generate file kode protobuf secara terpisah.

## C. Penyesuaian Implementasi Program

Setelah proses generate selesai, dilakukan penyesuaian pada Go Server, Python Client, dan Node.js Client.

1. **Go Server:** Menambahkan data nomor telepon `081234567890` pada respons pengguna.
2. **Python Client:** Menambahkan `response.phone` agar nomor telepon dapat ditampilkan di terminal.
3. **Node.js Client:** Menambahkan `response.phone` agar informasi nomor telepon juga ditampilkan di terminal.

## D. Langkah-Langkah Pengujian

Pengujian dilakukan untuk memastikan Go Server dapat mengirimkan data nama, email, dan nomor telepon kepada Python Client dan Node.js Client melalui komunikasi gRPC.

### 1. Menjalankan Go Server

Buka terminal pertama di Visual Studio Code, kemudian masuk ke folder `go-server`:

```bash
cd go-server
```

Jalankan server dengan perintah:

```bash
go run .
```

Jika berhasil, terminal akan menampilkan:

```text
gRPC Server (Go) berjalan di port :50051...
```
![alt text](<img/Cuplikan layar 2026-11-02 132441.png>)
**Gambar 1. Menjalankan Go Server**

### 2. Menguji Python Client

Buka terminal kedua di Visual Studio Code. Masuk ke folder `python-client`:

```bash
cd python-client
```

Jalankan Python Client:

```bash
python client.py
```

Jika berhasil, terminal akan menampilkan data dari Go Server:

```text
Mengirim request ke Go Server menggunakan Python...
Respon dari Server -> Nama: Budi Santoso, Email: budi@universitas.ac.id, Telepon: 081234567890
```
![alt text](<img/Cuplikan layar 2026-11-02 132500.png>)
**Gambar 2. Pengujian Python Client**

### 3. Menguji Node.js Client

Buka terminal ketiga di Visual Studio Code. Masuk ke folder `node-client`:

```bash
cd node-client
```

Jalankan Node.js Client:

```bash
node client.js
```

Jika berhasil, terminal akan menampilkan:

```text
Mengirim request ke Go Server menggunakan Node.js...
Respon dari Server -> Nama: Budi Santoso, Email: budi@universitas.ac.id, Telepon: 081234567890
```
![alt text](<img/Cuplikan layar 2026-11-02 132514.png>)
**Gambar 3. Pengujian Node.js Client**

## E. Analisis Pendekatan Kompilasi

Pada praktikum ini terdapat dua pendekatan dalam pemrosesan skema `.proto`, yaitu static code generation dan dynamic runtime generation. Go dan Python menggunakan pendekatan static code generation, yaitu file `user.proto` harus dikompilasi terlebih dahulu untuk menghasilkan file kode tambahan sebelum program dijalankan. Pada Go, hasil generate berupa file `user.pb.go` dan `user_grpc.pb.go`, sedangkan pada Python berupa file `user_pb2.py` dan `user_pb2_grpc.py`. Apabila terdapat perubahan pada skema, proses generate harus dilakukan kembali agar kode sesuai dengan struktur terbaru. Berbeda dengan Go dan Python, Node.js menggunakan pendekatan dynamic runtime generation dengan bantuan library `@grpc/proto-loader`, sehingga file `user.proto` dibaca secara langsung ketika program dijalankan tanpa perlu membuat file kode protobuf secara terpisah.

## F. Hasil dan Pembahasan

Berdasarkan pengujian yang dilakukan, Go Server berhasil berjalan pada port `50051`. Python Client dan Node.js Client berhasil mengirimkan request kepada server dan menerima respons berupa nama, email, serta nomor telepon pengguna. Penambahan field `phone` pada file `user.proto` dapat diterapkan pada ketiga bahasa pemrograman sehingga seluruh program dapat menggunakan struktur data yang sama.

## G. Kesimpulan

Dari tugas evaluasi mandiri ini, saya memahami cara menambahkan field baru pada skema protobuf dan melakukan generate ulang kode untuk Go dan Python. Saya juga mempelajari perbedaan static code generation dengan dynamic runtime generation pada Node.js. Selain itu, saya berhasil memahami proses komunikasi antara Go Server, Python Client, dan Node.js Client menggunakan gRPC dengan kontrak data yang sama.

## H. Git Commit dan Push

Setelah seluruh perubahan dan pengujian selesai, perubahan disimpan ke dalam Git dengan menjalankan perintah berikut dari folder utama proyek:

```bash
git add .
git commit -m "Tugas Evaluasi Mandiri Bagian 7"
git push
```

Perintah tersebut digunakan untuk menyimpan perubahan ke dalam commit baru dan mengirimkannya ke repositori GitHub.