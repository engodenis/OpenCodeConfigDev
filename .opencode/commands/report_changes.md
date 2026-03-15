---
description: Son önemli değişiklikler için geliştirme raporu oluştur
agent: sisyphus
model: opencode-go/kimi-k2.5
mode: all
---

# Son Değişiklikler İçin Geliştirme Raporu Oluştur

## 🎯 Görev

Son yapılan git değişikliklerini analiz et ve sebep-sonuç ilişkisi içinde detaylı bir geliştirme raporu oluştur.

## 📋 Yapılacaklar

### 1. Değişiklik Analizi
- Son commit'ten bu yana değişen dosyaları tespit et
- **Takip edilmeyen (untracked) dosya ve dizinleri tespit et**
- Değişiklik türünü belirle (config/feature/bugfix/refactor/docs)
- Etkilenen sistemleri ve bileşenleri analiz et
- Breaking change olup olmadığını kontrol et

### 1a. Untracked Dosyaların Yönetimi

Untracked dosyalar git tarafından henüz izlenmeyen, yeni eklenen dosyalardır. Raporlama için önemlidir:

**Tespit Komutu:**
```bash
# Sadece untracked dosyaları göster
git ls-files --others --exclude-standard

# Untracked dizinleri göster
git status --porcelain | grep "^??"
```

**Değerlendirme:**
- [ ] Hangi untracked dosyalar var?
- [ ] Bu dosyalar rapora dahil edilmeli mi?
- [ ] Kullanıcıya "Bu dosyaları ekleyeyim mi?" diye sor

**Kullanıcı Seçenekleri:**
```
📁 Yeni (Untracked) Dosyalar Tespit Edildi:
  ?? .opencode/commands/
  ?? .sisyphus/plans/
  ?? devs_report/

❓ Bu dosyalar rapora dahil edilsin mi?
  [1] Evet, tümünü ekle
  [2] Sadece belirli olanları ekle (seçim yap)
  [3] Hayır, dışarıda bırak
  [4] .gitignore'a ekle (takip etme)
```

**Rapor İçeriği:**
- Untracked dosyalar ayrı bir bölümde listelenir
- "Yeni Eklenecek Dosyalar" başlığı altında gösterilir
- Her dosya için: boyut, tip, içerik özeti
- Değişiklik türünü belirle (config/feature/bugfix/refactor/docs)
- Etkilenen sistemleri ve bileşenleri analiz et
- Breaking change olup olmadığını kontrol et

### 2. Önem Düzeyi Değerlendirmesi
Aşağıdaki kriterlere göre önem düzeyini hesapla:

**🔴 Yüksek (High)**
- 10+ dosya değişikliği
- Konfigürasyon dosyası değişikliği
- API değişikliği / Breaking change
- Database migration/schema değişikliği
- Güvenlik fix'i

**🟡 Orta (Medium)**
- 5-10 dosya değişikliği
- Yeni özellik eklenmesi
- Refactoring işlemi
- Test coverage değişikliği

**🟢 Düşük (Low)**
- 1-5 dosya değişikliği
- Dokümantasyon güncellemesi
- Typo düzeltmesi
- Formatting değişikliği

### 3. Rapor ID Oluşturma
Format: `DEV-[YYYY]-[MM]-[NNN]`
- YYYY: Yıl
- MM: Ay
- NNN: Otomatik artan numara (001, 002, ...)

Örnek: `DEV-2026-03-001`

### 4. Rapor Başlığı Belirleme
- Değişikliği özetleyen kısa ve açıklayıcı başlık
- Kullanıcıdan onay al (önerilen başlık göster)
- Örnek: "opencode-go-provider-optimization"

### 5. Sebep-Sonuç Zinciri Oluşturma
Her değişiklik için:
- **SORUN**: Ne gözlemledik?
- **ANALİZ**: Neden oluyor?
- **ÇÖZÜM**: Ne yapıyoruz?
- **SONUÇ**: Ne değişecek?

### 6. Risk Değerlendirmesi
Her değişiklik için risk analizi yap:
- Risk tanımı
- Etki alanı
- Mitigasyon stratejileri
- Geri alma planı (rollback)

### 7. Rapor Dosyası Oluşturma

**Konum**: `devs_report/YYYY-MM-DD-[baslik].md`

**Şablon Bölümleri**:
1. Rapor Başlığı ve Meta Bilgiler
2. Yürütme Özeti (tablo formatında)
3. Sorun Analizi (Sebep)
4. Çözüm Stratejisi
5. Uygulanan Değişiklikler (tablolar)
6. Sonuç Analizi (metrikler)
7. Riskler ve Mitigasyonlar
8. Test Sonuçları
9. Öğrenilen Dersler
10. Gelecek Geliştirmeler
11. Referanslar

## ⚠️ Önemli Kurallar

### Rapor Oluşturma Kriterleri
**Oluştur** (Aşağıdaki durumlarda rapor oluştur):
- ✅ Konfigürasyon değişiklikleri
- ✅ Mimari değişiklikler
- ✅ Performance optimizasyonları
- ✅ Breaking changes
- ✅ Yeni özellikler
- ✅ Kritik bug fix'ler
- ✅ 5+ dosya değişikliği

**Oluşturma** (Aşağıdaki durumlarda rapor oluşturma, kullanıcıya bildir):
- ❌ Tek dosya typo düzeltmesi
- ❌ Yorum ekleme
- ❌ Formatting değişiklikleri
- ❌ Test dosyası güncellemeleri (sadece test)
- ❌ README güncellemesi (küçük)

### Dosya Formatı Standartları
- Markdown formatında yaz
- Türkçe dilinde hazırla
- Emoji kullanımı: 🔍 🎯 ✅ ❌ ⚠️ 📊 📚
- Tablolar markdown formatında
- Kod blokları dil belirterek

### İçerik Kalitesi
- Sebep-sonuç ilişkisini açıkça kur
- Teknik detaylar ver
- Varsayımları belirt
- Alternatif çözümleri değerlendir
- Ölçülebilir metrikler kullan

## 📝 Rapor İçeriği Detayları

### Meta Bilgiler
```markdown
**Rapor ID**: DEV-YYYY-MM-XXX
**Tarih**: YYYY-MM-DD
**Geliştirici**: [AI Agent veya kullanıcı adı]
**Tip**: [config/feature/bugfix/refactor/docs]
**Önem Düzeyi**: [🔴 Yüksek / 🟡 Orta / 🟢 Düşük]
```

### Yürütme Özeti Tablosu
```markdown
| Özellik | Değer |
|---------|-------|
| **Sorun** | [Kısa tanım] |
| **Çözüm** | [Uygulanan çözüm] |
| **Etki** | [Beklenen etki] |
| **Dosyalar** | [Değişen dosyalar listesi] |
| **Değişiklik Sayısı** | [+/- satır sayısı] |
| **Test** | [✅/❌ Test durumu] |
```

### Değişiklik Tablosu
```markdown
| Bileşen | Önceki | Sonraki | Gerekçe | Risk |
|---------|--------|---------|---------|------|
| [X] | [Y] | [Z] | [Açıklama] | 🟢/🟡/🔴 |
```

### Yeni (Untracked) Dosyalar Tablosu
```markdown
| Dosya/Dizin | Tip | Boyut | Açıklama | Rapor Durumu |
|-------------|-----|-------|----------|--------------|
| .opencode/ | Config | 15KB | OpenCode komutları | ✅ Dahil edildi |
| .sisyphus/ | Docs | 8KB | Planlama dosyaları | ✅ Dahil edildi |
| devs_report/ | Docs | 12KB | Geliştirme raporları | ✅ Dahil edildi |
```

**Not**: Untracked dosyalar git tarafından henüz izlenmiyor ancak raporda belgelenmelidir.

## 🔍 Analiz Adımları
```

## 🔍 Analiz Adımları

1. **Git Log Analizi**
   ```bash
   git log --oneline -10
   git diff --name-only HEAD~1
   git diff --stat HEAD~1
   ```

2. **Dosya İçeriği Analizi**
   - Değişen dosyaları oku
   - Config dosyası mı kontrol et
   - API endpoint değişikliği mi kontrol et
   - Database migration var mı kontrol et

2a. **Untracked Dosya Analizi**
   ```bash
   # Untracked dosyaları listele
   git ls-files --others --exclude-standard
   
   # Untracked dosya detayları
   git status --porcelain | grep "^??"
   ```
   
   - Her untracked dosyayı incele
   - İçerik tipini belirle (kaynak kod, config, doküman)
   - Boyutunu kontrol et (büyük dosyalar için uyarı)
   - Raporlamaya dahil edilip edilmeyeceğine karar ver

3. **Commit Mesajı Analizi**
   - Değişen dosyaları oku
   - Config dosyası mı kontrol et
   - API endpoint değişikliği mi kontrol et
   - Database migration var mı kontrol et

3. **Commit Mesajı Analizi**
   - feat:, fix:, refactor:, docs:, config: prefix'lerini kontrol et
   - Breaking change belirtileri var mı kontrol et
   - İlgili issue/PR var mı kontrol et

4. **Test Durumu Kontrolü**
   - Test dosyaları değişti mi?
   - Yeni test eklendi mi?
   - Test coverage etkilendi mi?

## 🎨 Rapor Yazım Stili

### Dil ve Ton
- Profesyonel ve teknik
- Açık ve anlaşılır
- Tarafsız ve objektif
- Gelecek zaman kullanma ("yapılacak" değil "yapıldı")

### Yapı
- Başlıklar hiyerarşik (H1, H2, H3)
- Kısa paragraflar (3-4 cümle)
- Maddeli listeler kullan
- Tablolar ile karşılaştırma yap

### Görsel Unsurlar
- Emoji kullanımı tutarlı
- Kod blokları vurgulu
- Tablolar hizalı
- Linkler tıklanabilir

## ✅ Doğrulama Kontrol Listesi

Rapor tamamlandığında şunları kontrol et:

- [ ] Rapor ID doğru formatta ve benzersiz
- [ ] Tarih doğru
- [ ] Tüm değişen dosyalar listelenmiş
- [ ] Sebep-sonuç zinciri mantıklı
- [ ] Riskler belirtilmiş
- [ ] Test sonuçları (varsa) eklenmiş
- [ ] Öğrenilen dersler yazılmış
- [ ] Referanslar verilmiş
- [ ] Dil ve yazım hatası yok
- [ ] Markdown formatı doğru

## 🚀 Çıktı

Rapor tamamlandığında kullanıcıya şunları bildir:

1. Rapor dosya yolu
2. Rapor ID
3. Önem düzeyi
4. Kısa özet (3-5 bullet)
5. Sonraki adımlar (varsa)

Örnek:
```
✅ Rapor oluşturuldu!

📄 Dosya: /devs_report/2026-03-15-opencode-go-optimization.md
🆔 ID: DEV-2026-03-001
🔴 Önem Düzeyi: Yüksek

Özet:
• 17 model ataması güncellendi
• %60-70 maliyet tasarrufu sağlandı
• Gemini rate limiting sorunu çözüldü
• OpenCode Go provider'ına geçiş yapıldı

Sonraki Adımlar:
• Yeni konfigürasyonu test et
• Kullanım limitlerini izle
```

## 🔄 Alternatif Senaryolar

### Senaryo 1: Çok Küçük Değişiklik
Eğer sadece 1-2 dosya ve basit bir değişiklik varsa:
- "Bu değişiklik raporlanmayacak kadar küçük." mesajı ver
- Kullanıcıdan onay iste (yine de rapor isteyebilir)
- Gerekirse kısa bir özet oluştur

### Senaryo 2: Birden Fazla Commit
Eğer birden fazla commit varsa:
- Son commit odaklı analiz yap
- Veya kullanıcıdan hangi commit aralığını istediğini sor
- Örnek: "Son 3 commit mi analiz edeyim?"

### Senaryo 3: Değişiklik Yok
Eğer değişiklik yoksa:
- "Son commit'ten bu yana değişiklik bulunamadı." mesajı ver
- Kullanıcıdan commit aralığı veya dosya belirtmesini iste

### Senaryo 4: Sadece Untracked Dosyalar Var
Eğer commitlenmiş değişiklik yok ama untracked dosyalar varsa:

```
📁 Untracked Dosyalar Tespit Edildi:
  ?? .opencode/commands/report_changes.md
  ?? .opencode/commands/push_repo.md
  ?? devs_report/opencode-go-optimization.md

💡 Bu dosyalar henüz git tarafından izlenmiyor.

❓ Bu dosyaları rapora dahil edeyim mi?
  [1] Evet, tüm untracked dosyaları ekle
  [2] Sadece belirli dosyaları ekle
  [3] Hayır, rapor oluşturma
  [4] Önce git add yap, sonra raporla

📝 Not: Untracked dosyalar commit edilmemiş olsa bile 
geliştirme sürecinin önemli parçasıdır ve dokümante edilmelidir.
```

**Rapor İçeriği:**
- "Yeni Oluşturulan Dosyalar" başlığı altında listele
- Her dosyanın amacını ve içeriğini özetle
- Git durumunu (untracked) belirt
- Gelecekte commit edilmesi gerektiğini not düş

## 📚 Referanslar
Eğer değişiklik yoksa:
- "Son commit'ten bu yana değişiklik bulunamadı." mesajı ver
- Kullanıcıdan commit aralığı veya dosya belirtmesini iste

## 📚 Referanslar

- [Geliştirme Raporu Template](/devs_report/TEMPLATE.md)
- [Geliştirme Rapor Rehberi](/devs_report/README.md)
- [Git Best Practices](https://git-scm.com/doc)
- [Markdown Guide](https://www.markdownguide.org/)

---

**Versiyon**: 1.0.0
**Son Güncelleme**: 2026-03-15
**Yazar**: OpenCode AI Agent (Sisyphus)
