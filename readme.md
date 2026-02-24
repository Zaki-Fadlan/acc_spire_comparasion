# Forecast Accuracy Validation Pipeline

## 1. Observation Data Processing

**Status:** ✓ Completed (Run once per station)

| Script | Fungsi |
|--------|--------|
| `filter.py` | Filter obs.json: keep submitted_at (ISO format) + rain_rate |
| `cleanup_obs.py` | Remove entries sebelum 2026-02-09T00:00:00Z |
| `json_to_csv.py` | Convert obs.json → obs.csv |

## 2. Forecast Data Processing

**Status:** ✓ Completed (Run once per station with coordinates)

| Script | Fungsi |
|--------|--------|
| `fetch_forecast.py` | Fetch dari API (29 issued_times per station) → forecast_*.json files |
| `forecast_cleaner.py` | Keep time (ISO) + precipitation_1h |
| `sync_forecast_obs.py` | Remove forecast entries tidak ada di obs.json |

## 3. Data Merging & Analysis

| Script | Fungsi |
|--------|--------|
| `obs_forecast_csv.py` | Merge obs + 29 forecasts → obs_forecast.csv (one row per obs time) |
| `accuracy_analysis_pt3.py` | Evaluate binary rain event accuracy (first 24h lead time) |

---

## Key Concepts

### Apa itu Pairs?

**Pair** = satu perbandingan antara satu **observation** dan satu **forecast**.

Dari CSV dengan struktur:
```
time,value_obs,issued_2026-02-09T00:00:00,issued_2026-02-09T12:00:00,issued_2026-02-10T00:00:00,...
2026-02-09T01:00:00Z,1.2,0.0,,,
2026-02-09T02:00:00Z,0,0.38,,
2026-02-09T03:00:00Z,1.2,0.21,,
```

Setiap baris obs dapat di-pair dengan setiap kolom forecast, **asalkan lead_time ≤ 24 jam**.

**Contoh dari Bendung Cibalok:**
- Row 1: obs(2026-02-09T01:00Z)=1.2 + forecast(issued 2026-02-09T00:00)=0.0 → Pair 1 (lead: 1h)
- Row 2: obs(2026-02-09T02:00Z)=0 + forecast(issued 2026-02-09T00:00)=0.38 → Pair 2 (lead: 2h)
- Row 3: obs(2026-02-09T03:00Z)=1.2 + forecast(issued 2026-02-09T00:00)=0.21 → Pair 3 (lead: 3h)

**Perhitungan total pairs:**
- 323 observation times (rows valid setelah filter)
- 29 issued times (forecast columns)
- **NOT** 323 × 29 = 9,367 pairs (terbatas lead_time 24h)
- **HANYA 660 pairs** yang valid

**Untuk setiap pair:**
1. Cek: apakah obs > 0? (hujan = YES, tidak hujan = NO)
2. Cek: apakah forecast > 0? (tebak hujan = YES, tebak tidak hujan = NO)
3. Hitung: apakah tebakan benar?

---

### Overall Accuracy Formula

**Rumus:**
```
Overall Accuracy (%) = (Hits + Correct Negatives) / Total Pairs × 100
```

**Confusion Matrix:**

|  | Forecast HUJAN | Forecast TIDAK HUJAN | Total |
|---|---|---|---|
| **Actual HUJAN** | Hits (TP) | Misses (FN) | Obs Rain |
| **Actual TIDAK HUJAN** | False Alarms (FP) | Correct Negatives (TN) | Obs No Rain |
| **Total** | Forecast Rain | Forecast No Rain | **Total Pairs** |

**Contoh – Bendung Cibalok (660 pairs):**

| Metrik | Value |
|--------|-------|
| Hits | 41 |
| Misses | 25 |
| False Alarms | 164 |
| Correct Negatives | 430 |
| **Correct Total** | **471** |
| **Overall Accuracy** | **71.36%** |

**Perhitungan:**
```
471 / 660 × 100 = 71.36%
```

**⚠️ Catatan penting:** 
- Stasiun dengan 0% obs rain (dry period) akan punya akurasi tinggi karena model hanya perlu memprediksi "tidak hujan" terus-menerus untuk selalu benar.
- Hanya stasiun dengan actual rain events (Bendung Cibalok 71.36%, Hidrologi-PCH Karanganyar 80.43%) memberikan evaluasi yang bermakna.

---

## Results Summary

**6 Stations Analyzed (24h First Lead Time):**

| Station | Accuracy | Pairs | Obs Rain Events | Meaningful? |
|---------|----------|-------|-----------------|-------------|
| Bendung Cibalok | 71.36% | 660 | 66 (10.0%) | ✓ Yes |
| Hidrologi-PCH Karanganyar | 80.43% | 690 | 59 (8.5%) | ✓ Yes |
| Saipem | 92.97% | 711 | 0 (0%) | ✗ Dry |
| Intiland Regatta | 79.61% | 711 | 0 (0%) | ✗ Dry |
| AWLR-Hulu (HK) | 74.26% | 711 | 0 (0%) | ✗ Dry |
| ARR-Bendung Jragung | 78.48% | 711 | 0 (0%) | ✗ Dry |

**Output locations:**
- Per-issued accuracy: `station/<name>/analysis_binary_24h/1_accuracy_per_issued.csv`
- Summary metrics: `station/<name>/analysis_binary_24h/2_summary.csv`