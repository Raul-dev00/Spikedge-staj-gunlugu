# Tarih: 10 Ağustos 2026

## Araştırma ve Planlama Çalışmaları
Bu aşamada uygulamanın temel video ve kayıt altyapısında kullanılacak kütüphanelerin entegrasyonu ve veri işleme yöntemleri üzerine genel teknik araştırmalar yaptım.

1. **GStreamer ve Bellek Yönetimi (Buffer Management):**
   - GStreamer'ın `appsink` modülü üzerinden video paketlerinin `GstBuffer` uygulamanın arayüzüne nasıl aktarılacağı üzerine dokümantasyon incelemeleri yaptım.
   - `gst_buffer_map` ile C++ tarafında bellek okuma yaparken dikkat edilmesi gereken hizalama kuralları ve referans sayacı mekanizmalarını araştırdım.

2. **Qt QImage ve İş Parçacığı (Thread) Mantığı:**
   - Ağ üzerinden gelen ham verilerin ekranda gecikmesiz gösterilebilmesi için Qt'nin ana arayüz iş parçacığı ile arka planda çalışan GStreamer iş parçacıkları arasındaki veri aktarım güvenliğini araştırdım.
   - Çizim işlemlerinde kullanılacak `QImage` formatlarının (`Format_RGB888` vs `Format_RGB32`) performans farklarını ve bellek dizilimi uyumluluklarını inceledim.

3. **Çoklu Medya ve FFmpeg Altyapısı:**
   - İki farklı görüntü kaynağının tek bir çerçevede birleştirilip FFmpeg kayıt motoruna nasıl güvenle beslenebileceği konusunda yöntem araştırmaları yaptım.
   - Görüntülerin yan yana dizilmesi aşamasında orantıların korunması ve çizim sınırları hakkında teorik bilgiler topladım.

## Kazanımlar
- GStreamer tabanlı video işleme boru hatlarının (pipeline) Qt arayüzüne (QOpenGLWidget) nasıl entegre edileceği konusunda derinlemesine teorik bilgi sahibi oldum.
- Çoklu kamera sistemlerinde (RGB ve Thermal) video çerçevelerinin (`GstBuffer`) okunması sırasında uyulması gereken veri hizalama (memory alignment) ve format dönüşüm kurallarını öğrendim.
- Video kayıt motoru (FFmpeg) ile kullanıcı arayüzü arasındaki etkileşimlerde yaşanabilecek kilitlenme (race condition) senaryolarını öngörerek, gelecekteki geliştirmeler için güvenli bir Thread senkronizasyonu altyapısı modelledim.
- Yapılan bu ön araştırmalar sayesinde, uygulamanın medya modülünde yaşanabilecek olası darboğazları (bottleneck) ve performans kayıplarını minimuma indirecek yol haritasını çıkardım.
