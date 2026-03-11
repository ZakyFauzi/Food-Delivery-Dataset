# 🍕 Synthetic Food Delivery Dataset
## Dataset Operasional & Kepuasan Pelanggan Food-Delivery Lokal

**Simulasi data aplikasi food delivery seperti GoFood atau GrabFood dengan segala "kekacauan" data real-world.**

---

## 📌 Deskripsi Proyek

Dataset sintetik **8,500 baris** yang mensimulasikan operasional layanan pesan-antar makanan lokal (Indonesia). Dataset dirancang untuk terlihat **asli dengan segala kelemahannya**: missing values, outliers, format inconsistency, dan relasi kausalitas antar kolom.

**Tujuan:**
- ✅ Simulasi data real-world untuk praktik data science
- ✅ Testing algoritma data cleaning & preprocessing
- ✅ Training model machine learning (classification)
- ✅ Exploratory Data Analysis (EDA) dan visualisasi
- ✅ Modeling untuk prediksi order status, rating, dll

---

## 📁 File Structure

```
Food-Delivery-Dataset/
├── README.md                                      # Dokumentasi ini
├── synthetic_fooddelivery_dataset.csv             # Data raw (messy) - 8,500 baris
└── synthetic_fooddelivery_dataset_CLEANED.csv     # Data cleaned (untuk compare)
```

---

## 📊 Dataset Schema (11 Kolom)

| # | Kolom | Tipe Data | Deskripsi | Real-World Challenge |
|---|-------|-----------|-----------|----------------------|
| 1 | **ID_Pesanan** | String | ID unik (ORD-2024-000001) | Nominal identifier |
| 2 | **Waktu_Transaksi** | String/Datetime | Tanggal & jam pesanan masuk | ⚠️ Format inconsistent (80% YYYY-MM-DD, 20% DD/MM/YYYY) |
| 3 | **Kategori_Menu** | String | Jenis makanan (Ayam, Kopi, Martabak, Mie) | ⚠️ Imbalanced distribution |
| 4 | **Harga_Pesanan** | Integer | Total harga dalam Rupiah | ⚠️ 270 outliers (Rp0 - Rp8.3M) |
| 5 | **Jarak_Kirim_KM** | Float | Jarak resto ke rumah pelanggan | ⚠️ 595 NaN (~7% GPS errors) |
| 6 | **Waktu_Tunggu_Menit** | Integer | Total waktu pesan → sampai | ✓ Causal (depends on distance) |
| 7 | **Rating_Pelanggan** | Float | Skor 1-5 dari pelanggan | ⚠️ 1,700 NaN (~20% no rating) |
| 8 | **Ulasan_Teks** | String | Komentar pelanggan | ⚠️ Slang Indonesia, typo, emoji |
| 9 | **Status_Promo** | Boolean | Pakai kode promo? | ✓ 36% True, 64% False |
| 10 | **Tingkat_Keluhan** | String | Severity komplain | ✓ Causal (depends on rating) |
| 11 | **Status_Pesanan** | String | Order completion status | ✓ Imbalanced: 89% Selesai |

---

## 🔍 Data Quality Overview

### ✅ Missing Values (Intentional)
```
Jarak_Kirim_KM:    595 NaN (7.0%)  → GPS sensor failures
Rating_Pelanggan: 1700 NaN (20.0%) → Users too lazy to rate
```

### ✅ Outliers (Generated)
```
Harga_Pesanan:
  - Min: Rp 0 (extremely low)
  - Max: Rp 8,302,000 (extremely high)
  - 270 extreme values (~3.2%)
  - Normal range: mostly Rp 8K - Rp 85K
```

### ✅ Format Inconsistency
```
Waktu_Transaksi:
  80% Format: "2024-03-22 13:15:14" (YYYY-MM-DD HH:MM:SS)
  20% Format: "22/03/2024 13:15"     (DD/MM/YYYY HH:MM)
```

### ✅ Imbalanced Distributions
```
Kategori_Menu:
  - Ayam:     3,030 (35.6%)
  - Kopi:     2,141 (25.2%)
  - Martabak: 1,648 (19.4%)
  - Mie:      1,681 (19.8%)

Status_Pesanan:
  - Selesai:    7,584 (89.2%)
  - Dibatalkan:   611 (7.2%)
  - Refund:       305 (3.6%)
```

---

## 🔗 Causal Relationships (Verified)

### 1️⃣ Distance → Waiting Time
```
Correlation: 0.755 (strong positive)
Logic: 10 min prep + 3 min per km + traffic noise + rush hour effect
```

### 2️⃣ Waiting Time > 60 min → Lower Rating
```
Correlation: -0.263 (moderate negative)
Probability:
  Wait > 60 min: 40% rating 1, 30% rating 2, 10% rating 3, 15% rating 4, 5% rating 5
  Wait < 30 min: 2% rating 1, 3% rating 2, 10% rating 3, 35% rating 4, 50% rating 5
```

### 3️⃣ Low Rating → High Complaint Level
```
Rating ≤ 2:  80% "Tinggi" complaint, 15% "Rendah", 5% "Tidak Ada"
Rating = 3:  20% "Tinggi", 50% "Rendah", 30% "Tidak Ada"
Rating ≥ 4:  5% "Tinggi", 15% "Rendah", 80% "Tidak Ada"
```

### 4️⃣ Issues → Cancellation/Refund
```
Long wait (>70 min) OR High complaint OR Low rating (≤2):
  70% Selesai, 20% Dibatalkan, 10% Refund
  
Normal cases:
  92% Selesai, 5% Dibatalkan, 3% Refund
```

---

## 📝 Review Text Examples

### ⭐ Positive Reviews (Rating: 4-5)
```
"Mantap gan!"
"Enak banget"
"Cepat sampai"
"Sesuai pesanan"
"Puas bgt"
"Top bgt kualitasnya"
"Buruan order!11"
```

### ⭐ Neutral Reviews (Rating: 3)
```
"Lumayan"
"Biasa saja"
"OK"
"Standar lah"
```

### ⭐ Negative Reviews (Rating: 1-2)
```
"Lama bgt nunggunya"
"Makanannya dingin"
"Gak sesuai foto"
"Kecewa bgt"
"Ayamnya keras"
"Driver gak ramah"
"😠😠😠"
```

---

## 🎯 Temporal Patterns

### Peak Order Hours
```
13:00 (1 PM):  900 orders  (10.6%)  ← Lunch peak
14:00 (2 PM):  889 orders  (10.5%)
12:00 (12 PM): 845 orders  (9.9%)
11:00 (11 AM): 842 orders  (9.9%)
20:00 (8 PM):  652 orders  (7.7%)   ← Dinner peak
```

### Time Range
- **Jan-Mar 2024**: 90 days of data
- **Distribution**: Realistic with lunch (11-14) and dinner (17-21) peaks
- **Weekend variation**: Implicit in random distribution

---

## 💰 Price Statistics

```
By Category:
  Kopi (Coffee):     Rp 8K - Rp 25K   (mean: ~Rp 16K)
  Mie (Noodles):     Rp 12K - Rp 40K  (mean: ~Rp 26K)
  Martabak:          Rp 15K - Rp 45K  (mean: ~Rp 30K)
  Ayam (Chicken):    Rp 18K - Rp 85K  (mean: ~Rp 52K)

Overall Dataset:
  Min:    Rp 0 (extreme outlier)
  Max:    Rp 8,302,000 (extreme outlier)
  Mean:   Rp 113,474
  Median: Rp 29,000
```

---

## 🚀 Quick Start

### 1️⃣ Load Dataset
```python
import pandas as pd
import numpy as np

# Load raw data (messy)
df = pd.read_csv('synthetic_fooddelivery_dataset.csv')

# Load cleaned version (for comparison)
df_clean = pd.read_csv('synthetic_fooddelivery_dataset_CLEANED.csv')

print(df.shape)  # (8500, 11)
print(df.info())
```

### 2️⃣ Basic Exploration
```python
# Missing values
print(df.isnull().sum())

# Data types
print(df.dtypes)

# Quick stats
print(df.describe())

# Category distribution
print(df['Kategori_Menu'].value_counts())
```

### 3️⃣ Data Cleaning Example
```python
# Handle missing values
df['Jarak_Kirim_KM'].fillna(df['Jarak_Kirim_KM'].median(), inplace=True)
df['Rating_Pelanggan'].fillna(3, inplace=True)

# Fix timestamp format (standardize to YYYY-MM-DD)
def parse_timestamp(ts):
    try:
        if "/" in str(ts):
            return pd.to_datetime(ts, format="%d/%m/%Y %H:%M")
        else:
            return pd.to_datetime(ts)
    except:
        return pd.NaT

df['Waktu_Transaksi'] = df['Waktu_Transaksi'].apply(parse_timestamp)

# Remove outliers (optional)
df_clean = df[(df['Harga_Pesanan'] >= 500) & (df['Harga_Pesanan'] <= 500000)]
```

### 4️⃣ Modeling Example (Classification)
```python
from sklearn.model_selection import train_test_split
from sklearn.preprocessing import LabelEncoder
from sklearn.ensemble import RandomForestClassifier

# Prepare features
X = df[['Harga_Pesanan', 'Jarak_Kirim_KM', 'Waktu_Tunggu_Menit', 'Rating_Pelanggan']]
y = df['Status_Pesanan']

# Remove rows with NaN
X = X.dropna()
y = y[X.index]

# Train-test split
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42)

# Train model
model = RandomForestClassifier(n_estimators=100, random_state=42)
model.fit(X_train, y_train)

# Evaluate
print(f"Accuracy: {model.score(X_test, y_test):.2%}")
```

---

## 📚 Use Cases

### 1. Data Cleaning & Preprocessing
- ✅ Handle missing values (Jarak_Kirim_KM, Rating_Pelanggan)
- ✅ Detect & treat outliers (Harga_Pesanan)
- ✅ Standardize datetime formats
- ✅ Text preprocessing for reviews

### 2. Exploratory Data Analysis (EDA)
- ✅ Distribution analysis
- ✅ Correlation analysis (verify causal relationships)
- ✅ Temporal patterns (peak hours, day-of-week effects)
- ✅ Category imbalance investigation

### 3. Feature Engineering
- ✅ Extract hour, day, month from timestamp
- ✅ Create lag features (time-based)
- ✅ Sentiment analysis on reviews
- ✅ One-hot encoding for categories

### 4. Machine Learning
- ✅ **Classification**: Predict Status_Pesanan (Selesai/Dibatalkan/Refund)
- ✅ **Regression**: Predict Waktu_Tunggu_Menit
- ✅ **Regression**: Predict Rating_Pelanggan
- ✅ **Clustering**: Customer segmentation
- ✅ **NLP**: Review sentiment classification

### 5. Business Intelligence
- ✅ Peak hour analysis
- ✅ Category performance insights
- ✅ Complaint analysis
- ✅ Revenue patterns

---

Dataset ini **100% synthetic** dan dibuat untuk tujuan **educational & research**.
Silakan gunakan untuk:
- ✅ Learning data science
- ✅ Portfolio projects
- ✅ Model training
- ✅ Teaching/workshop

---