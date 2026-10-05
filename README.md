# 🎬 TMDB Film Başarı Tahmini & Kalite Modellemesi
> **Özellik Mühendisliği (Oyuncu/Yönetmen Geçmişi) & HistGradientBoosting ile Film Kalite ve Başarı Tahmini**

[![Python 3.10+](https://img.shields.io/badge/python-3.10+-blue.svg)](https://www.python.org/downloads/)
[![scikit-learn](https://img.shields.io/badge/scikit--learn-1.3+-orange.svg)](https://scikit-learn.org/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

Bu proje, **The Movie Database (TMDB)** platformundaki 1990–2026 yılları arasında yayımlanmış **27.027 film** üzerinde, sinema sektörünün en temel sorularından birini makine öğrenmesiyle yanıtlamak amacıyla geliştirilmiştir: *Bir filmin başarısını ve kalitesini asıl belirleyen bütçe ve tür müdür, yoksa kadro ve yönetmen geçmişi mi?*

---

## 📌 Öne Çıkan Sonuçlar & Bulgular

- **%83.3 Test Doğruluğu:** `HistGradientBoostingClassifier` modeli, 5 katlı çapraz doğrulama (5-Fold Stratified CV) ve `RandomizedSearchCV` hiperparametre optimizasyonu ile test setinde **%83.3 doğruluk** ve başarısız filmlerde **%85 F1 skoru** elde etti.
- **Kadro vs. Tür Karşılaştırması (%56'ya %8.3):**
  - Özellik önem analizi (Feature Importance), karar sürecinin **%56'sını** oyuncu ve yönetmen geçmişinin (`cast_avg_rating` %31, `director_avg_rating` %25) oluşturduğunu kanıtladı.
  - Buna karşılık, analiz edilen 19 farklı film türünün toplam karar ağırlığı yalnızca **%8.3**'te kaldı.
- **Keskin Karar Sınırı Stratejisi:** 3'lü sınıflandırmadaki (Kötü/Ortalama/İyi) belirsizlik bölgesi analiz edilmiş; "ortalama" filmlerin oluşturduğu yapay gürültü yerine net sınırlarla ikili sınıflandırmaya geçilerek model performansı %54'ten **%83.3'e** yükseltilmiştir.
- **Veri Sızıntısı (Data Leakage) Koruması:** Oyuncu ve yönetmen geçmiş puanları yalnızca eğitim seti üzerinden hesaplanıp test setine aktarılmış; geleceğe dair bilgi sızıntısı tamamen engellenmiştir.

---

## 🏗️ Proje Mimarisi

```
Film-Tahmini/
├── data/
│   └── tmdb_movies_1990_2026.csv        # 27.000+ satırlık ham TMDB veri seti
├── docs/
│   ├── TMDB_Film_Basari_Tahmini_Raporu.pdf  # Akademik formatlı detaylı araştırma raporu
│   ├── TMDB_Film_Basari_Tahmini_Raporu.docx # Rapor kaynak dosyası
│   └── TMDB_Film_Basari_Sunum.pptx      # Proje sunum slaytları
├── models/
│   └── film_model.pkl                   # Eğitilmiş ve serileştirilmiş nihai model
├── notebooks/
│   └── Moovie_Quality_Control.ipynb     # Veri analizi, özellik mühendisliği & modelleme defteri
├── .gitignore
├── README.md
└── requirements.txt
```

---

## ⚙️ Özellik Mühendisliği (Feature Engineering)

Modelin başarısındaki en kritik yenilik, kadro geçmişini matematiksel sinyallere dönüştüren 4 yeni özelliktir:

| Özellik Adı | Açıklama | Karar Ağırlığı |
| :--- | :--- | :---: |
| `cast_avg_rating` | Filmdeki ilk 5 başrol oyuncusunun geçmiş film puan ortalaması | **%31.0** |
| `director_avg_rating`| Yönetmenin önceki tüm filmlerinin TMDB puan ortalaması | **%25.0** |
| `cast_experience` | Oyuncuların yer aldığı toplam film sayısı (sektörel deneyim) | Destekleyici |
| `director_experience`| Yönetmenin geçmiş proje adedi (yönetmenlik deneyimi) | Destekleyici |
| *Temel Özellikler* | Süre, bütçe, hasılat, popülarite, vizyon yılı, 19 One-Hot tür | %44.0 |

---

## 🚀 Kurulum & Çalıştırma

```bash
# 1. Depoyu klonlayın
git clone https://github.com/emirhanoguz0/tmdb-movie-success-predictor.git
cd tmdb-movie-success-predictor

# 2. Bağımlılıkları yükleyin
pip install -r requirements.txt

# 3. Jupyter Notebook'u başlatın
jupyter notebook notebooks/Moovie_Quality_Control.ipynb
```

---

## 📊 Eğitilmiş Modeli Yükleme & Tahmin

```python
import joblib

# Kayıtlı modeli yükle
model = joblib.load('models/film_model.pkl')

# Yeni bir film için başarı tahmini (1: Başarılı, 0: Başarısız)
# prediction = model.predict(X_new)
```

---

## 👤 Geliştirici

**Mehmet Emirhan Oğuz**
- LinkedIn: [linkedin.com/in/emirhanoguz0](https://linkedin.com/in/emirhanoguz0)
- GitHub: [github.com/emirhanoguz0](https://github.com/emirhanoguz0)
- Portfolyo: [emirhanoguz0.github.io/portfolio](https://emirhanoguz0.github.io/portfolio)

---

## 📄 Lisans
Bu proje [MIT Lisansı](LICENSE) kapsamında sunulmaktadır.
