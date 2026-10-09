English: [TMF-OPEN-API-MAPPING.md](TMF-OPEN-API-MAPPING.md)

# Güvenli Sözleşme Akışı: TM Forum Open API Eşlemesi (Uygulama Notu)

> **Bağlayıcı olmayan uygulama notu.** [Öneri](README.tr.md) "ne" ve "neden"i anlatır, herhangi bir teknoloji öngörmez. Bu not, akışı [TM Forum Open API](https://www.tmforum.org/oda/open-apis/)'leri üzerine kurmak isteyen ekipler içindir: neyin doğrudan eşlendiğini, neyin genişletme gerektirdiğini ve neyin hiç karşılığı olmadığını gösterir. Yayımlanmış **TMF651 Agreement Management API v4.0.0** şemasıyla karşılaştırılarak hazırlanmıştır; hedeflediğiniz sürüme göre yeniden kontrol edin (v5 mevcuttur).

## Özet

- **Temel model uyar.** Sözleşme bir TMF651 `Agreement`, sözleşme türü bir `AgreementSpecification` olur; TMF651'de benzersiz belge numarası için bir `documentNumber` alanı bile vardır.
- **Durum modeli uyar, çünkü TMF651 bir durum modeli tanımlamaz.** `status` serbest bir metindir (şartname yalnızca "in process, approved, rejected" değerlerini örnek verir); bu yüzden önerideki yaşam döngüsü durumları uygulamanın kendi durum modeli olarak tanımlanabilir.
- **Temel güvenceler eksiktir.** Sürüm kilidi, ön inceleme pazarlığı, idrak testi, noter adımı, sözleşmeler arası türü belli bağlantılar (tadil, alt sözleşme, yenileme), fesih ayrıntıları ve denetim/izin kurallarının yerleşik karşılığı yoktur. Bunlar genişletmeler (`@type`, `@baseType`, `@schemaLocation`, `characteristic`), tamamlayıcı API'ler ve sistemin kendisinin uyguladığı kurallar gerektirir.

## 1. Doğrudan eşlemeler

| Öneri | TM Forum Open API (aksi belirtilmedikçe TMF651 v4) | Not |
|---|---|---|
| Sözleşme | `Agreement` | |
| Sözleşme türü veya şablonu | `AgreementSpecification` | Sözleşme türü başına şablon (gayrimenkul satışı, vekâletname, kira…) |
| Benzersiz belge numarası | `Agreement.documentNumber` | Tipi **tamsayı**; biçimli veya kontrol haneli bir numara için genişletme alanı gerekir |
| Sürüm | `Agreement.version` | Yalnızca metin; sürüm geçmişi için eksikler bölümüne bakın |
| Sözleşme metni ve şartları | `agreementItem[].termOrCondition[]` (`description`, `validFor`) | Kalemler ürün teklifleri etrafında tasarlanmıştır; hukuki sözleşmelerde yalnızca şartları kullanın |
| Taraflar | `Agreement.engagedParty[]` (`role` içeren `RelatedParty`) → TMF632 Party, TMF669 Party Role | |
| Noter, tanıklar, avukat | `notary` gibi bir rolle `engagedParty[]` | Agreement'ta ayrı bir `relatedParty` olmadığından taraf olmayan roller de bu listede yer alır |
| Yürürlük süresi, bitiş tarihi | `Agreement.agreementPeriod` (`startDateTime`, `endDateTime`) | |
| Tamamlanma | `Agreement.completionDate` | Tek bir tarih değil, zaman aralığı tipinde |
| İmzalar | `Agreement.agreementAuthorization[]` (`date`, `state`, `signatureRepresentation`) | Eksikler: imzalayan kişiye referans yok, biyometrik değer yok |
| Durum (incelemede, kilitli, yürürlükte, süresi dolmuş, iptal, fesih, tamamlanmış…) | `Agreement.status` | Serbest metin; değerler serbestçe tanımlanabilir |
| Bağlantılı sözleşmeler | `Agreement.associatedAgreement[]` (`AgreementRef`) | Türsüz; eksikler bölümüne bakın |
| Ekler | TMF667 Document Management (`attachment` içeren `Document`), bir genişletmeyle referans verilir | `AgreementSpecification`'da `attachment` var, `Agreement`'ta yok |
| Bildirimler (süre dolması, fesih, her durum değişikliği) | `AgreementStateChangeEvent`, `AgreementAttributeValueChangeEvent` (hub/listener) | |
| Oluşturma, değiştirme, sorgulama | `POST/GET /agreement`, `GET/PATCH/DELETE /agreement/{id}` | PATCH ve DELETE için çatışma noktalarına bakın |

## 2. Yaşam döngüsü olayları

| Olay | Eşleme | Uyum |
|---|---|---|
| Taslak ve ön inceleme | `status` = örn. `inReview` olan `Agreement`; sürümler `version` ile | Kısmi: pazarlık veya teklif nesnesi yok |
| Tüm tarafların onayı | `state` = approved olan `agreementAuthorization[]` kayıtları | Kısmi: hangi tarafın onayladığına bağlantı yok |
| Sürüm kilidi | `status` = `locked` ve `characteristic` içinde içerik özeti (hash) | **Eksik:** TMF'de sonraki değişiklikleri engelleyen bir şey yok |
| İmza | `signatureRepresentation` ile `agreementAuthorization[]` | Kısmi: ıslak veya dijital karşılanır; biyometrik, sertifika ve imzalayan kişi genişletme gerektirir |
| Ekler | Genişletme referansıyla bağlanan TMF667 `Document` | Kısmi |
| Tadil veya ek protokol | `associatedAgreement` ile bağlanan yeni `Agreement` (veya yeni `version`) | **Eksik:** bağlantının türü ("tadil eder") yok |
| Alt sözleşme | `associatedAgreement` ile bağlanan alt `Agreement` | **Eksik:** üst/alt türü ve "ana sözleşmeyi aşamaz" kuralı yok |
| Devir veya taraf değişikliği | `PATCH engagedParty` | **Eksik:** kimin neyi kime, ne zaman devrettiğinin kaydı yok |
| Yenileme veya süre uzatımı | Yeni `agreementPeriod` veya bağlantılı sözleşme | Kısmi: yenileme türü ve hatırlatmalar için genişletme ve zamanlayıcı gerekir |
| Sürenin dolması | `agreementPeriod.endDateTime` ve `status` = `expired` ile durum değişikliği olayı | İyi; ancak durum değişikliğini sistemin tetiklemesi gerekir |
| İptal (karşılıklı) | `status` = `cancelled` ve `agreementAuthorization` içinde onaylar | Kısmi: gerekçe veya sonuçlar (iade, cezai şart) yok |
| Fesih (tek taraflı) | `status` = `terminated` | **Eksik:** gerekçe, bildirim tarihi, bildirim süresi veya fesheden taraf yok |
| Askıya alma | `status` = `suspended` | **Eksik:** askı süresi veya nedeni yok; süreler yeniden hesaplanamaz |
| Tamamlanma | `completionDate` ve `status` = `completed` | İyi |
| Uyuşmazlık | `status` = örn. `disputed` | **Eksik:** geçmiş veya delil modeli yok (denetim kaydına bakın) |

## 3. Karşılığı olmayanlar (eksiklik eşlemesi)

| Önerideki unsur | TMF'de var mı? | Önerilen yaklaşım |
|---|---|---|
| Tutar sınırı (her yıl gözden geçirilen) | Hayır | API dışında iş kuralı; sözleşme bedeli bir `characteristic` olarak |
| Çevrim içi pazarlık (önerilen sürümler, yorumlar, kimin neyi değiştirdiği) | Hayır | Genişletme kaynağı (örn. `AgreementVersion` / `ChangeProposal`) veya bir belge iş birliği hizmeti |
| Sürüm kilidi (değiştirilemez metin) | Hayır | İçerik özeti ve kilitli alanlarda PATCH'i reddeden sistem kuralı; kilitli metin değiştirilemez bir TMF667 `Document` olarak saklanır |
| İdrak testi (sorular, cevaplar, sonuç, erteleme) | Hayır | Sözleşmeye ve tarafa bağlı genişletme kaynağı; kanunun gerektirdiğinin ötesinde kişisel cevaplar değil, yalnızca sonuç saklanır |
| Noter işlemi (kimlik doğrulama, ehliyet kanaati, noterin kendi kaydı) | Hayır | `engagedParty` içinde noter rolü ve noter işlemi için genişletme; kimlik için TMF720 Digital Identity veya ulusal e-kimlik |
| Her imzanın imzalayanı | Hayır (`AgreementAuthorization`'da taraf referansı yok) | `AgreementAuthorization`'ı taraf referansı, sertifika ve imza türüyle (ıslak, dijital, biyometrik) genişletmek |
| Sözleşmeler arası türü belli bağlantılar (tadil eder, alt sözleşmesidir, yeniler, yerine geçer) | Hayır (`AgreementRef` türsüz) | Genişletme: `associatedAgreement` üzerinde ilişki türü (şablondaki `specificationRelationship`'e benzer) |
| Fesih ayrıntıları (gerekçe, bildirim tarihi, bildirim süresi, fesheden taraf) | Hayır | `characteristic` veya genişletilmiş bir `Agreement` alt tipi |
| Askı süresi ve nedeni | Hayır | Zaman aralığı içeren `characteristic` |
| Durum geçiş kuralları (hangi değişikliğe ne zaman izin verilir) | Hayır (durum modeli yok) | Uygulamanın kendi durum makinesi, bir API profili olarak yayımlanır |
| Orantılılık (hangi olayların noter gerektirdiği) | Hayır | İş akışı kuralları; örneğin TMF701 Process Flow ile orkestrasyon |
| Denetim kaydı (her görüntüleme ve değişiklik) | Hayır | Olay deposu veya denetim kaydı; dağıtım için TMF688 Event Management |
| İzinli üçüncü kişi durum kontrolü | Hayır | İzin için TMF644 Privacy Management veya ulusal izin hizmeti; salt okunur bir durum uç noktası |
| Saklama ve silme kuralları | Hayır | API dışında politika; imzalı sözleşmeler için `DELETE /agreement/{id}` kapatılır |
| Biçimli belge numarası | Kısmi (`documentNumber` tamsayı) | Biçimli numara için genişletme alanı |

## 4. Çatışma noktaları

- **`DELETE /agreement/{id}`:** İmzalı bir sözleşme asla silinmemelidir. API profili bunu yasaklamalı; iptal veya fesih bir durum değişikliği olmalıdır.
- **`PATCH /agreement/{id}`:** Her alanın değiştirilmesine izin verir. Kilitlemeden sonra sistem metin, taraf ve şart değişikliklerini reddetmeli; her değişiklik yeni bir sürüm veya bağlantılı bir sözleşme üzerinden yapılmalıdır.
- **Ürün odaklı kalemler:** `AgreementItem`, `productOffering` ve `product` etrafında kuruludur. Hukuki sözleşmelerde `termOrCondition` kullanılmalı, ürün referansları boş bırakılmalıdır.

## 5. TMF'nin ötesi

TM Forum Open API'leri telekom kökenlidir ve kurumsal bir sözleşme platformuna iyi uyar. Ulusal bir e-devlet ve noter sisteminde hukuki katman en az bu kadar önemlidir: e-imza mevzuatı (örneğin eIDAS ve ETSI standartları, Türkiye'de 5070 sayılı Elektronik İmza Kanunu), hukuki belge biçimleri, noterlik mevzuatı ve kişisel verilerin korunması mevzuatı. TMF veri ve entegrasyon modeli olabilir; kuralları ise bunlar belirler.

## Kaynaklar

- [TMF651 Agreement Management API v4.0.0 şeması (GitHub'da tmforum-apis)](https://github.com/tmforum-apis/TMF651_AgreementManagement)
- [TM Forum Open API dizininde TMF651](https://www.tmforum.org/oda/open-apis/directory/TMF651)
- [TM Forum Engage: "TMF651 agreement management: what states to use"](https://engage.tmforum.org/discussion/tmf651-agreement-management-what-states-to-use)
- [TMF667 Document Management API User Guide v4.0.0](https://www.tmforum.org/resources/guidebook/tmf667-document-management-api-user-guide-v4-0-0/)
