# Lezzet Hanı — Oyun Tasarım Dokümanı (GDD)

> **Çalışma başlığı:** *Lezzet Hanı: Türk Mutfağı Tycoon* (başlık kesinleşmedi)
> **Tür:** Idle / Incremental Management Tycoon
> **Platform:** Mobil öncelikli (iOS + Android) · Motor: Unity (C#)
> **Pazar:** Türkiye-önce, sonra global
> **Doküman durumu:** v0.1 — İskelet / üzerinde anlaşılan çerçeve
> **İlham referansı:** *Idle Miner Tycoon* (Kolibri Games) çekirdek döngüsü, dikey olarak ters çevrilmiş hâli

---

## 1. Yüksek Konsept

Oyuncu, İpek Yolu üzerinde tarihî bir **han** işletir. Han kat kat yukarı yükselir: avlu ve zemin katta simit-çay satılır, en üst katlarda Osmanlı saray mutfağı sunulur. Oyuncu mutfak katlarını kurar, servis asansörünü ve salonu büyütür, aşçıbaşı ve servis müdürleri atayarak işi otomatikleştirir; han o yokken bile çalışmaya devam eder (idle gelir). Han doldukça **renovasyon** ile kalıcı çarpan kazanır ve İpek Yolu boyunca yeni şehirlerde (Erzurum, Ankara, Gaziantep…) yeni hanlar açar. İlerledikçe **yemek kitabı** Türk mutfağıyla, **Türk Dünyası Ansiklopedisi** halı, çini, fresk gibi kültürel kartlarla dolar.

**Tek cümle:** *Idle Miner Tycoon'un dikey üretim zincirini yukarı çevirip, madenin yerine İpek Yolu üzerinde yükselen bir Türk hanını koyan; ilerledikçe gerçek bir Türk mutfağı ve kültür ansiklopedisi inşa eden mobil idle tycoon.*

---

## 2. Tasarım Sütunları (Design Pillars)

Her tasarım kararı bu dört sütundan en az birine hizmet etmelidir. Bir özellik hiçbirine hizmet etmiyorsa kapsam dışıdır.

1. **Tatmin edici tripod akışı.** Mutfak → asansör → salon dengesini kurmak; bir tarafı güçlendirince diğerinin tıkanması, oyuncuya hiç bitmeyen "şimdi neyi yükselteyim?" kararı verir.
2. **Otantik Türk kültürü, nostaljik ton.** İçerik turistik klişe değil; Türk oyuncuya tanıdık, sıcak ve doğru gelmeli. Kültür, dekor olarak değil oyunun dokusu olarak içeride.
3. **Sürdürülebilir ilerleme.** İçerik (yemek + ansiklopedi + şehirler) oyuncuyu sıkmadan ileri taşır; meta döngü uzun vadeli hedef verir.
4. **Patron hissi, işçi değil.** Tıklama bir süre sonra anlamını yitirir; oyuncu otomasyonu ve ekonomiyi yöneten kişidir.

---

## 3. Hedefler ve Hedef-Olmayanlar

### Hedefler
- Oyuncunun ilk 5 dakikada çekirdek döngüyü (üret → taşı → sat → yükselt) anlaması ve "bir yükseltme daha" hissine kapılması.
- Otantik bir Türk mutfağı ve kültür koleksiyonunun ilerlemeyle organik olarak dolması (anlatımı dayatmadan keşif yoluyla).
- Tripod darboğazını oyunun ana karar motoru yapmak.
- Mimariyi en baştan (a) lokalizasyona ve (b) monetizasyona *hazır* kurmak — ikisini de sonradan açmak yeniden tasarım gerektirmemeli.

### Hedef-Olmayanlar (bu sürümde)
- **Gerçek zamanlı çok oyunculu / PvP** — kapsam dışı; oyun rekabetçi değil, endless progression. (Sebep: erken aşamada karmaşıklık ve sunucu maliyeti.)
- **Aktif tıklama becerisine dayalı oyun** — IMT gibi, tıklama opsiyonel hızlandırıcı; ana akış otomasyon. (Sebep: idle felsefesine aykırı.)
- **Agresif monetizasyon dürtüleri (v1)** — para sistemi tasarlanır ama kadran kapalı. (Sebep: "önce oyun otursun".)
- **Tam yemek/şehir kataloğu (v1)** — içerik araştırması oyuncu/ekip tarafından sonra detaylandırılacak; GDD veri-odaklı yapı verir, dolu içeriği değil.
- **3B serbest kamera** — sabit, kaydırılabilir izometrik diorama yeterli. (Sebep: maliyet ve okunabilirlik.)

---

## 4. Çekirdek Oyun Döngüsü (Core Loop)

Üç istasyonlu **tripod**. Üçü tek bir organizmadır; en zayıf halka tüm geliri sınırlar (darboğaz).

### 4.1 Mutfak Katları (Üretim)
- Her kat **bir yemek** üretir. Aşağıdan yukarıya yemekler lüksleşir (simit/çay → … → saray mutfağı).
- Her katta: çalışan aşçı(lar), üretim hızı, kat seviyesi. Yükseltme = daha hızlı/değerli üretim.
- Kat, üzerinde "hazır tabak" biriktirir; asansör gelip alana kadar bekler (kapasite dolarsa üretim durur → darboğaz sinyali).

### 4.2 Servis Asansörü (Taşıma / Darboğaz Kalbi)
- Tabakları mutfak katlarından **yukarı**, salona taşır. (Oyunun imza tersine çevirmesi: maden aşağı değil, han yukarı.)
- Parametreler: taşıma kapasitesi, hız, durak sayısı. Mutfak çıktısı asansör kapasitesini aşarsa para buharlaşır.
- Yükseltme = daha fazla tabak, daha hızlı tur.

### 4.3 Salon & Servis (Satış → Nakit)
- Garsonlar tabağı müşteriye servis eder; müşteri öder → **Nakit (Lira)**.
- Parametreler: garson sayısı/hızı, masa kapasitesi, müşteri talebi.
- Salon yavaşsa asansör boşalamaz → zincir tıkanır.

### 4.4 Otomasyon (Yöneticiler)
- Her istasyona yönetici atanır: **Aşçıbaşı** (mutfak), **Hancı/Kilerci** (asansör-lojistik), **Servis Şefi** (salon).
- Üçü de atandığında han tamamen otomatik döner; oyuncu artık yalnızca yatırım kararları verir.

### 4.5 Idle Gelir
- Oyun kapalıyken han çalışır; dönüşte birikmiş Lira sunulur (aktif gelirden düşük bir oranla).
- Hesap: `kapanış zaman damgası` ile `açılış zaman damgası` farkı × tripod'un en düşük (darboğaz) akış hızı.
- "Eve dönüş" animasyonu/ekranı: ne kadar kazandığını göster (geri-yatırım dürtüsü).

> **Şema:** Çekirdek döngünün görsel hâli sohbette çizildi (Mutfak → Asansör → Salon → Nakit → ↻ geri yatırım). Meta döngü şeması Bölüm 6'da tarif edilir.

---

## 5. Ekonomi ve Matematik Modeli

### 5.1 Para Birimleri (öneri — kesinleşmedi)
| Birim | Tür | Kaynak | Harcama |
|---|---|---|---|
| **Lira (₺)** | Yumuşak | Satış, idle gelir | Kat/asansör/salon yükseltmeleri, yeni kat açma |
| **Altın** | Sert | Görev/ödül/günlük; (sonra: IAP) | Anlık açma/hızlandırma, süper yönetici, premium dekor |
| **İtibar** | Prestige | Renovasyon | Kalıcı çarpan ağacı, yönetici becerileri |
| **Mühür** | Etkinlik/koleksiyon | Etkinlik, han kilometre taşları | Ansiklopedi/yemek kitabı kilidi, sınırlı dekor |

### 5.2 Eğriler
- **Yükseltme maliyeti:** üstel artış (ör. `maliyet = taban × oran^seviye`). Oran tripod istasyonları arasında dengelenir ki hiçbiri kalıcı darboğaz olmasın.
- **Gelir ölçeği:** kat değeri yukarı çıktıkça katlanarak artar; üst katların açılması "atlama" hissi verir.
- **Darboğaz matematiği:** efektif gelir = min(mutfak çıkışı, asansör kapasitesi, salon satışı). UI bu min'i görünür kılar (hangi istasyon yavaşlatıyor).
- **Big-number desteği:** çok büyük sayılar (notasyon: 1.2K, 3.4M, 5.6aa…) — özel sayı kütüphanesi gerekir (teknik bölüm).

### 5.3 Renovasyon (Prestige) Matematiği
- Bir hanı belli bir olgunluğa getirince **renovasyon** yapılır: han sıfırlanır, kalıcı **gelir çarpanı** + **İtibar** kazanılır.
- Çarpan, renovasyon anındaki ilerlemeye bağlı (erken renovasyon az çarpan, geç renovasyon zaman kaybı → optimal zamanlama kararı).
- Sınır kararı: IMT'de han başına 5 ile sınırlı. Biz **yumuşak sınır + azalan getiri** öneriyoruz (sonsuz ama her seferinde daha az verimli) — açık soru (Bölüm 14).

### 5.4 Monetizasyona Hazır, Kapalı
- Tüm kaynak/gider akışları, sonradan açılacak gelir kanallarına bağlanabilecek şekilde soyutlanır:
  - **Ödüllü reklam yuvası:** geçici gelir çarpanı ("müşteri akını"), idle ödülü 2×, hızlı açma. (Kapalı.)
  - **IAP:** Altın paketleri, reklamsız, başlangıç paketi, sezon geçişi. (Kapalı.)
- v1'de bu yuvalar koddadır ama kullanıcıya gösterilmez/etkin değildir. Açma kararı remote config ile.

---

## 6. Meta İlerleme

### 6.1 Renovasyon Döngüsü
Han olgunlaşır → renovasyon → çarpan + İtibar → daha hızlı yeniden büyüme → yeni kat/şehir hedefi. (Bkz. 5.3.)

### 6.2 İpek Yolu / Şehir Açma
- Dünya haritası = İpek Yolu rotası. Her şehir = kendi bölgesel mutfağına sahip ayrı bir han.
- Açılış sırası (öneri): **İstanbul → Erzurum → Ankara → Gaziantep → …**
- Her şehir: yeni yemek seti (yemek kitabına eklenir), yeni kültürel set (ansiklopediye eklenir), yeni atmosfer/palet.
- **MVP notu:** v1'de yalnızca İstanbul tam oynanır; harita ve geçiş mimarisi hazır ama diğer şehirler "yakında" kilidiyle.

### 6.3 Yönetici / Beceri Ağacı
- İtibar ile açılan kalıcı yükseltmeler: idle gelir çarpanı, otomasyon hızı, ödül süreleri, kat-tipi bonusları.
- Süper yöneticiler (özel, isimli karakterler): belirli yemek/kat tiplerine güçlü bonus. (IMT'deki Super Manager karşılığı.)

---

## 7. İçerik Sistemleri

### 7.1 Yemek Dikey Yayı
- Kat = yemek; yukarı = lüks. Açılış yemeği: **simit + çay**.
- Bölgesel yaylar şehir bazında genişler. (Dolu liste oyuncu/ekip araştırmasıyla doldurulacak — GDD veri yapısını verir, içeriği değil.)
- Veri-odaklı: her yemek bir kayıt (ad, bölge, taban değer/maliyet, görsel, kitap metni).

### 7.2 Yemek Kitabı (Cookbook)
- Açılan her yemek bir **tarif kartı** olarak kitaba düşer: görsel + kısa lezzet/kültür notu.
- İlerleme ödülü: kitap tamamlama yüzdeleri Mühür/İtibar verir.
- Koleksiyon, ilerlemenin doğal yan ürünü (ekstra grind değil).

### 7.3 Türk Dünyası Ansiklopedisi
- Kültürel kartlar: **halı, kilim, çini, fresk, motif, hat, müzik aleti, mimari öğe** vb.
- Kartlar; kat açma, renovasyon, şehir kilometre taşları ve etkinliklerle düşer (IMT koleksiyon mini-oyununun tematik karşılığı).
- **İstanbul ilk seti** v1 kapsamında (MVP).

### 7.4 Dekorasyon Sistemi
- Toplanan kültürel öğeler hanın katlarına **dekor** olarak yerleştirilir → izometrik diorama'da görünür.
- Dekor çift işlevli: estetik + küçük üretim/gelir buff'u. (Sütun 2 ve 3'e hizmet eder.)

---

## 8. UX / UI

### 8.1 Ana Görünüm: Kat Kat İzometrik Diorama
- Han, kat kat **izometrik diorama** olarak çizilir; oyuncu **yukarı kaydırarak** katlar arasında gezer (asansörü takip eder hissi).
- Her kat zengin bir iç mekan: aşçılar, ocaklar, dekor, müşteriler.

### 8.2 Tripod Okunabilirliği (kritik tasarım problemi)
> Tüm katlar tek ekranda görünmediği için darboğazı oyuncuya **kalıcı bir üretim HUD'u** ile göstermeliyiz:
- Üstte/yanda sabit özet çubuğu: mutfak çıkışı, asansör kapasitesi, salon satışı — ve hangisinin **darboğaz** olduğunu vurgulayan işaret.
- Darboğaz olan istasyona "buraya bak" yönlendirmesi (ok / pulsing rozet).
- Asansör durumu (dolu/boş, hız) her zaman görünür bir şerit olarak.

### 8.3 FTUE (İlk Deneyim)
- İlk 60 sn: tek mutfak (simit), tek tıkla üretim → asansör → salon → ilk Lira → ilk yükseltme.
- 5. dk: çay katı açılır, ilk yönetici (Aşçıbaşı) atanır, idle kavramı tanıtılır.
- Kademeli açılış: asansör/salon yükseltmeleri, ikinci yönetici, ilk renovasyon teaser'ı.

### 8.4 Menüler
Yemek Kitabı · Ansiklopedi · İpek Yolu Haritası · Yöneticiler/Beceri Ağacı · Dükkân (v1'de pasif) · Ayarlar/Hesap.

---

## 9. Sanat Yönü

- **Stil:** 2.5D izometrik, detaylı han iç mekanı; el-çizimi sıcaklığı.
- **Ton:** Otantik ve nostaljik (turistik değil). Türk dünyası figürleri (halı, çini, fresk, ahşap oyma, bakır) ortamın dokusu.
- **Şehir kimliği:** Her şehir farklı palet/atmosfer (İstanbul: Boğaz/çini mavisi; Erzurum: taş/kar; Gaziantep: bakır/sıcak tonlar).
- **Okunabilirlik > gösteriş:** diorama detaylı ama siluetler net; üretim durumu renkle de okunur.

---

## 10. Ses ve Müzik

- Bölgesel Türk müziği motifleri (şehir temaları); sakin, ambient idle hissi.
- Ortam sesleri: ocak cızırtısı, çay bardağı tıngırtısı, han uğultusu, müşteri sesleri.
- UI sesleri: yükseltme "ka-ching", kart açma, renovasyon zili.

---

## 11. Teknik Tasarım

- **Motor:** Unity (C#). **Platform:** iOS + Android (mobil öncelikli; ileride PC/Steam mimaride engellenmeyecek.)
- **Idle/sayı:** büyük sayı kütüphanesi (BigDouble vb.); tüm ekonomi bunun üstünde.
- **Kayıt/Idle hesabı:** offline-first; kapanışta zaman damgası + durum; açılışta fark üzerinden idle gelir. Kayıt güvenliği/anti-tamper.
- **Veri-odaklı içerik:** yemekler, şehirler, kartlar, yöneticiler ScriptableObject/JSON ile (içerik koddan ayrı; non-engineer'lar dolusunu ekleyebilir).
- **Lokalizasyon (i18n-hazır):** TR ilk dil; tüm metin string tablolarında; yerleşim/dil değişimine hazır UI.
- **LiveOps altyapısı:** remote config (kadran/oran/etkinlik), analytics event şeması, A/B hazır.
- **Reklam/IAP:** mediation katmanı stub/kapalı; sonradan remote ile açılır.
- **Performans:** düşük-orta seviye mobilde 60 fps hedefi; diorama draw call/atlas optimizasyonu.

---

## 12. Monetizasyon (Hazır, Kapalı)

v1'de kullanıcıya gösterilmez; mimaride yer alır.
- **Ödüllü reklam (sonra):** geçici gelir çarpanı, idle ödülü 2×, anlık açma. (IMT'de gelirin büyük kısmı buradan; oturum başı yüksek opt-in.)
- **IAP (sonra):** Altın paketleri, başlangıç paketi, reklamsız, sezon geçişi.
- **İlke:** rekabetçi olmadığı için zaman-hızlandırma satışı "pay-to-win" değil; etik denge gözetilir.

---

## 13. KPI / Başarı Metrikleri

### Öncü (hızlı) göstergeler
- FTUE tamamlama oranı (hedef: yüksek; ölçüm: tutorial bitiş event'i).
- D1 retention, oturum süresi, gün başı oturum sayısı.
- İlk renovasyona ulaşma oranı/süresi.

### Ardıl (yavaş) göstergeler
- D7 / D30 retention.
- (Monetizasyon açılınca) ARPDAU, reklam opt-in, dönüşüm, LTV.

> Hedef değerleri benchmark/ölçüm sonrası netleşir; v1 amacı "oyun oturuyor mu?" sorusunu retention ile yanıtlamak.

---

## 14. MVP / İlk Dikey Dilim Kapsamı

**MVP = Tek şehir tam (İstanbul Hanı).**

### Must-Have (P0)
- [ ] Çekirdek tripod döngüsü (mutfak katları + servis asansörü + salon) tam çalışır.
- [ ] En az ~6–8 mutfak katı/yemeği (simit/çay → üst lüks), veri-odaklı.
- [ ] Üç yönetici tipi + tam otomasyon.
- [ ] Idle gelir + eve dönüş ekranı.
- [ ] Renovasyon (prestige) döngüsü + İtibar + en az temel çarpan ağacı.
- [ ] Yemek Kitabı (İstanbul seti) — açılan yemekler kart olur.
- [ ] Türk Dünyası Ansiklopedisi **1. set (İstanbul)**.
- [ ] Dekorasyon sistemi (temel) — kartlar diorama'da görünür + buff.
- [ ] Kat kat izometrik diorama + üretim HUD + FTUE.
- [ ] Kayıt/offline-idle, TR lokalizasyon, temel ses.
- [ ] İpek Yolu haritası **görünür** (diğer şehirler "yakında" kilidi) + analytics + remote config iskeleti.

### Nice-to-Have (P1, hızlı takip)
- [ ] Süper yöneticiler.
- [ ] Günlük ödül / ilk etkinlik çerçevesi.
- [ ] Mühür ekonomisi ve koleksiyon ödülleri.
- [ ] İkinci şehir (Erzurum) içerik dilimi.

### Future / Architectural Insurance (P2)
- [ ] Reklam/IAP kanallarının açılması.
- [ ] Tüm İpek Yolu şehirleri + bölgesel mutfak yayları.
- [ ] Etkinlik/sezon sistemi, bulut kayıt, global lokalizasyon, PC/Steam.

---

## 15. Yol Haritası / Kilometre Taşları

1. **Prototip (gri-kutu):** tek mutfak + asansör + salon, sahte sayılarla tripod hissi. *Soru: eğlenceli mi?*
2. **Dikey dilim:** İstanbul hanı, otomasyon + idle + 1 renovasyon, placeholder sanat.
3. **İçerik + sanat geçişi:** izometrik diorama, yemek kitabı + 1. ansiklopedi seti, FTUE.
4. **MVP sertleşmesi:** denge (ekonomi eğrileri), kayıt güvenliği, analytics, TR lokalizasyon, soft-launch hazır.
5. **Soft-launch & ölçüm:** retention ölçümü; sonra P1 + monetizasyon kadranını açma kararı.

---

## 16. Riskler ve Açık Sorular

| # | Konu | Soru / Risk | Karar veren |
|---|---|---|---|
| 1 | UX | Tüm katlar tek ekranda olmayınca darboğaz okunabilirliği yeterli mi? HUD tasarımı kritik. | Tasarım + UX |
| 2 | Ekonomi | Renovasyon: sert sınır (5) mı, yumuşak azalan getiri mi? | Ekonomi tasarımı |
| 3 | Kapsam | İzometrik diorama sanat maliyeti yüksek; MVP'de kaç kat/yemek gerçekçi? | Tasarım + Sanat |
| 4 | İçerik | Yemek/şehir kataloğunu kim, hangi kaynaklarla dolduracak (otantiklik)? | İçerik/araştırma |
| 5 | Kültür | Kültürel öğelerin doğru ve saygılı temsili; danışman gerekli mi? | İçerik/araştırma |
| 6 | Para birimi isimleri | Lira/Altın/İtibar/Mühür kesinleşecek mi? | Tasarım |
| 7 | Monetizasyon zamanı | Kadran ne zaman, hangi metrik eşiğinde açılır? | Ürün |

---

## 17. Sözlük / İsimlendirme (öneri)

- **Han / Lezzet Hanı** — oyuncunun işlettiği çok katlı yapı (oyun mekânı + birimi).
- **Kat** — bir yemek üreten istasyon.
- **Servis Asansörü** — tabakları yukarı taşıyan taşıma istasyonu (darboğaz kalbi).
- **Salon** — satış/servis istasyonu.
- **Aşçıbaşı / Hancı / Servis Şefi** — istasyon yöneticileri (otomasyon).
- **Renovasyon** — prestige sıfırlaması (kalıcı çarpan + İtibar).
- **İpek Yolu** — dünya haritası / şehir açma rotası.
- **Yemek Kitabı / Türk Dünyası Ansiklopedisi** — iki koleksiyon sistemi.
- **Lira / Altın / İtibar / Mühür** — yumuşak / sert / prestige / etkinlik para birimleri.

---

*Bu doküman bir iskelettir; her bölüm üretim sürecinde derinleşecek. Bir sonraki adım: bu GDD'yi inşa edecek uzman ajanların/iş kollarının tanımlandığı çalışma planı (ayrı dosya).*
