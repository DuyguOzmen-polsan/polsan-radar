# ADR 0002 — Teknoloji seti: .NET 10 + React/TypeScript + PostgreSQL 17

- Status: Accepted
- Date: 2026-11-26

## Context
Ürün sahibi frontend için React + TypeScript, veritabanı için PostgreSQL ve monolitik bir backend
istedi; backend dili açık bırakıldı ve "önerilerle devam" talimatı verildi. Geliştirme makinesinde
.NET 10, Node 24 ve Docker kurulu; kurum ortamı Windows/Microsoft ağırlıklı görünüyor. Sistem kişisel
ve finansal veri işleyecek (KVKK), rol bazlı yetkilendirme ve denetim kaydı istiyor.

## Decision
Backend **ASP.NET Core / .NET 10 (LTS)** + EF Core/Npgsql; frontend **React 19 + TypeScript (strict)
+ Vite + Apache ECharts**; veritabanı **PostgreSQL 17**. Tahmin Motoru `IRiskPredictionEngine`
arayüzünün arkasında, ilk sürüm .NET içinde istatistiksel demo motorudur.

## Consequences
- Kazanç: uzun destek süresi (LTS); kimlik doğrulama, yetki politikaları, antiforgery hazır; tek
  backend dili; kurumun Windows/AD ortamına uyum; ECharts radar ve etkileşimli grafikleri karşılar.
- Maliyet: tahmin/analiz kütüphaneleri Python kadar zengin değil — gerçek modeller için ML.NET veya
  ayrı bir model servisi gerekebilir (ADR 0001 sınırları buna izin verir).

## Alternatives considered
- **Python FastAPI backend:** veri bilimi ekosistemi daha güçlü; ama kurumsal kimlik/yetki daha çok
  el emeği ister ve makinedeki Python 3.15 kütüphane uyumu için fazla yeni.
- **Node/TypeScript backend:** tek dil avantajı; ama decimal/para işleme ve kurumsal kimlik
  entegrasyonu .NET'e göre zayıf.

## Revisit triggers
- Gerçek tahmin modellerinin .NET'te karşılanamaması (→ ayrı model servisi ADR'si).
- Kurumun standart veritabanının PostgreSQL dışında olduğunun ortaya çıkması.
