# 📚 PPDB

Sistem manajemen pendaftaran online untuk penerimaan peserta didik baru dengan fitur verifikasi dokumen otomatis, seleksi siswa, dan dashboard interaktif.

---

## 🎯 Fitur Utama

- ✅ Registrasi & Login untuk Admin dan Siswa
- ✅ Dashboard Siswa untuk mengelola biodata dan berkas
- ✅ Verifikasi dokumen otomatis oleh Admin
- ✅ Sistem seleksi dengan scoring otomatis
- ✅ Cetak kartu ujian dan bukti pendaftaran (PDF)
- ✅ Manajemen nilai rapor/ijazah
- ✅ Upload dan validasi berkas pendaftar

---

## 🏗️ Struktur Project

```
PPDB/
├── app/
│   ├── Console/                           # Default Laravel untuk CLI
│   ├── Http/
│   │   ├── Controllers/
│   │   │   ├── Auth/
│   │   │   │   ├── LoginController.php    # Proses login pengguna
│   │   │   │   └── RegisterController.php # Registrasi akun baru
│   │   │   ├── PublicController.php       # Halaman publik (Beranda, Bantuan)
│   │   │   ├── Student/
│   │   │   │   └── StudentController.php  # Dashboard, Biodata, Nilai, Berkas, Cetak
│   │   │   └── Admin/
│   │   │       └── AdminController.php    # Verifikasi, Kuota, Seleksi
│   │   └── Middleware/
│   │       ├── IsAdmin.php                # Middleware untuk role Admin
│   │       └── IsStudent.php              # Middleware untuk role Student
│   ├── Models/
│   │   ├── User.php                       # Model Autentikasi (Admin & Akun Siswa)
│   │   ├── Student.php                    # Model Data Profil Siswa (Relasi 1:1 dengan User)
│   │   ├── Score.php                      # Model Nilai Rapor/Ijazah (Relasi 1:M dengan Student)
│   │   └── Document.php                   # Model Berkas Upload (Relasi 1:M dengan Student)
│   └── Services/
│       └── SeleksiService.php             # Service untuk logika seleksi otomatis & scoring
├── config/
│   └── database.php                       # Konfigurasi koneksi MySQL
├── database/
│   └── migrations/
│       ├── 2026_05_21_000000_create_users_table.php      # Tabel Users
│       ├── 2026_05_21_000001_create_students_table.php   # Tabel Students
│       ├── 2026_05_21_000002_create_scores_table.php     # Tabel Scores
│       └── 2026_05_21_000003_create_documents_table.php  # Tabel Documents
├── resources/
│   └── views/
│       ├── layouts/
│       │   ├── master.blade.php           # Template utama aplikasi
│       │   └── sidebar.blade.php          # Sidebar navigasi
│       ├── public/
│       │   ├── beranda.blade.php          # Halaman utama publik
│       │   └── bantuan.blade.php          # Halaman FAQ/Bantuan
│       ├── student/
│       │   ├── dashboard.blade.php        # Dashboard siswa
│       │   ├── biodata.blade.php          # Form biodata siswa
│       │   ├── nilai.blade.php            # Daftar nilai/nilai rapor
│       │   ├── berkas.blade.php           # Manajemen berkas upload
│       │   └── cetak.blade.php            # Cetak kartu ujian/bukti daftar
│       └── admin/
│           ├── seleksi.blade.php          # Halaman proses seleksi
│           └── verifikasi.blade.php       # Halaman verifikasi dokumen
├── routes/
│   └── web.php                            # Definisi semua routing aplikasi
├── storage/
│   └── app/
│       ├── public/uploads/                # Direktori upload berkas pendaftar (Ijazah, KK, dll)
│       └── exports/                       # Direktori export PDF (Kartu Ujian, Bukti Daftar)
├── composer.json                          # Dependensi PHP/Laravel
└── README.md                              # Dokumentasi project
```

---

## 🗄️ Database Schema

### Users (Autentikasi)
- `id` - Primary Key
- `name` - Nama lengkap
- `email` - Email unik
- `password` - Password terenkripsi
- `role` - 'admin' atau 'student'

### Students (Profil Siswa)
- `id` - Primary Key
- `user_id` - Foreign Key ke Users (1:1)
- `nis` - Nomor Identitas Siswa
- `nisn` - Nomor Induk Siswa Nasional
- `nama_lengkap` - Nama lengkap siswa
- `jenis_kelamin` - L/P
- `tempat_lahir` - Tempat lahir
- `tanggal_lahir` - Tanggal lahir
- `alamat` - Alamat lengkap
- `telepon` - No. telepon
- `asal_sekolah` - Asal sekolah
- `status_verifikasi` - 'pending', 'verified', 'rejected'
- `status_seleksi` - 'pending', 'lulus', 'tidak_lulus'

### Scores (Nilai Siswa)
- `id` - Primary Key
- `student_id` - Foreign Key ke Students (1:M)
- `nama_mata_pelajaran` - Nama pelajaran
- `nilai_rapor` - Nilai dari rapor
- `semester` - Semester (1-6)

### Documents (Berkas Upload)
- `id` - Primary Key
- `student_id` - Foreign Key ke Students (1:M)
- `jenis_dokumen` - 'ijazah', 'kk', 'akta', 'raport', dll
- `file_path` - Path file yang diupload
- `status_verifikasi` - 'pending', 'approved', 'rejected'
- `catatan_admin` - Catatan dari admin jika ada masalah

---

## 🚀 Instalasi & Setup

### Persyaratan
- PHP >= 8.0
- Composer
- MySQL/MariaDB
- Node.js & npm (untuk frontend)

### Langkah-langkah

1. **Clone Repository**
   ```bash
   git clone https://github.com/ITTODORI/PPDB.git
   cd PPDB
   ```

2. **Install Dependencies**
   ```bash
   composer install
   ```

3. **Setup Environment**
   ```bash
   cp .env.example .env
   php artisan key:generate
   ```

4. **Konfigurasi Database**
   - Edit file `.env` dan sesuaikan database credentials:
     ```
     DB_HOST=localhost
     DB_DATABASE=ppdb
     DB_USERNAME=root
     DB_PASSWORD=
     ```

5. **Migrasi Database**
   ```bash
   php artisan migrate
   ```

6. **Jalankan Aplikasi**
   ```bash
   php artisan serve
   ```

   Aplikasi akan berjalan di `http://localhost:8000`

---

## 👥 Alur Pengguna

### Admin
1. Login dengan akun admin
2. Verifikasi dokumen siswa (Ijazah, KK, Akta, dll)
3. Lihat daftar nilai siswa
4. Jalankan proses seleksi otomatis
5. Manage kuota dan hasil akhir

### Siswa
1. Register akun baru
2. Login ke dashboard
3. Isi biodata lengkap
4. Upload berkas pendaftaran
5. Lihat status verifikasi dan hasil seleksi
6. Cetak kartu ujian / bukti pendaftaran (PDF)

---

## 📋 Stack Teknologi

| Layer | Teknologi |
|-------|-----------|
| **Backend** | Laravel 10.x |
| **Database** | MySQL/MariaDB |
| **Frontend** | Blade Templates + Bootstrap/Tailwind |
| **Authentication** | Laravel Auth |
| **File Storage** | Local Storage (Dapat dikembangkan ke S3) |
| **PDF Export** | TCPDF/DomPDF |

---

## 📞 Support & Kontribusi

Jika ada saran, bug report, atau ingin berkontribusi, silakan buat issue atau pull request.

---

**Created with ❤️ by ITTODORI**
