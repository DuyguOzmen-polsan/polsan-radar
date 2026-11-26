# Backlog — deferred findings

Findings a human triaged as **defer**: real, but fixed later by decision (`workflows/feature-development.md`,
step 7). Nothing lands here without that decision; noise is rejected in the triage record, not parked here.

- A criterion moved out at the ship gate (R-08, `→ moved to B-<n>`) lands here too, by the
  human's ship decision; the entry names the spec and criterion.
- One entry per finding, newest last. Delete the entry in the commit of the work that closes it.
- An entry that grows into new behavior becomes a spec (`specs/active/`), not a fix.

<!-- Format (labels stay English):
- **B-<n>** — <finding, one line> — Source: spec <NNNN>, review round <n> (<file:line>) — Severity: <…>
  — Recommendation: <the reviewer's action> — Deferred by: <name>, <YYYY-MM-DD>, <why>
  A decision deferred at bootstrap (its REPORT) uses Source: bootstrap <YYYY-MM-DD>.
-->

- **B-1** — "İdari takip" ve "yardım ödemesi" POLSAN'daki tam tanımları (yardım ödemesi türleri dahil) belirsiz; `docs/domain.md` sözlüğü brifteki genel anlamı kullanıyor — Source: bootstrap 2026-11-26 — Severity: Medium
  — Recommendation: modül A ve B'nin spec'lerinden (dilim 5) önce iş birimiyle netleştir, sözlüğü güncelle — Deferred by: duyguozmen, 2026-11-26, MVP örnek veriyle çalışıyor; gerçek tanım dilim 5'te gerekli
- **B-2** — Kişisel veri saklama süresi (KVKK) ve silme/anonimleştirme politikası tanımsız — Source: bootstrap 2026-11-26 — Severity: High
  — Recommendation: hukuk/KVKK biriminden karar al; dilim 6–7 spec'lerinden önce `docs/security.md`'ye yaz — Deferred by: duyguozmen, 2026-11-26, MVP'de gerçek kişisel veri yok
- **B-3** — QuestPDF Community lisansı ciro sınırlı; PDF dışa aktarım kütüphanesi seçilmedi — Source: bootstrap 2026-11-26 — Severity: Low
  — Recommendation: dilim 8 planında PDFsharp/MigraDoc (MIT) veya tarayıcıdan PDF (Playwright) karşılaştır — Deferred by: duyguozmen, 2026-11-26, raporlar MVP dışında
- **B-4** — Gerçek kurum verisine geçmeden önce çalışma modunu strict'e geçirme kararı — Source: bootstrap 2026-11-26 — Severity: Medium
  — Recommendation: dilim 6 (Identity) ship edilirken veya ekibe ikinci kişi katıldığında AGENTS.md Mode satırını `strict` yap — Deferred by: duyguozmen, 2026-11-26, MVP örnek veri + localhost
