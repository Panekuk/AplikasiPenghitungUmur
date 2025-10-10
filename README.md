# AplikasiPenghitungUmur
Latihan 2 - Ita Khairati 2310010219

# 🧮 Aplikasi Penghitung Umur (Java GUI)

Aplikasi ini dibuat menggunakan **Java Swing** dan **NetBeans** untuk menghitung umur seseorang, menampilkan hari ulang tahun berikutnya, dan menampilkan peristiwa penting yang terjadi pada tanggal tersebut. Data peristiwa diambil dari API publik dan diterjemahkan ke Bahasa Indonesia.

---

## 🌟 Fitur Utama

- 📅 Memilih tanggal lahir dengan **JDateChooser**.
- 🧮 Menghitung umur secara detail (tahun, bulan, hari).
- 🎂 Menampilkan tanggal dan hari ulang tahun berikutnya.
- 🌍 Menampilkan peristiwa penting pada tanggal yang dipilih, dengan terjemahan ke Bahasa Indonesia.
- ⏹️ Pengambilan data peristiwa bisa dihentikan saat tanggal lahir diganti.
- ❌ Keluar dari aplikasi dengan tombol **Keluar**.

---

## 🏗️ Struktur Kode

Aplikasi ini terdiri dari **dua file utama**:

1. `PenghitungUmurFrame.java`
2. `PenghitungUmurHelper.java`

---

### 1️⃣ PenghitungUmurFrame.java

File ini adalah **GUI utama**. Berfungsi sebagai tampilan interaktif pengguna dan menghubungkan semua logika.  

**Penjelasan bagian penting:**

| Bagian Kode | Fungsi |
|-------------|--------|
| `JDateChooser dateChooserTanggalLahir` | Pilih tanggal lahir |
| `txtUmur` | Menampilkan umur setelah dihitung |
| `txtHariUlangTahunBerikutnya` | Menampilkan hari ulang tahun berikutnya |
| `txtAreaPeristiwa` | Menampilkan peristiwa penting secara baris per baris |
| `btnHitung` | Tombol untuk memulai perhitungan umur dan ambil peristiwa |
| `btnKeluar` | Tombol untuk keluar dari aplikasi |
| `peristiwaThread` | Thread untuk mengambil peristiwa agar GUI tetap responsif |
| `stopFetching` | Flag untuk menghentikan thread lama jika tanggal lahir diganti |

**Alur program saat tombol Hitung ditekan:**

1. Ambil tanggal lahir dari `JDateChooser`.
2. Hitung umur dan tanggal ulang tahun berikutnya menggunakan `PenghitungUmurHelper`.
3. Tampilkan hasil di `txtUmur` dan `txtHariUlangTahunBerikutnya`.
4. Jalankan thread baru untuk mengambil peristiwa baris per baris.
5. Jika tanggal lahir berubah, hentikan thread lama untuk menghindari error dan update GUI.

---

### 2️⃣ PenghitungUmurHelper.java

File ini **mengandung semua logika perhitungan** dan pengambilan data peristiwa.  

**Fungsi utama:**

| Fungsi | Penjelasan |
|--------|------------|
| `hitungUmurDetail(LocalDate lahir, LocalDate sekarang)` | Menghitung umur dalam format "tahun, bulan, hari" menggunakan `Period`. |
| `hariUlangTahunBerikutnya(LocalDate lahir, LocalDate sekarang)` | Menghitung tanggal ulang tahun berikutnya dari tanggal lahir. |
| `getDayOfWeekInIndonesian(LocalDate date)` | Mengubah nama hari menjadi Bahasa Indonesia. |
| `getPeristiwaBarisPerBaris(LocalDate tanggal, JTextArea txtAreaPeristiwa, Supplier<Boolean> shouldStop)` | Mengambil data peristiwa penting dari API `https://byabbe.se/on-this-day/` dan menampilkannya di `JTextArea`. |
| `translateToIndonesian(String text)` | Menerjemahkan deskripsi peristiwa ke Bahasa Indonesia menggunakan API `Lingva.ml`. |

**Alur pengambilan peristiwa:**

1. Buat URL dari tanggal ulang tahun berikutnya.
2. Ambil JSON dari API.
3. Ambil array `events` dari JSON.
4. Iterasi setiap event, terjemahkan ke Bahasa Indonesia, tampilkan di `txtAreaPeristiwa`.
5. Gunakan `Thread.sleep(200)` untuk efek menampilkan peristiwa per baris.
6. Thread dapat dihentikan menggunakan flag `stopFetching`.

---

## 🖼️ Tampilan Aplikasi

- **Tampilan Utama**: Pilih tanggal lahir → klik Hitung → lihat umur & tanggal ulang tahun.
- **Hasil Peristiwa**: Menampilkan peristiwa penting dari tanggal ulang tahun yang dipilih, diterjemahkan ke Bahasa Indonesia.

<div style="display:flex; gap:20px;">
<img src="image/1.PNG" alt="Tampilan Utama" width="300"/>
<img src="image/2.PNG" alt="Hasil Perhitungan" width="300"/>
</div>

---

## 💻 Cara Menjalankan

1. Buka **NetBeans IDE**.
2. Buat project baru → Java Application.
3. Tambahkan **dua file**:
   - `PenghitungUmurFrame.java`
   - `PenghitungUmurHelper.java`
4. Pastikan library **JCalendar** (`com.toedter.calendar.JDateChooser`) sudah ditambahkan.
5. Jalankan project.

---

## ⚡ Catatan Penting

- API peristiwa membutuhkan koneksi internet.
- Pengambilan data bisa dibatalkan jika mengganti tanggal lahir.
- Jika gagal terjemah, teks asli ditampilkan dengan catatan `(Gagal diterjemahkan)`.

---

## 👩‍💻 Pembuat

**Nama:** Ita Khairati  
**Kelas / Mata Kuliah:** PBO2-LATIHAN2-23100101219  

---

README ini sudah menjelaskan **dua kode** secara menyeluruh, alur, dan interaksi antar file.  

