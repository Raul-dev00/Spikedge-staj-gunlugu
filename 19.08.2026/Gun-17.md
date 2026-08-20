# Tarih: 19 Ağustos 2026

## Yapılan Çalışmalar 
Kullanıcı arayüzünde MFD tuşlarının işlevlerini kodun içine gömülü olmaktan çıkarıp, JSON tabanlı dinamik ve kullanıcı tarafından özelleştirilebilir bir yapıya dönüştürmek için aşağıdaki çalışmaları gerçekleştirdim:

1. **MFD Tuşlarının Dinamikleşmesi İçin Mimari Planın Çıkarılması:**
   - MFD butonlarının işlevlerini değiştirebilmek amacıyla `mfd_layout.json` tabanlı yeni bir mimari plan hazırladım. Daha önce `SECTION_MFD` içindeki statik State Machine'ler ve `MFDStrings` ile yönetilen buton metinlerinin yerine, uygulamanın konfigürasyonu dosyadan okuyarak arayüzü çizmesi ve aksiyonları buna göre atamasına karar verdim.

2. **PAGE_layout Sayfasının Bağımlılıklarının Koparılması ve Tasarımının Güncellenmesi:**
   - Ayarlar sekmesindeki layout sayfasının, MFD'nin anlık çalışma durumuna (`MFDStrings`) olan bağımlılığını tamamen kopardım. Layout sayfası önceden yüklendiğinde, MFD'nin henüz "RGB" veya "Thermal" durumuna geçmemesi sebebiyle etiketlerin boş gelmesi sorunu yaşanıyordu. Sayfayı, anlık durumu okuyan değil de doğrudan kalıcı ayar dosyasını okuyan ve gösteren bağımsız bir arayüze çevirdim.

3. **Kullanıcı Ayar Arayüzüne QComboBox'ların Eklenmesi:**
   - Kullanıcının tuşlara farklı işlevler atayabilmesi için `PAGE_layout` sayfasındaki boş butonları, açılır listelerle değiştirdim. Sol ve sağ MFD ekranında bulunan 6'şar tuş için içerisine "Toggle RGB / Thermal", "Windowed / Fullscreen", "Start / Stop Record" gibi eylem seçeneklerinin bulunduğu ComboBox'lar yerleştirdim.

4. **JSON Okuma/Yazma İşlemlerinin Entegre Edilmesi:**
   - Kullanıcı arayüzde bir butona yeni bir işlev atadığında, bu seçimin arka planda hemen dosyaya kaydedilmesini sağladım. `QJsonDocument`, `QJsonObject` ve `QJsonArray` kullanarak ComboBox'lardaki `currentIndexChanged` sinyalini yakalayarak tetiklenen bir `saveConfig()` mekanizması kurdum. Artık yapılan her atama `mfd_layout.json` dosyasına güvenle kaydediliyor.

5. **Qt MOC (Meta-Object Compiler) Hatalarının Giderilmesi:**
   - Sürükle-bırak mantığı için projeye yeni dahil edilen `DragButton` sınıfının derlenmesi sırasında ortaya çıkan sahte `Q_OBJECT` hatasını çözdüm. `src/DragButton.cpp` dosyası içerisinde başlık dosyasının yanlış (`#include "DragButton.h"`) çağrıldığını tespit ettim. Proje hiyerarşisine uygun olarak `#include "include/DragButton.h"` şeklinde değiştirerek derleyici ve IDE bazlı derleme hatalarını giderdim.

## Kazanımlar
- Qt uygulamalarında sınıfların `setupUI()` gibi arayüz çizme fonksiyonlarının çağrılma sırasının (Execution Order) global değişkenlerin o anki halini doğrudan etkilediğini birbirini bekleyen veya birbirinden veri çeken pencerelerde "N/A" veya boş veri sorunlarına yol açtığını bizzat analiz etmiş oldum.
- Uygulama ayarlarını bir UI'dan okurken o an çalışan *canlı state'lerden (durumlardan)* okumak yerine, tek kaynak noktası olarak statik bir ayar dosyasını (JSON, XML vs.) "Source of Truth" olarak almanın modülerliği ve kod bağımsızlığını dramatik şekilde artırdığını teyit ettim.
- Yeni bir `Q_OBJECT` barındıran dosya projeye dahil edildiğinde IDE'nin hata göstermesinin çoğu zaman `#include` yollarındaki bir pürüzden dolayı `moc` işleminin başarısız olmasından kaynaklandığını include yolunu düzelterek ve `qmake`'i yeniden çalıştırarak her şeyin sorunsuzca çözüldüğünü bir kez daha deneyimledim.

