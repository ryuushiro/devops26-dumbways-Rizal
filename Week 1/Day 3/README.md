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
# 4. Nyalakan ufw dengan memberikan akses untuk port 22, 80, 443, 3000, 5000 dan 6969!
Untuk menyalakan UFW, kita jalankan command "sudo ufw enable".<br>
<img width="718" height="79" alt="image" src="https://github.com/user-attachments/assets/0294de5f-ba18-4964-a264-061acfd226fd" /><br><br>
Setelah itu, kita bisa buka/izinkan akses port masing-masing menggunakan "sudo ufw allow angkaport".<br>
<img width="328" height="370" alt="image" src="https://github.com/user-attachments/assets/56b78073-fef6-4da7-85e8-30daadb90b89" /><br><br>
Port yang sudah dibuka/diizinkan bisa dicek dengan command "sudo ufw status verbose". Saya menggunakan "verbose" untuk melihat detail lebih.<br>
<img width="588" height="386" alt="image" src="https://github.com/user-attachments/assets/3289d0fa-674f-4a3d-9749-f2e1426e85d2" /><br><br>
Jika sebelumnya kita membuka akses port, selanjutnya kita bisa juga menolak akses port menggunakan "sudo ufw deny angkaport". Contohnya, saya ingin menolak akses port 6767. Terlihat pada gambar di bawah kalau aksesnya menjadi "DENY IN".<br>
<img width="581" height="486" alt="image" src="https://github.com/user-attachments/assets/78a8fd17-b9e4-4b99-8676-f4bbef55bf37" /><br><br>
# CHALLENGE!
## Text Manipulation: membuat bash script yang bertujuan untuk menambah/menghapus/mengubah isi dari file yang sudah ada.
Sebelum membuat BASH Script, saya membuat file yang akan diedit terlebih dahulu yaitu "belanja.txt".<br>
<img width="547" height="140" alt="image" src="https://github.com/user-attachments/assets/c203c7d4-f707-491f-bd70-5fa481610931" /><br><br>
Setelah membuat file yang akan diedit, sekarang kita buat BASH Script-nya. Saya buat file baru dengan nano menggunakan command "nano atur_belanja.sh". Lalu, saya isikan dengan script di bawah ini.<br>
```bash
#!/bin/bash

FILE="belanja.txt"

if [ ! -f "$FILE" ]; then
    touch "$FILE"
fi

while true; do
    clear
    
    echo "=========================================="
    echo "        APLIKASI CATATAN BELANJA          "
    echo "=========================================="
    
    echo "Isi File Saat Ini:"
    if [ -s "$FILE" ]; then
        cat -n "$FILE"
    else
        echo "(Daftar masih kosong)"
    fi
    echo "=========================================="
    
    echo "Mau ngapain nih?"
    echo "[1] Tambah Barang"
    echo "[2] Ubah Barang"
    echo "[3] Hapus Barang"
    echo "[4] Keluar Aplikasi"
    echo "------------------------------------------"
    
    read -p "Masukkan pilihanmu (1-4): " pilihan

    case "$pilihan" in
        1)
            echo "--- TAMBAH BARANG ---"
            read -p "Mau nambah apa? : " barang_baru
            
            echo "$barang_baru" >> "$FILE"
            echo ">> Baik, '$barang_baru' sudah dicatat."
            ;;
            
        2)
            echo "--- UBAH BARANG ---"
            read -p "Nama barang lama : " lama
            read -p "Nama barang baru : " baru
            
            if grep -q "$lama" "$FILE"; then
                sed -i "s/$lama/$baru/g" "$FILE"
                echo ">> Baik, '$lama' sudah diganti menjadi '$baru'."
            else
                echo ">> Barang '$lama' tidak ditemukan."
            fi
            ;;
            
        3)
            echo "--- HAPUS BARANG ---"
            read -p "Apa yang mau dihapus? : " hapus
            
            if grep -q "$hapus" "$FILE"; then
                sed -i "/$hapus/d" "$FILE"
                echo ">> Baik,'$hapus' sudah dibuang dari daftar."
            else
                echo ">> Barang '$hapus' tidak ditemukan."
            fi
            ;;
            
        4)
            echo "Sampai jumpa lagi..."
            break  
            ;;
            
        *)
            echo "Pilihan salah. Pilih 1 sampai 4."
            ;;
    esac

    echo ""
    read -p "Tekan Enter untuk kembali ke menu..." dummy
done

```

### PENJELASAN
>Pada BASH Script di atas, saya menggunakan berbagai command dan code baru.<br>
Di awal, saya menggunakan **"if"** untuk mengecek kondisi. Contoh "if [ -s "$FILE" ]" mengecek apakah $file ada isinya atau tidak.<br><br>
Untuk pilihan pengguna (user), saya pertama menggunakan **"read"** untuk membaca masukan, dan menggunakan **"case"** untuk memilah masukan dari pengguna. Contohnya, pada code "read -p "Masukkan pilihanmu (1-4): " pilihan", pengguna akan memasukkan angka 1 sampai 4. Setelah pengguna memasukkan angkanya, fungsi "case" (case "$pilihan" in) akan terpanggil sesuai dengan angka yang dimasukkan.<br><br>
Lalu saya juga menggunakan **"While Loop"** di aplikasi ini karena saya ingin aplikasi ini tetap berjalan sampai pengguna memilih untuk menutup aplikasinya (jika pengguna memilih 4, maka **"break"** akan berjalan dan menghentikan aplikasi).<br><br>
Di awal saya menggunakan **"clear"** agar tampilan tidak menumpuk.<br><br>
Di akhir code setelah "case", saya menggunakan variabel **"dummy"**. Masukan yang diberikan pengguna ke variabel ini tidak terpakai. Saya gunakan code ini agar pengguna bisa membaca "echo" sukses atau tidak sukses dalam mengedit teks sebelum command "clear" berjalan. Jadi, "clear" baru berjalan setelah pengguna memencet tombol enter. <br>

Setelah membuat file "atur_belanja.sh", kita berikan izin eksekusi agar script bisa dijalankan dengan command "chmod +x atur_belanja.sh"<br>
Untuk memulai scriptnya, kita bisa memanggil nama scriptnya dengan "./atur_belanja.sh". Setelah pengguna memencet tombol enter, script akan langsung berjalan.<br><br>
<img width="408" height="304" alt="image" src="https://github.com/user-attachments/assets/ea60204d-0f63-428a-99f3-198357771c42" /><br>Tampilan utama BASH Script autr_belanja.sh.<br><br>
<img width="396" height="413" alt="image" src="https://github.com/user-attachments/assets/8eaa3a66-ea8c-40cb-ab90-4563ced733c7" /><br><img width="248" height="105" alt="image" src="https://github.com/user-attachments/assets/a31e7119-ad86-489a-b11a-9677193880af" /><br>Tampilan setelah pengguna menambahkan barang.<br><br>
<img width="463" height="427" alt="image" src="https://github.com/user-attachments/assets/39ca0873-3483-4f6f-960c-2bae31160474" /><br><img width="183" height="101" alt="image" src="https://github.com/user-attachments/assets/160bc8b6-cf88-4b20-bbad-eaa2a7115242" /><br>Tampilan setelah pengguna mengganti"Pisang" ke "Mangga".<br><br>
<img width="403" height="417" alt="image" src="https://github.com/user-attachments/assets/cdd06fff-ac76-4170-a5e1-b9fa04c673af" /><br><img width="231" height="83" alt="image" src="https://github.com/user-attachments/assets/a2d2f0f7-6c96-4311-b5b7-926a0c3def63" /><br>Tampilan setelah pengguna menghapus "Susu".<br><br>
<img width="406" height="334" alt="image" src="https://github.com/user-attachments/assets/b4c2a145-f38b-40bb-b944-3a00c2c1329f" /><br>Tampilan setelah pengguna memilih untuk selesai.

## UFW Manipulation with BASH Script
Pertama, buat file BASH Scriptnya dengan "nano ufwscript.sh" dan isi dengan code di bawah: <br>
```bash
#!/bin/bash

echo "SISTEM UFW CHALLENGE!"
echo "Mau apa sekarang?"
echo "Masukkan ON untuk menyalakan firewall."
echo "Masukkan OFF untuk mematikan firewall."
echo "Masukkan ALLOW untuk membuka akses port."
echo "Masukkan DENY untuk menolak akses port."
echo "Masukkan HAPUS untuk menghapus aturan yang sudah dibuat."
read perintah

perintah=${perintah,,}

case "$perintah" in
    "on")
        echo "Menyalakan Firewall..."
        sudo ufw enable
        ;;
        
    "off")
        echo "Mematikan Firewall..."
        sudo ufw disable
        ;;
        
    "allow")
        echo "Mau kasih akses ke port berapa?"
        read port
        sudo ufw allow $port
        echo "Akses ke port $port berhasil dibuka."
        ;;
        
    "deny")
        echo "Mau tolak akses ke port berapa?"
        read port
        sudo ufw deny $port
        echo "Akses ke port $port berhasil ditolak."
        ;;
        
    "hapus")
        echo "Mau hapus aturan yang mana?"
        echo "Format: [allow/deny] [port] (Contoh: allow 80)"
        read rule
        sudo ufw delete $rule
        ;;
        
    *)
        echo "Maaf, perintah '$perintah' tidak dikenali."
        echo "Pilihannya cuma: on, off, allow, deny, atau hapus."
        exit 1
        ;;
esac
```
### PENJELASAN
>Pada BASH Script di atas, script akan menjalankan command sesuai dengan panggilan case-nya.<br>









































