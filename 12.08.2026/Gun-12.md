# Tarih: 12 Ağustos 2026

## Yapılan Çalışmalar
Kayıt sırasındaki görüntü kalitesi kayıplarını ve başlangıçta yaşanan "siyah ekran" / "port başlamama" yanılgılarını kalıcı olarak çözdüm:

1. **Yüksek Çözünürlüklü Ham Kayıt Altyapısı:**
   - `getLatestImage()` fonksiyonunu, ekrandaki ufaltılmış arayüz görüntüsünü (`screenshot`) kullanmak yerine doğrudan kameradan gelen ham veriyi kullanacak şekilde baştan yazdım. Eskiden widget gizliyken yüksek, görünürken düşük çözünürlükte kayıt alınıyordu. GStreamer tampon belleğini (`m_pendingBuffer`) OpenGL çizim olayında (`paintEvent`) yok etmek yerine koruyarak, widget görünür olsun veya olmasın kaydın daima maksimum kamera kalitesinde alınmasını sağladım.

2. **Başlangıçtaki Siyah Ekran Hata Çözümü:**
   - Kullanıcının termal kameraya hiç geçmeden doğrudan kayıt aldığında karşılaştığı "kamera başlatılamadı" problemini arayüz tarafında çözdüm. Hataya `QStackedWidget` içindeki gizli sekmelerin OpenGL motorunu başlatmaması sebep oluyordu. Görüntü alma mantığını, sekmenin açık/kapalı durumuna (`isVisible()`) bağımlı olmaktan kurtardım ve doğrudan ağ portundan gelen veriyi okuyacak şekilde yapılandırdım.

3. **Otomatik Tıklama Çözümü:**
   - OpenGL motorunun her iki sekme için de garanti olarak uyanması adına pratik bir geçici çözüm (hack) uyguladım. `SECTION_MFD::setupUI()` içerisine bir `QTimer::singleShot` ekledim. Uygulama açılır açılmaz çok kısa bir aralıkla arka planda otomatik olarak Thermal butonuna tıklanıp hemen geri RGB'ye dönülmesini sağladım. Böylece kullanıcı fark etmeden sistem her iki kamera akışını da güvenle başlatmış oldu.

4. **Ayarlar Menüsü Senkronizasyonu:**
   - Bir önceki aşamada arayüze eklenen Checkbox ve Combobox'ları arka plandaki kayıt motoruna gerçek zamanlı olarak bağladım. `PAGE_channels.cpp` içerisindeki ayar kaydetme işlemlerine müdahale ederek, kullanıcı seçim yaptığı an `MediaManager::updatePaths()` fonksiyonunu tetikleyecek yapıyı kurdum. Böylece terminalde kayıt durumuyla ilgili sinyalleri yakalayarak, sadece kullanıcının seçtiği kanalların veya ikisinin birden dinamik olarak kaydedilmesini sağladım.

