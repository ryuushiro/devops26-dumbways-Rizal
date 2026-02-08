# Challenge 1: APP ON BACKGROUND
```
Challenge :
1. NodeJS + Python berjalan di background (tanpa kondisi attached di terminal)
   - artinya, teman-teman tetep bisa menggunakan terminal di window yang sama namun app tetap berjalan
2. Golang bisa dibuka di browser kalian, menampilkan text "Jangan lupa sahur baby gurl rawr"
```

## 1. NodeJS + Python berjalan di background (tanpa kondisi attached di terminal)
Setelah mencari caranya di internet, saya menemukan jika saya menaruh simbol `&` di akhir command, maka proses dari command tersebut akan berjalan di _background_ dan langsung mengembalikan kendali terminal ke pengguna.<br>
- **Jalankan npm dengan `&` di akhir command.**<br>
<img width="562" height="52" alt="image" src="https://github.com/user-attachments/assets/1858b8c5-0e7e-4017-8c81-692e9cb5f850" /><br><br>
- **Setelah menjalankan command tersebut, terminal akan mengizinkan pengguna untuk digunakan lagi dan laman web yang tadi dinyalakan masih berjalan.**
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/bf068a17-d9d2-4112-a03b-efa9ee141d58" /><br><br>
- **Sekarang kita bisa jalankan file pythonnya.**
<img width="1199" height="227" alt="image" src="https://github.com/user-attachments/assets/7b8e479f-9be5-4ecf-9096-70f4d5559f57" /><br><br>
- **Setelah keduanya dijalankan, kita bisa buktikan kalau keduanya berjalan berbarengan tanpa mengganggu satu sama lain.**
<img width="1920" height="1040" alt="image" src="https://github.com/user-attachments/assets/ff6a79ca-553e-4a59-a6c0-0aacd3af3619" /><br><br>

## 2. Golang bisa dibuka di browser kalian, menampilkan text "Jangan lupa sahur baby gurl rawr"
- **Buat file go baru untuk dijalankan.** <br>
<img width="612" height="355" alt="image" src="https://github.com/user-attachments/assets/717a9cb5-2618-4150-90ce-a7dcd0886181" /><br><br>

>**PENJELASAN SINGKAT KODE**<br>
> - `package main`           : Menandakan kalau ini adalah aplikasi utama yang bisa dijalankan.<br>
> - `import(...)`            : Kode untuk mengimpor _library_ bawaan Go yang sudah diinstal. Di _script_ ini, aplikasi mengimpor `fmt` dan `net/http` yang berfungsi untuk manajemen teks di layar dan menangani request HTTP.<br>
> - `func sahurHandler(...)` : Fungsi yang akan bekerja di _script_ untuk menerima koneksi dari browser (`r *http.Request`), dan langsung mengirimkan teks yang tertulis di bawahnya (`w http.ResponseWriter` mengambil teks yang ada di fungsi `fmt.Fprintf(w,".....")`. <br>
> - `func main()` : Fungsi utama yang akan dicari oleh komputer saat pengguna menjalankan command `go run`. Isi dari fungsi ini adalah `http.HandleFunc("/", sahurHandler)` yang akan membawa pengguna ke laman utama lalu ke fungsi `func sahurHandler(...)` tadi. Lalu `fmt.Println("Server jalan di port 8080...")` mengeluarkan _feedback_ ke terminal dengan mengeluarkan teks berisikan ""Server jalan di port 8080..." Code `http.ListenAndServe(":8080", nil)` berfungsi sebagai alat untuk membuka dan melayani laman ini di port 8080, dan akan _standby_ .<br>

- **Setelah membuat _script_ Go nya, cek ufw apakah portnya sudah terbuka.** <br>
<img width="516" height="273" alt="image" src="https://github.com/user-attachments/assets/6bf873a7-f97a-4783-9b7a-17bf94fe3b38" /><br><br>
- **Setelah yakin kalau port 8080 sudah terbuka, maka kita bisa jalankan _script_ tersebut dan buka IP:PORT.** <br>
<img width="396" height="70" alt="image" src="https://github.com/user-attachments/assets/143f634d-2d47-444f-b37b-51e6387af85e" /> <br>
<img width="548" height="117" alt="image" src="https://github.com/user-attachments/assets/c0385a5e-eab7-4b60-97f0-57360844ef7d" /><br><br>




