# Tarih: 21 Ağustos 2026

## Yapılan Çalışmalar
Kullanıcı arayüzünden GStreamer üzerinden gelen video yayın portlarını değiştirdiğimizde yayının gelmemesi sorununu çözmek ve sabit kanal sistemini tamamen dinamik bir yapıya dönüştürmenin mimari temellerini atmak için aşağıdaki çalışmaları gerçekleştirdim:

1. **GStreamer Port Güncelleme ve Çökme Hatasının Çözülmesi:**
   - Ayarlar menüsünden port numarası (örneğin 4244) değiştirildiğinde GStreamer yayınının yeni porta geçmemesi sorununu inceledim. `main.cpp` içerisindeki `syncSave()` fonksiyonunda, eski ve kod içinde hiç başlatılmamış `receiver2` isimli TCP alıcı nesnesine port değiştirme emri gönderildiğini tespit ettim. Bu geçersiz referans hatasını temizledim ve port değişim sinyalinin doğrudan `globalData.videoReceivers` içindeki aktif `GStreamerReceiver` nesnelerine güvenli bir şekilde iletilmesini sağladım.

2. **Dinamik Kanal Yönetimi İçin Mimari Planlama:**
   - Sadece `endpoint_0` ve `endpoint_1` olarak sabit kodlanmış 2 kanallı yapıdan, istenildiği kadar kanal eklenebilen ve çıkarılabilen esnek bir yapıya geçmek için kapsamlı bir plan hazırladım. Arka planda verileri güvenli tutmak için `util.h` içerisinde `ChannelConfig` adında yeni bir veri nesnesi tanımlanmasına ve bu nesnelerin `globalData.channels` isimli bir dinamik listede  tutulmasına karar verdim.

3. **Arayüz / Kullanıcı Deneyimi Tasarımı ve Fikir Alışverişi:**
   - Dinamik kanal ekleme sürecinin kullanıcı arayüzünde nasıl sunulacağı konusunda değerlendirmeler yaptım. Alt alta uzayan ayar formları yerine, "Kanal Ekle" butonuna basıldığında açılacak şık bir `QDialog` (Pop-up) penceresi üzerinden Kanal Adı, IP ve Port bilgilerinin alınmasına karar verdik.
   - Kanalların ayarlar sayfasında listelenmesi için iki farklı arayüz yaklaşımı geliştirdim: Biri standart `QListWidget` kullanan klasik yöntem, diğeri ise her kanalın oval hatlı bir `QFrame` içerisinde kendi "Düzenle" ve "Sil" butonlarını barındırdığı modern "Kart" tasarımı. Uygulamanın modern görünmesi adına Kart tasarımını kurgulayıp önerdim.

## Kazanımlar
- C++'ta başlatılmamış veya boşa düşmüş işaretçiler üzerinden metot çağırmanın, işletim sistemine bağlı olarak uygulamayı bariz şekilde çökertmese bile iş akışını sessizce durdurup mantık hatalarına yol açabildiğini ve pointer kontrollerinin ne kadar kritik olduğunu pratikte gözlemledim.
- Sistemin çekirdek mimarisini değiştirmeye başlamadan önce, arayüzü geliştirecek kişiyle arka planda veri alışverişinin hangi standartta (`ChannelConfig` yapısı) olacağı konusunda önceden plan yapmanın, takım çalışması ve kodlamadaki zaman kaybını ciddi oranda önlediğini tecrübe ettim.

