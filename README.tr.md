# Android Hayalet Şarj Tüketimi Teşhis Aracı — Ücretsiz Batarya Sömürme Dedektörü

> **Hızlı Cevap:** Hayalet şarj tüketimi (halk arasında "şarjın kendi kendine bitmesi" veya "bataryanın su gibi akması"), ekranınız kapalıyken ve arka planda hiçbir uygulama çalışmıyor gibi göründüğünde meydana gelen batarya kaybıdır. Buna, cihazın derin uykuya (_deep sleep_) geçmesini engelleyen arka plan _wakelock_ işlemleri (uyandırma kilidi) neden olur. Battery Health Monitor bu hayalet tüketimi otomatik olarak algılar: Cihazınız ekran kapalıyken saatte %3'ten fazla şarj kaybederse, kaynağı bulmanız için doğrudan bir bağlantı içeren turuncu bir uyarı verir.

**[⬇ Battery Health Monitor'ü İndir — Google Play'de Ücretsiz](https://play.google.com/store/apps/details?id=com.ghost.drain.battery.health.monitor)**
_Ücretsiz. Root gerekmez. Veri toplama yok. Tamamen çevrimdışı çalışır._

---

## Android'de Hayalet Şarj Tüketimi (Ghost Drain) Nedir?

Hayalet şarj tüketimi, **anormal bekleme modu batarya kaybı** için kullanılan resmi olmayan bir terimdir. Yani telefon cepteyken, ekranı kapalıyken ve siz hiçbir şey yapmıyorken yaşanan batarya sömürülmesidir. Android'in normal derin uyku (_deep sleep_) tüketimi **saatte %1'in altındadır**. Hayalet tüketim ise genellikle **saatte %3 ila %8** arasındadır; bu da telefonun gece boyunca hiçbir şey yapmadan şarjının %30 ila %60'ını kaybetmesi (şarjın kendi kendine erimesi) anlamına gelir.

Bunun arkasındaki mekanizma bir **wakelock**'tır. Wakelock, yazılımın Android çekirdeğine (kernel) "derin uykuya geçme" talimatı vermesidir. Bir uygulama, sistem işlemi veya arka plan servisi ne zaman aktif bir _wakelock_ tutsa, işlemci kısmen açık kalır. Uygulama görev yöneticisinde "çalışmıyor" gibi görünür, ancak _wakelock_ çekirdek seviyesinde aktif kalmaya devam eder ve standart uygulama listelerinde görünmez.

**Hayalet şarj tüketimi, normal batarya eskidiyse (pil sağlığı düşmesi) yaşanan durumla aynı şey değildir.** Batarya yıpranması aylarca süren bir süreçte kademeli olarak gerçekleşir. Hayalet tüketim ise bir uygulama güncellemesi, sistem güncellemesi veya yeni bir uygulama yükledikten sonra aniden ortaya çıkar ve _wakelock_ kaynağı kaldırıldığında tamamen çözülür (geri döndürülebilirdir).

---

## Telefonumun Şarjı Gece Hiçbir Şey Açık Değilken Neden Bitiyor? (Şarj Neden Kendi Kendine Gidiyor?)

En yaygın hayalet şarj tüketimi nedenleri, sıklık sırasına göre şunlardır:

### 1. Hatalı güncellenmiş bir uygulamanın sürekli wakelock tutması

Hatalı (buglı) bir güncelleme alan uygulama, arka planda süresiz olarak _wakelock_ tutmaya başlayabilir. Bu, gece yatarken şarjın durduk yere düşmesinin en yaygın sebebidir.

**Teşhis:** Ayarlar → Batarya → Pil kullanımı → "Ekran kapalı" sütununu kontrol edin. Ekran kapalıyken %2'den fazla batarya tüketen her uygulama şüphelidir.

**Çözüm:** Uygulamanın arka plan etkinliğini kısıtlayın veya uygulamayı kaldırıp bir önceki sürümünü yükleyin.

### 2. Android sistem işlemi döngüsü (Yeniden başlatma döngüsü)

Agresif bir pil tasarrufu modu (Xiaomi HyperOS, Samsung One UI vb.) arka plandaki bir uygulamayı zorla kapattığında, uygulamanın "Boot Receiver" bileşeni onu tekrar tetikler. Sistem uygulamayı tekrar kapatır ve bu durum sürekli bir kapat-aç döngüsü yaratır. Her yeniden başlatma işlemciyi (CPU) yorar ve ekranda hiçbir hareket yokken bataryanın sömürülmesine yol açar.

**Çözüm:** Uygulamayı pil optimizasyonundan muaf tutun (üreticiye özel [Xiaomi](https://www.google.com/search?q=../Xiaomi-HyperOS-Battery-Drain-Fix) ve [Samsung](https://www.google.com/search?q=../Samsung-OneUI-Battery-Drain-Fix) kılavuzlarına göz atın).

### 3. Arka plan servislerinin sürekli GPS araması

Navigasyon, paket servis / kurye (Yemeksepeti, Getir, Trendyol vb.) ve fitness takip uygulamaları arka planda sürekli GPS konum kontrolü yapabilir. GPS, bir akıllı telefondaki en büyük güç canavarlarından biridir.

**Teşhis:** Ayarlar → Gizlilik → İzin yöneticisi → Konum → Hangi uygulamaların "Her zaman izin ver" şeklinde ayarlandığını kontrol edin.

**Çözüm:** Şüpheli uygulamaların iznini "Yalnızca uygulama kullanılırken izin ver" olarak değiştirin.

### 4. Çok sık aralıklarla çalışan senkronizasyon servisleri

E-posta uygulamaları, bulut yedekleme servisleri ve sosyal medya uygulamaları her birkaç dakikada bir sunucularını kontrol ederek telefonun "uyku moduna" girmesini engelleyebilir.

**Çözüm:** Ayarlar → Hesaplar → Kullanmadığınız hesaplar için arka plan senkronizasyonunu kapatın.

### 5. Always On Display (Her Zaman Açık Ekran) modu

AOD modu, AMOLED panellerde saatte %2 ila %5 arasında pil tüketebilir. Ana ekran işlemcisi teknik olarak kapalı olsa da ortam katmanı aktif olduğundan, bu durum batarya grafiklerinde ekran kapalı tüketimi olarak görünür.

---

## Arka Planda Bataryayı Sömüren Uygulama Nasıl Bulunur?

**Yöntem 1: Android'in Yerleşik Pil Kullanımı Ekranı**
Ayarlar → Batarya → Pil Kullanımı
Sıralama ölçütü: Son tam şarjdan bu yana
Aranacak şey: Arka planda %2'den fazla tüketim yapan uygulamalar

**Yöntem 2: Battery Health Monitor Hayalet Tüketim Uyarı Bandı**
Battery Health Monitor, ekran kapalıyken bataryanızın düşüş hızını ölçer. Tüketim saatte %3'ü geçtiğinde şunları gösterir:

- Anlık tüketim hızı (Örn: "Saatte %5.2")
- Android pil ayarlarına doğrudan bir kısayol bağlantısı
- Tüketimin tam olarak ne zaman başladığına dair zaman damgası

Bu yöntem yerleşik araçtan daha kullanışlıdır çünkü size sadece toplam harcamayı değil, **tüketim hızını** verir. Böylece hayalet tüketimi spesifik bir olayla (yeni yüklenen bir uygulama, sistem güncellemesi vb.) kolayca bağdaştırabilirsiniz.

**Yöntem 3: ADB (Gelişmiş Kullanıcılar İçin)**

```bash
adb shell dumpsys batterystats | grep "wake_lock"

```

Bu komut, Android ayarlarında görünmeyen sistem düzeyindeki kilitler de dahil olmak üzere, çekirdek düzeyinde o an aktif olan tüm _wakelock_ işlemlerini listeler.

---

## Gece Boyunca Ne Kadar Şarj Gitmesi Normaldir?

| Tüketim Hızı | Sınıflandırma                | Yapılması Gereken Aksiyon                           |
| ------------ | ---------------------------- | --------------------------------------------------- |
| Saatte < %1  | Normal derin uyku            | İşlem gerekmiyor                                    |
| Saatte %1–2  | Hafif bekleme modu kullanımı | Wi-Fi ve bildirimler aktifse normal                 |
| Saatte %2–3  | Yüksek tüketim               | Arka plan uygulamalarını kontrol edin               |
| Saatte %3–5  | Muhtemel hayalet tüketim     | Teşhis için Battery Health Monitor'ü kullanın       |
| Saatte > %5  | Şiddetli hayalet tüketim     | Acil — Muhtemelen hatalı uygulama veya çöken servis |

---

## Kullanıcı Türüne Göre Batarya Tüketimi — Özel Çözümler

### 📱 Oyuncular (Gamers) — Oyun Esnasında ve Sonrasında Şarjın Hızla Bitmesi

Oyun seansları yüksek ısı üreterek batarya ömrünü kısaltır ve şarjı normalden 3 ila 8 kat daha hızlı tüketir. Oyuncuların yaptığı en büyük hatalar:

- **Oyun oynarken telefonu şarj etmek** (%80'in üzerindeyken): Aynı anda hem oyun oynamanın hem de şarj etmenin yarattığı ısı, batarya kapasitesini kalıcı olarak öldürmenin en kısa yoludur.
- **Gece boyu %100'de şarjda bırakmak**: Bataryayı saatlerce maksimum voltaj stresinde tutarak yaşlanmayı hızlandırır.

**Oyuncu çözümü:** Oyun oynarken Battery Health Monitor'ün şarj alarmını %90'a ayarlayın. Böylece %100 voltaj stresine girmeden güvenli bir sınırda kalırsınız. 42°C'deki termal uyarı ise size ne zaman oyuna ara vermeniz ve bir sonraki maça geçmeden önce telefonu soğutmanız gerektiğini söyler.

---

### 🚗 Kuryeler ve Sürücüler (Uber, Getir, Yemeksepeti, Trendyol, BiTaksi) — Telefonu Tüm Gün Canlı Tutmak

Kuryeler ve taksi sürücüleri, 8-12 saatlik vardiyaları boyunca navigasyonu, aramaları ve ekranı sürekli açık tutarlar. Temel sorunlar:

- **Araç şarj cihazının ekran tüketiminden daha az akım vermesi:** Telefonunuz şarjda olmasına rağmen pili azalmaya devam eder. Battery Health Monitor, araç şarjınızın telefona yetip yetmediğini görmeniz için anlık watt değerini canlı gösterir.
- **Konsolda güneş altında durmaktan kaynaklanan ısı:** Vardiya sırasında 45°C'nin üzerindeki sıcaklıklar, haftalar içinde kalıcı batarya kapasitesi kaybına yol açar.
- **Çakma / Sahte araç şarj cihazları:** Battery Health Monitor'ün şarj cihazı doğrulama özelliği tarafından tespit edilen kararsız akım dalgalanmaları (>200mA sapma).

**Sürücü çözümü:** Telefonunuzu doğrudan güneş ışığı alan yerlere değil, mümkünse klima ızgarasının önüne monte edin. En az 15W+ gücünde kaliteli bir araç şarjı kullanın (Battery Health Monitor ile test edin) ve her zaman acil durum yedeğiniz olması için düşük şarj alarmını %30'a ayarlayın.

---

### 👴 Standart Kullanıcılar — "Telefonum Artık Akşama Kadar Dayanmıyor"

Eğer 2-3 yıllık telefonunuz eskiden tüm gün gidiyorken artık öğleden sonra saat 3 gibi kapanıyorsa, bu durum %99 yazılımsal bir sorun değil, bataryanın eskimesidir (degradasyon).

Battery Health Monitor'ün sağlık kalibrasyonu, 3 başarılı şarj döngüsünden sonra telefonunuzun **mAh cinsinden gerçek kalan kapasitesini** hesaplar. 4000mAh'lik bir telefonunuz şu an sadece 2800mAh (yani %70 pil sağlığı) tutabiliyorsa, tam günlük bataryanızın neden yarım güne düştüğü net bir şekilde açıklanmış olur.

**Pil sağlığı %80'in altına düştüğünde, Türkiye'de batarya değişim ücreti ortalama ₺400–₺1200 arasındadır. Bu bedel, yeni bir telefon satın alma maliyetinin yanında devede kulak kalır.**

## Ücretsiz AccuBattery Alternatifi — Battery Health Monitor Neden Var?

AccuBattery, 10 milyondan fazla indirme ile Android'deki en popüler batarya uygulamasıdır. Ancak en kullanışlı iki özelliği ne yazık ki ödeme duvarının arkasındadır (Pro versiyon gerektirir):

- **Şarj alarmı (%80 sınırı):** AccuBattery Pro aboneliği gerektirir
- **Detaylı şarj geçmişi:** AccuBattery Pro gerektirir

Battery Health Monitor bu özellikleri tamamen ve kalıcı olarak ücretsiz sunar:

- ✅ Döngüsel %80 şarj alarmı (Siz fişten çekene kadar çalar — tek seferlik bildirim değildir)
- ✅ Grafik trendiyle birlikte 7 seanslık detaylı şarj geçmişi
- ✅ Tüketim hızı ölçümlü hayalet şarj algılama
- ✅ Gerçek siyah karanlık mod (AMOLED ekranlar için optimize edilmiştir)
- ✅ 38°C ve 42°C sıcaklık uyarıları
- ✅ Akım dalgalanması analiziyle kalitesiz / çakma şarj aleti tespiti
- ✅ Sıfır veri toplama — tüm analizler tamamen cihazınızın içinde yapılır

---

## Sıkça Sorulan Sorular (FAQ)

**S: Hayalet şarj tüketimi ile normal pil eskimesi arasındaki fark nedir?**
C: Pil eskimesi (aşınması), yüzlerce şarj döngüsü boyunca bataryanın kimyasal kapasitesinin kademeli olarak azalmasıdır. Maksimum pil ömrünüzü kalıcı olarak düşürür ve geri döndürülemez. Hayalet şarj tüketimi ise yazılımdan kaynaklanan anormal bir güç harcamasıdır; aniden başlar, genellikle şiddetlidir (saatte %3-8 düşüş) ve yazılımsal sorun çözüldüğünde pil tüketimi tamamen normale döner.

**S: Telefonumun şarjı geceden sabaha %40 düşüyor. Pilim bitti mi, öldü mü?**
C: Şart değil. 7-8 saatlik bir uyku süresinde %40 şarj kaybı, saatte yaklaşık %5-6 düşüş demektir. Bu durum şiddetli bir hayalet şarj tüketimine (arka planda pili sömüren bir şeye) işarettir, pilin tamamen eskidiğini göstermez. Eskiyen bir pil maksimum kapasiteyi düşürür ancak bekleme modunda bu derece "şarj yemez". Pili değiştirmeyi düşünmeden önce Battery Health Monitor yükleyerek ekran kapalıyken tüketim hızınızı ölçün.

**S: Battery Health Monitor, hayalet tüketimi bulmak için root erişimi ister mi?**
C: Hayır. Hayalet şarj tüketimi tespiti için yalnızca standart Android sistem araçları (`ACTION_BATTERY_CHANGED` sinyalleri ve ekran durumu alıcıları) kullanılır. Root gerekmez, ADB gerekmez, standart batarya izleme dışında hiçbir tuhaf izin istenmez.

**S: Bu uygulamayı AccuBattery ile aynı anda kullanabilir miyim?**
C: Evet. İki uygulama da aynı Android batarya sistem altyapısını kullanır ve birbiriyle çakışmaz. Ancak arka planda aynı anda iki farklı batarya izleme servisi çalıştırmanın arka plan kaynak kullanımını çok az da olsa artıracağını unutmayın.

**S: Uygulama Android 8 ile Android 15 arasındaki sürümlerde çalışır mı?**
C: Evet. Minimum desteklenen sürüm Android 8.0'dır (API 26). Hassas alarm planlaması, ön plan servisleri ve Android 8 ile gelen bildirim kanalları dahil tüm özellikler Android 8'den Android 15'e kadar sorunsuz çalışmaktadır.

---

**[⬇ Battery Health Monitor'ü Google Play'den İndir — Ücretsiz](https://play.google.com/store/apps/details?id=com.ghost.drain.battery.health.monitor)**

_Takip kodu yok. Veri toplama yok. %100 cihaz içi analiz._

---

_Keywords: android hayalet sarj tuketimi çözümü, telefonun sarji gece kendi kendine bitiyor, ekran kapaliyken pil tuketimi android, wakelock batarya sömürmesi, arka planda sarj yiyen uygulamalari bulma, ücretsiz accubattery alternatifi android, android derin uyku pil sorunu düzeltme, telefon batarya teshis araci, hayalet sarj dedektörü android uygulama_

_Additional Search Keywords:_

- şarjım kendi kendine düşüyor
- telefon durduğu yerde şarj yiyor
- gece şarj neden çok düşer
- telefonun bataryası su gibi akıyor
- arka planda şarj tüketen uygulamaları görme
- şarjı ne bitiriyor android
- telefonu uykuda şarj yemesi
- pil ömrü neden hızlı bitiyor
- akıllı telefon batarya kalibrasyonu ücretsiz
- telefon şarjı neden çabuk biter
