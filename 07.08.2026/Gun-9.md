# Tarih: 07 Ağustos 2026

## Yapılan Çalışmalar 
Kullanıcı arayüzünde MFD ve sanal joystick davranışlarını iyileştirmek, kişiselleştirmeyi artırmak ve arayüz hatalarını gidermek için aşağıdaki çalışmaları gerçekleştirdim:

1. **Aynalama Özelliğinin Sadece MFD'ye İndirgenmesi:**
   - Daha önce tüm uygulama genelinde `qApp` üzerinden uygulanan solak modu mantığını değiştirdim. Bu özelliğin sadece MFD ekranında çalışması istendiği için yansıtma işlemini doğrudan `globalData.sectionMFDWidget` pointer'ını kullanarak sadece MFD widget'ına uygulanacak şekilde sınırlandırdım.

2. **Joystick Boyutunun Orantılı Hale Getirilmesi:**
   - Eskiden statik olarak 240x240 piksel değerinde sabit kalıp, tam ekrana geçildiğinde ufak görünen sanal joystick'in boyutunu dinamik hale getirdim. Kullanıcının isteği üzerine joystick'in üst kenarını tam olarak sağ taraftaki 5. butona hizalamak için düzen birimlerini hesapladım. Joystick'in yüksekliğini, MFD ekranının yüksekliğinin tam olarak **6/19'una** denk gelecek orana ayarlayarak hem pencereli hem de tam ekran modunda kusursuz hizalanmasını sağladım.

3. **Joystick'in Ekranda Sürüklenebilir Yapılması:**
   -  MFD içindeki butona tıklandığında beliren joystick'i, kullanıcının fare ile tutup istediği yere taşıyabilmesi için serbest bir yapıya kavuşturdum. Joystick'i bağlı olduğu kısıtlayıcı düzenden çıkardım. Joystick'i yarı saydam bir çerçevenin içine aldım ve en üstüne **"::: DRAG :::"** yazan bir sürükleme çubuğu ekledim. Qt'nin `eventFilter` yapısını kullanarak fare hareketlerini dinledim ve kutuyu hareket ettiren matematiksel altyapıyı kurdum. Ekranın dışına çıkmaması için de sınır kontrolleri ekledim.

4. **Joystick Konumunun Oransal Olarak Kaydedilmesi:**
   - Kullanıcı joystick'i bir yere sürükleyip bıraktığında, uygulamayı kapatıp açsa bile aynı yerde durması için kayıt özelliği ekledim. Konumu kaydederken sabit piksel değerlerini değil, ekranın o anki boş alanının yüzdelik oranlarını hesaplayarak `QSettings` sınıfı vasıtasıyla diske kaydettim. Böylece farklı çözünürlüklerde veya ekran boyutlarında joystick daima aynı köşe oranında konumlanmasını başardım.

5. **Modlar Arası Geçişlerde Yaşanan Ekran Dışına Taşma Sorununun Giderilmesi:**
   - Kullanıcı joystick'i sağ alt gibi köşelere taşıyıp pencereli moddan tam ekrana geçtiğinde, joystick'in sağ ve alt sınırlarının ekranın dışına taştığı görüldü. Bunun nedeninin, tam ekrana geçerken sistemin yeni konum hesaplamasını yapmasının *ardından* joystick kutusunun kendini büyütmesi olduğunu tespit ettim. Büyüyen fazlalık kısımlar sağ ve alt kısımlardan taşıyordu. Yeniden boyutlandırma dinleyicisini hem ana ekrana hem de **bizzat joystick çerçevesine** bağladım. Böylece joystick kendi boyutunu büyüttüğü milisaniye içerisinde, yeni boyutuna göre konumunu hızla tekrar denetleyip ekran sınırlarının içerisine itilmesini sağladım. Sorun kökten çözülmüş oldu.

## Kazanımlar
- Arayüz bileşenlerini, serbest gezen eklentiler haline getirmek için Layout'tan çıkarmanın yeterli olmadığını ana ekran boyut değişimlerinde bu öğelerin konumlarının eski ölçülerde kalmasını engellemek adına manuel `Resize` event takipleri yapılması gerektiğini deneyimledim.
- Pencere içi bileşenlerin konumunu diske kaydederken piksel olarak kaydetmenin, ekran boyutları değiştiğinde hatalara yol açtığını bunun yerine `mevcut boş alana bölünerek elde edilen % oran ` değerlerinin kaydedilmesinin en güvenli yöntem olduğunu yaşayarak teyit ettim.
- Bir objenin boyutu değiştiğinde o objenin taşmaması için, sınırlandırma işlemlerinin o objenin doğrudan kendi `QEvent::Resize` tetikleyicisi içerisinde tekrar denetlenmesinin zamanlama sorunlarını tamamen önlediğini bizzat gördüm.

