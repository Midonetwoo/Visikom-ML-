# Visikom-ML

Nama: Muhammad Dirga Apriliansyah

NIM: 2209106050

Proyek ini bertujuan untuk membangun dan mengevaluasi model machine learning sederhana untuk tugas klasifikasi gambar, khususnya membedakan antara gambar kucing dan anjing. Proyek ini menggunakan dataset "Cats vs Dogs" dan mengimplementasikan pipeline klasifikasi gambar konvensional yang melibatkan tahap preprocessing, ekstraksi fitur, dan pelatihan model klasifikasi.

## Dataset yang Digunakan

**Dataset:** Cats vs Dogs Dataset dari Microsoft
**Link:** https://download.microsoft.com/download/3/E/1/3E1C3F21-ECDB-4869-8368-6DEBA77B919F/kagglecatsanddogs_5340.zip

**Penjelasan Dataset:**
Dataset ini merupakan versi yang disederhanakan dari dataset Cats vs Dogs yang populer di Kaggle. Dataset ini berisi ribuan gambar yang dibagi menjadi dua kategori utama: gambar kucing dan gambar anjing. Dataset ini umum digunakan sebagai benchmark awal untuk tugas klasifikasi gambar binary.

- **Kelas:** Dataset ini memiliki 2 kelas, yaitu Kucing (direpresentasikan dengan label 0) dan Anjing (direpresentasikan dengan label 1). Ini menjadikannya masalah klasifikasi biner.
- **Jumlah Data yang Digunakan:** Dalam proyek ini, kami menggunakan **5000 gambar kucing** dan **5000 gambar anjing**, sehingga total ada **10.000 gambar** yang digunakan untuk pelatihan dan pengujian model. Pembatasan jumlah ini dilakukan untuk efisiensi waktu komputasi, terutama saat menggunakan metode ekstraksi fitur tradisional seperti HOG dan melatih model seperti SVM.

## Preprocessing

Tahap preprocessing bertujuan untuk menyiapkan data gambar agar sesuai untuk proses ekstraksi fitur dan pelatihan model.

- **Resize gambar ke 128x128 piksel:**
    - **Alasan Penggunaan:** Model machine learning umumnya memerlukan input dengan ukuran yang konsisten. Dengan me-resize semua gambar ke ukuran yang sama (128x128), kita memastikan bahwa input ke tahap ekstraksi fitur memiliki dimensi yang seragam. Ukuran 128x128 dipilih sebagai kompromi antara menjaga detail gambar dan mengurangi dimensi data serta kompleksitas komputasi.
- **Konversi ke grayscale:**
    - **Alasan Penggunaan:** HOG adalah metode ekstraksi fitur yang bekerja paling baik pada gambar grayscale karena fokus pada intensitas gradien piksel, bukan informasi warna. Mengubah gambar berwarna menjadi grayscale mengurangi dimensi data (dari 3 channel menjadi 1 channel) tanpa kehilangan informasi struktural penting yang dibutuhkan oleh HOG.
- **Normalisasi dengan StandardScaler:**
    - **Alasan Penggunaan:** StandardScaler melakukan standarisasi fitur dengan menghapus rata-rata dan menskalakan ke varians unit. Ini penting untuk algoritma seperti SVM yang sensitif terhadap skala data. Dengan menormalisasi fitur, kita memastikan bahwa tidak ada satu fitur pun (komponen dari vektor HOG) yang mendominasi yang lain hanya karena skalanya lebih besar, sehingga membantu model konvergen lebih baik dan meningkatkan kinerja.

## Feature Extraction

Tahap ekstraksi fitur mengubah data gambar mentah menjadi representasi numerik yang dapat dipahami oleh model machine learning.

- **HOG (Histogram of Oriented Gradients):**
    - **Alasan Penggunaan:** HOG adalah deskriptor fitur yang kuat yang efektif dalam menangkap informasi bentuk dan struktur objek dalam gambar. HOG menghitung distribusi arah gradien intensitas piksel dalam area kecil (cell) dari gambar, yang kemudian dikelompokkan ke dalam histogram. Histogram ini dirangkai menjadi vektor fitur untuk seluruh gambar. HOG dipilih karena kemampuannya menangkap fitur lokal seperti tepi dan sudut yang penting untuk membedakan bentuk hewan seperti kucing dan anjing, dan relatif kuat terhadap variasi pencahayaan dan deformasi kecil.
    - **Parameter HOG:**
        - **9 orientations:** Jumlah bin untuk histogram orientasi gradien. Lebih banyak bin menangkap arah gradien dengan lebih detail.
        - **8x8 pixels per cell:** Ukuran area kecil di mana histogram gradien dihitung. Ukuran cell yang lebih kecil menangkap detail yang lebih halus, sementara yang lebih besar menangkap fitur yang lebih kasar.
        - **2x2 cells per block:** Blok adalah area yang lebih besar yang terdiri dari beberapa cell. Histogram dari cell dalam satu blok dinormalisasi bersama. Normalisasi blok membantu mengatasi perubahan pencahayaan lokal.
- **Output Feature Extraction:** Tahap ini menghasilkan vektor fitur HOG untuk setiap gambar yang digunakan sebagai input untuk model klasifikasi.

## Algoritma

Algoritma klasifikasi digunakan untuk mempelajari pola dari fitur yang diekstraksi dan membuat prediksi.

- **Support Vector Classifier (SVC):**
    - **Alasan Penggunaan:** SVC adalah algoritma klasifikasi yang efektif, terutama pada dataset dengan dimensi fitur yang tinggi dan jumlah sampel yang moderat. SVC bekerja dengan menemukan hyperplane pemisah terbaik yang memiliki margin terbesar antara kelas-kelas. SVC dengan kernel non-linear (seperti 'rbf' yang digunakan di sini) mampu memisahkan data yang tidak dapat dipisahkan secara linear di ruang fitur asli dengan memproyeksikannya ke ruang berdimensi lebih tinggi. SVC sering kali memberikan kinerja yang baik pada masalah klasifikasi gambar berbasis fitur tradisional seperti HOG.
