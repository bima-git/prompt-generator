# Dokumentasi Konteks Proyek: Generator Prompt Storyboard & Iklan Video

Dokumen ini disusun sebagai panduan konteks (*system context*) untuk AI Agent dalam memahami, memelihara, dan mengembangkan aplikasi web **Generator Prompt Storyboard Iklan**.

## 1. Gambaran Umum Proyek (Project Overview)

* **Nama Proyek:** Generator Prompt Storyboard Iklan
* **Tujuan Utama:** Membantu pengguna menghasilkan *prompt* AI (untuk Gemini, ChatGPT, atau Claude) yang terstruktur dan siap pakai guna menciptakan ide skrip video serta *storyboard* pengiklanan produk hanya dengan memasukkan 1–2 kata kunci tema/produk.
* **Cara Kerja Sistem (Skenario Tanpa API):** Aplikasi ini murni bekerja sebagai **Alat Perakit Teks (*Prompt Builder*)** di sisi klien (*client-side*). Aplikasi **tidak** terhubung ke API AI mana pun. Pengguna memilih parameter di form, sistem menggabungkannya ke dalam *template* kalimat, lalu pengguna menyalin hasilnya ke platform AI pilihan mereka.
* **Alur 2 Tahap (Output 2 Bagian):** Prompt yang dihasilkan dirancang untuk menghasilkan **dua bagian** dari AI teks: **Bagian A — Storyboard** (kreatif, bahasa Indonesia) dan **Bagian B — Prompt per-scene untuk AI video generator** (deskriptif visual, *standalone*, bilingual EN/ID, universal untuk Veo/Sora/Runway/Kling/Pika). Pengguna menempelkan prompt ke ChatGPT/Gemini, lalu menyalin tiap prompt Bagian B ke AI pembuat video.

## 2. Arsitektur & Spesifikasi Teknis (Tech Stack)

* **Arsitektur:** *Single-Page Static Web Application* (100% *Client-Side*, tanpa *backend/server*).
* **HTML:** HTML5 semantik (seluruh kode disatukan dalam satu file utama `index.html`).
* **Styling (CSS):** **Tailwind CSS** (dimuat langsung melalui CDN `<script src="https://cdn.tailwindcss.com"></script>` agar praktis tanpa proses *build* atau *bundler*).
* **Logika (JavaScript):** **Vanilla JavaScript** (JS Murni). Menggunakan fitur *Template Literals* untuk menggabungkan nilai input form menjadi *string* utuh. Tidak menggunakan *framework* JS seperti React, Vue, atau Angular.
* **Platform Hosting (Final):** **GitHub Pages** (Ditetapkan sebagai layanan *hosting* utama karena aplikasi 100% statis, gratis, mendukung penggunaan komersial/publik, dan langsung menayangkan file dari repositori).

## 3. Struktur Antarmuka (UI/UX Layout)

Desain mengadaptasi tata letak kartu terpusat yang terbagi menjadi **dua panel utama** (bersebelahan pada layar desktop `md:flex-row`, dan bertumpuk pada layar seluler `flex-col`):

### A. Panel Kiri: 1) Input & Pengaturan

* **Tema Visual:** Gelap (*Dark Mode* menggunakan `bg-slate-900 text-white`).
* **Komponen Input:**
  1. **Tema / Produk Utama (`#tema`):** Input teks singkat (1–2 kata, misal: "Sepatu Lari", "Kopi Dingin").
  2. **Durasi Video (`#durasi`):** Dropdown (`15 detik`, `30 detik`, `60 detik`).
  3. **Format & Rasio Video (`#rasio`):** Dropdown fleksibel lintas platform maupun layar kustom:
     * `9:16 (Vertikal - TikTok/Reels)`
     * `16:9 (Horizontal - YouTube/TV)`
     * `1:1 (Persegi - Feed IG)`
     * `4:3 (Standar Presentasi)`
  4. **Gaya Iklan / Tone (`#gaya`):** Dropdown (`Lucu dan menghibur (Komedik)`, `Estetik dan Elegan`, `Emosional dan Storytelling`, `Hard-selling dan langsung ke intinya`).
  5. **Format Output (`#format`):** Dropdown (`Tabel Storyboard Rinci (Scene, Visual, Audio, Teks)`, `Skrip Dialog & Narasi (Paragraf)`).
  6. **Opsi Tambahan:** Checkbox + input CTA (`#ctaAktif`/`#cta`); Checkbox target audiens terstruktur (`#audiensAktif` → panel `#audiensForm` berisi `#umur` (dropdown rentang usia), `#profesi` & `#minat` (input + `datalist`), dan `#audiensLain` (detail bebas)); Dropdown batasan anggaran produksi (`#anggaran`).
* **Tombol Aksi:**
  * **Buat Prompt (`buatPrompt()`):** Merakit input menjadi teks *prompt*.
  * **Reset (`resetForm()`):** Mengembalikan seluruh input dan output ke kondisi awal.

### B. Panel Kanan: 2) Prompt Siap Pakai

* **Tema Visual:** Terang (*Light Mode* menggunakan `bg-white text-slate-800`).
* **Komponen Output:**
  1. **Kotak Teks (`#outputPrompt`):** Elemen `<textarea>` yang menampilkan hasil *prompt*. Bersifat *editable* (pengguna bisa mengedit teks secara manual sebelum disalin).
  2. **Tombol Utilitas (`salinTeks()`):** Tombol untuk menyalin isi `<textarea>` ke *clipboard* perangkat.

## 4. Logika Formula Prompt Saat Ini

Berikut adalah kerangka *template literal* yang digunakan di dalam fungsi JavaScript `buatPrompt()` (diringkas; teks lengkap ada di `index.html`):

```text
Bertindaklah sebagai Creative Director dan spesialis pembuat Storyboard iklan profesional. Buatkan ide skrip video iklan untuk produk/tema "${tema}".

Video ini berdurasi maksimal ${durasi} dan dirancang khusus untuk format rasio visual ${rasio}. Gunakan pendekatan visual dan narasi yang ${gaya}.

Hasilmu harus terdiri dari DUA bagian utama:

=== BAGIAN A: STORYBOARD ===
Sajikan dalam format ${format} (tabel: Scene, Detik, Arahan Visual & Sudut Kamera, Audio/Voiceover, Teks Animasi + 3 opsi Hook 3 detik pertama).

=== BAGIAN B: PROMPTS UNTUK AI VIDEO GENERATOR ===
Untuk SETIAP scene, tulis satu prompt standalone (visual-only: subjek, aksi, latar, gerak kamera, pencahayaan, gaya visual, durasi; maks 60 kata; tanpa musik/dialog/teks on-screen; hindari kata abstrak) dalam DUA versi: (EN) bahasa Inggris dan (ID) bahasa Indonesia, dengan penomoran scene konsisten dengan Bagian A.
```

*Catatan:* Opsi tambahan (CTA, target audiens terstruktur, batasan anggaran) hanya disisipkan ke akhir prompt jika diaktifkan/diisi.

## 5. Status Pengembangan & Rencana Fitur Lanjutan (Roadmap)

* **Status Saat Ini:** Prototipe dasar (`index.html`) sudah selesai; **seluruh backlog versi 1 telah diimplementasikan** (Download .txt, indikator karakter/kata *real-time*, opsi tambahan CTA/Audiens/Anggaran, serta salin via `navigator.clipboard` + *toast* notifikasi).
* **Ide Pengembangan Selanjutnya (Backlog v2):**
  * Menyimpan riwayat prompt ke `localStorage` (riwayat 5–10 prompt terakhir, tetap 100% *client-side*) untuk GitHub Pages.
  * Menambahkan *preset* gaya iklan tambahan (misal: UGC ala TikTok, *AI Image Sequence*, *Testimoni Pelanggan*).
  * Opsi bahasa *output* prompt (Bahasa Indonesia / English) agar bisa dipakai di AI berbahasa Inggris.
  * Menambahkan *shortcut* keyboard (`Ctrl+Enter` untuk Buat Prompt, `Ctrl+Shift+C` untuk Salin).

## 6. Spesifikasi Deployment & Hosting (GitHub Pages)

Proyek ini telah disepakati untuk di-*hosting* menggunakan **GitHub Pages**. Berikut adalah ketentuan arsitektur untuk mendukung *deployment* tersebut:

* **Struktur Direktori:** File utama wajib bernama **`index.html`** dan diletakkan tepat di direktori akar (`/root`) repositori.
* **Tanpa Build Step:** Kode harus siap tayang (*ready-to-serve*) begitu diunggah ke *branch* `main` tanpa memerlukan kompilasi Node.js/NPM atau GitHub Actions yang rumit.
* **Path Relatif:** Jika di masa mendatang terdapat pemisahan file (misalnya menambahkan gambar, ikon, atau file `.js`/`.css` terpisah), gunakan *relative path* (contoh: `./script.js` bukan `/script.js`) agar tidak terjadi *error* `404 Not Found` saat ditayangkan di sub-path URL `https://[username].github.io/[nama-repo]/`.

## 7. Instruksi Khusus untuk AI Agent Selanjutnya

Jika Anda (AI Agent) diminta untuk memodifikasi atau menambahkan fitur pada proyek ini, patuhi aturan berikut:

1. **Pertahankan Kesederhanaan:** Tetap gunakan pendekatan *Vanilla HTML/JS* dan *Tailwind CSS via CDN*. Jangan mengubah proyek ini menjadi proyek Node.js/React/Vite kecuali diminta secara eksplisit oleh pengguna.
2. **Kompatibilitas Penuh GitHub Pages:** Pastikan seluruh kode tetap dapat berjalan 100% secara statis di *browser* tanpa memerlukan *server backend* atau *environment variables*, serta mematuhi aturan pada **Bagian 6**.
3. **Konsistensi UI:** Pertahankan desain dua panel (Kiri: Gelap untuk Input, Kanan: Terang untuk Output) agar tetap rapi, responsif, dan sesuai dengan referensi desain awal pengguna.