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
