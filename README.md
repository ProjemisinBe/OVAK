# OVAK
OVAK-Temel Proje Tanımlaması:
---
📁 proje-adi/
├── 📁 veri_toplayici/                # Veri kaynaklarından veri çekmek için sınıflar içerir
│   ├── veri_toplayici.py             # API, CSV ve veritabanı bağlantılarını yöneten sınıflar
├── 📁 veri_temizleyici/              # Verilerin temizlenmesi ve ön işlenmesi ile ilgili işlemler
│   ├── veri_temizleyici.py           # Eksik verilerin silinmesi, normalizasyon işlemleri vb.
├── 📁 kumelestirme/                  # Farklı kümeleme algoritmalarını barındıran klasör
│   ├── kumelestirme_algoritmasi.py   # Soyut kümeleme algoritma sınıfı
│   ├── kmeans_kume.py                # K-Means kümeleme algoritmasını uygulayan sınıf
│   ├── dbscan_kume.py                # DBSCAN kümeleme algoritmasını uygulayan sınıf
├── 📁 model_optimizasyonu/           # Kümeleme algoritmalarının optimizasyonu için sınıflar
│   ├── model_optimizasyonu.py        # Elbow yöntemi, Silhouette skoru gibi optimizasyon teknikleri
├── 📁 kume_analizi/                  # Kümeleme sonuçlarının analiz edilmesi için araçlar
│   ├── kume_analizi.py               # Kümeler arası mesafe analizi, küme içi varyasyon vb.
├── 📁 rapor_olusturucu/              # Raporlama sistemini barındıran klasör
│   ├── rapor_olusturucu.py           # PDF ve Excel formatında rapor oluşturma
├── requirements.txt                  # Proje için gerekli Python bağımlılıklarını tanımlar
├── README.md                         # Proje hakkında genel bilgiler ve kullanım talimatları
---

---
## Proje Özeti

Otonom Veri Analizi ve Karar Verme Platformu,("OVAK") işletmelerin veya araştırmacıların büyük veri kümeleri üzerinde kümeleme algoritmalarını uygulamasına ve sonuçlarını görselleştirerek anlamlı içgörüler elde etmesine olanak tanır. Proje, kümeleme algoritmaları (K-Means, DBSCAN, Hierarchical Clustering) ve çeşitli optimizasyon yöntemleri ile veri setlerini analiz eder.# Proje temelinde kullanılacak algoritmalar en verimli sonucu veren algoritma yönteminin Optuna ve optimizasyon teknikleri ile belirlenmesini sağlancaktır.

## Özellikler

- **Modüler Yapı**: Veri toplama, temizleme, kümeleme ve raporlama süreçleri modüler olarak tasarlanmıştır.
- **Çoklu Kümeleme Algoritmaları**: K-Means, DBSCAN ve Hiyerarşik Kümeleme desteklenir.
- **Optimizasyon Teknikleri**: Elbow yöntemi, Silhouette skoru, genetik algoritmalar gibi yöntemler kullanılarak model performansı artırılır.
- **Otomatik Raporlama**: Kümeleme sonuçları PDF ve Excel formatlarında raporlanır ve periyodik olarak iletilir.
- **Gerçek Zamanlı Veri Akışı**: Veriler gerçek zamanlı olarak kümeleme algoritmalarına beslenebilir.

---
---

### **1. Veri Toplama ve Depolama**
Bu adımın amacı, veriyi çeşitli kaynaklardan toplayıp, işleyip, depolamaktır. Her bileşeni ayrı sınıflar ve modüller halinde düzenlemek, kodun yeniden kullanılabilirliğini ve sürdürülebilirliğini artıracaktır.
 
#### **1.1. Veri Kaynaklarına Bağlantı**
   - **Sınıf: `DataFetcher`**
     - **Metotlar**:
       - `fetch_from_api()`: API'den veri çekmek için bir fonksiyon.
       - `load_from_csv()`: CSV dosyalarını yüklemek için bir fonksiyon.
       - `load_from_database()`: SQL veya NoSQL veritabanlarından veri çekmek için fonksiyonlar.
     - **Modülerlik**: Her veri kaynağı ayrı bir metot ile ele alınacak. Yeni veri kaynakları eklemek gerekirse, `DataFetcher` sınıfı genişletilebilir.
   
   - **Bağımsız Geliştirme**: Bu sınıf, verilerin toplandığı yer olduğu için diğer modüller bu sınıfı sadece veri sağlama açısından kullanacak.

#### **1.2. Veri Temizleme ve Ön İşleme**
   - **Sınıf: `DataCleaner`**
     - **Metotlar**:
       - `remove_missing_values()`: Eksik verilerin temizlenmesi.
       - `remove_duplicates()`: Tekrar eden satırların tespiti ve silinmesi.
       - `normalize_data()`: Verileri normalleştirme ve ölçekleme.
       - `reduce_dimensions()`: PCA veya t-SNE kullanarak boyut indirgeme.
     - **Modülerlik**: Her işlem ayrı bir metot olarak düzenlenir, böylece gerekli olan işlemler bağımsız olarak çağrılabilir.
   
   - **Bağımsız Geliştirme**: `DataCleaner` sınıfı, ham veriyi işleyip temizler ve `DataFetcher` sınıfından bağımsızdır.

#### **1.3. Veri Depolama**
   - **Sınıf: `DataSaver`**
     - **Metotlar**:
       - `save_to_sql()`: SQL veritabanına kaydetmek.
       - `save_to_nosql()`: NoSQL (örneğin, MongoDB) veritabanına kaydetmek.
       - `save_to_csv()`: CSV dosyasına veri kaydetmek.
     - **Modülerlik**: Verilerin hangi formatta kaydedileceği modüler olarak ele alınır, bu da esnekliği artırır.

   - **Bağımsız Geliştirme**: Bu sınıf, veriyi depolama görevine odaklanır ve diğer veri işleme sınıflarından bağımsız çalışabilir.

---

### **2. Kümeleme Yöntemleri ve Model Geliştirme**
Kümeleme algoritmaları, veri analizinde önemli bir yer tutar. Bu bölümde, her algoritmanın kendi sınıfı ve fonksiyonlarıyla bağımsız olarak geliştirileceği modüler bir yapı tasarlanır.

#### **2.1. Kümeleme Algoritmalarının Seçimi**
   - **Sınıf: `ClusteringAlgorithm` (Abstract Base Class)**
     - **Alt sınıflar**:
       - `KMeansCluster()`: K-Means algoritmasını uygulayan sınıf.
       - `DBSCANCluster()`: DBSCAN algoritmasını uygulayan sınıf.
       - `HierarchicalCluster()`: Hiyerarşik kümeleme algoritmasını uygulayan sınıf.
     - **Metotlar**:
       - `fit()`: Algoritmanın verilere uygulanması.
       - `predict()`: Kümelerin tahmin edilmesi.
     - **Modülerlik**: Farklı kümeleme algoritmaları alt sınıflar olarak uygulanacak. Böylece her algoritma kendi fonksiyonlarıyla bağımsız çalışabilir.

   - **Bağımsız Geliştirme**: Her algoritma sınıfı bağımsız olarak geliştirilebilir. Yeni bir algoritma eklemek için yalnızca bir alt sınıf eklemek yeterlidir.

#### **2.2. Model Eğitimi ve Optimizasyonu**
   - **Sınıf: `ModelOptimizer`**
     - **Metotlar**:
       - `apply_elbow_method()`: Elbow yöntemini uygulayarak en uygun küme sayısını bulma.
       - `apply_silhouette_score()`: Silhouette skorunu kullanarak kümeleri değerlendirme.
       - `apply_genetic_algorithm()`: Genetik algoritma ile kümeleme optimizasyonu.
     - **Modülerlik**: Optimizasyon yöntemleri bağımsız metotlar olarak tanımlanır. Farklı optimizasyon teknikleri gerektiğinde çağrılabilir.

   - **Bağımsız Geliştirme**: Bu sınıf diğerlerinden bağımsız olarak geliştirilebilir ve farklı algoritmalar için optimize edilebilir.

#### **2.3. Otomatik Kümeleme ve Geri Bildirim Döngüsü**
   - **Sınıf: `AutoCluster`**
     - **Metotlar**:
       - `stream_data()`: Gerçek zamanlı veri akışını modele beslemek.
       - `update_clusters()`: Kümeleri belirli periyotlarla güncellemek.
       - `report_clusters()`: Kümeler hakkında rapor üretmek.
     - **Modülerlik**: Veri akışı, küme güncelleme ve raporlama süreçleri modülerdir ve birbirinden bağımsız olarak işlenebilir.

   - **Bağımsız Geliştirme**: Geri bildirim döngüsü, diğer işlemlerden bağımsız olarak çalışır ve kümeleme sürecini sürekli optimize eder.

---

### **3. Veri Analizi ve Raporlama**
Bu bölümde kümeleme sonuçlarının analizi ve raporlanması gerçekleştirilir. Analiz ve raporlama süreçleri ayrı modüller halinde yapılandırılır.

#### **3.1. Detaylı Küme Analizi**
   - **Sınıf: `ClusterAnalysis`**
     - **Metotlar**:
       - `analyze_intra_cluster_variation()`: Kümeler içindeki veri dağılımını analiz etme.
       - `analyze_inter_cluster_distance()`: Kümeler arası mesafeleri analiz etme.
       - `visualize_clusters()`: Kümelerin görselleştirilmesi (`matplotlib`, `seaborn` kullanarak).
     - **Modülerlik**: Her analiz fonksiyonu bağımsız bir metot olarak uygulanır ve gerektiğinde çağrılabilir.

   - **Bağımsız Geliştirme**: Görselleştirme ve analiz araçları, diğer veri işleme ve kümeleme modüllerinden bağımsız olarak çalışır.

#### **3.2. Veri Gömme ve Özellik Seçimi**
   - **Sınıf: `FeatureSelector`**
     - **Metotlar**:
       - `select_features()`: Özellik seçimi algoritmalarını (RFE, feature importance) kullanarak önemli özellikleri seçmek.
       - `reduce_dimensionality()`: PCA, t-SNE gibi tekniklerle boyut indirgeme işlemleri.
     - **Modülerlik**: Her özellik seçimi ve boyut indirgeme işlemi ayrı bir metot olarak tanımlanır.

   - **Bağımsız Geliştirme**: Boyut indirgeme ve özellik seçimi süreçleri, diğer aşamalardan bağımsız olarak geliştirilebilir.

#### **3.3. Otomatik Raporlama Sistemi**
   - **Sınıf: `ReportGenerator`**
     - **Metotlar**:
       - `generate_pdf_report()`: Kümeleme analiz sonuçlarını PDF formatında raporlama.
       - `generate_excel_report()`: Analiz sonuçlarını Excel formatında kaydetme.
       - `send_report_via_email()`: Kullanıcıya raporları e-posta ile gönderme.
     - **Modülerlik**: Her raporlama işlemi, farklı çıktı formatlarına bağımsız metotlarla yapılır.

   - **Bağımsız Geliştirme**: Raporlama sistemi, kümeleme süreci tamamlandıktan sonra devreye girer ve analiz sonuçlarını kullanıcıya iletir.

---

### **4. Optimizasyon Teknikleri**
Kümeleme sonuçlarının doğruluğunu artırmak için optimizasyon teknikleri kullanılır. Bu adım, kümeleme algoritmalarının performansını artırmaya odaklanır.

#### **4.1. Elbow Yöntemi ile Kümeleme Optimizasyonu**
   - **Sınıf: `ClusterOptimizer`**
     - **Metotlar**:
       - `apply_elbow_method()`: Elbow yöntemini uygulayarak en uygun küme sayısını bulma.
     - **Modülerlik**: Elbow yöntemi diğer optimizasyon tekniklerinden bağımsız bir şekilde çalışır.

#### **4.2. Silhouette Skoru ile Değerlendirme**
   - **Sınıf: `ClusterEvaluator`

**
     - **Metotlar**:
       - `calculate_silhouette_score()`: Silhouette skoru ile kümelerin uygunluğunu değerlendirme.
     - **Modülerlik**: Silhouette skoruna göre kümeleme performansı bağımsız olarak analiz edilir.

#### **4.3. Gelişmiş Optimizasyon Teknikleri**
   - **Sınıf: `AdvancedOptimizer`**
     - **Metotlar**:
       - `apply_genetic_algorithm()`: Genetik algoritma ile kümeleme optimizasyonu.
       - `apply_particle_swarm_optimization()`: Parça sürü optimizasyonu ile küme sınırlarını iyileştirme.
     - **Modülerlik**: İleri düzey optimizasyon teknikleri bağımsız modüller halinde uygulanır ve gerektiğinde kullanılabilir.

---
---
- **Python 3.8+**
- Gerekli Python kütüphaneleri:
  - `pandas`
  - `numpy`
  - `scikit-learn`
  - `matplotlib`
  - `seaborn`
  - `SQLAlchemy`
  - `requests`
  ---
