# 🎓OpenCode &oh-my-openagent Eğitim Materyalleri

## 📚 İçindekiler

Bu eğitim serisi,OpenCode ve oh-my-openagent'i profesyonel seviyede kullanmanız için tasarlanmıştır.

| Bölüm | Konu | Dosya |
|-------|------|-------|
| **1** | Konfigürasyon Detayları | `docs/01-Configuration-Guide.md` |
| **2** | Ajan Kullanımı | `docs/02-Agent-Usage-Guide.md` |
| **3** | Kategori Sistemi | `docs/03-Category-System-Guide.md` |
| **4** | Paralel Çalışma | `docs/04-Parallel-Execution-Guide.md` |
| **5** | Skills, Hooks, MCP | `docs/05-Skills-Hooks-MCP-Guide.md` |
| **6** | Pratik Senaryo | `docs/06-Practical-Workflow-Guide.md` |
| **0** | Optimize Edilmiş Konfigürasyon | `opencode_config_dev/oh-my-opencode-optimized.jsonc` |

---

## 📥 Kurulum

### İnsanlar İçin

Bu komutu LLM ajanınıza (Claude Code, AmpCode, Cursor, vb.) yapıştırın:

```
oh-my-opencode kurulumu için şu talimatları takip et:
https://raw.githubusercontent.com/engodenis/OpenCodeConfigDev/refs/heads/master/setup/INSTALLATION-GUIDE.md
```
```
Veya detaylı kurulum rehberini okuyun: setup/INSTALLATION-GUIDE.md
Ama ciddi, bir ajana yaptırın. İnsanlar config'leri eliyle mahveder.
```

### LLM Ajanları İçin

Kurulum rehberini indir ve takip et:

```bash
curl -s https://raw.githubusercontent.com/engodenis/OpenCodeConfigDev/refs/heads/master/setup/INSTALLATION-GUIDE.md
```

Detaylı rehber için: [`setup/INSTALLATION-GUIDE.md`](setup/INSTALLATION-GUIDE.md)

---

## 📖 Kısa Özet

### BÖLÜM 1: Konfigürasyon

**Ana dosya:** `opencode_config_dev/oh-my-opencode-optimized.jsonc`

Bir önceki derste oluşturduğum optimize edilmiş konfigürasyon dosyası şunları içerir:

- **11 Ajan yapılandırması** (Sisyphus, Hephaestus, Prometheus, vb.)
- **8 Kategori tanımı** (visual-engineering, ultrabrain, deep, vb.)
- **Concurrency ayarları** (paralel çalışma limitleri)
- **Hook yapılandırmaları** (yaşam döngüsü müdahaleleri)
- **MCP ayarları** (dış dünya bağlantıları)

**Detaylı rehber:** `docs/01-Configuration-Guide.md`

### BÖLÜM 2: Ajan Kullanımı

**Kilit noktalar:**
- Her ajanın farklı bir "kişiliği" var
- Model seçimi ajanın karakterine uygun olmalı
- Salt okunur ajanlar var (Oracle, Librarian, Explore)
- Hephaestus için GPT zorunlu

**Detaylı rehber:** `docs/02-Agent-Usage-Guide.md`

### BÖLÜM 3: Kategori Sistemi

**Kilit noktalar:**
- Kategori = İş türü → Otomatik model seçimi
- `visual-engineering` → Gemini (frontend için en iyi)
- `ultrabrain` → GPT-5.4 (derin düşünme için)
- `quick` → Flash/Haiku (basit işler için ucuz)

**Detaylı rehber:** `docs/03-Category-System-Guide.md`

### BÖLÜM 4: Paralel Çalışma

**Kilit noktalar:**
- `run_in_background: true` → Arka planda çalış
- Bağımsız görevleri paralel başlat
- Sonuçları sistem bildirimi gelince topla
- Concurrency limitlerini kontrol et

**Detaylı rehber:** `docs/04-Parallel-Execution-Guide.md`

### BÖLÜM 5: Skills, Hooks, MCP

**Kilit noktalar:**
- Skill = Uzmanlık + MCP araçları
- Hook = Yaşam döngüsü müdahalesi
- MCP = Dış dünya bağlantısı
- `load_skills` parametresi ile skill yükle

**Detaylı rehber:** `docs/05-Skills-Hooks-MCP-Guide.md`

### BÖLÜM 6: Pratik Senaryo

**Örnek iş akışı:**
1. Intent Gate → Görevi sınıflandır
2. Paralel araştırma → Explore + Librarian
3. Planlama → Prometheus
4. Yürütme → Atlas + Category delegation
5. Doğrulama → lsp_diagnostics + tests
6. Bitirme → %100 tamamlama garantisi

**Detaylı rehber:** `docs/06-Practical-Workflow-Guide.md`

---

## 🚀 Hızlı Başlangıç

```bash
# 1. Konfigürasyonu kopyala
cp opencode_config_dev/oh-my-opencode-optimized.jsonc ~/.config/opencode/oh-my-opencode.jsonc

# 2. OpenCode başlat
cd your-project
opencode

#3. Proje başlat
/init-deep

# 4. İlk görev
ulw add a simple health check endpoint
```

---

## 📊 Hızlı Referans

| Ne Yapmak İstiyorsunuz? | Komut/Yöntem |
|-------------------------|--------------|
| Basit bir işi bitirmek | `ulw <görev>` |
| Planlamak | `@plan "<görev>"` veya Tab→Prometheus |
| Planı yürütmek | `/start-work` |
| Paralel arama | `task(subagent_type="explore", run_in_background=true, ...)` |
| Oracle'a sormak | `@oracle <soru>` |
| Dokümantasyon bulmak | `@librarian <sorgu>` |
| Pattern bulmak | `@explore <pattern>` |
| Frontend işi | `category: "visual-engineering"` |
| Derin düşünme | `category: "ultrabrain"` |

---

## 📁 Dosya Yapısı

```
D:\Projects\OpenCodeDev\
├── opencode_config_dev/
│   ├── opencode.json                    # Temel OpenCode config
│   ├── oh-my-opencode.json              # Mevcut config
│   ├── oh-my-opencode-optimized.jsonc   # Optimize edilmiş config
│   └── package.json
├── setup/
│   └── INSTALLATION-GUIDE.md           # Kurulum rehberi ( yeni başlayanlar için)
├── docs/
│   ├── 01-Configuration-Guide.md        # Konfigürasyon rehberi
│   ├── 02-Agent-Usage-Guide.md          # Ajan kullanım rehberi
│   ├── 03-Category-System-Guide.md      # Kategori sistemi rehberi
│   ├── 04-Parallel-Execution-Guide.md   # Paralel çalışma rehberi
│   ├── 05-Skills-Hooks-MCP-Guide.md     # Skills/Hooks/MCP rehberi
│   └── 06-Practical-Workflow-Guide.md   # Pratik senaryo rehberi
└── README.md                            # Bu dosya
```

---

## 🔗 Kaynaklar

- [OpenCode Dokümantasyonu](https://opencode.ai/docs/tr/)
- [oh-my-openagent GitHub](https://github.com/code-yeongyu/oh-my-openagent)
- [oh-my-openagent Overview](https://github.com/code-yeongyu/oh-my-openagent/blob/dev/docs/guide/overview.md)
- [oh-my-openagent Orchestration](https://github.com/code-yeongyu/oh-my-openagent/blob/dev/docs/guide/orchestration.md)
- [oh-my-openagent Configuration](https://github.com/code-yeongyu/oh-my-openagent/blob/dev/docs/reference/configuration.md)
- [oh-my-openagent Features](https://github.com/code-yeongyu/oh-my-openagent/blob/dev/docs/reference/features.md)
