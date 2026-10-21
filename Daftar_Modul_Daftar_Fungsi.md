# DAFTAR MODUL DAN DAFTAR FUNGSI SISTEM
## (MODULAR ARCHITECTURE & FUNCTIONAL SPECIFICATION)
### SUBSISTEM RELAWAN DAN OPERASIONAL ACARA

**Proyek Klaster Operasional — Baitulmal Tazkia**  
**Program Studi Sistem Informasi, STMIK Tazkia (Gasal 2026/2027)**  
**Kelompok 11**

| Anggota Kelompok | NIM |
| :--- | :--- |
| Abrar Danendra Kurnia Putra | 251572010002 |
| Alyatur Rofiah | 251572010010 |
| Mutia Carinna | 251552010028 |

---

## 1. Pendahuluan

Dokumen ini memuat rincian Daftar Modul dan Daftar Fungsi perangkat lunak untuk subsistem Relawan dan Operasional Acara Baitulmal Tazkia. Seluruh spesifikasi modul dirancang berdasarkan arsitektur berlapis (*Controller-Service-Repository*) dan berpedoman pada ruang lingkup proyek dalam *Workbook* Klaster Operasional STMIK Tazkia.

---

## 2. Daftar Modul Utama Sistem (System Modules)

Subsistem Relawan dan Operasional Acara dibagi menjadi beberapa modul fungsional utama yang berdiri secara independen namun saling terintegrasi:

| No | Nama Modul | Deskripsi & Tanggung Jawab Modul |
| :--- | :--- | :--- |
| **1** | **Modul Autentikasi & Otorisasi** | Mengelola manajemen akun pengguna, *login*, *hashing* kata sandi (Argon2/Bcrypt), penerbitan token JWT bermasa berlaku wajar, serta kontrol akses berbasis peran (RBAC). |
| **2** | **Modul Manajemen Relawan** | Mengelola pendaftaran relawan, input profil, keahlian khusus, wilayah domisili, dan rentang waktu ketersediaan berulang mingguan (*recurring weekly availability*). |
| **3** | **Modul Penjadwalan & Penugasan** | Mengelola penyusunan jadwal penugasan relawan per acara/sesi, lengkap dengan algoritma validasi otomatis untuk mencegah bentrokan jadwal (*schedule conflict check*). |
| **4** | **Modul Pencatatan Kehadiran (Attendance)** | Menyediakan antarmuka *mobile-friendly* bagi petugas lapangan untuk mencatat kehadiran aktual (*check-in/check-out*) relawan di lokasi acara. |
| **5** | **Modul Rekapitulasi & Reward** | Menghitung akumulasi jam kerelawanan riil secara otomatis berdasarkan data kehadiran, serta menyajikan laporan rekapitulasi untuk keperluan penghargaan. |

---

## 3. Daftar Fungsi Sistem (System Functions)

Berikut adalah rincian fungsi-fungsi utama yang tersedia di dalam masing-masing modul perangkat lunak:

| Kode Fungsi | Nama Modul | Nama Fungsi | Deskripsi & Perilaku Fungsional |
| :--- | :--- | :--- | :--- |
| **FN-AUTH-01** | Autentikasi | Registrasi Akun Pengguna | Mendaftarkan akun baru dengan validasi format *email* dan enkripsi kata sandi menggunakan *hashing* yang aman. |
| **FN-AUTH-02** | Autentikasi | Login & Autentikasi Token | Memverifikasi kredensial pengguna dan menerbitkan akses token bermasa berlaku terbatas. |
| **FN-VOL-01** | Manajemen Relawan | Input & Perbarui Profil Relawan | Menyimpan data identitas, keahlian khusus, dan wilayah domisili relawan dengan perlindungan data sesuai UU PDP. |
| **FN-VOL-02** | Manajemen Relawan | Manajemen Ketersediaan Waktu | Mengatur rentang waktu ketersediaan berulang mingguan relawan sebagai acuan pencocokan penugasan. |
| **FN-SCH-01** | Penjadwalan & Penugasan | Penyusunan Jadwal Sesi Acara | Memungkinkan koordinator memilih acara, sesi, dan menyaring relawan yang memiliki ketersediaan serta keahlian sesuai. |
| **FN-SCH-02** | Penjadwalan & Penugasan | Validasi Bentrokan Jadwal (*Conflict Check*) | Secara otomatis mendeteksi apakah relawan memiliki penugasan ganda di waktu yang sama dan menolak jika terjadi bentrokan. |
| **FN-ATT-01** | Kehadiran | Pencatatan Kehadiran Lapangan (*Check-In*) | Mencatat kehadiran relawan secara riil di lokasi acara melalui antarmuka web peramban seluler. |
| **FN-ATT-02** | Kehadiran | Pembaruan Status Ketidakhadiran | Mencatat status pembatalan atau ketidakhadiran relawan di saat-saat terakhir untuk proses pencarian pengganti. |
| **FN-REP-01** | Rekapitulasi & Reward | Perhitungan Jam Kerelawanan | Menghitung akumulasi total jam kerja relawan berdasarkan data kehadiran aktual di lapangan (bukan dari jadwal). |
| **FN-REP-02** | Rekapitulasi & Reward | Laporan Rekapitulasi & Penghargaan | Menyajikan laporan rekapitulasi jam kerja per relawan untuk keperluan evaluasi apresiasi dan penghargaan manajemen. |
