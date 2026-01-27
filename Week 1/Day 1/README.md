# Day 1 - Introduction to DevOps

## 1\. Apa itu DevOps?

Secara gamblang, DevOps (gabungan antara kata Development dan Operations) adalah penghubung antara dua tim yang berbeda, yaitu tim Development dan tim Operational. Keduanya dihubungkan agar alur kerja mereka tidak bertabrakan satu sama lain, sehingga mempercepat proses pembuatan produk dari awal hingga rilis.

Siklus dari DevOps sendiri dapat divisualisasikan dengan proses lingkaran tak terbatas (infinity loop) yang terdiri dari beberapa tahapan:

1.  Plan - Tahap awal perencanaan dalam membuat program aplikasi yang diinginkan.
2.  Code - Setelah perencanaan selesai, tim development akan mulai bekerja untuk membuat kode program aplikasi tersebut.
3.  Build - Di tahap ini, tim development mempersiapkan kode-kode yang sudah dibuat sebelumnya lalu menggabung semua kode tersebut menjadi satu aplikasi yang dapat diuji di langkah selanjutnya.
4.  Test - Tahap pengetesan aplikasi yang sudah dibuat oleh tim development. Program aplikasi diuji secara internal untuk mengetahui apakah ada masalah fungsionalitas atau tidak. Jika terdapat masalah, maka akan dilaporkan ke tim development untuk diperbaiki.
5.  Release - Setelah melewati tahap-tahap sebelumnya, dinyatakan lolos tes, dan sudah sesuai dengan apa yang direncanakan di tahap awal, maka tim development akan membuat sebuah versi rilis dari produk tersebut.
6.  Deploy - Program aplikasi yang diberikan oleh tim development selanjutnya akan dijalankan oleh tim operation ke server untuk disiapkan ke tahap operasi.   
7.  Operate - Di tahap ini, program aplikasi dirilis ke publik agar bisa digunakan oleh para pengguna (disebut juga "end user"). Tim harus bertindak cepat jika terdapat masalah saat aplikasi digunakan.  
8.  Monitor - Tim operation akan terus memantau kinerja aplikasi untuk memastikan performa dan "kesehatan" aplikasi tersebut saat dipakai oleh publik. Jika terdapat masalah, tim operation bisa memberitahu ke tim development untuk memperbaiki kesalahan, kembali ke tahap build dan test.
