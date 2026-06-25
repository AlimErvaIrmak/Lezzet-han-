# Lezzet Hanı — CLAUDE.md

## Oyun Nedir
**Lezzet Hanı: Türk Mutfağı Tycoon** — İpek Yolu üzerinde tarihî bir han işleten mobil idle/incremental
management tycoon. Oyuncu hanı kat kat yukarı büyütür: zemin katta simit-çay, üst katlarda Osmanlı saray
mutfağı. Çekirdek döngü bir **tripod**tur — Mutfak Katları (üretim) → Servis Asansörü (taşıma/darboğaz
kalbi) → Salon (satış) — üçü tek organizma gibi çalışır, en zayıf halka geliri sınırlar. Yöneticiler
atanınca han otomatikleşir ve oyuncu kapalıyken de **idle gelir** üretir. İlerlemeyle **Renovasyon**
(prestige) kalıcı çarpan kazandırır, İpek Yolu'nda yeni şehirler (Erzurum, Ankara, Gaziantep…) açılır,
**Yemek Kitabı** ve **Türk Dünyası Ansiklopedisi** otantik içerikle dolar. MVP kapsamı tek şehir: İstanbul
Hanı tam oynanır.

Detaylı tasarım kararları, ekonomi modeli, KPI'lar ve açık sorular için → [`GDD_Lezzet_Hani.md`](./GDD_Lezzet_Hani.md).
Üretim iş bölümü (ajan tüzükleri, bağımlılık sırası, sözleşme dosyaları) için → [`Yapim_Ajanlari_Calisma_Plani.md`](./Yapim_Ajanlari_Calisma_Plani.md).

## Hedef
- **Platform:** Mobil öncelikli (iOS + Android); mimari ileride PC/Steam'i engellemeyecek şekilde kurulur.
- **Motor:** Unity 6.3 LTS (`6000.3.18f1`), URP 2D.
- **Dil:** C#.
- **Pazar:** Türkiye-önce, sonra global. TR ilk dil, tüm metin string tablolarında (i18n-hazır).

## Kodlama Kuralları
- **Veri-odaklı içerik:** Yemekler, şehirler, kartlar, yöneticiler `ScriptableObject`/JSON ile tanımlanır;
  içerik koddan ayrı tutulur ki non-engineer içerik ekleyebilsin.
- **Oynanış mantığını UI'dan ayır:** Sistem mantığı (üretim/taşıma/satış/idle hesabı) test edilebilir,
  Unity sahne/UI bağımlılığı olmayan sınıflarda yaşar.
- **Darboğaz hesabı tek otoriter fonksiyonda yaşar:** `efektif gelir = min(mutfak çıkışı, asansör
  kapasitesi, salon satışı)` — bu mantık çoğaltılmaz, UI ve ekonomi aynı kaynaktan okur.
- **Büyük sayılar:** Ekonomi katmanı standart `float`/`double` taşmasına karşı büyük-sayı kütüphanesi
  (BigDouble vb.) üzerine kurulur.
- **Offline/idle hesabı:** Kapanış/açılış zaman damgası farkına dayanır; kayıt formatı sürümlenir ve
  anti-tamper'a karşı savunmasız bırakılmaz.
- **Monetizasyon ve LiveOps hazır ama kapalı:** Reklam/IAP/remote-config hook'ları koddadır ama v1'de
  kullanıcıya gösterilmez/etkin değildir; açma kararı remote config ile verilir.
- **Sözleşmeler önce kilitlenir:** İçerik şeması, ekonomi parametre tablosu, event taksonomisi, string
  tablosu ve kayıt formatı değişmeden önce ilgili ajanlar/iş kolları arasında uyumlu kalmalıdır (bkz.
  `Yapim_Ajanlari_Calisma_Plani.md` → "Sözleşme Dosyaları").
- Türk kültürel içerik (yemek, halı/çini/fresk vb.) otantik ve saygılı temsil edilmeli; turistik klişeden
  kaçınılmalı.

## Doküman Konumları
- GDD: `GDD_Lezzet_Hani.md` (kök dizin)
- Çalışma planı / ajan tüzükleri: `Yapim_Ajanlari_Calisma_Plani.md` (kök dizin)
- A1 (Çekirdek Sistemler) subagent tanımı: `.claude/agents/core-systems.md`
