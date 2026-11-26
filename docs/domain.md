# Domain

> Ortak dil: iş birimi, insanlar ve ajanlar aynı kelimeyi aynı anlamda kullanır. Burada olmayan bir
> terim için ajan kendi anlamını uydurur — eksik terim görürseniz ekleyin. (Bootstrap 2026-11-26.)

## Product & roadmap

POLSAN RADAR, POLSAN yönetimine finansal, idari ve operasyonel riskleri **gerçekleşmeden önce**
gösteren bir karar destek platformudur. Kullanıcılar kurum içidir: üst yönetim, birim yöneticileri,
analiz uzmanları. Tam ihtiyaç listesi: `docs/product-brief.md`.

**MVP = dilim 1–4** (örnek veriyle çalışan, yönetici sunumuna hazır çekirdek). Her dilim ≥ 1 spec.

| # | Dilim | Durum |
|---|---|---|
| 1 | Walking skeleton: sol menülü kabuk → API → DB → örnek veri → dashboard'da 1 gösterge | MVP |
| 2 | Örnek veri seti + risk modeli (skor, seviye, gerekçe, ÖRNEK etiketi) + tüm dashboard göstergeleri | MVP |
| 3 | POLSAN Radar ekranı + risk detay ekranı | MVP |
| 4 | Erken uyarılar (eşikler, liste, bildirim kanalına açık yapı) | MVP |
| 5 | Risk modülleri A–E (her biri ayrı spec) | sonra |
| 6 | Kimlik doğrulama + 5 rol + denetim kaydı | sonra — **gerçek veriden önce zorunlu** |
| 7 | Veri yükleme akışı (Excel/CSV, eşleştirme, doğrulama, geçmiş) | sonra |
| 8 | Raporlar + PDF/Excel dışa aktarım | sonra |

## Ubiquitous language

| Term | Meaning | Kod karşılığı · Notes / not to be confused with |
|---|---|---|
| Risk Kaydı | Bir Varlık için üretilmiş tahmin: modül, Risk Skoru, Risk Seviyesi, gerekçe, dayandığı veriler, önerilen aksiyon, tahmini dönem, örnek veri bayrağı | `RiskRecord` · Erken Uyarı değildir; her Risk Kaydı uyarı üretmez |
| Risk Skoru | 0–100 arası tamsayı olasılık | `RiskScore` · yüzde olarak gösterilir (%82) |
| Risk Seviyesi | Kritik / Yüksek / Orta / Düşük; **yalnızca** skordan eşik tablosuyla türetilir (BR-1) | `RiskLevel.Critical/High/Medium/Low` · elle girilmez |
| Erken Uyarı | Risk Kaydı eşiği aştığında oluşan, durumu takip edilen kayıt: Açık → İnceleniyor → Kapatıldı | `EarlyWarning`, `WarningStatus.Open/InReview/Closed` |
| Risk Modülü | İdari Takip · Yardım Ödeme · Tahsilat · Anormal Hareket · Operasyonel | `RiskModule.AdministrativeFollowUp/AidPayment/Collection/Anomaly/Operational` · kod modülü (architecture.md) ile karıştırmayın |
| Varlık | Riskin ait olduğu kişi/hesap, birim veya işlem grubu | `RiskSubject` (`Person`/`Account`/`Unit`/`TransactionGroup`) |
| Birim | POLSAN organizasyon birimi | `Unit` |
| Kurum Risk Skoru | Modül skorlarının ağırlıklı birleşimi (ağırlıklar Sistem Ayarları'nda) | `InstitutionRiskScore` |
| Finansal / Operasyonel Risk Göstergesi | Finansal modüllerin (İdari Takip, Yardım Ödeme, Tahsilat, Anormal Hareket) / Operasyonel modülün birleşik skoru | `FinancialRiskIndex` / `OperationalRiskIndex` |
| Tahmin Motoru | Veriden Risk Kaydı üreten bileşen; ilk sürüm istatistiksel demo motoru | `IRiskPredictionEngine` · "yapay zekâ" ifadesi arayüzde; kodda motor |
| Veri Kaynağı | Yüklenen dosya veya bağlantı; sürümü ve geçmişi tutulur | `DataSource` |
| Örnek Veri | Sentetik demo verisi; bundan üretilen her çıktı etiketlenir (BR-4) | `IsSample = true` · gerçek veri **değildir** |
| İdari Takip | Brifte: ödemesi aksayan kredinin idari takip sürecine geçmesi. **POLSAN tanımı açık soru (B-1)** | — |
| Yardım Ödemesi | Brifte: üyelere yapılan yardım ödemeleri. **Türleri açık soru (B-1)** | `AidPayment` |

## Business rules

- **BR-1 — Eşikler.** Risk Seviyesi skordan türetilir: Kritik ≥ 85 · Yüksek 70–84 · Orta 40–69 ·
  Düşük < 40 (sınırlar dahil, ör. 85 = Kritik, 84 = Yüksek). Eşikler Sistem Ayarları'ndan
  değiştirilebilir; her değişiklik denetim kaydına düşer.
- **BR-2 — Açıklanabilirlik.** Gerekçesi ve dayandığı veriler olmayan tahmin yayınlanmaz.
- **BR-3 — Karar desteği.** Sistem bir kişi hakkında otomatik işlem yapmaz; son karar insanındır.
- **BR-4 — Örnek veri etiketi.** Örnek veriden üretilen her çıktı (ekran, API cevabı, rapor,
  dışa aktarım) açıkça "ÖRNEK VERİ" olarak işaretlenir.
- **BR-5 — Tek açık uyarı.** Aynı Varlık + aynı Risk Modülü için aynı anda en fazla bir açık
  (Açık veya İnceleniyor) Erken Uyarı bulunur; yeni tahmin mevcut uyarıyı günceller.
- **BR-6 — Radar renkleri.** Kırmızı (`#F04452`) = Kritik/Yüksek, turuncu (`#F5A623`) = Orta,
  yeşil (`#20C997`) = Düşük. Kritik, Yüksek'ten ayrıca ayırt edilir (ör. vurgu/nabız) — biçim spec'te.
- **BR-7 — Kişi görünürlüğü.** Kişi bazlı ayrıntı yalnızca yetkili rollere gösterilir; panel ve
  radar varsayılan olarak toplu (birim/hesap grubu) görünüm sunar.

## Key domain invariants

- Örnek veri ile gerçek veri asla karışmaz ve etiketsiz bir örnek çıktı asla gösterilmez (BR-4).
- Risk Seviyesi ile Risk Skoru asla çelişmez (seviye her zaman BR-1'den türetilir).
- Identity dilimi (6) bitmeden gerçek kurum verisi sisteme girmez (`docs/security.md`).
- Denetim kayıtları değiştirilemez ve silinemez.

## Sensitive areas

- **Kişisel ve finansal veri (KVKK)** — üye, kredi, ödeme bilgileri.
- **Kişi hakkında risk etiketi** — "riskli kişi", "şüpheli hareket" işaretlemeleri; yanlış etiketin
  hukuki ve itibari sonucu vardır.
- **Yetkilendirme ve denetim kaydı** — roller, veri düzeyinde kısıtlar, audit log.
- **Veri yükleme** — dışarıdan gelen dosyalar.
