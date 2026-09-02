# Tarih: 2 Eylül 2026

## Yapılan Çalışmalar
Bugün hem MFD ekranındaki kayıt göstergesinin kullanılabilirliğini iyileştirdim hem de arayüz testleri için bilimsel temellere dayalı bir süre hesaplama/analiz aracı geliştirdim. Yaptığım çalışmaları şu şekilde özetleyebilirim:

1. **MFD Ekranı Kayıt Göstergesinin İyileştirilmesi:**
   - MFD ekranındaki kayıt yazısının yanıp sönme animasyonunu optimize ettim. İlk etapta yazıyı kırmızı renkli "RECORD" olarak değiştirdim, ancak sonrasında tekrar orijinal sarı renkli "Stop Recording" formatına geri döndürdüm.
   - Eskiden sadece kameradan yeni bir kare geldiğinde güncellenen şeffaflık geçişi, düşük video frekanslarında takılmalara neden oluyordu. `VideoWidget` sınıfına, donanımdan ve kamera kare hızından bağımsız saniyede yaklaşık 30 kez çalışan bir `QTimer` ekledim.
   - Bu sayede kamera görüntüsü takılsa veya video akışı yavaşlasa bile, kayıt göstergesinin bir sinüs dalgası (`std::sin`) etrafında son derece yumuşak ve akıcı geçişlerle yanıp sönmesini sağladım.

2. **Arayüz Testleri İçin Teorik Altyapı ve Makale Araştırmaları:**
   - İnsan-Bilgisayar Etkileşimi prensiplerini projemize entegre etmek amacıyla arayüz ölçümleri zerine derinlemesine bir literatür taraması yaptım.
   - Geliştirdiğim MFD arayüzünün kullanılabilirliğini bilimsel bir temele oturtmak için Fitts Yasası, KLM (Keystroke-Level Model) ve GOMS testlerini inceledim. Amacım, arayüzde ileride yapacağımız gerçek kullanıcı bazlı "Time on Task" testleri için önceden sağlam bir teorik temel oluşturmaktı.
   - Bu araştırmalarımda temel referans olarak şu makale ve kaynakları baz aldım:
     - *"The Keystroke-Level Model for User Performance Time with Interactive Systems"*
     - *"A Guide to GOMS Model Evaluation using NGOMSL"*
     - *"Measuring the User Experience: Collecting, Analyzing, and Presenting Usability Metrics"* (Özellikle Time-Based Metrics bölümü üzerinden pratik metriklerin teorik olanlarla nasıl harmanlanacağını inceledim)

3. **KLM ve Fitts Yasası Hesaplama Aracının Geliştirilmesi:**
   - Makalelerden edindiğim bilgiler ışığında, teorik süre testlerimizi otomatize etmek için Python dilinde bağımsız bir hesaplama aracı kodladım.
   - Araca M (Düşünme), K (Tıklama), H (El Değiştirme), T (Klavye ile Yazma) ve P (Yönelme) operatörlerini detaylıca entegre ettim. Özellikle IP ve Port girme gibi senaryolar için `T(n)` ile karakter bazlı yazma süresini ve `H` operatörü ile fareden klavyeye el geçiş sürelerini modeledim.
   - P (Yönelme) operatörünü standart 1.1 saniyelik ortalamadan kurtararak, doğrudan Fitts Yasası'nın formülüne (Shannon Formülasyonu) bağladım. Belirlediğim butonların uzaklık ve genişlik değerlerini dinamik olarak alıp, hedefe gitme süresini gerçekçi bir şekilde hesaplamasını sağladım.
   - Ölçeklenebilirlik testi yapabilmek için aracı aynı senaryoyu **Pencereli**, **Büyütülmüş** ve **Tam Ekran** modlarında yan yana eş zamanlı çalışacak şekilde tasarladım. Ekran büyüdükçe mesafelerin ve buton boyutlarının nasıl etkilendiğini teorik olarak gözlemledim.
   - *Senaryo Testleri:* Yeni kanallar ekleme (klavye ile detaylı IP ve Port girme senaryosu), kayıt ayarlarındaki akordiyon menüden ayrık/birleşik seçimi yapma ve kamerayı tam ekran yapma gibi somut UI akışlarını araca kodlayarak, her biri için çok hassas teorik tamamlanma süreleri elde ettim.

## Kazanımlar
- Görsel arayüz animasyonlarının video/frame akışından bağımsız bir time loop içine alınmasıyla daha istikrarlı bir UX elde ettim.
- MFD arayüzümüzün düğme yerleşimleri, form giriş alanları ve genel mimarisi için bilimsel standartlara uygun, referans niteliğinde bir test altyapısı kurdum. Gelecekte tasarıma eklenecek yeni panellerin ne kadar ergonomik olacağını daha kodu yazmadan bu araçla test edebilme yetkinliği kazandım.

