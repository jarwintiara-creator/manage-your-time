Manage Your Time - Aplikasi Manajemen Kegiatan & To-Do List Asrama

Proyek aplikasi web ini dirancang dan dibangun untuk memenuhi tugas ujian menjelang Penilaian Sumatif Akhir Semester (PSAS).

Latar Belakang Masalah

Penghuni asrama memiliki rutinitas dan kewajiban harian yang padat—mulai dari jadwal piket kebersihan kamar/selasar, tugas belajar dan persiapan ujian, hingga kegiatan ibadah bersama. Tanpa pencatatan yang terstruktur serta pengingat batas waktu yang jelas, penghuni asrama rentan melewatkan kewajiban mereka. Manage Your Time hadir sebagai solusi pencatatan mandiri berbasis web untuk meningkatkan kedisiplinan manajemen waktu di lingkungan asrama.

Fitur Utama

Sistem Autentikasi Akun Pribadi: Setiap penghuni asrama dapat mendaftarkan akun baru beserta identitas nomor kamar/gedungnya masing-masing. Data kegiatan tersimpan secara terpisah dan aman untuk setiap akun pengguna.

Pencatatan Kegiatan & Kategori: Pengguna dapat menambahkan daftar aktivitas atau tugas dan mengelompokkannya ke dalam kategori (Piket & Kebersihan, Belajar & Ujian, Ibadah & Kegiatan, dan Pribadi).

Tenggat Waktu Mandiri (Deadline): Pengguna menentukan tanggal dan jam batas waktu penyelesaian secara mandiri. Tugas otomatis diurutkan berdasarkan tenggat waktu terdekat.

Sistem Notifikasi & Peringatan Tenggat Waktu:

Browser Notification: Mengirimkan notifikasi sistem secara otomatis saat batas waktu tugas telah tercapai (apabila pengguna memberikan izin notifikasi).

In-App Alert Banner: Menampilkan banner peringatan merah di bagian atas dashboard untuk tugas yang belum selesai dan sudah melewati tenggat waktu (Terlewat!).

Statistik & Filter Progres: Menampilkan ringkasan jumlah total kegiatan, kegiatan yang belum selesai, dan kegiatan yang telah diselesaikan, dilengkapi tombol filter status.

Struktur Proyek

manage-your-time/
├── index.html      # Struktur halaman utama (Login, Daftar Akun, & Dashboard Tugas)
├── style.css       # Desain antarmuka modern & responsif (Mobile & Desktop)
├── app.js          # Logika autentikasi, manajemen tugas, penyimpanan lokal, & notifikasi
└── README.md       # Dokumentasi proyek untuk penilaian ujian PSAS


Teknologi yang Digunakan

HTML5: Struktur semantik halaman web.

CSS3: Tata letak responsif menggunakan Flexbox dan CSS Grid dengan desain modern.

JavaScript (ES6+): Manipulasi DOM, sistem autentikasi, pengurutan tenggat waktu, Browser Notification API, dan penyimpanan data berbasis localStorage.

GitHub Pages: Layanan hosting statis untuk publikasi aplikasi web.

Cara Menjalankan & Menguji Aplikasi

Buka tautan GitHub Pages dari repository ini melalui browser (Chrome/Edge/Safari).

Klik tab Daftar Akun, masukkan nama lengkap, nomor kamar, username, dan kata sandi, lalu klik Buat Akun Baru.

Masuk menggunakan akun yang telah dibuat pada tab Masuk.

Klik tombol Aktifkan Notif di pojok kanan atas dashboard dan pilih Izinkan (Allow) agar pengingat sistem dapat muncul.

Tambahkan kegiatan baru beserta tanggal dan jam tenggat waktunya. Saat batas waktu tercapai, sistem akan memunculkan notifikasi pengingat secara otomatis.
