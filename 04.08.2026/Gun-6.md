# Tarih: 04 Ağustos 2026

## Yapılan Çalışmalar

**Proje Sorumlusuyla Değerlendirme Toplantısı:**
   - Sabah saatlerinde proje sorumlusu ile bir araya gelerek projenin güncel durumunu değerlendirdik. Şu ana kadar arayüzde ve altyapıda yaptığım tüm geliştirmeleri detaylıca aktardım. İlerleyen süreçte arayüze ekleyeceğimiz yeni özellikler (tam ekran modları, grid yapıları, buton durumları, yeni ayarlar) ve yapacağımız iyileştirmeler üzerine planlama yaptık.

Arayüz düzenlemeleri, FullScreen modunun düzgün çalışması ve dinamik grid yapısının kontrol altına alınmasına dair aşağıdaki çalışmaları gerçekleştirdim:

1. **FullScreen / Windowed Mod Geçiş Altyapısı:**
   - Tam ekrana geçtiğimde tüm uygulama beraberinde büyüyor, sadece MFD ekranının ana odak yapılıp diğer menülerin gizlenmesini sağlayamıyordum. `include/util.h` içerisindeki `GlobalData` yapısına `sectionUpperWidget`, `sectionPanoramaWidget` ve `sectionActionWidget` isimli `QWidget*` pointer'ları ekledim. `main.cpp`'de bu bölümleri oluştururken bu pointer'lara kayıt ettim. Böylece `MFD_section.cpp` içerisinden `hide()` ve `show()` fonksiyonlarıyla diğer panelleri doğrudan kontrol edilebilir hale getirdim.

2. **Grid Layout Üzerinde Boşluk Hatalarının Çözümü:**
   - Diğer menü panellerini (Action ve Upper) gizlediğimde, `QGridLayout` o menülerin kapladığı alanları tutmaya devam ediyordu; bu da MFD ekranının sola yaslı kalmasına (sağda %20 boşluk) ve üstte boşluk kalmasına sebep oluyordu. `MFDStateFullScreen()` içine `QGridLayout` span değerlerini dinamik olarak güncelleyen kodlar ekledim. Tam ekrana geçildiğinde `SECTION_MFD`'nin kolon genişliğini 80'den 100'e (`GlobalData::x + GlobalData::y`), başlangıç satırını ise `z`'den `0`'a çektim. Bu müdahaleyle MFD'nin boşalan tüm alanları işgal ederek ekranı kusursuz bir şekilde ortalamasını sağladım.

3. **Panorama Bölümünün Dinamik Genişletilmesi:**
   -  MFD tam ekranda genişlerken, altta görünür halde kalan Panorama bölümü eski genişliğinde (80 birim) dar kalıyordu. MFD için uyguladığım dinamik alan genişletme mantığını (grid->addWidget) `sectionPanoramaWidget` için de birebir uyguladım. Tam ekran modunda onun da alanının 100 birime yayılmasını sağladım. Pencereli moda (Windowed) döndüğümde her iki panelin de eski standart oranlarına (80 birime) geri dönmesini ayarladım.

4. **Yeni Buton Durumları (State Machine) Eklenmesi:**
    Kayıt ve Joystick özelliklerinin kontrolü için QStateMachine altyapısını genişlettim. `RightButton_1` için Kayıt mekanizmasını (`stateMachine_7`: "Record" ve "Stop Recording") ve `RightButton_5` için Joystick mekanizmasını (`stateMachine_11`: "Joystick" ve "Joystick Off") sisteme kazandırdım. İlgili buton geçişlerinde ekran yenilemesi (update) yaparak buton yazılarının (MFDStrings) anlık değişimini güvence altına aldım.

## Kazanımlar
- Qt'de `QGridLayout` yapısının "gizlenen" (hide) bileşenler üzerinden kolon boşluklarını nasıl rezerve etmeye devam ettiğini ve bu boşlukların span değerlerinin dinamik olarak değiştirilmesiyle (`grid->addWidget` kullanarak) nasıl sıfırlanıp yeniden boyutlandırılabileceğini derinlemesine öğrendim.
- Layout içerisindeki bir widget'ı tam ekrana yaymak için, ebeveyn (parent) üzerinden layout yapısına ulaşıp (`this->parentWidget()->layout()`) esneklik ayarlarını runtime anında kontrol etmenin Qt tarafında ne kadar güçlü ve temiz bir yöntem olduğunu deneyimledim.
- `QStateMachine` ile arayüze yeni state yapılarının (Record, Joystick vb.) entegre edilmesinin, buton mantığını ve ekran güncellemelerini (`update()`) ne kadar modüler ve okunabilir kıldığını bir kez daha pekiştirmiş oldum.

