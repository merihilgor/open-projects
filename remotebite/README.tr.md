English: [README.md](README.md)

# RemoteBite: Akıllı Televizyonunuz İçin Gizli ve Çevrimdışı Yapay Zekâ Sesli Kontrol

## Özet

**RemoteBite**, temel özelliği **cihaz üzerinde çalışan yapay zekâ sesli kontrolü** olan bir akıllı telefon TV kumandasıdır. Kullanıcı doğal bir dille konuşur, örneğin *"sesi kapat, 5. kanala geç, sonra rehberi aç"* ya da *"televizyonuma bağlan"* der. Uygulama isteği anlar, doğru kumanda adımlarına böler, bunları televizyona gönderir ve sesli olarak onaylar.

Konuşma tanıma ve dil anlama işlemlerinin tamamı **telefonda** yapılır. Model bir kez indirildikten sonra sesli kontrol tamamen çevrimdışı çalışır. Ses kaydı cihazdan çıkmaz, kullanım verisi hiçbir sunucuya gönderilmez ve sunucu maliyeti oluşmaz. Anlama işini **hibrit bir motor** üstlenir: önce hızlı kurallar çalışır, kuralların çözemediği ifadeler için küçük bir yerel dil modeli (**LFM2.5-350M**) devreye girer. İsteğe bağlı olarak telefonun yerleşik konuşma tanıyıcısı yerine çevrimdışı çalışan bir **Whisper** tanıyıcısı kullanılabilir.

Nihai hedef **"sıfır dokunuş" (zero-click) kontroldür**: uygulamadaki her işlem yalnızca sesle yapılabilir. Buna televizyonu bulup bağlanmak, bekleme modundan uyandırmak, kanalı adıyla değiştirmek, herhangi bir uygulama ayarını değiştirmek ve ekranlar arasında geçmek dahildir. Fikir, ses ve dokunmatik kontrolün aynı kapsamı sunabilmesi için zengin bir kumanda özellik setini de içerir: TV yöneticisi, Wake-on-LAN, akıllı kanal düğmeleri, dokunmatik yüzey ve klavye, adlandırılabilir giriş kaynakları.

Sesli çekirdeğin çalışan bir prototipi mevcuttur. Sıfır dokunuş kontrolü bir sonraki kilometre taşıdır.

## Problem

- **Fiziksel ve ekrandaki kumandalar küçük düğmelere dayanır.** Kanal rakamları, renkli tuşlar, giriş seçimi ve ayar menüleri hassas dokunuş ve ekranı net görmeyi gerektirir. Küçük düğmelere basmakta ya da onları görmekte zorlanan kullanıcıların gerçek bir alternatifi yoktur.
- **Sesli kumandalar genellikle buluta bağımlıdır.** Yaygın sesli asistanlar sesi uzak sunuculara gönderir. Bu durum gizlilik kaygısı doğurur, internet bağlantısı gerektirir ve hizmeti işletene sürekli maliyet yükler.
- **Basit sesli komutlar yetmez.** Gerçek istekler çoğu zaman bileşiktir ("sesi kapat ve 5. kanala geç"), numara yerine ad kullanır ("Show TV'yi aç") ya da bir adım yerine bir hedef belirtir ("sesi 20 yap"). Basit bir anahtar kelime eşleştirici ya başarısız olur ya da yanlış tek bir işlemi seçer.
- **Ses nadiren uygulamanın tamamına ulaşır.** Sesli kontrol olsa bile genellikle yalnızca temel kumanda tuşlarını kapsar. Televizyona bağlanmak, birden fazla televizyonu yönetmek, ayarları değiştirmek ve ekranlar arasında geçmek hâlâ dokunmatik gerektirir.
- **Televizyonlar durumlarını bildirmez.** Yaygın kumanda protokolü mevcut ses düzeyini, kanalı ya da açık/kapalı durumunu geri okuyamaz. Bu nedenle "sesi 20 yap" gibi komutlar televizyona sorularak yerine getirilemez.

## Önerilen Çözüm

1. **Cihaz üzerinde ses hattı.** Konuşma telefonda metne dönüştürülür (telefonun yerleşik tanıyıcısı ya da isteğe bağlı çevrimdışı Whisper modeli), telefonda anlaşılır, televizyonda uygulanır ve sesli geri bildirimle onaylanır.
2. **Hibrit anlama.** Katmanlı motor önce kullanıcıdan öğrendiği düzeltmelere bakar, ardından kural tablolarını uygular (birden fazla konuşma tahmini üzerinde tam ve bulanık eşleştirme yapan 18 dilde anahtar kelime tablosu), ancak bunlardan sonra yerel dil modeline başvurur. Böylece sık kullanılan komutlar anında çalışır, serbest ifadeler de anlaşılır.
3. **"Model hiçbir şeyi kendisi yürütmez."** Dil modeli yalnızca bir niyet (intent) seçer ve ayrıntılarını doldurur. Deterministik bir planlayıcı bunu somut alt düzey adımlara çevirir (tuş basışları, rakam çevirme, bağlanma, ayar değiştirme), bir yürütücü de bu adımları tek tek çalıştırır. Bu yaklaşım davranışı öngörülebilir ve test edilebilir kılar.
4. **Uygulamanın tamamını kapsayan bir niyet kataloğu.** Söz dağarcığı 21 TV niyetinden 41 niyete çıkar. TV kontrolü, bağlantı yönetimi, ek kumanda tuşları, uygulama içi gezinme, ayarlar, model yönetimi ve ses düzeyi kalibrasyonu kapsanır.
5. **Bileşik komutlar ve takip soruları.** İfadeler parçalara ayrılır, her parça ayrı anlaşılır ve adımlar sırayla çalıştırılır. Asistan onay ya da açıklama isteyebilir ve cevabı sesle alır.
6. **Sesle ayar.** Her uygulama ayarı sesle değiştirilebilir. Değer doğrulanır, sesli onay verilir, uygulama dilini değiştirmek gibi riskli değişikliklerde ek bir onay adımı istenir.
7. **Her yerde sıfır dokunuş.** Ses her ekranda çalışır ve ses hattı televizyonları kendisi tarayabilir, onlara bağlanabilir ve onları uyandırabilir. Uygulama açıldığında *"televizyonuma bağlan"* komutu, ekrana dokunmadan keşif ve eşleştirme adımlarından geçip kullanıma hazır kumanda ekranına ulaştırır.
8. **Geri okuma yerine durum tahmini.** Televizyon durumunu bildiremediği için uygulama tahmini ses düzeyini, sessiz durumunu ve son çevrilen kanalları takip eder. Kullanıcı bu tahmini sesle yeniden ayarlayabilir ("ses 35'te").
9. **Sesin yanında eksiksiz bir kumanda.** Kayıtlı televizyonlarla TV yöneticisi, Wake-on-LAN, yapılandırılabilir akıllı kanal düğmeleri, dokunmatik yüzey ve klavye sekmesi, adlandırılabilir giriş kaynağı seçici, elle IP ve VPN modları, dokunsal geri bildirim ve otomatik yeniden bağlanma.

## Nasıl Çalışır

### Ses hattı

1. **Dinleme.** Kullanıcı mikrofon düğmesine dokunur ya da bir uyandırma ifadesi söyler. Uygulama sesi cihazda kaydeder. Ortam gürültüsüne göre kısa bir kalibrasyondan sonra, kısa bir ön kayıt arabelleğine sahip bir ses etkinliği kapısı konuşmanın ne zaman başlayıp bittiğine karar verir. Canlı bir mikrofon seviye çubuğu kullanıcıya sesinin duyulduğunu gösterir.
2. **Yazıya dökme.** Telefonun yerleşik ve cihaz üzerinde çalışan tanıyıcısı (dikte için ayarlanmış, cihaz üzerinde modu tercih eden) güven puanlarıyla birlikte birkaç alternatif metin üretir. Kullanıcı **çevrimdışı konuşma tanıma** seçeneğini açarsa yazıya dökme işini indirilmiş bir Whisper modeli yapar.
3. **Anlama.** Hibrit motor ifadenin her parçasını ayrıntılarıyla birlikte bir niyete (örneğin *kanal değiştir, ad = "Show TV"*) ve bir güven puanına dönüştürür. Kayıtlı TV adları ve akıllı kanal adları gibi bağlam bilgileri de motora verilir.
4. **Planlama.** Planlayıcı niyeti somut adımlar listesine çevirir. Tahmini ses düzeyi 50 iken *"sesi 20 yap"* komutu, aralarında kısa beklemeler olan 30 ses kısma basışına dönüşür, ardından tahmin 20 olarak güncellenir.
5. **Yürütme ve onay.** Yürütücü adımları televizyona (ya da uygulamanın bağlantı, ayar veya gezinme bölümlerine) gönderir ve telefonun yerleşik metin okuma özelliğiyle kısa bir sesli onay verir.

### Hibrit anlama motoru

| Aşama | Ne yapar | Neden |
|---|---|---|
| **0. Öğrenilmiş düzeltmeler** | Kullanıcının daha önce düzelttiği ifadelere bakar (en fazla 500 kayıt, en uzun süredir kullanılmayan ilk silinir). | Her kullanıcının aksanına ve ifade biçimine uyum sağlar. |
| **1–2. Kural motoru** | Tüm alternatif metinler üzerinde önce tam, sonra bulanık anahtar kelime eşleştirmesi yapar. 18 dili destekler, kanal adı ve numarası için sezgisel yöntemler kullanır. | Anında ve deterministik çalışır, modeli çalıştıramayan telefonlarda da işler. |
| **3. Yerel dil modeli** | LFM2.5-350M isteği okur ve fonksiyon çağırma yoluyla yapılandırılmış bir niyet döndürür. Yalnızca kurallar yeterince emin olmadığında kullanılır. Daha alt sıradaki bir metin büyük bir farkla öne geçtiğinde de modele danışılır. | Serbest ve alışılmadık ifadeleri anlar. |

Güven eşiğinin altındaki sonuçlar körü körüne uygulanmaz. Asistan *"Şunu mu demek istediniz…?"* diye sorar ya da anlamadığını söyler. Ayrı bir **çok turlu sesli sohbet** modu modelle bir konuşma sürdürür ve modelin aynı TV komutlarını araç olarak çağırmasına izin verir. iOS'ta sistem sesli kısayolları doğrudan aynı komut yoluna yönlendirilebilir.

**Neden LFM2.5-350M?** Bir TV komutunu anlamak, bilgi gerektiren bir görev değil, yapılandırılmış bir görevdir: kapalı bir listeden niyet seçmek ve ayrıntılarını doldurmak. Bu model, talimat izleme, veri çıkarma ve araç kullanımı için eğitilmiş küçük bir uç cihaz (edge) modelidir. Sıkıştırılmış hâliyle yaklaşık 229 MB'tır ve kurulumdan sonra indirilir, böylece uygulamanın kendisi küçük kalır. Tipik bir niyet, yaygın telefonlarda bir saniyenin epey altında çözülür. Büyüyen söz dağarcığında doğruluk yetersiz kalırsa sırasıyla daha yüksek hassasiyetli bir varyanta, istemde az sayıda örneğe (few-shot) ve son olarak daha büyük LFM2.5-1.2B modeline geçilir.

### Niyet kataloğu

| Grup | Niyetler | Örnek ifadeler |
|---|---|---|
| **Ses** | Sesi aç / kıs (adım sayısıyla), ses düzeyi ayarla, sessize al, sesi geri aç, ses düzeyini kalibre et | "Sesi aç", "Sesi 5 kez aç", "Sesi 30 yap", "Ses 35'te" |
| **Kanallar** | Kanal değiştir (numara ya da ad), sonraki / önceki, son kanal, kanal listesini göster | "42. kanal", "ESPN'e geç", "İzlediğim kanala geri dön" |
| **Oynatma** | Oynat, duraklat, durdur, ileri sar, geri sar, kaydet | "Devam et", "30 saniye ileri atla" |
| **Güç** | Aç, kapat, uyku zamanlayıcısı | "Televizyonu kapat" |
| **Gezinme ve tuşlar** | Yukarı / aşağı / sol / sağ / Tamam / geri / ana ekran, çıkış, rehber, bilgi, araçlar, renkli tuşlar | "Tamam'a bas", "Rehberi aç", "Kırmızıya bas" |
| **Uygulamalar, giriş ve altyazı** | Uygulama aç, giriş kaynağı, altyazı aç / kapat, metin yaz | "Netflix'i aç", "HDMI 2'ye geç", "Altyazıyı aç" |
| **Bağlantı** | Televizyonları tara, bağlan, bağlantıyı kes, televizyonu aç (ağ üzerinden uyandırma), televizyon değiştir | "Televizyonuma bağlan", "Salondaki televizyona geç" |
| **Uygulama içi gezinme** | Ekrana git (kumanda, televizyonlar, ayarlar, sesli sohbet) | "Ayarlara git", "Televizyonlarımı göster" |
| **Ayarlar** | Herhangi bir ayarı değiştir, uygulama dilini değiştir | "Dokunsal geri bildirimi kapat", "Dili Türkçe yap" |
| **Model yönetimi** | Dil modelini ya da Whisper modelini indir veya sil | "Yapay zekâ modelini indir", "Whisper modelini sil" |
| **Yedek** | Bilinmeyen | "Üzgünüm, bunu anlayamadım." |

Toplamda 41 niyet vardır. Hepsini dil modeli anlar, hepsi planlayıcı tarafından işlenir ve kural motoru hepsini en az İngilizce, Türkçe ve Almanca olarak kapsar.

### Sıfır dokunuş akışı

1. **İlk açılış.** Tanıtım adımları dil modelinin (ve isteğe bağlı olarak Whisper'ın) indirilmesini önerir. Model herkese açık bir model deposundan indirilir. Yeniden deneme, depolama alanı kontrolü ve yalnızca Wi-Fi seçeneği vardır.
2. **Sesle bağlanma.** *"Televizyonuma bağlan"* komutu yerel ağı tarar, bağlanır, eşleştirir ve hiç dokunmadan kumanda ekranını açar. *"Televizyonu aç"* komutu kayıtlı bir televizyonu ağ üzerinden uyandırır ve yeniden bağlanır.
3. **Sesle kontrol.** *"Kanalı Show TV'ye çevir"* komutu kayıtlı akıllı kanalı eşleştirir, numarasını çevirir ve onaylar. *"Sesi kapat, 5. kanala geç, sonra rehberi aç"* üç adımı sırayla çalıştırır.
4. **Sesle ayar.** *"Uyandırma kelimesini hey kumanda yap"* ya da *"Dili Türkçe yap"* önce onay ister, sonra değişikliği uygular.
5. **Sesle gezinme.** *"Ayarlara git"*, *"Televizyonlarımı göster"* ve *"Kumandayı aç"* her ekrandan çalışır. İsteğe bağlı bir ayar (varsayılan olarak kapalı) uygulama açılır açılmaz dinlemeyi başlatır.

### Gizlilik ve çevrimdışı çalışma

- Konuşma tanıma cihazda yapılır ve hiçbir ses kaydı telefondan çıkmaz.
- Anlama ve yürütme cihazda yapılır. Hiçbir kullanım verisi ya da analitik hiçbir sunucuya gönderilmez.
- Ağ yalnızca modelleri bir kez indirmek için gerekir. Bundan sonra her şey uçak modunda da çalışır. Tek istisna televizyonları taramak ve onlara bağlanmaktır, bunlar yerel ağ gerektirir (internetsiz Wi-Fi yeterlidir).
- Modeller uygulamanın özel depolama alanında tutulur ve indirildikten sonra bütünlükleri doğrulanır.
- Modeli çalıştıramayan telefonlarda (eski Android sürümleri, yetersiz bellek ya da desteklenmeyen işlemci türü) özellik kapatılmaz, kural motoru sesli kontrolü çalışır durumda tutar.

### Sesin yanındaki kumanda özellikleri

- **TV yöneticisi:** çevrimiçi ve kayıtlı televizyonlar tek listede, tek dokunuşla geçiş, aşağı çekerek yenileme, kaydırarak silme. Eşleştirme anahtarları uygulama yeniden başlatıldığında da korunur.
- **Wake-on-LAN:** kayıtlı bir televizyonu bekleme modundan açar ve yanıt vermeye başladığında otomatik olarak bağlanır.
- **Üç kontrol sekmesi:** *Standart* (uzun basıldığında otomatik onaylayan rakam tuşlarıyla klasik kumanda), *Akıllı* (ad ve logolu, yapılandırılabilir kanal düğmelerinden oluşan sayfalar, ses ve kanal için kaydırma hareketleri ve çift dokunuşla sessize alma) ve *Gezinme* (imleç kontrolü için dokunmatik yüzey, klavyeyle metin girişi, hızlı Ana Ekran / Geri / Arama / Menü düğmeleri).
- **Akıllı Kaynak:** giriş kaynaklarını ızgara olarak gösterir. Kullanıcı kaynaklara ad verebilir, örneğin HDMI 2 için "PlayStation".
- **Ayarlar:** ağ keşfini atlayan sabit IP ile bağlantı, VPN modu, kumanda düzeni seçimi, uzun basma süresi, dokunsal geri bildirim, dil seçimi, akıllı kanalları sıfırlama ve dışa aktarma.
- **Güvenilirlik:** bağlantının neden koptuğunun (televizyon kapandı mı, Wi-Fi mı gitti) daha iyi tespiti ve otomatik yeniden bağlanma.
- **Yerelleştirme:** arayüz 18 dilde kullanılabilir.

## Uygulama ve Aşamalar

| Aşama | Kapsam |
|---|---|
| **1. Kumanda temeli** | Kayıtlı televizyonlar ve eşleştirme, TV yöneticisi, Wake-on-LAN, sekmeli kumanda (Standart / Akıllı / Gezinme), Akıllı Kaynak, ayarlar, elle IP ve VPN modu, dokunsal geri bildirim, otomatik yeniden bağlanma, arayüz çevirileri. |
| **2. Cihaz üzerinde ses çekirdeği** | İlk açılışta model indirme, konuşmadan metne dönüştürme, sesli onaylar, yerel modelli hibrit motor, ilk 21 niyetlik katalog, kural tabanlı yedekle birlikte cihaz yeterlilik kontrolleri, öğrenilmiş düzeltmeler, çok turlu sesli sohbet ve iOS sesli kısayolları. |
| **3. Ses tanımanın yenilenmesi** | Alternatif metinler üreten, ayarlanmış cihaz üzerinde dikte, yerel ses yakalama, ön kayıtlı ses etkinliği algılama, metin tabanlı uyandırma ifadesi, isteğe bağlı çevrimdışı Whisper ve isteğe bağlı hata ayıklama görünümüyle birlikte mikrofon seviye göstergesi. |
| **4. Eylem planlama çekirdeği** | Anlamayı yürütmeden ayırmak: deterministik bir planlayıcı ve adım adım çalışan bir yürütücü, mevcut komutların bunların üzerine yeniden kurulması. |
| **5. Genişletilmiş niyet kataloğu** | Bağlantı, ek tuşlar, uygulama içi gezinme, ayarlar, model yönetimi ve kalibrasyonu kapsayacak şekilde 21'den 41 niyete çıkmak. |
| **6. Sesle ayar** | Her ayara sesle ulaşılabilmesi. Doğrulama, sesli onay ve riskli değişiklikler için onay adımı. |
| **7. Daha akıllı anlama** | Bileşik komutlar, modele verilen bağlam (kayıtlı TV ve kanal adları), onay ya da açıklama için takip soruları. |
| **8. Her yerde sıfır dokunuş** | Her ekranda ses, sesle tarama, bağlanma ve uyandırma, uygulama açılışında isteğe bağlı dinleme. |
| **9. Durum tahmini** | Sesle yeniden ayarlanabilen tahmini ses düzeyi ve sessiz durumu, "son kanal" için kanal hafızası. |
| **10. Model ve konuşma iyileştirmeleri** | Daha dayanıklı indirmeler (yeniden deneme, depolama kontrolü, yalnızca Wi-Fi, yedek kaynak), sesle model yönetimi, isteğe bağlı yüksek doğruluklu model varyantı ve tanıma için komut söz dağarcığı ipuçları. |
| **11. Doğrulama** | Planlayıcı ve yürütücü testleri, birden fazla dilde örnek ifadelerden oluşan bir referans derlemi, gerçek bir televizyonla gerçek Android ve iOS telefonlarda elle test matrisi. |

Sıfır dokunuş kilometre taşı, şunların hepsi gerçek telefonlarda ve gerçek bir televizyonla çalıştığında tamamlanmış olur: tanıtım adımlarından sonra hiç dokunmadan bağlanma, kanalı adıyla değiştirme, hedef ses düzeyi, bileşik komutlar, sesle ayar ve gezinme, ağ üzerinden uyandırma, desteklenmeyen bir telefonda kural tabanlı kapsam ve indirmeden sonra uçak modunda çalışma.

## Paydaşlar ve Faydalar

| Paydaş | Fayda |
|---|---|
| **Televizyon izleyicileri** | Bileşik ve ada dayalı istekler de dahil olmak üzere, düğme aramadan doğal dille televizyonu ve uygulamayı kontrol etme. |
| **Küçük düğmelere basmakta ya da onları görmekte zorlananlar** | Televizyona bağlanmaktan ayar değiştirmeye kadar her işlem sesle yapılabilir ve ne olduğu sesli olarak onaylanır. |
| **Gizliliğe önem veren kullanıcılar** | Hiçbir ses kaydı ya da kullanım verisi telefondan çıkmaz. Asistan çevrimdışı çalışır. |
| **Çok dilli haneler** | 18 dilde arayüz, 18 dilde kural tabloları, İngilizce, Türkçe ve Almanca ayar kelimeleri. |
| **Eski ya da düşük donanımlı telefon kullananlar** | Modelin çalışamadığı cihazlarda kural tabanlı yedek sesli kontrolü kullanılabilir tutar. |
| **Uygulama yayıncısı** | Sesli özellik için sunucu maliyeti yoktur. Modeller kurulumdan sonra indirildiği için uygulama indirmesi küçük kalır. |
| **Açık kaynak ve uç cihaz yapay zekâ topluluğu** | Tüketici uygulamasında küçük yerel dil modellerini deterministik kurallar ve planlamayla birleştirmek için somut bir örnek. |

## Riskler ve Önlemler

| Risk | Önlem |
|---|---|
| **Model telefonların büyük bir kısmında çalışamıyor** (Android cihazların yaklaşık %35'i gereken Android sürümünün altında, bazı telefonların belleği yetersiz) | Çalışma anında yeterlilik kontrolü. Bu telefonlarda sık kullanılan komutları kural motoru karşılar. |
| **Yeterli görünen bir telefonda modelin belleği tükeniyor** | Hatayı yakalamak, o kurulumda modeli devre dışı bırakmak ve kurallara geri dönmek. |
| **Söz dağarcığı büyüdükçe doğruluk düşüyor** | Daha yüksek hassasiyetli model varyantı, az sayıda örnek (few-shot), ardından daha büyük bir model. Komutları adımlara bölmeyi model değil planlayıcı yapar. |
| **Yanlış duyulan ya da düşük güvenli komutlar** | Güven eşikleri, birden fazla alternatif metin, öğrenilmiş kullanıcı düzeltmeleri ve sesli "Şunu mu demek istediniz…?" onayı. |
| **Yanlışlıkla yapılan riskli değişiklikler** (dil değişikliği, model silme, uyandırma ifadesi) | Uygulamadan önce açık bir sesli onay adımı. |
| **Televizyon ses düzeyini, kanalı ya da açık/kapalı durumunu bildiremiyor** | Tahmini durum, sesle yeniden ayarlama ve kanal hafızası. Bu kısıt açıkça belgelenir. |
| **Model indirmesi başarısız oluyor ya da bağlantı veya cihaz için çok büyük** | Artan aralıklarla yeniden deneme, yedek kaynak, boş alan kontrolü, varsayılan olarak yalnızca Wi-Fi ve anlaşılır sesli ya da ekrandaki hata mesajları. |
| **Model çalışma ortamı için kullanılan üçüncü taraf kütüphaneler bakımsız kalıyor** | Sabitlenmiş sürümler ve yerine takılabilecek alternatif bir kütüphane. |
| **Sürekli açık akustik uyandırma kelimesi uygulamaya eklenemedi** | Bunun yerine metin tabanlı bir uyandırma ifadesi kullanılır. Akustik uyandırma motoru isteğe bağlı bir sonraki adım olarak kalır. |
| **Model lisansı atıf gerektiriyor** | Uygulamanın Hakkında ekranında ve mağaza sayfalarında atıf. |
| **Pratikte yalnızca Samsung televizyonlar destekleniyor** | TV kontrolü ortak bir arayüzün arkasındadır. Başka bir marka desteği şu an yalnızca yer tutucu düzeyindedir. |

## Teşekkür

Temel kumanda altyapısı, mazen-salah'ın MIT lisanslı açık kaynak Samsung TV kumanda projesine dayanır. Burada anlatılan fikirler bu altyapının üzerine yapılan eklemelerdir.

## Köken

İlk olarak Merih İlgör tarafından tasarlanmıştır (2026). Burada açık bir proje fikri olarak yayımlanmaktadır.

## Lisans

Bu belge ve anlattığı proje fikri, ticari olmayan kullanım için CC BY-NC-SA 4.0 lisansı altındadır (bkz. [LICENSE](LICENSE)). Ticari kullanım için gelir paylaşımı anlaşması gerekir (bkz. [COMMERCIAL-LICENSE.md](COMMERCIAL-LICENSE.md)). MIT lisanslı kumanda altyapısı ile dil ve konuşma modelleri dahil olmak üzere üçüncü taraf bileşenler kendi lisanslarına tabidir.
