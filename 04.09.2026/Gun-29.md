# Tarih: 4 Eylül 2026

## Yapılan Çalışmalar ve Entegrasyon Süreçleri

Bugün ağırlıklı olarak projenin arka plan mimarisini güçlendirdiğim ve arayüzdeki buton etkileşimlerini ileri seviyeye taşıdığım bir gündü. Temel olarak şu geliştirmeleri gerçekleştirdim:

1. **`panoramaKlasör 2` Özelliklerinin Ana Projeye Entegrasyonu:**
   - Arka planda geliştirilmiş olan yeni yapay zeka/görüntü iyileştirme algoritmalarını (`EnhanceBackend`), komut satırı desteklerini (`CliOptions`) ve LighterGlue ağlarını içeren güncel mimariyi (`panoramaKlasör 2`) başarıyla mevcut projeme (`panoramaKlasör`) entegre ettim.
   - Bu taşıma ve kod birleştirme işlemi esnasında, yeni versiyonda silinmiş olan **ScreenRecorder** modülünün kaybolmasını engelledim. Eski kod ile yeni kodun çakıştığı yerleri analiz edip kayıt fonksiyonlarını satır satır asıl projeye "cerrahi" bir müdahale ile geri kazandırdım.
   - OpenCV bağımlılıklarını güncelleyip (`features.hpp` referanslarını düzelterek) derleme hatalarını giderdim ve `panorama_standalone` uygulamasını hatasız bir şekilde derlenebilir hale getirdim.

2. **MFD Butonlarında İleri Düzey Etkileşim:**
   - MFD buton eylemlerine arayüzden atanabilen fakat kodda karşılığı olmayan çift tıklama ve uzun basma desteklerini kodlayarak hayata geçirdim.
   - Standart Qt buton yapısı (`QPushButton`) bu özellikleri yerleşik olarak desteklemediği için, `MFDButton` adında zamanlayıcı (QTimer) mantığıyla çalışan tamamen özel bir buton sınıfı geliştirdim.
   - Bu sınıf sayesinde sistem artık basılı tutma süresini (500ms) veya iki tıklama arası gecikmeyi (200ms) hesaplayarak doğru aksiyonu tetikliyor.
   - `SECTION_MFD` mimarisini güncelleyerek, oluşturduğum butonları JSON (`mfd_layout.json`) konfigürasyon dosyasına uyumlu hale getirdim ve projeyi başarıyla test ettim.

3. **Kullanılabilirlik Analizleri: TOT ve KLM Karşılaştırması:**
   - Arayüzün pratik performansını ölçmek amacıyla TOT (Time on Task - Görev Tamamlama Süresi) testleri gerçekleştirdim.
   - Gerçek testlerden elde edilen TOT sürelerini, teorik olarak hesapladığım KLM (Keystroke-Level Model) test sonuçlarıyla karşılaştırdım.
   - Bu karşılaştırma sayesinde, teorik hesaplamaların gerçek dünya kullanımıyla ne kadar örtüştüğünü analiz etme ve arayüzdeki potansiyel gelişim alanlarını belirleme fırsatı buldum.

## Kazanımlar
- Karmaşık, çok dosyalı ve donanımsal bağımlılıkları olan bir projede kod birleştirme yeteneklerimi uygulayarak test etme şansı buldum. Yeni mimari ile eski özelliklerin çakışmalarını başarıyla yönettim.
- Qt Framework içerisinde varsayılan olarak bulunmayan olayları Olay Yöneticisi ve Zamanlayıcılar kullanarak nesne yönelimli programlama prensipleri dahilinde pratik olarak nasıl geliştirebileceğimi öğrendim.
- TOT (Time on Task) ve KLM (Keystroke-Level Model) test ölçümlerini karşılıklı kıyaslayarak; İnsan-Bilgisayar Etkileşimi (HCI) teorilerinin, ampirik verilerle nasıl desteklenip raporlanacağını tecrübe ettim.

