# SIMUTEH - Praktikum P5-P6 Computer Vision
**Nama:** Aleya Giska Andini  
**NIM:** 2330521015  
**Kelas:** nonreg  
**SEED:** 1015  

## Studi Kasus
Sistem Pemeriksaan Mutu Daun Teh Berbasis Citra untuk Kelompok Tani Teh Rakyat di Kabupaten Sukabumi.

## Cara menjalankan
1. `pip install -r requirements.txt`
2. Dari folder utama: `python src/buat_dataset_sintetis.py` (jika data belum ada)
3. Jalankan notebook di folder `notebook/` dari awal sampai akhir (Restart & Run All).

## Ringkasan hasil P5-P6
- **Kanal terpilih untuk tepi & bercak:** R (kanal 2)
- **Operator tepi terbaik:** Canny (sigma=2, ambang 30/90) pada kanal R sesudah praproses, F1 ~0.7+
- **Keputusan tepi:** Dipakai untuk menemukan batas daun (gradien morfologi lebih baik) dan batas bercak (Canny); tidak untuk mengisi wilayah atau fitur bentuk utama.
- **SE morfologi:** elips 5x5, opening lalu closing, k=4.0
- **Penyaringan:** luas_min=20, bulat_min=0.5, rasio_maks=1.8
- **Evaluasi modul bercak.py (60 citra):**
  - Galat absolut rerata persen bercak: ~0.28
  - Korelasi persen prediksi vs acuan: ~0.98
  - Akurasi kelas: ~0.88
- Tag: `v0.2-tepi`, `v0.3-morfologi`

## Modul utama
- `src/praproses.py` — rangkaian praproses (dari P1-P4)
- `src/utils_tepi.py` — evaluasi tepi & mask awal
- `src/bercak.py` — analisis bercak lengkap (bersihkan, ukur, saring, kelas)
- `hasil/param_praproses.json`, `hasil/param_bercak.json`

## Pemakaian analisis_daun
```python
from praproses import praproses
from bercak import analisis_daun
import json
PARAM = json.load(open("hasil/param_praproses.json"))
h = analisis_daun(praproses(img_bgr, PARAM))
print(h["jumlah_bercak"], h["persen_bercak"], h["kelas"])
```
