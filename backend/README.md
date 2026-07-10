# ⚙️ Backend API — TanamGizi
# Anggota 2: Backend & Database Developer

## Stack
- Framework: FastAPI (Python)
- Database: PostgreSQL
- Cache: Redis
- ORM: SQLAlchemy + Alembic (migrasi)
- Auth: JWT (python-jose)
- Server: Uvicorn

## Struktur Folder

```
backend/
├── app/
│   ├── main.py                    → Entry point FastAPI
│   ├── core/
│   │   ├── config.py              → Konfigurasi (env variables)
│   │   ├── security.py            → JWT Auth helper
│   │   └── database.py            → Koneksi PostgreSQL & Redis
│   ├── api/
│   │   └── v1/
│   │       ├── router.py          → Menggabungkan semua endpoint
│   │       └── endpoints/
│   │           ├── auth.py        → POST /login, /register
│   │           ├── users.py       → GET/PUT /users/me (profil & kondisi klinis)
│   │           ├── menu.py        → POST /menu/recommend (input bahan → rekomendasi)
│   │           ├── logs.py        → GET/POST /logs (catat & ambil riwayat makan)
│   │           └── kader.py       → GET /kader/binaan (monitoring ibu binaan)
│   ├── models/                    → Model database SQLAlchemy
│   │   ├── user.py
│   │   ├── profile.py
│   │   ├── dkbm.py
│   │   ├── recipe.py
│   │   └── meal_log.py
│   ├── schemas/                   → Schema Pydantic (validasi request/response)
│   │   ├── user.py
│   │   ├── menu.py
│   │   └── log.py
│   └── services/
│       ├── ai_client.py           → Pemanggil AI Engine (HTTP ke ai-engine)
│       └── cache_service.py       → Helper Redis Cache
├── migrations/                    → File migrasi Alembic
├── seeds/
│   └── seed_dkbm.py               → Skrip import data CSV TKPI ke PostgreSQL
├── .env.example                   → Contoh variabel environment
├── requirements.txt               → Dependensi Python
└── Dockerfile                     → Kontainerisasi backend
```

## Cara Menjalankan
```bash
cd backend

# 1. Buat virtual environment
python -m venv venv
venv\Scripts\activate  # Windows

# 2. Install dependensi
pip install -r requirements.txt

# 3. Salin dan isi file environment
cp .env.example .env

# 4. Jalankan migrasi database
alembic upgrade head

# 5. Import data DKBM (jalankan sekali saja)
python seeds/seed_dkbm.py

# 6. Jalankan server
uvicorn app.main:app --reload --port 8000
```

## Dokumentasi API (Otomatis)
Setelah server berjalan, buka:
- Swagger UI: http://localhost:8000/docs
- ReDoc: http://localhost:8000/redoc
