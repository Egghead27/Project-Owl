# Project Owl🦉: Prediksi Produktivitas Berdasarkan Pola Tidur Menggunakan Machine Learning.
Capstone Project Dicoding Academy Bootcamp 2025

## Members
B25B8P019 - Nova Dwi Prasetyo Putra - Flutter 

B25B8M064 - Muhammad Yazid Hakim - Machine Learning

B25B8M065 - Jason Samuel - Machine Learning

B25B8M067 - Eros Prasetyo Putra Wijaya - Machine Learning

B25B8M128 - Raul Julito Safarrel Gamawanto - Flutter

## Description

### Apa itu Project Owl ?

Project Owl adalah aplikasi pelacak tidur (sleep tracker) berbasis Flutter. Aplikasi ini dirancang untuk membantu pengguna memahami hubungan antara kualitas tidur, durasi tidur, dan tingkat stres harian mereka.

Pengguna dapat mencatat jam tidur, jam bangun, dan tingkat stres harian mereka. Aplikasi kemudian akan memberikan skor Kualitas Tidur yang diprediksi oleh model Machine Learning, membantu pengguna mengidentifikasi pola untuk meningkatkan produktivitas dan kesehatan mereka.

### Latar Belakang

Di era yang serba cepat, banyak orang, terutama pelajar dan pekerja, mengorbankan waktu tidur yang berdampak langsung pada produktivitas dan kesehatan mental. Seringkali, kita tidak menyadari seberapa besar pengaruh tidur satu malam terhadap kinerja kita keesokan harinya. Project Owl dibuat untuk menjembatani kesenjangan informasi tersebut, memberikan data nyata kepada pengguna tentang bagaimana tidur mereka memengaruhi kehidupan sehari-hari mereka.

### Penjelasan Fitur & Alur

Aplikasi ini memiliki beberapa fitur utama yang saling terhubung:

Personalisasi Nama: Saat pertama kali membuka aplikasi, pengguna akan diminta memasukkan nama panggilan untuk sapaan yang personal ("Selamat Pagi, [Nama]").

Onboarding: Pengguna baru akan disambut dengan serangkaian layar orientasi yang menjelaskan fungsi utama aplikasi.

Input Data Harian:

Jam Tidur & Bangun: Pengguna memilih waktu mereka mulai tidur dan waktu bangun.

Tingkat Stres: Pengguna memberikan input tingkat stres (skala 1-5) untuk hari itu.

Prediksi Kualitas Tidur: Data (durasi tidur & tingkat stres) dikirim ke API back-end, di mana model Machine Learning memprosesnya dan mengembalikan prediksi skor kualitas tidur (0-100%).

Visualisasi Data: Hasil prediksi ditampilkan dalam bentuk lingkaran progres yang menarik secara visual, beserta detail durasi tidur.

Cara Menjalankan/Duplikasi Proyek

Jika Anda ingin menjalankan atau menduplikasi proyek ini di mesin lokal Anda, ikuti langkah-langkah berikut:

### Prasyarat:

Pastikan Anda telah menginstal Flutter SDK (versi 3.x.x atau lebih baru).

Memiliki editor kode seperti Visual Studio Code.

Perangkat (Emulator Android, iOS Simulator, atau perangkat fisik) untuk menjalankan aplikasi.

### Langkah-langkah Instalasi:

1. Clone repositori ini ke komputer lokal Anda:

    git clone [https://github.com/trunxl0g1c/capstone-flutter.git](https://github.com/trunxl0g1c/capstone-flutter.git)


2. Masuk ke direktori proyek:

    cd capstone-flutter


3. Instal semua dependensi yang diperlukan:

    flutter pub get


4. Jalankan aplikasi:

    flutter run


(Catatan: Aplikasi ini memerlukan back-end API Machine Learning untuk menjalankan fitur prediksi. Untuk fungsionalitas penuh, pastikan Anda juga menjalankan server API secara lokal dan memperbarui URL endpoint di lib/data/api/api_service.dart.)

## Proses Pembuatan Project

### ⚙️ Modelling

Tahapan modelling dilakukan untuk membangun model prediksi produktivitas berdasarkan pola tidur menggunakan algoritma Random Forest Regressor.

#### 1️⃣ Ambil Fitur dan Target

Langkah pertama adalah menentukan fitur (variabel independen) yang digunakan untuk memprediksi target (variabel dependen).
Sleep Duration : lama tidur seseorang (jam)

Quality of Sleep : skor kualitas tidur (1–10)

Stress Level : skor tingkat stres (1–10, semakin tinggi semakin stres)

Productivity Score : skor produktivitas menjadi target prediksi

#### 2️⃣ Split Data Menjadi Data Latih dan Uji

Data dibagi menjadi dua bagian agar model dapat diuji pada data yang belum pernah dilihat sebelumnya.
Pembagian umum yang digunakan adalah 80% untuk pelatihan dan 20% untuk pengujian.

#### 3️⃣ Standarisasi Data (Scaling)

Proses standarisasi dilakukan hanya berdasarkan data latih, untuk menghindari data leakage ke data uji.
Fitur seperti “Sleep Duration” (jam) dan “Quality of Sleep” (skor) memiliki skala berbeda.
Dengan StandardScaler, semua fitur akan berada pada skala yang sebanding, sehingga model tidak berat sebelah terhadap fitur dengan nilai lebih besar.

#### 4️⃣ Latih Model Random Forest

Model dilatih menggunakan algoritma RandomForestRegressor.
Model ini dipilih karena mampu menangkap hubungan non-linear antar variabel serta tahan terhadap outlier.

n_estimators=300 -> jumlah pohon yang digunakan dalam ensemble

max_depth=4 -> kedalaman maksimal tiap pohon (mengontrol kompleksitas model)

random_state=1 -> memastikan hasil replikasi yang konsisten

#### 5️⃣ Evaluasi Model

Setelah model dilatih, dilakukan evaluasi pada data uji untuk mengetahui seberapa baik model mampu memprediksi skor produktivitas.

R² (Koefisien Determinasi) menunjukkan proporsi variasi target yang bisa dijelaskan oleh model (semakin mendekati 1 semakin baik).

MSE (Mean Squared Error) menunjukkan rata-rata kuadrat selisih antara nilai aktual dan prediksi (semakin kecil semakin baik).

#### 6️⃣ Simpan dan Load Model dan Scaler

Model dan scaler yang sudah dilatih disimpan menggunakan pickle (pkl) agar bisa digunakan kembali saat proses deployment.


### 🚀 Deployment

Tahapan deployment bertujuan untuk menyajikan model machine learning yang telah dilatih agar bisa digunakan oleh aplikasi mobile menggunakan flutter. Pada tahap ini, model Random Forest Regressor yang sudah disimpan (rf_model.pkl) dan scaler.pkl akan dijalankan melalui FastAPI.

#### a. Inisialisasi Aplikasi
FastAPI membuat API interaktif yang otomatis menyediakan dokumentasi di /docs.

#### b. Load Artefak
Pada saat API dijalankan, model, scaler, dan urutan fitur akan otomatis dimuat dari folder models/.

#### c. Schema Input dan Output
Menggunakan Pydantic untuk validasi data otomatis.
Input harus memiliki tiga nilai numerik sesuai batasan fitur yakni sleep_duration, sleep_quality, dan stress_level
Output berupa nilai prediksi produktivitas.

#### d. Fungsi Prediksi
Langkah dalam fungsi:

-Input JSON → DataFrame sesuai urutan fitur.

-Transformasi menggunakan scaler.pkl.

-Prediksi produktivitas menggunakan model Random Forest.

-Mengembalikan hasil prediksi dalam format JSON.

#### e. Hosting API di Railway
Setelah aplikasi berhasil dideploy di Railway, API dapat langsung digunakan melalui endpoint berikut:

https://model-to-api-production.up.railway.app/predict


# Credits & Teknologi

Berikut adalah teknologi utama yang digunakan untuk membangun aplikasi Flutter ini:

Framework: <a href="https://flutter.dev/">Flutter</a>

Bahasa: <a href="https://dart.dev/">Dart</a>

State Management: <a href="https://pub.dev/packages/provider">Provider</a>

UI Components: <a href="https://pub.dev/packages/shadcn_flutter">shadcn_flutter</a>

Fonts: <a href="https://pub.dev/packages/google_fonts">google_fonts</a>

Local Storage: <a href="https://pub.dev/packages/shared_preferences">shared_preferences</a>

Networking: <a href="https://pub.dev/packages/http">http</a>

Onboarding: <a href="https://pub.dev/packages/introduction_screen">introduction_screen</a>

