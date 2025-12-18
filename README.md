

# Multi-Channel 3D Hybrid SRGAN for Quantitative QALAS MRI

Bu proje, **QALAS** (Quantification using an Interleaved Look-Locker Acquisition Sequence) sekansı ile elde edilen nicel MRI haritalarındaki (T1, T2, PD) düşük çözünürlük ve gürültü sorunlarını gidermek amacıyla geliştirilmiştir. Model, çok kanallı (multi-channel) yapısı sayesinde farklı nicel haritalar arasındaki ilişkileri öğrenerek görüntüyü iyileştirir.

## 📌 Proje Özeti

QALAS, tarama süresini kısaltsa da mekânsal çözünürlükten ödün verilmesine ve gürültü artışına neden olur. Bu çalışma, **Residual U-Net tabanlı 3D Hybrid SRGAN** mimarisi kullanarak:

* Mekânsal çözünürlüğü 4 kat artırmayı (Super-Resolution),
* Klinik verilerdeki gürültüyü azaltmayı (Denoising),
* Nicel (kantitatif) değerlerin doğruluğunu korumayı hedefler.


## 🛠️ Teknik Mimari

### Çok Kanallı (Multi-Channel) Yaklaşım

Model, tek bir kanal yerine QALAS'tan gelen tüm veriyi (5 kanal) girdi olarak alır. Bu sayede T1 ve T2 haritaları arasındaki anatomik korelasyonu kullanarak daha keskin detaylar üretir.

### Model Bileşenleri

1. **Generator:** 3D Residual U-Net. Derinlikli özellik çıkarımı ve `ConvTranspose3d` ile 4x çözünürlük artışı.
2. **Discriminator:** Görüntülerin gerçekçiliğini denetleyen 3D evrişimli ağ.
3. **Loss Fonksiyonları:** * **Pixel Loss (L1):** Genel yapısal doğruluk için ().
* **Adversarial Loss:** Görsel keskinlik ve doku üretimi için.
* **Perceptual Loss:** VGG tabanlı özellik koruma (isteğe bağlı).



## 🏗️ Mimari Şema: 3D Multi-Channel Hybrid SRGAN

Model, 5 farklı kanaldan (T1, T2, PD, T1rho, T2rho) gelen verileri tek bir 4D hacim olarak işler. Bu çok kanallı yapı, dokular arasındaki biyolojik ve sayısal korelasyonun öğrenilmesini sağlar.

### **Mimari Bileşenleri:**
* **Giriş (Input):**  boyutlarında Düşük Çözünürlüklü (LR) yamalar.

* **Encoder (Residual U-Net):** MONAI tabanlı 3D Residual Bloklar kullanılarak derin özellik çıkarımı yapılır.

* **Bottleneck:** Verinin en derin ve bağlamsal özelliklerinin yakalandığı katman.

* **Decoder:** `ConvTranspose3d` katmanı ile mekânsal çözünürlük  oranında artırılır.

* **Çıkış (Output):**  boyutlarında Yüksek Çözünürlüklü (SR) yamalar.

---


## 🔄  Hibrit Eğitim Akışı ve Kayıp Fonksiyonları

Eğitim süreci, Üretici (Generator) ve Ayırıcı (Discriminator) ağların birbirini dengelediği bir GAN protokolüne dayanır. Geleneksel yöntemlerin aksine, sadece görsel kaliteyi değil, nicel doğruluğu da optimize eder.

### **Kayıp (Loss) Bileşenleri ve Ağırlıklar:**

Modelin başarısı, aşağıdaki bileşenlerin hibrit bir kombinasyonu ile sağlanır:

* **Pixel Loss ():** Görüntünün temel anatomik yapısını ve parlaklık değerlerini korur ().


* **Adversarial Loss:** Çıktının "gerçekçi" görünmesini sağlayarak bulanıklığı giderir ().


* **Quantitative (Nicel) Loss:** T1 ve T2 haritalarındaki sayısal değerlerin fiziksel doğruluğunu garanti altına alır ().

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

---

## 📊 Veri Kaynağı ve Atıf

Bu projede kullanılan ana veriler, Shohei Fujita tarafından paylaşılan açık kaynaklı QALAS veri setinden elde edilmiştir:

* **Veri Seti:** Shohei Fujita (2025). *Vendor-agnostic 3D multiparametric relaxometry improves cross-platform reproducibility*. Zenodo.


* **Erişim:** [https://doi.org/10.5281/zenodo.15045989](https://doi.org/10.5281/zenodo.15045989)


---

**Geliştirici:** Esma Elifsu Cerit

**Danışman:** Dr. Öğr. Üyesi Ramin Abbaszadi

**Akademik Dönem:** 2025 Sonbahar - Bilgisayarlı Görü / Yapay Zeka Mühendisliği

