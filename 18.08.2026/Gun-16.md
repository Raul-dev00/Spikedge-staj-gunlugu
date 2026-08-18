# Tarih: 18 Ağustos 2026

## Yapılan Çalışmalar
GStreamer tabanlı video yayın mimarisini kurmak, görüntünün uygulamaya işlenmesini belgelemek ve kayıt modülündeki kritik hataları çözmek adına aşağıdaki çalışmaları gerçekleştirdim:

1. **GStreamer Çift Kanal Yayın Komutu ve "Device Busy" Çözümü:**
   - Laptop web kamerasından görüntü alıp tek bir donanım okumasıyla iki ayrı UDP portuna (4242 ve 4243) aynı anda görüntü basan bir yayın komutu oluşturdum. İki farklı GStreamer komutu çalıştırıldığında alınan "device busy" hatasını aşmak için `v4l2src` ile alınan ana görüntüyü `tee name=t` eklentisiyle ikiyi böldüm. Birinci kolu normal x264 olarak ikinci kolu ise efekt uygulanmış şekilde ağa aktardım.

2. **GStreamer OpenGL ile "Termal Kamera" Simülasyonu:**
   - Normal RGB kameradan alınan görüntüyü eşzamanlı ve çok düşük gecikmeyle sahte bir termal kameraya dönüştüren filtre zincirini tasarladım. İkinci yayın kolunda `videobalance` kullanarak doygunluğu sıfırlayıp kontrast ve parlaklık farklarını ayarlayarak görüntüyü siyah-beyaz derinliğine getirdim. Ardından OpenGL tabanlı donanım filtresi olan `gleffects_heat` eklentisini (`glupload` ve `gldownload` arasına alarak) pipeline'a dahil edip, gerçekçi bir Isı Haritası elde ettim.

3. **Görüntü Akışının Mimari Olarak Yorumlanması ve Belgelenmesi:**
   - Qt uygulamasında ham video verisinin pipeline'dan çıkıp grafiksel arayüze basılma sürecini anlatan detaylı Türkçe yorum satırları ekledim. `GStreamerReceiver.cpp` dosyasında GStreamer sinyallerinin (`onNewSample`) yakalanıp uygulamaya nasıl paslandığını `videowidget.h` dosyasında ise bu ham piksellerin bellekte güvenle kilitlenip GPU üzerinde (`glTexImage2D`) nasıl doku olarak giydirildiğini satır satır belgeledim. Dosyanın en başına bu fonksiyonların çalışma sırasını anlatan bir akış haritası bıraktım.

4. **Kaydedilen Video Görüntüsündeki Bozulma Sorununun Giderilmesi:**
   - Kullanıcı arayüzdeki kayıt butonuna bastığında video çekildiğini, ancak .mkv dosyası izlendiğinde görüntünün tamamen bozulduğu ve renklerin kaydığı geri dönüşünü aldım. Sorunun FFmpeg'de değil, Qt'nin `QImage::scaled` fonksiyonunda olduğunu tespit ettim. Kamera çerçevesi arayüzdeki pencereye uydurulurken `Qt::SmoothTransformation` kullanılıyordu. Bu işlemin `Format_RGB888` (24-bit) formattaki resmi arka planda gizlice `Format_ARGB32` (32-bit) formatına yükselttiğini fark ettim. Dosyaya raw okuma yapan `fwrite` kodumuz ise hala `genişlik * 3` bayt hesaplaması yaptığı için pikseller arası renk kanalı kayması ve bozulmalar meydana geliyordu. `recorder.cpp` dosyasındaki ilgili satırları güncelledim. Boyutlandırmanın hemen sonrasında `convertToFormat(QImage::Format_RGB888)` çağrısı yaparak görüntüyü güvenli bir şekilde 24-bit'e geri zorladım ve bozulma sorununu kökten çözdüm.

5. **Qt Proje Yönetimi Bilgilendirmesi:**
   - Uygulamaya dahil edilen yeni bir C++ sınıfının derleme sonrasında hata vermemesi için `qt5-test.pro` dosyasına (SOURCES veya HEADERS bölümlerine) işlenmesi gerektiği bilgisini sundum.

## Kazanımlar
- GStreamer üzerinde fiziksel bir kamerayı birden çok kanala bölerken uygulamayı iki kez başlatmak yerine `tee` elemanını kullanmanın kritik olduğunu aksi takdirde donanım bazlı kilitlenme yaşandığını bizzat deneyimledim.
- Qt altyapısında `QImage` nesnelerine `SmoothTransformation` gibi kalite artırıcı özellikler uygulandığında formatın istemsizce 32-bit renk uzayına çıkabileceğini dolayısıyla ham bayt kopyalama, ffmpeg pipe aktarımı veya diske yazma gibi işlemlerden hemen önce her zaman `image.format()` değerinin izole ve teyit edilmesi gerektiğini tecrübe ettim.
- GStreamer'in `gst-plugins-base` paketindeki OpenGL eklentilerinin özel bir koda gerek kalmadan yüksek çözünürlüklerde anlık termal renk dönüşümleri yapabilecek kadar güçlü olduğunu keşfettim.

