<div align="center">

<img src="assets/banner.png" alt="Deteksi Berita Hoaks dengan IndoBERT" width="100%">

<br>

<a href="https://github.com/hilmanmaulana1237">
  <img src="https://readme-typing-svg.demolab.com?font=Fira+Code&weight=600&size=22&duration=3000&pause=1000&color=38BDF8&center=true&vCenter=true&width=760&height=50&lines=Klasifikasi+Berita+Hoaks+Bahasa+Indonesia;Fine-tuning+IndoBERT+Base+P2;Akurasi+Test+99.55%25+%7C+ROC-AUC+0.9992;Explainable+AI+dengan+LIME" alt="Typing SVG">
</a>

<br><br>

![Python](https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white)
![HuggingFace](https://img.shields.io/badge/IndoBERT-FFD21E?style=for-the-badge&logo=huggingface&logoColor=black)
![LIME](https://img.shields.io/badge/Explainability-LIME-8b5cf6?style=for-the-badge)
![Jupyter](https://img.shields.io/badge/Jupyter-F37626?style=for-the-badge&logo=jupyter&logoColor=white)
![Akurasi](https://img.shields.io/badge/Akurasi_Test-99.55%25-10b981?style=for-the-badge)
![ROC-AUC](https://img.shields.io/badge/ROC--AUC-0.9992-0ea5e9?style=for-the-badge)

<br>

[**Ringkasan**](#-ringkasan) &nbsp;•&nbsp;
[**Alur Kerja**](#-alur-kerja) &nbsp;•&nbsp;
[**Dataset**](#-dataset) &nbsp;•&nbsp;
[**Hasil**](#-hasil-evaluasi) &nbsp;•&nbsp;
[**Galeri**](#-galeri-visualisasi) &nbsp;•&nbsp;
[**LIME**](#-interpretabilitas-lime) &nbsp;•&nbsp;
[**Cara Menjalankan**](#-cara-menjalankan)

<br>

<a href="[https://colab.research.google.com/github/hilmanmaulana1237/NAMA-REPO/blob/main/uts_nlp_hilman.ipynb](https://colab.research.google.com/drive/1BSvK4n1NtBeIDiVFQ-TqKTnn9jRIQsiW?usp=sharing)">
  <img src="https://colab.research.google.com/assets/colab-badge.svg" alt="Buka di Google Colab">
</a>

</div>

<br>

## 📌 Ringkasan

Proyek ini adalah tugas **UTS Natural Language Processing (NLP)** untuk mengklasifikasikan artikel berita politik berbahasa Indonesia ke dalam dua kategori, menggunakan dataset **Indonesian Fact and Hoax Political News**.

| Label | Kelas | Arti |
| :---: | :--- | :--- |
| `0` | ✅ **Fakta** | Artikel berita yang sesuai fakta |
| `1` | 🚨 **Hoax** | Artikel berita bohong atau menyesatkan |

Model yang dipakai adalah **IndoBERT Base P2** ([`indobenchmark/indobert-base-p2`](https://huggingface.co/indobenchmark/indobert-base-p2)) dari HuggingFace. Untuk memahami *mengapa* model memutuskan sebuah artikel itu fakta atau hoax, hasil prediksinya dianalisis dengan **LIME**.

<div align="center">
<table>
  <tr>
    <td align="center" width="25%"><h2>99.55%</h2><sub>Akurasi Test</sub></td>
    <td align="center" width="25%"><h2>99.57%</h2><sub>F1-Score Test</sub></td>
    <td align="center" width="25%"><h2>0.9992</h2><sub>ROC-AUC</sub></td>
    <td align="center" width="25%"><h2>99.23 – 99.87%</h2><sub>95% Bootstrap CI</sub></td>
  </tr>
</table>
</div>

<br>

## 🧭 Alur Kerja

```mermaid
flowchart LR
    A["📰 Dataset<br/>18.310 artikel"] --> B["🔎 EDA<br/>kelas, panjang kata, kata teratas"]
    B --> C["✂️ Tokenisasi<br/>IndoBERT tokenizer"]
    C --> D["🧠 Fine-tuning<br/>IndoBERT Base P2"]
    D --> E["📈 Evaluasi<br/>confusion matrix, ROC-AUC, bootstrap"]
    E --> F["💡 LIME<br/>kata paling berpengaruh"]

    style A fill:#1e293b,stroke:#38bdf8,color:#e2e8f0
    style B fill:#1e293b,stroke:#38bdf8,color:#e2e8f0
    style C fill:#1e293b,stroke:#38bdf8,color:#e2e8f0
    style D fill:#1e293b,stroke:#8b5cf6,color:#e2e8f0
    style E fill:#1e293b,stroke:#10b981,color:#e2e8f0
    style F fill:#1e293b,stroke:#f43f5e,color:#e2e8f0
```

<br>

## 📚 Dataset

Dataset sudah dibagi menjadi tiga bagian dan disimpan di folder [`data/`](data/).

| Bagian | File | Jumlah Baris | Porsi | Fungsi |
| :--- | :--- | ---: | ---: | :--- |
| 🏋️ Train | `combined_train.csv` | 15.187 | ≈ 83% | Melatih model |
| 🧪 Validation | `combined_val.csv` | 1.561 | ≈ 8,5% | Memantau training per epoch |
| 🎯 Test | `combined_test.csv` | 1.562 | ≈ 8,5% | Evaluasi akhir |
| | **Total** | **18.310** | 100% | |

<br>

## 🏆 Hasil Evaluasi

Semua angka di bawah dihitung pada **data test** (1.562 artikel) yang tidak dipakai saat training.

| Metrik | Nilai |
| :--- | :---: |
| Akurasi | **99.55%** |
| F1-Score | **99.57%** |
| ROC-AUC | **0.9992** |
| 95% Bootstrap CI (1.000 iterasi) | **[99.23% – 99.87%]** |

Interval kepercayaan yang sempit menunjukkan bahwa hasil ini stabil terhadap pengambilan ulang sampel data test.

<br>

## 📊 Galeri Visualisasi

Klik tiap bagian untuk membuka grafiknya.

<details>
<summary><b>🔎 Eksplorasi Data (EDA)</b></summary>
<br>

<table>
  <tr>
    <td align="center" width="50%">
      <img src="hasil/distribusi_kelas.png" alt="Distribusi kelas" width="100%"><br>
      <sub>Jumlah data fakta dan hoax pada train, val, dan test</sub>
    </td>
    <td align="center" width="50%">
      <img src="hasil/panjang_kata_artikel.png" alt="Panjang kata artikel" width="100%"><br>
      <sub>Distribusi panjang kata artikel dan batas tokenisasi</sub>
    </td>
  </tr>
  <tr>
    <td align="center" width="50%">
      <img src="hasil/perbandingan_panjang_fakta_hoax.png" alt="Perbandingan panjang fakta dan hoax" width="100%"><br>
      <sub>Boxplot panjang kata fakta vs hoax</sub>
    </td>
    <td align="center" width="50%"></td>
  </tr>
  <tr>
    <td align="center" width="50%">
      <img src="hasil/top_kata_fakta.png" alt="Kata teratas berita fakta" width="100%"><br>
      <sub>12 kata paling sering muncul di berita fakta</sub>
    </td>
    <td align="center" width="50%">
      <img src="hasil/top_kata_hoax.png" alt="Kata teratas berita hoax" width="100%"><br>
      <sub>12 kata paling sering muncul di berita hoax</sub>
    </td>
  </tr>
</table>

</details>

<details>
<summary><b>📉 Proses Training</b></summary>
<br>

<table>
  <tr>
    <td align="center" width="50%">
      <img src="hasil/grafik_loss_training.png" alt="Grafik loss training" width="100%"><br>
      <sub>Penurunan loss training dan validation per epoch</sub>
    </td>
    <td align="center" width="50%">
      <img src="hasil/grafik_akurasi_training.png" alt="Grafik akurasi training" width="100%"><br>
      <sub>Peningkatan akurasi dan F1-score selama training</sub>
    </td>
  </tr>
</table>

</details>

<details open>
<summary><b>🎯 Evaluasi Model</b></summary>
<br>

<table>
  <tr>
    <td align="center" width="33%">
      <img src="hasil/confusion_matrix.png" alt="Confusion matrix" width="100%"><br>
      <sub>Confusion matrix pada data test</sub>
    </td>
    <td align="center" width="33%">
      <img src="hasil/kurva_roc_auc.png" alt="Kurva ROC AUC" width="100%"><br>
      <sub>Kurva ROC dan skor AUC</sub>
    </td>
    <td align="center" width="33%">
      <img src="hasil/distribusi_bootstrap.png" alt="Distribusi bootstrap" width="100%"><br>
      <sub>1.000 iterasi bootstrap dan 95% CI</sub>
    </td>
  </tr>
</table>

</details>

<details open>
<summary><b>💡 Interpretabilitas (LIME)</b></summary>
<br>

<table>
  <tr>
    <td align="center" width="50%">
      <img src="hasil/lime_fakta.png" alt="LIME berita fakta" width="100%"><br>
      <sub>Bobot kata pada prediksi berita <b>fakta</b></sub>
    </td>
    <td align="center" width="50%">
      <img src="hasil/lime_hoax.png" alt="LIME berita hoax" width="100%"><br>
      <sub>Bobot kata pada prediksi berita <b>hoax</b></sub>
    </td>
  </tr>
</table>

</details>

<br>

## 🔍 Interpretabilitas LIME

LIME menunjukkan kata mana yang paling mendorong model ke satu kelas. Polanya konsisten dengan gaya bahasa kedua jenis berita:

| Kecenderungan | Kata yang berpengaruh | Pola bahasa |
| :--- | :--- | :--- |
| ✅ **Fakta** | ![kementerian](https://img.shields.io/badge/kementerian-10b981?style=flat-square) ![klarifikasi](https://img.shields.io/badge/klarifikasi-10b981?style=flat-square) ![resmi](https://img.shields.io/badge/resmi-10b981?style=flat-square) | Bahasa formal dan kelembagaan |
| 🚨 **Hoax** | ![viralkan](https://img.shields.io/badge/viralkan-f43f5e?style=flat-square) ![gawat](https://img.shields.io/badge/gawat-f43f5e?style=flat-square) ![terbongkar](https://img.shields.io/badge/terbongkar-f43f5e?style=flat-square) | Memancing emosi dan mengajak menyebarkan |

<br>

## 📁 Struktur Folder

```text
.
├── assets/
│   └── banner.svg                        # banner README
├── data/
│   ├── combined_train.csv                # 15.187 baris
│   ├── combined_val.csv                  # 1.561 baris
│   └── combined_test.csv                 # 1.562 baris
├── hasil/                                # 12 grafik hasil analisis dan evaluasi
│   ├── distribusi_kelas.png
│   ├── panjang_kata_artikel.png
│   ├── perbandingan_panjang_fakta_hoax.png
│   ├── top_kata_fakta.png
│   ├── top_kata_hoax.png
│   ├── grafik_loss_training.png
│   ├── grafik_akurasi_training.png
│   ├── confusion_matrix.png
│   ├── kurva_roc_auc.png
│   ├── distribusi_bootstrap.png
│   ├── lime_fakta.png
│   └── lime_hoax.png
├── uts_nlp_hilman.ipynb                  # notebook utama
├── requirements.txt                      # daftar library Python
└── README.md
```

<br>

## 🚀 Cara Menjalankan

**1. Clone repositori**

```bash
git clone https://github.com/hilmanmaulana1237/NAMA-REPO.git
cd NAMA-REPO
```

**2. Install library**

```bash
pip install -r requirements.txt
```

**3. Buka notebook**

Buka `uts_nlp_hilman.ipynb` di Jupyter Notebook atau VS Code. Bisa juga langsung lewat Google Colab:

<a href="https://colab.research.google.com/github/hilmanmaulana1237/NAMA-REPO/blob/main/uts_nlp_hilman.ipynb">
  <img src="https://colab.research.google.com/assets/colab-badge.svg" alt="Buka di Google Colab">
</a>

**4. Jalankan semua cell**

Jalankan cell secara berurutan dari atas ke bawah.

<br>

## 🧰 Teknologi

| Komponen | Keterangan |
| :--- | :--- |
| 🤖 Model | [IndoBERT Base P2](https://huggingface.co/indobenchmark/indobert-base-p2) (`indobenchmark/indobert-base-p2`) |
| 💡 Interpretabilitas | LIME |
| 📓 Lingkungan | Jupyter Notebook, VS Code, atau Google Colab |
| 🐍 Bahasa | Python |

<br>

## 📝 Catatan

- Hasil LIME menunjukkan model banyak bersandar pada **gaya bahasa** (formal vs memancing emosi). Karena itu, performa pada berita dari sumber atau periode lain perlu diuji lagi sebelum dipakai di luar dataset ini.
- Proyek ini dibuat untuk keperluan akademik dan bukan pengganti pemeriksa fakta profesional.

<br>

## 👤 Pengembang

<div align="center">

<a href="https://github.com/hilmanmaulana1237">
  <img src="https://github.com/hilmanmaulana1237.png?size=140" width="110" alt="Hilman di GitHub">
</a>

### Hilman

Mahasiswa, mata kuliah Natural Language Processing (NLP)

<a href="https://github.com/hilmanmaulana1237">
  <img src="https://img.shields.io/badge/GitHub-hilmanmaulana1237-181717?style=for-the-badge&logo=github&logoColor=white" alt="GitHub hilmanmaulana1237">
</a>
<a href="https://github.com/hilmanmaulana1237?tab=followers">
  <img src="https://img.shields.io/github/followers/hilmanmaulana1237?label=Follow&style=for-the-badge&logo=github" alt="Followers">
</a>

<br><br>

Kalau proyek ini bermanfaat, jangan ragu memberi ⭐ pada repositori ini.

</div>

<img src="https://capsule-render.vercel.app/api?type=waving&color=0:0d1224,100:34d399&height=140&section=footer&animation=fadeIn" width="100%" alt="">
