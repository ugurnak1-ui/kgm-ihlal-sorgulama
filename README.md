# 🚦 KGM İhlal Sorgulama Otomasyonu

**v9.0** — Çoklu plaka için KGM ihlal sorgulama masaüstü uygulaması.

Sorumluluk Reddi
Bu yazılım eğitim ve araştırma amaçlı geliştirilmiştir. Yazılımın kullanımından doğabilecek her türlü hukuki sorumluluk kullanıcıya aittir. Resmi ihlal sorgulamaları için lütfen:
e-Devlet: https://www.turkiye.gov.tr/
KGM Resmi Sitesi: https://www.kgm.gov.tr/
adreslerini kullanınız.

## 📋 İçindekiler

- [Genel Bakış](#genel-bakış)
- [Özellikler](#özellikler)
- [Gereksinimler](#gereksinimler)
- [Kurulum](#kurulum)
- [Dosya Yapısı](#dosya-yapısı)
- [Kullanım](#kullanım)
- [Excel Çıktısı](#excel-çıktısı)
- [Plaka Listesi Formatı](#plaka-listesi-formatı)
- [Ayarlar](#ayarlar)

---

## 🎯 Genel Bakış

200+ plaka için [KGM İhlalli Geçiş Sorgulama](https://webihlaltakip.kgm.gov.tr/WebIhlalSorgulama/Sayfalar/Sorgulama.aspx?lang=tr) sitesini otomatize eden masaüstü uygulamasıdır.

Manuel mod ile çalışır: program plakaları sıralar ve formu doldurur, CAPTCHA çözümü ve Sorgula butonu kullanıcıya aittir.

---

## ✨ Özellikler

| Özellik | Açıklama |
|---------|----------|
| **10 Firma Desteği** | KGM, Otoyol, YSS, Avrasya, Avrupa Otoyolu, Anadolu Otoyolu, Kuzey Ege, Ankara-Niğde, Çanakkale, Aydın-Denizli |
| **Araç Tipi Önceliği** | ÇEKİCİ > KAMYON > OTOBÜS > MİNİBÜS > KAMYONET > DORSER > BİNEK |
| **Excel Raporu** | 5 sayfa otomatik oluşturulur |
| **Ödeme Takvimi Filtresi** | Geçmişe/ileriye gün aralığı ayarlanabilir |
| **Mükerrer Kontrolü** | Çıkış tarihi + saati ile benzersiz kayıt |
| **Plaka Editörü** | Araç tipi, sahip, öncelik düzenleme modalı |
| **F11 Tam Ekran** | F11 gir / Esc çık |
| **Günlük JSON Kaydı** | Her gün sıfırdan sorgulanan plakalar |

---

## 💻 Gereksinimler

### Python
```
Python 3.10+
```

Bu projede kullanılan Python yolu:
```
C:\Users\Lenovo\AppData\Local\Python\pythoncore-3.14-64\python.exe
```

### Kütüphaneler
```
pip install selenium webdriver-manager pywin32 openpyxl
```

### Chrome & ChromeDriver
- Google Chrome kurulu olmalı
- `chromedriver.exe` veya `chromedriver-win64\chromedriver.exe` uygulama klasöründe olmalı
- ChromeDriver sürümü Chrome sürümüyle eşleşmeli → [İndir](https://chromedriver.chromium.org/downloads)

---

## 🚀 Kurulum

### 1. Otomatik Kurulum (Önerilen)
```cmd
setup.bat
```
- Python'u otomatik bulur
- Gerekli paketleri kurar
- Kayıt klasörünü sorar
- EXE derler (`dist\KGM_Sorgulama.exe`)

### 2. Manuel Çalıştırma
```cmd
C:\Users\Lenovo\AppData\Local\Python\pythoncore-3.14-64\python.exe kgm_sorgulama.py
```

### 3. EXE Derleme
```cmd
python -m PyInstaller --onefile --windowed --name "KGM_Sorgulama" ^
  --icon="kgm_icon.ico" ^
  --hidden-import="selenium" ^
  --hidden-import="win32gui" ^
  --hidden-import="win32con" ^
  --hidden-import="pywintypes" ^
  --hidden-import="openpyxl" ^
  --collect-all selenium kgm_sorgulama.py
```
EXE oluştuktan sonra `dist\` klasöründen şunları yanına kopyala:
- `plakalistesi.json`
- `kgm_config.json`
- `kgm_icon.ico`
- `chromedriver.exe`

---

## 📁 Dosya Yapısı

```
KGM_Klasoru\
│
├── KGM_Sorgulama.exe          ← Ana program
├── plakalistesi.json         ← Plaka listesi
├── kgm_config.json           ← Ayarlar (otomatik oluşur)
├── kgm_icon.ico              ← Uygulama ikonu
├── chromedriver.exe          ← veya chromedriver-win64\chromedriver.exe
│
├── kgm_chrome_profile\       ← Ayrı Chrome profili (otomatik oluşur)
│
├── sorgulanan_DD.MM.YYYY.json  ← Günlük sorgu kayıtları (otomatik)
└── sorgulama_DD.MM.YYYY.xlsx   ← Günlük Excel raporu (otomatik)
```

---

## 📖 Kullanım

### 1. Uygulamayı Başlat
```cmd
sağtık KGM_Sorgulama.exe
```

### 2. Chrome'u Başlat
Sol paneldeki **"Chrome'u Başlat"** butonuna tıkla. Chrome açılır ve KGM sitesi yüklenir.

### 3. Plaka Sorgula
1. **SEÇ** butonu ile aktif plakayı Chrome'a yükle
2. CAPTCHA'yı çöz
3. **Sorgula** butonuna bas
4. Sonuç yüklenince:
   - İhlal yoksa → **✓ Temiz (Kaydet)**
   - İhlal varsa → **✕ İhlal Var (Kaydet)**

### 4. Navigasyon
- **◀ Önceki / Sonraki ▶** — Plakalar arası geçiş
- **↺ Yenile + Plaka Yükle** — Sayfayı sıfırla ve plakayla doldur

### 5. Tam Ekran
- `F11` → Tam ekrana gir/çık
- `Esc` → Tam ekrandan çık

---

## 📊 Excel Çıktısı

Her sorgulama sonrası `sorgulama_DD.MM.YYYY.xlsx` otomatik güncellenir.

| Sayfa | İçerik |
|-------|--------|
| `DD.MM.YYYY` | Plaka başına günlük özet — 10 firma + toplam |
| `Odeme Takvimi` | Tüm geçişler: Çıkış tarihi, istasyonlar, geçiş ücreti, ödenecek tutar, kalan gün |
| `Gun Ozeti` | Tarihe göre gruplu günlük toplamlar |
| `Odeme Ozeti` | Son ödeme tarihine göre günlük ödenecek toplamlar |
| `Fark Analizi` | Geçiş ücreti ile ödenecek tutar arasındaki fark (ceza analizi) |

### Renk Kodları (Ödeme Takvimi)
| Renk | Anlam |
|------|-------|
| 🔴 Kırmızı | Bugün veya yarın ödenmeli |
| 🟡 Sarı | 1-7 gün içinde |
| 🟢 Yeşil | 8+ gün var |
| ⚫ Gri | Süresi geçmiş |

---

## 📝 Plaka Listesi Formatı

`plakalistesi.json` dosyası:

```json
[
  {"plaka": "34TEST1", "tip": "CEKICI",   "sahip": "Ahmet Yilmaz"},
  {"plaka": "34TEST2", "tip": "KAMYON",   "sahip": "Mehmet Kaya"},
  {"plaka": "34TEST3", "tip": "KAMYON",   "sahip": "Hasan Celik"},
  {"plaka": "34TEST4", "tip": "MINIBUS",  "sahip": "Ali Veli"},
  {"plaka": "34TEST5", "tip": "BINEK",    "sahip": ""}
]
```

### Araç Tipi Değerleri
`CEKICI` · `KAMYON` · `OTOBUS` · `MINIBUS` · `KAMYONET` · `DORSER` · `BINEK` · `DIGER`

### Öncelik Sırası (Sorgulama Sırası)
```
ÇEKİCİ(1) > KAMYON(2) > OTOBÜS(3) > MİNİBÜS(4) > KAMYONET(5) > DORSER(6) > BİNEK(7) > Belirtilmemiş(8) > DİĞER(9)
```

---

## ⚙️ Ayarlar

`kgm_config.json` dosyası (otomatik oluşur):

```json
{
  "gun_filtre": 15,
  "gecmis_gun": 30,
  "kayit_klasoru": "C:\\Users\username\Desktop\\kgm"
}
```

| Alan | Açıklama | Varsayılan |
|------|----------|-----------|
| `gun_filtre` | İleriye kaç gün (max 365) | 15 |
| `gecmis_gun` | Geçmişe kaç gün (max 500) | 30 |
| `kayit_klasoru` | Excel ve JSON kayıt klasörü | Program klasörü |

> **Not:** Filtre aralığı dışında kalan ödemeler Excel'e yazılmaz. Giriş/çıkış istasyonu boş olan toplu borç kayıtları her zaman eklenir.

---

## 🔒 Lisans Doğrulama

Uygulama çalışırken Chrome'da `ugurnakliyat.com` sekmesi **açık olmalıdır**. Bu sekme kapatılırsa:

1. ⚠️ Uyarı penceresi açılır
2. 5 saniye geri sayım başlar
3. Uygulama otomatik kapatılır

---

## ⌨️ Klavye Kısayolları

| Tuş | İşlev |
|-----|-------|
| `F11` | Tam ekran aç/kapat |
| `Esc` | Tam ekrandan çık |
| `Enter` | Plaka ekle (giriş kutusunda) |

---
 

## 🛠️ Sorun Giderme

| Hata | Çözüm |
|------|-------|
| `session not created` | `kgm_chrome_profile` klasörünü sil, tekrar dene |
| `chromedriver not found` | `chromedriver.exe`'yi program klasörüne koy |
| Chrome yerine Opera açılıyor | `opts.binary_location` Chrome.exe yolunu gösteriyor mu kontrol et |
| Excel yazılmıyor | `openpyxl` kurulu mu? `pip install openpyxl` |
| Eski .txt kayıtlar | Otomatik `.json`'a dönüştürülür |

### 🛡️ "Windows kişisel bilgisayarınızı korudu" Uyarısı

> Bu uyarı **tamamen normaldir** — uygulama virüs değildir.
> Windows, ücretli kod imzalama sertifikası bulunmayan her programda bu uyarıyı gösterir.

**Yol 1 — Hızlı geçiş:**
Uyarı ekranında **"Ek bilgi"** → **"Yine de çalıştır"** — bir kez yapılır, bir daha sormaz.

**Yol 2 — Kalıcı çözüm (önerilen):**
EXE → Sağ tıkla → **Özellikler** → **"Engeli kaldır"** kutusunu işaretle → **Tamam**
Artık hiç uyarı vermez.


---

*KGM İhlal Sorgulama v1.0 — 2026*
