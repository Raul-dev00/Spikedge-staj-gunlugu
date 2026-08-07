# Tarih: 06 Ağustos 2026

## Yapılan Çalışmalar
Kullanıcı arayüzüne yeni özelliklerin eklenmesi, erişilebilirlik ayarlarının geliştirilmesi ve kayıt sırasında yaşanan depolama limitine bağlı hataların giderilmesine dair aşağıdaki çalışmaları gerçekleştirdim:

1. **Uygulama Açılış Ekranının İncelenmesi:**
   - Uygulama ilk açıldığında hangi arayüzün ekrana geldiğini analiz ettim. Özel bir ana sayfa dosyasının (örneğin `PAGE_main.cpp`) olmadığını; başlangıç ekranının `main.cpp` içerisinde `QGridLayout` yardımıyla `UPPER`, `MFD`, `PANORAMA` ve `ACTION` modüllerinin yapboz gibi birleştirilmesiyle dinamik olarak oluşturulduğunu saptadım.

2. **Erişilebilirlik (Accessibility) Menüsü ve Solak Modu Arayüzünün Eklenmesi:**
   - Ayarlar paneline yeni bir "Accessibility" (Erişilebilirlik) sekmesi ekledim. Bu sekme içerisine solak kullanıcılar için "Left-Handed Mode" ibaresini ve bu ayarı açıp kapatmak üzere tetikleyici bir buton yerleştirdim.

3. **Solak Modu (Y Ekseninde Aynalama) İşlevinin Kodlanması:**
   - Solak modu butonuna basıldığında uygulamanın Y ekseninde simetrik olarak dönmesi (sağdaki joystick'in sola, kamera panelinin sağa geçmesi vb.) işlevini hayata geçirdim. Kolon yerleşimlerini manuel olarak tek tek değiştirmek yerine, Qt kütüphanesinin yerleşik desteğini kullanarak `qApp->setLayoutDirection(Qt::RightToLeft)` fonksiyonu üzerinden tüm uygulamanın anında ve hatasız bir şekilde aynalanmasını sağladım.

4. **Hafıza/Kota Dolduğunda Kaydın Durmaması Sorununun Çözümü:**
   - MFD üzerindeki butondan kayıt devam ederken hafıza dolmasına rağmen uygulamanın hata vermeyerek kayda devam etmesi sorununu araştırdım. İki temel sorun buldum: İlki, işletim sisteminde video dosyasının boyutu artarken Qt'nin `QDir` kütüphanesinin bunu önbelleğe alıp eski boyutu okumaya devam etmesiydi. İkincisi ise, zamanlayıcı mantığında hafıza dolsa bile kaydı durdurmak yerine arka planda yer açmaya çalışacak bir mantığa sapılmasıydı. Bunun için `mediaManager.cpp` içerisinde yer alan disk okuma hesaplamalarına `recordPath->refresh()` komutunu ekleyerek dosya boyutlarının gerçek zamanlı çekilmesini güvence altına aldım. Ardından `recorder.cpp` içindeki `quotaTimer` yapısını eski çalışır haline geri döndürdüm.

## Kazanımlar
- Qt platformunda `QApplication::setLayoutDirection` özelliği sayesinde, karmaşık "Grid Layout" yerleşimlerini kodla teker teker baştan dizmeye gerek kalmaksızın, uygulamanın tamamını native ve sorunsuz bir biçimde sağdan sola çevirebildiğimi deneyimledim.
- `QDir` kütüphanesinin okuma performansını artırmak amacıyla dosya sistemi verilerini önbellekte tuttuğunu; dolayısıyla arka planda anlık olarak büyüyen video vb. dosyaların güncel boyutlarını tespit etmek için zorunlu olarak `refresh()` fonksiyonunu çağırmam gerektiğini bizzat yaşayarak öğrendim.

