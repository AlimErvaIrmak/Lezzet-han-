# Lezzet Hanı — Yapım Ajanları & Çalışma Planı

> **Amaç:** `GDD_Lezzet_Hani.md` dokümanını üretime dönüştürecek uzman ajanları (iş kollarını) tanımlamak.
> **Kullanım:** Her ajan; ister bir AI kodlama ajanına (ör. *Claude Code*), ister bir insan ekip üyesine verilebilecek odaklı bir **görev tüzüğüdür**. Bir orkestratör (sen veya bir lead ajan) görevleri sıralar, çıktıları birleştirir.
> **Tek doğruluk kaynağı:** GDD. Her ajan kararını GDD'nin ilgili bölümüne dayandırır; sapma olursa GDD güncellenir, sonra kod.

---

## Nasıl Çalışır (Orkestrasyon Modeli)

1. **Orkestratör** GDD'yi okur, sırayı belirler, ajanlara iş açar, çıktıları entegre eder, çakışmaları çözer.
2. Her ajanın net bir **girdisi** (GDD bölümü + diğer ajanların çıktısı), **çıktısı** (kod/asset/doküman) ve **bitti tanımı** (Definition of Done) vardır.
3. Ajanlar **veri sözleşmeleri** üzerinden buluşur: içerik (yemek/şehir/kart) şemaları, ekonomi parametre tablosu, event şeması, string tabloları. Bu sözleşmeler önce kilitlenir.
4. **Kritik kural:** Önce *gri-kutu* (placeholder) ile oynanış kanıtlanır; sanat/ses/içerik dolusu sonra gelir. "Eğlenceli mi?" sorusu sanat gelmeden cevaplanmalı.

---

## Bağımlılık / Sıra

```
A0 Orkestratör  ──> herkesi koordine eder
        │
A1 Çekirdek Sistemler ──> A2 Ekonomi ile çift sözleşme (parametre tablosu)
        │                         │
        ├──> A3 UX/UI (HUD, diorama nav, FTUE)
        │
A4 İçerik & Lokalizasyon ──> veri şemaları (A1'in tükettiği)
A5 Sanat Pipeline ──> A3'ün yerleştirdiği asset'ler
A6 Ses
A7 LiveOps & Monetizasyon (stub/kapalı) ──> A1 + A2 hook'larına bağlanır
A8 QA & Denge ──> hepsini test eder; A2 ile denge döngüsü
A9 Build / DevOps ──> CI + store hazırlık; baştan kurulur
```

**Kaba sıralama:** A0 + A9 (zemin) → A1↔A2 (oynanış + ekonomi) → A3 (oyuncu görür) → A4/A5/A6 (içerik dolusu) → A7 (kapalı altyapı) → A8 (sertleşme) → soft-launch.

---

## Ajan Tüzükleri

### A0 — Orkestratör / Teknik Lider
- **Misyon:** GDD'yi yürütülebilir iş paketlerine bölmek, mimari kararları sahiplenmek, sırayı ve entegrasyonu yönetmek.
- **Girdi:** Tüm GDD; ajan çıktıları.
- **Çıktı:** İş kırılımı, mimari karar kaydı (ADR), entegrasyon planı, sözleşme dosyaları (şemalar).
- **Bitti tanımı:** Her milestone için ajanlar arası sözleşmeler kilitli, kör nokta yok, build bütünleşiyor.

### A1 — Çekirdek Sistemler (Unity / C#)
- **Misyon:** Tripod çekirdek döngüsünü kurmak: mutfak katları → servis asansörü → salon; otomasyon (yöneticiler); idle gelir; kayıt/offline.
- **Girdi:** GDD §4 (core loop), §5 (matematik), §11 (teknik); A2'nin parametre sözleşmesi; A4'ün içerik şeması.
- **Çıktı:** Veri-odaklı oynanış kodu; istasyon/üretim/asansör/salon sistemleri; manager otomasyonu; idle hesabı (zaman damgası farkı, darboğaz min'i); kayıt/yükleme + anti-tamper iskeleti; big-number entegrasyonu.
- **Bitti tanımı:** Placeholder sayılarla tam tur dönüyor; darboğaz doğru hesaplanıyor; offline gelir doğru; kayıt güvenilir.
- **Riskler:** Oyun/UI mantığını ayrı tut (test edilebilirlik); darboğaz hesabı tek otoriter fonksiyon olsun.

### A2 — Ekonomi & Denge
- **Misyon:** Para birimleri, maliyet/gelir eğrileri, darboğaz dengesi, renovasyon matematiği; hepsini remote-config'le ayarlanabilir kılmak.
- **Girdi:** GDD §5, §6, §12; A1'in sistem API'leri.
- **Çıktı:** Parametre tablosu (taban değerler, üstel oranlar, prestige çarpan formülü); ekonomi simülasyon tablosu/aracı; ayarlanabilir kadranlar (kapalı monetizasyon dahil).
- **Bitti tanımı:** "Bir yükseltme daha" temposu çalışıyor; hiçbir istasyon kalıcı darboğaz değil; renovasyon zamanlaması anlamlı bir karar.
- **Riskler:** Erken/geç renovasyon dengesi; enflasyon/duvar (wall) noktaları.

### A3 — UX / UI
- **Misyon:** Kat kat izometrik diorama navigasyonu (yukarı kaydırma), **üretim HUD'u** (darboğaz okunabilirliği), FTUE, menüler (kitap/ansiklopedi/harita/yöneticiler).
- **Girdi:** GDD §8, §4; A1 sistem durumu; A5 asset'leri.
- **Çıktı:** Navigasyon, kalıcı tripod özet çubuğu + darboğaz vurgusu, asansör durum şeridi, FTUE akışı, menü ekranları.
- **Bitti tanımı:** Oyuncu tek ekranda olmasa da darboğazı 2 saniyede görebiliyor; ilk 60 sn döngüyü öğretiyor.
- **Riskler:** Bu MVP'nin #1 UX riski (GDD §16-1). Erken kullanıcı testi şart.

### A4 — İçerik & Lokalizasyon
- **Misyon:** Veri-odaklı içerik şemaları + İstanbul içerik dilimi + yemek kitabı/ansiklopedi verisi + TR string tabloları (i18n-hazır).
- **Girdi:** GDD §7, §6.2, §11 (veri-odaklı), §16-4/5 (otantiklik/saygı).
- **Çıktı:** Yemek/şehir/kart şemaları (ScriptableObject/JSON); İstanbul yemekleri (~6–8) ve 1. ansiklopedi seti; kart metinleri; tüm UI metni string tablosunda.
- **Bitti tanımı:** Non-engineer yeni içerik ekleyebiliyor; metinler tablodan; İstanbul seti tam.
- **Not:** Otantik içerik araştırması burada toplanır; gerekirse kültür danışmanı (GDD §16-5).

### A5 — Sanat Pipeline
- **Misyon:** 2.5D izometrik han iç mekanı asset'leri; kat/diorama parçaları; dekor (halı/çini/fresk…); şehir paletleri; atlas/optimizasyon.
- **Girdi:** GDD §9, §8.1; A4 kart/dekor listesi.
- **Çıktı:** İzometrik kat dioramaları, karakter/aşçı/müşteri görselleri, dekor objeleri, UI kit, atlaslar.
- **Bitti tanımı:** Siluetler net, üretim durumu görsel okunur, mobilde performanslı; İstanbul seti tam.
- **Riskler:** Maliyet/kapsam (GDD §16-3); önce tek kat şablonu, sonra çoğaltma.

### A6 — Ses & Müzik
- **Misyon:** Bölgesel müzik temaları (İstanbul), ortam sesleri (ocak, çay, han), UI sesleri.
- **Girdi:** GDD §10.
- **Çıktı:** İstanbul tema müziği, ambient katmanlar, UI/etkileşim sesleri, ses yöneticisi entegrasyonu.
- **Bitti tanımı:** Sakin idle atmosferi; UI eylemleri tatmin edici geri bildirim veriyor.

### A7 — LiveOps & Monetizasyon (Hazır, Kapalı)
- **Misyon:** Analytics event şeması, remote config, reklam/IAP mediation **stub**'ları — hepsi v1'de kapalı ama bağlı.
- **Girdi:** GDD §12, §13, §5.4.
- **Çıktı:** Event taksonomisi (FTUE, retention, ekonomi olayları), remote config anahtarları (oran/kadran/etkinlik), ödüllü reklam & IAP yuvaları (gizli/etkisiz), açma mekanizması.
- **Bitti tanımı:** Metrikler akıyor; kadranlar uzaktan ayarlanabilir; monetizasyon "tek anahtarla" açılabilir durumda ama kapalı.

### A8 — QA & Denge Testi
- **Misyon:** Test stratejisi; oynanış/ekonomi denge testleri; cihaz performansı; kayıt/offline doğrulama.
- **Girdi:** Tüm sistemler; A2 ile denge döngüsü.
- **Çıktı:** Test planı, otomatik testler (sistem mantığı), denge test raporları, performans/cihaz matrisi, hata kayıtları.
- **Bitti tanımı:** P0 akış hatasız; ekonomi temposu hedefte; düşük-orta cihazda akıcı; idle/kayıt sömürüye kapalı.

### A9 — Build / DevOps
- **Misyon:** CI/CD, build pipeline, sürüm yönetimi, store hazırlık (iOS/Android).
- **Girdi:** GDD §11; mağaza politikaları.
- **Çıktı:** Otomatik build, imzalama, dağıtım kanalları (internal/TestFlight/Play track), sürümleme, çökme raporlama.
- **Bitti tanımı:** Her merge'te yeşil build; tek komutla test sürümü dağıtılabiliyor; soft-launch'a hazır.

---

## Sözleşme Dosyaları (Önce Bunlar Kilitlenir)

Ajanlar bu paylaşılan sözleşmeler üzerinden buluşur; değişiklik sözleşmeden geçer:
1. **İçerik şeması** — yemek / şehir / kart / yönetici alanları (A4 ↔ A1/A5).
2. **Ekonomi parametre tablosu** — taban değer/oran/çarpan anahtarları (A2 ↔ A1/A7).
3. **Event taksonomisi** — analytics olay adları/alanları (A7 ↔ herkes).
4. **String tablosu** — tüm UI metni anahtarları (A4 ↔ A3).
5. **Kayıt formatı** — durum + zaman damgası şeması, sürümleme (A1 ↔ A8/A9).

---

## Milestone ↔ Ajan Eşlemesi

| Milestone (GDD §15) | Başrol | Destek |
|---|---|---|
| 1. Prototip (gri-kutu) | A1, A2 | A0, A3 (kaba) |
| 2. Dikey dilim | A1, A2, A3 | A4 (şema), A9 |
| 3. İçerik + sanat geçişi | A3, A4, A5, A6 | A1 |
| 4. MVP sertleşmesi | A2, A8, A4 (TR) | A1, A9 |
| 5. Soft-launch & ölçüm | A7, A9, A8 | A0 (karar) |

---

## Bir Sonraki Adım (Öneri)

İstersen sıradaki turda şunlardan birini birlikte derinleştirelim:
- **A2 ekonomi parametre tablosunu** somut sayılarla taslaklamak (oynanış temposunu erken görmek için), veya
- **A1 + A3 için "gri-kutu prototip" görev paketini** uygulanabilir adımlara bölmek (ilk çalışan tur), veya
- **A4 içerik şemasını** birebir alan listesi olarak kilitlemek (senin yemek araştırmanın oturacağı yapı).

Hangisi seni en çok ileri taşır?
