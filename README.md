# Audit Margin Rumaku Home & Living (2025)

Analisis data untuk menjawab satu pertanyaan dari Head of Retail di awal Januari 2026:

> "Penjualan kuartal empat kemarin naik dan itu bagus. Tapi laba kita malah turun dan saya tidak bisa
> menjelaskan ke direksi kenapa. Tolong cari tahu apa yang sebenarnya terjadi, dan kasih saya sesuatu
> yang bisa saya putuskan minggu depan."

Project ini adalah tugas mini project Bootcamp Data Analyst oleh Seara Data, memakai Python (pandas, matplotlib) untuk membersihkan, menggabungkan, dan
menganalisis data operasional Rumaku Home & Living periode 2024–2025.

## Pertanyaan yang dijawab

1. Berapa besar penurunan laba dan di kuartal mana persisnya?
2. Apakah penyebabnya bauran produk, kanal penjualan, atau kebijakan diskon?
3. Kategori mana yang paling terdampak dan seberapa besar kontribusinya?
4. Apakah kanal online tetap menguntungkan setelah ongkos kirim dihitung?
5. Tindakan apa yang paling cepat memulihkan margin tanpa membunuh volume?

## TL;DR hasil analisis

- **Revenue Q4 2025 naik 26%** dari Q3 (Rp 6,65 M), tapi **gross profit malah turun 26%** di kuartal
  yang sama. Gross margin anjlok ke **15,6%** — titik terendah dari 8 kuartal terakhir (rata-rata
  kuartal lain ~26–27%).
- Penyebabnya **murni kebijakan diskon**, bukan pergeseran bauran produk atau kanal: rata-rata diskon
  melompat dari ~9% (Q3 2025) menjadi ~21% (Q4 2025), terjadi merata di semua kategori dan kedua kanal.
- **Kasur & Springbed, Lemari & Rak, dan Meja** menyumbang ~78% dari total penurunan laba kuartalan.
- Kanal online **masih untung setelah ongkos kirim dihitung**, tapi tipis sekali: net margin turun dari
  17,7% (Q3) ke **6,1%** (Q4 2025).
- Rekomendasi utama: turunkan plafon diskon dari ~21% ke ~12–15%, terutama untuk kategori bermargin
  tipis, dan ganti diskon flat dengan skema bertingkat per kategori. Detail lengkap ada di notebook.

Satu temuan penting di proses cleaning: ada beberapa transaksi dengan `unit_price` tertulis
**Rp 950.000.000** (seharusnya jutaan, bukan ratusan juta) akibat salah input. Tanpa dikoreksi, bug ini
membuat margin bulanan tertentu terlihat jauh lebih sehat dari kenyataan dan bisa menutupi masalah
sesungguhnya di Q4 — detail deteksi dan koreksinya ada di bagian cleaning pada notebook.

## Struktur repo

```
rumaku-margin-audit/
├── README.md
├── requirements.txt
├── data/
│   ├── transaksi.csv      # 12.240 baris — tabel fakta, 1 baris = 1 item order
│   ├── produk.csv         # 170 baris — dimensi produk, kategori, HPP
│   ├── pelanggan.csv      # 6.825 baris — dimensi pelanggan
│   └── toko.csv           # 13 baris — dimensi toko (12 fisik + 1 warehouse online)
└── notebooks/
    └── Rumaku_Audit_Margin_2025.ipynb   # notebook analisis lengkap
```

## Cara menjalankan

```bash
git clone <url-repo-ini>
cd rumaku-margin-audit
pip install -r requirements.txt
jupyter notebook notebooks/Rumaku_Audit_Margin_2025.ipynb
```

Notebook membaca data langsung dari folder `data/` di repo ini (path relatif `../data/...`), jadi
tinggal `Run All` dari atas ke bawah — tidak perlu mount Google Drive atau path tambahan apa pun.

## Alur notebook

1. **Import & load data** — baca 4 tabel sumber.
2. **Data quality check** — `.shape`, `.info()`, `.describe()`, cek nilai kosong dan duplikat.
3. **Cleaning** — parsing tanggal 2 format, imputasi nilai kosong, standardisasi `channel` dan
   `unit_price`, deteksi & koreksi outlier harga terhadap `list_price` produk, penanganan `order_id`
   duplikat.
4. **Penggabungan & metrik bisnis** — merge 3 tabel, hitung `revenue`, `cogs`, `gross_profit`,
   `net_profit` per transaksi.
5. **Analisis per pertanyaan** — satu bagian notebook per pertanyaan (1–5), masing-masing dengan tabel,
   chart, dan interpretasi tertulis.
6. **Rekomendasi** — penutup berisi tindakan konkret untuk kuartal berikutnya.

## Tools

Python 3, pandas, numpy, matplotlib — dijalankan di Jupyter/Google Colab.

---
*Bootcamp Data Analyst by Seara Data (Batch 2) — Hari 6: Python untuk Analisis Data.*
