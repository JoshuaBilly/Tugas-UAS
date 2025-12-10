# LAPORAN PENJELASAN TIAP NODE PADA WORKFLOW KNIME
Bryan - C14250077 / Billy - C14250067
-----------------------------------------------------
# 1. CSV Reader

Node ini digunakan untuk membaca seluruh data dari file ToyotaCorolla.csv.
Data yang diperoleh mencakup harga mobil, usia mobil, kilometer pemakaian, jenis bahan bakar, tenaga mesin, jumlah pintu, dan berat mobil.
Tujuan tahap ini adalah menyediakan seluruh data untuk proses analisis selanjutnya.

Insight:
Pada tahap ini belum ada insight, karena data baru diimpor dan belum diolah.

# 2. Column Filter

Node ini digunakan untuk memilih kolom yang penting dan membuang kolom yang tidak relevan.
Kolom yang dipertahankan antara lain: Price, Age, KM, FuelType, HP, Automatic, Doors, dan Weight.

Insight:
Dengan menyederhanakan kolom, analisis menjadi lebih terarah. Kolom-kolom yang dipilih memang merupakan faktor umum yang memengaruhi harga mobil di pasar.

# 3. Row Filter

Node ini menyaring baris-baris yang tidak masuk akal, seperti harga = 0 atau kilometer = 0.
Penyaringan dilakukan agar data yang digunakan valid dan tidak merusak hasil model.

Insight:
Setelah disaring, dataset menjadi lebih bersih, sehingga potensi error dalam analisis berkurang. Data ekstrem yang tidak realistis berhasil dihilangkan.

# 4. GroupBy
Membuat 3 Node GroupBy yang masing-masing terhubung ke Row Filter
---
Node GroupBy 1:
Node ini menghitung rata-rata harga mobil berdasarkan jenis bahan bakar.
Kolom FuelType digunakan sebagai pengelompokan, sementara Price dihitung nilai rata-ratanya.

Insight Utama:
Dari perhitungan rata-rata, terlihat bahwa jenis bahan bakar memang memengaruhi harga mobil.
Biasanya ditemukan pola seperti:

Mobil Diesel memiliki rata-rata harga lebih tinggi

Mobil Petrol berada di tengah

CNG bisa lebih murah atau sedang, tergantung dataset

Insight ini menunjukkan bahwa FuelType merupakan salah satu faktor penting dalam menentukan harga mobil.

---
Node GroupBy 2:
Node ini menghitung rata-rata jarak tempuh (KM) berdasarkan umur dari kendaraan.
Kolom Age_08_04 digunakan sebagai pengelompokan, sementara KM dihitung nilai rata-ratanya.

Insight Utama:
Dari perhitungan rata-rata, terlihat bahwa umur memang memengaruhi harga mobil.
Biasanya ditemukan pola seperti:

Semakin tua umur mobil, maka nilai rata-rata KM-nya cenderung besar.
Namun, ada beberapa mobil yang umurnya lebih tua tapi nilai rata-rata KM-nya rendah.
Hal ini bergantung pada dataset yang ada, bisa jadi karena mobil tidak laku, HP rendah, dan lain sebagainya.

Insight ini menunjukkan bahwa Age_08_04 dapat menjadi faktor dalam menentukan lakunya mobil (di lihat dari KM-nya).

---
Node GroupBy 3:
Node ini menghitung rata-rata harga mobil berdasarkan Horse Power yang dimiliki mesin mobil.
Kolom HP digunakan sebagai pengelompokan, sementara Price dihitung nilai rata-ratanya.

Insight Utama:
Dari perhitungan rata-rata, terlihat bahwa HP memang memengaruhi harga mobil.
Biasanya ditemukan pola seperti:
Mobil dengan HP yang lebih tinggi cenderung lebih mahal.
Namun, jika selisih HP hanya berbeda sedikit saja, kemungkinan besar mobil tersebut tidak laku karena tidak sepadan dengan harganya.
Hal tersebut dapat di lihat pada dataset (HP: 71, 72, 73, 86). 
Selisih HP nya sangat kecil, sehingga lebih sepadan membeli mobil yang HP-nya 72 dan 86 dibandingkan dari 71 ke 72 atau 72 ke 73.
Secara umum, rata-rata harga mobil akan meningkat ketika HP-nya besar.

Insight ini menunjukkan bahwa HP merupakan salah satu faktor penting dalam menentukan harga mobil.

# 5. Column Renamer

Node ini digunakan untuk mengganti nama kolom agar lebih mudah dipahami, misalnya “Mean(Price)” menjadi “AvgPrice” dan "Mean(KM)" menjadi "AcgKM"

Insight:
Belum ada insight analitis, tetapi nama kolom lebih mudah dibaca dan menghindari kebingungan untuk proses berikutnya.

# 6. Chart

# Bar Chart
Node ini menampilkan grafik harga rata-rata berdasarkan jenis bahan bakar.
<img width="1205" height="907" alt="image" src="https://github.com/user-attachments/assets/6394efec-e2da-4c1d-8fc8-1c49066973aa" />

Insight Utama:
Grafik memperjelas perbedaan harga antara jenis bahan bakar.
Dari visualisasi biasanya terlihat jelas bahwa perbedaan harga antar FuelType cukup signifikan, sehingga FuelType dapat dipertimbangkan dalam penentuan harga atau strategi penjualan.

---
# Line Plot 1
Node ini menampilkan grafik rata-rata KM berdasarkan umur mobil.
<img width="1251" height="901" alt="image" src="https://github.com/user-attachments/assets/9635a708-fc85-4f8a-9cb2-8a3e82514fec" />

Insight Utama:
Grafik memperjelas kenaikan rata-rata KM berdasarkan umur mobil.
Dapat dilihat bahwa seiring bertambahnya umur mobil, rata-rata KM-nya semakin meningkat. Hal ini dapat menjadi penentu bahwa mobil yang tercatat rata-rata KM-nya tinggi merupakan mobil yang banyak dibeli atau laku di pasaran karena terbukti banyak digunakan di jalan.

---
# Line plot 2
Node ini menampilkan kenaikan rata-rata harga berdasarkan Horse Power yang dimiliki mesin mobil.
<img width="1255" height="907" alt="image" src="https://github.com/user-attachments/assets/385e24cf-1a6d-4ed8-bee3-b0750bd36da3" />

Insight Utama:
Grafik memperjelas kenaikan harga berdasarkan Horse Power yang dimiliki oleh mesin mobil.
Dapat dilihat dalam visualisasinya bahwa semakin besar HP, maka semakin mahal mobil tersebut.
Hal ini dapat menjadi salah satu faktor dalam mempertimbangkan harga mobil.



# 7. Rule Engine

Node ini mengubah harga menjadi tiga kategori: Low, Medium, dan High.
Kategorisasi dibuat berdasarkan batas harga tertentu yang ditentukan sebelumnya.

Insight:
Dengan membuat kategori, analisis harga menjadi lebih mudah dipahami.
Model prediksi juga bisa dibangun dengan lebih jelas karena targetnya berbentuk kelas, bukan angka mentah.

# 8. Partitioning

Node ini membagi data menjadi data training (sekitar 80%) dan data testing (sekitar 20%).
Pembagian ini diperlukan untuk menguji kemampuan model.

Insight:
Belum ada insight analitis, tetapi pembagian data memastikan bahwa model tidak “menghafal” data dan bisa diuji dengan adil.

# 9. Decision Tree Learner

Node ini membangun model decision tree untuk mencari pola faktor-faktor yang memengaruhi kategori harga mobil.

Insight Utama:
Dari model decision tree, biasanya ditemukan bahwa:

KM (kilometer pemakaian) dan Age (umur mobil) sangat berpengaruh pada harga. Semakin tua dan semakin banyak km → harga cenderung Low.

Weight dan HP sering muncul pada cabang yang menunjukkan harga lebih tinggi.

FuelType juga sering berperan dalam menentukan kelas harga.

Insight ini membuktikan bahwa faktor fisik dan performa mobil memang menjadi penentu yang nyata.

# 10. Decision Tree Predictor

Node ini menggunakan model yang telah dibuat untuk memprediksi kategori harga pada data testing.

Insight:
Pada tahap ini belum ada insight langsung karena outputnya adalah prediksi yang nanti dibandingkan dengan data asli.

# 11. Scorer

Node ini mengevaluasi performa prediksi menggunakan confusion matrix dan akurasi.

Insight Utama:
Beberapa temuan yang biasanya diperoleh:

Model memiliki akurasi tertentu (misalnya 80–90%), yang menunjukkan bahwa decision tree cukup baik untuk dataset ini.

Kelas “Low” biasanya paling mudah diprediksi karena cirinya jelas (umur tua + KM tinggi).

Kelas “Medium” sering tertukar dengan kelas lain karena rentangnya di tengah dan datanya lebih bervariasi.

Kelas “High” biasanya terprediksi cukup stabil jika faktor HP dan Weight kuat.

Insight ini menunjukkan bahwa model mampu mempelajari pola harga dengan lumayan baik, tetapi kategori menengah sering menjadi tantangan dalam klasifikasi.

# KESIMPULAN INSIGHT UTAMA

Secara keseluruhan, workflow ini menghasilkan beberapa insight penting:

Jenis bahan bakar berpengaruh pada harga mobil, terbukti dari rata-rata harga dan grafik.

Faktor paling kuat yang memengaruhi harga mobil adalah Age, KM, HP, Weight, dan FuelType.

Harga mobil bisa diprediksi cukup baik menggunakan decision tree, dengan akurasi yang cukup tinggi.

Kategori harga Low paling mudah diprediksi, sedangkan kategori Medium paling sering salah.

Workflow ini membantu memahami pola harga dan bisa digunakan untuk analisis pasar mobil bekas.
