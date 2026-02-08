# 1\. Gambarkan sturktur web server menggunakan reverse proxy dan jelaskan cara kerjanya
<img width="620" height="338" alt="Untitled Diagram drawio (1)" src="https://github.com/user-attachments/assets/1ecfd8e3-a6ac-4b4d-99cc-a2542df42b90" /><br><br>
_Reverse_ Proxy adalah server/lapisan perantara yang ditempatkan di depan web server _(backend)_ untuk menerima permintaan dari klien _(web browser)_ dan meneruskannya ke server yang sesuai.
Cara kerjanya adalah sebagai berikut:
- Klien (bisa dari komputer, laptop, hp, dll) mengirim permintaan (_request_) ke server tujuan.
- _Reverse proxy_ menerima permintaan tersebut. Selanjutnya, _reverse proxy_ akan memeriksa permintaan dan aturan dalam _proxy_.
- _Reverse proxy_ akan meneruskan permintaan ke web server sesuai dengan permintaan dari klien.
- Web server menerima permintaan lalu memberikan respon balik ke _reverse proxy_.
- _Reverse proxy_ menerima respon balik dari web server dan meneruskannya ke klien.

# 2\. Buatlah Reverse Proxy untuk aplilkasi yang sudah kalian deploy kemarin. (wayshub), untuk domain nya sesuaikan nama masing" ex: ade.xyz

- Jalankan "sudo apt update" terlebih dahulu agar sistem selalu _up to date_.<br>
<img width="904" height="787" alt="image" src="https://github.com/user-attachments/assets/e7fc162a-d828-4868-9567-0e7afba1add3" /><br><br>
- Lalu, mulai laman web "Wayshub Frontend" yang sudah dibuat dengan NodeJS yang berjalan di Port 3000.<br>
<img width="679" height="148" alt="image" src="https://github.com/user-attachments/assets/a49bb17a-3b83-4dbe-acb3-f811df87db5d" /><br><br>
- Setelah web sudah berjalan, buka aplikasi teks editor seperti notepad, sublime text, dll, dengan izin sebagai administrator (_Run as Administrator_), lalu buka file bernama "hosts" di dalam folder "C:\\Windows\\System32\\drivers\\etc".<br>
<img width="904" height="661" alt="image" src="https://github.com/user-attachments/assets/00f8cec5-d2a0-412d-914e-f250f75bcd09" /><br><br>
- Tambahkan alamat IP tempat web "Wayshub Frontend" berada ke dalam file tersebut dengan format "ip_address domain.xyz". Lalu simpan.<br>
<img width="779" height="515" alt="image" src="https://github.com/user-attachments/assets/4f068cf5-78ed-48d6-90c5-5689899fdb8a" /><br><br>
- Setelah web server dan hosts sudah diatur, install nginx yang akan digunakan untuk reverse proxy dengan command "sudo apt install nginx".<br>
<img width="835" height="130" alt="image" src="https://github.com/user-attachments/assets/bdb19d93-a6c7-426e-a901-2666186e9c81" /><br><br>
- Cek status ngnx dengan menjalankan command "sudo systemctl status nginx". Terlihat pada gambar di bawah kalua nginx sudah berjalan dan selalu mulai saat server dijalankan.<br>
<img width="904" height="278" alt="image" src="https://github.com/user-attachments/assets/ab230e63-19f8-4287-811f-3db34e843f6f" /><br><br>
- Sebelum lanjut, cek status ufw terlebih dahulu untuk memastikan bahwa port 80 dan 443 terbuka.<br>
<img width="734" height="305" alt="image" src="https://github.com/user-attachments/assets/55cfbd4a-3513-4c3d-971e-c0d16516ba99" /><br><br>
- Masuk ke _directory_ /etc/nginx/sites-enabled/ untuk membuat file konfigurasi baru di sana.<br>
<img width="904" height="76" alt="image" src="https://github.com/user-attachments/assets/8dc4170b-fcdd-4dce-988f-cf59b3f3f0ed" /><br>
<img width="904" height="250" alt="image" src="https://github.com/user-attachments/assets/d8777c72-2621-41fe-bc14-bb3adf95d221" /><br><br>
- Cek konfigurasi file nginx yang tadi baru dibuat dengan command "sudo nginx -t".<br>
<img width="904" height="118" alt="image" src="https://github.com/user-attachments/assets/ebf6d4d7-e675-4fdd-9fd7-8c4b5796e3da" /><br><br>
- Setelah tes berhasil, _reload_ nginx dengan command "sudo systemctl reload nginx", lalu cek apakah nginx Kembali berjalan dengan normal.<br>
<img width="904" height="135" alt="image" src="https://github.com/user-attachments/assets/632f76cf-0474-40d0-b89b-45ede3b01b45" /><br><br>
- Sekarang, url <http://rizal.xyz/> sudah bisa diakses dengan frontend NodeJS yang telah dibuat.<br>
<img width="904" height="600" alt="image" src="https://github.com/user-attachments/assets/56100c0f-6f09-45b7-ac09-9962171e165d" /><br><br>
