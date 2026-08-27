# Tarih: 27 Ağustos 2026

## Yapılan Çalışmalar
Bugün uygulamanın panoramik kayıt mekanizmasını, medya oynatıcı altyapısını ve arayüz seçeneklerini çok daha esnek ve profesyonel bir yapıya kavuşturdum. Aşağıdaki geliştirmeleri gerçekleştirdim:

1. **Panoramik Kayıt Medya Oynatıcı Entegrasyonu ve MP4/MKV Desteği:**
   - Normal kameralar `.mkv` formatında kaydedildiği için "Media" oynatıcı ekranında dosyaların uzantılarının sabit olarak okunduğunu fark ettim. Panorama kayıtları `.mp4` olarak çıktığı için medya oynatıcıda GStreamer hatası veriyordu. `MEDIA_section.cpp` ve `mediaManager.cpp` içerisindeki hardcode `.mkv` bağımlılıklarını kaldırarak dosyanın gerçek tam adını `Qt::UserRole` üzerinden okuyacak şekilde sistemi esnettim. Artık panoramik kayıtlar da doğru şekilde disk kotasından düşüyor, listede küçük resmi ile çıkıyor ve oynatıcıda sorunsuzca izlenebiliyor.

2. **Panoramik Görüntü için "Birleşik" Kayıt Özelliği:**
   - Ayarlar sayfasına eklenen `Panorama Record Choice` seçeneğinin altyapı mimarisini inşa ettim. "Combined Recording" seçildiğinde, ayrı bir panorama dosyası oluşturmak yerine ana kamera kaydının altına boş bir siyah kanvas alanı tahsis ettirdim. 

3. **Gerçek Zamanlı OpenGL Framebuffer Okuma ve Ölçekleme:**
   - Birleşik kayıt için, iki farklı videoyu işlem bittikten sonra arkada birleştirmek yerine doğrudan canlı birleştirme sistemini kurdum. `PanoramaGLWidget` üzerinden o anki güncel OpenGL karesini güvenli bir şekilde çeken bir köprü (`grabPanoramaView`) yazdım. Kaydedici saniyede 30 kez bu devasa kareyi alıp, en-boy oranını bozmadan ve görüntüyü sündürmeden üstteki kameranın genişliğine milisaniyeler içinde yüksek kaliteyle (`SmoothTransformation`) orantılayıp yapıştırıyor.

4. **Kanal Tercihi Seçeneklerinin Kalıcı Ayarlara Bağlanması:**
   - Eklenen Panorama seçim listesinin `QSettings` aracılığıyla kalıcı hafızaya bağlanmasını (`record_choice`) sağladım, böylece uygulama kapatılıp açıldığında önceki tercih korunuyor.

## Kazanımlar
- OpenGL pencerelerinden doğrudan Framebuffer üzerinden görüntü yakalamanın mantığını pratik ettim; asenkron çalışan GPU ve CPU süreçlerini senkronize bir video kaydında birleştirme deneyimi edindim.
- Medya yöneticisi gibi dosya sistemi bağımlı çalışan yapıları sabit uzantılarla sınırlamak yerine, farklı formatlara uyumlu modüler hale getirmenin kodun okunabilirliğini ve geleceğe dönüklüğünü artırdığını gözlemledim.
- FFMPEG'e giden ham görüntü boru hattına birden fazla kaynaktan gelen QImage nesnelerini matematiksel hesaplamalar ve hizalamalarla çizerek tek bir kompozit video oluşturma yeteneğimi ilerlettim.

