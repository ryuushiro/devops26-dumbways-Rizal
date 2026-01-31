# 1 A. Diagram Jaringan Komputer
<img width="826" height="428" alt="image" src="https://github.com/user-attachments/assets/c8a10504-40f0-41bf-a21d-7736f826c5c3" /> <br><br>

# 1 B. Tabel Konfigurasi IP
| Device | IP Address | Subnet Mask | Gateway | Keterangan |
| :--- | :--- | :--- | :--- | :--- |
| **Router** | 192.168.11.1 | 255.255.255.0 | - | Gateway Jaringan |
| **Device 1** | 192.168.11.31 | 255.255.255.0 | 192.168.11.1 | Laptop |
| **Device 2** | 192.168.11.32 | 255.255.255.0 | 192.168.11.1 | HP/Tablet|
| **Device 3** | 192.168.11.33 | 255.255.255.0 | 192.168.11.1 | Komputer 1 |
| **Device 4** | 192.168.11.34 | 255.255.255.0 | 192.168.11.1 | Komputer 2 |

# 2. Shell (SH) vs Bourne-Again Shell (BASH)
Shell merupakan interface dari command line. Di sinilah pengguna bisa berinteraksi dengan kernel untuk memasukkan "commands" dan menjalankannya.
Sedangkan Bash merupakan salah satu upgrade/penyempurnaan dari Shell. Perbedaan dari SH dan BASH terletak pada banyaknya perintah dan fitur. Karena BASH adalah versi penyempurnaan dari SH, BASH memiliki perintah dan fitur yang lebih banyak dari SH.

# 3. Linux Commands
| Command | Kategori | Fungsi | Contoh Penggunaan |
| :--- | :--- | :--- | :--- |
| **`pwd`** | Dasar | *Print Working Directory*, menampilkan posisi folder saat ini. | `pwd` |
| **`ls`** | Dasar | *List*, menampilkan daftar file dan folder. | `ls -la` (menampilkan detail & file tersembunyi) |
| **`cd`** | Dasar | *Change Directory*, berpindah ke folder lain. | `cd /var/www` |
| **`mkdir`** | File | *Make Directory*, membuat folder baru. | `mkdir tugasku` |
| **`touch`** | File | Membuat file kosong baru. | `touch index.html` |
| **`cp`** | File | *Copy*, menyalin file atau folder. | `cp foto.jpg /backup/foto.jpg` |
| **`mv`** | File | *Move*, memindahkan file atau mengganti nama (*rename*). | `mv file_lama.txt file_baru.txt` |
| **`rm`** | File | *Remove*, menghapus file. | `rm data_sampah.txt` |
| **`cat`** | Baca File | Menampilkan isi file teks langsung di terminal. | `cat catatan.txt` |
| **`nano`** | Editor | Membuka text editor sederhana di terminal. | `nano config.conf` |
| **`grep`** | **Lanjutan (+)** | Mencari kata kunci spesifik di dalam sebuah file teks. | `grep "error" syslog.log` |
| **`chmod`** | **System (+)** | *Change Mode*, mengubah izin akses (*permission*) file. | `chmod 777 script.sh` |
| **`chown`** | **System (+)** | *Change Owner*, mengubah pemilik file. | `chown user:group file.txt` |
| **`ping`** | Network | Mengecek koneksi ke alamat IP/Website lain. | `ping 8.8.8.8` |
| **`ip a`** | Network | Menampilkan informasi IP Address (pengganti `ifconfig`). | `ip a` |
| **`htop`** | **Extra (++)** | Task manager interaktif untuk monitor CPU & RAM. | `htop` |
| **`history`**| **Extra (++)** | Melihat riwayat perintah yang pernah diketik sebelumnya. | `history` |
| **`man`** | Manual | Membuka buku panduan lengkap untuk suatu perintah. | `man ls` |
