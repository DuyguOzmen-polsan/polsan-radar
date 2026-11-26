# Conventions

> Yalnızca gerçek kurallar: her kural ya araçla zorlanır (tercih edilen) ya da review'da denetlenir.
> (Bootstrap 2026-11-26.)

## Language
- Chat language (interviews, sessions): Türkçe (`tr`)
- Document language (docs/, specs, plans, ADRs, review and verify reports): Türkçe (`tr`)
- Always English (protocol, not prose): ANEW core files, template headings and field labels,
  `Status:` values (Draft / Approved / In progress / Shipped), `Approved by / on:`, `Source:`.
- Code identifiers, branch names, commit messages: İngilizce (`feat(risk): threshold table [plan 0002/3]`).
  Kullanıcıya görünen arayüz metinleri Türkçe.
- Agent messages (handoff summaries, refusals, `Next:` lines): chat language; command names, file
  paths and protocol fields verbatim.

## Language & framework versions
Tam sürümler walking skeleton'da sabitlenir (`global.json`, `package.json`, `Directory.Packages.props`
ile merkezi paket yönetimi) ve buraya yazılır.
- Backend: .NET 10 (LTS), ASP.NET Core, EF Core + Npgsql
- Frontend: Node 24 (LTS), React 19, TypeScript (strict), Vite, Apache ECharts, TanStack Query/Table
- Veritabanı: PostgreSQL 17 (yerelde ve testte Docker imajı)

## Naming
- Kod İngilizce; alan terimlerinin kod karşılıkları `docs/domain.md` sözlüğündedir — yeni sınıf
  adı uydurulmaz, sözlükteki karşılık kullanılır (yoksa önce sözlüğe eklenir).
- C#: .NET isimlendirme standartları; proje adları `PolsanRadar.<Module>` / `PolsanRadar.<Module>.Contracts`.
- TypeScript: değişken/fonksiyon camelCase, React bileşenleri ve dosyaları PascalCase (`RiskRadar.tsx`).
- Veritabanı: `snake_case`, modül başına şema (`risk.risk_records`, `alerts.early_warnings`).
- Testler: `<Method/Behavior>_<Condition>_<Expected>`; frontend `*.test.tsx` bileşenin yanında.
- Arayüz metinleri koda gömülmez; tek `tr` kaynak dosyasında toplanır.

## Error handling
- İş kuralı hataları istisna değildir: `Result<T>` döner. Beklenmeyen hataları tek bir global
  exception handler yakalar.
- API hataları **RFC 9457 ProblemDetails** + correlation ID ile döner.
- Kullanıcıya asla stack trace, SQL, iç tip adı veya kişisel veri gösterilmez.
- Loglarda kişisel veri yoktur (KVKK); gerekirse maskelenir.

## Data rules
- **Para:** C# `decimal`, PostgreSQL `numeric(18,2)`, para birimi TRY; JSON'da **string**
  (`"1250.50"`). `float`/`double` para için yasak.
- **Risk Skoru:** 0–100 tamsayı; Risk Seviyesi asla saklanan bağımsız bir değer olarak elle
  girilmez, BR-1'den türetilir.
- **Zaman:** veritabanında UTC (`timestamptz`, C#'ta `DateTimeOffset`); dönemler `DateOnly` veya
  yıl-ay; ekranda `Europe/Istanbul` ve `tr-TR` biçimi. Şimdiki zaman yalnızca `TimeProvider`'dan.
- **Kimlikler:** UUIDv7 (`Guid.CreateVersion7()`).
- **Örnek veri:** her kayıtta `IsSample`; API cevabına ve dışa aktarıma kadar taşınır (BR-4).

## Known traps
- Türkçe "I/ı" (`"i".ToUpper()` → `"İ"` tr-TR'de) — kod içi karşılaştırma/anahtar/enum/rol adında `StringComparison.Ordinal` veya `InvariantCulture`; tr-TR yalnızca gösterimde — from bootstrap
- Npgsql `timestamptz` yalnızca UTC kabul eder (`DateTimeKind.Local` → çalışma anı hatası) — `DateTimeOffset`/UTC kullan — from bootstrap
- EF Core decimal hassasiyeti tanımlanmazsa uyarı/yuvarlama — her para alanında `numeric(18,2)` açıkça tanımla — from bootstrap
- MSBuild node'ları ve derleme sunucusu check'ten sonra yaşar — `-nodeReuse:false`, `--disable-build-servers`, `UseSharedCompilation=false` — from bootstrap
- Testcontainers Docker Desktop ister; konteyner sızar — her konteyneri dispose et, `reuse` kapalı — from bootstrap
- Vitest varsayılanı izleme modudur — check'te her zaman `vitest run` — from bootstrap
- ECharts jsdom'da canvas bulamaz — radar SVG renderer ile; konum/renk hesabı saf fonksiyonda ayrıca test — from bootstrap
- Git Bash Docker volume yollarını dönüştürür — `MSYS_NO_PATHCONV=1`; satır sonları `.gitattributes` ile sabit — from bootstrap

## Enforced by tooling
Walking skeleton kurar ve check'e bağlar; o zamana kadar review'da denetlenir.
- .NET: `Nullable` enable, `TreatWarningsAsErrors`, .NET analyzer'lar, `dotnet format --verify-no-changes`.
- TypeScript: `strict`, ESLint (`react/no-danger` = error), Prettier.
- Mimari: `Architecture.Tests` (architecture.md, Forbidden dependencies).
