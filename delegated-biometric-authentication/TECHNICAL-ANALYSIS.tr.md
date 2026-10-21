English: [TECHNICAL-ANALYSIS.md](TECHNICAL-ANALYSIS.md)

# Delegeli Biyometrik Kimlik Doğrulama: Teknik Analiz (Araştırma Düzeyi)

> **Bağlayıcı olmayan teknik analiz.** [Öneri](README.tr.md) "ne" ve "neden"i anlatır. Bu not, modelin güvenilir çalışması için bir kurulum yolunu gösterir ve yeniden kullanılabilecek mevcut standartları sayar. Bir şartname değil, araştırma düzeyinde bir tasarımdır. Gerçek bir uygulama onu kendi ölçeğine, pazarlarına ve mevzuatına göre uyarlamalıdır. Ticari uygulamalar [ticari lisansa](COMMERCIAL-LICENSE.md) tabidir.

## 1. Kusursuz çalışması gerekenler

Model, aşağıdakilerden herhangi biri başarısız olduğunda başarısız olur. Aşağıdaki bileşenlerin her biri bunlardan birini garanti etmek için vardır.

| # | Garanti | Önlediği başarısızlık |
|---|---|---|
| G1 | Biyometrik şablon, tutulduğu cihazdan hiç çıkmaz | Başkasının veritabanında değiştirilemeyen kalıcı bir kimlik |
| G2 | Bir kanıt yalnızca kendisine verildiği okuyucuda, yalnızca kendisi için verildiği işlemde ve yalnızca bir kez kabul edilir | Çalınmış ya da kopyalanmış bir kanıtın başka bir kapıda veya kasada tekrar oynatılması |
| G3 | Bir delegasyon yalnızca kullanıcı verdiği için vardır; yetki görünür, kapsamlı ve geri alınabilirdir | Kullanıcının onaylamadığı delegasyon; kullanıcının geri alamadığı onay |
| G4 | Geri alınmış bir yetki, belirtilen sınır içinde her okuyucuda durur | Geri çekilmiş ama hâlâ kapıları açan delegasyon |
| G5 | Okuyucu canlı bir yüzü fotoğraftan, maskeden ya da tekrar oynatmadan ayırt eder | Basılı bir görüntüyle biyometrik kimlik doğrulamanın yenilmesi |
| G6 | Kimliğin gerekmediği yerde okuyucu, yinelenen ziyaretleri birbirinden ayırt edemez | Okuyucunun, hizmet verdiği insanlar için gizli bir gözetim kaydı oluşturması |
| G7 | Okuyucu yalnızca amacın gerektirdiğini öğrenir, fazlasını değil | Dar bir kontrolden yeniden kimliklendirme ya da işletmecinin profil biriktirmesi |
| G8 | Sistem kapalı çalışır: kullanılamayan güven hizmeti, süresi dolmuş yetki ya da başarısız canlılık denetimi "hayır" demektir, asla "evet" değil | Bir bileşen çöktüğü için başarılı sayılan bir doğrulama |
| G9 | Delegasyonlu kontrol, hiçbir biyometrik veri açıklamadan uçtan uca denetlenebilir | Kimin girdiğine dair doğrulanamayan bir iddia ve uzlaşmayı çözecek bir yolun olmaması |
| G10 | Sistemdeki kimlik bilgileri bir kuantum bilgisayarın gelişine dayanır | Yıllarca sonra taklit edilebilecek uzun ömürlü delegeli kimlik bilgileri |

## 2. Bileşenler

```
                 ┌──────────────────────────┐
                 │  Kullanıcının cihazı     │
                 │  ┌────────────────────┐  │
                 │  │ Biyometrik şablon  │  │  ← güvenli öğeden hiç çıkmaz
                 │  │ (güvenli öğe)      │  │
                 │  └─────────┬──────────┘  │
                 │  Yetki kaydı, anahtar     │
                 └────────────┼─────────────┘
                              │ imzalar
        ┌─────────────────────▼──────────────────────┐
        │  Delegasyon aracısı (işletim sistemi /     │
        │  cüzdan)                                   │
        │  - onay arayüzü, yetki listesi, iptal     │
        │  - meydan okuma yanıtlayıcısı              │
        └─────────────────────┬──────────────────────┘
                              │ yetkileri kaydeder, durumu yayımlar
                 ┌────────────▼─────────────┐
                 │  Güven hizmeti           │
                 │  - yetki kaydı            │
                 │  - iptal listesi          │
                 │  - güven çapaları         │
                 └────────────┬─────────────┘
                              │ durum, çapalar
   ┌──────────────────────────▼───────────────────────────┐
   │  Okuyucu (satış terminali, geçit, kiosk)            │
   │  - kamera, sunum saldırısı tespiti                 │
   │  - meydan okuma üretimi, kanıt doğrulama            │
   └──────────────────────────┬───────────────────────────┘
                              │ kanıt (biyometrik veri yok)
                 ┌────────────▼─────────────┐
                 │  İşletmeci               │
                 │  (banka, bina, ulaşım)   │
                 │  - denetim kaydı, kayıtlar│
                 └──────────────────────────┘
```

### 2.1 Delegasyon aracısı

Aracı, şablonu tutan ve kullanıcı adına konuşan taraftır. Uygulamada bu, cihazın işletim sistemidir ya da işletim sisteminin güvenli donanımında çalışan bir cüzdan uygulamasıdır.

- **Şablon güvenli öğede tutulur.** Oraya kaydedilir ve işletim sistemi, aracı uygulaması ya da sıradan dosya sistemine erişebilen hiçbir kimse tarafından okunamaz. Bu, telefonun kendi kilidini koruyan sınırın aynısıdır; kullanıcı zaten bu yüzdan onun üzerinden delegasyon vermeye isteklidir.
- **Aracı bir cihaz anahtarı tutar.** Güvenli öğeden çıkmayan özel bir anahtar kanıtları imzalar. Açık anahtar, aracı güven hizmetinde listelenen bir sertifikada yayımlanır; okuyucu, kanıtın kayıtlı bir cihazdan geldiğini böylece kontrol eder.
- **Aracı onay makamıdır.** Kullanıcı yetkileri verir ve geri alır. Hiçbir uygulama kullanıcı adına yetki veremez; kötü niyetli ya da ele geçirilmiş bir uygulama delegasyon oluşturamaz.
- **Aracı yüzü gören tek taraftır.** Okuyucudaki kamera sunumu yakalar; eşleştirme aracı ya da güvenli öğe tarafından yapılır. Görüntünün kendisi eşleştirme bittiğinde atılır. Okuyucu hiçbir zaman şablon, referans görüntü ya da yeniden kullanılabilir bir yerleştirme almaz.
- **Aracın iptali mümkündür.** Kayıp ya da çalınmış cihaz güven hizmetinden kayıt düşürülebilir; bu, verdiği bütün yetkileri geçersiz kılar.

### 2.2 Yetki kaydı

Yetki, delegasyonu gerçek kılan nesnedir. Kullanıcının oluşturduğu, okuyucunun kontrol ettiği ve işletmecinin denetlediği şey budur.

| Alan | Anlamı |
|---|---|
| Yetki tanımlayıcısı | Benzersiz, hiçbir zaman yeniden kullanılmaz |
| Özne | Delegasyon veren cihazın takma adı, kişinin kendisi değil |
| Okuyucu | Yetkinin geçerli olduğu okuyucu ya da okuyucu sınıfı |
| Amaç | İzin verilen işlem: ödeme, giriş, yaş kanıtı, devam |
| Öznitelikler | Okuyucunun öğrenebileceği: bir kimlik referansı, bir yaş bandı ya da yalnızca "yetkili" |
| Geçerlilik başlangıcı / bitişi | Yetkinin kullanılabileceği zaman aralığı |
| Azami kullanım sayısı | İsteğe bağlı sınır, örneğin tek bilet için bir kullanım |
| Durum | Etkin, iptal edilmiş, süresi dolmuş, askıya alınmış |
| Delegasyon sayacı | Yetkinin kaç kez kullanıldığı |

**Bir yetki bir hizmet sınıfını değil, bir okuyucuyu adıyla belirtir.** "Bu binanın geçit cihazı" bir yetkidir; "erişim uygulamaları" değildir. Okuyucu kimliği, bir kanıtın aktarılamaz olmasını sağlayan şeydir, çünkü kanıt belirli bir okuyucuya bağlıdır.

**Bir yetki bir yeteneği değil, bir amacı adıyla belirtir.** Ödeme, giriş, yaş kanıtı ve devam ayrı yetkilerdir ve ayrı kapsamlardır; aynı cihaz ve aynı okuyucu söz konusu olsa bile bir amaç için verilen delegasyon başka bir amaç için kullanılamaz.

**Bir yetki varsayılan olarak süresi biter.** Süresiz bir yetki, bütün önerinin çözmeye çalıştığı sorunun daha küçük bir kostümüdür. Kullanıcı kalıcı bir düzen istiyorsa — ev, işyeri — yetki uzun ömürlüdür ama yine görünür, yine geri alınabilir ve yine neye izin verdiğiyle sınırlıdır.

### 2.3 Güven hizmeti

Güven hizmeti, modelin cihaz üreticileri ve okuyucu üreticileri arasında çalışmasını sağlayan tarafsız taraftır. Hiçbir biyometrik veri tutmaz ve tutması da gerekmez.

- **Yetkileri kaydeder ve durumlarını yayımlar.** Okuyucu, cihazla bağlantı kurmadan ve kullanıcıdan hiçbir şey yüklemeden bir delegasyonun var olup olmadığını, etkin olup olmadığını ve bu okuyucu ile amacı kapsayıp kapsamadığını kontrol edebilir.
- **İptal bilgisini yayımlar.** Bir yetki geri alındığında cihaz bunu bildirir, güven hizmeti durumu okuyucuların kullanıcıyla çevrimiçi olmadan tüketebileceği bir mekanizmayla yayımlar. Mekanizma, sertifika iptal listesi ya da durum sorgusunun biçimindedir ve tazelik sınırı protokolde açıkça belirtilir, örtük bırakılmaz.
- **Güven çapalarını yayımlar.** Kayıtlı aracıların açık anahtarlarını ve aracı anahtarını bir cihaza bağlayan sertifikaları.
- **Hiçbir yüzü görmez.** Tanımlayıcı, anahtar ve durum tutar. Güven hizmeti ele geçirilirse saldırgan anahtarları ve metadatası alır, biyometrik veriyi değil. Önerinin tasarlanma amacı olan biyometrik veritabanından tam olarak bunun ayırdığı özelliktir.
- **Tarafsızdır ve kâr amacı gütmeyen ya da kamu tüzel kişiliğine sahip bir kuruluştur.** Kurallarını ve ücretlerini yayımlar, yayımlanmış ölçütleri karşılayan her aracıyı ve işletmeciyi kabul eder; cihazları ya da okuyucuları sıralamaz. Bir aracının kabulünü reddedebilen bir güven hizmeti bir kapıcı olurdu ve model tasarımıyla önlemek istediği kilitlenmeyi miras alırdı.

### 2.4 Okuyucu

Okuyucu, kullanıcının gerçekten bulunduğu yerdir: bir satış terminali, bir geçit, bir turnike, bir resepsiyon masası. Kimlik konusunda bilerek sadır, kanıt konusunda titizdir.

- **Canlı bir sunumu yakalar ve denetler.** Bir kamera, mümkünse derinlik ya da çok tayflı sensör ve bir fotoğrafı, maskeyi, ekran tekrar oynatımını ve basılı görüntüyü reddeden bir sunum saldırısı denetimi.
- **Bir meydan okuma üretir.** Meydan okuma her doğrulama için benzersizdir; yakalanmış bir kanıt tekrar oynatılamaz.
- **Kanıtı doğrular.** İmza, zincir, geçerlilik penceresi, okuyucuya bağlılık, amaç bağlılığı, yetki durumu ve kendi meydan okuma yanıtı.
- **Hiçbir biyometrik veri saklamaz.** Yakalama bellekte tutulur, eşleştirmede kullanılır ve atılır. Standart saklamayı yasaklar ve sertifikasyon düzeni bunu denetler. Delegasyonlu varlık mekanizması, okuyucuya *dönüştürülmüş* bir referans yerleştiren bir mekanizma ise — yenilenebilir referans ve sıfır bilgi mekanizmaları — okuyucu bu referansı tutar; [şartnamenin](SPECIFICATION.tr.md) belirlediği koşullarla: yenilenebilir, yetkiye özgü, okuyucuya bağlı, iptal edilebilir ve şablona geri türetilemez.
- **Hiçbir yeniden kullanılabilir tanımlayıcı saklamaz.** Kullanıcının yüzünün kararlı bir özetini saklayan bir okuyucu bir biyometrik veritabanı kuruyor demektir ve modeli ihlal eder. Okuyucunun doğrulama kaydı bir kişiye değil, bir yetkiye ve bir kanıta referanstır.

### 2.5 İki doğrulama kipi

Merkezî tasarım seçimi şudur: okuyucu, şablonu hiç görmediği hâlde canlı bir biyometrik kontrolün başarılı olduğunu nasıl öğrenir?

**Kip A — çevrimiçi, aracı aracılığı.** Okuyucu, güvenli bir kanal üzerinden aracıya bir meydan okuma gönderir; araç güvenli öğede biyometrik eşleştirmeyi yapar ve meydan okumanın delegasyon veren cihaz tarafından yanıtlandığını, yetkinin kapsamıyla birlikte imzalı bir beyanla döner. Okuyucu imzayı ve yetkinin geçerliliğini doğrular.

- *Gerektirir:* aracın erişilebilir olması. Telefon cebinde, başka bir odada ya da evde olabilir; yeterli olan bağlantı var.
- *Sağlar:* en güçlü bağlılık, çünkü meydan okuma canlı yanıtlanır ve eşleştirme şablonun var olmasına izin verilen tek yerde yapılır.
- *Bedeli:* her doğrulamada bir ağ turu ve çalışan bir hizmete bağımlılık.
- *Uygun olduğu durumlar:* ödeme, bina girişi, otel check-in, klinik girişi — ağın bulunduğu ve birkaç yüz milisaniyenin kabul edilebilir olduğu durumlar.

**Kip B — çevrimdışı, imzalı kanıt.** Araç, kısa ömürlü, okuyucuya ve amaca bağlı bir kanıtı cihaz anahtarıyla önceden imzalar. Okuyucu, yayımlanmış güven çapalarına ve önbelleğe alınmış bir duruma karşı, ağ çağrısı yapmadan kanıtı doğrular.

- *Gerektirir:* okuyucunun önbelleğe alınmış bir durumu olması ve kanıtın gerçekten kısa ömürlü olması.
- *Sağlar:* bağlantı olmadan çalışma; bodrum katlardaki kapılar, kırsal ulaşım ve ağın güvenilmez olduğu her yer için önemlidir.
- *Bedeli:* iptal sınırı görünür hâle gelir. İptal edilmiş bir yetki, durumu görmemiş bir okuyucuda kanıtın süresi dolana kadar çalışabilir. Bu sınır küçük ve açıkça belirtilmelidir.
- *Uygun olduğu durumlar:* kapılar, turnikeler, çevrimdışı kontrol noktaları.

**Seçim okuyucu başına ve yetki başınadır, platform başına değil.** Bir bina, kapılarında Kip B'yi, personel girişinde Kip A'yı kullanabilir. Yetki, okuyucunun desteklediği kipte düzenlenir ve kip kullanıcıya görünür, çünkü pratikte "geri alındı"nın ne anlama geldiğini değiştirir.

### 2.6 Canlılık ve sunum saldırısı tespiti

Model burada kazanılır ya da kaybedilir; çünkü bir fotoğrafı kabul eden bir biyometrik kontrol kimlik doğrulama değildir.

- **Denetimi okuyucu yapar, işletmeci değil.** Denetim, kullanıcının ayakta durduğu yerde, işletmecinin kontrol ettiği ama biyometriğine sahip olmadığı donanımda gerçekleşir.
- **Standart mevcut bir standarttır.** Denetim, tanınmış bir sunum saldırısı tespiti standardına uyar ve okuyucular, saldırı sınıfı başına yayımlanmış bir yanlış kabul oranıyla sertifikalanır. Basılı bir fotoğrafı kabul eden bir okuyucu bir sertifikasyon hatası olmalı, manşet haberi değil.
- **Derinlik ve çok tayflı sensörleme yardımcı olur ama yeterli değildir.** Saldırganın maliyetini ve emeğini artırır; standardın yerini almaz ve tek savunma olamaz.
- **Kontrol zayıfsa kullanıcı uyarılır.** Uyumlu bir denetim çalıştıramayan bir okuyucuda yetki bunu belirtmelidir; işletmeci, sertifikalı bir okuyucuyla aynı güvenceyi talep edemez.
- **Sunum atılır.** Denetim için kullanılan yakalama saklanmaz, günlüğe yazılmaz ve hiçbir şeyin eğitilmesinde kullanılmaz. Okuyucunun sessizce bir şablon deposuna dönüşmesini engelleyen budur.

### 2.7 İzlenemezlik ve veri minimizasyonu

Aynı okuyucu ve amaç için iki yetki düzenlenebilir ve ikisi de birbirinden farklı görünmelidir.

- **İzlenemez kip.** Okuyucunun yalnızca "yetkili bir kullanıcı" bilmesi gerektiği yerde yetki, tek kullanımlık ya da kısa ömürlü bir **izlenemez belirteç** olarak düzenlenebilir: okuyucu onu doğrular ve önceki bir sunumla ilişkilendiremez. Okuyucu kişi hakkında hiçbir şey öğrenmez.
- **Kimlik kipi.** Okuyucunun kişinin kim olduğunu bilmesi gerektiği yerde — otel, klinik, banka — yetki kapsamlı bir kimlik bilgisi taşır. Okuyucu kimliği ve başka hiçbir şeyi öğrenmez: yetki listesini değil, diğer okuyucuları değil, diğer amaçları değil, biyometriği değil.
- **Öznitelik kipi.** Yalnızca bir olgu gerektiği yerde — on sekiz üstü, sezon bileti sahibi, sakin — yetki, arkasındaki kimlik değil olguyu taşır. Perakendeci "eşik üstü" alır, doğum tarihi almaz ve daha fazlasını isteyemez.
- **Asıl risk okuyucu tarafındaki ilişkilendirmedir.** Doğrulama zamanlarını ve kararlı bir yetki tanımlayıcısını saklayan bir okuyucu, protokolün taşıdığı ne olursa olsun bir kişinin hareketlerini yeniden kurabilir. Standart, okuyucu kayıtlarının ya işlem başına ya da dönemsel olarak takma adla tutulmasını şart koşmalı ve işletmecinin denetimi, geri birleştirilmiş bir kaydı tespit edebilmelidir.
- **İşletmecinin kaydı bir bedenin değil, bir olayın kaydıdır.** "Bu okuyucuda bu yetki altında bu zamanda devredilmiş bir kontrol başarılı oldu" meşru bir kayıttır; birleştirilmiş bir günlükten kurulan "bu kişi buradaydı" meşru değildir ve denetim denetimi bu ayrımı kontrol eder.

### 2.8 İptal

Delegasyonu kalıcı bir kimlikten ayıran mekanizma iptaldir, bu yüzden gerçekten işe yaramalıdır.

- **Kullanıcı geri alır.** Yetkinin verildiği yerden. Aracı yetkiyi iptal eder ve güven hizmetine bildirir.
- **Güven hizmeti yayımlar.** Durum, iptal listesinde ve durum sorgusunda görünür; soran her okuyucu yanıtı alır.
- **Çevrimiçi okuyucular anında öğrenir.** Kip A okuyucular her doğrulamada sorgular, böylece geri alınmış bir yetki bir sonraki denemede başarısız olur.
- **Çevrimdışı okuyucular belirtilen sınır içinde öğrenir.** Kip B okuyucular önbelleğe alınmış durumdan çalışır ve süre dolunca kapalı çalışır. Sınır, protokolün izin verdiği önbelleğe alınmış durumun azami yaşıdır ve kısa olmalıdır — dakikalar, saatler değil — çünkü geri alınmış bir yetkinin hâlâ kapı açması, kullanıcıların en çok korktuğu başarısızlıktır.
- **İptal silme değildir.** Yetki kaydı ve denetim izi kalır; çünkü bir binanın bir girişin gerçekleştiğini göstermesi gerekir. Ortadan kalkan şey onu *kullanma yeteneğidir*. Bir delegasyonu geri çeken kullanıcı, onu verdiğine dair kaydı korur; bu kullanıcının kendi kaydıdır ve işletmecinin de, ikisinde de biyometrik yoktur.

### 2.9 Denetim

Denetim, "bize güvenin"i denetlenebilir bir şeye dönüştürür.

- **Her doğrulama imzalı bir kayıt üretir.** Yetki, kanıt, okuyucu, meydan okuma, sonuç ve zaman.
- **İşletmeci toplu rakamlar yayımlar.** Doğrulama sayıları, sonuçlar, hata nedenleri ve iptal sayıları — kişi başına veri olmadan.
- **Bağımsız bir denetçi uygunluğu denetler.** Okuyucuların biyometrik veri saklamadığını, yetkilerin iddia edildiği gibi kapsandığını, yetkilerin süresinin dolduğunu, çevrimdışı okuyucuların tazelik sınırına uyduğunu ve günlüklerin bir hareket profiline dönüştürülmediğini.
- **Güven hizmeti kendi şeffaflık rakamlarını yayımlar**, böylece bir okuyucu üreticisi ya da işletmeci karşılaştırılabilir.
- **Kullanıcı kendi izini görebilir.** Yetkiler, kullanımlar ve iptaller, işletim sisteminin izin görünümünde.

### 2.10 Post-kuantum değerlendirmeleri

Delegeli bir kimlik bilgisi doğası gereği uzun ömürlüdür. Bir bina erişim yetkisi, bir hastane delegasyonu, bir ödeme standing düzenlemesi ve bir ulaşım kartı yıllar önce düzenlenip yıllarca geçerli kalabilir. Bu, kuantum dirençli kriptografi için ders kitabı vakasıdır; bu yüzden [standardizasyon notu](STANDARDIZATION.md) bunu birinci sınıf bir katman olarak ele alır, bir yükseltme olarak değil.

- **İmzalar uzun ömürlü kısımdır.** Bugün yapılan bir imza on yıl sonra doğrulanabilmelidir; dolayısıyla imza şeması düzenlenme anında post-kuantum güçte olmalıdır.
- **Anahtar değişimi oturum kısmıdır.** Okuyucu ile aracı arasındaki oturum kısa ömürlüdür ve döner; klasik anahtar değişimi orada kabul edilebilir, ama geçiş döneminde karma yapı ucuz bir sigortadır.
- **Geçiş, zincirin tamamının problemidir.** Güven hizmeti, aracı, okuyucu ve işletmeci birlikte hareket etmelidir; çünkü yalnızca klasik imzaları kabul eden bir okuyucu post-kuantum okuyucuları ve tersini engeller. Algoritma profilini baştan adlandıran bir standart, geçişi keşfedilmek yerine planlanabilir kılar.
- **"Şimdi topla, sonra çöz" şablona değil denetim izine uygulanır.** Şablon hiç iletilmediği için toplanamaz. Önemli kayıtlar — yetki geçmişleri, denetim izleri, kanıt dökümleri — post-kuantum korumaya ihtiyaç duyan uzun ömürlü şifreli metinlerdir.
- **Rotasyon tasarımın içine alınmalı, sonradan eklenmemelidir.** Yalnızca on yılda bir değiştirilebilen bir kimlik bilgisi uzun ömürlü bir sırdır. Kısa ömürlü kanıtlar ve döndürülebilen cihaz anahtarları maruziyet penceresini daraltır.

### 2.11 Veri koruma mevzuatı

Model veri minimizasyonu bir kuruluştur ve mevzuat onu öyle ele almalıdır.

- **Şablon işletmeci ya da okuyucu tarafından işlenmez.** Cihazda kalır. Okuyucu eşleştirmeyi yaptığında yakalama yerelde işlenir ve atılır; bu, uzak biyometrik karşılaştırmaya göre çok daha küçük bir işleme alanıdır.
- **Biyometrik şablon özel nitelikli veridir** — Avrupa Birliği'nin Genel Veri Koruma Tüzüğü ve Türkiye'nin Kişisel Verilerin Korunması Kanunu uyarınca. Buradaki "delegasyon", denetleyicinin değil veri öznesinin doğrulama yöntemini seçmesi olarak anlaşılmalıdır; hukuki analizin bu ayrımı açıkça yapması gerekir.
- **Hukuki sebep yine de adlandırılmalıdır.** Bir delegasyon, işletmeci için kendiliğinden bir hukuki sebep yaratmaz; teknik bir kontrol sağlar. İşletmecinin dayanağı, saklama kuralları ve veri öznesi hakları protokolle değişmez.
- **Veri minimizasyonu gösterilebilir hâle gelir.** Bu modeli bir denetim makamı görüşmesinde en güçlü argümanı, işletmecinin bir kimlik bilgisi ve bir kayıt tuttuğu ve kaydedilmiş şablon tutmadığının kanıtlanabilir olduğu, dolayısıyla işletmeci ihlalinin anahtarların ihlali olduğu, değiştirilemez kişisel verinin ihlali olmadığıdır.
- **AB'deki yeni biyometrik kurallar uzak biyometrik kimlik tanıma sistemlerine uygulanır.** Delegasyonlu kontrol bu anlamda uzak bir biyometrik kimlik tanıma sistemi değildir; ancak uygulayıcı varsaymak yerine bu analizi açıkça yazmalıdır.

## 3. Hata durumları

| Hata | Algılama | Yanıt |
|---|---|---|
| Okuyucu bir fotoğrafı kabul eder | Sunum saldırısı standardına göre sertifikasyon testi; olay raporları | Okuyucu sertifikasını yitirir; o okuyucuya verilen yetkiler yeniden sertifikasyon bekleyene kadar askıya alınır |
| Şablon bir cihazdan sızdırılır | Güvenli öğe kurcalama algılama; attestation | Cihaz yeniden kaydedilir; sızan şablon hiçbir okuyucu tarafından kabul edilmediği için işe yaramaz |
| Kanıt başka bir okuyucuda tekrar oynatılır | Kanıt okuyucuya ve meydan okumaya bağlıdır; yinelenen meydan okuma tespiti | Kanıt reddedilir; olay işletmeciye ve güven hizmetine bildirilir |
| Kanıt aynı okuyucuda tekrar oynatılır | Kanıt geçerlilik penceresi içinde tek kullanımlı takip | Kanıt tüketilmiş olarak reddedilir |
| Kötü niyetli bir uygulama yetki oluşturur | Yetkiler yalnızca işletim sisteminin güvenlik alt sisteminde vardır | Yetki reddedilir; uygulama platformdan kaldırılır |
| Geri alınmış bir yetki çevrimdışı bir okuyucuda hâlâ çalışır | Tazelik sınırının ötesinde kalmış önbelleğe alınmış durum | Okuyucu kapalı çalışır; sınır daraltılır; bayat bir okuyucu hizmetten kaldırılır |
| İşletmeci gizli bir biyometrik veritabanı kurar | Uygunluk denetimi; depolama incelemesi | Standardın ihlali; sertifikasyon ve yetki kaydı geri çekilir |
| Okuyucu günlüklerini bir hareket profiline dönüştürür | Günlük yapısı denetimi | Standardın ihlali; işletmecinin sertifikasyonu geri çekilir |
| Güven hizmeti kullanılamaz | Sağlık kontrolleri; okuyucular kapalı çalışır | Doğrulama reddedilir, kabul edilmez; hiçbir bozulmuş kabul yolu yoktur |
| Arac kullanılamaz (Kip A) | Sağlık kontrolleri | Okuyucu kontrolü reddeder; işletmeci bir yedek kimlik bilgisi (kart, kod, passkey) sunuyorsa o kullanılabilir |
| Aracın anahtarı ele geçirilir | Anahtar attestation'ı; sertifika iptali | Cihaz kayıttan düşürülür; bütün yetkileri iptal edilir; etkilenen okuyucular bilgilendirilir |
| Bir yetki amacının dışında kullanılır | Kanıttaki amaç bağlılığı; yetki kapsamının denetimi | Kullanım reddedilir; olay kaydedilir |
| Bir yetki amacını aşar (bina erişim yetkisi ödemede kullanılır) | Denetimde amaçlar arası tespit | Yetkiler iptal edilir; işletmeci ve kullanıcı bilgilendirilir |
| Kullanıcı bir yetkinin kim tarafından kullanıldığını anlayamaz | Kullanıcının çözebildiği takma ada bağlı denetim kayıtları | Kullanıcı izi görebilir; işletmecinin ayrıntı düzeyi yetkiyle belirlenir |
| Okuyucunun saati yanlış ve geçerli kanıtları reddeder | Protokolde tolerans pencereleri; hata oranı izleme | Sertifikasyonda saat eşitleme şartı |
| Uzun ömürlü kimlik bilgileri toplanıp gelecekte çözülür | İlk günden post-kuantum imza profili zorunluluğu | Sonradan telafi edilemez; algoritma profili dağıtımdan önce standartta yer almalıdır |

## 4. Aşamalı kurulum

1. **Yetki ve kanıt şartnamesi:** yetki nesnesi, iki doğrulama kipi, kanıt biçimi, meydan okuma ve yanıt, bağlılık kuralları ve iptal modeli. Bir cihaz üreticisinin yardımı olmadan bir okuyucu üreticisi tarafından uygulanabilecek biçimde yazılır.
2. **Tek kapalı pilot:** bina erişimi, tek binada; işletmeci, kullanıcının kendi ev sahibi. Değer açıkça görülür ve risk sınırlıdır: tek site, tek yetki, tek okuyucu tipi.
3. **Canlılık ve okuyucu sertifikasyonu:** sunum saldırısı test düzeni, yayımlanmış yanlış kabul oranları ve "bir fotoğrafı kabul etti" başlığını bir sertifikasyon hatasına dönüştüren sertifikasyon.
4. **Çevrimiçi kip ölçekte:** ödeme ve ulaşım, bağlantı varsayıldığı ve işletmecinin profesyonel bir güven tarafı olduğu yerlerde Kip A ile.
5. **Çevrimdışı kip ve iptal sınırı:** Kip B; tazelik sınırı tahminle değil ölçümle belirlenir ve bayat önbellek altında kapalı çalışma davranışı test edilir.
6. **İzlenemez ve öznitelik yetkileri:** gizliliği en yüksek kipler; en iyi oturan kullanım durumlarında — ulaşım, yaş kanıtı ve kimliğin gerekmediği erişim.
7. **Post-kuantum geçiş:** algoritma profili, karma geçiş, anahtar rotasyonu ve yetki geçmişleri için uzun vadeli arşiv biçimi.
8. **Kamuya açık denetim ve şeffaflık:** uygunluk denetimleri, yayımlanmış toplu rakamlar ve güven hizmetinin kendi şeffaflık raporu.

## 5. Yeniden kullanılacak mevcut standartlar ve çalışmalar

*Bu bölüm [standardizasyon notunda](STANDARDIZATION.md) tamamlanmıştır: her gereksinim en yakın mevcut standarda eşlenir, standartların nerede yetersiz kaldığı belirtilir ve eksik parça önerilir. Kısaca: parçalar kapsanıyor, birleşim kapsanmıyor.*

Bu analizin en çok dayandığı üçü:

| Bu analizin ihtiyacı | Onu sağlayan mevcut çalışma |
|---|---|
| Cihazdan hiç çıkmayan, yerelde eşleştirilen şablon | FIDO2 yerel kullanıcı doğrulama; ISO/IEC 27553-1:2022 (yerel kipler), biyometrik verinin ve türetilmiş verinin doğrulama sonucu hariç cihazdan çıkmamasını şart koşar; ISO/IEC 24745:2022 geri döndürülemezlik, izlenemezlik ve yenilenebilirlik için |
| Okuyucuda puanlanmış, sertifikalı canlılık denetimi | ISO/IEC 30107-1:2023, -2:2017, -3:2023 (göreli saldırgan sunum kabul oranını ekleyen), -4:2024 (mobil cihaz test profili); uzak PAD eşiği için NIST SP 800-63-4 §3.11 |
| Okuyucunun kontrol edebildiği kapsamlı, iptal edilebilir kimlik bilgisi | W3C Doğrulanabilir Kimlik Belgeleri 2.0 (Öneri, Mayıs 2025) ve VC Confidence Methods çalışma taslağı; `BiometricVectorConfidenceMethod` zaten hiçbir biyometrik verinin cihazdan çıkmadığı istemci tarafı sıfır bilgi eşleştirmeyi anlatıyor |

İki izlenecek çalışma, çünkü iniş noktasını değiştirebilirler: **ISO/IEC 30108-1:2026** (Kimlik Nitelikleri Doğrulama Hizmetleri) konu bakımından yakın ama kapsamından kimlik doğrulama protokollerine entegrasyonu çıkarıyor, ve **ISO/IEC 26655** (biyometrik deepfake saldırı tespiti) geliştirme aşamasında, ki bu önemlidir çünkü üretken saldırılar artık okuyucunun tehdit modelinin parçasıdır.

Saygı gösterilmesi gereken bir ön bilgi: bu analizin delegasyonlu varlık denetimi için tarif ettiği bulanık kasa mekanizması, verilmiş bir **Visa patent ailesinin** ("Delegated biometric authentication") konusudur. Standardizasyon notu bunu açıkça ele alır.

## 6. Gerçek bir uygulama için açık sorular

- **Çevrimdışı iptal sınırı ne olmalı?** Bir binanın ağ kesintisi boyunca çalışabilmesi ile geri alınmış bir yetkinin kapı açmaya devam ettiği pencere arasında bir değiş-tir alış-veri vardır. Bunu bir karar değil, bir ölçüm programı belirlemelidir.
- **Delegasyonlu kontrol için doğru minimum cihaz güveni nedir?** Bazı dağıtımlar donanım destekli bir attestation ister, bazıları platform düzeyini kabul eder. Her yerde en güçlüsünü şart koşmak, özelliğe en çok ihtiyacı olan cihazları dışlar.
- **Delegasyonu olmayan kullanıcıyla okuyucu ne yapar?** Yedek kimlik bilgileri önemlidir; model bir kapıdan kart okuyucusunun sökülmesi için gerekçe olmamalıdır.
- **Delegasyonlu kontrol ile kimliğin hukuki kanıtı arasındaki ilişki nedir?** Otelde, klinikte ya da sandık başında işletmecinin hukuki ödevi, delegasyonlu kontrolün sağladığından fazlasını gerektirebilir; bu sınır belirsiz bırakılmak yerine yazılmalıdır.
- **Yaş ve diğer öznitelik yetkileri nereden çıkar?** Öznitelik yetkili bir yerden gelmelidir ve o yer, altındaki kanıtın sahibi hâline gelir.
- **Okuyucu sertifikasyon düzenini kim finanse eder?** Bu kamusal bir maliyettir; finanse edilmezse okuyucular sertifikalanmaz ve model canlılıkta başarısız olur.
- **İşletim sistemi üreticisi ile işletmeci anlaşmazlığa düşerse ne olur?** Standart kimin karar vereceğini belirtmelidir ve bir itiraz yolu bulunmalıdır, aksi hâlde modelin tarafsızlığı yalnızca kâğıt üzerinde olur.
