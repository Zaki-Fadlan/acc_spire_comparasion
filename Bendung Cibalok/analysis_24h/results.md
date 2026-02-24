# Hasil Analisis Akurasi Forecast — Bendung Cibalok

**Analisis Part 2: Hanya 24 Jam Pertama per Forecast**

- Stasiun: Bendung Cibalok
- Koordinat: -6.6533873, 106.8695836
- Periode: 2026-02-09 s/d 2026-02-23
- Jumlah issued time: 29
- Total pairs (obs vs forecast): **660** (dari 3.027 di Part 1)
- Lead time maksimum: **24 jam**
- Threshold hujan: > 0 mm/h

> **Hipotesis:** Dengan hanya mengambil 24 jam pertama dari setiap forecast, akurasi seharusnya meningkat karena forecast jangka pendek umumnya lebih akurat.

---

## Ringkasan Perbandingan dengan Part 1 (Semua Lead Time)

| Metrik | Part 1 (All) | Part 2 (≤24h) | Perubahan |
|--------|-------------|---------------|-----------|
| Total pairs | 3.027 | 660 | −78% |
| Overall Accuracy | 73.0% | 71.4% | −1.6% |
| POD | 52.4% | **62.1%** | **+9.7%** |
| FAR | 80.4% | 80.0% | −0.4% |
| CSI | 16.6% | **17.8%** | **+1.2%** |
| Bias Score | 2.67 | 3.11 | +0.44 |
| MAE | 1.40 mm/h | 1.39 mm/h | ~sama |
| RMSE | 5.61 mm/h | 5.63 mm/h | ~sama |

---

## Metode 1: MAE & RMSE per Lead Time (0–24 jam, bucket 3 jam)

**Penjelasan:** Sama seperti Part 1, tapi dengan bucket yang lebih halus (setiap 3 jam) karena rentang hanya 0–24 jam. Ini memberi gambaran lebih detail tentang kapan forecast mulai kehilangan akurasi.

| Lead Time | Pairs | MAE (mm/h) | RMSE (mm/h) | Mean Obs | Mean FC | Bias |
|-----------|-------|------------|-------------|----------|---------|------|
| 0–3 jam | 83 | 0.47 | 2.50 | 0.43 | 0.04 | −0.39 |
| 3–6 jam | 81 | 0.71 | 2.56 | 0.59 | 0.33 | −0.26 |
| 6–9 jam | 80 | 2.11 | 8.23 | 1.40 | 1.06 | −0.33 |
| 9–12 jam | 79 | 1.81 | 6.23 | 0.85 | 1.33 | +0.48 |
| 12–15 jam | 80 | 0.84 | 3.16 | 0.44 | 0.41 | −0.03 |
| 15–18 jam | 78 | 1.20 | 3.73 | 0.52 | 0.70 | +0.18 |
| 18–21 jam | 77 | 2.18 | 9.11 | 1.45 | 0.84 | −0.61 |
| 21–24 jam | 76 | 2.07 | 6.27 | 0.88 | 1.79 | +0.91 |

**Interpretasi:**
- **0–3 jam** paling akurat (MAE 0.47), sesuai ekspektasi.
- Error melonjak tajam di **6–9 jam** (MAE 2.11) dan **18–21 jam** (MAE 2.18).
- Ada pola **diurnal**: error cenderung tinggi di jam-jam yang bertepatan dengan sore/malam (saat hujan konvektif paling aktif di wilayah tropis).
- **21–24 jam** memiliki bias positif terbesar (+0.91) — model sangat overpredict di akhir hari forecast.
- Pola error ini tidak monoton (tidak selalu naik seiring lead time bertambah), menunjukkan pengaruh siklus diurnal.

---

## Metode 2: Contingency Table per Issued Time (24h)

**Penjelasan:** Tabel kontingensi yang sama seperti Part 1, tapi hanya menggunakan data 24 jam pertama.

**5 Issued Time Terbaik (by Accuracy):**

| Issued Time | Pairs | Accuracy | POD | FAR | CSI |
|-------------|-------|----------|-----|-----|-----|
| 2026-02-15T00:00 | 13 | 100.0% | — | — | — |
| 2026-02-14T12:00 | 20 | 95.0% | — | 1.00 | 0.00 |
| 2026-02-16T12:00 | 25 | 92.0% | 0.00 | 1.00 | 0.00 |
| 2026-02-15T12:00 | 16 | 87.5% | 1.00 | 0.67 | 0.33 |
| 2026-02-17T00:00 | 25 | 84.0% | 0.00 | 1.00 | 0.00 |

**5 Issued Time Terburuk:**

| Issued Time | Pairs | Accuracy | POD | FAR | CSI |
|-------------|-------|----------|-----|-----|-----|
| 2026-02-18T00:00 | 25 | 40.0% | 1.00 | 0.94 | 0.06 |
| 2026-02-21T00:00 | 25 | 52.0% | 1.00 | 0.86 | 0.14 |
| 2026-02-22T00:00 | 25 | 52.0% | 0.33 | 0.91 | 0.08 |
| 2026-02-12T00:00 | 25 | 56.0% | 0.25 | 0.89 | 0.08 |
| 2026-02-22T12:00 | 23 | 60.9% | 0.50 | 0.89 | 0.10 |

**Interpretasi:**
- Issued time dengan accuracy tinggi (≥90%) umumnya pada **periode sedikit hujan** (mid-Feb), sehingga correct negatives mendominasi.
- **2026-02-15T12:00** adalah yang terbaik secara CSI (0.33) — berhasil deteksi hujan sambil false alarm rendah.
- **2026-02-18T00:00** terburuk (40% accuracy) — model sangat overpredict (16 prediksi hujan dari 25 jam, padahal hanya 1 jam yang benar-benar hujan).
- Issued jam **19T00** dan **19T12** memiliki **CSI terbaik (0.50)** karena kebetulan banyak hujan di periode itu dan model cukup menangkap.

---

## Metode 3: Korelasi (24h)

**Penjelasan:** Korelasi antara intensitas forecast vs observasi, hanya 24 jam pertama.

| Statistik | Rata-rata | Min | Max |
|-----------|-----------|-----|-----|
| Pearson r | 0.13 | −0.10 | **0.95** |
| Spearman r | 0.19 | −0.18 | **0.64** |

**Issued Time dengan Korelasi Tertinggi:**

| Issued Time | Pearson r | Spearman r |
|-------------|-----------|------------|
| 2026-02-15T12:00 | **0.986** | 0.552 |
| 2026-02-11T00:00 | **0.949** | 0.396 |
| 2026-02-14T00:00 | **0.510** | 0.430 |
| 2026-02-19T00:00 | 0.384 | 0.567 |
| 2026-02-20T00:00 | 0.343 | 0.428 |

**Interpretasi:**
- Korelasi rata-rata masih lemah (0.13 Pearson), tapi **jauh lebih baik** daripada Part 1 (0.07).
- Beberapa issued memiliki korelasi **sangat tinggi** (0.95, 0.99) yang tidak terlihat di Part 1 — ini menunjukkan di window 24 jam, forecast kadang sangat akurat.
- Peningkatan ini mendukung bahwa **24 jam pertama memang lebih informatif**.
- Namun tetap inkonsisten — banyak issued dengan korelasi negatif atau mendekati nol.

---

## Metode 4: Skill Score (24h)

**Penjelasan:** Perbandingan forecast vs klimatologi (rata-rata historis).

| Statistik | Nilai |
|-----------|-------|
| Climatology Mean | 0.79 mm/h |
| Climatology Rain Prob | 10.0% |
| Avg MSE Skill Score | −8.63 |
| Avg Brier Skill Score | −5.15 |

**Issued Time dengan Skill Score Positif (lebih baik dari klimatologi):**

| Issued Time | MSE Skill Score | Brier Skill Score |
|-------------|----------------|-------------------|
| 2026-02-15T00:00 | **+1.000** | +1.000 |
| 2026-02-13T00:00 | +0.937 | −25.667 |
| 2026-02-16T12:00 | +0.905 | −0.905 |
| 2026-02-15T12:00 | +0.729 | −1.083 |
| 2026-02-19T00:00 | +0.181 | +0.030 |
| 2026-02-19T12:00 | — | +0.060 |

**Interpretasi:**
- Secara rata-rata, skill score masih **sangat negatif** — forecast lebih buruk dari klimatologi.
- Namun ada **beberapa issued yang menunjukkan skill bagus** (MSE SS > 0.7), terutama di pertengahan Februari.
- MSE Skill Score sangat negatif karena beberapa issued memiliki forecast yang sangat meleset (error puluhan kali lipat dari klimatologi, e.g., 2026-02-10T00 = −119).
- **Kesimpulan**: untuk 24 jam pertama, model kadang sangat bagus tapi kadang sangat buruk — tidak konsisten.

---

## Metode 5: Akumulasi Presipitasi (24h)

**Penjelasan:** Total hujan akumulatif 24 jam pertama dari obs vs forecast.

**5 Issued Time dengan Estimasi Terbaik:**

| Issued Time | Obs (mm) | FC (mm) | Selisih (mm) | Ratio |
|-------------|----------|---------|-------------|-------|
| 2026-02-15T00:00 | 0.0 | 0.0 | 0.0 | — |
| 2026-02-13T00:00 | 0.0 | 1.0 | +1.0 | — |
| 2026-02-16T00:00 | 3.6 | 4.7 | +1.1 | 1.31 |
| 2026-02-16T12:00 | 1.2 | 0.1 | −1.1 | 0.08 |
| 2026-02-09T00:00 | 8.4 | 6.7 | −1.7 | 0.80 |

**5 Issued Time dengan Estimasi Terburuk:**

| Issued Time | Obs (mm) | FC (mm) | Selisih (mm) | Ratio |
|-------------|----------|---------|-------------|-------|
| 2026-02-10T00:00 | 2.4 | 76.9 | +74.5 | 32.04 |
| 2026-02-19T12:00 | 64.8 | 3.2 | −61.6 | 0.05 |
| 2026-02-14T00:00 | 73.2 | 13.0 | −60.2 | 0.18 |
| 2026-02-09T12:00 | 1.2 | 59.9 | +58.7 | 49.92 |
| 2026-02-19T00:00 | 86.4 | 37.3 | −49.1 | 0.43 |

**Interpretasi:**
- Error akumulasi 24 jam sangat bervariasi — dari sempurna (0 mm selisih) hingga 74.5 mm meleset.
- Kasus terburuk: model prediksi 76.9 mm tapi hanya 2.4 mm hujan (overpredict 32×), atau prediksi 3.2 mm saat 64.8 mm hujan jatuh.
- Ini menunjukkan model **tidak reliable untuk perencanaan berbasis total curah hujan** 24 jam.

---

## Metode 6: Ranking Issued Time Terbaik (24h)

**Top 5 Forecast Terbaik (24 jam pertama):**

| Rank | Issued Time | MAE | CSI | Pearson r |
|------|-------------|-----|-----|-----------|
| 1 | 2026-02-15T12:00 | 0.12 | 0.333 | 0.986 |
| 2 | 2026-02-20T00:00 | 0.45 | 0.286 | 0.343 |
| 3 | 2026-02-09T00:00 | 0.59 | 0.200 | −0.075 |
| 4 | 2026-02-11T12:00 | 0.83 | 0.200 | 0.298 |
| 5 | 2026-02-18T12:00 | 1.51 | 0.429 | 0.011 |

**5 Forecast Terburuk:**

| Rank | Issued Time | MAE | CSI | Pearson r |
|------|-------------|-----|-----|-----------|
| 25 | 2026-02-10T00:00 | 3.08 | 0.167 | 0.083 |
| 26 | 2026-02-12T00:00 | 1.91 | 0.083 | −0.087 |
| 27 | 2026-02-18T00:00 | 1.53 | 0.063 | −0.088 |
| 28 | 2026-02-20T12:00 | 0.86 | 0.000 | −0.094 |
| 29 | 2026-02-13T12:00 | 4.91 | 0.125 | −0.064 |

**Interpretasi:**
- **2026-02-15T12:00** adalah forecast terbaik dengan skor sempurna di hampir semua metrik (MAE 0.12, CSI 0.33, Pearson r 0.99).
- Ranking 24h berbeda signifikan dari ranking all-time (Part 1) — menunjukkan bahwa kualitas antar issued time bervariasi tergantung window yang digunakan.

---

## Kesimpulan Part 2

1. **POD meningkat signifikan** (52.4% → 62.1%) — forecast 24 jam pertama lebih baik mendeteksi hujan.
2. **CSI sedikit membaik** (16.6% → 17.8%).
3. **FAR tetap tinggi** (~80%) — model masih terlalu sering false alarm.
4. **Korelasi membaik** (Pearson r: 0.07 → 0.13), beberapa issued sangat bagus (r > 0.9).
5. **Error tidak monoton** — ada pola diurnal, bukan sekadar naik seiring lead time bertambah.
6. **Skill Score masih negatif** — model belum konsisten mengalahkan klimatologi meski di window 24 jam.
7. **Hipotesis terkonfirmasi sebagian**: 24 jam pertama memang lebih baik untuk deteksi (POD), tapi masih punya masalah false alarm tinggi.
