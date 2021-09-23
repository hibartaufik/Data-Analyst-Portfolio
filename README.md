# Data Analyst Portfolio

Menganalisa COVID-19 Cases menggunakan COVID-19 dataset dari [Our World in Data](https://ourworldindata.org/) dengan rentang waktu 28 Januari 2020 - 20 September 2021. Dataset dapat diakses [disini](https://ourworldindata.org/covid-deaths). Tahapan analisa terbagi menjadi 4 tahap:
- SQL Data Exploration
- Tableau Visualization
- SQL Data Cleansing
- Python Analysing

## 1. SQL Data Exploration

### 1.1 Dataset
Download dataset pada situs Our World In Data khusus untuk COVID-19, lalu menetukan rentang waktu dataset yang akan diambil

<img width=500 src=https://user-images.githubusercontent.com/74480780/134254497-cdc353f5-f27f-4769-b2e7-21e99498b860.png>

File akan ter-download dengan extensi csv, tampilkan pada excel dengan beberapa perubahan format agar tampilan dataset pada excel berupa tabel

- comma-separated shape

<img width=500 src=https://user-images.githubusercontent.com/74480780/134254840-19f0fd4a-8a2e-451c-bb1f-01601c191e76.png>

- Perubahan menjadi tampilan tabel

<img width=500 src=https://user-images.githubusercontent.com/74480780/134255525-18e1b3a8-4da6-46bf-beec-e797c34455b0.png>

### 1.2 Membuat Tabel Baru dari Dataset Utama

Membuat dua tabel baru dari dataset utama, yaitu mengenai data kematian COVID-19 dan data vaksinasi COVID-19

- CovidDeaths.xlsx

<img width=500 src=https://user-images.githubusercontent.com/74480780/134256471-4c43c718-a55b-4de4-aa91-8104a4f1de2a.png>

- CovidVaccinations.xlsx

<img width=500 src=https://user-images.githubusercontent.com/74480780/134256531-f835053b-60eb-4efc-9005-8901719a8027.png>

### 1.3 Membuat Database dan Import Dataset

Membuat Database dengan Microsoft SQL Server Management Studio

<img width=500 src=https://user-images.githubusercontent.com/74480780/134482443-05337d06-de9f-463a-89c5-38a300ddd472.png>

Import CovidDeaths.xlxs dan CovidVaccinations.xlxs ke dalam database

<img width=500 src=https://user-images.githubusercontent.com/74480780/134498212-a8732aec-cc9a-4b5d-b868-5c572cc528be.png>

### 1.4 Data Exploration

Eksplorasi data dengan SQL Query

#### 1.4.1 Melihat data dari kedua tabel

```
SELECT * 
FROM PortfolioProject.dbo.CovidDeaths
ORDER BY 3, 4
```

<img width=500 src=https://user-images.githubusercontent.com/74480780/134499458-87dd37ec-a373-47d8-88ce-4e0193fdbf42.png>

```
SELECT * 
FROM PortfolioProject.dbo.CovidVaccinations
ORDER BY 3, 4
```

<img width=500 src=https://user-images.githubusercontent.com/74480780/134499616-6db88524-6fd3-4d8c-9c65-661df13f2c34.png>

#### 1.4.2 Menampilkan Data yang Akan Digunakan

Terdapat 26 field pada tabel sehingga tidak efisien jika kita menampilkan semua data termasuk data yang kurang informatif, pilih beberapa field yang dianggap penting dan lebih informatif dari field yang lainnya.

- Eksplorasi Data Pada Tabel CovidDeaths

Memilih field Location, date, total_cases, new_cases, total_deaths, population agar lebih informatif.

```
SELECT Location, date, total_cases, new_cases, total_deaths, population
FROM PortfolioProject.dbo.CovidDeaths
ORDER BY 1, 2
```

Mengurutkan berdasarkan Location dan date agar menampilkan data berdasarkan lokasi yang berisi nama negara lalu selanjutnya diurutkan berdasarkan tanggal sehingga kita lebih mudah menganalisa beberapa faktor yang berkaitan dengan rentang waktu seperti perbandingan kenaikan dan penurunan kasus tertentu.

<img width=500 src=https://user-images.githubusercontent.com/74480780/134501649-0b93c1f7-5a59-425a-b34c-8ba4c60aff55.png>

Dalam sekilas data yang kita lihat, untuk field total_deaths berisi NULL karena pandemi merebak di berbagai tempat tidak akan langsung menimbulkan kematian. 

```
SELECT Location, date, total_cases, new_cases, total_deaths, population
FROM PortfolioProject.dbo.CovidDeaths
WHERE total_deaths != 'NULL'
ORDER BY 1, 2
```

<img width=500 src=https://user-images.githubusercontent.com/74480780/134502883-df2c0b05-4808-439b-b509-3301fd6ce6b1.png>

Angka kematian akan muncul beberapa waktu setelah pandemi menyebar dalam suatu lokasi. Untuk Afghanistan misalnya, angka kematian baru muncul pada 23 Maret 2020. Tentu penyebaran pandemi COVID-19 dan dampak kematiannya akan berbeda pada setiap lokasi/negara, hal ini valid jika kita eksplor data dengan nama lokasi yang berbeda, Contohnya Indonesia.

```
SELECT Location, date, total_cases, new_cases, total_deaths, population
FROM PortfolioProject.dbo.CovidDeaths
WHERE Location = 'Indonesia' AND total_deaths != 'NULL'
ORDER BY 1, 2
```

<img width=500 src=https://user-images.githubusercontent.com/74480780/134503504-0e2442d2-24a2-49a1-ac71-c99900531bc5.png>

Dataset yang kita eksplor mencatat bahwa di Indonesia, pandemi baru menimbulkan kematian dimulai pada 11 Maret 2020.
