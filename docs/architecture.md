# Architecture

> Karar verilmiş mimari (bootstrap 2026-11-26). Ajanlar plan yapmadan önce bu dosyayı okur.

## System overview

POLSAN RADAR **modüler bir monolittir**: tek bir ASP.NET Core (.NET 10) uygulaması olarak
dağıtılır, içi net sınırlı modüllere bölünmüştür; tek PostgreSQL 17 veritabanında her modülün kendi
şeması vardır. React + TypeScript SPA derlenir ve aynı ASP.NET uygulamasından sunulur (tek dağıtım
birimi); frontend backend'e yalnızca `/api/v1` HTTP API'si ile erişir. Tek kurulum ve test kolaylığı
için monolit, ileride bir modülü (ör. Tahmin Motoru) ayırabilmek için net sınırlar seçildi —
`docs/decisions/0001-modular-monolith.md`; teknoloji seçimi `docs/decisions/0002-technology-stack.md`.

Hedef yerleşim (walking skeleton kurar):

```
src/
  backend/
    PolsanRadar.Host/            # ASP.NET Core host: DI, auth, middleware, SPA sunumu
    Modules/<Module>/            # <Module> (iç) + <Module>.Contracts (public arayüz + DTO + olay)
  web/                           # React + TypeScript + Vite SPA
tests/
  backend/<Module>.Tests/        # birim + entegrasyon (Category=Integration)
  backend/Architecture.Tests/    # yasak bağımlılıklar (ArchUnitNET)
```

## Modules / components and ownership

| Module | Single responsibility | Owns |
|---|---|---|
| Risk | Risk Kaydı, seviye ve eşik kuralları (BR-1, BR-2, BR-4) | Risk kayıtları, eşik ayarları, modül ağırlıkları |
| Prediction | `IRiskPredictionEngine` ve demo motoru: veriden Risk Kaydı üretir | Model sürümleri, tahmin çalıştırma geçmişi |
| Alerts | Erken uyarı oluşturma ve durum takibi (BR-5); `INotificationChannel` (ileride e-posta) | Erken uyarılar |
| Dashboard | Panel göstergeleri, radar ve detay için salt okunur sorgular | Kendi verisi yok — diğer modüllerin Contracts'ından okur |
| DataIngestion | Dosya yükleme, kolon eşleştirme, doğrulama, veri geçmişi, örnek veri tohumlama | Veri kaynakları, ham veri |
| Reporting | Rapor üretimi, PDF/Excel dışa aktarım | Rapor tanımları |
| Identity | Kullanıcılar, 5 rol, yetki kontrolü | Kullanıcılar, roller |
| Audit | Kritik işlemlerin değiştirilemez kaydı | Denetim kayıtları |
| Web (SPA) | Ekranlar, radar, grafikler; yalnızca API sözleşmesini kullanır | Kendi verisi yok |

## Communication rules

- Modüller birbirini yalnızca karşı modülün **`<Module>.Contracts`** arayüzleri üzerinden, süreç
  içinde ve senkron çağırır.
- Yan etkiler **süreç içi olaylarla** yürür (ör. `RiskScored` → Alerts uyarı oluşturur/günceller;
  Alerts → `INotificationChannel`). Olay tipleri yayınlayan modülün Contracts'ındadır.
- Frontend ↔ backend: yalnızca `/api/v1` REST + JSON; frontend tipleri OpenAPI'den üretilir.

## Forbidden dependencies (make them testable)

Her biri `tests/backend/Architecture.Tests` içinde bir ArchUnitNET testiyle doğrulanır.

1. Hiçbir modül başka bir modülün iç tiplerine, `DbContext`'ine veya şemasına erişmez — yalnızca
   `<Module>.Contracts`. Bir modülün `DbContext`'i yalnızca kendi şemasını eşler.
2. Prediction; ASP.NET Core (HTTP), Reporting ve Web'e bağımlı olamaz.
3. Denetim kaydı yalnızca `Audit.Contracts` ile yazılır; Audit şemasında güncelleme/silme yoktur.
4. Web (SPA) yalnızca üretilmiş API tiplerini kullanır; backend iç tipleri ve veritabanı bilgisi
   frontend'e girmez.
5. `Contracts` projeleri hiçbir modülün iç projesine bağımlı olamaz.

## Deliberately out of scope

MVP (dilim 1–4) için bilinçli olarak yok: mikroservisler, gerçek zamanlı akış (streaming), harici
yapay zekâ/LLM servisi, e-posta gönderimi (arayüz var, gönderim yok), kimlik doğrulama (dilim 6;
o zamana kadar yalnızca localhost — `docs/security.md`), veri yükleme (dilim 7), raporlar (dilim 8).
