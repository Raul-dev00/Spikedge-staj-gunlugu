# Tarih: 14 Ağustos 2026

## 🛠 Yapılan Çalışmalar ve Aksiyonlar
Bugün, projede tamamladığım tüm modülleri, mimari kararları ve altyapı çalışmalarını detaylandıran kapsamlı bir teknik rapor hazırladım. Oluşturduğum bu rapor; Storage, Accessibility ve Channels modüllerinin arka plan mantığını, arayüz bağlantılarını ve hata kontrol algoritmalarını içermektedir. 

Hazırladığım teknik raporun içeriği ve detaylandırdığı temel başlıklar şu şekildedir:

1. **Storage (Depolama ve Kota) Modülü Geliştirmeleri:**
   - Kullanıcıların video kayıt dizinini seçebilmesi ve diskin doluluk oranının akıllı bir biçimde yönetilmesi için tasarlanan `PAGE_storage.cpp` sayfasının işleyişi. Raporda `QFileDialog` ile klasör seçme entegrasyonu anlatıldı. Kullanıcının disk kapasitesinden daha yüksek bir kota girmesini engellemek için `QStorageInfo` üzerinden diskin toplam boyutunun nasıl çekildiği ve `QDoubleValidator` ile girişlerin donanımın sınırına nasıl kilitlendiği açıklandı. Ayrıca `QProgressBar` vasıtasıyla anlık disk doluluk yüzdesinin hesabı ve kota dolduğunda sistemin vereceği 3 farklı tepkinin (En eski videoları sil, En küçük videoları sil, Kaydı durdur) `QSettings` yapılandırmasına nasıl bağlandığı belgelendi.

2. **Accessibility Altyapısı:**
   - Kullanıcı arayüzünün solak kullanıcılar için tek bir tıklamayla simetrik olarak tersine çevrilebilmesini sağlayan yapının teknik analizi. `PAGE_accessibility.cpp` içindeki "Left-Handed Mode" butonuyla Qt framework'ünün `Qt::RightToLeft` yönlendirme algoritmasının doğrudan MFD ekranına nasıl uygulandığı anlatıldı. Ekrandaki metinlerin (`globalData.MFDStrings`) butonlarla beraber simetrik kalabilmesi için indekslerinin çalışma zamanında nasıl takas edildiği raporlandı.

3. **Channels Mimarisinin Tamamlanması:**
   - Sistemin tek veya çoklu kameralardan nasıl kayıt alacağını yöneten `PAGE_channels.cpp` ve `recorder.cpp` mimarisinin çalışma prensibi.Raporda "Dynamic Record" modunun, kullanıcının o an izlediği kamerayı nasıl algılayıp kaydettiği detaylandırıldı. "Static" seçimlerle arka planda kameraların nasıl bağımsız kaydedilebileceği açıklandı. Son olarak, iki kameranın görüntüsünü tek videoda birleştiren "Combined/Composite Kayıt" algoritması anlatıldı; RAM üzerinde `QPainter` aracılığıyla genişletilmiş bir `QImage` tuvali yaratılıp, kameraların anlık olarak yan yana çizilerek tek bir FFmpeg pipe'ı üzerinden h265 formatında nasıl kaydedildiği teknik boyutta izah edildi.

4. **Media Modülü - Dışa Aktarım (Export to USB):**
   - Kaydedilen video ve önizleme (thumbnail) görsellerinin harici bir belleğe (USB) veya diske aktarım mekanizmasının teknik detayları. Raporda devasa video dosyaları `QFile::copy()` ile USB'ye kopyalanırken uygulamanın donmasını engellemek adına döngü içine yerleştirilen `QCoreApplication::processEvents()` yapısı detaylandırıldı. Kullanıcının aktarımı anlık olarak takip edebilmesi ve dilediği zaman iptal butonuna basarak kopyalama döngüsünü anında durdurabilmesi (`progress.wasCanceled()`) gibi kullanıcı dostu geliştirmeler belgelendi.

5. **MFD Modülü Entegrasyonları:**
   - MFD ana kontrol arayüzüne eklenen dinamik bileşenler ve esnek mimari kararları. Raporda MFD üzerine yerleştirilen kontrollerin işlevleri anlatıldı. Özellikle eklenen Sanal Joystick bileşeninin statik yerleşimden kurtarılarak ekranda serbestçe sürüklenebilir hale getirilmesi; ayrıca konumunun oransal bazda hesaplanarak diske kaydedilmesi gibi özelliklerin teknik analizine yer verildi.

## Kazanımlar
- Hazırladığım bu teknik raporla birlikte, kullanıcıdan alınan kısıtlayıcı verilerde donanımın sınırlarını saniyelik okuyarak arayüz girişlerini kısıtlamanın uygulamanın kararlılığını ne kadar artırdığını teorik olarak belgelemiş oldum.
- Qt'nin `LayoutDirection` özelliğinin uzun koordinat hesaplamaları yerine tüm widget ağacını tek satır kodla aynalayabilmesinin projenin sürdürülebilirliğine sağladığı mimari katkıyı yazıya döktüm.
- İki farklı görüntü akışını birleştirmek için donanımsal karmaşık bir render motoru yerine, RAM üzerinde `QPainter` ile hızlı çizim yapıp FFmpeg'e aktarmanın performans analizlerini rapora ekleyerek sistemin son derece senkronize çalıştığını kanıtladım.
