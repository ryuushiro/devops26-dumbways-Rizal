# 1. Akses server menggunakan terminal (Windows Terminal/PuTTY/etc.)
Pertama, jalankan command “sudo apt install openssh-server” untuk menginstall openssh-server.
<img width="593" height="163" alt="image" src="https://github.com/user-attachments/assets/5c8bdf44-c1f9-4914-a60a-f72d413e0470" /><br><br>
Kedua, pastikan service-nya jalan dan otomatis menyala saat booting dengan menggunakan command “sudo systemctl enable --now ssh”. “systemctl” adalah command yang digunakan untuk mengontrol sistem yang ada di Linux. Jadi di command ini “systemctl enable --now ssh” bisa diartikan sebagai “Control, tolong nyalakan ssh nya setiap setelah reboot”.
<img width="807" height="71" alt="image" src="https://github.com/user-attachments/assets/d989d592-ecbd-43a7-bfde-122cde79ae50" /><br><br>
Lalu kita cek status SSH nya.<br>Bisa dilihat kalau statusnya active (running) berwarna hijau. Tandanya SSH sudah berjalan.
<img width="848" height="331" alt="image" src="https://github.com/user-attachments/assets/afa8f435-b5ff-45f9-8a5a-9b32981a4581" /><br><br>
Setelah SSH sudah dibuat, sekarang waktunya kita membuka Terminal di Windows. Jangan lupa untuk menjalankannya sebagai administrator!<br>
<img width="420" height="137" alt="image" src="https://github.com/user-attachments/assets/3cb560a5-975e-4fec-b259-d92719ce540e" /><br><br>
Lalu untuk mengakses SSH yang ada di VM, kita jalankan command “ssh rzl@ip-vm”. Maksud dari command itu kita memanggil aplikasi SSH dari user ‘rzl’ yang berada di dalam ‘ip-vm’ nya. Di kasus saya sekarang, karena IP yang saya gunakan untuk VM saya adalah “10.42.103.208”, jadi saya gunakan command “ssh rzl@10.42.103.208”. Setelah memasukkan command itu, masukkan password Ubuntu Servernya dan tekan ‘Enter’.
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
Setelah isi kode panjangnya sudah disalin, buka server dan meluncur ke folder /.ssh/ di server.
<img width="303" height="106" alt="image" src="https://github.com/user-attachments/assets/3e49720e-fbe4-4af9-842b-55d1fa372ab7" /><br><br>
Lalu kita gunakan command “nano authorized_keys” untuk edit file. Kita masukkan kode panjang dari “kunci.pub” tadi ke dalam file “authorized_keys”.
<img width="1438" height="610" alt="image" src="https://github.com/user-attachments/assets/011d3492-519b-4a99-a646-2e6a5c896f91" /><br><br>
Setelah itu, kita save filenya dan coba logout.<br>
Setelah logout, kita coba login ke dalam server menggunakan command “ssh -i .ssh/kunci rzl@10.126.87.208”. Di sini, terminal akan memanggil file “kunci” yang ada di Windows, bukan di dalam Ubuntu Server. Jika kode yang terdapat dalam file “kunci” cocok dipasangkan dengan kode yang kita masukkan kedalam “authorized_keys” tadi, maka login akan sukses.
<img width="1436" height="677" alt="image" src="https://github.com/user-attachments/assets/dd917eaf-3a0c-4b2b-9414-7361cb58da64" />





