# 1. Akses server menggunakan terminal (Windows Terminal/PuTTY/etc.)
Pertama, jalankan command “sudo apt install openssh-server” untuk menginstall openssh-server.
<img width="593" height="163" alt="image" src="https://github.com/user-attachments/assets/5c8bdf44-c1f9-4914-a60a-f72d413e0470" /><br><br>
Kedua, pastikan service-nya jalan dan otomatis menyala saat booting dengan menggunakan command “sudo systemctl enable --now ssh”. “systemctl” adalah command yang digunakan untuk mengontrol sistem yang ada di Linux. Jadi di command ini “systemctl enable --now ssh” bisa diartikan sebagai “Control, tolong nyalakan ssh nya setiap setelah reboot”.
<img width="807" height="71" alt="image" src="https://github.com/user-attachments/assets/d989d592-ecbd-43a7-bfde-122cde79ae50" /><br><br>
Lalu kita cek status SSH nya.<br>Bisa dilihat kalau statusnya active (running) berwarna hijau. Tandanya SSH sudah berjalan.
<img width="848" height="331" alt="image" src="https://github.com/user-attachments/assets/afa8f435-b5ff-45f9-8a5a-9b32981a4581" /><br><br>
Setelah SSH sudah dibuat, sekarang waktunya kita membuka Terminal di Windows. Jangan lupa untuk menjalankannya sebagai administrator!<br>
<img width="420" height="137" alt="image" src="https://github.com/user-attachments/assets/3cb560a5-975e-4fec-b259-d92719ce540e" /><br><br>
Lalu untuk mengakses SSH yang ada di VM, kita jalankan command “ssh rzl@ip-vm”. Maksud dari command itu kita memanggil aplikasi SSH dari user ‘rzl’ yang berada di dalam ‘ip-vm’ nya. Di kasus saya sekarang, karena IP yang saya gunakan untuk VM saya adalah “10.42.103.208”, jadi saya gunakan command “ssh rzl@10.42.103.208”. Setelah memasukkan command itu, masukkan password Ubuntu Servernya dan tekan ‘Enter’.<br>
<img width="891" height="675" alt="image" src="https://github.com/user-attachments/assets/c96d50a7-2f0b-4e06-b5c8-f45e0af94f73" />
<br>
<br>
# 2. Konfigurasi ssh kalian agar bisa di akses hanya menggunakan publickey (Password opsional, bisa dimatikan)
Pertama, kita buat kuncinya dulu di windows dengan command “ssh-keygen”. Setelah memasukkan command itu, nanti akan ditanya nama file-nya. Saya menamakannya dengan “kunci”.
<img width="1240" height="573" alt="image" src="https://github.com/user-attachments/assets/4fc21722-6b39-47a5-ba85-d678fb7b49a5" /><br><br>
Setelah kita membuat kunci ssh nya, kita bisa lanjut untuk mengecek kode kuncinya. Seperti yang ada di gambar di atas, “kunci” dan “kunci.pub” tersimpan di /.ssh/ .<br>
<img width="847" height="394" alt="image" src="https://github.com/user-attachments/assets/d4f5578e-3670-49ec-a324-4ce8b16864ec" /><br><br>
Setelah kita sudah mengecek keberadaan file kuncinya yang “kunci.pub”, kita baca file tersebut dan salin isinya.
<img width="1146" height="88" alt="image" src="https://github.com/user-attachments/assets/5662153a-cbdc-43cc-be31-bfa1ff242ed4" /><br><br>
Setelah isi kode panjangnya sudah disalin, buka server dan meluncur ke folder /.ssh/ di server.<br>
<img width="303" height="106" alt="image" src="https://github.com/user-attachments/assets/3e49720e-fbe4-4af9-842b-55d1fa372ab7" /><br><br>
Lalu kita gunakan command “nano authorized_keys” untuk edit file. Kita masukkan kode panjang dari “kunci.pub” tadi ke dalam file “authorized_keys”.
<img width="1438" height="610" alt="image" src="https://github.com/user-attachments/assets/011d3492-519b-4a99-a646-2e6a5c896f91" /><br><br>
Setelah itu, kita save filenya dan coba logout.<br>
Setelah logout, kita coba login ke dalam server menggunakan command “ssh -i .ssh/kunci rzl@10.126.87.208”. Di sini, terminal akan memanggil file “kunci” yang ada di Windows, bukan di dalam Ubuntu Server. Jika kode yang terdapat dalam file “kunci” cocok dipasangkan dengan kode yang kita masukkan kedalam “authorized_keys” tadi, maka login akan sukses.
<img width="1436" height="677" alt="image" src="https://github.com/user-attachments/assets/dd917eaf-3a0c-4b2b-9414-7361cb58da64" />
<br>
<br>
# 3. Buat step by step penggunaan text manipulation (grep, sed, cat, echo)
## 1. echo
Command "echo" memiliki banyak fungsi, mulai dari menampilkan sebuah tulisan di dalam terminal, menulis teks kedalam sebuah file, dan menambahkan teks ke dalam file yang sudah ada.<br><br>
Untuk contohnya, jika saya memberikan command "echo dumbways nih", teks "dumbways nih" akan muncul.<br>
<img width="317" height="40" alt="image" src="https://github.com/user-attachments/assets/75a0c818-e72c-4add-8fb1-bc9141bf7c7d" /><br><br>
Jika saya memberikan command "echo "Ini Baris Pertama" > catatanku.txt", ada dua kemungkinan. Jika 'catatanku.txt' belum ada, maka command ini akan membuat file tersebut. Jika 'catatanku.txt' sudah ada, maka isinya dihapus dulu, baru ditulis "Ini Baris Pertama".<br>
<img width="837" height="139" alt="image" src="https://github.com/user-attachments/assets/af015b04-3ce2-4579-9c74-094be08b28a4" /><br><br>
Jika saya memberikan command "echo "Ini Baris Kedua" >> catatanku.txt", maka teks "Ini Baris Kedua" akan ditambahkan ke file 'catatanku.txt' dan tidak menghapus teks baris pertama. Bedanya dari command sebelumnya adalah jumlah arah panah ">". Kalau cuma satu, dipakai untuk menulis ulang isi. Jika ada dua, dipakai untuk menambahkan isi.<br>
<img width="540" height="137" alt="image" src="https://github.com/user-attachments/assets/83b9030d-e4f2-452c-8d14-edcd540e108b" /><br><br>
## 2. cat
Command "cat" biasanya dipakai untuk melihat isi dari file yang ada di server. Di penjelasan sebelumnya tentang "echo", saya sudah memakai command "cat" untuk melihat isi dari file 'catatanku.txt'<br>
<img width="837" height="139" alt="image" src="https://github.com/user-attachments/assets/af015b04-3ce2-4579-9c74-094be08b28a4" /><br><br>
Namun, fungsi dari "cat" tidak hanya itu saja. "cat" juga bisa dipakai untuk melihat dua file yang berbeda menjadi satu. "cat" juga bisa menggabung kedua file tersebut menjadi file baru menggunakan ">".<br>
<img width="508" height="163" alt="image" src="https://github.com/user-attachments/assets/7eecdfa1-1dbe-4dee-bd20-52eec7c2a7c1" /><br><br>
Selain kedua di atas, "cat" juga bisa dipakai untuk menulis file baru menggunakan ">". Bedanya dengan "echo" adalah, kita bisa membuat teks yang lebih panjang dan juga berbeda baris.<br>
<img width="423" height="155" alt="image" src="https://github.com/user-attachments/assets/728a0cbd-e4c0-41a5-8f11-4d0ee2d5ff18" /><br><br>
## 3. sed
Simpelnya, command "sed" berfungsi untuk mengedit teks yang ada di dalam sebuah file tanpa harus membuka teks editor seperti "nano". Untuk contohnya, saya akan menggunakan file "partfull.txt" yang sebelumnya sudah dibuat. <br>
<img width="479" height="142" alt="image" src="https://github.com/user-attachments/assets/9e7a004c-3347-45de-acc0-a22786c7334a" /><br><br>
Jika kita hanya menggunakan command seperti di atas, perubahannya tidak permanen. Jika ingin permanen, tambahkan "-i" setelah command "sed".<br>
<img width="484" height="161" alt="image" src="https://github.com/user-attachments/assets/a365e14e-624d-43ec-95b4-a3d64b902e25" /><br><br>
Ada command tambahan lagi selain "-i". Jika kita menggunakan command di atas namun kata "kepala" ada lebih dari satu, maka yang terganti/berubah hanya kata yang pertama kali ditemukan saja. kita bisa menambahkan "g" (singkatan dari global) setelah garis miring terakhir untuk mengubah semua teks yang sama dengan apa yang mau diubah.<br>
<img width="534" height="197" alt="image" src="https://github.com/user-attachments/assets/b749771c-1a27-4f17-ad22-9a69b2fa3b91" /><br><br>
Selain mengubah teks, command ini juga bisa dipakai untuk menghapus baris. Contoh, jika kita ingin menghapus baris kedua, kita bisa menggunakan "sed -i '2d' namafile". Kita juga bisa menghapus baris yang mengandung kata tertentu dengan "sed -i '/katanya/d' namafile". <br>
<img width="457" height="310" alt="image" src="https://github.com/user-attachments/assets/ef494e2a-797d-40e6-98cc-394eafede14d" /><br><br>
## 4. grep
Command "grep" memiliki tugas utama yaitu mencari baris teks yang mengandung kata kunci tertentu. Untuk contoh, saya menggunakan file "log_harian.txt" seperti gambar di bawah.<br>
<img width="329" height="143" alt="image" src="https://github.com/user-attachments/assets/c160e9d1-e6a7-4109-b27a-a8d983a04817" /><br><br>
Jika saya menjalankan command "grep "Aman" log_harian.txt", maka terminal akan memunculkan baris yang mengandung kata tersebut. Command "grep" ini sensitif, jadi karakter "a" dan "A" akan dianggap berbeda.<br>
<img width="397" height="40" alt="image" src="https://github.com/user-attachments/assets/61b7baa1-a95b-4f24-923c-be62c8708355" /><br><br>
Jika saya ingin mencari kata "Aman" dan tidak peduli akan huruf kapital atau tidak, maka saya bisa menambahkan "-i" setelah command "grep". <br>
<img width="456" height="80" alt="image" src="https://github.com/user-attachments/assets/d7024617-f645-4a5f-8061-5a5ffbeffd3a" /><br><br>
Jika saya ingin mencari kata selain "Aman", saya bisa menambahkan "-v" setelah command "grep".<br>
<img width="454" height="81" alt="image" src="https://github.com/user-attachments/assets/f208f05b-0aa5-4002-961e-d1702a8edada" /><br><br>
























