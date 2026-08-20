# Tarih: 20 Ağustos 2026

## Yapılan Çalışmalar
Kullanıcı arayüzünde MFD tuşlarının yapılandırmasını dinamik hale getirmek ve JSON entegrasyonunu sağlamak için aşağıdaki çalışmaları gerçekleştirdim:

1. **MFD Tuşlarının JSON ile Dinamik Entegrasyonu:**
   - Önceki statik `QStateMachine` yapısını tamamen kaldırarak tuş görevlerini `mfd_layout.json` dosyasından okuyacak şekilde yeniden kurguladım. `MFD_section.cpp` içerisinde `loadConfig()` adında yeni bir fonksiyon yazıp JSON dosyasını ayrıştırdım. Bu sayede `toggle_camera` `toggle_fullscreen` `toggle_record` ve `toggle_joystick` gibi görevleri butonlara çalışma anında dinamik olarak atadım.

2. **Canlı Güncelleme Özelliğinin Eklenmesi:**
   - Ayarlar sayfasında yapılan MFD tuş dizilimi değişikliklerinin anında ekrana yansımasını sağladım. `PAGE_layout.cpp` içerisindeki `saveConfig()` fonksiyonunun sonuna `mfd->loadConfig()` çağrısı ekleyerek kullanıcının uygulamayı yeniden başlatmasına gerek kalmadan buton yapılandırmasının anında güncellenmesini mümkün kıldım.

3. **Yazı Durumlarının Korunması ve Varsayılan Değer Ataması:**
   - Tuşlara basıldığında ekrandaki yazıların ("RGB"den "Thermal"e geçmesi gibi) kendi içindeki durum döngülerini koruyarak dinamik yapıya adapte ettim. Kullanıcının isteği doğrultusunda JSON dosyasında ataması bulunmayan butonların ekrandaki varsayılan yazısını tekrar "N/A" olacak şekilde ayarladım.

4. **Solak Mod Mantıksal Hatasının Giderilmesi:**
   - Solak moduna geçildikten sonra kayıt gibi butonlara basıldığında ekrandaki yazıların tuşun yeni konumunda değil önceki konumunda güncellenmesi hatası yaşanıyordu. Bunun nedeninin solak mod açıldığında yazıların sadece manuel olarak yer değiştirmesi ancak arka planda hafızada kalan buton görevlerinin (lambda fonksiyonlarının) eski konum değerlerini tutmaya devam etmesi olduğunu tespit ettim. `PAGE_accessibility.cpp` dosyasında yer alan manuel dizi değiştirme işlemini tamamen kaldırdım. Bunun yerine solak mod her açılıp kapandığında `mfd->loadConfig()` fonksiyonunu çağırarak buton atamalarının ve ekran index hesaplamalarının baştan sıfırdan yapılmasını sağladım.

## Kazanımlar
- Statik durum makinelerinden dinamik ve veri güdümlü bir yapıya geçerken özellikle arayüz tetikleyicilerini bağlarken lambda fonksiyonlarının değişkenleri değer olarak kopyalamasının durum değişikliklerinde bayat verilere yol açabileceğini deneyimledim.
- Bir mod değişikliği olduğunda (örneğin solak mod) arayüz bileşenlerini manuel olarak manipüle etmek yerine ana yapılandırma fonksiyonunu tekrar çağırarak sistemi sıfırdan kurmanın veri tutarlılığını sağlamadaki en güvenilir yöntem olduğunu bizzat gördüm.

