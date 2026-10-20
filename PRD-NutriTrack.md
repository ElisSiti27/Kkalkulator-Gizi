# PRD — NutriTrack: Web App Kalkulator Gizi & Pelacak Kalori

| | |
|---|---|
| **Versi dokumen** | 1.0 (Draft) |
| **Tanggal** | 20 Oktober 2026 |
| **Status** | Draft untuk review |
| **Platform** | Web App (mobile-first, responsif ke tablet & desktop), PWA |
| **Bahasa** | Bahasa Indonesia (default), arsitektur siap i18n |
| **Sumber** | `screen.png`, `code.html`, `DESIGN.md` (Warm Wellness & Nutrition System) |

---

## 1. Ringkasan Eksekutif

**NutriTrack** adalah web app untuk menghitung dan mencatat asupan gizi harian (kalori, makronutrien, serat, dan hidrasi). Pengguna mencari makanan, menentukan porsi, lalu aplikasi otomatis menghitung kandungan gizinya dan membandingkannya dengan target harian pribadi.

Pendekatan produknya **hangat dan tidak menghakimi**: membantu pengguna konsisten tanpa rasa bersalah atau obsesi terhadap angka. Pendekatan ini tercermin di desain (palet krem-hangat, copywriting yang suportif seperti "Target Harian Berjalan Baik!").

### Proposisi nilai
- **Cepat**: mencatat satu makanan dalam kurang dari 15 detik.
- **Relevan secara lokal**: basis data makanan Indonesia (nasi, tempe, soto, dll.) dan satuan porsi lokal.
- **Jelas**: satu layar menjawab "sisa kalori saya berapa dan makro mana yang masih kurang?"
- **Nyaman**: visual hangat, bahasa suportif, tanpa gamifikasi agresif.

---

## 2. Latar Belakang & Masalah

| Masalah pengguna | Dampak |
|---|---|
| Aplikasi kalori global minim data makanan Indonesia, sehingga pengguna menebak angka. | Data tidak akurat, pengguna berhenti memakai. |
| Pencatatan terasa merepotkan (banyak langkah, formulir panjang). | Retensi rendah setelah minggu pertama. |
| UI kaku dan penuh angka merah/peringatan. | Memicu rasa bersalah dan hubungan tidak sehat dengan makanan. |
| Banyak orang tidak tahu target kalori & makro yang cocok untuknya. | Target asal-asalan atau tidak ada target sama sekali. |

---

## 3. Tujuan & Non-Tujuan

### 3.1 Tujuan (Goals)
1. Pengguna dapat mencatat makanan dan melihat total gizi harian secara akurat dan cepat.
2. Pengguna mendapat target kalori & makro yang dihitung otomatis dari profil dan tujuan mereka.
3. Pengguna dapat melihat tren mingguan/bulanan untuk menjaga motivasi.
4. Aplikasi terasa ringan, cepat dibuka di jaringan seluler Indonesia, dan dapat diinstal sebagai PWA.

### 3.2 Non-Tujuan (di luar scope v1)
- Rekomendasi diagnosis atau terapi medis (bukan alat medis).
- Rencana makan (meal plan) otomatis dan resep lengkap.
- Integrasi wearable (Apple Health, Google Fit) dan pemindaian foto makanan berbasis AI. Keduanya masuk roadmap.
- Fitur sosial, komunitas, dan papan peringkat.
- Aplikasi native iOS/Android.

---

## 4. Target Pengguna & Persona

| Persona | Deskripsi | Kebutuhan utama |
|---|---|---|
| **Sinta, 27, karyawan** | Ingin menurunkan berat badan secara sehat, jadwal sibuk. | Pencatatan super cepat, target kalori jelas. |
| **Raka, 22, atlet amatir/gym** | Mengejar target protein & massa otot. | Tracking makro presisi, porsi dalam gram. |
| **Bu Dewi, 45, intuitive eater** | Ingin lebih sadar pola makan tanpa tekanan. | Tampilan tenang, bahasa tidak menghakimi, ringkasan sederhana. |

---

## 5. Ruang Lingkup Fitur

### 5.1 Prioritas (MoSCoW)

| ID | Fitur | Prioritas | Rilis |
|---|---|---|---|
| F1 | Onboarding & profil (data tubuh, tujuan) | Must | MVP |
| F2 | Kalkulator target harian (BMR/TDEE, makro) | Must | MVP |
| F3 | Dashboard harian (ringkasan kalori & makro) | Must | MVP |
| F4 | Catat makanan (pencarian + kalkulator porsi) | Must | MVP |
| F5 | Log per sesi makan (Sarapan/Siang/Malam/Camilan) | Must | MVP |
| F6 | Navigasi tanggal (lihat/ubah hari lain) | Must | MVP |
| F7 | Hapus & edit entri | Must | MVP |
| F8 | Pelacak hidrasi | Should | MVP |
| F9 | Pelacak serat | Should | MVP |
| F10 | Halaman Analisis (tren mingguan/bulanan) | Should | MVP |
| F11 | Makanan kustom & favorit/terbaru | Should | MVP |
| F12 | Akun & sinkronisasi lintas perangkat | Must | MVP |
| F13 | PWA & mode offline dasar | Should | v1.1 |
| F14 | Ekspor data (CSV/PDF) | Could | v1.1 |
| F15 | Pengingat makan & minum (notifikasi) | Could | v1.1 |
| F16 | Pindai barcode & foto makanan | Could | v2 |
| F17 | Dark mode | Could | v2 |

---

## 6. Persyaratan Fungsional

### 6.1 F1 — Onboarding & Profil
- Alur 4–5 langkah: jenis kelamin, tanggal lahir, tinggi, berat, tingkat aktivitas, tujuan (turun/jaga/naik berat badan).
- Opsional: berat target dan kecepatan perubahan (mis. 0,25 / 0,5 kg per minggu).
- Pengguna dapat melewati target otomatis dan mengatur target secara manual.
- Profil dapat diedit kapan saja di halaman **Profil**.

**Kriteria penerimaan**
- Semua input divalidasi (rentang wajar: tinggi 100–250 cm, berat 30–300 kg, usia ≥ 17 tahun).
- Setelah selesai, pengguna langsung melihat target harian yang dihitung dan dapat menyesuaikannya.

### 6.2 F2 — Kalkulator Target Harian
**Rumus (default)**
- **BMR**: Mifflin-St Jeor
  - Pria: `10 × BB(kg) + 6,25 × TB(cm) − 5 × usia + 5`
  - Wanita: `10 × BB(kg) + 6,25 × TB(cm) − 5 × usia − 161`
- **TDEE** = BMR × faktor aktivitas (1,2 / 1,375 / 1,55 / 1,725 / 1,9).
- **Target kalori** = TDEE ± penyesuaian tujuan (defisit/surplus). Defisit dibatasi agar tidak di bawah **batas minimum aman** (mis. 1.200 kkal wanita / 1.500 kkal pria) dan pengguna melihat catatan jika target disesuaikan.
- **Distribusi makro default**: Karbohidrat 50%, Protein 25%, Lemak 25% (dapat diubah). Konversi: karbo & protein 4 kkal/g, lemak 9 kkal/g.
- **Target serat**: 14 g per 1.000 kkal (atau default 25–30 g). **Target air**: default 2,5 L, dapat diubah.

**Kriteria penerimaan**
- Persentase makro harus berjumlah 100%; UI mencegah penyimpanan jika tidak.
- Contoh dari desain: target 2.100 kkal, Karbo 260 g, Protein 130 g, Lemak 65 g harus bisa dihasilkan dan ditampilkan konsisten.
- Tersedia teks informasi bahwa angka adalah **estimasi**, bukan saran medis.

### 6.3 F3 — Dashboard Harian (Halaman "Pelacak")
Mengacu pada `screen.png`. Komponen berurutan dari atas:

| # | Komponen | Perilaku |
|---|---|---|
| 1 | **Header**: logo + nama "NutriTrack · Pelacak Kalori", pemilih tanggal ("Hari Ini, 24 Mei"), avatar profil | Pemilih tanggal membuka kalender; avatar menuju halaman Profil. |
| 2 | **Banner motivasi** ("Target Harian Berjalan Baik!", "Sisa 680 kkal…", chip "68% Selesai") | Teks dinamis berdasarkan progres (lihat 6.3.1). |
| 3 | **Kartu Ringkasan Kalori**: cincin progres "kkal sisa", "Sudah Masuk", "Target Harian", bar progres linear, chip "Target: 2.100 kkal" | Dihitung real-time dari semua entri hari itu. Chip target membuka pengaturan target. |
| 4 | **Tiga kartu makro**: Karbo, Protein, Lemak (persen, bar, `gram/target`) | Persen = konsumsi ÷ target. |
| 5 | **Dua kartu vital**: Serat (`24g /30g`) dan Hidrasi (`1,8L /2,5L`) | Hidrasi dapat ditambah lewat aksi cepat (+250 ml). |
| 6 | **Filter sesi**: Semua Sesi, Sarapan (420), Makan Siang (650), Makan Malam (350), Camilan (0) | Angka di tab adalah total kkal sesi; tab menyaring daftar di bawahnya. Strip dapat di-scroll horizontal. |
| 7 | **Kartu per sesi makan**: ikon, nama, waktu, total kkal, ringkasan P/K/L, daftar item, tombol "+ Tambah Makanan …" | Lihat 6.5. |
| 8 | **Empty state Camilan** ("Belum ada camilan dicatat…") | Tampil jika sesi kosong, disertai tombol tambah. |
| 9 | **Bottom navigation**: Pelacak, Analisis, tombol FAB (+), Profil | FAB membuka form catat makanan dengan sesi otomatis sesuai jam. |

#### 6.3.1 Logika Banner & Status
| Kondisi (konsumsi ÷ target) | Pesan (nada suportif) |
|---|---|
| 0% | "Selamat pagi! Yuk mulai catat sarapanmu." |
| 1–84% | "Target Harian Berjalan Baik! Sisa X kkal untuk mencapai target idealmu." |
| 85–105% | "Hampir tepat sasaran hari ini. Kerja bagus!" |
| > 105% | "Asupanmu sedikit melebihi target. Tidak apa-apa, besok bisa disesuaikan." |

Aturan desain: **jangan** memakai warna merah/peringatan keras untuk kelebihan kalori. Gunakan nada netral-hangat sesuai prinsip desain.

#### 6.3.2 Perhitungan
- `sisa = target − total masuk` (boleh negatif; tampilkan sebagai "melebihi X kkal" dan cincin berhenti di 100%).
- `persen selesai = round(masuk ÷ target × 100)`.
- Total per sesi = jumlah kkal item. Ringkasan P/K/L per sesi = jumlah gram item.
- **Validasi data**: kkal item harus konsisten dengan makro (`4P + 4K + 9L`) dalam toleransi ±10%. Lihat catatan temuan di bagian 14.

### 6.4 F4 — Catat Makanan ("Input & Hitung Makro", Bottom Sheet)
Mengacu pada modal di `code.html`.

**Elemen & perilaku**
1. **Waktu Makan**: pilihan Sarapan / Siang / Malam / Camilan. Default menyesuaikan sesi dari tombol yang diklik atau waktu saat ini.
2. **Nama Makanan**: kolom pencarian dengan autocomplete dari basis data. Mendukung pencarian toleran salah ketik dan sinonim (mis. "ayam dada" → "Dada Ayam").
3. **Chip saran cepat**: makanan favorit/terbaru pengguna (di desain: "+ Dada Ayam", "+ Nasi Putih", "+ Alpukat"). Di produksi berisi 3–5 item terbaru/terfavorit.
4. **Ukuran Porsi**: pemilih satuan **Gram (g)**, **Porsi**, **Mangkok**. Satuan yang tersedia mengikuti data makanan (mis. 1 centong nasi, 1 butir telur, 1 potong tempe).
5. **Stepper porsi**: tombol −/+ (langkah 25 g) dan input angka manual; pintasan **+50g**, **+100g**.
6. **Estimasi Otomatis (real-time)**: menampilkan kkal, Karbo, Protein, Lemak. Dihitung `nilai per 100 g × (gram ÷ 100)`, diperbarui tiap perubahan input (debounce ≤ 100 ms).
7. **Aksi**: **Batal** dan **Hitung & Simpan**. Setelah simpan, bottom sheet tertutup, item muncul di sesi, dan dashboard diperbarui. Konfirmasi memakai **toast** bergaya Level 3 (navy), bukan `alert()` browser.

**Kriteria penerimaan**
- Porsi minimal 1 g, maksimal 5.000 g; input non-numerik ditolak.
- Mengubah satuan mengonversi ke gram dengan benar (mis. 1 butir telur = ±50 g).
- Menyimpan tanpa memilih makanan dari database menampilkan opsi "Buat makanan kustom".
- Seluruh alur (buka sheet → simpan) dapat diselesaikan ≤ 4 ketukan untuk makanan favorit.

> **Catatan implementasi:** prototipe `code.html` memakai satu profil gizi tetap (165 kkal, K 18 g, P 14,5 g, L 4 g per 100 g) sebagai placeholder. Versi produksi **harus** membaca data per makanan dari basis data.

### 6.5 F5 & F7 — Log per Sesi, Edit, dan Hapus
- Setiap kartu sesi menampilkan: ikon (matahari/pagi, siang, bulan/malam, kue/camilan), nama sesi, waktu makan, total kkal, ringkasan **P/K/L**.
- Setiap item menampilkan: foto (thumbnail), nama (dipotong dengan elipsis jika panjang, tooltip nama penuh), porsi + makro (`1 mangkuk • P: 10g, K: 38g, L: 6g`), kkal, dan ikon hapus.
- **Ketuk item** → buka bottom sheet edit (ubah porsi, ganti sesi, ubah waktu).
- **Hapus** → dengan opsi **Urungkan (undo)** selama 5 detik melalui toast, bukan dialog konfirmasi berlapis.
- **Pindahkan/duplikat** item ke sesi/hari lain (menyalin "makan kemarin").
- Waktu sesi dapat diedit (mis. 07:30).

### 6.6 F6 — Navigasi Tanggal
- Pemilih tanggal di header membuka kalender mini; tersedia pintasan Hari Ini / Kemarin.
- Dukungan geser kiri/kanan antar hari di mobile.
- Hari lampau dan hari yang akan datang (hingga 7 hari) dapat diisi.

### 6.7 F8 & F9 — Hidrasi & Serat
- **Hidrasi**: tombol cepat +250 ml / +500 ml / jumlah kustom; satuan L. Dapat dikoreksi.
- **Serat**: terhitung otomatis dari item makanan (data serat per makanan).
- Kartu menunjukkan `konsumsi /target` dan berubah warna lembut saat target tercapai (tertiary/sage).

### 6.8 F10 — Halaman Analisis
- Rentang: 7 hari, 30 hari, 3 bulan.
- Grafik: kalori harian vs target, rata-rata makro (bar/donat), tren berat badan (input manual), konsistensi pencatatan (hari tercatat).
- Insight naratif singkat bernada positif (mis. "Protein kamu stabil di atas 90% target minggu ini.").
- Tidak menampilkan "kegagalan" atau skor negatif.

### 6.9 F11 — Makanan Kustom, Favorit & Terbaru
- Pengguna dapat membuat makanan sendiri (nama, takaran dasar, kkal, K/P/L, serat opsional).
- Makanan terbaru dan favorit muncul sebagai saran cepat di form.
- Dukungan **resep/kombinasi** sederhana (gabungan beberapa bahan menjadi satu entri) masuk v1.1.

### 6.10 F12 — Akun & Profil
- Registrasi/Login: email + kata sandi, login Google. Mode tamu dengan data lokal dan ajakan membuat akun untuk sinkronisasi.
- Halaman **Profil**: data tubuh, target & distribusi makro, satuan, bahasa, privasi, ekspor/hapus data, keluar.
- Hapus akun menghapus seluruh data pengguna (kepatuhan UU PDP).

---

## 7. Model Data (Ringkas)

```
User            (id, email, nama, tgl_lahir, jenis_kelamin, tinggi_cm, aktivitas, tujuan, created_at)
Goal            (id, user_id, kalori, karbo_g, protein_g, lemak_g, serat_g, air_ml, berlaku_dari)
Food            (id, nama, nama_alt[], kategori, kalori_100g, karbo_100g, protein_100g,
                 lemak_100g, serat_100g, sumber, is_custom, owner_id?, foto_url?)
FoodUnit        (id, food_id, label, gram_per_unit)        -- mis. "1 butir" = 50 g
MealEntry       (id, user_id, tanggal, sesi, waktu, food_id, jumlah_gram, satuan_input,
                 snapshot_kalori, snapshot_karbo, snapshot_protein, snapshot_lemak, snapshot_serat)
WaterLog        (id, user_id, tanggal, jumlah_ml, waktu)
WeightLog       (id, user_id, tanggal, berat_kg)
Favorite        (user_id, food_id, last_used_at)
```

**Catatan:** `MealEntry` menyimpan *snapshot* nilai gizi saat dicatat agar riwayat tidak berubah ketika data makanan diperbarui.

---

## 8. Sumber Data Gizi
- **Utama:** Tabel Komposisi Pangan Indonesia (TKPI, Kemenkes) sebagai basis makanan lokal.
- **Pendukung:** USDA FoodData Central / Open Food Facts untuk produk kemasan dan makanan internasional.
- **Kustom:** kontribusi pengguna ditandai terpisah dan tidak mengubah data resmi.
- Setiap makanan menampilkan sumber datanya. Lisensi tiap sumber harus diverifikasi sebelum rilis.
- Cakupan MVP: minimal 1.500 makanan/minuman populer Indonesia, dengan satuan porsi lokal.

---

## 9. Desain & UX

Mengikuti **Warm Wellness & Nutrition System** (`DESIGN.md`).

### 9.1 Prinsip
- **Warm minimalist editorial**: kanvas krem, aksen karamel, biru-slate navy, hijau sage.
- Interaksi lembut dan menenangkan; hindari warna alarm, animasi berlebihan, dan konfeti.
- Aksi utama berada di **zona jempol** (40% bawah layar): FAB, tombol "Tambah", bottom sheet.

### 9.2 Token Utama
| Elemen | Nilai |
|---|---|
| Font | Plus Jakarta Sans (400–800), angka metrik memakai `metric-numeral` 22px/700 dengan tabular figures |
| Kanvas | `#FCF9F4` (surface/background) |
| Kartu | Putih `#FFFFFF`, border hairline 1px, radius 16px |
| Tombol & input | Radius 8px |
| Tag makro | Pil penuh: Protein `#D97736`/`#FDF3EC`, Lemak `#5E7343`/`#F1F5EB`, Karbo `#C88D27`/`#FAF4E8`, Hidrasi `#3D708F`/`#EDF4F8` |
| Cincin progres | Track tanah liat `#EFE8DE`, isi gradien navy → karamel |
| Margin & gutter | 1,25 rem margin, 1 rem gutter (mobile) |
| Ikon | Material Symbols Outlined |

### 9.3 Layout Responsif
| Breakpoint | Grid | Perilaku |
|---|---|---|
| Mobile (< 768px) | 4 kolom | Satu kolom; bottom nav + FAB; form sebagai bottom sheet |
| Tablet (768–1023px) | 8 kolom | Kartu ringkasan 2 kolom; daftar sesi 2 kolom |
| Desktop (≥ 1024px) | 12 kolom | Sidebar navigasi, ringkasan di kiri/atas, log makan di tengah; form sebagai modal tengah |

### 9.4 Status UI yang Wajib Ada
- **Loading**: skeleton pada kartu ringkasan dan daftar sesi.
- **Empty**: seperti "Belum ada camilan dicatat…" untuk setiap sesi kosong.
- **Error**: pesan ramah dan tombol coba lagi; entri tidak hilang saat offline (antrian sinkron).
- **Sukses**: toast navy Level 3 ("Dada Ayam Panggang ditambahkan ke Makan Siang").

### 9.5 Aksesibilitas
- Target WCAG 2.1 AA: kontras teks ≥ 4,5:1 (perhatikan teks kecil `label-sm` 10px dan teks warna ochre di atas pastel).
- Target sentuh ≥ 44×44 px (tombol hapus saat ini terlalu kecil, perlu diperbesar area ketuknya).
- Label ARIA pada tombol ikon (sudah ada pada tombol hapus), dukungan keyboard penuh di desktop, fokus ring karamel 2px.
- Informasi tidak boleh hanya bergantung pada warna (selalu sertakan angka/teks).
- Hormati `prefers-reduced-motion`.
- Zoom pengguna **tidak boleh dikunci**: hapus `maximum-scale=1.0, user-scalable=no` dari meta viewport di `code.html`.

---

## 10. Persyaratan Non-Fungsional

| Kategori | Persyaratan |
|---|---|
| **Performa** | LCP < 2,5 dtk pada 4G; INP < 200 ms; bundel JS awal < 200 KB gzip; respons pencarian makanan < 300 ms (p95). |
| **Kompatibilitas** | 2 versi terakhir Chrome, Safari, Firefox, Edge; Android 9+ dan iOS 15+. |
| **Ketersediaan** | 99,5% uptime bulanan. |
| **Offline** | Entri baru tersimpan lokal dan tersinkron otomatis saat online (v1.1). |
| **Keamanan** | HTTPS, hashing kata sandi (Argon2/bcrypt), rate limiting, proteksi CSRF/XSS, token berumur pendek. |
| **Privasi** | Data kesehatan diperlakukan sensitif: persetujuan eksplisit, enkripsi saat transit dan saat disimpan, kepatuhan **UU No. 27/2022 (PDP)**, hak ekspor dan hapus data. |
| **Skalabilitas** | Mendukung 50.000 MAU pada tahun pertama tanpa re-arsitektur. |
| **Observabilitas** | Pelacakan error (mis. Sentry), analitik produk tanpa data pribadi sensitif, log audit untuk perubahan akun. |
| **Lokalisasi** | Format angka Indonesia (`1.420`, `1,8 L`), zona waktu WIB/WITA/WIT dari perangkat. |

---

## 11. Arsitektur & Teknologi (Rekomendasi)

| Lapisan | Rekomendasi | Catatan |
|---|---|---|
| Frontend | React/Next.js + TypeScript + Tailwind CSS | Token Tailwind diambil dari `DESIGN.md`; **bundel Tailwind via build**, jangan pakai CDN di produksi (`code.html` memakai CDN untuk prototipe). |
| State & data | TanStack Query + penyimpanan lokal (IndexedDB) | Optimistic update untuk catat/hapus makanan. |
| Grafik | Recharts atau Chart.js | Untuk halaman Analisis. |
| Backend | Node.js (NestJS/Fastify) atau Supabase/Firebase | REST/GraphQL, autentikasi, sinkronisasi. |
| Database | PostgreSQL + pencarian teks (pg_trgm/Meilisearch) | Pencarian makanan toleran salah ketik. |
| Hosting | Vercel/Cloudflare + region Asia Tenggara | Latensi rendah untuk pengguna Indonesia. |
| PWA | Service worker, manifest, ikon, cache aset | Mode instal ke layar utama. |

**Logika kalkulator** sebaiknya berupa modul murni (pure functions) yang dapat diuji unit dan dipakai bersama di klien dan server.

---

## 12. Metrik Keberhasilan

| Metrik | Target (3 bulan pasca-rilis) |
|---|---|
| Aktivasi: pengguna yang mencatat ≥ 1 makanan di hari pendaftaran | ≥ 60% |
| Waktu median mencatat 1 makanan | ≤ 15 detik |
| Retensi D7 / D30 | ≥ 35% / ≥ 20% |
| Hari tercatat per pengguna aktif per minggu | ≥ 4 |
| Pencarian makanan yang menemukan hasil | ≥ 92% |
| Tingkat pembuatan makanan kustom (indikator celah data) | ≤ 10% dari entri |
| Skor Lighthouse Performance / Accessibility (mobile) | ≥ 90 / ≥ 95 |
| Kepuasan (CSAT) | ≥ 4,3 / 5 |

---

## 13. Rencana Rilis

| Fase | Cakupan | Perkiraan |
|---|---|---|
| **MVP (v1.0)** | F1–F12 (onboarding, target, dashboard, catat makanan, sesi, tanggal, hidrasi, serat, analisis dasar, makanan kustom, akun) | 8–10 minggu |
| **v1.1** | PWA offline penuh, ekspor data, pengingat, resep/kombinasi | +4–6 minggu |
| **v2.0** | Barcode & foto makanan (AI), dark mode, integrasi wearable, meal plan | Roadmap |

**Tonggak MVP:** (1) desain final & token, (2) basis data makanan siap, (3) modul kalkulator + pengujian, (4) dashboard & form catat, (5) akun & sinkronisasi, (6) analisis, (7) uji aksesibilitas/performa, (8) beta tertutup 50–100 pengguna.

---

## 14. Temuan dari Desain yang Perlu Diselesaikan

Hasil telaah `screen.png`, `code.html`, dan `DESIGN.md`:

1. **Konsistensi kalori & makro pada contoh data.** Contoh sarapan menunjukkan 420 kkal dengan P 28 g, K 45 g, L 14 g. Hitungan `4×28 + 4×45 + 9×14 = 418` (konsisten). Namun total harian 1.420 kkal belum cocok dengan makro harian (`4×95 + 4×180 + 9×38 = 1.442`, selisih kecil). Pastikan data nyata selalu dihitung dari sumber yang sama.
2. **Dua nilai warna `primary`.** `DESIGN.md` menyebut `#1B2A3A` (Deep Marine Slate) sebagai primary, sedangkan `code.html` memakai `#0F2742` untuk `primary` dan `#1B2A3A` untuk `primary-container`. Tentukan satu sumber kebenaran token. Hal yang sama berlaku pada `secondary` (`#B87A44` di teks vs `#87521F` di konfigurasi) dan `outline-variant`.
3. **Radius tombol/input.** `DESIGN.md` menyatakan `rounded-md = 0.5rem` dan kartu `rounded-lg = 1rem`, tetapi konfigurasi Tailwind di `code.html` mengubah `lg` menjadi 0.5rem. Selaraskan skala radius.
4. **Alert browser** (`alert('Sukses menambahkan…')`) harus diganti toast sesuai Level 3 di desain.
5. **Tombol sesi (filter "Semua Sesi…")** perlu perilaku jelas apakah memfilter atau men-scroll ke bagian sesi. Spesifikasi ini memakai *filter*.
6. **Teks terpotong** pada nama makanan panjang ("Oatmeal Buah B…"); sediakan tooltip/expand dan jangan memotong angka makro.
7. **Kartu makro** pada mobile sempit rawan sesak (label dan persen nyaris bertabrakan: "Karbo69%"). Perlu jarak minimum atau penyederhanaan label.
8. **Ukuran tombol hapus** terlalu kecil untuk sentuhan; tambahkan area ketuk ≥ 44 px dan undo.
9. **Satuan Porsi / Mangkok** di modal belum memengaruhi perhitungan di prototipe; harus dipetakan ke gram per makanan.
10. **Zoom dikunci** lewat meta viewport; harus dihapus untuk aksesibilitas.
11. **Ikon header** memakai nama banner "Target Harian Berjalan Baik" dengan ikon api (`local_fire_department`); pastikan ikon tidak dikesankan sebagai "streak" yang menekan.

---

## 15. Risiko & Mitigasi

| Risiko | Dampak | Mitigasi |
|---|---|---|
| Akurasi data gizi rendah | Hilangnya kepercayaan | Prioritaskan TKPI, tampilkan sumber, izinkan koreksi, review berkala. |
| Pengguna dengan riwayat gangguan makan terpicu oleh angka | Risiko kesehatan, reputasi | Bahasa netral, batas minimum kalori aman, opsi menyembunyikan angka kalori, tautan bantuan profesional, tanpa pujian berlebih untuk defisit ekstrem. |
| Retensi rendah karena pencatatan melelahkan | Produk gagal | Favorit/terbaru, salin hari kemarin, entri ≤ 15 dtk, pengingat lembut. |
| Isu privasi data kesehatan | Hukum & reputasi | Minimisasi data, persetujuan eksplisit, enkripsi, kebijakan privasi jelas (UU PDP). |
| Lisensi sumber data | Hambatan hukum | Verifikasi lisensi sebelum impor; simpan atribusi. |
| Performa lambat di jaringan seluler | Pengguna pergi | Budget performa, cache pencarian, PWA, gambar dioptimalkan (WebP/AVIF, lazy load). |

---

## 16. Disclaimer Produk
NutriTrack memberikan **estimasi** berdasarkan rumus umum dan data pangan, **bukan nasihat medis**. Pengguna dengan kondisi kesehatan khusus (diabetes, kehamilan, penyakit ginjal, riwayat gangguan makan, dsb.) dianjurkan berkonsultasi dengan dokter atau ahli gizi. Teks ini ditampilkan saat onboarding dan di halaman Profil.

---

## 17. Pertanyaan Terbuka
1. Apakah v1 mendukung login tamu penuh (tanpa akun), atau akun wajib sejak awal?
2. Model bisnis: gratis penuh, freemium (Analisis lanjutan berbayar), atau berlangganan?
3. Apakah foto makanan pada item berasal dari basis data atau unggahan pengguna?
4. Apakah diperlukan target gizi khusus (hamil/menyusui, lansia, atlet) di v1?
5. Siapa penyedia/kurator basis data makanan Indonesia, dan apa kebijakan pembaruannya?
6. Apakah pengguna boleh melihat dan mengisi hari mendatang selain 7 hari ke depan?

---

## 18. Lampiran: Contoh Perhitungan

**Profil:** wanita, 28 th, 160 cm, 58 kg, aktivitas ringan (1,375), tujuan menjaga berat.

```
BMR  = 10×58 + 6,25×160 − 5×28 − 161 = 580 + 1000 − 140 − 161 = 1.279 kkal
TDEE = 1.279 × 1,375 ≈ 1.759 kkal
Target kalori (jaga berat) = 1.759 kkal

Makro 50/25/25:
  Karbo   = 1.759 × 0,50 ÷ 4 ≈ 220 g
  Protein = 1.759 × 0,25 ÷ 4 ≈ 110 g
  Lemak   = 1.759 × 0,25 ÷ 9 ≈ 49 g
```

**Contoh entri:** Dada Ayam Rebus, 150 g, dengan basis per 100 g: 165 kkal, P 31 g, K 0 g, L 3,6 g
→ `kkal = 165 × 1,5 = 248`, `P = 46,5 g`, `K = 0 g`, `L = 5,4 g`.
