# Tarih: 28 Temmuz 2026

## Başlangıç
- `qtcomp` projesinin kullandığım Ubuntu 22.04 işletim sistemine entegrasyonunu ve derlenmesini yaptım.
- GStreamer ve `ffmpeg` altyapılarının video kayıt süreciyle nasıl haberleştiğinin analiz edilmesiyle ilgilendim.

## Uyumluluk Analizi ve Ortam Kurulumu
1. - Projenin kaynak kodları inceleyerek bağımlılıkları tespit ettim ve Ubuntu 22.04 üzerinde gerekli kütüphaneleri kururarak derleme ortamı hazırladım. Ortamın hazırlanması için aşağıdaki komutu terminal üzerinde çalıştırdım:
   ```bash
   sudo apt install build-essential qtbase5-dev qtmultimedia5-dev qtdeclarative5-dev libqt5gamepad5-dev libevdev-dev libgstreamer1.0-dev libgstreamer-plugins-base1.0-dev
   ```

2. - Kurulumların ardından Qt5 projesinin `Makefile` dosyasını oluşturmak ve projeyi derleyip başlatmak için sırasıyla şu komutları kullanıldım:
   ```bash
   qmake qt5-test.pro
   make -j$(nproc)
   ./qt5-test
   ```

3. - Arayüzden gelen video karelerinin arka planda çalışan `ffmpeg` sürecine `pipe` yöntemiyle nasıl aktarıldığını incelendim.
   - "Stop Recording" işlemi sırasında, EOF (End of File) sinyalinin `pclose()` fonksiyonu ile `ffmpeg`'e iletilerek `.mkv` formatındaki video dosyasının 
mühürlenip izlenebilir hale getirilmesi mimarisini inceledim.
   - Video formatlarının (özellikle `.mp4` ve `.mkv`) sağlıklı kapanabilmesi için dosya header'larının ve metadata'larının tam yazılması gerektiği öğrenildi. 
Sürecin dışarıdan zorla kapatılmak yerine standart `pclose` çağrısı ile kontrollü sonlandırılması gerektiği doğrulandı ve uygulandı.

## Kazanımlar
- Linux ortamında C++ programları ile terminal komutları arasındaki süreçler arası iletişimin temellerini anladım.
- Qt5 platformunda donanım (gamepad vb. için `libevdev`) ve medya kütüphanelerinin (GStreamer) nasıl entegre edildiğini öğrendim.


