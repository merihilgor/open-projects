English: [TECHNICAL-ANALYSIS.md](TECHNICAL-ANALYSIS.md)

# Yapay Zekâ Cevaplarında Yayıncı Geliri: Teknik Analiz (Araştırma Düzeyi)

> **Bağlayıcı olmayan teknik analiz.** [Öneri](README.tr.md) "ne" ve "neden"i anlatır. Bu not, modelin güvenilir çalışması için bir kurulum yolunu gösterir ve yeniden kullanılabilecek mevcut standartları sayar. Bir şartname değil, araştırma düzeyinde bir tasarımdır. Gerçek bir uygulama onu kendi ölçeğine, pazarlarına ve mevzuatına göre uyarlamalıdır. Ticari uygulamalar [ticari lisansa](COMMERCIAL-LICENSE.md) tabidir.

## 1. Kusursuz çalışması gerekenler

Bunlardan biri aksarsa model çöker. Aşağıdaki her bileşen bunlardan birini güvenceye almak için vardır.

| # | Güvence | Önlediği sorun |
|---|---|---|
| G1 | Kaynağın içeriğinin her kullanımı bir kez ve yalnızca bir kez sayılır | Eksik ödeme, çifte ödeme |
| G2 | Her kaynağın cevaptaki payı yayımlanmış ve denetlenebilir kurallarla hesaplanır | İtirazlar, hile |
| G3 | Reklam ve ücretler cevabı ya da kaynak seçimini asla değiştirmez | Satın alınmış cevaplar, güven kaybı |
| G4 | Her reklam hiçbir şüpheye yer bırakmadan reklam olarak işaretlenir | Örtülü reklam, hukuka aykırılık |
| G5 | Sohbet içeriği reklam tarafına asla ulaşmaz | Profilleme, gizlilik ihlali |
| G6 | Ödemeler denetlenmiş sayımlarla eşleşir ve zamanında yapılır | Ödenmeyen yayıncılar |
| G7 | Sahtekârlık (sahte kaynak, sahte trafik, sahte tıklama) tespit edilir ve geri alınır | Paranın sahtekârlara akması |
| G8 | Cevap, reklam veya ücret sistemi yüzünden asla yavaşlamaz | Kötü kullanıcı deneyimi |

## 2. Bileşenler

```
 Yayıncı ──koşullar──▶ Kaynak sicili ◀──doğrulama── Güven etiketi kuruluşları
     ▲                     │
     │ ödemeler            ▼ koşullar, reklam envanteri kimlikleri, güven etiketi
 Takas ve  ◀──olaylar── Yapay zekâ sağlayıcısı ──cevap──▶ Kullanıcı
 ödeme                   │  │                              ▲
     ▲                   │  └─içerik kimlikleri──▶ Reklam seçimi ──etiketli reklam alanı──┘
     └──── denetim ◀── Kullanım defteri (imzalı olaylar)
```

### 2.1 Kaynak sicili ve koşullar
- **Kontrolün ispatıyla kayıt.** Yayıncı, sitelerin bugün yaptığı gibi alan adının kendi kontrolünde olduğunu ispatlar ve ödemeler için bir tüzel kişi kaydeder.
- **Yayıncının kendi sitesinde makine tarafından okunabilir koşullar.** Koşullar şunları belirtir: reklama izin (evet/hayır), çekim başına fiyat, önbellekteki içerik için gösterim başına fiyat, atıf ve bağlantı şartları ve model eğitimine izin verilip verilmediği (ayrı lisans). Yeniden kullanılabilecek mevcut çalışmalar: RSL lisans sözlüğü ve keşif için robots.txt geleneği.
- **Yetkili satıcılar dosyası.** Yayıncı, envanterini yapay zekâ oturumlarına hangi reklam platformlarının satabileceğini listeler. Bu, web'de envanter sahtekârlığıyla zaten mücadele eden IAB Tech Lab ads.txt / sellers.json modelini izler.
- **İmzalı sicil kayıtları.** Varsa güven etiketi, sertifika kuruluşunun imzaladığı ve süresi sınırlı bir onay olarak her kayıtta yer alır.

### 2.2 İçerik kimliği
- Çekilen her içerik bir **içerik kimliği** alır: kanonik URL, çekilen metnin özeti (hash) ve çekim zamanı. Haberin yeni sürümü yeni bir özet alır.
- Pasajlar **bölüm kimlikleri** alır (örneğin paragraf bazında); böylece atıf haber düzeyinin altında da çalışabilir.
- Önbellek içeriği içerik kimliğiyle birlikte saklar. Bu yüzden önbellekten yapılan her gösterim, asıl içerik kimliğini taşıyan bir olay üretir (bkz. 2.6).

### 2.3 Kimliği belli çekim
- Yapay zekâ tarayıcıları ve ajanları kendilerini **imzalı isteklerle** tanıtır (HTTP Message Signatures, [RFC 9421][rfc9421]). Bu, tarama başına ödeme hizmetlerinin kullandığı "Web Bot Auth" önerilerinin de temelidir.
- Yayıncının sunucusu veya içerik dağıtım ağı **imzalı bir çekim makbuzu** döner: içerik kimliği, sağlayıcı, zaman ve fiyat. İki taraf da makbuzu saklar; böylece aynı olayın birbirinden bağımsız iki kaydı olur.

### 2.4 Kullanım defteri
- Olaylar: `retrieval` (çekim), `cached_display` (önbellekten gösterim), `citation_shown` (kaynağın gösterilmesi), `citation_click` (kaynağa tıklama), `ad_impression` (reklam gösterimi), `ad_click` (reklama tıklama).
- Her olayın benzersiz bir olay kimliği (tekrarı önlemek için), içerik kimliği, oturum türü (ücretsiz / ücretli / API / ajan), zamanı ve sağlayıcı imzası vardır. Kullanıcı kimliği veya sohbet metni asla içermez.
- Defter **yalnızca eklemeye açık ve değiştirilemez**dir; örneğin kökleri yayımlanan, zincirleme özetli günlük paketler. Böylece denetçiler sonradan hiçbir şeyin silinmediğini ispatlayabilir (G1, G6).
- **Mutabakat:** Sağlayıcı defterleri her gün yayıncı tarafındaki makbuzlar ve içerik dağıtım ağı kayıtlarıyla karşılaştırılır. Eşiği aşan uyumsuzluklar otomatik olarak itiraz açar.

### 2.5 Atıf (G2)
- **Birim:** Üretilen cevap iddialara veya cümlelere ayrılır. Her iddia, onu destekleyen kaynak bölümlerine bağlanır. Kullanıcıya gösterilen kaynaklar da aynı bağlantılardan gelir.
- **Ağırlık:** Bir kaynağın payı = desteklediği iddialar ÷ cevaptaki desteklenen tüm iddialar. Birden fazla kaynağın desteklediği iddialar eşit bölünür. Hiçbir kaynağın desteklemediği iddialar (genel bilgi) hesaba katılmaz.
- **Yöntemler**, en ucuzdan en güçlüye: alıntıya dayalı (modelin gösterdiği kaynaklar), cevap ile çekilen bölümler arasında pasaj benzerliği eşleştirmesi, ve örneklem üzerinde birini çıkar testi (bir kaynak olmadan cevabı yeniden üretip farkı ölçmek). Sonuçların denetimde tekrarlanabilir olması için standart **yöntemi ve sürümünü sabitlemelidir**.
- **Açıkça belirtilen sınırlar:** Üretilen metnin atfı yaklaşıktır. Bu yüzden mükemmel bir ölçüm iddia etmek yerine kurallar yayımlanır, örneklem denetimi yapılır ve itiraz hakkı tanınır.
- **Hileye karşı:** Aynı veya neredeyse aynı içerikler (kopya yayınlar) gruplanır ve aslına ödenir. Anahtar kelime doldurulmuş sayfalar fazladan kazanmaz, çünkü ağırlık anılma sayısından değil desteklenen iddialardan gelir.

### 2.6 Ücretsiz oturumlar: reklam seçimi ve gösterimi (G3, G4, G5, G8)
- **İşlem sırası:** Önce cevap üretilir ve sabitlenir. Ancak ondan sonra cevabın kullandığı içerik kimliklerinin listesi reklam seçimine iletilir. Reklam tarafı sohbeti, cevap tarafı reklamları hiç görmez.
- **Reklamlar modelin bağlamına asla girmez.** Reklam görseli istemci tarafından ayrı bir alanda gösterilir. Bu, reklamın cevabı etkilemesini engeller ve reklam metnine gizlenmiş komut enjeksiyonunu (prompt injection) imkânsız kılar.
- **Seçim:** Adaylar, kaynak gösterilen içeriklerin yetkili satıcılarının o içerik için sunduğu reklamlardır. Bunlar reklamveren onayı, marka güvenliği kategorisi (IAB Tech Lab içerik taksonomisi) ve hassas konu engelleriyle süzülür. Reklam, uygun reklamlar arasında bir açık artırmayla seçilir; teklif isteği yalnızca içerik kategorilerini ve kaba bağlamı içerir. Bunun için OpenRTB protokolü genişletilebilir.
- **Gecikme:** Reklam seçimi cevap akışıyla paralel çalışır ve kesin bir süre sınırı vardır. Geç kalırsa reklam gösterilmez; cevap asla bekletilmez (G8).
- **Sıklık sınırları:** Cevap başına (örneğin tek reklam alanı), oturum başına ve gün başına; istemci uygular, defterde kontrol edilir.
- **Gelir paylaşımı:** Açık artırma fiyatından platform ücreti düşüldükten sonra kalan tutar, yapay zekâ sağlayıcısı ile kaynak gösterilen kaynaklar arasında atıf ağırlıklarına göre bölünür. Paylaşım oranları standardın parçasıdır ve yayımlanır.

### 2.7 Reklamın işaretlenmesi (G4)
Bir yapay zekâ oturumunda gösterilen her reklam:
- kullanıcının dilinde sabit ve görünür bir etiket taşır ("Reklam", İngilizcede "Ad" / "Sponsored");
- reklamverenin adını ve gerektiğinde reklamın parasını kimin ödediğini gösterir;
- cevaptan görsel olarak ayrılır (kendi çerçevesi, farklı arka plan) ve asla cevap metninin veya kaynak gösterimlerinin içinde yer almaz;
- sesli modda okunmadan önce reklam olduğu duyurulur; farklı bir sesle ya da açık bir sesli işaretten sonra okunur;
- ajan modunda asla bir eyleme dönüştürülmez (ajan bir reklam yüzünden satın alma, rezervasyon veya tıklama yapmaz);
- "Bu reklamı neden görüyorum?" bilgisini sunar (eşleştirildiği içerik).

Bu, mevcut bildirim kurallarını izler: AB Dijital Hizmetler Yasası, platformların her reklam için reklam olduğunu, kimin adına gösterildiğini, parasını kimin ödediğini ve başlıca hedefleme parametrelerini göstermesini ister (2022/2065 sayılı AB Tüzüğü, md. 26, [EUR-Lex][dsa]). Türk tüketici hukuku örtülü reklamı yasaklar (6502 sayılı Kanun, md. 61/4, [Mevzuat][tr6502]). ABD'de FTC, sıradan içerik gibi biçimlendirilmiş reklamları aldatıcı sayar ([FTC][ftc]).

### 2.8 Ücretli, API ve ajan oturumları: kullanım başına ücret
- **Çekim başına:** İçerik canlı çekildiğinde, yayıncı koşullarındaki fiyat ödenir.
- **Önbellekten gösterim başına:** Cevap önbellekteki içeriği kullandığında bunun yerine gösterim başına fiyat uygulanır. Önbellek ödemeden kaçmak için kullanılamaz, çünkü her gösterim asıl içerik kimliğini taşıyan bir `cached_display` olayı üretir.
- **Fiyat koruması:** Yayıncılar fiyatlarını kendileri belirler. Standart bir varsayılan fiyat ve sağlayıcı-haber başına aylık bir üst sınır tanımlayabilir; böylece tek bir viral haber ödenemez hâle gelmez ve küçük yapay zekâ sağlayıcıları ayakta kalır.
- **Ön kontrol:** Sağlayıcı çekimden önce sicilden koşulları kontrol eder (kısa süre önbellekte tutulur). Fiyat kendi sınırının üzerindeyse içeriği kullanmaz ve kaynak olarak göstermez.

### 2.9 Ölçüm ve sahtekârlık (G7)
- **Gösterim tanımı:** Bir reklam ancak ekranda gösterilmiş ve görünür olmuşsa sayılır; sektörün görünürlük standartlarına (Media Rating Council kılavuzları) uyulur. Bir tıklama ancak geçerli, insan kaynaklı ve tekilleştirilmiş ise sayılır.
- **Sahtekârlık kontrolleri:** doğrulanmış yayıncı kimliği ve ödeme hesapları; yetkili satıcı listeleri; imzalı sağlayıcı olayları; anomali tespiti (ani sıçramalar, imkânsız oranlar, veri merkezi trafiği); geçersiz trafik filtreleme; yeni yayıncıların kazancının bir kısmının inceleme süresince bekletilmesi; sahte ödemelerin geri alınması.
- **Bağımsız denetim:** Akredite bir denetçi defterleri, atıf örneklemlerini, tarafsızlık testlerini ve ödemeleri kontrol eder ve her yıl bir özet yayımlar.

### 2.10 Tarafsızlık testi (G3)
- **Tasarımdan ayrım:** Kaynak seçimi ve cevap üretimi reklam veya fiyat verisine erişemez. Ücretli ve ücretsiz oturumlar aynı kaynak seçim kurallarını kullanır.
- **Tekrar oynatma denetimi:** Denetçi bir sorgu örneklemini reklam ve ücret sistemi kapalıyken yeniden çalıştırır, kaynakları ve cevapları karşılaştırır. Sistematik farklar ihlal sayılır.
- **Fiyata körlük kontrolü:** Kullanım fiyatı yüksek kaynaklar, sağlayıcı onları hiç kullanmadığını beyan etmedikçe, ilgililiklerinin gerektirdiğinden daha az kaynak gösterilmemelidir. Daha ucuz olmak "daha çok alıntılanmak" anlamına gelmemelidir.

### 2.11 Takas ve ödeme (G6)
- Her yayıncı için tüm sağlayıcılar genelinde aylık mahsup: reklam payları + kullanım ücretleri − geri alımlar.
- İçerik kimliği ve oturum türüne göre olay sayılarını gösteren, yayıncının indirebildiği hesap dökümleri.
- Devreden bakiyeli asgari ödeme eşiği; böylece küçük yayıncılar küçük tutarları kaybetmez, eşiğe ulaştıklarında ödeme alır.
- İtirazlar: mutabakat uyumsuzlukları için otomatik, atıf için manuel; bağımsız bir kurula itiraz hakkıyla.

### 2.12 Gizlilik (G5)
- Reklam istekleri yalnızca içerik kimliklerini, içerik kategorilerini, dili, ülkeyi ve oturum türünü içerir. Sohbet metnini, kullanıcı kimliğini veya profili asla içermez.
- Raporlar toplu hâldedir. Tek tek kullanıcıların çıkarılamaması için küçük sayılar gizlenir veya gürültü eklenir.
- Veri koruma mevzuatı uygulanır: AB'de GDPR, Türkiye'de KVKK. Hassas verilere dayalı reklam profillemesi tamamen dışarıda tutulur.

### 2.13 Güven etiketleri
- Sertifika kuruluşları, sicile kaydedilen imzalı ve süresi sınırlı onaylar verir. Journalism Trust Initiative (CWA 17493) gibi mevcut süreç standartları temel alınabilir.
- Etiket, kaynak gösteriminin yanında, sertifikaya ve geçerlilik tarihine bağlantıyla gösterilir.
- Geri alma ve süre dolması, sabit bir süre içinde sağlayıcılara yansır.

## 3. İşletim modeli: sağlayıcı reklam ağları mı, tek ortak platform mu

Aynı standardı işletmenin iki yolu:

| | **A. Her yapay zekâ sağlayıcısı kendi reklam ağını işletir** ("Gemini Ads", "ChatGPT Ads", "Claude Ads"…) | **B. Yapay zekâ sağlayıcılarının ve yayıncıların üye olduğu tek, tarafsız ortak platform** |
|---|---|---|
| Reklamverenler | Her ağda ayrı ayrı satın almak zorunda | Bir kez satın alır, tüm üye sağlayıcılara ulaşır |
| Yayıncılar | Her sağlayıcıyla ayrı kayıt ve mutabakat | Tek kayıt, tek döküm, tek ödeme |
| Küçük yapay zekâ sağlayıcıları | Kendi reklam satışını kurmakta zorlanır | Katılır ve ilk günden kazanır |
| Rekabet | Güçlü sağlayıcılar baskın; kendini kayırma riski | Tarafsız; sağlayıcılar cevap kalitesiyle rekabet eder |
| Tarafsızlık ve denetim | Ağ başına ayrı denetçi; sonuçları karşılaştırmak zor | Tek denetim çerçevesi ve tek defter standardı |
| Başlama hızı | Zaten reklam sistemi işleten büyük sağlayıcılar için hızlı | Daha yavaş; yönetişim ve üye gerektirir |
| Risk | Parçalanma; küçük yayıncıların dışarıda kalması | Tek hata noktası veya ele geçirilme; rekabet hukuku incelemesi |

**Öneri: her iki modele açık, tarafsız bir takas katmanı olan ortak standart.**
- **Tek açık standart:** koşulları, içerik kimliklerini, olay şemasını, atıf kurallarını, işaretlemeyi ve denetimi kapsar; iki modelde de zorunludur.
- **Tek tarafsız takas ve sicil:** Yayıncılar, yapay zekâ sağlayıcıları, reklamverenler ve sivil toplum tarafından yönetilir ve herkes tarafından ortak kullanılır. Yayıncı sicilini tutar, defterleri mutabık kılar ve yayıncılara öder.
- **Reklam satışı rekabete açık olabilir.** Bir sağlayıcı kendi reklam ağını işletebilir (model A), ortak bir borsaya katılabilir (model B) ya da ikisini birden yapabilir. Her durumda aynı defter standardına raporlar ve aynı takas üzerinden öder. Böylece yayıncılar ve reklamverenler tek bir sistemle muhatap olur, sağlayıcılar ise rekabet alanını korur.
- **Ele geçirilmeye karşı güvenceler:** Takas kuruluşu kâr amacı gütmeyen veya kamu hizmeti niteliğindedir, bağımsız bir yönetim kurulu vardır, ücretlerini yayımlar ve yayımlanmış ölçütleri karşılayan her sağlayıcıyı ve yayıncıyı kabul etmek zorundadır.

## 4. Hata durumları

| Hata | Tespit | Yanıt |
|---|---|---|
| Sağlayıcının kullanımları eksik bildirmesi | Yayıncı makbuzları ve içerik dağıtım ağı kayıtlarıyla günlük mutabakat | Otomatik itiraz, faiziyle geriye dönük ödeme, denetime taşıma |
| Ücretlerden kaçmak için önbellek kullanımı | Kaynak gösterilirken `cached_display` olaylarının eksik olması | Standardın ihlali, geriye dönük ödeme |
| Çifte sayım | Tekrarlanan olay kimlikleri | Tekrara dayanıklı defter tekrarları atar |
| Reklamın cevabın içinde veya etiketsiz gösterilmesi | İstemci uygunluk testleri ve denetim ekran görüntüleri | Reklam gelirinin tutulması, kamuya duyuru |
| Reklam metninin modeli yönlendirmeye çalışması | Reklamlar modelin bağlamına hiç girmez | Tasarım gereği mümkün değil |
| İçerik çiftliklerinin atfı manipüle etmesi | Kopya gruplama, iddiaya dayalı ağırlıklar | Fazladan kazanç yok; tekrarlanan kötüye kullanımda kayıt silinir |
| Tıklama sahtekârlığı | Geçersiz trafik filtreleri, anomali tespiti | Tıklamaların iptali, kazancın geri alınması |
| Güven etiketinin geri alınması | Sicil güncellemesi | Etiketin sabit süre içinde cevaplardan kaldırılması |
| Reklam veya ücret sisteminin çökmesi | Sağlık kontrolleri | Cevap reklamsız sunulur; ücretler kaydedilip sonra ödenir |

## 5. Aşamalı kurulum

1. **Standart taslağı:** koşul sözlüğü, içerik kimliği, olay şeması, işaretleme kuralları, atıf kuralları v1 (alıntıya dayalı).
2. **Pilot:** bir veya iki ülkede haber yayıncıları ve iki-üç yapay zekâ sağlayıcısı; kullanım ücretleri ve etiketli reklamlar; günlük mutabakat; bir bağımsız denetçi.
3. **Atıf v2:** pasaj düzeyinde eşleştirme ve örneklemli birini çıkar denetimleri.
4. **Ortak takas:** tüm sağlayıcılara ve yayıncılara açık tarafsız sicil ve takas.
5. **Güven etiketleri ve daha geniş içerik:** sertifika onayları; araştırma ve başvuru kaynağı yayıncıları.

## 6. Yeniden kullanılabilecek mevcut standartlar ve çalışmalar

| İhtiyaç | Mevcut çalışma |
|---|---|
| Makine tarafından okunabilir lisans koşulları | RSL (Really Simple Licensing); robots.txt gelenekleri |
| Yetkili satıcılar, sahtekârlıkla mücadele | IAB Tech Lab ads.txt ve sellers.json |
| Reklam açık artırması ve teklif istekleri | IAB Tech Lab OpenRTB |
| İçerik ve marka güvenliği kategorileri | IAB Tech Lab içerik taksonomisi |
| Tarayıcı kimliği | HTTP Message Signatures (RFC 9421), Web Bot Auth önerileri |
| Görünürlük ve ölçüm | Media Rating Council kılavuzları |
| Reklam bildirimi | AB DSA md. 26; 6502 sayılı Kanun md. 61/4; FTC'nin aldatıcı biçimli reklamlar politikası |
| Güven sertifikası | Journalism Trust Initiative (CWA 17493) |
| Gizlilik | GDPR, KVKK |

[rfc9421]: https://www.rfc-editor.org/rfc/rfc9421
[dsa]: https://eur-lex.europa.eu/eli/reg/2022/2065/oj
[tr6502]: https://www.mevzuat.gov.tr/mevzuat?MevzuatNo=6502&MevzuatTur=1&MevzuatTertip=5
[ftc]: https://www.ftc.gov/legal-library/browse/commission-enforcement-policy-statement-deceptively-formatted-advertisements
