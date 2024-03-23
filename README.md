# Reflection
<hr>
Tegar Wahyu Khisbullohn - 2206082032 - Advanced Programming A

## Commit 1 - Handle-connection, check response
>Pada milestone pertama ini, Saya membuat Single-Threaded Web Server. 
Berikut adalah cara kerja dan kegunaan kode yang telah saya tulis menurut pemahaman saya.

Modul `std::io::prelude` dan `std::io::BufReader` digunakan untuk membaca dan menulis ke 
stream. Method `handle_connection` dibuat untuk menangani koneksi masuk, di mana ia membuat
instance `BufReader` yang membungkus stream tersebut. `BufReader` mengimplementasikan trait
`BufRead` yang menyediakan metode `lines` untuk mendapatkan setiap baris request HTTP 
dari browser. 

Setelah itu, baris-baris request tersebut dicetak dalam format yang 
dapat dibaca. Ketika program dijalankan dan request dikirim melalui browser, output akan
menampilkan baris-baris request HTTP yang telah dikirim oleh browser.
<hr>

## Commit 2 - Returning HTML
>Pada milestone kedua ini, Saya menggunakan modul filesystem Rust untuk membaca isi file dan mengirimkannya sebagai
respons server HTTP.

Proses ini diawali dengan mengimpor modul `fs` untuk mengakses modul `filesystem`. Kemudian, isi file 
`hello.html` dibaca dan di-convert ke dalam sebuah string menggunakan fungsi `fs::read_to_string("hello.html").unwrap();`. 

Selanjutnya, isi file tersebut diformat menjadi respons HTTP yang valid dengan menambahkan 
header `Content-Length` yang berisi panjang konten. Terakhir, respons dikirim ke klien melalui objek stream menggunakan metode `write_all`.

![commit2.png](assets/images/commit2.png)
<hr>

## Commit 3 - Validating request and selectively responding
>Pada milestone ketiga ini, Saya menambahkan validasi untuk request HTTP dan selectively responding
pada request yang valid.

Kode yang telah ditambahkan menggunakan `next` untuk hanya membaca baris pertama
dari HTTP request dan disimpan dalam variabel `request_line`.
```shell
Request: [
 "GET / HTTP/1.1", <--- baris pertama
 "Host: 127.0.0.1:7878", "Upgrade-Insecure-Requests: 1",
 ...
]
```
Jika `request_line` berisi request `GET /`, maka isi  file `hello.html` 
akan dikirimkan sebagai respons. Namun, jika bukan request `GET /`, kode 
akan dieksekusi pada blok `else` untuk menangani request tidak valid dengan
mengirimkan file `404.html`.

Refactoring dilakukan karena kode yang ditambahkan pada milestone ini
hampir sama dengan kode yang telah ditulis pada milestone kedua.
Perbedaannya hanya pada `status_line` dan `filename`.

![commit3.png](assets/images/commit3.png)
<hr>

## Commit 4 - Simulation slow respons
>Pada milestone keempat ini, Saya melakukan simulasi slow response dengan menambahkan 
delay 10 detik pada server.

Milestone ini mengubah `if-else` menjadi `match` untuk menangani tiga jenis request HTTP: 
`GET /`, `GET /sleep`, dan request lainnya. Untuk request `GET /sleep`, server akan 
tidur (sleep) selama 10 detik sebelum mengirimkan respons. Ketika server dijalankan dan 
request `/sleep` dilakukan, request lain seperti `/` akan tertunda sampai waktu sleep 
(10 detik) selesai sebelum merespons.
<hr>

## Commit 5 - Multithreaded Server
>Pada milestone kelima ini, Saya membuat multi-threaded web server dengan menggunakan thread pool.

Thread pool adalah kumpulan thread yang sudah dibuat dan siap untuk menangani tugas. 
Ketika ada tugas baru, salah satu thread dari pool akan ditugaskan untuk menanganinya. 

Thread lain dalam pool tetap tersedia untuk tugas-tugas selanjutnya. Setelah selesai, 
thread akan kembali ke pool untuk menangani tugas berikutnya. Thread pool memungkinkan 
server memproses banyak koneksi secara bersamaan, meningkatkan throughput. 

Jumlah thread dalam pool dibatasi untuk mencegah serangan Denial of Service (DoS) 
yang dapat menghabiskan semua sumber daya server. Dengan desain ini, server dapat 
memproses hingga N permintaan secara concurrent, dengan N adalah jumlah thread dalam pool.
<hr>

## Bonus -  Function improvement
>Pada bagian ini, Saya melakukan improvement pada method new.

Sebelumnya, kode yang ditulis menggunakan fungsi `new` yang mana akan langsung memanggil `panic!` 
apabila jumlah thread = 0. Pada dasarnya, fungsi `new` diharapkan untuk tidak pernah gagal sehingga
tidak didefinisikan untuk menangani error.
>![newError.png](assets/images/newError.png)

Perubahan fungsi `new` menjadi `build` menandakan bahwa fungsi tersebut dapat gagal dan mengembalikan hasil Error 
yang bisa diatur. Misal disini saya memberikan output `Failed to create thread pool: {err}` apabila
jumlah thread = 0 dan mengakibatkan error pembuatan `ThreadPool`.
>![buildError.png](assets/images/buildError.png)

Ini lebih baik daripada langsung memanggil `panic!` jika terjadi error karena
memberi output yang ramah pengguna dan lebih mudah dipahami.