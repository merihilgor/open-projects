English: [README.md](README.md)

# Küresel Maaş Ödeme Dengesi (KMÖD): Kademeli Maaş Ödeme Günleri

## Özet

Küresel Maaş Ödeme Dengesi (KMÖD) modeli, dünya genelinde özel ve tüzel kuruluşlardaki çalışan nüfusunu dört eşit gruba ayırmayı ve maaş ödeme tarihlerini ay içinde dengeli biçimde dağıtmayı öngörür. Buna göre çalışanlar maaşlarını ayın 7., 14., 21. veya 28. gününde alır. Böylece tüketici harcamaları, faturalama (billing), gecikmeli tahsilat ihtar süreçleri (dunning), lojistik trafiği ve veri merkezi işlem yükü ay geneline yayılır. Bu da enerji tüketimini ve karbon emisyonunu azaltır, ticari döngüyü istikrara kavuşturur ve dijital ödeme sistemlerinin güvenilirliğini artırır.

## Sorun

Günümüzde dünya genelinde hem özel hem tüzel kuruluşlarda çalışanların maaşları sıklıkla ayın belirli bir gününe, tipik olarak ilk yarısına (genellikle 1'i ila 15'i arası) yoğunlaşarak ödenmektedir. Tüketici davranışı da bu tarihe göre şekillenmektedir:

- Faturalama sistemlerinde 4 fatura döngüsü (bill cycle) bulunsa da kredi kartı kullanan çalışanlar, kart kesim tarihini maaş gününden hemen sonraki tarihlere ayarlamaktadır. Kredi kartı kullanmayanlar da alışverişlerini maaş aldıktan sonraki zamanlara ertelemektedir.
- Bunun sonucunda talep, faturalama ve ödeme faaliyetleri ayın belli günlerine yığılmaktadır.

Bu yoğunlaşma birbiriyle bağlantılı şu kritik sorunlara yol açmaktadır.

**Lojistik ağında yoğunluk ve enerji tüketimi**
- Maaş ödemesinin ardından tüketici harcamaları hızla artar. Bu artış, özellikle e-ticaret ve perakende sektörlerinde lojistik (kargo, teslimat) trafiğini anlık olarak yükseltir.
- Bu yoğunluk fosil yakıtlı araç kullanımını, trafik sıkışıklığını ve operasyonel verimsizlikleri artırır. Enerji sarfiyatı ve karbon salınımı da doğrudan yükselir.

**Dijital ödeme ve faturalama sistemlerinde aşırı yük**
- Fatura çıkarma (billing) sistemleri ile gecikmiş ödemeler için çalışan gecikmeli tahsilat ihtar süreçleri (dunning), ayın belirli günlerine yığılan işlem hacmi nedeniyle aşırı yüklenme (spiking) yaşamaktadır.
- Bu durum veri merkezlerinde (Datacenter) performans sorunlarına yol açmaktadır. Ani işlem artışını karşılamak için gereksiz yere ölçeklenen (scaling) sunucu kaynakları da aşırı enerji tüketmektedir.
- Yüksek enerji tüketimi ek soğutma ihtiyacı doğurmakta ve veri merkezlerinin karbon ayak izini artırmaktadır.

**Ekonomik dengesizlik**
- Tüketici harcamalarının ayın belli günlerine yığılması ticari döngüyü kesintili hâle getirmekte, ticari tahmini ve envanter yönetimini zorlaştırmaktadır.

Bu durum, sıfır karbon hedeflerine ulaşma çabalarıyla çelişen, öngörülebilir bir çevresel yüke işaret etmektedir.

## Önerilen Çözüm

Çözüm, çalışanların maaş ödeme tarihlerini küresel ölçekte, özel ve tüzel kuruluşlar arasında dört eşit gruba (çeyreğe) ayırarak dengeli bir ödeme takvimine geçilmesidir. Her grup maaşını ayın farklı bir haftasında alır:

| Grup | Çalışan nüfusundaki payı | Maaş ödeme günü |
|------|--------------------------|-----------------|
| Grup 1 | %25 | Ayın 7. günü |
| Grup 2 | %25 | Ayın 14. günü |
| Grup 3 | %25 | Ayın 21. günü |
| Grup 4 | %25 | Ayın 28. günü |

Bu düzenleme, dunning faaliyetleri de dahil olmak üzere hem ticaretteki hem de online sistemlerdeki yükü dengeler. Böylece daha çevreci ve ekonomik açıdan daha dengeli bir ekosistem oluşur.

## Nasıl Çalışır

1. **Gruplara ayırma:** Dünya genelindeki tüm çalışan nüfusu, özel ve tüzel kuruluşlar arasında dengeli biçimde dört eşit gruba (%25, %25, %25, %25) ayrılır.
2. **Ödeme takvimi:** Her grup maaşını sabit bir günde alır: ayın 7., 14., 21. veya 28. günü.
3. **Harcamalar maaş gününü izler:** Alışverişler maaş gününün hemen ardından yoğunlaştığı için tek büyük tepe yerine dört küçük ve dengeli tepe oluşur. Böylece tüketici harcamaları ve buna bağlı lojistik trafiği ay geneline yayılır.
4. **Fatura döngüleri gruplarla örtüşür:** Kredi kartı kullanıcıları kesim tarihlerini maaş gününden hemen sonraya ayarladığı için doğal olarak dört farklı kesim tarihine dağılır. Bu da faturalama sistemlerindeki mevcut dört fatura döngüsüne yükü dengeli biçimde yayar.
5. **Tahsilat ve ihtar süreçleri de dengelenir:** Faturalamanın ardından tahsilat (collection) gelir. Gecikenler için ayrı bir ikinci iş (job) olarak çalışan gecikmeli tahsilat ihtar süreçleri (dunning) de tek bir tarihe yığılmak yerine dört farklı tarihe yayılır.
6. **Altyapı yükü homojenleşir:** Online ödeme, faturalama ve bankacılık sistemlerindeki işlem yükü homojenleşir. Veri merkezlerinde ani ölçeklenme ihtiyacı azalır; bu da aşırı enerji ve soğutma kullanımını düşürür.

## Uygulama ve Aşamalandırma

1. **İnceleme:** Modelin, Dünya Ekonomik Forumu'nun (WEF) liderliğinde ulusal hükümetler ve büyük küresel kuruluşlarla iş birliği içinde incelenmesi.
2. **Pilot uygulamalar:** Başta G-20 ülkeleri olmak üzere hükümetler, merkez bankaları ve büyük özel sektör oyuncularıyla pilot uygulamaların başlatılması.
3. **Küresel yaygınlaştırma:** Pilot sonuçlarına dayanarak modelin tüm dünyada hayata geçirilmesi.

## Paydaşlar ve Faydalar

**Uygulayıcı paydaşlar ve iş birliği ortakları** (kaynakta belirtildiği biçimiyle):
- Önerilen lider kuruluş olarak Dünya Ekonomik Forumu (WEF)
- Başta G-20 ülkeleri olmak üzere ulusal hükümetler
- Merkez bankaları
- Büyük özel sektör oyuncuları ve küresel kuruluşlar
- Sıfır karbon ve sürdürülebilirlik hedeflerini belirleyen ve takip eden kuruluşlar (ör. Birleşmiş Milletler, Dünya Sürdürülebilir Kalkınma İş Konseyi - WBCSD)

**Beklenen faydalar:**

| Kategori | Faydalar |
|----------|----------|
| Çevresel sürdürülebilirlik | Lojistik ağındaki trafik ve yoğunluk dengelendiği için taşıma yakıtı tüketimi ve karbon emisyonu azalacaktır. Veri merkezlerindeki işlem yükünün homojenleşmesi ani ölçeklenme ihtiyacını ve aşırı enerji/soğutma kullanımını önleyecek, böylece veri merkezlerinin karbon ayak izini önemli ölçüde düşürecektir. |
| Ekonomik denge | Tüketici harcamaları ve ticari faaliyetler ay geneline yayılacağı için daha istikrarlı bir ticari döngü oluşacaktır. Faturalama (billing), tahsilat ve gecikmeli tahsilat ihtar süreçleri (dunning) dört farklı tarihe yayılacağından finansal operasyonların ve envanter yönetiminin verimliliği artacaktır. |
| Finansal verimlilik | Kredi kartı kullanıcıları, maaş günlerinden hemen sonraki kesim tarihlerini dört farklı seçeneğe yayma esnekliği kazanacak; bu da finansal planlamayı iyileştirecektir. |
| Sistem performansı ve güvenilirliği | Online ödeme, faturalama ve bankacılık sistemlerindeki yük ayın belli günlerine yığılmak yerine dengeli dağılacaktır. Bu, performansı ve güvenilirliği artıracak, hizmet kesintisi riskini azaltacaktır. |

KMÖD, yalnızca finansal bir düzenleme değildir. Lojistik ve dijital altyapı üzerinden küresel sıfır karbon hedeflerine somut katkı sağlayan, akıllı ve basit bir mekanizmadır. Küresel ticaret ve finansal altyapılar üzerinde yaratacağı sinerji hem ekonomik istikrarı hem de çevresel sürdürülebilirliği destekleyecek, ekonomik büyüme ile çevresel sorumluluğu birleştiren sürdürülebilir bir geleceğin inşasına katkı sunacaktır.

## Köken

İlk olarak Merih İlgör tarafından bir kamu politikası önerisi olarak hazırlanmıştır (Aralık 2025); burada açık bir proje fikri olarak yayımlanmaktadır.

## Lisans

Bu çalışma, ticari olmayan kullanım için CC BY-NC-SA 4.0 lisansı ile lisanslanmıştır; bkz. [LICENSE](LICENSE). Ticari kullanım bir gelir paylaşımı anlaşması gerektirir; bkz. [COMMERCIAL-LICENSE.md](COMMERCIAL-LICENSE.md).
