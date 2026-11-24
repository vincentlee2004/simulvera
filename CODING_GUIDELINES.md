# Panduan & Standar Penulisan Kode (`CODING_GUIDELINES.md`)

Dokumen ini adalah kesepakatan resmi tim sebelum menulis kode. Tujuannya agar aplikasi memiliki *runtime* yang cepat, mudah dilacak saat terjadi *bug*, mudah dipahami tim lokal maupun *developer* internasional di masa depan, serta siap jika suatu saat dimigrasi ke bahasa atau platform lain.

> **Catatan:** Untuk daftar istilah bisnis, terjemahan bahasa, dan catatan ide fitur baru, silakan lihat file [`GLOSSARY.md`](./GLOSSARY.md).

---

## 1. Kesepakatan Bahasa & Penamaan (*Naming Convention*)

* **Kode Menggunakan Bahasa Inggris Baku:** Seluruh nama folder, file, fungsi, variabel, dan pesan *error* sistem wajib menggunakan bahasa Inggris yang jelas dan baku agar siap untuk skala global.
* **Komentar & Dokumentasi:** Komentar penjelasan aturan bisnis di dalam kode boleh ditulis dalam bahasa Indonesia (atau bahasa Inggris sederhana).
* **Deskriptif & Tanpa Singkatan:** Tulis nama fungsi dan variabel secara utuh sesuai tugasnya meskipun panjang.
  * ✅ **Benar:** `function calculateRectangleArea(length, width)`
  * ❌ **Salah:** `function rectArea(l, w)` atau `function calcArea(p, l)`
* **Format Penulisan:**
  * **`camelCase`**: Untuk variabel dan fungsi (contoh: `currentUserScore`, `saveSimulationProgress`).
  * **`UPPER_CASE`**: Untuk nilai tetap / konstanta (contoh: `MAX_LIVES_LIMIT`, `CORRECT_ANSWER_POINTS`).
  * **`PascalCase`**: Khusus untuk nama komponen React/Next.js (contoh: `SimulationQuestionCard.jsx`, `LevelProgressBar.jsx`).

---

## 2. Kamus Kata Kerja Awalan (*Function Prefixes*)

Gunakan awalan kata kerja di bawah ini secara konsisten agar kode mudah dicari lewat fitur pencarian (*search keyword*) di *code editor*:

| Awalan (*Prefix*) | Kegunaan | Contoh Penamaan |
| :--- | :--- | :--- |
| **`calculate...`** | Rumus matematika atau kalkulasi nilai | `calculateTotalSimulationScore` |
| **`fetch...` / `get...`** | Mengambil data dari database/API (`fetch`) atau memori (`get`) | `fetchScenarioById` |
| **`save...` / `update...`** | Menyimpan data baru atau memperbarui data lama | `saveUserAnswerHistory` |
| **`delete...` / `remove...`** | Menghapus data | `deleteTemporarySession` |
| **`validate...`** | Memeriksa format atau kelengkapan data | `validateUserInputFormat` |
| **`convert...` / `format...`** | Mengubah bentuk/format data (*transform*) | `convertSecondsToMinutes` |
| **`is...` / `has...`** | Fungsi yang hanya mengembalikan `true` atau `false` | `isUserEligibleForLevelUp` |

---

## 3. Struktur Fungsi & Alur Logika

* **Satu Fungsi, Satu Tugas (*Single Responsibility*):** Setiap fungsi hanya boleh mengerjakan satu pekerjaan spesifik. Jika fungsi melebihi 30–40 baris, pecah menjadi fungsi-fungsi kecil.
* **Hindari Percabangan Bersarang (*Deep Nesting*):** Batasi penggunaan `if` atau perulangan (`for`/`while`) bertumpuk maksimal 2 tingkat. Gunakan teknik *early return* (hentikan fungsi di baris awal jika syarat tidak terpenuhi) agar kode tetap lurus dan cepat dieksekusi.
* **Dilarang Menulis *Magic Numbers*:** Jangan menulis angka atau teks mentah langsung di dalam rumus. Simpan ke dalam konstanta terlebih dahulu.
* **Validasi Input di Awal:** Selalu cek tipe data dan kekosongan parameter di baris paling atas fungsi sebelum diproses.

---

## 4. Pemisahan Tugas (*Separation of Concerns*)

Pisahkan "Otak" (rumus/aturan main) dari "Wajah" (tampilan Next.js) dan "Gudang" (database) agar mudah dimigrasi ke bahasa pemrograman lain di masa depan:

```text
src/
├── app/                  # WAJAH: Halaman & routing Next.js (khusus tampilan)
├── components/           # WAJAH: Komponen UI reusable (Button, QuestionCard, ResultModal)
├── businessLogic/        # OTAK (FUNGSI MURNI): Rumus skor, aturan simulasi, level (tanpa kode React/DB)
├── dataServices/         # GUDANG: Kode khusus komunikasi ke database / API luar
├── constants/            # Kumpulan nilai tetap (aturan poin, batas nyawa, pesan error)
├── dictionaries/         # File teks tampilan multibahasa (id.json, en.json)
└── tests/                # File unit testing untuk menguji folder businessLogic
```

---

## 5. Penanganan *Error* & Dokumentasi

* **Pesan *Error* Spesifik:** Wajib gunakan `try-catch` untuk proses eksternal (API/Database) dan sebutkan nama fungsinya pada pesan *error* dalam bahasa Inggris.
  ```javascript
  console.error("Error in fetchScenarioById:", error);
  ```
* **Gunakan JSDoc di Atas Fungsi:** Tulis ringkasan singkat tepat di atas fungsi menggunakan format `/** ... */` agar deskripsinya muncul otomatis saat nama fungsi disorot di VS Code.
* **Komentar Menjelaskan "Mengapa":** Tulis komentar di dalam kode hanya untuk menjelaskan **alasan aturan bisnis**, bukan menjelaskan cara kerja sintaks.

---

## 6. Contoh Kode Standar Tim

```javascript
// File: src/businessLogic/scoreCalculation.js

const BASE_CORRECT_POINTS = 100;
const STREAK_BONUS_MULTIPLIER = 1.5;
const MINIMUM_STREAK_FOR_BONUS = 3;

/**
 * Purpose: Calculates the points earned by a user for a single simulation step.
 * Used in: Active simulation screen & final summary page.
 */
export function calculateSimulationStepPoints(isAnswerCorrect, currentStreakCount) {
  // 1. Validasi input di awal (Early Return)
  if (typeof isAnswerCorrect !== "boolean") {
    throw new Error("Invalid input in calculateSimulationStepPoints: isAnswerCorrect must be a boolean.");
  }

  if (!isAnswerCorrect) {
    return 0;
  }

  if (typeof currentStreakCount !== "number" || currentStreakCount < 0) {
    throw new Error("Invalid input in calculateSimulationStepPoints: currentStreakCount must be a positive number.");
  }

  // 2. Alasan bisnis: Pengguna yang menjawab benar 3x berturut-turut mendapat pengali poin
  if (currentStreakCount >= MINIMUM_STREAK_FOR_BONUS) {
    return BASE_CORRECT_POINTS * STREAK_BONUS_MULTIPLIER;
  }

  return BASE_CORRECT_POINTS;
}
```

---

## 7. Aturan Keberlanjutan Jangka Panjang

1. **Pengujian Otomatis (*Unit Test*):** Setiap fungsi baru di dalam folder `src/businessLogic/` wajib memiliki file pengujian otomatis sebelum digabungkan ke cabang utama.
2. **Alur Kerja Git:** Dilarang melakukan *push* langsung ke cabang `main`. Gunakan nama cabang bahasa Inggris yang jelas, misalnya: `feat/daily-badge-system` atau `fix/negative-score-calculation`.
3. **Minimalkan *Library* Luar:** Diskusikan dengan tim sebelum menambah *package* baru (`npm install`). Jika bisa dibuat sendiri dengan fungsi sederhana, buat sendiri.
4. **Kerahasiaan Data (`.env`):** Dilarang menulis *API Key* atau kredensial database di dalam kode. Gunakan file `.env` dan selalu perbarui `.env.example`.
