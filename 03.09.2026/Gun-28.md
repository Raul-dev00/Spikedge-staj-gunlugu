# Tarih: 3 Eylül 2026

## Yapılan Çalışmalar
Bugün, MFD projemizin kullanılabilirlik ve İnsan-Bilgisayar Etkileşimi test altyapısı üzerinde yoğunlaştım. Arayüzümüzün kullanım kolaylığını ve verimliliğini matematiksel olarak ölçümleyebilmek adına önemli adımlar attım.

1. **KLM/GOMS Test Senaryolarının Finalize Edilmesi:**
   Arayüz üzerindeki en kritik ve sık kullanılan 12 farklı işlemi tespit ettim. Bu işlemler; tam ekrana geçişten kanal eklemeye, medya paneli kullanımından kayıt modlarını değiştirmeye kadar geniş bir yelpazeyi kapsıyor.

2. **UI Mimarisinin Senaryolara Doğru Yansıtılması:**
   `PAGE_layout.cpp` dosyasını inceleyerek arayüz bileşenlerinin hiyerarşisini doğruladım. Özellikle Settings > Layout sekmesindeki `QToolBox` yapısının (Single Click sekmesinin varsayılan olarak açık gelmesi vb.) KLM analizlerimize doğru yansıması için senaryolarda düzeltmeler yaptım. Bu sayede hesaplanan zihinsel ve fiziksel yük gerçeğe en yakın haline ulaştı.

3. **`klm_calculator.py` Güncellemeleri:**
   - Eski ve deneme amaçlı yazılmış testleri koddan tamamen temizledim.
   - 12 temel test senaryosunu gerçek buton mesafeleri ve boyutları ile koda entegre ettim.
   - Pencereli ve Tam Ekran modlarını analizden çıkardım; çünkü uygulamanın temel kullanım hedefi olan **Maximized** ekran boyutu bizim için tek anlamlı veri setini sunuyordu.
   - Fitts Yasası hesaplamaları, tüm hedefler ve M, P, K, H, T (düşünme, fareye/klavyeye geçiş, tıklama, klavye girişi) operatörleriyle hatasız şekilde hesaplanır duruma geldi.

4. **Test Dokümantasyonunun Eklenmesi:**
   Hesaplanan bu 12 senaryoyu detaylı bir şekilde derleyerek proje dokümanına yerleştirdim. İlgili raporda oluşturduğum **Tablo 1** üzerinden, ölçülecek 12 senaryonun amaçlarını ve adım adım hangi KLM dizilimlerine (`M P K H T`) denk geldiklerini detaylıca sundum.

Artık KLM testlerinin hem kod tabanlı otomatik hesaplayıcısı hem de düzgün bir metin dokümantasyonu elimizde mevcut. Bir sonraki aşamada bu test altyapısını GOMS analizleriyle detaylandırabilir veya doğrudan kullanıcı testlerine geçebilirim.

## Kazanımlar
- Arayüz test sürecimiz için hazırlanan KLM altyapısını 12 kritik kullanıcı senaryosuna uyarlayarak, tasarımımızın kullanılabilirliğini sayısal metriklere dönüştürebilme yetkinliği elde ettim.
- MFD arayüzünde QToolBox gibi gömülü (nested) menü yapılarının ve "Maximized" standart kullanım ortamının bilişsel/fiziksel (M, P, K vb.) yüklerini analiz edebilme kapasitesi kazandım.
- Kod tabanlı test senaryolarını dokümanda sistemli bir şekilde belgeleyerek, teorik altyapıyı pratik dokümantasyonla başarılı bir şekilde birleştirdim.
