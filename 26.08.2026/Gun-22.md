# Tarih: 26 Ağustos 2026

## Yapılan Çalışmalar
Staj defterimi doldurduktan ve dünkü çalışmaların dokümantasyonunu tamamladıktan sonra, uygulamanın en kritik bileşenlerinden biri olan kayıt motorunu ve kanal seçim arayüzünü tamamen dinamik ve sınırsız ölçeklenebilir bir yapıya kavuşturdum. Aşağıdaki büyük çaplı mimari değişiklikleri gerçekleştirdim:

1. **Dinamik Kanal Arayüzü Tasarımı:**
   - Önceden "Settings > Channels" sekmesinde sadece `CH 1` ve `CH 2` şeklinde sabit onay kutuları bulunuyordu. Sistemi, "Connection" sayfasından eklenen kanal sayısını okuyarak anlık olarak 3, 4 veya 10 kamera için sonsuz ölçeklenebilen dinamik bir onay kutusu listesi oluşturacak şekilde baştan yazdım. Bağlantı sayfasında bir kamerayı silip eklediğimde eski arayüz widget'larının havada asılı kalarak uygulamanın çökmesine sebep olduğunu fark ettim. Çözüm olarak arayüz güncelleme (`reloadUI`) işlemi sırasında widget'ları tek tek silmek yerine, hepsini barındıran üst `QFrame` çerçevesini belleğe iade edip baştan pırıl pırıl çizen güvenli bir bellek temizleme mantığı kurguladım.

2. **Çoklu Kamera Ayrık Kayıt Mekanizması:**
   - Artık kullanıcı 4 farklı kamerayı işaretleyip "Ayrı Ayrı Kaydet" seçeneğini seçtiğinde, `Recorder` sınıfı dinamik olarak işaretlenen kamera sayısı kadar eşzamanlı `ffmpeg` süreci başlatıyor ve bunları `_CH1.mkv`, `_CH2.mkv` şeklinde ayrı bağımsız dosyalar olarak diske sorunsuzca yazıyor.

3. **Gelişmiş Izgara Kayıt Sistemi:**
   - Kullanıcının seçtiği n adet kameranın tek bir video dosyasında toplanması için mükemmel bir matematiksel altyapı inşa ettim. Sistem, işaretlenen kamera sayısının karekökünü alarak en uygun ızgara boyutunu hesaplıyor. `QImage` ve `QPainter` kütüphanelerini kullanarak seçili tüm kameraların yayınlarını bu devasa siyah kanvasın üzerine milisaniyeler içerisinde anlık olarak işliyor ve tek bir `ffmpeg` işlemine gönderiyorum. Eğer 3 kamera seçilirse 2x2'lik ızgaranın 4. slotu mükemmel bir şekilde boş bırakılarak video bütünlüğü sağlanıyor.

4. **Panorama Modülündeki Kilitlenme Zafiyetinin Giderilmesi:**
   - Bağlantı sayfasında kanal ekleyip sildiğimde Panorama görüntüsünün donup kaldığını fark ettim. Panorama sisteminin, yayınları doğrudan GStreamer alıcısına (`GStreamerReceiver`) bir kanca atarak çektiğini tespit ettim. Bağlantılar sıfırlandığında eski alıcı silindiği için panorama boşlukta kalıyordu. Çözüm olarak, kameralar sıfırlanmadan hemen önce Panorama'ya yayın bağını koparmasını (`stopLive`), kanallar oluşturulduktan sonra da yepyeni kancayı atmasını (`startLiveClicked`) emreden senkronize bir yaşam döngüsü kurguladım. Çökmeler ve donmalar tarihe karıştı.

## Kazanımlar
- Qt mimarisinde kompleks Layout'ları silerken dikkatli olunmazsa C++ tarafında bellek sızıntıları ve "Use-after-free" çökmeleri yaşanabileceğini; en temiz çözümün üst kapsayıcı nesneyi silmek olduğunu pratik ederek pekiştirdim.
- Çok kanallı dinamik video oluşturma süreçlerinde, çerçeve boyutlarını ve ızgara koordinatlarını matematiksel olarak otonom hesaplamanın sistemin sonsuz büyümesine olanak tanıdığını gözlemledim.
- Bellekteki Pointer'ların yok edilmeden önce izole edilmesi gerektiği aksi takdirde asenkron çalışan modüllerin ölümcül hatalara yol açabileceğini gerçek bir "Debugging" seansıyla çözüme kavuşturdum.
- Bugün yaptığım yoğun kodlamalar sonrasında oluşturduğum staj defteri yazımları ve dokümantasyonlar, ileride projeyi devralacak ekip için paha biçilemez bir referans kaynağı oldu.

