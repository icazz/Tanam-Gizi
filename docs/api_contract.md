# Kontrak API TanamGizi
# Mobile App ↔ Backend ↔ AI Engine

Base URL Backend : http://localhost:8000/api/v1
Base URL AI Engine (Internal): http://localhost:8001

---

## AUTH

### POST /auth/register
Request:
{
  "username": "string",
  "password": "string",
  "role": "ibu" | "kader"
}
Response:
{
  "access_token": "jwt_token_string",
  "token_type": "bearer"
}

### POST /auth/login
Request:
{
  "username": "string",
  "password": "string"
}
Response:
{
  "access_token": "jwt_token_string",
  "token_type": "bearer"
}

---

## USER PROFILE

### GET /users/me
Header: Authorization: Bearer <token>
Response:
{
  "id": 1,
  "username": "sari_ibu",
  "role": "ibu",
  "profile": {
    "kondisi": "hamil_t2",
    "berat_badan": 58.5,
    "tinggi_badan": 160.0,
    "target_kalori": 2250,
    "target_protein": 70.0,
    "target_zat_besi": 36.0,
    "target_asam_folat": 600.0
  }
}

### PUT /users/me/profile
Header: Authorization: Bearer <token>
Request:
{
  "kondisi": "hamil_t1" | "hamil_t2" | "hamil_t3" | "menyusui_0_6" | "menyusui_6_24",
  "berat_badan": 58.5,
  "tinggi_badan": 160.0
}

---

## MENU RECOMMENDATION

### POST /menu/recommend
Header: Authorization: Bearer <token>
Request:
{
  "bahan": ["tempe", "bayam", "ikan", "tahu"]
}
Response:
{
  "rekomendasi": [
    {
      "nama_menu": "Sayur Bening Bayam Tempe",
      "langkah_memasak": "1. Didihkan air...",
      "total_gizi": {
        "energi_kcal": 280,
        "protein_g": 22.5,
        "zat_besi_mg": 4.2,
        "asam_folat_mcg": 180.0,
        "kalsium_mg": 210.0
      },
      "pemenuhan_akg_persen": {
        "energi": 62,
        "protein": 75,
        "zat_besi": 80,
        "asam_folat": 65,
        "kalsium": 55
      }
    }
  ]
}

---

## MEAL LOG

### POST /logs
Header: Authorization: Bearer <token>
Request:
{
  "nama_menu": "Sayur Bening Bayam Tempe",
  "porsi": 1.0,
  "total_kalori": 280,
  "total_protein": 22.5,
  "total_zat_besi": 4.2,
  "total_asam_folat": 180.0
}

### GET /logs/today
Header: Authorization: Bearer <token>
Response:
{
  "tanggal": "2026-07-09",
  "logs": [...],
  "rekapitulasi": {
    "total_kalori": 1850,
    "pemenuhan_akg_persen": {
      "kalori": 82,
      "protein": 70,
      "zat_besi": 65,
      "asam_folat": 88
    }
  }
}

---

## KADER DASHBOARD

### GET /kader/binaan
Header: Authorization: Bearer <token> (role: kader)
Response:
{
  "binaan": [
    {
      "nama": "Sari",
      "kondisi": "hamil_t2",
      "rerata_pemenuhan_akg_7hari": {
        "kalori": 75,
        "protein": 60,
        "zat_besi": 55
      },
      "status": "perlu_perhatian"
    }
  ]
}
