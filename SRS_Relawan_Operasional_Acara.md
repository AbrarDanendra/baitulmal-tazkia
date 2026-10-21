# SOFTWARE REQUIREMENTS SPECIFICATION (SRS)
## SUBSISTEM RELAWAN DAN OPERASIONAL ACARA
Berdasarkan Standar ISO/IEC/IEEE 29148:2018

**Baitulmal Tazkia — Relawan dan Operasional (Kelompok 11)**  
**Program Studi Sistem Informasi, STMIK Tazkia (Gasal 2026/2027)**

| Anggota Kelompok | NIM |
| :--- | :--- |
| Abrar Danendra Kurnia Putra | 251572010002 |
| Alyatur Rofiah | 251572010010 |
| Mutia Carinna | 251552010028 |

---

## 1. Pendahuluan

Dokumen Spesifikasi Kebutuhan Perangkat Lunak (*Software Requirements Specification* / SRS) ini merinci kebutuhan fungsional dan nonfungsional untuk subsistem Relawan dan Operasional Acara pada Baitulmal Tazkia. Dokumen ini disusun berdasarkan panduan Workbook Proyek Klaster Operasional STMIK Tazkia (Gasal 2026/2027) dan berpedoman pada standar ISO/IEC/IEEE 29148.

### 1.1 Tujuan Penulisan
Tujuan dari SRS ini adalah untuk memberikan deskripsi yang komprehensif mengenai ruang lingkup, arsitektur, batasan, serta seluruh *use case* dan *user story* yang harus dipenuhi oleh subsistem Relawan dan Operasional Acara. Dokumen ini berfungsi sebagai acuan utama bagi pengembang (mahasiswa), dosen pengampu, tim pendamping, serta klien (Baitulmal Tazkia) dalam proses verifikasi dan validasi.

### 1.2 Lingkup Proyek
Baitulmal Tazkia adalah lembaga pengelola zakat, infak, sedekah, dan wakaf yang berkantor di Sentul, Bogor, dengan platform penggalangan dana utama `sahabatkebaikan.com`. Penghimpunan dana sangat bergantung pada relawan dalam acara maupun *booth* di tempat umum. Subsistem ini dirancang untuk mengotomatisasi proses pengelolaan relawan dan kebutuhan operasional acara.

> **CATATAN PENTING KEPATUHAN DATA**  
> Data donatur dan relawan memuat identitas serta riwayat partisipasi, sedangkan data penerima manfaat memuat kondisi ekonomi dan kesehatan. Seluruh data ini dikategorikan sebagai Data Pribadi Spesifik berdasarkan **Undang-Undang Nomor 27 Tahun 2022 tentang Pelindungan Data Pribadi (UU PDP)**. Pengembangan wajib menggunakan data sintetis sebelum perjanjian kerahasiaan dan pemrosesan data ditandatangani.

---

## 2. Deskripsi Umum Sistem

### 2.1 Perspektif Produk
Subsistem Relawan dan Operasional Acara merupakan salah satu dari 12 subsistem independen yang dikembangkan untuk mereformasi sistem manual Baitulmal Tazkia (pembukuan *spreadsheet*, rekonsiliasi manual, dan koordinasi via grup WhatsApp). Subsistem ini berdiri sendiri lengkap dengan *database*, aplikasi web, dan *deployment* tersendiri, serta terintegrasi dengan subsistem lain melalui kontrak API formal.

### 2.2 Kelas dan Karakteristik Pengguna
Berdasarkan analisis pemangku kepentingan awal, pengguna sistem ini dikelompokkan ke dalam beberapa peran utama:
1. **Koordinator Relawan:** Bertanggung jawab mengelola *database* relawan, memvalidasi keahlian dan wilayah, menyusun jadwal penugasan per acara/sesi, serta memantau kehadiran relawan di lapangan.
2. **Staf Operasional Acara:** Bertanggung jawab menyusun daftar perlengkapan, menentukan penanggung jawab sesi acara, dan mengelola kebutuhan operasional di lokasi acara.
3. **Relawan:** Mendaftarkan diri, memilih jadwal kesediaan waktu, melihat penugasan penugasan, serta memantau perolehan jam kerelawanan dan penghargaan.
4. **Manajemen Baitulmal Tazkia:** Mengakses data rekapitulasi jam kerelawanan dan laporan operasional untuk keperluan audit dan evaluasi manajemen.

### 2.3 Batasan Lingkup (In-Scope & Out-of-Scope)
* **Lingkup Utama (In-Scope):**
  * Pendaftaran relawan beserta keahlian, wilayah, dan kesediaan waktu berulang mingguan.
  * Penjadwalan penugasan relawan per acara/sesi, termasuk deteksi bentrokan jadwal dan penggantian saat pembatalan.
  * Pencatatan kehadiran relawan pada setiap sesi acara secara *real-time* di lokasi.
  * Rekapitulasi jam kerelawanan per relawan sebagai dasar pemberian penghargaan.
  * Pengelolaan daftar perlengkapan acara beserta penanggung jawab tiap sesi.
* **Batasan Luar (Out-of-Scope):**
  * Pencatatan donasi di *booth* dikerjakan oleh kelompok Penerimaan Donasi Luring.
  * Penetapan kelayakan dan pendampingan penerima manfaat dikerjakan oleh kelompok Pendataan & Verifikasi.
  * Penggajian dan kepegawaian staf tetap berada di luar lingkup (subsistem hanya mengelola relawan).
  * Pengadaan fisik perlengkapan berada di luar lingkup (subsistem hanya mencatat kebutuhan dan penanggung jawab).

---

## 3. Ketergantungan dan Integrasi Sistem

### 3.1 Ketergantungan Hulu dan Hilir
Subsistem ini memiliki ketergantungan arsitektural dengan subsistem lain di dalam ekosistem Baitulmal Tazkia:

| Arah | Pihak Terkait | Jenis Hubungan | Catatan Integrasi |
| :--- | :--- | :--- | :--- |
| **Hulu** | Penerimaan Donasi Luring | Kontrak API | Menyediakan data penugasan petugas *booth* per sesi sehingga kehadiran petugas tidak dicatat dua kali. |

*Catatan Teknis:* Subsistem ini tidak memiliki kelompok hilir. Kontrak API menggunakan format OpenAPI dengan autentikasi token berbasis peran, memastikan tidak ada *foreign key* yang melintasi batas basis data antar subsistem.

---

## 4. Kebutuhan Fungsional (Functional Requirements)

Berikut adalah rincian *Use Case* Utama yang dikelola oleh subsistem Relawan dan Operasional Acara:

### 4.1 UC-01: Manajemen Profil dan Kesediaan Relawan
* **Aktor Utama:** Koordinator Relawan atau Relawan mandiri.
* **Deskripsi Singkat:** Relawan mendaftarkan diri, melengkapi data keahlian, wilayah domisili, serta rentang waktu ketersediaan berulang (*weekly recurring availability*).
* **Alur Utama:** 
  1. Pengguna mengakses menu pendaftaran/profil relawan.
  2. Pengguna memasukkan identitas diri, keahlian khusus, wilayah, dan ketersediaan waktu.
  3. Sistem memvalidasi kelengkapan data dan format waktu.
  4. Sistem menyimpan data relawan ke dalam *database* dengan enkripsi data pribadi sesuai UU PDP.

### 4.2 UC-02: Penjadwalan Penugasan Acara dan Sesi
* **Aktor Utama:** Koordinator Relawan.
* **Deskripsi Singkat:** Menyusun jadwal penugasan relawan untuk setiap acara dan sesi, serta melakukan pengecekan bentrokan jadwal secara otomatis.
* **Alur Utama:** 
  1. Koordinator memilih acara dan sesi yang membutuhkan relawan.
  2. Sistem menampilkan daftar relawan yang memiliki kesediaan waktu dan keahlian yang cocok.
  3. Koordinator menugaskan relawan pada sesi tersebut.
  4. Sistem memeriksa apakah relawan memiliki jadwal ganda (bentrok) pada waktu yang sama. Jika bentrok, sistem menolak penugasan.
  5. Penugasan berhasil disimpan dan dikirimkan ke notifikasi relawan.

### 4.3 UC-03: Pencatatan Kehadiran (*Attendance Tracking*) di Lokasi Acara
* **Aktor Utama:** Koordinator Relawan / Petugas Lapangan.
* **Deskripsi Singkat:** Mencatat kehadiran relawan yang hadir di lokasi acara melalui antarmuka *mobile-friendly*.
* **Alur Utama:** 
  1. Petugas membuka aplikasi di lokasi acara.
  2. Petugas memindai atau mencentang daftar relawan yang hadir pada sesi tersebut.
  3. Sistem mencatat waktu *check-in* aktual.
  4. Jam kerelawanan dihitung berdasarkan kehadiran riil, bukan semata-mata dari jadwal penugasan.

### 4.4 UC-04: Rekapitulasi Jam Kerelawanan dan Penghargaan
* **Aktor Utama:** Manajemen / Koordinator Relawan.
* **Deskripsi Singkat:** Merekapitulasi total jam kerelawanan terkumpul per relawan untuk keperluan apresiasi dan penghargaan.

---

## 5. Kebutuhan Nonfungsional (Non-Functional Requirements)

1. **Keamanan & Privasi:** Seluruh kata sandi wajib di-*hash* menggunakan algoritma modern yang aman (seperti Argon2 atau bcrypt). Kredensial produksi dan *secret keys* dilarang keras disimpan di dalam *repository* kode (harus melalui *environment variables*). Kontrol akses berbasis peran (RBAC) diterapkan secara terpusat untuk melindungi data pribadi spesifik relawan.
2. **Performa & Responsivitas:** Antarmuka web wajib responsif dan dapat diakses dengan lancar melalui *browser* perangkat seluler (*smartphone*), mengingat petugas sering beroperasi di lapangan (mal, masjid, area terbuka). Waktu tanggapan API untuk operasi standar di bawah 500 ms.
3. **Keandalan & Validasi:** Validasi masukan wajib diterapkan di dua sisi (*client-side* pada *browser* dan *server-side* pada *backend*). Permintaan ilegal yang langsung dikirim ke API tanpa melalui antarmuka web wajib ditolak oleh server.
4. **Pengujian & Kualitas Kode:** Kode harus memenuhi prinsip SOLID, memiliki cakupan pengujian unit (*unit test coverage*) yang tinggi pada *service layer* menggunakan *mock repository*, serta dilengkapi *integration test* untuk seluruh *endpoint* berkategori *Must Have* dalam prioritas MoSCoW.

---

## 6. Analisis Risiko Awal (Risk Register)

Berdasarkan Workbook Proyek, berikut adalah identifikasi risiko awal beserta strategi mitigasinya:

| Risiko Potensial | Probabilitas | Dampak | Mitigasi / Rencana Penanganan |
| :--- | :--- | :--- | :--- |
| Satu relawan terjadwal pada dua sesi bersamaan. | Sedang | Tinggi | Sistem menerapkan validasi pencegahan bentrokan jadwal secara otomatis saat penugasan disimpan. |
| Pembatalan relawan di saat terakhir tanpa pemantauan. | Tinggi | Tinggi | Menyediakan fitur cadangan daftar tunggu (*waiting list*) dan pencatatan status pembatalan *real-time*. |
| Kerahasiaan data pribadi relawan terbuka pada pihak yang tidak berwenang. | Rendah | Tinggi | Menerapkan enkripsi dan kontrol otorisasi ketat berbasis peran sesuai UU Nomor 27 Tahun 2022. |
