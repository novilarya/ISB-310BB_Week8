# Submission Praktikum ISB 310 Sistem Informasi Berbasis Web Minggu ke-8

## Identitas Pemilik

- **Nama :** Novila Arya Minar Saputra
- **NRP :** 162023024

---

## Penjelasan Kode

Pada minggu kedelapan ini, praktikum difokuskan pada **Slicing Template Advance & CRUD** serta **Upload Gambar & File pada Laravel**. Berikut adalah penjelasan dari modifikasi dan struktur kode yang telah dikembangkan:

### 1. Slicing Template (Layouts & Partials)
Teknik *slicing template* digunakan untuk membagi layout website menjadi komponen modular. File `resources/views/layouts/main.blade.php` dibuat sebagai kerangka utama (*Master Layout*) yang memuat elemen statis HTML dasar, pemanggilan CSS/JS (Bootstrap), serta perintah `@yield('content')` sebagai ruang dinamis. 
Bagian navigasi dipisahkan secara khusus ke dalam `resources/views/partials/navbar.blade.php`. Dengan demikian, file halaman anak seperti `index.blade.php` dan `product.blade.php` tidak perlu menulis ulang kode HTML dari awal. File tersebut cukup mewarisi layout utama dengan `@extends('layouts.main')` dan mengisi bagian konten menggunakan `@section('content')`.

### 2. Modularisasi Komponen Modal
Untuk mencegah penumpukan baris kode yang panjang pada satu file, seluruh elemen *Modal* dipisahkan ke dalam direktori `resources/views/modal/`. Terdapat `createProduct.blade.php` untuk formulir tambah data, `updateProduct.blade.php` untuk formulir edit data, dan `wishlist.blade.php` untuk daftar keinginan. 
Modal-modal ini kemudian dipanggil ke dalam halaman utama menggunakan `@include('modal.nama_file')`. Khusus untuk modal *update*, pemanggilan diletakkan di dalam *looping* `@foreach` dengan membawa parameter spesifik (`['item' => $item]`) agar form modal dapat mengenali dan menampilkan data produk yang sedang dipilih.

### 3. Fitur Upload Gambar (ProductController & Storage)
Pada sisi Controller, fungsi `store` dan `update` menggunakan metode `$request->file('product_image')->store('products', 'public')` untuk menyimpan file secara aman ke dalam folder `storage/app/public/products` sekaligus menyimpan *path* namanya ke database. Agar gambar fisik ini dapat diakses oleh browser, perintah `php artisan storage:link` dieksekusi di terminal untuk membuat *symlink* (jalan pintas) ke folder public. Di dalam file Blade, gambar dipanggil menggunakan fungsi helper `{{ asset('storage/' . $item->product_image) }}`.

### 4. Pengelolaan CRUD (Update & Delete)
Sistem CRUD disempurnakan dengan penambahan *routing* spesifik untuk mengubah dan menghapus data. Rute menggunakan metode HTTP `PUT` untuk proses *update* dan `DELETE` untuk proses *destroy*. 
Pada form HTML di dalam file Blade, teknik *method spoofing* (menggunakan `@method('PUT')` dan `@method('DELETE')`) diterapkan karena form HTML standar pada browser pada dasarnya hanya mendukung GET dan POST. Selain itu, Controller juga telah diprogram dengan logika `Storage::disk('public')->delete()` agar secara otomatis menghapus file gambar fisik dari direktori ketika sebuah produk dihapus atau ketika gambar lamanya diganti, sehingga mencegah penumpukan file sampah pada sistem *storage*.
