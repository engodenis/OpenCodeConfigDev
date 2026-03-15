# Geliştirme Rapor Template'i

**Kullanım**: Bu template `/devs_report/` dizininde yeni raporlar oluştururken kullanılır.  
**Dosya Adı Formatı**: `YYYY-MM-DD-brief-title.md` veya `DEV-XXX-brief-title.md`

---

## 📋 Rapor Başlığı Şablonu

```markdown
# [Geliştirme Başlığı]

**Rapor ID**: DEV-[YYYY]-[MM]-[XXX]  
**Tarih**: YYYY-MM-DD  
**Geliştirici**: [İsim veya Agent]  
**Tip**: [Konfigürasyon/Feature/Bugfix/Refactor]  
**Önem Düzeyi**: 🔴 Yüksek / 🟡 Orta / 🟢 Düşük

---

## 📊 Yürütme Özeti

| Özellik | Değer |
|---------|-------|
| **Sorun** | [Sorunun kısa tanımı] |
| **Çözüm** | [Uygulanan çözüm] |
| **Etki** | [Beklenen etki/sonuç] |
| **Dosyalar** | [Değiştirilen dosyalar listesi] |
| **Değişiklik Sayısı** | [Satır/dosya sayısı] |
| **Test** | [✅/❌ Test durumu] |

---

## 🔍 Sorun Analizi (Sebep)

### 1. [Sorun Başlığı 1]

**Gözlem**: [Gözlemlenen davranış/hata]

**Neden**:
- [Neden 1]
- [Neden 2]
- [Neden 3]

### 2. [Sorun Başlığı 2]

[Eğer birden fazla sorun varsa]

---

## 🎯 Çözüm Stratejisi

### Temel Prensip

[Çözümün temel mantığı ve yaklaşımı]

### Sebep-Sonuç Zinciri

```
Sorun: [Sorun tanımı]
    ↓
Sebep: [Kök neden]
    ↓
Çözüm: [Uygulanan çözüm]
    ↓
Yöntem: [Teknik yaklaşım]
    ↓
Sonuç: [Beklenen sonuç]
```

---

## 🔧 Uygulanan Değişiklikler

### 1. [Değişiklik Grubu 1]

| Bileşen | Önceki | Sonraki | Gerekçe | Risk |
|---------|--------|---------|---------|------|
| [X] | [Y] | [Z] | [Açıklama] | 🟢/🟡/🔴 |

### 2. [Değişiklik Grubu 2]

[Eğer birden fazla grup varsa]

### 3. Korunan/Geri Tutulan Değişiklikler

| Bileşen | Sebep |
|---------|-------|
| [X] | [Neden değiştirilmedi] |

---

## 📊 Sonuç Analizi

### 1. [Metrik 1]

**Önceki**: [Değer]

**Sonraki**: [Değer]

**İyileştirme**: [% veya mutlak fark]

### 2. [Metrik 2]

[Eğer birden fazla metrik varsa]

---

## ⚠️ Riskler ve Mitigasyonlar

### Risk 1: [Risk Adı]

**Risk**: [Risk tanımı]

**Mitigasyon**:
- [Önlem 1]
- [Önlem 2]

### Risk 2: [Risk Adı]

[Eğer birden fazla risk varsa]

---

## 🧪 Test Sonuçları

### Test 1: [Test Adı]

```
[Command/Adım]
[Output]
[Durum: ✅/❌]
```

### Test 2: [Test Adı]

[Eğer birden fazla test varsa]

---

## 📚 Öğrenilen Dersler

### 1. [Ders Başlığı]

[Detaylı açıklama ve bağlam]

### 2. [Ders Başlığı]

[Eğer birden fazla ders varsa]

---

## 🔮 Gelecek Geliştirmeler

### [Gelecek Özellik 1]

[Planlanan geliştirme]

### [Gelecek Özellik 2]

[Eğer birden fazla plan varsa]

---

## 📎 Referanslar

- [Dokümantasyon/Plan/PR linkleri]

---

**Rapor Durumu**: [⏳ Devam Ediyor / ✅ Tamamlandı / ❌ İptal]  
**Sonraki Rapor**: [Varsa sonraki rapor ID'si]

---

*Bu rapor `/devs_report/` dizininde tutulmaktadır.*
```

---

## 📝 Rapor Yazım Rehberi

### 1. Sebep-Sonuç İlişkisi

Her değişiklik için şu zinciri takip edin:

1. **SORUN**: Ne gözlemledik?
2. **ANALİZ**: Neden oluyor?
3. **ÇÖZÜM**: Ne yapıyoruz?
4. **SONUÇ**: Ne değişecek?

### 2. Risk Değerlendirmesi

Her değişiklik için risk seviyesi belirleyin:

- 🟢 **Düşük Risk**: Geri alınabilir, test edilmiş, yaygın pattern
- 🟡 **Orta Risk**: Yeni pattern, dikkat gerektiren, izleme gerekli
- 🔴 **Yüksek Risk**: Breaking change, kritik sistem, geri alınamaz

### 3. Önem Düzeyi Kriterleri

| Düzey | Kriterler | Örnek |
|-------|-----------|-------|
| 🔴 **Yüksek** | Kritik hata çözümü, performans etkisi, breaking change | API değişikliği, güvenlik fix |
| 🟡 **Orta** | Yeni özellik, optimizasyon, refactoring | Yeni modül, kod temizliği |
| 🟢 **Düşük** | Dokümantasyon, küçük fix, stil değişikliği | Yorum ekleme, typo düzeltme |

### 4. Ne Zaman Rapor Oluşturulmalı?

**Oluştur**:
- ✅ Konfigürasyon değişiklikleri
- ✅ Mimari değişiklikler
- ✅ Performance optimizasyonları
- ✅ Breaking changes
- ✅ Yeni özellikler
- ✅ Kritik bug fix'ler

**Oluşturma**:
- ❌ Tek dosya typo düzeltmesi
- ❌ Yorum ekleme
- ❌ Formatting değişiklikleri
- ❌ Test dosyası güncellemeleri

---

## 📁 Dizin Yapısı

```
devs_report/
├── README.md                    # Bu dosya
├── TEMPLATE.md                  # Rapor template'i
├── opencode-go-provider-optimization.md  # Örnek rapor
└── [YYYY-MM-DD-feature-name.md] # Yeni raporlar
```

---

## 🔧 report_changes Komutu

Bu komut, önemli değişiklikler için otomatik rapor oluşturur.

### Kullanım

```
report_changes
```

### Ne Zaman Çalıştırılır?

1. Önemli bir değişiklik tamamlandığında
2. Kullanıcı "rapor oluştur" istediğinde
3. Konfigürasyon değişikliklerinden sonra

### Çıktı

Komut çalıştırıldığında:
1. Son değişiklikleri analiz eder
2. Rapor ID oluşturur
3. `/devs_report/` dizinine markdown dosyası yazar
4. Kullanıcıya rapor özetini gösterir

---

**Son Güncelleme**: 2026-03-15  
**Versiyon**: 1.0.0
