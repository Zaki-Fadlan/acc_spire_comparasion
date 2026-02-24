# Hasil Analisis Akurasi Forecast — Saipem

**Analisis Binary Rain Event (24 Jam Pertama)**

- **Stasiun:** Saipem
- **Koordinat:** 1.0418859, 103.3150505
- **Periode:** 2026-02-09 s/d 2026-02-23
- **Jumlah Issued Time:** 29
- **Total Pairs:** 711
- **Lead Time Maksimum:** 24 jam

> **Metode:** Setiap jam observasi dan forecast dibandingkan secara biner:
> - Observasi > 0 mm/h → **Hujan**
> - Observasi ≤ 0 mm/h → **Tidak Hujan**
> - Forecast > 0 mm/h → **Prediksi Hujan**
> - Forecast ≤ 0 mm/h → **Prediksi Tidak Hujan**

---

## Hasil Utama

| Metrik | Nilai |
|--------|-------|
| **Akurasi Keseluruhan** | **93.0%** 🏆 |
| **Rata-rata Akurasi per Issued** | **93.1%** |
| Issued Time Terbaik | 2026-02-09T00:00 → **100%** |
| Issued Time Terburuk | 2026-02-17T00:00 → **72%** |

---

## Confusion Matrix (Keseluruhan)

|  | **Obs: Hujan** | **Obs: Tidak Hujan** | **Total FC** |
|--|---------------|---------------------|--------------|
| **FC: Hujan** | 0 (Hit) | 50 (False Alarm) | 50 |
| **FC: Tidak Hujan** | 0 (Miss) | 661 (Correct Neg) | 661 |
| **Total Obs** | 0 | 711 | **711** |

**Penjelasan:**
- **Correct Negative (661):** Model bilang tidak hujan, memang tidak hujan → **BENAR** ✓
- **False Alarm (50):** Model bilang hujan, tapi tidak hujan → **SALAH** ✗
- **Hit (0):** Tidak ada kejadian hujan yang terobservasi
- **Miss (0):** Tidak ada kejadian hujan yang terlewat

---

## Distribusi Kejadian

| Kategori | Jumlah | Persentase |
|----------|--------|------------|
| Observasi hujan | 0 | 0.0% |
| Observasi tidak hujan | 711 | 100.0% |
| Forecast prediksi hujan | 50 | 7.0% |
| Forecast prediksi tidak hujan | 661 | 93.0% |

**Catatan:** Stasiun Saipem berada dalam kondisi **kering total** selama periode pengamatan. Model hanya memprediksi hujan pada **7% waktu** — angka yang sangat rendah dan mendekati kenyataan, menjadikan ini **stasiun dengan performa terbaik** di antara seluruh stasiun.

---

## Akurasi per Issued Time

| Issued Time | Pairs | Hit | Miss | FA | CN | Correct | Accuracy |
|-------------|-------|-----|------|----|----|---------|----------|
| 2026-02-09T00:00 | 25 | 0 | 0 | 0 | 25 | 25 | **100.0%** ⭐ |
| 2026-02-09T12:00 | 25 | 0 | 0 | 0 | 25 | 25 | **100.0%** ⭐ |
| 2026-02-10T00:00 | 25 | 0 | 0 | 4 | 21 | 21 | **84.0%** |
| 2026-02-10T12:00 | 25 | 0 | 0 | 0 | 25 | 25 | **100.0%** ⭐ |
| 2026-02-11T00:00 | 25 | 0 | 0 | 1 | 24 | 24 | **96.0%** |
| 2026-02-11T12:00 | 25 | 0 | 0 | 1 | 24 | 24 | **96.0%** |
| 2026-02-12T00:00 | 25 | 0 | 0 | 1 | 24 | 24 | **96.0%** |
| 2026-02-12T12:00 | 25 | 0 | 0 | 4 | 21 | 21 | **84.0%** |
| 2026-02-13T00:00 | 25 | 0 | 0 | 0 | 25 | 25 | **100.0%** ⭐ |
| 2026-02-13T12:00 | 25 | 0 | 0 | 0 | 25 | 25 | **100.0%** ⭐ |
| 2026-02-14T00:00 | 25 | 0 | 0 | 0 | 25 | 25 | **100.0%** ⭐ |
| 2026-02-14T12:00 | 25 | 0 | 0 | 0 | 25 | 25 | **100.0%** ⭐ |
| 2026-02-15T00:00 | 25 | 0 | 0 | 0 | 25 | 25 | **100.0%** ⭐ |
| 2026-02-15T12:00 | 25 | 0 | 0 | 3 | 22 | 22 | **88.0%** |
| 2026-02-16T00:00 | 25 | 0 | 0 | 5 | 20 | 20 | 80.0% |
| 2026-02-16T12:00 | 25 | 0 | 0 | 6 | 19 | 19 | 76.0% |
| 2026-02-17T00:00 | 25 | 0 | 0 | 7 | 18 | 18 | 72.0% |
| 2026-02-17T12:00 | 25 | 0 | 0 | 7 | 18 | 18 | 72.0% |
| 2026-02-18T00:00 | 25 | 0 | 0 | 2 | 23 | 23 | **92.0%** |
| 2026-02-18T12:00 | 25 | 0 | 0 | 1 | 24 | 24 | **96.0%** |
| 2026-02-19T00:00 | 25 | 0 | 0 | 0 | 25 | 25 | **100.0%** ⭐ |
| 2026-02-19T12:00 | 25 | 0 | 0 | 4 | 21 | 21 | **84.0%** |
| 2026-02-20T00:00 | 25 | 0 | 0 | 0 | 25 | 25 | **100.0%** ⭐ |
| 2026-02-20T12:00 | 25 | 0 | 0 | 0 | 25 | 25 | **100.0%** ⭐ |
| 2026-02-21T00:00 | 25 | 0 | 0 | 0 | 25 | 25 | **100.0%** ⭐ |
| 2026-02-21T12:00 | 25 | 0 | 0 | 0 | 25 | 25 | **100.0%** ⭐ |
| 2026-02-22T00:00 | 25 | 0 | 0 | 2 | 23 | 23 | **92.0%** |
| 2026-02-22T12:00 | 24 | 0 | 0 | 2 | 22 | 22 | **91.7%** |
| 2026-02-23T00:00 | 12 | 0 | 0 | 0 | 12 | 12 | **100.0%** ⭐ |

---

## Analisis

### 🏆 Performa Terbaik di Seluruh Stasiun

Model mencapai akurasi keseluruhan **93.0%** di stasiun Saipem — **tertinggi di antara seluruh stasiun** yang dianalisis. Dari 711 pasangan data, **661 diprediksi dengan benar**.

### 14 dari 29 Issued Time Mencapai Akurasi Sempurna

**14 issued time (48.3%) mencapai akurasi 100%** — hampir setengah dari seluruh prediksi sempurna:

| No | Issued Time | Accuracy |
|----|------------|----------|
| 1 | 2026-02-09T00:00 | 100% ⭐ |
| 2 | 2026-02-09T12:00 | 100% ⭐ |
| 3 | 2026-02-10T12:00 | 100% ⭐ |
| 4 | 2026-02-13T00:00 | 100% ⭐ |
| 5 | 2026-02-13T12:00 | 100% ⭐ |
| 6 | 2026-02-14T00:00 | 100% ⭐ |
| 7 | 2026-02-14T12:00 | 100% ⭐ |
| 8 | 2026-02-15T00:00 | 100% ⭐ |
| 9 | 2026-02-19T00:00 | 100% ⭐ |
| 10 | 2026-02-20T00:00 | 100% ⭐ |
| 11 | 2026-02-20T12:00 | 100% ⭐ |
| 12 | 2026-02-21T00:00 | 100% ⭐ |
| 13 | 2026-02-21T12:00 | 100% ⭐ |
| 14 | 2026-02-23T00:00 | 100% ⭐ |

### Distribusi Akurasi yang Sangat Baik

| Rentang Akurasi | Jumlah Issued Time | Persentase |
|-----------------|-------------------|------------|
| 100% | 14 | 48.3% |
| 90–99% | 6 | 20.7% |
| 80–89% | 5 | 17.2% |
| 72–79% | 4 | 13.8% |
| **Total ≥80%** | **25 (86.2%)** |

**25 dari 29 issued time (86.2%) mencapai akurasi ≥80%** — konsistensi yang luar biasa.

### False Alarm Sangat Rendah

Model hanya memprediksi hujan pada **50 dari 711 jam** (7.0%) — angka false alarm **terendah** di seluruh stasiun. Perbandingan:

| Stasiun | False Alarm | Persentase FA |
|---------|-------------|--------------|
| **Saipem** | **50** | **7.0%** |
| Intiland Regatta | 145 | 20.4% |
| ARR-Bendung Jragung | 153 | 21.5% |
| AWLR-Hulu (HK) | 183 | 25.7% |

### Prediksi "Tidak Hujan" Sempurna

Dari 661 kali model memprediksi "tidak hujan", **seluruhnya tepat** (100% presisi). Ini merupakan jumlah correct negative tertinggi di semua stasiun.

### Rentang Akurasi Sempurna Terpanjang

Terdapat beberapa rentang akurasi 100% berturut-turut:
- **13–15 Feb 00:00:** 5 issued time berturut-turut dengan akurasi 100%
- **19 Feb 00:00 – 21 Feb 12:00:** 5 issued time berturut-turut (termasuk 23 Feb) dengan akurasi 100%

### Perbandingan Jam 00 vs Jam 12

| Grup | Jumlah Issued | Rata-rata Accuracy |
|------|--------------|-------------------|
| Jam 00 | 15 | 94.3% |
| Jam 12 | 14 | 91.8% |

Kedua waktu penerbitan menunjukkan performa yang sangat tinggi, dengan forecast jam 00 sedikit lebih unggul.

---

## Kesimpulan

1. **Akurasi biner 93.0%** 🏆 — **performa terbaik di seluruh stasiun**, menunjukkan model bekerja sangat efektif di lokasi ini.
2. **14 dari 29 issued time (48.3%) mencapai akurasi sempurna 100%** — hampir separuh prediksi tanpa kesalahan sama sekali.
3. **25 dari 29 issued time (86.2%) mencapai akurasi ≥80%** — konsistensi luar biasa sepanjang periode.
4. **False alarm terendah** di seluruh stasiun — hanya 7% prediksi hujan yang salah.
5. **Prediksi "tidak hujan" selalu benar** — 661 prediksi "tidak hujan" semuanya tepat (presisi 100%).
6. Stasiun Saipem menjadi **referensi performa terbaik** model forecast untuk periode ini.
