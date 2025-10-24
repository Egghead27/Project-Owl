# Project Owl🦉: Prediksi Produktivitas Berdasarkan Pola Tidur Menggunakan Machine Learning.
Capstone Project Dicoding Academy Bootcamp 2025

## Members
B25B8P019 - Nova Dwi Prasetyo Putra - Flutter 

B25B8M064 - Muhammad Yazid Hakim - Machine Learning

B25B8M065 - Jason Samuel - Machine Learning

B25B8M067 - Eros Prasetyo Putra Wijaya - Machine Learning

B25B8M128 - Raul Julito Safarrel Gamawanto - Flutter

## Description

⚙️ Modelling

Tahapan modelling dilakukan untuk membangun model prediksi produktivitas berdasarkan pola tidur menggunakan algoritma Random Forest Regressor.

1️⃣ Ambil Fitur dan Target

Langkah pertama adalah menentukan fitur (variabel independen) yang digunakan untuk memprediksi target (variabel dependen).
Sleep Duration : lama tidur seseorang (jam)

Quality of Sleep : skor kualitas tidur (1–10)

Stress Level : skor tingkat stres (1–10, semakin tinggi semakin stres)

Productivity Score : skor produktivitas menjadi target prediksi

2️⃣ Split Data Menjadi Data Latih dan Uji

Data dibagi menjadi dua bagian agar model dapat diuji pada data yang belum pernah dilihat sebelumnya.
Pembagian umum yang digunakan adalah 80% untuk pelatihan dan 20% untuk pengujian.

3️⃣ Standarisasi Data (Scaling)

Proses standarisasi dilakukan hanya berdasarkan data latih, untuk menghindari data leakage ke data uji.
Fitur seperti “Sleep Duration” (jam) dan “Quality of Sleep” (skor) memiliki skala berbeda.
Dengan StandardScaler, semua fitur akan berada pada skala yang sebanding, sehingga model tidak berat sebelah terhadap fitur dengan nilai lebih besar.

4️⃣ Latih Model Random Forest

Model dilatih menggunakan algoritma RandomForestRegressor.
Model ini dipilih karena mampu menangkap hubungan non-linear antar variabel serta tahan terhadap outlier.

n_estimators=300 -> jumlah pohon yang digunakan dalam ensemble

max_depth=4 -> kedalaman maksimal tiap pohon (mengontrol kompleksitas model)

random_state=1 -> memastikan hasil replikasi yang konsisten

5️⃣ Evaluasi Model

Setelah model dilatih, dilakukan evaluasi pada data uji untuk mengetahui seberapa baik model mampu memprediksi skor produktivitas.

R² (Koefisien Determinasi) menunjukkan proporsi variasi target yang bisa dijelaskan oleh model (semakin mendekati 1 semakin baik).

MSE (Mean Squared Error) menunjukkan rata-rata kuadrat selisih antara nilai aktual dan prediksi (semakin kecil semakin baik).

6️⃣ Simpan dan Load Model dan Scaler

Model dan scaler yang sudah dilatih disimpan menggunakan pickle (pkl) agar bisa digunakan kembali saat proses deployment.


🚀 Deployment

Tahapan deployment bertujuan untuk menyajikan model machine learning yang telah dilatih agar bisa digunakan oleh aplikasi mobile menggunakan flutter. Pada tahap ini, model Random Forest Regressor yang sudah disimpan (rf_model.pkl) dan scaler.pkl akan dijalankan melalui FastAPI.

a. Inisialisasi Aplikasi
FastAPI membuat API interaktif yang otomatis menyediakan dokumentasi di /docs.

b. Load Artefak
Pada saat API dijalankan, model, scaler, dan urutan fitur akan otomatis dimuat dari folder models/.

c. Schema Input dan Output
Menggunakan Pydantic untuk validasi data otomatis.
Input harus memiliki tiga nilai numerik sesuai batasan fitur yakni sleep_duration, sleep_quality, dan stress_level
Output berupa nilai prediksi produktivitas.

d. Fungsi Prediksi
Langkah dalam fungsi:

-Input JSON → DataFrame sesuai urutan fitur.

-Transformasi menggunakan scaler.pkl.

-Prediksi produktivitas menggunakan model Random Forest.

-Mengembalikan hasil prediksi dalam format JSON.

e. Hosting API di Railway
Setelah aplikasi berhasil dideploy di Railway, API dapat langsung digunakan melalui endpoint berikut:

https://model-to-api-production.up.railway.app/predict


