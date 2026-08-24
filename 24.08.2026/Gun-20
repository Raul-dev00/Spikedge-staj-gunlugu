# Tarih: 24 Ağustos 2026

## Yapılan Çalışmalar 
Kullanıcı arayüzünde video kanallarının (endpoint) yönetimini esnekleştirmek, çalışma zamanı verimliliğini artırmak ve yeni eklenen Panorama modülünü mevcut yapıya entegre etmek için aşağıdaki çalışmaları gerçekleştirdim:

1. **Dinamik Video Kanalı (Endpoint) Altyapısının Kurulması:**
   - Daha önceden kod içine "ep0" ve "ep1" olarak sabit (statik) kodlanmış olan video port yapılarını sildim ve tamamen dinamik, istenildiği kadar eklenip çıkarılabilen bir liste (`QList`) yapısına geçirdim. Bağlantı ayarları sayfasına (`PAGE_connection`), kanalların yanına "Sil" ve en alta "Kanal Ekle" butonlarını ekledim. Bu sayede her kanal eklendiğinde butonlar otomatik olarak bir alt satıra kaydırılıyor ve ayarlar dosyasına (`globalData.settings`) "connection/channel_count" dinamik döngüsüyle kaydediliyor.

2. **Yeniden Başlatma Olmadan Kanalları Güncelleme (Hot-Swap):**
   - Eskiden port değişikliklerinin yansıması için uygulamanın kapatılıp açılması gerekirken, bu gereksinimi tamamen ortadan kaldırdım. `MFD_section` sınıfının içine `reloadVideoWidgets()` adında yeni bir metot yazdım. Ayarlar kaydedildiği anda bu fonksiyon tetiklenerek eski GStreamer alıcılarını temizliyor, grid layout'taki eski video penceresini siliyor ve yeni ayarlanan portlarla taze bağlantıları saniyeler içinde ekrana getiriyor.

3. **Panorama Sisteminin Modern OpenGL ile Entegrasyonu:**
   - Proje arkadaşımın sunduğu proje klasöründeki OpenCV tabanlı Panorama modülünü, dinamik ve gelişmiş projemizle birleştirdim. Panorama'nın ihtiyaç duyduğu modern ekran çizim kuralları sebebiyle `main.cpp` içerisine `QSurfaceFormat::CoreProfile` (OpenGL 3.3) ayarlarını dahil ettim. Bu profil zorunluluğuna uyması adına `videowidget.h` dosyasında bulunan eski usül (`glDrawPixels`) çizim fonksiyonlarını, tamamen performans odaklı **Shader tabanlı (`QOpenGLShaderProgram`)** modern yapıya göç ettirdim. Ekrandaki sarı yazılar ve kırmızı nişangah gibi HUD çizimlerini de (QPainter entegrasyonuyla) sorunsuz taşıdım.

4. **Canlı Akış Kopyalama Yöntemi:**
   - Panorama eklentisine görüntü sağlamak için fazladan ağ portu dinlemek yerine, halihazırda MFD'de akan yayını kopyalama mekanizmasını devreye aldım. `GStreamerReceiver` sınıfına bir `panoramaTap` ekledim. Ekrana giden her bir görüntü tamponundan anlık olarak `CV_8UC3` formatında OpenCV RGB matrisi türetip BGR'ye dönüştürerek doğrudan Panorama iş hattına iletiyorum. Bu sayede işlemciyi ve ağı yormadan iki ekranı tek akıştan beslemiş oldum.

5. **UI Yerleşim ve Oran İyileştirmeleri:**
   - MFD'nin hemen altında yer alan Panorama önizleme şeridinin eskiye oranla çok küçük kaldığı yönündeki tespiti çözdüm. Üst şeritte eksik olan menü butonunu tamamladım. `UPPER_section` dosyasına yeni bir `Panorama` butonu ekleyerek kullanıcıyı doğrudan tam ekran indeksine yönlendirebilmesini sağladım. Ana `main.cpp` içerisindeki grid yerleşim sabitlerine müdahale ettim. MFD'nin yüksekliğini temsil eden `t` değerini 85'ten 70'e indirip, panorama şeridinin `k` değerini 10'dan 25'e çıkarttım. Böylece toplam çözünürlük alanını bozmadan Panorama ekranının 2.5 kat daha büyük ve göze hitap eder bir yerleşimde durmasını sağladım.

## Kazanımlar
- Eski GStreamer boru hatlarını dinamik olarak "sil-yeniden yarat" mantığıyla yönetmenin oldukça iyi çalıştığını ve C++ tarafında `gst_element_set_state(NULL)` adımlarının dikkatli takip edildiği sürece yeniden başlatma ihtiyacını tamamen rafa kaldırdığını doğruladım.
- Qt5 üzerinde birden fazla OpenGL tabanlı ekran bileşeni kullanırken bağlam profillerinin karıştırılamayacağını tecrübe ettim. Projenin tek bir bölgesindeki OpenGL ihtiyacının (Core Profile 3.3), aslında sistemin bütün `QOpenGLWidget` içeren sınıflarını (VideoWidget) Shader destekli formata geçmeye zorladığını yaşayarak teyit ettim.
- QPainter ile standart Qt çizim nesnelerinin, Core Profile aktif bir Shader arkası tamponda dahi eşgüdümlü çalışabildiğini; sadece çizim işlem sırasına sadık kalınmasının yeterli olduğunu gördüm.
