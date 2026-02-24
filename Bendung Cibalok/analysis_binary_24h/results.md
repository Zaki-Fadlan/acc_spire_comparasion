# Hasil Analisis Akurasi Forecast — Bendung Cibalok

**Analisis Part 3: Binary Rain Event (24 Jam Pertama)**

- Stasiun: Bendung Cibalok
- Koordinat: -6.6533873, 106.8695836
- Periode: 2026-02-09 s/d 2026-02-23
- Jumlah issued time: 29
- Total pairs: **660**
- Lead time maksimum: **24 jam**

> **Pertanyaan sederhana:** Apakah model bisa menebak HUJAN atau TIDAK HUJAN?
> - Observasi > 0 mm/h → **Hujan**
> - Observasi ≤ 0 mm/h → **Tidak Hujan**
> - Forecast > 0 mm/h → **Prediksi Hujan**
> - Forecast ≤ 0 mm/h → **Prediksi Tidak Hujan**

---

## Hasil Utama

| Metrik | Nilai |
|--------|-------|
| **Akurasi Keseluruhan** | **71.4%** |
| **Rata-rata Akurasi per Issued** | **72.1%** |
| Issued Time Terbaik | 2026-02-15T00:00 → **100%** |
| Issued Time Terburuk | 2026-02-18T00:00 → **40%** |

---

## Confusion Matrix (Keseluruhan)

|  | **Obs: Hujan** | **Obs: Tidak Hujan** | **Total FC** |
|--|---------------|---------------------|--------------|
| **FC: Hujan** | 41 (Hit) | 164 (False Alarm) | 205 |
| **FC: Tidak Hujan** | 25 (Miss) | 430 (Correct Neg) | 455 |
| **Total Obs** | 66 | 594 | **660** |

**Penjelasan setiap sel:**
- **Hit (41)**: Model bilang hujan, memang hujan → **BENAR** ✓
- **False Alarm (164)**: Model bilang hujan, tapi tidak hujan → **SALAH** ✗
- **Miss (25)**: Model bilang tidak hujan, tapi ternyata hujan → **SALAH** ✗
- **Correct Negative (430)**: Model bilang tidak hujan, memang tidak hujan → **BENAR** ✓

---

## Distribusi Kejadian

| Kategori | Jumlah | Persentase |
|----------|--------|------------|
| Observasi hujan | 66 | 10.0% |
| Observasi tidak hujan | 594 | 90.0% |
| Forecast prediksi hujan | 205 | 31.1% |
| Forecast prediksi tidak hujan | 455 | 68.9% |

**Penjelasan:**
- Dari 660 jam pengamatan, hanya **10% yang benar-benar hujan**.
- Tapi model **memprediksi hujan 31.1% dari waktu** — 3× lebih sering dari kenyataan.
- Ini menyebabkan banyak **false alarm** (164 dari 205 prediksi hujan ternyata salah).

---

## Akurasi per Issued Time

| Issued Time | Pairs | Hit | Miss | FA | CN | Correct | Accuracy |
|-------------|-------|-----|------|----|----|---------|----------|
| 2026-02-09T00:00 | 25 | 2 | 1 | 7 | 15 | 17 | 68.0% |
| 2026-02-09T12:00 | 25 | 1 | 0 | 5 | 19 | 20 | 80.0% |
| 2026-02-10T00:00 | 25 | 1 | 1 | 4 | 19 | 20 | 80.0% |
| 2026-02-10T12:00 | 25 | 1 | 1 | 6 | 17 | 18 | 72.0% |
| 2026-02-11T00:00 | 25 | 1 | 0 | 8 | 16 | 17 | 68.0% |
| 2026-02-11T12:00 | 25 | 2 | 0 | 8 | 15 | 17 | 68.0% |
| 2026-02-12T00:00 | 25 | 1 | 3 | 8 | 13 | 14 | **56.0%** |
| 2026-02-12T12:00 | 20 | 1 | 2 | 2 | 15 | 16 | 80.0% |
| 2026-02-13T00:00 | 15 | 0 | 0 | 4 | 11 | 11 | 73.3% |
| 2026-02-13T12:00 | 20 | 1 | 1 | 6 | 12 | 13 | 65.0% |
| 2026-02-14T00:00 | 25 | 2 | 0 | 8 | 15 | 17 | 68.0% |
| 2026-02-14T12:00 | 20 | 0 | 0 | 1 | 19 | 19 | **95.0%** |
| 2026-02-15T00:00 | 13 | 0 | 0 | 0 | 13 | 13 | **100.0%** |
| 2026-02-15T12:00 | 16 | 1 | 0 | 2 | 13 | 14 | 87.5% |
| 2026-02-16T00:00 | 22 | 0 | 2 | 3 | 17 | 17 | 77.3% |
| 2026-02-16T12:00 | 25 | 0 | 1 | 1 | 23 | 23 | **92.0%** |
| 2026-02-17T00:00 | 25 | 0 | 1 | 3 | 21 | 21 | 84.0% |
| 2026-02-17T12:00 | 25 | 2 | 0 | 9 | 14 | 16 | 64.0% |
| 2026-02-18T00:00 | 25 | 1 | 0 | 15 | 9 | 10 | **40.0%** |
| 2026-02-18T12:00 | 25 | 3 | 0 | 4 | 18 | 21 | 84.0% |
| 2026-02-19T00:00 | 25 | 8 | 2 | 6 | 9 | 17 | 68.0% |
| 2026-02-19T12:00 | 25 | 7 | 2 | 5 | 11 | 18 | 72.0% |
| 2026-02-20T00:00 | 25 | 2 | 1 | 4 | 18 | 20 | 80.0% |
| 2026-02-20T12:00 | 25 | 0 | 2 | 7 | 16 | 16 | 64.0% |
| 2026-02-21T00:00 | 25 | 2 | 0 | 12 | 11 | 13 | **52.0%** |
| 2026-02-21T12:00 | 25 | 0 | 2 | 4 | 19 | 19 | 76.0% |
| 2026-02-22T00:00 | 25 | 1 | 2 | 10 | 12 | 13 | **52.0%** |
| 2026-02-22T12:00 | 23 | 1 | 1 | 8 | 13 | 14 | 60.9% |
| 2026-02-23T00:00 | 11 | 0 | 0 | 4 | 7 | 7 | 63.6% |

---

## Analisis & Penjelasan

### Mengapa akurasi 71.4% menyesatkan?

Akurasi 71.4% terdengar "cukup baik", tapi sebenarnya kurang informatif karena:

- **90% waktu memang tidak hujan.** Jika model selalu bilang "TIDAK HUJAN" tanpa melihat data apapun, akurasi sudah 90%!
- Akurasi 71.4% sebenarnya **lebih buruk** dari model "tebak selalu tidak hujan" (90%).
- Penyebab penurunan: model terlalu sering prediksi hujan (31% padahal cuma 10% hujan), menghasilkan 164 false alarm yang menurunkan akurasi.

### False Alarm sangat mendominasi

Dari 205 kali model memprediksi hujan:
- Hanya **41 yang benar** (20%)
- **164 salah** (80%)

Artinya: **4 dari 5 prediksi hujan ternyata salah.**

### Kemampuan mendeteksi hujan

Dari 66 kejadian hujan sesungguhnya:
- **41 terdeteksi** (62.1%) — lumayan
- **25 terlewat** (37.9%) — hampir 4 dari 10 hujan tidak terprediksi

### Issued Time Terbaik: 2026-02-15T00:00 (100%)

- 13 pasangan data, semua benar
- Tapi ini karena **tidak ada hujan sama sekali** dan model juga bilang tidak hujan
- Bukan karena model "pintar", tapi karena periode kebetulan kering

### Issued Time Terburuk: 2026-02-18T00:00 (40%)

- 25 pasangan data, hanya 10 benar
- 1 jam hujan sesungguhnya, model berhasil deteksi → 1 hit
- Tapi model prediksi hujan 16 kali! → 15 false alarm
- Pada hari ini model sangat "trigger happy" memprediksi hujan

### Pola Menarik: 19 Feb — Hari dengan Hujan Terbanyak

Issued 2026-02-19T00:00 dan 12:00:
- **10 dan 9 jam hujan** dari 25 jam — proporsi hujan tertinggi
- Model mendeteksi **8 dari 10** dan **7 dari 9** hujan (POD tinggi)
- False alarm juga lebih rendah (6 dan 5)
- **Akurasi 68% dan 72%** — tapi CSI jauh lebih baik

Ini menunjukkan model **lebih akurat saat cuaca benar-benar hujan deras**, tapi gagal pada hari-hari dengan hujan sporadis ringan.

---

## Perbandingan: Issued Jam 00 vs Jam 12

| Grup | Jumlah Issued | Rata-rata Accuracy |
|------|--------------|-------------------|
| Jam 00 | 15 | 71.4% |
| Jam 12 | 14 | 72.8% |

Perbedaan sangat kecil — **tidak ada keunggulan signifikan** antara forecast jam 00 vs jam 12.

---

## Kesimpulan

1. **Akurasi biner 71.4%** — tapi ini lebih rendah dari baseline "selalu tebak tidak hujan" (90%).

2. **Model terlalu sering prediksi hujan** — 3× lebih sering dari kenyataan, menyebabkan false alarm masif (164 dari 205).

3. **Deteksi hujan lumayan** — 62% hujan terdeteksi, tapi 38% terlewat.

4. **Model lebih baik saat cuaca jelas** — sangat bagus menebak hari kering, tapi memformat untuk hari hujan sporadis.

5. **Model lebih baik saat hujan banyak** — akurasi deteksi meningkat saat memang banyak hujan (19 Feb), tapi buruk saat hujan ringan sesekali.

6. **Untuk pengambilan keputusan**: Jika model bilang "tidak hujan" → cukup bisa dipercaya (455 prediksi, 430 benar = 94.5%). Tapi jika model bilang "hujan" → hanya 20% kemungkinan benar.

7. **Saran**: Perlu menaikkan threshold (misalnya > 0.5 mm/h alih-alih > 0) untuk mengurangi false alarm sambil mempertahankan deteksi hujan signifikan.
