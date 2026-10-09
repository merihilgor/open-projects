# Changelog: Secure Contract Flow

All notable changes to this project idea are recorded here, newest first.
Bu proje fikrindeki önemli değişiklikler burada, en yenisi üstte olacak şekilde kaydedilir.

## 2026-10-09

### Added / Eklendi
- **Contract lifecycle:** the unique document number stays the contract's anchor after signing. A new section covers attachments and annexes, amendments, sub-contracts, assignment, renewal and extension, expiration, mutual cancellation, one-sided termination, suspension, completion and disputes, each recorded as a linked record with a clear current status. Includes proportionality (rights-changing events go through the notary flow, simple status events are recorded online) and third-party status checks.
  **Sözleşmenin yaşam döngüsü:** Benzersiz belge numarası imzadan sonra da sözleşmenin dayanağı olarak kalır. Yeni bölüm ekleri, tadilleri, alt sözleşmeleri, devri, yenileme ve süre uzatımını, sürenin dolmasını, karşılıklı iptali, tek taraflı feshi, askıya almayı, tamamlanmayı ve uyuşmazlıkları kapsar; her biri bağlantılı bir kayıt olarak, açık bir güncel durumla tutulur. Orantılılık (hakları değiştiren olaylar noter akışından geçer, basit durum olayları çevrim içi kaydedilir) ve üçüncü kişilerin durum kontrolü de eklendi.
- A note that this proposal is an initial design and a real implementation should adapt it to its own requirements.
  Bu önerinin bir başlangıç tasarımı olduğu ve gerçek bir uygulamanın onu kendi gereksinimlerine göre uyarlaması gerektiğine dair not.
- **TM Forum Open API mapping (implementation note):** a separate, non-binding EN/TR note mapping the flow and its lifecycle to TMF651 Agreement Management (checked against the v4.0.0 schema) and related APIs, with an absence mapping of what has no counterpart (version lock, comprehension check, notary act, signer reference, typed contract links, termination and suspension details, audit, consent) and conflict points (DELETE, PATCH). Linked from the lifecycle section.
  **TM Forum Open API eşlemesi (uygulama notu):** Akışı ve yaşam döngüsünü TMF651 Agreement Management (v4.0.0 şemasıyla karşılaştırıldı) ve ilgili API'lerle eşleyen, bağlayıcı olmayan ayrı bir EN/TR not. Karşılığı olmayanların eksiklik eşlemesini (sürüm kilidi, idrak testi, noter işlemi, imzalayan referansı, türü belli sözleşme bağlantıları, fesih ve askı ayrıntıları, denetim, izin) ve çatışma noktalarını (DELETE, PATCH) içerir. Yaşam döngüsü bölümünden bağlantı verildi.
- Solution step 10, two comparison-table rows, a lifecycle phasing step, a risk row on changes made outside the system, and lifecycle keywords.
  Çözüm adımı 10, iki karşılaştırma tablosu satırı, yaşam döngüsü için bir uygulama aşaması, sistem dışı değişikliklere ilişkin bir risk satırı ve yaşam döngüsü anahtar kelimeleri.

## 2026-10-08

### Added / Eklendi
- **Flexible signing:** depending on the means available, the signature at the notary can be a wet signature on paper, or a digital or biometric signature on the digital document. The version lock and comprehension check apply the same way.
  **Esnek imza:** İmkânlara göre noterdeki imza, kâğıt üzerinde ıslak imza ya da dijital belge üzerinde dijital veya biyometrik imza olabilir. Sürüm kilidi ve idrak testi aynı şekilde uygulanır.
- New risk row on the security of digital and biometric signatures, and the keywords "digital signature" and "biometric signature".
  Dijital ve biyometrik imzaların güvenliğine ilişkin yeni risk satırı ve "dijital imza", "biyometrik imza" anahtar kelimeleri.

### Changed / Değişti
- The summary rule now reads "pre-approval before signing, a comprehension check during signing" instead of referring only to the wet signature.
  Özet kural artık yalnızca ıslak imzayı değil, "imzadan önce ön onay, imza sırasında idrak testi" ifadesini kullanıyor.
- The notary now "uses" the locked version: it can be printed or opened as a digital document. The illustration was updated to match.
  Noter artık kilitli sürümü "kullanır": çıktısı alınabilir veya dijital belge olarak açılabilir. Görsel buna göre güncellendi.

## 2026-10-07

### Added / Eklendi
- Initial publication: a value threshold for private contracts (reviewed yearly), online pre-review with versioning, mutual pre-approval locked under a unique document number, the notary using exactly that version, and a comprehension check at signing. The same flow applies to powers of attorney and lawyer mandates. EN/TR README, illustration, CC BY-NC-SA 4.0 license and commercial license.
  İlk yayın: adi sözleşmeler için her yıl gözden geçirilen tutar sınırı, sürümlemeli çevrim içi ön inceleme, benzersiz belge numarasıyla kilitlenen karşılıklı ön onay, noterin tam olarak o sürümü kullanması ve imza sırasında idrak testi. Aynı akış vekâletname ve avukat vekâleti için de geçerlidir. EN/TR README, görsel, CC BY-NC-SA 4.0 lisansı ve ticari lisans.
