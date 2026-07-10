# 📂 Data — Dataset Bersama TanamGizi
> Dikelola oleh: Anggota 2 (Backend) & Anggota 3 (AI)

---

## 📊 Status Dataset (Update: 10 Juli 2026)

### Semua Dataset yang Sudah Ada

| # | Dataset | File | Ukuran | Sumber | Keterangan |
|---|---|---|---|---|---|
| 1 | **Database DKBM Final (TKPI + Folat)** | `tkpi_2020_with_folate_semantic.csv` | 261 KB | TKPI Kemenkes RI + USDA FDC | ⭐ **Data Utama Aplikasi.** 1.066 bahan pangan lokal Indonesia, dilengkapi kolom asam folat hasil pemetaan AI Semantic Matching (SentenceTransformers). |
| 2 | **DKBM Mentah (TKPI 2020)** | `tkpi_2020.csv` | 171 KB | [github.com/ancxlol/tkpi-2020-database](https://github.com/ancxlol/tkpi-2020-database) (ekstrak dari dokumen Kemenkes RI) | Arsip data mentah 1.066 bahan pangan lokal. Tanpa kolom asam folat. Gunakan file final di atas. |
| 3 | **Arsip Raw USDA SR Legacy** | `FoodData_Central_sr_legacy_food_csv_2018-04/` | ~37 MB | [fdc.nal.usda.gov](https://fdc.nal.usda.gov/download-datasets.html) (April 2018, Public Domain) | Arsip 7.794 bahan pangan USDA Amerika. Sumber nilai asam folat. Sudah diproses; dipertahankan sebagai referensi. |
| 4 | **Tabel AKG Kemenkes** | `akg/akg_permenkes_2019.json` | 17 KB | Permenkes No. 28 Tahun 2019 + PDF dokumen resmi | Target kebutuhan gizi harian (Base, Additions, & 10 Kombinasi Computed) untuk semua fase ibu hamil & menyusui. Sudah mencakup Makro, Vitamin, dan Mineral. |
| 5 | **Batas Atas Aman (UL)** | `akg/akg_upper_limit.json` | 3 KB | WHO / IOM (Institute of Medicine) Reports | Batas atas maksimum asupan harian yang aman untuk mencegah over-nutrition atau toksisitas (misal: Vitamin A retinol maks 3.000 mcg). |
| 6 | **Faktor Bioavailabilitas** | `akg/bioavailability_factors.json` | 4 KB | WHO / FAO Joint Expert Report | Faktor konversi penyerapan nyata oleh tubuh untuk: Zat Besi (heme vs non-heme), Kalsium (oksalat), Seng, dan Vitamin A (beta-karoten → RAE). |
| 7 | **Faktor Retensi Memasak** | `akg/retention_factors.json` | 3 KB | USDA Nutrient Retention Factors, Release 6 | Faktor penyusutan gizi akibat proses memasak (misal: merebus bayam mengurangi folat hingga ~50%). Menggantikan kebutuhan file CSV mentah dari USDA. |
| 8 | **Dokumen Resmi AKG (PDF)** | `akg/Permenkes-No-28-Tahun-2019-Angka-Kecukupan-Gizi-yang-Dianjurkan.pdf` | 472 KB | Kementerian Kesehatan RI | Dokumen hukum primer yang menjadi dasar seluruh angka di `akg_permenkes_2019.json`. |
| 9 | **Contoh Resep Lokal** | `recipes/resep_lokal.json` | 2 KB | Tim TanamGizi (Manual) | 3 contoh resep awal sebagai template. **Perlu dilengkapi minimal 50 resep.** Lihat instruksi di bawah. |

---

## 📁 Struktur Folder Saat Ini

```
data/
├── tkpi_2020_with_folate_semantic.csv     ⭐ DATABASE FINAL — Gunakan ini untuk aplikasi
├── tkpi_2020.csv                          📦 Arsip mentah DKBM Kemenkes RI
├── FoodData_Central_sr_legacy_food_csv_2018-04/
│   ├── food.csv                           📦 Arsip 7.794 nama bahan USDA
│   ├── food_nutrient.csv                  📦 Arsip nilai gizi USDA (36 MB)
│   └── nutrient.csv                       📦 Kamus ID nutrisi (Folate ID = 1177)
├── akg/
│   ├── akg_permenkes_2019.json            ✅ Target AKG per fase (Makro + Vitamin + Mineral)
│   ├── akg_upper_limit.json               ✅ Batas aman maksimum asupan harian
│   ├── bioavailability_factors.json       ✅ Faktor penyerapan usus (WHO/FAO)
│   ├── retention_factors.json             ✅ Faktor penyusutan gizi saat dimasak (USDA)
│   └── Permenkes-No-28-Tahun-2019-*.pdf  ✅ Dokumen resmi hukum (sumber primer AKG)
├── recipes/
│   └── resep_lokal.json                   ⚠️  PERLU DILENGKAPI — Baru 3 contoh
└── README.md
```

---

## ⚠️ Satu-Satunya Tugas yang Tersisa

### Tugas Anggota 2 (Backend Dev) — Lengkapi `recipes/resep_lokal.json`

Tambahkan minimal **50 resep masakan lokal** bergizi untuk ibu hamil ke dalam file yang sudah ada. Ikuti format yang sudah ada di sana, dan pastikan `nama_dkbm` selalu mengacu pada kolom `clean_name` di file database utama `tkpi_2020_with_folate_semantic.csv`.

**Cara cepat menemukan nama yang benar:**
Buka `tkpi_2020_with_folate_semantic.csv`, cari kolom `clean_name`, dan salin nama persis dari sana.

**Referensi resep yang direkomendasikan:**
- Buku PMT (Pemberian Makanan Tambahan) Puskesmas Kemenkes RI
  - Keyword Google: `"Buku Resep Makanan Lokal Balita dan Ibu Hamil Kemenkes 2023 PDF"`
- Portal resmi Kemenkes: `ayosehat.kemkes.go.id` → Cari "Resep PMT Ibu Hamil"

---

## 🔗 Sumber Data Resmi

| Dataset | Sumber | Lisensi |
|---|---|---|
| TKPI 2020 (DKBM) | Kementerian Kesehatan RI / github.com/ancxlol/tkpi-2020-database | Open / Publik |
| USDA SR Legacy 2018 | fdc.nal.usda.gov | Public Domain (US Government) |
| AKG Kemenkes | Permenkes No. 28 Tahun 2019 | Dokumen resmi pemerintah |
| Upper Limit (UL) | WHO / IOM Reports | Global Medical Guidelines |
| Bioavailabilitas | WHO / FAO Joint Expert Consultation | Global Medical Guidelines |
| Faktor Retensi Memasak | USDA Nutrient Retention Factors, Release 6 | Public Domain (US Government) |
| Resep PMT | Buku PMT Kemenkes RI | Dokumen resmi pemerintah |
