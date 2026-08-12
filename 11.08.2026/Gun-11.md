# Tarih: 11 Ağustos 2026

## Yapılan Çalışmalar
Bir önceki gün yapılan araştırmalar ışığında, kayıt motorundaki kilitlenmeleri ve arayüz bağlantı kopukluklarını giderdim:

1. **İş Parçacığı Güvenliği Sağlanması:**
   - `VideoWidget` sınıfı içerisine `std::mutex imageLock` ekledim. GStreamer'dan gelen yeni kareleri işleyen `handleNewFrame` fonksiyonu ile, kayıt motoruna anlık görüntü sağlayan `getLatestImage` fonksiyonu arasına kilit mekanizması kurdum. Bu sayede eşzamanlı bellek okuma/yazma çakışmalarını ve program çökmelerini tamamen ortadan kaldırdım.

2. **Kayıt Ayarları Arayüzünün Tasarlanması:**
   - "Ayarlar -> Channels" sayfasına kayıt konfigürasyonları için yeni arayüz elemanları (Checkbox ve Combobox) ekledim. Kullanıcının Kanal 1 (RGB) ve Kanal 2 (Thermal) kayıt durumlarını seçebilmesi için görsel arayüzü tasarladım ve ilgili Checkbox'ları ekrana yerleştirdim. Bu aşamada sadece arayüz tarafı tamamlandı butonların ve kutucukların arka plandaki kayıt motoruna bağlanması ve işlevlerinin atanması işlemi bir sonraki güne bırakıldı.

3. **Kayıt Çözünürlüğü ve Düzen İncelemeleri:**
   - Çift kanallı kayıt sırasında ekranların yan yana dizilimi (Kanal 1 sağda, Kanal 2 solda) için kullanılan QPainter çizim algoritmasını optimize etmek adına teknik incelemeler yaptım. FFmpeg'e gönderilecek çerçevenin boyutlarının hesaplanmasında `m_w` ve `m_h` değişkenlerinin nasıl hesaplandığını tespit ettim. Kayıt motorunun videoları birleştirirken oluşturduğu potansiyel orantı bozukluklarını önlemek için Qt'nin `Qt::SmoothTransformation` özelliğinin performans etkilerini değerlendirdim.

4. **HUD Çizim Kalitesinin İncelenmesi:**
   - Kaydedilen videonun üzerine eklenen yazılar ve kırmızı nişangahın (Crosshair) daha keskin görünmesi için çizim katmanı hazırlıkları yaptım. `QImage::Format_RGB888` formatında doğrudan çizim yapmanın bazı durumlarda bellekte kaymaya ve görüntü bozulmasına yol açabileceğini tespit ederek, çizim işleminden hemen önce görüntünün `Format_RGB32`'ye çevrilmesi gerekliliğini doğruladım ve görsel testlerini tamamladım.

## Kazanımlar
- `std::mutex` kullanılarak Qt arayüz iş parçacığı ile GStreamer iş parçacığı arasındaki veri aktarımında oluşabilecek eşzamanlı erişim riskleri ve program çökmeleri tamamen giderdim.
- "Ayarlar -> Channels" sayfası için yeni arayüz kontrolleri oluşturularak uygulamanın kişiselleştirilebilirlik yetenekleri arayüz seviyesinde genişletttim.
- İki farklı kamera görüntüsünün birleştirilmesi esnasında yaşanabilecek en-boy oranı bozulmalarını önlemek adına `Qt::SmoothTransformation` özellikleri test edildi ve düzen algoritmaları optimize ettim.
- HUD katmanının çizim kalitesini artırmak için format dönüşüm analizleri yapılarak, bellek hizalama problemleri nedeniyle oluşabilecek piksel kaymalarının önüne geçtim.
