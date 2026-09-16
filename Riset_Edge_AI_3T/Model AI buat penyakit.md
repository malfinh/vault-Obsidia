**Riset terkait sudah ada** — WHO dan beberapa tim akademik sudah membangun model AI untuk penyakit kulit tropis terabaikan (skin NTDs). Ada studi yang menggunakan CNN pada dataset 1229 gambar kulit dari 222 pasien kusta di Brazil, mencapai akurasi klasifikasi 90% dengan AUC 96,46%. WHO sendiri sudah mengembangkan algoritma computer vision untuk mengidentifikasi 12 penyakit kulit NTD termasuk kusta, filariasis limfatik, skabies, dan frambusia, menggunakan dataset gabungan skin NTD, penyakit kulit lain, kulit sehat, dan gambar non-kulit. Studi lain di Afrika Barat memakai arsitektur ResNet-50 dan VGG-16 untuk lima penyakit: Buruli ulcer, kusta, mycetoma, skabies, dan frambusia.

saran penyakit adalah kusta karena :
- Endemik tinggi di Papua, NTT, Sulawesi — cocok relevansi geografis
- Ciri visual jelas (bercak hipopigmentasi/eritema dengan hilang rasa) tapi cukup bervariasi antar stadium, jadi cukup menantang secara ilmiah (bagus untuk paper, tidak trivial)
- Ada preseden riset yang bisa jadi rujukan metodologi dan benchmark akurasi
- Dampak nyata: diagnosis dini kusta mencegah kecacatan permanen — narasi kuat untuk latar belakang paper

Model AI yang relevan :
- **MobileNetV3-Small/Large** : Didesain khusus untuk edge, dukungan native untuk quantization-aware training (QAT), sudah dioptimasi TFLite
- **EfficientNet-Lite**  : Varian EfficientNet tanpa operasi yang sulit dikuantisasi (swish diganti relu6), cocok untuk INT8
- **SqueezeNet / ShuffleNetV2** : Kalau device benar-benar terbatas (RAM <2GB)

### Dataset spesifik kusta

- **CO2Wounds-V2** di Kaggle — dataset gambar luka kronis dari pasien kusta, ini paling relevan langsung kalau fokusmu kusta. Cari "Leprosy Chronic Wound Images CO2Wounds-V2" di Kaggle. [Kaggle](https://www.kaggle.com/datasets/orvile/leprosy-chronic-wound-images-co2wounds-v2)
- Dataset dari studi **AI4Leprosy** (Brazil) yang saya sebutkan sebelumnya — 1229 gambar kulit dan 585 set metadata dari 222 pasien, disimpan sebagai open-source dataset, tapi aksesnya dibatasi dan hanya tersedia setelah registrasi serta validasi pengguna — perlu kamu hubungi tim peneliti lewat paper aslinya (ScienceDirect/Lancet Regional Health Americas). [ScienceDirect](https://www.sciencedirect.com/science/article/pii/S2667193X22000096)[The Lancet](https://www.thelancet.com/journals/lanam/article/PIIS2667-193X\(22\)00009-6/fulltext)
- **WHO Skin NTD dataset** — dataset gabungan 12 skin NTD termasuk kusta, dengan total 5.760 gambar skin NTD, 16.577 penyakit kulit lain, 2.469 kulit sehat, dan 50.000 gambar non-kulit. Ini belum tentu publik penuh, tapi worth dicek lewat WHO NTD portal atau menghubungi tim risetnya — biasanya lewat aplikasi "Skin NTDs App" WHO. [ScienceDirect](https://www.sciencedirect.com/science/article/pii/S0022202X26011851)
