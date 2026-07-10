# 📂 Data — Dataset Bersama TanamGizi
# Dikelola oleh: Anggota 2 (Backend) & Anggota 3 (AI)

---

## 📊 Status Dataset (Update: 09 Juli 2026)

| # | Dataset | File | Status | Keterangan |
|---|---|---|---|---|
| 1 | **Database Final (TKPI + Folat)** | `tkpi_2020_with_folate_semantic.csv` | ✅ **SUDAH LENGKAP** | 1.066 bahan pangan lokal beserta hasil pemetaan Asam Folat (USDA) menggunakan AI Semantic Matching. **(Ini adalah data utama untuk aplikasi)** |
| 2 | **Resep & Menu Lokal** | `recipes/resep_lokal.json` | ❌ **BELUM ADA** | Dibuat manual oleh tim dari buku PMT Kemenkes |
| 3 | **Tabel AKG** | `akg/akg_permenkes_2019.json` | ✅ **SUDAH LENGKAP** | Berisi Base, Additions, & 10 Kombinasi Computed (Makro, Vitamin, Mineral) |
| 4 | **Upper Limit (UL)** | `akg/akg_upper_limit.json` | ✅ **SUDAH LENGKAP** | Batas aman asupan harian maksimum (WHO/IOM) |
| 5 | **Bioavailabilitas** | `akg/bioavailability_factors.json` | ✅ **SUDAH LENGKAP** | Faktor penyerapan zat besi, kalsium, seng, Vit A (WHO/FAO) |
| 6 | **Retensi Memasak** | `akg/retention_factors.json` | ✅ **SUDAH LENGKAP** | Pengurangan nilai gizi akibat proses memasak (USDA Release 6) |
| 7 | **Arsip Raw Data** | `tkpi_2020.csv` & `FoodData_Central/` | ✅ **SUDAH ADA** | Arsip data mentah sebelum digabung oleh AI |

---

## 📁 Struktur Folder (Target Akhir)

```
data/
├── tkpi_2020_with_folate_semantic.csv     ✅ SUDAH ADA — Database Final untuk Aplikasi!
├── tkpi_2020.csv                          ✅ SUDAH ADA — Arsip mentah 1.066 bahan
├── FoodData_Central_sr_legacy_food_csv_2018-04/  ✅ SUDAH ADA — Arsip mentah USDA
├── recipes/
│   └── resep_lokal.json                   ❌ BELUM — dibuat manual tim
├── akg/
│   ├── akg_permenkes_2019.json            ✅ SUDAH — target & computed AKG lengkap
│   ├── akg_upper_limit.json               ✅ SUDAH — batas aman asupan maksimum
│   ├── bioavailability_factors.json       ✅ SUDAH — faktor penyerapan usus
│   └── retention_factors.json             ✅ SUDAH — faktor penyusutan gizi saat masak
└── README.md
```

---

## ✅ Yang Sudah Didapat (Fase Database Selesai)

Kita telah menyelesaikan proses **Data Engineering**. Hasil akhirnya adalah file `tkpi_2020_with_folate_semantic.csv`. File ini merupakan gabungan dari data makronutrien lokal (TKPI Kemenkes RI) dengan data mikronutrien Asam Folat internasional (USDA). Proses penggabungan (pemetaan) dilakukan menggunakan AI Semantic Matching (SentenceTransformers) sehingga sangat akurat.

File-file referensi medis di dalam folder `akg/` juga sudah 100% lengkap dan siap digunakan oleh sistem Rekomendasi Menu (AI Engine).

---

## ❌ Yang Masih Kurang (Action Items)

🎉 **Tugas Anggota 3 (AI Dev) untuk persiapan data sudah SELESAI.**

### Tugas Anggota 2 (Backend Dev) — Buat `resep_lokal.json`
Buat minimal **50 resep masakan lokal** dengan format berikut di `data/recipes/resep_lokal.json`:

```json
[
  {
    "id": 1,
    "nama_menu": "Sayur Bening Bayam Tempe",
    "kecocokan_klinis": ["hamil_t1", "hamil_t2", "hamil_t3", "menyusui"],
    "langkah_memasak": "1. Didihkan air... 2. Masukkan bayam...",
    "bahan": [
      { "nama_dkbm": "bayam mentah", "jumlah_gram": 100 },
      { "nama_dkbm": "tempe kedelai murni mentah", "jumlah_gram": 50 },
      { "nama_dkbm": "bawang merah mentah", "jumlah_gram": 10 }
    ]
  }
]
```

**Referensi resep:** Buku PMT (Pemberian Makanan Tambahan) Puskesmas / Kemenkes RI.
*(Pastikan `nama_dkbm` sama persis dengan yang ada di kolom `clean_name` pada file CSV `tkpi_2020_with_folate_semantic.csv` agar AI Engine bisa membaca datanya).*

---

## 🔗 Sumber Data Resmi

| Dataset | Sumber | Lisensi |
|---|---|---|
| TKPI 2020 | Ekstrak dari dokumen Kemenkes RI | Open / Publik |
| USDA SR Legacy | fdc.nal.usda.gov | Public domain (US Government) |
| AKG Kemenkes | Permenkes No. 28 Tahun 2019 | Dokumen resmi pemerintah |
| Resep PMT | Buku PMT Kemenkes RI | Dokumen resmi pemerintah |
| Upper Limit & Bioavailability | WHO / FAO / IOM Reports | Global Medical Guidelines |
