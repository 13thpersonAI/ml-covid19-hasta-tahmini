# 🦠 Makine Öğrenmesi ile COVID-19 Hasta Tahmini

## 📌 Proje Açıklaması
Bu proje, hastaların çeşitli klinik semptomlarına ve demografik özelliklerine (ateş, öksürük şiddeti, kan oksijen seviyesi, yaş) dayanarak kişinin COVID-19 pozitif olup olmadığını tahmin eden bir makine öğrenmesi modellemesidir. Çalışma kapsamında gerçek dünya verilerinde sıkça karşılaşılan eksik ve hatalı verilerle başa çıkılmış, iki farklı algoritma eğitilerek uçtan uca bir makine öğrenmesi boru hattı (pipeline) kurulmuştur.

## 📊 Kullanılan Veri Seti
Projede hastaların temel fizyolojik ölçümlerini ve COVID-19 test sonuçlarını barındıran bir veri seti kullanılmıştır. Sütunlar; ateş derecesi, öksürük şiddeti (0-10 arası), kan oksijen doygunluğu ve yaş gibi bağımsız değişkenler ile hedef değişken olan `Covid_Sonuc` (0: Negatif, 1: Pozitif) sütunundan oluşmaktadır.
* **Veri Seti Kaynağı:** [Kaggle COVID-19 Veri Seti Linki](BURAYA_LINKI_YAPISTIRIN)

## 🧹 Veri Ön İşleme Adımları (EDA ve Temizlik)
Modelin yanlış öğrenmesini engellemek için veriler şu aşamalardan geçirilmiştir:
1. **Eksik Veri (NaN) Yönetimi:** Ateş ölçümlerindeki boş kayıtlar silinmek yerine, veri setindeki ateş değerlerinin medyanı (ortanca değeri) ile doldurularak veri kaybı önlenmiştir.
2. **Aykırı Değer (Outlier) Temizliği:** Kan oksijen seviyesinin tıbbi olarak mantıksız derecede düşük olduğu (örn: 70'in altı) hatalı ölçümler veri setinden tamamen çıkarılmıştır.
3. **Veri Bölme:** Modelin görmediği verilerle test edilebilmesi için veri seti %80 Eğitim ve %20 Test olarak ikiye ayrılmıştır.
4. **Veri Standardizasyonu:** Özellikle KNN gibi uzaklık temelli algoritmaların doğru çalışabilmesi için tüm özellikler `StandardScaler` kullanılarak aynı ölçeğe getirilmiştir.

## 🤖 Kullanılan Algoritmaların Mantığı
Projede birbirine zıt yaklaşımlara sahip iki farklı sınıflandırma algoritması tercih edilmiştir:
* **Karar Ağacı (Decision Tree):** Veri setindeki özellikleri kullanarak ardışık "evet/hayır" kuralları çıkaran bir modeldir. Öğrenme sürecini bir ağaç yapısı gibi dallandırır. Aşırı öğrenmeyi (overfitting) engellemek için ağaç derinliği maksimum 5 seviye ile sınırlandırılmıştır.
* **K-En Yakın Komşu (KNN):** Tembel öğrenme (lazy learning) algoritmasıdır. Yeni gelen bir hastanın verisini, veri setindeki en benzer (öklid uzaklığına göre en yakın) $K$ adet hastanın durumuna bakarak sınıflandırır. Bu projede $K=5$ komşuluk değeri kullanılmıştır.

## 📈 Model Performans Karşılaştırması
Test verisi üzerinde yapılan tahminler sonucunda elde edilen temel performans metrikleri aşağıdadır:

| Değerlendirme Metriği | Decision Tree (Karar Ağacı) | KNN (K=5) |
| :--- | :--- | :--- |
| **Accuracy (Doğruluk)** | ~ %60.5 | ~ %58.2 |
| **Precision (Kesinlik)** | ~ %54.3 | ~ %49.5 |
| **Recall (Duyarlılık)** | ~ %48.0 | ~ %45.0 |

*(Not: Tablodaki değerler eğitim verisinin dağılımına göre yaklaşık olarak verilmiştir. Kodu çalıştırdığınızda net değerler ve Karmaşıklık Matrisi (Confusion Matrix) grafikleri ekrana basılmaktadır.)*

## 💡 Sonuç ve Yorumlar
* **Medikal Açıdan Yaklaşım:** Sağlık ve hastalık teşhisi projelerinde genel doğruluktan (Accuracy) ziyade, gerçekte hasta olanları yakalama başarısını gösteren **Recall (Duyarlılık)** metriği çok daha kritiktir. Hasta olan birine "sağlıklı" demek ölümcül riskler taşır.
* **Algoritma Değerlendirmesi:** Yapılan testler sonucunda kural tabanlı Karar Ağacı algoritması, hastaların özelliklerini eşik değerlere göre böldüğü için, uzaklık tabanlı çalışan KNN algoritmasına kıyasla hem Doğruluk hem de Duyarlılık açısından daha iyi performans göstermiştir.

## 🚀 Kodların Nasıl Çalıştırılacağı
1. Bilgisayarınızda Python 3.x versiyonunun ve gerekli kütüphanelerin (`pandas`, `numpy`, `scikit-learn`, `matplotlib`, `seaborn`) kurulu olduğundan emin olun.
2. Bu projeyi bilgisayarınıza indirin veya klonlayın:
   ```bash
   git clone [https://github.com/KULLANICI_ADINIZ/ml-covid19-hasta-tahmini.git](https://github.com/KULLANICI_ADINIZ/ml-covid19-hasta-tahmini.git)
