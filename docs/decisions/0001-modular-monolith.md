# ADR 0001 — Modüler monolit, tek dağıtım birimi

- Status: Accepted
- Date: 2026-11-26

## Context
POLSAN RADAR sekiz iş yeteneğini (risk, tahmin, uyarı, panel, veri yükleme, raporlama, kimlik,
denetim) kapsıyor; başlangıçta tek geliştirici + ajan ile, kurum içi bir sunucuda çalışacak. Ürün
sahibi backend için monolitik mimari istedi. Öte yandan Tahmin Motoru'nun ileride gerçek modellerle
(ML.NET ya da ayrı bir Python servisi) değiştirilmesi ve bildirim kanallarının eklenmesi bekleniyor.

## Decision
Backend tek bir ASP.NET Core uygulaması olan **modüler monolittir**: modüller yalnızca
`<Module>.Contracts` üzerinden konuşur, her modülün kendi PostgreSQL şeması vardır ve sınırlar
ArchUnitNET testleriyle zorlanır. React SPA aynı uygulamadan sunulur — tek dağıtım birimi.

## Consequences
- Kazanç: tek kurulum, tek süreç, basit test ve hata ayıklama; dağıtık sistem karmaşıklığı yok;
  net sınırlar sayesinde bir modül (ör. Prediction) ileride ayrı servise çıkarılabilir.
- Maliyet: modül sınırlarının disiplini testlere ve review'a bağlı; tüm modüller birlikte ölçeklenir
  ve birlikte dağıtılır; tek veritabanı tek arıza noktasıdır.

## Alternatives considered
- **Katmanlı klasik monolit (modül sınırı yok):** başlangıçta daha hızlı, ama 8 yetenek büyüdükçe
  "her şey her şeye bağlı" olur ve Tahmin Motoru'nu ayırmak zorlaşır.
- **Mikroservisler:** tek ekip ve kurum içi tek sunucu için gereksiz operasyon yükü; ürün sahibinin
  monolit tercihine aykırı.

## Revisit triggers
- Tahmin Motoru'nun farklı bir dil/çalışma zamanı (ör. Python) veya ayrı ölçekleme gerektirmesi.
- Birden fazla ekibin bağımsız dağıtım ihtiyacı.
- Bir modülün yükünün diğerlerini etkilemesi.
