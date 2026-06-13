# **🚴‍♂️ Bike Stores Sales & Inventory Analytics**

Proyek *Data Engineering* & *Business Intelligence* skala *End-to-End* menggunakan **Medallion Architecture** di atas **Google Cloud Platform (GCP)**. Proyek ini melakukan penarikan data dari Google Sheets (via JSON metadata), pembersihan data di BigQuery, hingga pemodelan dimensi (**Star Schema**) untuk dasbor interaktif **Looker Studio**.

## **🛠️ Tech Stack**

| Kategori | Komponen / Teknologi |
| :---- | :---- |
| **Bahasa Pemrograman** | Python, SQL (Standard SQL BigQuery) |
| **Cloud Platform** | Google Cloud Platform (GCP) |
| **Data Warehouse** | Google BigQuery |
| **Library Python Utama** | Pandas, google-cloud-bigquery, pandas-gbq, requests, pytz |
| **Data Quality & Profiling** | ydata-profiling (Pandas Profiling) |
| **Business Intelligence (BI)** | Looker Studio |

## **📌 Alur Arsitektur Data (Medallion)**

Google Sheets (Source) ──\> Bronze (Raw) ──\> Silver (Cleaned) ──\> Gold (Star Schema) ──\> Looker Studio

1. **Bronze Layer (brz\_bike\_store)**: Mengambil data mentah secara dinamis menggunakan Python (Pandas) tanpa mengubah skema asli (raw\_ tables).  
2. **Silver Layer (slv\_bike\_store)**: Pembersihan data, penanganan nilai kosong (*nulls*), penghapusan duplikasi, standarisasi nama kolom (*snake\_case*), dan konversi tipe data tanggal (DATE).  
3. **Gold Layer (gld\_bike\_store)**: Denormalisasi menjadi model bintang (**Star Schema**) untuk mempercepat performa kueri analitik dan visualisasi bisnis.

## **📂 Struktur File Proyek**

Berikut adalah peta penataan berkas dan folder yang ada di dalam repositori ini:

📁 bike-stores-analytics/  
├── 📁 notebooks/  
│   ├── Bronze\_Layer.ipynb                      
│   ├── Silver\_Layer.ipynb                      
│   └── Gold\_Layer.ipynb                        
├── 📁 reports/  
│   └── profile\_detail\_transaction.html        
├── .gitignore                                  
├── Bike\_Stores\_Sales\_&\_Inventory\_Dashboard.pdf  
├── README.md                                   
└── requirements.txt                          

## **📐 Pemodelan Data (Gold Layer \- Star Schema)**

### **📊 Tabel Fakta (Fact Tables)**

| Nama Tabel | Deskripsi | Metrik Utama / Rumus Bisnis |
| :---- | :---- | :---- |
| **fact\_detail\_transaction** | Transaksi tingkat item (granular) hasil gabungan dari entitas transaksi Silver Layer. | quantity, product\_price, discount  ![][image1] |
| **fact\_summary\_transaction** | Ringkasan harian performa penjualan untuk mempercepat *rendering* grafik. | total\_order\_items, total\_revenue *(Menghemat query cost BigQuery)* |

### **👥 Tabel Dimensi (Dimension Tables)**

| Nama Tabel | Deskripsi | Atribut Utama |
| :---- | :---- | :---- |
| **dim\_products** | Atribut produk sepeda yang dikonsolidasikan. | product\_id, product\_name, brand\_name, category\_name |
| **dim\_customers** | Profil lengkap demografi konsumen. | customer\_id, first\_name, last\_name, city, state, zip\_code |
| **dim\_stores** | Entitas data fisik operasional cabang. | store\_id, store\_name, city, state |
| **dim\_staffs** | Struktur internal tim penjualan dan manajerial. | staff\_id, first\_name, last\_name, active, manager\_id |

## **📈 Temuan Utama Dasbor (Looker Studio)**

Berdasarkan analisis data operasional historis periode **2016 \- 2018**:

### **🔑 Key Performance Indicators (KPIs)**

| Metrik Utama | Nilai Pencapaian | Deskripsi Bisnis |
| :---- | :---- | :---- |
| **Total Revenue** | **$2,88 Juta** | Akumulasi total pendapatan kotor perusahaan |
| **Units Sold** | **2.438 unit** | Total volume sepeda yang berhasil diserap pasar |
| **Total Orders** | **1.615 pesanan** | Jumlah pesanan unik (faktur) yang diproses |
| **Average Order Value (AOV)** | **$1.785** | Rata-rata nilai belanja pelanggan per transaksi |
| **Average Discount** | **7,1%** | Rata-rata potongan harga yang diberikan |

### **🔍 Insight Strategis**

1. **Dominasi Merek:** Merek **TREK** menyumbang pendapatan sebesar **$1.761.160** (lebih dari 60% total omset perusahaan).  
2. **Efektivitas Diskon:** Diskon **5%** adalah batas optimal (*sweet spot*) yang menghasilkan penjualan tertinggi (**1.358 unit / $1,64 Juta**). Menaikkan diskon hingga 20% terbukti tidak efektif karena hanya menghasilkan penjualan 155 unit dan merusak profit margin.  
3. **Pilar Penjualan:** Dua sales staff, **Marcelene Boyer** ($957.839) dan **Venita Daniel** ($957.440), berkontribusi terhadap lebih dari 60% total penjualan toko.

## **👤 Author**

**Febrianto Eko Saputra**

*Aspiring Data Engineer* | Makassar, Indonesia

[LinkedIn]((https://www.linkedin.com/in/febrianto-eko-saputra-67b98b17a/)) | [GitHub](https://github.com/febri-eko)

## **📌 Project Status**

* \[x\] Completed

[image1]: <data:image/png;base64,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>
