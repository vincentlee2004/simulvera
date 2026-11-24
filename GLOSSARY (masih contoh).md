# Kamus Istilah & Catatan Ide Bisnis (`GLOSSARY.md`)

Dokumen ini adalah satu-satunya acuan penamaan istilah resmi di dalam kode (bahasa Inggris) beserta artinya dalam bahasa Indonesia dan catatan pengembangan ide.

> **Aturan Tim:** Setiap kali ada ide fitur baru atau aturan simulasi yang kepikiran saat diskusi, **catat dulu di dokumen ini** dan sepakati istilah bahasa Inggrisnya sebelum mulai membuat variabel, fungsi, atau tabel database.

---

## 1. Istilah Inti Simulasi (*Core Simulation*)

| Istilah Kode (EN) | Istilah Indonesia (ID) | Penjelasan Fungsi & Catatan Ide Bisnis |
| :--- | :--- | :--- |
| **`Scenario`** | Skenario | Satu rangkaian kasus atau cerita simulasi utuh dari awal hingga selesai.<br>*(Ide: Bisa dikelompokkan berdasarkan kategori topik dan tingkat kesulitan).* |
| **`Step`** | Langkah | Satu tahapan layar pertanyaan, dialog, atau pilihan keputusan di dalam sebuah `Scenario`. |
| **`Choice` / `Option`** | Pilihan Jawaban | Opsi tindakan yang bisa dipilih pengguna pada setiap `Step`. |
| **`Branch` / `NextStep`** | Percabangan | Alur perpindahan ke `Step` berikutnya berdasarkan `Choice` yang diambil pengguna. |
| **`Feedback`** | Umpan Balik | Penjelasan edukatif yang langsung muncul setelah pengguna memilih jawaban (baik benar maupun salah). |
| **`SimulationSummary`** | Ringkasan Simulasi | Laporan hasil akhir setelah pengguna menyelesaikan seluruh `Step` dalam satu `Scenario`. |

---

## 2. Istilah Gamifikasi (*Gamification System*)

| Istilah Kode (EN) | Istilah Indonesia (ID) | Penjelasan Fungsi & Catatan Ide Bisnis |
| :--- | :--- | :--- |
| **`ExperiencePoints` (XP)** | Poin Pengalaman | Angka akumulasi yang didapat dari setiap simulasi untuk menaikkan level pengguna. |
| **`UserLevel`** | Level Pengguna | Tingkatan kemahiran pengguna berdasarkan total `ExperiencePoints` yang terkumpul. |
| **`Lives`** | Nyawa | Batas kesempatan menjawab salah dalam satu `Scenario` sebelum simulasi gagal.<br>*(Ide: Nyawa terisi ulang otomatis setiap 1 jam, atau bisa diisi menggunakan XP).* |
| **`Streak`** | Runtutan Harian | Jumlah hari berturut-turut pengguna aktif menyelesaikan minimal satu `Scenario`.<br>*(Ide: Tambahkan item "Streak Freeze" agar streak tidak hangus jika absen 1 hari).* |
| **`Badge`** | Lencana | Penghargaan visual yang terbuka otomatis saat pengguna mencapai target tertentu (misal: "Selesai 5 Skenario Tanpa Salah"). |
| **`Leaderboard`** | Papan Peringkat | Daftar urutan pengguna dengan `ExperiencePoints` tertinggi.<br>*(Ide: Bisa dibagi menjadi peringkat mingguan dan peringkat sepanjang masa).* |

---

## 3. Ruang Catatan Ide Fitur Mendatang (*Backlog Ideas*)

*Gunakan bagian ini untuk menampung ide-ide mentah yang baru kepikiran sebelum dipindahkan ke tabel kamus di atas:*

* **[Ide 1 - Mode Waktu / *Timer*]:** Tambahkan batas waktu per `Step` untuk skenario tingkat lanjut agar melatih pengambilan keputusan cepat (Istilah usulan: `TimeLimitSeconds`).
* **[Ide 2 - Petunjuk / *Hint*]:** Pengguna bisa meminta bantuan petunjuk jika bingung menjawab, dengan konsekuensi pengurangan poin (Istilah usulan: `Hint` / `UseHintPenalty`).
* **[Ide 3 - Sertifikat]:** Pengguna yang menyelesaikan semua `Scenario` dalam satu kategori berhak mengunduh sertifikat digital (Istilah usulan: `CompletionCertificate`).
