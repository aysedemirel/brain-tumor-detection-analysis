# Brain Tumor Detection Analysis

## Projenin Amacı

Bu proje, beyin MR görüntüleri üzerinden tümör tespiti yapmak amacıyla geliştirilmiştir. Amaç, sınırlı bir veri seti kullanarak farklı ön eğitimli konvolüsyonel sinir ağı (CNN) modellerinin performansını incelemek, fine-tuning yöntemleri ile sınıflandırma doğruluğunu artırmak ve küçük veri setlerinde etkin modelleri belirlemektir.

Proje kapsamında [brain-mri-images-for-brain-tumor-detection](https://www.kaggle.com/datasets/navoneel/brain-mri-images-for-brain-tumor-detection) veri seti kullanılmıştır. Orijinal veri setinde 253 MR görüntüsü bulunmakta olup, data augmentation ile 1.737 eğitim, 450 doğrulama ve 90 test görüntüsü elde edilmiştir. Sınıflar “Tümör”(YES) ve “Tümör Yok”(NO) şeklindedir.

---

## Kullanılan Yöntemler

Bu projede, **TensorFlow ve Keras** kütüphaneleri kullanılarak CNN modelleri oluşturulmuş ve ön eğitimli modeller üzerinde fine-tuning uygulanmıştır. Projede ayrıca veri artırma (data augmentation) ve EarlyStopping gibi yöntemler uygulanmıştır.

Projede iki ana yaklaşım analiz edilmiştir: sıfırdan oluşturulan CNN modeli ve transfer learning ile kullanılan ön eğitimli CNN modelleri.  

Özel CNN modelinde overfitting hızlı bir şekilde gözlemlendiği için, hiperparametre optimizasyonu Bayesian Optimization ve Keras Tuner ile uygulanmıştır. Modelde kullanılan bileşenler:

- Convolutional Katmanlar
- Pooling Katmanları
- Dropout
- Dense Katmanlar
- Aktivasyon Fonksiyonları (ReLU, Sigmoid vb.)

Overfitting durumlarını önlemek için eğitim sırasında **EarlyStopping** kullanılarak doğrulama doğruluğu izlenmiştir.

Veri seti ikili sınıflı olduğundan, kayıp fonksiyonu olarak **Binary Crossentropy** ve son katmanın aktivasyon fonksiyonu olarak **Sigmoid** seçilmiştir.

### Data Augmentation

Ana veri setinin sınırlı olması nedeniyle, modelin overfitting yapmasını önlemek ve çeşitlendirilmiş eğitim verisi sağlamak amacıyla **data augmentation** uygulanmıştır. Özellikle tümör verisinin merkezde kalmasını sağlamak için öncelikle kırpma işlemi yapılmış, ardından aşağıdaki yöntemler uygulanmıştır. Tüm yöntemler birleştirilerek eğitim seti oluşturulmuştur. Data augmentation yapılmadan, ilk epochlardan itibaren overfitting gözlemlenirken, augmentation sonrası bu durum önemli ölçüde azalmıştır.

Kullanılan tüm yöntemler:
- Kırpma (Cropping)  
- Yakınlaştırma (Zoom)  
- Döndürme (Rotation)  
- Kaydırma (Shift)  
- Elastik deformasyon (Elastic deformation)  
- Gürültü eklenmesi (Noise injection)  
- Parlaklık ve kontrast ayarlamaları (Brightness/Contrast adjustment)  
- Dikey çevirme (Vertical flip)


### Transfer Learning

Altı farklı ön eğitimli CNN modeli, küçük veri setinde performanslarını artırmak amacıyla transfer learning ile değerlendirilmiştir:

- EfficientNetB0
- MobileNetV2
- DenseNet169
- ResNet50
- VGG16
- InceptionV3

Son 20 katman fine-tuning ile eğitilmiş ve performanslarında gözle görülür iyileşmeler elde edilmiştir. Transfer learning, sınırlı veri setlerinde oldukça etkili olmuştur.

---

## Elde Edilen Sonuçlar


| Model          | Eğitim Doğruluğu | Doğrulama Doğruluğu |
|----------------|-----------------|-------------------|
| EfficientNetB0 |                 |                   |
| MobileNetV2    | ~96%            | ~93%              |
| DenseNet169    | ~93%            | ~87%              |
| ResNet50       | ~98%            | ~91%              |
| VGG16          |                 |                   |
| InceptionV3    |                 |                   |
| CustomCNN      |                 |                   |
| CustomCNN_Bayesian_Turner |      |                   |


Transfer learning ile elde edilen modeller en başarılı sonuçları göstermiştir. Veri sayısı yeterli olmadığında, transfer learning ile yüksek performans elde edilebilmektedir. Özel CNN modellerde hiperparametre optimizasyonu yapılsa da, en yüksek doğruluk transfer learning ile sağlanmıştır.

---



## Sonuç ve Gelecek Çalışmalar



Bu çalışma, sınırlı veri ile ön eğitimli CNN modellerinin etkinliğini göstermektedir. Gelecekte:

- Daha büyük ve çeşitlendirilmiş veri setleri ile performans artırılabilir
- Gerçek zamanlı MR görüntü işleme entegrasyonu eklenebilir
- Başarılı transfer learning modellerinde ek optimizasyonlar yapılabilir


## Linkler

- [Veri seti: brain-mri-images-for-brain-tumor-detection](https://www.kaggle.com/datasets/navoneel/brain-mri-images-for-brain-tumor-detection)  
- [Kaggle Notebook: Beyin Tümörü Tespiti](https://www.kaggle.com/code/aysedemirel/brain-tumor-detection)
