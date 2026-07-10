# 📱 TanamGizi (Seribu Gizi) — Rangkuman Aplikasi & Arsitektur Sistem

---

## 🌱 Latar Belakang

**TanamGizi** adalah aplikasi mobile rekomendasi menu berbasis pangan lokal yang dirancang khusus untuk **ibu hamil dan ibu menyusui** dalam rangka mendukung pencegahan stunting pada **1000 Hari Pertama Kehidupan (1000 HPK)**.

### Mengapa 1000 HPK?

| Periode | Durasi | Signifikansi |
|---|---|---|
| Dalam kandungan | 270 hari | Otak berkembang 80%, pola metabolisme ditetapkan |
| Setelah lahir s.d. 2 tahun | 730 hari | Masa ASI + MPASI, sel-sel tubuh terbentuk |
| **Total 1000 HPK** | **~3 tahun** | **Window of opportunity yang tidak bisa diulang** |

> **Stunting tidak terlihat saat bayi baru lahir.** Ia terbentuk diam-diam di dapur ibu, di piring yang tidak bergizi, selama 1000 hari pertama — dan baru terlihat ketika sudah terlambat.


## 🎯 Target Pengguna

```
PRIMARY USER:    Ibu hamil & ibu menyusui
                 → Bisa digunakan mandiri, kapan saja, di mana saja
                 → UX dirancang untuk literasi digital RENDAH

SECONDARY USER:  Kader posyandu
                 → Dashboard memantau ibu binaannya
                 → Merekomendasikan menu ke ibu tertentu
```

**Scope kondisi klinis yang dicakup:**
- Ibu hamil Trimester 1 — kebutuhan zat besi tinggi, rentan mual
- Ibu hamil Trimester 2 — pertumbuhan plasenta optimal
- Ibu hamil Trimester 3 — kebutuhan kalori lebih tinggi
- Ibu menyusui eksklusif (0–6 bulan) — protein dan cairan tinggi
- Ibu menyusui + MPASI (6–24 bulan) — panduan tekstur dan nutrisi bayi

---

## ✨ Fitur Utama (MVP)

| Fitur | Deskripsi |
|---|---|
| **Onboarding Berbasis Klinis** | Profil otomatis sesuai fase (Hamil T1/T2/T3 atau Menyusui) untuk menyesuaikan target AKG |
| **Smart Input Bahan Lokal** | Ibu memasukkan bahan di dapur (tempe, kangkung, ikan) → sistem merekomendasikan menu bergizi |
| **AI Menu Recommendation** | Menghasilkan 2–3 rekomendasi menu harian + panduan masak yang memenuhi AKG per kondisi |
| **Visualisasi Log Harian (AKG Tracker)** | Riwayat makan harian dikonversi ke % pemenuhan zat besi, asam folat, protein, dll. |
| **Kader Dashboard** | Monitoring dan rekomendasi menu untuk ibu-ibu binaan kader posyandu |


---

## 🏗️ Arsitektur Sistem — Penjelasan Detail

Arsitektur TanamGizi menggunakan pendekatan **berlapis (layered architecture)** dengan 4 lapisan utama yang terhubung secara vertikal. Setiap lapisan memiliki tanggung jawab yang jelas dan terpisah.

```
┌──────────────────────────────────────────────┐
│         LAYER 1: Mobile App                  │
│         (Flutter / React Native)             │
├──────────────────────────────────────────────┤
│         LAYER 2: API Gateway                 │
│         (Auth · Rate Limiting · Routing)     │
├──────────────────────────────────────────────┤
│         LAYER 3: Backend Services            │
│     User │ Menu │ Log │ Kader Dashboard      │
├──────────────────────────────────────────────┤
│         LAYER 4: AI / ML Engine (Python)     │
│  Recommendation │ NLP Parser │ AKG Calculator│
├──────────────────────────────────────────────┤
│         LAYER 5: Data Layer                  │
│    PostgreSQL │ DKBM Database │ Redis Cache  │
└──────────────────────────────────────────────┘
```

---

### 📱 LAYER 1 — Mobile App (Flutter / React Native)

**Peran:** Antarmuka langsung yang dipegang pengguna.

| Aspek | Detail |
|---|---|
| **Framework** | Flutter (Dart) *atau* React Native (JavaScript) |
| **Filosofi UX** | *Offline-first* — tetap bisa diakses meski sinyal buruk |
| **Target literasi** | Rendah — tombol besar, bahasa sederhana, ikon visual |
| **Dua mode tampilan** | Mode Ibu (panduan menu harian) + Mode Kader (monitoring binaan) |

**Mengapa offline-first penting?**
Pengguna utama berada di daerah pedesaan/semi-urban dengan koneksi internet tidak stabil. Data rekomendasi menu di-cache secara lokal agar tetap bisa diakses tanpa internet.

**Pilihan Framework:**
- **Flutter** → 1 codebase untuk Android & iOS, performa mendekati native, UI kustom kaya
- **React Native** → berbagi logika dengan web, ekosistem JavaScript lebih luas

---

### 🔐 LAYER 2 — API Gateway

**Peran:** Pintu masuk tunggal (*single entry point*) ke semua backend service.

| Fungsi | Penjelasan |
|---|---|
| **Auth (JWT)** | Setiap request wajib membawa token JWT yang valid. Token berisi identitas user (ibu / kader) dan masa berlakunya |
| **Rate Limiting** | Membatasi jumlah request per menit per user, mencegah penyalahgunaan dan serangan DDoS |
| **Request Routing** | Meneruskan request ke service yang tepat: `/user` → User Service, `/menu` → Menu Service, dst |

**Analogi:** API Gateway seperti resepsionis gedung — semua tamu (request) harus lapor dulu, baru diarahkan ke ruangan yang tepat.

---

### ⚙️ LAYER 3 — Backend Services (FastAPI / Django REST)

Terdiri dari **4 microservice** yang berjalan independen:

#### 🧑‍💼 User Service — *Profil kondisi trimester/fase*
- Menyimpan dan mengelola profil pengguna
- Mencatat kondisi klinis: trimester kehamilan atau fase menyusui
- Menghitung target AKG personal berdasarkan kondisi
- Contoh data: `{user_id, nama, usia, kondisi: "hamil_trimester_2", berat_badan, tinggi_badan}`

#### 🍽️ Menu Service — *Input bahan → rekomendasi menu*
- Menerima daftar bahan makanan yang diinput ibu
- Memanggil AI/ML Engine untuk mendapatkan rekomendasi
- Mengembalikan 2–3 opsi menu lengkap beserta resep dan panduan memasak
- Contoh flow: `["tempe", "kangkung", "ikan"] → [Menu A, Menu B, Menu C]`

#### 📊 Log Service — *Tracking pemenuhan AKG harian*
- Mencatat setiap menu yang dikonsumsi beserta porsi
- Menghitung akumulasi zat gizi: zat besi, asam folat, protein, kalsium, dll.
- Menyimpan riwayat untuk visualisasi progress mingguan/bulanan
- Contoh output: `{zat_besi: 78%, asam_folat: 65%, protein: 90%}`

#### 👩‍⚕️ Kader Dashboard — *Monitoring ibu binaan*
- Endpoint khusus untuk pengguna berperan kader posyandu
- Menampilkan daftar ibu binaan beserta status pemenuhan AKG masing-masing
- Memungkinkan kader mengirim rekomendasi menu ke ibu tertentu
- Alert otomatis jika ada ibu binaan yang konsistensi gizinya menurun

---

### 🤖 LAYER 4 — AI / ML Engine (Python)

Otak dari aplikasi. Terdiri dari **3 komponen** yang bekerja bersama:

#### 🎯 Recommendation Engine — *CBF + Content-Based Filtering per kondisi*
**Teknologi:** Content-Based Filtering (CBF)

**Cara kerja:**
1. Menerima input bahan dari Menu Service
2. Membandingkan profil nutrisi bahan yang tersedia dengan kebutuhan AKG user
3. Menggunakan algoritma CBF untuk menemukan kombinasi bahan yang paling memenuhi kebutuhan gizi
4. Mempertimbangkan kondisi klinis: trimester atau fase menyusui

**Contoh logika:**
```
User: Hamil Trimester 1
Input bahan: [tempe, bayam, tahu, ikan]
Target: Zat besi tinggi, folat tinggi, protein cukup
Output: Menu "Tumis Bayam Tempe + Ikan Goreng" (zat besi 85% AKG)
```

#### 🔤 NLP Ingredient Parser — *Normalisasi input bahan lokal → DKBM lookup*
**Teknologi:** Natural Language Processing (NLP)

**Masalah yang diselesaikan:**
Ibu tidak akan mengetik "Spinacia oleracea" — mereka akan mengetik "bayem", "bayam hijau", "sayur bayam", atau bahkan salah eja "bayang".

**Cara kerja:**
1. Menerima teks bebas yang diketik ibu
2. Menormalisasi variasi penulisan, singkatan, atau bahasa daerah
3. Memetakan ke nama standar di database DKBM
4. Contoh: `"tempe goreng"` → normalisasi → `"tempe kedelai"` → DKBM lookup → `{protein: 18.3g, zat_besi: 2.7mg per 100g}`

#### 📐 AKG Calculator — *Kalkulasi per trimester/fase menyusui*
**Sumber data resmi:** AKG Kemenkes (Permenkes No. 28 Tahun 2019)

**Cara kerja:**
1. Menerima profil pengguna (kondisi, usia, berat badan)
2. Menghitung kebutuhan gizi harian yang spesifik per kondisi
3. Menjadi referensi target yang digunakan Recommendation Engine

**Contoh kalkulasi AKG:**
| Kondisi | Zat Besi | Asam Folat | Protein | Kalsium |
|---|---|---|---|---|
| Ibu normal | 26 mg | 400 mcg | 56 g | 1000 mg |
| Hamil T1 | 26 mg | 600 mcg | 60 g | 1000 mg |
| Hamil T2/T3 | 36 mg | 600 mcg | 70 g | 1200 mg |
| Menyusui 0–6 bln | 32 mg | 500 mcg | 75 g | 1200 mg |

---

### 🗄️ LAYER 5 — Data Layer

Tiga komponen penyimpanan dengan fungsi berbeda:

#### 🐘 PostgreSQL — *Users, logs, profil*
- Database relasional utama
- Menyimpan: data user, profil kondisi klinis, riwayat log makan, data kader, relasi kader-ibu binaan
- Alasan dipilih: ACID-compliant, cocok untuk data terstruktur yang perlu integritas tinggi

#### 🥦 DKBM Database — *Data komposisi bahan makanan*
- Daftar Komposisi Bahan Makanan resmi Kemenkes RI
- Berisi nilai gizi ratusan bahan pangan lokal Indonesia
- Menjadi sumber kebenaran tunggal untuk semua kalkulasi gizi
- Contoh isi: `bayam merah → zat besi 2.2mg, asam folat 140mcg, protein 2.3g per 100g`

#### ⚡ Redis Cache — *Menu populer, sesi*
- In-memory database untuk kecepatan tinggi
- Menyimpan: sesi login user (JWT), menu-menu yang sering diakses (hot cache), hasil rekomendasi yang sudah pernah dihitung
- Manfaat: respons lebih cepat, beban ke PostgreSQL berkurang signifikan

---

## 🔄 Alur Data End-to-End (Contoh: Input Bahan → Rekomendasi Menu)

```
1. [Mobile App]
   Ibu mengetik: "tempe, bayam, ikan, tahu"
   ↓
2. [API Gateway]
   Validasi JWT token → route ke Menu Service
   ↓
3. [Menu Service]
   Ambil profil user dari User Service
   Kirim bahan + kondisi ke AI Engine
   ↓
4. [NLP Ingredient Parser]
   "tempe" → DKBM ID: 12045
   "bayam" → DKBM ID: 30012
   "ikan" → DKBM ID: 20033
   ↓
5. [AKG Calculator]
   User: Hamil T2 → Target: zat besi 36mg, folat 600mcg
   ↓
6. [Recommendation Engine]
   CBF: cari kombinasi menu yang memenuhi target AKG
   Output: 3 opsi menu + resep sederhana
   ↓
7. [Redis Cache]
   Simpan hasil rekomendasi (cache 24 jam)
   ↓
8. [Mobile App]
   Tampilkan: "Menu A: Tumis Bayam Tempe + Ikan Kuah Jahe"
   + persentase AKG terpenuhi
```

---

## 🗺️ Roadmap Pengembangan (Saran)

| Sprint | Fokus | Output |
|---|---|---|
| Sprint 1 | Setup project + auth + user profil | Onboarding selesai |
| Sprint 2 | DKBM import + NLP parser dasar | Input bahan berfungsi |
| Sprint 3 | AKG Calculator + Recommendation Engine | Rekomendasi menu muncul |
| Sprint 4 | Log harian + visualisasi AKG | Tracker berjalan |
| Sprint 5 | Kader dashboard + UI polish | MVP siap demo |

---

## 📚 Dasar Hukum & Referensi Resmi

- **DKBM Kemenkes** — Daftar Komposisi Bahan Makanan resmi Indonesia
- **AKG Kemenkes** — Permenkes No. 28 Tahun 2019 tentang Angka Kecukupan Gizi
- **Perpres No. 42/2013** — Gerakan Nasional Percepatan Perbaikan Gizi (landasan program 1000 HPK)
- **SSGI 2024** — Survei Status Gizi Indonesia, prevalensi stunting 19,8%

---

*Dokumen ini dibuat berdasarkan [tanam gizi.md.md](file:///c:/Users/Asus/Documents/project/tanam-gizi/tanam%20gizi.md.md) dan screenshot arsitektur sistem TanamGizi.*
