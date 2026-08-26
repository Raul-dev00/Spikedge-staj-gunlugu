# Tarih: 25 Ağustos 2026

## Yapılan Çalışmalar
Kullanıcı arayüzünde tam ekran geçişlerindeki oran bozulmalarını çözmek ve sistemin kanal kapasitesini sadece iki kamera ile sınırlamamak adına genel altyapı iyileştirmeleri yaptım. Aşağıdaki çalışmaları gerçekleştirdim:

1. **Tam Ekran ve Pencereli Mod Arasındaki Oran Bozulmalarının Giderilmesi:**
   - Uygulama ilk açıldığında MFD ve Panorama bölümleri doğru en-boy oranında görünmesine rağmen, tam ekrana geçip tekrar pencereli moda dönüldüğünde Panorama'nın yüksekliğinin azaldığı ve ekran düzeninin bozulduğu tespit edildi. Qt'nin `QGridLayout` yapısı, pencereler yeniden boyutlandırıldığında satır/sütun oranlarını sıfırlama eğilimindedir. Bunu engellemek için, tam ekrana geçiş yapıldığı anki `upper`, `mfd` ve `panorama` widget'larının güncel piksel yüksekliklerini `static` değişkenlerde hafızaya alan bir yapı kurdum. Pencereli moda geri dönüldüğünde bu kaydedilmiş pikselleri `setRowStretch` fonksiyonuna besleyerek, pencereli moda dönüşte oranların milimetrik olarak korunmasını sağladım.

2. **Kanal Geçiş Butonunun Dinamik Hale Getirilmesi:**
   - Daha önceden uygulamanın sol üstündeki (`LeftButton_0`) kamera değiştirme butonu sadece 0. ve 1. indeks (RGB ve Thermal) arasında geçiş yapacak şekilde "hardcode" yazılmıştı. Bu yapıyı tamamen yıktım. Modüler aritmetik (`(mevcut_indeks + 1) % toplam_kanal_sayısı`) kullanarak butonun, `globalData.videoWidgets` listesinde kaç tane yayın olursa olsun sonsuz bir döngüde gezebilmesini sağladım. 

3. **MFD Üzerindeki İsimlendirme Standartlarının Güncellenmesi:**
   - "RGB" ve "Thermal" gibi sadece iki kanalı temsil eden sabit etiketlemelerden vazgeçerek sistemi endüstri standardı olan "CH 1", "CH 2", "CH 3" (Channel) formatına geçirdim. `QString("CH %1").arg(index + 1)` yapısıyla isimlendirmeyi kanal sayısından bağımsız, dinamik bir formata oturttum.

4. **Başlangıçtaki OpenGL Yükleme Gecikmesinin Giderilmesi:**
   - OpenGL bağlamlarının hızlıca yüklenmesi için uygulamanın açılışında arka planda `QTimer::singleShot` ile çalıştırılan bir oto-tıklama mekanizması vardı. Ancak bu mekanizma sadece ilk iki kanalı tetikliyordu. Bunu da yeni dinamik yapıya entegre ederek, uygulama açılışında bağlı olan *tüm kanalları* döngüye sokup OpenGL bağlamlarının hepsi için pürüzsüzce yüklenmesini garanti altına aldım.

## Kazanımlar
- Qt'nin layout yönetiminde, widget'ların gizlenip tekrar gösterildiği senaryolarda `stretch` değerlerinin uçucu olduğunu ve kritik arayüzlerde bu değerlerin manuel olarak `cache` mantığıyla saklanıp geri yüklenmesi gerektiğini öğrendim.
- Kod mimarisinde `if-else` ile yazılmış sabit durumların ilerleyen süreçte genişlemeyi engellediğini, bunun yerine dinamik liste okuma ve modüler aritmetik kullanarak yapılan kodlamaların daha esnek bir sistem doğurduğunu tecrübe ettim.

