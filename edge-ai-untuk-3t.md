# Edge AI untuk Deteksi Penyakit di Daerah 3T Indonesia

## Pendahuluan

Menjalankan model AI untuk deteksi penyakit di daerah 3T (Tertinggal, Terdepan, Terluar) Indonesia adalah challenge yang nyata. Komputernya tidak sebagus di kota besar, koneksi internet tidak stabil, dan data spesifikasi hardware di faskes 3T jarang dipublikasikan. Dokumen ini membahas strategi arsitektur dan optimisasi yang bisa dipakai.

---

## 1. Model Compression & Quantization (Paling Praktis)

### Quantization

Mengurangi presisi angka dalam model dari float32 (32-bit) menjadi int8 (8-bit):

- **Keuntungan:**
  - Model bisa 4x lebih kecil dalam memori
  - Kecepatan eksekusi bisa 2-4x lebih cepat
  - Akurasi turun sedikit, tapi masih acceptable untuk use case medis jika ditrain dengan hati-hati

- **Tools yang tersedia:**
  - TensorFlow Lite Quantization
  - PyTorch Quantization
  - ONNX Runtime

**Contoh Nyata:**
```
Model asli (float32):  200 MB  →  50 MB (setelah quantization)
Waktu inference:       5 detik →  1-2 detik (di CPU lama)
```

### Pruning

Menghilangkan weight/neuron yang kurang penting dari model:

- Bisa ngurangi model size 50-90% dengan akurasi loss minimal
- Memerlukan fine-tuning setelah pruning
- Hasil model lebih ringan dan cepat dieksekusi

---

## 2. Pilih Model yang Lightweight dari Awal

**Jangan gunakan model besar seperti:**
- ResNet-152
- EfficientNet-B7
- Vision Transformer

**Gunakan model yang dirancang untuk resource terbatas:**

| Model | Size | Inference Time (CPU Lama) | Akurasi |
|-------|------|---------------------------|---------|
| **MobileNet v2/v3** | ~14 MB | 200-300 ms | ✓ Bagus |
| **EfficientNet-B0/B1** | ~30 MB | 500 ms | ✓ Sangat Bagus |
| **SqueezeNet** | ~5 MB | 400-600 ms | ✓ Cukup |
| **Custom CNN Kecil** | ~10-20 MB | 300-400 ms | ✓ Bisa Custom |
| **ResNet-50** (Pembanding) | ~100 MB | 2-3 detik | ✗ Terlalu Berat |

**Rekomendasi:** MobileNetV2 atau EfficientNet-B0 sebagai starting point.

---

## 3. Edge Computing Architecture (Konsep Penting!)

Alih-alih mengirim data ke server cloud, jalankan model **langsung di komputer lokal**:

### Arsitektur Cloud-Only (Masalah)

```
Faskes → Internet → Cloud Server → Hasil
```

**Kelemahan:**
- Butuh koneksi internet stabil (langka di 3T)
- Butuh bandwidth besar (foto X-ray ~5-10 MB)
- Latency tinggi (bisa beberapa detik)
- Privacy concern (data keluar dari fasilitas)

### Arsitektur Edge Computing (Solusi) ✓

```
Faskes → Komputer Lokal → Hasil
```

**Keunggulan:**
- Offline capable (bisa tanpa internet)
- Instant result (latency minimal)
- Privacy lebih terjaga
- Bisa cache model sekali, eksekusi berkali-kali

---

## 4. Optimisasi Inference Runtime

Pakai tools khusus inference, bukan training framework:

| Runtime | Kecepatan | Ukuran | Platform | Best For |
|---------|-----------|--------|----------|----------|
| **TensorFlow Lite** | ⭐⭐⭐⭐ | Kecil | Android, iOS | Mobile/Edge |
| **ONNX Runtime** | ⭐⭐⭐⭐ | Kecil | Cross-platform | Edge Device |
| **OpenVINO (Intel)** | ⭐⭐⭐⭐ | Sedang | CPU Intel/AMD | Desktop Lama |
| **ncnn (TencentCloud)** | ⭐⭐⭐⭐ | Sangat Kecil | Mobile | Ultra-lightweight |

### Contoh TensorFlow Lite

```python
import tensorflow as tf

# Load model
interpreter = tf.lite.Interpreter(model_path="model.tflite")
interpreter.allocate_tensors()

# Inference
input_data = preprocess_image(img)
interpreter.set_tensor(input_details[0]['index'], input_data)
interpreter.invoke()
output_data = interpreter.get_tensor(output_details[0]['index'])

print(f"Hasil: {output_data}")
```

---

## 5. Realistic Hardware Target untuk 3T

Karena susah dapat data pasti, bisa target hardware yang "likely" tersedia:

| Hardware | Est. Harga | Inference Time (MobileNetV2) | Availability di 3T |
|----------|-----------|------------------------------|------------------|
| **Laptop lama (i3, 4GB RAM)** | Rp 1-2 juta | 300-500 ms | ✓ Cukup Umum |
| **Desktop lama (Pentium/i5, 2-4GB)** | Rp 2-3 juta | 200-400 ms | ✓ Cukup Umum |
| **Raspberry Pi 4 (4GB)** | Rp 1.5 juta | 1-2 detik | ~ Mulai Ada |
| **Jetson Nano (GPU mini)** | Rp 2-3 juta | 100-200 ms | ✓ BEST OPTION |

### Rekomendasi Paling Praktis

**Pilihan terbaik:** Jetson Nano atau laptop bekas i5 generasi 7-8 dengan 8GB RAM (budget Rp 3-4 juta)

**Alasan:**
- Jetson Nano punya GPU kecil → bisa jalankan model lebih cepat
- Laptop i5 bekas mudah dicari dan affordable
- Keduanya bisa offline, instalasi software mudah

---

## 6. Strategi Hybrid (Optimal untuk Scalability)

Kombinasi edge + cloud:

```
Flow Diagram:
┌─────────────────┐
│  Faskes/Clinic  │
│  (Komputer Lokal)
└────────┬────────┘
         │
         ├─ Inference dg Model Ringan (MobileNet)
         │
         ├─ Confidence Score Tinggi? → Tampilkan Hasil ✓
         │
         └─ Confidence Score Rendah? → Kirim ke Cloud
                                        │
                                   Cloud Server
                                   (Model Akurat Berat)
                                        │
                                   Kirim Balik Hasil
                                        │
                                   Tampilkan di Faskes ✓
```

**Keuntungan:**
- Mengurangi beban cloud
- Tetap punya akurasi tinggi untuk case sulit
- Fleksibel: offline untuk case simple, online untuk case kompleks

---

## 7. Training Strategy yang Cocok

Jika kamu mau train model sendiri (bukan transfer learning dari model pre-trained):

### Best Practices:

1. **Data Augmentation Aggressive**
   - Karena data di 3T mungkin terbatas
   - Rotation, flip, zoom, brightness adjustment

2. **Class Imbalance Handling**
   - Penyakit tertentu lebih sering dari yang lain
   - Pakai weighted loss atau oversampling

3. **Quantization-Aware Training (QAT)**
   - Train model sambil simulasikan quantization
   - Hasil lebih optimal dibanding post-training quantization

4. **Knowledge Distillation**
   - Teacher model besar → Student model kecil
   - Student model belajar dari teacher, jadi lebih akurat meski kecil

### Contoh Quantization-Aware Training (PyTorch)

```python
import torch
import torch.quantization as quantization

# Load model
model = load_model()

# Prepare untuk QAT
model.qconfig = quantization.get_default_qat_qconfig('fbgemm')
quantization.prepare_qat(model, inplace=True)

# Training loop (normal training)
for epoch in range(num_epochs):
    for images, labels in train_loader:
        outputs = model(images)
        loss = criterion(outputs, labels)
        loss.backward()
        optimizer.step()

# Convert ke quantized model
quantization.convert(model, inplace=True)
torch.jit.script(model).save("model_quantized.pt")
```

---

## Ringkasan: Rekomendasi Praktis

### Untuk Model

✓ **Pilih:** MobileNetV2 atau EfficientNet-B0 (transfer learning dari pre-trained)
✗ **Hindari:** ResNet besar, Vision Transformer

### Untuk Optimisasi

1. Quantization ke int8
2. Pruning (~30-50% weight reduction)
3. Quantization-Aware Training saat fine-tuning

### Untuk Runtime

- **TensorFlow Lite** untuk deployment
- **ONNX Runtime** untuk cross-platform
- **OpenVINO** jika target CPU Intel/AMD lama

### Untuk Deployment

1. **Edge-first architecture** (inference lokal)
2. **Offline-capable** (tidak wajib internet)
3. **Optional cloud** untuk refine diagnosis sulit

### Hardware Target

- **Primary:** Laptop/Desktop bekas i5-i7 gen 7-8 (Rp 3-4 juta)
- **Secondary:** Jetson Nano (Rp 2-3 juta, lebih cepat)
- **Budget:** Raspberry Pi 4 (Rp 1.5 juta, lebih lambat)

---

## Ide untuk Karya Tulis Ilmiah (Skripsi/Makalah)

Topik yang menarik dan relevan:

**"Edge AI untuk Diagnosis Penyakit di Daerah 3T Indonesia: Optimisasi Model dan Arsitektur Komputasi"**

### Outline yang bisa dipakai:

1. **Pendahuluan**
   - Gap infrastruktur di 3T
   - Kebutuhan AI untuk healthcare
   - Kenapa model kecil penting

2. **Tinjauan Literatur**
   - Model lightweight (MobileNet, EfficientNet)
   - Quantization techniques
   - Edge computing architecture

3. **Metodologi**
   - Transfer learning dari pre-trained model
   - Comparison quantization methods (post-training vs QAT)
   - Performance metrics: model size, latency, accuracy

4. **Implementasi**
   - Dataset (bisa synthetic atau real dari open source)
   - Training dan quantization process
   - Deployment di berbagai hardware

5. **Hasil & Analisis**
   - Model size vs accuracy trade-off
   - Inference time di berbagai hardware
   - Energy consumption (jika relevant)

6. **Kesimpulan & Rekomendasi**
   - Hardware minimum untuk faskes 3T
   - Best practices deployment
   - Saran untuk penelitian lanjut

### Nilai Tambah:

- **Praktis** — langsung applicable di dunia nyata
- **Relevan** — addressing real problem di Indonesia
- **Novel** — belum banyak yang garap khusus untuk konteks 3T Indonesia
- **Implementasi** — bisa langsung demo dengan hardware murah

---

## Resource Tambahan

### Tools & Framework
- [TensorFlow Lite Documentation](https://www.tensorflow.org/lite)
- [PyTorch Quantization](https://pytorch.org/docs/stable/quantization.html)
- [ONNX Runtime](https://onnxruntime.ai/)
- [OpenVINO Toolkit](https://github.com/openvinotoolkit/openvino)

### Pre-trained Models
- TensorFlow Hub
- PyTorch Hub
- Hugging Face Model Hub

### Datasets (Medis)
- ChexPert (Chest X-rays)
- MIMIC-CXR (Radiologi)
- Kaggle Medical Datasets

---

## Pertanyaan yang Sering Diajukan

### Q: Berapa banyak akurasi yang hilang setelah quantization?

**A:** Typically 0.5-2% jika dilakukan dengan baik. Dengan quantization-aware training, bisa diminimalkan hingga <0.5%.

### Q: Apakah model quantized bisa di-deploy ulang ke cloud?

**A:** Ya, bisa. Quantized model bisa dimuat ulang dan di-fine-tune jika diperlukan.

### Q: Apa bedanya int8 dengan float16?

**A:** int8 lebih kecil dan lebih cepat, tapi presisi lebih rendah. float16 adalah compromise antara float32 dan int8.

### Q: Berapa minimum bandwidth untuk kirim model ke edge device pertama kali?

**A:** MobileNetV2 quantized (~5 MB) bisa didownload dalam hitungan menit bahkan dengan koneksi 3G.

### Q: Bagaimana jika hardware di faskes lebih jelek dari prediksi?

**A:** Bisa gunakan model yang lebih kecil lagi (SqueezeNet) atau batch inference (kumpulin requests, proses bareng untuk efisiensi).

---

**Dokumen ini dibuat pada:** 1 Agustus 2026

**Status:** Ready for Discussion & Implementation

*Untuk pertanyaan lebih lanjut, tanyakan ke sumber atau discussion bersama mentor/pembimbing.*
