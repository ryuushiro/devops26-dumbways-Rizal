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
| **`pwd`** | Dasar | *Print Working Directory*, menampilkan posisi directory saat ini. | `pwd` |
| **`ls`** | Dasar | *List*, menampilkan daftar file dan directory. | `ls -la` (menampilkan detail & file tersembunyi) |
| **`cd`** | Dasar | *Change Directory*, berpindah ke directory lain. | `cd .ssh` (pindah ke directory /.ssh/)  |
| **`mkdir`** | File | *Make Directory*, membuat directory baru. | `mkdir directorybaru` |
| **`touch`** | File | Membuat file kosong baru. | `touch filekosongbaru` |
| **`cp`** | File | *Copy*, menyalin file atau directory. | `cp foto.jpg /backup/foto.jpg` |
| **`mv`** | File | *Move*, memindahkan file atau mengganti nama (*rename*). | `mv file_lama.txt file_baru.txt` |
| **`rm`** | File | *Remove*, menghapus file. | `rm data_sampah.txt` |
| **`cat`** | Baca File | Menampilkan isi file teks langsung di terminal. | `cat catatan.txt` |
| **`nano`** | Editor | Membuka text editor sederhana di terminal. | `nano /etc/netplan/50-cloud-init.yaml` |
