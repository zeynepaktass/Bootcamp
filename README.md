Makine Öğrenimi Bootcamp - Dolandırıcılık Tespiti ve Kümeleme Projesi

Overview
Bu proje, finansal bir veri kümesinde dolandırıcılık faaliyetlerini tespit etmek amacıyla çeşitli makine öğrenimi modellerinin oluşturulmasını ve değerlendirilmesini içermektedir. Projede ayrıca veri kümesi üzerinde kümeleme algoritmaları uygulanarak örüntülerin keşfedilmesi hedeflenmiştir. Hedef, dolandırıcılık faaliyetlerini etkili bir şekilde ayırt edebilen güçlü bir model geliştirmek ve veri kümelerini analiz ederek alt gruplar oluşturmaktır.

Veri Kümesi
Bu projede kullanılan veri kümesi aşağıdaki sütunlardan oluşmaktadır:

step: Simülasyondaki zaman adımı (tam sayı).
type: İşlem türü (CASH_IN, CASH_OUT, TRANSFER, vb.).
amount: İşlemin tutarı.
nameOrig: İşlemi başlatan hesabın kimliği.
newbalanceOrig: İşlemden sonra orijinal hesabın yeni bakiyesi.
oldbalanceDest: İşlemden önce hedef hesabın eski bakiyesi.
newbalanceDest: İşlemden sonra hedef hesabın yeni bakiyesi.
isFraud: İşlemin dolandırıcılık olup olmadığını belirten ikili hedef değişken.

Veri Ön İşleme
Modelleri eğitmeden önce birkaç ön işleme adımı uygulanmıştır:
Özellik Seçimi: İşlem türleri ve tutarları gibi önemli özellikler seçilmiştir.
Eğitim-Test Ayrımı: Veri kümesi, %80 eğitim ve %20 test olarak bölünmüştür. Bu sayede modellerin performansı doğru bir şekilde değerlendirilebilmiştir.
Normalizasyon: Özellikler MinMaxScaler ile ölçeklendirilmiştir. Bu, sayısal özelliklerin aynı aralığa getirilmesini sağlar ve modellerin daha hızlı öğrenmesine yardımcı olur.
SMOTE: Smote algoritması kullanılarak sınıf örneği az olan verileri sentetik olarak çoğaltarak imbalanced data set balanced data sete dönüştürmek hedeflenmiştir.

Modeller
Dolandırıcılık tespiti için aşağıdaki makine öğrenimi modelleri eğitilmiş ve değerlendirilmiştir:

(Supervised Models):
Lojistik Regresyon
Karar Ağacı
K-En Yakın Komşu (KNN)
Destek Vektör Makineleri (SVM)
Rastgele Orman (Random Forest)

Rastgele Orman Modeli Hiperparametre Optimizasyonu
En iyi performansı veren Rastgele Orman modeli için aşağıdaki hiperparametreler optimize edilmiştir:

max_depth: 20
min_samples_leaf: 1
min_samples_split: 2
n_estimators: 100

En İyi Eşik Değerini (Threshold) Bulma ve Uygulama
Modelin varsayılan karar eşik değeri genellikle 0.5 olarak kabul edilir. Ancak, dolandırıcılık tespiti gibi dengesiz veri kümelerinde bu eşik değeri optimal sonuçlar vermeyebilir. F1 skoru, hem kesinlik (precision) hem de duyarlılığı (recall) dengelemeyi amaçladığı için, model performansını bu skora göre optimize etmek daha etkili olabilir. Aşağıdaki adımlar, en iyi eşik değerini bulup uygulamak için gerçekleştirilmiştir:
Tahmin Olasılıklarının Alınması: Modelin test verisindeki örnekler için dolandırıcılık olasılıklarını tahmin ettik.
Precision-Recall Eğrisi: Farklı eşik değerlerine göre kesinlik ve duyarlılık değerleri hesaplandı.
En İyi Eşiğin Belirlenmesi (F1 Skoruna Göre): F1 skoru, kesinlik ve duyarlılığı dengelediği için, her eşik değeri için F1 skorları hesaplandı. En yüksek F1 skorunu veren eşik değeri seçildi.
Yeni Threshold ile Tahmin Yapma: Yeni eşik değeri kullanılarak test verisinde tahminler yapıldı. Olasılık değeri yeni eşikten büyük veya eşit olan örnekler dolandırıcılık olarak sınıflandırıldı.
Yeni Threshold ile Performans Değerlendirmesi: Yeni eşik değeriyle modelin performansı ölçüldü ve kesinlik, duyarlılık ve F1 skoru tekrar hesaplandı.
Bu süreç, modelin varsayılan eşik değeri yerine daha optimize edilmiş bir eşik değeri kullanarak performansını artırmayı amaçlamaktadır. Bu sayede dolandırıcılık tespiti gibi dengesiz veri setlerinde daha dengeli sonuçlar elde edilebilir.

Modelin Son Hali ve Performans Sonuçları
En iyi eşik değeri (0.6600) kullanılarak modelin son performansı aşağıdaki gibidir:

Doğruluk (Accuracy): 0.9980
Kesinlik (Precision): 0.7826
Duyarlılık (Recall): 0.6667
F1 Skoru: 0.7200
Bu yeni eşik değeri ile model, dolandırıcılık tespiti sırasında daha iyi bir performans göstermiş, genel doğruluk yüksek tutulurken dolandırıcılık vakalarını tespit etme yeteneği artmıştır.

Kümeleme Algoritmaları
Dolandırıcılık tespiti dışında, veri kümesini segmentlere ayırmak ve içsel desenleri incelemek amacıyla aşağıdaki kümeleme algoritmaları uygulanmıştır:

K-Means Kümeleme:

KMeans algoritması 2 küme ile (n_clusters=2) uygulanmıştır.
Sonuçlar, iki farklı küme içinde segmentlere ayrılmış veriler göstermiştir.
Apriori Algoritması (İlişkilendirme Kuralı Madenciliği):

Apriori algoritması, işlem türleri arasında ilişki kuralları bulmak için kullanılmıştır. Ancak belirlenen destek eşiği ile anlamlı bir ilişki kuralı bulunamamıştır.
Hiyerarşik Kümeleme:

Agglomeratif Kümeleme algoritması ile 2 küme (n_clusters=2) kullanılarak veri hiyerarşik gruplara ayrılmıştır.
DBSCAN (Gürültü İçeren Yoğunluk Tabanlı Kümeleme):

DBSCAN algoritması, kümeleme ve gürültü noktalarını (aykırı değerler) tespit etmek amacıyla uygulanmıştır.
Bazı veri noktaları gürültü olarak tanımlanmış (label = -1) ve 6 farklı küme tespit edilmiştir.
Gauss Karışım Modelleri (GMM):

Gaussian Mixture Model, iki farklı Gaussian dağılımdan oluşan verileri modellemek için kullanılmıştır.
Kümeleme Sonuçlarının Karşılaştırması:
K-Means Kümeleme: Her bir kümede [4331, 4158] örnek bulunmaktadır.
Hiyerarşik Kümeleme: Her bir kümede [4169, 4320] örnek bulunmaktadır.
DBSCAN: Birden fazla küme tespit edilmiştir ve bazı veri noktaları gürültü olarak belirlenmiştir.
Gaussian Mixture Models: K-Means ile benzer sonuçlar elde edilmiştir, kümeler [4331, 4158] örnek içermektedir.
