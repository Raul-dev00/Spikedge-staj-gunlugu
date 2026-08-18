# Tarih: 13 Ağustos 2026

## Yapılan Çalışmalar
Kullanıcıya yönelik hazırlanan arayüz raporu doğrultusunda sistemin genel yapısı ve modüllerin işleyişi basit bir dille özetlendi. Rapor içeriğindeki temel özellikler şunlardır:

1. **Arayüz ve Tam Ekran Modu:**
   - MFD ekranına tam ekran, kayıt ve joystick butonları atandı.
   - Windowed butonuna basıldığında uygulama tam ekran moduna geçiş yapıyor.

2. **Kayıt İşlemleri:**
   - Record butonuna basılarak kayıt başlatılıyor.
   - Depolama alanı veya kota dolduğunda uyarı ekrana geliyor ve kayıt durduruluyor.

3. **Sanal Joystick:**
   - Joystick penceresi fareyle sürüklenip bırakılabiliyor.
   - Sürükleme işlemi özel çubuk üzerinden yapılıyor ve joystick son konumunu hafızada tutuyor.

4. **Dışa Aktarım:**
   - Media sekmesindeki dışa aktar butonu ile tüm kayıtlar cihaza bağlı USB belleğe aktarılabiliyor.
   - USB bağlantısı bulunmuyorsa veya birden fazla bellek takılıysa sistem kullanıcıyı yönlendiriyor.

5. **Depolama ve Kota Yönetimi:**
   - Kullanıcı kayıt klasörünü seçip kota üst sınırı belirleyebiliyor.
   - Kota dolduğunda en eski videoyu silmek en küçük boyutlu videoyu silmek veya yeni kaydı tamamen engellemek şeklinde üç farklı senaryo devreye giriyor.

6. **Erişilebilirlik ve Solak Modu:**
   - Ayarlar menüsünden solak modu aktif edildiğinde MFD ekranı aynalanarak kullanıcı deneyimi iyileştiriliyor.

7. **Kanal Kayıt Seçenekleri:**
   - Kullanıcı ekranda aktif olan kanalı dinamik olarak kaydedebiliyor veya statik kanalları seçebiliyor.
   - İki farklı kamera kanalı isteğe bağlı olarak ayrı ayrı veya tek bir videoda birleştirilmiş şekilde kaydedilebiliyor.

## Kazanımlar
- Teknik detaylardan arındırılmış kullanıcı odaklı raporların sistemin genel özelliklerini anlamak açısından son derece etkili olduğu görüldü.
- Geliştirilen arayüz modüllerinin birbirleriyle olan etkileşimi ve arayüz tepkileri son kullanıcı bakış açısıyla belgelenmiş oldu.

