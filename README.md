# Multi-Channel 3D Hybrid SRGAN for Quantitative QALAS MRI

Bu proje, **QALAS** (Quantification using an Interleaved Look-Locker Acquisition Sequence) sekansı ile elde edilen nicel MRI haritalarındaki (T1, T2, PD) düşük çözünürlük ve gürültü sorunlarını gidermek amacıyla geliştirilmiştir. Model, çok kanallı (multi-channel) yapısı sayesinde farklı nicel haritalar arasındaki ilişkileri öğrenerek görüntüyü iyileştirir.

## 📌 Proje Özeti

QALAS, tarama süresini kısaltsa da mekânsal çözünürlükten ödün verilmesine ve gürültü artışına neden olur. Bu çalışma, **Residual U-Net tabanlı 3D Hybrid SRGAN** mimarisi kullanarak:

* Mekânsal çözünürlüğü 4 kat artırmayı (Super-Resolution),
* Klinik verilerdeki gürültüyü azaltmayı (Denoising),
* Nicel (kantitatif) değerlerin doğruluğunu korumayı hedefler.

## 📊 Veri Seti ve Kaynakça

Bu çalışmada kullanılan modelin eğitimi ve test süreçlerinde, **Shohei Fujita** tarafından Zenodo platformunda paylaşılan açık kaynaklı "Vendor-agnostic 3D multiparametric relaxometry" veri seti kullanılmıştır.

* **Kullanılan Dosyalar:** `img_qalas.nii` (QALAS görüntüleri) ve `img_b1.nii`.
* **Veri Kaynağı:** [Zenodo - DOI: 10.5281/zenodo.15045989](https://doi.org/10.5281/zenodo.15045989)
* **Yazılım Referansı:** [Pulseq-qalas GitHub](https://github.com/shoheifujitaSF/Pulseq-qalas)
* **Resmi Atıf:** > Fujita, S. (2025). Vendor-agnostic 3D multiparametric relaxometry improves cross-platform reproducibility. Zenodo. [https://doi.org/10.5281/zenodo.15045989](https://doi.org/10.5281/zenodo.15045989)

*Not: Çalışmada kullanılan ek klinik veriler gizlilik protokolleri gereği paylaşılmamaktadır.*

## 🛠️ Teknik Mimari

### Çok Kanallı (Multi-Channel) Yaklaşım

Model, tek bir kanal yerine QALAS'tan gelen tüm veriyi (5 kanal) girdi olarak alır. Bu sayede T1 ve T2 haritaları arasındaki anatomik korelasyonu kullanarak daha keskin detaylar üretir.

### Model Bileşenleri

1. **Generator:** 3D Residual U-Net. Derinlikli özellik çıkarımı ve `ConvTranspose3d` ile 4x çözünürlük artışı.
2. **Discriminator:** Görüntülerin gerçekçiliğini denetleyen 3D evrişimli ağ.
3. **Loss Fonksiyonları:** * **Pixel Loss (L1):** Genel yapısal doğruluk için ().
* **Adversarial Loss:** Görsel keskinlik ve doku üretimi için.
* **Perceptual Loss:** VGG tabanlı özellik koruma (isteğe bağlı).



## 🚀 Kurulum ve Kullanım

### Gereksinimler

```bash
pip install monai torch nibabel matplotlib torchmetrics

```

### Veri Hazırlama

`.nii` formatındaki QALAS verilerini `LoadImaged` ve `EnsureChannelFirstd` transformları ile 4D (C, H, W, D) yapısına getirerek eğitime dahil edebilirsiniz.

## 📊 Öngörülen Sonuçlar

Model, geleneksel **Bicubic Interpolation** yöntemine kıyasla:

* Daha yüksek **SSIM** (Structural Similarity Index)
* Daha keskin kenar detayları
* Daha düşük hata haritası (Error Map) değerleri sağlamaktadır.

---

**Geliştirici:** Esma Elifsu Cerit

**Danışman:** Dr. Öğr. Üyesi Ramin Abbaszadi

**Akademik Dönem:** 2025 Sonbahar - Bilgisayarlı Görü / Yapay Zeka Mühendisliği
