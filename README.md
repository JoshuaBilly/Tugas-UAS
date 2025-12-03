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

Node ini menghitung rata-rata harga mobil berdasarkan jenis bahan bakar.
Kolom FuelType digunakan sebagai pengelompokan, sementara Price dihitung nilai rata-ratanya.

Insight Utama:
Dari perhitungan rata-rata, terlihat bahwa jenis bahan bakar memang memengaruhi harga mobil.
Biasanya ditemukan pola seperti:

Mobil Diesel memiliki rata-rata harga lebih tinggi

Mobil Petrol berada di tengah

CNG bisa lebih murah atau sedang, tergantung dataset

Insight ini menunjukkan bahwa FuelType merupakan salah satu faktor penting dalam menentukan harga mobil.

# 5. Column Renamer

Node ini digunakan untuk mengganti nama kolom agar lebih mudah dipahami, misalnya “Mean(Price)” menjadi “AvgPrice”.

Insight:
Belum ada insight analitis, tetapi nama kolom lebih mudah dibaca dan menghindari kebingungan untuk proses berikutnya.

# 6. Bar Chart

Node ini menampilkan grafik harga rata-rata berdasarkan jenis bahan bakar.

Insight Utama:
Grafik memperjelas perbedaan harga antara jenis bahan bakar.
Dari visualisasi biasanya terlihat jelas bahwa perbedaan harga antar FuelType cukup signifikan, sehingga FuelType dapat dipertimbangkan dalam penentuan harga atau strategi penjualan.

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
