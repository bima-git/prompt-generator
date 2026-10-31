# Dokumentasi Konteks Proyek: Generator Prompt Storyboard & Iklan Video

Dokumen ini disusun sebagai panduan konteks (*system context*) untuk AI Agent dalam memahami, memelihara, dan mengembangkan aplikasi web **Generator Prompt Storyboard Iklan**.

## 1. Gambaran Umum Proyek (Project Overview)

* **Nama Proyek:** Generator Prompt Storyboard & Video AI Sinematik
* **Tujuan Utama:** Membantu pengguna menghasilkan *prompt* AI (untuk Gemini, ChatGPT, atau Claude) yang terstruktur dan siap pakai guna menciptakan ide skrip video serta *storyboard* pengiklanan produk — maupun video sinematik/atmosferik suasana alam — hanya dengan memasukkan 1–2 kata kunci tema.
* **Cara Kerja Sistem (Skenario Tanpa API):** Aplikasi ini murni bekerja sebagai **Alat Perakit Teks (*Prompt Builder*)** di sisi klien (*client-side*). Aplikasi **tidak** terhubung ke API AI mana pun. Pengguna memilih parameter di form, sistem menggabungkannya ke dalam *template* kalimat, lalu pengguna menyalin hasilnya ke platform AI pilihan mereka.
* **Dua Mode (Tab Switcher):** Aplikasi memiliki **Mode 1 — Storyboard Iklan Produk** dan **Mode 2 — Video AI Sinematik/Atmosferik**, dipilih melalui tab di atas panel kiri. Tombol Buat Prompt mendelegasikan ke fungsi pembentuk prompt sesuai mode aktif.
* **Alur 2 Tahap Mode 1 (Output 2 Bagian):** Prompt Mode 1 menghasilkan **Bagian A — Storyboard** (kreatif, bahasa Indonesia) dan **Bagian B — Prompt per-scene untuk AI video generator** (deskriptif visual, *standalone*, bilingual EN/ID, universal untuk Veo/Sora/Runway/Kling/Pika). Pengguna menempelkan prompt ke ChatGPT/Gemini, lalu menyalin tiap prompt Bagian B ke AI pembuat video.
* **Alur Mode 2 (Video Sinematik):** Prompt Mode 2 mengarahkan AI sebagai **AI Video Director** untuk menyusun rangkaian cerita visual berkesinambungan (Establishing Shot → aktivitas subjek → Tracking Shot); tiap scene menghasilkan **3 prompt**: Text-to-Image, Image-to-Video Motion, dan Audio/SFX — untuk alur generate gambar lalu image-to-video di Kling/Runway/Veo.

## 2. Arsitektur & Spesifikasi Teknis (Tech Stack)

* **Arsitektur:** *Single-Page Static Web Application* (100% *Client-Side*, tanpa *backend/server*).
* **HTML:** HTML5 semantik (seluruh kode disatukan dalam satu file utama `index.html`).
* **Styling (CSS):** **Tailwind CSS** (dimuat langsung melalui CDN `<script src="https://cdn.tailwindcss.com"></script>` agar praktis tanpa proses *build* atau *bundler*).
* **Logika (JavaScript):** **Vanilla JavaScript** (JS Murni). Menggunakan fitur *Template Literals* untuk menggabungkan nilai input form menjadi *string* utuh. Tidak menggunakan *framework* JS seperti React, Vue, atau Angular.
* **Platform Hosting (Final):** **GitHub Pages** (Ditetapkan sebagai layanan *hosting* utama karena aplikasi 100% statis, gratis, mendukung penggunaan komersial/publik, dan langsung menayangkan file dari repositori).

## 3. Struktur Antarmuka (UI/UX Layout)

Desain mengadaptasi tata letak kartu terpusat yang terbagi menjadi **dua panel utama** (bersebelahan pada layar desktop `md:flex-row`, dan bertumpuk pada layar seluler `flex-col`):

### A. Panel Kiri: Input & Pengaturan (Dual-Mode)

* **Tema Visual:** Gelap (*Dark Mode* menggunakan `bg-slate-900 text-white`).
* **Tab Switcher (di paling atas):** Dua tombol mode — `#tabMode1` (**Mode 1 · Iklan Produk**, aktif secara bawaan) dan `#tabMode2` (**Mode 2 · Video Sinematik**). Fungsi `switchMode()` menampilkan/menyembunyikan `#panelMode1` / `#panelMode2` dan mengganti gaya tab aktif (`bg-blue-500`).
* **Komponen Input Mode 1 (`#panelMode1`):**
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
* **Komponen Input Mode 2 (`#panelMode2`, tersembunyi secara bawaan):**
  1. **Tema / Lokasi Utama (`#lokasi`):** Input teks bebas (misal: "Pedesaan Jawa", "Hutan Pinus").
  2. **Suasana Cuaca & Waktu (`#cuaca`):** Dropdown — Badai Petir & Mendung Pekat, Hujan Deras Sore Hari, Kabut Pagi Dingin, Senja Golden Hour, Malam Sunyi.
  3. **Jumlah Scene (`#jumlahScene`):** Dropdown — 3 / 4 / 5 Scene.
  4. **Rasio Video (`#rasioSinema`):** Dropdown — 9:16 Vertikal, 16:9 Horizontal, 1:1 Persegi, 4:3 Standar.
  5. **Gaya Visual (`#gayaSinema`):** Dropdown — Photorealistic Cinematic 8K, Moody Dark Realism, Nostalgic Documentary.
  6. **Atmosfer Audio (`#audioSinema`):** Dropdown — Suara Alam + Senandung Humming Melankolis, ASMR Cuaca & Alam Murni, Instrumen Ambient Sinematik.
  7. **Opsi Bahasa Prompt Teknis (`#inggrisSinema`):** Checkbox (bawaan aktif) untuk menulis prompt teknis dalam Bahasa Inggris agar akurat di AI Video Generator.
* **Tombol Aksi (dipakai kedua mode):**
  * **Buat Prompt (`buatPrompt()`):** Mendelegasikan ke `buatPromptIklan()` atau `buatPromptSinematik()` sesuai mode aktif (`modeAktif`).
  * **Reset (`resetForm()`):** Mengembalikan seluruh input **kedua mode** dan output ke kondisi awal.

### B. Panel Kanan: 2) Prompt Siap Pakai

* **Tema Visual:** Terang (*Light Mode* menggunakan `bg-white text-slate-800`).
* **Komponen Output:**
  1. **Kotak Teks (`#outputPrompt`):** Elemen `<textarea>` yang menampilkan hasil *prompt*. Bersifat *editable* (pengguna bisa mengedit teks secara manual sebelum disalin).
  2. **Tombol Utilitas (`salinTeks()`):** Tombol untuk menyalin isi `<textarea>` ke *clipboard* perangkat.

## 4. Logika Formula Prompt Saat Ini

Aplikasi memiliki dua formula prompt yang dipilih berdasarkan mode aktif.

### 4a. Mode 1 — Iklan Produk (`buatPromptIklan()`)

Kerangka *template literal* (diringkas; teks lengkap ada di `index.html`):

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

### 4b. Mode 2 — Video AI Sinematik (`buatPromptSinematik()`)

```text
Bertindaklah sebagai AI Video Director dan sinematografer profesional. Buatkan rangkaian cerita visual sinematik yang berkesinambungan tentang suasana "${lokasi}".

Kondisi video: cuaca/waktu (${cuaca}), gaya visual (${gayaSinema}), atmosfer audio (${audioSinema}), rasio (${rasioSinema}), jumlah scene (${jumlahScene}).

Susun cerita sebagai perjalanan: Establishing Shot lingkungan → aktivitas manusia/subjek di lokasi → diakhiri Tracking Shot mengikuti subjek; setiap scene tersambung secara visual.

Untuk SETIAP scene hasilkan TIGA bagian:
[SCENE n]
1. TEXT-TO-IMAGE PROMPT (detail visual, pencahayaan, tekstur, komposisi — cukup untuk 1 frame statis).
2. IMAGE-TO-VIDEO MOTION PROMPT (gerakan kamera, hembusan angin, efek cuaca, pergerakan subjek — untuk Kling/Runway/Veo).
3. AUDIO/SFX PROMPT (suara lingkungan sesuai suasana).

Aturan: tanpa dialog/voiceover/teks on-screen/tanda kutip; deskripsi konkret; judul pendek per scene; seluruh prompt teknis dalam Bahasa Inggris jika checkbox #inggrisSinema aktif (default), Bahasa Indonesia jika tidak.
```

## 5. Status Pengembangan & Rencana Fitur Lanjutan (Roadmap)

* **Status Saat Ini:** Aplikasi berjalan sebagai **Dual-Mode** (Mode 1 Iklan Produk + Mode 2 Video Sinematik via Tab Switcher) dan **sudah live** di GitHub Pages (`https://bima-git.github.io/prompt-generator/`). Seluruh backlog versi 1 telah diimplementasikan (Download .txt, indikator karakter/kata *real-time*, opsi tambahan CTA/Audiens/Anggaran, salin via `navigator.clipboard` + *toast* notifikasi, prompt 2 bagian bilingual untuk AI video).
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