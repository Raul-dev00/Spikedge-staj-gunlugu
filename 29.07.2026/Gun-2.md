# Tarih: 29 Temmuz 2026

## Yapılan Çalışmalar
Proje içerisindeki "Storage Quota" (Depolama Kotası) yönetim sistemini kodlayarak dinamik hale getirmek ve kota dolduğunda arkaplanda kaydı otomatik durduracak yapıyı kurgulamak amacıyla aşağıdaki geliştirmeleri yaptım:

1. **Dinamik Kota Girişi Eklentisi:** 
   - Arayüzde `PAGE_storage.cpp` içerisindeki kota giriş alanına (QLineEdit) `QDoubleValidator` sınıfını entegre ettim. Uygulamayı test ederken 0.001 GB (yaklaşık 1 MB) gibi hassas kotalarla hızlı denemeler yapabilmek için sisteme ondalıklı sayı desteği kazandırdım.

2. **Arka Plan Kota Kontrolü (QTimer) ve Güvenli Durdurma:**
   - Video kaydı devam ederken disk boyutunun anlık takip edilebilmesi için `Recorder` sınıfına 5 saniyede bir tetiklenen bir `QTimer` (`quotaTimer`) entegre ettim. Bu zamanlayıcı üzerinden `MediaManager::enforceQuota()` fonksiyonunu çağırarak klasör boyutunu arka planda denetleyen algoritmayı yazdım.
   - **Karşılaşılan Problem:** "Action When Full" seçeneği olarak "Yeni kayıt alınmasını engelle" ayarlandığında, disk dolduğu saniye kaydın durması ve program çökmeden arayüzün uyarı (`QMessageBox`) vermesi gerekiyordu.
   - **Çözüm:** Arka plan `QTimer` nesnesi, `MediaManager`'dan dönen kapasite uyarısına göre `pclose()` çalıştırarak mevcut videoyu mühürleyecek ve kullanıcıya kilitlenmeden uyarı fırlatacak güvenli bir kontrol bloğu tasarladım.

3. **Signal-Slot (Sinyal-Yuva) ile Arayüz Senkronizasyonu:**
   - Kota dolduğunda arka plan süreci (thread) kaydı otomatik olarak kesiyordu ancak UI tarafındaki "Stop Recording" butonunun durumdan haberdar olup kendini "Start Recording" şekline döndürmesi gerekiyordu.
   - Bu kopukluğu çözmek için `Recorder` sınıfına `recordingStopped()` isminde özel bir sinyal tanımladım ve bu sinyali doğrudan butonun durumunu değiştiren arayüz slot fonksiyonuna bağladım (connect).

## Kazanımlar
- Qt ekosisteminde UI bileşenlerini sayısal verilere kısıtlamak için `QDoubleValidator` mantığını deneyimledim.
- Arka planda okuma/yazma gerektiren (klasör boyutu hesaplama gibi) ağır süreçlerin `QTimer` kullanılarak arayüzü (GUI) kilitlemeden belirli aralıklarla çalıştırılmasının önemini kavradım.
- Farklı sınıflar (`Recorder` ve `ACTION_section`) arasındaki olay tabanlı haberleşmeyi (event-driven communication) Signal/Slot mimarisi ile kusursuz senkronize etmeyi öğrendim.
