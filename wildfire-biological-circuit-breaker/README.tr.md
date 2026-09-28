English: [README.md](README.md)

# Hücresel Biyolojik Devre Kesici (Circuit Breaker) Modeli ile Orman Yangınları Yayılım Önleme Projesi

*Yazılım mimarisi yaklaşımıyla domino etkisinin engellenmesi ve yangına dirençli yeşil kuşaklar*

| | |
|---|---|
| **Konu** | Yangın yayılım bariyerleri ve biyolojik kuşaklama |
| **Odak bölge** | Ege ve Akdeniz orman ekosistemleri |
| **İlham kaynağı** | Software Architecture - Circuit Breaker Pattern |
| **Uygulama gücü** | Pilot saha ve askeri iş gücü entegrasyonu |

## Özet

Orman yangınlarının en yıkıcı özelliği, alevlerin kesintisiz bitki örtüsü üzerinden zincirleme biçimde ilerlemesi ve kontrolden çıkan bir **domino etkisi (Cascading Failure)** oluşturmasıdır. Geleneksel koruma yöntemlerinde düşünülen hendek kazma veya yapay su kanalları, yüksek finansal maliyet, ağır iş makinesi gereksinimi ve doğal topografyaya verilen zararlar nedeniyle pratik olarak uygulanamamaktadır.

Bu proje, yazılım mimarilerinde sistem kilitlenmelerini ve ardışık çöküşleri engellemek için kullanılan **"Circuit Breaker" (Devre Kesici)** prensibini doğaya uygulamayı hedeflemektedir. Ormanlık alanlar yüksek maliyetli hendekler yerine **yangına yüksek dirençli yerel biyolojik türlerle ayrık hücresel kümelere (arı peteği yapısı)** bölünecek, böylece yangın bir hücrede çıksa dahi diğer hücrelere sıçramadan izole edilecektir.

Model, yangın riski yüksek bir bölgede 500 hektarlık bir pilot sahada doğrulandıktan sonra yaygınlaştırılabilir. Yaygınlaştırma sırasındaki iş gücü ihtiyacı için vatani görevini yapan gençler sahada görev alabilir.

## Sorun

- Son yıllarda Akdeniz ve Ege havzalarında yaşanan orman yangınlarının en kritik boyutu, alevlerin rüzgar ve kesintisiz örtü nedeniyle kontrol edilemez bir domino etkisi (Cascading Failure) yaratarak hızla yayılmasıdır.
- Mevcut koruma yaklaşımlarında düşünülen yapay su kanalları, göletler veya hendek kazma işlemleri:
  - aşırı yüksek finansal maliyetlere (hafriyat, yakıt, iş makinesi) yol açar;
  - ağır iş makinesi gerektirir;
  - toprak yapısını ve doğal topografyayı bozar, erozyon riski doğurur;
  - kozalak atlamasını engelleyemez.
- Bu nedenlerle sürdürülebilir bir çözüm sunamamaktadır.

## Önerilen Çözüm

Projenin ilham aldığı yapı, yazılım mimarisinde cascading failure'ı engellemek için kullanılan circuit breaker yapısıdır. Bu yapı, domino dizimi sırasında domino etkisini ortadan kaldırmak için aradan çıkartılan domino taşlarına benzer. Yazılım sistemlerinde zincirleme hataları önlemek amacıyla ardışık servisler arasına "devre kesici" mekanizmaları yerleştirilir. Orman mühendisliğindeki karşılığı olarak, yangının rüzgar ve kozalak fırlatma ile kat ettiği mesafeyi sıfırlayan biyolojik **"yeşil kilitler"** oluşturulacaktır.

Projenin temel yaklaşımı üç unsura dayanır:

1. **Hücresel arı peteği yapısı ve domino etkisinin kesilmesi:** Orman sahaları, suni hendekler yerine yangına yüksek dirençli yeşil biyolojik kuşaklarla bağımsız altıgen/hücresel kümelere ayrılır. Böylece bir hücrede başlayan yangının diğer hücrelere sıçraması engellenir ve yangın çıktığı alanda izole edilir.
2. **Hendek alternatifi olarak Türkiye florasına uygun biyolojik yeşil kuşaklar:** Yapay ve pahalı bariyerler yerine, Akdeniz ve Ege ekosistemine tam uyumlu yerel türler katmanlı olarak dikilir.
3. **Sürdürülebilir iş gücü modeli (askeri entegrasyon):** Fidan dikimi ve saha bakımı, vatani görevini yapan yükümlülerle yürütülür.

## Nasıl Çalışır

### Teorik model: Circuit Breaker ve hücresel izolasyon

```
   ___       ___      |||||||      ___       ___
  /   \     /   \     |YEŞİL|     /   \     /   \
 | A   |---| B   |    |KUŞAK|    | C   |---| D   |
  \___/     \___/     |||||||     \___/     \___/
  Hücre A   YANGIN    biyolojik    korunan hücreler
            (Hücre B) devre kesici
```

*Şekil 1: Hücresel arı peteği yapısı ve biyolojik devre kesici (Circuit Breaker) izolasyon modeli. Hücre B'deki yangın yeşil kuşakta durdurulur ve Hücre C ile Hücre D'ye ulaşmaz.*

Sistem prensipleri:

- Domino etkisi engeli
- Taç ve taban sıcaklık kalkanı
- Ayrık küme izolasyonu

### Türkiye ekosistemine uygun biyolojik yeşil kuşak flora seçimi

Yapay hendeklerin yüksek maliyetini bertaraf etmek üzere petek sınırlarında yüksek nem kapasitesine sahip, zor tutuşan Türkiye yerel florası katmanlı olarak planlanmıştır:

| Bitki türü (botanik ve Türkçe adı) | Katman görevi | Yangın direnç mekanizması | Ekolojik uyum |
|---|---|---|---|
| **Cupressus sempervirens var. horizontalis** (Dallı Akdeniz Servisi) | Üst katman ağaç bariyeri | Rüzgar kesici etki yapar ve kozalak yapısı patlamaz. Yüksek taç nemi tutma yeteneği ve rüzgar kıran yapısıyla kozalak sıçramasını tutar. İspanya'da yangın bariyeri olarak test edilmiştir. | Ege ve Akdeniz kurakçıl alanları |
| **Ceratonia siliqua** (Keçiboynuzu / Harnup) | Orta katman ağaç bariyeri | Geniş etli yaprakları yüksek su tutar ve tutuşma sıcaklığı yüksektir. Dökülen yaprakları örtü yangını yakmaz. Zor tutuşma kapasitesiyle alev gücünü emer. | Kıyı ve alt Akdeniz kuşağı |
| **Nerium oleander ve Laurus nobilis** (Zakkum ve Defne) | Alt katman çalı bariyeri | Taban yangınının ilerlemesini engeller. Alev boyunu düşürerek ateşin ağaç tepelerine sıçramasını keser. | Tüm Ege, Akdeniz, Marmara |
| **Arbutus unedo** (Kocayemiş) | Toprak koruyucu rüzgar kıran | Duyarlı orman tabanında nem birikimi sağlar, alev enerjisini buharlaşma ile emer. | Marmara ve Ege maktu alanları |

Bu bitkiler alevle karşılaştığında parlamak yerine suyu buharlaştırarak ateşin enerjisini emer ve yangının bir hücreden diğerine atlamasını zorlaştırır.

### Doğal topografyanın kullanılması

Petekleri kusursuz altıgenler yapmak yerine ormandaki mevcut kayalıklar, nehir yatakları, açıklıklar ve yollar haritalanarak petek sınırına dahil edilir ve altıgen yapı doğal topografyaya göre uyarlanır. Yalnızca eksik kalan yerlerin bitkilendirilmesi, maliyeti sıfıra yaklaştırır.

## Uygulama ve Aşamalandırma

### 1. aşama: Pilot saha

Projenin ilk aşamasında yangın riski üst düzeyde olan **Muğla (Marmaris/Datça) veya İzmir (Urla/Seferihisar)** Orman Bölge Müdürlükleri sahasında belirlenecek **500 hektarlık** bir alanda pilot çalışma yürütülecektir:

- **Coğrafi model:** Mevcut kayalıklar, nehir yatakları ve yollar haritalanarak petek sınırına dahil edilir ve altıgen yapı doğal topografyaya göre adapte edilir.
- **Saha doğrulaması:** **5 ayrık hücre** oluşturularak biyolojik kuşakların nem tutma ve rüzgar kesme performansı izlenir.

### 2. aşama: Kademeli yaygınlaştırma

Model pilot sahada doğrulandıktan sonra yangın riski yüksek diğer bölgelere kademeli olarak yaygınlaştırılabilir.

### İş gücü operasyon modeli (vatani görev / askeri entegrasyon)

Biyolojik kuşakların oluşturulması, fidan dikimi, saha bakımı ve arazi düzenlemesi ciddi insan gücü gerektirmektedir. Maliyeti düşürmek ve kamu kaynaklarını etkin kullanmak için **Milli Savunma Bakanlığı ile Tarım ve Orman Bakanlığı arasında ortak bir iş birliği protokolü** önerilmektedir. Bu protokolle vatani görevini yapan yükümlüler ve askeri birlikler (er/yedek subay) fidan dikim sahalarında görev alacaktır. Bu yaklaşım hem iş gücü maliyetini sıfıra yaklaştıracak hem de toplumsal farkındalığı artıracaktır.

## Paydaşlar ve Faydalar

### Uygulayıcı paydaşlar ve iş birliği ortakları

- **Tarım ve Orman Bakanlığı, Orman Genel Müdürlüğü (OGM)** ve bağlı Orman Bölge Müdürlükleri (pilot saha ve teknik yürütme).
- **Milli Savunma Bakanlığı** (ortak protokol kapsamında vatani görev iş gücü).
- **TEMA Vakfı** (uzman değerlendirmesi ve projenin geliştirilmesi).
- Uluslararası yaygınlaştırma için değerlendirme ve pilot doğrulama ortakları: Avrupa Komisyonu (DG ECHO / AB Sivil Koruma Mekanizması), UNCCD, IUCN ve Avrupa ormancılık kurumları.
- Disiplinler arası katkı sunacak çevre mühendisleri, yazılım mimarları ve doğa koruma uzmanları.

### Karşılaştırmalı analiz

| Parametre | Geleneksel hendek / kanal | Biyolojik devre kesici (önerilen proje) |
|---|---|---|
| **Maliyet** | Yüksek (hafriyat, yakıt, iş makinesi) | Çok düşük (fidan üretimi ve askeri iş gücü) |
| **Çevresel etki** | Toprak yapısı bozulur, erozyon riski doğar | Doğal ekosistemi zenginleştirir, arıcılığı destekler |
| **Sıçrama engelleme** | Kozalak atlamasını engelleyemez | Yüksek taç servi bariyeri kozalak sıçramasını tutar |

### Faydalar

- Hendek maliyetlerine girmeden ormanlar "hücre hücre" koruma altına alınır.
- Ege ve Akdeniz ekosistemine uyumlu, onu zenginleştiren yerel türler kullanılır.
- İş gücü maliyeti sıfıra yaklaşır ve toplumsal farkındalık artar.
- Yazılım ile ekolojiyi birleştiren disiplinler arası yaklaşım diğer Akdeniz ülkelerine de aktarılabilir.

## Riskler ve Önlemler

| Risk | Önlem (öneride yer alan) |
|---|---|
| Modelin yerel koşullarda henüz doğrulanmamış olması | Yaygınlaştırmadan önce 500 hektarlık, 5 hücreli pilot sahada nem tutma ve rüzgar kesme performansının izlenmesi |
| Yapay bariyerlerin yüksek maliyeti ve ekolojik tahribatı | Hendek/kanal yerine biyolojik yeşil kuşaklar ve mevcut doğal engellerin petek sınırı olarak kullanılması |
| Yangının kıvılcım ve kozalak fırlatmasıyla bariyerleri aşması | Yüksek taç nemli, rüzgar kıran servi bariyerleri |
| Dikim ve bakım için yüksek iş gücü ihtiyacı | Kurumlar arası protokolle vatani görev iş gücünün sahaya dahil edilmesi |

## Kaynaklar / Örnekler

- **Andilla yangını, Valensiya, İspanya (2012):** Her şeyi kül eden orman yangınında Akdeniz Servisi (*Cupressus sempervirens*) ağaçlarından oluşan bir şeridin yanmadığı ve yangını durdurduğu kanıtlanmıştır. Bu örnek, üst katman bariyer türünün seçiminin ampirik dayanağıdır.
- **Circuit Breaker tasarım prensibi (yazılım mimarisi):** Ardışık servisler arasında zincirleme hataları (cascading failure) durdurmak için kullanılan tasarım prensibi. Modelin kavramsal kaynağıdır.

## Köken

Merih İlgör tarafından bir kamu politikası önerisi olarak hazırlanmıştır (Temmuz 2026); burada açık proje fikri olarak yayımlanmaktadır.

## Lisans

Bu çalışma, ticari olmayan kullanım için CC BY-NC-SA 4.0 lisansı ile lisanslanmıştır; bkz. [LICENSE](LICENSE). Ticari kullanım bir gelir paylaşımı anlaşması gerektirir; bkz. [COMMERCIAL-LICENSE.md](COMMERCIAL-LICENSE.md).
