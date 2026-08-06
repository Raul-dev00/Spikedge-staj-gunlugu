# Tarih: 05 Ağustos 2026

## Yapılan Çalışmalar
Recorder ile ilgili tetiklemeler, dosya isimlendirmeleri ve depolama alanı kontrollerine dair aşağıdaki çalışmaları gerçekleştirdim:

1. **Record (Kayıt) Butonunun Kontrolü:**
   - `ACTION_section.cpp` içindeki "Record" butonuna basıldığında tetiklenecek eylemlerin mevcut durumunu inceledim. İlgili butonun `recordButtonClicked` fonksiyonuna bağlandığını ve `recorder->start("test.mp4")` ile `recorder->stop()` işlemleri üzerinden çalıştığını teyit ettim.

2. **MFD Üzerinden Kayıt (Record) İşleminin Tetiklenmesi ve Pushbutton Ataması:**
   - Kayıt işlemlerinin sadece Action panelinden değil, doğrudan MFD üzerindeki butonlar üzerinden de kontrol edilebilmesini sağladım. `MFD_section.cpp` içerisinde `recorder` objesini başlatarak (`recorder = new Recorder(globalData);`), MFD sağ panel buton durumlarına entegre ettim. `MFDStateRecording()` metodunda kaydın başlatılmasını, `MFDStateStopRecording()` metodunda ise durdurulmasını kodladım.

3. **Depolama Alanı Pop-up (QMessageBox) Kontrollerinin Bulunması:**
   - Depolama (storage) veya kota (quota) dolduğunda ekranda otomatik çıkan pop-up uyarılarının altyapısını inceledim. Bu uyarıların `src/recorder.cpp` içerisinde barındırıldığını tespit ettim. Disk limiti dolduğunda kaydı durduran, kayıt başlatılamadığını bildiren ve kullanıcıya eski videoları silmek isteyip istemediğini soran `QMessageBox::warning` ve `QMessageBox::question` yapılarının lokasyonlarını belirledim.

## Kazanımlar
- Qt kütüphanesini kullanırken projeye yeni özellikler eklediğimde (örn. `QDateTime`), gerekli başlık (header) dosyalarını unutmanın doğrudan derleme hatalarına (`incomplete type` vb.) yol açtığını ve bu nedenle her yeni objede bağımlılıkları kontrol etmem gerektiğini tecrübe ettim.
- `QMessageBox` yapısının projede uyarılar ve karar onayları almak için `src/recorder.cpp` içerisinde nasıl konumlandırıldığını ve doğrudan `globalData.window`'a bağlı olarak nasıl çağrıldığını analiz ettim.
