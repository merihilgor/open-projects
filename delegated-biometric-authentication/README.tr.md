English: [README.md](README.md)

# Delegeli Biyometrik Kimlik Doğrulama: Kasada ve Binada Telefonsuz, Biyometriğinizi Paylaşmadan Yüzle Ödeme ve Giriş

## Özet

Biyometrik kimlik doğrulama pratiktir, ama insanlar yüzlerini her kuruluşla paylaşmak istemez. Biyometrik şablon kalıcı bir kimliktir: şifre gibi sızdıktan sonra değiştirilemez ve yüz veritabanı, verisini toplayan herkes için hedefe dönüşür. Bu yüzden insanlar reddeder, ya da bir forma yüz fotoğrafı verip kimin nereye kopyaladığını hiç öğrenmez.

Ama insanlar cep telefonlarındaki biyometrik sistemine zaten güvenir. Telefonun kilidi, her gün açtıkları uygulama, yaptıkları ödeme budur. Bu güvenilir sistemler **delegeli biyometrik kimlik doğrulama** sunarsa — yüz kontrolünü belirli bir okuyucuya, belirli bir amaçla, sınırlı bir süreyle, istendiği anda geri alınabilir biçimde devretmek — o zaman:

- **Bir kişi kasada yüzle, telefonsuz ve kartsız ödeme yapar.** Telefon cebinde kalır. Kasa yüz yüzünden doğrular ve ödeme tamamlanır.
- **Bir kişi kartsız, anahtarsız ve telefonsuz kendi binasına girer.** Sakin uygulaması yüzünü binanın geçit cihazına devretmiştir; bina biyometrik şablonu hiç almaz, saklamaz, görmez bile.
- **Okuyucu biyometriği hiçbir zaman almaz.** Telefon şablonu güvenli donanımında tutar, okuyucu bunun yerine kısa ömürlü imzalı bir kanıtı doğrular ve o kanıt başka hiçbir yerde işe yaramaz.

**Kısacası: telefonunuzun biyometrik sistemi, cepte kalırken sizi bir kasada veya bir kapıda destekleyebilir ve hiçbir kuruluş biyometrik verinizi almaz.**

Öneri, insanların zaten bildiği bir uygulama başına açma hareketini — bir uygulamaya basılı tutup **Face ID Zorunlu** seçeneğini açmak — okuyuculara, yani hiçbir zaman telefonla entegre olmamış bir satış terminaline, bir apartman geçidine, bir turnikeye **hizmet başına delegasyon** haline getirir ve bu işi işletim sisteminin sizin için yapmasını sağlar.

## Sorun

- **Biyometrik veri kalıcıdır, şifre değil.** Sızan bir şifre değiştirilebilir; sızan bir yüz şablonu değiştirilemez. Kullanıcı, bir ihlalden, bir içeriden, bir mahkeme kararından veya bir sahiplik değişikliğinden sonsuza kadar korunacağına güvenmek zorundadır.
- **Güven zaten başka yere konmuştur.** İnsanlar dijital hayatlarının tamamıyla telefonun Face ID'sine veya parmak izine güvenir. Bu güven bankanın uygulamasına, spor salonunun uygulamasına veya binanın uygulamasına taşınmaz; çünkü her biri biyometrik veriyi ayrı ayrı işler.
- **Her yeni tutucu yeni bir ihlal yüzeyidir.** Yüz kabul eden bir kafe, fotoğraf saklayan bir sigortacı, parmak izi kaydı tutan bir ev sahibi: her biri kimsenin tutmasını istemediği bir veritabanına dönüşür.
- **Bugün "yüzünü kullan" demek yüzünü vermek demektir.** Mevcut biyometrik ödeme ve erişim sistemleri ya şablonu ya referans görüntüyü sunucuya yükler, ya da kullanıcıyı onu saklayan bir hizmete kaydeder. Kullanıcı farkı göremez ve önleyemez.
- **Kartsız olmak hâlâ bir şey taşımaktır.** Temassız kartlar, telefon dokunuşları ve QR kodları çalışır, ama hepsi elde veya cebde bir belirteç ister ve hepsi aynı şekilde başarısız olur: paylaşılan kart elden dolaşır, telefon kasada şarjı bitmiş çıkar, ekran kırıktır.
- **Telefonsuz durumda erişim zayıftır.** Telefonunu evde, çantasında ya da misafirinde bırakan bir kişinin içeri girmesi gerekir. Bugün bu anahtar, kod, anahtarlık ya da resepsiyon demektir.
- **Delegasyon bugünkü cihazlar arası akışlarla çözülmüş değildir.** Mevcut cihazlar arası girişler hâlâ kayıtlı cihazın hazır bulunmasını, doğrulamayı onun yapmasını ve odada olmasını gerektirir. Üçüncü taraf bir okuyucuya **biyometrik kontrolü** devretmek başka bir problemdir.

## Önerilen Çözüm

1. **Telefon güvenilir aracı olur.** Biyometrik şablonu bir şirket değil, işletim sistemi güvenli donanımın içinde tutar. Telefon, "bu kişi, size delegasyon veren kişidir" diyen taraftır.
2. **Delegasyon kullanıcı tarafından, okuyucu başına, telefondan verilir.** Kullanıcı yüzünü devredebilecek hizmetleri seçer — bir ödeme kuruluşu, kendi binası, bir ulaşım işletmesi — tıpkı bir uygulamada "Face ID Zorunlu" seçeneğini açtığı gibi. İşletim sistemi işi yapar; böylece okuyucunun üreticisinin telefonla hiç entegre olmasına gerek kalmaz.
3. **Her yetki kapsamlı, süreli, geri alınabilir ve denetlenebilirdir.** Yetki okuyucuyu, amacı, sınırı ve bitiş tarihini belirtir. Kullanıcı her yetkiyi, her kullanımı ve her iptali görebilir ve istediği an geri alabilir; o okuyucudaki bir sonraki doğrulama hemen başarısız olur.
4. **Okuyucu bir kanıtı doğrular, asla şablonu değil.** Okuyucu, devredilmiş bir biyometrik kontrolün başarılı olduğunu bildiren, o okuyucuya ve o işleme bağlı, kısa ömürlü imzalı bir beyan alır. Bu beyan başka bir okuyucuda tekrar kullanılamaz, kopyalanamaz ve hiçbir biyometrik veri taşımaz.
5. **Biyometrik kontrol kullanıcının olduğu yerde yapılır.** Okuyucu, kullanıcının yüzüne bakarak canlılık denetimiyle kontrolü yapar; bir fotoğraf ya da maske kabul edilmez. Şablon telefonda kalır; okuyucu bir meydan okuma gönderir, telefon onu yanıtlar, okuyucu yanıtı doğrular.
6. **Okuyucu, güven çapasının tek sahibi değildir.** Yetki bir güven hizmetine kaydedilir; böylece okuyucu, telefon çevrimiçi olmasa da kullanıcı da uygulama yüklemese de, bir delegasyonun gerçek ve hâlâ geçerli olduğunu doğrulayabilir.
7. **Her kullanım durumunun kimliği tanımlaması gerekmez.** Okuyucu yalnızca "yetkili bir kullanıcı" bilmesi gerektiğinde yetki, okuyucunun dün ve bugün aynı kişiyle gelip gelmediğini anlayamayacağı **izlenemez, tek kullanımlık bir belirteç** olarak düzenlenebilir. Okuyucunun kimi olduğunu bilmesi gerektiğinde ise yetki, okuyucunun başka hiçbir şeyi öğrenemeden doğrulayabileceği kapsamlı bir kimlik bilgisi taşır.
8. **Kullanıcıya hiçbir şey zorunlu olarak dayatılmaz.** Delegasyon her zaman isteğe bağlıdır, her zaman görünürdür ve her zaman geri alınabilirdir. Merkezî bir biyometrik veritabanı yoktur, çünkü saklanacak merkezî bir biyometrik de yoktur. Hiçbir şeyi devretmeyen kullanıcı bugünkü davranışını aynen sürdürür.
9. **Vendor silosu değil, tek açık standart.** Aynı yetki, kanıt, doğrulama ve iptal kuralları her işletim sistemi, her okuyucu ve her işletmeci için geçerlidir; böylece bir işletmeci her üretici için akışı baştan kurmaz.

## Nasıl Çalışır

![Delegeli biyometrik kimlik doğrulama: kullanıcı telefondan bir satış terminaline ve bir apartman geçidine yüz delegasyonu verir; sonra her bir okuyucunun önüne telefonsuz ve kartsız gelir; okuyucu canlılık denetimiyle yüzünü canlı kontrol eder, bir meydan okuma gönderir, telefon güvenli donanımından yanıtlar ve okuyucu kısa ömürlü imzalı kanıtı doğrular; biyometrik şablon telefondan hiç çıkmaz ve bina hiçbir biyometrik veri almaz](assets/delegated-biometric-authentication.tr.svg)

| Durum | Bugün | Bu öneriyle |
|---|---|---|
| Kasada ödeme | Telefon dokunuşu, kart veya bir hizmete yüklenen yüz | Okuyucuya bakarsınız; telefon cepte kalır ve ödeme tamamlanır |
| Apartman binasına girmek | Anahtar, kod, kart, anahtarlık ya da hâlâ yerinde ve şarjlı olması gereken telefon | Geçide bakarsınız; kart yok, anahtar yok, telefon yok |
| Bina veya banka biyometriği ister | Şablon, fotoğraf ya da imza verilir ve saklanır | Hiçbir şey verilmez; yalnızca kapsamlı, süresi biten bir delegasyon kanıtı |
| Kullanıcı fikrini değiştirir | Veri zaten kuruluşun elinde | Yetki geri alınır; o okuyucudaki sonraki kontrol başarısız olur |
| Aynı kişi her gün aynı okuyucuyu kullanır | İşletmeci o kişiyi görünür biçimde tanımlar | Kimliğin gerekmediği yerde izlenemez belirteç |
| Okuyucunun üreticisi biyometrik entegrasyonu ister | Her telefon üreticisiyle ayrı entegrasyon | Tek standart; işletim sistemi işi yapar |
| Biri fotoğrafla denemeye çalışır | Genellikle yalnızca üreticinin kendi güven eşiği | Okuyucuda uluslararası standarda uygun sunum saldırısı tespiti |
| Telefon çalınır | Hırsızın kullanıcının yüzü olabilir | Telefon, delegasyon için gereken aynı biyometrikle kilitlidir; yetkiler görünür ve geri alınabilir |
| Yeni bir okuyucu kurulur | Her telefon ekosistemiyle entegrasyon | Okuyucu standardı kabul eder ve güven hizmetine katılır |

## Ayrıntılı Kullanım Durumları

Bütün katalog — on dokuz durum, her biri **neyi çözmediğiyle** birlikte — **[USE-CASES.tr.md](USE-CASES.tr.md)** dosyasındadır ([English](USE-CASES.md)). Durumlar, okuyucunun dört türüne göre gruplanır:

| Grup | Okuyucu | Durumlar |
|---|---|---|
| **Ödeme** | Bir kasa | Nakitsiz, kartsız ve telefonsuz satış noktası ödemesi; küçük tutarlı ödemeler ve yükleme; toplu taşıma; satış noktasında yaş doğrulama |
| **Giriş** | Bir geçit | Bina ve daire girişi; misafir ve taşeron erişimi; kısa süreli kiralama; otel check-in ve anahtarsız odalar; işyeri erişimi ve devam |
| **Kanıtlama** | Bir masa ya da kiosk | Hastane ve klinikte kimliklendirme; sivil kimlik ve oy verme; yaş ve yetenek kontrolleri; sigorta ve garanti için varlık kanıtı |
| **Çalışma ve seyahat** | Herhangi bir yer | Okul ve kampüs devamı; pasaportsuz sınır geçişi; ortak sistemi olmayan yargı bölgeleri arasında geçiş; hava yolu ve demiryolu kapuşturma; paylaşımlı ve geçici çalışma alanları; acil durum ve sosyal yardım erişimi ile okuyucu kullanamayan insanlar için erişim |

Bunlar boyunca üç örüntü tutuyor ve katalogu kısaca okumak yerine okumanın nedeni bu:

- **Gizlilik kazancı, işletmecinin ihtiyacı en küçük olduğu yerde en büyüktür.** Ulaşım turnikeleri ve paylaşımlı çalışma alanları "yetkili" bilgisine ihtiyaç duyar, "kim" bilgisine değil. Otel ve klinik resepsiyonu tersini ister ve daha az kazanır.
- **Hukuki zorluk, teknik zorluğun kabaca tersidir.** Bina erişimi teknik olarak kolay ve hukuken tartışmalıdır; oy kontrolü teknik olarak kolay ve hukuken neredeyse imkânsızdır; sınır kontrolü modelle bütünüyle uyumsuzdur.
- **En çok fayda sağlayacak olan durumlar, en zor yedekleri olan durumlardır.** Katalogdaki her madde, modelin kapsamadığı bir durumu adlandırır. Yedeği kaldıran bir dağıtım, yedeği gerekli kılan şeyi de kaldırmış olur.

Modelin en keskin sınavı olan iki durumu burada adlandırmak gerekir, çünkü ikisi de zıt yönlerde çekiyor. **Pasaportsuz sınır geçişi** — "seamless borders" fikri, bir yolcunun belge yerine canlı bir kontrolle sınırdan geçmesi — önerinin bütünü için en doğal uyumdur: yolcunun cihazı soruya yanıt verebiliyorken bir sınır otoritesinin şablon tutmasına hiçbir sebep yoktur, ve yolcu telefonu elinde olmadan geçtiği için telefonun yokluğu özelliği en çok tam da orada önemlidir. Aynı zamanda riski en yüksek ve hata payı en az olan durumdur. **Oy kontrolü** ise teknik olarak kümenin en kolay ve hukuken neredeyse imkânsızdır: delegasyonlu bir denetim *bir* kişinin yetkili olduğunu ortaya koyar, belirli bir kişinin seçmen listesinde olduğunu değil; bu ayrımı bulanıklaştıran bir düzen, iki kez oy kullanmaya ya da yanlış kişi tarafından kullanmaya açık bir düzen olurdu. Önerinin her ikisi üzerindeki dürüst konumu, üstüne örtmek yerine yazılarak ortaya konmuştur.

## Onay ve Kayıt: "Face ID Zorunlu" Analojisi

İnsanlar, işletim sisteminden bir uygulama açılmadan önce biyometrik talep etmesini istemeyi zaten biliyorlar: simgeye basılı tutup **Face ID Zorunlu** seçeneğini açmak ve işletim sisteminin gerisini halletmesi. Kimsenin bir üreticinin kendi biyometrik koduna güvenmesi gerekmez ve kimse şablon vermez.

Apple'ın ne kurduğunu tam olarak söylemek gerekir, çünkü fark önemli: iOS 18'den itibaren **Face ID Zorunlu**, Ana ekranda uygulamaya basılı tutulduğunda çıkan bir eylemdir ve *o uygulamayı kullanıcının kendi cihazında* kapatır — açılışta yeniden kimlik doğrulama ister ve uygulamanın içeriğini Spotlight, bildirimler ve Siri önerilerinden gizler. Hiçbir üçüncü tarafa yüz kontrolü devretmez ve okuyucu başına değildir. Cihaz yerel, uygulama başına bir kapıdır.

Bu öneri o hareketi uygulamalardan **okuyuculara** taşır ve işi yapan taraf işletim sistemidir:

- **Kullanıcı verir, işletim sistemi uygular.** Yetki işletim sisteminin güvenlik alt sisteminde durur, bir uygulamada değil. Bir uygulama isteyebilir, kendi başına veremez.
- **Yetki okuyucuyu adıyla belirtir.** "Genel olarak ödeme uygulamaları" değil, "bu ödeme işletmesi, bu satıcıda" ya da "bu binanın geçit cihazı". Bir okuyucuya verilen yetki başka bir okuyucuda çalışmaz.
- **Yetki amacı adıyla belirtir.** Ödemeye delegasyon girişe delegasyon değildir; yaş kanıtlamaya delegasyon da değildir. Bir amaç için verilen yetki başka bir amaç için kullanılamaz.
- **Yetki süresi bitir.** Bir gün, bir ay, bir yıl ya da kullanıcı geri alana kadar verilebilir. Kullanıcının göremediği uzun ömürlü bir yetki, daha küçük bir kılıkta olsa da kalıcı biyometrik teslimiyle aynı sorundur.
- **Kullanıcı her şeyi görebilir.** İşletim sisteminin zaten izinleri gösterdiği yerde her delegasyon, her okuyucu, her kullanım ve her iptal listelenir. Geri alma, sonraki kontrolde etkili olur.
- **İşletim sistemi saçmalıkları reddeder.** Bir yetkiyi kötü niyetli bir uygulama alamaz, kimlik avına konu olamaz ve telefonu alan kişiye devredilemez. Çalınan telefon kapalı kalır: sahibinin kendi biyometriği olmadan hiçbir delegasyon kullanılamaz.

"Telefon yok ama sahtelenemez"in çalışmasını sağlayan budur. Okuyucu yalnızca kişinin bizzat verdiği bir delegasyonu onurlandırır ve kişi onu istediği anda geri alabilir.

## Merkezî Bal Küpü Olmadan Güvenilir Aracılar

Model bilinçli olarak **merkezî bir biyometrik veritabanı içermez**, çünkü değerli olan varlık zaten telefondan hiç çıkmaz:

| Taraf | Ne tutar | Ne tutmaz |
|---|---|---|
| Kullanıcının telefonu | Biyometrik şablon, güvenli donanımın içinde | — |
| İşletim sistemi | Yetki kaydı, imzalama anahtarı, şablon | — |
| Okuyucu (kasa, geçit) | Açık anahtar, kısa ömürlü kanıt, canlılık sonucu | Şablon, yeniden kullanılabilir görüntü, biyometrik tanımlayıcı |
| İşletmeci (banka, bina, ulaşım) | Delegasyon tanımlayıcısı, denetim kaydı, ödeme kaydı | Şablon |
| Güven hizmeti | Hangi yetkilerin var olduğu ve hâlâ geçerli olduğu | Şablon ve hiçbir kişinin yüzüne referans |

Her taraf bir **açık anahtar, bir kimlik bilgisi ya da bir kayıt** tutar — asla yüz tutmaz. Şablon döndürülemeyen tek şeydir ve o da hiçbir yere taşınmaz. Bir okuyucunun, bir işletmecinin ya da bir güven hizmetinin ele geçirilmesi anahtarları ve kayıtları açığa çıkarır, değiştirilemez biyometrik veriyi değil. Bütün gizlilik argümanı budur.

## İşletim Modeli

Model iki şekilde işleyebilir. Her cihaz üreticisi kendi aracısını işletebilir ya da üreticiler ve işletmeciler tek bir ortak, tarafsız delegasyon hizmetinde bir araya gelebilir.

| | Her işletim sistemi kendi aracısını işletir | Tek ortak, tarafsız delegasyon hizmeti |
|---|---|---|
| Okuyucu üreticisi | Her işletim sistemi için ayrı uygulama | Bir kez uygular |
| Bina işletmecisi | Her işletim sistemiyle ayrı ayrı görüşür | Tek bir muhatapla görüşür |
| Yeni bir işletim sistemi | Sıfırdan başlar | Var olan bir güven çapasına katılır |
| Sınırlar arası uyumluluk | Düşük | Yüksek; bu seyahatler ve sınır ötesi işletmeciler için önemlidir |
| Başlıca risk | Parçalanma, kilitlenme, yinelenen çaba | Tek bir işletmecinin ele geçirmesi |

**Öneri: tek açık standart ve tek tarafsız güven hizmeti, üstünde aracılar.** Yetki biçimi, kanıt biçimi, doğrulama kuralları ve iptal kuralları herkes için aynıdır; böylece bir kez yazılan okuyucu her telefonla çalışır ve işletmeci tek bir sistemle görüşür. İşletim sistemleri kendi aracılarını, güvenli donanımlarını ve şablonlarını tutmaya devam eder — güven ve risk zaten oradadır — ve tarafsız bir güven hizmeti açık güven çapalarını ve iptal listesini yürütür. Rekabet eden aracılar donanım, pil, hız ve fiyat üzerinden, birbirleriyle birlikte çalışmayı sağlayan bir standardın üzerinde rekabet edebilir.

Güven hizmeti kâr amacı gütmeyen ya da kamu tüzel kişiliğine sahip bir kuruluştur, kurallarını ve ücretlerini yayımlar ve yayımlanmış ölçütleri karşılayan her aracıyı ve işletmeciyi kabul eder. Biyometrik şablonu asla almaz ve alması da gerekmez: anahtarları, tanımlayıcıları ve bir iptal listesini tutar.

Araştırma düzeyindeki [teknik analiz](TECHNICAL-ANALYSIS.md) bunun güvenilir çalışmasını anlatır: sistemin karşılaması gereken güvenceler, bileşenler, iki doğrulama kipi, iptalin okuyuculara nasıl ulaştığı, canlılığın nasıl denetlendiği, bir okuyucunun nasıl izlenemez kaldığı, hata durumları ve hangi mevcut standartların yeniden kullanılacağı.

Taslak [teknik şartname](SPECIFICATION.tr.md) bunu normatif gereksinimlere dönüştürür — yetki nesnesi, kanıt, iki doğrulama kipi, delegasyonlu varlık denetimi, sunum saldırısı tespiti, iptal, gizlilik kipleri, post-kuantum profili ve bir uygunluk test paketi. Bölüm 8, gerçekten çözülmemiş tek kısmı, dört aday mekanizma ve aralarında seçim ölçütleriyle birlikte ortaya koyar.

Ayrı bir [standardizasyon notu](STANDARDIZATION.md), mevcut standartların bu projenin hangi parçasını kapsadığını, nerede gerçekten yetersiz kaldığını ve eksiği doldurmak için öneriyi — uzun ömürlü delegeli kimlik bilgisinin ihtiyaç duyduğu post-kuantum katmanı dâhil — ortaya koyar. Bu çalışmanın iki bulgusu bütün öneriyi şekillendiriyor: fikir yeni değil (bir biyometrik kontrolün üçüncü taraf bir cihaza devredilmesini zaten tanımlayan verilmiş bir patent ailesi ve akademik çalışmalar var; dolayısıyla bir açık standart, mekanizmayı tescilli ve münhasır olmaktan çıkarıp ücretsiz ve birlikte çalışabilir kılmak için gereklidir) ve gerçek boşluk "biyometriler standartlaştırılmamıştır"dan daha dardır — şablonu cihazda tutmak *zaten* standartlaştırılmıştır; eksik olan, hazır bulunmayan bir yetki sahibinin yetkisini üçüncü taraf bir okuyucunun kanıtlayabilmesidir.

## Benzer Uygulamalar

Bunların bir kısmı zaten var; ancak bu önerinin yazarının bildiği kadarıyla hiçbiri şunları bir araya getirmiyor: okuyucu başına onay yetkisi, telefondan hiç çıkmayan şablon, telefonsuz doğrulama, şablon yerine kanıtı doğrulayan okuyucu ve kullanıcı çevrimiçi olmasa bile okuyuculara ulaşan iptal.

| Uygulama | Ne yapar | Bu öneriye en yakın parçası |
|---|---|---|
| **FIDO2 ve passkey'ler** | Cihaza bağlı anahtarla kriptografik kimlik doğrulama; kullanıcı doğrulama cihazda yapılır ve bağımlı taraf biyometriği hiç görmez. | Kanıta dayalı, şablonsuz doğrulama ve cihaz güven çapası |
| **FIDO cihazlar arası (hibrit) akış ve Proximity Exchange Protocol** | Masaüstüne yakın bir telefondan kod yazmadan giriş imzalama; telefon yine de kontrolü yapan kimlik doğrulayıcıdır. | Elinizde olmadığınız bir cihaz için imzalama, ama telefon hâlâ gerekli |
| **Apple'ın uygulama başına "Face ID Zorunlu" seçeneği** | Ana ekranda uygulamaya basılı tutulduğunda çıkan menüden açılan, işletim sisteminin sizin adınıza uyguladığı biyometrik kapı. | Bu önerinin okuyuculara doğru taşıdığı onay hareketi — ama Apple'ınki uygulama başına ve cihaz yereldir, okuyucu başına değil |
| **Android kilit ekranı uygulama kimlik doğrulaması** | Sistem, bir uygulama açılırken cihazın kendi kimlik bilgisini isteyebilir. | Aynı işletim sistemi-aracı fikri, uygulama başına |
| **Amazon One avuç ve damar** | Amazon'un kendi mağazalarına, ofislerine ve kliniklerine telefonsuz ve kartsız avuçla giriş. **Amazon bunu Ocak 2026'da sonlandırdı**, gerekçe olarak müşteri benimsemesinin düşüklüğünü gösterdi; perakende avuçla ödeme 3 Haziran 2026'da sona erdi ve saklanan avuç verileri silindi. ([ID Tech][amz]) ([American Banker][amz2]) | Telefonsuz biyometrik giriş — her mağazaya yeni donanım gerektirdi ve biyometrik elden çıktı. Başarısızlığı bu önerinin gerekçesidir |
| **İş Bankası'nın Visa ile "yüzle ödeme"si, CarrefourSA'da (Türkiye, Haziran 2025)** | Bir Türk bankası ve kart şeması, müşterinin banka uygulamasında yüz kaydettirmesine ve ardından markette **kart, telefon, cüzdan, QR kod ya da PIN olmadan** — yalnızca yüz taramasıyla — ödeme yapmasına imkân verdi. ([AA][isb]) ([CNBC Borsa][isb2]) | Ödeme kullanım durumu, zaten Türkiye'de canlı — ama yüz bankada kayıtlı |
| **Toss "Face Pay" (Güney Kore)** | Operatörün kendi terminallerinde telefonsuz yüzle ödeme; üç boyutlu yüz eşleştirme ve canlılık denetimiyle, sunum saldırısı standardına göre test ediliyor; yaklaşık 240.000 işletme, 2026 sonu itibarıyla bir milyon mağaza hedefi olarak bildirildi. ([Biometric Update][toss]) | Ölçekteki en başarılı telefonsuz yüzle ödeme — ama bu, terminalde üç boyutlu eşleştirmedir, delegasyonlu bir telefon kimliği değil |
| **Alipay PL1 ve Tencent avuçla ödeme** | Avuç izi ve avuç damarını birlikte okuyan avuç tarayıcıları; ödeme hesabına bağlı; mağazalarda, metro turnikelerinde **ve bina giriş tarayıcılarında** kullanılıyor. ([Biometric Update][alipay]) | Avuçla öde ve avuçla gir — ama kayıt işletmeci tarafından tutulur |
| **Lotte Duty Free "Lotte HandPay" (Güney Kore, Eylül 2025)** | Gimpo Uluslararası Havalimanı'nda avuç damarıyla ödeme; pasaport, boarding pass ve kart yerine geçiyor, kartlar havalimanının kendi uygulaması üzerinden kaydediliyor. ([Biometric Update][lotte]) | Seyahat ve havalimanı kullanım durumu, gizlilik modeli olmadan |
| **Temassız parmak izi ve yüz erişim terminalleri** (Suprema, Verkada) | Kapılar için biyometrik erişim okuyucuları. Verkada'nın erişim istasyonu yüz işlemesini tamamen okuyucuda yapar ve geri döndürülemeyen, şifrelenmiş bir şablon saklar. ([Verkada][verkada]) | Okuyucu donanımı ve şablonun cihazda kalması — ama şablonu *kişi* değil, *okuyucu* sahiplenir |
| **Apple ve Google Wallet** | Cihazda tutulan, üretici başına kimlik bilgileri ve ödeme yetkileri. | Cihazda tutulan, okuyucu başına kimlik bilgisi ve ödeme yetkilendirme modeli |
| **W3C Doğrulanabilir Kimlik Belgeleri ve AB Kimlik Cüzdanı** | Doğrulayanın, vereni sunumu görmeden kontrol edebildiği, cihazda tutulan ve kullanıcı onaylı kimlik bilgisi; VC 2.0, Mayıs 2025'te W3C Önerisi oldu. | Tek doğrulayana bağlı, cihazda tutulan, kapsamlı, iptal edilebilir kimlik bilgisi |
| **Privacy Pass ve Apple'ın Private Access Token'ları** | Kimliğinizi açıklamadan uygunluğu kanıtlayan izlenemez, tek kullanımlık belirteçler; üretimde kullanılıyor. ([RFC 9576][pp]) ([Cloudflare][cf]) | Kimliğin gerekmediği yerde izlenemez belirteç kipi |
| **ISO/IEC 30107 sunum saldırısı tespiti** | Sahte biyometrik sunumlarını tespit etmek için uluslararası standart ve test yöntemleri. | Okuyucudaki canlılık şartı |
| **ISO/IEC 27553 ve 24745 biyometrik bilgi koruma** | Yerel kiplerde biyometrik verinin cihazdan çıkmamasını şart koşan ve geri döndürülemezlik, izlenemezlik, yenilenebilirlik ile iptal edilebilirliği kapsayan standartlar. | Sahibinden hiç ayrılmayan bir şablonun kuralları |

**Neden şimdi önemli.** İki olgu, hiçbir argümandan daha iyi bir gerekçe sunuyor.

Birincisi: **yeni donanım modeli piyasaya çıktı, ölçeklendi ve kapatıldı.** Amazon One, son yılında 17,7 milyon alışveriş oturumunda 36,7 milyon ürün işledi — gerçek ölçek, gerçek kolaylık — ve Amazon yine de onu, müşteri benimsemesinin düşüklüğünü gerekçe göstererek bitirdi. Sebep tasarımda görünür: her mağazaya özel bir avuç tarayıcısı gerektiriyordu ve avuç görüntüsünü Amazon'un sunucularında tutuyordu. Bu, insanların en çok çekindiği model ve aynı zamanda başarısız olan model.

İkincisi: **telefondaki model de Türkiye'de piyasaya çıktı.** İş Bankası, Visa ve CarrefourSA, Haziran 2025'te marketlere yalnızca yüzle ödemeyi, kart ve telefon olmadan koydu. Çalışıyor. Ama yüz bankada kayıtlı, yani banka bir biyometrik şablon tutuyor — aynı takas, sadece farklı bir sahiple.

Alternatif biyometriği hiç kullanmamak değil; telefonun zaten güvenilen biyometrik sisteminin sizin adınıza, okuyucu başına delegasyon vermesi ve şablonu zaten bulunduğu yerde tutmasıdır. **Bu önerinin ekledikleri:** aracı olarak işletim sistemi, böylece okuyucu üreticisi bir kez entegre olur; kullanıcının görebildiği ve geri alabileceği, okuyucu, amaç ve süre başına delegasyon; telefondan hiç çıkmayan şablon; kanıtı doğrulayan ve sahteyi denetleyen okuyucu; okuyuculara ulaşan iptal; kimliğin gerekmediği yerde izlenemez belirteçler; ve vendor silosuna bölmeyen, tarafsız güven hizmetiyle birlikte tek açık standart.

[amz]: https://idtechwire.com/amazon-discontinues-amazon-one-palm-biometric-payment-system-due-to-limited-customer-adoption/
[amz2]: https://www.americanbanker.com/payments/news/the-future-of-palm-biometrics-after-amazons-pullback
[isb]: https://www.aa.com.tr/tr/isdunyasi/sirketler/is-bankasi-yuz-tanima-teknolojisini-odeme-yontemleri-arasina-dahil-etti/695827
[isb2]: https://www.cnbce.com/borsa/is-bankasi-yuz-tanima-teknolojisi-ile-odeme-donemini-baslatiyor-h14074
[toss]: https://www.biometricupdate.com/202601/toss-gets-lift-from-biometric-retail-payments-plans-2026-us-ipo
[alipay]: https://www.biometricupdate.com/202505/tencent-considers-bringing-palm-biometric-payment-to-thailand-alipay-pushes-the-pl1
[lotte]: https://www.biometricupdate.com/202509/lotte-duty-free-pioneers-palm-biometric-payments-in-korean-airport
[verkada]: https://www.verkada.com/blog/introducing-the-af64-access-station-pro/
[pp]: https://www.rfc-editor.org/rfc/rfc9576.pdf
[cf]: https://developers.cloudflare.com/privacy-pass/

## Uygulama ve Aşamalar

1. **Açık standart:** cihaz üreticileri, okuyucu üreticileri, işletmeciler ve standart kuruluşları yetki biçiminde, kanıt biçiminde, doğrulama kurallarında, iptal modelinde ve canlılık şartında anlaşırlar. [Standardizasyon notu](STANDARDIZATION.md), bunların her birinin nerede standartlaştırılması gerektiğini önerir.
2. **Tek kapalı ortamda okuyucu pilotu:** bir bina erişim pilotu; işletmeci kendi mal sahibi kiracısıdır, risk sınırlıdır ve değer — anahtar yok, kart yok, binanın veritabanında biyometrik yok — en kolay görülür.
3. **Ödeme pilotu:** bir ödeme işletmesi; önce izlenemez belirteç kipiyle, böylece işletmeci kimin ödediğini öğrenmeden çalıştığını gösterebilsin.
4. **Ölçekte iptal ve denetim:** iptal listeleri, kamuya açık bir şeffaflık raporu ve işletmecilerin ve veri koruma otoritelerinin yürütebileceği denetim.
5. **Canlılık sertifikasyonu:** okuyucuların sunum saldırısı standardına göre sertifikalandırılması, böylece "bir fotoğrafı kabul etti" bir sertifikasyon hatası olur, manşet haberi değil.
6. **Yaş ve öznitelik kullanım durumları:** aynı yetki mekanizmasının kimlik yerine yaş ya da öznitelik beyanı taşıması.
7. **Politika desteği:** veri koruma mevzuatıyla uyum, böylece delegasyonlu kontrol, gerçekte olduğu gibi veri minimizasyonu alternatifi sayılsın ve kamu uygulamaları onu seçtiği için dezavantajlı olmasın.

Başlangıç tasarımı, uyarlanacak: gerçek bir uygulama güven modelini, iptal sınırlarını, işletmecinin rolünü ve yönetişimi kendi pazarına ve hukuk sistemine göre uyarlamalıdır.

## Paydaşlar ve Faydalar

- **Yüzünü vermeyi reddeden insanlar:** biyometrik verileriyle yeni bir tutucu yaratmadan bugünkinden daha fazla yerde biyometrik kullanabilirler.
- **Telefon kullanıcıları:** zaten güvendikleri, uygulamalar için kullandıkları hareketle, tek bir yerde bütün delegasyonları yönetirler.
- **Bina işletmecileri ve ev sahipleri:** anahtar yok, kart yok, kayıp anahtar çağrısı yok, ihlal edilecek biyometrik veritabanı yok ve paylaşılan kart yok.
- **Perakendeciler ve ödeme işletmecileri:** kasada daha az tartışma, daha hızlı akış ve bir fotoğraftan kopyalanamayan bir ödeme; biyometrik veritabanının ve beraberindeki veri koruma riskinin maliyeti olmadan.
- **Ulaşım işletmecileri:** yolcu başına biyometrik kayıt oluşturmadan geçiş hacmi.
- **Veri koruma otoriteleri ve kamu:** değiştirilemez varlığın sahibinde kaldığı, delegasyonlu kontrolün denetlenebilir ve geri alınabilir olduğu bir model.
- **Cihaz üreticileri:** işletim sisteminin kendi koşullarında kimliğin merkezinde duran, gizlilik lehine ayırt edici bir özellik.
- **Okuyucu üreticileri:** her ekosistem için ayrı entegrasyon yerine tek standart ve cihazdan bağımsız bir ürün.

## Riskler ve Azaltımlar

| Risk | Azaltım |
|---|---|
| Okuyucu bir fotoğrafı ya da maskeyi kabul eder | Uluslararası standarda uygun sunum saldırısı tespiti, okuyucu sertifikasyonu ve model başına yayımlanan yanlış kabul oranı |
| Telefon çalınır ve sahibinin yüzü kullanılır | Delegasyon yalnızca sahibin kendi biyometriğiyle kullanılır, ayrıca cihaz kilidi gerekir; yetkiler görünür ve geri alınabilir; çalınan telefon bir delegasyon kimlik bilgisi değil, kayıp telefondur |
| Bir delegasyon başka amaç ya da okuyucu için kullanılır | Yetki okuyucuyu ve amacı adıyla belirtir ve kanıt her ikisine de bağlanır; başka yerde tekrar kullanılamaz |
| Yetkiler birikir ve kullanıcılar takip edemez | Yetkiler tek yerde listelenir, varsayılan olarak süresi biter ve gözden geçirilir; kullanılmayan yetki tutulmaz, kaldırılır |
| İşletmeci yine de gölge bir biyometrik veritabanı kurar | Standart, kanıtın biyometrik veri ve yeniden kullanılabilir tanımlayıcı taşımadığını şart koşar ve denetim bunu kontrol eder; işletmeci bir tane tutmaktan hiçbir şey kazanmaz |
| İptal edilmiş bir yetki, çevrimdışı bir okuyucuda hâlâ çalışır | İptal sınırlıdır ve yayımlanır; okuyucular son bilinen durumu önbelleğe alır ve sınır standartta belirtilir, örtük bırakılmaz |
| İzlenemez belirteçler bazı kullanım durumları için fazla zayıf | Yetki, amaca göre iki kipten birinde düzenlenir; kimlik kipi okuyucunun ihtiyaç duyduğu kadarıyla ve fazlasıyla sınırlıdır |
| Model tek bir şirketin tekeli olur | Tek açık standart, tarafsız güven hizmeti, yayımlanmış kurallar ve ücretler ve özel şablon ya da tanımlayıcı yoluyla kilitlenme yok |
| Biyometrik sistemler tek arıza noktası olur | Delegasyon diğer kimlik doğrulama seçeneklerinden yalnızca biridir; kart, kod ya da passkey'i de kabul eden okuyucu kullanılabilir kalır ve başarısız delegasyon hiçbir kullanıcıyı bir hizmetin dışında bırakmaz |
| Okuyucu, devredilmiş bir kontrol bahanesiyle biyometriği yakalar | Okuyucunun saklayacağı bir şablon yoktur ve canlı görüntü kontrol sonrası atılır; standart sunumu saklamayı yasaklar |
| Standart çalışması başka yerde iki kez yapılır | Öneri, uygun olduğu yerlerde mevcut standartların üzerine kurar ve eksiği tam olarak belirtir; yeni çalışma eksik parçadır, paralel bir evren değil |
| Çalınmış ya da tekrar oynatılan kanıt yanlış okuyucuda kullanılır | Kısa ömürlü, okuyucuya, işleme ve meydan okumaya bağlı kanıtlar ve okuyucuda tekrar oynatma koruması |
| Biri fikrin yeni olduğunu iddia eder ve verilmiş bir patent onu kapsıyor | [Standardizasyon notu](STANDARDIZATION.md) patent ailesini ve daha önceki akademik çalışmayı açıkça atıf yapar; öneri, aynı güvencelere ücretsiz ve standartlaştırılmış bir yol içindir, fikrin mülkiyeti için değil |
| Gelecekte yetkinin imzası kırılır ve önbelleğe alınmış kanıtlar doğrulanır | Yetki üzerinde post-kuantum imzalar, kısa yetki ömürleri, rotasyon ve yeniden düzenleme yolu — sonradan eklenmek yerine standarda baştan gömülür |

**Anahtar kelimeler:** biyometrik kimlik doğrulama, delegasyonlu kimlik doğrulama, face ID, passkey, gizlilik, veri minimizasyonu, güvenli öğe, ödeme, satış noktası, erişim kontrolü, bina girişi, canlılık denetimi, sunum saldırısı tespiti, doğrulanabilir kimlik belgeleri, izlenemezlik, açık standart, post-kuantum, güven hizmeti

## Ticari Kullanım

Bu modelin ticari uygulamaları [ticari lisansa](COMMERCIAL-LICENSE.md) tabidir:

- **Kimin lisans alması gerekir:** bir delegasyon aracısı işleten şirketler (kullanıcının biyometrik kontrolünü belirli bir okuyucuya devreden cihaz işletim sistemi veya kimlik cüzdanı üreticileri); okuyucularıyla birlikte delegasyonlu kimlik doğrulama hizmeti işleten erişim kontrolü, satış noktası, ulaşım ve bina işletmecileri; modeli uygulayan yazılım veya hizmet satan şirketler.
- **Telif tabanı:** Bu şirketlerin delegasyonlu kimlik doğrulama işletmesinden, okuyucuları işleten taraflara aktarılan paylar düşüldükten sonra **kalan** gelir.
- **Donanım üreticileri**, yayımlanmış standardı uygulayan okuyucu, telefon ya da benzeri cihazları satıp kendi delegasyon ya da doğrulama hizmetini işletmedikleri sürece bu satışlar için muaftır. Son kullanıcılar lisans alan değildir.
- **Devletin zorunlu kıldığı kamusal uygulamalar** telif ödemez; şirketler standart ticari lisansı beklemeden kendileri alabilir.

## Bu Çalışmayı Destekleyin

Bu fikri faydalı bulduysanız yazarın açık çalışmalarını GitHub Sponsors üzerinden destekleyebilirsiniz.

<iframe src="https://github.com/sponsors/merihilgor/button" title="Sponsor merihilgor" height="32" width="114" style="border: 0; border-radius: 6px;"></iframe>

[GitHub'da merihilgor'a sponsor olun](https://github.com/sponsors/merihilgor)

## Köken

Merih İlgör tarafından önerilmiştir (Ekim 2026); burada açık bir proje fikri olarak yayımlanmaktadır. Değişiklik geçmişi: [CHANGELOG.md](CHANGELOG.md).

## Lisans

Bu çalışma ticari olmayan kullanım için CC BY-NC-SA 4.0 ile lisanslanmıştır; bkz. [LICENSE](LICENSE). Ticari kullanım gelir paylaşımı sözleşmesi gerektirir; bkz. [COMMERCIAL-LICENSE.md](COMMERCIAL-LICENSE.md). Değiştirilmiş sürümler yeni fikir olarak satılamaz veya üçüncü kişilere devredilemez; patent benzeri haklar saklıdır; bkz. [COMMERCIAL-LICENSE.md](COMMERCIAL-LICENSE.md#derivatives-improvements-and-idea-protection).
