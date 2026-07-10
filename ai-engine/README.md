# 🧠 AI Engine — TanamGizi
# Anggota 3: AI/ML Developer

## Stack
- Language: Python 3.11+
- Framework API: FastAPI (microservice internal)
- NLP: RapidFuzz (fuzzy matching nama bahan)
- ML/Rekomendasi: Scikit-learn (Content-Based Filtering)
- Kalkulasi Gizi: Pandas + NumPy
- Notebook Riset: Jupyter

## Struktur Folder

```
ai-engine/
├── app/
│   ├── main.py                    → Entry point FastAPI AI microservice
│   └── api/
│       └── endpoints/
│           └── recommend.py       → POST /recommend (dipanggil oleh backend)
├── engine/
│   ├── nlp_parser/
│   │   ├── parser.py              → Normalisasi input teks bahan makanan
│   │   └── synonym_dict.json      → Kamus sinonim (tempe goreng → tempe, bayem → bayam)
│   ├── akg_calculator/
│   │   ├── calculator.py          → Hitung target AKG per kondisi klinis
│   │   └── akg_table.json         → Tabel AKG resmi (Permenkes No.28/2019)
│   └── recommendation/
│       ├── engine.py              → Algoritma Content-Based Filtering (CBF)
│       └── vectorizer.py          → Mengubah profil gizi bahan menjadi vektor angka
├── notebooks/
│   ├── 01_explorasi_dkbm.ipynb    → Riset & eksplorasi data DKBM
│   ├── 02_nlp_parser_test.ipynb   → Testing algoritma NLP Parser
│   └── 03_cbf_prototype.ipynb     → Prototyping algoritma rekomendasi CBF
├── tests/
│   └── test_recommendation.py     → Unit test akurasi rekomendasi
├── requirements.txt
└── Dockerfile
```

## Cara Menjalankan
```bash
cd ai-engine

# 1. Buat virtual environment
python -m venv venv
venv\Scripts\activate  # Windows

# 2. Install dependensi
pip install -r requirements.txt

# 3. Jalankan AI microservice
uvicorn app.main:app --reload --port 8001
```

## Endpoint Internal
AI Engine ini dipanggil **secara internal** oleh Backend (bukan dari Mobile langsung).

| Method | Endpoint | Input | Output |
|---|---|---|---|
| POST | `/recommend` | `{bahan: [...], kondisi: "hamil_t2"}` | `{menus: [{nama, resep, total_gizi}]}` |

## Alur Kerja Rekomendasi

```
Input: ["tempe", "bayam", "ikan"]
    ↓
[NLP Parser] → normalisasi & cocokkan ke DKBM ID
    ↓
[AKG Calculator] → hitung target gizi (kondisi: hamil_t2)
    ↓
[Recommendation Engine CBF] → cari menu yang paling memenuhi target AKG
    ↓
Output: 3 menu rekomendasi + resep + % pemenuhan AKG
```
