# Custom Komut: `report_changes`

## 📋 Genel Bakış

`report_changes` komutu, son yapılan önemli geliştirmeler için otomatik rapor oluşturan bir OpenCode custom komutudur.

**Amaç**: Önemli değişikliklerin sebep-sonuç ilişkisi içinde dokümante edilmesini sağlamak  
**Hedef**: `/devs_report/` dizini altında standart formatta markdown raporları oluşturmak  
**Kullanım**: `report_changes` veya `@report_changes`

---

## 🎯 Kullanım Senaryoları

### Senaryo 1: Konfigürasyon Değişikliği Sonrası

```
Kullanıcı: Son yapılan API endpoint değişikliklerini raporla
→ report_changes komutu çalıştırılır
→ Değişiklikler analiz edilir
→ Rapor oluşturulur: `/devs_report/2026-03-15-api-endpoint-refactor.md`
```

### Senaryo 2: Önemli Feature Tamamlandığında

```
Kullanıcı: report_changes
→ Son commit'ten sonraki değişiklikler taranır
→ Önem düzeyi değerlendirilir (🔴🟡🟢)
→ Rapor şablonu doldurulur
→ Dosya oluşturulur ve gösterilir
```

### Senaryo 3: Mimari Karar Dokümantasyonu

```
Kullanıcı: Yeni veritabanı şeması değişikliklerini raporla
→ Schema değişiklikleri analiz edilir
→ Migration dosyaları incelenir
→ Sebep-sonuç zinciri oluşturulur
→ Rapor: `/devs_report/db-schema-migration-v2.md`
```

---

## 🔧 Komut Spesifikasyonu

### Komut İmzası

```typescript
interface ReportChangesCommand {
  name: "report_changes";
  aliases: ["@report_changes", "/report"];
  description: "Son önemli değişiklikler için geliştirme raporu oluşturur";
  
  parameters: {
    // Opsiyonel: Belirli bir değişiklik setini raporla
    scope?: "last_commit" | "last_session" | "all_unreported" | "file_path";
    
    // Opsiyonel: Manuel rapor ID (otomatik atanır)
    reportId?: string;
    
    // Opsiyonel: Önem düzeyi (otomatik tespit edilir)
    severity?: "high" | "medium" | "low";
  };
}
```

### Çalışma Akışı

```
1. Kullanıcı "report_changes" der
   ↓
2. Git değişiklikleri analiz edilir
   - Son commit'ten bu yana değişen dosyalar
   - Değişiklik türü (config/feature/bugfix/refactor)
   - Etkilenen bileşenler
   ↓
3. Önem düzeyi değerlendirmesi
   - Dosya sayısı > 5 → Yüksek
   - Konfigürasyon dosyası → Yüksek/Orta
   - API değişikliği → Yüksek
   - Test dosyası → Düşük (raporlama)
   ↓
4. Rapor ID oluşturulur
   - Format: DEV-[YYYY]-[MM]-[NNN]
   - Örnek: DEV-2026-03-001
   ↓
5. Rapor başlığı önerilir
   - Kullanıcı onaylar veya değiştirir
   ↓
6. Rapor şablonu doldurulur
   - Temel bilgiler (tarih, geliştirici)
   - Değişiklikler tablosu
   - Sebep-sonuç analizi
   ↓
7. Dosya oluşturulur
   - Konum: `/devs_report/[tarih]-[baslik].md`
   - Template: `/devs_report/TEMPLATE.md` baz alınır
   ↓
8. Kullanıcıya özet sunulur
   - Rapor dosya yolu
   - İçerik özeti
   - Sonraki adımlar
```

---

## 📊 Akıllı Analiz Özellikleri

### 1. Değişiklik Tespiti

```typescript
interface ChangeAnalysis {
  // Değişen dosyalar
  files: string[];
  
  // Değişiklik türü
  type: "config" | "feature" | "bugfix" | "refactor" | "docs";
  
  // Etkilenen sistemler
  affectedSystems: string[];
  
  // Breaking change var mı?
  isBreaking: boolean;
  
  // Test coverage etkisi
  testImpact: "none" | "added" | "modified" | "required";
}
```

### 2. Önem Düzeyi Hesaplama

```typescript
function calculateSeverity(analysis: ChangeAnalysis): Severity {
  let score = 0;
  
  // Dosya sayısı
  if (analysis.files.length > 10) score += 3;
  else if (analysis.files.length > 5) score += 2;
  else if (analysis.files.length > 1) score += 1;
  
  // Konfigürasyon dosyası
  if (analysis.files.some(f => f.includes("config"))) score += 3;
  
  // API değişikliği
  if (analysis.files.some(f => f.includes("api") || f.includes("route"))) score += 2;
  
  // Breaking change
  if (analysis.isBreaking) score += 3;
  
  // Database değişikliği
  if (analysis.files.some(f => f.includes("migration") || f.includes("schema"))) score += 3;
  
  // Sonuç
  if (score >= 6) return "🔴 high";
  if (score >= 3) return "🟡 medium";
  return "🟢 low";
}
```

### 3. Sebep-Sonuç Çıkarımı

```typescript
interface CausalAnalysis {
  // Tespit edilen sorunlar (PR açıklamasından, commit mesajından)
  problems: string[];
  
  // Uygulanan çözümler
  solutions: string[];
  
  // Beklenen sonuçlar
  expectedOutcomes: string[];
  
  // Riskler
  risks: string[];
}
```

---

## 📝 Rapor Oluşturma Formatı

### Otomatik Doldurulan Alanlar

```markdown
# [Rapor Başlığı]

**Rapor ID**: DEV-[YYYY]-[MM]-[XXX]  
**Tarih**: [Otomatik: Bugün]  
**Geliştirici**: [Kullanıcı adı veya AI Agent]  
**Tip**: [Otomatik tespit: config/feature/bugfix/refactor]  
**Önem Düzeyi**: [🔴🟡🟢 Otomatik hesaplanır]

---

## 📊 Yürütme Özeti

| Özellik | Değer |
|---------|-------|
| **Sorun** | [Commit mesajından veya PR açıklamasından çıkarılır] |
| **Çözüm** | [Uygulanan değişiklik özeti] |
| **Etki** | [Dosya sayısı ve etki alanı] |
| **Dosyalar** | [Değişen dosya listesi] |
| **Değişiklik Sayısı** | [+/- satır sayısı] |
| **Test** | [✅/❌ Test durumu] |
```

### Kullanıcıdan İstenen Alanlar

Komut çalıştırıldığında şu bilgiler kullanıcıdan istenir:

1. **Rapor Başlığı** (Otomatik öneri sunulur)
2. **Sorun Açıklaması** (Commit mesajı kullanılabilir)
3. **Risk Değerlendirmesi** (Varsayılan: Otomatik hesaplanan)
4. **Öğrenilen Dersler** (Opsiyonel)

---

## 🎨 Etkileşim Akışı

### Akış Diyagramı

```
[report_changes]
    ↓
"Son değişiklikler analiz ediliyor..."
    ↓
[Git diff analizi]
    ↓
"Aşağıdaki değişiklikler tespit edildi:"
- 5 dosya değişti
- 2 yeni dosya
- 1 konfigürasyon güncellemesi
    ↓
"Rapor başlığı önerisi: api-authentication-refactor"
[Onaylıyor musunuz?] → [Hayır] → [Yeni başlık girin]
    ↓ [Evet]
"Sorun açıklaması girin (opsiyonel):"
[Varsayılan: Commit mesajı]
    ↓
"Risk düzeyi: 🟡 Orta (Otomatik)"
[Onaylıyor musunuz?] → [Hayır] → [🔴🟡🟢 Seçin]
    ↓
Rapor oluşturuluyor...
    ↓
"✅ Rapor oluşturuldu:"
"/devs_report/2026-03-15-api-authentication-refactor.md"
    ↓
[Rapor içeriği önizlemesi]
```

---

## 🔌 Entegrasyon Önerileri

### 1. Git Hook Entegrasyonu

```bash
# .git/hooks/post-commit
# Önemli değişiklikler için otomatik rapor önerisi

if [ $(git diff HEAD~1 --name-only | wc -l) -gt 5 ]; then
  echo "⚠️  Önemli değişiklik tespit edildi. Rapor oluşturmak için: report_changes"
fi
```

### 2. CI/CD Entegrasyonu

```yaml
# .github/workflows/report.yml
on:
  push:
    branches: [main]
    
jobs:
  generate-report:
    if: contains(github.event.head_commit.message, '[REPORT]')
    steps:
      - name: Generate Development Report
        run: |
          opencode report_changes --auto-approve
```

### 3. VS Code Extension

```json
// .vscode/settings.json
{
  "opencode.autoReport": {
    "enabled": true,
    "threshold": 5,  // 5+ dosya değişikliğinde rapor öner
    "prompt": true   // Kullanıcıdan onay iste
  }
}
```

---

## 📁 Dosya Organizasyonu

### Dizin Yapısı

```
devs_report/
├── README.md                           # Raporlama rehberi
├── TEMPLATE.md                         # Şablon dosyası
├── INDEX.md                           # Tüm raporların indeksi
│
├── 2026/
│   ├── 03-march/
│   │   ├── 2026-03-15-opencode-go-optimization.md
│   │   ├── 2026-03-14-api-v2-release.md
│   │   └── 2026-03-10-db-migration.md
│   └── 02-february/
│       └── ...
│
└── archived/                          # Eski raporlar
    └── 2025/
```

### İndeks Dosyası (Otomatik Güncellenir)

```markdown
# Geliştirme Raporları İndeksi

## 🔴 Yüksek Önemli
- [DEV-2026-03-001] OpenCode Go Provider Optimizasyonu (2026-03-15)
- [DEV-2026-03-002] API v2 Breaking Changes (2026-03-14)

## 🟡 Orta Önemli
- [DEV-2026-03-003] Database Schema Migration (2026-03-10)

## 🟢 Düşük Önemli
- [DEV-2026-03-004] README Güncellemesi (2026-03-08)

## 📊 İstatistikler
- Toplam Rapor: 4
- Bu Ay: 2
- Yüksek Önemli: 2
```

---

## 🧪 Test Senaryoları

### Test 1: Basit Değişiklik

**Durum**: 1 dosya, typo düzeltmesi
**Beklenen**: "Bu değişiklik raporlanmayacak kadar küçük. Onaylıyor musunuz?"

### Test 2: Konfigürasyon Değişikliği

**Durum**: `config.json` güncellendi
**Beklenen**: 🔴 Yüksek önem düzeyi, otomatik rapor oluşturma

### Test 3: Çoklu Dosya Değişikliği

**Durum**: 12 dosya, yeni feature
**Beklenen**: Detaylı rapor şablonu, tüm dosyalar listelenecek

### Test 4: Breaking Change

**Durum**: API endpoint değişikliği
**Beklenen**: Risk analizi, migration rehberi önerisi

---

## 🔮 Gelecek Geliştirmeler

### V2.0 Özellikleri

1. **AI Destekli Analiz**
   - Değişikliklerin otomatik sınıflandırılması
   - Risk tahmini
   - Sebep-sonuç çıkarımı

2. **Rapor Karşılaştırma**
   - `compare_reports DEV-001 DEV-002`
   - Fark analizi

3. **İstatistik Dashboard**
   - Aylık değişiklik trendleri
   - Kod kalitesi metrikleri
   - Rapor kapsamı analizi

4. **Markdown'dan Geri Dönüşüm**
   - Rapor -> Kod değişikliği önerisi
   - Task list oluşturma

5. **Çoklu Dil Desteği**
   - Türkçe/İngilizce raporlar
   - Otomatik çeviri

---

## 📝 Örnek Kullanım Diyalogları

### Örnek 1: Öneri ile

```
Kullanıcı: report_changes

Sisyphus: Son değişiklikler analiz ediliyor...
         3 dosya değişmiş, 1 konfigürasyon güncellemesi tespit edildi.
         
         Önerilen rapor başlığı: "api-rate-limiting-config"
         Onaylıyor musunuz? (Evet/Hayır/Yeni başlık)

Kullanıcı: evet

Sisyphus: Rapor oluşturuldu: /devs_report/2026-03-15-api-rate-limiting-config.md
         🔴 Yüksek önem düzeyi (konfigürasyon değişikliği)
         
         Özet:
         - Sorun: API rate limiting sorunu
         - Çözüm: Yeni throttle konfigürasyonu
         - Dosyalar: config/api.yaml, src/middleware/rateLimiter.ts
```

### Örnek 2: Detaylı

```
Kullanıcı: Son database migration'ı raporla

Sisyphus: Migration dosyaları analiz ediliyor...
         2 migration, 1 schema değişikliği bulundu.
         
         Rapor başlığı: db-schema-v3-migration
         
         Değişiklikler:
         1. users tablosuna email_verified_at kolonu
         2. orders tablosuna status indeksi
         
         Bu bir breaking change mi? (Evet/Hayır/Bilmiyorum)

Kullanıcı: hayır

Sisyphus: Rapor oluşturuldu! 
         İncelemek için: cat /devs_report/2026-03-15-db-schema-v3-migration.md
```

---

## ⚙️ Konfigürasyon

### `.opencode/config.json`

```json
{
  "commands": {
    "report_changes": {
      "enabled": true,
      "auto_prompt": true,
      "min_files_threshold": 2,
      "output_directory": "./devs_report",
      "template": "./devs_report/TEMPLATE.md",
      "default_author": "Sisyphus",
      "git_integration": true,
      "severity_rules": {
        "high": ["config", "migration", "api"],
        "medium": ["feature", "refactor"],
        "low": ["docs", "test", "style"]
      }
    }
  }
}
```

---

## 📚 Referanslar

- [OpenCode Commands Documentation](https://opencode.ai/docs/tr/commands/)
- [Custom Tools Guide](https://opencode.ai/docs/tr/custom-tools/)
- [oh-my-opencode Features](https://github.com/code-yeongyu/oh-my-openagent/blob/dev/docs/reference/features.md)

---

**Versiyon**: 1.0.0  
**Son Güncelleme**: 2026-03-15  
**Durum**: 🚧 Tasarım Aşamasında - Implementasyon Bekliyor

---

*Bu doküman `devs_report/` dizininde tutulur ve proje ekibi tarafından geliştirilir.*
