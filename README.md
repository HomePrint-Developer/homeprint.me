# Home Print 
**Version 2.1.0 - 2026 Release**

[![Live Demo](https://img.shields.io/badge/Live_Demo-Buka_Aplikasi-cyan?style=for-the-badge)](http://homeprint-developer.github.io/homeprint.me/)

Home Print adalah platform layanan cetak dokumen dan foto berbasis web. Aplikasi ini dirancang untuk memberikan kemudahan bagi pengguna dalam menentukan spesifikasi cetak secara interaktif, menghitung biaya transaksi dan ongkos kirim secara otomatis menggunakan integrasi peta, serta memfasilitasi komunikasi pesanan langsung ke WhatsApp admin.

Platform ini juga dilengkapi dengan halaman dasbor khusus admin untuk manajemen stok barang, varian kertas foto, penyedia alat cerdas pembuat dokumen (CV/Lamaran & Nota Resmi otomatis), serta pengelolaan kritik dan saran dari pelanggan dengan sistem keamanan maksimal untuk skala aplikasi web statis.

---

## Informasi Versi

Aplikasi ini menggunakan standar *Semantic Versioning* (SemVer). Saat ini sistem berada pada **Versi 2.1.0 (2026 Release)**, dengan rincian makna angka sebagai berikut:

- **2 (Major)**: Menggambarkan perombakan besar-besaran pada antarmuka UI/UX (migrasi dari halaman terpisah menjadi arsitektur _Pop-Up Modal_ bergaya SPA), penambahan fitur inti tingkat lanjut (Integrasi API Peta Interaktif & Mesin Generator PDF), serta perombakan struktur keamanan sistem.
- **1 (Minor)**: Menandakan pembaruan fitur baru secara substansial, mencakup penambahan sistem notifikasi rilis (What's New Modal) dan integrasi form dinamis interaktif untuk pengisian data CV serta Surat Lamaran secara *real-time*.
- **0 (Patch)**: Menunjukkan bahwa belum ada tambalan perbaikan _bug_ tambahan yang diaplikasikan setelah rilis minor ini diluncurkan.

---

## Fitur Utama

### Antarmuka Pelanggan

- **Tema Gelap & Terang (Dark/Light Mode)**: Dukungan tema visual yang dinamis dan elegan yang tersimpan pada memori _Local Storage_, dilengkapi dengan transisi _smooth_ dan _bubble tooltip_ interaktif untuk memandu pengguna baru.
- **Sistem Notifikasi Rilis (What's New)**: Terintegrasi dengan tombol lonceng berindikator titik merah (*red dot*) di halaman utama untuk menginformasikan pembaruan fitur-fitur unggulan sistem kepada pelanggan secara rapi melalui modal interaktif.
- **UI/UX & Responsivitas Modern**: Menggunakan arsitektur layout responsif (adaptasi 1-kolom di *mobile* dan Grid 2-kolom di desktop) untuk memaksimalkan ruang layar, serta dukungan _Backdrop Click-to-Close_ pada seluruh _Pop-Up Modal_.
- **Pilihan Jasa Kondisional & Otomatisasi Form**: Pemisahan alur spesifikasi terstruktur untuk *Print*, *Foto*, dan *Jasa Lainnya*. Khusus layanan CV & Lamaran, kini dilengkapi form dinamis dengan validasi input otomatis (Email & Nomor HP) yang mengekstraksi riwayat data menjadi manifes rapi ke WhatsApp. 
- **Integrasi Peta Interaktif (Multi-Layer)**: Menggunakan Leaflet.js yang dikombinasikan dengan _Google Maps Tile_ (mendukung mode **Peta Dasar** dan **Satelit**) untuk menentukan titik koordinat pengantaran. Dilengkapi fitur deteksi GPS otomatis (Lokasi Saya) dan tombol kendali yang rapi.
- **Kalkulasi Biaya & Ongkir Cerdas**: Menghitung subtotal cetak/jasa secara *real-time* (termasuk kuantitas lembar & produk toko tambahan) dan ongkos kirim berbasis jarak mengemudi riil (OSRM API) dengan tarif **Rp3.000/KM** serta pengunci **Tarif Minimum Rp5.000** untuk proteksi jarak dekat.
- **Gateway QRIS & WhatsApp**: Menyediakan instruksi QRIS instan dan otomatis menyusun teks manifes pesanan (nota digital) berformat rapi untuk dikirim ke WhatsApp admin.
- **Kolom Kritik & Saran Anonim**: Halaman umpan balik pelanggan untuk mengirim masukan secara langsung tanpa nomor telepon, menjamin 100% bebas dari risiko kebocoran privasi data.

### Panel & Alat Admin (Admin Tools)

- **Autentikasi Gerbang Login Terpadu**: Proteksi halaman internal dari serangan _Bypass Direct URL Access_ menggunakan _Session Storage_ JavaScript. Login terintegrasi di halaman utama menggunakan antarmuka _Pop-Up Modal_.
- **Manajemen Stok & Kertas Terpusat (SPA Feel)**: Memantau, menambah, mengubah, dan menghapus data stok ATK/varian kertas. Interaksi form terpusat melalui _Modal/Pop-Up_ elegan tanpa memuat ulang (reload) halaman. 
- **Dynamic Layout & Action Buttons**: Area kerja Dasbor dioptimalkan agar lebih lebar (*Wide Container*) di mode Desktop. Tombol aksi utama (Tambah Data & Alat) bersifat **Dinamis** — melayang (*Fixed Bottom*) di layar sentuh Mobile, namun otomatis masuk bergabung dengan elegan ke dalam panel data saat dibuka via Desktop.
- **Pusat Alat Admin (PDF Generators)**:
  1. **Dokumen Generator Engine**: Mesin otomatis untuk membuat CV dan Surat Lamaran Kerja (Margin Narrow A4) yang langsung dapat diekspor ke PDF menggunakan `html2pdf.js`.
  2. **Generator Nota Resmi**: Pencetak nota kosong ber-seri massal (cth: `HP-000001`). Sistem melacak seri terakhir di Firebase, menanamkan teks ke _template_ PDF menggunakan `pdf-lib`, dan merekam riwayatnya kembali ke server.
- **Manajemen Feedback Interaktif & Aman**:
  - Tampilan tabel dengan sistem paginasi (maks 10 data per halaman).
  - Fitur hapus data terproteksi aturan _Firebase Rules_ (`!data.exists() || !newData.exists()`).
  - **Interactive Masking Mode**: Fitur ikon mata (_Eye Toggle_) untuk menyamarkan nama pelanggan (contoh: `Zainal Ilmi` menjadi `Z***** I***`) guna mencegah kebocoran privasi dari pandangan orang lain di sekitar (Shoulder Surfing).

---

## Keamanan & Validasi Database (Firebase Rules)

Database dilindungi dari serangan _Database Deface_ dan _Malicious Code Injection_ menggunakan aturan validasi ketat level server:

1. **Type Checking**: Node harga dan sisa stok dikunci wajib menggunakan tipe numerik `.isNumber()`.
2. **Length Restrict**: Panjang karakter nama dibatasi maksimal 50 karakter dan pesan masukan maksimal 500 karakter untuk mencegah ledakan kuota database (_Buffer Overflow_).
3. **XSS Protection**: Logika penayangan data pada halaman admin bermigrasi penuh menggunakan properti `.textContent` murni, sehingga skrip HTML berbahaya (`<script>`) yang sengaja disisipkan peretas akan dicetak sebagai teks biasa dan gagal dieksekusi oleh browser.

---

## Teknologi yang Digunakan

- **Frontend**: HTML5, Tailwind CSS (Desain Modern, Glassmorphism, Responsive & Dark Mode)
- **Backend & Database**: Firebase Realtime Database (Firebase SDK v12.15.0)
- **Peta & Geokoding**: Leaflet.js, OpenStreetMap, OSRM (Open Source Routing Machine) API, Google Maps Tile Layer
- **Pemrosesan File PDF**:
  - `html2pdf.js` (Rendering halaman web HTML ke file PDF)
  - `pdf-lib` (Manipulasi, penambahan teks dinamis, dan modifikasi template PDF asli)

---

## Struktur File Proyek

- `index.html`: Halaman beranda utama pelanggan yang memuat akses navigasi, pemilihan jenis pesanan, login modal admin, serta notifikasi fitur unggulan (*What's New*).
- `pesan.html`: Formulir layanan percetakan dokumen & cetak foto (terintegrasi API Peta).
- `jasa.html`: Formulir layanan jasa (CV, Surat Lamaran, Ketik) yang dilengkapi dengan generator pop-up input data dinamis untuk menyusun profil pengguna, serta terhubung ke pembelian produk fisik toko dan Peta Ongkir.
- `feedback.html`: Halaman untuk pelanggan mengirim masukan/saran anonim.
- `index-admin.html`: Hub dasbor utama admin (Manajemen stok barang, kertas, dan menu alat).
- `feedback-admin.html`: Panel administrasi untuk membaca dan mengelola masukan/feedback.
- `generate-file.html`: Alat pembuat file CV & Lamaran internal (Dokumen Generator).
- `nota-kosong.html`: Alat pencetak seri nota fisik otomatis secara massal.
- `close.html` / `maintenance.html`: Halaman pengalihan sementara (Hold Traffic) saat layanan sedang libur/perbaikan.

*(Catatan: File usang seperti `login.html` dan `add_stok.html` telah dilebur seluruhnya ke dalam bentuk interaksi Pop-Up Modal modern).*

---

## Konfigurasi dan Instalasi

Karena aplikasi ini dibangun menggunakan arsitektur web statis (*Client-Side Rendering*), proses instalasinya sangat mudah dan tidak memerlukan Node.js atau pengaturan *backend* khusus.

1. **Klon Repositori**
   `git clone https://github.com/HomePrint-Developer/homeprint.me.git`

2. **Buka Direktori Proyek**
   `cd homeprint.me`

3. **Jalankan Aplikasi**
   - Anda dapat langsung membuka file `index.html` di browser pilihan Anda.
   - **Rekomendasi:** Untuk pengalaman pengembangan terbaik dan mencegah isu CORS (terutama saat memuat modul Firebase), jalankan proyek menggunakan ekstensi **Live Server** di VS Code.