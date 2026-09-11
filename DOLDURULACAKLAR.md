# Kimo sitesi — yayın öncesi kontrol listesi

Bu site **draft** durumda. Aşağıdaki maddeler kapatılmadan yayına çıkmamalı.
Her madde: **ne**, **hangi dosyada**, **nereden alınacağı**.

Köşeli parantezli her alan (`[şirket unvanı]` gibi) sitede **olduğu gibi görünüyor**.
Şu an toplam **17 farklı alan, 64 yerde** doldurulmayı bekliyor.

Bir alanı doldurmak için tüm dosyalarda tek seferde değiştirin:

```bash
# örnek: şirket unvanını 13 yerde birden doldurur
sed -i '' 's/\[şirket unvanı\]/Örnek Yazılım A.Ş./g' *.html
```

Doldurulmamış alan kaldı mı diye kontrol:

```bash
grep -o '\[[^][]\{2,80\}\]' *.html app-ads.txt | sort | uniq -c | sort -rn
```

---

## 1 · Yayın engelleyiciler

Bunlar yalnızca metin doldurma işi değil; karar veya başka bir ekip işi gerektiriyor.

- [ ] **Abonelik verisi hukuki metinlere işlenmeli.**
  Uygulama içi satın alma (Kimo Plus) canlıya çıkınca yeni bir veri kategorisi doğuyor:
  abonelik durumu, makbuz/işlem kimliği, deneme hakkının kullanılıp kullanılmadığı.
  Bugün bu veriler **hiçbir metinde geçmiyor**:
  - `gizlilik-politikasi.html` §1 (toplanan veriler) ve §6 (sağlayıcı tablosu — Apple/Google ödeme tarafı için satır yok)
  - `kvkk-aydinlatma-metni.html` §2 (işlenen veriler), §4 (hukuki sebep), §7 (saklama süresi)

  Metni yazabilmek için önce şu netleşmeli: makbuz doğrulaması sunucuda mı yapılıyor,
  hangi alanlar saklanıyor, Apple/Google'dan ne geliyor.

- [ ] **Mağaza gizlilik formlarında "Purchases" cevabı güncellenmeli.**
  Kaynak belge (`hukuki-metinler.md` §3.2) bugün **"Purchases: HAYIR"** diyor ve gerekçesi
  *"uygulama içi satın alma yok"*. Satın alma açılınca bu cevap hem App Store Connect'te
  hem Google Play Veri Güvenliği formunda değişmek zorunda.

- [ ] **Store rozetleri gerçek adreslerle bağlanmalı.**
  `index.html` — hero bölümündeki App Store ve Google Play rozetleri şu an bağlantısız
  kesikli çerçeveler ve üzerlerinde **"Yakında"** yazıyor. Uygulama mağazalarda
  yayınlandığında hem `<a href>` eklenmeli hem "Yakında" etiketi kaldırılmalı.

- [ ] **`app_config.legal_version` = `1.3` yapılmalı.**
  Yayınlanan üç hukuki metin **1.3** damgalı. Sunucudaki değer buna eşitlenmezse
  kullanıcı onay kayıtları yanlış sürüm damgalanır — kaynak belge:
  *"Atlanırsa onay kayıtları `1.0` damgalanır ve sunucu günlüğüne uyarı yazılır;
  kayıt akışı durmaz."* Yani sessizce yanlış olur.

- [ ] **Uygulama içi bağlantı adresleri girilmeli** (site yayınlandıktan sonra).
  `supabase.json` içine: `LEGAL_TERMS_URL`, `LEGAL_PRIVACY_URL`, `LEGAL_KVKK_URL`,
  `LEGAL_DELETE_URL`, `SUPPORT_EMAIL`. Kaynak belge uyarısı:
  *"Adres girilmezse ilgili satır uygulamada hiç görünmez — sessizce eksik kalır, hata vermez."*

---

## 2 · Şirket bilgileri — sizde

| # | Alan | Nerede | Kaç yer |
|---|---|---|---|
| [ ] | `[şirket unvanı]` | 6 sayfanın footer'ı + `iletisim.html` §İşletme bilgileri + `kullanim-kosullari.html` §1/§8/§17 + `gizlilik-politikasi.html` giriş/§13 + `kvkk-aydinlatma-metni.html` §1 | 13 |
| [ ] | `[iletişim e-postası]` | 5 sayfa — destek, veli başvuruları, itiraz, KVKK başvurusu, hesap silme talebi | 24 |
| [ ] | `[telif bildirim e-postası]` | `iletisim.html` §Telif, `kullanim-kosullari.html` §9 | 4 |
| [ ] | `[açık adres]` | `iletisim.html`, `kullanim-kosullari.html` §17, `kvkk-aydinlatma-metni.html` §1 ve §11 (yazılı başvuru adresi), `gizlilik-politikasi.html` §13 | 5 |
| [ ] | `[sicil no]` | `iletisim.html` — 5651 tanıtıcı bilgiler | 1 |
| [ ] | `[vergi dairesi]` / `[vergi no]` | `iletisim.html` — 5651 tanıtıcı bilgiler | 2 |
| [ ] | `[KEP adresi]` | `iletisim.html`, `kvkk-aydinlatma-metni.html` §1 | 2 |
| [ ] | `[VERBİS kayıt bilgisi / "kayıt yükümlülüğümüz bulunmamaktadır"]` | `kvkk-aydinlatma-metni.html` §1 | 1 |
| [ ] | `[tarih]` — yürürlük / son güncelleme | `gizlilik-politikasi.html`, `kullanim-kosullari.html`, `kvkk-aydinlatma-metni.html` başlıkları | 3 |
| [ ] | `[yetkili mahkeme ve icra daireleri]` | `kullanim-kosullari.html` §16 — tüketici olmayanlar için yetki şartı | 1 |

> **Not:** VERBİS kaydı ve yetkili mahkeme maddesi hukukçuyla teyit edilmeli.
> Yetki şartı, tüketici olmayan kullanıcılar için geçerli — tüketicilerin kendi
> yerleşim yerindeki hakem heyetine/mahkemeye gitme hakkı metinde zaten saklı tutulmuş.

---

## 3 · Dış servislerden doğrulanacaklar

Bunlar kod tabanında yok; uydurulmadı, köşeli parantez olarak bırakıldı.

| # | Alan | Nereden alınır | Neden gerekli |
|---|---|---|---|
| [ ] | `[Supabase bölge/ülke]` | Supabase Dashboard → Project Settings → General → Region | KVKK Md. 9 yurt dışı aktarım beyanı |
| [ ] | `[Sentry bölge/ülke]` | Sentry → Settings | KVKK §4.5 ve §7 |
| [ ] | `[Sentry saklama süresi]` | Sentry → Settings → olay saklama süresi | Gizlilik §7, KVKK §7 |
| [ ] | `[OpenAI veri işleme politikası URL'i]` | OpenAI'ın güncel veri işleme sayfası | Gizlilik §3, KVKK §5 |
| [ ] | `[AdMob yayıncı kimliği]` | AdMob Console → Ayarlar → Yayıncı kimliği (`pub-...`) | `app-ads.txt` |
| [ ] | `[SCC durumu doldurulacak]` | Sağlayıcılarla imzalanan SCC durumu | Gizlilik §11 — AB/AEA ve BK kullanıcıları tablosu |
| [ ] | **OpenAI hesabının veri işleme koşulları** (DPA imzalı mı, sıfır-saklama açık mı) | OpenAI hesap ayarları / kurumsal sözleşme | Gizlilik §3 ve KVKK §5'teki saklama cümlesi buna göre kesinleşir. Kaynak belge bunu **kritik** işaretlemiş |

---

## 4 · Hukukçu kararı bekleyen — yurt dışına aktarımın dayanağı

Bu metin sitede yayında duruyordu; **iç değerlendirme notu olduğu için kaldırıldı**
(`kvkk-aydinlatma-metni.html` §5 ile §6 arasındaydı). Karar burada bekliyor:

> KVKK'nın 9. maddesi 2024 değişikliğiyle yeniden düzenlendi. İki yol var:
>
> **Seçenek 1 — Standart sözleşme (Md. 9/3-b/3).** Kurul'un ilan ettiği standart sözleşme
> metni OpenAI (ve diğer yurt dışı sağlayıcılar) ile imzalanır ve imzadan itibaren
> **5 iş günü içinde** Kurul'a bildirilir. Daha koruyucu ve daha sürdürülebilir yol budur:
> aktarımı açık rızanın kırılganlığından kurtarır, kullanıcı rızasını geri alsa bile
> hizmet güvenliği taraması hukuken ayakta kalır ve sürekli/sistematik aktarım için doğru
> araçtır. Maliyeti: sağlayıcıyı imzaya ikna etmek ve bildirim yükümlülüğü.
>
> **Seçenek 2 — Açık rıza (Md. 9/6-a).** Uygulamanın bugün yaptığı budur. Daha hızlı ve
> hemen uygulanabilir, ama Md. 9/6 istisnaları **"arızi olmak"** kaydıyla düzenlenmiştir.
> Her fotoğrafta çalışan sürekli bir aktarımın "arızi" sayılması tartışmalıdır; ayrıca
> zorunlu moderasyon taramasına açık rıza dayanağı hiç uymaz (rızayı geri alan kullanıcıda
> tarama yine de çalışır).
>
> **Öneri:** Seçenek 1'i hedefleyin, Seçenek 2'yi geçiş döneminde koruyun — yani standart
> sözleşme tamamlanana kadar açık rıza almaya devam edin.

- [ ] Hukukçu bu geçiş yapısını onayladı mı?
- [ ] Standart sözleşme imzalanırsa: `kvkk-aydinlatma-metni.html` §4 tablosunun son satırına
      *"ve Kurul'a bildirilen standart sözleşme (Md. 9/3-b/3)"* ifadesi eklenmeli.
- [ ] İlgili ikinci soru: zorunlu moderasyon taraması için **meşru menfaat** (Md. 5/2-f)
      yeterli mi, yoksa 5651 sayılı Kanun bunu **hukuki yükümlülüğe** (Md. 5/2-ç) mi taşıyor?
      Meşru menfaat kalırsa bir denge testi belgesi hazırlanmalı.

---

## 5 · Ölçüm sonrası teyit edilecek

- [ ] **Analiz kotası rakamları.** Sitede yazan değerler:
  ücretsiz **8 saatte 10 analiz / ayda 300**, Plus **8 saatte 50 / ayda 1.000**
  (`index.html` §Plus, `kullanim-kosullari.html` §13 dolaylı olarak).

  **Birim maliyet ölçümü henüz yapılmadı.** Rakamlar ölçüm sonrasında değişebilir.
  Mağaza açıklamasında, reklam metninde veya basında kullanılmadan önce teyit edilsin —
  ilan edilip sonra düşürülen kota, tüketici tarafında sorun yaratır.

  > Not: kaynak belge `hukuki-metinler.md` §1.3 hâlâ *"Günde 5 analiz hakkı"* diyor.
  > O satır daha eski bir commit'e dayanıyor ve **bayattır**; sitedeki rakamlar günceldir.
  > Belge bir sonraki revizyonda güncellenmeli.

---

## 6 · Alan adı belli olunca

- [ ] `og:url` ve `og:image` — 6 sayfanın `<head>`'ine eklenecek (şu an yok; paylaşımda
      görsel önizleme çıkmaz, sadece başlık + açıklama çıkar)
- [ ] `<link rel="canonical">` — 6 sayfa
- [ ] `sitemap.xml` ve `robots.txt`
- [ ] `app-ads.txt` **alan adının kökünde** yayınlanmalı (`https://alanadi.com/app-ads.txt`).
      AdMob yalnızca kökte arar; alt dizinde çalışmaz.
- [ ] Google Play Console → Veri güvenliği → hesap silme alanına
      `https://alanadi.com/hesap-silme.html` girilmeli (Play'in zorunlu alanı)

---

## 7 · Görseller

- [ ] **Ekran görüntüleri.** `index.html` §Ekranlar bölümünde 4 boş gri yer tutucu var:
      Bugün ekranı, Hatalarım, Pratik oturumu, Lig. Gerçek ekran görüntüleriyle değişmeli.
- [ ] **`og:image`** için 1200×630 bir paylaşım görseli.

---

## 8 · Bilinçli olarak böyle bırakılanlar

Bunlar hata değil, alınmış kararlar — listede olmalarının sebebi ileride kafa karıştırmasın:

- **Placeholder'lar sitede görünür durumda** ve site **arama motorlarına açık**.
  `noindex` eklenmedi. Yani `[şirket unvanı]` yazan hukuki metinler indekslenebilir.
  Bu kabul edildi; rahatsız ederse her sayfanın `<head>`'ine
  `<meta name="robots" content="noindex,nofollow">` eklemek tek satırlık iş.
- **`index.html` Plus fiyatları** (100 ₺/ay yıllık, 150 ₺ aylık) doğru kabul edildi ve
  `kullanim-kosullari.html` §13 bunlara göre yeniden yazıldı.
