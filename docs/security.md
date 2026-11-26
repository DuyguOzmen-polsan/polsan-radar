# Security

> Her planda ve review'da uyulan temel kurallar (bootstrap 2026-11-26). Hassas alanlar:
> `docs/domain.md` "Sensitive areas".

## Secrets
- Secrets never enter the repo, specs, prompts, or chat. `.env` is gitignored; provide `.env.example`.
- Agents never print secret values, even when debugging.
- Yerelde `dotnet user-secrets`, sunucuda ortam değişkenleri. `appsettings.*.local.json` gitignore'da.
- Bağlantı dizeleri ve anahtarlar loglanmaz, hata mesajına girmez.

## Input & output
- **Doğrulama** API sınırında FluentValidation (Apache-2.0) ile; geçersiz istek → 400 ProblemDetails.
- **Dosya yükleme (dilim 7):** yalnızca `.xlsx` ve `.csv`; makrolu `.xlsm` ve diğerleri reddedilir;
  boyut sınırı 20 MB; tür uzantıdan değil içerikten (imza/ayrıştırma) doğrulanır; dosya adı
  kullanıcıdan alınıp diske yol olarak kullanılmaz.
- **Excel/CSV dışa aktarım:** `= + - @` (ve tab/CR) ile başlayan hücreler kaçışlanır — formül
  enjeksiyonu önlemi.
- **Frontend:** React'in varsayılan kaçışlaması; `dangerouslySetInnerHTML` yasak (ESLint ile zorlanır).
- **Hata mesajları:** kullanıcıya stack trace, SQL, iç tip adı veya kişisel veri sızmaz.
- **KVKK:** veri minimizasyonu; TC kimlik / hesap numarası maskelenir (ör. yalnızca son 4 hane);
  loglarda kişisel veri yok; kişi verisinin görüntülenmesi ve dışa aktarılması denetim kaydına
  düşer; saklama süresi açık soru (`specs/backlog.md` B-2).
- **Yapay zekâ:** harici bir LLM/YZ servisine kişisel veri gönderilmeden önce ADR + insan onayı şart.

## AuthN / AuthZ
- **Sert kural — dilim 6'ya kadar:** Identity dilimi bitene kadar uygulama yalnızca `localhost`'a
  bağlanır ve yalnızca örnek veriyle çalışır; gerçek kurum verisi yüklenmez, uygulama ağda
  yayınlanmaz.
- **Varsayılan yasak:** ASP.NET `FallbackPolicy` ile her uç nokta yetki ister; yalnızca login ve
  health uçları açıkça `[AllowAnonymous]`.
- **Yetki backend'de** zorlanır; frontend'de menü gizlemek yalnızca kullanım kolaylığıdır, güvenlik
  değildir.
- **Roller:** Sistem yöneticisi · Üst yönetici · Birim yöneticisi · Analiz uzmanı · Salt okunur
  kullanıcı. Rol adları sabit bir allow-list'tir; karşılaştırma `Ordinal` (Türkçe "I" tuzağı).
- **Veri düzeyinde kısıt:** Birim yöneticisi yalnızca kendi biriminin verisini görür; kişi bazlı
  ayrıntıyı yalnızca yetkili roller görür (BR-7).
- **Oturum:** same-origin SPA için HttpOnly + `Secure` + `SameSite=Strict` cookie, antiforgery
  (CSRF) koruması. MVP sonrası ilk sürüm yerel hesaplar; tasarım kurumun Active Directory/LDAP
  altyapısına bağlanmaya açık tutulur.
- **Denetim:** kritik işlemler (giriş, yetki değişikliği, eşik/ayar değişikliği, veri yükleme, kişi
  verisi görüntüleme/dışa aktarma, uyarı durum değişikliği) `Audit` modülüne yazılır; değiştirilemez.

## Dependencies
- Her yeni paket (NuGet/npm) plan içinde gerekçesi ve lisansıyla listelenir; **plan onayı paket
  onayıdır**. Plan dışı paket eklenmez.
- **İzinli lisanslar:** MIT, Apache-2.0, BSD-2/3, ISC. **Reddedilir:** GPL/AGPL/LGPL ve kurumu
  ücrete/ciroya bağlı lisansa bağlayanlar (ör. FluentAssertions v8, QuestPDF Community — yıllık
  ciro sınırı; PDF için alternatif `specs/backlog.md` B-3).
- Bakımı süren paket tercih edilir (son 12 ayda sürüm).
- **Açık taraması:** `dotnet list package --vulnerable --include-transitive` ve
  `npm audit --audit-level=high` full profilde çalışır (ağ ister → `requires` satırı); high/critical
  bulgu check'i kırmızı yapar.

## Review lens
Security is a mandatory dimension of every independent review (see `prompts/review.md`), not a
separate afterthought phase.
