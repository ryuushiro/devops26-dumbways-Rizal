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


# 2\. Pemasangan Ubuntu Server 24.04.3

Sebelum melanjutkan, saya ingin menjelaskan alasan mengapa saya menggunakan Ubuntu Server versi terbaru yang ada di laman web [https://ubuntu.com/download/server](https://ubuntu.com/download/server) dan bukan yang sama dengan yang ada di tutorial (Ubuntu Server 22.04.x).

Dalam beberapa kali percobaan dalam menginstal Ubuntu versi tersebut, saya selalu mendapatkan masalah di tahap akhir penginstalan seperti yang ada pada gambar di bawah ini.

<img width="837" height="629" alt="image" src="https://github.com/user-attachments/assets/d268bd0c-ce74-413e-b3cf-44dab89e97ad" />

Setelah saya mencari berbagai cara di internet, saya memutuskan untuk mengganti ke versi Ubuntu Server yang terbaru (24.04.3).

## Langkah-Langkah Pemasangan 
_Virtual Machine_ yang saya gunakan adalah **_VMWare Workstation Pro 17_**. 


**1.** Buka aplikasi _VMWare Workstation Pro 17_ dan klik tombol **_Create a New Virtual Machine_**.
<img width="533" height="256" alt="image" src="https://github.com/user-attachments/assets/6ac67e68-5233-4a39-bd32-fc7d5dbdf200" />


**2.** Setelah ada jendela baru yang terbuka, pilih tipe penginstalan yang diinginkan. Agar cepat, saya memilih **_Typical (recommended)_** lalu klik tombol _Next >_.

<img width="438" height="441" alt="image" src="https://github.com/user-attachments/assets/cbb4aff7-46a2-428b-a030-8c24706dd358" />


**3.** Pilih **_Installer disc image file (iso)_** dan cari file.ISO Ubuntu Server yang sudah diunduh. Lalu klik tombol _Next >_.
<img width="421" height="423" alt="image" src="https://github.com/user-attachments/assets/1dcc5e7a-6dfb-44e5-81af-98ff7b73245c" />


**4.** Masukkan nama _Virtual Machine_ sesuai dengan yang diinginkan, lalu pilih di mana VM tersebut diinstal. Lalu klik tombol _Next >_
<img width="422" height="426" alt="image" src="https://github.com/user-attachments/assets/72f73c26-ddec-4523-9809-53e999f8de90" />

**5.** Di jendela ini kita bisa mengalokasikan berapa banyak penyimpanan yang dipakai oleh VM. Saya mengisi 10 GB, lalu klik tombol _Next >_
<img width="425" height="418" alt="image" src="https://github.com/user-attachments/assets/cdcffb94-c811-41f6-9ada-463cc2f88317" />


**6**. Di jendela ini, terdapat informasi spesifikasi yang sudah kita pilih sebelumnya. Saya klik _Costumize Hardware..._
<img width="419" height="419" alt="image" src="https://github.com/user-attachments/assets/617e78fb-c22a-4ac2-8a93-6f24d27f8969" />


**7.** Saya mengubah pengaturan _Network Adapter_ menjadi **_Bridged: Connected directly to the physical network_** dan mencentang kotak di bawahnya. Saya melakukan hal tersebut agar VM dapat terkoneksi ke internet yang sama dengan komputer saya. (Saya dapat informasi ini setelah beberapa kali gagal saat koneksi dalam VM.)
Untuk _Memory_ saya biarkan memakai 2GB untuk menghindari masalah yang sudah beberapa kali terjadi. Setelah semuanya selesai, saya tutup jendelanya dan kembali.

<img width="746" height="710" alt="image" src="https://github.com/user-attachments/assets/6b3fb993-b46f-45a8-8d5e-a055a5b0604d" />


**8.** Setelah kembali ke jendela ini, saya klik _Finish_. VM akan otomatis menyala.

<img width="419" height="426" alt="image" src="https://github.com/user-attachments/assets/5a75d099-a3d0-4db1-84e7-8621076c6345" />

**9.** Saat VM menyala dan jendela baru terbuka, waktu untuk menginstall Ubuntu di VM dimulai. Pilih bahasa dan konfigurasi kibor yang sesuai dengan yang digunakan.

<img width="1299" height="818" alt="image" src="https://github.com/user-attachments/assets/b7c760da-881d-4900-9954-a838be8ba8b8" />
<img width="1293" height="807" alt="image" src="https://github.com/user-attachments/assets/0661c504-62f0-49ca-8416-219a86f112db" />


**10.** Pilih _Ubuntu Server_.
<img width="1295" height="808" alt="image" src="https://github.com/user-attachments/assets/d9c15c86-eab7-47aa-b67d-543f94e76245" />


**11.** Di laman **_Network Configuration_** saya memilih untuk mengedit tipe koneksi seperti di bawah ini. 
<img width="1292" height="806" alt="image" src="https://github.com/user-attachments/assets/061f679d-1f25-4acc-ab62-31722d4b69ac" />
<img width="494" height="163" alt="image" src="https://github.com/user-attachments/assets/bd46373b-8f77-4072-a176-5300ada1e4ac" />
<img width="620" height="416" alt="image" src="https://github.com/user-attachments/assets/e04dad4e-2bb2-4a36-847e-be4f4bd85597" />

Saat saya sudah selesai mengedit, saya lanjut ke tahap berikutnya.
<img width="992" height="159" alt="image" src="https://github.com/user-attachments/assets/b2016427-c73d-4023-a999-09c54d78a1f7" />


**12.** Laman **_Proxy Adress_** dan **_Mirror Address_** saya lewatkan saja.
<img width="1041" height="168" alt="image" src="https://github.com/user-attachments/assets/450c430e-8f62-4bc3-ae88-4ec5c7ad9f8f" />
<img width="1039" height="142" alt="image" src="https://github.com/user-attachments/assets/fcf52970-e864-4a31-9b32-072aa0e34cc9" />

**13.** Pada laman **_Guided storage configuration_**, pilih _Custom storage layout_ dan atur partisi sesuai dengan yang diinginkan.
<img width="1293" height="811" alt="image" src="https://github.com/user-attachments/assets/b14e54a7-f8aa-46f7-be5c-4e650e3011ea" />
<img width="535" height="392" alt="image" src="https://github.com/user-attachments/assets/786b996c-d4b0-4393-a2dd-5843f4dc6654" />
<img width="604" height="253" alt="image" src="https://github.com/user-attachments/assets/30d1f152-b24f-42ca-98c5-bffd44bd6b49" />
<img width="612" height="262" alt="image" src="https://github.com/user-attachments/assets/47b192e7-0a80-4e79-bb67-7e9fb6d820d2" />

Setelah mengatur, saya lanjut ke tahap berikutnya.

<img width="613" height="449" alt="image" src="https://github.com/user-attachments/assets/ae8c7fcc-dbc5-4bb2-87a6-3d38d7ebc8ea" />


**14.** Konfirmasi untuk membuat partisi, klik _Continue_.

<img width="607" height="231" alt="image" src="https://github.com/user-attachments/assets/bd68425e-ed78-4dba-863b-87ddc59e626c" />


**15.** Lengkapi data di laman **_Profile configuration_**
<img width="1037" height="320" alt="image" src="https://github.com/user-attachments/assets/e3a04df7-0b83-45af-958a-f8a7d8dcac15" />


**16.** Laman **_Upgrade to Ubuntu Pro_**, **_SSH configuration_**, dan **_Featured server snaps_** saya lewatkan saja.
<img width="1036" height="208" alt="image" src="https://github.com/user-attachments/assets/32090e4d-05ed-4ee4-950e-3fb7f20ec811" />
<img width="819" height="256" alt="image" src="https://github.com/user-attachments/assets/ad0186c8-a4a9-418a-8b24-e817e3bbc9c8" />
<img width="1032" height="382" alt="image" src="https://github.com/user-attachments/assets/f4cae3fa-6c80-4484-beaa-b7ff478af8d0" />



**17.** Setelah semua yang di atas sudah dilakukan, tinggal menunggu Ubuntu Server terinstal.
<img width="1298" height="812" alt="image" src="https://github.com/user-attachments/assets/028b56c8-09ff-41a0-811c-37b0bf2d25d7" />


**18.** Setelah menunggu cukup lama, akhirnya instalasi selesai. Pilih _Reboot Now_ untuk menyelesaikan instalasi.
<img width="1299" height="811" alt="image" src="https://github.com/user-attachments/assets/cf5477a8-023d-4116-ae95-9d0527ef4f42" />


**19.** Proses _Reboot_ dimulai.

<img width="710" height="343" alt="image" src="https://github.com/user-attachments/assets/b9c975e9-0fc9-4dac-a78a-87ce7b919238" />

**20.** Setelah proses _Reboot_ selesai. Ubuntu Server siap digunakan. Namun sebelumnya saya harus _login_ terlebih dahulu dengan data yang sebelumnya saya masukkan.
<img width="1326" height="500" alt="image" src="https://github.com/user-attachments/assets/0f65a406-3141-4d05-82e8-ee28ae5454f2" />

**21.** Selesai. Sekarang saya bisa menulis _command_ di OS ini. Untuk tes pertama, saya mencoba apakah VM terkoneksi ke internet atau tidak dengan melakukan _PING_ ke DNS [https://google.com/](https://google.com/)
<img width="658" height="671" alt="image" src="https://github.com/user-attachments/assets/28bb530d-8e27-4002-a4c1-800d0af71869" />


Seperti yang bisa dilihat, Ubuntu Servernya sudah terkoneksi ke internet. Dengan ini, selesai sudah tugas pertama untuk minggu pertama.
<img width="724" height="457" alt="image" src="https://github.com/user-attachments/assets/1142867d-5a0a-4188-8147-b6f1964a3edd" />





























