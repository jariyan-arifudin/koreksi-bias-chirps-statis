# Koreksi Bias Data CHIRPS: Metode Faktor Koreksi Statis (Static Correction Factor)

![Python](https://img.shields.io/badge/Python-3.8%2B-blue)
![License](https://img.shields.io/badge/License-MIT-green)
![Status](https://img.shields.io/badge/Status-Research-orange)

## Deskripsi Proyek

Repositori ini berisi implementasi kode Python untuk melakukan **koreksi bias sistematik** pada data estimasi curah hujan satelit **CHIRPS v3.0**. Metode yang digunakan adalah pendekatan *Static Bias Correction* (Faktor Koreksi Statis) yang diterapkan secara seragam pada seluruh data runtun waktu (time-series) dan spasial di wilayah Provinsi Jambi.

Pendekatan ini mengasumsikan bahwa bias satelit bersifat linear dan konstan, sehingga dapat dikoreksi menggunakan satu koefisien global (Global Coefficient) yang diturunkan dari perbandingan total curah hujan observasi dan satelit.

## Metodologi

Metode ini menggunakan prinsip keseimbangan massal (*mass balance*) antara data stasiun penakar hujan dan data satelit. Persamaan koreksi yang digunakan adalah:

$$P_{corr} = P_{sat} \times CF$$

Dimana Faktor Koreksi ($CF$) dihitung dengan rumus:

$$CF = \frac{\sum_{i=1}^{n} P_{obs}}{\sum_{i=1}^{n} P_{sat}}$$

Keterangan:
- $P_{corr}$: Curah hujan terkoreksi.
- $P_{sat}$: Data mentah CHIRPS.
- $P_{obs}$: Data observasi stasiun permukaan.
- $CF$: *Correction Factor* (Nilai skalar tunggal).

Script ini memisahkan stasiun menjadi dua kelompok:
1.  **Stasiun Kalibrasi**: Digunakan untuk menghitung nilai $CF$.
2.  **Stasiun Validasi**: Digunakan secara independen untuk menguji akurasi hasil koreksi (tidak terlibat dalam perhitungan $CF$).

## Prasyarat Instalasi

Script ini dikembangkan di lingkungan **Google Colab**. Berikut adalah dependensi pustaka yang diperlukan:

### Daftar Pustaka
* **Geospasial:** `rasterio`, `geopandas`, `shapely`
* **Analisis Data:** `pandas`, `numpy`, `scikit-learn`, `scipy`
* **Visualisasi:** `matplotlib`, `seaborn`
* **Lainnya:** `tqdm` (Progress monitoring)

### Instalasi
```bash
pip install rasterio geopandas shapely matplotlib scikit-learn pandas numpy seaborn tqdm folium

```

## Struktur Direktori Data

Agar script dapat berjalan, susun data di Google Drive/Local Storage Anda seperti berikut:

```text
/Direktori_Project/
├── CHIRPS v3/                  # File raster .tif data mentah CHIRPS
├── Batas Adm Jambi/            # Shapefile batas administrasi (.shp)
├── Data Stasiun BWS/           # File .csv data curah hujan observasi
└── script_static_bias.ipynb    # Script utama

```

*Catatan: Sesuaikan variabel `BASE_DIR` di dalam script dengan lokasi folder Anda.*

## Cara Penggunaan

1. **Input Data**: Pastikan data CHIRPS (bulanan), Shapefile batas wilayah, dan data stasiun (format CSV time-series) sudah tersedia.
2. **Konfigurasi Stasiun**: Edit daftar `bias_stations` (untuk kalibrasi) dan `validation_stations` (untuk validasi) di dalam script sesuai kebutuhan riset.
3. **Eksekusi**: Jalankan script. Program akan secara otomatis:
* Menghitung total hujan obs vs satelit.
* Menghasilkan nilai .
* Melakukan validasi silang pada stasiun uji.


4. **Output**: Hasil berupa tabel validasi (`.csv`) dan grafik performa akan tersimpan di folder `bias_corrected_static`.

## Contoh Hasil Validasi

Metode ini menghasilkan koreksi yang cepat dan efisien secara komputasi. Berikut adalah contoh format output statistik yang dihasilkan:

| Stasiun Validasi | NSE (Raw) | NSE (Corrected) | RSR (Raw) | RSR (Corrected) |
| --- | --- | --- | --- | --- |
| **Muara Tembesi** | -0.45 | **0.55** | 1.20 | **0.67** |
| **Rantau Pandan** | 0.12 | **0.48** | 0.93 | **0.72** |

> *Output visual berupa Scatter Plot dan Bar Chart perbandingan RSR juga dihasilkan secara otomatis.*

## Penulis

**Jariyan Arifudin** Mahasiswa Geografi Lingkungan

Universitas Gadjah Mada (UGM)

## Lisensi & Sitasi

Kode ini didistribusikan di bawah **MIT License**.
Jika Anda menggunakan metode ini untuk penelitian, silakan sitasi repositori ini:

> Arifudin, J. (2026). *Koreksi Bias Data CHIRPS: Metode Faktor Koreksi Statis (Static Correction Factor)*. GitHub Repository.
