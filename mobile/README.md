# 📱 Mobile App — TanamGizi
# Anggota 1: Frontend Developer

## Stack
- Framework: Flutter (Dart)
- State Management: Riverpod / Provider
- Local Storage (Offline-first): Hive / Isar
- HTTP Client: Dio

## Struktur Folder

```
mobile/
├── lib/
│   ├── main.dart                  → Entry point aplikasi
│   ├── core/
│   │   ├── constants/             → Warna, ukuran font, string
│   │   ├── themes/                → Tema aplikasi (dark/light)
│   │   └── routes/                → Routing & navigasi
│   ├── data/
│   │   ├── models/                → Model data (User, Menu, Log, dll)
│   │   ├── repositories/          → Repository (abstraksi sumber data)
│   │   └── services/
│   │       ├── api_service.dart   → Koneksi ke Backend API
│   │       └── local_service.dart → Penyimpanan lokal (Hive/Isar)
│   ├── presentation/
│   │   ├── screens/
│   │   │   ├── onboarding/        → Halaman pilih kondisi klinis (T1/T2/T3/Menyusui)
│   │   │   ├── home/              → Halaman utama dashboard ibu
│   │   │   ├── input_bahan/       → Halaman input bahan makanan
│   │   │   ├── rekomendasi/       → Halaman hasil menu rekomendasi
│   │   │   ├── log_harian/        → Halaman log & tracker AKG harian
│   │   │   └── kader/             → Halaman dashboard kader posyandu
│   │   └── widgets/               → Komponen UI yang dapat dipakai ulang
│   └── providers/                 → State management (Riverpod)
├── assets/
│   ├── images/                    → Gambar & ilustrasi
│   └── fonts/                     → Font kustom
├── test/                          → Unit & widget test
└── pubspec.yaml                   → Dependensi Flutter
```

## Cara Menjalankan
```bash
cd mobile
flutter pub get
flutter run
```

## Koneksi ke Backend
Ubah base URL di `lib/core/constants/api_constants.dart`:
```dart
const String baseUrl = 'http://localhost:8000/api/v1';
```
