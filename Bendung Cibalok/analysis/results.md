# Hasil Analisis Akurasi Forecast — Bendung Cibalok

**Analisis Part 1: Semua Lead Time (0–144 jam)**

- Stasiun: Bendung Cibalok
- Koordinat: -6.6533873, 106.8695836
- Periode: 2026-02-09 s/d 2026-02-23
- Jumlah issued time: 29
- Total pairs (obs vs forecast): 3.027
- Threshold hujan: > 0 mm/h

---

## Metode 1: MAE & RMSE per Lead Time

**Penjelasan:** MAE (Mean Absolute Error) mengukur rata-rata selisih absolut antara forecast dan observasi. RMSE (Root Mean Square Error) mirip tapi memberi bobot lebih besar pada error yang besar. Bias menunjukkan apakah forecast cenderung overpredict (+) atau underpredict (−).

| Lead Time | Pairs | MAE (mm/h) | RMSE (mm/h) | Mean Obs | Mean FC | Bias |
|-----------|-------|------------|-------------|----------|---------|------|
| 0–6 jam | 164 | 0.59 | 2.53 | 0.51 | 0.18 | −0.33 |
| 6–12 jam | 159 | 1.96 | 7.30 | 1.12 | 1.19 | +0.07 |
| 12–24 jam | 311 | 1.56 | 6.02 | 0.82 | 0.93 | +0.11 |
| 24–48 jam | 586 | 1.29 | 5.36 | 0.87 | 0.73 | −0.14 |
| 48–72 jam | 538 | 1.58 | 6.21 | 0.92 | 0.77 | −0.15 |
| 72–96 jam | 490 | 1.26 | 5.58 | 0.99 | 0.46 | −0.53 |
| 96–120 jam | 447 | 1.49 | 6.03 | 0.96 | 0.66 | −0.30 |
| 120–144 jam | 332 | 1.49 | 5.83 | 0.85 | 0.73 | −0.11 |

**Interpretasi:**
- **Lead time 0–6 jam** memiliki error paling kecil (MAE 0.59), artinya forecast paling akurat di jam-jam awal.
- MAE melonjak di rentang **6–12 jam** (1.96), lalu relatif stabil di 1.26–1.58 untuk lead time lebih panjang.
- Bias negatif dominan artinya **model cenderung underpredict** (prediksi hujan lebih rendah dari kenyataan), kecuali di 6–24 jam yang sedikit overpredict.
- RMSE yang jauh lebih besar dari MAE menandakan adanya **error ekstrem sesekali** (misalnya saat hujan deras yang tidak terprediksi).

---

## Metode 2: Contingency Table (Tabel Kontingensi)

**Penjelasan:** Metode ini mengklasifikasikan setiap pasangan obs-forecast menjadi 4 kategori:
- **Hit**: Forecast hujan, observasi memang hujan ✓
- **Miss**: Forecast tidak hujan, tapi observasi hujan ✗
- **False Alarm**: Forecast hujan, tapi observasi tidak hujan ✗
- **Correct Negative**: Forecast tidak hujan, observasi memang tidak hujan ✓

Metrik turunan:
- **POD (Probability of Detection)**: Dari semua kejadian hujan, berapa yang terdeteksi? (semakin tinggi semakin baik)
- **FAR (False Alarm Ratio)**: Dari semua prediksi hujan, berapa yang salah? (semakin rendah semakin baik)
- **CSI (Critical Success Index)**: Gabungan POD dan FAR, mengukur skill mendeteksi hujan (semakin tinggi semakin baik)
- **Bias Score**: Rasio prediksi hujan / kejadian hujan nyata (1.0 = sempurna, >1 = overprediksi)

| Metrik | Rata-rata Semua Issued |
|--------|----------------------|
| Accuracy | 73.0% |
| POD | 52.4% |
| FAR | 80.4% |
| CSI | 16.6% |
| Bias Score | 2.67 |

**5 Issued Time Terbaik (by Accuracy):**

| Issued Time | Pairs | Accuracy | POD | FAR | CSI |
|-------------|-------|----------|-----|-----|-----|
| 2026-02-12T12:00 | 115 | 88.7% | 0.56 | 0.64 | 0.28 |
| 2026-02-14T00:00 | 125 | 83.2% | 0.55 | 0.73 | 0.22 |
| 2026-02-10T12:00 | 115 | 82.6% | 0.67 | 0.74 | 0.23 |
| 2026-02-21T12:00 | 47 | 80.9% | 0.00 | 1.00 | 0.00 |
| 2026-02-09T12:00 | 128 | 80.5% | 0.33 | 0.86 | 0.11 |

**Interpretasi:**
- **FAR sangat tinggi (80.4%)** — 4 dari 5 prediksi hujan ternyata salah (false alarm).
- **POD hanya 52.4%** — hampir setengah kejadian hujan tidak terdeteksi.
- **CSI sangat rendah (16.6%)** — skill forecast dalam mendeteksi hujan buruk.
- **Bias 2.67** — model memprediksi hujan 2.7× lebih sering dari kenyataan.
- Accuracy 73% terlihat lumayan, tapi ini misleading karena 90% waktu memang tidak hujan (correct negatives mendominasi).

---

## Metode 3: Korelasi

**Penjelasan:** Korelasi mengukur kekuatan hubungan linier antara forecast dan observasi.
- **Pearson r**: Korelasi linier (-1 s/d +1). Nilai mendekati +1 berarti saat obs naik, forecast juga naik.
- **Spearman r**: Korelasi peringkat, lebih robust terhadap outlier.

| Statistik | Rata-rata | Min | Max |
|-----------|-----------|-----|-----|
| Pearson r | 0.07 | −0.09 | 0.35 |
| Spearman r | 0.18 | −0.10 | 0.38 |

**5 Issued Time dengan Pearson r Tertinggi:**

| Issued Time | Pearson r | Spearman r |
|-------------|-----------|------------|
| 2026-02-18T12:00 | 0.354 | 0.311 |
| 2026-02-14T00:00 | 0.336 | 0.299 |
| 2026-02-18T00:00 | 0.309 | 0.156 |
| 2026-02-10T12:00 | 0.291 | 0.335 |
| 2026-02-19T00:00 | 0.277 | 0.269 |

**Interpretasi:**
- Korelasi sangat lemah secara keseluruhan (rata-rata Pearson r = 0.07).
- Bahkan Pearson r terbaik hanya 0.354 — menunjukkan **hubungan linier yang sangat lemah** antara intensitas forecast dan observasi.
- Beberapa issued time memiliki korelasi negatif, artinya forecast justru berkebalikan dengan kenyataan.
- Spearman r sedikit lebih baik (0.18), menunjukkan ada sedikit kesesuaian ranking tapi masih lemah.

---

## Metode 4: Skill Score

**Penjelasan:** Skill Score membandingkan performa forecast terhadap "klimatologi" (prediksi naif berdasarkan rata-rata historis).
- **MSE Skill Score**: > 0 berarti forecast lebih baik dari klimatologi, < 0 lebih buruk.
- **Brier Skill Score**: Mengukur skill prediksi probabilistik kejadian hujan vs rata-rata.

| Statistik | Rata-rata |
|-----------|-----------|
| Climatology Mean | 0.896 mm/h |
| Climatology Rain Prob | 10.3% |
| MSE Skill Score | −0.44 |
| Brier Skill Score | −3.50 |

**Interpretasi:**
- **MSE Skill Score negatif (−0.44)** — forecast justru **lebih buruk dari sekadar pakai rata-rata** klimatologi!
- **Brier Skill Score sangat negatif (−3.50)** — prediksi biner (hujan/tidak) jauh lebih buruk dari klimatologi.
- Hanya beberapa issued time (2026-02-10T12, 2026-02-12T12, 2026-02-14T00, 2026-02-19T00, 2026-02-21T12, 2026-02-22T00) yang memiliki MSE Skill Score positif.
- Artinya secara umum, **prediksi cuaca dari model ini tidak menunjukkan skill yang signifikan** relatif terhadap baseline klimatologi.

---

## Metode 5: Akumulasi Presipitasi

**Penjelasan:** Membandingkan total hujan akumulatif dari observasi vs forecast untuk setiap issued time. Ratio < 1 = underpredict, > 1 = overpredict.

| Statistik | Nilai |
|-----------|-------|
| Total Obs (semua issued) | 2.657 mm |
| Total FC (semua issued) | 2.150 mm |
| Rata-rata Ratio FC/Obs | 0.81 |

**5 Issued Time dengan Error Akumulasi Terbesar:**

| Issued Time | Obs (mm) | FC (mm) | Selisih (mm) | Ratio |
|-------------|----------|---------|-------------|-------|
| 2026-02-14T00:00 | 134.4 | 28.1 | −106.3 | 0.21 |
| 2026-02-13T12:00 | 129.6 | 35.6 | −94.0 | 0.27 |
| 2026-02-12T12:00 | 127.2 | 34.0 | −93.2 | 0.27 |
| 2026-02-18T00:00 | 129.6 | 212.6 | +83.0 | 1.64 |
| 2026-02-15T00:00 | 121.2 | 58.8 | −62.4 | 0.49 |

**Interpretasi:**
- Secara umum model **cenderung underpredict** total akumulasi hujan (rata-rata hanya 81% dari observasi).
- Ada beberapa issued yang sangat overpredict (2026-02-18T00), tapi mayoritas underpredict.
- Error terbesar terjadi saat hujan observasi banyak tapi model gagal menangkap (selisih > 90 mm).

---

## Metode 6: Ranking Issued Time Terbaik

**Penjelasan:** Membuat ranking komposit dari MAE (error terkecil) dan CSI (skill deteksi terbaik) untuk menentukan issued time mana yang paling bisa diandalkan.

**Top 5 Issued Time Terbaik:**

| Rank | Issued Time | MAE | CSI | Mean Lead (jam) |
|------|-------------|-----|-----|-----------------|
| 1 | 2026-02-12T12:00 | 1.29 | 0.278 | 72.0 |
| 2 | 2026-02-18T12:00 | 1.28 | 0.244 | 59.0 |
| 3 | 2026-02-14T00:00 | 1.19 | 0.222 | 72.2 |
| 4 | 2026-02-16T00:00 | 1.30 | 0.226 | 70.5 |
| 5 | 2026-02-20T00:00 | 0.83 | 0.194 | 41.0 |

**5 Issued Time Terburuk:**

| Rank | Issued Time | MAE | CSI | Mean Lead (jam) |
|------|-------------|-----|-----|-----------------|
| 25 | 2026-02-12T00:00 | 1.43 | 0.094 | 69.5 |
| 26 | 2026-02-14T12:00 | 1.55 | 0.122 | 73.5 |
| 27 | 2026-02-09T00:00 | 1.99 | 0.143 | 66.0 |
| 28 | 2026-02-09T12:00 | 1.71 | 0.107 | 66.3 |
| 29 | 2026-02-10T00:00 | 2.09 | 0.107 | 63.6 |

**Interpretasi:**
- **Issued 2026-02-12T12:00** adalah forecast terbaik secara keseluruhan dengan CSI tertinggi (0.278) dan MAE yang cukup rendah.
- Forecast di awal periode (9–10 Feb) umumnya terburuk, mungkin karena model belum ter-inisiasi optimal.
- Tidak ada pola jelas bahwa issued jam 00 selalu lebih baik dari jam 12 atau sebaliknya.

---

## Kesimpulan Umum

1. **Akurasi keseluruhan 73%** terlihat cukup baik, tapi **menyesatkan** karena didominasi correct negatives (saat tidak hujan dan forecast juga bilang tidak hujan). Ini wajar karena 90% waktu memang tidak hujan.

2. **Kemampuan mendeteksi hujan sangat lemah** — CSI hanya 16.6%, FAR 80.4%. Model terlalu sering False Alarm dan sering melewatkan hujan nyata (Miss).

3. **Model tidak menunjukkan skill** terhadap klimatologi (Skill Score negatif). Sekadar menebak "rata-rata historis" justru lebih akurat.

4. **Korelasi intensitas sangat lemah** (Pearson r ≈ 0.07) — model gagal menangkap variasi intensitas hujan.

5. **Lead time 0–6 jam** paling akurat (MAE 0.59), setelah itu error meningkat signifikan. Implikasi: hanya gunakan forecast 6 jam pertama untuk keputusan operasional kritis.

6. **Issued time terbaik**: 2026-02-12T12:00:00 (CSI 0.278, accuracy 88.7%).
