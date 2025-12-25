# zemberekiles-n-fland-rma
Türkçenin dilbilgisel yapısını daha iyi anlamak ve kelimelerin farklı çekimlerini kök hallerine indirgeyerek anlamsal tutarlılığı artırmak amacıyla Zemberek kütüphanesi ile lemmatizasyon (kök bulma) adımı text_cleaning_with_zemberek fonksiyonu aracılığıyla entegre edilmiştir. Bu adım sonrası veriler yeniden temizlenerek modellere beslenmiştir.

Ön işlenmiş metin verileri, makine öğrenimi modellerinin anlayabileceği sayısal bir formata dönüştürülmüştür. Bu amaçla TF-IDF (Term Frequency-Inverse Document Frequency) vektörleştirme yöntemi kullanılmıştır. TfidfVectorizer ile en sık geçen 5000 özellik (kelime veya ikili kelime grubu - bigram) çıkarılmıştır

Üç farklı makine öğrenimi modeli (Lojistik Regresyon, LinearSVC ve Naive Bayes) eğitilmiş ve performansları değerlendirilmiştir. Modellerin performansı hem başlangıçtaki ön işleme adımlarıyla hem de Zemberek lemmatizasyonu sonrası elde edilen verilerle ayrı ayrı incelenmiştir.

Zemberek lemmatizasyonunun doğrudan bir Accuracy artışı sağlamadığı görülmüştür; model Accuracy değerleri Zemberek öncesiyle aynı kalmıştır. Ancak, lemmatizasyon metnin anlamsal temsili üzerinde bir iyileşme sağlayabilir ve bazı spesifik sınıflar için Recall veya Precision değerlerini etkileyebilir. Örneğin, Naive Bayes modelinde 'olumsuz' sınıf için çok yüksek bir Recall (0.85) elde edilmesi dikkat çekicidir, bu da modelin olumsuz duyguya sahip tweet'leri kaçırma oranının oldukça düşük olduğunu gösterir.
Genel olarak, Lojistik Regresyon ve Naive Bayes modelleri yaklaşık %67.54'lük bir doğruluk oranı ile en iyi performansı sergilemişlerdir. LinearSVC modeli ise %66.33 ile biraz daha düşük kalmıştır.

Duygu Dağılımı Analizi
Eğitim ve test veri setlerindeki duygu etiketlerinin dağılımı aşağıdaki gibidir:
•	Eğitim Veri Seti Duygu Dağılımı:
o	Olumsuz: 5511
o	Nötr: 4658
o	Olumlu: 3663
•	Test Veri Seti Duygu Dağılımı:
o	Olumsuz: 1377
o	Nötr: 1164
o	Olumlu: 916
Her iki veri setinde de 'olumsuz' duygu en yaygın, 'olumlu' duygu ise en az yaygın olanıdır. Bu durum, veri setinde bir sınıf dengesizliği olduğunu göstermektedir. Bu dengesizlik, modellerin azınlık sınıflarını (özellikle 'olumlu') öğrenmesini zorlaştırabilir.

Temel Bulgular:
•	Model Performansı: Lojistik Regresyon ve Naive Bayes modelleri genel doğruluk açısından LinearSVC'ye göre daha iyi veya eşdeğer performans sergilemiştir.
•	Zemberek Etkisi: Zemberek lemmatizasyonu, genel Accuracy'yi değiştirmemiş olsa da, metin temsili kalitesini artırma ve sınıf bazlı performans metrikleri üzerinde olası ince ayarlamalar yapma potansiyeline sahiptir.
•	Veri Dengesizliği: Veri setindeki duygu sınıfları arasında önemli bir dengesizlik bulunmaktadır; bu durum modelin özellikle azınlık sınıflarındaki performansını etkilemektedir.
