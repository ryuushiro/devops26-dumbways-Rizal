# Tugas 1: NodeJS
```
- Deploy app wayshub-frontend
- Berjalan di port 3000
- Menggunakan NodeJS 10 & 12
Resource: https://github.com/dumbwaysdev/wayshub-frontend
```

- Unduh NodeJS dengan mengunjungi laman web resminya. Pilih versi, OS, nvm, dan npm, lalu ikuti petunjuk yang ada.<br>
<img width="527" height="352" alt="image" src="https://github.com/user-attachments/assets/38db1b21-a0ea-44b7-83c1-fc3f9e3ec409" /><br><br>
- Setelah mengikuti petunjuk yang ada di web, seharusnya sudah terinstall NodeJS versi LTS terbaru. Namun, karena di tugas mengharuskan saya menggunakan versi 10 atau 12, maka saya install nvm versi 12 dengan menjalankan command "nvm install 12". Lalu, gunakan "nvm use 12" untuk menggunakannya.
<img width="527" height="204" alt="image" src="https://github.com/user-attachments/assets/dae50b01-d101-44c3-84db-73e6189cfce3" /><br><br>
- Setelah menginstall NodeJS, sekarang saya akan _clone_ repository resource "wayshub-frontend" ke git lokal dengan menjalankan command "git clone <https://github.com/dumbwaysdev/wayshub-frontend.git>". Setelah selesai, masuk ke directory yang sudah diunduh.<br>
<img width="527" height="166" alt="image" src="https://github.com/user-attachments/assets/a2fcbe22-4b89-45a0-be7d-05a6f0b11aa8" /><br><br>
- Jalankan "npm install" untuk menginstall paket-paket npm yang dibutuhkan untuk menjalankan proyeknya.
<img width="527" height="287" alt="image" src="https://github.com/user-attachments/assets/1daccea2-f48c-4f4c-aedc-8a9d0331a17c" /><br><br>
- Kalau paket dependensinya sudah terinstall, kita bisa jalankan command "PORT=3000 npm start" untuk memulai proyek NodeJS tersebut. Mengapa menggunakan command tersebut? "PORT=3000" digunakan untuk membuat proyek tersebut berjalan di Port 3000 (sesuai tugas), dan "npm start" adalah command standar NodeJS untuk menjalankan script.
<img width="541" height="36" alt="image" src="https://github.com/user-attachments/assets/77cc7174-eb06-42d6-bb83-41e888c0fcf4" /><br><br>
- Setelah command dijalankan, maka proyek web "wayshub-frontend" sudah bisa diakses lewat IP_Lokal:Port.
<img width="527" height="284" alt="image" src="https://github.com/user-attachments/assets/8aa5d483-69e0-4d8e-b66f-ea4db0475739" /><br><br>

# Tugas 2: Python
```
- Deploy app menampilkan text nama kalian!
- Berjalan di port 5000 & bisa dibuka melalui web
```
- Install python dan pip menggunakan command "sudo apt install python3" dan "sudo apt install python3-pip"<br>
<img width="527" height="241" alt="image" src="https://github.com/user-attachments/assets/a94cf464-841e-47cc-bea5-0fbf4c1a1e68" /><br><br>
- Buat folder khusus untuk python agar tidak berantakan.<br>
<img width="284" height="72" alt="image" src="https://github.com/user-attachments/assets/b6d0441b-5c81-47ac-acfd-95069765a3f0" /><br><br>
- Install paket flask untuk kerangka web python dengan menggunakan command "pip3 install flask". Tapi, ternyata ada error saat saya mencoba untuk menginstallnya. Setelah say acari solusi, kemungkinan error ini terjadi karena saya menggunakan Ubuntu Server versi terbaru dan sistemnya melarang untuk menginstall paket python langsung ke sistem. Jadi, saya harus membuat _Virtual Environment_ baru terlebih dahulu untuk proyek ini seperti gambar di bawah.
<img width="1316" height="328" alt="image" src="https://github.com/user-attachments/assets/85fb5e9c-665a-469d-b5aa-f10dfeef9074" /><br>
Gambar 1: Error saat install flask.<br><br>
<img width="1311" height="543" alt="image" src="https://github.com/user-attachments/assets/422fd648-7dba-4d32-bb48-e5f5b3efc55f" /><br>
Gambar 2: buat _Virtual Environment_ baru dan install flask.<br><br>
- Setelah flask selesai terinstall, lanjut membuat dokumen python baru untuk ditampilkan di laman web.
<img width="527" height="190" alt="image" src="https://github.com/user-attachments/assets/46cd448b-25cd-4449-9b21-bdbf586c2a98" /><br><br>
- Jalankan _script_ python tersebut dengan command "python3 index.py".
<img width="527" height="126" alt="image" src="https://github.com/user-attachments/assets/90c2fa53-3b70-49e6-942c-a532a7e44b47" /><br><br>
- Setelah dijalankaan _script_\-nya, kunjungi Alamat_IP:Port sesuai yang tertampil.
<img width="527" height="98" alt="image" src="https://github.com/user-attachments/assets/c84d3a1f-276d-4fe3-b701-3a4ac3c61a5f" /><br><br>

# Tugas 3: Golang
```
- Deploy app menampilkan text "Golang geming!"
```
- Pertama, untuk menginstall Go versi terbaru, buka <https://go.dev/dl/> dan klik kanan _link_ untuk mengunduh versi Linux.<br>
<img width="904" height="512" alt="image" src="https://github.com/user-attachments/assets/b615a610-c82f-439c-a438-35d1335aea1f" /><br><br>
- Setelah itu, jalankan command "wget url_link_unduh" untuk mengunduh aplikasinya.<br>
<img width="904" height="283" alt="image" src="https://github.com/user-attachments/assets/0a084edc-3487-4b3c-b8f9-650cec0dfc61" /><br><br>
- Lalu, pergi ke <https://go.dev/doc/install> dan ikuti tutorialnya. Disarankan untuk mengganti _super user_ _account_ di server menjadi "root" terlebih dahulu menggunakan command "sudo su" untuk menjalankan command pertama.
<img width="614" height="619" alt="image" src="https://github.com/user-attachments/assets/7ffefa72-ced2-49f4-865a-ec6fbcc32c4a" /><br>
<img width="904" height="268" alt="image" src="https://github.com/user-attachments/assets/1e509ed5-018c-4bde-a79c-9ff5034fea71" /><br>
<img width="631" height="404" alt="image" src="https://github.com/user-attachments/assets/65e93cab-1316-415c-8ae3-9de8edb8f65f" /><br><br>
- Jalankan _script_ golangnya dengan command "go run index.go"<br>
<img width="614" height="103" alt="image" src="https://github.com/user-attachments/assets/f7c7b7e6-f2db-4b8e-8d7e-14b2f3062d35" />
