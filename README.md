# KIMIA FARMA DASHBOARD ANALYTICS 2020-2023 
## Patricia Pingkan Kumenap

## Project Portfolio 
PT Kimia Farma merupakan perusahaan industri farmasi pertama di Indonesia yang sampai saat ini terus berkembang menghadirkan layanan kesehatan bagi setiap masyarakat Indonesia. 
Dalam mencapai komitmen dan konsistensinya untuk senantiasa berkontribusi dalam industri farmasi, PT Kimia Farma tentu memerlukan analisis dan evaluasi terhadap kinerja bisnis sebagai upaya dalam mencapai ketahanan yang berkelanjutan. Untuk itu, implementasi tersebut diwujudkan salah satunya melalui proses analisa big data yang disajikan secara visual dengan mencakup tren penjualan PT Kimia Farma dari periode 2020-2023, total transaksi cabang tertinggi dalam jangkauan provinsi, total penjualan cabang tertinggi dalam jangkauan provinsi, persebaran profit untuk masing-masing provinsi, dan lain-lain yang mencakup kinerja bisnis selama periode tersebut. Adapun implementasi ini menjadi salah satu cara yang tepat bagi PT Kimia Farma untuk terus berkembang melalui hasil evaluasi yang nantinya disajikan pada dashboard analytics. Dengan begitu, PT Kimia Farma dapat mengidentifikasi, mengukur, dan menentukan strategi-strategi selanjutnya dalam menghadapi keberlangsungan pola konsumen di Indonesia. 

## Prioritas Tujuan 
Dalam menyajikan visualisasi dashboard analytics terhadap performa penjualan PT Kimia Farma selama periode 2020-2023, terdapat beberapa tujuan yang menjadi prioritas utama yang ingin dicapai PT Kimia Farma yakni sebagai berikut. 
1. Memperoleh Perbandingan Pendapatan Kimia Farma dari tahun ke tahun
2. Mengetahui Total transaksi cabang provinsi
3. Mengetahui total penjualan atau nett sales cabang provinsi
4. Mengetahui kondisi ada tidaknya ketimpangan seperti cabang dengan rating tertinggi, namun memiliki rating transaksi terendah
5. Memperoleh gambaran secara grafis mengenai total profit untuk masing-masing provinsi
Selain itu adapun tujuan lainnya sebagai sumber informasi tambahan bagi evaluasi PT Kimia Farma yakni :
1. Mengetahui proporsi penjualan per produk
2. Mengetahui produk mana yang menghasilkan profit tertinggi

## Sumber Data Yang Digunakan 
Adapun beberapa sumber data yang digunakan dalam melakukan proses analisa terhadap evaluasi performa kinerja PT Kimia Farma Periode 2020-2023 yakni sebagai berikut. 
1. Tabel Data kf_final_transaction :

| Kolom               | Deskripsi                                               | Tipe Data        |
|----------------------|---------------------------------------------------------|------------------|
| transaction_id       | Kode ID transaksi                                       | STRING / VARCHAR |
| product_id           | Kode produk obat                                        | STRING / VARCHAR |
| branch_id            | Kode ID cabang Kimia Farma                              | STRING / VARCHAR |
| customer_name        | Nama customer yang melakukan transaksi                  | STRING / VARCHAR |
| date                 | Tanggal transaksi dilakukan                             | DATE / DATETIME  |
| price                | Harga obat                                              | NUMERIC / FLOAT  |
| discount_percentage  | Persentase diskon yang diberikan pada obat              | NUMERIC / FLOAT  |
| rating               | Penilaian konsumen terhadap transaksi yang dilakukan    | INTEGER (1–5)    |

2. Tabel Data kf_product :

| Kolom            | Deskripsi            | Tipe Data        |
|------------------|----------------------|------------------|
| product_id       | Kode produk obat     | STRING / VARCHAR |
| product_name     | Nama produk obat     | STRING / VARCHAR |
| product_category | Kategori produk obat | STRING / VARCHAR |
| price            | Harga obat           | NUMERIC / FLOAT  |

3. Tabel Data kf_inventory :
   
| Kolom         | Deskripsi                  | Tipe Data        |
| ------------- | -------------------------- | ---------------- |
| inventory\_id | Kode inventory produk obat | STRING / VARCHAR |
| branch\_id    | Kode ID cabang Kimia Farma | STRING / VARCHAR |
| product\_id   | Kode ID produk obat        | STRING / VARCHAR |
| product\_name | Nama produk obat           | STRING / VARCHAR |
| opname\_stock | Jumlah stok produk obat    | INTEGER          |

4. Tabel Data kf_kantor_cabang :

| Kolom            | Deskripsi                                      | Tipe Data        |
| ---------------- | ---------------------------------------------- | ---------------- |
| branch\_id       | Kode ID cabang Kimia Farma                     | STRING / VARCHAR |
| branch\_category | Kategori cabang Kimia Farma                    | STRING / VARCHAR |
| branch\_name     | Nama kantor cabang Kimia Farma                 | STRING / VARCHAR |
| kota             | Kota cabang Kimia Farma                        | STRING / VARCHAR |
| provinsi         | Provinsi cabang Kimia Farma                    | STRING / VARCHAR |
| rating           | Penilaian konsumen terhadap cabang Kimia Farma | INTEGER (1–5)    |

## Hasil Dashboard Analytics 
Adapun berikut hasil dashboard analytics mengenai performa penjualan PT Kimia Farma periode 2020-2023 yakni sebagai berikut. 
![DASHBOARD_PERFORMA_PENJUALAN_KIMIA_FARMA_2020-2023_page-0001](https://github.com/user-attachments/assets/5b0fb093-aada-4848-8c1b-c03c4e427883)# Kimia Farma Dashboard Analytics 2020-2023

## Syntax Query 
Berikut merupakan salinan kode dari BigQuery dalam membuat tabel analisa sebagai dasar visualisasi dashboard analytics : 


```sql
CREATE OR REPLACE TABLE rakamin-kf-analytics-470615.Kimia_Farma.tabel_analisa AS
SELECT
    t.transaction_id,
    t.date,
    t.branch_id,
    kc.branch_name,
    kc.kota,
    kc.provinsi,
    kc.rating AS rating_cabang,
    t.customer_name,
    p.product_id,
    p.product_name,
    p.price AS actual_price,
    t.discount_percentage,

    CASE
        WHEN p.price <= 50000 THEN 0.10
        WHEN p.price > 50000 AND p.price <= 100000 THEN 0.15
        WHEN p.price > 100000 AND p.price <= 300000 THEN 0.20
        WHEN p.price > 300000 AND p.price <= 500000 THEN 0.25
        ELSE 0.30
    END AS persentase_gross_laba,
    -- Nett Sales : Harga Setelah Diskon 
    (p.price * (1 - t.discount_percentage / 100.0)) AS nett_sales, 
    -- Nett Profit 
    (p.price * (1 - t.discount_percentage / 100.0)) * 
    CASE 
        WHEN p.price <= 50000 THEN 0.10
        WHEN p.price > 50000 AND p.price <= 100000 THEN 0.15
        WHEN p.price > 100000 AND p.price <= 300000 THEN 0.20
        WHEN p.price > 300000 AND p.price <= 500000 THEN 0.25
        ELSE 0.30
    END AS nett_profit, t.rating AS rating_transaksi

FROM rakamin-kf-analytics-470615.Kimia_Farma.kf_final_transaction t
JOIN rakamin-kf-analytics-470615.Kimia_Farma.kf_kantor_cabang kc
    ON t.branch_id = kc.branch_id
JOIN rakamin-kf-analytics-470615.Kimia_Farma.kf_product p
    ON t.product_id = p.product_id;
```
## Sumber Link 
### Link Dashboard Analytics 
[Dashboard Analytics](https://lookerstudio.google.com/reporting/9136f94f-572d-4ec6-ae5a-2d2b46acffd8)

## Penulis 
Patricia Pingkan Kumenap 
Mahasiswa S-1 Statistika Universitas Sultan Ageng Tirtayasa 





    

