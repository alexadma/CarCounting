# 🚗 CarCounting — Vehicle Detection & Counting System

Sistem deteksi dan penghitungan kendaraan secara **realtime** dari stream CCTV jalan tol  
menggunakan **YOLOv12 + ByteTrack tracker**.

![Python](https://img.shields.io/badge/Python-3.9%2B-blue)
![YOLOv12](https://img.shields.io/badge/YOLO-v12-green)
![OpenCV](https://img.shields.io/badge/OpenCV-4.x-red)
![License](https://img.shields.io/badge/License-MIT-yellow)

---

[## 📸 Demo

> Sistem mendeteksi kendaraan dari CCTV tol secara realtime,  
> menghitung kendaraan yang melewati garis virtual di 2 jalur berbeda.
](https://youtu.be/FiZLmGX7uZ8)
```
← Jalur Kiri  (Sukabumi/Jakarta) — kendaraan menjauh
→ Jalur Kanan (Puncak/Ciawi)    — kendaraan mendekati
```

---

## ✨ Fitur

- ✅ Deteksi **13 kelas kendaraan** (car, big bus, big truck, small truck, dll)
- ✅ **Dual-lane counting** — jalur kiri & kanan dihitung terpisah
- ✅ **Virtual line crossing** dengan ByteTrack tracker (tidak double-count)
- ✅ Stream langsung dari **CCTV HLS** (`.m3u8`)
- ✅ Confidence threshold yang bisa dikustomisasi
- ✅ Export log crossing ke **CSV**
- ✅ Simpan output video hasil deteksi

---

## 🗂️ Struktur Project

```
CarCounting/
├── notebooks/
│   └── vehicle_counting.ipynb   ← semua kode ada di sini
├── assets/
│   ├── sample_predictions.png
│   ├── map_metrics.png
│   └── confusion_matrix_combined.png
├── .gitignore
├── requirements.txt
└── README.md
```

> **Catatan:** Dataset, model weights (`*.pt`), dan video output tidak disertakan  
> karena ukuran file besar. Ikuti langkah instalasi di bawah untuk setup dari awal.

---

## ⚙️ Instalasi

### Prasyarat

Pastikan sudah terinstall:
- Python **3.9** atau lebih baru → [download](https://www.python.org/downloads/)
- Git → [download](https://git-scm.com/downloads)
- GPU NVIDIA (opsional tapi disarankan, untuk training lebih cepat)

---

### 1. Clone Repository

```bash
git clone https://github.com/alexadma/CarCounting.git
cd CarCounting
```

---

### 2. Buat Virtual Environment (disarankan)

**Windows:**
```bash
python -m venv venv
venv\Scripts\activate
```

**Mac / Linux:**
```bash
python3 -m venv venv
source venv/bin/activate
```

---

### 3. Install Dependencies

```bash
pip install -r requirements.txt
```

Jika pakai GPU NVIDIA, install PyTorch versi CUDA dulu sebelum langkah ini:
```bash
# Cek versi CUDA kamu dulu: nvidia-smi
pip install torch torchvision --index-url https://download.pytorch.org/whl/cu118
```

---

### 4. Buka Notebook

```bash
jupyter notebook notebooks/vehicle_counting.ipynb
```

---

## 🚀 Cara Penggunaan

Jalankan cell di notebook **secara berurutan**:

| Cell | Fungsi | Keterangan |
|------|--------|------------|
| **Cell 1** | Download dataset | Butuh Roboflow API key |
| **Cell 2** | Training YOLOv12 | Butuh GPU, ~1-2 jam |
| **Cell 3** | Prediksi gambar test | Cek hasil deteksi |
| **Cell 4** | Sample prediksi | Visualisasi 9 gambar acak |
| **Cell 5** | Grafik mAP & Loss | Analisis performa training |
| **Cell 6-7** | Confusion matrix | Evaluasi per kelas |
| **Cell 8** | Ringkasan evaluasi | mAP, Precision, Recall |
| **Cell 9-10** | Cek stream CCTV | Verifikasi koneksi HLS |
| **Cell 11** | Deteksi realtime | Single line counting |
| **Cell 12** | **Dual-lane counting** | ⭐ Fitur utama |
| **Cell 13** | Export CSV | Simpan log crossing |

---

### Konfigurasi Penting di Cell 12

Sesuaikan nilai berikut dengan kondisi CCTV kamu:

```python
CONF_THRESHOLD   = 0.70   # threshold confidence (0.0 - 1.0)
DIVIDER_RATIO    = 0.50   # posisi pembatas jalan (% dari lebar frame)
LINE_LEFT_RATIO  = 0.62   # posisi garis jalur kiri (% dari tinggi frame)
LINE_RIGHT_RATIO = 0.55   # posisi garis jalur kanan (% dari tinggi frame)

HLS_URL = "https://..."   # ganti dengan URL stream CCTV kamu
```

---

## 📦 Dataset

- **Source:** [Roboflow Universe — Vehicles](https://universe.roboflow.com/roboflow-100/vehicles-q0x2v)
- **Jumlah gambar:** ~3.000+ (train/val/test)
- **13 Kelas:**

| Kelas | Kelas | Kelas |
|-------|-------|-------|
| car | big bus | big truck |
| small bus | small truck | mid truck |
| bus-l- | bus-s- | truck-s- |
| truck-m- | truck-l- | truck-xl- |

> Download otomatis saat menjalankan Cell 1. Butuh **Roboflow API key** gratis  
> dari [roboflow.com](https://roboflow.com).

---

## 🧠 Model

| Parameter | Value |
|-----------|-------|
| Architecture | YOLOv12n |
| Input size | 640 × 640 |
| Parameters | ~2.5M |
| Tracker | ByteTrack |
| Confidence default | 0.70 |

---

## 📋 Requirements

```
ultralytics>=8.0.0
roboflow
opencv-python
numpy
pandas
matplotlib
lapx
PyYAML
jupyter
ipython
```

Install semua sekaligus:
```bash
pip install -r requirements.txt
```

---

## ❓ Troubleshooting

**`ModuleNotFoundError: No module named 'lap'`**
```bash
pip install lapx
```

**`fatal: detected dubious ownership`**
```bash
git config --global --add safe.directory 'PATH/TO/CarCounting'
```

**Stream CCTV tidak terbuka:**
```python
# Coba tambahkan backend FFMPEG
cap = cv2.VideoCapture(HLS_URL, cv2.CAP_FFMPEG)
```

**Training sangat lambat (CPU):**
```bash
# Pastikan PyTorch versi CUDA terinstall
python -c "import torch; print(torch.cuda.is_available())"
# Harus output: True
```

---

## 👤 Author

**Alexander Adma**

[![Portfolio](https://img.shields.io/badge/Portfolio-alexander--adma.vercel.app-blue)](https://alexander-adma.vercel.app)
[![GitHub](https://img.shields.io/badge/GitHub-alexadma-black)](https://github.com/alexadma)

---

## 📄 License

MIT License — bebas digunakan dan dimodifikasi dengan mencantumkan kredit.
