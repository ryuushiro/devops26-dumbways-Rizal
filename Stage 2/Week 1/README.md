<img width="975" height="585" alt="image" src="https://github.com/user-attachments/assets/ae8d5a32-225c-4779-bae8-f57800cd5e95" />## Week 1 - Linux & Server Management

---

## Persiapan

- Repository: `devops26-dumbways-rzl`
- Server IP: `202.10.34.220`
- OS: Ubuntu 24.04
- Web Server: Nginx
- Process Manager: PM2

---

## 1. Membuat User Baru

Membuat user baru di server untuk masing-masing anggota tim.

```bash
sudo adduser rizal
```
<img width="975" height="494" alt="image" src="https://github.com/user-attachments/assets/665b43ca-370e-4b66-a8b9-bfc28e187aef" />


---

## 2. SSH Key - Login Tanpa Password

### Generate SSH Key di Windows (PowerShell)

```powershell
ssh-keygen
```

Simpan key dengan nama custom, misalnya `rzl`. Hasilnya:
- Private key: `~/.ssh/rzl`
- Public key: `~/.ssh/rzl.pub`

<img width="975" height="430" alt="image" src="https://github.com/user-attachments/assets/14ed2352-7e46-434a-97bd-dacf4638762e" />


### Copy Public Key ke Server

```powershell
scp ~/.ssh/rzl.pub rzl@202.10.34.220:.ssh/authorized_keys
```

### Konfigurasi SSH Config (Windows)

Buat file `~/.ssh/config` agar tidak perlu mengetik perintah panjang setiap kali login:

```
Host dumbways-rzl
    HostName 202.10.34.220
    User rzl
    IdentityFile ~/.ssh/rzl
```
<img width="681" height="545" alt="image" src="https://github.com/user-attachments/assets/fb92a024-0e4c-4045-b9ec-faf7f968499c" />


### Aktifkan PubkeyAuthentication di Server

```bash
sudo nano /etc/ssh/sshd_config
```

Pastikan baris berikut aktif (tidak ada tanda `#`):

```
PubkeyAuthentication yes
AuthorizedKeysFile .ssh/authorized_keys
PasswordAuthentication no
```

Restart SSH:

```bash
sudo systemctl restart sshd
```

### Login Menggunakan SSH Key

```powershell
ssh dumbways-rzl
```
<img width="975" height="430" alt="image" src="https://github.com/user-attachments/assets/fccb7b15-ee4c-4195-b1c5-2ba9f59bc307" /><br>

---

## 3. Deploy MySQL (Appserver)

### Install MySQL

```bash
sudo apt update
sudo apt install mysql-server -y
```
<img width="975" height="606" alt="image" src="https://github.com/user-attachments/assets/95c5a1bb-2880-4b1a-8544-5cb085308ff8" />


### Jalankan Secure Installation

```bash
sudo mysql_secure_installation
```
<img width="975" height="606" alt="image" src="https://github.com/user-attachments/assets/a9824b29-c503-4240-aa8d-4761329f9858" />


Ikuti langkah-langkah berikut:
- Validate password plugin → `Y`
- Password strength → `1` (MEDIUM)
- Remove anonymous users → `Y`
- Disallow root login remotely → `Y`
- Remove test database → `Y`
- Reload privilege tables → `Y`

### Set Password Root & Buat User Baru

Login ke MySQL:

```bash
sudo mysql
```

Jalankan perintah berikut:

```sql
-- Set password root
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'password_root';

-- Buat user baru
CREATE USER 'rzl'@'%' IDENTIFIED BY 'password_user';

-- Buat database baru
CREATE DATABASE wayshub-dumbways;

-- Berikan privileges ke user baru
GRANT ALL PRIVILEGES ON dumbflix.* TO 'rzl'@'%';
FLUSH PRIVILEGES;
EXIT;
```
<img width="622" height="209" alt="dumbways-01-01" src="https://github.com/user-attachments/assets/35c69cdd-1dca-4f50-89fa-954a8641113f" /><br>
<img width="975" height="453" alt="image" src="https://github.com/user-attachments/assets/789036b1-a2a9-46b4-bbc1-0f5a8353b76c" /><br>


### Ubah Bind Address

```bash
sudo nano /etc/mysql/mysql.conf.d/mysqld.cnf
```

Ubah baris berikut:

```
bind-address = 127.0.0.1
```

Menjadi:

```
bind-address = 0.0.0.0
```
<img width="975" height="327" alt="image" src="https://github.com/user-attachments/assets/14fefa7c-fbed-4d82-bb60-1b1df1690404" /><br>

Restart MySQL:

```bash
sudo systemctl restart mysql
```

---

## 4. Role Based

### Buat Database `demo` dan Tabel `transaction`

```sql
CREATE DATABASE demo;
USE demo;

CREATE TABLE transaction (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(100),
    amount DECIMAL(10,2),
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```
<img width="975" height="347" alt="image" src="https://github.com/user-attachments/assets/e7a07bcd-64e4-4c3a-ba1c-f60b4a5fae36" />
<br>

### Buat Role

```sql
CREATE ROLE 'admin_role';
CREATE ROLE 'guest_role';
```

### Berikan Privileges ke Masing-masing Role

```sql
-- admin bisa melakukan semua operasi
GRANT SELECT, INSERT, UPDATE, DELETE ON demo.transaction TO 'admin_role';

-- guest hanya bisa melihat data
GRANT SELECT ON demo.transaction TO 'guest_role';
```

### Buat User dan Assign Role

```sql
-- User admin
CREATE USER 'rzl'@'%' IDENTIFIED BY 'password_admin';
GRANT 'admin_role' TO 'rzl'@'%';
SET DEFAULT ROLE ALL TO 'rzl'@'%';

-- User guest
CREATE USER 'guest'@'%' IDENTIFIED BY 'guest';
GRANT 'guest_role' TO 'guest'@'%';
SET DEFAULT ROLE ALL TO 'guest'@'%';

FLUSH PRIVILEGES;
```
<img width="975" height="514" alt="image" src="https://github.com/user-attachments/assets/d8d81a78-7726-4494-b047-d7910d1fa8fc" /> <br>


### Test User

Login sebagai `rzl` (admin):

```bash
mysql -u rzl -p
```

```sql
USE demo;
INSERT INTO transaction (name, amount) VALUES ('test', 100.00); -- ✅ Berhasil
SELECT * FROM transaction;                                      -- ✅ Berhasil
UPDATE transaction SET amount = 200.00 WHERE id = 1;            -- ✅ Berhasil
DELETE FROM transaction WHERE id = 1;                           -- ✅ Berhasil
```
<img width="975" height="700" alt="image" src="https://github.com/user-attachments/assets/6b6b40a6-afad-4594-8376-ae3a9cdaf0e7" />
<br><br>

Login sebagai `guest`:

```bash
mysql -u guest -p
```

```sql
USE demo;
SELECT * FROM transaction;                                      -- ✅ Berhasil
INSERT INTO transaction (name, amount) VALUES ('test', 100.00); -- ❌ Access Denied
UPDATE transaction SET amount = 200.00 WHERE id = 1;            -- ❌ Access Denied
DELETE FROM transaction WHERE id = 1;                           -- ❌ Access Denied
```
<img width="975" height="599" alt="image" src="https://github.com/user-attachments/assets/b3deabcf-7377-4bf8-afb2-f7ccc55a1b44" /><br>

---

## 5. Remote User (MySQL Client)

Install MySQL client di Windows:

```powershell
winget install Oracle.MySQL
```

Tambahkan ke PATH:

```powershell
$env:PATH += ";C:\Program Files\MySQL\MySQL Server 8.4\bin"
```

Buka UFW di server:

```BASH
sudo ufw allow 3306
```
<img width="975" height="494" alt="image" src="https://github.com/user-attachments/assets/03e7a3bf-a208-444f-a8a1-daca0d7c6992" />
<br><br>

Koneksi ke database server dari lokal:

```powershell
mysql -u rzl -p -h 202.10.34.220
```
<img width="975" height="494" alt="image" src="https://github.com/user-attachments/assets/01be3cb2-ae6d-4d89-b08f-34d767347ce0" /> <br>

---

## 6. Deploy Wayshub-Backend

### Clone Repository

```bash
git clone https://github.com/dumbwaysdev/wayshub-backend.git
cd wayshub-backend
```

### Gunakan Node Version 12

```bash
nvm install 12
nvm use 12
```

### Konfigurasi Database

Edit file `config/config.json`:

```json
{
  "development": {
    "username": "rzl",
    "password": "password_user",
    "database": "wayshub",
    "host": "127.0.0.1",
    "dialect": "mysql"
  }
}
```
<img width="624" height="372" alt="dumbways-01-01" src="https://github.com/user-attachments/assets/c3861cc3-b6c6-47a8-b0af-08894e445a6d" />
<br>

### Install Dependencies & Sequelize CLI

```bash
npm install
npm install -g sequelize-cli
```
<img width="975" height="264" alt="image" src="https://github.com/user-attachments/assets/a63998f5-51da-4d22-b785-b166592af309" />
<br>
<img width="975" height="759" alt="image" src="https://github.com/user-attachments/assets/988c54cf-a3d5-42e1-9d69-dbd7fa694611" />
<br>

### Jalankan Migration

```bash
sequelize db:migrate
```
<img width="975" height="641" alt="image" src="https://github.com/user-attachments/assets/30b14d0f-1b63-408e-8b0d-8e85f2f174ac" />
<br>

### Jalankan PM2 init
```bash
pm2 init
```

### Edit file config
```bash
sudo nano ecosystem.config.js
```

Ganti menjadi seperti ini :

```bash                                                                     
module.exports = {
  apps : [{
    script: 'npm start',
    watch: '.'
  }],

  deploy : {
    production : {
      user : 'SSH_USERNAME',
      host : 'SSH_HOSTMACHINE',
      ref  : 'origin/master',
      repo : 'GIT_REPOSITORY',
      path : 'DESTINATION_PATH',
      'pre-deploy-local': '',
      'post-deploy' : 'npm install && pm2 reload ecosystem.config.js --env production',
      'pre-setup': ''
    }
  }
};
```
<img width="849" height="410" alt="image" src="https://github.com/user-attachments/assets/48dc57dd-f65f-4ef3-a7db-129743f569cf" /><br>
(Gambar baru diambil)

### Deploy dengan PM2

```bash
pm2 start
```
<img width="975" height="585" alt="image" src="https://github.com/user-attachments/assets/d19cca4c-74c4-4c37-978b-7e26d2a9067c" />
<br>

---

## 7. Deploy Wayshub-Frontend

### Clone Repository

```bash
git clone https://github.com/dumbwaysdev/wayshub-frontend.git
cd wayshub-frontend
```

### Gunakan Node Version 12

```bash
nvm use 12
```

### Konfigurasi API URL

Edit file `src/config/api.js`:

```javascript
const API = axios.create({
    baseURL: "http://202.10.34.220/api/v1"
});
```
<img width="731" height="391" alt="image" src="https://github.com/user-attachments/assets/95867515-2b84-49f3-b332-1c90763bd411" /><br>
(Gambar baru diambil)

### Install Dependencies

```bash
npm install
```

### Jalankan PM2 init
```bash
pm2 init
```

### Edit file config
```bash
sudo nano ecosystem.config.js
```

Ganti menjadi seperti ini:
```bash
module.exports = {
  apps : [{
    name: 'frontend',
    script: 'npm start',
    watch: '.'
  }],

  deploy : {
    production : {
      user : 'SSH_USERNAME',
      host : 'SSH_HOSTMACHINE',
      ref  : 'origin/master',
      repo : 'GIT_REPOSITORY',
      path : 'DESTINATION_PATH',
      'pre-deploy-local': '',
      'post-deploy' : 'npm install && pm2 reload ecosystem.config.js --env production',
      'pre-setup': ''
    }
  }
};
```
<img width="917" height="458" alt="image" src="https://github.com/user-attachments/assets/6b9d3781-97b3-4cb7-ae17-a0fc6646356f" /><br>
(Gambar baru diambil

### Deploy dengan PM2

```bash
pm2 start npm
```

---

## 8. Gateway - Nginx Reverse Proxy

### Install Nginx

```bash
sudo apt install nginx -y
```

### Konfigurasi Nginx

```bash
sudo nano /etc/nginx/sites-available/wayshub
```

Isi dengan konfigurasi berikut:

```nginx
server {
    listen 80;
    server_name 202.10.34.220;

    # Frontend
    location / {
        proxy_pass http://localhost:3000;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }

    # Backend
    location /api {
        proxy_pass http://localhost:5000;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }
}
```

### Aktifkan Konfigurasi

```bash
sudo ln -s /etc/nginx/sites-available/wayshub /etc/nginx/sites-enabled/
sudo nginx -t
sudo systemctl reload nginx
```

### Hasil

Akses frontend melalui browser: `http://202.10.34.220`

Frontend dan backend terhubung melalui Nginx reverse proxy. ✅

---

## Ringkasan

| Task | Status |
|------|--------|
| Buat user baru | ✅ |
| SSH key login (tanpa password) | ✅ |
| Deploy MySQL | ✅ |
| Secure installation | ✅ |
| Role Based (admin & guest) | ✅ |
| Remote User (mysql-client) | ✅ |
| Deploy Wayshub-Backend | ✅ |
| Deploy Wayshub-Frontend | ✅ |
| Gateway (Nginx Reverse Proxy) | ✅ |
