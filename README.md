# Kimo — tanıtım ve hukuki metin sitesi

Kimo, YKS'ye hazırlanan öğrenciler için bir hata defteri uygulaması.
Bu depo uygulamanın tanıtım sitesini ve zorunlu hukuki metinlerini barındırır.

> ⚠️ **Bu site draft durumdadır.** Köşeli parantezli alanlar (`[şirket unvanı]`,
> `[iletişim e-postası]` gibi) henüz doldurulmadı ve sayfalarda olduğu gibi görünüyor.
> Yayına çıkmadan önce **[DOLDURULACAKLAR.md](DOLDURULACAKLAR.md)** listesi kapatılmalı.

## Sayfalar

| Dosya | İçerik |
|---|---|
| `index.html` | Ana sayfa — tanıtım, özellikler, Kimo Plus, SSS |
| `kullanim-kosullari.html` | Kullanım Koşulları (sürüm 1.3) |
| `gizlilik-politikasi.html` | Gizlilik Politikası (sürüm 1.4) |
| `kvkk-aydinlatma-metni.html` | KVKK Aydınlatma Metni (sürüm 1.3) |
| `hesap-silme.html` | Hesap silme — Google Play'in zorunlu tuttuğu web sayfası |
| `iletisim.html` | Destek, telif bildirimi, 5651 tanıtıcı bilgiler |
| `404.html` | Sayfa bulunamadı |
| `app-ads.txt` | AdMob yayıncı doğrulaması |
| `favicon.svg` | Site ikonu |

## Teknik

Düz statik HTML. **Derleme adımı yok, bağımlılık yok, JavaScript yok.**
Her sayfa kendi CSS'ini `<head>` içinde taşır; dışarıdan yalnızca Google Fonts yüklenir
ve o da yüklenmezse sayfa sistem yazı tipiyle sorunsuz görünür.

Sayfalar koyu temayı `prefers-color-scheme` ile destekler.

### Yerel önizleme

```bash
python3 -m http.server 8000
# http://localhost:8000
```

`file://` ile de açılır, ama sayfalar arası bağlantıların doğru çalışması için
yerel sunucu tercih edilmeli.

### GitHub Pages ile yayın

1. Settings → Pages → Source: `Deploy from a branch`
2. Branch: `main`, klasör: `/ (root)`
3. Kaydedin; birkaç dakika içinde yayında olur.

`.nojekyll` dosyası depoda duruyor — GitHub Pages'in Jekyll işlemesini kapatır,
böylece dosyalar olduğu gibi sunulur.

> **`app-ads.txt` uyarısı:** AdMob bu dosyayı yalnızca **alan adının kökünde** arar
> (`https://alanadi.com/app-ads.txt`). GitHub Pages'i proje sayfası olarak
> (`kullanici.github.io/depo/`) yayınlarsanız dosya kökte olmaz ve doğrulama başarısız olur.
> Özel alan adı veya kullanıcı sayfası (`kullanici.github.io`) kullanın.

## Metinlerin kaynağı

Hukuki metinler, kod tabanının incelenmesiyle üretilen bir kaynak belgeden geliyor
(Claude Design projesindeki `uploads/hukuki-metinler.md`). O belgenin 1, 2, 3 ve 7.
bölümleri **iç kullanım içindir ve yayınlanmaz**; yalnızca 4, 5 ve 6. bölümler
(KVKK, Gizlilik, Kullanım Koşulları) bu sitedeki sayfalara karşılık gelir.

Metinler güncellendiğinde sayfa başındaki **sürüm numarası** ile sunucudaki
`app_config.legal_version` birlikte güncellenmelidir — kullanıcı onay kayıtları
o değeri damgalar.
