# Tarih: 1 Eylül 2026

## Yapılan Çalışmalar
Bugün arayüzdeki fonksiyonelliği artırıp, karmaşık yapıları daha kullanıcı dostu bir düzene kavuşturdum. Özellikli olarak sıralama mantıkları ve Layout sekmesinin baştan yapılandırılması üzerine çalıştım:

1. **Medya Sıralama Özelliğinin Eklenmesi:**
   - Medya sekmesindeki filtreleme paneline "Sort by:" etiketli bir açılır menü entegre ettim.
   - Kullanıcıların videoları tarihe, süreye ve isme göre sıralayabileceği gelişmiş bir algoritma yazdım. `QDateTime` dönüştürmeleri ve string çözümleme algoritmalarıyla liste verilerinin anlık olarak sıralanmasını sağladım.
   - Arayüzde spacing e genişlik yarlamaları ile daha profesyonel bir görünüm elde ettim. Menü elemanlarını İngilizceye çevirdim.

2. **Layout Sekmesinin QToolBox ile Yeniden Tasarlanması:**
   - Normalde sadece tek bir tıklama ataması yapılabilen MFD buton konfigürasyonunu "Single Click", "Double Click" ve "Long Click" olmak üzere üçlü yapıya çıkardım.
   - 3 farklı ayar grubunu aynı ekrana yığıp arayüzü boğmak yerine, Qt'nin `QToolBox` sınıfını kullanarak açılır-kapanır sekmeli bir tasarım inşa ettim.
   - Her sekmenin içine sol ve sağ ekranlar için 6'şarlı yapıları dinamik olarak kopyaladım.

3. **Gelişmiş JSON Konfigürasyon Yönetimi ve Geriye Dönük Uyumluluk:**
   - Ayarların kaydedildiği `mfd_layout.json` dosyasının kayıt şemasını bu yeni sekmeli yapıya uyumlu olacak şekilde (`action_single`, `action_double`, `action_long`) genişlettim.
   - `MFD_section.cpp` dosyasında geriye dönük uyumluluk kodu ekleyerek eski JSON yapılarındaki `action` anahtarlarının hata vermeden okunmasını ve üzerine yeni bir kayıt yapıldığında `action_single` olarak güncellenmesini sağladım.

## Kazanımlar
- Oldukça fazla sayıda giriş ve form elemanı barındıran menü yapılarını, `QToolBox` akordeon arayüzü sayesinde kullanıcıyı yormayan, son derece derli toplu bir tasarıma kavuşturmayı pratik ettim.
- C++ içerisinde standart sıralama algoritmalarını zelleştirerek tarih dönüşümleri ve kelime ayrıştırmaları gibi kompleks veri analizlerini bir arada çalıştıran güçlü ve hızlı bir filtreleme sistemi kurmayı tecrübe ettim.
- İleriye dönük kapsamlı değişiklikler yaparken, yazılımın mevcut kullanıcılar veya eski veriler için hata vermesini önleyecek geriye dönük uyumluluk kodlarının önemini ve kurgulanmasını daha iyi kavradım.

