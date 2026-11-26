# Ürün Brifi — POLSAN RADAR

> Kaynak: ürün sahibinin bootstrap'ta (2026-11-26) verdiği brif; içerik değiştirilmeden kaydedildi,
> yalnızca markdown biçimi eklendi. Bu dosya **ihtiyaç kaynağıdır, spec değildir**: her özellik
> önce `specs/active/` altında bir spec olur (AGENTS.md kural 1). Dilim sırası ve MVP kapsamı
> `docs/domain.md` "Product & roadmap" bölümündedir; brif ile domain.md çelişirse domain.md
> (onaylanmış karar) geçerlidir.

## Proje

POLSAN RADAR – Yapay zekâ destekli erken uyarı ve risk tahmin sistemi. POLSAN için kurumun
gelecekte karşılaşabileceği finansal, idari ve operasyonel problemleri önceden tespit eden, analiz
eden ve yöneticilere erken uyarı veren modern bir web uygulaması.

## 1. Uygulamanın Amacı

POLSAN RADAR, gerçekleşmiş sorunları raporlayan klasik bir raporlama sistemi değildir. Henüz
gerçekleşmemiş riskleri geçmiş veriler, mevcut göstergeler, eğilimler ve yapay zekâ destekli
analizlerle tahmin etmeye çalışan proaktif bir karar destek sistemidir.

Sistemin temel amacı, yöneticilerin sorunlar ortaya çıktıktan sonra müdahale etmesini değil,
sorunları önceden görerek gerekli tedbirleri almasını sağlamaktır.

## 2. Ana Risk Analiz Modülleri

**A. İdari Takibe Düşme Riski**
- İdari takibe düşme ihtimali bulunan kredileri tespit et.
- Kredi ve ödeme geçmişlerini analiz et.
- Riskli kişi veya hesapları belirle.
- Gelecekte idari takibe düşme olasılığını yüzde olarak göster.
- Riskin nedenlerini açıkla.

**B. Gelecekteki Yardım Ödemeleri Tahmini**
- Önümüzdeki aylarda yoğunlaşabilecek yardım ödemelerini tahmin et.
- Geçmiş dönem yardım ödeme verilerini incele.
- Aylık ve yıllık ödeme projeksiyonları oluştur.
- Beklenen ödeme yükünü grafiklerle göster.

**C. Tahsilat Problemi Tahmini**
- Tahsilat sorunu yaşanabilecek dönemleri belirle.
- Tahsilat performansındaki düşüş eğilimlerini tespit et.
- Geçmiş tahsilat verileri ile gelecek dönem tahminleri oluştur.
- Tahsilat riski yüksek alanları listele.

**D. Olağan Dışı Finansal Hareketler**
- Normal finansal davranışların dışında kalan hareketleri tespit et.
- Ani artış ve azalışları belirle.
- Beklenmeyen işlem yoğunluklarını analiz et.
- Şüpheli veya incelenmesi gereken hareketleri işaretle.
- Her tespit için açıklama ve risk seviyesi sun.

**E. Operasyonel Yoğunluk ve Sistem Darboğazları**
- İşlem yoğunluğu yaşanabilecek birimleri tespit et.
- Gelecekte oluşabilecek operasyonel darboğazları tahmin et.
- İş yükü dağılımını analiz et.
- Personel ve kaynak ihtiyacını öngör.
- Yoğunluk yaşanabilecek dönemler için erken uyarı oluştur.

## 3. Ana Dashboard (Yönetici Paneli)

Modern, profesyonel, koyu lacivert ve mavi ağırlıklı bir yönetim paneli. Ana ekran göstergeleri:

- Toplam analiz edilen kayıt sayısı
- Yüksek riskli kayıt sayısı
- Orta riskli kayıt sayısı
- Düşük riskli kayıt sayısı
- Önümüzdeki 30 gün için tahmin edilen risk sayısı
- Açık erken uyarı sayısı
- Finansal risk göstergesi
- Operasyonel risk göstergesi
- Genel kurum risk skoru

Her gösterge dinamik ve etkileşimli olsun.

## 4. Radar Görselleştirmesi

Uygulamanın en önemli özelliği interaktif RADAR ekranı. Referans görseldeki gibi merkezinde POLSAN
RADAR bulunan dairesel bir analiz ekranı. Radar üzerinde:

- Riskli kişi, kurum, birim veya işlem grupları noktalarla gösterilsin.
- Kırmızı noktalar yüksek riski, turuncu noktalar orta riski, yeşil noktalar düşük riski temsil etsin.
- Merkezden çevreye uzanan bağlantılar risk ilişkilerini göstersin.
- Noktalara tıklandığında ilgili riskin detayları açılsın.
- Risklerin zaman içerisindeki değişimi izlenebilsin.

Radar animasyonlu, modern ve profesyonel görünümlü olsun. (Referans görsel repoda değildir; ilgili
spec'in "User experience" bölümü bağlantı olarak verir.)

## 5. Yapay Zekâ Risk Tahmin Motoru

Motor: geçmiş dönem verilerini analiz etsin; eğilimleri belirlesin; anormal durumları tespit etsin;
gelecekte oluşabilecek riskleri tahmin etsin; her tahmin için risk olasılığı oluştursun; tahminin
hangi verilere dayandığını açıklasın; yöneticilere alınabilecek önlemler önersin.

Örnek:
- Risk: Tahsilat Performansında Düşüş
- Tahmin: Önümüzdeki 30 gün içerisinde tahsilat performansında düşüş yaşanma ihtimali.
- Risk Skoru: %82 · Risk Seviyesi: Yüksek
- Açıklama: Son üç aylık tahsilat verilerinde düşüş eğilimi gözlemlenmiştir.
- Önerilen Aksiyon: İlgili hesapların incelenmesi ve tahsilat süreçlerinin gözden geçirilmesi.

**Önemli:** Gerçek veri bulunmadığında yapay zekâ tahminlerini gerçek analiz sonucu gibi sunma.
Demo verilerini açıkça örnek olarak işaretle.

## 6. Erken Uyarı Sistemi

Risk seviyesi kritik eşiğe ulaştığında sistem otomatik uyarı oluştursun. Uyarı seviyeleri: Kritik,
Yüksek, Orta, Düşük. Her uyarıda: risk başlığı, ilgili birim, risk seviyesi, tahmini gerçekleşme
tarihi veya dönemi, risk olasılığı, riskin nedeni, önerilen aksiyon.

Uyarılar yönetici panelinde görüntülensin. İleride e-posta ve diğer bildirim kanallarına entegrasyon
yapılabilecek şekilde mimari oluştur.

## 7. Raporlama ve Analiz

Rapor ekranları: günlük, haftalık, aylık risk raporu; finansal risk analiz raporu; tahsilat tahmin
raporu; yardım ödeme tahmin raporu; operasyonel risk raporu; yapay zekâ tahmin performansı raporu.
Raporlar PDF ve Excel olarak dışa aktarılabilsin.

## 8. Veri Yönetimi

Veri yükleme alanı: Excel dosyası yükleme, CSV dosyası yükleme, veri önizleme, kolon eşleştirme,
veri doğrulama, eksik veri kontrolü, veri geçmişi takibi. Yüklenen veriler analiz motorunda
kullanılabilsin. Gerçek kurum verileri bulunmadığı için başlangıçta örnek veri setleri kullan; gerçek
veriye geçişte kullanılacak veri altyapısını hazırla.

## 9. Kullanıcı ve Yetkilendirme

Rol bazlı kullanıcı sistemi: Sistem yöneticisi, Üst yönetici, Birim yöneticisi, Analiz uzmanı, Salt
okunur kullanıcı. Her kullanıcı sadece yetkili olduğu verilere ve ekranlara erişebilsin. Tüm kritik
işlemler kayıt altına alınsın.

## 10. Tasarım ve Kullanıcı Deneyimi

Tasarım dili: kurumsal, modern, premium, teknolojik, minimal, veri odaklı.

| Renk | Kod |
|---|---|
| Koyu lacivert | `#06172E` |
| Derin mavi | `#0B2C52` |
| Parlak mavi | `#168BFF` |
| Turkuaz | `#00D6C9` |
| Risk kırmızısı | `#F04452` |
| Uyarı turuncusu | `#F5A623` |
| Başarı yeşili | `#20C997` |

Sol tarafta sabit navigasyon menüsü: Ana Dashboard · POLSAN Radar · Finansal Riskler · İdari Takip
Riskleri · Yardım Ödeme Tahminleri · Tahsilat Analizi · Anormal Hareketler · Operasyonel Riskler ·
Erken Uyarılar · Raporlar · Veri Yönetimi · Kullanıcı Yönetimi · Sistem Ayarları.
Tüm ekranlar birbirine bağlı ve çalışır durumda olsun.

## 11. Teknik Beklentiler

Responsive web uygulaması (masaüstü öncelikli); modern frontend mimarisi; güvenli backend; veritabanı
bağlantısı; API tabanlı mimari; yapay zekâ entegrasyonuna uygun yapı; interaktif grafikler ve radar;
filtrelenebilir, sıralanabilir, aranabilir veriler; demo verileriyle tüm ekranların test edilebilmesi.

## 12. En Önemli Beklenti

Statik tasarım ya da tıklanmayan prototip değil, gerçek bir yazılım ürünü: menülerin tamamı çalışsın;
dashboard verileri dinamik olsun; grafikler gerçek verilerden beslensin; filtreler çalışsın; radar
noktaları tıklanabilsin; risk detay ekranları açılsın; veri yükleme ve analiz akışı çalışsın; erken
uyarılar oluşturulabilsin; raporlar indirilebilsin.

Yapay zekâ tahminleri için başlangıçta örnek veriyle çalışan bir demo motoru; gerçek tahminler için
gerekli model ve veri entegrasyon mimarisi hazırlansın.

**Nihai hedef:** POLSAN yönetiminin geçmişi raporlayan değil, gelecekte karşılaşabileceği sorunları
önceden görebilen, riskleri ölçebilen ve doğru zamanda aksiyon almasını sağlayan akıllı bir karar
destek platformu. POLSAN kurumsal kimliğine uygun, üst düzey yönetici sunumunda kullanılabilecek
profesyonellikte.
