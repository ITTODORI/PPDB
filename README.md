PPDB/
├── app/
│   ├── Console/                      # Default Laravel untuk CLI
│   ├── Http/
│   │   ├── Controllers/
│   │   │   ├── Auth/                 # Login, Register, Logout
│   │   │   ├── PublicController.php  # Beranda, Bantuan
│   │   │   ├── Student/              # Dashboard, Biodata, Nilai, Berkas, Cetak
│   │   │   └── Admin/                # Verifikasi, Kuota, Seleksi
│   │   └── Middleware/               # Pembatas jalur (e.g., IsAdmin, IsStudent)
│   ├── Models/
│   │   ├── User.php                  # Autentikasi (Admin & Akun Siswa)
│   │   ├── Student.php               # Data profil siswa (Relasi 1:1 dengan User)
│   │   ├── Score.php                 # Nilai rapor/ijazah (Relasi 1:1 atau 1:M dengan Student)
│   │   └── Document.php              # Berkas upload (Relasi 1:M dengan Student)
│   └── Services/                     # Bagus! Untuk rumus seleksi otomatis (e.g., SeleksiService.php)
├── config/
│   └── database.php                  # Konfigurasi MySQL
├── database/
│   └── migrations/                   # File cetakan tabel MySQL
├── resources/                        # <--- Lokasi standar untuk aset dan views
│   └── views/
│       ├── layouts/                  # master.blade.php, sidebar.blade.php
│       ├── public/                   # beranda.blade.php, bantuan.blade.php
│       ├── student/                  # dashboard.blade.php, biodata.blade.php, dll.
│       └── admin/                    # seleksi.blade.php, verifikasi.blade.php, dll.
├── routes/
│   └── web.php                       # Definisi URL routing
└── storage/
    └── app/
        ├── public/uploads/           # Berkas pendaftar (ijazah, kk, dll)
        └── exports/                  # PDF Kartu Ujian / Bukti Daftar
