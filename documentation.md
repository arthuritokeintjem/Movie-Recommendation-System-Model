# Laporan Proyek Machine Learning - Sistem Rekomendasi Film

Nama: Arthurito Keintjem
Email: keintjemarthurito@gmail.com
ID Dicoding: arthuritokeintjem

## Project Overview

Sistem rekomendasi telah menjadi komponen integral dalam berbagai platform digital, mulai dari e-commerce hingga layanan streaming hiburan. Dalam konteks industri film, di mana jumlah konten terus bertambah secara eksponensial, sistem rekomendasi memainkan peran vital. Mereka tidak hanya membantu pengguna menemukan film yang sesuai dengan selera dan preferensi unik mereka di tengah lautan pilihan, tetapi juga meningkatkan keterlibatan pengguna (user engagement) dan loyalitas terhadap platform. Dengan menyajikan saran yang relevan dan dipersonalisasi, platform dapat memperkaya pengalaman menonton pengguna dan mendorong eksplorasi konten yang lebih luas.

**Pentingnya Proyek Ini Diselesaikan:**
Proyek pengembangan sistem rekomendasi film ini penting karena beberapa alasan:
1.  **Mengatasi Information Overload**: Dengan ribuan judul film yang tersedia, pengguna seringkali kesulitan memilih. Sistem rekomendasi membantu menyaring dan menyajikan pilihan yang paling relevan, menghemat waktu dan tenaga pengguna.
2.  **Meningkatkan Pengalaman Pengguna**: Rekomendasi yang akurat dan personal dapat secara signifikan meningkatkan kepuasan pengguna terhadap sebuah platform layanan film.
3.  **Mendorong Eksplorasi Konten**: Sistem rekomendasi dapat memperkenalkan pengguna pada film-film baru atau genre yang mungkin belum pernah mereka pertimbangkan sebelumnya, namun sesuai dengan profil preferensi mereka (serendipity).
4.  **Nilai Bisnis bagi Platform**: Bagi penyedia layanan, sistem rekomendasi yang efektif dapat meningkatkan metrik penting seperti durasi tonton, jumlah film yang ditonton per pengguna, dan retensi pengguna, yang pada akhirnya dapat meningkatkan pendapatan.

Proyek ini bertujuan untuk merancang dan mengimplementasikan model sistem rekomendasi film menggunakan dataset `filmtv_movies.csv`. Dataset ini berisi beragam informasi mengenai film, seperti genre, sutradara, aktor, deskripsi, dan berbagai skor atribut. Proyek ini akan mengeksplorasi dua pendekatan utama: *Content-Based Filtering* dan sebuah pendekatan yang terinspirasi dari *Collaborative Filtering* yang disesuaikan dengan ketersediaan data.

**Referensi Terkait (Contoh/Placeholder - Silakan ganti dengan referensi aktual yang lebih spesifik):**
-   Ricci, F., Rokach, L., & Shapira, B. (2011). Introduction to Recommender Systems Handbook. In *Recommender Systems Handbook* (pp. 1-35). Springer US. (Dasar-dasar sistem rekomendasi).
-   Aggarwal, C. C. (2016). *Recommender Systems: The Textbook*. Springer. (Pembahasan mendalam berbagai teknik sistem rekomendasi).

## Business Understanding

### Problem Statements
-   **Pernyataan Masalah 1**: Pengguna platform film seringkali merasa kewalahan dengan banyaknya pilihan film yang tersedia, sehingga kesulitan menemukan film yang benar-benar sesuai dengan preferensi dan minat pribadi mereka secara efisien.
-   **Pernyataan Masalah 2**: Platform penyedia layanan film ingin meningkatkan keterlibatan (engagement) dan retensi pengguna dengan cara menyediakan saran tontonan yang lebih personal dan relevan, sehingga pengguna merasa platform memahami selera mereka.

### Goals
-   **Tujuan 1**: Mengembangkan model sistem rekomendasi yang mampu menghasilkan daftar film yang dipersonalisasi dan relevan berdasarkan film input atau preferensi implisit dari atribut film.
-   **Tujuan 2**: Menyediakan dua pendekatan sistem rekomendasi yang berbeda (Content-Based Filtering dan pendekatan berbasis Collaborative Filtering) untuk memberikan variasi dalam cara film direkomendasikan.

### Solution Approach
Untuk mencapai tujuan-tujuan di atas, dua pendekatan solusi (algoritma atau pendekatan sistem rekomendasi) akan diajukan dan diimplementasikan:

1.  **Content-Based Filtering**:
    Pendekatan ini akan merekomendasikan film berdasarkan kemiripan konten antara film-film tersebut. Fitur-fitur konten yang akan dipertimbangkan meliputi atribut tekstual seperti genre, deskripsi plot, nama sutradara, dan daftar aktor. Jika seorang pengguna menyukai suatu film (atau sedang melihat detail suatu film), sistem akan mencari film lain dengan atribut konten yang serupa. Teknik seperti TF-IDF (Term Frequency-Inverse Document Frequency) akan digunakan untuk mengubah data teks menjadi representasi vektor, dan kemiripan antar film akan dihitung menggunakan metrik seperti Cosine Similarity.

2.  **Collaborative Filtering (dengan implementasi Item-Based berbasis Atribut Film)**:
    Pendekatan Collaborative Filtering idealnya merekomendasikan item berdasarkan preferensi dari pengguna-pengguna yang memiliki selera serupa (User-Based CF) atau item-item yang sering disukai bersama oleh banyak pengguna (Item-Based CF). Namun, dataset `filmtv_movies.csv` yang digunakan tidak memiliki data interaksi pengguna-item secara eksplisit (misalnya, matriks rating user-film).
    Oleh karena itu, sebagai solusi untuk memenuhi kriteria pendekatan Collaborative Filtering, akan diimplementasikan sebuah **pendekatan Item-Based yang menggunakan kemiripan atribut antar film sebagai proxy**. Asumsinya adalah film-film dengan atribut yang serupa (seperti genre, skor dimensi film, popularitas vote) cenderung disukai oleh kelompok pengguna yang sama atau memiliki karakteristik yang "disukai bersama". Dengan demikian, jika sebuah film A mirip dengan film B berdasarkan atribut-atribut ini, maka pengguna yang menyukai A kemungkinan juga akan menyukai B. Ini adalah bentuk penalaran yang terinspirasi dari Item-Based Collaborative Filtering, di mana "kolaborasi" terjadi secara implisit melalui kesamaan karakteristik item.

## Data Understanding

Dataset yang digunakan dalam proyek ini adalah `filmtv_movies.csv`. Dataset ini berisi informasi detail mengenai berbagai film.
Sumber data (berdasarkan informasi dari notebook): [FilmTV movies Dataset on Kaggle](https://www.kaggle.com/datasets/stefanoleone992/filmtv-movies-dataset).

**Informasi Umum Dataset (berdasarkan output notebook):**
-   Jumlah data (film) awal: 41.399
-   Jumlah fitur awal: 19 fitur
-   Kondisi data: Terdapat beberapa missing values pada kolom-kolom tertentu yang perlu ditangani.

**Deskripsi Variabel/Fitur Utama (sesuai notebook):**
-   `filmtv_id`: ID unik untuk setiap entri film (Integer).
-   `title`: Judul film (Object/String).
-   `year`: Tahun rilis film (Integer).
-   `genre`: Genre film (dapat berupa multiple genre, dipisahkan koma) (Object/String).
-   `duration`: Durasi film dalam satuan menit (Integer).
-   `country`: Negara asal produksi film (Object/String).
-   `directors`: Nama sutradara film (dapat lebih dari satu) (Object/String).
-   `actors`: Nama aktor utama film (dapat lebih dari satu) (Object/String).
-   `avg_vote`: Rata-rata skor voting yang diterima film (Float).
-   `critics_vote`: Skor voting dari kritikus (Float).
-   `public_vote`: Skor voting dari publik (Float).
-   `total_votes`: Jumlah total voting yang diterima film (Integer).
-   `description`: Sinopsis atau deskripsi singkat mengenai alur cerita film (Object/String).
-   `notes`: Catatan atau informasi tambahan terkait film (Object/String).
-   `humor`, `rhythm`, `effort`, `tension`, `erotism`: Skor numerik yang menilai film berdasarkan dimensi-dimensi tersebut (Integer).

**Exploratory Data Analysis (EDA) (Kriteria Tambahan):**
Beberapa tahapan EDA dilakukan untuk memahami data lebih dalam, seperti yang terlihat pada notebook:

1.  **Pengecekan Missing Values Awal**:
    Dilakukan dengan `df.isnull().sum()`.
    *Insight*: Ditemukan missing values pada kolom `genre`, `country`, `directors`, `actors`, `description`, `notes`, serta `critics_vote` dan `public_vote`. Ini mengindikasikan perlunya strategi imputasi atau penghapusan pada tahap Data Preparation.

2.  **Distribusi Fitur Numerik**:
    Dilakukan visualisasi histogram untuk kolom-kolom numerik seperti `year`, `duration`, `avg_vote`, `total_votes`, dan skor dimensi setelah potensi konversi tipe data.
    ```python
    # Contoh snippet dari notebook untuk visualisasi distribusi
    # df[numerical_cols].hist(bins=30, figsize=(20, 15), layout=(-1, 3))
    # plt.suptitle("Distribusi Fitur Numerik", fontsize=20)
    # plt.show()
    ```
    *Insight*: Visualisasi ini membantu memahami sebaran nilai (misalnya, apakah miring atau simetris), rentang data, dan potensi adanya outlier pada fitur-fitur numerik. Misalnya, distribusi `avg_vote` bisa menunjukkan konsentrasi film pada rentang rating tertentu, atau distribusi `year` bisa menunjukkan tren produksi film dari waktu ke waktu.

3.  **Analisis Genre Film**:
    Dilakukan pemisahan genre, penghitungan frekuensi, dan visualisasi (misalnya, bar chart) untuk genre-genre yang paling umum.
    ```python
    # Contoh snippet dari notebook untuk analisis genre
    # genre_counts = pd.Series(genre_flat_list).value_counts()
    # sns.barplot(x=genre_counts.head(20).values, y=genre_counts.head(20).index, palette='viridis')
    # plt.title('Top 20 Genre Film Paling Umum')
    # plt.show()
    ```
    *Insight*: Mengidentifikasi genre dominan dalam dataset (misalnya Drama, Comedy, Thriller) memberikan pemahaman tentang komposisi dataset dan area di mana rekomendasi mungkin lebih banyak muncul atau lebih dibutuhkan.

## Data Preparation

Proses persiapan data sangat penting untuk memastikan kualitas dan kesesuaian data untuk tahap pemodelan. Teknik data preparation yang dilakukan pada notebook secara berurutan adalah sebagai berikut:

1.  **Penanganan Missing Values**:
    -   **Proses**:
        -   **Kolom Tekstual**: Missing values (NaN) pada kolom-kolom tekstual seperti `genre`, `country`, `directors`, `actors`, `description`, dan `notes` diisi dengan string kosong (`''`).
        -   **Kolom Numerik (Vote dan Skor Dimensi)**: Kolom `critics_vote` dan `public_vote` pertama-tama dikonversi ke tipe data numerik (dengan `errors='coerce'` yang mengubah entri non-numerik menjadi NaN). Setelah itu, missing values pada kolom-kolom ini, diisi menggunakan nilai median dari masing-masing kolom.
    -   **Alasan**:
        -   Mengisi kolom teks dengan string kosong mencegah error saat operasi string (seperti penggabungan untuk fitur `soup`) dan memastikan semua baris memiliki input untuk vectorizer.
        -   Menggunakan median untuk mengisi NaN pada fitur numerik dipilih karena median lebih robust (tahan) terhadap nilai outlier dibandingkan mean, sehingga membantu mempertahankan distribusi data asli sebaik mungkin.

2.  **Feature Engineering (untuk Model 1 - Content-Based Filtering Tekstual)**:
    -   **Proses**: Sebuah fitur baru bernama `soup` dibuat dengan menggabungkan konten dari beberapa kolom teks: `genre`, `directors`, `actors`, `description`, dan `notes`. Untuk `directors` dan `actors`, spasi dalam nama dihilangkan (misalnya, "Quentin Tarantino" menjadi "QuentinTarantino") agar nama tersebut diperlakukan sebagai satu token oleh vectorizer.
        ```python
        # Contoh snippet dari notebook untuk membuat fitur 'soup'
        # def create_soup(x):
        #     directors_str = ''.join(str(x['directors']).split()) if pd.notnull(x['directors']) else ''
        #     actors_str = ''.join(str(x['actors']).split()) if pd.notnull(x['actors']) else ''
        #     # ... (gabungkan dengan genre_str, description_str, notes_str)
        #     return f"{genre_str} {directors_str} {actors_str} {description_str} {notes_str}"
        # df['soup'] = df.apply(create_soup, axis=1)
        ```
    -   **Alasan**: Menggabungkan fitur-fitur tekstual ini ke dalam satu 'dokumen' per film memungkinkan model Content-Based Filtering untuk menangkap kemiripan berdasarkan kombinasi informasi yang lebih kaya dan holistik. Menghilangkan spasi pada nama sutradara dan aktor memastikan entitas multi-kata ini diperlakukan sebagai unit tunggal oleh vectorizer, sehingga meningkatkan akurasi pencocokan.

3.  **Seleksi dan Transformasi Fitur (untuk Model 2 - Item-Based CF berbasis Atribut)**:
    -   **Proses**:
        -   **Fitur Numerik**: Fitur-fitur numerik yang relevan (`avg_vote`, `total_votes`, `humor`, `rhythm`, `effort`, `tension`, `erotism`) dipilih.
        -   **Normalisasi Fitur Numerik**: Fitur-fitur numerik ini dinormalisasi menggunakan `MinMaxScaler` untuk mengubah semua nilai ke dalam rentang [0, 1].
        -   **Fitur Genre**: Fitur kategorikal `genre` diubah menjadi representasi numerik menggunakan `TfidfVectorizer` (dengan tokenizer khusus yang memisahkan genre berdasarkan koma dan membersihkan spasi).
        -   **Penggabungan Fitur**: Fitur numerik yang sudah dinormalisasi dan matriks genre TF-IDF kemudian digabungkan (`pd.concat`) untuk membentuk matriks fitur akhir yang akan digunakan oleh Model 2.
    -   **Alasan**:
        -   Normalisasi fitur numerik penting karena algoritma berbasis jarak atau kemiripan (seperti Cosine Similarity) sensitif terhadap skala fitur. Tanpa normalisasi, fitur dengan rentang nilai yang lebih besar dapat mendominasi perhitungan kemiripan secara tidak proporsional.
        -   Mengubah fitur `genre` menjadi representasi numerik (matriks TF-IDF) memungkinkan genre untuk dimasukkan ke dalam model kemiripan bersama dengan fitur numerik lainnya. TF-IDF dipilih untuk memberi bobot pada genre berdasarkan frekuensi dan keunikannya.
        -   Penggabungan fitur bertujuan untuk menciptakan representasi item (film) yang lebih komprehensif untuk Model 2.

## Modeling dan Result

Dua model sistem rekomendasi dikembangkan untuk menyelesaikan permasalahan. Output dari kedua model adalah top-N rekomendasi film.

### 1. Model Content-Based Filtering (berdasarkan Fitur Tekstual 'soup')
Model ini merekomendasikan film berdasarkan kemiripan konten tekstual yang telah digabungkan dalam fitur `soup`.

-   **Algoritma**:
    1.  Fitur `soup` dari setiap film digunakan sebagai input.
    2.  `TfidfVectorizer` (dengan `stop_words='english'`) diterapkan pada fitur `soup` untuk mengubah teks menjadi matriks representasi numerik TF-IDF. Ini memberikan bobot pada kata-kata/token berdasarkan frekuensinya dalam deskripsi film dan seberapa unik token tersebut di seluruh dataset.
    3.  `cosine_similarity` dihitung antara vektor TF-IDF dari semua pasangan film. Hasilnya adalah matriks kemiripan kosinus (ukuran N x N, di mana N adalah jumlah film) yang menunjukkan seberapa mirip setiap film dengan film lainnya berdasarkan konten teksnya.
    4.  Sebuah pemetaan ( `pd.Series` bernama `indices_text`) dibuat dari judul film ke indeks barisnya dalam DataFrame untuk memudahkan pencarian.
    5.  Untuk mendapatkan rekomendasi bagi film tertentu, fungsi `get_recommendations_text_generic` mencari film input dalam pemetaan indeks, mengambil baris skor kemiripan yang sesuai dari matriks kemiripan kosinus, mengurutkan skor tersebut secara menurun, dan mengambil N film teratas (tidak termasuk film input itu sendiri).

-   **Output Top-N Recommendation (Contoh Format dari Notebook)**:
    ```
    Rekomendasi untuk film '[Judul Film Input dari df.iloc[0]]' (berdasarkan teks):
    ---
    1. [Judul Rekomendasi 1]
    2. [Judul Rekomendasi 2]
    3. [Judul Rekomendasi 3]
    ... (hingga 10 film)
    ```

-   **Kelebihan**:
    -   Tidak memerlukan data historis pengguna lain (dapat mengatasi masalah *user cold-start* jika pengguna baru dapat menentukan preferensi awal melalui film yang disukai).
    -   Mampu merekomendasikan item yang spesifik dan kurang populer (*niche items*) selama metadatanya kaya.
    -   Rekomendasi bersifat transparan dan dapat dijelaskan berdasarkan fitur konten (misalnya, "direkomendasikan karena memiliki genre, aktor, atau deskripsi yang sama/mirip").
    -   Tidak ada masalah *item cold-start* yang signifikan jika item baru memiliki deskripsi fitur konten yang cukup.
-   **Kekurangan**:
    -   Kualitas fitur konten sangat menentukan; ekstraksi dan rekayasa fitur (`soup`) yang baik sangat krusial.
    -   Cenderung merekomendasikan item yang sangat mirip (kurang memberikan *serendipity* atau penemuan tak terduga yang menyenangkan).
    -   Sangat bergantung pada kualitas dan kelengkapan metadata film. Jika metadata buruk atau tidak lengkap, rekomendasi akan kurang optimal.

### 2. Model Collaborative Filtering (Pendekatan Item-Based berbasis Atribut Film)
Model ini bertujuan merekomendasikan film berdasarkan kemiripan antar item (film) itu sendiri, dengan menggunakan atribut-atribut film sebagai dasar perhitungan kemiripan. Ini adalah bentuk Item-Based Collaborative Filtering yang diadaptasi, di mana "kolaborasi" implisit terjadi karena film dengan atribut serupa cenderung disukai oleh kelompok pengguna yang sama.

-   **Algoritma**:
    1.  **Seleksi Fitur**: Fitur numerik (`avg_vote`, `total_votes`, skor dimensi seperti `humor`, `tension`, dll.) dan fitur kategorikal (`genre`) dipilih.
    2.  **Transformasi Fitur Numerik**: Fitur numerik dinormalisasi menggunakan `MinMaxScaler` ke rentang [0, 1].
    3.  **Transformasi Fitur Genre**: Fitur `genre` diubah menjadi representasi numerik (matriks TF-IDF) menggunakan `TfidfVectorizer` dengan tokenizer khusus.
    4.  **Penggabungan Fitur**: Matriks fitur numerik yang telah dinormalisasi dan matriks fitur genre TF-IDF digabungkan menjadi satu matriks fitur komprehensif untuk setiap film.
    5.  **Perhitungan Kemiripan**: `cosine_similarity` dihitung pada matriks fitur gabungan ini untuk mendapatkan matriks kemiripan antar film berdasarkan atribut-atribut tersebut.
    6.  **Pemetaan Indeks**: Sebuah pemetaan (`indices_attr`) dari judul film ke indeks dibuat.
    7.  **Pemberian Rekomendasi**: Fungsi `get_recommendations_attr` (yang menggunakan logika `get_recommendations_text_generic`) digunakan untuk memberikan top-N rekomendasi berdasarkan matriks kemiripan atribut ini.

-   **Output Top-N Recommendation (Contoh Format dari Notebook)**:
    ```
    Rekomendasi untuk film '[Judul Film Input dari df.iloc[1]]' (berdasarkan atribut):
    ---
    1. [Judul Rekomendasi 1]
    2. [Judul Rekomendasi 2]
    3. [Judul Rekomendasi 3]
    ... (hingga 10 film)
    ```

-   **Kelebihan**:
    -   Tidak memerlukan data rating eksplisit pengguna-item yang detail, cukup atribut item yang kaya.
    -   Mampu menemukan item-item yang secara atribut mirip dan berpotensi disukai oleh pengguna dengan selera serupa (efek "pengguna yang menyukai X juga menyukai Y" secara implisit).
    -   Dapat membantu dalam mengatasi masalah *item cold-start* sampai batas tertentu jika item baru memiliki atribut yang lengkap dan dapat dibandingkan.
    -   Perhitungan kemiripan item (matriks `cosine_sim_attr`) bisa dilakukan secara offline, sehingga proses rekomendasi saat runtime bisa lebih cepat.
-   **Kekurangan**:
    -   Masih sangat bergantung pada kualitas, kelengkapan, dan relevansi atribut film yang dipilih. Jika atribut tidak representatif terhadap faktor penentu selera pengguna, rekomendasi kurang akurat.
    -   Kurang mampu menangkap *serendipity* (rekomendasi yang mengejutkan namun relevan) dibandingkan Collaborative Filtering murni yang berbasis interaksi aktual pengguna.
    -   Tidak bersifat personal secara individu pengguna jika tidak ada profil pengguna; rekomendasi lebih bersifat "item-to-item" berdasarkan kemiripan atribut global.
    -   Bisa rentan terhadap bias popularitas jika atribut seperti `total_votes` atau `avg_vote` sangat mendominasi perhitungan kemiripan tanpa penanganan yang tepat.

## Evaluation

Evaluasi sistem rekomendasi, terutama untuk pendekatan yang diimplementasikan (Content-Based dan Item-Based CF berbasis atribut) dengan dataset yang tidak memiliki data interaksi pengguna eksplisit (seperti rating individual atau riwayat tontonan per pengguna), lebih banyak mengandalkan penilaian kualitatif dan konsep metrik kuantitatif yang dapat diadaptasi.

**Metrik Evaluasi yang Digunakan (Konseptual dan Kualitatif):**

1.  **Evaluasi Kualitatif (Observasi Langsung)**:
    -   **Cara Kerja**: Melibatkan pemeriksaan manual terhadap beberapa contoh rekomendasi yang dihasilkan oleh kedua model untuk beberapa film sampel (input). Penilaian difokuskan pada apakah film-film yang direkomendasikan secara intuitif masuk akal dan relevan dengan film input. Aspek yang diperhatikan meliputi kesamaan genre, sutradara, aktor, tema cerita, atau 'mood' dan skor dimensi yang serupa.
    -   **Hasil Proyek (Berdasarkan Observasi Umum dari Notebook)**:
        -   *Model 1 (Content-Based Tekstual)*: Cenderung menghasilkan rekomendasi yang kuat dalam hal kemiripan naratif, pemeran, atau sutradara, yang ditangkap dari fitur `soup`. Jika sebuah film adalah bagian dari seri atau memiliki gaya penceritaan yang khas dari seorang sutradara, model ini berpotensi menangkapnya dengan baik.
        -   *Model 2 (Item-Based CF berbasis Atribut)*: Cenderung menghasilkan rekomendasi yang cocok dalam hal 'rasa' atau pengalaman menonton yang ditawarkan, seperti tingkat tensi, humor, genre yang sama, atau popularitas/rating (`avg_vote`) yang serupa. Rekomendasi bisa jadi lebih beragam dalam hal cerita spesifik dibandingkan Model 1, tetapi serupa dalam atribut-atribut film yang diukur.

2.  **Evaluasi Kuantitatif (Precision@10 dan Recall@10)**

    Untuk memberikan gambaran kuantitatif terhadap kinerja model, metrik Precision@10 dan Recall@10 dihitung. Dalam perhitungan ini, sebuah **film rekomendasi dianggap 'relevan' jika memiliki setidaknya satu genre yang sama** dengan genre utama dari film input. Genre utama film input diambil dari daftar genrenya yang tercatat dalam dataset.

    -   **Precision@k**:
        -   **Formula**: $Precision@k = \frac{\text{Jumlah item relevan yang direkomendasikan dalam top-k}}{\text{k}}$
        -   **Cara Metrik Bekerja**: Precision@k mengukur proporsi item yang direkomendasikan dalam `k` item teratas yang benar-benar 'relevan' menurut definisi proxy yang digunakan. Metrik ini menilai akurasi dari item-item yang paling atas direkomendasikan.

    -   **Recall@k**:
        -   **Formula**: $Recall@k = \frac{\text{Jumlah item relevan yang direkomendasikan dalam top-k}}{\text{Total jumlah item relevan yang mungkin ada untuk film input di seluruh dataset}}$
        -   **Cara Metrik Bekerja**: Recall@k mengukur proporsi item relevan yang berhasil ditemukan dan direkomendasikan oleh sistem dari keseluruhan item relevan yang ada (yang seharusnya direkomendasikan menurut definisi proxy) untuk film input tersebut. Metrik ini menilai kemampuan sistem untuk menemukan semua item yang relevan.

    Hasil Perhitungan Metrik Kuantitatif

    Perhitungan Precision@10 dan Recall@10 dilakukan untuk tiga film sampel menggunakan kedua model. Berikut adalah hasilnya:

    **Hasil Perhitungan Individual Precision@10 dan Recall@10:**

    * **Model 1 (Content-Based Textual):**
        * Film: 'Bugs Bunny's Third Movie: 1001 Rabbit Tales' -> Precision@10: 1.00, Recall@10: 0.01
        * Film: '18 anni tra una settimana' -> Precision@10: 0.30, Recall@10: 0.00
        * Film: 'Ride a Wild Pony' -> Precision@10: 0.10, Recall@10: 0.00
    * **Model 2 (Item-Based CF Atribut):**
        * Film: 'Bugs Bunny's Third Movie: 1001 Rabbit Tales' -> Precision@10: 1.00, Recall@10: 0.01
        * Film: '18 anni tra una settimana' -> Precision@10: 1.00, Recall@10: 0.00
        * Film: 'Ride a Wild Pony' -> Precision@10: 1.00, Recall@10: 0.01

    **Ringkasan Rata-rata Hasil Evaluasi (Precision@10 & Recall@10):**

    * **Model 1 (Content-Based Textual) - Rata-rata dari 3 film sampel:**
        * Rata-rata Precision@10: 0.47
        * Rata-rata Recall@10: 0.00 (dibulatkan, nilai aslinya mungkin sangat kecil mendekati nol)
    * **Model 2 (Item-Based CF Atribut) - Rata-rata dari 3 film sampel:**
        * Rata-rata Precision@10: 1.00
        * Rata-rata Recall@10: 0.01

    Interpretasi Hasil Metrik Kuantitatif

    Dari hasil di atas, terlihat bahwa **Model 2 (Item-Based CF Atribut)** secara konsisten memberikan **Precision@10 yang sempurna (1.00)** untuk ketiga film sampel. Ini berarti bahwa semua 10 film yang direkomendasikan oleh Model 2 untuk film-film sampel tersebut dianggap relevan berdasarkan kriteria kesamaan setidaknya satu genre dengan film input. Hasil ini menunjukkan bahwa Model 2 sangat efektif dalam menyajikan rekomendasi yang relevan (menurut proxy genre) di posisi-posisi teratas.

    **Model 1 (Content-Based Textual)** menunjukkan rata-rata Precision@10 sebesar 0.47. Ini berarti, rata-rata, sekitar 4-5 dari 10 film yang direkomendasikan oleh Model 1 relevan berdasarkan kesamaan genre. Untuk film 'Bugs Bunny's Third Movie: 1001 Rabbit Tales', Model 1 juga mencapai Precision@10: 1.00, namun untuk dua film lainnya presisinya lebih rendah.

    Mengenai **Recall@10**, kedua model menunjukkan nilai yang sangat rendah (rata-rata 0.00 untuk Model 1 dan 0.01 untuk Model 2). Nilai recall yang rendah ini dapat diinterpretasikan sebagai berikut:
    1.  **Definisi Relevansi yang Luas dan Jumlah Total Item Relevan yang Besar**: Kriteria relevansi yang digunakan ("setidaknya satu genre sama") bersifat cukup luas. Akibatnya, jumlah total film dalam dataset yang memenuhi kriteria ini untuk sebuah film input (penyebut dalam formula recall) bisa jadi sangat besar.
    2.  **Keterbatasan `k`**: Dengan hanya merekomendasikan 10 film (`k=10`), sangat sulit bagi model untuk mencakup sebagian besar dari total item relevan yang jumlahnya mungkin ratusan atau ribuan di seluruh dataset.
    3.  **Fokus Model**: Model rekomendasi seringkali dioptimalkan untuk menyajikan beberapa item terbaik di posisi atas (yang diukur oleh presisi), bukan untuk menemukan setiap item yang mungkin relevan (yang diukur oleh recall, terutama dengan `k` kecil).

    Sebagai contoh, Recall@10 sebesar 0.01 untuk Model 2 pada film 'Bugs Bunny's Third Movie: 1001 Rabbit Tales' berarti bahwa 10 film yang direkomendasikan (yang semuanya relevan menurut presisi) hanya mencakup sekitar 1% dari total film di dataset yang memiliki kesamaan genre dengan film input tersebut. Ini bukan berarti modelnya buruk dalam menemukan item yang relevan, melainkan mencerminkan perbandingan antara jumlah rekomendasi yang terbatas (10) dengan potensi jumlah total item relevan yang sangat besar di dataset berdasarkan definisi proxy yang digunakan. Nilai Recall@10: 0.00 untuk beberapa kasus menunjukkan bahwa dari 10 rekomendasi, tidak ada yang dianggap relevan menurut proxy *ATAU* jumlah total film relevan di dataset sangat besar sehingga hasil pembagiannya (setelah pembulatan) menjadi 0.00.

    Kesimpulannya, berdasarkan metrik kuantitatif dengan proxy kesamaan genre:
    -   **Model 2 menunjukkan presisi yang sangat tinggi**, menyajikan rekomendasi yang sangat relevan di posisi teratas.
    -   **Model 1 menunjukkan presisi yang cukup baik**, namun bervariasi tergantung film input.
    -   **Recall untuk kedua model rendah**, yang utamanya disebabkan oleh kombinasi definisi relevansi yang luas, jumlah rekomendasi `k` yang kecil, dan besarnya dataset.

    Meskipun perhitungan Precision@k dan Recall@k dengan proxy ini memberikan gambaran kuantitatif, penting untuk diingat bahwa ini adalah penyederhanaan dari relevansi yang sebenarnya dirasakan pengguna. Evaluasi kualitatif dan feedback pengguna langsung tetap menjadi standar emas untuk menilai sistem rekomendasi secara komprehensif.

3.  **Metrik Lainnya (Konseptual)**:
    -   **Diversity**: Mengukur seberapa beragam item-item dalam daftar rekomendasi. Diversitas yang baik berarti sistem tidak hanya merekomendasikan item yang sangat mirip satu sama lain. Dapat dinilai secara kualitatif dengan melihat variasi genre atau atribut lain dalam hasil rekomendasi.
    -   **Novelty & Serendipity**: Novelty mengukur seberapa baru atau belum diketahui item yang direkomendasikan. Serendipity mengukur seberapa mengejutkan namun tetap relevan dan menarik item yang direkomendasikan. Kedua metrik ini sulit diukur secara kuantitatif tanpa feedback pengguna, tetapi bisa dinilai secara kualitatif.

**Penjelasan Hasil Proyek Berdasarkan Metrik Evaluasi:**
Evaluasi model dilakukan secara kualitatif dan kuantitatif. Secara kualitatif, kedua model menunjukkan kemampuan untuk menghasilkan rekomendasi yang relevan secara intuitif berdasarkan kriteria kemiripan masing-masing. Model Content-Based cenderung memberikan rekomendasi dengan hubungan naratif atau tim produksi yang kuat, sementara Model Item-Based CF berbasis Atribut lebih fokus pada kemiripan "rasa" atau karakteristik film.

Secara kuantitatif, dengan menggunakan proxy relevansi berbasis kesamaan genre (setidaknya satu genre sama), diperoleh hasil sebagai berikut untuk evaluasi pada tiga film sampel (dengan k=10):
* **Model 1 (Content-Based Textual)** mencapai rata-rata Precision@10 sebesar **0.47** dan rata-rata Recall@10 sebesar **0.00**. Ini menunjukkan bahwa sekitar separuh dari rekomendasi teratasnya relevan menurut proxy genre, namun cakupannya terhadap keseluruhan item relevan di dataset (berdasarkan proxy) masih terbatas.
* **Model 2 (Item-Based CF Atribut)** menunjukkan kinerja yang sangat baik dalam hal presisi, dengan rata-rata **Precision@10 mencapai 1.00**, yang berarti semua rekomendasi teratasnya sangat relevan menurut proxy genre. Rata-rata Recall@10 untuk model ini adalah **0.01**.

Nilai recall yang rendah pada kedua model, terutama dengan k=10, dapat diatribusikan pada luasnya definisi relevansi proxy yang digunakan (kesamaan satu genre) dan besarnya jumlah total film relevan potensial dalam dataset dibandingkan dengan jumlah rekomendasi yang disajikan. Meskipun demikian, presisi yang baik, terutama pada Model 2, menunjukkan efektivitas model dalam menyajikan rekomendasi berkualitas tinggi di posisi teratas.

Proyek ini menegaskan bahwa meskipun dengan keterbatasan dataset (tidak adanya interaksi pengguna eksplisit), pendekatan Content-Based Filtering dan Item-Based Collaborative Filtering berbasis atribut dapat dikembangkan untuk memberikan rekomendasi film yang berguna. Pemilihan antara kedua model atau bahkan potensi pengembangan model hybrid di masa depan dapat disesuaikan dengan kebutuhan spesifik platform dan preferensi pengguna yang ingin dilayani. Proyek ini memberikan dasar yang baik untuk eksplorasi lebih lanjut dalam membangun sistem rekomendasi film yang lebih canggih, personal, dan dengan metode evaluasi yang lebih komprehensif.