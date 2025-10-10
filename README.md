# AplikasiPenghitungUmur
Latihan 2 - Ita Khairati 2310010219

# 🧮 Aplikasi Penghitung Umur (Java GUI)

Aplikasi **Java Swing** untuk menghitung **umur seseorang**, menampilkan **hari ulang tahun berikutnya**, dan **peristiwa penting** pada tanggal tersebut.  
UI interaktif dengan **JDateChooser**, **JTextField**, **JTextArea**, dan tombol warna-warni.  

---

## 🌟 Fitur Utama

<div style="border:1px solid #ff69b4; padding:10px; border-radius:8px; background-color:#fff0f5;">
<ul>
<li>📅 Pilih tanggal lahir menggunakan <b>JDateChooser</b>.</li>
<li>🧮 Menampilkan umur saat ini secara detail.</li>
<li>🎉 Menampilkan hari & tanggal ulang tahun berikutnya.</li>
<li>📝 Menampilkan peristiwa penting secara <i>baris per baris</i>.</li>
<li>⏹️ Bisa menghentikan pengambilan data peristiwa saat tanggal lahir diganti.</li>
<li>❌ Keluar dari aplikasi dengan tombol <b>Keluar</b>.</li>
</ul>
</div>

---

## 🎨 Tampilan Aplikasi

<div style="display:flex; gap:20px;">
<img src="image/1.png" alt="Tampilan Utama" width="300"/>
<img src="image/2.png" alt="Hasil Perhitungan" width="300"/>
</div>

---

## 🧱 Komponen GUI

| Komponen                 | Fungsi                                         |
|---------------------------|-----------------------------------------------|
| JFrame                    | Jendela utama program                          |
| JPanel                    | Panel untuk menampung semua komponen          |
| JLabel                    | Label teks                                     |
| JTextField                | Menampilkan umur dan hari ulang tahun berikutnya |
| JDateChooser              | Memilih tanggal lahir                           |
| JButton                   | Tombol Hitung Umur & Keluar                   |
| JTextArea                 | Menampilkan peristiwa penting                  |

---

## ⚙️ Logika Program

1. Ambil tanggal lahir dari **JDateChooser**.
2. Hitung umur dengan **LocalDate**.
3. Hitung tanggal ulang tahun berikutnya.
4. Ambil peristiwa penting secara asinkron menggunakan **Thread**.
5. Update **TextField** & **TextArea** secara real-time.
6. Hentikan thread saat tanggal lahir diganti untuk menjaga UI responsif.

---

## 🖱️ Event Handling

| Komponen                 | Event                  | Fungsi                                         |
|---------------------------|----------------------|-----------------------------------------------|
| Tombol Hitung             | ActionListener       | Hitung umur, tanggal ulang tahun, tampilkan peristiwa |
| Tombol Keluar             | ActionListener       | Keluar dari aplikasi                           |
| JDateChooser              | PropertyChangeListener | Hapus hasil sebelumnya & hentikan thread lama  |
| JTextArea                 | Thread Update        | Menampilkan peristiwa baris per baris          |

---

## 💻 Cara Menjalankan Program

1. Buka **NetBeans IDE**.
2. Pilih **File → New Project → Java Application**.
3. Buat package baru, misal: `penghitungumur`.
4. Tambahkan file:
   - `PenghitungUmurFrame.java`
   - `PenghitungUmurHelper.java`
5. Jalankan program dengan **Shift + F6**.

---

## 🧰 Teknologi

- Java SE 8+
- Swing GUI
- NetBeans IDE
- JCalendar Library (`com.toedter.calendar.JDateChooser`)

---

## 👩‍💻 Pembuat

**Nama:** ITA KHAIRATI  
**Kelas / Mata Kuliah:** PBO2-LATIHAN2-23100101219

---

## 📂 Kode Sumber Lengkap

```java
import java.time.LocalDate;
import java.time.ZoneId;
import java.time.format.DateTimeFormatter;
import java.util.Date;

public class PenghitungUmurFrame extends javax.swing.JFrame {
    private PenghitungUmurHelper helper;
    private volatile boolean stopFetching = false;
    private Thread peristiwaThread;

    public PenghitungUmurFrame() {
        initComponents();
        helper = new PenghitungUmurHelper();
        setLocationRelativeTo(null); 
        setTitle("Aplikasi Penghitung Umur"); 
    }

    @SuppressWarnings("unchecked")
    private void initComponents() {
        // ... inisialisasi semua komponen GUI seperti jLabel, jPanel, dateChooser, tombol, textfield, textarea ...
    }

    private void btnHitungActionPerformed(java.awt.event.ActionEvent evt) {
        Date tanggalLahir = dateChooserTanggalLahir.getDate();
        LocalDate ulangTahunBerikutnya = null;

        if (tanggalLahir != null) {
            LocalDate lahir = tanggalLahir.toInstant().atZone(ZoneId.systemDefault()).toLocalDate();
            LocalDate sekarang = LocalDate.now();
            String umur = helper.hitungUmurDetail(lahir, sekarang);
            txtUmur.setText(umur);

            ulangTahunBerikutnya = helper.hariUlangTahunBerikutnya(lahir, sekarang);
            String hariUlangTahunBerikutnya = helper.getDayOfWeekInIndonesian(ulangTahunBerikutnya);
            DateTimeFormatter formatter = DateTimeFormatter.ofPattern("dd-MM-yyyy");
            String tanggalUlangTahunBerikutnya = ulangTahunBerikutnya.format(formatter);
            txtHariUlangTahunBerikutnya.setText(hariUlangTahunBerikutnya + " (" + tanggalUlangTahunBerikutnya + ")");
        }

        if (ulangTahunBerikutnya == null) return;

        stopFetching = true;
        if (peristiwaThread != null && peristiwaThread.isAlive()) {
            peristiwaThread.interrupt();
        }

        stopFetching = false;
        final LocalDate tanggalPeristiwa = ulangTahunBerikutnya;
        peristiwaThread = new Thread(() -> {
            try {
                txtAreaPeristiwa.setText("Tunggu, sedang mengambil data...\n");
                helper.getPeristiwaBarisPerBaris(tanggalPeristiwa, txtAreaPeristiwa, () -> stopFetching);
                if (!stopFetching) {
                    javax.swing.SwingUtilities.invokeLater(() ->
                        txtAreaPeristiwa.append("\nSelesai mengambil data peristiwa"));
                }
            } catch (Exception e) {
                if (Thread.currentThread().isInterrupted()) {
                    javax.swing.SwingUtilities.invokeLater(() ->
                        txtAreaPeristiwa.setText("Pengambilan data dibatalkan.\n"));
                }
            }
        });
        peristiwaThread.start();
    }

    private void btnKeluarActionPerformed(java.awt.event.ActionEvent evt) {
        System.exit(0);
    }

    private void dateChooserTanggalLahirPropertyChange(java.beans.PropertyChangeEvent evt) {
        txtUmur.setText("");
        txtHariUlangTahunBerikutnya.setText("");
        stopFetching = true;
        if (peristiwaThread != null && peristiwaThread.isAlive()) {
            peristiwaThread.interrupt();
        }
        txtAreaPeristiwa.setText("");
    }

    public static void main(String args[]) {
        java.awt.EventQueue.invokeLater(() -> {
            new PenghitungUmurFrame().setVisible(true);
        });
    }

    // Deklarasi variabel GUI
    private javax.swing.JButton btnHitung;
    private javax.swing.JButton btnKeluar;
    private com.toedter.calendar.JDateChooser dateChooserTanggalLahir;
    private javax.swing.JTextArea txtAreaPeristiwa;
    private javax.swing.JTextField txtHariUlangTahunBerikutnya;
    private javax.swing.JTextField txtUmur;
}
