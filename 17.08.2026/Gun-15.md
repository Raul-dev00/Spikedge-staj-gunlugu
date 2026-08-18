# Tarih: 17 Ağustos 2026

## Yapılan Çalışmalar
Bugün proje yöneticimiz ile genel durum ve değerlendirme toplantısı gerçekleştirdim. Toplantıda projede bugüne kadar tamamladığım tüm modülleri arayüz iyileştirmelerini ve altyapı çalışmalarını detaylı bir sunumla aktardım. Sunumun ve yapılan değerlendirmelerin ardından projenin ilerleyen fazları için aşağıdaki aksiyon planı ve gereksinimler karara bağlandı:

1. **MFD Butonlarının Dinamikleştirilmesi ve JSON Altyapısı:**
   - MFD üzerindeki butonların sabit kalmaması yerlerinin ve işlevlerinin tamamen değiştirilebilir olması gerektiği kararlaştırıldı.
   - Bu buton konfigürasyonlarının kaynak koda gömülü olmak yerine tamamen metin bazlı tutulması ve sistemin bu JSON dosyasını okuyarak arayüzü dinamik olarak inşa etmesi hedeflendi.

2. **Panoramik Görüntü Kaydının Sisteme Eklenmesi:**
   - Mevcut medya kayıt altyapısına ek olarak panoramik görüntü formatlarının da sisteme entegre edilerek kayıt alınabilmesi istendi.

3. **Kod Entegrasyonu :**
   - Şu ana kadar parça parça veya modüler olarak yazılmış tüm kodların tam entegre bir biçimde birbirini engellemeden birlikte yürütülecek şekilde birleştirilmesine karar verildi.

4. **Kanal Ekleme ve Çıkarma Özelliği:**
   - Sistemin tek veya çift kanalla sınırlı kalmaması istenildiğinde arayüz veya altyapı üzerinden yeni görüntü kanallarının eklenebilmesi veya mevcut kanalların çıkarılabilmesi gerektiği vurgulandı.

5. **Webcam Üzerinden Gerçek Zamanlı Testler:**
   - Geliştirilen görüntü alma işleme ve kanal yönetimi sistemlerinin harici veya otonom kamera donanımları beklenmeden doğrudan laptop'un dahili web kamerası kullanılarak yoğun bir şekilde test edilmesi kararlaştırıldı.

6. **UI/UX Araştırması:**
   - Kullanıcının sistemle etkileşimini en üst düzeye çıkarmak ve daha kurumsal bir his yaratmak adına modern UI/UX tasarım standartları üzerine akademik veya sektörel makale araştırması yapılması görevini aldım. Toplantının hemen ardından vakit kaybetmeden UI/UX makale araştırması sürecine başladım ve arayüzde nelerin iyileştirilebileceğine dair teorik notlar çıkardım.

7. **Araştırma Temelli Test Betiği Hazırlığı:**
   - Yazılımın güvenilirliğini kanıtlamak için kapsamlı bir test betiği yazılmasına karar verildi.
   - Bu test kurgusunun yalnızca rastgele değil UI/UX makalelerinden elde edilen araştırma bulgularıyla temellendirilerek bilimsel ve teknik bir metodolojiye dayandırılması hedeflendi.

## Kazanımlar
- Teknik geliştirmelerin başarılı olmasının yanı sıra projenin kullanıcı deneyimi standartlarına uygunluğunun kurumsal projelerde ne kadar belirleyici olduğunu bu yüzden arayüz tasarımlarının bilimsel makaleler ve sektörel standartlarla temellendirilmesi gerektiğini anladım.
- Buton konfigürasyonlarını JSON gibi esnek veri yapılarıyla dışa aktarmanın kodu yeniden derlemeden arayüzün saniyeler içinde baştan aşağı değiştirilebilmesine olanak tanıyacağını ve kodun sürdürülebilirliğini inanılmaz ölçüde artıracağını gördüm.
- Geliştirme test ve entegrasyon süreçlerinin projenin ileriki donanım entegrasyonu aşamalarında yaşanabilecek büyük sorunları daha en başından çözecek çok doğru bir mühendislik yaklaşımı olduğunu bizzat deneyimledim.

