# Analisis Sentimen Opini Publik terhadap Pemindahan Ibu Kota (IKN) di Twitter

![IKN Sentiment Banner](https://raw.githubusercontent.com/user-attachments/assets/c50259b3-1f16-43f2-8c90-f5a092fd64c7)

## Ringkasan Proyek (TL;DR)
Proyek ini menganalisis sentimen publik terhadap isu pemindahan Ibu Kota Negara (IKN) Indonesia berdasarkan data dari media sosial Twitter. Proses dimulai dengan pengambilan data mentah (*scraping*), diikuti oleh pelabelan sentimen otomatis menggunakan metode berbasis leksikon, dan diakhiri dengan melatih beberapa model *machine learning* untuk klasifikasi sentimen. Model **Support Vector Classifier (SVC)** berhasil menjadi yang terbaik dengan **akurasi 89%**.

**Tags:** `Python`, `Scikit-Learn`, `Pandas`, `NLTK`, `Sastrawi`, `Sentiment Analysis`, `Web Scraping`, `NLP`

---

## Daftar Isi
1. [Latar Belakang](#latar-belakang)
2. [Tujuan Proyek](#tujuan-proyek)
3. [Dataset](#dataset)
4. [Alur Kerja Proyek (Workflow)](#alur-kerja-proyek)
5. [Hasil & Evaluasi](#hasil--evaluasi)
6. [Visualisasi](#visualisasi)
7. [Kesimpulan](#kesimpulan)

---

### Latar Belakang
Pemindahan Ibu Kota Negara (IKN) ke Kalimantan Timur adalah salah satu isu kebijakan publik terbesar di Indonesia saat ini. Isu ini menimbulkan beragam opini di masyarakat. Media sosial seperti Twitter menjadi platform utama bagi warga untuk menyuarakan pendapat mereka. Menganalisis sentimen dari percakapan ini secara otomatis dapat memberikan wawasan berharga bagi pembuat kebijakan, peneliti, dan publik untuk memahami peta opini secara keseluruhan.

### Tujuan Proyek
* Mengumpulkan data opini publik mengenai IKN langsung dari Twitter melalui teknik *web scraping*.
* Melakukan pembersihan dan pra-pemrosesan data teks untuk analisis.
* Memberikan label sentimen (positif, negatif, netral) pada setiap tweet menggunakan metode berbasis leksikon (InSet Lexicon).
* Membangun dan membandingkan beberapa model klasifikasi *machine learning* untuk memprediksi sentimen.
* Mengidentifikasi model dengan performa akurasi terbaik.

### Dataset
* **Pengambilan Data:** Data dikumpulkan menggunakan skrip Python (`kode_scraping.py`) yang melakukan *scraping* pada situs Twitter dengan kata kunci terkait "IKN".
* **Pra-Pelabelan:** Sebanyak 1000 tweet mentah berhasil dikumpulkan. Setelah melalui proses pelabelan otomatis berbasis leksikon, dataset yang digunakan untuk pemodelan terdiri dari **912 data** yang memiliki sentimen positif atau negatif.
* **Distribusi Sentimen:**
    * Sentimen Positif: 671 data
    * Sentimen Negatif: 241 data

### Alur Kerja Proyek (Workflow)
Proyek ini mencakup siklus lengkap dari pengolahan data teks hingga pemodelan:
1.  **Data Cleaning & Preprocessing:** Setiap tweet melalui serangkaian proses pembersihan yang ketat untuk menstandardisasi teks:
    * *Case Folding*: Mengubah semua teks menjadi huruf kecil.
    * *Noise Removal*: Menghapus URL, angka, tanda baca, dan karakter non-alfabet.
    * *Tokenization*: Memecah kalimat menjadi kata-kata (token).
    * *Stopword Removal*: Menghapus kata-kata umum dalam Bahasa Indonesia yang tidak memiliki makna sentimen (misalnya: "yang", "di", "dan") menggunakan daftar *stopwords* dari NLTK dan tambahan manual.
    * *Stemming*: Mengubah setiap kata ke bentuk dasarnya menggunakan library **Sastrawi** (misalnya: "memindahkan" -> "pindah").

2.  **Pelabelan Sentimen (Lexicon-Based):**
    * Sentimen setiap tweet ditentukan dengan menghitung skor berdasarkan **InSet Lexicon**, sebuah kamus sentimen untuk Bahasa Indonesia.
    * Kata-kata positif diberi skor +1 dan kata-kata negatif diberi skor -1. Skor total dari sebuah tweet menentukan label akhirnya (positif jika > 0, negatif jika < 0).

3.  **Feature Engineering (TF-IDF):**
    * Teks yang sudah bersih diubah menjadi representasi numerik menggunakan `TfidfVectorizer`. TF-IDF memberikan bobot pada kata-kata yang penting dalam sebuah dokumen relatif terhadap keseluruhan korpus data.

4.  **Pemodelan (Modeling):**
    * Dataset dibagi menjadi **data latih (80%)** dan **data uji (20%)**.
    * Empat model klasifikasi yang berbeda dilatih dan dievaluasi untuk menemukan yang terbaik:
        * Regresi Logistik
        * Naive Bayes
        * Random Forest Classifier
        * **Support Vector Classifier (SVC)**

### Hasil & Evaluasi
Perbandingan akurasi dari keempat model pada data uji menunjukkan bahwa **Support Vector Classifier (SVC)** memberikan performa tertinggi.

* **Perbandingan Akurasi Model:**
    * Regresi Logistik: 88%
    * Naive Bayes: 87%
    * Random Forest: 86%
    * **Support Vector Classifier (SVC): 89%**

* **Laporan Klasifikasi (SVC):**
    ```
                  precision    recall  f1-score   support
        Negatif       0.93      0.65      0.77        46
        Positif       0.88      0.98      0.93       137
    ----------------------------------------------------
      micro avg       0.89      0.89      0.89       183
      macro avg       0.91      0.82      0.85       183
    weighted avg      0.89      0.89      0.89       183
    ```
    Laporan ini menunjukkan bahwa model SVC sangat baik dalam mengenali sentimen positif (recall 0.98) dan memiliki presisi yang tinggi untuk sentimen negatif (precision 0.93).

### Visualisasi
Untuk mendapatkan pemahaman kualitatif dari data, sebuah *Word Cloud* (Awan Kata) dibuat untuk memvisualisasikan kata-kata yang paling sering muncul dalam data sentimen positif dan negatif.

![Word Cloud Sentimen](https://raw.githubusercontent.com/user-attachments/assets/61f9d273-0979-459e-9b7e-01a6b0c2a71f)
*Visualisasi ini membantu mengidentifikasi tema utama dalam setiap kategori sentimen.*

### Kesimpulan
Proyek ini berhasil membangun sebuah alur kerja analisis sentimen *end-to-end* untuk opini publik mengenai IKN. Dengan akurasi mencapai 89% menggunakan model SVC, sistem ini terbukti efektif dalam mengklasifikasikan sentimen secara otomatis. Hasil analisis ini dapat menjadi dasar untuk studi lebih lanjut atau sebagai input untuk dasbor pemantauan opini publik secara *real-time*.
