# Tarih: 03 Ağustos 2026

## Yapılan Çalışmalar
Kullanıcı deneyimini iyileştirmek, arka planda yaşanan veri kayıplarını engellemek ve sistemdeki klasör/önbellek yönetiminde tespit edilen hataları gidermek için aşağıdaki çalışmaları gerçekleştirdim:

1. **Manuel "Save" Butonlarının Kaldırılması ve Otomatik Kayıt Entegrasyonu:**
   - Depolama (Storage) ayarları sayfasındaki verilerin manuel bir butonla kaydedilmesi kullanıcı deneyimini zorlaştırıyordu. Butonlar kaldırılmış ancak ayarların kaydedilmesi devre dışı kalmıştı. Bu yüzden `PAGE_storage.cpp` dosyasındaki metin kutuları (`recordPathInput`, `quotaInput`) ve açılır menülere (`actionCombo`) ait değişiklik sinyallerini, sistemin genel `saveTimer` zamanlayıcısına bağladım. Ayarlar değiştiği an arka planda tek bir tuşa basmaya gerek kalmadan otomatik ve verimli bir şekilde `QSettings`'e yazılmasını sağladım.

2. **Gizli Dosya Silme Mekanizmasının Kullanıcı Onaylı Modele Dönüştürülmesi:**
   - Disk kotası dolduğunda, sistem kullanıcının haberi olmadan eski kayıtları arka planda `enforceQuota` üzerinden anında siliyordu. Bunun için olayı ikiye bölerek `checkQuota()` ve `freeUpSpace()` adında yeni bir mimari kurdum. Kayıt esnasında alan dolarsa video sessizce silinmiyor, kayıt hemen durdurulup uyarı veriliyor. Yeni bir kayda basıldığında disk doluysa ekrana "Alanınız dolu, eski videonuz silinerek yer açılsın mı?" onay penceresi (`QMessageBox`) çıkardım. Kullanıcı inisiyatifi ve güvenliğini maksimuma çektim.

3. **Eski "Compress" (Sıkıştırma) Kodlarının Tamamen Temizlenmesi:**
   - Önceki çalışmalarda tasarlanan "Sıkıştırılmış video kalsın" mantığı iptal edilmesine rağmen, kodlar (`compressed_` önek tespiti vs.) hala sistemde duruyor ve kirlilik yaratıyordu. Çözüm için `MEDIA_section.cpp` ve ilgili dosyalarda "compress" ifadesine veya özelliğine dair tüm parçaları (string parçalamaları ve arayüz etiketlemelerini) kalıcı olarak kaldırdım, kod bloklarını temizledim.

4. **"Hayalet Video" ve Klasör Güncelleme (Path) Hatasının Çözümü:**
   - Depolama sekmesinden yeni ve olmayan bir kayıt yolu (ör: `records/b`) girildiğinde, `MediaManager` bu durumu yakalayamıyor, eski yollara göre işlem yapıyordu. Bu durum, `.thumbnails` dizininin oluşmamasına ve arayüzde silinemeyen/oynatılamayan önizlemesiz hayalet videoların kalmasına sebep oluyordu. Bunu çözmek için `main.cpp` dosyasındaki asıl kayıt fonksiyonu `syncSave()` içerisine `mediaManager->updatePaths()` komutunu ekleyerek, ayar değiştiği an tüm sistemin yol bilgisini sarsarak güncellemesini sağladım. `readCache()` fonksiyonuna hata durumlarında eski arayüz listesini anında temizleyecek garantör kod satırları (`globalData.videoItems.clear()`) ve kayıt başlamadan klasörleri otomatik yaratacak kontroller (`mkpath`) ekleyerek zincirleme bug'ı tamamen kökünden kazıdım.

## Kazanımlar
- Qt'de `QSettings` ve tek atımlık (SingleShot) Timer mimarisi kurmanın, kullanıcı eylemlerini arka planda asenkron olarak nasıl zarifçe kaydedebileceğini yakından inceledim.
- Cache dosyaları yazılırken ve okunurken yapılabilecek "return" hatalarının, bellekte çöp veriler bırakarak UI hatalarına yol açabildiğini deneyimledim. Değişken temizliğinin her koşulda önemini kavradım.

