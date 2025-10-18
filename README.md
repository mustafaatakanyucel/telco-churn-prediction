# Telekom Müşteri Kaybı (Churn) Tahmin Modeli

## 📋 Proje Özeti

Bu proje, IBM Telco Customer Churn veri setini kullanarak makine öğrenmesi yöntemiyle müşteri kaybını (churn) önceden tahmin eden bir sınıflandırma modeli oluşturmaktadır. Projede **Lojistik Regresyon** ve **Random Forest** modelleri geliştirilmiş, karşılaştırılmış ve değerlendirilmiştir.

### 🎯 Amaç

- Müşteri kaybı riskini erken tespit etmek
- Telekomünikasyon şirketi için proaktif müşteri elde tutma stratejileri geliştirmek
- Veri bilimi iş akışının tam bir örneğini sunmak

---

## 📊 Veri Seti

**Kaynak**: [IBM Telco Customer Churn Dataset](https://raw.githubusercontent.com/IBM/telco-customer-churn-on-icp4d/master/data/Telco-Customer-Churn.csv)

- **Boyut**: 7.043 müşteri × 21 özellik
- **Temizlik Sonrası**: 7.032 müşteri × 20 özellik (11 boş satır kaldırıldı)
- **Hedef Değişken**: Churn (Hayır/Evet)
- **Sınıf Dağılımı**: 
  - No Churn: %73.4
  - Churn: %26.6

### 🔧 Özellikler

- **Demografik**: Yaş, Cinsiyet, Partner, Bağımlılar
- **Hizmet**: İnternet Servisi, Telefon, Ek Hizmetler
- **Sözleşme**: Sözleşme Türü, Ödeme Yöntemi
- **Finansal**: Aylık Ücret, Toplam Ücret, Müşterilik Süresi

---

## 🏗️ Proje Yapısı

```
Proje 7 ana adımdan oluşmaktadır:

1. Ortam Kurulumu            → Kütüphanelerin yüklenmesi
2. Veri Yükleme              → CSV'den veri okunması
3. Keşifsel Veri Analizi     → EDA ve Ön İşleme
4. Özellik Mühendisliği      → Veri Dönüştürme
5. Train-Test Ayrımı         → 80-20 stratified split
6. Model Eğitimi             → LR ve RF modellerinin eğitimi
7. Sonuçlar & Karşılaştırma  → Model performans analizi
```

---

## 📈 Sonuçlar

### Model Performansı

| Metrik | Lojistik Regresyon | Random Forest |
|--------|-------------------|---------------|
| **Accuracy** | 80.45% | 78.75% |
| **Precision** | 64.95% | 63.07% |
| **Recall** | 57.49% | 48.40% |
| **ROC-AUC** | 0.8359 | 0.8137 |

### 🥇 Sonuç

**Lojistik Regresyon modeli tüm metriklerde daha iyi performans göstermiştir.**

- ✓ En yüksek Accuracy: **80.45%**
- ✓ En yüksek Precision: **64.95%** → Churn tahmini doğruluğu iyi
- ✓ En yüksek Recall: **57.49%** → Gerçek churn'ların çoğunluğu tespit ediliyor
- ✓ En yüksek ROC-AUC: **0.8359** → Mükemmel sınıflandırma başarısı

---

## 💻 Kullanım

### Gerekli Kütüphaneler

```bash
pandas>=2.0.0
numpy>=1.24.0
matplotlib>=3.7.0
seaborn>=0.12.0
scikit-learn>=1.3.0
```

### Kurulum

```bash
# Sanal ortam oluştur (opsiyonel)
python -m venv churn_env
churn_env\Scripts\activate   # Windows
source churn_env/bin/activate # macOS/Linux

# Kütüphaneleri yükle
pip install pandas numpy matplotlib seaborn scikit-learn
```

### Çalıştırma

```bash
# Jupyter Notebook'u aç
jupyter notebook churn_prediction.ipynb

# Hücreler sırayla çalıştır (Shift+Enter)
```

---

## 📁 Dosya Yapısı

```
Desktop/
├── churn_prediction.ipynb    # Ana Jupyter Notebook
├── README.md                 # Bu dosya
```

---

## 🔍 Metodoloji

### Veri Ön İşleme

1. **Eksik Veri Temizliği**: TotalCharges sütunundaki boşluk karakterleri kaldırıldı
2. **Hedef Kodlama**: Churn → No=0, Yes=1
3. **Kategorik/Sayısal Ayrımı**: 15 kategorik, 4 sayısal özellik
4. **Normalizasyon**: StandardScaler ile sayısal veriler ölçeklendirildi
5. **One-Hot Encoding**: Kategorik veriler sayısallaştırıldı

### Model Pipeline

```python
Pipeline([
    ('preprocessor', ColumnTransformer([
        ('num', StandardScaler(), numerical_cols),
        ('cat', OneHotEncoder(handle_unknown='ignore'), categorical_cols)
    ])),
    ('classifier', [LogisticRegression | RandomForestClassifier])
])
```

### Validasyon

- **Train-Test Split**: 80% eğitim, 20% test
- **Stratification**: Churn oranı her iki sette korundu (~26.5%)
- **Metrikleri**: Accuracy, Precision, Recall, ROC-AUC, Confusion Matrix

---

## 📊 Önemli Bulgular

### Churn Dağılımı
- Veri dengeli olmayan (26.6% Churn, 73.4% No Churn) → Recall önem kazıyor

### Sayısal Özellikler
- **tenure** (müşterilik süresi): Yeni müşteriler daha fazla churn etme eğilimi gösteriyor
- **TotalCharges**: Yüksek ücretli müşteriler daha az churn etme eğilimi gösteriyor
- **MonthlyCharges**: Orta-yüksek fiyat noktasında churn artıyor

### Kategorik Özellikler
- **Contract Type**: Aylık sözleşmeli müşteriler daha yüksek churn oranına sahip
- **InternetService**: Fiber optik kullananlar daha fazla churn etme eğilimi gösteriyor

---

## 🚀 Sonraki Adımlar (İyileştirmeler)

1. **Özellik Önemi Analizi**
   - Random Forest modelinden feature importance çıkarıp hangi özelliklerin en etkili olduğunu belirlemek

2. **Hiperparametre Optimizasyonu**
   - GridSearchCV/RandomizedSearchCV ile model parametrelerini optimize etmek
   - Örn: RandomForest için n_estimators, max_depth, min_samples_split

3. **Threshold Tuning**
   - ROC eğrisi analizi yaparak karar eşiğini iş gereksinimlerine ayarlama
   - Recall vs Precision trade-off'unu optimize etme

4. **Ensemble Modelleri**
   - Stacking, Voting Classifier gibi ensemble teknikleriyle daha güçlü tahminler
   - XGBoost, LightGBM gibi advanced modeller deneme

5. **Sınıf Dengeleme**
   - SMOTE, Class Weights gibi tekniklerle imbalanced data problemi çözme
   - Recall metriğini iyileştirme

6. **Production Deployment**
   - Modeli API olarak deploy etme (Flask, FastAPI)
   - Yüksek churn riski olan müşterilere automated kampanya başlatma

---

## 📚 Referanslar

- **Dataset**: [IBM Telco Customer Churn - GitHub](https://github.com/IBM/telco-customer-churn-on-icp4d)
- **Scikit-learn**: [sklearn Documentation](https://scikit-learn.org/)
- **Pandas**: [Pandas Documentation](https://pandas.pydata.org/)

---

## 👨‍💻 Geliştirici Notları

### Notebook Yapısı
- **32 hücre**: 16 Markdown (açıklamalar), 16 Python (kod)
- **Tüm hücreler çalışır durumdadır** ✓
- **7 adımlı profesyonel iş akışı** sunar

### Olası Sorunlar ve Çözümleri

| Sorun | Çözüm |
|-------|-------|
| ImportError | `pip install -r requirements.txt` çalıştır |
| İnternet bağlantısı hatası | CSV'yi local olarak kaydet, URL'yi değiştir |
| Memory hatası | Veri seti boyunu azalt veya RAM'i arttır |
| Görselleştirme sorunları | Matplotlib backend'ini değiştir: `%matplotlib inline` |

---

## 📝 Lisans

Bu proje eğitim amaçlı olarak hazırlanmıştır.

---

## ✉️ İletişim

Sorular ve öneriler için lütfen iletişime geçin.

**Son Güncelleme**: 19 Ekim 2025
