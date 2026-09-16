# 5. Pembelajaran Mesin Supervised: Algoritma Naive Bayes (Learning Part-1)

Navigasi: Modul Sebelumnya: [[04_Algoritma_Pencarian_Informed_Search_dan_Heuristic]] | [[Konsep_Kecerdasan_Buatan]] | Modul Berikutnya: [[06_Machine_Learning_Supervised_kNN_dan_Artificial_Neural_Network]]

---

**Supervised Learning (Pembelajaran Terawasi)** adalah paradigma Machine Learning di mana model dilatih menggunakan data berlabel (*labeled data*) yang terdiri dari fitur input ($X$) dan target output ($y$).

---

## 5.1. Teorema Probabilitas Bayes

Algoritma **Naive Bayes** didasarkan pada **Teorema Bayes** yang menghitung probabilitas posterior terjadinya suatu kelas peristiwa berdasarkan pengetahuan kondisi sebelumnya:

$$P(y \mid X) = \frac{P(X \mid y) \cdot P(y)}{P(X)}$$

### Komponen Formula Teorema Bayes:
- **$P(y \mid X)$ [Posterior Probability]**: Probabilitas bahwa data sampel termasuk dalam kelas $y$, diberikan kumpulan fitur $X$.
- **$P(X \mid y)$ [Likelihood]**: Probabilitas kemunculan fitur $X$ apabila kelasnya adalah $y$.
- **$P(y)$ [Prior Probability]**: Probabilitas awal kemunculan kelas $y$ secara independen pada dataset.
- **$P(X)$ [Evidence]**: Probabilitas kemunculan kombinasi fitur $X$ secara keseluruhan.

---

## 5.2. Asumsi "Naive" dan Klasifikasi Naive Bayes

Kata **"Naive"** berasal dari asumsi penyederhanaan bahwa **setiap fitur $X_i$ saling bebas (*independen*) satu sama lain**, diberikan nilai kelas $y$:

$$P(X_1, X_2, \dots, X_n \mid y) = P(X_1 \mid y) \cdot P(X_2 \mid y) \dots P(X_n \mid y) = \prod_{i=1}^{n} P(X_i \mid y)$$

### Varian Utama Naive Bayes:
1. **Gaussian Naive Bayes**: Digunakan jika data fitur berbentuk nilai kontinu berdistribusi normal (Gaussian).
2. **Multinomial Naive Bayes**: Digunakan untuk data diskrit/frekuensi kata (populer untuk klasifikasi teks Spam/Ham).
3. **Bernoulli Naive Bayes**: Digunakan untuk data fitur biner (0 atau 1).

---

## 5.3. Contoh Kode Python Scikit-Learn: Gaussian Naive Bayes

```python
import numpy as np
from sklearn.naive_bayes import GaussianNB
from sklearn.model_selection import train_test_split
from sklearn.metrics import accuracy_score

# 1. Dataset Fitur (X: Usia, Gaji) dan Label (y: Beli Produk [0/1])
X = np.array([[22, 25000], [25, 32000], [47, 89000], [52, 110000], [46, 75000]])
y = np.array([0, 0, 1, 1, 1])

# 2. Inisialisasi Model Gaussian Naive Bayes
model = GaussianNB()

# 3. Pelatihan Model (Fit)
model.fit(X, y)

# 4. Prediksi Data Baru (Usia: 48, Gaji: 80000)
sample_baru = np.array([[48, 80000]])
prediksi = model.predict(sample_baru)
probabilitas = model.predict_proba(sample_baru)

print(f"Hasil Prediksi Kelas: {prediksi[0]}") # Output: 1 (Beli)
print(f"Probabilitas Kelas [Tidak Beli, Beli]: {probabilitas[0]}")
```

---

Navigasi: Modul Sebelumnya: [[04_Algoritma_Pencarian_Informed_Search_dan_Heuristic]] | [[Konsep_Kecerdasan_Buatan]] | Modul Berikutnya: [[06_Machine_Learning_Supervised_kNN_dan_Artificial_Neural_Network]]
