English: [SPECIFICATION.md](SPECIFICATION.md)

# Delegeli Biyometrik Kimlik Doğrulama: Taslak Teknik Şartname (DBA-TS v0.1)

> **Taslak, yayımlanmış bir standart değildir.** Bu, [standardizasyon notunun](STANDARDIZATION.tr.md) yazılmasını önerdiği şartnamenin çalışma taslağıdır. Gereksinimlerde **"shall"**, önerilerde **"should"** ve izinlerde **"may"** kullanılır. Herhangi bir standart kuruluşunun sürecinden geçmemiştir ve bir uygunluk iddiası değildir. Ticari uygulamalar [ticari lisansa](COMMERCIAL-LICENSE.md) tabidir.

> **Açık kriptografik çekirdek hakkında.** Bölüm 8 — delegasyonlu varlık denetimi — bu şartnamenin kalbidir. **Kip B'nin biçimi kesindir:** doğrulamada okuyucu canlı sunumu yetkiye özgü dönüştürülmüş bir referansa karşı eşleştirir, cihaz aranmaz. Açık kalan, o referansı M1, M2 ya da M4'ten hangisinin kurduğu ve ölçülen hata oranlarıdır (O1). M3 yalnızca Kip A'dır. Aritmetiği çözülmüş gibi gösteren bir şartname değersiz olurdu; biçimi ve kalan seçimi açıkça belirten bir şartname bugün yazılabilecek en kullanışlı şeydir.

## 1. Kapsam

Bu şartname, bir kişinin **cihazı okuyucunun bulunduğu yerde bulunmadan**, kendi cihazında gerçekleştirilen bir biyometrik kontrolü kullanarak üçüncü taraf bir okuyucuya — bir satış noktası terminaline, bir erişim kontrol geçidine, bir ulaşım turnikesine, bir resepsiyon masasına — kimlik doğrulamasını tanımlar; bu sırada:

- biyometrik şablon, onu tutan cihazdan **hiç çıkmaz**;
- okuyucu **şablon, yeniden kullanılabilir biyometrik tanımlayıcı ya da yeniden kullanılabilir bir görüntü almaz**; Kip A'da kısa ömürlü imzalı bir kanıtı doğrular; Kip B'de Bölüm 12.1.2 altındaki yetkiye özgü dönüştürülmüş bir referansı tutabilir;
- delegasyon **kişinin kendisi tarafından**, okuyucu başına, amaç başına verilir, **sürelidir** ve **geri alınabilirdir**;
- okuyucu, canlı bir sunumu bir sunum saldırısından **yayımlanmış bir oranla ayırt eder**;
- bir Kip A kanıtı başka bir okuyucuda, başka bir amaçla ya da delegasyon geri alındıktan sonra **tekrar oynatılamaz**; bir Kip B eşleşmesi tekrar oynatılmış bir sunumda (Bölüm 9), bayat durumda (Bölüm 10.3) ya da bayat referansta (Bölüm 10.5) **başarılı olamaz**.

**Kapsam dâhilinde:** delegasyon yetkisi nesnesi; iki doğrulama kipi; delegasyonlu varlık denetimi; kanıt biçimi ve bağlılıkları; durum ve iptal mekanizması; gizlilik kipleri; okuyucu uygunluğu ve sertifikasyon gereksinimleri; kriptografik profil, post-kuantum gereksinimleri dâhil.

**Kapsam dışında:** şablonun nasıl kaydedildiği, güvenli öğe içinde nasıl korunduğu ya da *cihazın içinde* nasıl eşleştirildiği — bunlar cihaz platformunun işidir ve mevcut standartların konusudur. Tek kısmi istisna Bölüm 8'dir: orada okuyucunun şablon yerine *dönüştürülmüş bir referansa* karşı eşleştirme yaptığı mekanizmalar ele alınır; bu eşleştirmenin nasıl yapıldığı seçilen mekanizmaya ve ISO/IEC 24745'e bırakılır ve Bölüm 8, aritmetiği değil sonucun karşılaması gereken gereksinimleri belirtir. Ayrıca kapsam dışında: okuyucunun kamerasının bir görüntüyü nasıl yakaladığı ve sunum saldırısı tespitini nasıl yaptığı — bu, ISO/IEC 30107 ve ISO/IEC 19795'in konusudur; başarılı bir doğrulamanın hukuki etkisi; hangi ödemelerin veya işlemlerin bunu kullanabileceği; ve herhangi bir dağıtımın ticari koşulları.

**Hukuktan bağımsızdır.** Bu şartname, uyumlu bir uygulamanın ne yapması gerektiğini söyler. Hukuki haklar yaratmaz ve bir işleme için hukuki sebep seçmez. Bir delegasyon yasal bir şartı karşılamak için kullanıldığında, o şart kanunla belirlenir ve bu şartname yalnızca akışın bu koşullara uyduğunu söyler.

## 2. Normatif atıflar

Aşağıdakiler, içerikleri bu şartnamenin bir parçası olacak biçimde atıf yapılır. Tarihli atıflar için yalnızca atıf yapılan basım geçerlidir.

| Atıf | İlgililik |
|---|---|
| ISO/IEC 24745:2022 | Biyometrik bilgi koruma: geri döndürülemezlik, izlenemezlik, yenilenebilirlik, iptal edilebilirlik; dağıtık karşılaştırma modelleri dâhil uygulama modelleri; yenilenebilir bir biyometrik referansın tanımlandığı çerçeve |
| ISO/IEC 27553-1:2022 | Mobil cihazlarda yerel kiplerde biyometrik kimlik doğrulamanın güvenlik ve gizlilik gereksinimleri; biyometrik verinin ve türetilmiş verinin doğrulama sonucu hariç cihazdan çıkmaması şartı |
| ISO/IEC 27553-2:2025 | Aynısı, uzak kiplerde |
| ISO/IEC 30107-1:2023, -2:2017, -3:2023, -4:2024 | Sunum saldırısı tespiti: çerçeve, veri biçimleri, test ve raporlama, mobil cihaz test profili |
| ISO/IEC 39794-1 ve ilgili modalite bölümleri | Biyometrik veri değişim biçimleri; bir biyometrik örneğin bir kanıtta ya da kayıtta taşındığı yerde |
| ISO/IEC 19795-1:2021, -10:2024 | Biyometrik performansın testi ve demografik gruplar arasındaki performans değişiminin ölçülmesi |
| NIST SP 800-63-4 | Kimlik kanıtlama, kimlik doğrulama ve federasyon; özellikle §3.11 (biyometrikler) ve uzak sunum saldırısı gereksinimleri ile eşikleri |
| RFC 5280 | Sertifika iptal listeleri; bir durum mekanizması bunları kullandığında |
| RFC 9576, 9577, 9578 | Privacy Pass mimarisi, HTTP kimlik doğrulama şeması ve ihra protokolleri; izlenemez kip kullanıldığında |
| NIST FIPS 203, 204, 205 | ML-KEM, ML-DSA ve SLH-DSA; post-kuantum profili kullanıldığında |
| İlgili IETF TLS şartnameleri | Okuyucu-aracı oturumu korunduğunda |
| ISO/IEC 25456 ( enjeksiyon saldırısı tespiti, 2027 bekleniyor) | Dijital enjeksiyon saldırılarının tespiti; sunum saldırısı ve deepfake tespitiyle birlikte okuyucunun tehdit modelini tamamlar |
| ISO/IEC 25049-1/-2 (Özelleştirilebilir biyometrik web hizmetleri) | Delegasyonlu bir okuyucu bir biyometrik web hizmeti olarak gerçekleştirildiğinde; uyumlu bir uygulama bu çalışmayı **tekrar etmek yerine onunla birlikte çalışmalıdır** |
| ZCAP (W3C Topluluk Grubu) | Bölüm 5.3'te yetkinin üzerine kurulduğu delegasyon zinciri, daraltma, süre ve çevrimdışı doğrulama yetenek modeli |

*Normatif atıflar, [standardizasyon notunda](STANDARDIZATION.tr.md#bölüm-6-lisanslama-haklar-ve-yol) tarif edilen yola göre kurul başına kesinleştirilecektir. Bölüm 11'de composite ya da karma olarak işaretlenen kalemler yazıldığı sırada RFC Editor incelemesindedir ve nihai atıflarıyla anılacaktır.*

## 3. Terimler ve tanımlar

| Terim | Tanım |
|---|---|
| **Özne (Subject)** | Bir delegasyonu kullanabilecek doğal kişi. Takma adla anılır; şartname, özneyin okuyucuya kimliğinin bildirilmesini asla gerektirmez. |
| **Delegasyon aracısı (Delegating broker)** | Öznenin biyometrik şablonunu güvenli donanımın içinde tutan, cihaz anahtarını tutan ve yetki oluşturan ya da geri alan tek taraf. Uygulamada: cihazın işletim sistemi ya da onun üzerinde çalışan bir cüzdan uygulaması. |
| **Okuyucu (Reader)** | Öznenin fiziksel olarak bulunduğu cihaz: bir terminal, bir geçit, bir kiosk. Sunumu yakalar ve sunum saldırısı tespitini yapar. Kip A'da meydan okumalar üretir ve kanıtları doğrular. Kip B'de sunumu önbellekteki dönüştürülmüş referansa karşı eşleştirir ve yetki durumu ile referans yaşını kontrol eder. |
| **İşletmeci (Operator)** | Okuyucuları işleten ve doğrulama sonuçlarını alan kuruluş: bir banka, bir bina yöneticisi, bir ulaşım kurumu. |
| **Güvenen taraf (Relying party)** | Doğrulama sonucunu tüketip karar veren hizmet. İşletmeci olabilir ya da işletmecinin sonucu ilettiği üçüncü bir taraf olabilir. |
| **Güven hizmeti (Trust service)** | Açık güven çapalarını tutan, yetkileri kaydeden ve durum ile iptal bilgisini yayımlayan tarafsız taraf. Biyometrik veri tutmaz. |
| **Delegasyon yetkisi (Delegation grant)** | Bir öznenin, adlandırılmış bir okuyucunun belirli bir amaç için delegasyonlu varlık denetimi gerçekleştirmesine kişisel, kapsamlı, süreli ve geri alınabilir yetkisini kaydeden nesne. |
| **Delegasyon kanıtı (Delegation proof)** | Bir **Kip A** okuyucusunun doğruladığı kısa ömürlü imzalı beyan. Biyometrik veri ve yeniden kullanılabilir tanımlayıcı taşımaz. Kip B bu nesneyi kullanmaz; bkz. Bölüm 7.2. |
| **Delegasyonlu varlık denetimi** | Okuyucuda, önünde duran kişinin adlandırılmış yetkinin öznesi olduğunu ortaya koyma eylemi. Bölüm 8. |
| **Dönüştürülmüş referans** | ISO/IEC 24745 anlamında yenilenebilir, geri dönüşsüz, izlenemez, biyometrikten türetilmiş bir nesne; bir Kip B okuyucusu canlı sunumu buna karşı eşleştirebilir. Kaydedilmiş şablon değildir. |
| **Referans yaşı** | Kip B okuyucusunun önbelleğindeki dönüştürülmüş referansın aracı tarafından basılmasından bu yana geçen süre. Sınır Bölüm 10.5. |
| **Sunum (Presentation)** | Denetim anında okuyucunun yakaladığı biyometrik örnek. Denetim sonrası imha edilir. |
| **Sunum saldırısı** | Bir biyometrik sensöre, öznenin samimi sunumu olmayan bir sunum. |
| **Okuyucu sınıfı (Reader class)** | Tek bir yetkinin kapsayabileceği, güven hizmeti tarafından tanımlanmış okuyucu grubu — örneğin "B yapısının bütün geçitleri". |
| **Öznitelik açıklaması (Attribute disclosure)** | Okuyucunun varlık sonucuna ek olarak öğrendiği beyan: bir kimlik referansı, bir yaş bandı ya da "yetkili"den fazlası. |
| **Durum (Status)** | Güven hizmetinin yayımladığı bir yetkinin güncel hâli: etkin, iptal edilmiş, süresi dolmuş ya da askıya alınmış. |

## 4. Roller, güven ve tehdit modeli

### 4.1 Her taraf ne tutar

| Taraf | Tuttuğu | Tutmaması gereken |
|---|---|---|
| Öznenin cihazı | Biyometrik şablon, güvenli donanımın içinde; cihaz anahtarı | — |
| Delegasyon aracısı | Şablon, cihaz anahtarı, yetki listesi, imzalama anahtarı | Bölüm 12.1.2 altındaki yetkiye özgü dönüştürülmüş referans dışında, özneyi bir okuyucuya tanıtan ve cihazdan çıkan bir şey |
| Okuyucu | Açık anahtar, güven çapaları, önbelleğe alınmış durum, doğrulama sonucu; Kip B'de Bölüm 12.1.2 altındaki yetkiye özgü dönüştürülmüş referans | Şablon, yetkiler arasında yeniden kullanılabilir biyometrik tanımlayıcı, saklanan sunum, hareket profiline dönüştürülebilen günlük |
| İşletmeci | Delegasyon tanımlayıcısı, olay kaydı, ödeme ya da erişim kaydı | Şablon, amacın gerektirdiğinin ötesinde yeniden kullanılabilir özne tanımlayıcısı |
| Güven hizmeti | Açık anahtarlar, yetki tanımlayıcıları, durum, iptal listesi; Kip B'de Bölüm 12.1.2(a) ve (e)'yi karşılayan yetkiye özgü dönüştürülmüş referanslar | Şablon, yüz görüntüsü ya da kaydedilmiş şablonun türetilebileceği bir değer |

### 4.2 Güven varsayımları

- **Cihazın güvenli donanımının**, platform için sertifikalandırılan düzeyde fiziksel ve yazılım saldırılarına dayandığı varsayılır. Bir yetki platformun güvence düzeyini devralır; onu yükseltmez.
- **Okuyucunun dürüst ama gizlilik açısından güvenilmez olduğu** varsayılır: öznenin bir gözlemcisi olarak ele alınır, şablonunun sahibi olarak değil. Kip B'de Bölüm 12.1.2 altında dönüştürülmüş bir referans tutabilir; bu şablon değildir.
- **Güven hizmetinin doğru ve erişilebilir ama gizlilik açısından güvenilmez olduğu** varsayılır — anahtarları, durumu ve Kip B'de yetkiye özgü dönüştürülmüş referansları tutar. Ele geçirilmesi şablon ya da yüz görüntüsü kazandırmaz.
- **İşletmecinin kaydettiği konusunda dürüst olduğu** varsayılır ve Bölüm 12'deki denetime tabidir.

### 4.3 Düşmanlar ve gereken özellikler

| # | Düşman | Gereken özellik |
|---|---|---|
| A1 | Fotoğraf, maske, ekran tekrar oynatımı ya da basılı görüntü sunar | Okuyucu saldırıyı saldırı sınıfı başına yayımlanmış bir oranla tespit eder (Bölüm 9) |
| A1b | Fiziksel olarak sunmak yerine dijital ya da sentetik bir sunum enjeksiyonu yapar | Okuyucu, yakalama cihazı ve platformun imkân verdiği ölçüde enjeksiyon saldırılarını tespit eder; bunu yapamayan bir okuyucu, tehdit modeli enjeksiyonu içeren bir denetim için **sertifikalandırılmaz** ve dağıtım bunu **söylemelidir** |
| A2 | Geçerli bir Kip A kanıtını kaydeder ve aynı okuyucuda tekrar oynatır | Kanıt geçerlilik penceresi içinde tek kullanımlıktır; okuyucu tüketilmiş kanıtı reddeder |
| A3 | Geçerli bir Kip A kanıtını kaydeder ve **başka** bir okuyucuda sunar | Kanıt bir okuyucu tanımlayıcısına bağlıdır; okuyucu başka bir okuyucuyu adıyla belirten kanıtı reddeder |
| A4 | **Bir** amaç için aldığı Kip A kanıtını başka bir amaçla kullanır | Kanıt bir amaca bağlıdır; okuyucu başka bir amaç adı taşıyan kanıtı reddeder |
| A5 | Özne geri çektikten sonra bir yetkiyi kullanır | İptal, Bölüm 10'daki sınır içinde okuyucuya ulaşır; okuyucu bayat durumda kapalı çalışır |
| A6 | Cihazı çalar ve öznenin biyometriğini zorlar | Yetki oluşturmak, yenilemek ve bir Kip A yetkisini kullanmak cihazda öznenin kendi biyometriğini ister; Kip B, okuyucudaki canlı sunumla kullanılır, dolayısıyla cihaz hırsızlığına yanıt iptal (Bölüm 10.4) ve referans yaşı sınırıdır (Bölüm 10.5); yetkiler özne tarafından görülebilir |
| A7 | Kötü niyetli bir uygulama yetki ister | Yetkiler yalnızca işletim sisteminin güvenlik alt sistemi tarafından oluşturulur; bir uygulama isteyebilir ama oluşturamaz; kullanıcının onaylamadığı bir kapsam isteği reddedilir |
| A8 | İşletmeci gizli bir biyometrik veritabanı kurar | Bölüm 12'deki denetim, saklanan sunumları, yeniden kullanılabilir tanımlayıcıları ve birleştirilmiş günlükleri tespit eder; işletmecinin sertifikasyonu geri çekilir |
| A9 | Saldırgan **daha sonra** klasik imzayı kırar ve önbelleğe alınmış kanıtları tekrar oynatır | Bölüm 11'deki kriptografik profil, post-kuantum yetenekli bir imza kullanır ve yetki ömürleri geçiş ufkunun altında sınırlanır |
| A10 | Bir öznenin yetkisi, cihazı elde eden kişiye miras kalır | Bir yetki cihaz anahtarına bağlıdır; cihazın kaybı onun yetkilerini iptal eder (Bölüm 10.4) |

## 5. Delegasyon yetkisi

### 5.1 Alanlar

Bir delegasyon yetkisi aşağıdakileri taşımalıdır:

| Alan | Gereksinim |
|---|---|
| `grant_id` | **Tekil** olmalı ve **asla yeniden kullanılmamalıdır** |
| `delegatee_ref` | Delegasyon aracısını sabit bir takma adla tanımlamalı, özneyi değil |
| `subject_ref` | Özneyi bir takma adla tanımlamalı. **Doğrudan tanımlayıcı bir değer olmamalıdır** |
| `reader_scope` | Tek bir okuyucuyu ya da adlandırılmış bir okuyucu sınıfını belirtmeli. **Hizmet kategorisi olmamalıdır** |
| `purpose` | Kayıtlı amaçlardan biri olmalı: ödeme, erişim, yaş doğrulama, devam, kimlik teyidi. **Boş olmamalıdır** |
| `attributes` | Okuyucunun öğrenebileceğini belirtmeli. Şunlardan biri olmalı: açıklama yok, yaş bandı, kapsamlı kimlik referansı |
| `not_before`, `not_after` | **Bulunmalıdır.** Özne açıkça kalıcı bir yetki seçmediyse `not_after` sınırsız olmamalıdır; kalıcı seçildiğinde bu özneye kalıcı olarak gösterilmelidir |
| `max_uses` | **Bulunabilir.** Varsa, sayaç erişildiğinde yetki geçersiz olmalıdır |
| `verification_mode` | Kip A ya da Kip B olmalıdır. Bir yetki ikisinde birden kullanılamaz (Bölüm 7.3.2) |
| `proof_key` | Bu yetki için Kip A kanıtlarını imzalayan anahtarın açık anahtarı ya da güven hizmetindeki referansı olmalıdır. Yalnızca Kip B yetkisinde **bulunmayabilir** |
| `reference` | Kip B yetkisinde, güncel dönüştürülmüş referansı ya da güven hizmetindeki konumunu **tanımlamalıdır** |
| `reference_issued_at` | Kip B yetkisinde, güncel dönüştürülmüş referansın basıldığı zaman **olmalıdır** |
| `attestation` | Aracının anahtarını platformun güvence düzeyine bağlayan bir cihaz attestation'ı olmalıdır |

### 5.2 Kapsam kuralları

5.2.1 Bir yetki **yalnızca** `reader_scope` içindeki bir okuyucuda ve **yalnızca** kendi `purpose` amacı için kullanılabilmelidir.
5.2.2 Bir yetki, aynı okuyucuda ve aynı işletmeci için bile, adını belirtmediği başka bir amaç için kullanılamaz.
5.2.3 Amacı ödeme olan bir yetki, ayrıca bir işleme, bir tutara ve bir üreticiye bağlanabilir; bu bağlamayı aşan bir kanıtı okuyucu reddetmelidir.
5.2.4 Bir yetki bir uygulama tarafından oluşturulamaz. Yalnızca öznenin işletim sistemi bir yetki oluşturabilir.
5.2.5 Özne, herhangi bir anda kendi cihazı için var olan bütün yetkileri ve her bir yetkinin kullanımını görebilmelidir.

### 5.3 Yetki bir yetenek (capability) olarak

5.3.1 Bir delegasyon yetkisi, **yetenek** olarak modellenmelidir: aracıda tutulan, kopyalanamaz bir yetki beyanı; devredilebilir, daraltılabilir ve süresi bitirilebilir — W3C Topluluk Grubu'nun ZCAP çalışmasında geliştirilen ve OAuth ile WIMSE çalışma gruplarında delegeli yetki için zaten kullanılan model. Uyumlu bir uygulama, paralel bir mekanizma tanımlamak yerine bir yetkiyi bu biçimde temsil etmelidir.

5.3.2 Bir yetki **bir yetenek zinciri** olmalıdır: aracı, öznenin delegasyon verme yetkisini temsil eden bir kök yeteneği tutar ve her yetki, bir `parentCapability` bağıyla onun çocuğudur. Okuyucu, bir kanıtı kabul etmeden önce zinciri köke kadar doğrulamalı ve kırık, güven hizmetinin sicilinde köklenmemiş ya da zincirin herhangi bir yerinde iptal edilmiş bir zinciri reddetmelidir.

5.3.3 Bir yetki bir **son kullanma tarihi** taşımalıdır ve süresi dolmuş bir yetki istisnasız reddedilmelidir. Son kullanma tarihi olmayan bir yetki **uyumlu değildir**.

5.3.4 Bir yetki **daraltılabilir** — ebeveynine göre daraltılmış — ve okuyucu, daraltmanın yetkiyi genişletmediğini doğrulamalıdır: bir çocuğun kapsamı, amacı, öznitelikleri ve ömrünün her biri ebeveyninin alt kümesi olmalıdır.

5.3.5 Zincir, yayımlanmış güven çapalarına karşı **çevrimdışı doğrulanabilir** olmalıdır; böylece bir Kip B okuyucusu, zincirin kendisini doğrulamak için güven hizmetine başvurmak zorunda kalmaz. Başvuru durum ve güncel dönüştürülmüş referans için gerekir ve yalnızca Bölüm 10.3 ile Bölüm 10.5'teki sınırlar içinde.

5.3.6 Bir yetenek temsili, bu şartnamenin hiçbir gereksinimini zayıflatmamalıdır. Kip A'da geçerli bir yetenek olan bir kanıt **yine de** Bölüm 6'yı karşılamalıdır — okuyucu bağlılığı, amaç bağlılığı, tazelik ve tek kullanım kuralı *bu* protokolün özellikleridir ve yetenek mekanizması tarafından sağlanmaz. Kip B'de geçerli bir yetenek zinciri, okuyucunun varlık denetimini yine de güncel dönüştürülmüş referansa karşı Bölüm 8'e göre yapmasını bırakmalıdır.

5.3.7 Yetenek modeli, delegasyonlu kontrolün **yetki** yarısını sağlar. **Varlık** yarısı — okuyucunun önündeki kişinin yetkinin adlandırdığı kişi olduğunu kanıtlamak — onun tarafından sağlanmaz. Kip B'de o yarı, dönüştürülmüş bir referansa karşı eşleştirmedir (Bölüm 7.2 ve 8.2). Referansı M1, M2 ya da M4'ten hangisinin kurduğu Bölüm 8'deki açık soru olarak kalır.

### 5.4 Onay töreni

5.4.1 İşletim sistemi, yetki oluşturulurken şunları göstermelidir: yetkinin kapsayacağı okuyucu, amaç, okuyucunun öğreneceği öznitelikler, ömür ve yetkinin kalıcı olmasının sonucu.
5.4.2 Onay töreni **açık bir kullanıcı eylemiyle** başlatılmalı ve daha geniş bir izinden çıkarılamamalıdır.
5.4.3 Onay **yetki başına** olmalıdır. Bir okuyucu sınıfını kapsayan bir yetki, sınıfı ve bilinen hâliyle boyutunu adlandırarak sunulmalıdır.
5.4.4 Bir yetki varsayılan olarak, varsayılan açık olarak ya da başka bir yetkinin yan etkisi olarak oluşturulamaz.

## 6. Delegasyon kanıtı

Bu bölüm **Kip A** için geçerlidir. Bir Kip B okuyucusu Bölüm 6 kanıtı **istememelidir** ve bir aracı, okuyucunun henüz üretmediği bir meydan okumayı yanıtladığını iddia eden bir kanıtı önceden **düzenlememelidir**.

### 6.1 Alanlar

Bir delegasyon kanıtı şunları taşımalıdır:

| Alan | Gereksinim |
|---|---|
| `grant_id` | Kanıtın kullandığı yetki |
| `reader_id` | Kanıtın bağlı olduğu okuyucuyu **tanımlamalıdır** |
| `purpose` | Kanıtın bağlı olduğu amacı **tanımlamalıdır** |
| `challenge` | Kanıtın yanıtladığı okuyucunun taze meydan okuması olmalıdır |
| `presence_result` | Delegasyonlu varlık denetiminin sonucu (Bölüm 8) |
| `attributes` | Yalnızca yetkinin izin verdiği öznitelikler |
| `issued_at`, `expires_at` | Geçerlilik penceresi. **Kısa olmalıdır:** bir kanıtın ömrü **Bölüm 10.3'teki iptal sınırını aşmamalıdır** |
| `counter` | Yetki kullanım sınırlıysa kullanım sayacı |
| `signature` | Önceki alanların kanonik kodlaması üzerine, yetkinin kanıt anahtarıyla atılan imza |

### 6.2 Bağlılık kuralları

6.2.1 Okuyucu, `reader_id`si kendi tanımlayıcısıyla eşleşmeyen bir kanıtı reddetmelidir.
6.2.2 Okuyucu, `purpose`si gerçekleştirdiği işlemle eşleşmeyen bir kanıtı reddetmelidir.
6.2.3 Okuyucu, `challenge`ı kendi ürettiği ve henüz tüketmediği bir meydan okumayla eşleşmeyen bir kanıtı reddetmelidir.
6.2.4 Okuyucu, kendi saatine göre geçerlilik penceresinin dışındaki bir kanıtı reddetmelidir; okuyucunun saati Bölüm 9.4'te belirtilen tolerans içinde olmalıdır.
6.2.5 Okuyucu, imzası yetki için kayıtlı anahtar altında doğrulanmayan ya da yetkisi güven hizmetinde yayımlanmamış bir kanıtı reddetmelidir.
6.2.6 Okuyucu, `attributes`i yetkinin izin verdiğinin ötesinde olan bir kanıtı reddetmeli ve izin verilmeyen bir özniteliğe değil, ona **görmezden gelmelidir**.

### 6.3 Bir kanıtın içermemesi gerekenler

6.3.1 Bir kanıt **biyometrik şablon, referans görüntü, yerleştirme, öznitelik vektörü ya da bunlardan türetilebilecek bir değer içermemelidir**.
6.3.2 Bir kanıt **okuyucular arasında kararlı bir özne tanımlayıcısı içermemelidir**.
6.3.3 Bir kanıt, yetkinin izin verdiği `attributes`in ötesinde öznenin kişisel verisini içermemelidir.
6.3.4 Bir kanıt **tek kullanımlık bir yapıt olmalıdır**: okuyucu, kabul ettiği her kanıtın tanımlayıcısını kanıtın geçerlilik penceresi boyunca kaydetmeli ve tekrarlanan bir kanıtı reddetmelidir.

## 7. Doğrulama kipleri

### 7.1 Kip A — çevrimiçi, aracı aracılığı

Aracı erişilebilirdir. Okuyucu canlı bir sunum yakalar, sunum saldırısı tespitini yapar ve bir meydan okumayı o sunumun korunan bir türeviyle birlikte gönderir (M3 ya da okuyucuya dönüştürülmüş referans yerleştirmeyen eşdeğer). Aracı bu sunumu güvenli donanımdaki şablonla eşleştirir ve bir Bölüm 6 kanıtı döner.

Telefonu cihazın kendi biyometriğiyle açmak, okuyucunun önündeki kişi başkasıyken, P1'i **karşılamaz**. Bir Kip A denetimi **okuyucuda yakalanan** sunumu eşleştirmelidir.

```
Okuyucu                          Aracı (cihaz)                Güven hizmeti
  |                                  |                             |
  |-- CHALLENGE(reader_id, purpose,  |                             |
  |   nonce, ts, korunan örnek) ---->|                             |
  |                                  |-- STATUS(grant_id) ------>|
  |                                  |<-- durum ------------------|
  |                                  |                             |
  |   (okuyucu sunumunu cihazdaki    |                             |
  |    şablonla eşleştir)            |                             |
  |                                  |                             |
  |<-- Delegation Proof --------------|                             |
  |   (imza, bağlılıklar,            |                             |
  |    varlık sonucunu doğrula)       |                             |
  |                                  |-- USE(grant_id) ---------->|
```

7.1.1 Okuyucu her deneme için en az 128 bit entropili, taze ve öngörülemeyen bir meydan okuma üretmelidir.
7.1.2 Aracı, yapılandırılabilir bir azami yaştan eski bir meydan okumayı yanıtlamayı reddetmeli ve aynı meydan okumayı iki kez yanıtlamamalıdır.
7.1.3 Aracı, yalnızca okuyucunun sunumunu (ya da korunan türevini) cihazdaki şablonla eşleştirdikten sonra yanıt vermeli ve hangi yetkiyi kullandığını kaydetmelidir. O sunuma bağlı olmayan başarılı bir cihaz kilidi **yeterli değildir**.
7.1.4 Okuyucu **kapalı çalışmalıdır**: araca ulaşılamıyorsa denetim başarısız olmalıdır. Yetki Kip B'de düzenlenmemişse Kip B'ye geri dönülmemelidir.

### 7.2 Kip B — çevrimdışı, önceden teslim edilmiş referans

Aracı doğrulama anında **aranmaz**. Yetki oluşturulurken ve Bölüm 8.6 altındaki her yenilemede aracı, güncel cihaz kaynağından (Bölüm 8.6.4) yenilenebilir, geri dönüşsüz, izlenemez bir dönüştürülmüş referans türetir, cihaz anahtarıyla imzalar ve güven hizmetine kaydeder. Doğrulamada okuyucu:

1. canlı bir sunum yakalar ve sunum saldırısı tespitini yapar (Bölüm 9);
2. özellik çıkarır ve bunları önbellekteki dönüştürülmüş referansla eşleştirir;
3. yetki durumunun Bölüm 10.3'teki sınırdan ve referansın Bölüm 10.5'teki sınırdan eski olmadığını kontrol eder;
4. kabul eder ya da reddeder. Herhangi bir sınır aşılmışsa ya da önbellekte durum veya referans yoksa **kapalı çalışmalıdır**.

Cihazla meydan-okuma-yanıt turu yoktur ve Bölüm 6 kanıtı yoktur.

```
Okuyucu                          Güven hizmeti                  Aracı (cihaz)
  |                                  |                             |
  |   (yetki / yenilemede, şimdi değil)|<-- imzalı referans --------|
  |                                  |                             |
  |-- önbellek durum + referans ---->|                             |
  |   (10.3 ve 10.5 içinde)          |                             |
  |                                  |                             |
  |   yakala + PAD + eşleştir        |                             |
  |   önbellekteki referansa karşı   |                             |
  |   (cihaz aranmaz)                |                             |
```

7.2.1 Bir Kip B doğrulaması Bölüm 6 kanıtı **istememelidir** ve aracının yanıtladığı bir okuyucu meydan okumasına **bağlanmamalıdır**.
7.2.2 Okuyucu, Bölüm 10.3'teki sınırdan eski olmayan bir duruma karşı doğrulamalı ve durumu yoksa **kapalı çalışmalıdır**.
7.2.3 Okuyucu, Bölüm 10.5'teki sınırdan eski olmayan bir dönüştürülmüş referansa karşı doğrulamalı; referans yoksa, bu yetki ve okuyucu kapsamına bağlı değilse ya da emekli edildiyse **kapalı çalışmalıdır**.
7.2.4 Kip B ile çalışan bir okuyucu, doğruladığı durumun yaşını ve referansın yaşını göstermeli ya da kaydetmelidir; böylece işletmeci kabul ettiği iptal maruziyetini ve biyometrik sapma maruziyetini bildirebilir.
7.2.5 Dönüştürülmüş referans Bölüm 12.1.2'yi **tam olarak** karşılamalıdır. Karşılayamayan bir okuyucu Kip B'de **çalışmamalıdır**.
7.2.6 Referansın yenilenmesi doğrulama yolunun **dışında**, Bölüm 8.6'ya göre olmalıdır. Bir Kip B denetimini tamamlamak için cihazı aramak **uyumlu değildir**.

### 7.3 Kip seçimi

7.3.1 Kip, yetkinin bir özelliği olmalı ve **özneye görünür olmalıdır**, çünkü pratikte "geri alındı"nın ne anlama geldiğini ve okuyucuya dönüştürülmüş referans konup konmadığını belirler.
7.3.2 Bir yetki **her iki kipte birden kullanılabilir olmamalıdır**: Kip A canlı meydan okumalı bir Bölüm 6 kanıtı kullanır; Kip B önbellekteki referans eşleştirmesini kullanır. İkisinin tekrar oynatma özellikleri farklıdır.

## 8. Delegasyonlu varlık denetimi

**Bu bölüm, neyin başarılması gerektiğini belirtir.** Kip B'nin *biçimi* Bölüm 7.2'dir. Kalan seçim, referansı M1, M2 ya da M4'ten hangisinin kurduğunu ve ölçülen oranlarıdır (O1). M3 yalnızca Kip A'dır.

### 8.1 Gereksinimler

Bir delegasyonlu varlık denetimi:

- **P1 — Varlık.** Okuyucunun önündeki kişinin, adlandırılmış yetkinin öznesi olduğunu ortaya koymalıdır.
- **P2 — Açıklama yapmama.** Şablon, referans görüntü, öznitelik vektörü ya da okuyucunun özneyi sonraki ziyarette tanımasına yarayacak bir tanımlayıcı açığa çıkarmamalıdır.
- **P3 — Tazelik.** Kip A'da, yakalanmış bir kanıtın tekrar oynatılamayacağı için okuyucunun güncel meydan okumasına bağlanmalıdır. Kip B'de, Bölüm 9'u geçen bir canlı sunum, Bölüm 10.5'ten eski olmayan bir referansa karşı, Bölüm 10.3'ten eski olmayan bir durumla birlikte olmalıdır. Kip B, araca doğrulama başına meydan okuma **istememelidir**.
- **P4 — Canlılık.** Bölüm 9'a uyan bir sunum saldırısı denetiminden geçmiş bir sunuma dayanmalıdır.
- **P5 — Ölçülebilirlik.** Saldırı sınıfı başına yayımlanmış bir yanlış kabul oranı ve yayımlanmış bir yanlış reddetme oranı olmalıdır.
- **P6 — Çevrimdışı yetenek.** Kip B'de, araca ağ erişimi olmadan tamamlanabilmelidir.

### 8.2 Neden olağan yanıt burada geçerli değil

Geleneksel bir biyometrik denetim "bu örnek bu şablonla eşleşiyor mu" sorusuna yanıt verir. Burada şablon, hazır bulunmayan bir cihazdadır ve örnek, şablonu tutmayan bir okuyucudadır. İki yarı farklı yerlerdedir; bu, bütün zorluğun kendisidir ve mevcut standartların bu problemi çözebilmesinin nedeni, cihazın hazır bulunmasını şart koşmalarıdır.


Kip B'deki delegasyonlu varlık denetimi **önceden teslim edilmiş bir nesneye karşı biyometrik eşleştirmedir:** cihaz, şablonundan yenilenebilir, geri dönüşsüz, izlenemez bir dönüştürülmüş referans türetir ve bunu **bir kez, yetki oluşturulurken** güven hizmetine ya da okuyucuya kaydeder (Mekanizma M2, Bölüm 8.3). Doğrulama anında okuyucunun kamerası, kızılötesi sensörü ve canlılık denetimi bir canlı sunum yakalar, özellikleri çıkarır ve önceden teslim edilmiş referansla eşleştirir. **Cihaz aranmaz.** Gerçek zamanlı bir bağlantı, bir meydan-okuma-yanıt turu ve okuyucu ile cihaz arasında bir ağ bağımlılığı yoktur.

Bu, kriptografik anlamda bir imza doğrulaması değildir. Bir imza sabit bir mesajı doğrular; bir kamera yakalaması sabit bir mesaj değildir ve hiçbir imza şeması taze bir yakalamayı önceden imzalanmış bir değerle eşleştiremez. Cihazın önceden teslim ettiği nesne bir *yakalama üzerine imza* değil, bir *dönüştürülmüş referanstır:* cihazın imzası referansı okuyucuya kimliklendirir; referans canlı biyometrik eşleştirmesinin yapıldığı nesnedir. İkisi de gereklidir. Denetim, Kip B'de P3'ü canlı sunumla (Bölüm 9), referans yaşı sınırıyla (Bölüm 10.5) ve durum yaşı sınırıyla (Bölüm 10.3) karşılar — bir doğrulama başına meydan okuma ile değil ve referansı tek kullanımlık saymakla değil. Referans yetkiye özgüdür ve yenilene, emekli edile ya da süre dolana dek yeniden kullanılabilirdir. Tek kullanımlık olan, başarılı bir denetimin işletmecinin olay kaydınıdır.
### 8.3 Aday mekanizmalar

**M1 — Bulanık kasa / paylaşımlı sır.** Aracı, şablonu bir *bulanık taahhüt* (noktalar setinin sahte noktalarla karıştırılmış hâli) aracılığıyla bir sırrı kilitler. Okuyucu canlı bir örnek yakalar, eşleşen alt kümeyi bulur, interpolasyonla sırrı yeniden kurar ve bilgisini kanıtlar.

- *Gizlilik:* güçlü. Okuyucu yalnızca "kasa açıldı" bilgisini öğrenir.
- *Maliyet:* okuyucu, bulanık bir yapıya karşı tam bir eşleştirme yapar; hata oranları ayarlanabilir ama yapı, sahte noktaların dağılımı yoluyla şablon hakkında biraz bilgi sızdırır ve bu analiz edilmelidir.
- *İptal:* kasa döndürülebilir, ama yakalanmış bir kasadır ve emekliye kadar yakalanmış kasadır.
- *Durum:* mekanizma **zaten ISO tarafından onaylanmış bir teknoloji örneğidir** — ISO/IEC 24745:2022 Ek C, "Biyometrik bilgi koruma için teknoloji örnekleri", bulanık kasayı Tablo C.1'de yardımcı veriyle bir takma ad oluşturmanın somut bir yolu olarak listeler (terim 3.35). Mekanizma ayrıca verilmiş bir **Visa patent ailesinin** ("Delegated biometric authentication") konusudur. Bir uygulayıcı kullanmadan önce **görüş almalıdır**.

**M2 — Güvenli karşılaştırma ile yenilenebilir ve iptal edilebilir dönüştürülmüş referans.** Aracı, okuyucunun doğrudan karşılaştırma yapabileceği bir *yenilenebilir biyometrik referans* — dönüştürülmüş, korumalı bir gösterim — üretir, ISO/IEC 24745 anlamında; dağıtık karşılaştırma modelleri dâhil. Her yetki için taze bir referans türetilir, dolayısıyla bir yetki, referansını kullanmaktan vazgeçmekle iptal edilir.

- *Gizlilik:* tamamen dönüşüme ve yetki başına çeşitlemeye bağlıdır; şartname, referansların yetkiler arasında izlenemez olmasını ve referansın şablona geri döndürülememesini şart koşmalıdır.
- *Maliyet:* okuyucu dönüştürülmüş bir referansa karşı eşleştirme yapar, dolayısıyla hata oranı dönüşümün hata oranıdır ve genellikle doğrudan eşleştirmeden kötüdür; referans şablondan büyüktür.
- *İptal:* dördü içinde **en güçlüsüdür**, çünkü iptal doğasındandır: referans işe yaramaz olur.
- *Durum:* bu, ISO/IEC 24745'in zaten yönetiği ailedir; bu yüzden bir ISO iniş noktası için doğal evdir. Belirgin biçimde, **ISO/IEC 24745 "delegasyon" kelimesini hiç kullanmaz** — Bölüm 8'deki A–K modelleri yalnızca *referansın nerede saklandığını ve karşılaştırmanın nerede yapıldığını* tanımlar ve 2022'de eklenen üç model (I, J, K) **dağıtık karşılaştırma** modelleridir; karşılaştırma saklanan referansla aynı yerde değildir. Standart gereksini sağlar ve bu şartnamenin sorduğu soruda sessizdir.

**M3 — Geçici örnekle aracı aracılıklı eşleştirme (yalnızca Kip A).** Okuyucu canlı örneği araca iletir; araç onu şablonla eşleştirir ve imzalı kanıtı döner.

- *Gizlilik:* en zayıfı. Şablon cihazda kalır, ama canlı bir biyometrik örnek bir ağdan geçer ve bir sunucuya ulaşır.
- *Maliyet:* kurması en kolayı; yeni matematik yok.
- *İptal:* güçlü, çünkü aracı yanıt vermeyi reddedebilir.
- *Durum:* mevcut bileşenlerle bugün mümkün. Yedek yöntemdir ve yalnızca bunu izin veren bir şartname, önerinin gizlilik sözünü yalnızca kısmen yerine getirirdi.

**M4 — Dönüştürülmüş referans üzerinde sıfır bilgi eşleştirme.** M2 gibi, ama okuyucu ayrıca bir doğrulayıcıya **referansı ya da örneği açıklamadan** eşleşmenin gerçekleştiğini kanıtlar — kayıt anında taahhüt edilen referans ile örnek arasındaki sıfır bilgi yakınlık kanıtı.

- *Gizlilik:* dördü içinde en güçlüsü ve tek başına okuyucunun sonucu üçüncü bir tarafa göstermesine izin veren tek yöntem.
- *Maliyet:* ciddi. Eşleştirme işlevi üzerinde bir kanıt devresi gerektirir ve eşleştirme işlevi genellikle temiz bir aritmetik ilişki değildir; bu yüzden gerçek uygulamalar "bulanık" eşleştirmeyi kanıtın içinde ya da genel amaçlı bir sıfır bilgi makinesi kullanır.
- *İptal:* M2 ile aynı.
- *Durum:* W3C VC Confidence Methods taslağının `BiometricVectorConfidenceMethod` ve istemci tarafı sıfır bilgi eşleştirme akışıyla birlikte zaten izlediği yön. O taslak üzerine inşa etmek sıfırdan başlamaktan ucuzdur.

### 8.4 Değerlendirme ölçütleri

Adaylar arasındaki seçim **yayımlanmış ölçütlere karşı ölçümle belirlenmelidir** ve standart, o ölçüm var olana kadar tek bir zorunlu mekanizma adlandırmamalıdır.

| Ölçüt | M1 | M2 | M3 | M4 |
|---|---|---|---|---|
| P2 — okuyucuda açıklama yapmama | Güçlü | Dönüşüme bağlı | Zayıf (örnek aktarılır) | En güçlü |
| P4 — canlılık | Mekanizmadan bağımsız | Bağımsız | Bağımsız | Bağımsız |
| P5 — ölçülebilir yanlış kabul oranı | Ayarlanabilir | Dönüşüme bağlı | En iyi (doğrudan eşleştirme) | Dönüşüme bağlı |
| P6 — çevrimdışı yetenek | Var | Var | **Yok** | Var |
| İptal | Döndürme | **Doğal** | Güçlü | Doğal |
| Patent özgürlüğü | **Kısıtlı** | Serbest | Serbest | Serbest |
| Uygulama maliyeti | Yüksek | Orta | Düşük | Yüksek |
| Standartlaşma evi | Patentler | **ISO/IEC 24745** | Mevcut bileşenler | W3C VC |

### 8.5 Seçilen mekanizma ne olursa olsun gereksinim

8.5.1 Kip A'da delegasyonlu varlık denetimi okuyucunun meydan okumasına bağlanmalı ve sonuçta oluşan kanıt **tek kullanımlık** olmalıdır. Kip B'de denetim, aracının yanıtladığı bir meydan okumaya **bağlanmamalıdır**. Dönüştürülmüş referans **yetkiye özgü** olmalı ve yenileme, emekliye alma ya da süre dolana dek yeniden kullanılabilir olmalıdır; başarılı bir denetimin işletmecinin olay kaydı **tek kullanımlık** olmalıdır.
8.5.2 ISO/IEC 30107-3 Ek A'daki sunum saldırısı türlerine karşı değerlendirilmelidir ve her biri için **yanlış kabul oranı yayımlamalıdır**.
8.5.3 Okuyucunun özneyi yetkiler arasında tanımlayabileceği bir değer türetmesine ** izin vermemelidir**. Bir mekanizma bunu garanti edemiyorsa, yalnızca öznitelik açıklaması olmayan kiple sınırlandırılmalıdır.
8.5.4 Seçilen mekanizma için **uygunluk test paketi**, herhangi bir uygulama uygunluk iddiasında bulunmadan önce var olmalı ve tekrar oynatma, okuyucular arası ve amaçlar arası saldırı durumlarını içermelidir.

8.5.5 Seçilen mekanizma okuyucuya dönüştürülmüş bir referans yerleştiriyorsa, Bölüm 12.1.2'nin gereksinimleri **tam olarak** geçerli olmalı ve bu gereksinimleri karşılayamayan bir okuyucu o referansı **tutmamalıdır** — biyometrikten türetilmiş hiçbir değer tutmayan bir kipe geri dönmelidir.

### 8.6 Referans yenileme (Kip B)

Canlı bir yüz, iris ya da parmak izi değişir. Bir kez basılıp asla değiştirilmeyen bir Kip B referansı, kişiden ve telefonun kendisinin uyarlamaya devam ettiği şablondan sapacaktır. Yenileme, Kip B'nin doğrulama anında cihazı **aramadan** güncel cihaz kaynağıyla hizalı kalmasını sağlar.

8.6.1 Dönüştürülmüş bir referansın yenilenmesi **bir Kip B doğrulamasının parçası olarak yapılmamalıdır.** Başarmak için cihazı gerektiren bir denetim Kip A'dır.

8.6.2 Aracı, cihaz erişilebilirken güncel cihaz kaynağından yeni bir dönüştürülmüş referans türetebilmeli, imzalamalı ve güven hizmeti aracılığıyla yayımlamalıdır. Okuyucular, bunu durumu taşıyan aynı önbellek yenilemesinde (Bölüm 10.1.4) almalıdır.

8.6.3 Bir yenileme **en az** şunlarla tetiklenmelidir: Bölüm 10.5'teki sınıra yaklaşan geçen süre; cihaz kaynağının uyarlandığına dair bir platform sinyali; ve açık bir kullanıcı eylemi. Bir dağıtım, per-özne izleme kanalı oluşturmadığı sürece birleşik yanlış-reddet sinyallerini de kullanabilir.

8.6.4 Cihaz kaynağı **ya** (a) platformun kaydedilmiş şablondan şablonu dışa aktarmadan türetebildiği bir yenilenebilir referans, **ya da** (b) bu amaç için cihazda yakalanan ayrı bir DBA kaydı olmalıdır. Platform şablonu birlikte çalışabilir bir yenilenebilir referans veremiyorsa — güvenli-örtü Face ID ya da benzeri tipik bir kısıtı — aracı **(b)**'yi kullanmalı ve platform kilid şablonunu senkronize ettiğini **iddia etmemelidir.** Bu seçim O10'dur.

8.6.5 Cihazda **yeni bir görünüm**ü izleyen bir yenileme — ikinci kaydedilmiş yüz, maddeler düzeyinde farklı bir yakalama, kullanıcı başlattığı yeniden kayıt — özne için bir onay olayı olarak gösterilmelidir. Her okuyucuda sessizce referansı **değiştirmemelidir.** Cihaz kaybı Bölüm 10.4'te kalır.

8.6.6 Bir yenileme, o yetki için önceki referansı **değiştirmelidir.** Önceki referans yayımlanan durumda **emekliye alınmalıdır.** Okuyucular, Bölüm 10.3'ün sınırı içinde emekliye alınmış bir referansı iptal edilmiş bir yetki gibi ele almalıdır.

8.6.7 Bölüm 11.3.5 altındaki kriptografik yeniden düzenleme, biyometrik yenileme olarak **sayılmamalıdır.** Biyometrik bir yenileme, yetkinin kapsamını, amacını, öznelliklerini ya da ömrünü **genişletmemelidir.**

## 9. Sunum saldırısı tespiti

### 9.1 Denetim nerede yapılır

Denetim **okuyucu tarafından**, yakaladığı sunum üzerinde yapılmalıdır. Denetimin başarılı olmasının ön koşulu olarak işletmecinin sunucusuna devredilmemelidir, çünkü telefonu gerektiren bir sunucu tarafı denetim, varlık şartını yeniden getirirdi.

### 9.2 Gereksinimler

9.2.1 Okuyucunun sunum saldırısı tespiti ISO/IEC 30107-1, -2 ve -3'e uygun olmalı ve yakalama cihazı bir mobil cihazsa okuyucu ISO/IEC 30107-4'teki mobil cihaz profili altında test edilmelidir.

9.2.1a Okuyucunun tehdit modeli **üç saldırı ailesini** birbirinden ayırmalıdır, çünkü tek bir canlılık testi hepsini kapsamaz: **sunum saldırıları** (ISO/IEC 30107), **deepfake ve sentetik saldırılar** (ISO/IEC 26655) ve **dijital enjeksiyon saldırıları** (ISO/IEC 25456). Yalnızca birincisini karşılayan bir okuyucu bu sınırlamayı sertifikasyonunda **belirtmelidir** ve bir dağıtım, okuyucunun sunum saldırısı performansını diğer ikisine karşı kanıt olarak **göstermemelidir**.
9.2.2 Okuyucu, direnmeyi iddia ettiği her ISO/IEC 30107-3 Ek A saldırı türü için bir yanlış kabul oranı yayımlamalı, ISO/IEC 19795-1 altında ölçülmüş olmalıdır.
9.2.3 Okuyucu, hata oranlarındaki demografik değişimi ISO/IEC 19795-10 altında bildirmeli ve **herhangi bir demografik grupta nüfusa göre %25'ten daha kötü olmamalıdır** — NIST SP 800-63-4'ün uyguladığı eşik budur.
9.2.4 Okuyucu uzak, etkileşimsiz bir ortamda kullanılıyorsa, sunum saldırısı performansı NIST SP 800-63-4 §3.11'in uzak kullanım için koyduğu gereksinimleri karşılamalıdır.
9.2.5 Okuyucu sunumu **saklamamalıdır**. Yalnızca denetim süresince tutulmalı ve imha edilmelidir; uygulama bunu bir denetçiye gösterebilecek biçimde gerçeklemelidir.
9.2.6 Okuyucu sunumu denetim dışında **hiçbir amaçla kullanmamalıdır**, model eğitimi dâhil.

### 9.3 Bozulmuş çalışma

9.3.1 Uyumlu bir sunum saldırısı denetimi gerçekleştiremeyen bir okuyucu, delegasyon denetimi için **sertifikalandırılmamalıdır** ve okuyucunun bilindiği durumlarda yetki bunu belirtmelidir.
9.3.2 Okuyucu **sessizce daha zayıf bir denetime düşmemelidir**. Denetim gerçekleştirilemiyorsa doğrulama başarısız olmalıdır.

### 9.4 Saat toleransı

9.4.1 Okuyucu, geçerlilik penceresi kendi saatine göre tolerans içinde kalan bir kanıtı kabul etmelidir. Tolerans okuyucunun sertifikasyonunda yayımlanmalı ve **kanıtın kendi geçerlilik penceresini aşmamalıdır**.
9.4.2 Okuyucu saat eşitlemesini izlemeli ve saati toleransı aşan bir okuyucuyu bildirmelidir.

## 10. Durum, iptal ve süre dolma

### 10.1 Durum mekanizması

10.1.1 Güven hizmeti her yetkinin durumunu yayımlamalıdır: etkin, iptal edilmiş, süresi dolmuş ya da askıya alınmış.
10.1.2 Durum, öznenin cihazıyla temas hâlinde olmayan bir okuyucu tarafından alınabilmelidir.
10.1.3 Durum kaydı **kişisel veri içermemelidir**. Bir kişiyi değil bir yetkiyi tanımlamalı ve okuyucunun bir özne kimliği türetmesine ya da öznenin yetkilerini birbiriyle ilişkilendirmesine imkân vermemelidir.
10.1.4 Durum mekanizması, ara sıra bağlantılı bir okuyucuyu desteklemeli ve bunun için önbelleğe alınmış durum için azami yaş tanımlamalıdır.

### 10.2 İptal

10.2.1 Özne, bir yetkiyi istediği an, yetkinin oluşturulduğu yerden geri alabilmelidir.
10.2.2 İptal, her çevrimiçi okuyucuda bir sonraki denetimde etkili olmalıdır.
10.2.3 İptal **denetim kaydını silmemelidir**. İptal edilmiş bir yetki, var olduğunun kaydı olarak saklanmalı ve artık kullanılamaz olarak işaretlenmelidir.
10.2.4 Bir güven hizmeti, belgelenmiş bir gerekçe ile — örneğin şüpheli bir ihlal — işletmecinin talebi üzerine bir yetkiyi iptal edebilmeli ve gerekçe kategorisini yayımlamalıdır.
10.2.5 Bir güven hizmeti, araştırma sürerken bir yetkiyi askıya alabilmeli ve yayımlanan durumda askıya almayı iptalden ayırmalıdır.

### 10.3 Çevrimdışı iptal sınırı

10.3.1 Bir Kip B okuyucusu, önbelleğe alınmış durumu azami durum yaşından eskiyse kanıtı reddetmelidir.
10.3.2 Azami durum yaşı, standardın profili olarak yayımlanmalı ve **varsayımla değil ölçümle belirlenmelidir** — bu sınırı sağlayamayan bir dağıtım Kip A kullanmalıdır.
10.3.3 Kip B okuyucuları işleten bir işletmeci, şeffaflık açıklamasında fiilen uyguladığı azami durum yaşını bildirmeli ve hedefin üzerindeki bir duruma karşı doğrulanan her doğrulamayı raporlamalıdır.
10.3.4 Sınır, geri çekilmiş bir yetkinin hâlâ kapı açabildiği pencere **özneye tek cümleyle anlatılabilecek kadar kısa** olmalıdır. Değilse, dağıtım Kip B için yanlış biçimdedir.

### 10.4 Cihaz kaybı

10.4.1 Bir cihaz, cihaz anahtarının kaybını güven hizmetine bildirmeli ve güven hizmeti o anahtara bağlı bütün yetkileri iptal etmelidir.
10.4.2 Bir yetki **başka bir cihaza aktarılamamalıdır**. Özne yeni bir cihaza geçtiğinde yeni bir yetki oluşturulmalı ve eskisi iptal edilmelidir.

### 10.5 Çevrimdışı referans yaşı sınırı

10.5.1 Bir Kip B okuyucusu, önbellekteki dönüştürülmüş referans azami referans yaşından eskiyse denetimi reddetmelidir, yetki `not_after` içinde olsa ve durum taze olsa bile.
10.5.2 Azami referans yaşı, standardın profili olarak yayımlanmalı ve **varsayımla değil ölçümle belirlenmelidir** (O9) — bu sınıra referans yenileyemeyen bir dağıtım Kip A kullanmalı ya da **daha kısa yetkiler** vermeli.
10.5.3 Kip B okuyucuları işleten bir işletmeci, fiilen uyguladığı azami referans yaşını bildirmeli ve hedefin üzerindeki bir referansa karşı doğrulanan her doğrulamayı raporlamalıdır.
10.5.4 Sınır, telefona artık uymayan bir yüzün hâlâ kapı açabildiği pencere **özneye tek cümleyle anlatılabilecek kadar kısa** olmalıdır. Değilse, dağıtım Kip B için yanlış biçimdedir.

## 11. Kriptografik profil

### 11.1 Yetki ve kanıt imzalama

11.1.1 Yetki, güven hizmeti tarafından ya da güven hizmetinin kaydettiği bir anahtarla delegasyon aracısı tarafından, 11.3'teki profildeki bir imza algoritmasıyla imzalanmalıdır.
11.1.2 Bir Kip A kanıtı yetkinin kanıt anahtarıyla imzalanmalıdır.
11.1.3 Bir Kip A kanıt imzası okuyucu bağlılığını, amaç bağlılığını, meydan okumayı ve geçerlilik penceresini kapsamalıdır. Bu kapsamı içermeyen bir imzayı okuyucu reddetmelidir.
11.1.4 Bir Kip B dönüştürülmüş referans cihaz anahtarıyla imzalanmalıdır. Bu imza yetki tanımlayıcısını, okuyucu kapsamını, amacı ve `reference_issued_at`'i kapsamalıdır. Bir okuyucu meydan okumasını kapsamamalıdır.

### 11.2 Anahtar kurma

11.2.1 Okuyucu-aracı oturumu 11.3'teki profildeki karma anahtar değişimini kullanmalıdır.
11.2.2 Profil yürürlükte olduğu yerde okuyucu, yalnızca klasik anahtar değişimiyle korunan bir oturum kullanmamalıdır.

### 11.3 Post-kuantum profili

11.3.1 Yetki imzası bir **composite** imza olmalıdır: klasik bir bileşen ve bir ML-DSA bileşeni, tek bir açık anahtar ve tek bir imza olarak, tek bir algoritma tanımlayıcısı altında sunulmalıdır; böylece mevcut X.509, CMS ve COSE altyapıları onu tek bir atomik algoritma olarak ele alır.

11.3.2 Composite imza, iki bileşen de kontrol edilerek doğrulanmalıdır. Okuyucu, **yalnızca klasik bileşenin gücüne dayanarak bir yetkiyi kabul etmemelidir**.

11.3.3 Okuyucu-aracı oturumu, klasik bir bileşeni ML-KEM ile birleştiren bir karma anahtar değişimi kullanmalıdır.

11.3.4 Profil, bir composite imzanın güvenlik özelliğini not etmelidir: **varoluşsal** kurcalamaya dayanıklılık verir, **güçlü** kurcalamaya dayanıklılık vermez. Bu nedenle okuyucu, yeniden imzalanmış bir yetkiyi kullanım sayacı bakımından **yeni** bir yetki olarak ele almalı ve tekrar oynatma olarak saymamalıdır.

11.3.5 Bir yetkinin kriptografik ömrü **post-kuantum geçiş ufkunu aşmamalıdır** ve bir yetki, kapsamı değişmediği için özneden yeni onay alınmadan post-kuantum bir anahtar altında yeniden düzenlenebilmelidir.

11.3.6 Yetki geçmişleri ve denetim kayıtları için uzun vadeli arşiv biçimi **post-kuantum güvenli bir karma** belirtmelidir.

11.3.7 Bölüm 13'teki izlenemez kip kullanıldığında, uygulama kendi izlenemezlik temelinin post-kuantum güvenliğini belirtmeli ve **göstermediği bir post-kuantum izlenemezliği iddia etmemelidir**.

### 11.4 Rastgelelik

11.4.1 Meydan okumalar kriptografik olarak güvenli bir rastgele sayı üreteciyle üretilmeli ve en az 128 bit olmalıdır.
11.4.2 Aracı aynı meydan okumayı iki kez yanıtlamamalı ve bunu bir yeniden başlatma boyunca dayanacak kadar durum saklamalıdır.

## 12. Gizlilik, veri minimizasyonu ve denetim

### 12.1 Okuyucunun depolaması

12.1.1 Okuyucu **kaydedilmiş şablonu**, referans görüntüyü ya da korumasız bir öznitelik vektörünü **saklamamalıdır**.

12.1.2 **Seçilen mekanizma okuyucuya dönüştürülmüş bir referans yerleştiriyorsa** — Bölüm 8.3'teki M2 ya da M4 mekanizması — okuyucu bu referansı tutabilir ve aşağıdakilerin **tamamını** karşılamalıdır:

- a. ISO/IEC 24745 anlamında bir *yenilenebilir biyometrik referans* olmalı ve o standardın geri döndürülemezlik ile izlenemezlik gereksinimlerini karşılamalıdır;
- b. **tek bir yetkiye özgü** olmalıdır, böylece aynı özne için farklı okuyucularda tutulan referanslar birbirine bağlanamaz;
- c. onu tutan okuyucuya **bağlı** olmalıdır, böylece bir referans bir okuyucudan alınıp başka bir okuyucuda kullanılamaz;
- d. yetki yeniden düzenlendiğinde **değiştirilmelidir** ve yetki iptal edildiğinde **kullanılamaz hâle gelmelidir**;
- e. referansın tutucusu tarafından **kaydedilmiş şablona dönüştürülebilir olmamalıdır**;
- f. `reference_issued_at` taşımalı ve Bölüm 10.5'teki sınırı aştığında **kullanılamaz olmalıdır**.

12.1.3 Okuyucu, öznenin biyometrik sunumundan türetilen kararlı bir tanımlayıcıyı **saklamamalıdır** ve okuyucular arasında kararlı bir özne tanımlayıcısı **saklamamalıdır**.
12.1.4 Bir Kip A okuyucu, tekrar oynatma koruması için kabul ettiği her kanıtın tanımlayıcısını o kanıtın geçerlilik penceresi boyunca saklamalı ve sonra **silmeli**dir. Bir Kip B okuyucu, kaydettiği her başarılı denetimin tanımlayıcısını durum yaşı sınırından uzun olmayan bir pencere boyunca saklamalı ve sonra **silmeli**dir.
12.1.5 Okuyucu, öznenin hareketlerini yeniden kurmaya yarayabilecek bir günlük **saklamamalıdır**. Bir okuyucu olay günlüğü tutmak zorundaysa, bu günlük okuyucu başına ve dönem başına takma adla tutulmalı ve başka bir okuyucunun ya da güven hizmetinin kayıtlarıyla birleştirilememelidir.

### 12.2 İşletmecinin kaydı

12.2.1 İşletmeci olayı kaydetmelidir — bu okuyucuda, bu zamanda, bu yetki altında bir delegasyonlu denetim başarılı oldu — ve **biyometriği kaydetmemelidir**.
12.2.2 Amacın kimlik gerektirdiği yerde işletmeci, yetkiden gelen kapsamlı kimlik referansını kaydetmeli ve yetkinin diğer alanlarını operasyonel ihtiyacın ötesinde tutmamalıdır.
12.2.3 İşletmeci, kendi günlüğünü bir özne için okuyucular arasında birleştirerek bir özne profili kurmamalıdır. Yasal bir amaç boyuna göre kayıtlar gerektiğinde, bu amaç yetkide beyan edilmiş olmalı ve yetki bunu belirtmelidir.

### 12.3 Denetim

12.3.1 Bağımsız bir denetçi, bir okuyucunun ya da işletmecinin kayıtlarından şablon ya da yeniden kullanılabilir tanımlayıcı saklanmadığını, yetkilerin iddia ettikleri gibi kapsandığını, yetkilerin süresinin dolduğunu, Kip B okuyucularının durum yaşı sınırına ve referans yaşı sınırına uyduğunu ve günlüklerin birleştirilmediğini doğrulayabilmelidir.
12.3.2 Güven hizmeti toplu şeffaflık rakamları yayımlamalıdır: yetki, iptal, durum sorgusu ve doğrulama sonucu sayıları; kişisel veri olmadan.
12.3.3 Özne, kendi yetkileri altında gerçekleştirilen her denetimin kaydını, özneye çözümlenen takma adlı özne referansıyla alabilmeli ve bir kayda itiraz edebilmelidir.

## 13. Gizlilik kipleri

### 13.1 Üç kip

13.1.1 Bir yetki, amaca göre seçilmiş olarak şu üç kipten **tam olarak birinde** düzenlenmelidir:

| Kip | Okuyucu ne öğrenir | Nerede kullanılır |
|---|---|---|
| **Açıklama yok** | Yalnızca yetkili bir kişinin hazır olduğu | Kimliğin gerekmediği erişim |
| **Öznitelik** | Arkasındaki kimlik olmadan belirtilmiş bir olgu — örneğin bir yaş bandı | Yaş doğrulama |
| **Kimlik** | Kapsamlı bir kimlik referansı | Otel check-in, klinik girişi |

13.1.2 Kip yetkide kaydedilmeli ve **özneye görünür olmalıdır**.

### 13.2 İzlenemezlik

13.2.1 Açıklama olmayan kipte, okuyucu iki denetimi ilişkilendirememelidir. Uygulama bu özelliği sağlayan bir yapı kullanmalıdır — Privacy Pass ihra protokolleri anlamında bir izlenemez belirteç ya da aynı özelliğe sahip, yetki başına rastgeleleştirilmiş bir referans.
13.2.2 Okuyucu denetimleri hiçbir yolla ilişkilendirmeye çalışmamalı ve Bölüm 12.3'teki denetim böyle bir girişimi aramalıdır.

### 13.3 Öznitelik minimality

13.3.1 Öznitelik kipinde okuyucu özniteliği almalı ve **kimliği, doğum tarihini ya da kimlik türetilebilecek bir değeri almamalıdır**.
13.3.2 Bir öznitelik, nokta değerin yeniden kimliklendirmeye yol açabileceği yerde **bant** olmalıdır.

## 14. Uygunluk

### 14.1 Uygunluk sınıfları

| Sınıf | Gerektirdikleri |
|---|---|
| **Çekirdek** | Bölüm 5 ve 10'a göre yetki oluşturma ve iptal; Bölüm 6'ya göre kanıt biçimi ve bağlılık kuralları; durum alma; tek kullanımlık uygulaması; başarılı/başarısız doğrulama anlamı |
| **Çekirdek + PAD** | Çekirdek, artı Bölüm 9'a göre sertifikalı sunum saldırısı denetimi ve yayımlanmış oranlar |
| **Çekirdek + PAD + Kip A** | Çekirdek + PAD, artı Bölüm 7.1'deki çevrimiçi kip |
| **Çekirdek + PAD + Kip B** | Çekirdek + PAD, artı durum yaşı ve referans yaşı sınırları uygulanmış ve raporlanmış Bölüm 7.2'deki çevrimdışı kip |
| **Tam** | Çekirdek + PAD, artı Kip A veya Kip B'den en az biri, artı Bölüm 13'teki üç gizlilik kipi, artı Bölüm 11.3'teki post-kuantum profili, artı Bölüm 12.3'teki denetime katılım |

### 14.2 Uygunluğun nasıl değerlendirileceği

14.2.1 Bir **okuyucu**, Bölüm 9, 10.3, 10.5 ve 12.1'in okuyucu gereksinimlerine karşı, Kip A iddia ediyorsa Bölüm 6, Kip B iddia ediyorsa Bölüm 7.2 ile birlikte, Bölüm 14.3'teki test paketiyle, akredite bir kuruluş tarafından sertifikalandırılır.
14.2.2 Bir **delegasyon aracısı**, Bölüm 5, 7, 10 ve 11'e karşı güven hizmeti tarafından sertifikalandırılır.
14.2.3 Bir **işletmeci**, Bölüm 12.3'teki denetimle değerlendirilir ve sonucu yayımlamalıdır.
14.2.4 Bir **güven hizmeti**, tarafsızlığı, durum mekanizmasının doğruluğu ve Bölüm 10.1.3'teki açıklama yapmama gereksinimleri bakımından değerlendirilir.

### 14.3 Uygunluk test paketi

Paket en az şunları içermelidir:

| Test | Beklenen sonuç |
|---|---|
| Sınır okuyucuda ve amaçta geçerli bir kanıt | Kabul edilir |
| Aynı kanıt başka bir okuyucuda | Reddedilir — okuyucu bağlılığı |
| Aynı kanıt başka bir amaç için | Reddedilir — amaç bağlılığı |
| Aynı kanıt bayat bir meydan okumayla | Reddedilir — tazelik |
| Aynı kanıt iki kez sunulur | Reddedilir — tek kullanım |
| İptal edilmiş bir yetkiyle kanıt, çevrimiçi okuyucuda | Reddedilir |
| İptal edilmiş bir yetkiyle kanıt, çevrimdışı okuyucuda, durum yaşı aşılmış | Reddedilir — kapalı çalışma |
| Kip B eşleşmesi, Bölüm 10.5'ten eski bir referansla | Reddedilir — kapalı çalışma |
| Kip B denetimi, başarmak için cihaza başvurur | Reddedilir — Kip B değil |
| Telefon kilidini okuyucu sunumunu eşleştirmeden açma (Kip A) | Reddedilir — 7.1.3 |
| Yalnızca klasik bileşenle imzalanmış bir kanıt | Reddedilir — 11.3.2 |
| Yeniden imzalanmış bir yetkiyle kanıt | Kabul edilir, ancak **yeni** bir kullanım olarak sayılır — 11.3.4 |
| ISO/IEC 30107-3 Ek A'daki her saldırı türüyle sunum | Yayımlanmış orandan daha iyi ya da eşit olacak şekilde reddedilir |
| Şablon ya da kararlı tanımlayıcı saklayan okuyucu | Sertifikasyon hatası |
| Günlükleri okuyucular arasında birleştiren işletmeci | Sertifikasyon hatası |
| Adını belirtmediği bir amaç için kullanılan yetki | Reddedilir — Bölüm 5.2.2 |

## 15. Açık kalemler

Aşağıdakiler, bu şartnamenin nihai olarak önerilebilmesi için çözülmelidir. Her biri burada açıkça belirtilmiştir, çünkü açık sorularını saklayan bir şartname, gözden geçirmede reddedilecek bir şartnamedir.

| # | Açık kalem | Nasıl çözülmeli |
|---|---|---|
| O1 | **M1, M2 ya da M4'ten hangisi Kip B referansını kurar** | Kip B'nin *biçimi* Bölüm 7.2'de kesindir. M3 yalnızca Kip A'dır. Bölüm 8.4'e karşı, yayımlanmış FAR/FRR ile açık bir değerlendirme. M1 patent kısıtlıdır; M2 patentle serbest ISO/IEC 24745 yuvasıdır; M4 en güçlü gizlilik varyantıdır. Seçim bir araştırma sonucudur, bir yazım kararı değil. |
| O2 | **Kip B için azami durum yaşı** | Gerçek dağıtımlarda ölçüm. Değer yazımla belirlenemez. |
| O3 | **Bir composite imzanın her hedef kimlik belgesi biçiminin gereksinimlerini karşılayıp karşılamadığı** | Tek-algoritmalı composite'in temel olduğu varsayımıyla, X.509, CMS ve COSE karşısında uygulama testi. |
| O4 | **Amaç ve öznitelik sicili** | Bir teknik değil yönetişim sorusudur: bir sicilin bir bakımı, bir kabul kuralı ve bir değişim süreci gerekir. |
| O5 | **ISO/IEC 30108 ile ilişki** | ISO/IEC 30108-1:2026, kimlik doğrulama protokolü entegrasyonunu kapsamından çıkarıyor, dolayısıyla seri tamamlayıcıdır. Bir Part 3 doğru ev mi yoksa ayrı bir belge mi, SC 37'nin sorusudur. |
| O6 | **Delegasyonlu varlık denetiminin mevcut bir kimlik kanıtıyla etkileşimi** | Delegasyonlu denetim bir belge denetimiyle birleştirildiğinde hangisinin esas olduğu ve bir uyuşmazlığın nasıl ele alınacağı. |
| O7 | **Öznenin yaşının hangi eşikte beyan edileceği** | Öznitelik kipinin bir öznitelik kaynağına ve onu kim beyan edebilir kuralına ihtiyacı var. |
| O8 | **Çevrimdışı Kip B'nin hiç istenip istenmeyeceği** | Gerçek bir iptal sınırı ve referans yaşı sınırı koyar, her ikisi de gerçek güvenlik maliyetidir. O2 ya da O9'daki ölçüm bir sınırın yeterince küçük tutulamayacağını gösterirse, Kip B zayıf bir sınırla sürdürülmek yerine standarttan tamamen çıkarılabilir. |
| O9 | **Kip B için azami referans yaşı** | Cihaz kaynağına ve canlı okuyucu yakalamalarına karşı biyometrik sapmanın ölçümü. O2'nin kardeşi; değer yazımla belirlenemez. |
| O10 | **Bir platform kilid şablonu birlikte çalışabilir bir yenilenebilir referans verebiliyor mu** | Cihaz üreticileriyle uygulama testi. Veremiyorsa, Kip B cihazda ayrı bir DBA kaydı kullanır (Bölüm 8.6.4), Face ID veya benzeri senkronizasyonu değil. |

## Ek A (bilgilendirici) — Örnek uygulama

Bir sakin, apartmanının geçidine kalıcı bir erişim delegasyonu verir; açıklama yapılmayan kipte, bir yıl geçerli.

1. Sakinin bina uygulaması, işletim sisteminden bir yetki oluşturmasını ister. İşletim sistemi şunu gösterir: *Y yapısı — zemin kat ve konut geçitlerinin tümü, erişim, kimlik açıklanmaz, bir yıl, kalıcı.* Sakin onaylar.
2. İşletim sistemi `grant_id` G'yi, `reader_scope`i Y yapısının okuyucu sınıfını, `purpose`ı erişimi, `attributes`ı boşluğu, `verification_mode`ı Kip B'yi, `not_after`ı bir yılı oluşturur, güncel cihaz kaynağından bir dönüştürülmüş referans türetir ve G ile referansı güven hizmetine kaydeder. Güven hizmeti G'yi etkin olarak yayımlar.
3. Sakin bir geçide yaklaşır. Geçit bir sunum yakalar, sertifikalı sunum saldırısı denetimini çalıştırır ve sunumu önbellekteki referansla eşleştirir. Telefon aranmaz.
4. Geçit, önbellekteki durumun Bölüm 10.3 içinde ve önbellekteki referansın Bölüm 10.5 içinde olduğunu, G'yi etkin görür ve kilidi açar.
5. Geçit şunu kaydeder: *bu geçitte, bu zamanda, G yetkisi altında bir delegasyonlu erişim denetimi başarılı oldu.* Kim olduğunu kaydetmez. Sakin olayı kendi yetki listesinde görür ve istediği an G'yi geri alabilir; okuyucular durumu gördükten sonra bir sonraki denetim başarısız olur.
6. Yetki ayakta kaldığı sürece, telefon dönüştürülmüş referansı doğrulama yolunun dışında yeniler (Bölüm 8.6), böylece geçitler telefonun o an tanıdığı kişiyle eşleşmeye devam eder. Telefonda yeni bir görünüm, geçitler onu almazdan önce sakine gösterilir.

## Ek B (bilgilendirici) — Mevcut standartlarla eşleşmesi

| Bu şartname | Üzerine inşa edilen mevcut çalışma |
|---|---|
| Bölüm 5, yetki nesnesi | W3C Doğrulanabilir Kimlik Belgeleri 2.0 kimlik belgesi yapısı; VC Confidence Methods taslağının `confidenceMethod` ve `assuranceLevel` sözlüğü |
| Bölüm 6, kanıt | VC Data Integrity; `validFrom`/`validUntil` alanları; yetkinin durumu için bir durum listesi |
| Bölüm 7, Kip A | W3C VC sunum akışları; OpenID4VP 1.0 |
| Bölüm 7, Kip B | ISO/IEC 24745:2022 dağıtık karşılaştırma (I/J/K modelleri) ve yenilenebilir biyometrik referanslar; ISO/IEC 18013-5 yalnızca çevrimdışı *yetki* için (yetki zinciri ve durum), varlık için değil |
| Bölüm 8, M2 ve M4 | ISO/IEC 24745:2022; özellikle dağıtık karşılaştırma uygulama modelleri ve yenilenebilir biyometrik referans çerçevesi; VC Confidence Methods taslağının `BiometricVectorConfidenceMethod` bölümü |
| Bölüm 9 | ISO/IEC 30107-1/-2/-3/-4; NIST SP 800-63-4 §3.11 |
| Bölüm 10 | FIDO Metadata Service durum modeli; VC Bitstring Status List 1.0; RFC 5280 iptal mekanizmaları |
| Bölüm 13 | IETF Privacy Pass, RFC 9576/9577/9578; izlenemez kip için |
| Bölüm 11.3 | NIST FIPS 203/204/205; LAMPS composite imza şartnamesi; RFC 9954 ve RFC 10024'teki karma anahtar değişimi |
| Bölüm 5.1, `attestation` | FIDO cihaz attestation'ı; platform cihaz-attestation hizmetleri |

## Ek C (bilgilendirici) — Bu şartnamenin yapmadıkları

- Biyometrik doğrulamayı kabul edilebilir kılmaz. Yalnızca belirli bir gizliliği koruyan *biçimini* şartnamelendirilebilir kılar. Verilen bir dağıtımın biyometriği kullanıp kullanmayacağı işletmecinin kararıdır ve birçok yargı bölgesinde bir veri koruma değerlendirmesidir.
- Okuyucunun bir FIDO güvenen tarafı, bir web origin'i ya da var olan bir ekosistemin parçası olmasını gerektirmez.
- Delegasyonlu varlık için bir pazar yaratmaz. M1'deki patent konumu, Bölüm 9'daki sertifikasyon maliyeti ve Bölüm 14.2'deki güven hizmeti yönetişimi maliyetlerdir ve iş modeli şartnameden ayrı bir konudur.
- Delegasyonlu varlık denetiminin zaten patentli bir mekanizmayla çözülüp çözülmediğini ortaya koymaz. Bu, kalan O1 araştırma sorusudur (M1 karşısında M2 karşısında M4) ve en önemli olanıdır. Kip B'nin biçimi — doğrulamada okuyucunun önceden teslim edilmiş bir referansa karşı eşleştirmesi, doğrulama yolunun dışında yenileme — o soru değildir.
