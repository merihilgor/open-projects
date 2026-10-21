English: [USE-CASES.md](USE-CASES.md)

# Delegeli Biyometrik Kimlik Doğrulama: Kullanım Durumları

> **Bir satış metni değil, çalışma kataloğu.** Aşağıdaki her kullanım durumu durumu, bugün ne olduğunu, bu öneri altında ne değiştiğini ve — en önemlisi — **durumun neyi çözmediğini** anlatır. Son sütun, biyometrik kolaylık üzerine önerilerin en zayıf olduğu yerdir ve bir dağıtımın bir veri koruma otoritesine, bir işletmecinin yönetim kuruluna ya da kapının önündeki kişiye savunulabilir olup olmadığını belirler.
>
> Bunlar, [önerinin](README.tr.md) dayandığı fikirlerdir. Mekanizmalar, güvenceler ve standart boşlukları [teknik analizde](TECHNICAL-ANALYSIS.tr.md) ve [standardizasyon notunda](STANDARDIZATION.tr.md) yer alır.

## Durumlar nasıl gruplandı

| Grup | Durumlar | Ortak nokta |
|---|---|---|
| **Ödeme** | 1–4 | Okuyucu bir kasa; işletmeci genellikle bir banka ya da bir üretici; karar parayla ilgili |
| **Giriş** | 5–9 | Okuyucu bir geçit; işletmeci genellikle mülkiyet ya da çalışma ilişkisini tutar; karar erişimle ilgili |
| **Kanıtlama** | 10–13 | Okuyucu bir masa ya da bir kiosk; işletmeci bir olgu ya da bir teyit ister; değer, işletmecinin *öğrenmediği* şeydedir |
| **Çalışma ve seyahat** | 14–19 | Okuyucu herhangi bir yer; işletmeci işveren, okul, taşıyıcı ya da sınır otoritesi; fayda ile riskin en birlikte en büyük olduğu, en zor durumlar |

## Bölüm 1: Ödeme

### 1. Satış noktasında nakitsiz, kartsız ve telefonsuz ödeme

**Durum.** Bir kişi sepetiyle kasada duruyor ve kart çıkarmak ya da telefonu açmak istemiyor.

**Bugün.** Kart olmadan ödeme telefondur: NFC dokunuşu, QR kod, bankacılık uygulaması. Her biri telefonun elde, kilidi açık, şarjlı ve bağlantılı olmasını ister. Bazı mağazalar zaten avuç ya da yüz kabul eder; ama bunu bir sunucuda saklayan bir şablon ya da referans görüntü toplayarak yapar — kişi kalıcı bir kimlik karşılığında bir kolaylık takas eder. Temassız kart paylaşımı yaygındır ve kasada şarjı bitmiş bir telefon gerçek ve tekrarlayan bir arızadır.

**Bu öneriyle.** Kişi bir kez, uygulamaya erişim verdiği hareketin aynısıyla, yüzünü bir ödeme işletmesine devreder. Kasada okuyucuya bakar. Okuyucu yüzü canlı kontrol eder, sahteyi tespit eder, güven hizmetine bu okuyucu ve bu ödeme için geçerli bir delegasyon olup olmadığını sorar, ödemeyi tamamlar ve hiçbir biyometrik veri kaydetmez. Telefon başka bir odada olabilir.

**Çözmediği şey.** Telefon hâlâ çapadır: telefonu olan ve onun biyometrik kontrolünü sağlayan kişi ödeyebilir. Paylaşılan bir terminalde işletmeci tutarı ve zamanı öğrenir; açıklama yapılmayan kipte kimin ödediğini öğrenmez. Ödemenin güvenliği artık kasadaki canlılık denetimine bağlıdır; dolayısıyla uyumlu bir denetim çalıştıramayan bir kasa aday bile değildir — bu, önerinin ortadan kaldırdığı değil yarattığı bir maliyettir.

### 2. Küçük tutarlı ödemeler, ulaşım yükleme ve düşük değerli alışverişler

**Durum.** Bir kiosk kahve, yükleme, gazete, otopark bileti satıyor; tutarlar işlem başına ücret ya da kart okuması için orantısız küçük.

**Bugün.** Bunlar nakit ya da yüklü değer kartı işlemleridir. Nakit giderek kabul edilmiyor ve toplumsal maliyeti artıyor; yüklü değer kartları, değer taşınabilir olduğu için dolandırıcılık hedefidir.

**Bu öneriyle.** Küçük ve tavanlı bir tutar ile kısa bir süre için bir yüklü değer yetkisi düzenlenebilir; böylece delegasyonun kendisi değer sınırını taşır. Kişi belirli bir üretici sınıfına on dolarlık bir kalıcı yetkilendirme verir; okuyucu bundan düşer; işlemde telefon hiç devreye girmez. Açıklama yapılmayan kipte üretici, kişinin dün de aynı şeyi alıp almadığını ayırt edemez.

**Çözmediği şey.** Kalıcı bir değer yetkilendirmesi, canlı olduğu sürece bir taşıyıcı belgedir; kişinin yanında olmadığı bir cihaza bağlandığında, yetkilendirmenin kendisi çalınacak şey hâline gelir. Öneri bunu kısa ömürler, üretici başına kapsam ve iptal ile hafifletir ama maruziyeti ortadan kaldırmaz: bir delegasyon yetkisi küçük ve kısa ömürlü bir şeydir ve standart kuruluşları onu öyle ele almalıdır.

### 3. Toplu taşıma ve kontrollü geçiş

**Durum.** Bir yolcu haftada yirmi kez turnikeden geçiyor.

**Bugün.** Turnikeler kart, bilet, QR kod ve telefon kullanır. Tek kişilik bilet paylaşılabilir. Telefona dayalı olan, telefonun hazır ve şarjlı olmasını ister — yolcunun en çok ihtiyaç duyduğu durum tam olarak budur: peronda, yağmurda.

**Bu öneriyle.** Yolcu bir kez ulaşım işletmesine delegasyon verir; geçit ciheti cihaz olmadan yüz yüzünden doğrular. İzlenemez belirteçler bu duruma en uygun çözümdür, çünkü işletmeci yalnızca "yetkili bir yolcu" bilgisine ihtiyaç duyar, kimliğe değil. İşletmeci daha yüksek geçiş hacmi kazanır ve hangi yolcunun ne zaman seyahat ettiğine dair veritabanı oluşturmaz.

**Çözmediği şey.** Toplu taşıma, izlenemezliğin en çok önemli ve en zor olduğu durumdur, çünkü bir ulaşım işletmesinin dolandırıcılık analizi meşru bir çıkarı vardır ve bu çıkar doğası gereği yolculukları ilişkilendirmek ister. Öneri, işletmecinin günlüklerini birleştirmemesini söyler; uygulamada, dolandırıcılık ekibi ve veri analizi işlevi olan ulusal bir ulaşım işletmesi bunu deneyecektir. **Bu durum en zor yönetilebilir olan olarak ele alınmalıdır, en kolay olan olarak değil**, ve bir dağıtım, teknik kısıt bunu imkânsız kılmadıkça işletmecinin ilişkilendirme girişiminde bulunacağını varsaymalıdır.

### 4. Satış noktasında yaş doğrulama

**Durum.** Bir mağaza, alkol veya tütün satmak ya da bir mekâna giriş için alıcının on sekiz üstü olduğunu doğrulamak zorunda.

**Bugün.** Alıcı pasaport, ehliyet ya da kart gösterir. Bunların her biri yaştan fazlasını sızdırır: isim, belge numarası, fotoğraf, doğum tarihi ve sıklıkla mağazada saklanan bir kopya. Birçok yargı bölgesinde mağazanın belge numarasını kaydetmesi yasal olarak zorunludur; bu, bir bira alan on dokuz yaşındakinin, diğer herkesle birlikte perakendecinin kaydına geçmesi demektir.

**Bu öneriyle.** Kişi perakendecye bir **yaş özniteliğini** — kimliği değil — devreder. Okuyucu yüzü canlı kontrol eder, telefon eşiğin üzerinde olduğunu belirten bir beyanla yanıtlar; mağaza evet/hayır kaydeder: yüz yok, doğum tarihi yok, belge yok, kopya yok.

**Çözmediği şey.** Mağazanın yine de hukuki bir sebebe ve bir ret yoluna ihtiyacı var; bir yaş bandı başka hiçbir şeyi kanıtlamaz. Yoğun bir mağazada başarısız bir denetim bir kuyruk ve bir ret satış demektir, dolayısıyla işletmeci elle bir yedek ister ve elle yedek, fotokopinin geri geldiği yerdir. Öneri, işletmecinin geri düşmek yerine ret etmeyi kabul ettiğini varsayar — bu bir teknik değil iş kararıdır ve bu durumdaki en büyük pratik engeldir.

## Bölüm 2: Giriş

### 5. Bina, apartman ve daire girişi — kart yok, anahtar yok, telefon yok

**Durum.** Sakin evine anahtarsız, kartsız ve belki şarjsız telefonla dönüyor.

**Bugün.** Anahtarlar, kartlar, anahtarlıklar, kodlar, telefonlar. Her biri kaybolabilir, ödünç alınabilir, kopyalanabilir ya da paylaşılabilir. Parmak izi ya da yüz okuyucu takan bina, sakinlerinin biyometrik verisini saklama ve ihlal riskini üstlenir; bu tam da sakinlerin kabul etmediği risktir.

**Bu öneriyle.** Sakinin giriş uygulaması, işletim sisteminden yüzünü binanın geçit cihazına devretmesini ister. Bina bir şablon değil, devredilmiş bir kontrolün imzalı beyanını alır ve kapıya kişinin doğrulandığını kanıtlayabilir. Sakinler misafire, temizlikçiye ya da taşerona kendi kapsamlı ve süreli delegasyonunu verebilir; anahtar teslim etmeye gerek kalmaz. Binanın sakinleri yüzlerini binanın veritabanına koymak zorunda kalmaz.

**Çözmediği şey.** Ev sahibi sorumluluğu ve konut hukuku. Biyometrik kapı sistemi kuran bir ev sahibi, hukuki ağırlığı olan bir karar veriyor ve birçok yargı bölgesi biyometrik giriş şartını istihdam ya da kiracılık koşulu sayıyor. Delegasyon vermeyi reddeden bir sakinin başka bir yolu olmalı ve bina o yolu tutmalı. Öneri teknik bir modeldir; hukuki soruyu ortadan kaldırmaz.

### 6. Anahtar teslim etmeden misafir ve taşeron girişi

**Durum.** Bir tesisatçı geliyor. Bir hemşire ziyarete geliyor. Bir kurye ikinci kata girmesi gerekiyor. Birinin içeri almak gerekiyor ve birinin sonrasında hesap vermesi gerekiyor.

**Bugün.** Anahtar veriliyor, ya da kod paylaşılıyor, ya da resepsiyon birini alıp bir yere not ediyor. Kodlar kopyalanıyor ve hiç değiştirilmiyor; anahtarlar kopyalanıyor ve hiç geri verilmiyor.

**Bu öneriyle.** Misafir, *kendi telefonu* üzerinden, o bina için, belirtilen süre boyunca, giriş amacıyla delegasyon verir. Bina delegasyonu düzenler; misafirin telefonu biyometriği tutar; binanın kaydı, o kapıda o pencerede, binanın düzenlediği ve geri alabileceği bir yetki altında başarılı bir delegasyonlu denetim gösterir. Bina misafirden biyometrik istemez.

**Çözmediği şey.** Kendi telefonu olmayan, banka hesabı olmayan ya da akıllı telefonu olmayan bir misafir. Delegasyon modeli bir cihaz varsayar. Önerinin kitlesi modern telefonu olan insanlardır ve bu gerçek bir eşitlik sınırıdır. Ayrıca misafirin gelmeden önce delegasyon alabilmesi varsayılır; bu, binanın çözmesi gereken bir planlama sorunudur.

### 7. Kısa süreli kiralama ve konaklama erişimi

**Durum.** Bir misafir, bir uygulamadan rezervasyon yaptığı ve hiç görmediği bir daireye geliyor.

**Bugün.** Klavyedeki bir kod, ya da bir kilit kutusu, ya da bir masaüstünde teslim edilen fiziksel anahtar. Kod bütün grupla paylaşılır ve olmaması gerektiğinde geçerli kalır.

**Bu öneriyle.** Misafir, gelişten önce, kalış süresi boyunca, o kapı için delegasyon verir. Kapının önünde okuyucuya bakar. Mülk delegasyonu düzenler, misafirin telefonu biyometriği tutar ve misafir, mülkün bir şey geri istemesine gerek kalmadan çıkışta delegasyonu sonlandırabilir.

**Çözmediği şey.** Mülk, misafirin *kim* olduğunu hâlâ rezervasyona karşı doğrulamak zorundadır; bu bir belge kontrolüdür, biyometrik değil, ve kişisel verinin aslında gittiği yer de burasıdır. Delegasyon "rezervasyonu yapan kişi kapıda" der; bu, "bu kişi rezervasyondaki kişidir" ile aynı şey değildir. Aradaki fark genel bir kimlik kanıtlama sorunudur ve burada çözülmez.

### 8. Otel check-in ve anahtarsız oda girişi

**Durum.** Bir misafir gece 23'te geliyor, sırada bekliyor ve bir masada pasaport göstermek zorunda.

**Bugün.** Otel pasaport ya da kimlik kartı okur, bazen tarar, bazen fotokopi çeker ve yıllarca saklar. Oda anahtarları kaybolan kartlardır. Resepsiyon her misafiri elle karşılar.

**Bu öneriyle.** Misafir gelişten önce otele delegasyon verir. Resepsiyonda okuyucuya bakar; otel doğrulanmış bir kimlik beyanı ve kanun izin verirse asgari öznitelik kümesini alır. Oda kapısı aynı delegasyonla açılır; kaybolacak anahtar, devredilecek kart ve dosya dolabında pasaport kopyası kalmaz.

**Çözmediği şey.** Oteller birçok yargı bölgesinde her misafiri makamlara kayıt etmekkle yükümlüdür ve bu kayıt kimlik belgeleri gerektirir. Delegasyonlu denetim bu yükümlülüğü kaldırmaz ve uygulamada otel kayıt için belgeyi yine tarar. Öneri, mülkün yasal asgarinin ötesinde tuttuğunu azaltabilir ama yasal asgarinin kendisini kaldırmaz ve böyle sunulmamalıdır.

### 9. İşyeri erişimi ve çalışanların güvendiği devam takibi

**Durum.** Vardiya başlıyor. Çalışanın kendisi olduğunu kanıtlaması gerekiyor ve orada olduğunu kanıtlaması gerekiyor.

**Bugün.** Kart, PIN, parmak izi okuyucu, yüz terminali. Parmak izi ve yüz terminalleri biyometriği toplar. Kart paylaşımı yaygındır. Kimin nerede bulunduğu kaydı çoğunlukla işverenin özel verisidir ve çalışanın bu konuda görünürlüğü azdır.

**Bu öneriyle.** İşveren, erişimi çalışan, şube ve zaman aralığı başına devreder. Devam, çalışanın görebildiği, geri alabileceği ve düzeltebileceği devredilmiş bir kontrolden kaydedilir. Paylaşılan kart ortadan kalkar. İşveren "bu çalışan oradaydı" bilgisini yüz olmadan alır.

**Çözmediği şey.** **Bu, kümedeki en hukuken zor durumdur ve böyle sunulmalıdır.** Çoğu yargı bölgesinde işyerinde biyometrik izlemeyi bir temel hak meselesi sayar: bir istihdam ilişkisi içinde alınan onay tartışmalıdır ve reddeden bir çalışanın yasal olarak başka bir yöntem kullanması istenebilir, ki bu uygulamanda kart demektir. Öneri işverene *daha az müdahil* bir teknik seçenek verir; bu, bir veri koruma etki değerlendirmesinde gerçek bir faydadır ama istihdam ilişkisini değiştirmez ve öyle sunulmamalıdır.

## Bölüm 3: Kanıtlama

### 10. Hastane ve klinikte hasta kimlik doğrulama

**Durum.** Bir hasta geliyor. personel doğum tarihini, kartı ya da kimliği soruyor ve ilk yanıt yanlış geldiğinde tekrar soruyor.

**Bugün.** Hastanelerde kimliklendirme hataları gerçek bir hasta güvenliği sorunudur. İdari çözüm — fotoğraf kimlik kartı, bileklik, doğum tarihi — zayıf bir tanımlayıcıdır ve tam da kalabalık koşullarda başarısız olan şeydir.

**Bu öneriyle.** Hasta kliniğe delegasyon verir, okuyucu onu canlı doğrular ve kontrol hastanın kendi kaydına bağlanır. Klinik, kimlik belgesi kopyası toplamadan kişinin kim olduğunu doğrulanmış olarak öğrenir; kimliklendirme hatası azalır ve yeni bir sağlık verisi havuzu oluşmaz.

**Çözmediği şey.** Sağlık ortamı, bir delegasyon yetkisinin çoğu durumda **hiç kullanılmaması gereken** yerdir. Sağlık verisi en hassas kategorilerdendir, hastanın durumu biyometrik denetimi etkileyebilir (maske, bandaj, titreme) ve yoğun bir resepsiyonda yanlış kimliklendirme bir güvenlik olayıdır. "Söylediğiniz kişi olduğunuzu kanıtlayın" kontrolünü **kontrollü bir kapıda personel veya ziyaretçiye** yapmak, yoğun bir resepsiyonda hastaya yapmaktan daha güvenli bir uygulamadır. Öneri, personel ve ziyaretçi durumunu kolaylaştıracak biçimde okunmalıdır; hasta durumu ise sadeliğinin ima ettiğinden çok daha fazla özen gerektirir.

### 11. Sivil kimlik ve sandık başında oy verme

**Durum.** Bir seçmen sandık başına geliyor ve seçmen listesine karşı kimliği belirlenmeli.

**Bugün.** Kâğıt listeler, kimlik kartları ve bazı yerlerde kart ve PIN ile elektronik oy verme. Kimlik kontrolleri elle ve yavaştır; bu, bazı ülkelerde uzun kuyruklar ve bazı yerlerde katılımın düşmesi anlamına gelir. Seçim kurulu listeyi tutar.

**Bu öneriyle.** Seçmen, seçim gününden önce sandık başı okuyucularına delegasyon verir; kapsam oy verme kapanışında biter ve yeniden kullanılamaz. Kontrol, istasyonun ihtiyacından fazlasını öğrenmeden uygunluğu doğrular ve delegasyon tasarım gereği seçimle birlikte süresini doldurur.

**Çözmediği şey.** **Seçimler meşruiyet açısından en zor durumdur, en kolay olan değil.** Doğrulayıcının işi, belirli bir kişinin oy hakkına sahip olduğunu ortaya koymaktır; delegasyonlu denetim ise bir kişinin *yetkili* olduğunu ortaya koyar — kimin, hangi kimlikle, hangi yetkiyle yetkilendirdiği ayrı bir sorudur. Bir delegasyonun seçmen listesinin kontrolünün yerini alabileceği bir düzen, iki kez oy kullanmaya ya da yanlış kişi tarafından kullanmaya açık bir düzen olurdu; bir seçim otoritesi benimsemeden önce bunu düşünmesi gerekir. Önerinin buradaki katkısı dar ve nettir: **oy verme masasındaki** — yavaş, hatalı, elle yapılan — kimlik belirleme kısmını değiştirebilir, listeye dokunmadan.

### 12. Hizmet ve yardımlar için yaş ve yetenek kontrolleri

**Durum.** Bir hizmet, başvuranın on sekiz ya da altmış beş yaş üstü ya da öğrenci olduğunu, uygunluğu belirlemek için doğrulamak zorunda.

**Bugün.** Pasaport, ehliyet, fatura, öğrenci kartı ekran görüntüsü. Bunların her biri e-postalanan, fotoğrafı çekilen ya da kimse doküman tasarımı için yapmadığı bir vaka yönetim sistemine gönderilen bir belgedir.

**Bu öneriyle.** Başvuran bir hizmete bir **öznitelik** devreder — bir yaş bandı, bir öğrencilik durumu. Hizmet olguyu alır ve arkasındaki kimliği değil, daha fazlasını da isteyemez. Onu kanıtlayan belge yerinde kalır.

**Çözmediği şey.** Özniteliğin bir yerden gelmesi gerekir ve *o* yer, alttaki kanıtın sahibi hâline gelir. Bu durum sorunu çözmez, yerini değiştirir; ve yer değiştirme ancak öznitelik vericisi, özgün belge sahibiyle aynı ölçüde hesap vermeye tabi tutulursa işe yarar — bu da tekniksel bir yanıtı olmayan bir yönetişim sorusudur.

### 13. Sigorta, garanti ve claim süreçleri için varlık kanıtı

**Durum.** Bir sigortacı hasarın gerçekleştiğini doğrulamak ya da bir üretici cihazın hasar gördüğünü doğrulamak ya da bir garanti claim'inde başvuranın sahip olduğunu ortaya koymak istiyor.

**Bugün.** Fotoğraflar, videolar, seri numaraları, bir eksper, kâğıtta imzalanmış bir form. Claim dosyası başvuranın ve malının görüntülerini biriktirir.

**Bu öneriyle.** Başvuran, hasar anında sigortacıya delegasyon verir. Okuyucu onu canlı doğrular, sigortacı claim'e bağlı bir doğrulanmış-varlık beyanı alır ve dosyada delegasyon referansı bulunur, biyometrik değil. Sigortacı, gerçek bir canlı claim'i yeniden kullanılan bir fotoğraftan ayırt edebilir — bugün gerçek bir dolandırıcılık sorunudur.

**Çözmediği şey.** Delegasyonlu denetim kişinin hazır olduğunu kanıtlar, hasarın gerçekleştiğini değil. Kimlik taklidini ve fotoğraf tekrar kullanımını ele alır, claim'in esası hakkında hiçbir şey söylemez. Claim'i **poliçeye** bağlamak gereksinimini de çözmez; bu bir belge eşleştirme sorunudur.

## Bölüm 4: Çalışma ve seyahat

### 14. Okul, kampüs ve sınav devamı

**Durum.** Bir öğrenci okula ya da üniversiteye geliyor, ya da sınava giriyor.

**Bugün.** Yoklama listeleri, kimlik kartları, bir parmak izi okuyucu, bir yüz terminali — son ikisi biyometriği toplar ve ikisi de, güvenlik kapasitesi sınırlı bir okul tarafından işletilen bir sistemde çocukların biyometrik kaydını yaratır.

**Bu öneriyle.** Öğrenci dönem boyunca, bina başına kuruma delegasyon verir. Devam, biyometrik hiçbir zaman okulun sunucusuna ulaşmadan kaydedilir. Bir öğrencinin biyometriği — sızarsa değiştirilemez — hiçbir zaman okul kaydı olmaz ve okulun ihlal yüzeyi "çocukların yüz veritabanı"ndan "bir delegasyon listesi"ne düşer.

**Çözmediği şey.** Reşit olmayandan ya da ebeveynden alınan onay, ülkelere göre çok farklı ve bir teknik modelle çözülmüyor. Yüz tanımanın okullarda iyi bilinen bir eşitlik sorunu da vardır: hata oranları demografik gruplar arasında farklıdır ve en çok etkilenen öğrenciler genellikle bir kaydı itiraz edecek güçte olmayanlardır. Delegasyonlu denetim, okuyucu sertifikalandırılıp oranlar yayımlanmadıkça aynı hata profilini devralır.

### 15. "Seamless borders" — pasaport göstermeden sınır geçişi

**Durum.** Bir yolcu, kendi cihazı soruya yanıt verdiği için bir pasaport göstermeden iç ya da dış sınırı geçiyor.

**Bugün.** Pasaport alettir ve ağır bir alettir. Schengen alanı iç sınırlarda olağan pasaport sunma büyük ölçüde kaldırılmıştır — yolcunun bir iç geçişte belge göstermesi normalde gerekmez, ancak polis spot kontrolleri meşrudur ve her üye devlet geçici iç kontrolleri yeniden getirirse geçerli bir pasaport ya da kimlik kartı göstermek gerekir. Dış sınırlarda eğilim tersidir: biyometrik e-kapılar yayılmakta ve AB'nin Giriş/Çıkış Sistemi, 2026 Nisan'ında tam olarak faaliyete geçerek Schengen dışındaki uyrukların dış sınırlarda biyometrik olarak kaydedilmesini sağlıyor. Yani eğilim sınırdaki biyometrik kaydın *azalması* değil, artmasıdır; belge bir şablonla birleşiyor, yerine geçmiyor.

**Bu öneriyle.** Yolcu sınır otoritesine — ya da daha olası olarak geçişi işleten taşıyıcıya ya da ulaşım işletmesine — delegasyon verir ve canlı bir kontrolle geçer. Yolcunun şablonu cihazından çıkmaz; otorite, yıllarca saklamak zorunda olacağı bir şablon kaydetmeden geçişi doğrular. Pasaport gösterilmez ve yüz saklanmaz. Denetim, yolcunun kendi cihazının "Ben buradayım ve bu delegasyonun sahibiyim" demesidir; bu, her sınırdaki her yolcunun yüzünü gösteren bir veritabanından tamamen farklı bir gizlilik teklifidir.

**Neden kümenin en doğal uyumudur.** Üç şey aynı anda örtüşür. Okuyucu üçüncü bir taraftır — otorite ya da işletmeci — yani modelin tam olarak onun için kurulduğu durumdur. Şablon, hiçbir devletin tutmak istemeyeceği tek şeydir, çünkü bir sınıra kaydedilen yüz, sınır kaydının yaşadığı yıllarca tutulur ve yolcu onu değiştiremez. Ve telefonun yokluğu özelliği en çok burada önemlidir, çünkü sınırdan geçen bir yolcu tipik olarak yürüyerek, bir kuyrukta, çantasıyla ilerler ve telefonu bir kameraya tutmaz. Günlük öneri — telefonu okuyucuya tutmak — gizlilik durumunun en güçlü olduğu yerde en zayıftır ve delegasyon modeli çalışan sürümüdür.

**Çözmediği şey.** **Bu, kümedeki en yüksek riskli durumdur ve dürüst yanıt, teknik modelin kolay kısım olduğudur.** Sınır geçişi bir güvenlik işlevidir ve okuyucunun kararı "bu kişi kim olduğunu söylediği mi" değil, "bu kişi kim olduğunu söylediği *ve* burada olmaya hakkı var mı"dır. Delegasyon birinci soruyu yanıtlar; ikincisi bir delegasyonun taşımadığı bir izleme listesi, vize durumu ve seyahat kaydı gerektirir. Sınır otoritesi bu yüzden **iki** sistem işletmek zorunda kalır — canlı bir varlık denetimi ve belge tabanlı bir uygunluk kontrolü — ve asıl soru, belge kontrolünün zaten yapıldığı bir geçişte canlı denetimin bir şey ekleyip eklemediği ya da ikinci bir arıza noktası ekleyip eklemediğidir. Ayrıca bu modelin dokunamadığı bir hukuki katman vardır: uluslararası sınır anlaşmaları belirli belgeleri ve belirli doğrulama sıralarını öngörür, canlı bir biyometrik denetim bunların arasında değildir. Ve hata biçimi bir giriş geçidindekiyle simetrik değildir: bir kasada yanlış kabul bir satış kaybıdır, bir sınırdaki yanlış kabul ise yayımlanmış hiçbir hata oranının sınırlayamadığı sonuçları olan bir güvenlik olayıdır. **Doğru sonuç şudur: bu durum, modelin ne için olduğunun en iyi gösterimidir ve pilot için en kötü yerdir.**

### 16. İç sınır geçişi ve ortak bir sistemi olmayan iki yargı bölgesi arasındaki sınır

**Durum.** Bir yolcu, ortak kimlik altyapısı olmayan iki ülke arasında geçiyor — bir gümrük birliğinin iki üye devleti arasında bir nakliye şoförü, bir kara sınırından geçen bir komşu, iki ulusal demiryolu ağı arasında geçen bir tren.

**Bugün.** Sabit bir noktada belge kontrolü ya da hiçbir şey. Ortak bir sistem olmadığında her geçiş, "kimlik olarak ne kabul edilir" konusunda iki taraflı bir müzakere ve yolcunun her iki tarafın kabul ettiği şeyi taşımasıdır.

**Bu öneriyle.** Yolcunun cihazı delegasyonu taşır ve geçişi işleten taraf her iki tarafta aynı okuyucu standardıyla aynı kişiyi doğrulayabilir. İki taraflı müzakere yerine ortak bir protokol geçer: iki taraf kabul edilebilir bir belge listesi üzerinde değil, bir standart üzerinde anlaşır. Yolcu bir cihaz ve başka hiçbir şeyle geçer ve hiçbir devlet bir şablon tutmaz.

**Çözmediği şey.** İki taraf yine de *kimin yolcu sayıldığı* konusunda anlaşmak zorundadır — bu vatandaşlık ve ikamet hakkında siyasi ve hukuki bir sorudur ve hiçbir protokol onu çözmez. Öneri teknik el sıkışmayı ucuz ve standart yapar; iki yargı bölgesinin anlaşmasını sağlamaz. Mantıklı bir dağıtım, zaten ortak bir sistemi paylaşan iki işletmeciden başlar ve üçüncüsünü ekler; iki yabancıyla başlamaz.

### 17. Hava yolu ve demiryolu kapuşturma

**Durum.** Bir yolcu uçağa ya da trene biniyor.

**Bugün.** Kapuşturma kartı, barkod, pasaport, kapıda kimlik kontrolü ve bazı havalimanlarında yüz karşılaştırması. Havayolu yolcunun kimlik belgesi numarasını alır ve belirli bir süre saklar.

**Bu öneriyle.** Yolcu taşıyıcıya delegasyon verir, kapı canlı doğrular ve hava yolu belge kopyası yerine doğrulanmış-varlık beyanı alır. Kimlik kontrolü yapmayan bir demiryolu işletmesi için — birçok bölgesel servis yapmaz — delegasyonun bütün noktası budur: kimlik kaydı olmadan doğrulanmış bir yolcu.

**Çözmediği şey.** Havacılık güvenlik düzenleri düzenlemelerle ve uluslararası anlaşmalarla belirlenir; belge kontrolleri, yolcu ad kayıtları ve teröist listesi eşleştirmesi öngörürler. Delegasyonlu varlık denetimi bunların hiçbirinin yerine geçmez ve canlı denetimin güvenlik değeri gizlilik değerinden bağımsızdır. Bu durum kapuşturma için makul bir **ek**tir, ikamesi değil.

### 18. Paylaşımlı ve geçici çalışma alanları, co-working ve kısa süreli ofisler

**Durum.** Bir serbest meslek erken ya da küçük bir ekip, bir günlüğü paylaşılan bir ofiste çalışıyor.

**Bugün.** Bir resepsiyon masası, bir üyelik kartı, bir PIN, bir QR kod, bir telefon araması. Erişim bir kişi tarafından verilir ve işletmeci kimin girdiğine dair kayıt tutar.

**Bu öneriyle.** Ekip, gün boyunca çalışma alanına delegasyon verir. Okuyucu her üyeyi doğrular, çalışma alanı delegasyonun geçerli olduğunu bilir ve işletmeci bir biyometrik yerine bir olay kaydı tutar. Co-working işletmecisi — çoğu zaman güvenlik ekibi olmayan küçük bir işletme — en küçük maruziyete sahip olur.

**Çözmediği şey.** Paylaşımlı bir çalışma alanının asıl riski yabancının içeri alınmasıdır ve delegasyon modeli bunu ele almaz: delegasyonu olan kişileri doğrular, delegasyonu olmayanlar hakkında hiçbir şey söylemez. Resepsiyonun yargısı hâlâ belirleyicidir.

### 19. Acil durum ve sosyal yardım erişimi, ve okuyucu kullanamayan insanlar için erişim

**Durum.** Bir kurtarma ekibi bir binaya girmesi gerekiyor. Bir titreme, maske, engel ya da koyu tenli bir kişi, onu tanımayan bir okuyucunun önünde duruyor.

**Bugün.** Acil durum geçersiz kılma mekanizmaları, ana anahtarlar ve biyometrik girişten muaf tutulan kişilerin listesi. Muaf listeleri nadiren yayımlanır; bu da insanların muafiyetlerini kapının önünde öğrenmesi anlamına gelir.

**Bu öneriyle.** Okuyucunun hata oranları demografik grup ve saldırı sınıfı başına yayımlanır, yedek yol tasarımın bir parçası olur (istisna değil) ve yetki listesi özneye görünür olur. Bir kurtarma ekibi normal denetimi atlayan bir delegasyon taşıyabilir ve okuyucunun yüksek yanlış reddetme oranı olan bir kişi, okuyucuya dayanmayan bir yola kaydedilebilir.

**Çözmediği şey.** Burada hiçbir şey, yüz tanımanın dengesiz performansının altında yatan sorunu çözmez. Onu **ölçülebilir ve dolayısıyla eyleme dönüştürülebilir** kılar; bu gerekli ama yeterli değildir. **Kümenin geri kalanı buna göre okunmalıdır:** biyometrik erişimi daha ucuz ve daha kolay hâle getiren ama ondan geçemeyen insanları adlandırmayan bir öneri, dışlamayı da daha ucuz ve daha kolay hâle getirir.

## Sınır durumları için kaynaklar

Sınır durumları, canlı biyometrik denetimle pasaportsuz geçişe yönelik hiçbir "seamless borders" pilotunun kamuya açık kaydı bulunamadığı için, pilot raporları yerine kamuya açık çerçeve belgelerine dayanır. Kayıtta bulunan şey hukuki ve operasyonel çerçevedir:

| Olgu | Kaynak |
|---|---|
| Schengen alanı iç sınırlarda olağan pasaport sunma gerekmez, ancak polis spot kontrolleri meşrudur; geçici iç kontroller getiren bir üye devlet geçerli bir pasaport ya da kimlik kartı göstermeyi gerektirir | [Your Europe — EU vatandaşları için seyahat belgeleri](https://europa.eu/youreurope/citizens/travel/entry-exit/eu-citizen/index_en.htm) |
| İç kontroller 30 gün, 6 ay, ardından 2 ve 2,5 yıl uzatılabilir; bildirim ile gereklilik ve orantılılık değerlendirmesi gerekir | [Schengen alanı](https://eur-lex.europa.eu/legal-content/EN/TXT/?uri=LEGISSUM:l33020&frontOfficeSuffix=/) · [Komisyon Uygulama Kararı (AB) 2025/315](https://eur-lex.europa.eu/legal-content/EN/TXT/?uri=OJ:L_202500315) |
| AB'nin Giriş/Çıkış Sistemi, Schengen dışındaki uyrukları dış sınırlarda biyometrik olarak kaydeden sistem olarak 10 Nisan 2026'da tam faaliyete geçti | [Smart Borders — Avrupa Komisyonu](https://home-affairs.ec.europa.eu/policies/schengen/smart-borders_en) |

**Son satırın dürüst okunuşu, bu durumun bütün noktasıdır.** Dış sınırdaki eğilim *daha fazla* biyometrik kayıt, daha az değil: eğilim belgeye bir şablon eklemek yönünde ve Giriş/Çıkış Sistemi bunu genişletiyor. Öneri bunun tersini sunuyor — devletin bir yüz tutmadan bir kişiyi doğrulayan bir geçiş — ve bunun olmamış olmasının sebebi, bunu düşünen kimsenin olmaması değil, onu mümkün kılan hukuki araçların mevcut olmamasıdır. **Bu bir standart ve antlaşma boşluğudur, teknoloji boşluğu değildir** ve katalogun geri kalanının tanımladığı sorundan farklı bir sorundur. Burada pasaportsuz bir "seamless borders" pilotunun var olduğu iddiasında bulunulmamaktadır.

## Kümenin gösterdikleri

On dokuz durum boyunca üç örüntü tutuyor:

1. **Gizlilik kazancı, işletmecinin ihtiyacı en küçük olduğu yerde en büyüktür.** Ulaşım turnikeleri ve paylaşımlı çalışma alanları "yetkili" bilgisine ihtiyaç duyar, "kim" bilgisine değil; açıklama yapılmayan kip doğal uyumludur. Otel resepsiyonu ve klinik resepsiyonu tersini ister ve daha az kazanır.
2. **Hukuki zorluk, teknik zorluğun kabaca tersidir.** Bina erişimi teknik olarak kolay, hukuken tartışmalıdır; oy kontrolü teknik olarak kolay, hukuken neredeyse imkânsızdır; sınır kontrolü ne teknik ne hukuki olarak kolaydır.
3. **En sert başarısız olan durumlar, en çok fayda sağlayacak olan durumlardır.** Yukarıdaki her madde, modelin kapsamadığı bir durumu belirtir. Yedeği kaldıran bir dağıtım, yedeği gerekli kılan şeyi de kaldırmış olur.

## İlk uygulama hangisi olmalı

Bunu tez eden değil kuran biri olsaydım, sıra şöyle olurdu:

| Aşama | Durum | Neden bu |
|---|---|---|
| 1 | **5 — bina erişimi**, tek bina, tek işletmeci, tek okuyucu tipi | İşletmeci, kullanıcının kendi ev sahibidir; risk sınırlıdır ve değer — anahtar yok, kart yok, binanın veritabanında şablon yok — en kolay görülen ve anlatılan değerdir |
| 2 | **6 — misafir girişi** | Üçüncü tarafın *telefonuna* delegasyon vermeyi getirir; bu, mekanizmanın asıl sınavıdır, ama henüz para taşınmaz |
| 3 | **4 veya 12 — öznitelik kipi** | İşletmecinin bugünden daha az öğrendiği bir kipi getirir; gizlilik iddiasının en güçlü ve en ölçülebilir olduğu durumdur |
| 4 | **1 — ödeme** | Yukarıdakiler delegasyonun hiç çalıştığını gösterdikten sonra, çünkü bir ödeme başarısızlığı kamuya açık bir başarısızlıktır |

Bilerek ilk sırada değil: ulaşım (3), işyeri (9), oy verme (11) ve sınır durumları (15 ve 16). Her birinin belirli bir çözülmemiş sorunu var — işletmeci ilişkilendirmesi, istihdam onayı, seçimsel meşruiyet ya da sınır geçişinin bir erişim kararı değil bir güvenlik işlevi olması — ve her biri ilk ters gidecek şey olurdu. **Seamless borders durumu ayrıca söylenmelidir: modelin ne için olduğunun en iyi gösterimidir ve pilot için en kötü yerdir.** Bir sınırdaki başarısız pilot, bir güvenlik sisteminin kamuya açık başarısızlığı olurdu ve yayımlanmış hiçbir hata oranı sonuçlarını sınırlamaz.
