English: [STANDARDIZATION.md](STANDARDIZATION.md)

# Delegeli Biyometrik Kimlik Doğrulama: Bir Açık Standarda Giden Yol (Taslak)

> **Çalışma taslağı.** Bu not, [öneriyi](README.tr.md) resmî standardizasyona hazırlar. Üç bölümden oluşur: her gereksinimi en yakın mevcut standarda eşleyen dürüst bir **boşluk analizi**, eksik parça için bir **öneri** ve standartlaşma kuruluşuna başvurmak için bir **yol**. Bir atölyenin yazacağı normatif metin ayrıca [teknik şartname](SPECIFICATION.tr.md) olarak taslaklandırılmıştır ve bu notun gerekçesidir. Başlangıç noktasıdır, bitmiş bir şartname değildir. Standart kuruluşlarının usul, form ve ücretleri değişir; başvurmadan önce başvuracağınız kuruluşla bunları kontrol edin.

## Bu notun cevapladığı soru

Yazar, bu projeye standart önerisi **yalnızca mevcut standartlar projeyi zaten kapsamıyorsa** eklemeyi istedi ve bir öneri dosunması hâlinde post-kuantum güvenliği de içermesini istedi.

**Kapsamıyorlar** — standart kuruluşlarının kamuya açık kayıtları, çalışma taslakları ve yayımlanmış belgeleri itibarıyla. Boşluk gerçek ve "biyometriler standartlaştırılmamıştır" dediğimizden daha dar ve daha ilginç: standartlar bunu, çok eksiksiz biçimde yapıyorlar. Üç mevcut standart bu problemin üçte birini birer ölçüde kapsıyor ve hiçbiri aralarındaki dikişi kapsamıyor. Bu not dikişi tam olarak tanımlar, çünkü boşluğu yanlış tanımlayan bir öneri, hiç öneri olmamasından kötüdür. (Bu iddianın gücüne dair uyarı ve tamamen kamuya açık olmayan tek standart kataloğu hakkında not, Bölüm 7'nin kapanış notundadır.)

Projeyi şekillendiren ikinci bir bulgu var: **bu projenin ihtiyaç duyduğu mekanizma zaten var ve patentli.** Dolayısıyla öneri "bunu kimse düşünmedi" değil; "fikir eski, mekanizma tek bir şirketin portföyünde kilitli ve ekosistemin ücretsiz, standartlaştırılmış, birlikte çalışabilir bir yola ihtiyacı var."

## Bölüm 1: Boşluk analizi

### 1.1 Gereksinimler, tek tek

Proje, bir okuyucunun (kasa, geçit) bir kişiyi yüz yüzünden, telefon yokken, biyometrik şablon telefondan hiç çıkmadan, okuyucu başına kapsamlı ve geri alınabilir bir yetkiyle doğrulamasını, okuyucunun da geriye dönük kullanılabilir bir tanımlayıcı bırakmamasını gerektirir.

| # | Gereksinim | Bugün kapsanıyor mu? | En yakın mevcut standart | Nerede kırılıyor |
|---|---|---|---|---|
| R1 | **Şablon cihazdan hiç çıkmaz** | **Evet, tam olarak** | FIDO2 yerel kullanıcı doğrulama; ISO/IEC 27553-1:2022 (yerel kipler), biyometrik verinin ve türetilmiş verinin doğrulama sonucu hariç cihazdan çıkmamasını şart koşar; ISO/IEC 24745:2022 (geri döndürülemezlik, izlenemezlik, yenilenebilirlik) | Hiçbir yerde. Bu, bütün önerinin üzerine kurulduğu ve *zaten* standartlaştırılmış tek gereksinimdir. |
| R2 | **Okuyucuda canlılık** | **Evet, büyük ölçüde** | ISO/IEC 30107-1:2023 (çerçeve), -2:2017 (veri biçimleri), -3:2023 (test ve raporlama), -4:2024 (mobil cihaz test profili); NIST SP 800-63-4 §3.11 | Hiçbir yerde. Standart puanlanmış ve sertifikalıdır. Bu öneri onu yeniden icat etmemeli, ona atıf yapmalıdır. |
| R3 | **Tek doğrulayana bağlı, kapsamlı, iptal edilebilir kimlik bilgisi** | **Kısmen** | W3C Verifiable Credentials 2.0 (Öneri, 15 Mayıs 2025); AB Kimlik Cüzdanı / eIDAS 2.0 | VC kimlik belgeleri durum listeleriyle iptal edilebilir, ama **tutanın sunmasını varsayar**. Telefonun yokluğu durumunda tutan da yoktur. |
| R4 | **Telefon tamamen yok** | **Hayır — kesin kırılma** | FIDO cihazlar arası (hibrit), CTAP 2.3; ISO/IEC 18013-5 mobil belgeler; AB Kimlik Cüzdanı | Bunların hepsi telefonun kimlik doğrulayıcı olarak hazır bulunmasını gerektirir. Telefon cebinde 200 metre ötedeyken "okuyucu X'te bir insanın fiziksel olarak hazır bulunduğunu" doğrulayabilen hiçbir şey yoktur. |
| R5 | **Okuyucu bir web güvenen tarafı değil** | **Hayır — kesin kırılma** | FIDO2 / WebAuthn Level 3 (W3C Öneri, 25 Ağustos 2026) | FIDO2 mimari olarak origin'a bağlıdır: bir kayıt ve meydan okuma töreni, bir RP ID, bir web origin'i. Bir satış terminali ya da bir kapı bir origin değildir ve "gözetimsiz, sık sık çevrimdışı bir okuyucu güvenen taraf olarak" diye bir FIDO profili yoktur. |
| R6 | **İptal, kullanıcıyla çevrimiçi olmayan okuyuculara ulaşır** | **Hayır — kesin kırılma** | FIDO Metadata Service 3.1.1 (Önerilen Standart, 5 Ocak 2026); VC Bitstring Status List 1.0 | MDS ağdan çekilen bir blob'tur ve "rogue listesi" **cihaz başına**dır, ECDAA anahtarı gerektirir — kullanıcı başına, okuyucu başına bir yetki için işe yaramaz. Durum listeleri de ağdan çekilir. Hiçbiri aralıklı bağlantılı üçüncü taraf okuyucu için sınırlı yayılım anlatısı değildir. |
| R7 | **Yetkinin kendisi birinci sınıf, iptal edilebilir bir nesne olması** | **Hayır — kesin kırılma** | ISO/IEC 24745:2022 uygulama modelleri A–K, 2022'de eklenen dağıtık modeller I, J ve K dâhil; ISO/IEC 30108-1:2026 (Kimlik Nitelikleri Doğrulama Hizmetleri) | 24745 bir **çerçeve ve gereksinim** standardıdır. Çözümü sınıflandırır; onu şartnamelendirmez. Kablosuz biçimi, onay-yetkisi nesnesi, okuyucu doğrulama yordamı ve iptal yayılımı yoktur. ISO/IEC 30108-1:2026 konu bakımından yakındır ama kapsamı biyometrik işlevselliği açıkça uzak arka uç hizmetlerine sınırlar ve **kimlik doğrulama protokollerine entegrasyonu kapsam dışı bırakır** — yani yakın bir isabet, ama bir ev değil. |
| R8 | **Okuyucular arası izlenemezlik** | **Büyük ölçüde, tek bir istisna dâhil** | ISO/IEC 24745:2022 §7.2 ve uygulama modelleri; izlenemez belirteçler için IETF Privacy Pass (RFC 9576 mimari, RFC 9577 HTTP kimlik doğrulama şeması, RFC 9578 ihra protokolleri) | Kimlik belgeleri ve belirteçler için kapsanıyor. Ancak Privacy Pass çalışma grubu kendi izlenemezlik temelini **post-kuantum-izlenemez değil** buldu — bkz. Bölüm 4.3. |
| R9 | **Çevrimdışı çalışma** | **Kısmen** | ISO/IEC 18013-5, ihra eden tarafın imzaladığı sertifikalara karşı çevrimdışı doğrulamayı destekler; Privacy Pass belirteçleri çevrimdışı kullanılabilir | FIDO2 istisnadır — her zaman çevrimiçidir. Çevrimdışı çalışma kapılar için ilginç olan durumdur ve R6'nın en sert bastığı yerdir. |

### 1.2 Dikiş, tam olarak söylenmiş hâliyle

> **Üç mevcut standart bu problemin üçte birini birer ölçüde kapsıyor ve hiçbiri aralarındaki dikişi kapsıyor.**
>
> **FIDO2**, yerel doğrulamayla birlikte cihaza bağlı varlık sunar — şablon gerçekten cihazdan hiç çıkmaz — ama cihazın hazır bulunmasını ve doğrulayıcının bir web güvenen tarafı olmasını gerektirir.
>
> **ISO/IEC 30107, 24745 ve 27553**, sunum saldırısı tespitini, şablon korumayı, geri döndürülemezliği, izlenemezliği ve cihaz üzerinde işlemeyi verir — ama *delegasyon* hakkında hiçbir şey söylemezler. Çözümü sınıflandırırlar; şartnamelendirmezler.
>
> **Doğrulanabilir Kimlik Belgeleri**, kapsamlı, iptal edilebilir, gizliliği koruyan kimlik belgeleri verir — ama tutanın sunmasını varsayar, ve tutan tam olarak yok olan şeydir.
>
> **Dikiş şudur: telefonun yokluğunda, üçüncü taraf bir okuyucuya, bir kişi hakkında ileri sürülen, kapsamlı ve iptal edilebilir bir yetki.**

Daraltılmış hâlinde, gerçekten çözülmemiş üç alt boşluk:

1. **G1 — Yakınlık olmadan varlık.** Okuyucudaki yerel canlılık denetimi *burada canlı bir yüz* olduğunu kanıtlar. *Delegasyonu veren kişinin* burada olduğunu kanıtlamaz. Mevcut her standart birincisiyle yanıt verir ve cihaza bağlı bir anahtarın ikincisini yanıtladığını varsayar. Delegasyon bu varsayımı kırar, çünkü anahtar orada değildir.
2. **G2 — Birinci sınıf nesne olarak yetki.** *Hazır bulunmayan* bir cihaz tarafından, bir kişi hakkında, *ekosistemin dışındaki* bir okuyucuya ileri sürülen ve sınırlı bir zaman penceresinde iptal edilebilen bir yetki, mevcut hiçbir standardın tanımladığı bir nesnedir.
3. **G3 — Keyfî okuyuculara sınırlı iptal.** İptal, aralıklı bağlantılı bir üçüncü taraf okuyucuya, yalnızca takma adlı bir yetki tanımlayıcısını bilen bir okuyucuya, ve "kim kime neyi devretti" merkezî bir veritabanı oluşturmadan ulaşmalıdır — ki bu, projenin var olma nedenini ortadan kaldırırdı.

### 1.3 Standart nereye inmeli, ve neden yeni bir tane değil

Öneri **yeni bir kimlik belgesi biçimi icat etmemelidir**. W3C gereken dikişi doldurmak için gereken zemini zaten kurmuştur:

- Doğrulanabilir Kimlik Belgeleri 2.0 ailesi (Öneri, 15 Mayıs 2025), `credentialSubject` içinde `confidenceMethod` ve `assuranceLevel` tanımlar.
- **VC Confidence Methods** çalışma taslağı (10 Eylül 2026), **hiçbir biyometrik verinin cihazdan çıkmadığı istemci tarafı sıfır bilgi eşleştirme akışına** sahip bir `BiometricVectorConfidenceMethod` ile birlikte bir `BiometricMatchingModel` tanımlar — ve Fransa'daki CLR Labs ile sıfır bilgi biyometrik eşleştirme üzerine işbirliği yürütmektedir.
- Aynı çalışma grubu, devlet kurumları için kafa karıştırıcı bulduğu **"holder binding" terimini bilinçli olarak geri çekmekte** ve "confidence method" terimine geçmektedir; grubun kendi tartışması **delegasyon durumlarını** açıkça kapsamaktadır.
- `assuranceLevel` bir Feature at Risk'tir ve çeşitli bölümler hâlâ TBD işaretlidir — **doldurulacak boşluk tam olarak budur.**

**Dolayısıyla öneri şudur: VC 2.0 için bir `DelegatedPresence` confidence method ve talep tipi, artı bir okuyucu, sunum saldırısı ve iptal yayılımı profili.** Yeni bir veri modeli değil — eksik bir confidence method ve profili.

Zarif bir şekilde ele alınması gereken bir sözlük notu: OpenID4VP 1.0 (kesin) hâlâ "Holder Binding or Key Binding" ifadesini, biyometri tabanlı bir varyant dâhil, kullanmaktadır; W3C ise "confidence method"a geçmiştir. W3C ve OpenID şu anda aynı dikiş için farklı kelimeler kullanıyor. Bir öneri bunları birbirine bağlamalı, taraf seçmemelidir.

### 1.4 Yakın isabetler ve teslim edilmesi gereken bir öncül

Bir standart gözden geçiricisi, yaklaşan üç çalışmayı gündeme getirecek ve bunları ele almayan bir öneri elenecek. Üçünü de ele almalı.

**ISO/IEC 30108-1:2026 — Kimlik Nitelikleri Doğrulama Hizmetleri (eski BIAS serisi).** ISO/IEC JTC 1/SC 37'de 12 Ocak 2026'da yayımlandı; bir hizmet tabanlı çerçeve üzerinden çağrılan kimlik güvence biyometrik hizmetlerini tanımlıyor. Konu bakımından "delegasyonlu varlık doğrulaması"na en yakın ISO belgesidir. Ama kapsamı **biyometrik işlevselliği uzak arka uç hizmetlerine sınırlar ve kimlik doğrulama protokollerine entegrasyonu kapsam dışı bırakır** — ve ekosistem dışı bir okuyucuya delegasyonlu kontrol tam olarak bir kimlik doğrulama protokolüne entegrasyondur. Yakın bir isabet, ama bir ev değil; birlikte çalışılacak ve atıf yapılacak bir belge. İki SC 37 kalemi daha adlandırılmalı: **ISO/IEC 25049-1/-2 (Özelleştirilebilir biyometrik web hizmetleri)** ve **ISO/IEC 26655 (Biyometrik deepfake saldırı tespiti — test ve raporlama)**; sonuncusu önemlidir, çünkü üretken saldırılar artık okuyucunun tehdit modelinin parçasıdır.

**FIDO'nun kendi delegasyon kavramı — iddia edilecek değil, teslim edilecek bir öncül.** FIDO, Mart 2021'de *"FIDO for SCA Delegation to Merchants or Wallet Providers"* başlıklı belgeyi yayımladı; güçlü müşteri kimlik doğrulamasının bir üreticiye ya da cüzdana devredilmesine dair kesin bir hukuki ve teknik çerçeve verdi, EBA'nın EBA/OP/2020/10 Görüşü'ne dayandı ve **dinamik bağlama** şartı koydu, böylece delegasyon işleme bağlanır. **Bir öneri, bu alanda "delegasyon" kelimesini icat ettiğini iddia etmemelidir.** FIDO'nun devrettiği şey *işlem yetkisi*ydi, *varlık* değil; ve telefon yine de hazır bulunuyordu. Devralınan üç şey kullanışlıdır: kavram zaten vardır, delegasyon dinamik bağlama gerektirir (dolayısıyla bir yetki okuyucuya, amaca ve tutara bağlanmalıdır) ve taşıma için `FIDO:/` URI şeması kullanılır.

**ZCAP — yetki yarısı zaten standartlaştırılmış ve öneri onun üzerine inşa edilmelidir.** W3C Topluluk Grubu'nun **ZCAP** (JSON Web Token Proof-of-Possession Authorization Capabilities) şartnamesi, taslak v0.4.0'da, *delegeli, kapsamlı, daraltılabilir, süreli ve çevrimdışı doğrulanabilir yetki* için dünyadaki en eksiksiz mekanizmadır: yetenek zincirleri, `parentCapability` bağları, daraltma, zorunlu son kullanma tarihi. **Tamamen biyometrik dışıdır** — canlılık, varlık ya da şablon yoktur. Ama bu, bu önerinin ihtiyaç duyduğu *yetki* yarısının tam olarak kendisidir ve yetkisini sıfırdan tanımlayan bir öneriye "delegasyon zincirlerini yeniden icat ettiniz" denir. **Yetki, ZCAP anlamında bir yetenek olarak tanımlanmalıdır**; bu, çok miktarda şartname işini ortadan kaldırır ve öneriyi zayıflatmak yerine güçlendirir.

**Bir gözden geçiricinin daha ileri sürebileceği üç çalışma daha.** **IEEE 2410-2021 (Biometric Open Protocol)** gerçek bir yakın isbettir: istemci biyometriği tutar, sunucu homomorfik olarak şifrelenmiş bir yük alır ve düz metin sunucuya hiç ulaşmaz. **ITU-T X.1089** (Telebiometrics authentication infrastructure, 2008) ve **ITU-T X.1083 / ISO/IEC 24708** (BioAPI interworking), bir gözden geçiricinin bütünlük için atıf yapabileceği eski uzak-biyometrik-hizmet standartlarıdır. **BSI TR-03121-2 / TR-03122-2**, ISO/IEC 25049'in genelleştirdiği Alman kamu sektörü biyometrik hizmet mimarisidir.

**Aynı kelimenin farklı bir nesne için kullanıldığı iki örnek daha**, yeniden icat suçlamasına karşı adlandırılmalı: **EMVCo'nun ajan tabanlı ödeme çerçevesi** "tüketici tarafından yetkilendirilen amaç ve delegasyonlu yetki" kaydeder — *ödeme amacının* delegasyonu; ve **FIDO delegasyonlu cihazlar arası akış** — *imzalamanın* delegasyonu. Bu önerinin ihtiyaç duyduğu nesne hepsinden farklıdır: *bir biyometrik varlık kontrolünün, cihaz olmayan bir okuyucuya* delegasyonu.

### 1.5 Gereksini zaten ISO onaylı — ve delegasyonda açıkça sessiz

**ISO/IEC 24745:2022** hakkında öneriyi belirgin biçimde güçlendiren iki bulgu var.

**Birincisi, teknoloji yeni değil.** Ek C, *"Biyometrik bilgi koruma için teknoloji örnekleri"* ve Tablo C.1, **bulanık kasayı** yardımcı veriyle bir takma ad oluşturmanın somut bir yolu olarak zaten listeliyor — terim 3.35'te "yardımcı veri yaklaşımı, bulanık taahhüt şeması ya da bulanık kasa" biçiminde anlatılıyor. Bulanık kasa üzerine kurulu bir delegasyonlu varlık denetimi, dolayısıyla **mevcut bir ISO tarafından onaylanmış bir teknoloji örneğinin** üzerine kurulmuş olur; yeni bir mekanizma icat etmez. Bu, bir öneri ile bir araştırma programı arasındaki farktır.

**İkincisi, 24745 delegasyondan hiç söz etmiyor.** Bu kelime belgede hiç geçmiyor. Bölüm 8'deki A–K uygulama modelleri katı biçimde *saklama ile karşılaştırma konumu* modelleridir — biyometrik referansın nerede saklandığı ve karşılaştırmanın nerede yapıldığı — ve 2022'de eklenen üç model (I, J ve K) **dağıtık karşılaştırma** modelleridir: karşılaştırma, saklanan referansla aynı yerde değildir. Bu, bu önerinin ihtiyaç duyduğu biçime yakındır ve "referansın kontrolün yapıldığı yerde olmadığı" durumun herhangi bir standarttaki en yakın karşılığıdır.

**Sonuç, "24745 yardımcı olmuyor"dan daha keskindir.** ISO/IEC 24745 gereksini sağlar, mekanizmayı bir teknoloji örneği olarak onaylar ve dağıtık karşılaştırmayı modeller — ve *delegasyon sorusunda* açıkça sessizdir. Bu boşluğu dolduran bir standart, 24745 ile rekabet etmez, onu genişletir.

### 1.6 Negatif sonuç: bugün alanda bunu yapan hiçbir şey yok

Boşluk, kapsamlı bir kontrolden sağ çıktı ve kontrolün kendisi yayımlanmaya değer.

**IETF, kapsamlı biçimde tarandı.** Datatracker'da her çalışma grubu boyunca tam metin araması, adında "biometric" geçen yalnızca **iki** belge döndürüyor: biyometrik özelliklerden anahtar türetmeyle ilgili süresi dolmuş bir bireysel taslak ve Wi-Fi algılama üzerine bir sunum dosyası. **Hiçbir IETF belgesi bir biyometrik kontrolü üçüncü taraf bir cihaza devretmiyor.** OAuth ve WIMSE'deki "delegeli yetki" kümesi — delegated authorization, agent delegation, WIMSE, Web4 delegated authority, ZCAP — istisnasız biçimde *yetki* ile ilgilidir ve hiçbir yerde *biyometrik varlık* ile değil.

**Her ticari üründe ya telefon hazırdır ya da delegasyon bir okuyucuya değil, bir insana yapılır.**

| Ürün | Telefon / tutan | Üçüncü taraf bir okuyucu doğruluyor mu? |
|---|---|---|
| **Suprema "Face Template on Mobile"** (BioStation 3) | Telefon **hazır** — şablon ve eşleştirme kullanıcının kendi telefonunda; terminale yalnızca *sonuç* gider | Terminal ayrı bir okuyucudur, ama kişi **telefonuyla birlikte** orada durur |
| **KeyShare "Delegated Agent Mode"** | Vatandaşın telefonu **yok**; anahtarlar bulut cüzdanında; yetkili bir insan **ajanın** cihazı arayüz | Doğrulayan taraf hizmettir, kamuya açık bir okuyucu değil; ajan vatandaş *adına* hareket eder |
| **Dentity "Agentic Identity"** | Cüzdan **hazır** | Kamuya açık okuyucu yok; bir yapay zekâ ajanına yetki delegasyonu |
| **Proxy Checkout** | Ebeveynin telefonu **hazır** | Sepet onayı, okuyucu değil |
| **VeryAI** | Kendi telefonu, **hazır** | Üçüncü taraf okuyucu değil |

**Suprema'nın Template-on-Mobile'ı en yakın ticari benzerdir ve yenilik iddiası için en tehlikeli şeydir** — cihazda şablon, üçüncü taraf okuyucu, hepsi tek bir üründe. Ama kullanıcı telefonuyla birlikte orada duruyor, ki bu tam olarak önerinin değiştirdiği koşuldur. Ve bu bir üründür, bir standart değil.

## Bölüm 2: Mekanizma zaten patentli

Bunu açıkça söylemek gerekir, çünkü bir standardın neden var olması gerektiğini değiştirir.

**Visa International Service Association, "Delegated biometric authentication" adlı verilmiş bir patent ailesine sahiptir** — mucitler Sunpreet Singh Arora ve Maliheh Shirvanian, öncelik tarihi 1 Ekim 2019, ABD'de [US11321445B2](https://patents.google.com/patent/US11321445B2/en) (3 Mayıs 2022) ve [US12282539B2](https://patents.google.com/patent/US12282539B2/en) (22 Nisan 2025) olarak tescillenmiş, Avrupa ve Çin karşılıklarıyla ([EP4038523B1](https://patents.google.com/patent/EP4038523B1/en), [CN114556339A](https://patents.google.com/patent/CN114556339A/en)).

Tanımladığı mekanizma, **G1'in eksik parçasının tam olarak kendisidir**:

- Kullanıcının telefonu, bir **bulanık kasa "veri kasası"** üretir — şablondan bir kodlama fonksiyonuyla türetilen biyometrik noktalar, rastgele sahte noktalarla karıştırılmış hâlde — ve bu kasa **bir etkileşim belirteçini kilitler**.
- Telefon, **kasayı şablonu değil**, bir otel terminali, kütüphane kiosku, ATM ya da bina güvenlik terminali gibi genel bir cihaza gönderir.
- Genel cihaz yerel olarak taze bir şablon yakalar, eşleşen alt kümeyi bulur, kodlama fonksiyonunu polinom interpolasyonuyla yeniden kurar, belirteci açar — ve **şablonu imha eder**.
- Patent ayrıca **iptali, kasa değiştirmeyle tekrarlanan delegasyonu ve sınırlı kullanımlı, süreli ve tutarlı belirteçleri** kapsar; ve tam olarak bu kullanım durumlarını adlandırır: otel akıllı TV'si, kütüphane terminali, ATM, apartman binası güvenlik terminali.

Akademik ön bilgi ondan eskidir: Chabanne, Keuffer ve Lescuyer, *"Delegating Biometric Authentication with the Sumcheck Protocol"* (WISTP 2016) ve Gritti, Önen ve Molva, *"Privacy-preserving delegable authentication in the Internet of Things"* (ACM SAC 2019).

**Bu öneri için ne anlama gelir.**

- Fikir yeni değildir ve bir standart önerisi bunu öyle sunmamalıdır. Patenti ve akademik çalışmayı atıf yapmalıdır.
- Mekanizma şu anda yalnızca tescilli, lisanslı bir yoldan erişilebilirdir. Aynı güvenceleri — delegasyonlu varlık, iptal edilebilir kapsamlı yetki, okuyucuda şablon olmaması — şartnamelendiren bir açık standart, uygulayıcılara aynı özelliğe **ücretsiz** bir yol verir.
- Bu, bu depoda zaten var olan lisanslama mantığıyla aynıdır: **bütün kamu için ücretsiz, rekabet avantajı için gelir payı, ve sonucu ücretsiz yayımlayan bir standart kuruluşu katkıyı ücretsiz kullanır.** Mekanizmayı açıkça şartnamelendiren bir standart, katkının kendisidir ve sorunu anlatan bir belgeden çok daha güçlü bir katkıdır.
- Bu konuda çalışan herkes, kendi mekanizması hakkında kamuya açık bir açıklama yapmadan ve herhangi bir tescil başvurusundan önce bir patent avukatından görüş almalıdır. Bu depo bir operasyon serbestisi görüşü değildir.

## Bölüm 3: Öneri

### 3.1 Katmanlama — her parça nereye ait

Bu öneri kullanıcısı **katmanlı, çok kuruluşlu** bir yol seçti. Her parça, her şeyi tek bir belgeye sıkıştırmak yerine, ait olduğu yere gider.

| Katman | Kuruluş | Orada ne standartlaştırılır | Neden orada |
|---|---|---|---|
| **Delegasyon yetkisi nesnesi ve tel protokolü** | **IETF** | Yetki biçimi, meydan okuma ve yanıt, kanıt biçimi, okuyucuya ve amaca bağlılık kuralları, durum ve iptal mesajları | Tel protokolü IETF'in işidir ve Privacy Pass ile post-kuantum karma çalışmaları zaten orada yaşar. Yeni bir güvenlik alanında yeni bir çalışma grubu, yeni bir yerde yeni bir şartnameden daha ucuz ve daha başarılı olma şansına sahiptir. |
| **Biyometrik şablon işleme, sunum saldırısı tespiti, değerlendirme** | **ISO/IEC JTC 1/SC 37** (koruma için SC 27 ile birlikte) | ISO/IEC 30107-3 ve -4, 24745, 27553 ve 39794'ün yeniden kullanılması ve sıkılaştırılması; delegasyonlu okuyucunun ihtiyaç duyduğu gereksinimlerin, sunumun imha edilmesi şartı dâhil, eklenmesi | Biyometrik kısımlar zaten burada, onların sahibi komitede standartlaştırılmıştır. Bu öneri onları yeniden açmamalıdır. |
| **Kimlik belgesi ve confidence method biçimi** | **W3C Verifiable Credentials Çalışma Grubu** | `DelegatedPresence` confidence method, talep tipi, durum listesi entegrasyonu | VC 2.0 bir Öneridir ve Confidence Methods taslağı zaten biyometrik bölümü içerir. Bu, en küçük ve en belirli iş parçası ve doğal ilk adımdır. |
| **Dağıtım ekosistemi, okuyucu sertifikasyonu, kriptografik profil** | **FIDO Alliance** | Okuyucu uygunluğu, izin verilen kriptografi profili, post-kuantum algoritma listesi | FIDO zaten kimlik doğrulayıcıları ve biyometrik bileşenleri sertifikalandırıyor, izin verilen kriptografi listesini post-kuantum bölümüyle birlikte yayımlıyor ve okuyucuya komşu ekosisteme zaten sahip. Benimseme orada gerçekleşir. |
| **Ödeme tarafı yetkilendirme** | **EMVCo** | Delegasyonlu ödeme amacı ve yetkisi, gelişen ajan tabanlı ödeme çerçevesinin yanında | Delegasyonlu ödeme yetkilendirmesi şu anda burada standartlaştırılıyor ve biyometrik bir delegasyonun ödemeyi yetkilendirmesi buna uyum sağlamak zorunda. |
| **Okuyucu canlılık sertifikasyonu** | **ISO/IEC SC 37** ve ulusal akreditasyon kurumları | Bir okuyucunun kim tarafından, nasıl sertifikalandırılacağı | Sertifikasyon, "bir fotoğrafı kabul etti" meselesini bir sertifikasyon hatasına dönüştüren şeydir. |

**Neden tek bir belge değil.** IETF, ISO, W3C, FIDO ve EMVCo'yu kapsayan tek bir standardın tek bir atölyede hayata geçirilmesi mümkün değildir ve benimsenmezdi. Katmanlı bir öneri, her kuruluşunun sahip olduğu parçayı almasına izin verir ve bir dağıtımın yalnızca W3C ve FIDO parçalarıyla başlamasına imkân verir.

### 3.2 Standardın şartnamelendirmesi gerekenler

**Delegasyon aracısı.** İşletim sistemi veya cüzdan, şablonu bir güvenli öğede tutar, oradan çıkmayan bir cihaz anahtarı tutar ve yetki oluşturabilen ya da geri alabilen tek taraftır. Bir uygulama yetki isteyebilir; kendi başına oluşturamaz.

**Yetki nesnesi.** Benzersiz tanımlayıcı, delegasyon veren cihazın takma adı, geçerli olduğu okuyucu, izin verilen amaç, okuyucunun öğrenebileceği öznitelikler, geçerlilik penceresi, isteğe bağlı kullanım sınırı, durum ve delegasyon sayacı. Bir yetki bir *hizmet kategorisini* değil, bir *okuyucuyu* ve bir *amacı* adıyla belirtir — bir kanıtın aktarılamaz olmasını sağlayan budur.

**İki doğrulama kipi.**
- *Çevrimiçi, aracı aracılığı:* okuyucu güvenli bir kanal üzerinden aracıya meydan okuma verir; araç güvenli öğede eşleştirir ve imzalı bir beyan döner. En güçlü bağlılık, bağlantı gerektirir.
- *Çevrimdışı, imzalı kanıt:* aracı kısa ömürlü, okuyucuya ve amaca bağlı bir kanıtı önceden imzalar; okuyucu onu yayımlanmış güven çapalarına ve önbelleğe alınmış bir duruma karşı, ağ çağrısı olmadan doğrular.

**Delegasyonlu varlık denetimi (G1).** Bu, önerinin kalbidir ve kamuya açık kayıttaki hiçbir standardın kapsamadığı kısımdır. Okuyucu, şablon ve çoğu zaman ağ olmadan, kendisinin önündeki kişinin delegasyonu veren kişi olduğunu kanıtlamak zorundadır. Aday mekanizmalar, hiçbiri henüz kesinleşmiş değil:

- VC Confidence Methods `BiometricVectorConfidenceMethod` içinde zaten atılmış yönü izleyen bir **sıfır bilgi biyometrik eşleştirme** — okuyucu, şablonu ya da geriye dönük kullanılabilir bir tanımlayıcıyı öğrenmeden eşleşmeyi kanıtlar.
- Visa patentinin tanımladığı türden bir **bulanık kasa ya da paylaşımlı sır yapısı**. Güçlüdür ama patent kısıtlaması altındadır; bu yüzden bir standart ya bir lisans ya da farklı bir yapı gerektirir.
- **Güvenilir bir yakalama cihazına bağlı, meydan okumaya bağlı attestation** — okuyucunun sahip olmayabileceği donanım varsayar.
- Kimliğin değil yalnızca "on sekiz üstü" ya da "yetkili" bilgisinin gerektiği durumlar için **diferansiyel gizlilik ya da sıfır bilgi öznitelik kanıtı.**

Bu mekanizmaları açık bırakan bir standart sunmak bile değerlidir: gereksinimi, tehdit modelini ve değerlendirme ölçütlerini ortaya koyar ve çalışmanın devam etmesine izin verir.

**Sunum saldırısı tespiti.** Okuyucu denetimi ISO/IEC 30107-3 ve -4'e göre yapar, saldırı sınıfı başına yayımlanmış bir yanlış kabul oranı ile ve yakalamayı imha ederek. Standart, sunumun saklanmasını yasaklar.

**İptal yayılımı (G3).** Ara sıra bağlantılı üçüncü taraf okuyucuya, belirtilen sınır içinde ulaşan ve kişisel veri taşımayan bir durum mekanizması; anahtarı cihaz değil yetki üzerinden kurulur, böylece yayımlanması bu projenin var olma nedenini ortadan kaldırmaz. Burada durum listesinin tasarımı kriptografiden daha önemlidir.

**İzlenemezlik kipleri.** Kimliğin gerekmediği yerde tek kullanımlık ya da kısa ömürlü izlenemez belirteç olarak düzenlenen yetki; gerektiğinde kapsamlı kimlik bilgisi; yalnızca bir olgu gerektiğinde öznitelik-only talebi ("eşiğin üstünde"). Okuyucunun kendi depolaması da kısıtlanmalıdır, çünkü günlüklerini birleştiren bir okuyucu, protokol ne taşırır taşımaz hareket profili kurabilir.

**Veri minimizasyonu gereksinimleri.** Okuyucu şablon, geriye dönük kullanılabilir tanımlayıcı ve birleştirilebilir günlük saklamaz; işletmeci bir olayın kaydını saklar, bir bedenin değil. Bunlar denetlenebilirdir ve asıl mesele de budur.

**Uygunluk ve sertifikasyon düzeni.** Sunum saldırısı standardına karşı okuyucu sertifikasyonu, yayımlanmış yanlış kabul oranı ve kamuya açık bir uygunluk test paketi. Bu olmadan canlılık bir iddia olur, bir özellik olmaz.

### 3.3 Var olan ve yeniden icat edilmemesi gerekenler

| İhtiyaç | Mevcut çalışma |
|---|---|
| Şablonun cihazdan hiç çıkmadığı yerel kullanıcı doğrulama | FIDO2 / WebAuthn Level 3; ISO/IEC 27553-1:2022; FIDO Biometrics Requirements |
| Sunum saldırısı tespiti ve testi | ISO/IEC 30107-1/-2/-3/-4; NIST SP 800-63-4 §3.11 (uzak PAD zorunlu, yanlış sunum kabul oranı eşiği) |
| Şablon koruma, geri döndürülemezlik, izlenemezlik, yenilenebilirlik | ISO/IEC 24745:2022; ISO/IEC 39794 değişim biçimleri; uzak kipler için ISO/IEC 27553-2:2025 |
| Kapsamlı, iptal edilebilir, kullanıcı onaylı kimlik belgeleri | W3C VC 2.0; AB Kimlik Cüzdanı ve eIDAS 2.0; OpenID4VCI 1.0 ve OpenID4VP 1.0 |
| İzlenemez belirteçler | IETF Privacy Pass (RFC 9576, 9577, 9578); Apple'ın Private Access Token'ları; büyük tarayıcılarla Private Access Control Tokens çalışması |
| Cihazlar arası yakınlık kanıtı | FIDO CTAP 2.3; FIDO Proximity Exchange Protocol çalışma taslağı |
| Dinamik bağlamalı işlem yetkisi delegasyonu | FIDO'nun 2021 SCA delegasyonu beyaz paperi; EMVCo 3-D Secure decoupled authentication; EMVCo ajan tabanlı ödeme çerçevesi |
| Hizmetler çerçevesi üzerinden biyometrik hizmetler | ISO/IEC 30108-1:2026 (IAVS) — kapsamının kimlik doğrulama protokolü entegrasyonunu dışladığına dikkat |
| Post-kuantum algoritmaları ve geçiş | NIST FIPS 203 / 204 / 205; TLS 1.3'te karma anahtar değişimi için RFC 9954 ve RFC 10024; LAMPS karma ML-DSA şartnamesi (RFC-to-be 10073) |
| Kimlik kanıtlama kuralları | NIST SP 800-63-4; piyasadaki eKYC uygulaması |

## Bölüm 4: Birinci sınıf katman olarak post-kuantum güvenlik

Delegasyonlu bir kimlik bilgisi doğası gereği uzun ömürlüdür ve bu, post-kuantum kriptografiyi baştan planlamak için gerçek bir gerekçedir. Ancak dürüst anlatım, alışılmış olandan daha dardır ve fark burada önemlidir.

### 4.1 Neyin neyin risk altında olduğu

**Risk altında değil: şablon.** Şablon cihazdan hiç çıkmaz, dolayısıyla "şimdi topla, sonra çöz" saldırısına maruz değildir. Onu güvenli öğe korur, matematik değil. Şablonu korumak için bir post-kuantum şema gerekmez ve aksi iddia etmek yanlış bir argüman olurdu.

**Risk altında: uzun ömürlü önbelleğe alınmış doğrulama yapıtları.** Bir yetki aylarca ya da yıllarca geçerli olmak üzere tasarlanır ve sahadaki bir okuyucu bir kanıtı haftalarca önbelleğe alabilir. Klasik imzayı kırabilen gelecekteki bir saldırgan **bir yetkiyi geriye dönük olarak sahteleyebilir** ve o zaman bütün önbelleğe alınmış kanıtlar doğrulanır. Bu, bu tasarım için "şimdi topla, sonra çöz" eşdeğeridir ve yetki üzerinde post-kuantum imzaları tercih etmek için sağlam bir gerekçedir.

**Risk altında: anahtar kurma.** NIST'in kendi sıralaması, genel 2030 ve 2035 tarihlerinden önce kuantum dirençli anahtar kurmayı öne alır — aracı-okuyucu oturumu, okuyucu-güven hizmeti kanalı ve bir delegasyon el sıkışmasını koruyan her TLS önce geçmelidir.

**Çerçeveleme biçimi olarak doğru olmayan: "imzalar onlarca yıl dayanmalı."** Kimlik doğrulama, şifreleme ile aynı biçimde "şimdi topla, sonra çöz"e tabi değildir; bir algoritmanın kullanıldığı anda güvenli olması yeterlidir. Doğru soru "imza ne kadar dayanmalı" değil, "bir doğrulama yapıtı ne kadar süre önbelleğe alınabilir ve bir yetki, geçiş ufkuyla karşılaştırıldığında ne kadar süre geçerli kalır" sorusudur.

### 4.2 Önerilen kriptografik profil

| Bileşen | Profil | Dayanak |
|---|---|---|
| **Yetki imzası** | **Karma (composite)**: klasik (Ed25519 ya da ECDSA P-256) **+ ML-DSA-44**, *tek* bir açık anahtar ve *tek* bir imza olarak, tek bir algoritma tanımlayıcısıyla sunulur; böylece bugünün X.509, CMS ve COSE altyapıları onu tek bir atomik algoritma olarak ele alır, protokolün hibritleri öğrenmesi gerekmez | NIST FIPS 204; IETF LAMPS karma imza çalışması, şartnamesi RFC Editor son incelemesinde, RFC-to-be 10073 olarak |
| **Okuyucu-aracı kanalı** | Karma anahtar değişimi, `X25519MLKEM768` | RFC 10024 (Recommended = Yes), RFC 9954 |
| **Kompakt alternatif** | FN-DSA | **Beklemede** — FIPS 206 henüz yok. Mevcut olarak değil, geliştirme aşamasında olarak atıf yapılmalı |
| **Hash tabanlı yedek** | Uzun ömürlü, düşük hacimli yetkiler için SLH-DSA | NIST FIPS 205 |
| **Okuyucu attestation** | FIDO izin verilen kriptografi listesindeki gibi; liste post-kuantum bölümünde zaten ML-KEM-512/768/1024 ve ML-DSA-44/65/87 içeriyor | FIDO Alliance |

**Karma değil, hibrit-değil-konvansiyon.** Bu ayrım göründüğünden daha önemli. Aslında iki imzanın birleştirilmesinden oluşan bir "hibrit" imza, yoldaki her protokolün onun hibrit olduğunu bilmesini ve iki yarıyı doğru sırayla, doğru mesaj temsiliyle doğrulamasını gerektirir. Bir **composite** imza ise tek anahtar ve tek imza taşır; böylece bir yetki kimlik bilgisi, hiçbirinin değişmesine gerek kalmadan mevcut altyapıya — bir X.509 sertifikasına, bir CMS yapısına, bir COSE nesnesine — ihra edilebilir. LAMPS karma şartnamesi böyle on sekiz algoritma tanımlar ve standarda taşınması gereken güvenlik uyarısı şudur: bir composite imza **varoluşsal** kurcalamaya dayanıklılık verir ama **güçlü** kurcalamaya dayanıklılık vermez; bu, uzun ömürlü bir yetki için önemlidir, çünkü yeniden imzalanmış bir yetki taze bir yetki olarak kabul edilmemelidir.

**Geçiş takvimi.** NIST'in geçiş taslağı, 112-bit RSA, ECDSA ve ECDH'yi 2030'dan sonra kullanımdan kaldırıyor ve 2035'ten sonra yasaklıyor; AB'nin koordineli yol haritası, üye devletlerin post-kuantum yol haritalarını 2026'nın sonuna kadar, tam geçişi 2035'in sonuna kadar hedefliyor. Bugün piyasaya çıkan bir cihaz işletim sistemi 2035'te hâlâ sahada olacak. **Dolayısıyla yetkinin kriptografik ömrü geçiş ufkunun altında tutulmalı ya da yetki post-kuantum imzalar altında yeniden düzenlenmelidir** — standart bunu şart koşmalıdır, çünkü ürün tasarımını değiştirir.

**Anahtar rotasyonu ve yeniden düzenleme.** Yetkiler döndürülebilir ve yeniden düzenlenebilir olmalı, bir cihaz güncellemesi kapsam değişmediği için mevcut yetkileri kullanıcıdan yeni onay almadan post-kuantum bir anahtarla yeniden düzenleyebilmelidir.

### 4.3 İzlenemez kip için bir not

Privacy Pass çalışma grubu, kendi izlenemezlik temelinin **post-kuantum-izlenemez olmadığını** buldu: kuantum saldırganı belirtecin etiketindeki ayrık logaritmayı çözerek sunumları birbirine bağlayabilir. Grubun önerdiği çözümler farklı bir yapı, KZG ile taahhüt edilmiş bir kurulum, Legendre tarzı bir sözde rastgele fonksiyon ya da post-kuantum güvenli bir karma. **İzlenemez bir kip sunan bir standart bunu açıkça söylemelidir**, bugünün izlenemezliğinin kalıcı olduğu izlenimi bırakmamalıdır.

## Bölüm 5: Yönetişim ve onay

**Masada kimler olmalı.** Cihaz işletim sistemi ve güvenli donanım üreticileri; okuyucu ve satış noktası terminali üreticileri; erişim kontrolü ve ödeme işletmecileri; biyometri teknolojisi üreticileri; Bölüm 3.1'de adı geçen standart kuruluşları; veri koruma otoriteleri ve teknik muadilleri; biyometrik kriptografi ve gizlilik alanında akademik araştırmacılar; ve veri minimizasyonu konusunda çalışmış sivil toplum kuruluşları.

**Açıkta ne kararlaştırılır.** Tehdit modeli, gereksinim kümesi, uygunluk ölçütleri, varlık denetimi için değerlendirme yöntemi ve test vektörleri. Herhangi bir uygulamanın ticari koşulları değil.

**Tarafsızlık.** Güven hizmeti kâr amacı gütmeyen ya da kamu tüzel kişiliğine sahip bir kuruluştur, kurallarını ve ücretlerini yayımlar, yayımlanmış ölçütleri karşılayan her aracıyı ve işletmeciyi kabul eder, cihazları ya da okuyucuları sıralamaz. Bir aracının kabulünü reddedebilen bir güven hizmeti bir kapıcı olurdu ve model tasarımıyla önlemek istediği kilitlenmeyi miras alırdı.

**Yayımlanmış itiraz.** Bir sertifikasyon, bir uygunluk kararı ya da bir erişim kararı tartışıldığında yayımlanmış bir itiraz yolu bulunmalıdır. İtirazsız sertifikasyon düzenleri kapalı dükkânlara dönüşür.

## Bölüm 6: Lisanslama, haklar ve yol

**Yazarın konumu: bütün kamu için ücretsiz, rekabet avantajı için ödeme.** Bu, bu depoda zaten var olan konumdur.

- **Herkes için kamuya açık kullanım: ücretsiz.** Devlet akışı zorunlu kılıp bütün kamu fayda görüyorsa yazar ücret talep etmez. Standart o zaman serbestçe erişilebilir olmalı, geliştirilmesi ve yönetilmesi standardı satarak değil vergilerle karşılanmalıdır.
- **Gönüllü ticari kullanım, rekabet avantajı için: gelir payı.** Akış zorunlu olmadığı sürece, onu rakiplerinden daha fazla güven sunmak için kullanan bir şirket bunu pazarın bir kısmı için pazarlama avantajı olarak kullanır, bütün kamu için değil. Bu kullanım, bir devlet uygulaması akışı zorunlu kılana kadar [ticari lisansın](COMMERCIAL-LICENSE.md) gelir payı altındadır.
- **Standart kuruluşu için de aynı kural.** Sonucu bütun kamuya ücretsiz sunan bir kuruluş katkıyı ücretsiz kullanır. Onu ticari bir ürün olarak satan bir kuruluş, yazardan ücretli koşullarla almalıdır.

**Bu, standardizasyon için ne anlama gelir.**

- **CEN ve ETSI katkıcılara ödeme yapmaz.** Bir atölyeye katılanlar, katkılarının kullanımını kuruluşa ücretsiz verir ve kuruluş yayımlanan belgeleri satar. Yazar bu yüzden yalnızca **serbestçe erişilebilir** bir teslimata katkı verir — ücretsiz yayımlanan bir ETSI teslimatı ya da serbestçe erişilebilir bir atölye çıktısı.
- **Gelir payının bir kancası gerekir.** Yayımlanmış bir standart, bu deponun metni olmadan uygulanabilir ve telif metni korur, fikri değil. Gelir payı şunlara dayanır: (1) bu deponun metni ve materyallerinin ticari kullanımı; (2) uyumlu bir akış için bir **sertifikasyon işareti**, böylece uyumu bir güven işareti olarak sunmak isteyen bir şirket lisans almak zorunda kalırken devlet uygulamaları ücretsiz kullanır; ve (3) yazarın edinmeye devam edebileceği patentler.
- **Patent konumu için görüş alınmalı.** Bölüm 2'de anlatılan tescilli patent ailesi nedeniyle, delegasyonlu varlık mekanizması üzerinde çalışan herkes, belirli bir yapı hakkında kamuya açık açıklama yapmadan ve tescil yapmadan önce patent görüşü almalıdır. Bölüm 2'nin sonundaki nota bakınız.
- Katkı ya da Fikri Mülkiyet Beyanı imzalamadan, ya da bir işaret tescil etmeden önce metinler bir avukata bakılmalıdır.

**Yol.** İlk ve en küçük adım **W3C VC Confidence Methods** çalışmasıdır: bir `DelegatedPresence` confidence method önerisi, biyometrik ve delegasyon bölümlerinin doldurulması ve sıfır bilgi eşleştirme akışının şartnamelendirilmesi. En dar parçadır, canlı bir çalışma taslağı vardır ve yeni bir kuruluş gerektirmez. **IETF** katmanı onu, mevcut güvenlik alanında Privacy Pass ve post-kuantum karma çalışmalarının yanında bir protokol şartnamesi olarak izler. **ISO/IEC JTC 1/SC 37** katmanı yeni bir belge değil, mevcut bölümlerin revizyonudur. **FIDO**, okuyucu uygunluğu ve izin verilen kriptografi işini, sertifikasyon programı zaten orada olduğu için üstlenir. **EMVCo**, geliştirmekte olduğu ajan tabanlı ödeme çerçevesinin yanında ödeme tarafı delegasyonunu üstlenir.

**Destekçiler.** Bu adımlardan herhangi birinden önce, en az bir cihaz ya da cüzdan üreticisinden, bir okuyucu ya da terminal üreticisinden, bir işletmeciden, bir veri koruma otoritesinden ya da teknik muadilinden ve bir akademik ya da sivil toplum kuruluşundan destek mektupları toplanmalıdır. Destekçisiz gelen bir öneri önceliklendirilmez.

## Bölüm 7: Neyin eklenmesi gerekiyor, ne zaten var

Bu, gerçek bir standardizasyon çabasının izleyeceği aşamadır.

| Zaten var — yeniden yazma, kullan | Eklenmesi gereken — asıl iş |
|---|---|
| Şablonun cihazdan hiç çıkmadığı yerel kullanıcı doğrulama (FIDO2, ISO/IEC 27553-1) | **Delegasyonlu varlık** denetimi: şablonsuz, üçüncü taraf bir okuyucuda, hazır bulunmayan bir yetki sahibinin yetkisini kanıtlamak |
| Sunum saldırısı tespiti ve testi (ISO/IEC 30107-1/-2/-3/-4) | **Yetki nesnesi** ve tel formatı, okuyucuya bağlılık, amaca bağlılık ve kapsam kurallarıyla |
| Kapsamlı, iptal edilebilir kimlik belgeleri (W3C VC 2.0, AB Kimlik Cüzdanı, OpenID4VCI/VP) | **`DelegatedPresence` confidence method** ve ilgili talep tipi |
| İzlenemez belirteçler (IETF Privacy Pass, Private Access Tokens) | **Sınırlı iptal yayılımı**: aralıklı bağlantılı üçüncü taraf okuyuculara, durum listesinde kişisel veri olmadan |
| Cihazlar arası yakınlık kanıtı (CTAP 2.3, Proximity Exchange Protocol) | **Okuyucu uygunluğu ve sertifikasyonu**, yayımlanmış yanlış kabul oranlarıyla |
| Post-kuantum algoritmaları (NIST FIPS 203/204/205, RFC 9954/10024) | **Yetkiler ve oturumlar için post-kuantum profil** ve mevcut yetkiler için yeniden düzenleme yolu |
| Ödeme yetkilendirmesi (EMVCo 3-D Secure, ajan tabanlı ödemeler) | **Bir biyometrik kontrolün bir yetkilendirmeye delegasyonu** |

**İddianın gücü hakkında bir kapanış notu.** Yukarıdaki boşluk analizi, standart kuruluşlarının kamuya açık kayıtlarına, çalışma taslaklarına ve yayımlanmış belgelerine, 2026 Ekim itibarıyla dayanmaktadır. ISO/IEC JTC 1/SC 37 çalışma kalemi kataloğu tümüyle kamuya açıkta aranamaz ve çeşitli ilgili kalemler sürmektedir — ISO/IEC 30108-1:2026, 26655, 25049-1/-2 ve 59794-1/-5 — bu yüzden delegasyonla ilgili canlı bir SC 37 kalemi olasılık dışı değildir. Yayımlamadan önce bu katalogda hedefli bir kontrol yapılmalı ve bu nottaki iddia en doğru biçimiyle **"kamuya açık kaynaklarda mevcut bir standart bulunamadı"** olarak ifade edilmelidir, mutlak bir hüküm olarak değil. Diğer boşluk ifadeleri — FIDO2'nin origin'a bağlı doğrulayıcı modeli, Doğrulanabilir Kimlik Belgeleri'nin sunan tutan varsayımı, FIDO MDS ile VC durum listelerinin iptal mekanizmaları ve ISO/IEC 30108-1'in kimlik doğrulama protokolü entegrasyonunu dışlaması — yayımlanmış şartnamelerden çıkar ve güvenle ileri sürülebilir.
