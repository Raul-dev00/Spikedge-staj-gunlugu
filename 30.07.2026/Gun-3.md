# Tarih: 30 Temmuz 2026

## Yapılan Çalışmalar
Arayüzdeki hata tespitlerini yapmak, performans iyileştirmeleri sağlamak ve harici disklere otomatik yedekleme modülünü sisteme entegre etmek için aşağıdaki çalışmaları gerçekleştirdim:

1. **ProgressBar Ölçekleme ve "Kayıp Metin" Hatasının Çözümü:**
   - **Problem:** Testleri hızlandırmak için 0.001 GB gibi düşük kotalar girdiğimde, formülümde `double`'dan `int`'e dönüşüm esnasında küsüratlar sıfıra yuvarlanıyor ve bar tamamen bozuluyordu. Ayrıca barın üzerine `%` işareti yazdırmak için gönderdiğim string formatında C++ hata veriyor, bu da metnin arayüzde görünmez olmasına yol açıyordu.
   - **Çözüm:** Kullanım miktarını `(Kullanım / Kota) * 100` formülüyle doğrudan 0-100 arasında bir yüzde değerine çekip barın maksimum aralığını 100'e sabitledim. Böylece "integer overflow" sorununu engelledim. Kaybolan metin sorununu ise format dizisinin içine `%` yerine `%%` escape karakteri ekleyerek kökten çözdüm.

2. **Silinen Dosyaların Arayüzde Gerçek Zamanlı Güncellenmemesi:**
   - **Problem:** Medya menüsünden manuel olarak bir videoyu sildiğimde, depolama alanım fiziksel olarak boşalmasına rağmen Ayarlar sayfasındaki "Kota Barı" eski durumunda donup kalıyordu.
   - **Çözüm:** Klasör önbelleğini yenileyen `mediaManager::cacheFolder()` metodunun içine `storageUsageChanged()` özel sinyalini ekledim. Depolama sayfası yüklendiğinde bu sinyali doğrudan UI'ı tazeleyen foksiyona bağlayarak arayüzün anlık güncellenmesini sağladım.

3. **Sıkıştırılmış Videoların Yanlışlıkla Silinmesi (Mantık Hatasının Giderilmesi):**
   - **Problem:** Sistemde yer kalmadığında çalışması gereken "En küçük videoları sil" seçeneği, boyut dezavantajı olduğu için sistemin kendi sıkıştırdığı yararlı videoları siliyordu.
   - **Çözüm:** Medya sıkıştırma bitince dosyanın orijinal isimle geri yüklenme adımını iptal ettim; dosyaları kalıcı olarak `compressed_` ön eki ile kaydettim. Kota yönetici algoritmasına bir kontrol yazarak bu dosyaların silme aday listesinden filtrelenmesini sağladım. Arayüz kodlarında ufak bir parser yazarak UI katmanında bu ön eki kırptım ve dosyanın başlığının hemen altına **"(Compressed)"** ibaresini basarak kullanıcı dostu bir görünüm elde ettim.

4. **Harici USB Sürücü Tespit ve Yedekleme (Auto-Export) Algoritması:**
   - Cihaza dışarıdan bir harici bellek bağlandığında, kayıtların arayüzdeki tek tuşla bu belleğe aktarılmasını sağlayan bir altyapı kodladım.
   - `QStorageInfo` kütüphanesini kullanarak Linux sisteminin montaj (mount) noktalarını (`/media/`, `/run/media/`, `/mnt/`) tarayan ve işletim sistemi dosyalarını eleyen güvenli bir filtre geliştirdim.
   - Çoklu USB bellek durumunda `QInputDialog` ile kullanıcıya liste sunarak seçim yaptırdım. Yedekleme işlemi esnasında uygulamanın donmaması için I/O sürecine **QProgressDialog** ile adım adım bir ilerleme çubuğu ekledim.

## Kazanımlar
- OOP prensiplerinin (Signal-Slot mantığı) statik UI bileşenlerini gerçek zamanlı dinamik bir yapıya dönüştürmekteki hayati gücünü pekiştirdim.
- Qt kütüphanesini kullanarak işletim sistemi alt seviyesindeki harici donanım/disklerin (`QStorageInfo`) ve uzun veri okuma-yazma işlemlerinin (I/O) profesyonelce nasıl yönetileceğini tecrübe ettim.

