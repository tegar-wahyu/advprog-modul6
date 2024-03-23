# Reflection

## Commit 1 Reflection notes
>Pada milestone pertama ini, Saya membuat Single-Threaded Web Server. 
Berikut adalah cara kerja dan kegunaan kode yang telah saya tulis menurut pemahaman saya.

Modul `std::io::prelude` dan `std::io::BufReader` digunakan untuk membaca dan menulis ke 
stream. Method `handle_connection` dibuat untuk menangani koneksi masuk, di mana ia membuat
instance `BufReader` yang membungkus stream tersebut. `BufReader` mengimplementasikan trait
`BufRead` yang menyediakan metode `lines` untuk mendapatkan setiap baris request HTTP 
dari browser. Setelah itu, baris-baris request tersebut dicetak dalam format yang 
dapat dibaca. Ketika program dijalankan dan request dikirim melalui browser, output akan
menampilkan baris-baris request HTTP yang telah dikirim oleh browser.

## Commit 2 Reflection notes
>Pada milestone kedua ini, Saya menggunakan modul filesystem Rust untuk membaca isi file dan mengirimkannya sebagai
respons server HTTP.

Proses ini diawali dengan mengimpor modul `fs` untuk mengakses modul `filesystem`. Kemudian, isi file 
`hello.html` dibaca dan di-convert ke dalam sebuah string menggunakan fungsi `fs::read_to_string("hello.html").unwrap();`. 
Selanjutnya, isi file tersebut diformat menjadi respons HTTP yang valid dengan menambahkan 
header `Content-Length` yang berisi panjang konten. Terakhir, respons dikirim ke klien melalui objek stream menggunakan metode `write_all`.

![commit2.png](assets/images/commit2.png)