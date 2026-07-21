# 🏆 2026 FIFA World Cup Player Performance Analysis

Proyek ini berfokus pada proses **Data Cleaning**, **Exploratory Data Analysis (EDA)**, dan **Visualisasi Data** menggunakan Python (Pandas, SQLite3) untuk menganalisis performa simulasi pemain pada turnamen Piala Dunia 2026. Data yang telah dibersihkan dirancang agar siap digunakan untuk pembuatan dashboard interaktif di Tableau.

---

## 📊 Tautan Penting (Links)
* **Dataset Asli (Kaggle):** [FIFA World Cup 2026 Player Performance Dataset](https://www.kaggle.com/datasets/rauffauzanrambe/fifa-world-cup-2026-player-performance-dataset)
* **Dashboard Visualisasi (Tableau):** *[Tautan Dashboard Tableau di sini](https://public.tableau.com/views/WorldCupAnalysisDummyData/Dashboard1?:language=en-US&:sid=&:redirect=auth&:display_count=n&:origin=viz_share_link)*

---

## 🛠️ Alur Kerja Proyek (Project Workflow)

### 1. Ekstraksi & Import Data
* Mengunduh dataset menggunakan library `kagglehub`.
* Memasukkan data mentah berbentuk `.csv` ke dalam database lokal **SQLite3** (`data_analyst.db`) menggunakan `pandas.to_sql()`.
* Membaca ulang data menggunakan query SQL untuk memastikan efisiensi memori (Data berukuran 54.600 baris dan 75 kolom).

### 2. Data Cleaning & Validasi Logika
Proses pembersihan data dilakukan secara ketat untuk menjamin validitas metrik sepak bola sebelum masuk ke tahap visualisasi:
* **Pengecekan Duplikat:** Dipastikan `0` duplikat murni maupun duplikat logis pada kombinasi kunci (`player_id` + `match_id`).
* **Konversi Waktu:** Mengubah kolom `match_date` menjadi tipe data *datetime* (Rentang turnamen konsisten pada Juni–Juli 2026).
* **Validasi Batas Nilai:** Memastikan menit bermain per *match* maksimal 90 menit, serta memastikan data statistik penyerangan logis (Contoh: `shots_on_target` tidak boleh lebih besar dari total `shots`).
* **Perbaikan Inkonsistensi Agregat (Temuan Krusial):** Ditemukan anomali pada kolom bawaan `total_minutes_tournament` yang tidak sinkron dengan akumulasi menit bermain aktual pada 1.248 pemain. Masalah ini diselesaikan dengan menghitung ulang (*re-aggregating*) total menit, gol, dan assist langsung dari data *match-by-match* menggunakan fungsi `.transform('sum')`.
* **Standardisasi Teks:** Membersihkan *trailing spaces* menggunakan `.str.strip()` pada kolom kategorikal (`position`, `preferred_foot`, `tournament_stage`).

### 3. Temuan Utama Exploratory Data Analysis (EDA)
* **Karakteristik Fisik Pemain:** Rata-rata umur pemain berada di angka **26,3 tahun** (usia emas) dengan tinggi rata-rata **181,6 cm**. Distribusi kaki dominan menunjukkan pengguna kaki kanan (*Right*) mendominasi sebesar **74,4%**.
* **Efisiensi Lini Serang:** Posisi *Forward* memiliki produktivitas tertinggi (rata-rata 0.14 gol per match). Ditemukan fenomena *overperformance xG*, di mana angka gol aktual secara konsisten melampaui nilai `expected_goals_xg` di semua posisi penyerangan.
* **Kedisplinan per Posisi:** Gelandang (*Midfielder*) mencatatkan rata-rata pelanggaran tertinggi (**0.44 per match**), namun posisi Bek (*Defender*) menjadi pengoleksi kartu kuning terbanyak (**0.12 per match**).
* **Faktor Penentu Player Rating:** Melalui matriks korelasi, ditemukan bahwa performa individu (`player_rating`) sangat dipengaruhi oleh etos kerja fisik di lapangan, yaitu **jarak tempuh** (`distance_covered_km` dengan korelasi **0.83**) dan **aksi bertahan** (`defensive_actions` dengan korelasi **0.56**), ketimbang jumlah gol atau assist secara langsung.

---

## 🗂️ Struktur File di Repositori
* `data.ipynb` : Jupyter Notebook yang berisi seluruh *source code* proses cleaning dan EDA.
* `piala_dunia_2026_clean.csv` : Dataset hasil akhir pembersihan yang siap digunakan (Format siap pakai untuk Tableau).
* `README.md` : Dokumentasi penjelasan proyek.

---

## 🚀 Teknologi yang Digunakan
* **Bahasa Pemrograman:** Python 3
* **Libraries:** Pandas, SQLite3, Kagglehub, OS
* **Alat Visualisasi:** Tableau Desktop / Public
