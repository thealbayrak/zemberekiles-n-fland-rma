# zemberekiles-n-fland-rma
Türkçenin dilbilgisel yapısını daha iyi anlamak ve kelimelerin farklı çekimlerini kök hallerine indirgeyerek anlamsal tutarlılığı artırmak amacıyla Zemberek kütüphanesi ile lemmatizasyon (kök bulma) adımı text_cleaning_with_zemberek fonksiyonu aracılığıyla entegre edilmiştir. Bu adım sonrası veriler yeniden temizlenerek modellere beslenmiştir.

Nasıl Uygulanır?
 # Zemberek ile Türkçe Tweetlerde Duygu Analizi (Olumlu / Olumsuz / Nötr)

Bu çalışmada Türkçe tweetlerden oluşan bir veri seti kullanılarak **Zemberek ile morfolojik analiz**, ardından **metin vektörleştirme (TF-IDF)** ve son olarak **Naive Bayes, Support Vector Classifier (SVC) ve Logistic Regression** modelleriyle **duygu analizi** yapılmıştır. Aşağıda projenin GitHub’da anlatılabilecek şekilde **aşama aşama** yapılışı yer almaktadır.

---

## 1. Problem Tanımı

Amaç; Türkçe tweet metinlerini işleyerek her bir tweet’i **olumlu**, **olumsuz** veya **nötr** olarak sınıflandırmaktır. Türkçe eklemeli bir dil olduğu için klasik metin ön işleme yöntemleri yeterli değildir. Bu nedenle **Zemberek NLP** kütüphanesi kullanılarak kelimelerin kökleri (lemma) üzerinden analiz yapılmıştır.

---

## 2. Kullanılan Teknolojiler ve Kütüphaneler

* Python
* Pandas, NumPy
* Scikit-learn
* Zemberek NLP (Java tabanlı – Python üzerinden kullanım)
* Regex (re)

---

## 3. Veri Seti

* Veri seti **Türkçe tweetlerden** oluşmaktadır.
* Her satırda:

  * `tweet`: Tweet metni
  * `label`: Duygu etiketi

    * `0`: Olumsuz
    * `1`: Nötr
    * `2`: Olumlu

Veri seti denetimli (supervised) öğrenme için uygundur.

---

## 4. Veri Ön İşleme (Preprocessing)

Türkçe metinler doğrudan modele verilmeden önce temizlenmiştir.

### 4.1 Metin Temizleme

Aşağıdaki işlemler uygulanmıştır:

* Küçük harfe çevirme
* URL, mention (@), hashtag (#) temizleme
* Noktalama işaretlerinin kaldırılması
* Sayıların silinmesi
* Fazla boşlukların temizlenmesi

Amaç, modele gürültüsüz ve tutarlı metin vermektir.

---

## 5. Zemberek ile Morfolojik Analiz

Türkçe eklemeli bir dil olduğu için aynı kelime birçok farklı ek alabilir. Bu durum vektör uzayını gereksiz büyütür.

### 5.1 Zemberek Nedir?

Zemberek;

* Kelime kökü bulma (lemma)
* Ek ayırma
* Yazım denetimi
* Morfolojik çözümleme

işlemlerini yapan açık kaynaklı bir Türkçe NLP kütüphanesidir.

### 5.2 Morfolojik Çözümleme Süreci

* Her tweet kelimelere ayrılır
* Her kelime için Zemberek ile analiz yapılır
* Kelimenin **kökü (lemma)** alınır
* Analiz edilemeyen kelimeler olduğu gibi bırakılır

Örnek:

> "seviyorum" → "sev"

Bu sayede aynı anlamı taşıyan kelimeler tek temsil altında toplanır.

---

## 6. Metinlerin Sayısallaştırılması (Vektörleştirme)

Makine öğrenmesi algoritmaları metni doğrudan anlayamaz. Bu nedenle metinler sayısal vektörlere dönüştürülmüştür.

### 6.1 TF-IDF Nedir?

TF-IDF (Term Frequency – Inverse Document Frequency):

* Bir kelimenin dokümandaki sıklığını
* Tüm veri setindeki ayırt ediciliğini

birlikte hesaplar.

Avantajları:

* Sık ama anlamsız kelimelerin etkisini azaltır
* Ayırt edici kelimelere daha fazla ağırlık verir

### 6.2 TF-IDF Uygulaması

* Zemberek ile kökleri çıkarılmış metinler kullanılır
* `max_features` ile vektör boyutu sınırlandırılır
* `ngram_range` ile tekli veya ikili kelimeler denenebilir

---

## 7. Veri Setinin Bölünmesi

Modelin başarısını ölçebilmek için veri ikiye ayrılmıştır:

* %80 Eğitim (Train)
* %20 Test

Bu işlem `train_test_split` ile yapılmıştır.

---

## 8. Makine Öğrenmesi Modelleri

Projede üç farklı sınıflandırma algoritması kullanılmıştır.

---

### 8.1 Naive Bayes (MultinomialNB)

**Çalışma Mantığı:**

* Olasılık temellidir
* Kelimelerin birbirinden bağımsız olduğunu varsayar

**Avantajları:**

* Hızlıdır
* Metin sınıflandırmada oldukça etkilidir

**Dezavantajları:**

* Bağımsızlık varsayımı her zaman gerçekçi değildir

---

### 8.2 Support Vector Classifier (SVC)

**Çalışma Mantığı:**

* Sınıflar arasına en iyi ayıran hiper-düzlemi bulur
* Yüksek boyutlu uzaylarda etkilidir

**Avantajları:**

* Metin verilerinde yüksek doğruluk

**Dezavantajları:**

* Büyük veri setlerinde yavaştır

---

### 8.3 Logistic Regression

**Çalışma Mantığı:**

* Olasılık hesaplayarak sınıflandırma yapar
* Çok sınıflı problemlere uygundur

**Avantajları:**

* Yorumlanabilir
* Dengeli veri setlerinde başarılı

---

## 9. Model Eğitimi

Her model için:

* Eğitim verisi ile model eğitilmiştir
* Test verisi ile tahmin yapılmıştır

Aynı TF-IDF vektörleri tüm modellerde kullanılarak adil bir karşılaştırma sağlanmıştır.

---

## 10. Model Değerlendirme

Başarım ölçütleri:

* Accuracy (Doğruluk)
* Precision
* Recall
* F1-Score
* Confusion Matrix

Bu metrikler sayesinde hangi modelin olumlu, olumsuz ve nötr sınıfları ne kadar iyi ayırdığı analiz edilmiştir.

---

## 11. Sonuç ve Karşılaştırma

* **Naive Bayes**: Hızlı ve baseline model
* **Logistic Regression**: Dengeli ve yorumlanabilir sonuçlar
* **SVC**: En yüksek doğruluk, ancak daha maliyetli

Zemberek kullanımı sayesinde Türkçe ek yapısından kaynaklı veri dağınıklığı azaltılmış ve modellerin başarımı artmıştır.

---

## 12. Projenin Katkısı

* Türkçe NLP için Zemberek kullanımına iyi bir örnek
* Duygu analizi pipeline’ının uçtan uca gösterimi
* GitHub üzerinde akademik veya bitirme projesi olarak kullanılabilir

---

## 13. Geliştirme Önerileri

* Stopword listesinin Zemberek uyumlu hale getirilmesi
* Word2Vec / FastText kullanımı
* Derin öğrenme (LSTM, BERTurk)
* Hiperparametre optimizasyonu

-
